---
title: 'Entrenamiento de modelos con aprendizaje automático automatizado en la nube: Azure Machine Learning'
description: En este artículo se explica cómo crear un recurso de proceso remoto para entrenar los modelos de aprendizaje automático automáticamente.
services: machine-learning
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 00d34fd0fe5f62e4da4be7d80afceb29753251bc
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46946976"
---
# <a name="train-models-with-automated-machine-learning-in-the-cloud-with-azure-machine-learning"></a>Entrenamiento de modelos con aprendizaje automático automatizado en la nube mediante Azure Machine Learning

En Azure Machine Learning puede entrenar un modelo en los diferentes tipos de recursos de proceso que administre. El destino de proceso podría ser un equipo local o un equipo en la nube.

Mediante la incorporación de destinos de procesos adicionales como Data Science Virtual Machine (DSVM) basado en Ubuntu o Azure Batch AI, resulta fácil escalar vertical y horizontalmente el experimento de aprendizaje automático. DSVM es una imagen de máquina virtual personalizada en la nube de Azure de Microsoft diseñada específicamente para realizar la ciencia de datos. Tiene muchas ciencias de datos conocidas y otras herramientas preinstaladas y configuradas previamente.  

En este artículo, aprenderá a crear un modelo mediante el aprendizaje automático automatizado en DSVM.  

## <a name="how-does-remote-differ-from-local"></a>¿En qué se diferencia el modo remoto del local?

El tutorial "[Train a classification model with automated machine learning](tutorial-auto-train-models.md)" (Entrenamiento de un modelo de clasificación con el aprendizaje automático automatizado) le enseña a usar un equipo local para entrenar el modelo de aprendizaje automático automatizado.  El flujo de trabajo del entrenamiento local también se aplica a los destinos remotos. Con un proceso remoto, las iteraciones del experimento de aprendizaje automático se ejecutan de forma asincrónica. De ese modo, se puede cancelar una iteración determinada, ver el estado de la ejecución y continuar trabajando en otras celdas del cuaderno de Jupyter. Para entrenar de forma remota, en primer lugar cree un destino de proceso remoto, como una máquina DSVM de Azure, configúrelo y envíe allí el código.

Este artículo muestra los pasos adicionales necesarios para ejecutar un experimento de aprendizaje automático automatizado en una instancia de DSVM remota.  Aquí se usa, en todo el código, el objeto del área de trabajo, `ws` del tutorial.

```python
ws = Workspace.from_config()
```

## <a name="create-resource"></a>Crear el recurso

Cree la máquina DSVM en el área de trabajo (`ws`) si aún no existe. Si la máquina DSVM se creó anteriormente, este código omite el proceso de creación y carga los detalles de los recursos existentes en el objeto `dsvm_compute`.  

**Tiempo previsto**: la creación de la máquina virtual tarda aproximadamente cinco minutos.

```python
from azureml.core.compute import DsvmCompute

dsvm_name = 'mydsvm' #Name your DSVM
try:
    dsvm_compute = DsvmCompute(ws, dsvm_name)
    print('found existing dsvm.')
except:
    print('creating new dsvm.')
    dsvm_config = DsvmCompute.provisioning_configuration(vm_size = "Standard_D2_v2")
    dsvm_compute = DsvmCompute.create(ws, name = dsvm_name, provisioning_configuration = dsvm_config)
    dsvm_compute.wait_for_completion(show_output = True)
```

Ahora puede usar el objeto `dsvm_compute` como destino del proceso remoto.

Entre las restricciones en los nombres de máquina DSVM se incluyen:
+ La longitud debe ser inferior a 64 caracteres.  
+ No puede contener ninguno de los caracteres siguientes: {`\` ~ ! @ # $ % ^ & * ( ) = + _ [ ] { } \\\\ | ; : \' \\" , < > / ?.`

>[!Warning]
>Si la creación no puede llevarse a cabo y aparece un mensaje sobre la idoneidad de la compra de Marketplace:
>    1. Vaya a [Azure Portal](https://portal.azure.com).
>    1. Empiece a crear una máquina DSVM. 
>    1. Seleccione "Want to create programmatically" (Deseo crear mediante programación) para habilitar la creación mediante programación.
>    1. Salga sin crear realmente la máquina virtual.
>    1. Vuelva a ejecutar el código de creación.


## <a name="create-a-runconfiguration-with-dsvm-name"></a>Creación de RunConfiguration con el nombre de la máquina DSVM
Aquí se indica el valor de runconfiguration del nombre de la máquina DSVM

```python

# create the run configuration to use for remote training
from azureml.core.runconfig import RunConfiguration
run_config = RunConfiguration() 
# set the target to dsvm_compute created above
run_config.target = dsvm_compute 
```

Ahora puede usar el objeto `run_config` como destino para el aprendizaje automático automatizado. 

## <a name="access-data-using-get-data-file"></a>Acceso a los datos con un archivo de obtención de datos

Proporcione a los recursos remotos acceso a los datos de entrenamiento. Para los experimentos de aprendizaje automático automatizado que se ejecutan en el proceso remoto, los datos deben capturarse con una función `get_data()`.  

Para proporcionar acceso, debe:
+ Crear un archivo get_data.py que contenga una función `get_data()` 
* Colocar ese archivo en el directorio raíz de la carpeta que contenga los scripts 

Puede encapsular el código para leer datos desde un almacenamiento de blobs o un disco local en el archivo get_data.py. En el siguiente ejemplo de código, los datos proceden del paquete sklearn.

>[!Warning]
>Si usa un proceso remoto, debe usar `get_data()` para realizar las transformaciones de datos.


```python
%%writefile $project_folder/get_data.py

from sklearn import datasets
from scipy import sparse
import numpy as np

def get_data():
    
    digits = datasets.load_digits()
    X_digits = digits.data[10:,:]
    y_digits = digits.target[10:]

    return { "X" : X_digits, "y" : y_digits }
```

## <a name="configure-automated-machine-learning-experiment"></a>Configuración del experimento de aprendizaje automático automatizado

Especifique la configuración de `AutoMLConfig`.  (Consulte una [lista completa de parámetros]() y sus posibles valores).

En la configuración, `run_configuration` está establecido en el objeto `run_config`, que contiene la configuración de la máquina DSVM.  

```python
from azureml.train.automl import AutoMLConfig
import time
import logging

automl_settings = {
    "name": "AutoML_Demo_Experiment_{0}".format(time.time()),
    "max_time_sec": 600,
    "iterations": 20,
    "n_cross_validations": 5,
    "primary_metric": 'AUC_weighted',
    "preprocess": False,
    "concurrent_iterations": 10,
    "verbosity": logging.INFO
}

automl_config = AutoMLConfig(task='classification',
                             debug_log='automl_errors.log',
                             path=project_folder,
                             run_configuration=run_config,
                             data_script=project_folder + "./get_data.py",
                             **automl_settings
                            )
```

## <a name="submit-automated-machine-learning-training-experiment"></a>Envío del experimento de entrenamiento de aprendizaje automático automatizado

Ahora, envíe la configuración para seleccionar automáticamente el algoritmo y los hiperparámetros, y entrenar el modelo. (Más información [acerca de la configuración]() para el método `submit`).

```python
from azureml.core.experiment import Experiment
experiment=Experiment(ws, 'automl_remote')
remote_run = experiment.submit(automl_config, show_output=True)
```
Verá una salida similar a esto:

    Running on remote compute: mydsvmParent Run ID: AutoML_015ffe76-c331-406d-9bfd-0fd42d8ab7f6
    ***********************************************************************************************
    ITERATION: The iteration being evaluated.
    PIPELINE:  A summary description of the pipeline being evaluated.
    DURATION: Time taken for the current iteration.
    METRIC: The result of computing score on the fitted pipeline.
    BEST: The best observed score thus far.
    ***********************************************************************************************
    
     ITERATION     PIPELINE                               DURATION                METRIC      BEST
             2      Standardize SGD classifier            0.0                      0.954     0.954
             7      Normalizer DT                         0.0                      0.161     0.954
             0      Scale MaxAbs 1 extra trees            0.0                      0.936     0.954
             4      Robust Scaler SGD classifier          0.0                      0.867     0.954
             1      Normalizer kNN                        0.0                      0.984     0.984
             9      Normalizer extra trees                0.0                      0.834     0.984
             5      Robust Scaler DT                      0.0                      0.736     0.984
             8      Standardize kNN                       0.0                      0.981     0.984
             6      Standardize SVM                       2.2                      0.984     0.984
            10      Scale MaxAbs 1 DT                     0.0                      0.077     0.984
            11      Standardize SGD classifier            0.0                      0.863     0.984
             3      Standardize gradient boosting         5.4                      0.971     0.984
            12      Robust Scaler logistic regression     2.0                      0.955     0.984
            14      Scale MaxAbs 1 SVM                    0.0                      0.989     0.989
            13      Scale MaxAbs 1 gradient boosting      3.4                      0.971     0.989
            15      Robust Scaler kNN                     0.0                      0.904     0.989
            17      Standardize kNN                       0.0                      0.974     0.989
            16      Scale 0/1 gradient boosting           2.8                      0.968     0.989
            18      Scale 0/1 extra trees                 0.0                      0.828     0.989
            19      Robust Scaler kNN                     0.0                      0.983     0.989


## <a name="explore-results"></a>Exploración de los resultados

Puede usar el mismo widget de Jupyter que el del [tutorial de aprendizaje](tutorial-auto-train-models.md#explore-the-results) para ver un gráfico y la tabla de resultados.

```python
from azureml.train.widgets import RunDetails
RunDetails(remote_run).show()
```
Esta es una imagen estática del widget.  En el cuaderno, puede hacer clic en cualquier línea de la tabla para ver las propiedades de ejecución y los registros de salida para los que se ejecutan.   También puede usar la lista desplegable encima del grafo para ver un grafo de cada métrica disponible para cada iteración.

![tabla de widget](./media/how-to-auto-train-remote/table.png)
![trazado de widget](./media/how-to-auto-train-remote/plot.png)

El widget muestra una dirección URL que puede usar para ver y explorar los detalles de ejecución individuales.
 
### <a name="view-logs"></a>Ver registros

Busque los registros en la máquina DSVM en /tmp/azureml_run/{iterationid}/azureml-logs.

## <a name="example"></a>Ejemplo

El cuaderno `automl/03.auto-ml-remote-execution.ipynb` demuestra los conceptos de este artículo.  Obtenga este cuaderno:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Pasos siguientes

Más información acerca de [cómo configurar opciones para el aprendizaje automático]().
