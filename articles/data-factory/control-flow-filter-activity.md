---
title: Azure Data Factory etkinliğinde filtre | Microsoft Docs
description: Filtre etkinliği girişleri filtreler.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/04/2018
ms.author: shlo
ms.openlocfilehash: 0ec6aa9d226231802cd753c7216e9988b85ae5bd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34617469"
---
# <a name="filter-activity-in-azure-data-factory"></a>Azure Data Factory içinde filtre etkinliği
Bir filtre ifadesi için Giriş dizisinin uygulamak için ardışık düzeninde bir filtre etkinliği kullanın. 

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [veri fabrikası V1 belgelerine](v1/data-factory-introduction.md).

## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "MyFilterActivity",
    "type": "filter",
    "typeProperties": {
        "condition": "<condition>",
        "items": "<input array>"
    }
}
```

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
ad | Adını `Filter` etkinlik. | Dize | Evet
type | Ayarlanmalıdır **filtre**. | Dize | Evet
koşul | Giriş filtreleme için kullanılacak koşulu. | İfade | Evet
öğeler | Filtre uygulanması gereken giriş dizisi. | İfade | Evet

## <a name="example"></a>Örnek

Bu örnekte, ardışık düzen iki etkinlik vardır: **filtre** ve **ForEach**. Filtre etkinliği 3'ten büyük bir değere sahip öğeleri için Giriş dizisinin filtrelemek için yapılandırılır. ForEach etkinlik filtrelenmiş değer üzerinde yinelenir ve geçerli değeri tarafından belirtilen saniye bekler.

```json
{
    "name": "PipelineName",
    "properties": {
        "activities": [{
                "name": "MyFilterActivity",
                "type": "filter",
                "typeProperties": {
                    "condition": "@greater(item(),3)",
                    "items": "@pipeline().parameters.inputs"
                }
            },
            {
                "name": "MyForEach",
                "type": "ForEach",
                "typeProperties": {
                    "isSequential": "false",
                    "batchCount": 1,
                    "items": "@activity('MyFilterActivity').output.value",
                    "activities": [{
                        "type": "Wait",
                        "typeProperties": {
                            "waitTimeInSeconds": "@item()"
                        },
                        "name": "MyWaitActivity"
                    }]
                },
                "dependsOn": [{
                    "activity": "MyFilterActivity",
                    "dependencyConditions": ["Succeeded"]
                }]
            }
        ],
        "parameters": {
            "inputs": {
                "type": "Array",
                "defaultValue": [1, 2, 3, 4, 5, 6]
            }
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Data Factory ile desteklenen diğer denetim akışı etkinlikleri bakın: 

- [If Koşulu Etkinliği](control-flow-if-condition-activity.md)
- [İşlem Hattı Yürütme Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
- [Bitiş Etkinliği](control-flow-until-activity.md)
