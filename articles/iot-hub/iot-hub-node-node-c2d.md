---
title: Azure IOT hub'ı (Node) ile bulut-cihaz iletilerini | Microsoft Docs
description: Node.js için Azure IOT SDK'larını kullanarak bir Azure IOT hub'ından bir cihaza bulut-cihaz iletilerini göndermek nasıl. Bir sanal cihaz uygulamasının bulut-cihaz iletilerini ve bulut-cihaz iletilerini göndermek için bir arka uç uygulaması değiştirme değiştirin.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: javascript
ms.topic: conceptual
ms.date: 06/16/2017
ms.author: dobett
ms.openlocfilehash: c2b8df708a568fa995cf3af783c112127726811b
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47221817"
---
# <a name="send-cloud-to-device-messages-with-iot-hub-node"></a>IOT hub'ı (Node) ile bulut buluttan cihaza iletileri gönderme
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Giriş
Azure IOT hub'ı yardımcı olan tam olarak yönetilen bir hizmet, milyonlarca cihaz arasında güvenilir ve güvenli çift yönlü iletişimi etkinleştirmek ve bir çözüm arka ucu ' dir. [IOT Hub ile çalışmaya başlama] öğretici, IOT hub oluşturma, bir cihaz kimliği da sağlamak ve CİHAZDAN buluta iletiler gönderen bir sanal cihaz uygulamasının kodu nasıl gösterir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğreticide yapılar [IOT Hub ile çalışmaya başlama]. Bunun nasıl yapılacağı anlatılmaktadır için:

* Çözüm arka ucunuz, tek bir cihaz IOT hub'ı aracılığıyla bulut-cihaz iletilerini gönderin.
* Bir cihazda bulut-cihaz iletilerini alır.
* Çözüm arka ucunuz, teslimat alındısı istek (*geri bildirim*) için bir cihaz IOT Hub'ından gönderilen iletileri.

Bulut-cihaz iletileri hakkında daha fazla bilgi bulabilirsiniz [IOT Hub Geliştirici kılavuzunun][IoT Hub developer guide - C2D].

Bu öğreticinin sonunda iki Node.js konsol uygulaması çalıştırın:

* **SimulatedDevice**, oluşturulan uygulamayı değiştirilmiş bir sürümünü [IOT Hub ile çalışmaya başlama], IOT hub'ınıza bağlanır ve bulut-cihaz iletilerini alır.
* **SendCloudToDeviceMessage**, IOT hub'ı aracılığıyla sanal cihaz uygulaması için bir bulut buluttan cihaza ileti gönderir ve ardından, teslimat alındısı.

> [!NOTE]
> IOT Hub SDK desteği birçok cihaz platformlarını ve Azure IOT cihaz SDK'ları aracılığıyla diller (C, Java ve Javascript dahil) sahiptir. Bu öğreticinin koda ve genellikle Azure IOT hub'a Cihazınızı bağlamak hakkında adım adım yönergeler için bkz. [Azure IOT Geliştirici Merkezi].
> 
> 

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Node.js 4.0.x sürümü veya sonraki bir sürüm.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

## <a name="receive-messages-in-the-simulated-device-app"></a>Sanal cihaz uygulamasında ileti alma
Bu bölümde, oluşturduğunuz sanal cihaz uygulaması değiştirme [IOT Hub ile çalışmaya başlama] IOT hub'ından bulut-cihaz iletilerini almak için.

1. Bir metin düzenleyicisi kullanarak SimulatedDevice.js dosyasını açın.
2. Değiştirme **connectCallback** IOT hub'dan gönderilen iletileri işlemek için işlev. Bu örnekte, cihazın her zaman çağırır **tam** ileti işlenmemiş olan IOT hub'ı bildirmek için işlevi. Yeni sürümünüzü **connectCallback** işlevi aşağıdaki kod parçacığı gibi görünür:
   
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
   > HTTPS MQTT veya AMQP yerine aktarım olarak kullanırsanız **DeviceClient** örneği iletileri IOT hub'dan seyrek (küçüktür 25 dakikada bir) denetler. MQTT, AMQP ve HTTPS desteği ve IOT hub'ı azaltma arasındaki farklar hakkında daha fazla bilgi için bkz. [IOT Hub Geliştirici kılavuzunun][IoT Hub developer guide - C2D].
   > 
   > 

## <a name="send-a-cloud-to-device-message"></a>Bulut buluttan cihaza ileti gönderme
Bu bölümde, sanal cihaz uygulaması için bulut-cihaz iletilerini gönderen bir Node.js konsol uygulaması oluşturun. Cihazın kimliği, eklediğiniz ihtiyacınız [IOT Hub ile çalışmaya başlama] öğretici. IOT Hub bağlantı dizesine, bulabileceğiniz hub'ınız için etmeniz [Azure portal].

1. Adlı bir boş klasör oluşturun **sendcloudtodevicemessage**. İçinde **sendcloudtodevicemessage** klasöründe komut isteminizde aşağıdaki komutu kullanarak bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:
   
    ```shell
    npm init
    ```
2. Komut isteminizde **sendcloudtodevicemessage** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure-iothub** paket:
   
    ```shell
    npm install azure-iothub --save
    ```
3. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **SendCloudToDeviceMessage.js** dosyası **sendcloudtodevicemessage** klasör.
4. Aşağıdaki `require` başlangıcında deyimleri **SendCloudToDeviceMessage.js** dosyası:
   
    ```javascript
    'use strict';
   
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```
5. Aşağıdaki kodu ekleyin **SendCloudToDeviceMessage.js** dosya. "{IOT hub bağlantı dizesine}" yer tutucu değerini hub için oluşturduğunuz, IOT Hub bağlantı dizesiyle değiştirin [IOT Hub ile çalışmaya başlama] öğretici. Eklediğiniz cihazın cihaz kimliği "{cihaz id}" yer tutucusunu değiştirin [IOT Hub ile çalışmaya başlama] Öğreticisi:
   
    ```javascript
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';
   
    var serviceClient = Client.fromConnectionString(connectionString);
    ```
6. İşlem sonuçlarını konsola yazdırmak için aşağıdaki işlevi ekleyin:
   
    ```javascript
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. Teslim geri bildirim iletileri konsola yazdırmak için aşağıdaki işlevi ekleyin:
   
    ```javascript
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```
8. Cihazınız için bir ileti gönderin ve cihaz bulut-cihaz iletiyi onayladıktan, geri bildirim iletisini işlemek için aşağıdaki kodu ekleyin:
   
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

1. Komut isteminde **simulateddevice** klasörü, IOT Hub'ına telemetri göndermeyi ve bulut-cihaz iletileri dinlemek için aşağıdaki komutu çalıştırın:
   
    ```shell
    node SimulatedDevice.js 
    ```
   
    ![Sanal cihaz uygulaması çalıştırma][img-simulated-device]
2. Bir komut isteminde **sendcloudtodevicemessage** klasörü, bulut buluttan cihaza ileti göndermek ve bildirim hakkındaki geri bildirimleri beklemek için aşağıdaki komutu çalıştırın:
   
    ```shell
    node SendCloudToDeviceMessage.js 
    ```
   
    ![Bulut-cihaz komut göndermek için uygulamayı çalıştırın][img-send-command]
   
   > [!NOTE]
   > Basitlik'ın çok için bu öğreticiyi herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda yeniden deneme ilkelerini (üstel geri alma), örneğin makalesinde önerildiği uygulamalıdır [geçici hata işleme](/azure/architecture/best-practices/transient-faults).
   > 
   > 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bulut-cihaz iletilerini gönderip öğrendiniz. 

IOT hub'ı kullanan tam uçtan uca çözümler örneklerini görmek için bkz: [Azure IOT Uzaktan izleme çözüm Hızlandırıcısını].

IOT Hub ile çözümleri geliştirme hakkında daha fazla bilgi için bkz. [IOT Hub Geliştirici Kılavuzu].

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

[IOT Hub ile çalışmaya başlama]: quickstart-send-telemetry-node.md
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[IOT Hub Geliştirici Kılavuzu]: iot-hub-devguide.md
[Azure IoT Geliştirici Merkezi]: http://azure.microsoft.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[Azure portal]: https://portal.azure.com
[Azure IOT Uzaktan izleme çözüm Hızlandırıcısını]: https://azure.microsoft.com/documentation/suites/iot-suite/
