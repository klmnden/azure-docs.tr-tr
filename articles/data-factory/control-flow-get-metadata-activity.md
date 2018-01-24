---
title: "Meta veri etkinliği Azure Data Factory'de alma | Microsoft Docs"
description: "SQL Server saklı yordam etkinliği bir saklı yordam bir Azure SQL Database veya Azure SQL veri ambarı Data Factory işlem hattı çağırmak için nasıl kullanabileceğinizi öğrenin."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: 20f3d4bb876a46b67385dd4435296e149641149e
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="get-metadata-activity-in-azure-data-factory"></a>Meta veri etkinliği Azure Data Factory'de Al
GetMetadata etkinliği, Azure Data Factory içindeki herhangi bir verinin meta verilerini almak için kullanılabilir. Bu etkinlik yalnızca sürüm 2, veri oluşturucuları için desteklenir. Aşağıdaki senaryolarda kullanılabilir:

- Herhangi bir veri meta veri bilgilerini doğrula
- Veriler hazır / kullanılabilir olduğunda bir ardışık düzen Tetikle

Aşağıdaki işlevselliği denetim akışında kullanılabilir:
- GetMetadata etkinlik çıkışı koşullu ifadelerle doğrulama gerçekleştirmek için kullanılabilir.
- Koşul yapmak yerine getirdiğinizde bir ardışık düzen tetiklenebilir-döngü kadar

GetMetadata etkinlik alır gereken giriş ve çıkış meta veri bilgileri olarak kullanılabilir olarak dataset bir çıktı. Şu anda yalnızca Azure blob dataset desteklenir. Desteklenen meta veri boyutu, yapı ve lastModified zaman alanlardır.  

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [veri fabrikası V1 belgelerine](v1/data-factory-introduction.md).


## <a name="syntax"></a>Sözdizimi

### <a name="get-metadata-activity-definition"></a>Meta veri etkinlik tanımı alın:
Aşağıdaki örnekte, GetMetadata etkinlik MyDataset tarafından temsil edilen veri hakkındaki meta verileri döndürür. 

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
### <a name="dataset-definition"></a>Veri kümesi tanımı:

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
            "Filename": "file.json",
            "format":{
                "type":"JsonFormat"
                "nestedSeperator": ","
            }
        }
    }
}
```

### <a name="output"></a>Çıktı
```json
{
    "size": 1024,
    "structure": [
        {
            "name": "id",
            "type": "Int64"
        }, 
    ],
    "lastModified": "2016-07-12T00:00:00Z"
}
```

## <a name="type-properties"></a>Tür özellikleri
Şu anda GetMetadata etkinlik meta veri bilgileri aşağıdaki türleri bir Azure depolama kümesinden getirebilir.

Özellik | Açıklama | İzin Verilen Değerler | Gerekli
-------- | ----------- | -------------- | --------
alan listesi | Gerekli meta veri bilgileri türlerini listeler.  | <ul><li>boyut</li><li>yapısı</li><li>lastModified</li></ul> |    Hayır<br/>Boşsa, etkinlik tüm 3 desteklenen meta veri bilgileri döndürür. 
Veri kümesi | Başvuru dataset, meta veri GetMetadata etkinlik tarafından alınmasına izin etkinliktir. <br/><br/>Şu anda desteklenen veri kümesi, Azure Blob türüdür. İki alt özellikleri şunlardır: <ul><li><b>başvuruadı</b>: var olan bir Azure Blob Dataset başvurusu</li><li><b>tür</b>: dataset başvuru yapıldığı türü "DatasetReference" değil</li></ul> |    <ul><li>Dize</li><li>DatasetReference</li></ul> | Evet

## <a name="next-steps"></a>Sonraki adımlar
Data Factory ile desteklenen diğer denetim akışı etkinlikleri bakın: 

- [İşlem Hattı Yürütme Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her etkinlik için](control-flow-for-each-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web etkinliği](control-flow-web-activity.md)