---
title: İçin Azure IOT Edge modülleri geliştirme | Microsoft Docs
description: Azure IOT Edge çalışma zamanı ve IOT Hub ile iletişim kurabilen için modüllerinizi geliştirebilir.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 02/25/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 5285490ca1a27494cbcd3ea3d6527b78c7d38c8c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65833432"
---
# <a name="develop-your-own-iot-edge-modules"></a>Kendi IOT Edge modülleri geliştirin

Azure IOT Edge modülleri, diğer Azure Hizmetleri ile bağlanma ve daha büyük bulut veri ardışık düzeninize katkıda. Bu makalede, IOT Edge çalışma zamanı ve IOT Hub ve Azure bulut geri kalanı ile bu nedenle iletişim kurmak için modülleri nasıl geliştirebileceğinizi açıklanır. 

## <a name="iot-edge-runtime-environment"></a>IOT Edge çalışma zamanı ortamı
IOT Edge çalışma zamanı birden çok IOT Edge modülleri işlevlerini tümleştirin ve bunları IOT Edge cihazları dağıtmak için altyapı sağlar. Yüksek düzeyde, bir IOT Edge modülü herhangi bir programı paketlenebilir. Bununla birlikte, iletişimi ve yönetim işlevlerini IOT Edge tüm avantajlarından yararlanmak için bir modülde çalışan bir program IOT Edge çalışma zamanı'nda tümleşik yerel IOT Edge hub'ına bağlanabilirsiniz.

## <a name="using-the-iot-edge-hub"></a>IOT Edge hub'ı kullanma
IOT Edge hub'ı iki ana işlevleri sağlar: IOT hub'ı ve yerel iletişimler proxy.

### <a name="iot-hub-primitives"></a>IOT hub'ı temelleri
IOT hub'ı öğesine anlamında bir cihaz için bir modül örneğinin görür:

* ayrı ve ayrı olan bir modül ikizi sahip [cihaz ikizi](../iot-hub/iot-hub-devguide-device-twins.md) ve bir modül ikizlerini aygıtın;
* gönderebilmesi [CİHAZDAN buluta iletileri](../iot-hub/iot-hub-devguide-messaging.md);
* alabileceği [doğrudan yöntemler](../iot-hub/iot-hub-devguide-direct-methods.md) kimliğini özel olarak hedeflenen.

Şu anda, bir modül bulut-cihaz iletilerini almak ve karşıya dosya yükleme özelliği kullanın.

Bir modülü yazarken, kullanabileceğiniz [Azure IOT cihaz SDK'sı](../iot-hub/iot-hub-devguide-sdks.md) IOT Edge hub'ına bağlanmak ve tek fark, uygulamanızdan olan bir cihaz uygulaması ile IOT hub'ı kullanırken yaptığınız gibi yukarıdaki işlevselliği kullanmak için arka uç, modül kimliği yerine cihaz kimliği başvurmak zorunda.

### <a name="device-to-cloud-messages"></a>Cihazdan buluta iletiler
Etkinleştirmek için CİHAZDAN buluta iletileri işleme karmaşık IOT Edge hub'ı bildirim temelli modülleri ve IOT hub'ı arasında ve modüller arasında iletileri yönlendirme sağlar. Bildirim temelli yönlendirme modülleri izlemesine izin verir ve işlem iletileri diğer modüller tarafından gönderilen ve bunları karmaşık komut zincirlerinde yayar. Daha fazla bilgi için [modülleri dağıtma ve IOT Edge'de yollar kurmak](module-composition.md).

IOT Edge modülü, normal bir IOT Hub cihaz uygulaması aksine, bunları işlemek için kendi yerel IOT Edge hub tarafından proxy gönderildiğini CİHAZDAN buluta iletiler alabilir.

IOT Edge hub'ı modülünüzde açıklanan bildirim temelli rota tabanlı iletileri yayar [dağıtım bildirimi](module-composition.md). IOT Edge modülü geliştirirken, ileti işleyicileri ayarlayarak bu iletiler alabilir.

Yollar oluşturulmasını kolaylaştırmak için IOT Edge modülü kavramını ekler *giriş* ve *çıkış* uç noktaları. Bir modül için herhangi bir giriş belirtmeden yönlendirilen tüm CİHAZDAN buluta iletiler alabilir ve herhangi bir çıktı belirtmeden CİHAZDAN buluta iletileri gönderebilir. Açık girişler ve çıkışlar, kullanarak yine de Yönlendirme kuralları anlamak daha basit hale getirir. 

Son olarak, cihaz-bulut iletileri Edge hub'ı tarafından işlenen, aşağıdaki sistem özellikleri ile içerdiği:

| Özellik | Açıklama |
| -------- | ----------- |
| $connectionDeviceId | İletiyi gönderen istemci cihaz kimliği |
| $connectionModuleId | İletiyi gönderen modülün modül kimliği |
| $inputName | Bu ileti alındı. giriş. Boş olabilir. |
| $outputName | İleti göndermek için kullanılan çıkış. Boş olabilir. |

### <a name="connecting-to-iot-edge-hub-from-a-module"></a>Bir modülden IOT Edge hub'ına bağlama
Bir modülden yerel IOT Edge hub'ına bağlanan iki adımdan oluşur: 
1. Uygulamanızda ModuleClient örneği oluşturun.
2. Bu cihazın IOT Edge hub tarafından sunulan sertifika, uygulamanızın kabul ettiğinden emin olun.

Modülünüzün nasıl DeviceClient örnekleri IOT cihazları IOT Hub'ına bağlanmak için benzer cihaz üzerinde çalışan IOT Edge hub'ına bağlanmak için ModuleClient örneği oluşturun. Tercih ettiğiniz SDK dili için API Başvurusu ModuleClient sınıfı ve kendi iletişim yöntemleri hakkında daha fazla bilgi için bkz: [C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet), [C ve Python](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-h), [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.moduleclient?view=azure-java-stable), veya [Node.js](https://docs.microsoft.com/javascript/api/azure-iot-device/moduleclient?view=azure-node-latest).


## <a name="next-steps"></a>Sonraki adımlar

[IOT Edge için geliştirme ve test ortamı hazırlama](development-environment.md)

[Geliştirmek için Visual Studio kullanma C# IOT Edge modülleri](how-to-visual-studio-develop-module.md)

[Visual Studio Code için IOT Edge modülleri geliştirmek için kullanın](how-to-vs-code-develop-module.md)

