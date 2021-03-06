---
title: Métricas de Azure Stack compatibles con Azure Monitor | Microsoft Docs
description: Descubra las métricas de Azure Stack compatibles con Azure Monitor.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: mabrigg
ms.openlocfilehash: 0cd8d309cfbf72a05c83c2a536d754e9cbc6e008
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2018
ms.locfileid: "44022666"
---
# <a name="supported-metrics-with-azure-monitor-on-azure-stack"></a>Métricas de Azure Stack compatibles con Azure Monitor

*Se aplica a: sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Puede recuperar las métricas de Azure Monitor en Azure Stack igual que en Azure. Puede ver las medidas en el portal, obtenerlas con la API REST o consultarlas con PowerShell o la CLI.

En las tablas siguientes, se muestran las métricas disponibles con la canalización de métricas de Azure Monitor en Azure Stack. Para consultar estas métricas y acceder a ellas, necesita la versión **2018-01-01** del perfil de la API. Para más información sobre los perfiles de API y Azure Stack, consulte [Administración de perfiles de versión de API en Azure Stack](azure-stack-version-profiles.md).

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

| Métrica | Nombre de métrica para mostrar | Unidad | Tipo de agregación | DESCRIPCIÓN | Dimensiones |
|----------------|---------------------|---------|------------------|-----------------------------------------------------------------------------------------------|---------------|
| Porcentaje de CPU | Porcentaje de CPU | Percent | Media | El porcentaje de unidades de proceso asignadas que las máquinas virtuales usan actualmente | Sin dimensiones |

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

| Métrica | Nombre de métrica para mostrar | Unidad | Tipo de agregación | DESCRIPCIÓN | Dimensiones |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| UsedCapacity | Capacidad usada | Bytes | Media | Capacidad usada de la cuenta | Sin dimensiones |
| Transacciones | Transacciones | Recuento | Total | El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta. | ResponseType, GeoType, ApiName |
| Entrada | Entrada | Bytes | Total | La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure. | GeoType, ApiName |
| Salida | Salida | Bytes | Total | La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables. | GeoType, ApiName |
| SuccessServerLatency | Latencia del servidor correcta | Milisegundos | Media | La latencia media utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en AverageE2ELatency. | GeoType, ApiName |
| SuccessE2ELatency | Latencia de E2E correcta | Milisegundos | Media | La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta. | GeoType, ApiName |
| Disponibilidad | Disponibilidad | Percent | Media | El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada. | GeoType, ApiName |

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

| Métrica | Nombre de métrica para mostrar | Unidad | Tipo de agregación | DESCRIPCIÓN | Dimensiones |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| BlobCapacity | Capacidad de Blob | Bytes | Total | La cantidad de almacenamiento que ha utilizado el Blob service de la cuenta de almacenamiento, en bytes. | BlobType |
| BlobCount | Recuento de blobs | Recuento | Total | El número de blobs en el Blob service de la cuenta de almacenamiento. | BlobType |
| ContainerCount | Recuento de contenedores de blobs | Recuento | Media | El número de contenedores en el Blob service de la cuenta de almacenamiento. | Sin dimensiones |
| Transacciones | Transacciones | Recuento | Total | El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta. | ResponseType, GeoType, ApiName |
| Entrada | Entrada | Bytes | Total | La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure. | GeoType, ApiName |
| Salida | Salida | Bytes | Total | La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables. | GeoType, ApiName |
| SuccessServerLatency | Latencia del servidor correcta | Milisegundos | Media | La latencia media utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en AverageE2ELatency. | GeoType, ApiName |
| SuccessE2ELatency | Latencia de E2E correcta | Milisegundos | Media | La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta. | GeoType, ApiName |
| Disponibilidad | Disponibilidad | Percent | Media | El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada. | GeoType, ApiName |

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

| Métrica | Nombre de métrica para mostrar | Unidad | Tipo de agregación | DESCRIPCIÓN | Dimensiones |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| TableCapacity | Capacidad de Table | Bytes | Media | La cantidad de almacenamiento que ha utilizado el servicio Table de la cuenta de almacenamiento, en bytes. | Sin dimensiones |
| TableCount | Recuento de tablas | Recuento | Media | El número de tablas en el servicio Table de la cuenta de almacenamiento. | Sin dimensiones |
| TableEntityCount | Recuento de entidades de Table | Recuento | Media | El número de entidades de tabla en el servicio Table de la cuenta de almacenamiento. | Sin dimensiones |
| Transacciones | Transacciones | Recuento | Total | El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta. | ResponseType, GeoType, ApiName |
| Entrada | Entrada | Bytes | Total | La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure. | GeoType, ApiName |
| Salida | Salida | Bytes | Total | La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables. | GeoType, ApiName |
| SuccessServerLatency | Latencia del servidor correcta | Milisegundos | Media | La latencia media utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en AverageE2ELatency. | GeoType, ApiName |
| SuccessE2ELatency | Latencia de E2E correcta | Milisegundos | Media | La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta. | GeoType, ApiName |
| Disponibilidad | Disponibilidad | Percent | Media | El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada. | GeoType, ApiName |

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

| Métrica | Nombre de métrica para mostrar | Unidad | Tipo de agregación | DESCRIPCIÓN | Dimensiones |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| QueueCapacity | Capacidad de Queue | Bytes | Media | La cantidad de almacenamiento que ha utilizado el Queue service de la cuenta de almacenamiento, en bytes. | Sin dimensiones |
| QueueCount | Recuento de colas | Recuento | Media | El número de colas en el Queue service de la cuenta de almacenamiento. | Sin dimensiones |
| QueueMessageCount | Recuento de mensajes de Queue | Recuento | Media | El número aproximado de mensajes de cola en el Queue service de la cuenta de almacenamiento. | Sin dimensiones |
| Transacciones | Transacciones | Recuento | Total | El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta. | ResponseType, GeoType, ApiName |
| Entrada | Entrada | Bytes | Total | La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure. | GeoType, ApiName |
| Salida | Salida | Bytes | Total | La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables. | GeoType, ApiName |
| SuccessServerLatency | Latencia del servidor correcta | Milisegundos | Media | La latencia media utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en AverageE2ELatency. | GeoType, ApiName |
| SuccessE2ELatency | Latencia de E2E correcta | Milisegundos | Media | La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta. | GeoType, ApiName |
| Disponibilidad | Disponibilidad | Percent | Media | El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada. | GeoType, ApiName |

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre [Azure Monitor en Azure Stack](azure-stack-metrics-azure-data.md).