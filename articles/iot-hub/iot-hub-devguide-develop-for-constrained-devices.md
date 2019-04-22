---
title: Azure IOT hub'ı geliştirmek için IOT Hub C SDK'sını kullanarak cihazları kısıtlı | Microsoft Docs
description: Geliştirici Kılavuzu - kısıtlı cihazlar için Azure IOT SDK'ları kullanarak geliştirme hakkında yönergeler.
author: yzhong94
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: yizhon
ms.openlocfilehash: 7788bca621a59ec8cdfe36edf73a99efca8c460c
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59261403"
---
# <a name="develop-for-constrained-devices-using-azure-iot-c-sdk"></a>Azure IOT C SDK'sını kullanarak kısıtlanmış cihazlar için geliştirme

Azure IOT Hub C SDK'sı, C'de ANSI çeşitli platformlarda küçük disk ve bellek Ayak izi ile çalışmak için uygun hale getiren (C99) yazılır. Önerilen RAM Miktarı, en az 64 KB olmakla birlikte kullanılan protokol, açılan bağlantı sayısını, olarak hedeflenen platformun tam bellek Ayak izi bağlıdır.
> [!NOTE]
> * Azure IOT C SDK'sı ile geliştirmeye yardımcı olmak için kaynak tüketimi bilgilerini düzenli olarak yayımlar.  Lütfen bizim [GitHub deposu](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/c_sdk_resource_information.md) ve en son Kıyaslama gözden geçirin.
>

C SDK'sı MBED apt-get ve NuGet paket formundan kullanıma sunulmuştur. Kısıtlanmış cihazları hedeflemek için hedef platform için yerel SDK'sını derleme isteyebilirsiniz. Bu belge, C SDK'sını kullanarak ayak izini daraltmak için belirli özellikleri kaldırmayı gösterilmiştir [cmake](https://cmake.org/). Ayrıca, bu belge, programlama modellerini kısıtlanmış cihazları ile çalışmak için en iyi ele alınmaktadır.

## <a name="building-the-c-sdk-for-constrained-devices"></a>Kısıtlanmış cihazlar için C SDK'yı oluşturma

Kısıtlanmış cihazlar için C SDK'sı oluşturun.

### <a name="prerequisites"></a>Önkoşullar

İzleyin [C SDK'sı Kurulum Kılavuzu](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) C SDK'yı derlemek için geliştirme ortamınızı hazırlamak için. Cmake ile derlemek için adıma geçmeden önce kullanılmayan funkce odebrat cmake bayrakları çağırabilirsiniz.

### <a name="remove-additional-protocol-libraries"></a>Ek protokol kitaplıkları kaldırın

C SDK'yı bugün beş protokollerini destekler: MQTT, WebSocket AMQPs, WebSocket ve HTTPS üzerinden AMQP üzerinden MQTT. Çoğu senaryoda bir istemci üzerinde çalışan bir veya iki protokolleri gerektirir, bu nedenle SDK'sından kullanmadığınız Protokolü kitaplığı kaldırabilirsiniz. Senaryonuz bulunabilir için uygun bir iletişim protokolü seçme hakkında ek bilgi [bir IOT Hub iletişim protokolü seçme](iot-hub-devguide-protocols.md). Örneğin, MQTT kısıtlanmış cihazlar için genellikle daha uygun olan basit bir protokoldür.

AMQP ve HTTP kitaplıkları aşağıdaki cmake komutunu kullanarak kaldırabilirsiniz:

```
cmake -Duse_amqp=OFF -Duse_http=OFF <Path_to_cmake>
```

### <a name="remove-sdk-logging-capability"></a>SDK'sı günlüğe kaydetme özelliğini Kaldır

C SDK'sı, kapsamlı boyunca hatalarını ayıklamaya yardımcı olmak için oturum sağlar. Aşağıdaki cmake komutu kullanarak üretim cihazlar için günlüğe kaydetme özelliğini kaldırabilirsiniz:

```
cmake -Dno_logging=OFF <Path_to_cmake>
```

### <a name="remove-upload-to-blob-capability"></a>Kaldırma yeteneği BLOB karşıya yükleme

Yerleşik yetenek SDK'yı kullanarak Azure Depolama'ya büyük dosyaları karşıya yükleyebilirsiniz. Azure IOT Hub, ilişkili Azure depolama hesabınız için bir dağıtıcı olarak görev yapar. Bu özellik, ortam dosyaları ve büyük telemetri toplu günlükleri göndermek için kullanabilirsiniz. Daha fazla bilgi edinebilirsiniz [IOT Hub ile dosyaları karşıya](iot-hub-devguide-file-upload.md). Bu işlev uygulamanızı gerektirmiyorsa, bu özellik aşağıdaki cmake komutunu kullanarak kaldırabilirsiniz:

```
cmake -Ddont_use_uploadtoblob=ON <Path_to_cmake>
```

### <a name="running-strip-on-linux-environment"></a>Linux ortamında çalışan Şerit

İkili dosyalarınızı Linux sisteminde çalıştırırsanız, yararlanabileceğiniz [Şerit komut](https://en.wikipedia.org/wiki/Strip_(Unix)) derleme sonra son uygulama boyutunu azaltmak için.

```
strip -s <Path_to_executable>
```

## <a name="programming-models-for-constrained-devices"></a>Kısıtlanmış cihazlar için programlama modelleri

Ardından, programlama modellerini kısıtlanmış cihazlar için en arayın.

### <a name="avoid-using-the-serializer"></a>Seri hale getirici kullanmaktan kaçının

C SDK'sı isteğe sahip [C SDK'sı: seri hale getirici](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer), yöntemleri ve cihaz ikizi özelliklerini tanımlamak için bildirim temelli eşleme tabloları kullanmanızı sağlar. Seri hale getirici geliştirme basitleştirmek için tasarlanmış ancak ek yükü, kısıtlanmış cihazlar için en uygun değil ekler. Bu durumda, ilkel istemci API'leri kullanmayı göz önünde bulundurun ve basit bir ayrıştırıcı kullanarak JSON Ayrıştır [parson](https://github.com/kgabis/parson).

### <a name="use-the-lower-layer-ll"></a>Daha düşük katman (_LL_)

C SDK'sı, iki programlama modeli destekler. Bir ayarlanmış API'leri ile bir _LL_ infix, daha düşük katman için hangi anlamına gelir. Bu API kümesi açık ağırlık ve çalışan iş parçacıklarını, yani kullanıcı gereken el ile denetim zamanlama döndürme değil. Örneğin, cihaz istemcisi için _LL_ bu API'leri bulunabilir [üstbilgi dosyası](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_device_client_ll.h). 

Başka bir dizi API _LL_ dizini, burada iş parçacığı otomatik olarak hazırladık kolaylık katman çağrılır. Örneğin, aygıt istemcileri için kolaylık katman API'leri bu bulunabilir [IOT cihaz istemcisi üstbilgi dosyası](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_device_client.h). Her ek iş parçacığı önemli sistem kaynakları yüzdesi bulabilir burada kısıtlanmış cihazları göz önünde bulundurun için kullanarak _LL_ API'leri.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT C SDK'sı mimarisi hakkında daha fazla bilgi için:
-   [Azure IOT C SDK'sı kaynak kodu](https://github.com/Azure/azure-iot-sdk-c/)
-   [Azure IOT cihaz SDK'sını C giriş](iot-hub-device-sdk-c-intro.md)
