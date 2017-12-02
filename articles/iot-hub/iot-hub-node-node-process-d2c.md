---
title: "Azure IOT hub'ı (düğüm) yönlendirme iletilerle | Microsoft Docs"
description: "Diğer arka uç hizmetlerine iletilerinin gönderilmesi için yönlendirme kurallarını ve özel uç noktaları kullanarak Azure IOT Hub cihaz bulut iletilerini işlemek nasıl."
services: iot-hub
documentationcenter: node
author: msebolt
manager: timlt
editor: 
ms.assetid: bd9af5f9-a740-4780-a2a6-8c0e2752cf48
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/17/2017
ms.author: v-masebo
ms.openlocfilehash: 5a80195dd474414626cc54623945393c6f88093d
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="routing-messages-with-iot-hub-node"></a>IOT hub'ı (düğüm) ile ileti yönlendirme

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

Bu öğretici derlemeler [IOT Hub ile çalışmaya başlama] Öğreticisi.  Öğretici:

* Cihaz bulut iletilerini kolay, yapılandırma tabanlı bir şekilde gönderilmesi için yönlendirme kurallarını kullanmayı gösterir.
* Daha ayrıntılı işleme için çözüm arka uçtan Acil eylem gerekli etkileşimli iletilerin yalıtmak nasıl gösterilmektedir.  Örneğin, bir aygıt bir bilet bir CRM sistemine ekleme tetikleyen bir uyarı iletisi gönderebilir.  Buna karşılık, sıcaklık telemetri gibi veri noktası iletileri analytics motoruna akış.

Bu öğreticinin sonunda üç Node.js konsol uygulamaları çalıştırın:

* **SimulatedDevice.js**, oluşturduğunuz uygulama değiştirilmiş bir sürümünü [IOT Hub ile çalışmaya başlama] öğretici, her ikinci ve etkileşimli cihaz bulut iletilerini rastgele başına veri noktası cihaz bulut iletilerini gönderir Aralık. Bu uygulama, IOT Hub ile iletişim kurmak için MQTT protokolünü kullanır.
* **ReadDeviceToCloudMessages.js** cihaz uygulamanız tarafından gönderilen telemetriyi görüntüler.
* **ReadCriticalQueue.js** IOT hub'ına bağlı Service Bus kuyruğundan kritik iletileri çıkarır.

> [!NOTE]
> IOT hub'ı birçok cihaz platformları ve C, Java ve JavaScript gibi diller için SDK desteğe sahiptir. Cihaz Bu öğreticide bir fiziksel aygıt ile değiştirme ve aygıtları bir IOT Hub'ına bağlanmak hakkında yönergeler için bkz: [Azure IOT Geliştirme Merkezi].

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Eksiksiz bir çalışma sürümü [IOT Hub ile çalışmaya başlama] Öğreticisi.
* Node.js 4.0.x sürümü veya sonraki bir sürüm.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

Ayrıca okumayı öneririz [Azure Storage] ve [Azure Service Bus].

## <a name="send-interactive-messages-from-a-device-app"></a>Bir aygıt uygulamadan etkileşimli ileti gönderme
Bu bölümde, oluşturduğunuz cihaz uygulamayı değiştirmek [IOT Hub ile çalışmaya başlama] bazen hemen işlem iletileri göndermek için öğretici.

1. Açmak için bir metin düzenleyicisi kullanın **simulateddevice\SimulatedDevice.js** dosya. Bu dosya için kod içeren **SimulatedDevice** oluşturduğunuz uygulama [IOT Hub ile çalışmaya başlama] Öğreticisi.

2. Değiştir **connectCallback** işlevi aşağıdaki kod ile:

    ```nodejs
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
    
        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
        var data, message;
        if (Math.random() > 0.7) {
            if (Math.random() > 0.5) {
                data = "This is a critical message.";
                message = new Message(data);
                message.properties.add("level", "critical");
            } else {
                data = "This is a storage message.";
                message = new Message(data);
                message.properties.add("level", "storage");
            }
            console.log("Sending message: " + message.getData());
        }
        else {
                var temperature = 20 + (Math.random() * 15);
                var humidity = 60 + (Math.random() * 20);            
                data = JSON.stringify({ deviceId: 'myFirstNodeDevice', temperature: temperature, humidity: humidity });
                message = new Message(data);
                message.properties.add('temperatureAlert', (temperature > 30) ? 'true' : 'false');
                console.log("Sending message: " + message.getData());
            }
        client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```
   
    Bu yöntem rastgele özelliği ekler `"level": "critical"` ve `"level": "storage"` aygıt tarafından gönderilen iletiler için hangi benzetimini yapar, uygulama arka uç ya da kalıcı olarak depolanması gereken bir Acil eylem gerektiren bir ileti. Bu IOT hub'ın uygun mesajı hedefe ileti yönlendirmek için uygulama bu bilgileri ileti özelliklerinde yerine ileti gövdesinde geçirir.
   
   > [!NOTE]
   > Burada gösterilen etkin yolunuzda örnek yanı sıra soğuk yolu işleme dahil olmak üzere çeşitli senaryoları için ileti özellikleri iletileri yönlendirmek için kullanabilirsiniz.

2. Kaydet ve Kapat **simulateddevice\SimulatedDevice.js** dosya.

    > [!NOTE]
    > MSDN makalesinde önerilen üstel geri alma gibi bir yeniden deneme ilkesi uygulamak önerilir [geçici hata işleme].

## <a name="add-service-bus-queue-to-your-iot-hub-and-route-messages-to-it"></a>Service Bus kuyruğuna IOT hub ve rota iletileri ona ekleyin

Bu bölümde, hizmet veri yolu kuyruğu oluşturma, IOT hub'ınıza bağlanın ve ileti özellikte varlığına dayalı kuyruğa ileti göndermek için IOT hub'ınızı yapılandırma. Hizmet veri yolu sıralardan işlem iletileri hakkında daha fazla bilgi için bkz: [kuyruklarla çalışmaya başlama][lnk-sb-queues-node].

1. Service Bus kuyruğuna açıklandığı gibi oluşturmak [kuyruklarla çalışmaya başlama][lnk-sb-queues-node]. Ad alanı ve sıra adını not edin.

2. Azure portalında, IOT hub'ınızı açın ve **uç noktaları**.

    ![IOT hub uç noktaları][30]

3. İçinde **uç noktaları** dikey penceresinde tıklatın **Ekle** sıranız IOT hub'ınıza eklemek için üst. Uç nokta adı **CriticalQueue** ve seçmek için açılır listeleri kullanın **Service Bus kuyruğuna**, sıranız bulunduğu hizmet veri yolu ad alanı ve sıranız adı. İşiniz bittiğinde tıklatın **Tamam** altındaki.  

    ![Bir uç nokta ekleme][31]

4. Şimdi **yollar** IOT hub'ınızdaki. Tıklatın **Ekle** iletileri kuyruğa yönlendiren bir yönlendirme kuralı oluşturmak için dikey pencerenin üstündeki eklediğiniz. Seçin **cihaz iletilerini** veri kaynağı olarak. Girin `level="critical"` koşul olarak seçin **CriticalQueue** yönlendirme kuralı uç noktası olarak özel bir uç noktası olarak. Tıklatın **kaydetmek** altındaki.  

    ![Bir yol ekleme][32]

    Emin olun geri dönüş rota ayarlanmış **ON**. Bu ayar, bir IOT hub'ın varsayılan yapılandırmadır.

    ![Geri dönüş yolu][33]

## <a name="optional-read-from-the-queue-endpoint"></a>(İsteğe bağlı) Sıra uç noktasından okumak

Bu bölümde IOT hizmeti yolundan kritik iletileri okuyan bir Node.js konsol uygulaması oluşturun. Daha fazla bilgi [kuyruklarla çalışmaya başlama][lnk-sb-queues-node]. 

1. `readcriticalqueue` adlı bir boş klasör oluşturun. Komut isteminizde aşağıdaki komutu kullanarak `readcriticalqueue` klasöründe bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:

    ```cmd/sh
    npm init
    ```

2. Komut isteminizde `readcriticalqueue` klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure** paketi:

    ```cmd/sh
    npm install azure --save
    ```

3. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **ReadCriticalQueue.js** dosyasını `readcriticalqueue` klasör.

4. Aşağıdakileri ekleyin `require` deyimleri başlangıcında **ReadCriticalQueue.js** dosyası:

    ```nodejs
    'use strict';

    var azure = require('azure');
    ```

5. Aşağıdaki değişken bildirimi ekleyin ve yer tutucu değerlerini IOT hizmet veri yolu bağlantı dizesi ve kuyruk adı ile değiştirin:

    ```nodejs
    var connectionString = '{iotservicebus connection string}';
    var queueName = '{iotservice bus queue name}';
    ```

6. Konsola çıktı yazdıran aşağıdaki iki işlevi ekleyin:

    ```nodejs
    var serviceBusService = azure.createServiceBusService(connectionString);

    setInterval(function() {serviceBusService.receiveQueueMessage(queueName, function(error, receivedMessage) {
        if(!error){
            // Message received and deleted
            console.log(receivedMessage);
        } else { console.log(error); }
        });
    }, 3000);
    ```

7. Kaydet ve Kapat **ReadCriticalQueue.js** dosya.

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Artık üç uygulamaları çalıştırmak hazırsınız.

1. Çalıştırmak için **ReadDeviceToCloudMessages.js** uygulamasında, bir komut istemi veya kabuk readdevicetocloudmessages klasörüne gidin ve aşağıdaki komutu yürütün:

   ```cmd/sh
   node ReadDeviceToCloudMessages.js
   ```

   ![Read-d2c-messages çalıştırın][readd2c]

2. Çalıştırmak için **ReadCriticalQueue.js** uygulamasında, bir komut istemi veya kabuk readcriticalqueue klasörüne gidin ve aşağıdaki komutu yürütün:

   ```cmd/sh
   node ReadCriticalQueue.js
   ```
   
   ![Read-kritik-messages çalıştırın][readqueue]

3. Çalıştırmak için **SimulatedDevice.js** uygulamasında, bir komut istemi veya kabuk simulateddevice klasörüne gidin ve aşağıdaki komutu yürütün:

   ```cmd/sh
   node SimulatedDevice.js
   ```
   
   ![Simulated-device çalıştırın][simulateddevice]

## <a name="optional-add-storage-container-to-your-iot-hub-and-route-messages-to-it"></a>(İsteğe bağlı) Depolama kapsayıcısı IOT hub ve rota iletileri ona ekleyin

Bu bölümde, depolama hesabı oluşturma, IOT hub'ınıza bağlanın ve iletiyi bir özellik varlığına dayalı hesabına iletileri göndermek için IOT hub'ınızı yapılandırma. Depolama yönetme hakkında daha fazla bilgi için bkz: [Azure Storage ile çalışmaya başlama][Azure Storage].

 > [!NOTE]
   > Birle sınırlı değilse **Endpoint**, Kurulum **StorageContainer** ek olarak **CriticalQueue** ve her iki simulatneously çalıştırın.

1. Bölümünde açıklandığı gibi bir depolama hesabı oluşturma [Azure Storage belgeleri][lnk-storage]. Hesap adını not edin.

2. Azure portalında, IOT hub'ınızı açın ve **uç noktaları**.

3. İçinde **uç noktaları** dikey penceresinde, select **CriticalQueue** uç noktası ve tıklatın **silmek**. Tıklatın **Evet**ve ardından **Ekle**. Uç nokta adı **StorageContainer** ve seçmek için açılır listeleri kullanın **Azure depolama kapsayıcısının**ve oluşturma bir **depolama hesabı** ve **depolama kapsayıcı**.  Adlarını not edin.  İşiniz bittiğinde tıklatın **Tamam** altındaki. 

 > [!NOTE]
   > Birle sınırlı değilse **Endpoint**, silme gerekmez **CriticalQueue**.

4. Tıklatın **yollar** IOT hub'ınızdaki. Tıklatın **Ekle** iletileri kuyruğa yönlendiren bir yönlendirme kuralı oluşturmak için dikey pencerenin üstündeki eklediğiniz. Seçin **cihaz iletilerini** veri kaynağı olarak. Girin `level="storage"` koşul olarak seçin **StorageContainer** yönlendirme kuralı uç noktası olarak özel bir uç noktası olarak. Tıklatın **kaydetmek** altındaki.  

    Emin olun geri dönüş rota ayarlanmış **ON**. Bu ayar, bir IOT hub'ın varsayılan yapılandırmadır.

1. Önceki uygulamanızı emin olun **SimulatedDevice.js** hala çalışıyor. 

1. Azure Portalı'nda altında depolama hesabınıza gidin **Blob hizmeti**, tıklatın **BLOB'lar Gözat...** .  Kapsayıcıyı seçin, gidin ve JSON dosyasına tıklayın ve tıklayın **karşıdan** verileri görüntülemek için.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, IOT Hub'ının ileti yönlendirme işlevini kullanarak cihaz bulut iletilerini güvenilir bir şekilde gönderme öğrendiniz.

[IOT Hub ile bulut-cihaz iletilerini göndermek nasıl] [ lnk-c2d] aygıtlarınıza çözüm arka ucu iletileri göndermek nasıl gösterir.

IOT hub'ı kullanan tam uçtan uca çözümler örnekleri görmek için bkz: [Azure IOT paketi][lnk-suite].

IOT Hub ile çözümleri geliştirme hakkında daha fazla bilgi için bkz: [IOT Hub Geliştirici Kılavuzu].

IOT Hub içinde ileti yönlendirme hakkında daha fazla bilgi için bkz: [IOT Hub ile iletileri almasına ve göndermesine][lnk-devguide-messaging].

<!-- Images. -->
<!-- TODO: UPDATE PICTURES -->
[simulateddevice]: ./media/iot-hub-node-node-process-d2c/runsimulateddevice.png
[readd2c]: ./media/iot-hub-node-node-process-d2c/runprocessinteractive.png
[readqueue]: ./media/iot-hub-node-node-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-node-node-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-node-node-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-node-node-process-d2c/route-creation.png
[33]: ./media/iot-hub-node-node-process-d2c/fallback-route.png

<!-- Links -->

[lnk-sb-queues-node]: ../service-bus-messaging/service-bus-nodejs-how-to-use-queues.md

[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[IOT Hub Geliştirici Kılavuzu]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[IOT Hub ile çalışmaya başlama]: iot-hub-node-node-getstarted.md
[Azure IOT Geliştirme Merkezi]: https://azure.microsoft.com/develop/iot
[geçici hata işleme]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Geçici hata işleme]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-c2d]: iot-hub-node-node-c2d.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-free-trial]: https://azure.microsoft.com/free/
[lnk-storage]: https://docs.microsoft.com/en-us/azure/storage/