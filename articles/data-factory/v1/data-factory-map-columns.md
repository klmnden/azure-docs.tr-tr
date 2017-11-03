---
title: "Azure Data Factory veri kümesi sütun eşlemesi | Microsoft Docs"
description: "Kaynak sütunları hedef sütun eşleme öğrenin."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2017
ms.author: jingwang
robots: noindex
ms.openlocfilehash: ae092308c5d2579a5b513657787ae6dbbfadaf05
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="map-source-dataset-columns-to-destination-dataset-columns"></a>Kaynak veri kümesi sütunları hedef veri kümesi sütun eşleme
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. 

Sütun eşlemesi, """tablosunun yapısı Havuz belirtilen" Kaynak Tablo eşleme yapısı sütunlarına içinde belirtilen sütunlar nasıl belirtmek için kullanılabilir. **ColumnMapping** özelliği bulunan **typeProperties** kopyalama etkinliği bölümü.

Sütun eşlemesi aşağıdaki senaryoları destekler:

* Kaynak veri kümesi yapısında tüm sütunları havuz veri kümesi yapısında tüm sütunları eşlenir.
* Kaynak veri kümesi yapısında bir sütun alt kümesini havuz veri kümesi yapısında tüm sütunları eşleştirilir.

Bir özel durum neden hata koşulları şunlardır:

* Daha az sütun veya "eşleme içinde belirtilen"tablosunun yapısı havuz'den daha fazla sütun.
* Yinelenen eşleme.
* SQL sorgu sonucu eşleme içinde belirtilen bir sütun adı yok.

> [!NOTE]
> Aşağıdaki örnekler Azure SQL ve Azure Blob ancak dikdörtgen veri kümeleri destekleyen herhangi bir veri deposuna uygulanabilir. Veri kümesi ve ilgili veri kaynağındaki işaret etmek için örnekleri bağlantılı hizmet tanımlarında ayarlayın.

## <a name="sample-1--column-mapping-from-azure-sql-to-azure-blob"></a>Örnek 1 – sütun Azure SQL Azure blob eşleme
Bu örnekte, Giriş tablosunda bir yapı ve bir Azure SQL veritabanı bir SQL tablosuna işaret.

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

Bu örnek, çıktı tablosu yapısının ve bir Azure blob storage'da bir blob işaret.

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

Aşağıdaki JSON ardışık düzeninde kopyalama etkinliği tanımlar. Kaynak havuzu sütuna eşlenen sütunlarından (**columnMappings**) kullanarak **Çeviricisi** özelliği.

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
**Sütun eşlemesi akışı:**

![Sütun eşlemesi akışı](./media/data-factory-map-columns/column-mapping-flow.png)

## <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-to-azure-blob"></a>Örnek 2 – sütun SQL sorgusuna Azure SQL Azure blob ile eşleme
Bu örnekte, bir SQL sorgusu yalnızca tablo adını ve sütun adları "yapısı" bölümünde belirtmek yerine Azure SQL veri ayıklamak için kullanılır. 

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
Bu durumda, sorgu sonuçları kaynak "yapısı" içinde belirtilen sütun için ilk eşlenir. Ardından, kaynak "yapısı" sütunlarından havuz "yapısı" sütunlara columnMappings içinde belirtilen kuralları ile eşlenir.  Sorgunun döndürdüğü 5 sütun, "" kaynağının yapısı içinde belirtilenlerden iki sütun daha varsayalım.

**Sütun eşlemesi akışı**

![Sütun eşlemesi akış-2](./media/data-factory-map-columns/column-mapping-flow-2.png)

## <a name="next-steps"></a>Sonraki adımlar
Kopyalama etkinliği kullanma hakkında bir öğretici makalesine bakın: 

- [Verileri Blob depolama alanından SQL veritabanına kopyalamak.](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
