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
ms.openlocfilehash: 70f9ccbbe737bad4d6f88365e804d4421c418d28
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53164016"
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

[!INCLUDE [iot-accelerators-device-behavior](../../includes/iot-accelerators-device-behavior.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu makale, kendi özel sanal cihaz modeli davranışlarını tanımlamak açıklanmıştır. Bu makalede gösterilen, nasıl için:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Bir sanal cihaz durumunu denetleme
> * Bir sanal cihaz için bir yöntem çağrısının Uzaktan izleme çözümü nasıl yanıt tanımlama
> * Komut dosyalarınızı hata ayıklama

Bir sanal cihaz davranışını belirtmek öğrendiniz, önerilen sonraki adıma öğrenmektir nasıl [bir simülasyon cihazı oluşturma](iot-accelerators-remote-monitoring-create-simulated-device.md).

Uzaktan izleme çözümü hakkında daha fazla Geliştirici bilgi için bkz:

* [Geliştirici Başvuru Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Geliştirici Sorun Giderme Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->
