---
title: Azure Service Fabric CLI - sfctl ayarları telemetri | Microsoft Docs
description: Service Fabric CLI'sını sfctl ayarları telemetri komutlarını açıklamaktadır.
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
ms.openlocfilehash: 42a82ab0be37f260a48a1da6cecab5120c24d293
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58669274"
---
# <a name="sfctl-settings-telemetry"></a>sfctl settings telemetry
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