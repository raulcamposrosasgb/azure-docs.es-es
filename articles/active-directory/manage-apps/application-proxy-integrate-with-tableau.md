---
title: Azure Active Directory Application Proxy y Tableau | Microsoft Docs
description: Obtenga información sobre cómo usar Azure Active Directory (Azure AD) Application Proxy para proporcionar acceso remoto para la implementación de Tableau.
services: active-directory
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: barbkess
ms.reviewer: japere
ms.custom: it-pro
ms.openlocfilehash: a68b0465acdb416cd953e22d7f024eb399c94493
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2018
ms.locfileid: "40246612"
---
# <a name="azure-active-directory-application-proxy-and-tableau"></a>Azure Active Directory Application Proxy y Tableau 

Azure Active Directory Application Proxy y Tableau se han asociado para garantizar que pueda usar fácilmente Application Proxy para proporcionar acceso remoto a la implementación de Tableau. En este artículo se explica cómo configurar este escenario.  

## <a name="prerequisites"></a>Requisitos previos 

El escenario de este artículo supone que:

- [Tableau](https://onlinehelp.tableau.com/current/server/en-us/proxy.htm#azure) está configurado. 

- Tiene instalado un[conector Application Proxy](application-proxy-enable.md). 

 
## <a name="enabling-application-proxy-for-tableau"></a>Habilitación de Application Proxy para Tableau 

El proxy de aplicación es compatible con el flujo de concesión de OAuth 2.0, que es necesario para que Tableau funcione correctamente. Esto significa que ya no hay pasos especiales para habilitar esta aplicación, excepto por su configuración siguiendo los pasos de publicación siguientes.


## <a name="publish-your-applications-in-azure"></a>Publicación de aplicaciones en Azure 

Para publicar Tableau, debe publicar una aplicación en Azure Portal.

Si desea:

- Para instrucciones detalladas del paso 1 al 8, consulte [Publicación de aplicaciones mediante Azure AD Application Proxy](application-proxy-publish-azure-portal.md). 
- Para obtener información sobre cómo encontrar los valores de Tableau para los campos de Application Proxy, consulte la documentación de Tableau.  

**Para publicar la aplicación**: 


1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador global. 

2. Seleccione **Azure Active Directory > Aplicaciones empresariales**. 

3. Seleccione **Agregar** en la parte superior de la hoja. 

4. Seleccione **Aplicación local**. 

5. Rellene los campos requeridos con la información de la aplicación nueva. Utilice las siguientes instrucciones para realizar la configuración: 

    - **Dirección URL interna**: esta aplicación debe tener una dirección URL interna que es la propia dirección URL de Tableau. Por ejemplo, `https://adventure-works.tableau.com`. 

    - **Método de autenticación previa**: Azure Active Directory (se recomienda, pero no es obligatorio). 

6. Seleccione **Agregar** en la parte superior de la hoja. Se agrega la aplicación y se abre el menú de inicio rápido. 

7. En dicho menú, seleccione **Asignar un usuario para las pruebas**  y agregue al menos un usuario a la aplicación. Asegúrese de que esta cuenta de prueba tiene acceso a la aplicación local. 

8. Seleccione **Asignar** para guardar la asignación del usuario de prueba. 

9. (Opcional) En la página de administración de la aplicación, seleccione **Inicio de sesión único**. Elija **Autenticación integrada de Windows** en el menú desplegable y rellene los campos obligatorios según la configuración de Tableau. Seleccione **Guardar**. 

 

## <a name="testing"></a>Prueba 

La aplicación ya se puede probar. Acceda a la dirección URL externa que usó para publicar Tableu e inicie sesión como usuario asignado a ambas aplicaciones.



## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre Azure AD Application Proxy, consulte [Provisión de acceso remoto seguro a aplicaciones locales](application-proxy.md).

