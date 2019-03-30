---
title: Azure Service Fabric CLI - sfctl sa-küme | Microsoft Docs
description: Service Fabric CLI'sını sfctl tek başına küme komutlarını açıklamaktadır.
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
ms.openlocfilehash: a652439729e538b3ce2545ab3b09284e6645ce9d
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58668526"
---
# <a name="sfctl-sa-cluster"></a>sfctl sa-cluster
Tek başına Service Fabric kümeleri yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| yapılandırma | Service Fabric tek başına küme yapılandırmasını alın. |
| Yükseltme yapılandırma | Service Fabric tek başına küme yapılandırmasını yükseltme başlatın. |
| Yükseltme durumu | Service Fabric tek başına Küme Küme yapılandırma yükseltme durumunu alın. |

## <a name="sfctl-sa-cluster-config"></a>sfctl sa-cluster config
Service Fabric tek başına küme yapılandırmasını alın.

Küme yapılandırması, farklı bir düğüme türlerinin küme, güvenlik yapılandırmalarını, hata ve yükseltme etki alanı topolojiler, vb. içeren küme özelliklerini içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --configuration-API-VERSION [gerekli] | Tek başına küme json yapılandırma API sürümü. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-sa-cluster-config-upgrade"></a>sfctl sa-cluster config-yükseltme
Service Fabric tek başına küme yapılandırmasını yükseltme başlatın.

Sağlanan yapılandırma yükseltme parametreleri doğrulayın ve küme yapılandırması parametreleri geçerliyse yükseltme başlatın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --Küme yapılandırma [gerekli] | Küme yapılandırması. |
| --Uygulama sistem durumu ilkeleri | Uygulama türü adı ve en yüksek yüzdesi hatası tetiklenmeden önce sağlıksız çiftleri kodlamalı JSON sözlüğü. |
| --delta-unhealthy-nodes | İzin verilen maksimum delta sistem durumu performans düşüşü yükseltme sırasında yüzdesi. İzin verilen değerler sıfırdan 100 tamsayı değerleri. |
| --Sistem durumu denetimi deneme | Uygulama veya kümenin iyi durumda değilse, sistem durumu denetimleri gerçekleştirmek için girişimleri arasındaki süre uzunluğu.  Varsayılan\: PT0H0M0S. |
| --Sistem durumu denetimi kararlı | Süreyi sonraki yükseltme etki alanına yükseltmeye devam etmeden önce uygulama veya kümenin sağlıklı kalmasını gerekir.  Varsayılan\: PT0H0M0S. <br><br> Bu, önce bir ISO 8601 süre temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, milisaniye cinsinden toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --Sistem durumu denetimi bekleme | Sistem başlatmadan önce bir yükseltme etki alanını tamamladıktan sonra beklenecek süreyi işlemi denetler.  Varsayılan\: PT0H0M0S. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |
| --iyi durumda olmayan uygulamalar | Yükseltme sırasında izin verilen en fazla iyi durumda olmayan uygulamalar yüzdesi. İzin verilen değerler sıfırdan 100 tamsayı değerleri. |
| --iyi durumda olmayan düğümler | Yükseltme sırasında izin verilen en fazla iyi durumda olmayan düğümler yüzdesi. İzin verilen değerler sıfırdan 100 tamsayı değerleri. |
| --Yükseltme etki alanı-delta-sağlıksız-düğümler | Yükseltme sırasında yükseltme etki alanı delta sistem durumu performans düşüşü yüzdesi, izin verilen en fazla. İzin verilen değerler sıfırdan 100 tamsayı değerleri. |
| --Yükseltme-etki-zaman aşımı | Süreyi FailureAction yürütülmeden önce tamamlamak her bir yükseltme etki alanı vardır.  Varsayılan\: PT0H0M0S. <br><br> Bu, önce bir ISO 8601 süre temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, milisaniye cinsinden toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --Yükseltme zaman aşımı | Süreyi genel yükseltme FailureAction yürütülmeden önce tamamlanması gerekir.  Varsayılan\: PT0H0M0S. <br><br> Bu, önce bir ISO 8601 süre temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, milisaniye cinsinden toplam sayısını temsil eden bir sayı olarak yorumlanır. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

### <a name="examples"></a>Örnekler

Bir küme yapılandırmasını güncelleştirme Başlat

```
sfctl sa-cluster config-upgrade --cluster-config <YOUR CLUSTER CONFIG> --application-health-
policies "{"fabric:/System":{"ConsiderWarningAsError":true}}"
```

## <a name="sfctl-sa-cluster-upgrade-status"></a>sfctl sa-Küme Yükseltme durumu
Service Fabric tek başına Küme Küme yapılandırma yükseltme durumunu alın.

Küme yapılandırmasını yükseltme durumunu Service Fabric tek başına küme ayrıntılarını alın.

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


## <a name="next-steps"></a>Sonraki adımlar
- [Ayarlanan](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application).