---
title: Creación de un panel personalizado en Azure Log Analytics | Microsoft Docs
description: Con esta guía le resultará más fácil comprender cómo los paneles de Log Analytics pueden mostrar todas las búsquedas de registros guardadas, lo que le permite ver su entorno en una sola vista.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/08/2017
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 7b17b23d30b15fe71dcf031ac67f0c54fc3cfe3e
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37129319"
---
# <a name="create-a-custom-dashboard-for-use-in-log-analytics"></a>Creación de un panel personalizado para usarse en Log Analytics

Con esta guía le resultará más fácil comprender cómo los paneles de Log Analytics pueden mostrar todas las búsquedas de registros guardadas, lo que le permite ver su entorno en una sola vista.

>[!NOTE]
> Ya no puede editar la característica existente **Mi panel**. Esta característica está en proceso de quedar en desuso.

![Panel de ejemplo](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

Todos los paneles personalizados que cree en el portal de OMS también están disponibles en la aplicación móvil de OMS. Para más información acerca de las aplicaciones, consulte las páginas siguientes.

* [Aplicación móvil de OMS en Microsoft Store](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
* [Aplicación móvil de OMS en Apple iTunes](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![mobile dashboard](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a>¿Cómo creo mi panel?
Para empezar, vaya a la página de información general de OMS. Verá el icono **Mi panel** a la izquierda. Haga clic en él para explorar en profundidad el panel.

![Información general](./media/log-analytics-dashboards/oms-dashboards-overview.png)

## <a name="adding-a-tile"></a>Adición de un mosaico
En los paneles, los mosaicos se generan a partir de las búsquedas de registros guardadas. OMS incluye muchas búsquedas de registros guardadas de creación previa, para que pueda empezar inmediatamente. Los siguientes pasos describen cómo empezar.

En la vista Mi panel, haga clic en **Personalizar** para especificar el modo de personalización.

![Gráfica](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 En el panel que se abre a la derecha de la página se muestran todas las búsquedas de registros guardadas del área de trabajo. Para visualizar una búsqueda de registro guardada como un icono, mantenga el mouse sobre una búsqueda guardada y haga clic en el signo **más**.

![Agregar mosaicos 1](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

Al hacer clic en el signo **más**, aparece un icono nuevo en la vista Mi panel.

![Agregar mosaicos 2](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)

## <a name="edit-a-tile"></a>Editar un mosaico
En la vista Mi panel, haga clic en **Personalizar** para especificar el modo de personalización. Haga clic en el mosaico que quiere editar. El panel derecho cambia a editar y ofrece diferentes opciones:

![Editar mosaico](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Editar mosaico](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a>Visualizaciones de mosaico
Existen tres tipos de visualizaciones de iconos que puede elegir:

| tipo de gráfico | qué hace |
| --- | --- |
| ![Gráfico de barras](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png) |Muestra una escala de tiempo de los resultados de búsquedas de registros guardadas como un gráfico de barras, o una lista de resultados por un campo en función de si la búsqueda de registros agrega los resultados por un campo o no. |
| ![métrica](./media/log-analytics-dashboards/oms-dashboards-metric.png) |Muestra el número total de resultados de la búsqueda de registros como número en un icono. Los mosaicos de métrica permiten establecer un umbral que hará resaltar el mosaico si se alcanza el umbral. |
| ![line](./media/log-analytics-dashboards/oms-dashboards-line.png) |Muestra una escala de tiempo de los resultados de búsquedas de registros guardadas con los valores en forma de gráfico de líneas. |

### <a name="threshold"></a>Umbral
Puede crear un umbral en un mosaico mediante la visualización Métrica. Seleccione esta opción para crear un valor de umbral en el mosaico. Elija si quiere resaltar el mosaico cuando el valor está por encima o por debajo del umbral seleccionado y, luego, establezca el valor del umbral.

## <a name="organizing-the-dashboard"></a>Organización del panel
Para organizar el panel, vaya a la vista Mi panel y haga clic en **Personalizar** para entrar en el modo de personalización. Haga clic y arrastre el mosaico que quiere mover y muévalo hasta donde quiere que esté.

![Organizar el panel](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a>Quitar un mosaico
Para quitar un icono, vaya a la vista Mi panel y haga clic en **Personalizar** para entrar en el modo de personalización. Seleccione el icono que quiere quitar y, luego, en el panel de la derecha, seleccione **Quitar icono**.

![Quitar un mosaico](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a>Pasos siguientes
* Cree [alertas](log-analytics-alerts.md) en Log Analytics para generar notificaciones y solucionar problemas.
