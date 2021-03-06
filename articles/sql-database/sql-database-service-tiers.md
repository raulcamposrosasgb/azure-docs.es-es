---
title: Modelos de compra de Azure SQL Database | Microsoft Docs
description: Información sobre el modelo de compra de Azure SQL Database.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 08/17/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: 5fcdf02fe75905fb3e492671ba44adb65dfd0da7
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "42144710"
---
# <a name="azure-sql-database-purchasing-models-and-resources"></a>Modelos de compra y recursos de Azure SQL Database 

Azure SQL Database le permite comprar fácilmente motores de base de datos PaaS completamente administrados que se ajusten a sus necesidades de rendimiento y costos. Según el modelo de implementación de Azure SQL Database, puede seleccionar el modelo de compra que se adapte a sus necesidades: 
 - Los [servidores lógicos](sql-database-logical-servers.md) de [Azure SQL Database](sql-database-technical-overview.md) ofrecen dos modelos de compra para recursos de proceso, almacenamiento y E/S: uno basado en DTU y el otro en [núcleos virtuales](sql-database-service-tiers-vcore.md). 
 - Las [Instancias administradas](sql-database-managed-instance.md) de Azure SQL Database solo ofrecen el [modelo de compra basado en núcleos virtuales](sql-database-service-tiers-vcore.md).

En la tabla y el gráfico siguientes se comparan y contrastan estos dos modelos de compra.

|**Modelo de compra**|**Descripción**|**Más adecuado para**|
|---|---|---|
|Modelo basado en DTU|Este modelo se basa en una medida agrupada de recursos de proceso, almacenamiento y E/S. Los niveles de rendimiento se expresan como unidades de transmisión de datos (DTU) para las bases de datos únicas y como unidades de transmisión de datos elásticas (eDTU) para los grupos de bases de datos elásticas. Para más información sobre las DTU y las eDTU, consulte [¿Qué son las DTU y las eDTU?](sql-database-service-tiers.md#what-are-database-transaction-units-dtus)|Recomendado para los clientes que desean opciones de recursos simples y configuradas previamente.| 
|Modelo basado en núcleos virtuales|Este modelo le permite elegir los recursos de proceso y almacenamiento de manera independiente. También permite usar Ventaja híbrida de Azure para SQL Server para ahorrar en los costos.|Recomendado para los clientes que valoran la flexibilidad, el control y la transparencia.|
||||  

![Modelo de precios](./media/sql-database-service-tiers/pricing-model.png)

## <a name="vcore-based-purchasing-model"></a>Modelo de compra basado en núcleos virtuales 

Un núcleo virtual representa la CPU lógica que ofrece una opción para elegir entre varias generaciones de hardware y las características físicas de hardware (por ejemplo, el número de núcleos, memoria, el tamaño de almacenamiento). El modelo de compra basado en núcleos virtuales le ofrece flexibilidad, control, transparencia de consumo de recursos individuales y una manera sencilla de trasladar los requisitos de carga de trabajo locales a la nube. Este modelo le permite escalar los recursos de proceso, memoria y almacenamiento en función de las necesidades de la carga de trabajo. En el modelo de compra basado en núcleos virtuales, los clientes pueden elegir entre los niveles de servicio [De uso general](sql-database-high-availability.md#standardgeneral-purpose-availability) y [Crítico para la empresa](sql-database-high-availability.md#premiumbusiness-critical-availability) para [bases de datos únicas](sql-database-single-database-scale.md), [instancias administradas](sql-database-managed-instance.md) y [grupos elásticos](sql-database-elastic-pool.md). 

El modelo de compra basado en núcleos virtuales permite elegir los recursos de proceso y almacenamiento de manera independiente, igualar el rendimiento local y optimizar el precio. En un modelo de compra basado en núcleos virtuales, los clientes pagan por:
- Proceso (nivel de servicio + número de núcleos virtuales y cantidad de memoria + generación de hardware)*
- Tipo y cantidad de almacenamiento de datos y registros 
- Número de E/S** - aplicable solo a [servidores lógicos](sql-database-logical-servers.md)
- Almacenamiento de copia de seguridad (RA-GRS)** 

\* En la versión preliminar pública inicial, las CPU lógicas de Gen 4 se basan en procesadores Intel E5-2673 v3 (Haswell) a 2,4 GHz.

\*\* Durante la versión preliminar, las copias de seguridad y las E/S son gratuitas.

> [!IMPORTANT]
> Los recursos de proceso, E/S y almacenamiento de datos y registros se cobran por base de datos o grupo elástico. El almacenamiento de copia de seguridad se cobra por cada base de datos. Para más información sobre los precios de Instancia administrada, consulte [Instancia administrada de Azure SQL Database](sql-database-managed-instance.md).
> **Limitaciones de región:** el modelo de compra basado en núcleos virtuales aún no está disponible en las regiones siguientes: Europa Occidental, Centro de Francia, Sur de Reino Unido, Oeste de Reino Unido y Sudeste de Australia.

Si la base de datos o el grupo elástico consumen más de 300 DTU, la conversión a núcleos virtuales puede reducir los costos. Puede realizar la conversión mediante la API de su elección o usar Azure Portal, sin experimentar tiempo de inactividad. Sin embargo, la conversión no es necesaria. Si el modelo de compra de DTU satisface sus requisitos de rendimiento y empresariales, debe seguir usándolo. Si decide pasar del modelo de DTU al modelo de núcleos virtuales, debe seleccionar el nivel de rendimiento mediante la siguiente regla general: cada 100 DTU del nivel Estándar requieren al menos 1 núcleo virtual en el nivel De uso general, y cada 125 DTU del nivel Premium requieren como mínimo 1 núcleo virtual en el nivel Crítico para la empresa.

## <a name="dtu-based-purchasing-model"></a>Modelo de compra basado en DTU

La unidad de transacción de base de datos (DTU) representa una medida combinada de CPU, memoria, lecturas y escrituras. El modelo de compra basado en DTU ofrece un conjunto de agrupaciones preconfiguradas de recursos de proceso y almacenamiento incluido para impulsar diferentes niveles de rendimiento de la aplicación. Los clientes que prefieren la simplicidad de una agrupación preconfigurada y pagos fijos cada mes pueden encontrar el modelo basado en DTU más adecuado para sus necesidades. En el modelo de compra basado en DTU, los clientes pueden elegir entre los niveles de servicio **Básico**, **Estándar** y **Premium** tanto para [bases de datos únicas](sql-database-single-database-scale.md) como para [grupos elásticos](sql-database-elastic-pool.md). Este modelo de compra no está disponible en [las instancias administradas](sql-database-managed-instance.md).

### <a name="what-are-database-transaction-units-dtus"></a>¿Qué son las unidades de transacción de base de datos?
Para una sola base de datos de Azure SQL en un nivel de rendimiento específico dentro de un [nivel de servicio](sql-database-single-database-scale.md), Microsoft garantiza un determinado nivel de recursos para esa base de datos (independiente de cualquier otra base de datos en la nube de Azure), de forma que ofrece un nivel de rendimiento predecible. La cantidad de recursos se calcula como un número de unidades de transacción de base de datos o DTU y es una medida combinada de recursos de proceso, almacenamiento y E/S. La relación entre estos recursos se determinó originalmente mediante una [carga de trabajo OLTP de prueba comparativa](sql-database-benchmark-overview.md) diseñada para representar las típicas cargas de trabajo OLTP reales. Cuando la carga de trabajo supera la cantidad de cualquiera de estos recursos, se limita el rendimiento, con lo que se obtiene un rendimiento y tiempos de espera más lentos. Los recursos usados por la carga de trabajo no afectan a los recursos disponibles para otras bases de datos SQL en la nube de Azure, y los recursos usados por otras cargas de trabajo no afectan a los recursos disponibles para la base de datos SQL.

![rectángulo de selección](./media/sql-database-what-is-a-dtu/bounding-box.png)

Las DTU son especialmente útiles para comprender la cantidad relativa de recursos entre bases de datos de Azure SQL Database en diferentes niveles de rendimiento y niveles de servicio. Por ejemplo, duplicar el número de DTU al aumentar el nivel de rendimiento de una base de datos equivale a duplicar el conjunto de recursos disponibles para esa base de datos. Por ejemplo, una base de datos Premium P11 con 1750 DTU proporciona una potencia de proceso de DTU 350 veces mayor que una base de datos básica con 5 DTU.  

Para obtener información más detallada sobre el consumo de recursos (DTU) de la carga de trabajo, use [Información de rendimiento de consultas de Azure SQL Database](sql-database-query-performance.md) para:

- Identificar las consultas principales por CPU, duración y recuento de ejecuciones, que se pueden ajustar para mejorar el rendimiento. Por ejemplo, una consulta de uso intensivo de E/S puede beneficiarse del uso de [técnicas de optimización en memoria](sql-database-in-memory.md) para hacer un mejor uso de la memoria disponible en un cierto nivel de rendimiento y de nivel de servicio.
- Profundizar en los detalles de una consulta, ver su texto e historial de uso de recursos.
- Tener acceso a recomendaciones de ajuste del rendimiento que muestran las acciones realizadas por el [SQL Database Advisor](sql-database-advisor.md).

### <a name="what-are-elastic-database-transaction-units-edtus"></a>¿Qué son las unidades de transacción de base de datos elásticas (eDTU)?
En lugar de proporcionar un conjunto exclusivo de recursos (DTU) que puede que no siempre sea necesario para una SQL Database que está siempre disponible, puede colocar las bases de datos en un [grupo elástico](sql-database-elastic-pool.md) en un servidor de SQL Database que comparte un grupo de recursos entre esas bases de datos. Los recursos compartidos de un grupo elástico se miden mediante unidades de transacción de bases de datos elásticas o eDTU. Los grupos elásticos proporcionan una solución sencilla y rentable para administrar los objetivos de rendimiento de varias bases de datos que tienen patrones de uso muy diferentes e imprevisibles. Un grupo elástico garantiza que una base de datos del grupo no puede consumir los recursos, mientras que asegura que cada base de datos del grupo tenga disponible siempre una cantidad mínima de recursos necesarios. 

![Introducción a SQL Database: eDTU por servicio y nivel](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

A un grupo se le asigna un número fijo de eDTU, por un precio fijo. Dentro del grupo elástico, a las bases de datos individuales se les proporciona la flexibilidad de escalarse automáticamente dentro de los límites configurados. Una base de datos con cargas más elevadas consumirá más eDTU para satisfacer la demanda. Las bases de datos con cargas más bajas consumirán menos eDTU. Las bases de datos sin cargas no consumirán ningún eDTU. Mediante el aprovisionamiento de recursos para todo el grupo, en lugar de por base de datos, se simplifican las tareas de administración, de forma que se proporciona un presupuesto del grupo predecible.

Se pueden agregar eDTU adicionales a un grupo existente sin que la base de datos experimente tiempo de inactividad ni que las bases de datos del grupo resulten afectadas. De igual forma, si las eDTU adicionales dejan de necesitarse, se pueden quitar de cualquier grupo existente en cualquier momento. Puede agregar o quitar bases de datos del grupo, o limitar la cantidad de eDTU que puede usar una base de datos cuando está sobrecargada para reservar eDTU para otras bases de datos. Si una base de datos infrautiliza recursos de forma predecible, es posible sacarla del grupo y configurarla como una base de datos única con una cantidad predecible de recursos necesarios.

### <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>¿Cómo se puede determinar el número de DTU necesarias para la carga de trabajo?
Si desea migrar una carga de trabajo de máquina virtual existente local o de SQL Server en Azure SQL Database, puede usar la [calculadora de DTU](http://dtucalculator.azurewebsites.net/) para hacer una estimación del número aproximado de DTU que se necesitan. Para una carga de trabajo existente de Azure SQL Database, puede usar la [información de rendimiento de consultas de SQL Database](sql-database-query-performance.md) para comprender el consumo de recursos de la base de datos (DTU) y obtener información más detallada para optimizar la carga de trabajo. También puede usar la DMV [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) para ver el consumo de recursos de la última hora. Como alternativa, la vista de catálogo [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) muestra el consumo de recursos de los últimos 14 días, aunque con una fidelidad inferior media de cinco minutos.

### <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>¿Cómo se puede saber si podría beneficiarme de un grupo elástico de recursos?
Los grupos son apropiados para un amplio número de bases de datos con patrones de utilización específicos. Para una base de datos determinada, este patrón está caracterizado por una utilización media baja con picos de utilización relativamente poco frecuentes. SQL Database evalúa automáticamente el historial de uso de recursos de bases de datos en un servidor de SQL Database existente y recomienda la configuración de grupo apropiada en Azure Portal. Para más información, consulte [¿Cuándo se debe utilizar un grupo elástico?](sql-database-elastic-pool.md)


## <a name="next-steps"></a>Pasos siguientes

- Consulte este artículo sobre el [modelo de compra basado en núcleos virtuales](sql-database-service-tiers-vcore.md) si quiere obtener más información al respecto.
- Con respecto al modelo de compra basado en DTU, consulte [Modelo de compra basado en DTU](sql-database-service-tiers-dtu.md).
