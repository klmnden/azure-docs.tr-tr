---
title: Azure Service Fabric CLI sfctl kafes hizmet | Microsoft Docs
description: Service Fabric CLI'sını sfctl kafes hizmet komutlarını açıklamaktadır.
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
ms.openlocfilehash: e8b735780f4ed3402845d9d401f8e37701b9a1a6
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58667591"
---
# <a name="sfctl-mesh-service"></a>sfctl mesh service
Hizmet ayrıntıları ve uygulama kaynağı listesi Hizmetleri alın.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| liste | Tüm hizmet kaynakları listeler. |
| göster | Belirtilen ada sahip bir hizmet kaynağı alır. |

## <a name="sfctl-mesh-service-list"></a>sfctl kafes hizmet listesi
Tüm hizmet kaynakları listeler.

Uygulama kaynağı tüm hizmetleri hakkında bilgi alır. Bilgi, açıklama ve diğer hizmet özelliklerini içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| ---[gerekli] adı--uygulama adı | Uygulamanın adı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-mesh-service-show"></a>sfctl kafes hizmet Göster
Belirtilen ada sahip bir hizmet kaynağı alır.

Belirtilen ada sahip bir hizmet kaynağı hakkındaki bilgileri alır. Bilgi, açıklama ve diğer hizmet özelliklerini içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| ---[gerekli] adı--uygulama adı | Uygulamanın adı. |
| --ad -n [gerekli] | Hizmetin adı. |

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