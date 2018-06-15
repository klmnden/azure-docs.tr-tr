---
title: Azure Service Fabric CLI sfctl kapsayıcı | Microsoft Docs
description: Service Fabric CLI sfctl kapsayıcı komutlarını açıklar.
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
ms.openlocfilehash: cd3725ac547a1ed1fd9207dc48ba3b6227e85ef1
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34764038"
---
# <a name="sfctl-container"></a>sfctl kapsayıcı
Kapsayıcı ile ilgili komutları bir küme düğümünde çalıştırın.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| Invoke-API | Kapsayıcı REST API çağırma. |
| günlükler | Alınırken kapsayıcı günlükleri. |

## <a name="sfctl-container-invoke-api"></a>çağırma sfctl kapsayıcı-API
Kapsayıcı REST API çağırma.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. |
| ---Paket-örnek-[gerekli] kodu | 'Hizmet kod-paket-liste tarafından' alınabilir paket örnek kimliği kodu. |
| --kod-paket-name [gerekli] | Kod paketi adı. |
| --kapsayıcı-API-uri-[] yolu gerekli | Kapsayıcı REST API URI yolu, '{ID}' kapsayıcı adı/kimliği yerine kullanın. |
| --düğüm adı [gerekli] | Düğümün adı. |
| --service-bildirimi-name [gerekli] | Hizmet bildirim adı. |
| --kapsayıcı API gövdesi | HTTP isteği gövdesinin kapsayıcı REST API için. |
| --kapsayıcı-API-content-type | Kapsayıcı REST API için içerik türü varsayılan olarak 'application/json'. |
| --kapsayıcı API http fiili | Kapsayıcı REST API için HTTP fiili GET varsayılan olarak ayarlanır. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-container-logs"></a>sfctl kapsayıcı günlükleri
Alınırken kapsayıcı günlükleri.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama kimliği [gerekli] | Uygulama kimliği. |
| ---Paket-örnek-[gerekli] kodu | 'Hizmet kod-paket-liste tarafından' alınabilir paket örnek kimliği kodu. |
| --kod-paket-name [gerekli] | Kod paketi adı. |
| --düğüm adı [gerekli] | Düğümün adı. |
| --service-bildirimi-name [gerekli] | Hizmet bildirim adı. |
| --tail | Yalnızca günlükleri sonundan bu günlük satır sayısını döndürür. Bir tamsayı veya tüm olarak tüm günlük satırları çıktısını almak için belirtin. Varsayılan olarak 'tüm'. |
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