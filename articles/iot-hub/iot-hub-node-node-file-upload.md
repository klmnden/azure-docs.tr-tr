---
title: Azure IOT hub'a düğümle cihazlardan dosya yükleme | Microsoft Docs
description: Node.js için Azure IOT cihaz SDK'sını kullanarak bulutta bir CİHAZDAN dosyaları karşıya yükleme. Karşıya yüklenen dosyaları bir Azure depolama blob kapsayıcısında depolanır.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 06/28/2017
ms.author: dobett
ms.openlocfilehash: 12ff4fef5e04819e967a39fe65845b89790e22d6
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51234464"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub"></a>Cihazınızı IOT Hub ile buluta dosyaları karşıya yükleme

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Bu öğreticide kodda geliştirir [IOT Hub ile bulut buluttan cihaza ileti gönderme](iot-hub-node-node-c2d.md) nasıl kullanılacağını göstermek için öğretici [dosya karşıya yükleme özellikleri IOT Hub'ın](iot-hub-devguide-file-upload.md) bir dosyayı karşıya yüklemek için [Azure blob Depolama](../storage/index.yml). Öğretici şunların nasıl yapıldığını göstermektedir:

- Bir cihaz Azure ile güvenli bir şekilde sağlayan bir dosya karşıya yükleme için URI blob.
- IOT hub'ı dosya karşıya yükleme bildirimlerini, uygulama arka ucu dosyasında bir işlem tetiklemek için kullanın.

[IOT Hub ile çalışmaya başlama](quickstart-send-telemetry-node.md) öğretici, IOT hub'ı temel cihaz-bulut Mesajlaşma işlevlerini gösterir. Ancak, bazı senaryolarda cihazlarınızı IOT hub'ı kabul görece küçük bir CİHAZDAN buluta ileti gönderme verileri kolayca eşlenemiyor. Örneğin:

* Görüntüleri içeren büyük dosyaları
* Videolar
* Titreşim veri yüksek sıklıkta örneklenir
* Önceden işlenmiş verilerin bazı formlarıyla.

Bu dosyalar genellikle toplu işleme gibi araçları kullanarak bulutta olduğu [Azure Data Factory](../data-factory/introduction.md) veya [Hadoop](../hdinsight/index.yml) yığını. Bir CİHAZDAN upland dosyalara ihtiyacınız olduğunda, güvenlik ve güvenilirlik IOT hub'ı kullanmaya devam edebilirsiniz.

Bu öğreticinin sonunda iki Node.js konsol uygulaması çalıştırın:

* **SimulatedDevice.js**, hangi yükler bir dosya depolama, IOT hub tarafından sağlanan bir SAS URI'sini kullanarak.
* **ReadFileUploadNotification.js**, IOT hub'ından dosya karşıya yükleme bildirimleri alır.

> [!NOTE]
> IOT hub'ı, çok sayıda cihaz platformları ve Azure IOT cihaz SDK'ları aracılığıyla (C, .NET, Javascript, Python ve Java dahil) dilleri destekler. Başvurmak [Azure IOT Geliştirici Merkezi] Cihazınızı Azure IOT Hub'ına bağlanmak adım adım yönergeler için.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Node.js 4.0.x sürümü veya sonraki bir sürüm.
* Etkin bir Azure hesabı. (Hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Bir cihaz uygulamasından bir dosyayı karşıya yükleyin

Bu bölümde, IOT hub'ına bir dosyayı karşıya yüklemek için cihaz uygulaması oluşturun.

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

1. Bir ```deviceconnectionstring``` değişkeni ekleyin ve bir **İstemci** örneği oluşturmak için bunu kullanın.  Değiştirin ```{deviceconnectionstring}``` oluşturduğunuz cihaz adıyla _IOT Hub oluşturma_ bölümü:

    ```nodejs
    var connectionString = '{deviceconnectionstring}';
    var filename = 'myimage.png';
    ```

    > [!NOTE]
    > Basitleştirmek amacıyla bağlantı dizesini koda dahil edilmiş: Bu önerilen bir yöntem değildir ve mimarisi ve kullanım örneği bağlı olarak, bu gizli dizi depolama daha güvenli şekilde düşünmek isteyebilirsiniz.

1. İstemci bağlanmak için aşağıdaki kodu ekleyin:

    ```nodejs
    var client = clientFromConnectionString(connectionString);
    console.log('Client connected');
    ```

1. Bir geri çağırma oluşturun ve kullanın **uploadToBlob** dosyayı karşıya yüklemek için işlevi.

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

1. Bir görüntü dosyasına kopyalama `simulateddevice` klasörü ve yeniden adlandırmak `myimage.png`.

## <a name="receive-a-file-upload-notification"></a>Dosya karşıya yükleme bildirim alma

Bu bölümde, IOT Hub'ından dosya karşıya yükleme bildirim iletileri alan bir Node.js konsol uygulaması oluşturun.

Kullanabileceğiniz **iothubowner** bu bölümünü tamamlamak için IOT Hub'ınızın bağlantı dizesinden. Bağlantı dizesinde bulabilirsiniz [Azure portalında](https://portal.azure.com/) üzerinde **paylaşılan erişim ilkesi** dikey penceresi.

1. ```fileuploadnotification``` adlı bir boş klasör oluşturun.  Komut isteminizde aşağıdaki komutu kullanarak ```fileuploadnotification``` klasöründe bir package.json dosyası oluşturun.  Tüm varsayılanları kabul edin:

    ```cmd/sh
    npm init
    ```

1. Komut isteminizde ```fileuploadnotification``` klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure-iothub** SDK paketi:

    ```cmd/sh
    npm install azure-iothub --save
    ```

1. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **FileUploadNotification.js** dosyası ```fileuploadnotification``` klasör.

1. Aşağıdaki ```require``` başlangıcında deyimleri **FileUploadNotification.js** dosyası:

    ```nodejs
    'use strict';
    
    var Client = require('azure-iothub').Client;
    ```

1. Bir ```iothubconnectionstring``` değişkeni ekleyin ve bir **İstemci** örneği oluşturmak için bunu kullanın.  Değiştirin ```{iothubconnectionstring}``` oluşturduğunuz IOT hub bağlantı dizesiyle _IOT Hub oluşturma_ bölümü:

    ```nodejs
    var connectionString = '{iothubconnectionstring}';
    ```

    > [!NOTE]
    > Basitleştirmek amacıyla bağlantı dizesini koda dahil edilmiş: Bu önerilen bir yöntem değildir ve mimarisi ve kullanım örneği bağlı olarak, bu gizli dizi depolama daha güvenli şekilde düşünmek isteyebilirsiniz.

1. İstemci bağlanmak için aşağıdaki kodu ekleyin:

    ```nodejs
    var serviceClient = Client.fromConnectionString(connectionString);
    ```

1. İstemci açmak ve kullanmak **getFileNotificationReceiver** durum güncelleştirmeleri almak için işlev.

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

Aşağıdaki ekran görüntüsünde çıktısında **SimulatedDevice** uygulama:

![Simulated-device uygulama çıktısı](./media/iot-hub-node-node-file-upload/simulated-device.png)

Aşağıdaki ekran görüntüsünde çıktısında **FileUploadNotification** uygulama:

![Dosya karşıya yükleme bildirimini okuma uygulama çıktısı](./media/iot-hub-node-node-file-upload/read-file-upload-notification.png)

Karşıya yüklenen dosya yapılandırdığınız depolama kapsayıcısında görüntülemek için portalı kullanabilirsiniz:

![Karşıya yüklenen dosya](./media/iot-hub-node-node-file-upload/uploaded-file.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, cihazlardan karşıya dosya yükleme işlemleri basitleştirmek için dosya karşıya yükleme özellikleri IOT hub'ı kullanmayı öğrendiniz. IOT hub özelliklerini ve aşağıdaki makalelerde senaryolarını keşfetmeye devam edebilirsiniz:

* [Programlamalı IOT hub oluşturma][lnk-create-hub]
* [C SDK'ya giriş][lnk-c-sdk]
* [Azure IOT SDK'ları][lnk-sdks]

<!-- Links -->
[Azure IoT Geliştirici Merkezi]: http://azure.microsoft.com/develop/iot

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md
