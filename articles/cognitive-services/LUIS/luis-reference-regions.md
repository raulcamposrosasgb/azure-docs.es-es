---
title: Regiones y puntos de conexión de Language Understanding (LUIS) - Azure - Cognitive Services LUIS | Microsoft Docs
titleSuffix: Azure
description: La región en la que publique la aplicación de LUIS corresponde a la región o ubicación que especifique en Azure Portal al crear una clave de punto de conexión de LUIS para Azure. Al publicar una aplicación, LUIS genera automáticamente una dirección URL de punto de conexión para la región asociada a la clave. Para publicar una aplicación de LUIS en más de una región, necesitará al menos una clave por región.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 08/23/2018
ms.author: diberry
ms.openlocfilehash: 7812ce37cb22c8774c785f5f645b8fef90b02a3e
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42815510"
---
# <a name="regions-and-keys"></a>Regiones y claves

La región en la que publique la aplicación de LUIS corresponde a la región o ubicación que especifique en Azure Portal al crear una clave de punto de conexión de LUIS para Azure. Al [publicar una aplicación](./luis-how-to-publish-app.md), LUIS genera automáticamente una dirección URL de punto de conexión para la región asociada a la clave. Para publicar una aplicación de LUIS en más de una región, necesitará al menos una clave por región. 

## <a name="luis-website"></a>Sitio web de LUIS
Hay tres sitios web de LUIS, en función de la región. Debe crear y publicar el contenido en la misma región. 

|LUIS|Region|
|--|--|
|[www.luis.ai][www.luis.ai]|Datos de agencias y<br>Fuera de Europa<br>Fuera de Australia|
|[au.luis.ai][au.luis.ai]|Australia|
|[eu.luis.ai][eu.luis.ai]|Europa|


## <a name="publishing-regions"></a>Regiones de publicación

Las aplicaciones de LUIS creadas en https://www.luis.ai se pueden publicar en todos los puntos de conexión, salvo en [Europa](#publishing-to-europe) y [Australia](#publishing-to-australia). 

La aplicación de regiones de creación solo se puede publicar en la región de publicación correspondiente. Si la aplicación está en la región de creación incorrecta, expórtela e impórtela en la región de creación correcta para su región de publicación. 

 Región global | Región de creación | Región de publicación y de consulta   |   Sitio web de LUIS | Formato de dirección URL de punto de conexión   |
|-----|------|------|------|------|
| Asia | Oeste de EE. UU.| Asia oriental     | [www.luis.ai][www.luis.ai] |  https://eastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Asia | Oeste de EE. UU.| Sudeste asiático     | [www.luis.ai][www.luis.ai] |   https://southeastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| *[Australia](#publishing-to-australia) | Este de Australia| Este de Australia     |   [au.luis.ai][au.luis.ai] | https://australiaeast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| *[Europa](#publishing-to-europe)| Europa occidental| Europa del Norte     | [eu.luis.ai][eu.luis.ai]|  https://northeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| *[Europa](#publishing-to-europe) | Europa occidental| Europa occidental     | [eu.luis.ai][eu.luis.ai]|  https://westeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| Norteamérica | Oeste de EE. UU. | Este de EE. UU      |[www.luis.ai][www.luis.ai] |   https://eastus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Norteamérica | Oeste de EE. UU. | Este de EE. UU. 2     | [www.luis.ai][www.luis.ai] |  https://eastus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Norteamérica | Oeste de EE. UU. | Centro-Sur de EE. UU     | [www.luis.ai][www.luis.ai] |  https://southcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| Norteamérica | Oeste de EE. UU. | Centro occidental de EE.UU.     |[www.luis.ai][www.luis.ai] |  https://westcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Norteamérica | Oeste de EE. UU. | Oeste de EE. UU. |  [www.luis.ai][www.luis.ai] | https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| Norteamérica | Oeste de EE. UU. | Oeste de EE. UU. 2    | [www.luis.ai][www.luis.ai] |  https://westus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| Sudamérica | Oeste de EE. UU. | Sur de Brasil     | [www.luis.ai][www.luis.ai] |  https://brazilsouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |

## <a name="publishing-to-europe"></a>Publicación en Europa

Para publicar en las regiones europeas, solo debe crear aplicaciones de LUIS en https://eu.luis.ai. Si intenta publicar en cualquier otro lugar con una clave en la región europea, LUIS mostrará un mensaje de advertencia. En su lugar, use https://eu.luis.ai. Las aplicaciones de LUIS creadas en [https://eu.luis.ai][eu.luis.ai] no se migran automáticamente a otras regiones. Exporte e importe la aplicación de LUIS para poder migrarla.

## <a name="publishing-to-australia"></a>Publicación en Australia

Para publicar en las regiones australianas, solo debe crear aplicaciones de LUIS en https://au.luis.ai. Si intenta publicar en cualquier otro lugar con una clave en la región australiana, LUIS mostrará un mensaje de advertencia. En su lugar, use https://au.luis.ai. Las aplicaciones de LUIS creadas en [https://au.luis.ai][au.luis.ai] no se migran automáticamente a otras regiones. Exporte e importe la aplicación de LUIS para poder migrarla.

## <a name="endpoints"></a>Puntos de conexión

LUIS actualmente tiene dos puntos de conexión: uno para la creación y otro para el análisis de texto.

|Propósito|URL|
|--|--|
|Creación|`https://{region}.api.cognitive.microsoft.com/luis/api/v2.0/apps/{appID}/`|
|Análisis de texto (predicción de consultas)|`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/{appId}?q={q}[&timezoneOffset][&verbose][&spellCheck][&staging][&bing-spell-check-subscription-key][&log]`|

En la tabla siguiente se explican los parámetros que se muestran entre llaves `{}` en la tabla anterior.

|Parámetro|Propósito|
|--|--|
|region|Región de Azure: la creación y publicación tienen diferentes regiones|
|appID|Identificador de la aplicación de LUIS utilizado en la ruta de la dirección URL que se encuentra en el panel de la aplicación|
|q|Texto de expresión enviado desde la aplicación cliente, como un bot de chat|


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de entidades creadas previamente](./luis-reference-prebuilt-entities.md)

 [www.luis.ai]: https://www.luis.ai
 [au.luis.ai]: https://au.luis.ai
 [eu.luis.ai]: https://eu.luis.ai