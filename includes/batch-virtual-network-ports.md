- La red virtual debe estar en la misma **región** y **suscripción** de Azure que la cuenta de Batch.

- En los grupos creados con una configuración de máquina virtual, solo se admiten las redes virtuales basadas en Azure Resource Manager. En los grupos creados con una configuración de servicios en la nube, solo se admiten redes virtuales clásicas.
  
- Para utilizar una red virtual clásica, la entidad de servicio `MicrosoftAzureBatch` debe tener el rol de Control de acceso basado en roles (RBAC) `Classic Virtual Machine Contributor` para la red virtual especificada. Para usar una red virtual de Azure Resource Manager, debe tener permisos de acceso a la red virtual y para implementar máquinas virtuales en la subred.

- La subred especificada para el grupo debe tener suficientes direcciones IP sin asignar para acoger el número de VM destinadas al grupo; esto es, la suma de las propiedades `targetDedicatedNodes` y `targetLowPriorityNodes` del grupo. Si la subred no tiene suficientes direcciones IP sin asignar, el grupo asigna parcialmente los nodos de proceso y se produce un error de cambio de tamaño. 

- Los grupos de la configuración de máquina virtual implementada en una red virtual de Azure asignan automáticamente recursos de red de Azure adicionales. Se necesitan los siguientes recursos para cada 50 nodos de grupo en una red virtual: 1 grupo de seguridad de red, 1 dirección IP pública y 1 equilibrador de carga. Estos recursos están limitados por [cuotas](../articles/batch/batch-quota-limit.md) en la suscripción que contiene la red virtual proporcionada al crear el grupo de Batch.

- La red virtual debe permitir la comunicación desde el servicio Batch para poder programar tareas en los nodos de proceso. Esto se puede verificar si la red virtual tiene algún grupo de seguridad de red (NSG) asociado. Si un NSG deniega la comunicación con los nodos de proceso en la subred especificada, el servicio Batch establece el estado de dichos nodos en **No utilizable**. 

- Si la red virtual especificada tiene asociados NSG o un firewall, configure los puertos entrantes y salientes como se muestra en las tablas siguientes:


  |    Puertos de destino    |    Dirección IP de origen      |   Puerto de origen    |    ¿Agrega Batch los grupos de seguridad de red?    |    ¿Es necesario para que la máquina virtual se pueda usar?    |    Acción del usuario   |
  |---------------------------|---------------------------|----------------------------|----------------------------|-------------------------------------|-----------------------|
  |   <ul><li>Para los grupos creados con la configuración de máquina virtual 29876 y 29877</li><li>Para los grupos creados con la configuración de servicioS en la nube 10100, 20100 y 30100</li></ul>        |    * <br /><br />Aunque esto requiere efectivamente "permitir todo", el servicio Batch aplica un grupo de seguridad de red en el nivel de interfaz de red en cada máquina virtual creada en la configuración de máquina virtual que filtra todas las direcciones IP que no sean del servicio Batch. | * o 443 |    Sí. Batch agrega los grupos de seguridad de red en el nivel de las interfaces de red (NIC) que se conectan a las máquinas virtuales. Estos grupos de seguridad de red permiten el tráfico únicamente desde las direcciones IP de rol de servicio de Batch. Aunque abra estos puertos a Internet, el tráfico se bloqueará en la NIC. |    SÍ  |  No es necesario especificar un grupo de seguridad de red, ya que Batch solo permite direcciones IP de Batch. <br /><br /> Sin embargo, si lo hace, asegúrese de que estos puertos estén abiertos al tráfico entrante.|
  |    3389 (Windows), 22 (Linux)               |    Máquinas de usuario de depuración, para poder acceder a la máquina virtual de manera remota.    |   *  | Sin                                     |    Sin                     |    Agregue grupos de seguridad de red si desea permitir el acceso remoto (RDP o SSH) a la máquina virtual.   |                                


  |    Puertos de salida    |    Destino    |    ¿Agrega Batch los grupos de seguridad de red?    |    ¿Es necesario para que la máquina virtual se pueda usar?    |    Acción del usuario    |
  |------------------------|-------------------|----------------------------|-------------------------------------|------------------------|
  |    443    |    Azure Storage    |    Sin     |    SÍ    |    Si agrega algún grupo de seguridad de red, asegúrese de que este puerto esté abierto al tráfico saliente.    |

   Además, asegúrese de que todos los servidores DNS personalizados que dan servicio a red virtual pueden resolver el punto de conexión de Azure Storage. En concreto, las direcciones URL del tipo `<account>.table.core.windows.net`, `<account>.queue.core.windows.net` y `<account>.blob.core.windows.net` deben poderse resolver. 

   Si agrega una instancia de Resource Manager basada en NSG, puede hacer uso de [etiquetas de servicio](../articles/virtual-network/security-overview.md#service-tags) para seleccionar las direcciones IP de almacenamiento para la región específica en las conexiones salientes. Tenga en cuenta que las direcciones IP de almacenamiento deben ser de la misma región que la cuenta de lote y la red virtual. Las etiquetas de servicio están en la versión preliminar en algunas regiones de Azure.