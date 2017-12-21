---
title: "Azure veri fabrikası'nda arama etkinliği | Microsoft Docs"
description: "Bir dış kaynaktan bir değeri aramak için arama etkinliği kullanmayı öğrenin. Sonraki etkinliklerde bu çıktıya daha fazla başvurulabilir."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: shlo
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: spelluru
ms.openlocfilehash: f287b0287ad85ffe1654e0d574cd44aa4dd81a0f
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="lookup-activity-in-azure-data-factory"></a>Azure veri fabrikası'nda arama etkinliği
Arama Etkinliği herhangi bir dış kaynaktan bir record/ table name/ değerini okumak veya aramak için kullanılabilir. Sonraki etkinliklerde bu çıktıya daha fazla başvurulabilir. 

Arama etkinliği dinamik olarak dosyaların bir listesini almak istediğinizde faydalıdır / kayıt/tabloları bir yapılandırma dosyası veya bir veri kaynağından. Etkinlik çıkışı, daha fazla öğelerden üzerinde işleme belirli gerçekleştirmek için diğer etkinlikler tarafından kullanılabilir.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [veri fabrikası V1 belgelerine](v1/data-factory-introduction.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Aşağıdaki veri kaynakları şu anda arama için desteklenir:
- Azure Blob JSON dosyasında
- JSON dosyasının dosya sistemi
- Azure SQL veritabanı (sorgudan dönüştürülen JSON verilerini)
- Azure SQL veri ambarı (sorgudan dönüştürülen JSON verilerini)
- SQL Server (sorgudan dönüştürülen JSON verilerini)
- Azure Table Storage (sorgudan dönüştürülen JSON verilerini)

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
Ad | Açıklama | Tür | Gerekli
---- | ----------- | ---- | --------
Veri kümesi | Veri kümesi özniteliği, arama için veri kümesi başvurusu sağlamaktır. Şu anda desteklenen veri türleri şunlardır:<ul><li>`AzureBlobDataset`için [Azure Blob Storage](connector-azure-blob-storage.md#dataset-properties) kaynağı olarak</li><li>`FileShareDataset`için [dosya sistemi](connector-file-system.md#dataset-properties) kaynağı olarak</li><li>`AzureSqlTableDataset`için [Azure SQL veritabanı](connector-azure-sql-database.md#dataset-properties) veya [Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md#dataset-properties) kaynağı olarak</li><li>`SqlServerTable`için [SQL Server](connector-sql-server.md#dataset-properties) kaynağı olarak</li><li>`AzureTableDataset`için [Azure Table Storage](connector-azure-table-storage.md#dataset-properties) kaynağı olarak</li> | Anahtar/değer çifti | Evet
kaynak | Veri kümesi-özel kaynak özellikleri, kopyalama etkinliği kaynakla aynı. Karşılık gelen her bağlayıcı makale içindeki "etkinlik özellikleri Kopyala" bölümünden daha ayrıntılı bilgi. | Anahtar/değer çifti | Evet
firstRowOnly | Yalnızca ilk satırı veya tüm satırları döndürülmeyeceğini gösterir. | Boole değeri | Hayır. Varsayılan değer `ture`.

## <a name="use-lookup-activity-result-in-subsequent-activity"></a>Arama etkinlik sonuç izleyen bir etkinlikte kullanma

Arama sonuç döndürülür `output` etkinliğin sonucu çalışma bölümünde.

**Zaman `firstRowOnly` ayarlanır `true` (varsayılan)**, çıktı biçimi aşağıdaki gibidir. Arama sonucu sabit altında olan `firstRow` anahtarı. Sonuç izleyen bir etkinlikte, desenini kullanmayı `@{activity('MyLookupActivity').output.firstRow.TableName}`.

```json
{
    "firstRow":
    {
        "Id": "1",
        "TableName" : "Table1"
    }
}
```

**Zaman `firstRowOnly` ayarlanır `false`** , çıktı biçimi aşağıdaki gibidir. A `count` alan gösterir kaç kayıtlar döndürülür ve ayrıntılı değerlerdir sabit altında `value` dizi. Böyle bir durumda arama etkinliği genellikle tarafından izlenir bir [Foreach etkinlik](control-flow-for-each-activity.md), geçirebilirsiniz `value` ForEach etkinlik dizisine `items` desenini kullanarak alan `@activity('MyLookupActivity').output.value`. Erişim öğelerine `value`, aşağıdaki sözdizimini kullanın: `@{activity('lookupActivity').output.value[zero based index].propertyname}`. Örnek aşağıda verilmiştir:`@{activity('lookupActivity').output.value[0].tablename}`

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
Bu örnekte, kopya etkinliği verileri Azure SQL veritabanındaki bir SQL tablosundan Azure Blob Depolama birimine kopyalar. SQL tablosu adı Blob Storage JSON dosyasında depolanır. Çalışma zamanında tablo adı arama etkinliği arar. Bu yaklaşım JSON ardışık düzen/veri kümeleri yeniden dağıtmadan dinamik olarak değiştirilmesine izin verir. 

Bu örnek yalnızca ilk satır Ara gösterir. Tüm satırlar ve zinciri ForEach etkinliği ile arayın başvurmak [Öğreticisi - toplu kopyalama veri](tutorial-bulk-copy.md) örnek.

### <a name="pipeline"></a>İşlem hattı
Bu ardışık düzen iki etkinlik içerir: **aramak** ve **kopya**. 

- Arama etkinliği bir Azure Blob depolama konumuna başvuruyor LookupDataset kullanacak şekilde yapılandırılır. Arama etkinliği bu konumda bir JSON dosyası SQL tablosu adını okur. 
- Kopyalama etkinliği (SQL tablosu adı) arama etkinlik çıkışı kullanır. TableName kaynak kümesindeki (SourceDataset) arama etkinlik çıkışı kullanacak şekilde yapılandırılır. Kopyalama etkinliği verileri SQL tablosundan SinkDataset tarafından belirtilen Azure Blob depolama konumuna kopyalar. 


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
Arama dataset AzureStorageLinkedService tarafından belirtilen Azure Storage Arama klasöründe sourcetable.json dosyasında ifade eder. 

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
Kaynak veri kümesi SQL tablosu adı arama etkinlik çıkışını kullanır. Kopyalama etkinliği verileri bu SQL tablosundan Azure Blob Depolama havuzu veri kümesi tarafından belirtilen bir konuma kopyalar. 

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
Kopyalama etkinliği verileri SQL tablosundan AzureStorageLinkedService tarafından belirtilen Azure depolama alanında csv klasöründeki filebylookup.csv dosyasını kopyalar. 

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

### <a name="azure-storage-linked-service"></a>Azure Storage Bağlı Hizmeti
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
Bu Azure SQL veritabanı blob depolama alanına kopyalanacak verileri içerir. 

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

- [İşlem Hattı Yürütme Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
