---
title: 'Guía de inicio rápido: creación de un conjunto de escalado de máquinas virtuales en Azure Portal | Microsoft Docs'
description: Aprenda a crear rápidamente un conjunto de escalado de máquinas virtuales en Azure Portal
keywords: conjuntos de escalado de máquinas virtuales
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: quickstart
ms.custom: H1Hack27Feb2017
ms.date: 03/27/18
ms.author: cynthn
ms.openlocfilehash: fb3a3e1cec0d6ec15495e677e7bead1c02445803
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38680965"
---
# <a name="quickstart-create-a-virtual-machine-scale-set-in-the-azure-portal"></a>Guía de inicio rápido: creación de un conjunto de escalado de máquinas virtuales en Azure Portal
El conjunto de escalado de máquinas virtuales le permite implementar y administrar un conjunto de máquinas virtuales de escalado automático idénticas. Puede escalar el número de máquinas virtuales del conjunto de escalado manualmente o definir reglas de escalado automático según el uso de recursos tales como la CPU, la demanda de memoria o el tráfico de red. Un equilibrador de carga de Azure distribuirá el tráfico a las instancias de máquina virtual del conjunto de escalado. En esta guía de inicio rápido se crea un conjunto de escalado de máquinas virtuales en Azure Portal.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.


## <a name="log-in-to-azure"></a>Inicio de sesión en Azure
Inicie sesión en Azure Portal en http://portal.azure.com.


## <a name="create-virtual-machine-scale-set"></a>Creación de un conjunto de escalado de máquinas virtuales
Puede implementar un conjunto de escalado con una imagen de Windows Server o Linux como RHEL, CentOS, Ubuntu o SLES.

1. Haga clic en **Crear un recurso** en la esquina superior izquierda de Azure Portal.
2. Busque el *conjunto de escalado* elija **Conjunto de escalado de máquinas virtuales** y seleccione **Crear**.
3. Escriba un nombre para el conjunto de escalado, como *myScaleSet*.
4. Seleccione el tipo de sistema operativo deseado, como *Windows Server 2016 Datacenter*.
5. Escriba el nombre del grupo de recursos deseado, como *myResourceGroup*, y la ubicación, como *Este de EE. UU*.
6. Escriba el nombre de usuario que quiera y seleccione el tipo de autenticación que desee.
    - La **contraseña** debe tener 12 caracteres como mínimo y reunir, al menos, tres de los cuatro requisitos de complejidad siguientes: contener al menos una minúscula, una mayúscula, un número y un carácter especial. Para más información, consulte la sección acerca de los [requisitos de nombre de usuario y contraseña](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm).
    - Si selecciona una imagen de disco del sistema operativo Linux, puede elegir su **clave pública SSH** en su lugar. Proporcione solo su clave pública, como *~/.ssh/id_rsa.pub*. Puede usar Azure Cloud Shell del portal para [crear y utilizar claves SSH](../virtual-machines/linux/mac-create-ssh-keys.md).

7. Escriba un **nombre de dirección IP pública**, como *myPublicIP*.
8. Escriba una **etiqueta de nombre de dominio** única, como *myuniquedns*. Esta etiqueta del sistema de nombres de dominio es la base del FQDN del equilibrador de carga delante del conjunto de escalado.
9. Para confirmar las opciones del conjunto de escalado, seleccione **Crear**.

    ![Creación de un conjunto de escalado de máquinas virtuales en Azure Portal](./media/virtual-machine-scale-sets-create-portal/create-scale-set.png)


## <a name="connect-to-a-vm-in-the-scale-set"></a>Conexión a una máquina virtual en el conjunto de escalado
Al crear un conjunto de escalado en el portal se crea un equilibrador de carga. Las reglas de traducción de direcciones de red (NAT) se usan para distribuir el tráfico a las instancias del conjunto de escalado para la conectividad remota como RDP o SSH.

Para ver estas reglas NAT e información de conexión para las instancias de conjunto de escalado:

1. Seleccione el grupo de recursos que creó en el paso anterior, por ejemplo, *myResourceGroup*.
2. En la lista de recursos, seleccione el **equilibrador de carga**, como *myScaleSetLab*.
3. Elija **Reglas NAT de entrada** en el menú de la izquierda de la ventana.

    ![Las reglas NAT de entrada le permiten conectarse a las instancias de conjunto de escalado de máquinas virtuales](./media/virtual-machine-scale-sets-create-portal/inbound-nat-rules.png)

Puede conectarse a las máquinas virtuales del conjunto de escalado con estas reglas NAT. Cada instancia de máquina virtual muestra una dirección IP de destino y el valor del puerto TCP. Por ejemplo, si la dirección IP de destino es *104.42.1.19* y el puerto TCP es *50001*, se conecta a la instancia de máquina virtual como se indica a continuación:

- Para un conjunto de escalado de Windows, conéctese a la instancia de máquina virtual con RDP en `104.42.1.19:50001`
- Para un conjunto de escalado de Linux, conéctese a la instancia de máquina virtual con SSH en `ssh azureuser@104.42.1.19 -p 50001`

Cuando se le solicite, escriba las credenciales que especificó en el paso anterior al crear el conjunto de escalado. Las instancias de conjunto de escalado son máquinas virtuales normales que pueden interactuar con normalidad. Para más información sobre cómo implementar y ejecutar aplicaciones en las instancias de conjunto de escalado, consulte [Implementación de la aplicación en conjuntos de escalado de máquinas virtuales](virtual-machine-scale-sets-deploy-app.md)


## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no los necesite, elimine el grupo de recursos, el conjunto de escalado y todos los recursos relacionados. Para ello, seleccione el grupo de recursos de la máquina virtual y haga clic en **Eliminar**.


## <a name="next-steps"></a>Pasos siguientes
En esta guía de inicio rápido creó un conjunto de escalado de básico en Azure Portal. Para más información, continúe con el tutorial sobre cómo crear y administrar conjuntos de escalado de máquinas virtuales de Azure.

> [!div class="nextstepaction"]
> [Creación y administración de conjuntos de escalado de máquinas virtuales de Azure](tutorial-create-and-manage-powershell.md)