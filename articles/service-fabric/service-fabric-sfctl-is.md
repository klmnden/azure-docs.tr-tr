---
title: Azure Service Fabric CLI - sfctl olan | Microsoft Docs
description: Service Fabric CLI'yı açıklar sfctl olan komutları.
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
ms.date: 07/31/2018
ms.author: bikang
ms.openlocfilehash: 53e49099fd3486d51f021528c9354cf32f4952d2
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39492798"
---
# <a name="sfctl-is"></a>sfctl is
Sorgulamak ve hizmet için altyapı komutlar gönderebilirsiniz.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| command | Verilen altyapı hizmeti örneği yönetimsel bir komutu çalıştırır. |
| sorgu | Verilen altyapı hizmeti örneği üzerinde salt okunur bir sorguyu çağırır. |

## <a name="sfctl-is-command"></a>sfctl komutudur.
Verilen altyapı hizmeti örneği yönetimsel bir komutu çalıştırır.

Yapılandırılmış altyapı hizmeti bir veya daha fazla örneğe sahip kümeler için bu API, hizmet altyapı belirli bir örneğini altyapı özgü komutlar göndermek için bir yol sağlar. Kullanılabilir komutları ve bunların karşılık gelen yanıt biçimleri küme üzerinde çalıştığı altyapı bağlı olarak farklılık gösterir. Bu API, Service Fabric platform destekler. doğrudan sizin kodunuzdan kullanılmak üzere tasarlanmamıştır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --[gerekli] komutu | Çağrılacak komut metni. Komut içeriğini altyapı özeldir. |
| --Hizmet kimliği | Altyapı hizmeti kimliği. <br><br> Bu hizmet altyapı 'doku' URI düzeni olmadan tam adıdır. Bu parametre yalnızca birden fazla örneğini çalıştıran hizmet altyapı sahip küme için gereklidir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-is-query"></a>sfctl sorgudur.
Verilen altyapı hizmeti örneği üzerinde salt okunur bir sorguyu çağırır.

Yapılandırılmış altyapı hizmeti bir veya daha fazla örneğe sahip kümeler için bu API, hizmet altyapı belirli bir örneğini altyapı özgü sorguları göndermek için bir yol sağlar. Kullanılabilir komutları ve bunların karşılık gelen yanıt biçimleri küme üzerinde çalıştığı altyapı bağlı olarak farklılık gösterir. Bu API, Service Fabric platform destekler. doğrudan sizin kodunuzdan kullanılmak üzere tasarlanmamıştır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --[gerekli] komutu | Çağrılacak komut metni. Komut içeriğini altyapı özeldir. |
| --Hizmet kimliği | Altyapı hizmeti kimliği. <br><br> Bu hizmet altyapı tam adı, ' fabric\:' URI düzeni. Bu parametre yalnızca birden fazla örneğini çalıştıran hizmet altyapı sahip küme için gereklidir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |


## <a name="next-steps"></a>Sonraki adımlar
- [Ayarlanan](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application).