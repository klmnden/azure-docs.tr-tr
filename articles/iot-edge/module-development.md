---
title: Modülleri geliştirmek için Azure IOT kenar | Microsoft Docs
description: Azure IOT köşesi özel modüller oluşturmayı öğrenin
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: dbbd07e93602855afb0c9755e8872e0b46557611
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37030028"
---
# <a name="understand-the-requirements-and-tools-for-developing-iot-edge-modules"></a>IOT kenar modülleri geliştirmek için Araçlar ve gereksinimleri anlama

Bu makalede, IOT kenar modülü olarak çalışan uygulamaları yazarken hangi işlevlerin kullanılabilir ve bunları yararlanmak nasıl açıklanmaktadır.

## <a name="iot-edge-runtime-environment"></a>IOT kenar çalışma zamanı ortamı
IOT kenar çalışma zamanı birden fazla IOT kenar modülü işlevselliğini tümleştirmek ve IOT sınır cihazları dağıtmak için altyapı sağlar. Yüksek bir düzeyde herhangi bir programı IOT kenar modülü olarak paketlenebilir. Ancak, iletişim ve yönetim işlevlerini IOT kenar tam anlamıyla yararlanabilmek için bir modüle çalışan bir program IOT kenar çalışma zamanı'nda tümleşik yerel IOT kenar hub bağlanabilir.

## <a name="using-the-iot-edge-hub"></a>Kenar IOT hub'ı kullanma
Kenar IOT hub'ı iki ana işlevleri sağlar: IOT Hub ve yerel iletişimler proxy.

### <a name="iot-hub-primitives"></a>IOT hub'ı temelleri
Modül bir IOT Hub görür analogously herkese açık bir cihaza, onun örneği:

* Özel ve ayrı gelen olan bir modül çiftine sahip [cihaz çifti] [ lnk-devicetwin] ve diğer modülü çiftlerini aygıtın;
* gönderebilmesi [cihaz bulut iletilerini][lnk-iothub-messaging];
* alabileceği [doğrudan yöntemleri] [ lnk-methods] özellikle kendi kimliğini hedeflenen.

Şu anda bir modül bulut-cihaz iletilerini veya dosya karşıya yükleme özelliğini kullanın.

Bir modülü yazarken, yalnızca kullanabilirsiniz [Azure IOT cihaz SDK'sı] [ lnk-devicesdk] kenar IOT hub'ına bağlanmak ve IOT Hub cihaz uygulamasıyla kullanırken yaptığınız gibi yukarıdaki işlevselliği kullanmak için yalnızca fark, uygulama arka ucunu, cihaz kimliği yerine modül kimliği başvurmak olduğunuz, bırakılıyor.

Bkz: [geliştirme ve bir sanal cihaz IOT kenar modülü dağıtma] [ lnk-tutorial2] cihaz bulut iletilerini gönderir ve modül twin kullanan bir modül uygulama örneği.

### <a name="device-to-cloud-messages"></a>Cihazdan buluta iletiler
Etkinleştirmek için karmaşık cihaz-bulut iletileri işleme IOT kenar hub tanımlayıcı iletilerinin modülleri arasında ve modülleri ve IOT hub'ı arasında yönlendirme sağlar.
Bu modülleri izlemesine izin verir ve işlem iletilerinin diğer modüller tarafından gönderilen ve bunları karmaşık ardışık düzen yayar.
Makaleyi [modül bileşimi] [ lnk-module-comp] yollar kullanılarak karmaşık işlem hatlarını modülleri oluşturma açıklanmaktadır.

Bir IOT kenar modülü normal bir IOT Hub cihaz uygulamasını farklı bir biçimde işlemek için kendi yerel IOT kenar hub tarafından yönlendirilirken cihaz bulut iletilerini alabilir.

IOT kenar hub yayar bildirim temelli yolların açıklandığı göre modülünüzün iletileri [modül bileşimi] [ lnk-module-comp] makalesi. Bir IOT kenar modülü geliştirirken, bu ileti ileti işleyicileri ayarlayarak öğreticide gösterildiği gibi alabilir [geliştirme ve bir sanal cihaz IOT kenar modülü dağıtma][lnk-tutorial2].

Yollar oluşturma işlemini basitleştirmek için IOT kenar modülü kavramı ekler *giriş* ve *çıkış* uç noktaları. Bir modül herhangi bir giriş belirtmeden yönlendirilmeden tüm cihaz bulut iletilerini alabilir ve cihaz bulut iletilerini herhangi bir çıktı belirtmeden gönderebilirsiniz.
Açık girişleri ve çıkışları, kullanarak yine de yönlendirme kurallarını anlamak daha basit hale getirir. Bkz: [modül bileşimi] [ lnk-module-comp] daha fazla bilgi yönlendirme kuralları ve modülleri için giriş ve çıkış uç noktaları.

Son olarak, cihaz bulut iletilerini kenar hub tarafından işlenen aşağıdaki sistem özellikleri ile damgalanmış olduklarından:

| Özellik | Açıklama |
| -------- | ----------- |
| $connectionDeviceId | İletiyi gönderen istemci aygıt kimliği |
| $connectionModuleId | İletiyi gönderen modülün modül kimliği |
| $inputName | Bu ileti aldı giriş. Boş olabilir. |
| $outputName | İleti göndermek için kullanılan çıkış. Boş olabilir. |

### <a name="connecting-to-iot-edge-hub-from-a-module"></a>Bir modülden kenar IOT hub'ına bağlama
Bir modülden yerel kenar IOT hub'ına bağlanan iki adımı içerir: modülünüzün başladığında IOT kenar çalışma zamanı ve emin olun, uygulamanızın cihazdaki IOT kenar hub tarafından sunulan sertifika kabul sağlanan bağlantı dizesi kullanın.

Kullanılacak bağlantı dizesini ortam değişkeninde IOT kenar çalışma zamanı tarafından eklenen `EdgeHubConnectionString`. Bunu kullanmak istediği herhangi bir programı için kullanılabilir kılar.

Dizinin yolu ortam değişkeninde kullanılabilir bir dosyada IOT kenar çalışma zamanı tarafından IOT kenar hub bağlantısı doğrulamak için kullanılacak sertifikayı analogously, eklenen `EdgeModuleCACertificateFile`.

Öğretici [geliştirme ve bir sanal cihaz IOT kenar modülü dağıtma] [ lnk-tutorial2] sertifika modülü uygulamanızda makine deposunda olduğundan emin olun gösterilmektedir. Sertifika çalışan kullanarak bağlantı güvenmeyi Açıkçası, başka bir yöntem.

## <a name="packaging-as-an-image"></a>Bir görüntü olarak paketleme
IOT kenar modülleri Docker görüntü olarak paketlenir.
Öğreticide gösterildiği gibi doğrudan Docker araç zinciri ya da Visual Studio Code kullanabilirsiniz [geliştirme ve bir sanal cihaz IOT kenar modülü dağıtma][lnk-tutorial2].

## <a name="next-steps"></a>Sonraki adımlar

Bir modül geliştirmek sonra öğrenin nasıl [dağıtma ve izleme IOT kenar modülleri ölçekte][lnk-howto-deploy].

[lnk-devicesdk]: ../iot-hub/iot-hub-devguide-sdks.md
[lnk-devicetwin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-iothub-messaging]: ../iot-hub/iot-hub-devguide-messaging.md
[lnk-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-tutorial2]: tutorial-csharp-module.md
[lnk-module-comp]: module-composition.md
[lnk-howto-deploy]: how-to-deploy-monitor.md
