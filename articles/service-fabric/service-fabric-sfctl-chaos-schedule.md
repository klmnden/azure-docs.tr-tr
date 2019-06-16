---
title: Azure Service Fabric CLI - sfctl chaos schedule | Microsoft Docs
description: Service Fabric CLI'sını sfctl chaos zamanlama komutlarını açıklamaktadır.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: dc3dd06b5feac1f66598cd65fa79f447a1bbd9be
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60837513"
---
# <a name="sfctl-chaos-schedule"></a>sfctl chaos schedule
Alın ve kaos zamanlamasını ayarlayın.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| Al | Ne zaman ve nasıl çalıştırılacağını Chaos tanımlama Chaos zamanlama alın. |
| set | Kaos tarafından kullanılan zamanlamasını ayarlayın. |

## <a name="sfctl-chaos-schedule-get"></a>sfctl chaos zamanlamasını Al
Ne zaman ve nasıl çalıştırılacağını Chaos tanımlama Chaos zamanlama alın.

Kaos zamanlama kullanımda ve ne zaman ve nasıl Chaos çalıştırılacağını tanımlayan Chaos zamanlama sürümünü alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-chaos-schedule-set"></a>sfctl chaos zamanlaması Ayarla
Kaos tarafından kullanılan zamanlamasını ayarlayın.

Chaos çalıştırmaları Chaos bir zamanlamaya göre otomatik olarak zamanlar. Sağlanan giriş zamanlama sürümde sunucuda Chaos zamanlamasını eşleşmelidir. Sunucu sürümünde sağlanan sürüm eşleşmiyorsa, Chaos zamanlama güncelleştirilmez. Sunucu sürümünde sağlanan sürüm eşleşiyorsa, Chaos zamanlama güncelleştirilir ve Chaos zamanlamaya göre sunucu sürümü oluşturan bir ve dön 0 sarar 2.147.483.647 sonra artırılır. Bu çağrı yapıldığında Chaos çalışıyorsa, çağrı başarısız olur.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --chaos parametreleri sözlüğü | JSON dizesi adlarının bir eşleme işleri tarafından kullanılacak ChaosParameters temsil eden bir liste kodlanmış. |
| --expiry-date-utc | Tarih ve saat için ne zaman durdurulacağını Chaos zamanlamak için zamanlamayı kullanma.  Default\: 9999-12-31T23\:59\:59.999Z. |
| --jobs | JSON olarak kodlanmış listesinde ChaosScheduleJobs ne zaman çalıştırılacağını Chaos temsil eden ve Chaos ile çalıştırmak için hangi parametrelere sahip. |
| --Başlangıç tarihi utc | Tarih ve saat zamanlama Chaos zamanlamak için kullanmaya başlamak ne zaman.  Varsayılan\: 1601-01-01T00\:00\:00.000Z. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |
| --version | Zamanlama sürüm sayısı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

### <a name="examples"></a>Örnekler

Aşağıdaki komut, 2016-01-01 başlayıp 2038-01-Chaos 24 saat, haftada 7 gün günün çalıştıran 01 sona erme tarihi (geçerli zamanlama sürümde 0 varsayılarak) zamanlamasını ayarlar. Kaos kümede bu süre için zamanlanır.

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
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application).
