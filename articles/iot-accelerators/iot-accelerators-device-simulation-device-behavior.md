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
ms.openlocfilehash: 43edbc653ddbd55aab5e722071de1f2cf4bcd1c4
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344525"
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

Bir sanal cihaz davranışını belirtmek öğrendiniz, önerilen sonraki adıma öğrenmektir nasıl [bir simülasyon cihazı oluşturma](iot-accelerators-device-simulation-create-simulated-device.md).

Cihaz benzetimi çözüm hakkında daha fazla Geliştirici bilgi için bkz. [geliştirici başvuru kılavuzu](https://github.com/Azure/device-simulation-dotnet/wiki/Simulation-Service-Developer-Reference-Guide).
