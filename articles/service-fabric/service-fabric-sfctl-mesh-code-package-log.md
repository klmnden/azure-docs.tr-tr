---
title: Azure Service Fabric CLI - sfctl mesh kod paketi günlük | Microsoft Docs
description: Service Fabric CLI'sını sfctl kafes kod paketi günlük komutlarını açıklamaktadır.
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
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: 81ddcc8c5685a839afabc1e82ecf4246cb813c21
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53285098"
---
# <a name="sfctl-mesh-code-package-log"></a>sfctl kafes kod-paketi-log
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