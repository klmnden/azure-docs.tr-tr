---
title: Azure Service Fabric CLI - sfctl mesh hizmeti çoğaltması | Microsoft Docs
description: Service Fabric CLI'sını sfctl kafes hizmeti çoğaltması komutlarını açıklamaktadır.
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
ms.openlocfilehash: 1e0955ef2a52a6313f0449b956229e03f0a5b5e8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61038457"
---
# <a name="sfctl-mesh-service-replica"></a>sfctl mesh service-replica
Çoğaltma ayrıntılarını ve liste çoğaltmalarını belirli bir hizmete bir uygulama kaynağı alın.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| list | Bir hizmetin tüm çoğaltmaların listeler. |
| göster | Bir uygulamanın hizmet verilen kopyasını alır. |

## <a name="sfctl-mesh-service-replica-list"></a>sfctl kafes hizmet çoğaltma listesi
Bir hizmetin tüm çoğaltmaların listeler.

Bir hizmetin tüm çoğaltmaları hakkında bilgileri alır. Bilgi, açıklama ve diğer hizmet çoğaltma özelliklerini içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| ---[gerekli] adı--uygulama adı | Uygulamanın adı. |
| --service-name [gerekli] | Hizmetin adı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-mesh-service-replica-show"></a>sfctl kafes hizmeti çoğaltması Göster
Bir uygulamanın hizmet verilen kopyasını alır.

Belirtilen ada sahip service çoğaltma ile ilgili bilgileri alır. Bilgi, açıklama ve diğer hizmet çoğaltma özelliklerini içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| ---[gerekli] adı--uygulama adı | Uygulamanın adı. |
| --ad -n [gerekli] | Hizmet çoğaltma adı. |
| --service-name [gerekli] | Hizmetin adı. |

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