---
title: Registrar su suscripción de Azure con Cloudyn | Microsoft Docs
description: Use su suscripción de Azure para registrarse con Cloudyn.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 08/07/2018
ms.topic: quickstart
ms.custom: ''
ms.service: cost-management
manager: dougeby
ms.openlocfilehash: e49be3d61cfbbf71a0e9cbeef4c171f3af222544
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46979563"
---
# <a name="register-an-individual-azure-subscription-and-view-cost-data"></a>Registrar una suscripción individual de Azure y ver los datos de costo

Puede usar su suscripción de Azure para registrarse con Cloudyn. Este registro le proporciona acceso al portal de Cloudyn. En esta guía de inicio rápido se detalla el proceso de registro necesario para crear una suscripción de evaluación de Cloudyn e iniciar sesión en el portal de Cloudyn. También se muestra cómo empezar a ver inmediatamente los datos de costo.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

- Inicie sesión en Azure Portal en http://portal.azure.com.

## <a name="register-with-cloudyn"></a>Registrarse con Cloudyn

1. En Azure Portal, haga clic en **Administración de costos + facturación** en la lista de servicios.
2. En **Introducción** haga clic en **Cloudyn**  
    ![Página de Cloudyn](./media/quick-register-azure-sub/cost-mgt-billing-service.png)
3. En la página **Administración de costo**, haga clic en **Ir a Cloudyn** para abrir la página de registro de Cloudyn en una nueva ventana.
4. En la página de registro de evaluación del portal de Cloudyn, escriba el nombre de la compañía y, después, seleccione **Azure Individual Subscription Owner** (Propietario de suscripción individual de Azure). A continuación, haga clic en **Siguiente**. El nombre de la cuenta y el identificador del inquilino se agregarán automáticamente al formulario.  
    ![Registro de prueba](./media/quick-register-azure-sub/trial-reg-ind.png)
5. Seleccione el **nombre y el identificador de la oferta** asociados a la suscripción. Si no está seguro de cuál es el identificador de tasa de su suscripción, puede consultar la factura de Azure para buscar el **identificador de oferta**.
6. Acepte los términos de uso, valide la información y, después, haga clic en **Siguiente**.
7. En la página **Gather additional data**(Recopilar datos adicionales), haga clic en **Siguiente** para permitir que Cloudyn recopile los datos de recursos de Azure. Los datos recopilados incluyen datos de uso, de rendimiento, de facturación y de etiquetas de las suscripciones.  
    ![Recopilar datos adicionales](./media/quick-register-azure-sub/gather-additional.png)
8. El explorador le lleva a la página de inicio de sesión de Cloudyn. Inicie sesión con las credenciales de la suscripción de Azure.
9. Haga clic en **Go to Cloudyn** (Ir a Cloudyn) para abrir el portal de Cloudyn y, después, en la página **Administración de cuentas**, debería ver la información de la cuenta de la suscripción de Azure.  
    ![Administración de cuentas](./media/quick-register-azure-sub/accounts-mgt.png)

Para ver un tutorial en vídeo sobre cómo registrar su suscripción de Azure, consulte [Finding your Directory GUID and Rate ID for use in Cloudyn](https://youtu.be/PaRjnyaNGMI) (Buscar su GUID de directorio e Id. de tasa para su uso en Cloudyn).

[!INCLUDE [cost-management-create-account-view-data](../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, usó la información de suscripción de Azure para registrarse con Cloudyn. También se inicia sesión en el portal de Cloudyn para que pueda empezar a visualizar los datos de costo. Para obtener más información acerca de Cloudyn, continúe con el tutorial de Cloudyn.

> [!div class="nextstepaction"]
> [Revisión del uso y los costos](./tutorial-review-usage.md)
