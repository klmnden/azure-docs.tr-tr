---
title: "Azure Data Factory etkinliğinde bekleyin | Microsoft Docs"
description: "Bekle etkinliğinin ardışık yürütmeyi belirtilen süre boyunca duraklatır."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: 460d37b13a17eaf20d77ad4b1059e0461fb0181f
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="wait-activity-in-azure-data-factory"></a>Azure Data Factory etkinliğinde bekleyin
İşlem hattında Bekleme etkinliğini kullandığınızda, işlem hattı izleyen etkinlikleri yürütmeye devam etmeden önce belirtilen süre kadar bekler. 

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [veri fabrikası V1 belgelerine](v1/data-factory-introduction.md).

## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "MyWaitActivity",
    "type": "Wait",
    "typeProperties": {
        "waitTimeInSeconds": 1
    }
}

```

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
ad | Adını `Wait` etkinlik. | Dize | Evet
type | Ayarlanmalıdır **bekleyin**. | Dize | Evet
waitTimeInSeconds | Ardışık Düzen işleme devam etmeden önce bekleyeceği saniye sayısı. | Tamsayı | Evet

## <a name="example"></a>Örnek

> [!NOTE]
> Bu bölüm, JSON tanımları ve ardışık düzen çalıştırmak için örnek PowerShell komutlarını sağlar. Azure PowerShell ve JSON tanımlarını kullanarak Data Factory işlem hattı oluşturmak için adım adım yönergeler içeren bir anlatım için bkz: [Öğreticisi: Azure PowerShell kullanarak bir veri fabrikası oluşturma](quickstart-create-data-factory-powershell.md).

### <a name="pipeline-with-wait-activity"></a>Bekle etkinliğinin ile kanalı
Bu örnekte, ardışık düzen iki etkinlik vardır: **kadar** ve **bekleyin**. Bekle etkinliğinin bir saniye için beklenecek yapılandırılır. Ardışık Düzen Web etkinliğini bir bekleme süresi her çalışması arasındaki saniye ile bir döngüde çalışır. 

```json
{
    "name": "DoUntilPipeline",
    "properties": {
        "activities": [
            {
                "type": "Until",
                "typeProperties": {
                    "expression": {
                        "value": "@equals('Failed', coalesce(body('MyUnauthenticatedActivity')?.status, actions('MyUnauthenticatedActivity')?.status, 'null'))",
                        "type": "Expression"
                    },
                    "timeout": "00:00:01",
                    "activities": [
                        {
                            "name": "MyUnauthenticatedActivity",
                            "type": "WebActivity",
                            "typeProperties": {
                                "method": "get",
                                "url": "https://www.fake.com/",
                                "headers": {
                                    "Content-Type": "application/json"
                                }
                            },
                            "dependsOn": [
                                {
                                    "activity": "MyWaitActivity",
                                    "dependencyConditions": [ "Succeeded" ]
                                }
                            ]
                        },
                        {
                            "type": "Wait",
                            "typeProperties": {
                                "waitTimeInSeconds": 1
                            },
                            "name": "MyWaitActivity"
                        }
                    ]
                },
                "name": "MyUntilActivity"
            }
        ]
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