---
title: Acceso condicional basado en aplicaciones de Azure Active Directory | Microsoft Docs
description: Aprenda cómo funciona el acceso condicional basado en aplicaciones de Azure Active Directory.
services: active-directory
keywords: acceso condicional a aplicaciones, acceso condicional con Azure AD, acceso seguro a recursos de empresa, directivas de acceso condicional
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: spunukol
ms.openlocfilehash: f34fc4c41094292db9bed1294ee7b26ec04c96c6
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2018
ms.locfileid: "39630609"
---
# <a name="azure-active-directory-app-based-conditional-access"></a>Acceso condicional basado en aplicaciones de Azure Active Directory  

Los empleados usan dispositivos móviles para tareas personales y profesionales. Mientras se asegura de que los empleados pueden ser productivos, también puede evitar la pérdida de datos. Con el acceso condicional basado en la aplicación de Azure Active Directory (Azure AD), puede restringir el acceso para las aplicaciones en la nube a las aplicaciones cliente que pueden proteger los datos corporativos.  

En este tema se explica cómo configurar el acceso condicional basado en aplicaciones de Azure AD.

## <a name="overview"></a>Información general

Con el [acceso condicional de Azure AD](overview.md), puede ajustar el modo en que los usuarios autorizados pueden tener acceso a sus recursos. Por ejemplo, puede limitar el acceso de los dispositivos de confianza a las aplicaciones en la nube.

Puede usar [directivas de protección de aplicaciones de Intune](https://docs.microsoft.com/intune/app-protection-policy) para ayudar a proteger los datos de su compañía. Las directivas de protección de aplicaciones de Intune no requieren una solución de administración de dispositivos móviles (MDM), lo que le permite proteger los datos de su empresa con o sin inscribir los dispositivos en una solución de administración de dispositivos.

El acceso condicional basado en aplicaciones de Azure Active Directory le permite limitar el acceso a las aplicaciones en la nube para las aplicaciones cliente que admiten directivas de protección de aplicaciones de Intune. Por ejemplo, puede restringir el acceso a Exchange Online para la aplicación Outlook.

En la terminología del acceso condicional, estas aplicaciones cliente se conocen como **aplicaciones cliente aprobadas**.  


![Acceso condicional](./media/app-based-conditional-access/05.png)


Para una lista de las aplicaciones cliente aprobadas, consulte el [requisito de las aplicaciones cliente aprobadas](technical-reference.md#approved-client-app-requirement).


Puede combinar directivas de acceso condicional basado en aplicaciones con otras directivas como las [de acceso condicional basado en dispositivos](require-managed-devices.md) para proporcionar flexibilidad en la manera de proteger los datos de los dispositivos personales y corporativos.

 


## <a name="before-you-begin"></a>Antes de empezar

En este tema se da por supuesto que está familiarizado con:

- La referencia técnica del [requisito de las aplicaciones cliente aprobadas](technical-reference.md#approved-client-app-requirement).


- Los conceptos básicos del [Acceso condicional en Azure Active Directory](overview.md).

- La [configuración de la directiva de acceso condicional](app-based-mfa.md).

- La [migración de directivas de acceso condicional](best-practices.md#policy-migration).
 

## <a name="prerequisites"></a>Requisitos previos

Para crear una directiva de acceso condicional basado en aplicaciones, debe tener una suscripción a Enterprise Mobility + Security o una suscripción Premium a Azure Active Directory, y los usuarios deben disponer de una licencia para EMS o Azure AD. 


## <a name="exchange-online-policy"></a>Directiva de Exchange Online 

Este escenario consta de una directiva de acceso condicional basado en aplicaciones para acceder a Exchange Online.


### <a name="scenario-playbook"></a>Guía de escenario

Este escenario supone que un usuario:

- Configura el correo electrónico mediante una aplicación de correo electrónico nativa de iOS o Android para conectarse a Exchange.

- Recibe un correo electrónico que indica que el acceso solo está disponible mediante la aplicación Outlook.

- Descarga la aplicación con el vínculo.

- Abre la aplicación Outlook e inicia sesión con las credenciales de Azure AD

- Se le pide que instale Authenticator (iOS) o el Portal de empresa (Android) para continuar

- Instala la aplicación y puede volver a la aplicación Outlook para continuar.

- Se le solicita que registre un dispositivo.

- Puede acceder al correo electrónico.

Todas las directivas de protección de aplicaciones de Intune están activas en el momento de acceder a los datos corporativos y puede que se le pida al usuario que reinicie la aplicación, use un PIN adicional, etc (si esto está configurado para la aplicación y la plataforma).

### <a name="configuration"></a>Configuración 

**Paso 1: Configuración de una directiva de acceso condicional de Azure AD para Exchange Online**

Para la directiva de acceso condicional descrita en este paso, debe configurar los componentes siguientes:

![Acceso condicional](./media/app-based-conditional-access/01.png)

1. El **nombre** de la directiva de acceso condicional.

2. Los **usuarios y grupos**: cada directiva de acceso condicional debe tener al menos un usuario o grupo seleccionado.

3. **Aplicaciones en la nube:** como aplicación en la nube, debe seleccionar **Office 365 Exchange Online**.

    ![Acceso condicional](./media/app-based-conditional-access/07.png)

4. **Condiciones:** como **Condiciones**, debe configurar **Plataformas de dispositivo** y **Aplicaciones cliente**:

    a. Como **Plataformas de dispositivo**, seleccione **Android** e **iOS**.

    ![Acceso condicional](./media/app-based-conditional-access/03.png)

    b. Como **Aplicaciones cliente**, seleccione **Aplicaciones de escritorio y aplicaciones móviles**.

    ![Acceso condicional](./media/app-based-conditional-access/04.png)

5. Como **Controles de acceso**, debe seleccionar **Requerir aplicación cliente aprobada (versión preliminar)**.

    ![Acceso condicional](./media/app-based-conditional-access/05.png)
 

**Paso 2: Configuración de una directiva de acceso condicional de Azure AD para Exchange Online con Active Sync (EAS)**

Para la directiva de acceso condicional descrita en este paso, debe configurar los componentes siguientes:

![Acceso condicional](./media/app-based-conditional-access/06.png)

1. El **nombre** de la directiva de acceso condicional.

2. Los **usuarios y grupos**: cada directiva de acceso condicional debe tener al menos un usuario o grupo seleccionado.


3. **Aplicaciones en la nube:** como aplicación en la nube, debe seleccionar **Office 365 Exchange Online**.

    ![Acceso condicional](./media/app-based-conditional-access/07.png)

4. **Condiciones:** como **Condiciones**, debe configurar **Aplicaciones cliente**. 

    a. Como **Aplicaciones cliente**, seleccione **Exchange Active Sync**.

    ![Acceso condicional](./media/app-based-conditional-access/08.png)

    b. Como **Controles de acceso**, debe seleccionar **Requerir aplicación cliente aprobada (versión preliminar)**.

    ![Acceso condicional](./media/app-based-conditional-access/05.png)


**Paso 3: Configuración de la directiva de protección de aplicaciones de Intune para aplicaciones cliente de iOS y Android**


![Acceso condicional](./media/app-based-conditional-access/09.png)

Consulte [Protección de aplicaciones y datos con Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) para más información.


## <a name="exchange-online-and-sharepoint-online-policy"></a>Directiva de Exchange Online y SharePoint Online

Este escenario consta de un acceso condicional con directiva de administración de aplicaciones móviles para el acceso a Exchange Online y SharePoint Online con aplicaciones aprobadas.

### <a name="scenario-playbook"></a>Guía de escenario

Este escenario supone que un usuario:

- Intenta usar la aplicación de SharePoint para conectarse y también para ver sus sitios corporativos.

- Intenta iniciar sesión con las mismas credenciales que las credenciales de la aplicación Outlook

- No tiene que volver a registrarse y puede obtener acceso a los recursos.


### <a name="configuration"></a>Configuración

**Paso 1: Configuración de una directiva de acceso condicional de Azure AD para Exchange Online y SharePoint Online**

Para la directiva de acceso condicional descrita en este paso, debe configurar los componentes siguientes:

![Acceso condicional](./media/app-based-conditional-access/71.png)

1. El **nombre** de la directiva de acceso condicional.

2. Los **usuarios y grupos**: cada directiva de acceso condicional debe tener al menos un usuario o grupo seleccionado.


3. **Aplicaciones en la nube:** como aplicaciones en la nube, debe seleccionar **Office 365 Exchange Online** y **Office 365 SharePoint Online**. 

    ![Acceso condicional](./media/app-based-conditional-access/02.png)

4. **Condiciones:** como **Condiciones**, debe configurar **Plataformas de dispositivo** y **Aplicaciones cliente**:

    a. Como **Plataformas de dispositivo**, seleccione **Android** e **iOS**.

    ![Acceso condicional](./media/app-based-conditional-access/03.png)

    b. Como **Aplicaciones cliente**, seleccione **Aplicaciones de escritorio y aplicaciones móviles**.

    ![Acceso condicional](./media/app-based-conditional-access/04.png)

5. Como **Controles de acceso**, debe seleccionar **Requerir aplicación cliente aprobada (versión preliminar)**.

    ![Acceso condicional](./media/app-based-conditional-access/05.png)




**Paso 2: Configuración de una directiva de acceso condicional de Azure AD para Exchange Online con Active Sync (EAS)**

Para la directiva de acceso condicional descrita en este paso, debe configurar los componentes siguientes:

![Acceso condicional](./media/app-based-conditional-access/06.png)

1. El **nombre** de la directiva de acceso condicional.

2. Los **usuarios y grupos**: cada directiva de acceso condicional debe tener al menos un usuario o grupo seleccionado.

3. **Aplicaciones en la nube:** como aplicación en la nube, debe seleccionar **Office 365 Exchange Online**. En línea 

    ![Acceso condicional](./media/app-based-conditional-access/07.png)

4. **Condiciones:** como **Condiciones**, debe configurar **Aplicaciones cliente**:

    a. Como **Aplicaciones cliente**, seleccione **Exchange Active Sync**.

    ![Acceso condicional](./media/app-based-conditional-access/08.png)

    b. Como **Controles de acceso**, debe seleccionar **Requerir aplicación cliente aprobada (versión preliminar)**.

    ![Acceso condicional](./media/app-based-conditional-access/05.png)




**Paso 3: Configuración de la directiva de protección de aplicaciones de Intune para aplicaciones cliente de iOS y Android**


![Acceso condicional](./media/app-based-conditional-access/09.png)

Consulte [Protección de aplicaciones y datos con Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) para más información.


## <a name="app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>Directiva de dispositivos compatibles o basado en aplicaciones para Exchange Online y SharePoint Online

Este escenario consta de una directiva de acceso condicional de dispositivos compatibles o basado en aplicaciones para acceder a Exchange Online.


### <a name="scenario-playbook"></a>Guía de escenario

En este escenario se supone que:
 
- Algunos usuarios ya están inscritos (con o sin dispositivos corporativos)

- Los usuarios que no están inscritos ni registrados con Azure AD mediante una aplicación protegida deben registrar un dispositivo para acceder a los recursos

- Los usuarios inscritos mediante la aplicación protegida no tienen que volver a registrar el dispositivo


### <a name="configuration"></a>Configuración

**Paso 1: Configuración de una directiva de acceso condicional de Azure AD para Exchange Online y SharePoint Online**

Para la directiva de acceso condicional descrita en este paso, debe configurar los componentes siguientes:

![Acceso condicional](./media/app-based-conditional-access/62.png)

1. El **nombre** de la directiva de acceso condicional.

2. Los **usuarios y grupos**: cada directiva de acceso condicional debe tener al menos un usuario o grupo seleccionado.

3. **Aplicaciones en la nube:** como aplicaciones en la nube, debe seleccionar **Office 365 Exchange Online** y **Office 365 SharePoint Online**. 

     ![Acceso condicional](./media/app-based-conditional-access/02.png)

4. **Condiciones:** como **Condiciones**, debe configurar **Plataformas de dispositivo** y **Aplicaciones cliente**. 
 
    a. Como **Plataformas de dispositivo**, seleccione **Android** e **iOS**.

    ![Acceso condicional](./media/app-based-conditional-access/03.png)

    b. Como **Aplicaciones cliente**, seleccione **Aplicaciones de escritorio y aplicaciones móviles**.

    ![Acceso condicional](./media/app-based-conditional-access/04.png)

5. Como **Controles de acceso**, debe tener seleccionado lo siguiente:

    - **Requerir que el dispositivo esté marcado como compatible**

    - **Requerir aplicación cliente aprobada (versión preliminar)**

    - **Requerir uno de los controles seleccionados**   
 
    ![Acceso condicional](./media/app-based-conditional-access/11.png)



**Paso 2: Configuración de una directiva de acceso condicional de Azure AD para Exchange Online con Active Sync (EAS)**

Para la directiva de acceso condicional descrita en este paso, debe configurar los componentes siguientes:

![Acceso condicional](./media/app-based-conditional-access/61.png)

1. El **nombre** de la directiva de acceso condicional.

2. Los **usuarios y grupos**: cada directiva de acceso condicional debe tener al menos un usuario o grupo seleccionado.

3. **Aplicaciones en la nube:** como aplicación en la nube, debe seleccionar **Office 365 Exchange Online**. 

    ![Acceso condicional](./media/app-based-conditional-access/07.png)

4. **Condiciones:** como **Condiciones**, debe configurar **Aplicaciones cliente**. 

    Como **Aplicaciones cliente*, seleccione **Exchange Active Sync**.

    ![Acceso condicional](./media/app-based-conditional-access/08.png)

5. Como **Controles de acceso**, debe seleccionar **Requerir aplicación cliente aprobada (versión preliminar)**.
 
    ![Acceso condicional](./media/app-based-conditional-access/11.png)




**Paso 3: Configuración de la directiva de protección de aplicaciones de Intune para aplicaciones cliente de iOS y Android**


![Acceso condicional](./media/app-based-conditional-access/09.png)

Consulte [Protección de aplicaciones y datos con Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) para más información.





## <a name="app-based-and-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>Directiva de dispositivos compatibles y basada en aplicaciones para Exchange Online y SharePoint Online

Este escenario consta de una directiva de acceso condicional de dispositivos compatibles y basada en aplicaciones para acceder a Exchange Online.


### <a name="scenario-playbook"></a>Guía de escenario

Este escenario supone que un usuario:
 
-   Configura el correo electrónico mediante una aplicación de correo electrónico nativa de iOS o Android para conectarse a Exchange.
-   Recibe un correo electrónico que indica que es necesario que el dispositivo esté inscrito para poder acceder.
-   Descarga el portal de la empresa e inicia sesión en este.
-   Comprueba el correo electrónico y se le solicita que use la aplicación Outlook.
-   Descarga la aplicación Outlook.
-   Abre Outlook y escribe las credenciales usadas en la inscripción.
-   El usuario puede acceder al correo electrónico.

Todas las directivas de protección de aplicaciones de Intune están activas en el momento de acceder a los datos corporativos y puede que se le pida al usuario que reinicie la aplicación, use un PIN adicional, etc (si esto está configurado para la aplicación y la plataforma).


### <a name="configuration"></a>Configuración

**Paso 1: Configuración de una directiva de acceso condicional de Azure AD para Exchange Online y SharePoint Online**

Para la directiva de acceso condicional descrita en este paso, debe configurar los componentes siguientes:

![Acceso condicional](./media/app-based-conditional-access/62.png)

1. El **nombre** de la directiva de acceso condicional.

2. Los **usuarios y grupos**: cada directiva de acceso condicional debe tener al menos un usuario o grupo seleccionado.

3. **Aplicaciones en la nube:** como aplicaciones en la nube, debe seleccionar **Office 365 Exchange Online** y **Office 365 SharePoint Online**. 

     ![Acceso condicional](./media/app-based-conditional-access/02.png)

4. **Condiciones:** como **Condiciones**, debe configurar **Plataformas de dispositivo** y **Aplicaciones cliente**. 
 
    a. Como **Plataformas de dispositivo**, seleccione **Android** e **iOS**.

    ![Acceso condicional](./media/app-based-conditional-access/03.png)

    b. Como **Aplicaciones cliente**, seleccione **Aplicaciones de escritorio y aplicaciones móviles**.

    ![Acceso condicional](./media/app-based-conditional-access/04.png)

5. Como **Controles de acceso**, debe tener seleccionado lo siguiente:

    - **Requerir que el dispositivo esté marcado como compatible**

    - **Requerir aplicación cliente aprobada (versión preliminar)**

    - **Requerir todos los controles seleccionados**   
 
    ![Acceso condicional](./media/app-based-conditional-access/13.png)



**Paso 2: Configuración de una directiva de acceso condicional de Azure AD para Exchange Online con Active Sync (EAS)**

Para la directiva de acceso condicional descrita en este paso, debe configurar los componentes siguientes:

![Acceso condicional](./media/app-based-conditional-access/61.png)

1. El **nombre** de la directiva de acceso condicional.

2. Los **usuarios y grupos**: cada directiva de acceso condicional debe tener al menos un usuario o grupo seleccionado.

3. **Aplicaciones en la nube:** como aplicación en la nube, debe seleccionar **Office 365 Exchange Online**. 

    ![Acceso condicional](./media/app-based-conditional-access/07.png)

4. **Condiciones:** como **Condiciones**, debe configurar **Aplicaciones cliente**. 

    Como **Aplicaciones cliente**, seleccione **Exchange Active Sync**.

    ![Acceso condicional](./media/app-based-conditional-access/08.png)

5. Como **Controles de acceso**, debe tener seleccionado lo siguiente:

    - **Requerir que el dispositivo esté marcado como compatible**

    - **Requerir aplicación cliente aprobada (versión preliminar)**

    - **Requerir todos los controles seleccionados**   
 
    ![Acceso condicional](./media/app-based-conditional-access/64.png)




**Paso 3: Configuración de la directiva de protección de aplicaciones de Intune para aplicaciones cliente de iOS y Android**


![Acceso condicional](./media/app-based-conditional-access/09.png)

Consulte [Protección de aplicaciones y datos con Microsoft Intune](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) para más información.






## <a name="next-steps"></a>Pasos siguientes

Si quiere saber cómo configurar una directiva de acceso condicional, consulte [Requerir MFA para aplicaciones específicas con acceso condicional a Azure Active Directory](app-based-mfa.md).

Si está listo para configurar directivas de acceso condicional para su entorno, consulte [Procedimientos recomendados para el acceso condicional en Azure Active Directory](best-practices.md). 
