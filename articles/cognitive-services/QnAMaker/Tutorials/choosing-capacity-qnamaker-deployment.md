---
title: 'Elección de capacidad para la implementación de QnA Maker: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Una guía para elegir capacidad para la implementación de QnA Maker
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: b0219b9f7dbbee52406dab9d808134fa2e2a689d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381799"
---
# <a name="choosing-capacity-for-your-qna-maker-deployment"></a>Elegir capacidad para la implementación de QnA Maker

El servicio QnA Maker depende de tres recursos de Azure:
1.  App Service (para el tiempo de ejecución)
2.  Azure Search (para almacenar elementos de QnA)
3.  App Insights (opcional, para almacenar registros de chat y telemetría)

Antes de crear el servicio QnA Maker, debe decidir qué niveles de los servicios anteriores son convenientes en su caso. 

Normalmente hay tres parámetros que necesita tener en cuenta:
1. **El rendimiento necesario del servicio**: seleccione el [plan de aplicación](https://azure.microsoft.com/en-in/pricing/details/app-service/plans/) apropiado para el servicio aplicación en función de sus necesidades. Puede [escalar o reducir verticalmente](https://docs.microsoft.com/en-us/azure/app-service/web-sites-scale) la aplicación. Esto debería influir también en la selección de la SKU de Azure Search SKU; consulte más detalles [aquí](https://docs.microsoft.com/en-us/azure/search/search-sku-tier).

2. **Tamaño y número de bases de conocimiento**: seleccione la [SKU de Azure Search](https://azure.microsoft.com/en-in/pricing/details/search/) apropiada para su escenario. Puede publicar N-1 bases de conocimiento en un nivel particular, donde N se corresponde con los índices máximos permitidos en el nivel. Compruebe también el tamaño y el número máximos de documentos permitidos por cada nivel.

3. **Número de documentos como orígenes**: la SKU gratuita del servicio de administración de QnA Maker limita el número de documentos que puede administrar a través del portal y las API a 3 (de 1 MB cada una). La SKU estándar no tiene ningún límite en relación con el número de documentos que puede administrar. Puede consultar más información [aquí](https://aka.ms/qnamaker-pricing).

En la tabla siguiente se proporcionan algunas directrices de alto nivel.

|                        | Administración de QnA Maker | App Service | Azure Search | Limitaciones                      |
| ---------------------- | -------------------- | ----------- | ------------ | -------------------------------- |
| Experimentación        | SKU gratuita             | Nivel Gratis   | Nivel Gratis    | Publicar hasta 2 KB; tamaño de 50 MB  |
| Entorno de Desarrollo/pruebas   | SKU Estándar         | Compartido      | Básica        | Publicar hasta 4 KB; tamaño de 2 GB    |
| Entorno de producción | SKU Estándar         | Básica       | Estándar     | Publicar hasta 49 KB; tamaño de 25 GB |

Para actualizar la pila de QnA Maker, vea [Actualización del servicio QnA Maker](../How-To/upgrade-qnamaker-service.md).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Actualización del servicio QnA Maker](../How-To/upgrade-qnamaker-service.md)
