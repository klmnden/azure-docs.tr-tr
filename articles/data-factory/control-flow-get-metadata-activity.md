---
title: Meta veri etkinliği Azure Data Factory'de alma | Microsoft Docs
description: SQL Server saklı yordam etkinliği bir saklı yordam bir Azure SQL Database veya Azure SQL veri ambarı Data Factory işlem hattı çağırmak için nasıl kullanabileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/10/2018
ms.author: shlo
ms.openlocfilehash: c07199887faf073d19007f1ef410c193bbdbf3ee
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37049375"
---
# <a name="get-metadata-activity-in-azure-data-factory"></a>Meta veri etkinliği Azure Data Factory'de Al
GetMetadata etkinlik almak için kullanılabilir **meta veri** tüm verilerin Azure veri fabrikası'nda. Bu etkinlik aşağıdaki senaryolarda kullanılabilir:

- Herhangi bir veri meta veri bilgilerini doğrula
- Veriler hazır / kullanılabilir olduğunda bir ardışık düzen Tetikle

Aşağıdaki işlevselliği denetim akışında kullanılabilir:

- GetMetadata etkinlik çıkışı koşullu ifadelerle doğrulama gerçekleştirmek için kullanılabilir.
- Koşul yapmak yerine getirdiğinizde bir ardışık düzen tetiklenebilir-döngü kadar

## <a name="supported-capabilities"></a>Desteklenen özellikler

GetMetadata etkinlik gerekli giriş olarak bir veri kümesini alır ve etkinlik çıkış olarak kullanılabilir meta veri bilgilerini çıkarır. Şu anda aşağıdaki bağlayıcılar karşılık gelen alınabilir meta veriler ile desteklenir:

>[!NOTE]
>GetMetadata etkinlik üzerinde Self-hosted tümleştirmesi Çalışma Zamanı Modülü çalıştırırsanız, en son yetenek sürüm 3.6 veya üzeri desteklenir. 

### <a name="supported-connectors"></a>Desteklenen bağlayıcılar

**Dosya depolama:**

| Connector/meta veri | ItemName<br>(dosya/klasör) | itemType<br>(dosya/klasör) | boyut<br>(dosya) | oluşturuldu<br>(dosya/klasör) | lastModified<br>(dosya/klasör) |childItems<br>(klasör) |contentMD5<br>(dosya) | yapısı<br/>(dosya) | columnCount<br>(dosya) | var<br>(dosya/klasör) |
|:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |
| Amazon S3 | √/√ | √/√ | √ | x/x | √ / √ * | √ | x | √ | √ | √ / √ * |
| Azure Blob | √/√ | √/√ | √ | x/x | √ / √ * | √ | √ | √ | √ | √/√ |
| Azure Data Lake Store | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| Azure Dosya Depolama | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| Dosya Sistemi | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| SFTP | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| FTP | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |

- Amazon S3 için `lastModified` demet ve anahtar ancak sanal klasör; için geçerlidir; ve `exists` öneki veya sanal klasör değil, sepet ve anahtarı için geçerlidir.
- Azure Blob için `lastModified` kapsayıcı ve blob ancak sanal klasör için uygulanır.

**İlişkisel veritabanı:**

| Connector/meta veri | yapısı | columnCount | var |
|:--- |:--- |:--- |:--- |
| Azure SQL Database | √ | √ | √ |
| Azure SQL Veri Ambarı | √ | √ | √ |
| SQL Server | √ | √ | √ |

### <a name="metadata-options"></a>Meta veri seçenekleri

Aşağıdaki meta veri türleri almak için GetMetadata etkinlik alan listesindeki belirtilebilir:

| Meta veri türü | Açıklama |
|:--- |:--- |
| ItemName | Dosya veya klasörün adı. |
| itemType | Dosya veya klasör türü. Çıktı değeri `File` veya `Folder`. |
| boyut | Dosyanın bayt olarak boyutu. Yalnızca dosya için geçerli. |
| oluşturuldu | Oluşturulan datetime dosya veya klasör. |
| lastModified | Dosya veya klasörün son değiştirilen datetime. |
| childItems | Alt klasörleri ve dosyaları belirtilen klasörün içindeki listesi. Yalnızca klasör için geçerli. Çıkış değeri, adını ve her bir alt öğe türünü listesidir. |
| contentMD5 | MD5 dosyasının. Yalnızca dosya için geçerli. |
| yapısı | Dosya veya ilişkisel veritabanı tablosu içindeki veri yapısı. Çıkış değeri, sütun adını ve sütun türü listesidir. |
| columnCount | Dosya veya ilişkisel tablo içindeki sütun sayısı. |
| var| Bir dosya/klasör/tablosu veya varolup. "Var" GetaMetadata alan listesindeki belirtilirse, bile değil (dosya/klasör/tablosu) öğe varsa, etkinlik başarısız olmaz unutmayın; Bunun yerine, döndürür `exists: false` çıkışı. |

>[!TIP]
>Bir dosya/klasör/tablosu veya varsa doğrulamak istediğiniz zaman belirtmek `exists` GetMetadata etkinlik alan listesindeki sonra göz atabilirsiniz `exists: true/false` neden etkinlik çıkışı. Varsa `exists` nesne bulunamadığında etkinlik başarısız olur GetMetadata alan listesindeki yapılandırılmamış.

## <a name="syntax"></a>Sözdizimi

**GetMetadata etkinlik:**

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

Şu anda GetMetadata etkinlik meta veri bilgileri aşağıdaki türlerini getirebilir.

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
alan listesi | Gerekli meta veri bilgileri türlerini listeler. Ayrıntıları görmek [meta veri seçenekleri](#metadata-options) desteklenen meta veri bölümü. | Evet 
Veri kümesi | Başvuru dataset, meta veri GetMetadata etkinlik tarafından alınmasına izin etkinliktir. Bkz: [desteklenen yetenekler](#supported-capabilities) bölümünde üzerinde desteklenen bağlayıcılar ve veri kümesi sözdizimi ayrıntıları bağlayıcı konusuna bakın. | Evet

## <a name="sample-output"></a>Örnek çıktı

GetMetadata sonuç etkinlik çıkışı gösterilir. Alan listesinden başvuru olarak seçilen kapsamlı meta verileri seçeneklerle iki örnek aşağıda verilmiştir. Sonuç izleyen bir etkinlikte, desenini kullanmayı `@{activity('MyGetMetadataActivity').output.itemName}`.

### <a name="get-a-files-metadata"></a>Bir dosyanın meta verileri alma

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

### <a name="get-a-folders-metadata"></a>Klasörün meta verileri alma

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
Data Factory ile desteklenen diğer denetim akışı etkinlikleri bakın: 

- [İşlem Hattı Yürütme Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her etkinlik için](control-flow-for-each-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
