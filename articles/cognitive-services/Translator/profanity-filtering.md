---
title: Filtrado de blasfemias en Microsoft Translator Text API | Microsoft Docs
description: Use el filtrado de blasfemias en Microsoft Translator Text API.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: a7172e1e8aa336c011fb06e93fc5c4b54d26a3cd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380818"
---
# <a name="how-to-add-profanity-filtering-with-the-microsoft-translator-text-api"></a>Cómo agregar filtrados de blasfemias en Microsoft Translator Text API

Normalmente, el servicio de Translator conserva las blasfemias que están presentes en el origen de la traducción. El grado de blasfemia y el contexto que hace que las palabras sean soeces difieren entre las distintas culturas. Como consecuencia, el grado de blasfemia en el lenguaje de destino podría ampliarse o reducirse.

Si quiere evitar obtener blasfemias en la traducción (independientemente de su presencia en el texto de origen), puede usar la opción de filtrado de blasfemias en el método Translate(). La opción le permite elegir si quiere que las blasfemias se eliminen o se marquen con etiquetas adecuadas, o bien si no quiere realizar ninguna acción.

El método Translate() toma un parámetro "options", que contiene el nuevo elemento "ProfanityAction". Los valores aceptados de ProfanityAction son "NoAction", "Marked" y "Deleted".

## <a name="accepted-values-of-profanityaction-and-examples"></a>Valores aceptados de ProfanityAction y ejemplos
|Valor de ProfanityAction | . | Ejemplo: origen en japonés | Ejemplo: destino en español|
| :---|:---|:---|:---|
| NoAction | Predeterminada. Igual que si no se configura la opción. Las blasfemias pasan del origen al destino. | 彼は変態です。 | Es un estúpido. |
| Marked | Las palabras soeces aparecerán rodeadas por etiquetas XML \<profanity> … \</profanity>. | 彼は変態です。 | Es un \<profanity>estúpido\</profanity>. |
| Deleted | Las palabras soeces se quitan de la salida sin reemplazo. | 彼は。 | Es un. |

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Aplicar filtrado de blasfemias con la llamada a Translator API](reference/v3-0-translate.md)

