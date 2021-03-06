---
title: 'Tutorial: Entrenar un modelo de clasificación de imágenes con Azure Machine Learning'
description: Obtenga información sobre cómo entrenar un modelo de clasificación de imágenes de scikit-learn con un cuaderno de Jupyter en Python. Este tutorial es la primera de una serie de dos partes.
author: hning86
ms.author: haining
ms.topic: conceptual
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: bed4abcce3019607715416b5194a2ddecc89b76a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46966618"
---
# <a name="tutorial-1-train-an-image-classification-model-with-azure-machine-learning"></a>Tutorial n.º 1: Entrenar un modelo de clasificación de imágenes con Azure Machine Learning

En este tutorial, entrenará un modelo de aprendizaje automático tanto localmente como en los recursos de proceso remotos. Usará el flujo de trabajo de entrenamiento e implementación para el servicio Azure Machine Learning en un cuaderno de Jupyter en Python.  A continuación, puede utilizar el cuaderno como plantilla para entrenar su propio modelo de Machine Learning con sus propios datos. Este tutorial es la **primera de dos partes**.  

En este tutorial se entrena una regresión logística simple usando el conjunto de datos de [MNIST](http://yann.lecun.com/exdb/mnist/) y [scikit-aprender](http://scikit-learn.org) con Azure Machine Learning.  MNIST es un conjunto de datos popular que consta de 70 000 imágenes en escala de grises. Cada imagen es un dígito escrito a mano de 28×28 píxeles, que representa un número de 0 a 9. El objetivo es crear un clasificador multiclase para identificar el dígito que representa una imagen determinada. 

Obtenga información sobre cómo:

> [!div class="checklist"]
> * Configuración de su entorno de desarrollo
> * Acceder y examinar los datos
> * Entrenar una regresión logística simple con la popular biblioteca de aprendizaje automático scikit-learn 
> * Entrenar varios modelos en un clúster remoto
> * Revisar los resultados del entrenamiento y registrar el mejor modelo

Más adelante, en la [segunda parte de este tutorial](tutorial-deploy-models-with-aml.md) aprenderá a seleccionar un modelo e implementarlo. 

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="get-the-notebook"></a>Obtención del cuaderno

Para su comodidad, este tutorial está disponible como cuaderno de Jupyter. Use cualquiera de estos métodos para ejecutar el cuaderno `tutorials/01.train-models.ipynb`:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-in-azure-notebook.md)]

## <a name="set-up-your-development-environment"></a>Configuración de su entorno de desarrollo

Toda la configuración para el trabajo de desarrollo puede realizarse en un cuaderno de Python.  La configuración incluye:

* Importar paquetes de Python
* Conectarse a un área de trabajo para habilitar la comunicación entre el equipo local y los recursos remotos
* Crear un experimento para realizar un seguimiento de todas las ejecuciones
* Crear un destino de proceso remoto que se usará para entrenamiento

### <a name="import-packages"></a>Importación de paquetes

Importe los paquetes de Python que necesite en esta sesión. También visualice la versión del SDK de Azure Machine Learning.

```python
%matplotlib inline
import numpy as np
import matplotlib
import matplotlib.pyplot as plt

import azureml
from azureml.core import Workspace, Run

# check core SDK version number
print("Azure ML SDK Version: ", azureml.core.VERSION)
```

### <a name="connect-to-workspace"></a>Conexión al área de trabajo

Cree un objeto de área de trabajo desde el área de trabajo existente. `Workspace.from_config()` lee el archivo **config.json** y carga los detalles en un objeto denominado `ws`.

```python
# load workspace configuration from the config.json file in the current folder.
ws = Workspace.from_config()
print(ws.name, ws.location, ws.resource_group, ws.location, sep = '\t')
```

### <a name="create-experiment"></a>Crear un experimento

Cree un experimento para realizar un seguimiento de todas las ejecuciones en el área de trabajo.  

```python
experiment_name = 'sklearn-mnist'

from azureml.core import Experiment
exp = Experiment(workspace=ws, name=experiment_name)
```

### <a name="create-remote-compute-target"></a>Creación de un destino de proceso remoto

Azure Batch AI es un servicio administrado que permite a los científicos de datos entrenar modelos de aprendizaje automático en clústeres de máquinas virtuales de Azure, incluidas VM con compatibilidad con GPU.  En este tutorial, creará un clúster de Azure Batch AI como entorno de aprendizaje. Este código crea un clúster automáticamente si no existe aún en el área de trabajo. 

 **La creación del clúster tarda aproximadamente 5 minutos.** Si el clúster ya está en el área de trabajo, este código lo usa y omite el proceso de creación.


```python
from azureml.core.compute import ComputeTarget, BatchAiCompute
from azureml.core.compute_target import ComputeTargetException

# choose a name for your cluster
batchai_cluster_name = "traincluster"

try:
    # look for the existing cluster by name
    compute_target = ComputeTarget(workspace=ws, name=batchai_cluster_name)
    if compute_target is BatchAiCompute:
        print('found compute target {}, just use it.'.format(batchai_cluster_name))
    else:
        print('{} exists but it is not a Batch AI cluster. Please choose a different name.'.format(batchai_cluster_name))
except ComputeTargetException:
    print('creating a new compute target...')
    compute_config = BatchAiCompute.provisioning_configuration(vm_size="STANDARD_D2_V2", # small CPU-based VM
                                                                #vm_priority='lowpriority', # optional
                                                                autoscale_enabled=True,
                                                                cluster_min_nodes=0, 
                                                                cluster_max_nodes=4)

    # create the cluster
    compute_target = ComputeTarget.create(ws, batchai_cluster_name, compute_config)
    
    # can poll for a minimum number of nodes and for a specific timeout. 
    # if no min node count is provided it uses the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
    # Use the 'status' property to get a detailed status for the current cluster. 
    print(compute_target.status.serialize())
```

Ahora tiene los paquetes y los recursos de proceso necesarios para entrenar un modelo en la nube. 

## <a name="explore-data"></a>Exploración de los datos

Antes de entrenar un modelo, deberá comprender los datos que usa para entrenarlo.  También deberá copiar los datos en la nube para que el entorno de aprendizaje en la nube pueda acceder a ellos.  En esta sección aprenderá a:

* Descargar el conjunto de datos de MNIST
* Mostrar algunas imágenes de ejemplo
* Cargar datos en la nube

### <a name="download-the-mnist-dataset"></a>Descargar el conjunto de datos de MNIST

Descargue el conjunto de datos de MNIST y guarde los archivos en un directorio `data` localmente.  Se descargan las imágenes y etiquetas para el entrenamiento y las pruebas.  


```python
import os
import urllib.request

os.makedirs('./data', exist_ok = True)

urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz', filename='./data/train-images.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz', filename='./data/train-labels.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename='./data/test-images.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz', filename='./data/test-labels.gz')
```

### <a name="display-some-sample-images"></a>Mostrar algunas imágenes de ejemplo

Cargue los archivos comprimidos en matrices `numpy`. A continuación, use `matplotlib` para trazar 30 imágenes aleatorias del conjunto de datos con sus etiquetas sobre ellas.



```python
# make sure utils.py is in the same directory as this code
from utils import load_data

# note we also shrink the intensity values (X) from 0-255 to 0-1. This helps the model converge faster.
X_train = load_data('./data/train-images.gz', False) / 255.0
y_train = load_data('./data/train-labels.gz', True).reshape(-1)

X_test = load_data('./data/test-images.gz', False) / 255.0
y_test = load_data('./data/test-labels.gz', True).reshape(-1)

# now let's show some randomly chosen images from the traininng set.
count = 0
sample_size = 30
plt.figure(figsize = (16, 6))
for i in np.random.permutation(X_train.shape[0])[:sample_size]:
    count = count + 1
    plt.subplot(1, sample_size, count)
    plt.axhline('')
    plt.axvline('')
    plt.text(x=10, y=-10, s=y_train[i], fontsize=18)
    plt.imshow(X_train[i].reshape(28, 28), cmap=plt.cm.Greys)
plt.show()
```

Una muestra aleatoria de imágenes muestra:

![muestra aleatoria de imágenes](./media/tutorial-train-models-with-aml/digits.png)

Ahora tiene una idea del aspecto de estas imágenes y el resultado de predicción esperado.

### <a name="upload-data-to-the-cloud"></a>Cargar datos en la nube

Ahora haga que los datos sean accesibles remotamente mediante la carga de datos desde el equipo local en la nube para que se pueda acceder a ellos para el entrenamiento remoto. El almacén de datos es una construcción cómoda asociada con el área de trabajo para que cargue y descargue datos, e interactúe con ellos desde los destinos de proceso remotos. 

Los archivos de MNIST se cargan en un directorio denominado `mnist` en la raíz del almacén de datos.

```python
ds = ws.get_default_datastore()
print(ds.datastore_type, ds.account_name, ds.container_name)

ds.upload(src_dir='./data', target_path='mnist', overwrite=True, show_progress=True)
```
Ahora tiene todo lo que necesita para empezar a entrenar un modelo. 

## <a name="train-a-model-locally"></a>Entrenar un modelo localmente

Entrene un modelo de regresión logística simple de scikit-learn localmente.

**El entrenamiento local puede tardar un minuto o dos** según la configuración del equipo.

```python
%%time
from sklearn.linear_model import LogisticRegression

clf = LogisticRegression()
clf.fit(X_train, y_train)
```

A continuación, haga predicciones usando el conjunto de pruebas y calcule la precisión. 

```python
y_hat = clf.predict(X_test)
print(np.average(y_hat == y_test))
```

Aparece la precisión del modelo local:

`0.9202`

Con solo unas pocas líneas de código, tiene una precisión del 92 %.

## <a name="train-on-a-remote-cluster"></a>Entrenamiento en un clúster remoto

Ahora puede ampliar este modelo simple creando un modelo con una tasa de regularización diferente. Esta vez va a entrenar el modelo en un recurso remoto.  

Para esta tarea, envíe el trabajo al clúster de entrenamiento remoto que configuró anteriormente.  Para enviar un trabajo, deberá:
* Creación de directorios
* Crear un script de entrenamiento
* Crear un estimador
* Enviar el archivo 

### <a name="create-a-directory"></a>Creación de directorios

Cree un directorio para proporcionar el código necesario desde el equipo al recurso remoto.

```python
import os
script_folder = './sklearn-mnist'
os.makedirs(script_folder, exist_ok=True)
```

### <a name="create-a-training-script"></a>Crear un script de entrenamiento

Para enviar el trabajo al clúster, primero cree un script de entrenamiento. Ejecute el siguiente código para crear el script de entrenamiento denominado `train.py` en el directorio que acaba de crear. Este entrenamiento agrega una tasa de regularización al algoritmo de entrenamiento, por lo que genera un modelo ligeramente diferente de la versión local.

```python
%%writefile $script_folder/train.py

import argparse
import os
import numpy as np

from sklearn.linear_model import LogisticRegression
from sklearn.externals import joblib

from azureml.core import Run
from utils import load_data

# let user feed in 2 parameters, the location of the data files (from datastore), and the regularization rate of the logistic regression model
parser = argparse.ArgumentParser()
parser.add_argument('--data-folder', type=str, dest='data_folder', help='data folder mounting point')
parser.add_argument('--regularization', type=float, dest='reg', default=0.01, help='regularization rate')
args = parser.parse_args()

data_folder = os.path.join(args.data_folder, 'mnist')
print('Data folder:', data_folder)

# load train and test set into numpy arrays
# note we scale the pixel intensity values to 0-1 (by dividing it with 255.0) so the model can converge faster.
X_train = load_data(os.path.join(data_folder, 'train-images.gz'), False) / 255.0
X_test = load_data(os.path.join(data_folder, 'test-images.gz'), False) / 255.0
y_train = load_data(os.path.join(data_folder, 'train-labels.gz'), True).reshape(-1)
y_test = load_data(os.path.join(data_folder, 'test-labels.gz'), True).reshape(-1)
print(X_train.shape, y_train.shape, X_test.shape, y_test.shape, sep = '\n')

# get hold of the current run
run = Run.get_submitted_run()

print('Train a logistic regression model with regularizaion rate of', args.reg)
clf = LogisticRegression(C=1.0/args.reg, random_state=42)
clf.fit(X_train, y_train)

print('Predict the test set')
y_hat = clf.predict(X_test)

# calculate accuracy on the prediction
acc = np.average(y_hat == y_test)
print('Accuracy is', acc)

run.log('regularization rate', np.float(args.reg))
run.log('accuracy', np.float(acc))

os.makedirs('outputs', exist_ok=True)
# note file saved in the outputs folder is automatically uploaded into experiment record
joblib.dump(value=clf, filename='outputs/sklearn_mnist_model.pkl')
```

Tenga en cuenta cómo el script obtiene los datos y guarda los modelos:

+ El script de entrenamiento lee un argumento para encontrar el directorio que contiene los datos.  Al enviar el trabajo más adelante, apunta al almacén de datos para este argumento: `parser.add_argument('--data-folder', type = str, dest = 'data_folder', help = 'data directory mounting point')`

    
+ El script de entrenamiento guarda el modelo en un directorio denominado outputs. <br/>
`joblib.dump(value = clf, filename = 'outputs/sklearn_mnist_model.pkl')`<br/>
Todo lo que se escriba en este directorio se cargará automáticamente en el área de trabajo. Accederá al modelo desde este directorio más adelante en el tutorial.

Se hace referencia al archivo `utils.py` desde el script de entrenamiento para cargar correctamente el conjunto de datos.  Copie este script en la carpeta de scripts para que pueda acceder junto con el script de entrenamiento en el recurso remoto.


```python
import shutil
shutil.copy('utils.py', script_folder)
```


### <a name="create-an-estimator"></a>Crear un estimador

Un objeto estimador se usa para enviar la ejecución.  Para crear el estimador, ejecute el código siguiente para definir:

* El nombre del objeto estimador, `est`.
* El directorio que contiene los scripts. Todos los archivos de este directorio se cargan en los nodos del clúster para su ejecución. 
* El destino de proceso.  En este caso, se usará el clúster de Batch AI que creó.
* El nombre del script de entrenamiento, train.py.
* Los parámetros necesarios del script de entrenamiento. 
* Los paquetes de Python necesarios para el entrenamiento.

En este tutorial, este destino es el clúster de Batch AI. Todos los archivos en el directorio del proyecto se cargan en los nodos del clúster para su ejecución. La carpeta de datos se establece para usar el almacén de datos (`ds.as_mount()`).

```python
from azureml.train.estimator import Estimator

script_params = {
    '--data-folder': ds.as_mount(),
    '--regularization': 0.8
}

est = Estimator(source_directory=script_folder,
                script_params=script_params,
                compute_target=compute_target,
                entry_script='train.py',
                conda_packages=['scikit-learn'])
```


### <a name="submit-the-job-to-the-cluster"></a>Envío del trabajo al clúster

Para ejecutar el experimento, envíe el objeto estimador.

```python
run = exp.submit(config=est)
run
```

Puesto que la llamada es asincrónica, devuelve un estado **en ejecución** en cuanto se inicia el trabajo.

## <a name="monitor-a-remote-run"></a>Supervisión de una ejecución remota

En total, la primera ejecución toma **aproximadamente 10 minutos**. Pero para las ejecuciones posteriores, siempre que no cambien las dependencias del script, se reutiliza la misma imagen y, por tanto, el tiempo de inicio del contenedor es mucho más rápido.

Esto es lo que sucede mientras espera:

- **Creación de la imagen**: se crea una imagen de Docker que coincida con el entorno de Python especificado por el estimador. La imagen se carga en el área de trabajo. La creación y carga de la imagen toma **aproximadamente 5 minutos**. 

  Esta fase se produce una vez para cada entorno de Python, puesto que el contenedor se almacena en caché para las ejecuciones posteriores.  Durante la creación de la imagen, los registros se transmiten al historial de ejecución. Puede supervisar el progreso de la creación de la imagen con estos registros.

- **Escalado**: si el clúster remoto requiere más nodos que los que hay disponibles actualmente, se agregan nodos adicionales de manera automática. El escalado suele tomar **aproximadamente 5 minutos.**

- **Ejecución**: en esta etapa, los archivos y scripts necesarios se envían al destino de proceso, luego se montan o copian los almacenes de datos y se ejecuta entry_script. Mientras se ejecuta el trabajo, stdout y el directorio ./logs se transmiten al historial de ejecución. Puede supervisar el progreso de la ejecución con estos registros.

- **Posprocesamiento**: el directorio ./outputs de la ejecución se copia en el historial de ejecución en el área de trabajo para que pueda acceder a estos resultados.


Puede comprobar el progreso de un trabajo en ejecución de varias maneras. En este tutorial se usa un widget de Jupyter, así como un método `wait_for_completion`. 

### <a name="jupyter-widget"></a>Widget de Jupyter

Vea el progreso de la ejecución con un widget de Jupyter.  Al igual que el envío de ejecución, el widget es asincrónico y proporciona las actualizaciones directas cada 10 a 15 segundos hasta que se completa el trabajo.


```python
from azureml.train.widgets import RunDetails
RunDetails(run).show()
```

Esta es una instantánea del widget que se muestra al final del entrenamiento:

![widget del cuaderno](./media/tutorial-train-models-with-aml/widget.png)

### <a name="get-log-results-upon-completion"></a>Obtener los resultados de registros tras la finalización

El entrenamiento y la supervisión del modelo se producen en segundo plano. Espere a que el modelo haya completado el entrenamiento para poder ejecutar más código. Use `wait_for_completion` para mostrar cuando se haya completado el entrenamiento del modelo. 


```python
run.wait_for_completion(show_output=False) # specify True for a verbose log
```

### <a name="display-run-results"></a>Mostrar los resultados de la ejecución

Ahora tiene un modelo entrenado en un clúster remoto.  Recupere la precisión del modelo:

```python
print(run.get_metrics())
```
El resultado muestra que el modelo remoto tiene una precisión ligeramente mayor que el modelo local, debido a la adición de la tasa de regularización durante el entrenamiento.  

`{'regularization rate': 0.8, 'accuracy': 0.9204}`

En el tutorial de implementación explorará este modelo con más detalle.

## <a name="register-model"></a>Registro del modelo

El último paso del entrenamiento, el script escribió el archivo `outputs/sklearn_mnist_model.pkl` en un directorio denominado `outputs` en la VM del clúster donde se ejecuta el trabajo. `outputs` es un directorio especial, ya que todo el contenido de este directorio se carga automáticamente en el área de trabajo.  Este contenido aparece en el registro de ejecución en el experimento en el área de trabajo. Por lo tanto, el archivo del modelo ahora también está disponible en el área de trabajo.

Puede ver los archivos asociados a esa ejecución.

```python
print(run.get_file_names())
```

Registre el modelo en el área de trabajo para que usted u otros colaboradores puedan consultar, examinar e implementar este modelo más adelante.

```python
# register model 
model = run.register_model(model_name='sklearn_mnist', model_path='outputs/sklearn_mnist_model.pkl')
print(model.name, model.id, model.version, sep = '\t')
```

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

También puede eliminar solo el clúster de proceso administrado de Azure. Sin embargo, puesto que el escalado automático está activado y el mínimo del clúster es 0, este recurso en concreto no incurrirá en cargos de proceso adicionales cuando no esté en uso.


```python
# optionally, delete the Azure Managed Compute cluster
compute_target.delete()
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial de Azure Machine Learning ha usado Python para:

> [!div class="checklist"]
> * Configuración de su entorno de desarrollo
> * Acceder y examinar los datos
> * Entrenar una regresión logística simple con la popular biblioteca de aprendizaje automático scikit-learn
> * Entrenar varios modelos en un clúster remoto
> * Revisar los detalles del entrenamiento y registrar el mejor modelo

Está listo para implementar este modelo registrado con las instrucciones de la siguiente parte de la serie de tutoriales:

> [!div class="nextstepaction"]
> [Tutorial 2: Implementación de modelos](tutorial-deploy-models-with-aml.md)