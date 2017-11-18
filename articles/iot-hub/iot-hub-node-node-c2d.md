---
title: "Azure IOT hub'ı (düğüm) sahip bulut-cihaz iletilerini | Microsoft Docs"
description: "Node.js için Azure IOT SDK'ları kullanarak bir Azure IOT hub'dan bir aygıta bulut-cihaz iletilerini göndermek nasıl. Bulut-cihaz iletilerini ve bulut-cihaz iletilerini göndermek için bir arka uç uygulaması değiştirmek için bir sanal cihaz uygulamasının değiştirin."
services: iot-hub
documentationcenter: nodejs
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3ca8a78f-ade2-46e8-8a49-d5d599cdf1f1
ms.service: iot-hub
ms.devlang: javascript
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.openlocfilehash: 80f65e8e7fe562030c1e39787b910e2564969882
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2017
---
# <a name="send-cloud-to-device-messages-with-iot-hub-node"></a>IOT hub'ı (düğüm) sahip bulut-cihaz iletilerini gönder
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Giriş
Azure IOT Hub, milyonlarca cihaza arasında güvenilir ve güvenli çift yönlü iletişimler sağlayan tam olarak yönetilen hizmet etkinleştirin ve bir çözüm arka ucu ' dir. [IOT Hub ile çalışmaya başlama] öğretici, IOT hub'ı oluşturma, bir cihaz kimliği, sağlama ve cihaz-bulut iletileri gönderen bir sanal cihaz uygulamasının kodu gösterir.

Bu öğretici derlemeler [IOT Hub ile çalışmaya başlama]. Size bir nasıl gösterir için:

* Çözüm arka ucunuz, tek bir cihaz IOT hub'ı aracılığıyla bulut cihaz ileti gönderebilir.
* Bir cihazda bulut-cihaz iletilerini alır.
* Çözüm arka ucunuz, teslimat alındısı iste (*geri bildirim*) için bir cihaz IOT Hub'ından gönderilen iletiler için.

Bulut cihaz iletileri hakkında daha fazla bilgi bulabilirsiniz [IOT Hub Geliştirici Kılavuzu][IoT Hub developer guide - C2D].

Bu öğreticinin sonunda iki Node.js konsol uygulamaları çalıştırın:

* **SimulatedDevice**, oluşturduğunuz uygulama değiştirilmiş bir sürümünü [IOT Hub ile çalışmaya başlama], IOT hub'ınıza bağlanır ve bulut-cihaz iletilerini alır.
* **SendCloudToDeviceMessage**, IOT hub'ı aracılığıyla sanal cihaz uygulamasının bir bulut cihaz ileti gönderir ve teslimat alındısı alır.

> [!NOTE]
> IOT Hub SDK desteği birçok cihaz platformları ve Azure IOT cihaz SDK'ları aracılığıyla dilleri (C, Java ve Javascript dahil) sahiptir. Bu öğreticinin kod ve genellikle Azure IOT Hub Cihazınızı bağlamak hakkında adım adım yönergeler için bkz: [Azure IOT Geliştirme Merkezi].
> 
> 

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Node.js 4.0.x sürümü veya sonraki bir sürüm.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

## <a name="receive-messages-in-the-simulated-device-app"></a>Sanal cihaz uygulamasının ileti alma
Bu bölümde, oluşturduğunuz sanal cihaz uygulamasının değiştirme [IOT Hub ile çalışmaya başlama] IOT hub'ından bulut cihaz iletileri almak için.

1. Bir metin düzenleyicisi kullanarak SimulatedDevice.js dosyasını açın.
2. Değiştirme **connectCallback** IOT hub'dan gönderilen iletileri işlemek için işlev. Bu örnekte, cihazın her zaman çağırır **tam** iletiyi işledi IOT hub'ı bildirmek için işlevi. Yeni sürümünüz **connectCallback** işlevi aşağıdaki kod parçacığını gibi görünür:
   
    ```javascript
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
        client.on('message', function (msg) {
          console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
          client.complete(msg, printResultFor('completed'));
        });
        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var temperature = 20 + (Math.random() * 15);
            var humidity = 60 + (Math.random() * 20);            
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', temperature: temperature, humidity: humidity });
            var message = new Message(data);
            message.properties.add('temperatureAlert', (temperature > 30) ? 'true' : 'false');
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```
   
   > [!NOTE]
   > HTTPS MQTT veya AMQP yerine taşıma olarak kullanırsanız **DeviceClient** seyrek IOT Hub (değerinden 25 dakikada bir) gelen iletileri örneği denetler. MQTT, AMQP ve HTTPS desteği ve IOT hub'ı azaltma arasındaki farklar hakkında daha fazla bilgi için bkz: [IOT Hub Geliştirici Kılavuzu][IoT Hub developer guide - C2D].
   > 
   > 

## <a name="send-a-cloud-to-device-message"></a>Bulut cihaza ileti gönderme
Bu bölümde, sanal cihaz uygulamasının bulut cihaz iletileri gönderen bir Node.js konsol uygulaması oluşturun. Eklediğiniz aygıt cihaz Kimliğini gereksinim [IOT Hub ile çalışmaya başlama] Öğreticisi. Ayrıca bulabilirsiniz, hub'ın IOT Hub bağlantı dizesine gerekir [Azure portal].

1. Adlı bir boş klasör oluşturun **sendcloudtodevicemessage**. İçinde **sendcloudtodevicemessage** klasörü, komut isteminde aşağıdaki komutu kullanarak bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:
   
    ```shell
    npm init
    ```
2. Komut isteminizde **sendcloudtodevicemessage** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure-iothub** paketi:
   
    ```shell
    npm install azure-iothub --save
    ```
3. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **SendCloudToDeviceMessage.js** dosyasını **sendcloudtodevicemessage** klasör.
4. Aşağıdakileri ekleyin `require` deyimleri başlangıcında **SendCloudToDeviceMessage.js** dosyası:
   
    ```javascript
    'use strict';
   
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```
5. Aşağıdaki kodu ekleyin **SendCloudToDeviceMessage.js** dosya. "{IOT hub bağlantı dizesine}" yer tutucu değerini, oluşturduğunuz hub'ın IOT Hub bağlantı dizesiyle değiştirin [IOT Hub ile çalışmaya başlama] Öğreticisi. "{Aygıt id}" yer tutucusunu, eklediğiniz aygıt cihaz Kimliğini değiştirin [IOT Hub ile çalışmaya başlama] Öğreticisi:
   
    ```javascript
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';
   
    var serviceClient = Client.fromConnectionString(connectionString);
    ```
6. İşlem sonuçları konsoluna yazdırmak için aşağıdaki işlevi ekleyin:
   
    ```javascript
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. Teslim geri bildirim iletilerini konsola yazdırmak için aşağıdaki işlevi ekleyin:
   
    ```javascript
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```
8. Aygıtınıza ileti gönderme ve cihaz bulut cihaz iletiyi onayladıktan zaman geri bildirim iletisi işlemek için aşağıdaki kodu ekleyin:
   
    ```javascript
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud to device message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```
9. Kaydet ve Kapat **SendCloudToDeviceMessage.js** dosya.

## <a name="run-the-applications"></a>Uygulamaları çalıştırma
Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Komut isteminde **simulateddevice** klasörü, IOT Hub'ına telemetri göndermeyi ve bulut-cihaz iletilerini dinlemek için aşağıdaki komutu çalıştırın:
   
    ```shell
    node SimulatedDevice.js 
    ```
   
    ![Sanal cihaz uygulamasının çalıştırın][img-simulated-device]
2. Bir komut isteminde **sendcloudtodevicemessage** klasörü, bulut cihaza ileti gönderme ve onayı geri bildirim için beklemek için aşağıdaki komutu çalıştırın:
   
    ```shell
    node SendCloudToDeviceMessage.js 
    ```
   
    ![Bulut cihaz komut göndermek için uygulamayı çalıştırın][img-send-command]
   
   > [!NOTE]
   > Basitlik'ın artırmak amacıyla için bu öğreticiyi herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda yeniden deneme ilkelerini (üstel geri alma), önerilen MSDN makalesinde uygulamalıdır [geçici hata işleme].
   > 
   > 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğretici kapsamında, bulut cihaza ileti gönderme ve alma öğrendiniz. 

IOT hub'ı kullanan tam uçtan uca çözümler örnekleri görmek için bkz: [Azure IOT paketi].

IOT Hub ile çözümleri geliştirme hakkında daha fazla bilgi için bkz: [IOT Hub Geliştirici Kılavuzu].

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

[IOT Hub ile çalışmaya başlama]: iot-hub-node-node-getstarted.md
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[IOT Hub Geliştirici Kılavuzu]: iot-hub-devguide.md
[Azure IOT Geliştirme Merkezi]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[geçici hata işleme]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure portal]: https://portal.azure.com
[Azure IOT paketi]: https://azure.microsoft.com/documentation/suites/iot-suite/
