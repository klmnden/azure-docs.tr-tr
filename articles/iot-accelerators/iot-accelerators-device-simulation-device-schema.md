---
title: Cihaz şema cihaz benzetimi çözümüne - Azure | Microsoft Docs
description: Bu makalede, bir sanal cihaz cihaz benzetimi çözümünde tanımlayan JSON şemasını açıklar.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: a27624d4c7a0bde4b33aefe8d05881b743ce397d
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39285837"
---
# <a name="understand-the-device-model-schema"></a>Cihaz modeli şemasını anlama

Cihaz benzetimi çözüm Hızlandırıcı, IOT hub'ı kullanan çözümler için test yükleri oluşturmak için kullanabilirsiniz. Cihaz benzetimi çözümünü dağıttığınızda, sanal cihaz koleksiyonunu otomatik olarak sağlanır. Mevcut sanal cihazlar özelleştirme veya kendinizinkini oluşturun.

Bu makalede, bir sanal cihaz davranışını ve özellikleri belirten cihaz modeli şemasını açıklar. Cihaz modeli, bir JSON dosyasında depolanır.

Aşağıdaki makaleler için geçerli makalenin ilgili:

* [Cihaz modeli davranışlarını uygulamak](iot-accelerators-device-simulation-device-behavior.md) bir sanal cihaz davranışını uygulamak için kullandığınız JavaScript dosyalarını açıklar.
* [Yeni bir simülasyon cihazı oluşturma](iot-accelerators-remote-monitoring-test.md) tümünü bir araya getirir ve çözümünüze yeni bir sanal cihaz türü dağıtma işlemi gösterilmektedir.

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

JSON şeması hakkında öğrendiniz, önerilen sonraki adıma öğrenmektir nasıl [sanal Cihazınızı davranışını uygulayan](iot-accelerators-device-simulation-device-behavior.md).

Cihaz benzetimi çözüm hakkında daha fazla Geliştirici bilgi için bkz. [geliştirici başvuru kılavuzu](https://github.com/Azure/device-simulation-dotnet/wiki/Simulation-Service-Developer-Reference-Guide).
