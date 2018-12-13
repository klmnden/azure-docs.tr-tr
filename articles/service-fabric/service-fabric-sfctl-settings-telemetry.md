---
title: Azure Service Fabric CLI - sfctl ayarları telemetri | Microsoft Docs
description: Service Fabric CLI'sını sfctl ayarları telemetri komutlarını açıklamaktadır.
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
ms.openlocfilehash: e0e229f0edb078fe9ce289d0089f34d7cbf9dbf8
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53285399"
---
# <a name="sfctl-settings-telemetry"></a>sfctl ayarları telemetri
Sfctl Bu örneği için yerel telemetri ayarlarını yapılandırın.

Sfctl telemetri toplar komut adı olmadan sağlanan parametreleri veya değerleri, sfctl sürümü, işletim sistemi türü, python sürümü, başarı veya başarısızlık komutun, hata iletisi döndürdü.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| set-telemetri | Telemetri açıp kapatabilir. |

## <a name="sfctl-settings-telemetry-set-telemetry"></a>sfctl ayarları telemetri set-telemetri
Telemetri açıp kapatabilir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --devre dışı | Telemetriyi etkinleştirin. |
| --üzerinde | Telemetriyi etkinleştirin. Varsayılan değer budur. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

### <a name="examples"></a>Örnekler

Telemetriyi etkinleştirin.

```
sfctl settings telemetry set_telemetry --off
```

Telemetriyi etkinleştirin.

```
sfctl settings telemetry set_telemetry --on
```


## <a name="next-steps"></a>Sonraki adımlar
- [Ayarlanan](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application).