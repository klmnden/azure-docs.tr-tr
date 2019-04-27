---
title: Azure Data factory'de doğrulama etkinliği | Microsoft Docs
description: Kullanıcının belirttiği belirli ölçütlerle ekli veri kümesini doğrulayıncaya kadar doğrulama etkinliği işlem hattının yürütme devam etmez.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: shlo
ms.openlocfilehash: 46447bdbea93d1f99c5682cf878c2035e6f49b78
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60764331"
---
# <a name="validation-activity-in-azure-data-factory"></a>Azure Data factory'de doğrulama etkinliği
İşlem hattı ekli doğruladı sonra yürütme yalnızca devam sağlamak için bir işlem hattı, bir doğrulama kullanabilirsiniz veri kümesi başvurusu var, BT'nin belirtilen ölçütleri karşılayan veya zaman aşımı ulaşıldı.


## <a name="syntax"></a>Sözdizimi

```json

{
    "name": "Validation_Activity",
    "type": "Validation",
    "typeProperties": {
        "dataset": {
            "referenceName": "Storage_File",
            "type": "DatasetReference"
        },
        "timeout": "7.00:00:00",
        "sleep": 10,
        "minimumSize": 20
    }
},
{
    "name": "Validation_Activity_Folder",
    "type": "Validation",
    "typeProperties": {
        "dataset": {
            "referenceName": "Storage_Folder",
            "type": "DatasetReference"
        },
        "timeout": "7.00:00:00",
        "sleep": 10,
        "childItems": true
    }
}

```


## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
ad | 'Doğrulama' etkinliğinin adı | String | Evet |
type | Ayarlanmalıdır **doğrulama**. | String | Evet |
Veri kümesi | Etkinlik, bu veri kümesi başvurusu doğruladı kadar blok yürütme var olduğundan ve belirtilen ölçütleri karşılayan veya zaman aşımına ulaşıldığı olur. Sağlanan Dataset "MinimumSize" veya "ChildItems" özelliğini desteklemesi gerekir. | Veri kümesi başvurusu | Evet |
timeout | Çalıştırılacak etkinliğinin zaman aşımını belirtir. Hiçbir değer belirtilmemişse, varsayılan değer 7 günde ("7.00:00:00") ' dir. D.hh:mm:ss biçimidir | String | Hayır |
Uyku | Doğrulama denemeleri arasındaki saniye cinsinden gecikme. Hiçbir değer belirtilmemişse, varsayılan değer 10 saniyedir. | Tamsayı | Hayır |
childItems | Klasörün alt öğelerini sahip olup olmadığını denetler. İçin true değerini ayarlayabilirsiniz: Klasörün var olduğundan ve öğeleri sahip olduğunu doğrulayın. En az bir öğe klasörde mevcut veya zaman aşımı değeri ulaşılana kadar engeller.-false: Klasörün var olduğundan ve boş olduğunu doğrulayın. Klasör boş veya zaman aşımı süresi kadar blokları değeri ulaşıldı. Hiçbir değer belirtilmemişse, etkinlik kadar klasör yok veya zaman aşımı ulaşılana kadar engeller. | Boolean | Hayır |
minimumSize | Dosyasının bayt cinsinden en küçük boyutu. Hiçbir değer belirtilmemişse, varsayılan değer 0 bayt'tır | Tamsayı | Hayır |


## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen diğer denetim akışı etkinlikleri bakın:

- [If Koşulu Etkinliği](control-flow-if-condition-activity.md)
- [İşlem Hattı Yürütme Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
- [Bitiş Etkinliği](control-flow-until-activity.md)
