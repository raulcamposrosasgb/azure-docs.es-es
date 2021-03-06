---
title: Actualizaciones de sistema operativo automáticas con conjuntos de escalado de máquinas virtuales de Azure | Microsoft Docs
description: Obtenga información acerca de cómo actualizar automáticamente el sistema operativo en instancias de máquina virtual en un conjunto de escalado.
services: virtual-machine-scale-sets
documentationcenter: ''
author: yeki
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2018
ms.author: yeki
ms.openlocfilehash: 6b20ef98e008d9c5d984ba29eed894b1c5ec8c09
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2018
ms.locfileid: "39263255"
---
# <a name="azure-virtual-machine-scale-set-automatic-os-upgrades"></a>Actualizaciones de sistema operativo automáticas de un conjunto de escalado de máquinas virtuales de Azure

La actualización automática de la imagen de sistema operativo es una característica en versión preliminar para conjuntos de escalado de máquinas virtuales de Azure que actualiza automáticamente todas las máquinas virtuales a la imagen de sistema operativo más reciente.

La actualización automática del sistema operativo tiene las siguientes características:

- Una vez configurada, la imagen del sistema operativo más reciente publicada por los editores de la imagen se aplica automáticamente al conjunto de escalado sin la intervención del usuario.
- Actualiza lotes de instancias de forma gradual cada vez que el editor publica una nueva imagen de plataforma.
- Se integra con el sondeo de estado de aplicación (opcional, pero muy recomendado por motivos de seguridad).
- Funciona con todos los tamaños de máquina virtual.
- Funciona con imágenes de plataforma de Windows y Linux.
- Puede rechazar las actualizaciones automáticas en cualquier momento (las actualizaciones de sistema operativo también se pueden iniciar manualmente).
- El disco del sistema operativo de una máquina virtual se reemplaza por el nuevo disco de sistema operativo creado con la versión más reciente de la imagen. Las extensiones configuradas y los scripts de datos personalizados se ejecutan, mientras se conservan los discos de datos persistentes.


## <a name="preview-notes"></a>Notas de la versión preliminar 
Mientras la versión se encuentre en estado preliminar, existen las siguientes limitaciones y restricciones:

- Las actualizaciones automáticas del sistema operativo solo admiten [cuatro SKU de sistema operativo](#supported-os-images). No hay ningún contrato de nivel de servicio ni garantías. Se recomienda que no utilice las actualizaciones automáticas en cargas de trabajo críticas de producción durante la versión preliminar.
- El cifrado de disco de Azure **no** es compatible actualmente con la actualización del sistema operativo automática del conjunto de escalado de máquinas virtuales.


## <a name="register-to-use-automatic-os-upgrade"></a>Registro para usar la actualización automática del sistema operativo
Para usar la característica de actualización del sistema operativo automatizada, registre el proveedor de la versión preliminar con Azure PowerShell o la CLI de Azure 2.0.

### <a name="powershell"></a>PowerShell

1. Realice el registro con [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature):

     ```powershell
     Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName AutoOSUpgradePreview
     ```

2. El estado de registro tarda unos 10 minutos en aparecer como *Registrado*. Puede comprobar el estado de registro actual con [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature). 

3. Una vez registrado, confirme que el proveedor *Microsoft.Compute* está registrado. En el ejemplo siguiente se usa Azure Powershell con [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider):

     ```powershell
     Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
     ```


### <a name="cli-20"></a>CLI 2.0

1. Realice el registro con [az feature register](/cli/azure/feature#az-feature-register):

     ```azurecli
     az feature register --name AutoOSUpgradePreview --namespace Microsoft.Compute
     ```

2. El estado de registro tarda unos 10 minutos en aparecer como *Registrado*. Puede comprobar el estado de registro actual con [az feature show](/cli/azure/feature#az-feature-show). 
 
3. Una vez realizado el registro, asegúrese de que el proveedor *Microsoft.Compute* está registrado. En el ejemplo siguiente se usa la CLI de Azure (2.0.20 o posterior) con [az provider register](/cli/azure/provider#az-provider-register):

     ```azurecli
     az provider register --namespace Microsoft.Compute
     ```

> [!NOTE]
> Los clústeres de Service Fabric tienen su propia noción de estado de la aplicación, pero los conjuntos de escalado sin Service Fabric usan el sondeo de estado del equilibrador de carga para supervisar el estado de la aplicación. 
>
> ### <a name="azure-powershell"></a>Azure PowerShell
>
> 1. Realice el registro de la característica de proveedor para los sondeos de mantenimiento con [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature):
>
>      ```powershell
>      Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVmssHealthProbe
>      ```
>
> 2. De nuevo, el estado de registro tarda unos 10 minutos en aparecer como *Registrado*. Puede comprobar el estado de registro actual con [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature).
>
> 3. Una vez completado el proceso de registro, asegúrese de que el proveedor *Microsoft.Network* está registrado con [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider):
>
>      ```powershell
>      Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
>      ```
>
>
> ### <a name="cli-20"></a>CLI 2.0
>
> 1. Realice el registro de la característica de proveedor para los sondeos de mantenimiento con [az feature register](/cli/azure/feature#az-feature-register):
>
>      ```azurecli
>      az feature register --name AllowVmssHealthProbe --namespace Microsoft.Network
>      ```
>
> 2. De nuevo, el estado de registro tarda unos 10 minutos en aparecer como *Registrado*. Puede comprobar el estado de registro actual con [az feature show](/cli/azure/feature#az-feature-show). 
>
> 3. Una vez completado el proceso de registro, asegúrese de que el proveedor *Microsoft.Network* está registrado con [az provider register](/cli/azure/provider#az-provider-register) como se indica a continuación:
>
>      ```azurecli
>      az provider register --namespace Microsoft.Network
>      ```

## <a name="portal-experience"></a>Experiencia del portal
Tras seguir los pasos de registro anteriores, puede ir a [Azure Portal](https://aka.ms/managed-compute) para habilitar las actualizaciones automáticas del sistema operativo en sus conjuntos de escalado y ver el progreso de las actualizaciones:

![](./media/virtual-machine-scale-sets-automatic-upgrade/automatic-upgrade-portal.png)


## <a name="supported-os-images"></a>Imágenes de sistema operativo compatibles
Actualmente se admiten solo determinadas imágenes de plataforma del sistema operativo. Actualmente no puede usar las imágenes personalizadas que haya creado. La propiedad *versión* de la imagen de plataforma debe establecerse en *más reciente*.

Actualmente se admiten las siguientes SKU (se agregarán más):
    
| Publicador               | Oferta         |  SKU               | Versión  |
|-------------------------|---------------|--------------------|----------|
| Canonical               | UbuntuServer  | 16.04-LTS          | más reciente   |
| Microsoft Windows Server  | Windows Server | Centro de datos de 2012-R2 | más reciente   |
| Microsoft Windows Server  | Windows Server | 2016-Datacenter    | más reciente   |
| Microsoft Windows Server  | Windows Server | 2016-Datacenter-Smalldisk | más reciente   |
| Microsoft Windows Server  | Windows Server | 2016-Datacenter-with-Containers | más reciente   |



## <a name="application-health-without-service-fabric"></a>Estado de la aplicación sin Service Fabric
> [!NOTE]
> Esta sección solo es aplicable a conjuntos de escalado sin Service Fabric. Service Fabric tiene su propia noción de estado de la aplicación. Al utilizar actualizaciones automáticas del sistema operativo con Service Fabric, la nueva imagen del sistema operativo se implanta de dominio de actualización en dominio de actualización para mantener una alta disponibilidad de los servicios que se ejecutan en Service Fabric. Para más información sobre las características de durabilidad de los clústeres de Service Fabric, consulte [esta documentación](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster).

Durante una actualización del sistema operativo, las instancias de máquina virtual de un conjunto de escalado se actualizan en lote de uno en uno. La actualización debe continuar solo si la aplicación cliente se encuentra en buen estado en las instancias de máquina virtual actualizadas. Por este motivo, la aplicación debe proporcionar señales de mantenimiento al motor de actualización del sistema operativo del conjunto de escalado. Durante las actualizaciones del sistema operativo, la plataforma se fija en el estado de energía de la máquina virtual y el estado de aprovisionamiento de la extensión para determinar si una instancia de máquina virtual está en buen estado después de una actualización. Durante la actualización del sistema operativo de una instancia de máquina virtual, se reemplaza el disco del sistema operativo en una instancia de máquina virtual por un nuevo disco basado en la versión más reciente de la imagen. Una vez finalizada la actualización del sistema operativo, las extensiones configuradas se ejecutan en estas máquinas virtuales. Solo cuando todas las extensiones en una máquina virtual se han aprovisionado correctamente, la aplicación se considera en buen estado. 

Además, el conjunto de escalado *debe* configurarse con sondeos de mantenimiento de aplicación para proporcionar a la plataforma información correcta sobre el estado actual de la aplicación. Los sondeos de estado de aplicación son sondeos de Load Balancer personalizados que se usan como una señal de estado. La aplicación que se ejecuta en una instancia de máquina virtual del conjunto de escalado puede responder a solicitudes HTTP o TCP externas para indicar si el mantenimiento es correcto. Para obtener más información sobre cómo funcionan los sondeos de Load Balancer personalizados, consulte [Descripción de los sondeos del equilibrador de carga](../load-balancer/load-balancer-custom-probe-overview.md).

Si el conjunto de escalado está configurado para usar varios grupos de selección de ubicación, es necesario utilizar sondeos con una instancia de [Load Balancer estándar](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview).

### <a name="important-keep-credentials-up-to-date"></a>Importante: mantenga actualizadas las credenciales
Si el conjunto de escalado usa credenciales para acceder a recursos externos, deberá asegurarse de que las credenciales se mantengan actualizadas. Por ejemplo, una extensión de máquina virtual se puede configurar para usar un token SAS para la cuenta de almacenamiento. Si las credenciales, incluidos los certificados y los tokens, han expirado, se producirá un error en la actualización y el primer lote de máquinas virtuales se quedará en estado de error.

Los pasos recomendados para recuperar las máquinas virtuales y volver a habilitar la actualización automática del sistema operativo si se produce un error de autenticación de recursos son:

* Volver a generar el token (o cualquier otra credencial) pasada en las extensiones.
* Asegurarse de que cualquier credencial usada desde dentro de la máquina virtual para comunicarse con entidades externas está actualizada.
* Actualizar las extensiones en el modelo de conjunto de escala con los tokens nuevos.
* Implementar el conjunto de escala actualizada, lo que actualizará todas las instancias de máquina virtual, incluyendo las que dieran error. 

### <a name="configuring-a-custom-load-balancer-probe-as-application-health-probe-on-a-scale-set"></a>Configuración de un sondeo de Load Balancer personalizado como sondeo de estado de aplicación en un conjunto de escalado
*Debe* crear un sondeo del equilibrador de carga explícitamente para el estado del conjunto de escalado. Puede utilizarse el mismo punto de conexión para un sondeo HTTP o un sondeo TCP existente, pero un sondeo de mantenimiento puede requerir un comportamiento diferente por parte de un sondeo de equilibrador de carga tradicional. Por ejemplo, un sondeo de equilibrador de carga tradicional podría devolver un estado incorrecto si la carga en la instancia es demasiado alta. Por el contrario, esto podría no ser adecuado para determinar el mantenimiento de la instancia durante una actualización automática del sistema operativo. Configure el sondeo para que tenga una tasa de sondeo elevada de menos de dos minutos.

Se puede hacer referencia al sondeo de equilibrador de carga en el valor *networkProfile* del conjunto de escalado, y se puede asociar con un equilibrador de carga interno o público del modo siguiente:

```json
"networkProfile": {
  "healthProbe" : {
    "id": "[concat(variables('lbId'), '/probes/', variables('sshProbeName'))]"
  },
  "networkInterfaceConfigurations":
  ...
```


## <a name="enforce-an-os-image-upgrade-policy-across-your-subscription"></a>Aplicación de una directiva de actualización de imagen de sistema operativo a través de su suscripción
Para las actualizaciones de seguridad, es muy recomendable exigir una directiva de actualización. Esta directiva puede requerir sondeos de estado de la aplicación a través de su suscripción. La siguiente directiva de Azure Resource Manager rechaza las implementaciones que no tienen configurados los parámetros de la actualización automatizada de la imagen del sistema operativo:

### <a name="powershell"></a>PowerShell
1. Obtenga la definición de directiva de Azure Resource Manager integrada con [Get-AzureRmPolicyDefinition](/powershell/module/AzureRM.Resources/Get-AzureRmPolicyDefinition) como se indica a continuación:

    ```powershell
    $policyDefinition = Get-AzureRmPolicyDefinition -Id "/providers/Microsoft.Authorization/policyDefinitions/465f0161-0087-490a-9ad9-ad6217f4f43a"
    ```

2. Asigne una directiva a una suscripción con [New-AzureRmPolicyAssignment](/powershell/module/AzureRM.Resources/New-AzureRmPolicyAssignment) como se indica a continuación:

    ```powershell
    New-AzureRmPolicyAssignment `
        -Name "Enforce automatic OS upgrades with app health checks" `
        -Scope "/subscriptions/<SubscriptionId>" `
        -PolicyDefinition $policyDefinition
    ```

### <a name="cli-20"></a>CLI 2.0
Asignación de directiva a una suscripción con la directiva integrada de Azure Resource Manager:

```azurecli
az policy assignment create --display-name "Enforce automatic OS upgrades with app health checks" --name "Enforce automatic OS upgrades" --policy 465f0161-0087-490a-9ad9-ad6217f4f43a --scope "/subscriptions/<SubscriptionId>"
```

## <a name="configure-auto-updates"></a>Configuración de actualizaciones automáticas
Para configurar las actualizaciones automáticas, asegúrese de que la propiedad *automaticOSUpgrade* está configurada en *true* en la definición del modelo del conjunto de escalado. Puede configurar esta propiedad con Azure PowerShell o la CLI de Azure 2.0.

### <a name="powershell"></a>PowerShell
En el ejemplo siguiente se usa Azure PowerShell (4.4.1 o posterior) para configurar las actualizaciones automáticas del conjunto de escalado denominado *myVMSS* en el grupo de recursos denominado *myResourceGroup*:

```powershell
$rgname = myResourceGroup
$vmssname = myVMSS
$vmss = Get-AzureRmVMss -ResourceGroupName $rgname -VmScaleSetName $vmssname
$vmss.UpgradePolicy.AutomaticOSUpgrade = $true
Update-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname -VirtualMachineScaleSet $vmss
```

### <a name="cli-20"></a>CLI 2.0
En el ejemplo siguiente se usa la CLI de Azure (2.0.20 o posterior) para configurar las actualizaciones automáticas del conjunto de escalado denominado *myVMSS* en el grupo de recursos denominado *myResourceGroup*:

```azurecli
rgname="myResourceGroup"
vmssname="myVMSS"
az vmss update --name $vmssname --resource-group $rgname --set upgradePolicy.AutomaticOSUpgrade=true
```


## <a name="check-the-status-of-an-automatic-os-upgrade"></a>Comprobación del estado de una actualización automática del sistema operativo
Puede comprobar el estado de la actualización del sistema operativo más reciente realizada en el conjunto de escalado con Azure PowerShell, la CLI de Azure 2.0 o las API de REST.

### <a name="powershell"></a>PowerShell
En el ejemplo siguiente se usa Azure PowerShell (4.4.1 o posterior) para comprobar el estado del conjunto de escalado denominado *myVMSS* en el grupo de recursos denominado *myResourceGroup*:

```powershell
Get-AzureRmVmssRollingUpgrade -ResourceGroupName myResourceGroup -VMScaleSetName myVMSS
```

### <a name="cli-20"></a>CLI 2.0
En el ejemplo siguiente se usa la CLI de Azure (2.0.20 o posterior) para comprobar el estado del conjunto de escalado denominado *myVMSS* en el grupo de recursos denominado *myResourceGroup*:

```azurecli
az vmss rolling-upgrade get-latest --resource-group myResourceGroup --name myVMSS
```

### <a name="rest-api"></a>API DE REST
En el ejemplo siguiente se usa la API de REST para comprobar el estado del conjunto de escalado denominado *myVMSS* en el grupo de recursos denominado *myResourceGroup*:

```
GET on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/rollingUpgrades/latest?api-version=2017-03-30`
```

La llamada GET devuelve propiedades similares a la salida del ejemplo siguiente:

```json
{
  "properties": {
    "policy": {
      "maxBatchInstancePercent": 20,
      "maxUnhealthyInstancePercent": 5,
      "maxUnhealthyUpgradedInstancePercent": 5,
      "pauseTimeBetweenBatches": "PT0S"
    },
    "runningStatus": {
      "code": "Completed",
      "startTime": "2017-06-16T03:40:14.0924763+00:00",
      "lastAction": "Start",
      "lastActionTime": "2017-06-22T08:45:43.1838042+00:00"
    },
    "progress": {
      "successfulInstanceCount": 3,
      "failedInstanceCount": 0,
      "inprogressInstanceCount": 0,
      "pendingInstanceCount": 0
    }
  },
  "type": "Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades",
  "location": "southcentralus"
}
```


## <a name="automatic-os-upgrade-execution"></a>Ejecución de actualización automática del sistema operativo
Para ampliar el uso de sondeos de estado de aplicación, las actualizaciones del sistema operativo del conjunto de escalado ejecutan los pasos siguientes:

1. Si hay más de un 20 % de instancias con estado incorrecto, detenga la actualización; en caso contrario, continúe.
2. Identifique el siguiente lote de instancias de máquina virtual para actualizar, donde un lote puede tener como máximo el 20% de la cantidad total de instancias.
3. Actualice el sistema operativo del siguiente lote de instancias de máquina virtual.
4. Si más de un 20 % de las instancias actualizadas tienen estado incorrecto, detenga la actualización; en caso contrario, continúe.
5. En el caso de conjuntos de escalado que no forman parte de un clúster de Service Fabric, la actualización espera hasta 5 minutos a que los sondeos sean correctos y continúa inmediatamente con el lote siguiente. Los conjuntos de escalado que son parte de un clúster de Service Fabric deben esperar 30 minutos antes de pasar al siguiente lote.
6. Si quedan instancias por actualizar, vaya al paso 1) para el siguiente lote; en caso contrario, la actualización está completa.

El motor de actualización del sistema operativo del conjunto de escalado comprueba el estado global de la instancia de la máquina virtual antes de actualizar cada lote. Mientras se actualiza un lote, es posible que estén teniendo lugar de manera simultánea otras tareas de mantenimiento planeadas o no planeadas en los centros de datos de Azure, lo cual podría afectar a la disponibilidad de las máquinas virtuales. Por lo tanto, es posible que más del 20 % de las instancias estén temporalmente fuera de servicio. En tales casos, la actualización del conjunto de escalado se detiene al final del lote actual.


## <a name="deploy-with-a-template"></a>Implementación con una plantilla

Puede usar la plantilla siguiente para implementar un conjunto de escalado que usa las actualizaciones automáticas <a href='https://github.com/Azure/vm-scale-sets/blob/master/preview/upgrade/autoupdate.json'>Actualizaciones graduales automáticas - Ubuntu 16.04-LTS</a>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fvm-scale-sets%2Fmaster%2Fpreview%2Fupgrade%2Fautoupdate.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>


## <a name="next-steps"></a>Pasos siguientes
Para obtener más ejemplos sobre cómo usar las actualizaciones automáticas de sistema operativo con conjuntos de escalado, consulte el [repositorio de GitHub para las características en versión preliminar](https://github.com/Azure/vm-scale-sets/tree/master/preview/upgrade).
