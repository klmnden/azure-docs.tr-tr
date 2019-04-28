---
title: Azure Data Factory'de etkinlik meta veri alma | Microsoft Docs
description: GetMetadata etkinliği bir Data Factory işlem hattında nasıl kullanabileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: ''
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: jingwang
ms.openlocfilehash: 78f63b4f46fe5479d4d0fd5849ad80536d8a137c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61346927"
---
# <a name="get-metadata-activity-in-azure-data-factory"></a>Azure Data Factory'de etkinlik meta verilerini al

GetMetadata etkinliği, almak için kullanılabilir **meta verileri** Azure Data factory'deki herhangi bir veri. Bu etkinlik, aşağıdaki senaryolarda kullanılabilir:

- Herhangi bir verinin meta veri bilgilerini doğrula
- Veriler hazır / kullanılabilir olduğunda, bir işlem hattını tetikleme

Aşağıdaki işlevleri denetim akışında kullanılabilir:

- GetMetadata etkinliğin çıkışı doğrulama gerçekleştirmek için koşullu ifadelerde kullanılabilir.
- Koşul karşılanmazsa bir işlem hattı tetiklenebilir-kadar döngü

## <a name="supported-capabilities"></a>Desteklenen özellikler

GetMetadata etkinliği, bir veri kümesi gerekli bir giriş olarak alır ve etkinlik çıkışı kullanılabilir meta veri bilgilerini çıkarır. Şu anda aşağıdaki bağlayıcılar karşılık gelen alınabilir meta verileri ile desteklenir ve desteklenen en fazla meta veri boyutu en fazla olan **1MB**.

>[!NOTE]
>GetMetadata etkinliği şirket içinde barındırılan tümleştirme çalışma zamanı üzerinde çalıştırıyorsanız, en son özellik sürümü 3.6 veya sonraki sürümlere desteklenir. 

### <a name="supported-connectors"></a>Desteklenen bağlayıcılar

**Dosya depolama:**

| Bağlayıcı/meta verileri | ItemName<br>(dosya/klasör) | Itemtype<br>(dosya/klasör) | boyut<br>(dosya) | oluşturuldu<br>(dosya/klasör) | Son değiştirme<br>(dosya/klasör) |childItems<br>(klasör) |contentMD5<br>(dosya) | yapısı<br/>(dosya) | columnCount<br>(dosya) | Var.<br>(dosya/klasör) |
|:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |
| Amazon S3 | √/√ | √/√ | √ | x/x | √/√* | √ | x | √ | √ | √/√* |
| Google Cloud Storage | √/√ | √/√ | √ | x/x | √/√* | √ | x | √ | √ | √/√* |
| Azure Blob | √/√ | √/√ | √ | x/x | √/√* | √ | √ | √ | √ | √/√ |
| Azure Data Lake Storage Gen1 | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| Azure Data Lake Storage Gen2 | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| Azure Dosya Depolama | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| Dosya Sistemi | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| SFTP | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| FTP | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |

- Amazon S3 ve Google Sloud depolama `lastModified` demet ve anahtar ancak sanal klasör; için geçerlidir; ve `exists` ön eki veya sanal klasör değil, demet ve anahtarı için geçerlidir.
- Azure Blob için `lastModified` kapsayıcı ve blob ancak sanal klasör için geçerlidir.

**İlişkisel veritabanı:**

| Bağlayıcı/meta verileri | yapısı | columnCount | Var. |
|:--- |:--- |:--- |:--- |
| Azure SQL Veritabanı | √ | √ | √ |
| Azure SQL Veritabanı Yönetilen Örneği | √ | √ | √ |
| Azure SQL Veri Ambarı | √ | √ | √ |
| SQL Server | √ | √ | √ |

### <a name="metadata-options"></a>Meta veri seçenekleri

Alınacak GetMetadata etkinliği alan listesinde, aşağıdaki meta veri türleri belirtilebilir:

| Meta veri türü | Açıklama |
|:--- |:--- |
| ItemName | Dosya veya klasörün adı. |
| Itemtype | Dosya veya klasörün türü. Çıkış değeri `File` veya `Folder`. |
| boyut | Dosyanın bayt cinsinden boyutu. Yalnızca dosya için geçerli. |
| oluşturuldu | Dosya veya klasörün oluşturulan datetime. |
| Son değiştirme | Son değiştirme: dosyanın veya klasörün datetime. |
| childItems | Belirli klasörün içindeki dosyaları ve alt klasörlerin listesi. Yalnızca klasör için geçerli. Çıkış değeri, adı ve her alt öğenin türü listesidir. |
| contentMD5 | Dosya MD5. Yalnızca dosya için geçerli. |
| yapısı | İlişkisel veritabanı tablo ve dosya içinde veri yapısı. Çıkış değeri sütun adı ve sütun türü listesidir. |
| columnCount | Dosya veya ilişkisel tablo içindeki sütun sayısı. |
| Var.| Olup olmadığını veya bir dosya/klasör/tablosu bulunmaktadır. "Var" GetaMetadata alan listesinde belirtilirse, ' % s'öğesi (dosya/klasör/tablosu) bile yok, etkinlik başarısız olmaz dikkat edin. Bunun yerine döndürür `exists: false` çıktı. |

>[!TIP]
>Bir dosya/klasör/table veya varsa doğrulamak istediğinizde, belirtin `exists` GetMetadata etkinliği alan listesinde, daha sonra kontrol edebilirsiniz `exists: true/false` neden etkinlik çıkışı. Varsa `exists` alan listesinde, GetMetadata etkinliği başarısız olur nesne bulunamadığında yapılandırılmadı.

## <a name="syntax"></a>Sözdizimi

**GetMetadata etkinliği:**

```json
{
    "name": "MyActivity",
    "type": "GetMetadata",
    "typeProperties": {
        "fieldList" : ["size", "lastModified", "structure"],
        "dataset": {
            "referenceName": "MyDataset",
            "type": "DatasetReference"
        }
    }
}
```

**Veri kümesi:**

```json
{
    "name": "MyDataset",
    "properties": {
    "type": "AzureBlob",
        "linkedService": {
            "referenceName": "StorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath":"container/folder",
            "filename": "file.json",
            "format":{
                "type":"JsonFormat"
            }
        }
    }
}
```

## <a name="type-properties"></a>Tür özellikleri

Şu anda GetMetadata etkinliği, meta veri bilgilerini aşağıdaki türleri getirebilir.

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
alan listesi | Gerekli meta veri bilgilerini türlerini listeler. Ayrıntılara bakın [meta veri seçenekleri](#metadata-options) desteklenen meta veriler bölümü. | Evet 
Veri kümesi | GetMetadata etkinliği tarafından alınmasına izin, meta verileri etkinliği olan başvuru veri kümesi. Bkz: [desteklenen yetenekler](#supported-capabilities) bölüm üzerinde desteklenen bağlayıcılar ve veri kümesi söz dizimi ayrıntıları bağlayıcı konusuna bakın. | Evet

## <a name="sample-output"></a>Örnek çıktı

GetMetadata sonucu etkinlik çıktıda gösterilir. Başvuru olarak alan listesinde seçilen kapsamlı meta veri seçenekleri ile iki örnek aşağıda verilmiştir. Sonuç, sonraki etkinliği kullanmak için desenini kullanın `@{activity('MyGetMetadataActivity').output.itemName}`.

### <a name="get-a-files-metadata"></a>Bir dosyanın meta verilerini al

```json
{
  "exists": true,
  "itemName": "test.csv",
  "itemType": "File",
  "size": 104857600,
  "lastModified": "2017-02-23T06:17:09Z",
  "created": "2017-02-23T06:17:09Z",
  "contentMD5": "cMauY+Kz5zDm3eWa9VpoyQ==",
  "structure": [
    {
        "name": "id",
        "type": "Int64"
    },
    {
        "name": "name",
        "type": "String"
    }
  ],
  "columnCount": 2
}
```

### <a name="get-a-folders-metadata"></a>Klasör meta verilerini al

```json
{
  "exists": true,
  "itemName": "testFolder",
  "itemType": "Folder",
  "lastModified": "2017-02-23T06:17:09Z",
  "created": "2017-02-23T06:17:09Z",
  "childItems": [
    {
      "name": "test.avro",
      "type": "File"
    },
    {
      "name": "folder hello",
      "type": "Folder"
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen diğer denetim akışı etkinlikleri bakın: 

- [İşlem Hattı Yürütme Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her etkinlik için](control-flow-for-each-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
