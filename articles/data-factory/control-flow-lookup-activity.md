---
title: Azure Data factory'de arama etkinliği | Microsoft Docs
description: Bir dış kaynaktan bir değeri aramak için arama etkinliğini kullanmayı öğrenin. Bu çıktı, daha fazla etkinliklerde başvurulabilir.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: shlo
ms.openlocfilehash: 4f0662a71ee14af3c2c1aafee210641fc8b51f1b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60768667"
---
# <a name="lookup-activity-in-azure-data-factory"></a>Azure Data factory'de arama etkinliği

Arama etkinliği Azure Data Factory tarafından desteklenen veri kaynaklarının herhangi bir veri kümesi alabilirsiniz. Bunu, aşağıdaki senaryoda kullanın:
- Nesne adı sabit kodlama yerine bir sonraki etkinlik çalışması için hangi nesnelerin dinamik olarak belirler. Bazı nesne dosyaları ve tabloları verilebilir.

Arama etkinliği okur ve bir yapılandırma dosyası veya tablo içeriğini döndürür. Ayrıca, bir sorgu veya saklı yordam yürütme sonucunu döndürür. Tekil değer ise çıktısı arama etkinliği, bir sonraki kopyalama veya dönüştürme etkinliği içinde kullanılabilir. Özniteliklerin dizisini ise ForEach etkinliği, çıkış kullanılabilir.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Aşağıdaki veri kaynaklarını arama etkinliği için desteklenir. 5.000, en büyük arama etkinliği tarafından döndürülebilecek satır sayısı olan 2 MB boyutunda. Şu anda, arama için en uzun süre etkinlik zaman aşımından önce bir saattir.

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores-for-lookup-activity.md)]

## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "LookupActivity",
    "type": "Lookup",
    "typeProperties": {
        "source": {
            "type": "<source type>"
            <additional source specific properties (optional)>
        },
        "dataset": { 
            "referenceName": "<source dataset name>",
            "type": "DatasetReference"
        },
        "firstRowOnly": false
    }
}
```

## <a name="type-properties"></a>Tür özellikleri

Ad | Açıklama | Tür | Gerekli mi?
---- | ----------- | ---- | --------
Veri kümesi | Veri kümesi başvurusu arama için sağlar. Ayrıntılı bilgi **veri kümesi özellikleri** karşılık gelen her Bağlayıcısı makalesi bölümü. | Anahtar/değer çifti | Evet
source | Kopyalama etkinliği kaynak ile aynı veri kümesine özgü kaynak özelliklerini içerir. Ayrıntılı bilgi **kopyalama etkinliği özellikleri** karşılık gelen her Bağlayıcısı makalesi bölümü. | Anahtar/değer çifti | Evet
firstRowOnly | Yalnızca ilk satırı veya tüm satırları döndürülüp döndürülmeyeceğini gösterir. | Boolean | Hayır. Varsayılan değer: `true`.

> [!NOTE]
> 
> * Kaynak sütunlar **ByteArray** türü desteklenmez.
> * **Yapı** veri kümesi tanımında desteklenmiyor. Metin biçimi dosyaları için üst bilgi satırı sütun adını belirtmek için kullanın.
> * Arama kaynağınız bir JSON dosyası ise `jsonPathDefinition` ayarı JSON nesnesi yeniden şekillendirilmesine desteklenmiyor. Tüm nesneleri alınır.

## <a name="use-the-lookup-activity-result-in-a-subsequent-activity"></a>Arama etkinliği sonucu sonraki bir etkinliği kullanma

Arama sonucu döndürülür `output` etkinlik çalıştırma sonucunu bölümü.

* **Zaman `firstRowOnly` ayarlanır `true` (varsayılan)**, aşağıdaki kodda gösterildiği gibi çıktı biçimidir. Arama sonucu sabit altında olan `firstRow` anahtarı. Sonuç, sonraki etkinliği kullanmak için desenini kullanın `@{activity('MyLookupActivity').output.firstRow.TableName}`.

    ```json
    {
        "firstRow":
        {
            "Id": "1",
            "TableName" : "Table1"
        }
    }
    ```

* **Zaman `firstRowOnly` ayarlanır `false`** , aşağıdaki kodda gösterildiği gibi çıktı biçimidir. A `count` alanının kaç kayıtlar döndürülür. Ayrıntılı değerleri görüntülenir sabit altında `value` dizisi. Böyle bir durumda, arama etkinliği tarafından izlenen bir [Foreach etkinliği](control-flow-for-each-activity.md). Geçirdiğiniz `value` ForEach etkinliği dizisine `items` desenini kullanarak alan `@activity('MyLookupActivity').output.value`. Erişim öğelere `value` dizisi, aşağıdaki sözdizimini kullanın: `@{activity('lookupActivity').output.value[zero based index].propertyname}`. `@{activity('lookupActivity').output.value[0].tablename}` bunun bir örneğidir.

    ```json
    {
        "count": "2",
        "value": [
            {
                "Id": "1",
                "TableName" : "Table1"
            },
            {
                "Id": "2",
                "TableName" : "Table2"
            }
        ]
    } 
    ```

### <a name="copy-activity-example"></a>Kopyalama etkinliği örneği
Bu örnekte, kopyalama etkinliği verileri bir SQL tablosunu Azure SQL veritabanı örneğiniz Azure Blob depolama alanına kopyalar. SQL tablosunun adı, Blob depolama alanındaki bir JSON dosyasında depolanır. Arama etkinliği çalışma zamanında tablo adını arar. JSON, bu yaklaşımı kullanarak dinamik olarak değiştirilir. Ardışık düzen veya veri kümesi dağıtmanız gerekmez. 

Bu örnek yalnızca ilk satır için arama gösterir. Örneklerde tüm satırları ve sonuçları ile ForEach etkinliği zincirleyebilir, yani için arama için bkz. [Azure Data Factory kullanarak birden çok tabloyu toplu olarak kopyalama](tutorial-bulk-copy.md).

### <a name="pipeline"></a>İşlem hattı
Bu işlem hattı iki etkinlik içerir: Arama ve kopyalama. 

- Arama etkinliği kullanacak şekilde yapılandırılmış **LookupDataset**, Azure Blob depolama alanındaki bir konuma başvuruyor. Arama etkinliği SQL tablosunun adı bu konumda bir JSON dosyasından okur. 
- SQL tablosunun adı arama etkinliğin çıktı kopyalama etkinliği kullanır. **TableName** özelliğinde **SourceDataset** arama etkinliğinden gelen çıkış kullanacak şekilde yapılandırıldı. SQL tablosundan etkinlik kopya verileri Azure Blob depolama alanındaki bir konuma kopyalayın. Tarafından belirtilen konuma **SinkDataset** özelliği. 

```json
{
    "name": "LookupPipelineDemo",
    "properties": {
        "activities": [
            {
                "name": "LookupActivity",
                "type": "Lookup",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "dataset": { 
                        "referenceName": "LookupDataset", 
                        "type": "DatasetReference" 
                    }
                }
            },
            {
                "name": "CopyActivity",
                "type": "Copy",
                "typeProperties": {
                    "source": { 
                        "type": "SqlSource", 
                        "sqlReaderQuery": "select * from @{activity('LookupActivity').output.firstRow.tableName}" 
                    },
                    "sink": { 
                        "type": "BlobSink" 
                    }
                },                
                "dependsOn": [ 
                    { 
                        "activity": "LookupActivity", 
                        "dependencyConditions": [ "Succeeded" ] 
                    }
                 ],
                "inputs": [ 
                    { 
                        "referenceName": "SourceDataset", 
                        "type": "DatasetReference" 
                    } 
                ],
                "outputs": [ 
                    { 
                        "referenceName": "SinkDataset", 
                        "type": "DatasetReference" 
                    } 
                ]
            }
        ]
    }
}
```

### <a name="lookup-dataset"></a>Arama veri kümesi
**Arama** DataSet **sourcetable.json** tarafından belirtilen Azure depolama arama klasörü dosyasında **AzureStorageLinkedService** türü. 

```json
{
    "name": "LookupDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "lookup",
            "fileName": "sourcetable.json",
            "format": {
                "type": "JsonFormat",
                "filePattern": "SetOfObjects"
            }
        },
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="source-dataset-for-copy-activity"></a>**Kaynak** veri kümesi için kopyalama etkinliği
**Kaynak** SQL tablosunun adı arama etkinliğin çıkış veri kümesini kullanır. Bu SQL tablosundan etkinlik kopya verileri Azure Blob depolama alanındaki bir konuma kopyalayın. Tarafından belirtilen konuma **havuz** veri kümesi. 

```json
{
    "name": "SourceDataset",
    "properties": {
        "type": "AzureSqlTable",
        "typeProperties":{
            "tableName": "@{activity('LookupActivity').output.firstRow.tableName}"
        },
        "linkedServiceName": {
            "referenceName": "AzureSqlLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="sink-dataset-for-copy-activity"></a>**Havuz** veri kümesi için kopyalama etkinliği
Kopyalama etkinliği için SQL tablodan veri kopyalar **filebylookup.csv** dosyası **csv** Azure depolama alanında bir klasör. Dosyayı tarafından belirtilen **AzureStorageLinkedService** özelliği. 

```json
{
    "name": "SinkDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "csv",
            "fileName": "filebylookup.csv",
            "format": {
                "type": "TextFormat"                                                                    
            }
        },
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a>Azure Storage bağlı hizmeti
Bu depolama hesabı ile SQL tabloların adlarının JSON dosyası içerir. 

```json
{
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "value": "DefaultEndpointsProtocol=https;AccountName=<StorageAccountName>;AccountKey=<StorageAccountKey>",
                "type": "SecureString"
            }
        }
    },
        "name": "AzureStorageLinkedService"
}
```

### <a name="azure-sql-database-linked-service"></a>Azure SQL Veritabanı bağlı hizmeti
Bu Azure SQL veritabanı, Blob depolama alanına kopyalanacak verileri içerir. 

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": {
                "value": "Server=<server>;Initial Catalog=<database>;User ID=<user>;Password=<password>;",
                "type": "SecureString"
            }
        }
    }
}
```

### <a name="sourcetablejson"></a>SourceTable.JSON

#### <a name="set-of-objects"></a>Nesne

```json
{
  "Id": "1",
  "tableName": "Table1"
}
{
   "Id": "2",
  "tableName": "Table2"
}
```

#### <a name="array-of-objects"></a>Nesne dizisi

```json
[ 
    {
        "Id": "1",
        "tableName": "Table1"
    },
    {
        "Id": "2",
        "tableName": "Table2"
    }
]
```

## <a name="limitations-and-workarounds"></a>Sınırlamalar ve geçici çözümler

Arama etkinliği ve önerilen geçici çözümleri bazı sınırlamalar aşağıda verilmiştir.

| Sınırlama | Geçici çözüm |
|---|---|
| Arama etkinliği, en fazla 5000 satır ve 2 MB boyut sınırı vardır. | Burada en fazla satır veya boyutunu aşmadığını veri alır. bir iç işlem hattı dış işlem hattı yinelenir iki düzeyli işlem hattı tasarım. |
| | |

## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen diğer denetim akışı etkinlikleri bakın: 

- [İşlem hattı yürütme etkinliği](control-flow-execute-pipeline-activity.md)
- [ForEach etkinliği](control-flow-for-each-activity.md)
- [GetMetadata activity](control-flow-get-metadata-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
