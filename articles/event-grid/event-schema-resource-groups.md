---
title: Esquema de eventos del grupo de recursos de Azure Event Grid
description: Describe las propiedades que se proporcionan para los eventos del grupo de recursos con Azure Event Grid
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: reference
ms.date: 08/17/2018
ms.author: tomfitz
ms.openlocfilehash: 22629ba553cc58435f99ed0fed97be252b24b409
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2018
ms.locfileid: "42146976"
---
# <a name="azure-event-grid-event-schema-for-resource-groups"></a>Esquema de eventos de Azure Event Grid para grupos de recursos

En este artículo se proporcionan las propiedades y los esquemas de los eventos de grupo de recursos. Para una introducción a los esquemas de eventos, consulte [Esquema de eventos de Azure Event Grid](event-schema.md).

Las suscripciones y los grupos de recursos de Azure emiten los mismos tipos de eventos. Los tipos de eventos están relacionados con los cambios en los recursos. La diferencia principal es que los grupos de recursos emiten eventos para los recursos en el grupo de recursos y las suscripciones de Azure emiten eventos para los recursos a través de la suscripción.

Los eventos de recursos se crean para las operaciones PUT, PATCH y DELETE que se envían a `management.azure.com`. Las operaciones GET y POST no crean eventos. Las operaciones enviadas al plan de datos (como `myaccount.blob.core.windows.net`) no crean eventos.

Cuando se suscribe a eventos para un grupo de recursos, el punto de conexión recibe todos los eventos de ese grupo de recursos. Los eventos pueden incluir el evento que desea ver, como la actualización de una máquina virtual, pero también los eventos que quizá no son importantes para usted, como escribir una nueva entrada en el historial de implementación. Puede recibir todos los eventos en el punto de conexión y escribir código que procesa los eventos que desea controlar, o puede establecer un filtro al crear la suscripción de eventos.

Para controlar los eventos mediante programación, puede ordenarlos examinando el valor `operationName`. Por ejemplo, el punto de conexión de eventos podría procesar solamente eventos para las operaciones que son iguales a `Microsoft.Compute/virtualMachines/write` o `Microsoft.Storage/storageAccounts/write`.

El asunto del evento es el identificador de recurso correspondiente al recurso que es el destino de la operación. Para filtrar eventos para un recurso, proporcione ese identificador de recurso creando la suscripción de eventos.  Para filtrar por un tipo de recurso, use un valor en el formato siguiente: `/subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.Compute/virtualMachines`

Para ver una lista de scripts de ejemplo y tutoriales, consulte el [origen de eventos de grupo de recursos](event-sources.md#resource-groups).

## <a name="available-event-types"></a>Tipos de eventos disponibles

Los grupos de recursos emiten eventos de administración desde Azure Resource Manager, como cuando se crea una máquina virtual o se elimina una cuenta de almacenamiento.

| Tipo de evento | DESCRIPCIÓN |
| ---------- | ----------- |
| Microsoft.Resources.ResourceWriteSuccess | Se genera cuando una operación de actualización o de creación de un recurso se realiza correctamente. |
| Microsoft.Resources.ResourceWriteFailure | Se genera cuando una operación de actualización o de creación de un recurso da error. |
| Microsoft.Resources.ResourceWriteCancel | Se genera cuando se cancela una operación de actualización o de creación de un recurso. |
| Microsoft.Resources.ResourceDeleteSuccess | Se genera cuando una operación de eliminación de un recurso tiene éxito. |
| Microsoft.Resources.ResourceDeleteFailure | Se genera cuando una operación de eliminación de un recurso da error. |
| Microsoft.Resources.ResourceDeleteCancel | Se genera cuando se cancela una operación de eliminación de un recurso. Este evento sucede cuando se cancela la implementación de una plantilla. |

## <a name="example-event"></a>Evento de ejemplo

En el ejemplo siguiente se muestra el esquema para un evento **ResourceWriteSuccess**. Se usa el mismo esquema para eventos **ResourceWriteFailure** y **ResourceWriteCancel** con valores diferentes para `eventType`.

```json
[{
  "subject": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
  "eventType": "Microsoft.Resources.ResourceWriteSuccess",
  "eventTime": "2018-07-19T18:38:04.6117357Z",
  "id": "4db48cba-50a2-455a-93b4-de41a3b5b7f6",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
      "action": "Microsoft.Storage/storageAccounts/write",
      "evidence": {
        "role": "Subscription Admin"
      }
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "_claim_names": "{\"groups\":\"src1\"}",
      "_claim_sources": "{\"src1\":{\"endpoint\":\"{URI}\"}}",
      "http://schemas.microsoft.com/claims/authnclassreference": "1",
      "aio": "{token}",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "{ID}",
      "e_exp": "{expiration}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "{last-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "{first-name}",
      "ipaddr": "{IP-address}",
      "name": "{full-name}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "onprem_sid": "{ID}",
      "puid": "{ID}",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",
      "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "{user-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "{user-name}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "resourceProvider": "Microsoft.Storage",
    "resourceUri": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
    "operationName": "Microsoft.Storage/storageAccounts/write",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}"
}]
```

En el ejemplo siguiente se muestra el esquema para un evento **ResourceDeleteSuccess**. Se usa el mismo esquema para eventos **ResourceDeleteFailure** y **ResourceDeleteCancel** con valores diferentes para `eventType`.

```json
[{
  "subject": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
  "eventType": "Microsoft.Resources.ResourceDeleteSuccess",
  "eventTime": "2018-07-19T19:24:12.763881Z",
  "id": "19a69642-1aad-4a96-a5ab-8d05494513ce",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
      "action": "Microsoft.Storage/storageAccounts/delete",
      "evidence": {
        "role": "Subscription Admin"
      }
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "_claim_names": "{\"groups\":\"src1\"}",
      "_claim_sources": "{\"src1\":{\"endpoint\":\"{URI}\"}}",
      "http://schemas.microsoft.com/claims/authnclassreference": "1",
      "aio": "{token}",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "{ID}",
      "e_exp": "262800",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "{last-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "{first-name}",
      "ipaddr": "{IP-address}",
      "name": "{full-name}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "onprem_sid": "{ID}",
      "puid": "{ID}",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",
      "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "{user-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "{user-name}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "httpRequest": {
      "clientRequestId": "{ID}",
      "clientIpAddress": "{IP-address}",
      "method": "DELETE",
      "url": "https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2018-02-01"
    },
    "resourceProvider": "Microsoft.Storage",
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
    "operationName": "Microsoft.Storage/storageAccounts/delete",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}"
}]
```

## <a name="event-properties"></a>Propiedades de evento

Un evento tiene los siguientes datos de nivel superior:

| Propiedad | Escriba | DESCRIPCIÓN |
| -------- | ---- | ----------- |
| topic | string | Ruta de acceso completa a los recursos del origen del evento. En este campo no se puede escribir. Event Grid proporciona este valor. |
| subject | string | Ruta al asunto del evento definida por el anunciante. |
| eventType | string | Uno de los tipos de eventos registrados para este origen de eventos. |
| eventTime | string | La hora de generación del evento en función de la hora UTC del proveedor. |
| id | string | Identificador único para el evento |
| data | objeto | Datos de eventos de grupo de recursos. |
| dataVersion | string | Versión del esquema del objeto de datos. El publicador define la versión del esquema. |
| metadataVersion | string | Versión del esquema de los metadatos del evento. Event Grid define el esquema de las propiedades de nivel superior. Event Grid proporciona este valor. |

El objeto data tiene las siguientes propiedades:

| Propiedad | Escriba | DESCRIPCIÓN |
| -------- | ---- | ----------- |
| authorization | objeto | Autorización solicitada para la operación. |
| claims | objeto | Propiedades de las notificaciones. Para más información, consulte la [especificación de JWT](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html). |
| correlationId | string | Identificador de operación para solucionar el problema. |
| httpRequest | objeto | Detalles de la operación. Este objeto solo se incluye al actualizar un recurso existente o eliminar un recurso. |
| resourceProvider | string | Proveedor de recursos que se realiza la operación. |
| resourceUri | string | URI del recurso en la operación. |
| operationName | string | Operación que se realizó. |
| status | string | Estado de la operación. |
| subscriptionId | string | Identificador de suscripción del recurso. |
| tenantId | string | Identificador de inquilino del recurso. |

## <a name="next-steps"></a>Pasos siguientes

* Para una introducción a Azure Event Grid, consulte [Introducción a Azure Event Grid](overview.md).
* Para más información acerca de la creación de una suscripción de Azure Event Grid, consulte [Esquema de suscripción de Event Grid](subscription-creation-schema.md).
