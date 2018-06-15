---
title: Azure Service Fabric CLI - sfctl sa-küme | Microsoft Docs
description: Service Fabric CLI sfctl tek başına küme komutlarını açıklar.
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
ms.openlocfilehash: ffdbff7edc5af187071615c8b1e61790b3a38429
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34764039"
---
# <a name="sfctl-sa-cluster"></a>sfctl sa-küme
Tek başına Service Fabric kümeleri yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| Yapılandırma | Service Fabric tek başına küme yapılandırmasını alın. |
| config yükseltme | Service Fabric tek başına küme yapılandırmasını yükseltme başlatın. |
| Yükseltme durumu | Küme yapılandırması yükseltme durumunu Service Fabric tek başına küme alın. |

## <a name="sfctl-sa-cluster-config"></a>sfctl sa küme yapılandırması
Service Fabric tek başına küme yapılandırmasını alın.

Service Fabric tek başına küme yapılandırmasını alın. Küme yapılandırmasını farklı bir düğüme türlerinde küme, güvenlik yapılandırmalarını, arıza ve yükseltme etki alanı topolojiler, vb. dahil küme özelliklerini içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --configuration-API-VERSION [gerekli] | Tek başına küme json yapılandırma API sürümü. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-sa-cluster-config-upgrade"></a>sfctl sa küme config-yükseltme
Service Fabric tek başına küme yapılandırmasını yükseltme başlatın.

Sağlanan yapılandırma yükseltme parametreleri doğrulayın ve küme yapılandırması parametreleri geçerliyse yükseltme başlatın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Küme-config [gerekli] | Küme yapılandırması. |
| --delta sağlıksız-düğümler | Yükseltme sırasında delta sistem durumu düşüşü yüzdesi izin verilen en fazla. İzin verilen değerler, sıfırdan 100 tamsayı değerlerdir. |
| --Sistem durumu denetimi yeniden | Bir sistem durumu gerçekleştirmek için girişimleri arasındaki süre uygulama veya küme sağlıklı olup olmadığını denetler.  Varsayılan\: PT0H0M0S. |
| --Sistem durumu denetimi kararlı | Sürenin uzunluğu uygulama veya küme sağlıklı kalmalıdır.  Varsayılan\: PT0H0M0S. |
| --Sistem durumu denetimi bekleme | Sistem durumu başlatmadan önce bir yükseltme etki alanına tamamladıktan sonra beklenecek süreyi işlemi denetler.  Varsayılan\: PT0H0M0S. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye cinsinden.  Varsayılan\: 60. |
| --sağlıksız uygulamaları | Yükseltme sırasında izin verilen en fazla sağlıksız uygulamaları yüzdesi. İzin verilen değerler, sıfırdan 100 tamsayı değerlerdir. |
| --sağlıksız düğümleri | Yükseltme sırasında izin verilen en fazla sağlıksız düğümleri yüzdesi. İzin verilen değerler, sıfırdan 100 tamsayı değerlerdir. |
| --Yükseltme etki alanı-delta-sağlıksız-düğümler | Yükseltme sırasında yükseltme etki alanı delta sistem durumu düşüşü yüzdesi izin verilen en fazla. İzin verilen değerler, sıfırdan 100 tamsayı değerlerdir. |
| --Yükseltme etki alanı timeout | Yükseltme etki alanı için zaman aşımı.  Varsayılan\: PT0H0M0S. |
| --Yükseltme zaman aşımı | Yükseltme zaman aşımı.  Varsayılan\: PT0H0M0S. |

### <a name="global-arguments"></a>Genel bağımsız değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Günlük ayrıntı tüm hata ayıklama günlüklerini göster artırın. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| ---o çıktı | Çıktı biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --ayrıntılı | Günlüğün ayrıntı düzeyini artırın. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-sa-cluster-upgrade-status"></a>sfctl sa Küme yükseltme-durum
Küme yapılandırması yükseltme durumunu Service Fabric tek başına küme alın.

Küme yapılandırmasını yükseltme durumunu Service Fabric tek başına kümenin Al Ayrıntıları.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
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