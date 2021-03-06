---
title: Tutorial acerca del aprovisionamiento de Azure Data Box Gateway en Hyper-V | Microsoft Docs
description: El segundo tutorial para implementar Azure Data Box Gateway implica el aprovisionamiento de un dispositivo virtual en Hyper-V.
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/24/2018
ms.author: alkohli
ms.custom: ''
ms.openlocfilehash: bf744d2aaab168b8ce918f7b776d8855cdc5ad16
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46975254"
---
# <a name="tutorial-provision-azure-data-box-gateway-in-hyper-v-preview"></a>Tutorial: Aprovisionamiento de Azure Data Box Gateway en Hyper-V (versión preliminar)

## <a name="overview"></a>Información general

En este tutorial se describe cómo aprovisionar un dispositivo Data Box Gateway en un sistema host que ejecuta Hyper-V en Windows Server 2016 R2, Windows Server 2012 R2 o Windows Server 2012. 

Para aprovisionar y configurar un dispositivo virtual se necesitan privilegios de administrador. El aprovisionamiento y la instalación inicial pueden tardar unos 10 minutos en completarse.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Asegurarse de que el host cumple los requisitos mínimos del dispositivo
> * Aprovisionar un dispositivo virtual en el hipervisor
> * Iniciar el dispositivo virtual y obtener la dirección IP

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

> [!IMPORTANT]
> - Data Box Gateway está en versión preliminar. Antes de solicitar e implementar esta solución revise los [términos del servicio de Azure para la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Requisitos previos

Aquí encontrará los requisitos previos para aprovisionar un dispositivo virtual en un sistema host que ejecuta Hyper-V en Windows Server 2016 o Windows Server 2012 R2.

### <a name="for-the-data-box-gateway-resource"></a>Para el recurso Data Box Gateway

Antes de comenzar, asegúrese de que:

* Ha completado todos los pasos de [Preparación del portal para Data Box Gateway](data-box-gateway-deploy-prep.md).
* Ha descargado la imagen de dispositivo virtual para Hyper-V desde Azure Portal como se describe en [Preparación del portal para Data Box Gateway](data-box-gateway-deploy-prep.md).

  > [!IMPORTANT]
  > El software que se ejecuta en Data Box Gateway solo puede usarse con el recurso Data Box Gateway.
 
### <a name="for-the-data-box-gateway-virtual-device"></a>Para el dispositivo virtual de Data Box Gateway

Antes de implementar un dispositivo, asegúrese de que:

* Tiene acceso a un sistema host que ejecuta Hyper-V en Windows Server 2012 R2 o superior que puede utilizarse para aprovisionar un dispositivo.
* El sistema host es capaz de utilizar los recursos siguientes para aprovisionar el dispositivo virtual:

  * Un mínimo de 4 núcleos.
  * Al menos 8 GB de RAM. 
  * Una interfaz de red.
  * Un disco de sistema operativo de 250 GB.
  * Un disco virtual de 2 GB para datos.

### <a name="for-the-network-in-the-datacenter"></a>Para la red del centro de datos

Antes de empezar:

- Revise los requisitos de red para implementar Data Box Gateway y configure la red del centro de datos según dichos requisitos. Para más información, consulte [Requisitos de red de Data Box Gateway](data-box-gateway-system-requirements.md#networking-requirements).
- Asegúrese de que el ancho de banda mínimo de Internet es de 20 Mbps para que el dispositivo funcione de forma óptima.


## <a name="check-the-host-system"></a>Comprobación del sistema host

Para crear un dispositivo virtual, necesita:

* El rol de Hyper-V instalado en Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.
* Administrador de Hyper-V de Microsoft en un cliente Microsoft Windows conectado al host.
* Asegúrese de que el hardware subyacente (sistema de host) en el que está creando el dispositivo virtual pueda dedicar los siguientes recursos al dispositivo virtual:

    * Un mínimo de 4 núcleos.
    * Al menos 8 GB de RAM.
    * Una interfaz de red conectada a la red capaz de enrutar el tráfico a Internet. .
    * Un disco de sistema operativo de 250 GB.
    * Un disco virtual de 2 TB para datos del sistema.

## <a name="provision-a-virtual-device-in-hypervisor"></a>Aprovisionar un dispositivo virtual en el hipervisor

Realice los pasos siguientes para aprovisionar un dispositivo en el hipervisor.

1. En el host de Windows Server, copie la imagen del dispositivo virtual en una unidad local. Esta imagen VHDX la descargó a través de Azure Portal. Anote la ubicación en la que copió la imagen, ya que la va a utilizar más adelante en el procedimiento.
2. Abra el **Administrador del servidor**. En la esquina superior derecha, haga clic en **Herramientas** y seleccione **Administrador de Hyper-V**.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image1.png)  
  
3. En el **Administrador de Hyper-V**, en el panel de ámbito, haga clic con el botón derecho en el nodo del sistema para abrir el menú contextual y luego haga clic en **Nuevo** > **Máquina virtual**.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image2.png)
4. En la página **Antes de comenzar** del Asistente para nueva máquina virtual, haga clic en **Siguiente**.
5. En la página **Especificar nombre** y ubicación, proporcione un **nombre** para el dispositivo virtual. Haga clic en **Next**.
   
   > [!IMPORTANT]
   > En esta versión, puede usar solo letras en mayúsculas para el nombre del dispositivo virtual.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image3.png)
6. En la página **Especificar generación**, elija **Generation 2** como tipo de imagen del dispositivo virtual y haga clic en **Siguiente**.    

   ![](./media/data-box-gateway-deploy-provision-hyperv/image4.png)
7. En la página **Asignar memoria**, especifique una **memoria de inicio** de al menos **8192 MB**, no habilite la memoria dinámica y luego haga clic en **Siguiente**.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image5.png) 
8. En la página **Configurar funciones de red**, especifique el conmutador virtual que está conectado a Internet y luego haga clic en **Siguiente**.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image6.png)
9. En la página **Conectar disco duro virtual**, elija **Utilizar un disco duro virtual existente**, especifique la ubicación de la imagen de dispositivo virtual y luego haga clic en **Siguiente**.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image7.png)
10. Revise la sección **Resumen** y luego haga clic en **Finalizar** para crear la máquina virtual.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image8.png)
11. Para cumplir los requisitos mínimos, se necesitan cuatro núcleos. Para agregar cuatro procesadores virtuales, seleccione el sistema de host en la ventana **Administrador de Hyper-V**. En el panel derecho, en la lista de **máquinas virtuales**, busque la máquina virtual que acaba de crear. Seleccione el nombre del equipo, haga clic con el botón derecho y seleccione **Configuración**.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image9.png)
12. En la página **Configuración**, en el panel izquierdo, haga clic en **Procesador**. En el panel derecho, establezca **Número de procesadores virtuales** en 4 (o más). Haga clic en **Aplicar**.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image10.png)
13. Para cumplir los requisitos mínimos, también debe agregar un disco virtual de datos de 2 TB. En la página **Configuración** :

    1. En el panel izquierdo, seleccione **Controlador SCSI**.
    2. En el panel derecho, seleccione **Unidad de disco duro** y haga clic en **Agregar**.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image11.png)
14. En la página **Unidad de disco duro**, seleccione la opción **Disco duro virtual** y haga clic en **Nuevo**. Se inicia el **Asistente para crear discos rígidos virtuales**.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image12.png)
1. En la página **Antes de comenzar** del Asistente para crear nuevo disco virtual, haga clic en **Siguiente**.
2. En la página **Elegir formato de disco**, acepte la opción predeterminada de formato **VHDX**. Haga clic en **Next**.
   
17. En la página **Elegir tipo de disco**, establezca el tipo de disco duro virtual como **Expansión dinámica** (recomendado). Un disco de **tamaño fijo** funcionaría, pero puede que necesite esperar mucho tiempo. Se recomienda que no utilice la opción **Diferenciación** . Haga clic en **Next**. 

    ![](./media/data-box-gateway-deploy-provision-hyperv/image13.png)
18. En la página **Especificar nombre y ubicación**, proporcione un **nombre**, así como una **ubicación** (puede dirigirse a una) para el disco de datos. Haga clic en **Next**.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image14.png)
19. En la página **Configurar disco**, seleccione la opción **Crear un nuevo disco duro virtual en blanco** y especifique el tamaño como **2 TB** (o más). Aunque el requisito mínimo son 2 TB, siempre puede aprovisionar un disco más grande. Tenga en cuenta que una vez que el disco se aprovisiona su tamaño no se puede reducir.  Sin embargo, puede expandir el disco mediante la adición de un disco de datos. Haga clic en **Next**.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image15.png)
20. En la página **Resumen**, revise los detalles del disco de datos virtual y, si está satisfecho, haga clic en **Finalizar** para crear el disco. El asistente se cierra y se agrega un disco duro virtual al equipo.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image16.png)
21. Vuelva a la página **Configuración**. Haga clic en **Aceptar** para cerrar la página **Configuración** y volver a la ventana Administrador de Hyper-V.

    ![](./media/data-box-gateway-deploy-provision-hyperv/image17.png)

## <a name="start-the-virtual-device-and-get-the-ip"></a>Inicio del dispositivo virtual y obtención de la dirección IP
Realice los pasos siguientes para iniciar el dispositivo virtual y conectarse a él.

#### <a name="to-start-the-virtual-device"></a>Para iniciar el dispositivo virtual
1. Inicie el dispositivo virtual.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image18.png)
2. Cuando el dispositivo se esté ejecutando, seleccione el dispositivo, haga clic con el botón derecho y seleccione **Conectar**.

3. Es posible que tenga que esperar entre 10 y 15 minutos para que el dispositivo esté listo. En la consola se muestra un mensaje de estado para indicar el progreso. Cuando el dispositivo esté listo, vaya a **Acción**. Presione `Ctrl + Alt + Delete` para iniciar sesión en el dispositivo virtual. El usuario predeterminado es *EdgeUser* y la contraseña predeterminada es *Password1*.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image21.png)
4. Por motivos de seguridad, la contraseña del administrador de dispositivos expira en el primer inicio de sesión, por lo que se le pedirá que la cambie.

   Escriba una contraseña que contenga al menos 8 caracteres. La contraseña debe cumplir al menos 3 de los siguientes 4 requisitos: caracteres en mayúsculas, minúsculas, numéricos y especiales. Vuelva a escribir la contraseña para confirmarla. Se le notifica que la contraseña ha cambiado.
   
5. Tras cambiar la contraseña correctamente, puede reiniciar el dispositivo virtual. Espere a que el dispositivo se inicie.  La consola de Windows PowerShell del dispositivo se muestra junto a una barra de progreso.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image22.png)

6. Los pasos 6 a 8 solo se aplican cuando se arranca en un entorno sin DHCP. Si se encuentra en un entorno de DHCP, omita estos pasos y vaya al paso 9. Si ha arrancado el dispositivo en un entorno sin DHCP, verá un mensaje de confirmación al respecto.
    
7. Para configurar la red, utilice el comando `Get-HcsIpAddress` para enumerar las interfaces de red habilitadas en el dispositivo virtual. Si el dispositivo tiene una única interfaz de red habilitada, el nombre predeterminado asignado a esta interfaz es `DATA1`.

8. Utilice el cmdlet `Set-HcsIpAddress` para configurar la red. Consulte el ejemplo siguiente:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`
    
9. Una vez que haya finalizado la instalación inicial y el dispositivo haya arrancado, verá el texto de titular del dispositivo. Anote la dirección IP y la dirección URL que se muestran en el texto del titular para administrar el dispositivo. Use esta dirección IP para conectarse a la interfaz de usuario web del dispositivo virtual y completar la instalación local y la activación.

   ![](./media/data-box-gateway-deploy-provision-hyperv/image23.png)
      

Si el dispositivo no cumple los requisitos mínimos de configuración, verá un error en el texto del titular. Modifique la configuración del dispositivo para que el equipo tenga los recursos adecuados para cumplir los requisitos mínimos. A continuación, puede reiniciar y conectarse al dispositivo. Consulte los requisitos mínimos de configuración de [Paso 1: Asegurarse de que el sistema host cumple los requisitos mínimos del dispositivo virtual](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).

<!--If you face any other error during the initial configuration using the local web UI, refer to the following workflows:

* Run diagnostic tests to [troubleshoot web UI setup](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).
* [Generate log package and view log files](storsimple-ova-web-ui-admin.md#generate-a-log-package).-->

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información acerca de varios temas relacionados con Data Box Gateway, como:

> [!div class="checklist"]
> * Asegurarse de que el host cumple los requisitos mínimos del dispositivo
> * Aprovisionar un dispositivo virtual en el hipervisor
> * Iniciar el dispositivo virtual y obtener la dirección IP

Pase al siguiente tutorial para aprender a conectar, configurar y activar dispositivos virtuales.

> [!div class="nextstepaction"]
> [Conexión y configuración de Data Box Gateway](./data-box-gateway-deploy-connect-setup-activate.md)


