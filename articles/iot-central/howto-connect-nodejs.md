---
title: Bağlantı bir genel Node.js istemci uygulaması için Azure IOT Merkezi | Microsoft Docs
description: Bir aygıt geliştiricisi olarak Azure IOT merkezi uygulamanıza genel Node.js aygıt bağlanma.
services: iot-central
author: tanmaybhagwat
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 8666a2db051cbd4a93c3e587aeaef3e1722b1b83
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="connect-a-generic-client-application-to-your-azure-iot-central-application-nodejs"></a>Azure IOT merkezi uygulamanız (Node.js) genel istemci uygulamaya bağlama

Bu makalede Microsoft Azure IOT merkezi uygulamanıza fiziksel bir aygıtı temsil eden bir genel Node.js uygulaması bağlanmak için bir aygıt geliştiricisi olarak nasıl.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

1. Azure IOT merkezi bir uygulama. Daha fazla bilgi için bkz: [Azure IOT merkezi uygulamanızı oluşturma](howto-create-application.md).
1. Bir geliştirme makineyle [Node.js](https://nodejs.org/) sürüm 4.0.0 veya sonraki bir sürümü yüklü. Çalıştırabilirsiniz `node --version` sürümünüzü denetlemek için komut satırında. Node.js çok çeşitli işletim sistemleri için kullanılabilir.

Azure IOT merkezi uygulamanızda tanımlanan cihaz özellikleri ve aşağıdaki ölçümleri aygıt şablonla gerekir:

### <a name="telemetry-measurements"></a>Telemetri ölçümleri

Aşağıdaki telemetrisi Ekle **ölçümleri** sayfa:

| Görünen ad | Alan Adı  | Birimler | Min | Maks | Ondalık basamaklar |
| ------------ | ----------- | ----- | --- | --- | -------------- |
| Sıcaklık  | Sıcaklık | C     | 60  | 110 | 0              |
| Nem     | nem oranı    | %     | 0   | 100 | 0              |
| Basınç     | basınç    | kPa   | 80  | 110 | 0              |

> [!NOTE]
  Telemetri ölçüm datatype double olur.

Alan adları tam olarak cihaz şablonuna tabloda gösterildiği gibi girin. Alan adları eşleşmiyorsa, telemetri uygulamada görüntülenemiyor.

### <a name="state-measurements"></a>Durum ölçümleri

Şu durumda eklemek **ölçümleri** sayfa:

| Görünen ad | Alan Adı  | Değer 1 | Görünen ad | Değer 2 | Görünen ad |
| ------------ | ----------- | --------| ------------ | ------- | ------------ | 
| Fan modu     | fanmode     | 1       | Çalışıyor      | 0       | Durduruldu      |

> [!NOTE]
  Veri türü durumu ölçüm, bir dizedir.

Alan adları tam olarak cihaz şablonuna tabloda gösterildiği gibi girin. Alan adları eşleşmiyorsa durumu uygulamada görüntülenemiyor.

### <a name="event-measurements"></a>Olay ölçümleri

Aşağıdaki olay eklemek **ölçümleri** sayfa:

| Görünen ad | Alan Adı  | Önem derecesi |
| ------------ | ----------- | -------- |
| Aşırı  | overheat    | Hata    |

> [!NOTE]
  Veri türü olay ölçüm, bir dizedir.

### <a name="device-properties"></a>Cihaz özellikleri

Aşağıdaki cihaz özellikleri ekleyin **Özellikler sayfası**:

| Görünen ad        | Alan Adı        | Veri türü |
| ------------------- | ----------------- | --------- |
| Seri Numarası       | seri numarası      | metin      |
| Aygıt üreticisi | üretici      | metin      |

Alan adları tam olarak cihaz şablonuna tabloda gösterildiği gibi girin. Alan adları eşleşmiyorsa, uygulama özellik değeri gösterilemiyor.

### <a name="settings"></a>Ayarlar

Aşağıdakileri ekleyin **numarası** ayarlarında **Ayarları sayfası**:

| Görünen ad    | Alan Adı     | Birimler | Ondalık basamaklar | Min | Maks  | İlk |
| --------------- | -------------- | ----- | -------- | --- | ---- | ------- |
| Fan hızı       | fanSpeed       | RPM   | 0        | 0   | 3000 | 0       |
| Sıcaklık ayarlayın | setTemperature | C     | 0        | 20  | 200  | 80      |

Alan adı tam olarak cihaz şablonuna tabloda gösterildiği gibi girin. Alan adları eşleşmiyorsa, aygıt ayar değeri alamaz.

### <a name="add-a-real-device"></a>Gerçek bir cihaz ekleme

Azure IOT merkezi uygulamanızda gerçek bir aygıtı Aygıt şablonu oluşturun ve cihaz bağlantı dizesini not ekleyin. Daha fazla bilgi için bkz: [gerçek bir cihazı Azure IOT merkezi uygulamanıza ekleyin](tutorial-add-device.md)

## <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma

Aşağıdaki adımlar uygulamaya eklenen gerçek aygıtı uygulayan bir istemci uygulamasının nasıl oluşturulacağını gösterir.

1. Adlı bir klasör oluşturun `connected-air-conditioner-adv` makinenizde. Komut satırı ortamınızda bu klasöre gidin.

1. Node.js projenizi başlatmak için aşağıdaki komutları çalıştırın:

    ```cmd/sh
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Adlı bir dosya oluşturun **connectedAirConditionerAdv.js** içinde `connected-air-conditioner-adv` klasör.

1. Aşağıdakileri ekleyin `require` deyimleri başlangıcında **connectedAirConditionerAdv.js** dosyası:

    ```javascript
    "use strict";

    // Use the Azure IoT device SDK for devices that connect to Azure IoT Central.
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    ```

1. Aşağıdaki değişken bildirimlerini dosyaya ekleyin:

    ```javascript
    var connectionString = '{your device connection string}';
    var targetTemperature = 0;
    var client = clientFromConnectionString(connectionString);
    ```

    Yer tutucu güncelleştirme `{your device connection string}` cihaz bağlantı dizesine sahip. Gerçek Cihazınızı eklediğinizde bağlantı ayrıntıları sayfasından bu değeri kopyalar. Bu örnekte, biz başlatma `targetTemperature` sıfır olarak, isteğe bağlı olarak aygıttan geçerli okuma veya değeri cihaz çifti alabilir. 

1. Telemetri, durum ve olay ölçümleri Azure IOT merkezi uygulamanıza göndermek için dosyaya aşağıdaki işlevi ekleyin:

    ```javascript
    // Send device measurements.
    function sendTelemetry() {
      var temperature = targetTemperature + (Math.random() * 15);
      var humidity = 70 + (Math.random() * 10);
      var pressure = 90 + (Math.random() * 5);
      var fanmode = 0;
      var data = JSON.stringify({ 
        temperature: temperature, 
        humidity: humidity, 
        pressure: pressure,
        fanmode: (temperature > 25) ? "1" : "0",
        overheat: (temperature > 35) ? "ER123" : undefined });
      var message = new Message(data);
      client.sendEvent(message, (err, res) => console.log(`Sent message: ${message.getData()}` +
        (err ? `; error: ${err.toString()}` : '') +
        (res ? `; status: ${res.constructor.name}` : '')));
    }
    ```

    1. Cihaz özellikleri Azure IOT merkezi uygulamanıza göndermek için dosyanıza aşağıdaki işlevi ekleyin:

    ```javascript
    // Send device properties.
    function sendDeviceProperties(twin) {
      var properties = {
        serialNumber: '123-ABC',
        manufacturer: 'Contoso'
      };
      twin.properties.reported.update(properties, (err) => console.log(`Sent device properties; ` +
        (err ? `error: ${err.toString()}` : `status: success`)));
    }
    ```

1. Cihazınızı yanıtlar ayarları tanımlamak için aşağıdaki tanımını ekleyin:

    ```javascript
    // Add any settings your device supports,
    // mapped to a function that is called when the setting is changed.
    var settings = {
      'fanSpeed': (newValue, callback) => {
          // Simulate it taking 1 second to set the fan speed.
          setTimeout(() => {
            callback(newValue, 'completed');
          }, 1000);
      },
      'setTemperature': (newValue, callback) => {
        // Simulate the temperature setting taking two steps.
        setTimeout(() => {
          targetTemperature = targetTemperature + (newValue - targetTemperature) / 2;
          callback(targetTemperature, 'pending');
          setTimeout(() => {
            targetTemperature = newValue;
            callback(targetTemperature, 'completed');
          }, 5000);
        }, 5000);
      }
    };
    ```

1. Azure IOT merkezi uygulamanızdan güncelleştirilmiş ayarları işlemek için aşağıdakileri dosyaya ekleyin:

    ```javascript
    // Handle settings changes that come from Azure IoT Central via the device twin.
    function handleSettings(twin) {
      twin.on('properties.desired', function (desiredChange) {
        for (let setting in desiredChange) {
          if (settings[setting]) {
            console.log(`Received setting: ${setting}: ${desiredChange[setting].value}`);
            settings[setting](desiredChange[setting].value, (newValue, status, message) => {
              var patch = {
                [setting]: {
                  value: newValue,
                  status: status,
                  desiredVersion: desiredChange.$version,
                  message: message
                }
              }
              twin.properties.reported.update(patch, (err) => console.log(`Sent setting update for ${setting}; ` +
                (err ? `error: ${err.toString()}` : `status: success`)));
            });
          }
        }
      });
    }
    ```

1. Azure IOT Merkezi bağlantısı tamamlamak ve istemci kodu işlevlerde bağlanacağını aşağıdakileri ekleyin:

    ```javascript
    // Handle device connection to Azure IoT Central.
    var connectCallback = (err) => {
      if (err) {
        console.log(`Device could not connect to Azure IoT Central: ${err.toString()}`);
      } else {
        console.log('Device successfully connected to Azure IoT Central');

        // Send telemetry measurements to Azure IoT Central every 1 second.
        setInterval(sendTelemetry, 1000);

        // Get device twin from Azure IoT Central.
        client.getTwin((err, twin) => {
          if (err) {
            console.log(`Error getting device twin: ${err.toString()}`);
          } else {
            // Send device properties once on device start up.
            sendDeviceProperties(twin);
            // Apply device settings and handle changes to device settings.
            handleSettings(twin);
          }
        });
      }
    };

    // Start the device (connect it to Azure IoT Central).
    client.open(connectCallback);
    ```

## <a name="run-your-nodejs-application"></a>Node.js uygulamanızı çalıştırma

Komut satırı ortamınızda aşağıdaki komutu çalıştırın:

```cmd/sh
node connectedAirConditionerAdv.js
```

Azure IOT merkezi uygulamanızdaki bir operatör olarak, gerçek cihazınız için şunları yapabilirsiniz:

* Telemetri görüntülemek **ölçümleri** sayfa:

    ![Telemetri görüntüleme](media/howto-connect-nodejs/viewtelemetry.png)

* Cihazınızı gönderilen cihaz özellik değerlerini görüntülemek **özellikleri** sayfası.

    ![Cihaz özellikleri görüntüle](media/howto-connect-nodejs/viewproperties.png)

* Fan hızı ve hedef sıcaklık gelen ayarlamak **ayarları** sayfası.

    ![Set fan hızı](media/howto-connect-nodejs/setfanspeed.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT merkezi uygulamanıza genel bir Node.js istemcisini bağlanma öğrendiniz, önerilen sonraki adımlar şunlardır:
* [Hazırlama ve Raspberry Pi'yi bağlanın](howto-connect-raspberry-pi-python.md)
<!-- Next how-tos in the sequence -->
