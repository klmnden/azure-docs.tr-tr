---
title: Nesnelerin İnterneti (IoT Edge) için Azure çözümleri | Microsoft Docs
description: Örnek bir IoT çözümü mimarisi ile cihazlar, Azure IoT Hub hizmeti, Azure IoT cihaz SDK'ları, Azure IoT hizmeti SDK'ları ve diğer Azure hizmetleriyle olan ilişkisi.
author: dominicbetts
manager: timlt
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 09/15/2017
ms.author: dobett
ms.openlocfilehash: bd59e740803f8f0e6f5f542805d615772efba913
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34630349"
---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Edge, uç cihazlarında analize ve veri işlemeye olanak tanıyan bir Azure hizmetidir. IoT Edge ile, doğrudan zaten kullanıyor olduğunuz Azure hizmetlerinden gönderilen mantığı içeren kapsayıcı tabanlı kodla veya kendi çözümünüze özgü kodla cihazlarınızı güçlendirebilirsiniz. Cihazlarınızın şunları yapabilmesini sağlar:

* Ağ geçici cihazları gibi davranarak birden çok yaprak cihazdan verileri toplayabilir ve işleyebilir.
* Anomali algılaması yapabilir ve bulutta gelecek yönergeleri beklemek zorunda kalmadan ortamdaki değişikliklere yanıt verebilir.
* Verileri önceden işleyerek ve soruçları göndererek bant genişliği ve depolama maliyetlerini en aza indirebilir. 

IoT Edge, cihazların uzaktan yönetimine olanak tanıyan bir de bulut arabirimi içerir. Cihazlarınıza fiziksel olarak erişmek zorunda kalmadan kod dağıtabilir, durum izlemesi yapabilir ve güncelleştirebilirsiniz. Tek tek cihazları uzaktan yönetebileceğiniz gibi, tanımladığınız geniş bir cihaz kümesini etkileyecek dağıtımlar da oluşturabilirsiniz. Daha fazla bilgi için bkz. [Tek tek cihazlarda veya belirli bir ölçekte IoT Edge dağıtımlarını anlama][lnk-deployment].

IoT Edge'i etkinleştiren bileşenleri öğrenmek için bkz. [Azure IoT Edge nasıl çalışır][lnk-overview].

Azure IoT Hub'ını daha önce hiç kullanmadıysanız, [Azure IoT Hub'ı hizmetine gelen bakış][lnk-iot-hub] bilgileriyle başlamak isteyebilirsiniz.

[lnk-deployment]: module-deployment-monitoring.md
[lnk-overview]: how-iot-edge-works.md
[lnk-iot-hub]: ../iot-hub/iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: ../iot-hub/iot-hub-device-management-overview.md
