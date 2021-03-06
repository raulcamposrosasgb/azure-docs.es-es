---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 08/14/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: a7a4e4b487c324bada818d4815f253110f7f7a60
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "40234863"
---
# <a name="standard-ssd-managed-disks-for-azure-virtual-machine-workloads"></a>Discos administrados SSD estándar para cargas de trabajo de máquinas virtuales de Azure

Los discos administrados de estado sólido (SSD) estándar de Azure son una opción de almacenamiento rentable, optimizada para cargas de trabajo que necesitan un rendimiento constante en niveles inferiores de IOPS. Los discos SSD estándar ofrecen una buena experiencia de nivel de entrada para aquellos que desean pasarse a la nube, en especial si tiene problemas con la variación de las cargas de trabajo que se ejecutan en las soluciones de disco duro locales. Los SSD estándar ofrecen una mejor disponibilidad, coherencia, confiabilidad y latencia en comparación con los discos duros, y son adecuados para servidores web, servidores de aplicaciones de baja IOPS, aplicaciones empresariales poco utilizadas y cargas de trabajo de desarrollo y pruebas.

## <a name="standard-ssd-features"></a>Características de SSD estándar

**Discos administrados**: los discos SSD estándar solo están disponibles como discos administrados. No se admiten discos no administrados ni blobs en páginas en SSD estándar. Al crear el disco administra, especifica el tipo de disco como SSD estándar e indica el tamaño del disco que necesita, y Azure crea y administrar el disco automáticamente.
Los discos SSD estándar admiten todas las operaciones de administración de servicios que ofrecen los discos administrados. Por ejemplo, puede crear, copiar o crear una instantánea de discos administrados SSD estándar de la misma manera que hacer con los discos administrados.

**Máquinas virtuales**: los discos SSD estándar se pueden utilizar con todas las VM de Azure, incluidos los tipos de VM que no admiten discos Premium. Por ejemplo, si usa una VM de la serie A, serie N o serie DS, o cualquier otra serie de VM de Azure, puede usar discos SSD estándar con esa VM. Con la introducción de discos SSD estándar, permite una amplia gama de cargas de trabajo que anteriormente utilizaban discos basados en HDD para realizar la transición a discos basados en SSD, y disfruta de un rendimiento coherente, una mayor disponibilidad, una mejor latencia y una mejor experiencia general que vienen con los SSD.

**Mayor durabilidad y disponibilidad**: los discos SSD estándar se basan en la misma plataforma de los discos de Azure, que ofrecen alta disponibilidad y durabilidad de los discos. Los discos de Azure están diseñados para ofrecer una disponibilidad del 99,999 %. Al igual que todos los discos administrados, los discos SSD estándar ofrecen también almacenamiento con redundancia local (LRS). Con LRS, la plataforma mantiene varias réplicas de cada disco y ofrece de manera constante durabilidad de nivel empresarial para discos IaaS, con una tasa de error anualizada de cero por cierto líder en el sector.

**Instantáneas**: al igual que todos los discos administrados, los discos SSD estándar también admiten la creación de instantáneas. El tipo de instantánea puede ser Estándar (HDD) o Premium (SSD). Para ahorrar costos, se recomienda el tipo de instantánea Estándar (HDD) para todos los tipos de disco de Azure. Esto es porque cuando se crea un disco administrado desde una instantánea, siempre podrá elegir un nivel superior, como SSD estándar o SSD Premium.

## <a name="scalability-and-performance-targets"></a>Objetivos de escalabilidad y rendimiento

La tabla siguiente contiene los tamaños de disco que actualmente se ofrecen para SSD estándar.

|Tipo de disco SSD estándar  |Tamaño del disco  |IOPS por disco  |Rendimiento de disco  |
|---------|---------|---------|---------|
|E10     |128 GB         |Hasta 500         |Hasta 60 MiB por segundo         |
|E15     |256 GiB         |Hasta 500         |Hasta 60 MiB por segundo         |
|E20     |512 GB         |Hasta 500         |Hasta 60 MiB por segundo         |
|E30     |1024 GiB         |Hasta 500         |Hasta 60 MiB por segundo         |
|E40     |2048 GiB         |Hasta 500         |Hasta 60 MiB por segundo         |
|E50     |4095 GiB         |Hasta 500         |Hasta 60 MiB por segundo         |

Los discos SSD estándar están diseñados para proporcionar latencias de milisegundos de un solo dígito para la mayoría de las operaciones de E/S, y para entregar las IOPS y el rendimiento hasta los límites descritos en la tabla anterior. Las IOPS y el rendimiento reales pueden variar a veces, según los patrones de tráfico. Los discos SSD estándar proporcionarán un rendimiento más coherente que los discos HDD, con una latencia menor.

Por otro lado, los discos SSD Premium funcionan mejor que los SSD estándar, con latencias bajas, alto IOPS/rendimiento, y coherencia aún mejor con el rendimiento de discos aprovisionados. Es el tipo de disco recomendado para cargas de trabajo de producción críticas. Si la carga de trabajo requiere compatibilidad con discos de gran rendimiento y baja latencia, considere la posibilidad de usar Premium Storage.

Al igual que los SSD Premium, los SSD estándar también usan un tamaño de unidad de E/S de 256 KiB. Si el tamaño de los datos transferidos es inferior a 256 KiB, se considera 1 unidad de E/S. Los tamaños de E/S más grandes se cuentan como varias operaciones de E/S con un tamaño de 256 KiB. Por ejemplo, una E/S de 1100 KB se cuenta como cinco unidades de E/S.

## <a name="pricing-and-billing"></a>Precios y facturación

Al usar SSD estándar, se aplican las siguientes consideraciones de facturación:

- Tamaño del disco administrado
- Instantáneas
- Transferencias de datos de salida
- Transacciones

**Tamaño del disco administrado:** los discos administrados se facturan por el tamaño aprovisionado. Azure asigna el tamaño aprovisionado (redondeado al alza) a la oferta de tamaño de disco más cercana. Para obtener detalles sobre los tamaños de disco que se ofrecen, consulte la tabla en la sección Objetivos de escalabilidad y rendimiento anterior. Cada disco se asigna a un tamaño de disco aprovisionado compatible y se factura según corresponda. Por ejemplo, si ha aprovisionado un disco SSD estándar de 200 GiB, se asignará a la oferta de tamaño de disco E15 (256 GiB). La facturación de cualquier disco aprovisionado se prorratea cada hora con el precio mensual de la oferta de Premium Storage. Por ejemplo, si ha aprovisionado un disco E10 y lo ha eliminado después de 20 horas, se factura la oferta E10 prorrateada a 20 horas. Esto es así independientemente de la cantidad de datos que se escriba en el disco.

**Instantáneas**: las instantáneas de discos administrados se facturan por la capacidad usada por las instantáneas, en el destino y en el origen, de existir. Para obtener más información sobre las instantáneas, consulte [Instantáneas de disco administradas](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview#managed-disk-snapshots).

**Transferencias de datos de salida**: las [transferencias de datos de salida](https://azure.microsoft.com/pricing/details/bandwidth/) (datos que salen de los centros de datos de Azure) se facturan en función del uso de ancho de banda.

**Transacciones**: al igual que los discos HDD estándar, las transacciones en los discos SSD estándar conllevan una facturación. Las transacciones incluyen las operaciones de lectura y escritura en el disco. El tamaño de la unidad de E/S utilizado para contabilizar las transacciones en SSD estándar es de 256 KiB. Los tamaños de E/S más grandes se cuentan como varias operaciones de E/S con un tamaño de 256 KiB.

Para obtener más información acerca de los precios de máquinas virtuales y Managed Disks, consulte:

- [Precios de máquinas virtuales](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)
- [Precios de Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la creación de SSD estándar, consulte el ejemplo que muestra cómo crear una VM con varios SSD estándar.

> [!div class="nextstepaction"]
> [Crear una VM con varios SSD estándar](https://github.com/azure/azure-quickstart-templates/tree/master/101-vm-with-standardssd-disk/)