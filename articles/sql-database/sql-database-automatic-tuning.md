---
title: 'Azure SQL Database: ajuste automático | Microsoft Docs'
description: Azure SQL Database analiza una consulta SQL y se adapta automáticamente a la carga de trabajo del usuario.
services: sql-database
author: danimir
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: v-daljep
ms.reviewer: carlrab
ms.openlocfilehash: 38b59c28096b23a22b216158d9e945a2881a4f41
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189265"
---
# <a name="automatic-tuning-in-azure-sql-database"></a>Ajuste automático en Azure SQL Database

El ajuste automático de Azure SQL Database proporciona un rendimiento óptimo y cargas de trabajo estables gracias al ajuste continuo del rendimiento basado en inteligencia artificial y aprendizaje automático.

El ajuste automático es un servicio de rendimiento inteligente completamente administrado que usa la inteligencia integrada para supervisar continuamente las consultas ejecutadas en una base de datos y mejora automáticamente su rendimiento. Esto se logra mediante la adaptación dinámica de la base de datos a las cargas de trabajo cambiantes y la aplicación de las recomendaciones de ajuste. El ajuste automático aprende horizontalmente de todas las bases de datos de Azure mediante la inteligencia artificial y mejora dinámicamente sus acciones de ajuste. Cuanto más tiempo se ejecute una base de datos de Azure SQL Database con el ajuste automático activado, mejor rendimiento tendrá.

El ajuste automático de Azure SQL Database puede se una de las características que puede habilitar para proporcionar cargas de trabajo de base de datos estables y de rendimiento óptimo.

## <a name="what-can-automatic-tuning-do-for-you"></a>¿Cómo le puede ayudar el ajuste automático?

- Ajuste del rendimiento automatizado de bases de datos de Azure SQL Database
- Comprobación automática de mejoras de rendimiento
- Corrección y reversión automatizadas
- Historial de ajuste
- Scripts T-SQL de acción de ajuste para implementaciones manuales
- Supervisión proactiva del rendimiento de las cargas de trabajo
- Funcionalidad de escalabilidad horizontal de cientos de miles de bases de datos
- Impacto positivo para los recursos de DevOps y el costo total de propiedad

## <a name="safe-reliable-and-proven"></a>Seguro, confiable y probado

Las operaciones de ajuste aplicadas a bases de datos de Azure SQL Database son totalmente seguras para el rendimiento de las cargas de trabajo más intensas. El sistema se ha diseñado cuidadosamente para que no interfiera con las cargas de trabajo del usuario. Las recomendaciones de ajuste automático se aplican solo durante momentos de uso escaso. El sistema también puede deshabilitar temporalmente las operaciones de ajuste automático para proteger el rendimiento de la carga de trabajo. En este caso, se mostrará el mensaje "Disabled by the system" (Deshabilitado por el sistema) en Azure Portal. El ajuste automático tiene en cuenta las cargas de trabajo con la máxima prioridad de recursos.

Los mecanismos de ajuste automático están muy desarrollados y se han perfeccionado en varios millones de bases de datos que se ejecutan en Azure. Las operaciones de ajuste automatizado aplicadas se comprueban automáticamente para asegurarse de que hay una mejora en el rendimiento de la carga de trabajo. Las recomendaciones de rendimiento con regresión se detectan de forma dinámica y se revierten rápidamente. A través del historial de ajustes existe un claro seguimiento de las mejoras de los ajustes realizados en las instancias de Azure SQL Database. 

![¿Cómo funciona el ajuste automático?](./media/sql-database-automatic-tuning/how-does-automatic-tuning-work.png)

El ajuste automático de Azure SQL Database comparte su lógica básica con el motor de ajuste automático de SQL Server. Para obtener información técnica adicional sobre el mecanismo de inteligencia integrada, vea el artículo sobre el [ajuste automático de SQL Server](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning).

## <a name="use-automatic-tuning"></a>Uso del ajuste automático

El ajuste automático debe habilitarse manualmente en la suscripción. Para habilitar el ajuste automático mediante Azure Portal, vea [Habilitación del ajuste automático](sql-database-automatic-tuning-enable.md).

El ajuste automático puede funcionar de forma autónoma mediante la aplicación automática de las recomendaciones de ajuste, incluida la comprobación automática de las mejoras de rendimiento. 

Para tener un mayor control, se puede desactivar la aplicación automática de las recomendaciones de ajuste para aplicarlas manualmente a través de Azure Portal. También es posible usar la solución que permite solo ver las recomendaciones de ajuste automático y aplicarlas manualmente a través de los scripts y las herramientas que prefiera. 

Para obtener información general sobre cómo funciona el ajuste automático y los escenarios de uso habituales, vea el vídeo insertado:


> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Improve-Azure-SQL-Database-Performance-with-Automatic-Tuning/player]
>

## <a name="automatic-tuning-options"></a>Opciones de ajuste automático

Las opciones de ajuste automático disponibles en Azure SQL Database son:
 1. **CREATE INDEX**: identifica los índices que pueden mejorar el rendimiento de la carga de trabajo, crea índices y comprueba automáticamente que el rendimiento de las consultas ha mejorado.
 2. **DROP INDEX**: identifica diariamente los índices duplicados y redundantes, excepto los índices únicos, y aquellos que no se han usado durante mucho tiempo (más de 90 días). Tenga en cuenta que esta opción no es compatible con las aplicaciones que usan sugerencias de índice y conmutación de particiones.
 3. **FORCE LAST GOOD PLAN**: identifica consultas SQL que usan un plan de ejecución más lento que el plan correcto anterior, y consultas que usan el último plan correcto conocido, en lugar del plan revertido.

El ajuste automático identifica las recomendaciones de **CREATE INDEX**, **DROP INDEX** y **FORCE LAST GOOD PLAN** que pueden optimizar el rendimiento de su base de datos, las muestra en [Azure Portal](sql-database-advisor-portal.md) y las expone a través de [T-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azuresqldb-current) y [API REST](https://docs.microsoft.com/rest/api/sql/serverautomatictuning).

Puede aplicar manualmente las recomendaciones de ajuste mediante el portal, o bien puede dejar que Ajuste automático aplique de forma autónoma las recomendaciones de ajuste. Las ventajas de dejar que el sistema aplique las recomendaciones de ajuste de forma autónoma es que en ese caso valida automáticamente y se produce una ganancia positiva en el rendimiento de la carga de trabajo o bien, si se detecta una regresión, revertirá automáticamente la recomendación de ajuste. Tenga en cuenta de que si las consultas implicadas en las recomendaciones de ajuste no se ejecutan con frecuencia,la fase de validación puede durar hasta 72 horas por diseño. En caso de que aplique las recomendaciones de ajuste de forma manual, ni la validación automática del rendimiento ni los mecanismos de inversión estarán disponibles.

Las opciones de ajuste automático se pueden habilitar o deshabilitar en cada base de datos de forma independiente o pueden configurarse en servidores lógicos y aplicarse en todas las bases de datos que hereden la configuración del servidor. Los servidores lógicos pueden heredar valores predeterminados de Azure para la configuración de optimización automática. Los valores predeterminados de Azure en este momento son FORCE_LAST_GOOD_PLAN (habilitado), CREATE_INDEX (habilitado) y DROP_INDEX (deshabilitado).

El método recomendado para configurar el ajuste automático es configurar las opciones de ajuste automático en un servidor y utilizar la configuración heredada en las bases de datos que pertenecen al servidor primario. De ese modo, resulta más fácil administrar las opciones de ajuste automático en un gran número de bases de datos.

## <a name="next-steps"></a>Pasos siguientes

- Para habilitar la característica de ajuste automático en Azure SQL Database para administrar la carga de trabajo, vea [Habilitación del ajuste automático](sql-database-automatic-tuning-enable.md).
- Para revisar manualmente y aplicar las recomendaciones de ajuste automático, vea [Búsqueda y aplicación de recomendaciones de rendimiento](sql-database-advisor-portal.md).
- Para aprender a usar T-SQL para aplicar y ver las recomendaciones de ajuste automático, consulte [Manage automatic tuning via T-SQL](https://azure.microsoft.com/blog/automatic-tuning-introduces-automatic-plan-correction-and-t-sql-management/) (Administración del ajuste automático a través de T-SQL).
- Para aprender a crear notificaciones por correo electrónico para las recomendaciones de ajuste automático, consulte [Notificaciones por correo electrónico para el ajuste automático](sql-database-automatic-tuning-email-notifications.md).
- Para más información sobre la inteligencia integrada que se usa en el ajuste automático, consulte [Artificial Intelligence tunes Azure SQL Databases](https://azure.microsoft.com/blog/artificial-intelligence-tunes-azure-sql-databases/) (La inteligencia artificial ajusta las bases de datos de Azure SQL Database).
- Para más información sobre cómo funciona el ajuste automático en Azure SQL Database y SQL Server 2017, consulte [SQL Server automatic tuning](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) (Ajuste automático de SQL Server).
