---
title: Benzetimli cihaz davranışının Uzaktan izleme çözümünde - Azure | Microsoft Docs
description: Bu makalede, Uzaktan izleme çözümünde sanal cihaz davranışını tanımlamak için JavaScript kullanmayı açıklar.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: 5c05f2617025d5cb4f1328f04c8d71049e1efcc7
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39284784"
---
# <a name="implement-the-device-model-behavior"></a>Cihaz modeli davranışlarını uygulayın

Makaleyi [cihaz modeli şemasını anlama](iot-accelerators-remote-monitoring-device-schema.md) bir sanal cihaz modeli tanımlayan şemayı açıklanmaktadır. Bu makalede, bir sanal cihaz davranışını uygulayan iki tür JavaScript dosyası için başvurulan:

- **Durum** JavaScript dosyaları, cihaz iç durumunu güncelleştirmek için sabit aralıklarla çalıştırın.
- **Yöntemi** JavaScript dosyaları çözüm cihaz üzerinde bir yöntemi çağırdığında çalıştıran.

Bu makalede şunları öğreneceksiniz:

>[!div class="checklist"]
> * Bir sanal cihaz durumunu denetleme
> * Bir sanal cihaz için bir yöntem çağrısının Uzaktan izleme çözümü nasıl yanıt tanımlama
> * Komut dosyalarınızı hata ayıklama

[!INCLUDE [iot-accelerators-device-schema](../../includes/iot-accelerators-device-schema.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu makale, kendi özel sanal cihaz modeli davranışlarını tanımlamak açıklanmıştır. Bu makalede gösterilen, nasıl için:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Bir sanal cihaz durumunu denetleme
> * Bir sanal cihaz için bir yöntem çağrısının Uzaktan izleme çözümü nasıl yanıt tanımlama
> * Komut dosyalarınızı hata ayıklama

Bir sanal cihaz davranışını belirtmek öğrendiniz, önerilen sonraki adıma öğrenmektir nasıl [bir simülasyon cihazı oluşturma](iot-accelerators-remote-monitoring-test.md).

Uzaktan izleme çözümü hakkında daha fazla Geliştirici bilgi için bkz:

* [Geliştirici Başvuru Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Geliştirici Sorun Giderme Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->
