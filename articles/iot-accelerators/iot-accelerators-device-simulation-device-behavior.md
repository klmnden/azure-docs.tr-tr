---
title: Benzetimli cihaz davranışının cihaz benzetimi çözümüne - Azure | Microsoft Docs
description: Bu makalede, cihaz benzetimi çözümde sanal cihaz davranışını tanımlamak için JavaScript kullanmayı açıklar.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: b68550bce1f4e3fbe3845c21598720083c8e384c
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39285830"
---
# <a name="implement-the-device-model-behavior"></a>Cihaz modeli davranışlarını uygulayın

Makaleyi [cihaz modeli şemasını anlama](iot-accelerators-device-simulation-device-schema.md) bir sanal cihaz modeli tanımlayan şemayı açıklanmaktadır. Bu makalede, bir sanal cihaz davranışını uygulayan iki tür JavaScript dosyası için başvurulan:

- **Durum**: JavaScript dosyaları, cihaz iç durumunu güncelleştirmek için sabit aralıklarla çalıştırın.
- **Yöntemi**: çözüm, cihaz üzerinde bir yöntemi çağırdığında çalıştırılan JavaScript dosyaları.

Bu makalede şunları öğreneceksiniz:

>[!div class="checklist"]
> * Bir sanal cihaz durumunu denetleme
> * Metot çağrısı bir simülasyon cihazı bağlı IOT hub'ı nasıl yanıt tanımlama
> * Komut dosyalarınızı hata ayıklama

[!INCLUDE [iot-accelerators-device-schema](../../includes/iot-accelerators-device-schema.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu makale, kendi özel sanal cihaz modeli davranışlarını tanımlamak açıklanmıştır. Bu makalede gösterilen, nasıl için:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Bir sanal cihaz durumunu denetleme
> * Metot çağrısı bir simülasyon cihazı bağlı IOT hub'ı nasıl yanıt tanımlama
> * Komut dosyalarınızı hata ayıklama

Bir sanal cihaz davranışını belirtmek öğrendiniz, önerilen sonraki adıma öğrenmektir nasıl [bir simülasyon cihazı oluşturma](iot-accelerators-remote-monitoring-test.md).

Cihaz benzetimi çözüm hakkında daha fazla Geliştirici bilgi için bkz. [geliştirici başvuru kılavuzu](https://github.com/Azure/device-simulation-dotnet/wiki/Simulation-Service-Developer-Reference-Guide).
