<properties
    pageTitle="Node.js için Azure IoT Hub'ı kullanmaya başlama | Microsoft Azure"
    description="Node.js ile Azure IoT Hub'ı kullanmaya başlama öğreticisi. Bir Nesnelerin İnterneti çözümü uygulamak için Microsoft Azure IoT SDK'ları ile Azure IoT Hub ve Node.js kullanın."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>


# Node.js için Azure IoT Hub'ı kullanmaya başlayın

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Bu öğreticinin sonunda üç Node.js konsol uygulamanız olacak:

* Bir cihaz kimliği ve sanal cihazınızı bağlamak için ilişkili güvenlik anahtarı oluşturan **CreateDeviceIdentity.js**.
* Sanal cihazınız tarafından gönderilen telemetriyi gösteren **ReadDeviceToCloudMessages.js**.
* Daha önce oluşturulan cihaz kimliğiyle IoT hub'ınızı bağlayan ve AMQPS protokolünü kullanarak her saniye bir telemetri iletisi gönderen **SimulatedDevice.js**.

> [AZURE.NOTE] [IoT Hub SDK'ları][lnk-hub-sdks] makalesi, cihazlarda çalıştırmak için her iki uygulamayı da derlemek amacıyla kullanabileceğiniz çeşitli SDK'lar ve çözüm arka ucunuz hakkında bilgi sağlar.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

+ Node.js 0.12.x sürümü veya sonraki bir sürüm. <br/> [Geliştirme ortamınızı hazırlama][lnk-dev-setup], bu öğretici için Node.js'nin Windows veya Linux'ta nasıl yükleneceğini açıklar.

+ Etkin bir Azure hesabı. (Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü][lnk-free-trial].)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

IoT Hub’ınızı oluşturdunuz. Bu öğreticinin geri kalanını tamamlamak için gereken IoT Hub ana bilgisayar adına ve IoT Hub bağlantı dizesine sahipsiniz.

## Cihaz kimliği oluşturma

Bu bölümde, IoT hub'ınızdaki kimlik kayıt defterinde bir cihaz kimliği oluşturan bir Node.js konsol uygulaması oluşturacaksınız. Cihaz kimlik kayıt defterinde girişi olmayan bir cihaz IoT hub'ına bağlanamaz. Daha fazla bilgi için [IoT Hub Geliştirici Kılavuzu][lnk-devguide-identity] konumundaki **Cihaz Kimlik Kayıt Defteri** bölümüne bakın. Bu konsol uygulamasını çalıştırdığınızda, cihazınızın IoT Hub'a cihaz-bulut iletileri gönderdiğinde kendisini tanımlamak için kullanabileceği benzersiz bir cihaz kimliği ve anahtarı oluşturulur.

1. **createdeviceidentity** adlı yeni bir boş klasör oluşturun. Komut isteminizde aşağıdaki komutu kullanarak **createdeviceidentity** klasöründe bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:

    ```
    npm init
    ```

2. Komut isteminizde **createdeviceidentity** klasöründe **azure-iothub** Hizmet SDK paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```
    npm install azure-iothub --save
    ```

3. Bir metin düzenleyicisi kullanarak **createdeviceidentity** klasöründe bir **CreateDeviceIdentity.js** dosyası oluşturun.

4. Aşağıdaki `require` deyimini **CreateDeviceIdentity.js** dosyasının başlangıcına ekleyin:

    ```
    'use strict';
    
    var iothub = require('azure-iothub');
    ```

5. Aşağıdaki kodu **CreateDeviceIdentity.js** dosyasına ekleyin ve yer tutucu değerini önceki bölümde oluşturduğunuz IoT hub'ının bağlantı dizesiyle değiştirin: 

    ```
    var connectionString = '{iothub connection string}';
    
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```

6. IoT hub'ınızın cihaz kimlik kayıt defterinde bir cihaz tanımı oluşturmak için aşağıdaki kodu ekleyin. Bu kod, cihaz kimliği kayıt defterinde yoksa bir cihaz oluşturur, aksi halde var olan cihazın anahtarını döndürür:

    ```
    var device = new iothub.Device(null);
    device.deviceId = 'myFirstNodeDevice';
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
        console.log('Device id: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.SymmetricKey.primaryKey);
      }
    }
    ```

7. **CreateDeviceIdentity.js** dosyasını kaydedin ve kapatın.

8. **createdeviceidentity** uygulamasını çalıştırmak için, createdeviceidentity klasöründeki komut isteminde aşağıdaki komutu yürütün:

    ```
    node CreateDeviceIdentity.js 
    ```

9. **Cihaz kimliği** ve **Cihaz anahtarını** not edin. İleride IoT Hub'a bir cihaz olarak bağlanan bir uygulama oluşturduğunuzda bu değerlere ihtiyacınız olur.

> [AZURE.NOTE] IoT Hub kimlik kayıt defteri, yalnızca hub'a güvenli erişim sağlamak amacıyla cihaz kimliklerini depolar. Güvenlik kimlik bilgileri olarak kullanılmak üzere cihaz kimliklerini ve anahtarlarını ve tek bir cihaza erişimi devre dışı bırakmak için kullanabileceğiniz etkin/devre dışı bayrağını depolar. Uygulamanızın cihaza özgü diğer meta verileri depolaması gerekiyorsa uygulamaya özgü bir depo kullanması gerekir. Daha fazla bilgi için bkz. [IoT Hub Geliştirici Kılavuzu][lnk-devguide-identity].

## Cihazdan buluta iletileri alma

Bu bölümde, IoT Hub'dan cihazdan buluta iletiler okuyan bir Node.js konsol uygulaması oluşturacaksınız. IoT hub'ı, cihaz bulut iletilerini okumanızı sağlamak için [Event Hubs][lnk-event-hubs-overview] ile uyumlu bir uç noktasını kullanıma sunar. Sade ve basit bir anlatım gözetildiği için bu öğretici yüksek işleme dağıtımına uygun olmayan temel bir okuyucu oluşturur. [Cihazdan buluta iletileri işleme][lnk-process-d2c-tutorial] öğreticisi, cihazdan buluta iletilerin ölçekli olarak nasıl işleneceğini gösterir. [Event Hubs ile Çalışmaya Başlama][lnk-eventhubs-tutorial] öğreticisi, Event Hubs'dan alınan iletilerin nasıl işleneceği hakkında daha fazla bilgi sağlar; IoT Hub Event Hubs ile uyumlu uç noktalar için geçerlidir.

> [AZURE.NOTE] Cihazdan buluta iletileri okumak için Event Hubs ile uyumlu uç nokta her zaman AMQPS protokolünü kullanır.

1. **readdevicetocloudmessages** adlı yeni bir boş klasör oluşturun. Komut isteminizde aşağıdaki komutu kullanarak **readdevicetocloudmessages** klasöründe bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:

    ```
    npm init
    ```

2. Komut isteminizde **readdevicetocloudmessages** klasöründe **azure-event-hubs** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```
    npm install azure-event-hubs --save
    ```

3. Bir metin düzenleyicisi kullanarak **readdevicetocloudmessages** klasöründe bir **ReadDeviceToCloudMessages.js** dosyası oluşturun.

4. Aşağıdaki `require` deyimlerini **ReadDeviceToCloudMessages.js** dosyasının başlangıcına ekleyin:

    ```
    'use strict';

    var EventHubClient = require('azure-event-hubs').Client;
    ```

5. Aşağıdaki değişken bildirimini ekleyin ve yer tutucu değerini IoT hub bağlantı dizenizle değiştirin:

    ```
    var connectionString = '{iothub connection string}';
    ```

6. Konsola çıktı yazdıran aşağıdaki iki işlevi ekleyin:

    ```
    var printError = function (err) {
      console.log(err.message);
    };

    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```

7. **EventHubClient** oluşturmak, IoT Hub bağlantısını açmak ve her bölüme yönelik bir alıcı oluşturmak için aşağıdaki kodu ekleyin. Bu uygulama alıcı oluştururken bir filtre kullanır, böylece bir alıcı çalışmaya başladıktan sonra IoT Hub'a gönderilen iletileri yalnızca okur. Bu filtre, yalnızca geçerli ileti kümesini görmeniz açısından bir test ortamında kullanışlıdır. Bir üretim ortamında kodunuzun tüm iletileri işlediğinden emin olmanız gerekir. Daha fazla bilgi için [IoT Hub cihazdan buluta iletileri işleme][lnk-process-d2c-tutorial] öğreticisine bakın:

    ```
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

## Sanal cihaz uygulaması oluşturma

Bu bölümde, bir IoT hub'a cihazdan buluta iletiler gönderen bir cihaza benzetim yapan bir Node.js konsol uygulaması oluşturacaksınız.

1. **simulateddevice** adlı yeni bir boş klasör oluşturun. Komut isteminizde aşağıdaki komutu kullanarak **simulateddevice** klasöründe bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:

    ```
    npm init
    ```

2. Komut isteminizde **simulateddevice** klasöründe **azure-iot-device** Cihaz SDK paketini ve **azure-iot-device-amqp** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```
    npm install azure-iot-device azure-iot-device-amqp --save
    ```

3. Bir metin düzenleyicisi kullanarak **simulateddevice** klasöründe yeni bir **SimulatedDevice.js** dosyası oluşturun.

4. Aşağıdaki `require` deyimlerini **SimulatedDevice.js** dosyasının başlangıcına ekleyin:

    ```
    'use strict';

    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

5. Bir **connectionString** değişkeni ekleyin ve bir cihaz istemcisi oluşturmak için bunu kullanın. **{youriothostname}** yerine, *IoT Hub oluşturma* bölümünde oluşturduğunuz IoT hub'ın adını girin. **{yourdevicekey}** yerine, *Cihaz kimliği oluşturma* bölümünde oluşturduğunuz cihaz anahtarı değerini girin:

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
    
    var client = clientFromConnectionString(connectionString);
    ```

6. Uygulamadan çıktı görüntülemek için aşağıdaki işlevi ekleyin:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Bir geri çağırma oluşturun ve IoT hub'ınıza her saniye yeni bir ileti göndermek için **setInterval** işlevini kullanın.

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');

        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var windSpeed = 10 + (Math.random() * 4);
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', windSpeed: windSpeed });
            var message = new Message(data);
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```

8. IoT Hub'ınıza bağlantıyı açın ve iletileri göndermeye başlayın:

    ```
    client.open(connectCallback);
    ```

9. **SimulatedDevice.js** dosyasını kaydedin ve kapatın.

> [AZURE.NOTE] Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. [Geçici Hata İşleme][lnk-transient-faults] adlı MSDN makalesinde önerildiği üzere, üretim kodunda yeniden deneme ilkelerini (üstel geri alma gibi) uygulamanız gerekir.


## Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. **readdevicetocloudmessages** klasöründeki bir komut isteminde IoT hub'ınızı izlemeye başlamak için aşağıdaki komutu çalıştırın:

    ```
    node ReadDeviceToCloudMessages.js 
    ```

    ![][7]

2. **simulateddevice** klasöründeki bir komut isteminde IoT hub'ınıza telemetri verileri göndermeye başlamak için aşağıdaki komutu çalıştırın:

    ```
    node SimulatedDevice.js
    ```

    ![][8]

3. [Azure portalındaki][lnk-portal] **Kullanım** kutucuğu, hub'a gönderilen ileti sayısını gösterir:

    ![][43]

## Sonraki adımlar

Bu öğreticide, portalda yeni bir IoT hub'ı yapılandırdınız ve ardından hub'ın kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Bu cihaz kimliğini, sanal cihaz uygulamasının hub'a cihaz-bulut iletileri göndermesini sağlamak için kullandınız. Hub tarafından alınan iletileri görüntüleyen bir uygulama da oluşturdunuz. 

IoT Hub’ı kullanmaya başlamak ve diğer IoT senaryolarını keşfetmek için bkz:

- [Cihazınızı bağlama][lnk-connect-device]
- [Cihaz yönetimi ile çalışmaya başlama][lnk-device-management]
- [Ağ Geçidi SDK’sı ile çalışmaya başlama][lnk-gateway-SDK]

IoT çözümünüzün nasıl genişletileceğini ve cihazdan buluta iletilerin doğru ölçekte nasıl işleneceğini öğrenmek için [Cihazdan buluta iletileri işleme][lnk-process-d2c-tutorial] öğreticisine bakın.

<!-- Images. -->
[6]: ./media/iot-hub-node-node-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide.md#identityregistry
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-sdks-summary.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/



<!--HONumber=Sep16_HO3-->


