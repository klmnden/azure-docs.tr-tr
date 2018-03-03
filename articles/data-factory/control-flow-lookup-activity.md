---
title: "Azure veri fabrikası'nda arama etkinliği | Microsoft Docs"
description: "Bir dış kaynaktan bir değeri aramak için arama etkinliği kullanmayı öğrenin. Sonraki etkinliklerde bu çıktıya daha fazla başvurulabilir."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: 2f551e97b833460c7c4ccd276b0df1dae562c03b
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="lookup-activity-in-azure-data-factory"></a>Azure veri fabrikası'nda arama etkinliği
Okuma veya bir kayıt, tablo adı veya değer herhangi bir dış kaynaktan aramak için arama etkinliği kullanın. Sonraki etkinliklerde bu çıktıya daha fazla başvurulabilir. 

Arama etkinliği, dinamik olarak dosyaları, kayıt veya Tablo listesini bir yapılandırma dosyası veya bir veri kaynağından almak istediğinizde faydalıdır. Etkinlik çıkışı, daha fazla öğelerden üzerinde işleme belirli gerçekleştirmek için diğer etkinlikler tarafından kullanılabilir.

> [!NOTE]
> Bu makale, şu anda önizleme sürümünde olan Azure Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık 1. sürümünü kullanıyorsanız bkz. [Data Factory sürüm 1 belgeleri](v1/data-factory-introduction.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Aşağıdaki veri kaynakları şu anda arama için desteklenir:
- Azure Blob storage JSON dosyasında
- JSON dosyasının dosya sistemi
- Azure SQL veritabanı (sorgudan dönüştürülen JSON verilerini)
- Azure SQL veri ambarı (sorgudan dönüştürülen JSON verilerini)
- SQL Server (sorgudan dönüştürülen JSON verilerini)
- Azure Table storage (sorgudan dönüştürülen JSON verilerini)

Arama etkinlik tarafından döndürülen satır sayısının üst sınırını olan **5000**ve kadar **10MB** boyutu.

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
Veri kümesi | Veri kümesi başvurusu için arama sağlar. Şu anda desteklenen veri türleri şunlardır:<ul><li>`AzureBlobDataset` için [Azure Blob Depolama](connector-azure-blob-storage.md#dataset-properties) kaynağı olarak</li><li>`FileShareDataset` için [dosya sistemi](connector-file-system.md#dataset-properties) kaynağı olarak</li><li>`AzureSqlTableDataset` için [Azure SQL veritabanı](connector-azure-sql-database.md#dataset-properties) veya [Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md#dataset-properties) kaynağı olarak</li><li>`SqlServerTable` için [SQL Server](connector-sql-server.md#dataset-properties) kaynağı olarak</li><li>`AzureTableDataset` için [Azure Table depolama](connector-azure-table-storage.md#dataset-properties) kaynağı olarak</li> | Anahtar/değer çifti | Evet
kaynak | Kopya etkinliği kaynak ile aynı veri kümesi-özel kaynak özelliklerini içerir. Karşılık gelen her bağlayıcı makale içindeki "etkinlik özellikleri Kopyala" bölümünden ayrıntıları alın. | Anahtar/değer çifti | Evet
firstRowOnly | Yalnızca ilk satırı veya tüm satırları döndürülmeyeceğini gösterir. | Boole | Hayır. `true` varsayılan değerdir.

## <a name="use-the-lookup-activity-result-in-a-subsequent-activity"></a>Arama etkinlik sonuç izleyen bir etkinlikte kullanma

Arama sonuç döndürülür `output` etkinliğin sonucu çalışma bölümü.

* **Zaman `firstRowOnly` ayarlanır `true` (varsayılan)**, aşağıdaki kodda gösterildiği gibi çıktı biçimidir. Arama sonucu sabit altında olan `firstRow` anahtarı. Sonuç izleyen bir etkinlikte, desenini kullanmayı `@{activity('MyLookupActivity').output.firstRow.TableName}`.

    ```json
    {
        "firstRow":
        {
            "Id": "1",
            "TableName" : "Table1"
        }
    }
    ```

* **Zaman `firstRowOnly` ayarlanır `false`** , aşağıdaki kodda gösterildiği gibi çıktı biçimidir. A `count` alan gösterir kaç kayıtlar döndürülür ve ayrıntılı değerleri görüntülenir sabit altında `value` dizi. Böyle bir durumda, arama etkinliği genellikle tarafından izlenir bir [Foreach etkinlik](control-flow-for-each-activity.md). Geçirebilirsiniz `value` ForEach etkinlik dizisine `items` desenini kullanarak alan `@activity('MyLookupActivity').output.value`. Erişim öğelerine `value` dizisi, aşağıdaki sözdizimini kullanın: `@{activity('lookupActivity').output.value[zero based index].propertyname}`. Örnek aşağıda verilmiştir: `@{activity('lookupActivity').output.value[0].tablename}`.

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
Bu örnekte, kopya etkinliği verileri Azure SQL veritabanı örneği bir SQL tablosuna Azure Blob Depolama birimine kopyalar. SQL tablosu adı Blob storage JSON dosyasında depolanır. Çalışma zamanında tablo adı arama etkinliği arar. Bu yaklaşım JSON ardışık düzen veya veri kümelerini yeniden dağıtmak zorunda kalmadan dinamik olarak değiştirilmesine izin verir. 

Bu örnek yalnızca ilk satır için arama gösterir. Örnekleri tüm satırları ve sonuçları ForEach etkinlikle zincir için arama için bkz: [Azure Data Factory kullanarak birden çok tablo toplu kopyalama](tutorial-bulk-copy.md).

### <a name="pipeline"></a>İşlem hattı
Bu ardışık düzen iki etkinlik içerir: *arama* ve *kopya*. 

- Arama etkinliği, Azure Blob Depolama birimindeki bir konuma başvuruyor LookupDataset kullanacak şekilde yapılandırılır. Arama etkinliği bu konumda bir JSON dosyası SQL tablosu adını okur. 
- Kopyalama etkinliği (SQL tablosu adı) arama etkinlik çıkışı kullanır. Kaynak veri kümesi (SourceDataset) tableName özelliğinde arama etkinlik çıkışı kullanmak için yapılandırılır. Kopyalama etkinliği verileri SQL tablosundan SinkDataset özelliği tarafından belirtilen Azure Blob Depolama birimindeki bir konuma kopyalar. 


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
Arama veri kümesinin başvurduğu *sourcetable.json* AzureStorageLinkedService türü tarafından belirtilen Azure depolama arama klasörünü dosyasında. 

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

### <a name="source-dataset-for-the-copy-activity"></a>Kaynak veri kümesi kopyalama etkinliği için
Kaynak veri kümesi SQL tablosu adı arama etkinlik çıkışını kullanır. Kopyalama etkinliği verileri bu SQL tablosundan havuz veri kümesi tarafından belirtilen Azure Blob Depolama birimindeki bir konuma kopyalar. 

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

### <a name="sink-dataset-for-the-copy-activity"></a>Veri kümesi kopyalama etkinliği için havuz
Veri kopyalama etkinliği SQL tablosuna kopyalar *filebylookup.csv* dosyasını *csv* AzureStorageLinkedService özelliği tarafından belirtilen Azure depolama klasöründe. 

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
Bu depolama hesabı SQL tabloların adlarını JSON dosyasını içerir. 

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
Bu Azure SQL veritabanı örneğinde Blob depolama alanına kopyalanacak veriler içeriyor. 

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

### <a name="sourcetablejson"></a>sourcetable.json

#### <a name="set-of-objects"></a>Nesne kümesini

```json
{
  "Id": "1",
  "tableName": "Table1",
}
{
   "Id": "2",
  "tableName": "Table2",
}
```

#### <a name="array-of-objects"></a>Nesne dizisi

```json
[ 
    {
        "Id": "1",
          "tableName": "Table1",
    }
    {
        "Id": "2",
        "tableName": "Table2",
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar
Data Factory ile desteklenen diğer denetim akışı etkinlikleri bakın: 

- [Ardışık Düzen etkinliği yürütmek](control-flow-execute-pipeline-activity.md)
- [Her etkinlik için](control-flow-for-each-activity.md)
- [Meta veri etkinlik Al](control-flow-get-metadata-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
