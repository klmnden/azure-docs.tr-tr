---
author: wesmc7777
ms.author: wesmc
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: 72ccad94301e053d8103ca949d41202e58d9f5bb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66146520"
---
> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-twin-getstarted.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-twin-getstarted.md)
> * [Python](../articles/iot-hub/iot-hub-python-twin-getstarted.md)

Cihaz çiftleri, cihaz durumu bilgilerini (meta veriler, yapılandırmalar ve koşullar) depolayan JSON belgelerdir. IOT hub'ı ona bağlanan her cihaz için cihaz ikizi'ni kalıcıdır.

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Cihaz ikizlerini kullanın:

* Çözüm arka ucunuz cihaz meta verilerini Store.

* Geçerli durum bilgilerini kullanılabilir yetenekler ve koşullar (örneğin, kullanılan bağlantı yöntemi) gibi cihaz uygulamanızdan bildirin.

* Bir cihaz uygulaması ve arka uç uygulaması arasında uzun süre çalışan iş akışları (örneğin, üretici yazılımı ve yapılandırma güncelleştirmelerini) durumunu eşitleyin.

* Cihaz meta verilerini, yapılandırma ya da durum sorgulayın.

Cihaz çiftleri, cihaz yapılandırmaları ve Koşulları'nı sorgulamak için ve eşitleme için tasarlanmıştır. Cihaz ikizlerini kullanma zamanı hakkında daha fazla bilgi bulunabilir [cihaz ikizlerini anlama](../articles/iot-hub/iot-hub-devguide-device-twins.md).

Cihaz çiftleri, IOT hub'ı depolanır ve içerir:

* *etiketleri*, cihaz meta verilerini yalnızca çözüm arka ucu tarafından; erişilebilir

* *İstenen özellikleri*, JSON nesneleri çözüm tarafından değiştirilebilir, cihaza uygulama tarafından son ve observable geri ve

* *bildirilen özellikler*, JSON nesneleri cihaz uygulaması tarafından değiştirilebilir ve çözüm arka ucu tarafından okunabilir. Etiketler ve Özellikler diziler içeremez, ancak nesneler yuvalanabilir.

![Cihaz ikizi görüntü gösterme işlevi](./media/iot-hub-selector-twin-get-started/twin.png)

Ayrıca, çözüm arka ucu, cihaz çiftleri yukarıdaki tüm verilere dayalı sorgulayabilirsiniz.
Başvurmak [cihaz ikizlerini anlama](../articles/iot-hub/iot-hub-devguide-device-twins.md) ve cihaz ikizleri hakkında daha fazla bilgi için [IOT Hub sorgu dili](../articles/iot-hub/iot-hub-devguide-query-language.md) sorgulama için başvuru.


Bu öğretici şunların nasıl yapıldığını gösterir:

* Ekleyen bir arka uç uygulaması oluşturma *etiketleri* bir cihaz çifti ve kendi bağlantı kanalı olarak raporlar bir sanal cihaz uygulaması için bir *özelliği bildirilen* cihaz ikizinde üzerinde.

* Etiketleri ve daha önce oluşturduğunuz özellikleri filtreleri kullanarak arka uç uygulamanızın cihazlardan sorgulayın.
