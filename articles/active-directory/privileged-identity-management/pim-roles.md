---
title: Roles de directorio de Azure AD que se pueden administrar en PIM | Microsoft Docs
description: Describe los roles de directorio de Azure AD que puede administrar en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 07/23/2018
ms.author: rolyon
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.openlocfilehash: fc45cde1a5f0f287274302541ac0115569e2239d
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666343"
---
# <a name="azure-ad-directory-roles-you-can-manage-in-pim"></a>Roles de directorio de Azure AD que se pueden administrar en PIM
<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

En su organización, puede asignar a usuarios a roles administrativos diferentes en Azure AD. Estas asignaciones de roles controlan las tareas, como agregar y quitar usuarios o cambiar la configuración de los servicios, que pueden realizar los usuarios en Azure AD, Office 365 y otros servicios Microsoft Online Services y aplicaciones conectadas.  

Un administrador global puede actualizar qué usuarios están **permanentemente** asignados a roles en Azure AD a través del portal, tal como se describe en [Asignación de roles de administrador en Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md), o mediante [comandos de PowerShell](/powershell/module/azuread#directory_roles).

Privileged Identity Management (PIM) de Azure AD administra las directivas de acceso con privilegios para los usuarios en Azure AD. PIM asigna a usuarios a uno o varios roles de Azure AD y puede asignar una persona para que esté permanentemente en el rol o sea apta para el rol. Cuando un usuario se asigna permanentemente a un rol o activa una asignación de rol apta, puede administrar Azure Active Directory, Office 365 y otras aplicaciones con los permisos asignados a su rol.

No hay ninguna diferencia en el acceso proporcionado de forma permanente a una persona o una asignación de roles aptos. La única diferencia es que algunas personas no necesitan ese acceso todo el tiempo. Son aptas para el rol y, pueden activarlas y desactivarlas cada vez que lo necesiten.

## <a name="roles-managed-in-pim"></a>Roles administrados en PIM
Privileged Identity Management (PIM) permite asignar usuarios a roles de administrador comunes, como los siguientes:

* **administrador global** (también conocido como "administrador de la compañía") tiene acceso a todas las características administrativas. Puede tener más de un administrador global en su organización. La persona que se suscribe para comprar Office 365 automáticamente se convierte en un administrador global.
* **administrador de roles con privilegios** administra PIM de Azure AD y actualiza las asignaciones de roles para otros usuarios.  
* **Administrador de facturación** : hace compras, administra suscripciones, administra incidencias de soporte técnico y supervisa el estado del servicio.
* **Administrador de contraseñas** : restablece las contraseñas, administra las solicitudes de servicio y supervisa el estado del servicio. Los administradores de contraseñas están limitados a restablecer las contraseñas para los usuarios.
* **Administrador de servicios** : administra las solicitudes de servicio y supervisa el estado del servicio.
  
  > [!NOTE]
  > Si usa Office 365, antes de asignar el rol de administrador de servicio a un usuario, asigne primero al usuario permisos administrativos a un servicio, como Exchange Online.
  > 
  > 
* **administrador de control de usuarios** restablece las contraseñas, supervisa el estado del servicio y administra cuentas de usuario, grupos de usuarios y solicitudes de servicio. El administrador de usuarios no puede eliminar a un administrador global, crear otros roles de administrador o restablecer contraseñas de los administradores de facturación, globales y de servicios.
* **administrador de Exchange** tiene acceso administrativo a Exchange Online a través del Centro de administración de Exchange (EAC) y puede realizar casi cualquier tarea en Exchange Online.
* **Administrador de SharePoint (versión preliminar)** tiene acceso administrativo a SharePoint Online a través del Centro de administración de SharePoint Online y puede realizar casi cualquier tarea en SharePoint Online. Este rol se encuentra actualmente en versión preliminar. Los usuarios aptos pueden experimentar retrasos al usar este rol dentro de SharePoint después de la activación en PIM.
* **administrador de Skype Empresarial** tiene acceso administrativo a Skype Empresarial a través del Centro de administración de Skype Empresarial y puede realizar casi cualquier tarea en Skype Empresarial Online.

Consulte estos artículos para más información sobre la [asignación de roles de administrador en Azure AD](../users-groups-roles/directory-assign-admin-roles.md) y de roles de administrador en [Office 365](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504).

<!--**PLACEHOLDER: The above article may not be the one we want since PIM gets roles from places other that Office 365**-->


En PIM, puede [asignar estos roles a un usuario](pim-how-to-add-role-to-user.md) para que pueda [activar el rol cuando lo necesite](pim-how-to-activate-role.md).

Si desea dar a otro usuario acceso para administrar en el mismo PIM, los roles que debe tener el usuario para PIM se describen con más detalle en [Concesión de acceso a PIM](pim-how-to-give-access-to-pim.md).

<!-- ## The PIM Security Administrator Role **PLACEHOLDER: Need description of the Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>Roles no administrados en PIM
Los roles de Exchange Online o SharePoint Online, excepto los mencionados anteriormente, no están representados en Azure AD y, por lo tanto, no son visibles en PIM. Para obtener más información sobre cómo cambiar las asignaciones de roles específicas de estos servicios de Office 365, consulte [Permisos en Office 365](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

Las suscripciones de Azure y los grupos de recursos tampoco están representados en Azure AD. Para administrar suscripciones de Azure, consulte [Agregar o cambiar los administradores de la suscripción de Azure](../../billing/billing-add-change-azure-subscription-administrator.md) y para más información sobre Azure RBAC, consulte [Administración del acceso mediante RBAC y Azure Portal](../../role-based-access-control/role-assignments-portal.md).

<!--**The above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Roles de usuario e inicio de sesión
En algunos servicios y aplicaciones de Microsoft, asignar a un usuario un rol puede no ser suficiente para permitir que el usuario sea un administrador.

El acceso a Azure Portal requiere que el usuario sea un administrador de servicios o el coadministrador de una suscripción de Azure, incluso si el usuario no necesita administrar las suscripciones de Azure.  Por ejemplo, para administrar la configuración de Azure AD, el usuario debe ser tanto un administrador global de Azure AD como un coadministrador de suscripción en una suscripción de Azure.  Para obtener más información sobre cómo agregar usuarios a las suscripciones de Azure, consulte [Incorporación o cambio de roles de administrador de Azure](../../billing/billing-add-change-azure-subscription-administrator.md).

El acceso a Microsoft Online Services puede requerir que se le asigne una licencia al usuario para poder abrir el portal del servicio o realizar tareas administrativas.

## <a name="assign-a-license-to-a-user-in-azure-ad"></a>Asignación de una licencia a un usuario en Azure AD

1. Inicie sesión en [Azure Portal](http://portal.azure.com) con una cuenta de administrador global o una cuenta de coadministrador.

1. Seleccione el directorio de Azure AD con el que quiere trabajar y que tenga licencias asociadas.

1. En el panel de navegación izquierdo, haga clic en **Azure Active Directory**.

1. Haga clic en **Licencias**. Aparecerá la lista de licencias disponibles.

    ![Licencias de Azure Active Directory](./media/pim-roles/licenses-overview.png)

1. Haga clic en su **Producto**.

1. Seleccione el plan de licencias que contiene las licencias que quiere distribuir.

    ![Productos de licencias](./media/pim-roles/licenses-products.png)

1. Haga clic en **Asignar** para abrir el panel de asignación de licencias.

    ![Usuarios con licencias](./media/pim-roles/licenses-licensed-users.png)

1. Seleccione el usuario o grupo al que quiere asignar una licencia.

    ![Asignación de la licencia](./media/pim-roles/licenses-assign-license.png)

1. Haga clic en **Opciones de asignación** para configurar las opciones de asignación.

    ![Opciones de asignación](./media/pim-roles/licenses-assignment-options.png)

1. Haga clic en **Asignar** para asignar la licencia. El usuario ahora tiene la licencia.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Pasos siguientes

- [Primer uso de PIM](pim-getting-started.md)
- [Asignación de roles de directorio de Azure AD en PIM](pim-how-to-add-role-to-user.md)

