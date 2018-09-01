---
title: Cihaz şema Uzaktan izleme çözümünde - Azure | Microsoft Docs
description: Bu makalede Uzaktan izleme çözümünde bir simülasyon cihazı tanımlayan bir JSON şeması.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: f312f29e14c371e7b500f3eee6471151e3544513
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43338864"
---
# <a name="understand-the-device-model-schema"></a>Cihaz modeli şemasını anlama

Sanal cihazlar, Uzaktan izleme çözümünde davranışını test etmek için kullanabilirsiniz. Uzaktan izleme çözümünü dağıttığınızda, sanal cihaz koleksiyonunu otomatik olarak sağlanır. Mevcut sanal cihazlar özelleştirme veya kendinizinkini oluşturun.

Bu makalede, bir sanal cihaz davranışını ve özellikleri belirten cihaz modeli şemasını açıklar. Cihaz modeli, bir JSON dosyasında depolanır.

Aşağıdaki makaleler için geçerli makalenin ilgili:

* [Cihaz modeli davranışlarını uygulamak](iot-accelerators-remote-monitoring-device-behavior.md) bir sanal cihaz davranışını uygulamak için kullandığınız JavaScript dosyalarını açıklar.
* [Yeni bir simülasyon cihazı oluşturma](iot-accelerators-remote-monitoring-create-simulated-device.md) tümünü bir araya getirir ve çözümünüze yeni bir sanal cihaz türü dağıtma işlemi gösterilmektedir.

Bu makalede şunları öğreneceksiniz:

>[!div class="checklist"]
> * Bir sanal cihaz modeli tanımlamak için bir JSON dosyası kullanma
> * Sanal cihaz özelliklerini belirtin
> * Sanal cihazın gönderdiği telemetriyi belirtin
> * Cihaz yanıt veren bulut-cihaz yöntemleri belirtin

[!INCLUDE [iot-accelerators-device-schema](../../includes/iot-accelerators-device-schema.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kendi özel sanal cihaz modeli oluşturmak nasıl kaydedileceği açıklanır. Bu makalede gösterilen, nasıl için:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Bir sanal cihaz modeli tanımlamak için bir JSON dosyası kullanma
> * Sanal cihaz özelliklerini belirtin
> * Sanal cihazın gönderdiği telemetriyi belirtin
> * Cihaz yanıt veren bulut-cihaz yöntemleri belirtin

JSON şeması hakkında öğrendiniz, önerilen sonraki adıma öğrenmektir nasıl [sanal Cihazınızı davranışını uygulayan](iot-accelerators-remote-monitoring-device-behavior.md).

Uzaktan izleme çözümü hakkında daha fazla Geliştirici bilgi için bkz:

* [Geliştirici Başvuru Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Geliştirici Sorun Giderme Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)
