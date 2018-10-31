---
title: İçin Azure IOT Edge modülleri geliştirme | Microsoft Docs
description: Azure IOT Edge için özel modüller oluşturmayı öğrenin
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 761485de4bf52b7261ac8f1f8c3d937486c66546
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50248009"
---
# <a name="understand-the-requirements-and-tools-for-developing-iot-edge-modules"></a>IOT Edge modülleri geliştirmek için Araçlar ve gereksinimleri anlama

Bu makalede, IOT Edge modülü çalışan uygulamaları yazarken hangi işlevleri kullanılabilir ve bunlardan yararlanmak nasıl açıklanmaktadır.

## <a name="iot-edge-runtime-environment"></a>IOT Edge çalışma zamanı ortamı
IOT Edge çalışma zamanı birden çok IOT Edge modülleri işlevlerini tümleştirin ve bunları IOT Edge cihazları dağıtmak için altyapı sağlar. Yüksek düzeyde, bir IOT Edge modülü herhangi bir programı paketlenebilir. Bununla birlikte, iletişimi ve yönetim işlevlerini IOT Edge tüm avantajlarından yararlanmak için bir modülde çalışan bir program IOT Edge çalışma zamanı'nda tümleşik yerel IOT Edge hub'ına bağlanabilirsiniz.

## <a name="using-the-iot-edge-hub"></a>IOT Edge hub'ı kullanma
IOT Edge hub'ı iki ana işlevleri sağlar: IOT hub'ı ve yerel iletişimler proxy.

### <a name="iot-hub-primitives"></a>IOT hub'ı temelleri
Modül bir IOT hub'ı görür öğesine anlamında bir cihaza BT'nin örneği:

* ayrı ve ayrı olan bir modül ikizi sahip [cihaz ikizi](../iot-hub/iot-hub-devguide-device-twins.md) ve bir modül ikizlerini aygıtın;
* gönderebilmesi [CİHAZDAN buluta iletileri](../iot-hub/iot-hub-devguide-messaging.md);
* alabileceği [doğrudan yöntemler](../iot-hub/iot-hub-devguide-direct-methods.md) kimliğini özel olarak hedeflenen.

Şu anda, bir modül bulut-cihaz iletilerini almak ve karşıya dosya yükleme özelliği kullanın.

Bir modülü yazarken, yalnızca kullanabilirsiniz [Azure IOT cihaz SDK'sı](../iot-hub/iot-hub-devguide-sdks.md) IOT Edge hub'ına bağlanmak ve tek fark, gelen olan bir cihaz uygulaması ile IOT hub'ı kullanırken yaptığınız gibi yukarıdaki işlevselliği kullanmak için Uygulama arka ucu, cihaz kimliği yerine modül kimliği başvurmak gerekir.

Bkz: [geliştirme ve IOT Edge modülü sanal cihazı dağıtma](tutorial-csharp-module.md) CİHAZDAN buluta iletiler gönderen ve modül ikizi kullanan bir modül uygulama örneği.

### <a name="device-to-cloud-messages"></a>Cihazdan buluta iletiler
Etkinleştirmek için CİHAZDAN buluta iletileri işleme karmaşık IOT Edge hub'ı bildirim temelli modülleri ve IOT hub'ı arasında ve modüller arasında iletileri yönlendirme sağlar.
Bu modüller izlemesine izin verir ve işlem iletileri diğer modüller tarafından gönderilen ve bunları karmaşık komut zincirlerinde yayar.
Makaleyi [modül bileşimi](module-composition.md) modülleri yolları kullanarak karmaşık komut zincirlerinde birleştirmeyi açıklar.

IOT Edge modülü, normal bir IOT Hub cihaz uygulaması farklı bunları işlemek için kendi yerel IOT Edge hub proxy gönderildiğini CİHAZDAN buluta iletiler alabilir.

IOT Edge hub'ı modülünüzde açıklanan bildirim temelli rota tabanlı iletileri yayar [modül bileşimi](module-composition.md) makalesi. IOT Edge modülü geliştirme, bu ileti ileti işleyicileri ayarlayarak öğreticide gösterilen şekilde alabilirsiniz [geliştirme ve IOT Edge modülü sanal cihazı dağıtma] [lnk-tutorial2].

Yollar oluşturmayı basitleştirmek için IOT Edge modülü kavramını ekler *giriş* ve *çıkış* uç noktaları. Bir modül için herhangi bir giriş belirtmeden yönlendirilen tüm CİHAZDAN buluta iletiler alabilir ve herhangi bir çıktı belirtmeden CİHAZDAN buluta iletileri gönderebilir.
Açık girişler ve çıkışlar, kullanarak yine de Yönlendirme kuralları anlamak daha basit hale getirir. Bkz: [modül bileşimi](module-composition.md) daha fazla bilgi yönlendirme kuralları ve modülleri için giriş ve çıkış uç noktaları için.

Son olarak, cihaz-bulut iletileri Edge hub'ı tarafından işlenen, aşağıdaki sistem özellikleri ile içerdiği:

| Özellik | Açıklama |
| -------- | ----------- |
| $connectionDeviceId | İletiyi gönderen istemci cihaz kimliği |
| $connectionModuleId | İletiyi gönderen modülün modül kimliği |
| $inputName | Bu ileti alındı. giriş. Boş olabilir. |
| $outputName | İleti göndermek için kullanılan çıkış. Boş olabilir. |

### <a name="connecting-to-iot-edge-hub-from-a-module"></a>Bir modülden IOT Edge hub'ına bağlama
Bir modülden yerel IOT Edge hub'ına bağlanma iki adımı kapsar: modülünüzde başladığında IOT Edge çalışma zamanı ve emin olun, cihazdaki IOT Edge hub'ı tarafından sunulan sertifika, uygulamanızın kabul sağlanan bağlantı dizesini kullanın.

IOT Edge çalışma zamanı ortam değişkeninde tarafından kullanılacak bağlantı dizesini eklenen `EdgeHubConnectionString`. Bu, kullanmak istediği herhangi bir programı kullanılmasını sağlar.

Öğesine, IOT Edge hub'ı bağlantıyı doğrulamak için kullanılacak sertifikayı IOT Edge çalışma zamanı'nda, path ortam değişkeninde kullanılabilir bir dosya tarafından eklenen `EdgeModuleCACertificateFile`.

Öğretici [geliştirme ve IOT Edge modülü sanal cihazı dağıtma] [lnk-tutorial2] sertifikayı makine deposuna modülü uygulamanızdaki olduğundan emin olmak nasıl gösterir. Bağlantıları kullanarak ilgili sertifikayı iş güvenmeyi Açıkçası, başka bir yöntem.

## <a name="packaging-as-an-image"></a>Bir görüntü olarak paketleme
IOT Edge modülleri, Docker görüntüleri paketlenir.
Öğreticide gösterilen gibi doğrudan Docker araç zincirinizi veya Visual Studio Code kullanabilirsiniz [geliştirme ve IOT Edge modülü sanal cihazı dağıtma] [lnk-tutorial2].

## <a name="next-steps"></a>Sonraki adımlar

Bir modülü geliştirme sonra bilgi nasıl [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor.md).

