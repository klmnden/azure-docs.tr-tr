---
title: Azure Service Fabric CLI - sfctl kapsayıcı | Microsoft Docs
description: Service Fabric CLI'sını sfctl kapsayıcı komutlarını açıklamaktadır.
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
ms.openlocfilehash: a5037c535737946a50d8af6fa60d0815120276d9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60837341"
---
# <a name="sfctl-container"></a>sfctl container
Kapsayıcı çalıştırma komutları bir küme düğümünde ilgili.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| API çağırma | Belirli bir kod paketi için bir Service Fabric düğüm dağıtılan bir kapsayıcı kapsayıcı API'sini çağırır. |
| günlükler | Kapsayıcı günlükleri için kapsayıcı bir Service Fabric düğüm belirli bir kod paketi için dağıtılan alır. |

## <a name="sfctl-container-invoke-api"></a>çağırma sfctl container-API
Belirli bir kod paketi için bir Service Fabric düğüm dağıtılan bir kapsayıcı kapsayıcı API'sini çağırır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. <br><br> Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "fabric\:/myapp/app1", uygulama kimliği olur "myapp\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --kod-paketi-örnek-[gerekli] kimliği | Bir kod paketi örneği üzerinde bir service fabric düğümünü dağıtılan benzersiz olarak tanımlayan kimliği. <br><br> 'Hizmet kod-paketi-listesi tarafından' alınabilir. |
| --kod-paketi-name [gerekli] | Bir Service Fabric kümesindeki bir uygulama türünün bir parçası olarak kayıtlı hizmet bildiriminde belirtilen kod paketi adı. |
| --container-API-uri-[gerekli] yolu | Kapsayıcı REST API URI yolu, '{id}' kapsayıcı adı/kimliği yerine kullanın. |
| --[gerekli] düğüm adı | Düğümün adı. |
| --service-bildirim-name [gerekli] | Bir Service Fabric kümesindeki bir uygulama türünün bir parçası olarak kayıtlı bir hizmet bildiriminin adı. |
| --container-API-gövde | Kapsayıcı REST API için HTTP istek gövdesi. |
| --container-api-content-type | Kapsayıcı REST API için içerik türü varsayılan olarak 'application/json'. |
| --container-api-http-verb | GET için varsayılan olarak HTTP fiili kapsayıcısı, REST API için. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-container-logs"></a>sfctl kapsayıcı günlükleri
Kapsayıcı günlükleri için kapsayıcı bir Service Fabric düğüm belirli bir kod paketi için dağıtılan alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Uygulama-kimliği [gerekli] | Uygulama kimliği. <br><br> Bu genellikle uygulamayı olmadan tam adı, ' fabric\:' URI düzeni. Sürüm 6. 0 ' başlayarak, hiyerarşik adları ile ayrılmış "\~" karakter. Örneğin, uygulama adı ise "fabric\:/myapp/app1", uygulama kimliği olur "myapp\~app1" 6.0 + ve "myapp/app1" önceki sürümlerinde. |
| --kod-paketi-örnek-[gerekli] kimliği | 'Hizmet kod-paketi-listesi tarafından' alınabilir paket örnek kimliği kod. |
| --kod-paketi-name [gerekli] | Bir Service Fabric kümesindeki bir uygulama türünün bir parçası olarak kayıtlı hizmet bildiriminde belirtilen kod paketi adı. |
| --[gerekli] düğüm adı | Düğümün adı. |
| --service-bildirim-name [gerekli] | Bir Service Fabric kümesindeki bir uygulama türünün bir parçası olarak kayıtlı bir hizmet bildiriminin adı. |
| --tail | Günlükleri sonundan gösterilecek satırların sayısı. Varsayılan 100'dür. 'tüm' günlüklerin tamamını göstermek için. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |


## <a name="next-steps"></a>Sonraki adımlar
- [Ayarlanan](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application).