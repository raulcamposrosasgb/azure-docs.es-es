---
title: Punto de conexión de Autosuggest| Microsoft Docs
description: Resumen del punto de conexión de Autosuggest API.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-autosuggest
ms.topic: article
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: 5aaddd09006cb6f1812bb6ae213a2f5e6720fb1b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382086"
---
# <a name="autosuggest-endpoint"></a>Punto de conexión de Autosuggest

**Autosuggest API** incluye un punto de conexión, que devuelve una lista de consultas sugeridas a partir de un término de búsqueda parcial.

## <a name="endpoint"></a>Punto de conexión

Para obtener consultas sugeridas mediante la API de Bing, envíe una solicitud `GET` al punto de conexión siguiente. Use los encabezados y parámetros de dirección URL para definir especificaciones adicionales.

**Punto de conexión**: devuelve sugerencias de búsqueda como resultados JSON relacionados con la entrada del usuario definida por `?q=""`.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/Suggestions 
```

Para obtener más información sobre encabezados, parámetros, códigos de mercado, objetos de respuesta, errores etc., consulte la referencia [Bing Autosuggest API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference).

## <a name="response-json"></a>JSON de respuesta

La respuesta a una solicitud de sugerencias automáticas incluye resultados como objetos JSON. Para obtener ejemplos de análisis de los resultados, consulte el [tutorial](tutorials/autosuggest.md) y el [código fuente](tutorials/autosuggest-source.md).

## <a name="next-steps"></a>Pasos siguientes

Las API de **Bing** admiten acciones de búsqueda que devuelven resultados según su tipo. Todos los puntos de conexión de búsqueda devuelven resultados como objetos de respuesta JSON.
Todos los puntos de conexión admiten consultas que devuelven un idioma o una ubicación en concreto por longitud, latitud y radio de búsqueda.

Para información completa sobre los parámetros admitidos por cada punto de conexión, consulte las páginas de referencia de cada tipo.
Para obtener ejemplos de solicitudes básicas mediante Autosuggest API, consulte los [inicios rápidos de Autosuggest](https://docs.microsoft.com/azure/cognitive-services/Bing-Autosuggest).