---
title: Azure Service Fabric CLI - sfctl olan | Microsoft Docs
description: Service Fabric CLI açıklar sfctl olan komutları.
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
ms.openlocfilehash: aa1e4d588f367351f0bed10370c7d67d50dd9927
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763060"
---
# <a name="sfctl-is"></a>sfctl is
Sorgulamak ve hizmet altyapı komutlar gönderebilirsiniz.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| komutu | Verilen altyapı hizmeti örneği bir yönetimsel komutu çalıştırır. |
| sorgu | Verilen altyapı hizmeti örneği üzerinde salt okunur bir sorgu çağırır. |

## <a name="sfctl-is-command"></a>sfctl komutudur.
Verilen altyapı hizmeti örneği bir yönetimsel komutu çalıştırır.

Hizmet altyapı yapılandırılmış bir veya daha fazla örnekleri sahip kümeler için bu API altyapı özgü komutlar hizmet altyapı belirli bir örneğini göndermenin bir yolunu sağlar. Kullanılabilir komutlar ve bunların karşılık gelen yanıt biçimleri küme üzerinde çalıştığı altyapı olarak değişir. Bu API, Service Fabric platformundan destekler; doğrudan kodunuzdan kullanılmak üzere tasarlanmamıştır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --[gerekli] komutu | Çağrılacak komut metni. Komut içeriğini altyapı özeldir. |
| --Hizmet kimliği | Altyapı hizmeti kimliği. <br><br> Bu hizmet altyapı 'doku' URI şeması olmadan tam adıdır. Bu parametre yalnızca altyapı hizmeti çalıştıran birden fazla örneğine sahip küme için gereklidir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-is-query"></a>sfctl sorgudur.
Verilen altyapı hizmeti örneği üzerinde salt okunur bir sorgu çağırır.

Hizmet altyapı yapılandırılmış bir veya daha fazla örnekleri sahip kümeler için bu API, belirli bir altyapı hizmeti örneği altyapı özgü sorguları göndermek için bir yol sağlar. Kullanılabilir komutlar ve bunların karşılık gelen yanıt biçimleri küme üzerinde çalıştığı altyapı olarak değişir. Bu API, Service Fabric platformundan destekler; doğrudan kodunuzdan kullanılmak üzere tasarlanmamıştır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --[gerekli] komutu | Çağrılacak komut metni. Komut içeriğini altyapı özeldir. |
| --Hizmet kimliği | Altyapı hizmeti kimliği. <br><br> Bu hizmet altyapı tam adıdır ' doku\:' URI düzeni. Bu parametre yalnızca altyapı hizmeti çalıştıran birden fazla örneğine sahip küme için gereklidir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="next-steps"></a>Sonraki adımlar
- [Ayarlanan](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek komutlar](/azure/service-fabric/scripts/sfctl-upgrade-application).