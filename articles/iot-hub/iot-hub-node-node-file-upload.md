---
title: Düğüm ile Azure IOT Hub'ına aygıtlardan dosyaları karşıya yükleme | Microsoft Docs
description: Node.js için Azure IOT cihaz SDK'sını kullanarak buluta bir aygıttan dosyaları karşıya yükleme yapma. Karşıya yüklenen dosyaların bir Azure depolama blob kapsayıcısında depolanır.
services: iot-hub
documentationcenter: nodejs
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 4759d229-f856-4526-abda-414f8b00a56d
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: v-masebo;dobett
ms.openlocfilehash: b28a02462fe7a5a7f831102b3707fe03f84342ad
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub"></a>IOT Hub ile bulut cihazınızdan dosyaları karşıya yükleme

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Bu öğretici kodda inşa edilmiştir [IOT Hub ile bulut cihaza ileti gönderme](iot-hub-node-node-c2d.md) kullanmayı gösteren öğretici [dosya karşıya yükleme özellikleri IOT Hub'ın](iot-hub-devguide-file-upload.md) bir dosyayı karşıya yüklemeyi [Azure blob depolama](../storage/index.yml). Öğretici şunların nasıl yapıldığını gösterir için:

- Güvenli bir şekilde bir aygıt ile Azure sağlayan bir dosya yüklemek için URI blob.
- IOT hub'ı dosya karşıya yükleme bildirimleri, uygulama arka uç dosya işleme tetiklemek için kullanın.

[IOT Hub ile çalışmaya başlama](iot-hub-node-node-getstarted.md) öğretici, IOT hub'ı temel cihaz bulut Mesajlaşma işlevlerini gösterir. Ancak, bazı senaryolarda aygıtlarınızı IOT hub'ı kabul görece küçük bir cihaz bulut iletilerini göndermek verileri kolayca eşlenemiyor. Örneğin:

* Görüntüleri içeren büyük dosyaları
* Videolar
* Yüksek sıklıkta örneklenen titreşimi veri
* Önceden işlenmiş veri çeşit.

Bu dosyalar genellikle gibi araçları kullanılarak bulutta işlenen toplu olan [Azure Data Factory](../data-factory/introduction.md) veya [Hadoop](../hdinsight/index.yml) yığını. Bir aygıttan upland dosyalara ihtiyacınız olduğunda, güvenlik ve güvenilirlik IOT Hub'ının kullanmaya devam edebilirsiniz.

Bu öğreticinin sonunda iki Node.js konsol uygulamaları çalıştırın:

* **SimulatedDevice.js**, hangi yükleyen bir dosya, IOT hub tarafından sağlanan bir SAS URI'sini kullanarak depolama.
* **ReadFileUploadNotification.js**, IOT hub'ından dosya karşıya yükleme bildirimlerini alır.

> [!NOTE]
> IOT Hub, birçok cihaz platformları ve Azure IOT cihaz SDK'ları aracılığıyla (C, .NET, Javascript, Python ve Java dahil) dilleri destekler. Başvurmak [Azure IOT Geliştirme Merkezi] Cihazınızı Azure IOT Hub'ına bağlamak adım adım yönergeler için.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Node.js 4.0.x sürümü veya sonraki bir sürüm.
* Etkin bir Azure hesabı. (Bir hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](http://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Bir aygıt uygulamasından bir dosyayı karşıya yüklemek

Bu bölümde, bir dosyayı karşıya yüklemeyi IOT hub'ına cihaz uygulaması oluşturun.

1. ```simulateddevice``` adlı bir boş klasör oluşturun.  Komut isteminizde aşağıdaki komutu kullanarak ```simulateddevice``` klasöründe bir package.json dosyası oluşturun.  Tüm varsayılanları kabul edin:

    ```cmd/sh
    npm init
    ```

1. Komut isteminizde ```simulateddevice``` klasöründe **azure-iot-device** Cihaz SDK paketini ve **azure-iot-device-mqtt** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Bir metin düzenleyicisi kullanarak ```simulateddevice``` klasöründe bir **SimulatedDevice.js** dosyası oluşturun.

1. Aşağıdaki ```require``` deyimlerini **SimulatedDevice.js** dosyasının başlangıcına ekleyin:

    ```nodejs
    'use strict';
    
    var fs = require('fs');
    var mqtt = require('azure-iot-device-mqtt').Mqtt;
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    ```

1. Bir ```deviceconnectionstring``` değişkeni ekleyin ve bir **İstemci** örneği oluşturmak için bunu kullanın.  Değiştir ```{deviceconnectionstring}``` oluşturduğunuz cihaz adı ile _IOT Hub oluşturma_ bölümü:

    ```nodejs
    var connectionString = '{deviceconnectionstring}';
    var filename = 'myimage.png';
    ```

    > [!NOTE]
    > Basitleştirmek amacıyla bağlantı dizesini kodda eklenmiştir: Bu önerilen bir uygulama değildir ve kullanım örneği ve mimarisine bağlı olarak, bu parolayı saklama daha güvenli şekilde isteyebilirsiniz.

1. İstemci bağlanmak için aşağıdaki kodu ekleyin:

    ```nodejs
    var client = clientFromConnectionString(connectionString);
    console.log('Client connected');
    ```

1. Bir geri çağırma oluşturma ve kullanma **uploadToBlob** dosyayı karşıya yüklemeyi işlevi.

    ```nodejs
    fs.stat(filename, function (err, stats) {
        const rr = fs.createReadStream(filename);
    
        client.uploadToBlob(filename, rr, stats.size, function (err) {
            if (err) {
                console.error('Error uploading file: ' + err.toString());
            } else {
                console.log('File uploaded');
            }
        });
    });
    ```

1. **SimulatedDevice.js** dosyasını kaydedin ve kapatın.

1. Bir görüntü dosyasına kopyalayın `simulateddevice` klasörü ve yeniden adlandırmak `myimage.png`.

## <a name="receive-a-file-upload-notification"></a>Dosya karşıya yükleme bildirimi

Bu bölümde IOT hub'dan dosya karşıya yükleme bildirim iletileri alan bir Node.js konsol uygulaması oluşturun.

Kullanabileceğiniz **iothubowner** Bu bölümde tamamlamak için IOT Hub'ınızı bağlantı dizesi. Bağlantı dizesinde bulacaksınız [Azure portal](https://portal.azure.com/) üzerinde **paylaşılan erişim ilkesi** dikey.

1. ```fileuploadnotification``` adlı bir boş klasör oluşturun.  Komut isteminizde aşağıdaki komutu kullanarak ```fileuploadnotification``` klasöründe bir package.json dosyası oluşturun.  Tüm varsayılanları kabul edin:

    ```cmd/sh
    npm init
    ```

1. Komut isteminizde ```fileuploadnotification``` klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure-iothub** SDK paketi:

    ```cmd/sh
    npm install azure-iothub --save
    ```

1. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **FileUploadNotification.js** dosyasını ```fileuploadnotification``` klasör.

1. Aşağıdakileri ekleyin ```require``` deyimleri başlangıcında **FileUploadNotification.js** dosyası:

    ```nodejs
    'use strict';
    
    var Client = require('azure-iothub').Client;
    ```

1. Bir ```iothubconnectionstring``` değişkeni ekleyin ve bir **İstemci** örneği oluşturmak için bunu kullanın.  Değiştir ```{iothubconnectionstring}``` IOT hub'ına oluşturduğunuz bağlantı dizesiyle _IOT Hub oluşturma_ bölümü:

    ```nodejs
    var connectionString = '{iothubconnectionstring}';
    ```

    > [!NOTE]
    > Basitleştirmek amacıyla bağlantı dizesini kodda eklenmiştir: Bu önerilen bir uygulama değildir ve kullanım örneği ve mimarisine bağlı olarak, bu parolayı saklama daha güvenli şekilde isteyebilirsiniz.

1. İstemci bağlanmak için aşağıdaki kodu ekleyin:

    ```nodejs
    var serviceClient = Client.fromConnectionString(connectionString);
    ```

1. İstemci açmak ve kullanmak **getFileNotificationReceiver** durum güncelleştirmeleri almak için işlevi.

    ```nodejs
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFileNotificationReceiver(function receiveFileUploadNotification(err, receiver){
          if (err) {
            console.error('error getting the file notification receiver: ' + err.toString());
          } else {
            receiver.on('message', function (msg) {
              console.log('File upload from device:')
              console.log(msg.getData().toString('utf-8'));
            });
          }
        });
      }
    });
    ```

1. Kaydet ve Kapat **FileUploadNotification.js** dosya.

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

Bir komut isteminde `fileuploadnotification` klasörü, aşağıdaki komutu çalıştırın:

```cmd/sh
node FileUploadNotification.js
```

Bir komut isteminde `simulateddevice` klasörü, aşağıdaki komutu çalıştırın:

```cmd/sh
node SimulatedDevice.js
```

Aşağıdaki ekran görüntüsünde çıktısını gösterir **SimulatedDevice** uygulama:

![Simulated-device uygulamadan çıktı](./media/iot-hub-node-node-file-upload/simulated-device.png)

Aşağıdaki ekran görüntüsünde çıktısını gösterir **FileUploadNotification** uygulama:

![Dosya karşıya yükleme bildirimi okuma uygulamadan çıktı](./media/iot-hub-node-node-file-upload/read-file-upload-notification.png)

Yüklenen dosya yapılandırdığınız depolama kapsayıcısı içinde görüntülemek için portalı kullanabilirsiniz:

![Yüklenen dosya](./media/iot-hub-node-node-file-upload/uploaded-file.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, dosya yüklemelerini basitleştirmek için IOT hub'ı dosya karşıya yükleme becerilerini kullanacak öğrendiniz. IOT hub özelliklerini ve aşağıdaki makalelerde senaryolarını keşfetmeye devam edebilirsiniz:

* [IOT hub'ı program aracılığıyla oluşturma][lnk-create-hub]
* [C SDK Giriş][lnk-c-sdk]
* [Azure IOT SDK'ları][lnk-sdks]

<!-- Links -->
[Azure IOT Geliştirme Merkezi]: http://azure.microsoft.com/develop/iot

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md
