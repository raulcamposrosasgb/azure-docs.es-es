---
title: Solución de problemas | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: article
description: Desarrollo rápido de Kubernetes con contenedores y microservicios en Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores
manager: douge
ms.openlocfilehash: b66e43c0f40f184bfb2c62327f5742346ff8b187
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2018
ms.locfileid: "43841616"
---
# <a name="troubleshooting-guide"></a>Guía de solución de problemas

Esta guía contiene información sobre problemas habituales que pueden surgir al usar Azure Dev Spaces.

## <a name="enabling-detailed-logging"></a>Habilitar el registro detallado

Con el fin de solucionar problemas de forma más eficaz, puede ser útil para crear registros más detallados para su revisión.

Para la extensión de Visual Studio, puede hacerlo estableciendo la variable de entorno `MS_VS_AZUREDEVSPACES_TOOLS_LOGGING_ENABLED` en 1. Asegúrese de reiniciar Visual Studio para que la variable de entorno surta efecto. Una vez habilitada, los registros detallados se escribirán en su directorio `%TEMP%\Microsoft.VisualStudio.Azure.DevSpaces.Tools`.

En la CLI, puede generar más información durante la ejecución del comando mediante el uso del conmutador `--verbose`.

## <a name="error-failed-to-create-azure-dev-spaces-controller"></a>Error "Failed to create Azure Dev Spaces controller" (Error al crear el controlador de Azure Dev Spaces)

Es posible que vea este error cuando algo va mal con la creación del controlador. Si es un error transitorio, eliminar y volver a crear el controlador lo solucionará.

### <a name="try"></a>Pruebe lo siguiente:

Para eliminar el controlador, use la CLI de Azure Dev Spaces. No es posible hacerlo en Visual Studio o en Cloud Shell. Para instalar la CLI de Azure Dev Spaces, primero instale la CLI de Azure y, a continuación, ejecute este comando:

```cmd
az aks use-dev-spaces -g <resource group name> -n <cluster name>
```

Y, a continuación, ejecute este comando para eliminar el controlador:

```cmd
azds remove -g <resource group name> -n <cluster name>
```

La recreación del controlador se puede realizar desde la CLI o desde Visual Studio. Siga las instrucciones en los tutoriales como si fuera a iniciar por primera vez.


## <a name="error-service-cannot-be-started"></a>Error "No se puede iniciar el servicio".

Es posible que vea este error cuando el código del servicio no se inicia. La causa suele estar en el código de usuario. Para obtener más información de diagnóstico, realice los siguientes cambios en los comandos y valores de configuración:

### <a name="try"></a>Pruebe lo siguiente:

En la línea de comandos:

Al usar _azds.exe_, emplee la opción de línea de comandos detallada, y utilice la opción de línea de comandos de salida para especificar el formato de salida.
 
    ```cmd
    azds up --verbose --output json
    ```

En Visual Studio:

1. Abra **Herramientas > Opciones** y en **Proyectos y soluciones**, elija **Build and Run** (Compilar y ejecutar).
2. Cambie la configuración de **Detalles de la salida de la compilación del proyecto de MSBuild** a **Detallado** o **Diagnóstico**.

    ![Captura de pantalla del cuadro de diálogo Opciones de herramientas](media/common/VerbositySetting.PNG)
    
## <a name="dns-name-resolution-fails-for-a-public-url-associated-with-a-dev-spaces-service"></a>Se produce un error en la resolución de nombres DNS para una dirección URL pública asociada con un servicio Dev Spaces

Cuando esto sucede, es posible que aparezca un error del tipo de la página no se puede mostrar o no se puede acceder a este sitio en el explorador web al intentar conectarse a la dirección URL pública asociada con un servicio Dev Spaces.

### <a name="try"></a>Pruebe lo siguiente:

Puede usar el siguiente comando para enumerar todas las direcciones URL asociadas con los servicios Dev Spaces:

```cmd
azds list-uris
```

Si una dirección URL está en estado *Pendiente*, significa que Dev Spaces aún está esperando que se complete el registro DNS. En ocasiones, tarda unos minutos para que esto ocurra. Dev Spaces también abre un túnel de host local para cada servicio, que puede usar mientras espera el registro del DNS.

Si una dirección URL permanece en estado *Pendiente* durante más de 5 minutos, puede que indique un problema con el pod de DNS externo que crea al punto de conexión público o el pod controlador de entrada nginx que adquiere el punto de conexión público. Puede usar los siguientes comandos para eliminar estos pods. Se volverá a crear automáticamente.

```cmd
kubectl delete pod -n kube-system -l app=addon-http-application-routing-external-dns
kubectl delete pod -n kube-system -l app=addon-http-application-routing-nginx-ingress
```

## <a name="error-required-tools-and-configurations-are-missing"></a>Error que indica que faltan las herramientas y configuraciones necesarias

Este error puede producirse al iniciar VS Code: "[Azure Dev Spaces] Faltan las herramientas y configuraciones necesarias para compilar y depurar "[nombre de proyecto]"".
El error significa que azds.exe no está en la variable de entorno PATH, tal como se muestra en VS Code.

### <a name="try"></a>Pruebe lo siguiente:

Inicie VS Code desde un símbolo del sistema donde la variable de entorno PATH esté establecida correctamente.

## <a name="error-azds-is-not-recognized-as-an-internal-or-external-command-operable-program-or-batch-file"></a>El error de "azds" no se reconoce como un comando interno o externo, programa operable o archivo por lotes
 
Es posible que vea este error si azds.exe no está instalado o configurado correctamente.

### <a name="try"></a>Pruebe lo siguiente:

1. Compruebe la ubicación %ProgramFiles%/Microsoft SDKs\Azure\Azure Dev Spaces CLI (Preview) de azds.exe. Si está ahí, agregue esa ubicación a la variable de entorno PATH.
2. Si azds.exe no está instalado, ejecute el siguiente comando:

    ```cmd
    az aks use-dev-spaces -n <cluster-name> -g <resource-group>
    ```

## <a name="warning-dockerfile-could-not-be-generated-due-to-unsupported-language"></a>Advertencia: Dockerfile could not be generated due to unsupported language (No se puede generar un Dockerfile debido a un lenguaje no admitido)
Azure Dev Spaces proporciona compatibilidad nativa para C# y Node.js. Al ejecutar *azds prep* en un directorio que contiene código escrito en uno de estos lenguajes, Azure Dev Spaces creará automáticamente un Dockerfile adecuado automáticamente.

Aún puede usar Azure Dev Spaces con código escrito en otros lenguajes, pero deberá crear el Dockerfile usted mismo antes de ejecutar *azds up* por primera vez.

### <a name="try"></a>Pruebe lo siguiente:
Si la aplicación se escribe en un lenguaje que Azure Dev Spaces no admite de forma nativa, deberá proporcionar un Dockerfile apropiado para compilar una imagen de contenedor que se ejecute en el código. Docker ofrece una [lista de procedimientos recomendados para escribir un Dockerfile](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/), así como una [referencia sobre Dockerfile](https://docs.docker.com/engine/reference/builder/) que puede ayudarle a hacerlo.

Una vez que tenga un Dockerfile adecuado en su lugar, puede continuar ejecutando *azds up* para ejecutar la aplicación en Azure Dev Spaces.

## <a name="error-upstream-connect-error-or-disconnectreset-before-headers"></a>Error "Error de conexión en dirección ascendente o desconexión o restablecimiento antes de los encabezados"
Puede ver este error al intentar acceder al servicio. Por ejemplo, cuando vaya a la dirección URL del servicio en un explorador. 

### <a name="reason"></a>Motivo 
El puerto de contenedor no está disponible. Las causas de este problema pueden ser: 
* El contenedor está todavía en proceso de creación e implementación. Este problema puede producirse si ejecuta `azds up` o inicia el depurador y luego intenta acceder al contenedor antes de que se haya implementado correctamente.
* La configuración del puerto no es coherente en _Dockerfile_, el gráfico de Helm y cualquier otro código de servidor que abre un puerto.

### <a name="try"></a>Pruebe lo siguiente:
1. Si el contenedor está en proceso de creación o implementación, puede esperar entre dos y tres segundos e intentar acceder al servicio de nuevo. 
1. Compruebe la configuración del puerto. Los números de puerto especificados deben ser **idénticos** en todos los recursos siguientes:
    * **Dockerfile:** especificado por la instrucción `EXPOSE`.
    * **[Gráfico de Helm](https://docs.helm.sh):** especificado por los valores `externalPort` y `internalPort` valores para un servicio (a menudo se encuentra en un archivo `values.yml`).
    * Los puertos abiertos en el código de aplicación, por ejemplo, en Node.js: `var server = app.listen(80, function () {...}`


## <a name="config-file-not-found"></a>Archivo de configuración no encontrado
Ejecuta `azds up` y aparece el siguiente error: `Config file not found: .../azds.yaml`

### <a name="reason"></a>Motivo
Debe ejecutar `azds up` desde el directorio raíz del código que desea ejecutar y debe inicializar la carpeta de código para que se ejecute con Azure Dev Spaces.

### <a name="try"></a>Pruebe lo siguiente:
1. Cambie el directorio actual a la carpeta raíz que contiene el código de servicio. 
1. Si no tiene un archivo _azds.yaml_ en la carpeta de código, ejecute `azds prep` para generar los recursos de Docker, Kubernetes y Azure Dev Spaces.

## <a name="error-the-pipe-program-azds-exited-unexpectedly-with-code-126"></a>Error: "El programa de canalización "azds" terminó inesperadamente con el código 126."
El inicio del depurador de VS Code a veces produce este error. Este es un problema conocido.

### <a name="try"></a>Pruebe lo siguiente:
1. Cierre y vuelva a abrir VS Code.
2. Vuelva a presionar F5.

## <a name="debugging-error-failed-to-find-debugger-extension-for-typecoreclr"></a>Error de depuración “No se pudo encontrar la extensión del depurador para type:coreclr”
La ejecución del depurador de VS Code informa del error `Failed to find debugger extension for type:coreclr.`.

### <a name="reason"></a>Motivo
La máquina de desarrollo no tiene instalada la extensión de VS Code para C# que incluye compatibilidad de depuración para .Net Core (CoreCLR).

### <a name="try"></a>Pruebe lo siguiente:
Instale la [extensión de VS Code para C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

## <a name="debugging-error-configured-debug-type-coreclr-is-not-supported"></a>Error de depuración "El tipo de depuración "coreclr" configurado no es compatible."
La ejecución del depurador de VS Code informa del error `Configured debug type 'coreclr' is not supported.`.

### <a name="reason"></a>Motivo
No tiene la extensión de VS Code para Azure Dev Spaces instalada en la máquina de desarrollo.

### <a name="try"></a>Pruebe lo siguiente:
Instale la [extensión de VS Code para Azure Dev Spaces](get-started-netcore.md).

## <a name="the-type-or-namespace-name-mylibrary-could-not-be-found"></a>No se encontró el tipo o el nombre de espacio de nombres "MyLibrary"

### <a name="reason"></a>Motivo 
El contexto de compilación está en el nivel de proyecto o servicio de forma predeterminada, por lo que un proyecto de biblioteca que está usando no se encontrará.

### <a name="try"></a>Pruebe lo siguiente:
¿Qué debe hacer?:
1. Modifique el archivo _azds.yaml_ para establecer el contexto de compilación en el nivel de la solución.
2. Modifique los archivos _Dockerfile_ y _Dockerfile.develop_ para hacer referencia a los archivos _.csproj_ correctamente, en relación con el nuevo contexto de compilación.
3. Coloque un archivo _.dockerignore_ junto al archivo .sln y realice las modificaciones según sea necesario.

Puede encontrar un ejemplo en https://github.com/sgreenmsft/buildcontextsample.

## <a name="microsoftdevspacesregisteraction-authorization-error"></a>Error de autorización "Microsoft.DevSpaces/register/action"
Podría ver el error siguiente error cuando administra Azure Dev Spaces o si trabaja en una suscripción de Azure para la que no tiene acceso de tipo Propietario o Colaborador.
`The client '<User email/Id>' with object id '<Guid>' does not have authorization to perform action 'Microsoft.DevSpaces/register/action' over scope '/subscriptions/<Subscription Id>'.`

### <a name="reason"></a>Motivo
La suscripción de Azure seleccionada no ha registrado el espacio de nombres `Microsoft.DevSpaces`.

### <a name="try"></a>Pruebe lo siguiente:
Un usuario con acceso de tipo Propietario o Colaborador a la suscripción de Azure puede ejecutar el siguiente comando de la CLI de Azure para registrar manualmente el espacio de nombres `Microsoft.DevSpaces`:

```cmd
az provider register --namespace Microsoft.DevSpaces
```

## <a name="azure-dev-spaces-doesnt-seem-to-use-my-existing-dockerfile-to-build-a-container"></a>Azure Dev Spaces no parece usar mi Dockerfile existente para crear un contenedor 

### <a name="reason"></a>Motivo
Azure Dev Spaces puede configurarse para que apunte a un _Dockerfile_ específico en el proyecto. Si parece que Azure Dev Spaces no usa el _Dockerfile_ que se espera para crear los contenedores, podría tener que indicar explícitamente a Azure Dev Spaces dónde se encuentra. 

### <a name="try"></a>Pruebe lo siguiente:
Abra el archivo _azds.yaml_ que Azure Dev Spaces generó en el proyecto. Utilice la directiva `configurations->develop->build->dockerfile` para que apuntar al Dockerfile que desea usar:

```
...
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
```
