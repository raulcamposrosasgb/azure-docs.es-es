---
title: 'Incorporación de un certificado de entidad de certificación personalizado: Azure API Management | Microsoft Docs'
description: Aprenda a agregar un certificado de entidad de certificación personalizado a Azure API Management.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/20/2018
ms.author: apimpm
ms.openlocfilehash: 9d3399ba6ee724d91117486744ad1431f53edbce
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43053074"
---
# <a name="how-to-add-a-custom-ca-certificate-in-azure-api-management"></a>Incorporación de un certificado de entidad de certificación personalizado a Azure API Management

Azure API Management permite instalar los certificados de entidad de certificación en la máquina dentro de los almacenes de certificados intermedio y raíz de confianza. Esta funcionalidad debe usarse si los servicios requieren un certificado de entidad de certificación personalizado.

En este artículo se muestra cómo administrar certificados de entidad de certificación de una instancia del servicio Azure API Management en Azure Portal.

## <a name="step1"> </a>Carga de un certificado de entidad de certificación

![Incorporación de certificados de entidad de certificación](media/api-management-howto-ca-certificates/00.png)

Siga los pasos que se describen a continuación para cargar un nuevo certificado de entidad de certificación. Si todavía no ha creado una instancia del servicio API Management, consulte el tutorial [Creación de una instancia del servicio API Management](get-started-create-service-instance.md).

1. Vaya a la instancia del servicio Azure API Management en Azure Portal.

2. Seleccione **Certificados de entidad de certificado de entidad de certificación** en el menú.

3. Haga clic en el botón **+ Agregar**.  

    ![Incorporación de certificados de entidad de certificación](media/api-management-howto-ca-certificates/01.png)  

4. Busque el certificado y decida cuál es el almacén de certificados. Solo se necesita la clave pública, no la contraseña.

    ![Incorporación de certificados de entidad de certificación](media/api-management-howto-ca-certificates/02.png)  

5. Haga clic en **Save**(Guardar). Esta operación puede tardar algunos minutos.

    ![Incorporación de certificados de entidad de certificación](media/api-management-howto-ca-certificates/03.png)  

> [!NOTE]
> Puede cargar un certificado de entidad de certificación mediante el comando de Powershell `New-AzureRmApiManagementSystemCertificate`.

## <a name="step1a"></a>Eliminar un certificado de cliente

Para eliminar un certificado, haga clic en el menú contextual **...** y seleccione **Eliminar** junto a este.

![Eliminación de certificados de entidad de certificación](media/api-management-howto-ca-certificates/04.png)  

[Upload a CA certificate]: #step1
[Delete a CA certificate]: #step1a