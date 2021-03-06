---
title: Solución de problemas de dispositivos híbridos de nivel inferior unidos a Azure Active Directory | Microsoft Docs
description: Solución de problemas de dispositivos híbridos de nivel inferior unidos a Azure Active Directory
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 2c50ba1abfe3681a39b39bf52f127efd9d518aef
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43041875"
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>Solución de problemas de dispositivos híbridos de nivel inferior unidos a Azure Active Directory 

Este artículo solo es aplicable a los siguientes dispositivos: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Para Windows 10 o Windows Server 2016, consulte [Solución de problemas de dispositivos híbridos de Windows 10 y Windows Server 2016 unidos a Azure Active Directory](troubleshoot-hybrid-join-windows-current.md).

En este artículo se da por supuesto que [configuró dispositivos híbridos unidos a Azure Active Directory](hybrid-azuread-join-plan.md) para que admitan los escenarios siguientes:

- Acceso condicional basado en dispositivos

- [Perfiles móviles de empresa](../active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello para empresas](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification) 





En este artículo se proporcionan instrucciones sobre cómo resolver problemas potenciales.  

**Qué debería saber:** 

- El número máximo de dispositivos por usuario está centrado en dispositivos. Por ejemplo, si *jdoe* y *jharnett* inician sesión en un dispositivo, se crea un registro independiente (identificador de dispositivo) para cada uno de estos usuarios en la pestaña de información de **USUARIO**.  

- El registro inicial o unión de dispositivos se ha configurado para realizar un intento de inicio de sesión o de bloqueo/desbloqueo. Podría haber un retraso de 5 minutos desencadenado por una tarea de programador de tareas. 

- Puede que reciba varias entradas para un dispositivo en la pestaña de información del usuario debido a la reinstalación del sistema operativo o a un nuevo registro manual. 

- Asegúrese de que [KB4284842](https://support.microsoft.com/help/4284842) está instalado, en el caso de Windows 7 SP1 o Windows Server 2008 R2 SP1. Esta actualización evita errores de autenticación futuros debido a la pérdida de acceso del cliente en el caso de claves protegidas después de cambiar la contraseña.

## <a name="step-1-retrieve-the-registration-status"></a>Paso 1: Recuperar el estado del registro 

**Para verificar el estado del registro:**  

1. Inicie sesión con la cuenta de usuario que usó para crear una combinación híbrida de Azure AD.

2. Abra el símbolo del sistema como administrador. 

3. Escriba `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe" /i`

Este comando muestra un cuadro de diálogo que proporciona más detalles sobre el estado de la unión.

![Workplace Join for Windows](./media/troubleshoot-hybrid-join-windows-legacy/01.png)


## <a name="step-2-evaluate-the-hybrid-azure-ad-join-status"></a>Paso 2: Evaluación del estado de unión a Azure AD híbrido 

Si la unión a Azure AD híbrido no se realiza correctamente, el cuadro de diálogo proporciona detalles sobre el problema que se ha producido.

**Los problemas más comunes son:**

- Una configuración incorrecta de AD FS o Azure AD

    ![Workplace Join for Windows](./media/troubleshoot-hybrid-join-windows-legacy/02.png)

- No está registrado como un usuario de dominio

    ![Workplace Join for Windows](./media/troubleshoot-hybrid-join-windows-legacy/03.png)
    
    Existen motivos diferentes por los que esto puede ocurrir:
    
    - El usuario con la sesión iniciada no es un usuario del dominio (por ejemplo, un usuario local). La combinación híbrida de Azure AD en dispositivos de nivel inferior solo se admite para los usuarios del dominio.
    
    - Autoworkplace.exe no puede autenticarse de forma silenciosa con Azure AD o AD FS. Este problema podría deberse a un problema de conectividad de la red saliente con las direcciones URL de Azure AD. También podría ser que la autenticación de multifactor (MFA) esté habilitada o configurada para el usuario, y WIAORMUTLIAUTHN no esté configurado en el servidor de federación. Otra posibilidad es que la página de detección de dominio de inicio (HRD) esté esperando a la interacción del usuario, lo que impide que **autoworkplace.exe** solicite de forma silenciosa un token.
    
    - Su organización usa sin problemas el inicio de sesión único de Azure AD, `https://autologon.microsoftazuread-sso.com` o `https://aadg.windows.net.nsatc.net` no está presente en la configuración de Intranet de IE del dispositivo y la opción **Allow updates to status bar via script** (Permitir actualizaciones en la barra de estado mediante el script) no está habilitada en la zona de la Intranet.

- Se ha alcanzado una cuota

    ![Workplace Join for Windows](./media/troubleshoot-hybrid-join-windows-legacy/04.png)

- El servicio no responde 

    ![Workplace Join for Windows](./media/troubleshoot-hybrid-join-windows-legacy/05.png)

También puede encontrar la información de estado en el registro de eventos en **Applications and Services Log\Microsoft-Workplace Join** (Registros de aplicaciones y servicios\Microsoft-Workplace Join).
  
**Las causas más comunes para una unión a Azure AD híbrido con error son:** 

- El equipo no está conectado a la red interna de la organización, ni a una VPN con conexión al controlador de dominio de AD local.

- Ha iniciado sesión en el equipo con una cuenta del equipo local. 

- Problemas de configuración de servicio: 

  - El servidor de federación se ha configurado para admitir **WIAORMULTIAUTHN**. 

  - El bosque del equipo no tiene un objeto de punto de conexión de servicio que haga referencia a su nombre de dominio comprobado en Azure AD. 

  - Un usuario ha alcanzado el límite de dispositivos. 

## <a name="next-steps"></a>Pasos siguientes

Si tiene preguntas, consulte las [preguntas más frecuentes sobre la administración de dispositivos](faq.md).  
