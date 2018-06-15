---
title: Azure Service Fabric CLI - sfctl chaos zamanlaması | Microsoft Docs
description: Service Fabric CLI sfctl chaos zamanlama komutlarını açıklar.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 05/23/2018
ms.author: bikang
ms.openlocfilehash: 0d09338f71d71d07ab0e037d4736cfaa1f3cff85
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34764033"
---
# <a name="sfctl-chaos-schedule"></a>sfctl chaos zamanlama
Alın ve chaos zamanlamasını ayarlayın.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| Al | Ne zaman ve nasıl Chaos çalıştırılacağı tanımlama Chaos zamanlaması alın. |
| ayarlama | Chaos tarafından kullanılacak Chaos zamanlamasını ayarlayın. |

## <a name="sfctl-chaos-schedule-get"></a>sfctl chaos zamanlaması alın
Ne zaman ve nasıl Chaos çalıştırılacağı tanımlama Chaos zamanlaması alın.

Kullanımdaki Chaos zamanlama ve ne zaman ve nasıl Chaos çalıştırılacağını tanımlayan Chaos zamanlama sürümünü alır.

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-chaos-schedule-set"></a>sfctl chaos zamanlama ayarlama
Chaos tarafından kullanılacak Chaos zamanlamasını ayarlayın.

Chaos şu anda kullanımda tarafından Chaos ayarlamaktır. Chaos çalıştırır Chaos zamanlamaya göre otomatik olarak zamanlar. Sağlanan girdi zamanlama sürümünde sunucu Chaos zamanlamada sürümü aynı olmalıdır. Sağlanan sürüm sunucusundaki sürümü eşleşmiyorsa Chaos zamanlama güncelleştirilmez. Sağlanan sürüm sunucusundaki sürümü eşleşirse, Chaos zamanlama güncelleştirilir ve sunucu Chaos zamanlamayla sürümünü ayarlama tek ve sarmalayan için 0 2.147.483.647 sonra artırılır. Bu çağrısı yapıldığında Chaos çalışıyorsa, çağrı başarısız olur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --chaos parametreleri sözlük | JSON dize adları eşlenmesini işleri tarafından kullanılacak ChaosParameters temsil eden listesi kodlanmış. |
| --Bitiş tarihi utc | Tarih ve saat zaman Chaos zamanlamak için zamanlamayı kullanarak durdurmak.  Varsayılan\: 9999 12 31T23\:59\:59.999Z. |
| --işleri | JSON olarak kodlanmış listesi Chaos çalıştırma zamanı temsil eden ChaosScheduleJobs ve Chaos ile çalıştırmak için hangi parametrelere sahip. |
| --Başlangıç tarihi utc | Tarih ve saat zamanlama Chaos zamanlamak için kullanmaya başlamak ne zaman.  Varsayılan\: 1601-01-01T00\:00\:00.000Z. |
| --Sürüm | Zamanlama sürüm numarası. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

### <a name="examples"></a>Örnekler

Aşağıdaki komut 2016-01-01 üzerinde başlayan ve 2038-01-günün, haftanın 7 günü Chaos 24 saatlik çalıştıran 01 son kullanma tarihi (sürüm 0 geçerli zamanlamayı sahip olduğu varsayılır) bir zamanlama ayarlar. Chaos kümede o zaman için zamanlanacak.

    sfctl chaos schedule set --version 0 --start-date-utc "2016-01-01T00:00:00.000Z" --expiry-date-utc "2038-01-01T00:00:00.000Z"
    --chaos-parameters-dictionary 
    [  
    {  
        "Key":"adhoc",
        "Value":{  
            "MaxConcurrentFaults":3,
            "EnableMoveReplicaFaults":true,
            "ChaosTargetFilter":{  
                "NodeTypeInclusionList":[  
                "N0010Ref",
                "N0020Ref",
                "N0030Ref",
                "N0040Ref",
                "N0050Ref"
                ]
            },
            "MaxClusterStabilizationTimeoutInSeconds":60,
            "WaitTimeBetweenIterationsInSeconds":15,
            "WaitTimeBetweenFaultsInSeconds":30,
            "TimeToRunInSeconds":"600",
            "Context":{  
                "Map":{  
                "test":"value"
                }
            },
            "ClusterHealthPolicy":{  
                "MaxPercentUnhealthyNodes":0,
                "ConsiderWarningAsError":true,
                "MaxPercentUnhealthyApplications":0
            }
        }
    }
    ]
    --jobs 
    [  
    {  
        "ChaosParameters":"adhoc",
        "Days":{  
            "Sunday":true,
            "Monday":true,
            "Tuesday":true,
            "Wednesday":true,
            "Thursday":true,
            "Friday":true,
            "Saturday":true
        },
        "Times":[  
            {  
                "StartTime":{  
                "Hour":0,
                "Minute":0
                },
                "EndTime":{  
                "Hour":23,
                "Minute":59
                }
            }
        ]
    }
    ]

## <a name="next-steps"></a>Sonraki adımlar
- [Ayarlanan](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek komutlar](/azure/service-fabric/scripts/sfctl-upgrade-application).