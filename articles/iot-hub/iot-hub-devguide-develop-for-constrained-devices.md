---
title: Azure IOT hub'ı kısıtlanmış cihazlar için geliştirme | Microsoft Docs
description: Geliştirici Kılavuzu - kısıtlı cihazlar için Azure IOT SDK'ları kullanarak geliştirme hakkında yönergeler.
services: iot-hub
documentationcenter: c
author: yzhong94
manager: timlt
editor: ''
ms.assetid: 979136db-c92d-4288-870c-f305e8777bdd
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/24/2018
ms.author: yizhon
ms.openlocfilehash: 15fc5794c71428b5fb1036060af3e9c4a6890f4f
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38969071"
---
# <a name="develop-for-constrained-devices-using-azure-iot-sdks"></a>Azure IOT SDK'larını kullanarak kısıtlanmış cihazlar için geliştirme

## <a name="develop-using-azure-iot-hub-c-sdk"></a>Azure IOT Hub C SDK'sını kullanarak geliştirme
Azure IOT Hub C SDK'sı, C'de ANSI çeşitli platformlarda küçük disk ve bellek Ayak izi ile çalışmak için uygun hale getiren (C99) yazılır.  Önerilen RAM Miktarı, en az 64 KB olmakla birlikte kullanılan protokol, açılan bağlantı sayısını, olarak hedeflenen platformun tam bellek Ayak izi bağlıdır.

C SDK'sı MBED apt-get ve NuGet paket formundan kullanıma sunulmuştur.  Kısıtlanmış cihazları hedeflemek için hedef platform için yerel SDK'sını derleme isteyebilirsiniz. Bu belge, C SDK'sını kullanarak ayak izini daraltmak için belirli özellikleri kaldırmayı gösterilmiştir [cmake][lnk-cmake].  Ayrıca, bu belge, programlama modellerini kısıtlanmış cihazları ile çalışmak için en iyi ele alınmaktadır.

### <a name="building-the-c-sdk-for-constrained-devices"></a>Kısıtlanmış cihazlar için C SDK'yı oluşturma
#### <a name="prerequisites"></a>Önkoşullar
İzleyin [Kurulum Kılavuzu] [ lnk-devbox-setup] C SDK'yı derlemek için geliştirme ortamınızı hazırlamak için.  Cmake ile derlemek için adıma geçmeden önce kullanılmayan funkce odebrat cmake bayrakları çağırabilirsiniz.

#### <a name="remove-additional-protocol-libraries"></a>Ek protokol kitaplıkları kaldırın
C SDK'sı beş protokolünü bugün destekler: MQTT, WebSocket AMQPs, WebSocket ve HTTPS üzerinden AMQP üzerinden MQTT.    Çoğu senaryoda bir istemci üzerinde çalışan bir veya iki protokolleri gerektirir, bu nedenle SDK'sından kullanmadığınız Protokolü kitaplığı kaldırabilirsiniz.  Senaryonuz bu bulunabilir, uygun bir iletişim protokolü seçme hakkında ek bilgi [belge][lnk-choosing-protocol].  Örneğin, MQTT kısıtlanmış cihazlar için genellikle daha uygun olan basit bir protokoldür.

AMQP ve HTTP kitaplıkları aşağıdaki cmake komutunu kullanarak kaldırabilirsiniz:
```
cmake -Duse_amqp=OFF -Duse_http=OFF <Path_to_cmake>
```

#### <a name="remove-sdk-logging-capability"></a>SDK'sı günlüğe kaydetme özelliğini Kaldır
C SDK'sı, kapsamlı boyunca hatalarını ayıklamaya yardımcı olmak için oturum sağlar. Aşağıdaki cmake komutu kullanarak üretim cihazlar için günlüğe kaydetme özelliğini kaldırabilirsiniz:
```
cmake -Dno_logging=OFF <Path_to_cmake>
```

#### <a name="remove-upload-to-blob-capability"></a>Kaldırma yeteneği BLOB karşıya yükleme
Yerleşik yetenek SDK'yı kullanarak Azure Depolama'ya büyük dosyaları karşıya yükleyebilirsiniz.  Azure IOT Hub, ilişkili Azure depolama hesabınız için bir dağıtıcı olarak görev yapar.  Bu özellik, ortam dosyaları ve büyük telemetri toplu günlükleri göndermek için kullanabilirsiniz.  Bu IOT Hub ile dosyaları karşıya hakkında daha fazla bilgi edinin [belge][lnk-hub-file-upload].  Bu işlev uygulamanızı gerektirmiyorsa, bu özellik aşağıdaki cmake komutunu kullanarak kaldırabilirsiniz:
```
cmake -Ddont_use_uploadtoblob=ON <Path_to_cmake>
```
#### <a name="running-strip-on-linux-environment"></a>Linux ortamında çalışan Şerit
İkili dosyalarınızı Linux sisteminde çalıştırırsanız, yararlanabileceğiniz [Şerit komut] [ lnk-strip] derleme sonra son uygulama boyutunu azaltmak için.
```
strip -s <Path_to_executable>
```

### <a name="programming-models-for-constrained-devices"></a>Kısıtlanmış cihazlar için programlama modelleri
#### <a name="avoid-using-the-serializer"></a>Seri hale getirici kullanmaktan kaçının
C SDK'sı isteğe sahip [seri hale getirici][lnk-serializer], yöntemleri ve cihaz ikizi özelliklerini tanımlamak için bildirim temelli eşleme tabloları kullanmanızı sağlar.  Seri hale getirici geliştirme basitleştirmek için tasarlanmış ancak ek yükü, kısıtlanmış cihazlar için en uygun değil ekler.  Bu durumda, ilkel istemci API'leri kullanmayı göz önünde bulundurun ve basit bir ayrıştırıcı kullanarak json Ayrıştır [parson][lnk-parson].

#### <a name="use-the-lower-layer-ll"></a>Daha düşük katman (_LL_)
C SDK'sı, iki programlama modeli destekler.  Bir ayarlanmış API'leri ile bir _LL_ infix, daha düşük katman için hangi anlamına gelir.  Bu API kümesi açık ağırlığı olan ve kullanıcı el ile zamanlama denetimi gerekir yani çalışan iş parçacıklarını, döndürme değil.  Örneğin, cihaz istemcisi için _LL_ bu API'leri bulunabilir [üstbilgi dosyası](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_device_client_ll.h).  Başka bir dizi API _LL_ dizini, burada iş parçacığı otomatik olarak hazırladık kolaylık katman çağrılır.  Örneğin, aygıt istemcileri için kolaylık katman API'leri bu bulunabilir [üstbilgi dosyası](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_device_client.h).  Her ek iş parçacığı önemli sistem kaynakları yüzdesi bulabilir burada kısıtlanmış cihazları göz önünde bulundurun için kullanarak _LL_ API'leri.

## <a name="next-steps"></a>Sonraki adımlar
Azure IOT C SDK'sı mimarisi hakkında daha fazla bilgi için:
-   [Azure IOT C SDK'sı kaynak kodu](https://github.com/Azure/azure-iot-sdk-c/)
-   [Azure IOT cihaz SDK'sını C giriş](https://docs.microsoft.com/azure/iot-hub/iot-hub-device-sdk-c-intro)

------
[lnk-cmake]: https://cmake.org/
[lnk-devbox-setup]:  https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-choosing-protocol]: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-protocols
[lnk-hub-file-upload]: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-file-upload
[lnk-strip]: https://en.wikipedia.org/wiki/Strip_(Unix)
[lnk-serializer]: https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer
[lnk-parson]: https://github.com/kgabis/parson