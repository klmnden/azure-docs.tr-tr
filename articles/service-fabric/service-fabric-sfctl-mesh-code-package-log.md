---
title: Azure Service Fabric CLI - sfctl mesh kod paketi günlük | Microsoft Docs
description: Service Fabric CLI'sını sfctl kafes kod paketi günlük komutlarını açıklamaktadır.
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
ms.openlocfilehash: e7bc8491071946eaa2e322517e5d36d681a49130
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58661080"
---
# <a name="sfctl-mesh-code-package-log"></a>sfctl mesh code-package-log
Verili hizmet çoğaltması için belirtilen kod paketi kapsayıcı için günlükleri alın.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| Al | Günlükleri kapsayıcıdan alır. |

## <a name="sfctl-mesh-code-package-log-get"></a>sfctl kafes kod paketi günlük Al
Günlükleri kapsayıcıdan alır.

Belirtilen kod paketi hizmet çoğaltması kapsayıcı için günlükleri alır.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| ---[gerekli] adı--uygulama adı | Uygulamanın adı. |
| --kod-paketi-name [gerekli] | Hizmet kod paketinin adı. |
| --çoğaltma adı [gerekli] | Service Fabric çoğaltma adı. |
| --service-name [gerekli] | Hizmetin adı. |
| --tail | Günlükleri sonundan gösterilecek satırların sayısı. Varsayılan 100'dür. 'tüm' günlüklerin tamamını göstermek için. |

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