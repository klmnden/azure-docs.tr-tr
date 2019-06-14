---
title: Azure Data factory'de veri kümesi sütunlarını eşleme | Microsoft Docs
description: Kaynak sütunlar hedef sütunlara eşlemek öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 1b009ac2ca42e9804b88989b55b2e73524732550
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60238130"
---
# <a name="map-source-dataset-columns-to-destination-dataset-columns"></a>Hedef dataset sütunları için kaynak veri kümesi sütunlarını eşleme
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. 

Sütun eşlemesi, """tablosunun yapısı Havuz belirtilen" Kaynak Tablo eşleme yapısı sütunlarına içinde belirtilen sütunları nasıl belirtmek için kullanılabilir. **Columnmapping'in** özelliği kullanılabilir **typeProperties** bölümünü kopyalama etkinliği.

Sütun eşlemesi, aşağıdaki senaryoları destekler:

* Kaynak veri kümesi yapısında tüm sütunları havuz veri kümesi yapısında tüm sütunları eşlenir.
* Kaynak veri kümesi yapısında sütunların bir alt kümesini tüm sütunları havuz veri kümesi yapısında eşleştirilir.

Bir özel durumu hata koşulları şunlardır:

* Daha az sütun veya "eşlemesinde belirtilen"tablosunun yapısı havuz daha fazla sütun.
* Yinelenen eşleme.
* SQL sorgu sonucu eşlemesinde belirtilen bir sütun adı yok.

> [!NOTE]
> Aşağıdaki örnekler, Azure SQL ve Azure Blob içindir ancak dikdörtgen veri kümeleri destekleyen herhangi bir veri deposuna geçerlidir. Veri kümesi ve bağlı hizmet tanımları örneklerde ilgili veri kaynağındaki işaret edecek şekilde ayarlayın.

## <a name="sample-1--column-mapping-from-azure-sql-to-azure-blob"></a>Örnek 1 – Azure SQL Azure blobuna eşleme sütun
Bu örnekte, Giriş tablosunda bir yapıya sahiptir ve bir Azure SQL veritabanında bir SQL tablosunu işaret eder.

```json
{
    "name": "AzureSQLInput",
    "properties": {
        "structure": 
         [
           { "name": "userid"},
           { "name": "name"},
           { "name": "group"}
         ],
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Bu örnekte, çıktı tablosu bir yapıya sahiptir ve Azure blob depolama alanındaki bir bloba işaret.

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
         "structure": 
          [
                { "name": "myuserid"},
                { "name": "myname" },
                { "name": "mygroup"}
          ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName":"myfile.csv",
            "format":
            {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Aşağıdaki JSON bir işlem hattında kopyalama etkinliği tanımlar. Kaynak havuzu sütuna eşlenmiş sütunlarından (**Bunun amacı**) kullanarak **Translator** özelliği.

```json
{
    "name": "CopyActivity",
    "description": "description", 
    "type": "Copy",
    "inputs":  [ { "name": "AzureSQLInput"  } ],
    "outputs":  [ { "name": "AzureBlobOutput" } ],
    "typeProperties":    {
        "source":
        {
            "type": "SqlSource"
        },
        "sink":
        {
            "type": "BlobSink"
        },
        "translator": 
        {
            "type": "TabularTranslator",
            "ColumnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"
        }
    },
   "scheduler": {
          "frequency": "Hour",
          "interval": 1
        }
}
```
**Sütun eşleme akış:**

![Sütun eşleme akış](./media/data-factory-map-columns/column-mapping-flow.png)

## <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-to-azure-blob"></a>Örnek 2: sütun Azure blob'a SQL sorgusunu Azure SQL ile eşleme
Bu örnekte, bir SQL sorgusu yalnızca tablo adını ve sütun adları "yapı" bölümünde belirtmek yerine Azure SQL veri ayıklamak için kullanılır. 

```json
{
    "name": "CopyActivity",
    "description": "description", 
    "type": "CopyActivity",
    "inputs":  [ { "name": " AzureSQLInput"  } ],
    "outputs":  [ { "name": " AzureBlobOutput" } ],
    "typeProperties":
    {
        "source":
        {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartDateTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
        },
        "sink":
        {
            "type": "BlobSink"
        },
        "Translator": 
        {
            "type": "TabularTranslator",
            "ColumnMappings": "UserId: MyUserId, Group: MyGroup,Name: MyName"
        }
    },
    "scheduler": {
          "frequency": "Hour",
          "interval": 1
        }
}
```
Bu durumda, sorgu sonuçları kaynak "yapısı" içinde belirtilen sütun için ilk eşlenir. Ardından, "yapı" kaynağından sütunları havuz "yapı" sütunlar Bunun amacı belirtilen kuralları ile eşlenir.  Sorgunun döndürdüğü 5 sütun, "" kaynağının yapısı içinde belirtilenlerden iki sütun daha varsayalım.

**Sütun eşleme akış**

![Akış-2 sütun eşleme](./media/data-factory-map-columns/column-mapping-flow-2.png)

## <a name="next-steps"></a>Sonraki adımlar
Kopyalama etkinliği'ni kullanma hakkında bir öğretici için bkz: 

- [Verileri Blob depolama alanından SQL veritabanına kopyalama](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
