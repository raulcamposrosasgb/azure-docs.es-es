---
title: Integrar una solución de supervisión externa con Azure Stack | Microsoft Docs
description: Obtenga información acerca de cómo integrar Azure Stack con una solución de supervisión externa en el centro de datos.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 05/10/2018
ms.author: jeffgilb
ms.reviewer: thoroet
ms.openlocfilehash: d7c8520602132722fd0c7138de4a276b9ac2208a
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2018
ms.locfileid: "34807346"
---
# <a name="integrate-external-monitoring-solution-with-azure-stack"></a>Integrar una solución de supervisión externa con Azure Stack

Para la supervisión externa de la infraestructura de Azure Stack, deberá supervisar el software Azure Stack, los equipos físicos y los modificadores de red físicos. Cada una de estas áreas ofrece un método para recuperar la información sobre el estado de mantenimiento y las alertas:

- El software Azure Stack ofrece una API basada en REST para recuperar el estado de mantenimiento y las alertas. El uso de tecnologías definidas por software, como Espacios de almacenamiento directo, las alertas y el estado de mantenimiento del almacenamiento forma parte de la supervisión de software.
- Los equipos físicos pueden hacer que la información acerca del estado de mantenimiento y las alertas esté disponible a través de los controladores de administración de placa base (BMC).
- Los dispositivos de red físicos pueden hacer que la información acerca del estado de mantenimiento y las alertas esté disponible a través del protocolo SNMP.

Cada solución de Azure Stack se suministra con un host de ciclo de vida de hardware. Este host ejecuta el software de supervisión del fabricante del hardware OEM para los dispositivos de red y servidores físicos. Si quiere, puede omitir estas soluciones de supervisión e integrar directamente las soluciones de supervisión existentes en su centro de datos.

> [!IMPORTANT]
> La solución de supervisión externa que use debe ser sin agente. No puede instalar agentes de terceros dentro de componentes de Azure Stack.

En el siguiente diagrama se muestra el flujo de tráfico entre un sistema integrado de Azure Stack, el host de ciclo de vida de hardware, una solución de supervisión externa y un sistema externo de recopilación de datos y vales.

![Diagrama que muestra el tráfico entre Azure Stack y las soluciones de vales y de supervisión.](media/azure-stack-integrate-monitor/MonitoringIntegration.png)  

En este artículo se explica cómo integrar Azure Stack con soluciones de supervisión externas, como System Center Operations Manager y Nagios. También se incluye cómo trabajar con alertas mediante programación con PowerShell o a través de llamadas a la API de REST.

## <a name="integrate-with-operations-manager"></a>Integración con Operations Manager

Puede usar Operations Manager para realizar una supervisión externa de Azure Stack. El Módulo de administración de System Center para Microsoft Azure Stack le permite supervisar varias implementaciones de Azure Stack con una única instancia de Operations Manager. El módulo de administración usa las API de REST del proveedor de recursos de mantenimiento y actualización para comunicarse con Azure Stack. Si tiene pensado omitir el software de supervisión de OEM que se ejecuta en el host de ciclo de vida de hardware, puede instalar los módulos de administración del proveedor para supervisar los servidores físicos. También puede utilizar la detección de dispositivos de red de Operations Manager para supervisar los modificadores de red.

El módulo de administración de Azure Stack proporciona las siguientes funcionalidades:

- Puede administrar varias implementaciones de Azure Stack.
- Es compatible con Azure Active Directory (Azure AD) o Servicios de federación de Active Directory (AD FS).
- Puede recuperar y cerrar alertas.
- Dispone de un panel de estado de mantenimiento y de un panel de capacidad.
- Incluye la detección del modo de mantenimiento automático para cuando hay revisiones y actualizaciones en curso.
- Incluye tareas de Forzar actualización para implementaciones y regiones.
- Puede agregar información personalizada a una región.
- Admite notificaciones e informes.

Puede descargar el Módulo de administración de System Center para Microsoft Azure Stack y la [guía de usuario](https://www.microsoft.com/en-us/download/details.aspx?id=55184) asociada, o directamente desde Operations Manager.

Para obtener una solución de vales, puede integrar Operations Manager con System Center Service Manager. El conector de producto integrado habilita la comunicación bidireccional que permite cerrar una alerta de Azure Stack y Operations Manager después de resolver una solicitud de servicio en Service Manager.

En el diagrama siguiente se muestra la integración de Azure Stack con una implementación de System Center existente. Puede automatizar Service Manager aún más con System Center Orchestrator o Service Management Automation (SMA) para ejecutar las operaciones en Azure Stack.

![Diagrama que muestra la integración con OM, Service Manager y SMA.](media/azure-stack-integrate-monitor/SystemCenterIntegration.png)

## <a name="integrate-with-nagios"></a>Integración con Nagios

Se ha desarrollado un complemento de supervisión de Nagios junto con las soluciones del asociado Cloudbase, que está disponible bajo la permisiva licencia de software gratuito – MIT (Instituto Tecnológico de Massachusetts).

Este complemento se escribe en Python y aprovecha la API de REST del proveedor de recursos de mantenimiento. Ofrece la funcionalidad básica para recuperar y cerrar alertas de Azure Stack. Al igual que el Módulo de administración de System Center, le permite agregar varias implementaciones de Azure Stack y enviar notificaciones.

El complemento funciona con Nagios Enterprise y Nagios Core. Puede descargarlas [aquí](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details). El sitio de descarga también incluye detalles de instalación y configuración.

### <a name="plugin-parameters"></a>Parámetros del complemento

Configure el archivo de complemento "Azurestack_plugin.py" con los siguientes parámetros:

| . | DESCRIPCIÓN | Ejemplo |
|---------|---------|---------|
| *arm_endpoint* | Punto de conexión de Azure Resource Manager (administrador) |https://adminmanagement.local.azurestack.external |
| *api_endpoint* | Punto de conexión de Azure Resource Manager (administrador)  | https://adminmanagement.local.azurestack.external |
| *Tenant_id* | Identificador de suscripción de administrador | Recuperar a través del portal de administrador o PowerShell |
| *User_name* | Nombre de usuario de la suscripción del operador | operator@myazuredirectory.onmicrosoft.com |
| *User_password* | Contraseña de la suscripción del operador | mypassword |
| *Client_id* | Cliente | 0a7bdc5c-7b57-40be-9939-d4c5fc7cd417* |
| *region* |  Nombre de la región de Azure Stack | local |
|  |  |

*El GUID de PowerShell que se proporciona es universal. Puede utilizarlo para todas las implementaciones.

## <a name="use-powershell-to-monitor-health-and-alerts"></a>Usar PowerShell para supervisar el estado de mantenimiento y las alertas

Si no está utilizando una solución basada en Nagios, Nagios o Operations Manager, puede usar PowerShell para habilitar una amplia gama de soluciones de supervisión que pueden integrarse con Azure Stack.

1. Para usar PowerShell, asegúrese de tener [PowerShell instalado y configurado](azure-stack-powershell-configure-quickstart.md) para un entorno de operador de Azure Stack. Instale PowerShell en un equipo local que alcance el punto de conexión de Resource Manager (administrador) (https://adminmanagement.[region].[External_FQDN]).

2. Ejecute los comandos siguientes para conectarse con el entorno de Azure Stack como un operador de Azure Stack:

   ```PowerShell  
    Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint https://adminmanagement.[Region].[External_FQDN]

   Add-AzureRmAccount -EnvironmentName "AzureStackAdmin"
   ```

3. Utilice comandos como los que se indican en los ejemplos siguientes para trabajar con alertas:
   ```PowerShell
    #Retrieve all alerts
    Get-AzsAlert

    #Filter for active alerts
    $Active=Get-AzsAlert | Where {$_.State -eq "active"}
    $Active

    #Close alert
    Close-AzsAlert -AlertID "ID"

    #Retrieve resource provider health
    Get-AzsRPHealth

    #Retrieve infrastructure role instance health
    $FRPID=Get-AzsRPHealth|Where-Object {$_.DisplayName -eq "Capacity"}
    Get-AzsRegistrationHealth -ServiceRegistrationId $FRPID.RegistrationId

    ```

## <a name="learn-more"></a>Más información

Para obtener información acerca de la supervisión de estado de mantenimiento integrada, consulte [Supervisar el estado y las alertas en Azure Stack](azure-stack-monitor-health.md).

## <a name="next-steps"></a>Pasos siguientes

[Integración de seguridad](azure-stack-integrate-security.md)
