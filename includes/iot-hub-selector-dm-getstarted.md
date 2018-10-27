---
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.author: dobett
ms.openlocfilehash: 13eddced155eab6dedfbce77330e7a178ecfb3cb
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164884"
---
> [!div class="op_single_selector"]
> * [Cihaz: Node.js Service: Node.js](../articles/iot-hub/iot-hub-node-node-device-management-get-started.md)
> * [Cihaz: C# hizmeti:C#](../articles/iot-hub/iot-hub-csharp-csharp-device-management-get-started.md)
> * [Cihaz: Java hizmeti: Java](../articles/iot-hub/iot-hub-java-java-device-management-getstarted.md)
> * [Cihaz: Python Service: Python](../articles/iot-hub/iot-hub-python-python-device-management-get-started.md)

Arka uç uygulamaları kullanabileceğiniz Azure IOT hub'ı temelleri gibi [cihaz ikizi] [ lnk-devtwin] ve [doğrudan yöntemler][lnk-c2dmethod], uzaktan başlatın ve izleme cihazda yönetim eylemleri. Bu öğretici, bir arka uç uygulaması ve bir cihaz uygulaması birlikte nasıl başlatmak ve IOT hub'ı kullanarak uzak cihazı yeniden başlatma izlemek için çalışabileceğini gösterir.

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Bulutta bir arka uç uygulamasından (örneğin, yeniden başlatma, Fabrika sıfırlaması ve üretici yazılımı güncelleştirmesi) cihaz yönetim eylemleri başlatmak için bir doğrudan yöntem kullanın. Cihaz için sorumludur:

* IOT Hub'ından gönderilen yöntemi istek işleme.
* Cihazdaki ilgili cihaza özgü eylemi başlatılıyor.
* Durum güncelleştirmeleri aracılığıyla sağlama *bildirilen özellikler* IOT hub'ına.

Bulutta bir arka uç uygulaması, cihaz yönetim eylemleri, ilerleme üzerinde rapor için cihaz çifti sorguları çalıştırmak için kullanabilirsiniz.

[lnk-devtwin]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
