<properties
    pageTitle="Node.js için Azure IoT Hub'ı kullanmaya başlama | Microsoft Azure"
    description="Node.js ile Azure IoT Hub'ı kullanmaya başlama öğreticisi. Bir nesnelerin interneti çözümü uygulamak için Microsoft Azure IoT SDK'ları ile Azure IoT Hub ve Node.js kullanın."
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
     ms.date="03/22/2016"
     ms.author="dobett"/>

# Node.js için Azure IoT Hub'ı kullanmaya başlayın

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

## Giriş

Azure IoT Hub, milyonlarca nesnelerin interneti (IoT) cihazı ile bir çözüm arka ucu arasında güvenilir ve güvenli çift yönlü iletişimler sağlayan tam olarak yönetilen bir hizmettir. IoT projelerinin karşılaştığı en büyük zorluklardan biri, cihazların düzgün ve güvenli bir şekilde çözüm arka ucuna bağlanmasıdır. Bu zorluğu ele almak için IoT Hub:

- Cihazdan buluta ve buluttan cihaza hiper ölçekli güvenilir mesajlaşma olanağı sunar.
- Cihaz başına güvenlik kimlik bilgisi ve erişim denetimi kullanarak güvenli iletişimlere olanak sağlar.
- En popüler diller ve platformlar için cihaz kitaplıklarını içerir.

Bu öğretici şunların nasıl yapıldığını gösterir:

- IoT hub'ı oluşturmak için Azure portalını kullanma.
- IoT hub'ınızda bir cihaz kimliği oluşturma.
- Bulut arka ucunuza telemetri gönderen sanal bir cihaz oluşturma.

Bu öğreticinin sonunda üç Node.js konsol uygulamanız olacak:

* Bir cihaz kimliği ve sanal cihazınızı bağlamak için ilişkili güvenlik anahtarı oluşturan **CreateDeviceIdentity.js**.
* Sanal cihazınız tarafından gönderilen telemetriyi görüntüleyen **ReadDEviceToCloudMessages.js**.
* Daha önce oluşturulan cihaz kimliğiyle IoT hub'ınızı bağlayan ve AMQPS protokolünü kullanarak her saniye bir telemetri iletisi gönderen **SimulatedDevice.js**.

> [AZURE.NOTE] [IoT Hub SDK'ları][lnk-hub-sdks] makalesi, cihazlarda çalıştırmak için her iki uygulamayı da derlemek amacıyla kullanabileceğiniz çeşitli SDK'lar ve çözüm arka ucunuz hakkında bilgi sağlar.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

+ Node.js 0.12.x sürümü veya sonraki bir sürüm. <br/> [Geliştirme ortamınızı hazırlama][lnk-dev-setup], bu öğretici için Node.js'nin Windows veya Linux'ta nasıl yükleneceğini açıklar.

+ Etkin bir Azure hesabı. <br/>Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü][lnk-free-trial].

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Son adım olarak, IoT Hub dikey penceresinde **Ayarlar**'a tıklayın, ardından **Ayarlar** dikey penceresinde **Mesajlaşma**'ya tıklayın. **Mesajlaşma** dikey penceresinde **Event Hub ile uyumlu adı** ve **Event Hub ile uyumlu uç noktasını** not edin. Bu değerler, **read-d2c-messages** uygulamanızı oluştururken gerekir.

![][6]

Artık IoT hub'ınızı oluşturdunuz ve bu öğreticinin geri kalanını tamamlamak için ihtiyacınız olan IoT Hub ana bilgisayar adına, IoT Hub bağlantı dizesine, Event Hub ile uyumlu ada ve Event-Hub ile uyumlu uç noktası değerlerine sahipsiniz.

## Cihaz kimliği oluşturma

Bu bölümde IoT hub'ınızdaki kimlik kayıt defterinde yeni bir cihaz kimliği oluşturan bir Node.js konsol uygulaması oluşturacaksınız. Cihaz kimlik kayıt defterinde girişi olmayan bir cihaz IoT hub'ına bağlanamaz. Daha fazla bilgi için [IoT Hub Geliştirici Kılavuzu][lnk-devguide-identity] konumundaki **Cihaz Kimlik Kayıt Defteri** bölümüne bakın. Bu konsol uygulamasını çalıştırdığınızda, cihazınızın IoT Hub'a cihazdan buluta ileti gönderdiğinde kendisini tanımlayabileceği benzersiz bir cihaz kimliği ve anahtarı oluşturulur.

1. **createdeviceidentity** adlı yeni bir boş klasör oluşturun. Komut isteminizde aşağıdaki komutu kullanarak **createdeviceidentity** klasöründe yeni bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:

    ```
    npm init
    ```

2. Komut isteminizde **createdeviceidentity** klasöründe **azure-iothub** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```
    npm install azure-iothub --save
    ```

3. Bir metin düzenleyicisini kullanarak **createdeviceidentity** klasöründe yeni bir **CreateDeviceIdentity.js** dosyası oluşturun.

4. Aşağıdaki `require` deyimini **CreateDeviceIdentity.js** dosyasının başlangıcına ekleyin:

    ```
    'use strict';
    
    var iothub = require('azure-iothub');
    ```

5. Aşağıdaki kodu **CreateDeviceIdentity.js** dosyasına ekleyerek yer tutucu değerini önceki bölümde oluşturduğunuz IoT hub'ının bağlantı dizesiyle değiştirin: 

    ```
    var connectionString = '{iothub connection string}';
    
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```

6. IoT hub'ınızın cihaz kimlik kayıt defterinde yeni bir cihaz tanımı oluşturmak için aşağıdaki kodu ekleyin. Bu kod, cihaz kimliği kayıt defterinde yoksa yeni bir cihaz oluşturur, aksi halde var olan cihazın anahtarını döndürür:

    ```
    var device = new iothub.Device(null);
    device.deviceId = 'myFirstDevice';
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

8. **create-device-identity** uygulamasını çalıştırmak için, createdeviceidentity klasöründeki komut isteminde aşağıdaki komutu yürütün:

    ```
    node CreateDeviceIdentity.js 
    ```

9. **Cihaz kimliği** ve **Cihaz anahtarını** not edin. IoT Hub'a bir cihaz olarak bağlanan bir uygulama oluşturduğunuzda bunlara ihtiyacınız olacak.

> [AZURE.NOTE] IoT Hub kimlik kayıt defteri, yalnızca hub'a güvenli erişim sağlamak amacıyla cihaz kimliklerini depolar. Güvenlik kimlik bilgileri olarak kullanılmak üzere cihaz kimliklerini ve anahtarlarını ve tek bir cihaza erişimi devre dışı bırakmanızı sağlayan etkin/devre dışı bayrağını depolar. Uygulamanızın cihaza özgü diğer meta verileri depolaması gerekiyorsa uygulamaya özgü bir depo kullanması gerekir. Daha fazla bilgi için bkz. [IoT Hub Geliştirici Kılavuzu][lnk-devguide-identity].

## Cihazdan buluta iletileri alma

Bu bölümde IoT Hub'dan cihazdan buluta iletileri okuyan bir Node.js konsol uygulaması oluşturacaksınız. IoT hub'ı, cihaz bulut iletilerini okumanızı sağlamak için [Event Hubs][lnk-event-hubs-overview] ile uyumlu bir uç noktasını kullanıma sunar. Sade ve basit bir anlatım gözetildiği için bu öğretici yüksek işleme dağıtımına uygun olmayan temel bir okuyucu oluşturur. [Cihazdan buluta iletileri işleme][lnk-process-d2c-tutorial] öğreticisi, cihazdan buluta iletilerin ölçekli olarak nasıl işleneceğini gösterir. [Event Hubs ile Çalışmaya Başlama][lnk-eventhubs-tutorial] öğreticisi, Event Hubs'dan alınan iletilerin nasıl işleneceği hakkında daha fazla bilgi sağlar; IoT Hub ve Event Hub ile uyumlu uç noktalar için geçerlidir.

> [AZURE.NOTE] Cihazdan buluta iletileri okumak için Event Hubs ile uyumlu uç nokta her zaman AMQPS protokolünü kullanır.

1. **readdevicetocloudmessages** adlı yeni bir boş klasör oluşturun. Komut isteminizde aşağıdaki komutu kullanarak **readdevicetocloudmessages** klasöründe yeni bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:

    ```
    npm init
    ```

2. Komut isteminizde **readdevicetocloudmessages** klasöründe **amqp10** ve **bluebird** paketlerini yüklemek için aşağıdaki komutu çalıştırın:

    ```
    npm install amqp10 bluebird --save
    ```

3. Bir metin düzenleyicisini kullanarak **readdevicetocloudmessages** klasöründe yeni bir **ReadDeviceToCloudMessages.js** dosyası oluşturun.

4. Aşağıdaki `require` deyimlerini **ReadDeviceToCloudMessages.js** dosyasının başlangıcına ekleyin:

    ```
    'use strict';

    var AMQPClient = require('amqp10').Client;
    var Policy = require('amqp10').Policy;
    var translator = require('amqp10').translator;
    var Promise = require('bluebird');
    ```

5. Aşağıdaki değişken bildirimlerini ekleyip yer tutucuları daha önce not ettiğiniz değerlerle değiştirin. **{your event hub-compatible namespace}** yer tutucunuzun değeri, portaldaki **Event Hub ile uyumlu uç noktası** alanından gelir; **namespace.servicebus.windows.net** formundadır (*sb://* ön eki olmadan).

    ```
    var protocol = 'amqps';
    var eventHubHost = '{your event hub-compatible namespace}';
    var sasName = 'iothubowner';
    var sasKey = '{your iot hub key}';
    var eventHubName = '{your event hub-compatible name}';
    var numPartitions = 2;
    ```

    > [AZURE.NOTE] Bu kod, IoT hub'ınızı F1 (ücretsiz) katmanında oluşturduğunuzu varsayar. Ücretsiz IoT hub'ının "0" ve "1" adlı iki bölümü vardır. IoT hub'ınızı diğer fiyatlandırma katmanlarından birini kullanarak oluşturduysanız her bir bölüm için bir **MessageReceiver** oluşturacak şekilde kodu değiştirmeniz gerekir.

6. Aşağıdaki filtre tanımını ekleyin. Bu uygulama alıcı oluştururken bir filtre kullanır, böylece bir alıcı çalışmaya başladıktan sonra IoT Hub'a gönderilen iletileri yalnızca okur. Geçerli iletiler kümesini görebileceğiniz için bu bir test ortamında kullanışlıdır ancak bir üretim ortamında kodunuzun tüm iletileri işlediğinden emin olmanız gerekir. Daha fazla bilgi için [IoT Hub cihazdan buluta iletileri işleme][lnk-process-d2c-tutorial] öğreticisine bakın.

    ```
    var filterOffset = new Date().getTime();
    var filterOption;
    if (filterOffset) {
      filterOption = {
      attach: { source: { filter: {
      'apache.org:selector-filter:string': translator(
        ['described', ['symbol', 'apache.org:selector-filter:string'], ['string', "amqp.annotation.x-opt-enqueuedtimeutc > " + filterOffset + ""]])
        } } }
      };
    }
    ```

7. Alma adresi ve bir AMQP istemcisi oluşturmak için aşağıdaki kodu ekleyin:

    ```
    var uri = protocol + '://' + encodeURIComponent(sasName) + ':' + encodeURIComponent(sasKey) + '@' + eventHubHost;
    var recvAddr = eventHubName + '/ConsumerGroups/$default/Partitions/';
    
    var client = new AMQPClient(Policy.EventHub);
    ```

8. Konsola çıktı yazdıran aşağıdaki iki işlevi ekleyin:

    ```
    var messageHandler = function (partitionId, message) {
      console.log('Received(' + partitionId + '): ', message.body);
    };
    
    var errorHandler = function(partitionId, err) {
      console.warn('** Receive error: ', err);
    };
    ```

9. Filtre kullanarak belirli bir bölüm için alıcı görevi gören aşağıdaki işlevi ekleyin:

    ```
    var createPartitionReceiver = function(partitionId, receiveAddress, filterOption) {
      return client.createReceiver(receiveAddress, filterOption)
        .then(function (receiver) {
          console.log('Listening on partition: ' + partitionId);
          receiver.on('message', messageHandler.bind(null, partitionId));
          receiver.on('errorReceived', errorHandler.bind(null, partitionId));
        });
    };
    ```

10. Event Hub ile uyumlu uç noktasını bağlamak ve alıcıları başlatmak için aşağıdaki kodu ekleyin:

    ```
    client.connect(uri)
      .then(function () {
        var partitions = [];
        for (var i = 0; i < numPartitions; ++i) {
          partitions.push(createPartitionReceiver(i, recvAddr + i, filterOption));
        }
        return Promise.all(partitions);
    })
    .error(function (e) {
        console.warn('Connection error: ', e);
    });
    ```

11. **ReadDeviceToCloudMessages.js** dosyasını kaydedin ve kapatın.

## Sanal cihaz uygulaması oluşturma

Bu bölümde IoT Hub'a cihazdan buluta iletiler gönderen bir cihaza benzetim yapan bir Node.js konsol uygulaması oluşturacaksınız.

1. **simulateddevice** adlı yeni bir boş klasör oluşturun. Komut isteminizde aşağıdaki komutu kullanarak **simulateddevice** klasöründe yeni bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:

    ```
    npm init
    ```

2. Komut isteminizde **simulateddevice** klasöründe **azure-iot-device-amqp** paketini yüklemek için aşağıdaki komutu çalıştırın:

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

5. Bir **connectionString** değişkeni ekleyin ve bir cihaz istemcisi oluşturmak için bunu kullanın. **{youriothubname}** değerini IoT hub'ınızın adıyla ve **{yourdeviceid}** ve **{yourdevicekey}** değerlerini de *Bir cihaz kimliği oluşturma* bölümünde oluşturduğunuz cihaz değerleriyle değiştirin:

    ```
    var connectionString = 'HostName={youriothubname}.azure-devices.net;DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    
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
            var data = JSON.stringify({ deviceId: 'mydevice', windSpeed: windSpeed });
            var message = new Message(data);
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 2000);
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

Bu öğreticide, portalda yeni bir IoT hub'ı yapılandırdınız ve ardından hub'ın kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Bu cihaz kimliğini, sanal cihaz uygulamasının hub'a cihazdan buluta iletiler göndermesini sağlamak için kullandınız ve hub tarafından alınan iletileri görüntüleyen bir uygulama oluşturdunuz. Aşağıdaki öğreticilerde IoT hub'ının özelliklerini ve diğer IoT senaryolarını keşfetmeye devam edebilirsiniz:

- [IoT Hub ile Buluttan Cihaza iletileri gönderme][lnk-c2d-tutorial] cihazlara iletilerin nasıl gönderildiğini ve IoT Hub tarafından üretilen teslim geri bildiriminin nasıl işlendiğini gösterir.
- [Cihazdan buluta iletileri işleme][lnk-process-d2c-tutorial], cihazlardan gelen telemetrinin ve etkileşimli iletilerin güvenilir şekilde nasıl işlendiğini gösterir.
- [Cihazlardan karşıya dosya yükleme][lnk-upload-tutorial], cihazlardan karşıya dosya yüklemelerini gerçekleştirmek için buluttan cihaza iletilerden yararlanan bir deseni açıklar.

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

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/node/device/doc/devbox_setup.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-upload-tutorial]: iot-hub-csharp-csharp-file-upload.md

[lnk-hub-sdks]: iot-hub-sdks-summary.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/



<!---HONumber=Jun16_HO2-->


