---
title: Copia de datos de Teradata mediante Azure Data Factory | Microsoft Docs
description: "Obtenga información sobre el conector Teradata del servicio Data Factory que permite copiar datos desde una base de datos Teradata a almacenes de datos compatibles con Data Factory como receptores."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2017
ms.author: jingwang
ms.openlocfilehash: 77cf65930a6d622e54395dd563901977c9d76170
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2017
---
# <a name="copy-data-from-teradata-using-azure-data-factory"></a>Copia de datos de Teradata mediante Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versión 1: disponibilidad general](v1/data-factory-onprem-teradata-connector.md)
> * [Versión 2: versión preliminar](connector-teradata.md)

En este artículo se explica el uso de la actividad de copia de Azure Data Factory para copiar datos desde una base de datos Teradata. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que describe información general de la actividad de copia.

> [!NOTE]
> Este artículo se aplica a la versión 2 de Data Factory, que actualmente se encuentra en la versión preliminar. Si usa la versión 1 del servicio Data Factory, que está disponible con carácter general, vea el artículo sobre [la versión 1 del conector Teradata](v1/data-factory-onprem-teradata-connector.md).

## <a name="supported-scenarios"></a>Escenarios admitidos

Puede copiar datos desde una base de datos Teradata a cualquier almacén de datos receptor compatible. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

En concreto, este conector Teradata admite las siguientes funcionalidades:

- Teradata **versión 12 y versiones posteriores**.
- La copia de datos con autenticación **básica** o de **Windows**.

## <a name="prerequisites"></a>Requisitos previos

Para usar este conector ODBC, necesitará lo siguiente:

- Configurar un entorno Integration Runtime autohospedado. Consulte el artículo sobre [Integration Runtime autohospedado](create-self-hosted-integration-runtime.md) para más información.
- Instalar el [proveedor de datos .NET para Teradata](http://go.microsoft.com/fwlink/?LinkId=278886) versión 14 o posterior en la máquina de Integration Runtime.

## <a name="getting-started"></a>Introducción
Puede crear una canalización con la actividad de copia mediante el SDK de .NET, el SDK de Python, Azure PowerShell, la API de REST o la plantilla de Azure Resource Manager. Consulte el [tutorial de actividad de copia](quickstart-create-data-factory-dot-net.md) para obtener instrucciones paso a paso sobre cómo crear una canalización con una actividad de copia.

Las secciones siguientes proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas del conector Teradata.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado Teradata:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type se debe establecer en: **Teradata** | Sí |
| server | Nombre del servidor de Teradata. | Sí |
| authenticationType | Tipo de autenticación usado para conectarse a la base de datos Teradata.<br/>Los valores permitidos son: **Básica** y **Windows**. | Sí |
| nombre de usuario | Especifique el nombre de usuario para conectarse a la base de datos Teradata. | Sí |
| Contraseña | Especifique la contraseña de la cuenta de usuario especificada para el nombre de usuario. Marque este campo como SecureString. | Sí |
| connectVia | El entorno [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Tal y como se mencionó en los [requisitos previos](#prerequisites), se requiere un entorno Integration Runtime autohospedado. |Sí |

**Ejemplo:**

```json
{
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "server": "<server>",
            "authenticationType": "Basic",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
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

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre conjuntos de datos. En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de Teradata.

Para copiar datos desde Teradata, establezca la propiedad type del conjunto de datos en **RelationalTable**. Se admiten las siguientes propiedades:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del conjunto de datos debe establecerse en: **RelationalTable** | Sí |
| tableName | Nombre de la tabla de la base de datos Teradata. | No (si se especifica "query" en el origen de la actividad) |

**Ejemplo:**

```json
{
    "name": "TeradataDataset",
    "properties":
    {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<Teradata linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que admite el origen de Teradata.

### <a name="teradata-as-source"></a>Teradata como origen

Para copiar datos desde Teradata, establezca el tipo de origen de la actividad de copia como **RelationalSource**. Se admiten las siguientes propiedades en la sección **source** de la actividad de copia:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del origen de la actividad de copia debe establecerse en: **RelationalSource** | Sí |
| query | Use la consulta SQL personalizada para leer los datos. Por ejemplo: `"SELECT * FROM MyTable"`. | No (si se especifica "tableName" en el conjunto de datos) |

**Ejemplo:**

```json
"activities":[
    {
        "name": "CopyFromTeradata",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Teradata input dataset name>",
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
                "type": "RelationalSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-teradata"></a>Asignación de tipos de datos para Teradata

Al copiar datos desde Teradata, se usan las siguientes asignaciones de tipos de datos de Teradata en los tipos de datos provisionales de Azure Data Factory. Consulte el artículo sobre [asignaciones de tipos de datos y esquema](copy-activity-schema-and-type-mapping.md) para información sobre cómo la actividad de copia asigna el tipo de datos y el esquema de origen al receptor.

| Base de datos Teradata | Tipo de datos provisionales de Data Factory |
|:--- |:--- |
| BigInt |Int64 |
| Blob |Byte[] |
| Byte |Byte[] |
| ByteInt |Int16 |
| Char |String |
| Clob |String |
| Date |DateTime |
| Decimal |Decimal |
| Doble |Double |
| Graphic |String |
| Entero |Int32 |
| Interval Day |TimeSpan |
| Interval Day To Hour |TimeSpan |
| Interval Day To Minute |TimeSpan |
| Interval Day To Second |TimeSpan |
| Interval Hour |TimeSpan |
| Interval Hour To Minute |TimeSpan |
| Interval Hour To Second |TimeSpan |
| Interval Minute |TimeSpan |
| Interval Minute To Second |TimeSpan |
| Interval Month |String |
| Interval Second |TimeSpan |
| Interval Year |String |
| Interval Year To Month |String |
| Number |Double |
| Period(Date) |String |
| Period(Time) |String |
| Period(Time With Time Zone) |String |
| Period(Timestamp) |String |
| Period(Timestamp With Time Zone) |String |
| SmallInt |Int16 |
| Hora |TimeSpan |
| Time With Time Zone |String |
| Timestamp |DateTime |
| Timestamp With Time Zone |Datetimeoffset |
| VarByte |Byte[] |
| VarChar |String |
| VarGraphic |String |
| Xml |String |


## <a name="next-steps"></a>Pasos siguientes
Consulte los [almacenes de datos compatibles](copy-activity-overview.md##supported-data-stores-and-formats) para ver la lista de almacenes de datos que la actividad de copia de Azure Data Factory admite como orígenes y receptores.