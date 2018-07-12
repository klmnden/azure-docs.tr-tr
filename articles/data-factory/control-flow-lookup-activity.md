---
title: Azure Data factory'de arama etkinliği | Microsoft Docs
description: Bir dış kaynaktan bir değeri aramak için arama etkinliğini kullanmayı öğrenin. Sonraki etkinliklerde bu çıktıya daha fazla başvurulabilir.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: shlo
ms.openlocfilehash: 25ed439674fcf7136e29034eb97e0652ae9ba111
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38237841"
---
# <a name="lookup-activity-in-azure-data-factory"></a>Azure Data factory'de arama etkinliği

Arama etkinliği, ADF tarafından desteklenen veri kaynağının herhangi bir veri kümesi almak için kullanılabilir.  Aşağıdaki senaryoda kullanılabilir:
- Hangi nesnelerin (dosyalar, tablolar, vb.) üzerinde nesne adı sabit kodlama yerine bir sonraki etkinlik çalışması için dinamik olarak belirleme

Arama etkinliği, okuma ve bir yapılandırma dosyası, bir yapılandırma tablo veya bir sorgu veya saklı yordam yürütme sonucu içeriğini döndürür.  Arama etkinliğin çıkışı tekil değer ise bir sonraki kopyalama veya dönüştürme etkinliği kullanılan veya bir dizi öznitelikleri ise ForEach etkinliği, kullanılan.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Aşağıdaki veri kaynaklarını arama için desteklenir. Maksimum satır sayısı etkinlik araması tarafından döndürülen **5000**ve en fazla **2MB** boyutu. Ve şu anda zaman aşımından önce arama etkinliği max süresi bir saattir.

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
veri kümesi | Veri kümesi başvurusu arama için sağlar. Karşılık gelen her Bağlayıcısı makalesi "Veri kümesi özellikleri" bölümünde ayrıntıları alın. | Anahtar/değer çifti | Evet
source | Kopyalama etkinliği kaynak ile aynı veri kümesine özgü kaynak özelliklerini içerir. Karşılık gelen her Bağlayıcısı makalesi "kopyalama etkinliğinin özellikleri" bölümünde ayrıntıları alın. | Anahtar/değer çifti | Evet
firstRowOnly | Yalnızca ilk satırı veya tüm satırları döndürülüp döndürülmeyeceğini gösterir. | Boole | Hayır. `true` varsayılan değerdir.

**Aşağıdaki noktalara dikkat edin:**

1. Kaynak sütunu ByteArray türü desteklenmiyor.
2. Yapısı, veri kümesi tanımında desteklenmiyor. Metin biçimi dosyalar için özellikle sütun adı sağlamak için üst bilgi satırı kullanabilirsiniz.
3. Arama kaynağınız bir JSON dosyaları ise `jsonPathDefinition` JSON nesnesi yeniden şekillendirme desteklenmiyor ayarı, tüm nesneleri alınabilir.

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

* **Zaman `firstRowOnly` ayarlanır `false`** , aşağıdaki kodda gösterildiği gibi çıktı biçimidir. A `count` alanının kaç kayıtlar döndürülür ve ayrıntılı değerleri görüntülenir sabit altında `value` dizisi. Böyle bir durumda, arama etkinliğini genellikle takip bir [Foreach etkinliği](control-flow-for-each-activity.md). Geçirebilirsiniz `value` ForEach etkinliği dizisine `items` desenini kullanarak alan `@activity('MyLookupActivity').output.value`. Erişim öğelere `value` dizisi, aşağıdaki sözdizimini kullanın: `@{activity('lookupActivity').output.value[zero based index].propertyname}`. Bir örnek aşağıda verilmiştir: `@{activity('lookupActivity').output.value[0].tablename}`.

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

## <a name="example"></a>Örnek
Bu örnekte, kopyalama etkinliği verileri bir SQL tablosunu Azure SQL veritabanı örneğiniz Azure Blob depolama alanına kopyalar. SQL tablosunun adı, Blob depolama alanındaki bir JSON dosyasında depolanır. Arama etkinliği çalışma zamanında tablo adını arar. Bu yaklaşım, JSON işlem hatları veya veri kümelerine yeniden gerek kalmadan dinamik olarak değiştirilmesine izin verir. 

Bu örnek yalnızca ilk satır için arama gösterir. Örneklerde tüm satırları ve sonuçları ile ForEach etkinliği zincirleyebilir, yani için arama için bkz. [Azure Data Factory kullanarak birden çok tabloyu toplu olarak kopyalama](tutorial-bulk-copy.md).

### <a name="pipeline"></a>İşlem hattı
Bu işlem hattı iki etkinlik içerir: *arama* ve *kopyalama*. 

- Arama etkinliği, Azure Blob depolama alanındaki bir konuma başvuran LookupDataset kullanmak için yapılandırılır. Arama etkinliği SQL tablosunun adı bu konumda bir JSON dosyasından okur. 
- Arama etkinliği (SQL tablosunun adı) çıktısını kopyalama etkinliği kullanır. Kaynak veri kümesi (SourceDataset) tableName özelliğini arama etkinliğinden gelen çıkış kullanmak için yapılandırılır. Kopyalama etkinliği verileri SQL tablosundan SinkDataset özelliği tarafından belirtilen Azure Blob depolama alanındaki bir konuma kopyalar. 


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
Arama veri kümesinin başvurduğu *sourcetable.json* AzureStorageLinkedService türü tarafından belirtilen Azure depolama arama klasörü dosyasında. 

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

### <a name="source-dataset-for-the-copy-activity"></a>Kaynak veri kümesi için kopyalama etkinliği
Kaynak veri kümesi çıkışını SQL tablosunun adı arama etkinliğini kullanır. Kopyalama etkinliği havuz veri kümesi tarafından belirtilen Azure Blob depolama alanındaki bir konuma bu SQL tablodan veri kopyalar. 

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

### <a name="sink-dataset-for-the-copy-activity"></a>Havuz veri kümesi için kopyalama etkinliği
Kopyalama etkinliği için SQL tablosu veri kopyalar *filebylookup.csv* dosyası *csv* AzureStorageLinkedService özelliği tarafından belirtilen Azure depolama alanında bir klasör. 

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

## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen diğer denetim akışı etkinlikleri bakın: 

- [İşlem hattı yürütme etkinliği](control-flow-execute-pipeline-activity.md)
- [Her etkinlik için](control-flow-for-each-activity.md)
- [Meta veri alma etkinliği](control-flow-get-metadata-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
