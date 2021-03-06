---
title: Copia de datos con Azure Cosmos DB como origen o destino mediante Data Factory | Microsoft Docs
description: Obtenga información sobre cómo copiar datos desde cualquier almacén de datos de origen compatible a Azure Cosmos DB o desde Cosmos DB a cualquier almacén de receptor compatible mediante Data Factory.
services: data-factory, cosmosdb
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/28/2018
ms.author: jingwang
ms.openlocfilehash: 1afd64fbd7019164f0e1f5c850f2dcd8250cdbfc
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2018
ms.locfileid: "39600343"
---
# <a name="copy-data-to-or-from-azure-cosmos-db-using-azure-data-factory"></a>Copia de datos con Azure Cosmos DB como origen o destino mediante Azure Data Factory

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versión 1](v1/data-factory-azure-documentdb-connector.md)
> * [Versión actual](connector-azure-cosmos-db.md)

En este artículo se resume el uso de la actividad de copia de Azure Data Factory para copiar datos con Azure Cosmos DB (API de SQL) como origen o destino. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que describe información general de la actividad de copia.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Puede copiar datos desde Azure Cosmos DB a cualquier almacén de datos de receptor compatible, o desde cualquier almacén de datos de origen admitido a Azure Cosmos DB. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

En concreto, este conector de Azure Cosmos DB admite las siguientes funcionalidades:

- [API de SQL](https://docs.microsoft.com/azure/cosmos-db/documentdb-introduction) de Cosmos DB.
- Importación o exportación de documentos JSON tal cual, o copia de datos con un conjunto de datos tabular como origen o destino, por ejemplo, base de datos SQL, archivos CSV, etc. Para copiar datos tal cual con archivos JSON u otra colección de Cosmos DB como origen o destino, consulte [Import/Export JSON documents](#importexport-json-documents) (Importación o exportación de documentos JSON).

Data Factory se integra con la [biblioteca BulkExecutor en Cosmos DB](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) para proporcionar el mejor rendimiento de escritura en Cosmos DB.

## <a name="getting-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

En las secciones siguientes se proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas de Azure Cosmos DB.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de Azure Cosmos DB:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type debe establecerse en: **CosmosDb**. | SÍ |
| connectionString |Especifique la información necesaria para conectarse a la base de datos de Azure Cosmos DB. Tenga en cuenta que debe especificar la información de la base de datos en la cadena de conexión como el ejemplo siguiente. Marque este campo como SecureString para almacenarlo de forma segura en Data Factory o [para hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). |SÍ |
| connectVia | El entorno [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Puede usar los entornos Integration Runtime (autohospedado) (si el almacén de datos se encuentra en una red privada) o Azure Integration Runtime. Si no se especifica, se usará Azure Integration Runtime. |Sin  |

**Ejemplo:**

```json
{
    "name": "CosmosDbLinkedService",
    "properties": {
        "type": "CosmosDb",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre conjuntos de datos. En esta sección se proporciona una lista de las propiedades que el conjunto de datos de Azure Cosmos DB admite.

Para copiar datos con Azure Cosmos DB como origen o destino, establezca la propiedad type del conjunto de datos en **DocumentDbCollection**. Se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type del conjunto de datos debe establecerse en: **DocumentDbCollection**. |SÍ |
| collectionName |Nombre de la colección de documentos de Cosmos DB. |SÍ |

**Ejemplo:**

```json
{
    "name": "CosmosDbDataset",
    "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName":{
            "referenceName": "<Azure Cosmos DB linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "collectionName": "<collection name>"
        }
    }
}
```

### <a name="schema-by-data-factory"></a>Esquema de Data Factory

En los almacenes de datos sin esquemas como Azure Cosmos DB, la actividad de copia deduce el esquema de una de las maneras siguientes. Por lo tanto, a menos que desee [importar o exportar documentos JSON tal cual](#importexport-json-documents), el procedimiento recomendado consiste en especificar la estructura de datos en la sección **structure**.

*. Si especifica la estructura de los datos mediante la propiedad **structure** en la definición del conjunto de datos, el servicio Data Factory respeta esta como la estructura del esquema. En este caso, si una fila no contiene un valor para una columna, se le proporcionará un valor nulo.
*. Si no especifica la estructura de los datos mediante la propiedad **structure** en la definición del conjunto de datos, el servicio Data Factory deduce el esquema utilizando la primera fila en los datos. En este caso, si la primera fila no contiene el esquema completo, algunas columnas se pueden perder en el resultado de la operación de copia.

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que el receptor y el origen de Azure Cosmos DB admiten.

### <a name="azure-cosmos-db-as-source"></a>Azure Cosmos DB como origen

Para copiar datos desde Azure Cosmos DB, establezca el tipo de origen de la actividad de copia en **DocumentDbCollectionSource**. Se admiten las siguientes propiedades en la sección **source** de la actividad de copia:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type del origen de la actividad de copia debe establecerse en: **DocumentDbCollectionSource**. |SÍ |
| query |Especifique la consulta Cosmos DB para leer datos.<br/><br/>Ejemplo: `SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Sin  <br/><br/>Si no se especifica, la instrucción SQL que se ejecuta: `select <columns defined in structure> from mycollection` |
| nestingSeparator |Carácter especial para indicar que el documento está anidado y cómo aplanar el conjunto de resultados.<br/><br/>Por ejemplo, si una consulta de Cosmos DB devuelve un resultado anidado `"Name": {"First": "John"}`, la actividad de copia identificará el nombre de columna como "Name.First" con el valor "John" cuando nestedSeparator sea punto. |No (el valor predeterminado es punto `.`) |

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyFromCosmosDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Document DB input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DocumentDbCollectionSource",
                "query": "SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-cosmos-db-as-sink"></a>Azure Cosmos DB como receptor

Para copiar datos en Azure Cosmos DB, establezca el tipo de receptor de la actividad de copia en **DocumentDbCollectionSink**. Se admiten las siguientes propiedades en la sección **source** de la actividad de copia:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad type del receptor de la actividad de copia debe establecerse en: **DocumentDbCollectionSink**. |SÍ |
| writeBehavior |Describe cómo escribir datos en Cosmos DB. Los valores permitidos son: `insert` y `upsert`.<br/>El comportamiento de **upsert** consiste en reemplazar el documento si ya existe un documento con el mismo identificador; en caso contrario, lo inserta. Tenga en cuenta que ADF genera automáticamente un identificador para el documento si no se especifica en el documento original o mediante la asignación de columnas, lo que significa que debe asegurarse de que el documento tenga un "id" para que upsert funcione según lo esperado. |No, el valor predeterminado es insert. |
| writeBatchSize | Data Factory usa la [biblioteca BulkExecutor en Cosmos DB](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) para escribir datos en Cosmos DB. "writeBatchSize" controla el tamaño de los documentos que se proporcionan a la biblioteca cada vez. Puede intentar aumentar writeBatchSize para mejorar el rendimiento. |No, el valor predeterminado es 10 000 |
| nestingSeparator |Un carácter especial en el nombre de columna de origen que indica que el documento anidado es necesario. <br/><br/>Por ejemplo, `Name.First` en la estructura del conjunto de datos de salida genera la siguiente estructura JSON en el documento de Cosmos DB cuando nestedSeparator es punto: `"Name": {"First": "[value maps to this column from source]"}`. |No (el valor predeterminado es punto `.`) |

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyToCosmosDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Document DB output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "DocumentDbCollectionSink",
                "writeBehavior": "upsert"
            }
        }
    }
]
```

## <a name="importexport-json-documents"></a>Importación o exportación de documentos JSON

Con este conector de Cosmos DB, le resultará muy sencillo

* Importar documentos JSON desde varios orígenes a Cosmos DB, incluido Azure Blob, Azure Data Lake Store y otros almacenes basados en archivos compatibles con Azure Data Factory.
* Exportar documentos JSON de la colección de Cosmos DB a varios almacenes basados en archivos.
* Copiar documentos entre dos colecciones de Cosmos DB tal cual.

Para lograr dicha copia independiente del esquema:

* Cuando use la herramienta de copia de datos, active la opción **"Export as-is to JSON files or Cosmos DB collection"** (Exportar tal cual a archivos JSON o colección de Cosmos DB).
* Al usar la creación de actividad, no especifique la sección "structure" (también conocida como esquema) en los conjuntos de datos de Cosmos DB ni la propiedad "nestingSeparator" en el receptor u origen de Cosmos DB en la actividad de copia. Al realizar operaciones de importación o exportación con archivos JSON, en el conjunto de datos del almacén de archivos correspondiente, especifique el tipo de formato como "JsonFormat" y la configuración "filePattern" correctamente (consulte la sección [JSON format](supported-file-formats-and-compression-codecs.md#json-format) (Formato JSON) para obtener más información) y, luego, no especifique la sección "structure"(también conocida como esquema) y omita la configuración de formato restante.

## <a name="next-steps"></a>Pasos siguientes
Consulte los [almacenes de datos compatibles](copy-activity-overview.md##supported-data-stores-and-formats) para ver la lista de almacenes de datos que la actividad de copia de Azure Data Factory admite como orígenes y receptores.
