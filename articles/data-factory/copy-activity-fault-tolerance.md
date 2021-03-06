---
title: Tolerancia a errores de la actividad de copia en Azure Data Factory | Microsoft Docs
description: "Obtenga información sobre cómo agregar una tolerancia a errores para la actividad de copia en Azure Data Factory omitiendo las filas incompatibles."
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
ms.date: 09/05/2017
ms.author: jingwang
ms.openlocfilehash: 5b2658cecba80ef871cc38b930b0e52bc3952530
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2017
---
#  <a name="fault-tolerance-of-copy-activity-in-azure-data-factory"></a>Tolerancia a errores de la actividad de copia en Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versión 1: Disponibilidad general](v1/data-factory-copy-activity-fault-tolerance.md)
> * [Versión 2: Versión preliminar](copy-activity-fault-tolerance.md)

La actividad de copia de Azure Data Factory ofrece dos maneras de tratar filas incompatibles cuando se copian datos entre almacenes de datos de origen y de receptor:

- Puede anular y omitir la actividad de copia cuando se encuentren datos incompatibles (comportamiento predeterminado).
- Se puede continuar copiando todos los datos a través de la incorporación de tolerancia a errores y la omisión de filas de datos incompatibles. Además, puede registrar las filas incompatibles en Azure Blob Storage. Luego puede examinar el registro para obtener información sobre la causa del error, corregir los datos en el origen de datos y reintentar la actividad de copia.

> [!NOTE]
> Este artículo se aplica a la versión 2 de Data Factory, que actualmente se encuentra en versión preliminar. Si usa la versión 1 del servicio Data Factory, que está disponible con carácter general, vea [la tolerancia a errores de la actividad de copia en V1](v1/data-factory-copy-activity-fault-tolerance.md).


 ## <a name="supported-scenarios"></a>Escenarios admitidos
La actividad de copia admite tres escenarios para detectar, omitir y registrar datos incompatibles:

- **Incompatibilidad entre el tipo de datos de origen y el tipo nativo de receptor**. <br/><br/> Por ejemplo, copie los datos desde un archivo CSV en Blob Storage a una base de datos SQL con una definición de esquema que contiene tres columnas de tipo INT. Las filas del archivo CSV que contienen datos numéricos, como 123 456 789, se copian correctamente en el almacén de receptor. Pero las filas que contienen valores no numéricos, como 123 456 abc, se detectan como incompatibles y se omiten.
- **Error de coincidencia en el número de columnas entre el origen y el receptor**. <br/><br/> Por ejemplo: copie los datos desde un archivo CSV en Blob Storage a una base de datos SQL con una definición de esquema que contiene seis columnas. Las filas del archivo CSV que contiene seis columnas se copian correctamente en el almacén de receptor. Las filas del archivo CSV que contienen más o menos de seis columnas se detectan como incompatibles y se omiten.
- **Infracción de clave principal al escribir en una base de datos relacional**.<br/><br/> Por ejemplo: copie datos desde un servidor SQL a una base de datos SQL. Se define una clave principal en la base de datos SQL de receptor, pero no se define en el servidor SQL de origen. Las filas duplicadas que existen en el origen no se pueden copiar en el receptor. La actividad de copia solo copia la primera fila de los datos de origen en el receptor. Las filas de origen subsiguientes que contienen el valor de clave principal duplicado se detectan como incompatibles y se omiten.

## <a name="configuration"></a>Configuración
En el ejemplo siguiente se proporciona una definición JSON para configurar la omisión de las filas incompatibles en la actividad de copia:

```json
"typeProperties": {
    "source": {
        "type": "BlobSource"
    },
    "sink": {
        "type": "SqlSink",
    },         
    "enableSkipIncompatibleRow": true,           
    "redirectIncompatibleRowSettings": {
         "linkedServiceName": {
              "referenceName": "AzureBlobLinkedService",
              "type": "LinkedServiceReference"
            },
            "path": "redirectcontainer/erroroutput"
     }
}
```
Propiedad | Descripción | Valores permitidos | Obligatorio
-------- | ----------- | -------------- | -------- 
enableSkipIncompatibleRow | Especifica si se deben omitir las filas incompatibles durante la copia o no. | True<br/>False (valor predeterminado) | No
redirectIncompatibleRowSettings | Un grupo de propiedades que puede especificarse cuando quiere registrar las filas incompatibles. | &nbsp; | No
linkedServiceName | El servicio vinculado de Azure Storage para almacenar el registro que contiene las filas que se omiten. | El nombre de un servicio vinculado de AzureStorage o AzureStorageSas, que hace referencia a la instancia de almacenamiento que quiere usar para almacenar el archivo de registro. | No
path | La ruta de acceso del archivo de registro que contiene las filas que se omiten. | Especifique la ruta de acceso de Blob Storage que desee usar para registrar los datos incompatibles. Si no se proporciona una ruta de acceso, el servicio creará un contenedor para usted. | No

## <a name="monitor-skipped-rows"></a>Supervisar filas omitidas
Una vez finalizada la ejecución de la actividad de copia, puede ver el número de filas omitidas en la salida de la actividad de copia:

```json
"output": {
            "dataRead": 95,
            "dataWritten": 186,
            "rowsCopied": 9,
            "rowsSkipped": 2,
            "copyDuration": 16,
            "throughput": 0.01,
            "redirectRowPath": "https://myblobstorage.blob.core.windows.net//myfolder/a84bf8d4-233f-4216-8cb5-45962831cd1b/",
            "errors": []
        },

```
Si configura el registro de las filas incompatibles, encontrará el archivo de registro en esta ruta de acceso: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv`. 

Los archivos de registro solo pueden ser archivos csv. Los datos originales que se omitan se registrarán con una coma como delimitador de columna, si es necesario. Se agregan dos columnas más "ErrorCode" y "ErrorMessage" a los datos de origen originales en el archivo de registro, en las que podrá ver la causa principal de la incompatibilidad. El nombre de estas dos columnas aparece entre comillas dobles. 

A continuación se muestra un ejemplo del contenido del archivo de registro:

```
data1, data2, data3, "UserErrorInvalidDataValue", "Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' to type 'DateTime'."
data4, data5, data6, "2627", "Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. The duplicate key value is (data4)."
```

## <a name="next-steps"></a>Pasos siguientes
Consulte los otros artículos de la actividad de copia:

- [Información general de la actividad de copia](copy-activity-overview.md)
- [Rendimiento de la actividad de copia](copy-activity-performance.md)


