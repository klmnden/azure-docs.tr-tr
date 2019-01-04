---
title: Azure Data factory'de etkinlik bekleyin | Microsoft Docs
description: Bekle etkinliğinin işlem hattının yürütülmesini belirtilen süre boyunca duraklatır.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: 731df55a11f4671670a65dac8a83927d81da454c
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54015806"
---
# <a name="wait-activity-in-azure-data-factory"></a>Azure Data factory'de etkinlik bekleyin
İşlem hattında Bekleme etkinliğini kullandığınızda, işlem hattı izleyen etkinlikleri yürütmeye devam etmeden önce belirtilen süre kadar bekler. 

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

Özellik | Açıklama | İzin verilen değerler | Gereklidir
-------- | ----------- | -------------- | --------
ad | Adını `Wait` etkinlik. | Dize | Evet
type | Ayarlanmalıdır **bekleyin**. | Dize | Evet
waitTimeInSeconds | Ardışık Düzen işleme devam etmeden önce bekleyeceği saniye sayısı. | Tamsayı | Evet

## <a name="example"></a>Örnek

> [!NOTE]
> Bu bölümde, JSON tanımları ve işlem hattını çalıştırmak için örnek PowerShell komutları sağlanır. Azure PowerShell ve JSON tanımları'ı kullanarak Data Factory işlem hattı oluşturmak için adım adım yönergeler içeren bir kılavuz için bkz. [öğretici: Azure PowerShell kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-powershell.md).

### <a name="pipeline-with-wait-activity"></a>Bekleme etkinliği içeren işlem hattı
Bu örnekte, işlem hattı iki etkinlik içerir: **Kadar** ve **bekleyin**. Bekle etkinliğinin bir saniye için beklenecek yapılandırılır. İşlem hattı Web etkinliği ile bir bekleme süresi her çalıştırma arasındaki saniye döngüsel olarak çalıştırır. 

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
Data Factory tarafından desteklenen diğer denetim akışı etkinlikleri bakın: 

- [If Koşulu Etkinliği](control-flow-if-condition-activity.md)
- [İşlem Hattı Yürütme Etkinliği](control-flow-execute-pipeline-activity.md)
- [Her etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
- [Bitiş Etkinliği](control-flow-until-activity.md)
