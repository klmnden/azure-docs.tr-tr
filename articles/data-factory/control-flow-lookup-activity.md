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
ms.date: 08/31/2017
ms.author: spelluru
ms.openlocfilehash: 30173f8eea2ccbbcd44018596cf34b3769a64b50
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="lookup-activity-in-azure-data-factory"></a>Azure veri fabrikası'nda arama etkinliği
Arama Etkinliği herhangi bir dış kaynaktan bir record/ table name/ değerini okumak veya aramak için kullanılabilir. Sonraki etkinliklerde bu çıktıya daha fazla başvurulabilir. 

Aşağıdaki veri kaynakları şu anda arama için desteklenir:
- Azure Blob JSON dosyasında
- Şirket içi JSON dosyası
- Azure SQL veritabanı (sorgudan dönüştürülen JSON verilerini)
- Azure Table Storage (sorgudan dönüştürülen JSON verilerini)

Arama etkinliği dinamik olarak dosyaların bir listesini almak istediğinizde faydalıdır / kayıt/tabloları bir yapılandırma dosyası veya bir veri kaynağından. Etkinlik çıkışı, daha fazla öğelerden üzerinde işleme belirli gerçekleştirmek için diğer etkinlikler tarafından kullanılabilir.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [veri fabrikası V1 belgelerine](v1/data-factory-introduction.md).


## <a name="example"></a>Örnek
Bu örnekte, kopya etkinliği verileri Azure SQL veritabanındaki bir SQL tablosundan Azure Blob Depolama birimine kopyalar. SQL tablosu adı Blob Storage JSON dosyasında depolanır. Çalışma zamanında tablo adı arama etkinliği arar. Bu yaklaşım JSON ardışık düzen/veri kümeleri yeniden dağıtmadan dinamik olarak değiştirilmesine izin verir. 

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
                        "sqlReaderQuery": "select * from @{activity('LookupActivity').output.tableName}" 
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
            "tableName": "@{activity('LookupActivity').output.tableName}"
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



## <a name="type-properties"></a>Tür özellikleri
Ad | Açıklama | Tür | Gerekli
---- | ----------- | ---- | --------
Veri kümesi | Veri kümesi özniteliği, arama için veri kümesi başvurusu sağlamaktır. Şu anda desteklenen veri türleri şunlardır:<ul><li>FileShareDataset</li><li>AzureBlobDataset</li><li>AzureSqlTableDataset</li><li>AzureTableDataset</li> | anahtar/değer çifti | Evet
Kaynak | Kopyalama etkinliği kaynağı aynı veri kümesi-özel kaynak özellikleri | Anahtar/değer çifti | Hayır
firstRowOnly | İlk satırı veya tüm satırları döndürür. | Boole değeri | Hayır

## <a name="next-steps"></a>Sonraki adımlar
Data Factory ile desteklenen diğer denetim akışı etkinlikleri bakın: 

- [İşlem Hattı Yürütme Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
