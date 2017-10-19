---
title: "Azure IoT Hub'ı (Node) kullanmaya başlama | Microsoft Docs"
description: "Node.js için IoT SDK’larını kullanarak Azure IoT Hub’a cihazdan buluta ileti göndermeyi öğrenin. IoT hub’a cihazınızı kaydetmek, ileti göndermek ve ileti okumak için sanal cihaz ve hizmet uygulamaları oluşturun."
services: iot-hub
documentationcenter: nodejs
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 56618522-9a31-42c6-94bf-55e2233b39ac
ms.service: iot-hub
ms.devlang: javascript
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f3398e38cf7d3d28d9ca4edef5a9bca96aeaf2ab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="connect-your-simulated-device-to-your-iot-hub-using-node"></a>Node kullanarak sanal cihazınızı IoT hub’ınıza bağlama

[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Bu öğreticinin sonunda üç Node.js konsol uygulamanız olacak:

* Bir cihaz kimliği ve sanal cihaz uygulamanızı bağlamak için ilişkili güvenlik anahtarı oluşturan **CreateDeviceIdentity.js**.
* Sanal cihaz uygulamanız tarafından gönderilen telemetriyi gösteren **ReadDeviceToCloudMessages.js**.
* Daha önce oluşturulan cihaz kimliğiyle IoT hub'ınıza bağlanan ve MQTT protokolünü kullanarak her saniye bir telemetri iletisi gönderen **SimulatedDevice.js**.

> [!NOTE]
> [IoT Hub SDK'ları][lnk-hub-sdks] makalesi, hem cihazlarınızda hem de çözüm arka ucunuzda çalıştırılacak uygulamalar oluşturmak için kullanabileceğiniz Azure IoT SDK’ları hakkında bilgi içerir.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Node.js 4.0.x sürümü veya sonraki bir sürüm.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

IoT Hub’ınızı oluşturdunuz. Bu öğreticinin geri kalanını tamamlamak için gereken IoT Hub ana bilgisayar adına ve IoT Hub bağlantı dizesine sahipsiniz.

## <a name="create-a-device-identity"></a>Cihaz kimliği oluşturma

Bu bölümde, IoT hub'ınızdaki kimlik kayıt defterinde bir cihaz kimliği oluşturan bir Node.js konsol uygulaması oluşturacaksınız. Yalnızca kimlik kayıt defterinde girişi olan cihazlar IoT hub'ına bağlanabilir. Daha fazla bilgi için [IoT Hub Geliştirici Kılavuzu][lnk-devguide-identity]'nun **Kimlik Kayıt Defteri** bölümüne bakın. Cihazınızın-bulut iletileri gönderirken kendisini tanıtmak için kullandığı anahtarı ve benzersiz cihaz kimliğini oluşturmak için bu uygulamayı çalıştırın.

1. `createdeviceidentity` adlı yeni bir boş klasör oluşturun. Komut isteminizde aşağıdaki komutu kullanarak `createdeviceidentity` klasöründe bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:

    ```cmd/sh
    npm init
    ```

2. Komut isteminizde `createdeviceidentity` klasöründe `azure-iothub` Hizmet SDK paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    npm install azure-iothub --save
    ```

3. Bir metin düzenleyicisi kullanarak `createdeviceidentity` klasöründe bir **CreateDeviceIdentity.js** dosyası oluşturun.

4. Aşağıdaki `require` deyimini **CreateDeviceIdentity.js** dosyasının başlangıcına ekleyin:

    ```nodejs
    'use strict';

    var iothub = require('azure-iothub');
    ```

5. Aşağıdaki kodları **CreateDeviceIdentity.js** dosyasına ekleyin: Yer tutucu değerini, önceki bölümde hub için oluşturduğunuz IoT Hub bağlantı dizesiyle değiştirin.

    ```nodejs
    var connectionString = '{iothub connection string}';

    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```

6. IoT hub'ınızın kimlik kayıt defterinde bir cihaz tanımı oluşturmak için aşağıdaki kodu ekleyin. Bu kod, cihaz kimliği kayıt defterinde yoksa bir cihaz oluşturur, aksi halde var olan cihazın anahtarını döndürür:

    ```nodejs
    var device = {
      deviceId: 'myFirstNodeDevice'
    }
    registry.create(device, function(err, deviceInfo, res) {
      if (err) {
        registry.get(device.deviceId, printDeviceInfo);
      }
      if (deviceInfo) {
        printDeviceInfo(err, deviceInfo, res)
      }
    });

    function printDeviceInfo(err, deviceInfo, res) {
      if (deviceInfo) {
        console.log('Device ID: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.symmetricKey.primaryKey);
      }
    }
    ```

   [!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

7. **CreateDeviceIdentity.js** dosyasını kaydedin ve kapatın.

8. `createdeviceidentity` uygulamasını çalıştırmak için `createdeviceidentity` klasöründeki komut isteminde aşağıdaki komutu yürütün:

    ```cmd/sh
    node CreateDeviceIdentity.js 
    ```

9. **Cihaz kimliği** ve **Cihaz anahtarını** not edin. İleride IoT Hub'a bir cihaz olarak bağlanan bir uygulama oluşturduğunuzda bu değerlere ihtiyacınız olur.

> [!NOTE]
> IoT Hub kimlik kayıt defteri, yalnızca IoT hub'ına güvenli erişim sağlamak amacıyla cihaz kimliklerini depolar. Güvenlik kimlik bilgileri olarak kullanılmak üzere cihaz kimliklerini ve anahtarlarını ve tek bir cihaza erişimi devre dışı bırakmak için kullanabileceğiniz etkin/devre dışı bayrağını depolar. Uygulamanızın cihaza özgü diğer meta verileri depolaması gerekiyorsa uygulamaya özgü bir depo kullanması gerekir. Daha fazla bilgi için bkz. [IoT Hub geliştirici kılavuzu][lnk-devguide-identity].

<a id="D2C_node"></a>
## <a name="receive-device-to-cloud-messages"></a>Cihazdan buluta iletileri alma

Bu bölümde, IoT Hub'dan cihazdan buluta iletiler okuyan bir Node.js konsol uygulaması oluşturacaksınız. IoT hub'ı, cihazdan buluta iletilerini okumanızı sağlamak için [Event Hubs][lnk-event-hubs-overview] ile uyumlu bir uç noktasını kullanıma sunar. Sade ve basit bir anlatım gözetildiği için bu öğretici yüksek işleme dağıtımına uygun olmayan temel bir okuyucu oluşturur. [Cihazdan buluta iletileri işleme][lnk-process-d2c-tutorial] öğreticisi, cihazdan buluta iletilerin ölçekli olarak nasıl işleneceğini gösterir. [Event Hubs ile Çalışmaya Başlama][lnk-eventhubs-tutorial] öğreticisi, IoT Hub ve Event Hub ile uyumlu uç noktalar için geçerlidir.

> [!NOTE]
> Cihazdan buluta iletileri okumak için Event Hub ile uyumlu uç nokta her zaman AMQP protokolünü kullanır.

1. `readdevicetocloudmessages` adlı bir boş klasör oluşturun. Komut isteminizde aşağıdaki komutu kullanarak `readdevicetocloudmessages` klasöründe bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:

    ```cmd/sh
    npm init
    ```

2. Komut isteminizde `readdevicetocloudmessages` klasöründe **azure-event-hubs** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    npm install azure-event-hubs --save
    ```

3. Bir metin düzenleyicisi kullanarak `readdevicetocloudmessages` klasöründe bir **ReadDeviceToCloudMessages.js** dosyası oluşturun.

4. Aşağıdaki `require` deyimlerini **ReadDeviceToCloudMessages.js** dosyasının başlangıcına ekleyin:

    ```nodejs
    'use strict';

    var EventHubClient = require('azure-event-hubs').Client;
    ```

5. Aşağıdaki değişken bildirimini ekleyin ve yer tutucu değerini hub'ınızın IoT Hub'ı bağlantı dizesiyle değiştirin:

    ```nodejs
    var connectionString = '{iothub connection string}';
    ```

6. Konsola çıktı yazdıran aşağıdaki iki işlevi ekleyin:

    ```nodejs
    var printError = function (err) {
      console.log(err.message);
    };

    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```

7. **EventHubClient** oluşturmak, IoT Hub bağlantısını açmak ve her bölüme yönelik bir alıcı oluşturmak için aşağıdaki kodu ekleyin. Bu uygulama alıcı oluştururken bir filtre kullanır, böylece bir alıcı çalışmaya başladıktan sonra IoT Hub'a gönderilen iletileri yalnızca okur. Bu filtre, yalnızca geçerli ileti kümesini görmeniz açısından bir test ortamında kullanışlıdır. Bir üretim ortamında, kodunuzun tüm iletileri işlediğinden emin olmanız gerekir. Daha fazla bilgi için [IoT Hub cihazdan buluta iletiler nasıl işlenir?][lnk-process-d2c-tutorial] öğreticisine bakın:

    ```nodejs
    var client = EventHubClient.fromConnectionString(connectionString);
    client.open()
        .then(client.getPartitionIds.bind(client))
        .then(function (partitionIds) {
            return partitionIds.map(function (partitionId) {
                return client.createReceiver('$Default', partitionId, { 'startAfterTime' : Date.now()}).then(function(receiver) {
                    console.log('Created partition receiver: ' + partitionId)
                    receiver.on('errorReceived', printError);
                    receiver.on('message', printMessage);
                });
            });
        })
        .catch(printError);
    ```

8. **ReadDeviceToCloudMessages.js** dosyasını kaydedin ve kapatın.

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma

Bu bölümde, bir IoT hub'a cihazdan buluta iletiler gönderen bir cihaza benzetim yapan bir Node.js konsol uygulaması oluşturacaksınız.

1. `simulateddevice` adlı bir boş klasör oluşturun. Komut isteminizde aşağıdaki komutu kullanarak `simulateddevice` klasöründe bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:

    ```cmd/sh
    npm init
    ```

2. Komut isteminizde `simulateddevice` klasöründe **azure-iot-device** Cihaz SDK paketini ve **azure-iot-device-mqtt** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

3. Bir metin düzenleyicisi kullanarak `simulateddevice` klasöründe bir **SimulatedDevice.js** dosyası oluşturun.

4. Aşağıdaki `require` deyimlerini **SimulatedDevice.js** dosyasının başlangıcına ekleyin:

    ```nodejs
    'use strict';

    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

5. Bir `connectionString` değişkeni ekleyin ve bir **İstemci** örneği oluşturmak için bunu kullanın. `{youriothostname}` yerine, *IoT Hub oluşturma* bölümünde oluşturduğunuz IoT hub'ın adını girin. `{yourdevicekey}` yerine, *Cihaz kimliği oluşturma* bölümünde oluşturduğunuz cihaz anahtarı değerini girin:

    ```nodejs
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';

    var client = clientFromConnectionString(connectionString);
    ```

6. Uygulamadan çıktı görüntülemek için aşağıdaki işlevi ekleyin:

    ```nodejs
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Bir geri çağırma oluşturun ve IoT hub'ınıza her saniye bir ileti göndermek için **setInterval** işlevini kullanın:

    ```nodejs
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');

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

8. IoT Hub'ınıza bağlantıyı açın ve iletileri göndermeye başlayın:

    ```nodejs
    client.open(connectCallback);
    ```

9. **SimulatedDevice.js** dosyasını kaydedin ve kapatın.

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. [Geçici Hata İşleme][lnk-transient-faults] adlı MSDN makalesinde önerildiği üzere, üretim kodunda yeniden deneme ilkelerini (üstel geri alma gibi) uygulamanız gerekir.

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. `readdevicetocloudmessages` klasöründeki bir komut isteminde IoT hub'ınızı izlemeye başlamak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    node ReadDeviceToCloudMessages.js 
    ```

    ![Cihazdan buluta iletileri izlemeye yönelik Node.js IoT Hub hizmet uygulaması][7]

2. `simulateddevice` klasöründeki bir komut isteminde IoT hub'ınıza telemetri verileri göndermeye başlamak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    node SimulatedDevice.js
    ```

    ![Cihazdan buluta iletileri göndermeye yönelik Node.js IoT Hub cihaz uygulaması][8]

3. [Azure portalındaki][lnk-portal] **Kullanım** kutucuğu, IoT hub'ına gönderilen ileti sayısını gösterir:

    ![IoT Hub’a gönderilen ileti sayısını gösteren Azure portalı Kullanım kutucuğu][43]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Bu cihaz kimliğini, sanal cihaz uygulamasının, IoT hub'ına cihazdan buluta iletileri göndermesini sağlamak için kullandınız. Ayrıca, IoT hub’ı tarafından alınan iletileri görüntüleyen bir uygulama da oluşturdunuz.

IoT Hub’ı kullanmaya başlamak ve diğer IoT senaryolarını keşfetmek için bkz:

* [Cihazınızı bağlama][lnk-connect-device]
* [Cihaz yönetimini kullanmaya başlama][lnk-device-management]
* [Azure IoT Edge’i kullanmaya başlama][lnk-iot-edge]

IoT çözümünüzün nasıl genişletileceğini ve cihazdan buluta iletilerin doğru ölçekte nasıl işleneceğini öğrenmek için [Cihazdan buluta iletileri işleme][lnk-process-d2c-tutorial] öğreticisine bakın.
[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
