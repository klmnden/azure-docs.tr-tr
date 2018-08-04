---
title: Azure Service Fabric CLI - sfctl kapsayıcı | Microsoft Docs
description: Service Fabric CLI'sını sfctl kapsayıcı komutlarını açıklamaktadır.
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
ms.openlocfilehash: 27108d27ee27346e4cba44e6778faff56df70a36
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39495137"
---
# <a name="sfctl-container"></a>sfctl container
Kapsayıcı çalıştırma komutları bir küme düğümünde ilgili.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| API çağırma | Kapsayıcı REST API'sini çağırır. |
| günlükler | Kapsayıcı günlükleri alma. |

## <a name="sfctl-container-invoke-api"></a>çağırma sfctl container-API
Kapsayıcı REST API'sini çağırır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. |
| --kod-paketi-örnek-[gerekli] kimliği | 'Hizmet kod-paketi-listesi tarafından' alınabilir paket örnek kimliği kod. |
| --kod-paketi-name [gerekli] | Kod paketi adı. |
| --container-API-uri-[gerekli] yolu | Kapsayıcı REST API URI yolu, '{id}' kapsayıcı adı/kimliği yerine kullanın. |
| --[gerekli] düğüm adı | Düğümün adı. |
| --service-bildirim-name [gerekli] | Hizmet bildirim adı. |
| --container-API-gövde | Kapsayıcı REST API için HTTP istek gövdesi. |
| --container-API-içerik türü | Kapsayıcı REST API için içerik türü varsayılan olarak 'application/json'. |
| --container-API-http fiili | GET için varsayılan olarak HTTP fiili kapsayıcısı, REST API için. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-container-logs"></a>sfctl kapsayıcı günlükleri
Kapsayıcı günlükleri alma.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. |
| --kod-paketi-örnek-[gerekli] kimliği | 'Hizmet kod-paketi-listesi tarafından' alınabilir paket örnek kimliği kod. |
| --kod-paketi-name [gerekli] | Kod paketi adı. |
| --[gerekli] düğüm adı | Düğümün adı. |
| --service-bildirim-name [gerekli] | Hizmet bildirim adı. |
| --tail | Yalnızca günlükleri sonundan itibaren bu günlük satır sayısını döndürür. Bir tamsayı veya tüm olarak tüm günlük satırları çıktısını almak için bu seçeneği belirtin. Varsayılan olarak 'all' için. |
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