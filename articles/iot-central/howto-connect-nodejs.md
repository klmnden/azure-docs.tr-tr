---
title: Azure IOT Central için genel bir Node.js istemci uygulaması bağlayın | Microsoft Docs
description: Bir cihaz geliştirici olarak, Azure IOT Central uygulamasına genel bir Node.js cihaz bağlanma.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 55ce85702804d99d806220d7f0a4ea0820975f4f
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39206046"
---
# <a name="connect-a-generic-client-application-to-your-azure-iot-central-application-nodejs"></a>Azure IOT Central uygulamanızı (Node.js) genel istemci uygulamaya bağlama

Bu makalede, Microsoft Azure IOT Central uygulamanıza bir fiziksel cihazı temsil eden genel bir Node.js uygulaması bağlanmak için bir cihaz geliştirici olarak nasıl.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

1. Azure IOT Central bir uygulamadır. Daha fazla bilgi için [Azure IOT Central uygulaması oluşturmayı](howto-create-application.md).
1. Bir geliştirme makinesi ile [Node.js](https://nodejs.org/) 4.0.0 sürümünü veya sonraki bir sürümü yüklü. Çalıştırabileceğiniz `node --version` sürümünüzü denetlemek için komut satırına. Node.js çeşitli işletim sistemleri için kullanılabilir.

## <a name="create-a-device-template"></a>Bir cihaz şablonu oluşturma

Azure IOT Central uygulamanızda aşağıdaki ölçümleri ve cihaz özellikleri tanımlanan cihaz şablonuyla gerekir:

### <a name="telemetry-measurements"></a>Telemetri ölçümleri

Aşağıdaki telemetriyi ekleme **ölçümleri** sayfası:

| Görünen Ad | Alan Adı  | Birimler | Min | Maks | Ondalık basamak |
| ------------ | ----------- | ----- | --- | --- | -------------- |
| Sıcaklık  | sıcaklık | F     | 60  | 110 | 0              |
| Nem oranı     | Nem oranı    | %     | 0   | 100 | 0              |
| Basınç     | basınç    | kPa   | 80  | 110 | 0              |

> [!NOTE]
  Çift telemetri ölçümün veri türü.

Alan adları cihaz şablona tabloda gösterildiği gibi tam olarak girin. Alan adları eşleşmiyorsa, telemetri uygulamada görüntülenemiyor.

### <a name="state-measurements"></a>Durum ölçümleri

Şu durumda ekleme **ölçümleri** sayfası:

| Görünen Ad | Alan Adı  | 1 değeri | Görünen Ad | Değer 2 | Görünen Ad |
| ------------ | ----------- | --------| ------------ | ------- | ------------ | 
| Fan Modu     | fanmode     | 1       | Çalışıyor      | 0       | Durduruldu      |

> [!NOTE]
  Veri türü, durumu ölçümü dizedir.

Alan adları cihaz şablona tabloda gösterildiği gibi tam olarak girin. Alan adları eşleşmiyorsa, uygulama durumu görüntülenemiyor.

### <a name="event-measurements"></a>Olay ölçümleri

Aşağıdaki olayın ekleme **ölçümleri** sayfası:

| Görünen Ad | Alan Adı  | Severity |
| ------------ | ----------- | -------- |
| Elektriği  | overheat    | Hata    |

> [!NOTE]
  Veri türü, olay ölçümü dizedir.

### <a name="device-properties"></a>Cihaz özellikleri

Aşağıdaki cihaz özelliklerinde ekleme **özellikleri sayfasında**:

| Görünen Ad        | Alan Adı        | Veri türü |
| ------------------- | ----------------- | --------- |
| Seri Numarası       | serialNumber      | metin      |
| Cihaz üreticisi | üretici      | metin      |

Alan adları cihaz şablona tabloda gösterildiği gibi tam olarak girin. Uygulama, alan adları eşleşmiyorsa, özellik değeri gösterilemiyor.

### <a name="settings"></a>Ayarlar

Aşağıdaki **numarası** ayarlarında **Ayarları sayfası**:

| Görünen Ad    | Alan Adı     | Birimler | Ondalık sayı | Min | Maks  | İlk |
| --------------- | -------------- | ----- | -------- | --- | ---- | ------- |
| Fan hızı       | fanSpeed       | RPM   | 0        | 0   | 3000 | 0       |
| Sıcaklığı Ayarla | setTemperature | F     | 0        | 20  | 200  | 80      |

Cihaz şablona tabloda gösterildiği gibi tam olarak alan adı girin. Alan adları eşleşmiyorsa, cihaz ayarı değerini alamaz.

## <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Azure IOT Central uygulamanızı oluşturun ve cihaz bağlantı dizesini not edin cihaz şablonundan gerçek bir cihaz ekleyin. Daha fazla bilgi için [Azure IOT Central uygulamanıza gerçek bir cihaz ekleyin](tutorial-add-device.md)

### <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma

Aşağıdaki adımları uygulamaya eklenen gerçek cihaz uygulayan bir istemci uygulaması oluşturma işlemini göstermektedir.

1. Makinenizde `connected-air-conditioner-adv` adlı bir klasör oluşturun. Komut satırı ortamınızda bu klasöre gidin.

1. Node.js projenizi başlatmak için aşağıdaki komutları çalıştırın:

    ```cmd/sh
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Adlı bir dosya oluşturun **connectedAirConditionerAdv.js** içinde `connected-air-conditioner-adv` klasör.

1. Aşağıdaki `require` başlangıcında deyimleri **connectedAirConditionerAdv.js** dosyası:

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

    Yer tutucu güncelleştirme `{your device connection string}` cihaz bağlantısı dizeniz ile. Gerçek Cihazınızı eklediğinizde bağlantı ayrıntıları sayfasından bu değeri kopyalar. Bu örnekte biz başlatmak `targetTemperature` sıfır olarak, isteğe bağlı olarak bir CİHAZDAN geçerli okuma veya değeri cihaz ikizinden alabilir. 

1. Telemetri, durum ve olay ölçümleri, Azure IOT Central uygulamasına göndermek için dosyasına aşağıdaki işlevi ekleyin:

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

    1. Cihaz özellikleri, Azure IOT Central uygulamasına göndermek için dosyanıza aşağıdaki işlevi ekleyin:

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

1. Azure IOT Central uygulamanızın güncelleştirilmiş ayarları işlemek için aşağıdaki dosyaya ekleyin:

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

1. Azure IOT Central için bağlantıyı tamamlamak ve istemci kodu işlevlerde bağlama için aşağıdakileri ekleyin:

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

## <a name="run-your-nodejs-application"></a>Node.js uygulamanızı çalıştırın

Komut satırı ortamınızda aşağıdaki komutu çalıştırın:

```cmd/sh
node connectedAirConditionerAdv.js
```

Azure IOT Central, uygulamanızdaki bir operatör olarak, gerçek cihazınız için şunları yapabilirsiniz:

* Telemetri görünümünde **ölçümleri** sayfası:

    ![Telemetri görüntüleme](media/howto-connect-nodejs/viewtelemetry.png)

* Cihazınızın gönderen cihazın özellik değerlerini görüntülemek **özellikleri** sayfası.

    ![Cihaz özelliklerini görüntüleme](media/howto-connect-nodejs/viewproperties.png)

* Fan hız ve hedef sıcaklık gelen ayarlamak **ayarları** sayfası.

    ![Fan hızı ayarlama](media/howto-connect-nodejs/setfanspeed.png)

## <a name="next-steps"></a>Sonraki adımlar

Genel bir Node.js istemcisi, Azure IOT Central uygulamasına bağlanmak öğrendiniz, önerilen sonraki adımlar şunlardır:
* [Hazırlama ve Raspberry Pi'yi bağlanın](howto-connect-raspberry-pi-python.md)
<!-- Next how-tos in the sequence -->
