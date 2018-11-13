---
title: İçin Azure IOT Edge modülleri geliştirme | Microsoft Docs
description: Azure IOT Edge için özel modüller oluşturmayı öğrenin
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: cb97e2cf6d554753f64afc76de84f43e38443909
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567239"
---
# <a name="understand-the-requirements-and-tools-for-developing-iot-edge-modules"></a>IOT Edge modülleri geliştirmek için Araçlar ve gereksinimleri anlama

Bu makalede, IOT Edge modülü çalışan uygulamaları yazarken hangi işlevleri kullanılabilir ve bunlardan yararlanmak nasıl açıklanmaktadır.

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

Bkz: [geliştirme ve IOT Edge modülü sanal cihazı dağıtma](tutorial-csharp-module.md) CİHAZDAN buluta iletiler gönderen ve modül ikizi kullanan bir modül uygulama örneği.

### <a name="device-to-cloud-messages"></a>Cihazdan buluta iletiler
Etkinleştirmek için CİHAZDAN buluta iletileri işleme karmaşık IOT Edge hub'ı bildirim temelli modülleri ve IOT hub'ı arasında ve modüller arasında iletileri yönlendirme sağlar. Bildirim temelli yönlendirme modülleri izlemesine izin verir ve işlem iletileri diğer modüller tarafından gönderilen ve bunları karmaşık komut zincirlerinde yayar. Makaleyi [modül bileşimi](module-composition.md) modülleri yolları kullanarak karmaşık komut zincirlerinde birleştirmeyi açıklar.

IOT Edge modülü, normal bir IOT Hub cihaz uygulaması aksine, bunları işlemek için kendi yerel IOT Edge hub tarafından proxy gönderildiğini CİHAZDAN buluta iletiler alabilir.

IOT Edge hub'ı modülünüzde açıklanan bildirim temelli rota tabanlı iletileri yayar [modül bileşimi](module-composition.md) makalesi. IOT Edge modülü geliştirirken, ileti işleyicileri ayarlayarak bu iletiler alabilir.

Yollar oluşturmayı basitleştirmek için IOT Edge modülü kavramını ekler *giriş* ve *çıkış* uç noktaları. Bir modül için herhangi bir giriş belirtmeden yönlendirilen tüm CİHAZDAN buluta iletiler alabilir ve herhangi bir çıktı belirtmeden CİHAZDAN buluta iletileri gönderebilir.
Açık girişler ve çıkışlar, kullanarak yine de Yönlendirme kuralları anlamak daha basit hale getirir. Daha fazla bilgi yönlendirme kuralları ve modülleri için giriş ve çıkış uç noktalar için bkz. [modül bileşimi](module-composition.md).

Son olarak, cihaz-bulut iletileri Edge hub'ı tarafından işlenen, aşağıdaki sistem özellikleri ile içerdiği:

| Özellik | Açıklama |
| -------- | ----------- |
| $connectionDeviceId | İletiyi gönderen istemci cihaz kimliği |
| $connectionModuleId | İletiyi gönderen modülün modül kimliği |
| $inputName | Bu ileti alındı. giriş. Boş olabilir. |
| $outputName | İleti göndermek için kullanılan çıkış. Boş olabilir. |

### <a name="connecting-to-iot-edge-hub-from-a-module"></a>Bir modülden IOT Edge hub'ına bağlama
Bir modülden yerel IOT Edge hub'ına bağlanan iki adımdan oluşur: 
1. Modülünüzün başladığında IOT Edge çalışma zamanı tarafından sağlanan bağlantı dizesi kullanın.
2. Bu cihazın IOT Edge hub tarafından sunulan sertifika, uygulamanızın kabul ettiğinden emin olun.

IOT Edge çalışma zamanı ortam değişkeninde tarafından kullanılacak bağlantı dizesini eklenen `EdgeHubConnectionString`. Bu, kullanmak istediği herhangi bir programı kullanılmasını sağlar.

Öğesine, IOT Edge hub'ı bağlantıyı doğrulamak için kullanılacak sertifikayı IOT Edge çalışma zamanı'nda, path ortam değişkeninde kullanılabilir bir dosya tarafından eklenen `EdgeModuleCACertificateFile`.

## <a name="next-steps"></a>Sonraki adımlar

Bir modülü geliştirme sonra bilgi nasıl [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor.md).

