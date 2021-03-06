---
title: Uso de las bases de datos proporcionadas por el RP del adaptador de MySQL en Azure Stack | Microsoft Docs
description: Cómo crear y administrar bases de datos MySQL aprovisionadas con el proveedor de recursos del adaptador de MySQL
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 77dca29b0c60726f0a072dd662aba0d12730502a
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39413695"
---
# <a name="create-mysql-databases"></a>Creación de bases de datos MySQL

Puede crear y administrar bases de datos de autoservicio en el portal de usuarios. Un usuario de Azure Stack necesita una suscripción con una oferta que incluya el servicio de base de datos MySQL.

## <a name="test-your-deployment-by-creating-a-mysql-database"></a>Prueba de la implementación mediante la creación de una base de datos MySQL

1. Inicie sesión en el portal de usuarios de Azure Stack.
2. Seleccione **+ New** >  (+ Nuevo)**Data + Storage** >  (Datos y almacenamiento) **MySQL Database** >  (Base de datos MySQL) **Add** (Agregar).
3. En **Create MySQL Database** (Crear base de datos MySQL), escriba el nombre de la base de datos y configure los valores necesarios para su entorno.

    ![Crear una base de datos MySQL de prueba](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)

4. En **Create Database** (Crear base de datos), seleccione **SKU**. En **Select a MySQL SKU** (Seleccionar una SKU de MySQL), seleccione la SKU de su base de datos.

    ![Selección de una SKU de MySQL](./media/azure-stack-mysql-rp-deploy/mysql-select-sku.png)

    >[!Note]
    >A medida que se agregan servidores de hospedaje a Azure Stack, se les asigna una SKU. Las bases de datos se crean en el grupo de servidores de hospedaje de una SKU.

5. En **Login** (inicio de sesión), seleccione ***Configure required settings*** (Configurar valores necesarios).
6. En **Select a Login** (Seleccionar un inicio de sesión), puede elegir uno ya existente o seleccionar **+ Create a new login** (+ Crear un inicio de sesión) para configurar uno nuevo.  Escriba un nombre de **inicio de sesión de base de datos** y una **contraseña** y, luego, seleccione **OK** (Aceptar).

    ![Creación de un nuevo inicio de sesión en la base de datos](./media/azure-stack-mysql-rp-deploy/create-new-login.png)

    >[!NOTE]
    >La longitud del nombre de inicio de sesión de base de datos no puede superar los 32 caracteres en MySQL 5.7. En versiones anteriores, no puede superar los 16 caracteres.

7. Seleccione **Create** (Crear) para terminar de configurar la base de datos.

Una vez implementada la base de datos, anote la **cadena de conexión** que aparece debajo de **Essentials** (Información esencial). Puede usar esta cadena en cualquier aplicación que necesite acceder a la base de datos MySQL.

![Obtener la cadena de conexión para la base de datos MySQL](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png)

## <a name="update-the-administrative-password"></a>Actualizar la contraseña administrativa

Puede modificar la contraseña cambiándola la primera vez en la instancia del servidor MySQL.

1. Seleccione **RECURSOS ADMINISTRATIVOS** > **MySQL Hosting Servers** (Servidores de hospedaje MySQL). Seleccione el servidor de hospedaje.
2. En **Settings** (Configuración), seleccione **Password** (Contraseña).
3. En **Password** (Contraseña), escriba la nueva contraseña y seleccione **Save** (Guardar).

![Actualización de la contraseña de administrador](./media/azure-stack-mysql-rp-deploy/mysql-update-password.png)

## <a name="next-steps"></a>Pasos siguientes

[Actualización del proveedor de recursos de MySQL](azure-stack-mysql-resource-provider-update.md)
