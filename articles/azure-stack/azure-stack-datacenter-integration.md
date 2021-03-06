---
title: Consideraciones de integración del centro de datos general para sistemas integrados de Azure Stack | Microsoft Docs
description: Obtenga información acerca de lo que puede hacer para planear y preparar la integración del centro de datos con Azure Stack de varios nodos.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 0c43b66a9d6210ea951af3fae5eca8bc6d47c3d9
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261226"
---
# <a name="datacenter-integration-considerations-for-azure-stack-integrated-systems"></a>Consideraciones de integración del centro de datos para sistemas integrados de Azure Stack
Si está interesado en un sistema integrado de Azure Stack, debería comprender algunas de las principales consideraciones de planeamiento acerca de la implementación y el modo en que el sistema se adapta a su centro de datos. En este artículo se proporciona información general de alto nivel de estas consideraciones para ayudarle a tomar importantes decisiones de infraestructura para el sistema de varios nodos de Azure Stack. Entender estas consideraciones ayuda a trabajar con su proveedor de hardware OEM cuando se implementa Azure Stack en el centro de datos.  

> [!NOTE]
> Los sistemas de varios nodos de Azure Stack solo pueden adquirirse en los proveedores de hardware autorizados. 

Para implementar Azure Stack, debe proporcionar información de planeamiento al proveedor de soluciones antes de que comience la implementación a fin de facilitar el avance rápido y sencillo del proceso. La información requerida se extiende a través de redes, seguridad e información de identidad con muchas decisiones importantes, que pueden requerir conocimiento de diferentes áreas y de distintos responsables en la toma de decisiones. Por lo tanto, es posible que tenga que atraer a personas de varios equipos de su organización para asegurarse de que tiene lista toda la información necesaria antes de comenzar la implementación. Puede ser útil hablar con su proveedor de hardware mientras recopila esta información, ya que puede que tenga consejos útiles para su toma de decisiones.

Al investigar y recopilar la información necesaria, es posible que tenga que realizar algunos cambios en la configuración previa a la implementación en su entorno de red. Esto podría incluir la reserva de espacios de direcciones IP para la solución Azure Stack, la configuración de los enrutadores, conmutadores y firewall para prepararse para la conectividad con los nuevos conmutadores de la solución Azure Stack. Asegúrese de tener al experto del área temática a mano para ayudarle con el planeamiento.

## <a name="capacity-planning-considerations"></a>Consideraciones de planeamiento de capacidad
Al evaluar una solución Azure Stack para la adquisición es necesario seleccionar entre las opciones de configuración de hardware, esta selección tiene un impacto directo en la capacidad total de su solución Azure Stack. Esto incluye las opciones clásicas de CPU, densidad de memoria, configuración de almacenamiento y escala de solución global (por ejemplo, el número de servidores). A diferencia de una solución de virtualización tradicional, la aritmética simple de estos componentes no es aplicable a la hora de determinar la capacidad utilizable. El primer motivo es que Azure Stack se ha diseñado para hospedar los componentes de infraestructura o administración dentro de la propia solución. El segundo motivo es que parte de la capacidad de la solución está reservada en apoyo de resistencia; la actualización del software de la solución de forma que minimiza la interrupción de las cargas de trabajo de inquilino. 

La [hoja de cálculo del programador de capacidad de Azure Stack](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) ayuda a tomar decisiones fundamentadas con respecto al planeamiento de la capacidad de dos maneras: ya sea seleccionando una oferta de hardware e intentando colocar una combinación de recursos, o mediante la definición de la carga de trabajo que Azure Stack está destinado a ejecutar para ver los SKU de hardware disponibles que la admiten. Por último, la hoja de cálculo está pensada como una guía para ayudar a tomar decisiones relacionadas con el planeamiento y configuración de Azure Stack. 

La hoja de cálculo no está pensada para que actúe como un sustituto de su propia investigación y análisis.  Microsoft no ofrece ninguna manifestación o garantía, ni expresa ni implícita, con respecto a la información proporcionada en la hoja de cálculo.



## <a name="management-considerations"></a>Consideraciones de administración
Azure Stack es un sistema sellado en el cual la infraestructura está bloqueada desde una perspectiva de red y de permisos. Las listas de control de acceso (ACL) de red se aplican para bloquear todo el tráfico entrante no autorizado y todas las comunicaciones innecesarias entre los componentes de infraestructura. Esto dificulta el acceso al sistema de los usuarios no autorizados.

Para la administración y las operaciones diarias, no hay ningún acceso de administrador a la infraestructura sin restricciones. Los operadores de Azure Stack deben administrar el sistema a través del portal de administrador o de Azure Resource Manager (a través de PowerShell o de la API de REST). No hay ningún acceso al sistema mediante otras herramientas de administración, como el Administrador de Hyper-V o el Administrador de clústeres de conmutación por error. Para ayudar a proteger el sistema, no se puede instalar software de terceros (por ejemplo, agentes) dentro de los componentes de la infraestructura de Azure Stack. La interoperabilidad con el software de seguridad y la administración externa se produce a través de PowerShell o la API de REST.

Cuando se necesita un mayor nivel de acceso para la solución de problemas que no se resuelven a través de los pasos de mediación de alerta, debe trabajar con el soporte técnico de Microsoft. A través del soporte técnico, hay un método para proporcionar acceso de administrador completo temporal al sistema para realizar operaciones más avanzadas. 

## <a name="identity-considerations"></a>Consideraciones de identidad

### <a name="choose-identity-provider"></a>Elegir el proveedor de identidades
Debe tener en cuenta qué proveedor de identidades desea usar para la implementación de Azure Stack, Azure AD o AD FS. No puede cambiar los proveedores de identidades tras la implementación sin volver a implementar todo el sistema. Si no dispone de su propia cuenta de Azure AD, está utilizando una que le suministró su proveedor de servicios en la nube y decide cambiar de proveedor y usar una cuenta de Azure AD diferente, deberá ponerse en contacto con el proveedor de soluciones para que vuelva a implementar la solución, aunque tendrá que asumir los costos.



Su elección del proveedor de identidades no repercute en máquinas virtuales de inquilinos, en el sistema de identidades ni en las cuentas que utilizan, en si puede unirse a un dominio de Active Directory, etc. Esto es independiente.

Puede obtener más información sobre cómo elegir un proveedor de identidades en el [artículo sobre modelos de conexión a sistemas integrados de Azure Stack](.\azure-stack-connection-models.md).

### <a name="ad-fs-and-graph-integration"></a>Integración de AD FS y Graph
Si decide implementar Azure Stack mediante AD FS como proveedor de identidades, debe integrar la instancia de AD FS en Azure Stack con una instancia existente de AD FS a través de una confianza de federación. Esto permite autenticar las identidades de un bosque de Active Directory existente con los recursos de Azure Stack.

También puede integrar el servicio de Graph en Azure Stack con el Active Directory existente. Esto le permite administrar el control de acceso basado en rol (RBAC) en Azure Stack. Cuando se delega el acceso a un recurso, el componente de Graph busca la cuenta de usuario en el bosque de Active Directory existente mediante el protocolo LDAP.

El siguiente diagrama muestra el flujo de tráfico de AD FS y Graph integrado.
![Diagrama que muestra el flujo de tráfico de AD FS y Graph](media/azure-stack-datacenter-integration/ADFSIntegration.PNG)

## <a name="licensing-model"></a>Modelo de licencia
Debe decidir qué modelo de licencia desea usar. Las opciones disponibles dependen de si implementa Azure Stack conectado a Internet o no:
- Para una [implementación conectada](azure-stack-connected-deployment.md), puede elegir licencias de pago por uso o según la capacidad. El pago por uso requiere una conexión a Azure para informar del uso, que, a continuación, se factura a través del comercio de Azure. 
- La licencia según la capacidad solo se admite si [implementa desconectado](azure-stack-disconnected-deployment.md) desde Internet. 

Para obtener más información acerca de los modelos de licencias, consulte [Paquetes y precios de Microsoft Azure Stack](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf).


## <a name="naming-decisions"></a>Decisiones de nomenclaturas

Debe pensar en cómo desea planear el espacio de nombres de Azure Stack, especialmente el nombre de la región y del dominio externo. El nombre de dominio completo (FQDN) externo de la implementación de Azure Stack para puntos de conexión de acceso público es la combinación de estos dos nombres: &lt;*región*&gt;.&lt;*fqdn*&gt;. Por ejemplo, *east.cloud.fabrikam.com*. En este ejemplo, los portales de Azure Stack estarían disponibles en las direcciones URL siguientes:

- https://portal.east.cloud.fabrikam.com
- https://adminportal.east.cloud.fabrikam.com

> [!IMPORTANT]
> El nombre de región que elija para su implementación de Azure Stack debe ser único y se mostrará en las direcciones del portal. 

En la tabla siguiente se resumen estas decisiones de nomenclatura de dominio.

| NOMBRE | DESCRIPCIÓN | 
| -------- | ------------- | 
|Nombre de la región | Nombre de su primera región de Azure Stack. Este nombre se usa como parte del FQDN para las direcciones IP virtuales (VIP) públicas que administra Azure Stack. Normalmente, el nombre de la región sería un identificador de ubicación física, como una ubicación de centro de datos. | 
| Nombre de dominio externo | Nombre de la zona de Sistema de nombres de dominio (DNS) para los puntos de conexión con direcciones VIP de uso externo. Se utiliza en el FQDN para estas VIP públicas. | 
| Nombre de dominio (interno) privado | Nombre del dominio (y de la zona DNS interna) creado en Azure Stack para la administración de la infraestructura. 
| | |

## <a name="certificate-requirements"></a>Requisitos de certificados

Para la implementación, debe proporcionar los certificados de Capa de sockets seguros (SSL) para puntos de conexión de acceso público. En un nivel más alto, los certificados tienen los siguientes requisitos:

- Puede usar un único certificado de carácter comodín o bien un conjunto de certificados dedicados y utilizar caracteres comodín solo para los puntos de conexión, como el almacenamiento y Key Vault.
- Los certificados los debe emitir una entidad de certificación (CA) de confianza pública o una entidad de certificación administrada por el cliente.

Para más información acerca de qué certificados de PKI se requieren para implementar Azure Stack y cómo obtenerlos, consulte [Requisitos de certificados de infraestructura de clave pública de Azure Stack](azure-stack-pki-certs.md).  


> [!IMPORTANT]
> La información del certificado de PKI facilitada debe utilizarse como guía general. Antes de adquirir los certificados de PKI para Azure Stack, trabaje con los asociados de hardware OEM. Le proporcionarán una orientación más detallada de los certificados y los requisitos.


## <a name="time-synchronization"></a>Sincronización de la hora
Debe elegir un servidor de tiempo específico que va a usar para sincronizar Azure Stack.  La simbolización del tiempo es fundamental para Azure Stack y sus funciones de infraestructura, ya que se utiliza para generar incidencias de Kerberos que se usan para autenticar los servicios internos entre sí.

Debe especificar una dirección IP para el servidor de sincronización de tiempo; aunque la mayoría de los componentes de la infraestructura pueden resolver una dirección URL, algunos solo admiten direcciones IP. Si está utilizando la opción de implementación Desconectado, debe especificar un servidor de tiempo en la red corporativa para estar seguro de que se puede acceder desde la red de infraestructura en Azure Stack.

## <a name="connect-azure-stack-to-azure"></a>Conectar Azure Stack a Azure

Para escenarios de nube híbridos, debe planear cómo conectar Azure Stack a Azure. Hay dos métodos admitidos para conectar redes virtuales de Azure Stack a redes virtuales de Azure: 
- **De sitio a sitio**. Una conexión de red privada virtual (VPN) sobre el protocolo de seguridad de Internet (IKE v1 e IKE v2). Este tipo de conexión requiere un dispositivo VPN o un servicio de enrutamiento y acceso remoto (RRAS). Para más información acerca de VPN Gateway, consulte [Acerca de VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). La comunicación a través de este túnel está cifrada y es segura. Sin embargo, el ancho de banda está limitado por el rendimiento máximo del túnel (de 100 a 200 Mbps).
- **NAT de salida**. De forma predeterminada, todas las máquinas virtuales de Azure Stack tendrán conectividad a redes externas a través de NAT de salida. Cada red virtual que se crea en Azure Stack obtiene una dirección IP pública asignada. Si la máquina virtual se asigna directamente a una dirección IP pública o está detrás de un equilibrador de carga con una dirección IP pública, tendrá acceso de salida a través de NAT de salida mediante la dirección VIP de la red virtual. Esto funciona solo para la comunicación que inicia la máquina virtual y que se destina a redes externas (Internet o intranet). No se puede utilizar para comunicarse con la máquina virtual desde fuera.

### <a name="hybrid-connectivity-options"></a>Opciones de conectividad híbrida

Para la conectividad híbrida, es importante tener en cuenta qué tipo de implementación desea ofrecer y donde se implementará. Debe tener en cuenta si necesita aislar el tráfico de red por inquilino, y si tendrá una implementación de intranet o Internet.

- **Azure Stack de inquilino único**. Una implementación de Azure Stack que busca, al menos desde una perspectiva de la red, como si fuera un inquilino. Puede haber muchas suscripciones de inquilinos, pero al igual que con cualquier servicio de intranet, todo el tráfico se transmite a través de las mismas redes. El tráfico de red de una suscripción se transmite a través de la misma conexión de red que otra suscripción y no es necesario que esté aislado a través de un túnel cifrado.

- **Azure Stack de varios inquilinos**. Implementación de Azure Stack donde el tráfico de suscripción de cada inquilino que está limitado por redes externas a Azure Stack debe aislarse del tráfico de red de otros inquilinos.
 
- **Implementación en intranet**. Una implementación de Azure Stack que se encuentra en una intranet corporativa, normalmente en el espacio de direcciones IP privadas y detrás de uno o varios firewalls. Las direcciones IP públicas no son realmente públicas, ya que no se pueden redirigir directamente a través de la red pública de Internet.

- **Implementación en Internet**. Implementación de Azure Stack que está conectada a la red pública de Internet y usa las direcciones IP públicas que se pueden redirigir por Internet para el intervalo de dirección VIP público. La implementación todavía puede ubicarse detrás de un firewall, pero el intervalo de dirección VIP público es accesible directamente desde Azure y la red pública de Internet.
 
En la tabla siguiente se resumen los escenarios de conectividad híbrida, con las ventajas, las desventajas y los casos de uso.

| Escenario | Método de conectividad | Ventajas | Desventajas | Bueno para |
| -- | -- | --| -- | --|
| Azure Stack de inquilino único, implementación en intranet | NAT de salida | Mejor ancho de banda para transferencias más rápidas. Fácil de implementar; no se necesitan puertas de enlace. | Tráfico no cifrado; sin aislamiento ni cifrado fuera de la pila. | Implementaciones empresariales en las que se confía por igual en todos los inquilinos.<br><br>Empresas que tienen un circuito Azure ExpressRoute en Azure. |
| Azure Stack de varios inquilinos, implementación en intranet | VPN de sitio a sitio | El tráfico de la red virtual del inquilino al destino es seguro. | El ancho de banda está limitado por el túnel VPN de sitio a sitio.<br><br>Requiere una puerta de enlace en la red virtual y un dispositivo VPN en la red de destino. | Implementaciones empresariales en las que cierto tráfico de inquilinos debe protegerse de otros inquilinos. |
| Azure Stack de inquilino único, implementación en Internet | NAT de salida | Mejor ancho de banda para transferencias más rápidas. | Tráfico no cifrado; sin aislamiento ni cifrado fuera de la pila. | Escenarios de hospedaje en los que el inquilino obtiene su propia implementación de Azure Stack y un circuito dedicado en el entorno de Azure Stack. Por ejemplo, ExpressRoute y conmutación de etiquetas de multiprotocolo (MPLS).
| Azure Stack de varios inquilinos, implementación en Internet | VPN de sitio a sitio | El tráfico de la red virtual del inquilino al destino es seguro. | El ancho de banda está limitado por el túnel VPN de sitio a sitio.<br><br>Requiere una puerta de enlace en la red virtual y un dispositivo VPN en la red de destino. | Escenarios de hospedaje en los que el proveedor desea ofrecer una nube de varios inquilinos, donde los inquilinos no confían entre sí y el tráfico debe cifrarse.
|  |  |  |  |  |

### <a name="using-expressroute"></a>Uso de ExpressRoute

Puede conectar Azure Stack a Azure a través de [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) para escenarios de intranet de inquilino único y de varios inquilinos. Necesitará un circuito ExpressRoute aprovisionado a través de [un proveedor de conectividad](https://docs.microsoft.com/azure/expressroute/expressroute-locations).

El siguiente diagrama muestra ExpressRoute para un escenario de inquilino único (donde "Conexión del cliente" es el circuito de ExpressRoute).

![Diagrama que muestra un escenario de ExpressRoute de inquilino único](media/azure-stack-datacenter-integration/ExpressRouteSingleTenant.PNG)

El siguiente diagrama muestra ExpressRoute para un escenario de varios inquilinos.

![Diagrama que muestra un escenario de ExpressRoute de varios inquilinos](media/azure-stack-datacenter-integration/ExpressRouteMultiTenant.PNG)

## <a name="external-monitoring"></a>Supervisión externa
Para obtener una vista única de todas las alertas de la implementación de Azure Stack y dispositivos, así como para integrar las alertas en flujos de trabajo existentes de Administración de servicios de TI para el control de vales, puede [integrar Azure Stack en soluciones de supervisión del centro de datos externo](azure-stack-integrate-monitor.md).

Incluido con la solución de Azure Stack, el host de ciclo de vida de hardware es un equipo situado fuera de Azure Stack que ejecuta herramientas de administración proporcionadas por el distribuidor OEM para hardware. Puede usar estas herramientas u otras soluciones que se integran directamente con las soluciones de supervisión existentes en su centro de datos.

En la tabla siguiente se resume la lista de opciones disponibles actualmente.

| Ámbito | Solución de supervisión externa |
| -- | -- |
| Software de Azure Stack | [Módulo de administración de Azure Stack para Operations Manager](https://azure.microsoft.com/blog/management-pack-for-microsoft-azure-stack-now-available/)<br>[Complemento de Nagios](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details)<br>Llamadas API basadas en REST | 
| Servidores físicos (BMC a través de IPMI) | Hardware OEM: módulo de administración del distribuidor de Operations Manager<br>Solución suministrada por el distribuidor de hardware OEM<br>Complementos de Nagios del distribuidor de hardware | Solución de supervisión admitida por el asociado de OEM (incluido) | 
| Dispositivos de red (SNMP) | Detección de dispositivos de red de Operations Manager<br>Solución suministrada por el distribuidor de hardware OEM<br>Complemento conmutador de Nagios |
| Supervisión del estado de suscripción del inquilino | [Módulo de administración de System Center para Windows Azure](https://www.microsoft.com/download/details.aspx?id=50013) | 
|  |  | 

Tenga en cuenta los siguientes requisitos:
- La solución que use debe ser sin agente. No puede instalar agentes de terceros dentro de componentes de Azure Stack. 
- Si desea usar System Center Operations Manager, se requiere Operations Manager 2012 R2 u Operations Manager 2016.

## <a name="backup-and-disaster-recovery"></a>Copia de seguridad y recuperación ante desastres

El planeamiento de copia de seguridad y recuperación ante desastres implica planear tanto para la infraestructura subyacente de Azure Stack que hospeda máquinas virtuales de IaaS y servicios de PaaS como para los datos y las aplicaciones de inquilinos. Debe realizar estos planeamientos por separado.

### <a name="protect-infrastructure-components"></a>Protección de los componentes de infraestructura

Puede realizar una [copia de seguridad de los componentes de infraestructura de Azure Stack](azure-stack-backup-back-up-azure-stack.md) en el recurso compartido SMB que especifique:

- Necesitará un recurso compartido de archivos SMB externo en un servidor de archivos existente basado en Windows o un dispositivo de terceros.
- Debe usar este mismo recurso compartido para la copia de seguridad de los conmutadores de red y el host de ciclo de vida de hardware. El distribuidor de hardware OEM le servirá como guía para la copia de seguridad y la restauración de estos componentes ya que son externos a Azure Stack. Es responsable de ejecutar los flujos de trabajo de copia de seguridad según la recomendación del distribuidor de OEM.

Si se produce una pérdida de datos catastrófica, puede usar la copia de seguridad de la infraestructura para propagar los datos de la implementación, como las entradas de la implementación y los identificadores, las cuentas de servicio, el certificado de raíz de CA, los recursos federados (en implementaciones desconectadas), los planes, las ofertas, las suscripciones, las cuotas, las asignaciones de rol y directivas RBAC y los secretos de Key Vault.
 
### <a name="protect-tenant-applications-on-iaas-virtual-machines"></a>Protección de las aplicaciones del inquilino en máquinas virtuales de IaaS

Azure Stack no realiza copia de seguridad de los datos ni de las aplicaciones de inquilinos. Debe planear la copia de seguridad y la protección de recuperación ante desastres para un destino externo a Azure Stack. La protección de inquilino es una actividad controlada por el inquilino. En el caso de las máquinas virtuales de IaaS, los inquilinos pueden usar tecnologías de invitado para proteger las carpetas de archivos, los datos de la aplicación y el estado de sistema. Sin embargo, como proveedor empresarial o de un servicio, puede que desee ofrecer una solución de copia de seguridad y recuperación en el mismo centro de datos o externamente en una nube.

Para realizar copia de seguridad de máquinas virtuales de Linux o IaaS de Windows, debe usar productos de copia de seguridad con acceso al sistema operativo del invitado para proteger el archivo, la carpeta, el estado del sistema operativo y los datos de la aplicación. Puede usar Azure Backup, System Center Data Protection Manager o los productos de terceros compatibles.

Para replicar datos en una ubicación secundaria y orquestar la conmutación por error de la aplicación si se produce un desastre, puede usar Azure Site Recovery o productos de terceros compatibles. (En la versión inicial de los sistemas integrados, Azure Site Recovery no admite conmutación por recuperación. Sin embargo, puede lograr dicha conmutación a través de un proceso manual.). Además, las aplicaciones que admitan la replicación nativa (por ejemplo, Microsoft SQL Server) podrán replicar datos a otra ubicación donde se ejecute la aplicación.

> [!IMPORTANT]
> En la versión inicial de los sistemas integrados, se admiten las tecnologías de protección que funcionan en el nivel de invitado de una máquina virtual de IaaS. No se pueden instalar agentes en los servidores de infraestructura subyacente.

## <a name="learn-more"></a>Más información

- Para obtener información acerca de los casos de uso, las compras, los asociados y los distribuidores de hardware de OEM, consulte la página del producto de [Azure Stack](https://azure.microsoft.com/overview/azure-stack/).
- Para obtener información acerca del mapa de ruta y la disponibilidad geográfica para los sistemas integrados de Azure Stack, consulte las notas del producto: [Azure Stack: una extensión de Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 

## <a name="next-steps"></a>Pasos siguientes
[Modelos de conexión de la implementación de Azure Stack](azure-stack-connection-models.md)
