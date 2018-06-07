---
title: Azure IOT Hub kısıtlı cihazlar için geliştirme | Microsoft Docs
description: Geliştirici Kılavuzu - kısıtlı cihazlar için Azure IOT SDK'ları kullanarak geliştirme konusunda yönergeler.
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
ms.openlocfilehash: 62065b78e3f8191c6423ba9dba4a8f7d16fad114
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655571"
---
# <a name="develop-for-constrained-devices-using-azure-iot-sdks"></a>Azure IOT SDK'ları kullanarak kısıtlı cihazlar için geliştirme

## <a name="develop-using-azure-iot-hub-c-sdk"></a>Azure IOT Hub C SDK'sını kullanarak geliştirme
Azure IOT Hub C SDK'sı ANSI çeşitli platformlarda küçük disk ve bellek kaplama alanı ile çalışmak için oldukça uygun hale getiren C (C99) yazılır.  Önerilen RAM Miktarı, en az 64 KB olmakla birlikte kullanılan protokol, açılan bağlantı sayısı, yanı sıra üzerinde hedeflenen platform tam bellek alanını bağlıdır.

C SDK paketi formundan get apt, NuGet ve MBED kullanılabilir.  Kısıtlanmış cihazları hedeflemek için yerel olarak hedef platformunuz için SDK'sı oluşturmak isteyebilirsiniz. Bu belge, C SDK'sını kullanarak ayak daraltmak için belirli özellikleri kaldırmayı gösterilmiştir [cmake][lnk-cmake].  Ayrıca, bu belge kısıtlı cihazları ile çalışmak için modelleri programlama en iyi uygulama anlatılmaktadır.

### <a name="building-the-c-sdk-for-constrained-devices"></a>Kısıtlı cihazlar için C SDK'sı oluşturma
#### <a name="prerequisites"></a>Önkoşullar
Bu izleyin [kurulum kılavuzunu] [ lnk-devbox-setup] C SDK'sı oluşturmak için geliştirme ortamınızı hazırlamak için.  Cmake ile oluşturmak için adım ulaşmadan kullanılmayan özellikleri kaldırmak için cmake bayrakları çağırabilirsiniz.

#### <a name="remove-additional-protocol-libraries"></a>Ek protokol kitaplıkları kaldırın
C SDK beş protokollerini bugün destekler: MQTT, MQTT WebSocket, AMQPs, WebSocket ve HTTPS üzerinden AMQP üzerinden.    Çoğu senaryoda bir istemci üzerinde çalışan bir veya iki protokolleri gerektirir, bu nedenle SDK kullanmadığınız Protokolü kitaplığı kaldırabilirsiniz.  Senaryonuz bu bulunabilir uygun iletişim protokolü seçme hakkında ek bilgi [belge][lnk-choosing-protocol].  Örneğin, MQTT genellikle daha kısıtlı cihazlar için uygun olan basit bir protokoldür.

Aşağıdaki cmake komutunu kullanarak AMQP ve HTTP kitaplıkları kaldırabilirsiniz:
```
cmake -Duse_amqp=OFF -Duse_http=OFF <Path_to_cmake>
```

#### <a name="remove-sdk-logging-capability"></a>SDK günlük özelliğini Kaldır
C SDK'sı kapsamlı boyunca hata ayıklamaya yardımcı olmak için günlüğe kaydedilmesini sağlar. Aşağıdaki cmake komutu kullanarak üretim cihazlar için günlüğe kaydetme özelliğine kaldırabilirsiniz:
```
cmake -Dno_logging=OFF <Path_to_cmake>
```

#### <a name="remove-upload-to-blob-capability"></a>Yetenek BLOB karşıya yükleme Kaldır
Azure SDK'ın yerleşik yetenek kullanarak depolama için büyük dosyaları karşıya yükleyebilir.  Azure IOT Hub ile ilişkili bir Azure depolama hesabı için bir dağıtıcı olarak görev yapar.  Medya dosyalarını, büyük telemetri toplu işlemleri ve günlükleri göndermek için bu özelliği kullanın.  Bu IOT Hub ile dosyaları karşıya hakkında daha fazla bilgi edinin [belge][lnk-hub-file-upload].  Uygulamanız bu işlevsellik gerektirmiyorsa aşağıdaki cmake komutu kullanarak bu özelliği kaldırabilirsiniz:
```
cmake -Ddont_use_uploadtoblob=ON <Path_to_cmake>
```
#### <a name="running-strip-on-linux-environment"></a>Linux ortamda çalışan Şerit
İkili dosyaları Linux sisteminde çalıştırırsanız yararlanabileceğiniz [Şerit komutu] [ lnk-strip] derleme sonra son uygulama boyutunu azaltmak için.
```
strip -s <Path_to_executable>
```

### <a name="programming-models-for-constrained-devices"></a>Programlama modelleri kısıtlı cihazlar için
#### <a name="avoid-using-the-serializer"></a>Seri hale getirici kullanmaktan kaçının
C SDK'sı isteğe sahip [seri hale getirici][lnk-serializer], yöntemleri ve cihaz çifti özelliklerini tanımlamak için bildirim temelli eşleme tabloları kullanın sağlar.  Seri hale getirici geliştirme kolaylaştırmak için tasarlanmıştır ancak ek yükü, kısıtlı cihazlar için en iyi olmayan ekler.  Bu durumda, ilkel istemci API'ları kullanmayı düşünün ve basit bir Ayrıştırıcıyı kullanarak json ayrıştırılamadı [parson][lnk-parson].

#### <a name="use-the-lower-layer-ll"></a>Alt Katman kullanın (_ÜM_)
C SDK'sı iki programlama modeli destekler.  Bir ayarlanmış API'leri kullanılarak bir _ÜM_ infix, alt katman için hangi anlamına gelir.  Bu API kümesi açık ağırlığı olan ve çalışan iş parçacıklarını, kullanıcının zamanlama el ile denetim gerekir yani Döndür değil.  Örneğin, aygıt istemcisi _ÜM_ bu API'leri bulunabilir [üstbilgi dosyası](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_device_client_ll.h).  Başka bir dizi API _ÜM_ dizin adlı kolaylık katman burada bir çalışan iş parçacığı otomatik olarak başlar.  Örneğin, aygıt istemcileri için kullanışlı katman API bu bulunabilir [üstbilgi dosyası](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_device_client.h).  Burada her ek iş parçacığı alabilir önemli sistem kaynakları yüzdesi kısıtlanmış aygıtları göz önünde bulundurun için kullanarak _ÜM_ API'leri.

## <a name="next-steps"></a>Sonraki adımlar
Azure IOT C SDK mimarisi hakkında daha fazla bilgi için:
-   [Azure IOT C SDK'sı kaynak kodu](https://github.com/Azure/azure-iot-sdk-c/)
-   [Azure IOT cihaz SDK'sı C giriş](https://docs.microsoft.com/azure/iot-hub/iot-hub-device-sdk-c-intro)

------
[lnk-cmake]: https://cmake.org/
[lnk-devbox-setup]:  https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-choosing-protocol]: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-protocols
[lnk-hub-file-upload]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-file-upload
[lnk-strip]: https://en.wikipedia.org/wiki/Strip_(Unix)
[lnk-serializer]: https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer
[lnk-parson]: https://github.com/kgabis/parson