---
author: wesmc7777
ms.author: wesmc
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: 19331f35ea2fa773325ec61e728677e37767ab54
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66156385"
---
> [!div class="op_single_selector"]
> * [Cihaz: Node.js hizmeti: Node.js](../articles/iot-hub/iot-hub-node-node-device-management-get-started.md)
> * [Cihaz: C#Hizmet: C#](../articles/iot-hub/iot-hub-csharp-csharp-device-management-get-started.md)
> * [Cihaz: Java hizmeti: Java](../articles/iot-hub/iot-hub-java-java-device-management-getstarted.md)
> * [Cihaz: Python hizmeti: Python](../articles/iot-hub/iot-hub-python-python-device-management-get-started.md)

Arka uç uygulamaları kullanabileceğiniz Azure IOT hub'ı temelleri gibi [cihaz ikizi] [ lnk-devtwin] ve [doğrudan yöntemler][lnk-c2dmethod], uzaktan başlatın ve izleme cihazda yönetim eylemleri. Bu öğretici, bir arka uç uygulaması ve bir cihaz uygulaması birlikte nasıl başlatmak ve IOT hub'ı kullanarak uzak cihazı yeniden başlatma izlemek için çalışabileceğini gösterir.

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Bulutta bir arka uç uygulamasından (örneğin, yeniden başlatma, Fabrika sıfırlaması ve üretici yazılımı güncelleştirmesi) cihaz yönetim eylemleri başlatmak için bir doğrudan yöntem kullanın. Cihaz için sorumludur:

* IOT Hub'ından gönderilen yöntemi istek işleme.
* Cihazdaki ilgili cihaza özgü eylemi başlatılıyor.
* Durum güncelleştirmeleri aracılığıyla sağlama *bildirilen özellikler* IOT hub'ına.

Bulutta bir arka uç uygulaması, cihaz yönetim eylemleri, ilerleme üzerinde rapor için cihaz çifti sorguları çalıştırmak için kullanabilirsiniz.

[lnk-devtwin]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
