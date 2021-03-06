---
title: Configuración de orígenes de datos en Azure Log Analytics | Microsoft Docs
description: Los orígenes de datos definen los datos que Log Analytics recopila de agentes y otros orígenes conectados.  En este artículo se describe el concepto de cómo Log Analytics usa los orígenes de datos, se explican los detalles de cómo configurarlos y se brinda un resumen de los distintos orígenes de datos disponibles.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 67710115-c861-40f8-a377-57c7fa6909b4
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/26/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 4b7b1a9dc25b1bfaf72ab67dd0725a4518263ca5
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "42143409"
---
# <a name="data-sources-in-log-analytics"></a>Orígenes de datos en Log Analytics
Log Analytics recopila datos de los orígenes conectados y los almacena en el área de trabajo de Log Analytics.  Los orígenes de datos que configura definen los datos que se recopilan de cada uno de ellos.  Los datos de Log Analytics se almacenan como un conjunto de registros.  Cada origen de datos crea registros de un tipo determinado, donde cada tipo tiene su propio conjunto de propiedades.

![Recopilación de datos de Log Analytics](./media/log-analytics-data-sources/overview.png)

Los orígenes de datos son distintos de las [soluciones de administración](log-analytics-add-solutions.md), que también recopilan datos de los orígenes conectados y crean registros en Log Analytics.  Además de recopilar datos, las soluciones incluyen normalmente búsquedas y vistas de registros que ayudan a analizar el funcionamiento de una aplicación o servicio determinados.


## <a name="summary-of-data-sources"></a>Resumen de orígenes de datos
En la tabla siguiente se enumeran los orígenes de datos que actualmente se encuentran disponibles en Log Analytics.  Cada uno de ellos tiene un vínculo a un artículo independiente, donde se proporcionan detalles con respecto al origen de datos determinado.   También se proporciona información sobre el método y la frecuencia de recopilación de datos en Log Analytics.  Puede usar la información de este artículo para identificar las diferentes soluciones disponibles y comprender los requisitos de conexión y flujo de datos de las distintas soluciones de administración. Para una explicación de las columnas, consulte [Data collection details for management solutions in Azure](../monitoring/monitoring-solutions-inventory.md) (Detalles de la recopilación de datos para las soluciones de administración en Azure).


| Origen de datos | Plataforma | Microsoft Monitoring Agent | Agente de Operations Manager | Almacenamiento de Azure | ¿Se requiere Operations Manager? | Se envían los datos del agente de Operations Manager a través del grupo de administración | Frecuencia de recopilación |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [Registros personalizados](log-analytics-data-sources-custom-logs.md) | Windows |&#8226; |  | |  |  | a la llegada |
| [Registros personalizados](log-analytics-data-sources-custom-logs.md) | Linux   |&#8226; |  | |  |  | a la llegada |
| [Registros de IIS](log-analytics-data-sources-iis-logs.md) | Windows |&#8226; |&#8226; |&#8226; |  |  |depende de la configuración de la sustitución incremental de archivos de registro |
| [Contadores de rendimiento](log-analytics-data-sources-performance-counters.md) | Windows |&#8226; |&#8226; |  |  |  |mínimo de 10 segundos, según lo programado |
| [Contadores de rendimiento](log-analytics-data-sources-performance-counters.md) | Linux |&#8226; |  |  |  |  |mínimo de 10 segundos, según lo programado |
| [Syslog](log-analytics-data-sources-syslog.md) | Linux |&#8226; |  |  |  |  |De Almacenamiento de Azure: 10 minutos; del agente: a la llegada |
| [Registros de eventos de Windows](log-analytics-data-sources-windows-events.md) |Windows |&#8226; |&#8226; |&#8226; |  |&#8226; | a la llegada |


## <a name="configuring-data-sources"></a>Configuración de orígenes de datos
Los orígenes de datos se configuran en el menú **Datos** en **Configuración avanzada** de Log Analytics.  Cualquier configuración se proporciona a todos los orígenes conectados del área de trabajo.  Actualmente, no puede excluir ningún agente de esta configuración.

![Configurar eventos de Windows](./media/log-analytics-data-sources/configure-events.png)

1. En Azure Portal, seleccione **Log Analytics** > el área de trabajo > **Configuración avanzada**.
2. Seleccione **Datos**.
3. Haga clic en el origen de datos que quiere configurar.
4. Siga el vínculo a la documentación correspondiente a cada origen de datos de la tabla anterior para obtener detalles sobre su configuración.


## <a name="data-collection"></a>Colección de datos
Las configuraciones de orígenes de datos se entregan en cuestión de minutos a los agentes que están directamente conectados con Log Analytics.  Los datos especificados se recopilan desde el agente y se entregan directamente a Log Analytics a intervalos específicos a cada origen de datos.  Consulte la documentación de cada origen de datos para ver estas especificaciones.

En el caso de los agentes de System Center Operations Manager de un grupo de administración conectado, la configuración de origen de datos se convierte en módulos de administración y se proporciona al grupo de administración cada 5 minutos, de manera predeterminada.  El agente descarga el módulo de administración de la forma habitual y recopila los datos especificados. En función del origen de datos, los datos se enviarán a un servidor de administración que los reenvía a Log Analytics o el agente los enviará a Log Analytics sin pasar por el servidor de administración. Consulte [Detalles de la recopilación de datos para las soluciones de administración en Azure](../monitoring/monitoring-solutions-inventory.md) para más información.  Puede leer detalles sobre cómo conectar Operations Manager y Log Analytics y modificar la frecuencia con que la configuración se proporciona en [Configuración de integración con System Center Operations Manager](log-analytics-om-agents.md).

Si el agente no puede conectarse a Log Analytics u Operations Manager, continuará recopilando datos para entregarlos cuando se establezca la conexión.  Si el volumen de datos alcanza el tamaño máximo de la memoria caché del cliente o si el agente no es capaz de establecer conexión en un plazo de 24 horas, los datos pueden perderse.

## <a name="log-analytics-records"></a>Registros de Log Analytics
Todos los datos que recopila Log Analytics se almacenan en el área de trabajo como registros.  Los registros que recopilan distintos orígenes de datos tendrán su propio conjunto de propiedades y se identificar por su propiedad **Type** .  Consulte la documentación de cada origen de datos y solución para obtener detalles sobre cada tipo de registro.

## <a name="next-steps"></a>Pasos siguientes
* Conozca las [soluciones](../monitoring/monitoring-solutions.md) que agregan funcionalidad a Log Analytics y que también recopilan datos del área de trabajo.
* Obtenga información acerca de las [búsquedas de registros](log-analytics-log-searches.md) para analizar los datos recopilados de soluciones y orígenes de datos.  
* Configure [alertas](log-analytics-alerts.md) que le notifiquen de manera proactiva acerca de los datos críticos recopilados de soluciones y orígenes de datos.
