---
title: Azure Service Fabric CLI - sfctl sa-küme | Microsoft Docs
description: Service Fabric CLI'sını sfctl tek başına küme komutlarını açıklamaktadır.
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
ms.openlocfilehash: d7f33bf0657ca2a6888387b7651706f9de537bb4
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39494365"
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

|Bağımsız değişken|Açıklama|
| --- | --- |
| --configuration-API-VERSION [gerekli] | Tek başına küme json yapılandırma API sürümü. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız değişken|Açıklama|
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

|Bağımsız değişken|Açıklama|
| --- | --- |
| --Küme yapılandırma [gerekli] | Kümeye uygulanacak küme yapılandırması. |
| --Uygulama sistem durumu ilkeleri | Uygulama türü adı ve en yüksek yüzdesi hatası tetiklenmeden önce sağlıksız çiftleri kodlamalı JSON sözlüğü. |
| --delta iyi durumda olmayan-düğümler | İzin verilen maksimum delta sistem durumu performans düşüşü yükseltme sırasında yüzdesi. İzin verilen değerler sıfırdan 100 tamsayı değerleri. |
| --Sistem durumu denetimi deneme | Uygulama veya kümenin iyi durumda değilse, bir sistem durumu gerçekleştirmeyi dener arasındaki sürenin uzunluğunu denetler.  Varsayılan\: PT0H0M0S. |
| --Sistem durumu denetimi kararlı | Sürenin uzunluğunu uygulama veya kümenin iyi durumda kalmalıdır.  Varsayılan\: PT0H0M0S. |
| --Sistem durumu denetimi bekleme | Sistem başlatmadan önce bir yükseltme etki alanını tamamladıktan sonra beklenecek süreyi işlemi denetler.  Varsayılan\: PT0H0M0S. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |
| --iyi durumda olmayan uygulamalar | Yükseltme sırasında izin verilen en fazla iyi durumda olmayan uygulamalar yüzdesi. İzin verilen değerler sıfırdan 100 tamsayı değerleri. |
| --iyi durumda olmayan düğümler | Yükseltme sırasında izin verilen en fazla iyi durumda olmayan düğümler yüzdesi. İzin verilen değerler sıfırdan 100 tamsayı değerleri. |
| --Yükseltme etki alanı-delta-sağlıksız-düğümler | Yükseltme sırasında yükseltme etki alanı delta sistem durumu performans düşüşü yüzdesi, izin verilen en fazla. İzin verilen değerler sıfırdan 100 tamsayı değerleri. |
| --Yükseltme-etki-zaman aşımı | Yükseltme etki alanı için zaman aşımı.  Varsayılan\: PT0H0M0S. |
| --Yükseltme zaman aşımı | Yükseltme zaman aşımı.  Varsayılan\: PT0H0M0S. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

### <a name="examples"></a>Örnekler

Bir küme yapılandırmasını güncelleştirme sfctl sa-cluster config-yükseltmesi--küme-config Başlat <YOUR CLUSTER CONFIG> --uygulama sistem durumu ilkeleri "{" fabric: / Sistem ": {"ConsiderWarningAsError": true}}"

## <a name="sfctl-sa-cluster-upgrade-status"></a>sfctl sa-Küme Yükseltme durumu
Service Fabric tek başına Küme Küme yapılandırma yükseltme durumunu alın.

Küme yapılandırmasını yükseltme durumunu Service Fabric tek başına küme ayrıntılarını alın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
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