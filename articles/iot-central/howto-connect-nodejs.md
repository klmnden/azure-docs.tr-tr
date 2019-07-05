---
title: Azure IOT Central için genel bir Node.js istemci uygulaması bağlayın | Microsoft Docs
description: Bir cihaz geliştirici olarak, Azure IOT Central uygulamasına genel bir Node.js cihaz bağlanma.
author: dominicbetts
ms.author: dobett
ms.date: 06/14/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 90e4a061e38fdd3a13a640363069fae3a18e0b49
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444233"
---
# <a name="connect-a-generic-client-application-to-your-azure-iot-central-application-nodejs"></a>Azure IOT Central uygulamanızı (Node.js) genel istemci uygulamaya bağlama

Bu makalede, Microsoft Azure IOT Central uygulamanıza gerçek bir cihaz temsil eden genel bir Node.js uygulaması bağlanmak için bir cihaz geliştirici olarak nasıl.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

1. Azure IOT Central bir uygulamadır. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).
1. Bir geliştirme makinesi ile [Node.js](https://nodejs.org/) 4.0.0 sürümünü veya sonraki bir sürümü yüklü. Çalıştırabileceğiniz `node --version` sürümünüzü denetlemek için komut satırına. Node.js çeşitli işletim sistemleri için kullanılabilir.

## <a name="create-a-device-template"></a>Bir cihaz şablonu oluşturma

Azure IOT Central uygulamanızda aşağıdaki ölçümler, cihaz özelliklerini, ayarlarını ve komutları ile bir cihaz şablonu gerekir:

### <a name="telemetry-measurements"></a>Telemetri ölçümleri

Aşağıdaki telemetri ekleyin **ölçümleri** sayfası:

| Görünen ad | Alan Adı  | Birimler | Min | Maks | Ondalık Basamak Sayısı |
| ------------ | ----------- | ----- | --- | --- | -------------- |
| Sıcaklık  | sıcaklık | F     | 60  | 110 | 0              |
| Nem oranı     | Nem oranı    | %     | 0   | 100 | 0              |
| Basınç     | basınç    | kPa   | 80  | 110 | 0              |

> [!NOTE]
> Veri telemetri ölçü bir kayan türüdür nokta sayısı.

Alan adları cihaz şablona tabloda gösterildiği gibi tam olarak girin. İlgili cihaz kod özellik adları alan adları eşleşmiyorsa, telemetri uygulamada görüntülenemiyor.

### <a name="state-measurements"></a>Durum ölçümleri

Aşağıdaki durum eklemek **ölçümleri** sayfası:

| Görünen ad | Alan Adı  | 1 değeri | Görünen ad | Değer 2 | Görünen ad |
| ------------ | ----------- | --------| ------------ | ------- | ------------ | 
| Fan Modu     | fanmode     | 1       | Çalışıyor      | 0       | Durduruldu      |

> [!NOTE]
> Veri türü durumu ölçümü dizedir.

Alan adları cihaz şablona tabloda gösterildiği gibi tam olarak girin. İlgili cihaz kod özellik adları alan adları eşleşmiyorsa, uygulama durumu görüntülenemiyor.

### <a name="event-measurements"></a>Olay ölçümleri

Aşağıdaki olay eklemek **ölçümleri** sayfası:

| Görünen ad | Alan Adı  | Severity |
| ------------ | ----------- | -------- |
| Elektriği  | overheat    | Hata    |

> [!NOTE]
> Veri türü olay ölçümü dizedir.

### <a name="location-measurements"></a>Konum ölçümleri

Aşağıdaki konum ölçüm eklemek **ölçümleri** sayfası:

| Görünen ad | Alan Adı  |
| ------------ | ----------- |
| Location     | location    |

Veri türü iki kuyruğumuzu temizler konumu ölçüm boylam ve enlem noktalı sayıları ve yüksekliği için isteğe bağlı bir kayan nokta sayısı kayan.

Alan adları cihaz şablona tabloda gösterildiği gibi tam olarak girin. İlgili cihaz kod özellik adları alan adları eşleşmiyorsa, uygulamada konumu görüntülenemiyor.

### <a name="device-properties"></a>Cihaz özellikleri

Aşağıdaki cihaz özelliklerini ekleyin **özellikleri** sayfası:

| Görünen ad        | Alan Adı        | Veri türü |
| ------------------- | ----------------- | --------- |
| Seri Numarası       | serialNumber      | metin      |
| Cihaz üreticisi | Üretici      | metin      |

Alan adları cihaz şablona tabloda gösterildiği gibi tam olarak girin. İlgili cihaz kod özellik adları alan adları eşleşmiyorsa, uygulama özellikleri görüntülenemiyor.

### <a name="settings"></a>Ayarlar

Aşağıdaki **numarası** ayarlarını **ayarları** sayfası:

| Görünen ad    | Alan Adı     | Birimler | Ondalık sayı | Min | Maks  | İlk |
| --------------- | -------------- | ----- | -------- | --- | ---- | ------- |
| Fan hızı       | fanSpeed       | RPM   | 0        | 0   | 3000 | 0       |
| Sıcaklığı Ayarla | setTemperature | F     | 0        | 20  | 200  | 80      |

Cihaz şablona tabloda gösterildiği gibi tam olarak alan adı girin. İlgili cihaz kod özellik adları alan adları eşleşmiyorsa, cihaz ayarı değerini alamaz.

### <a name="commands"></a>Komutlar

Aşağıdaki komutu ekleyin **komutları** sayfası:

| Görünen ad    | Alan Adı     | Varsayılan Zaman Aşımı | Veri Türü |
| --------------- | -------------- | --------------- | --------- |
| Geri sayım       | Geri sayım      | 30              | number    |

Aşağıdaki giriş alanını geri sayım komutu ekleyin:

| Görünen ad    | Alan Adı     | Veri Türü | Değer |
| --------------- | -------------- | --------- | ----- |
| Gelen sayısı      | countFrom      | number    | 10    |

Alan adları cihaz şablona tabloda gösterildiği gibi tam olarak girin. İlgili cihaz kod özellik adları alan adları eşleşmiyorsa, cihaz komut işlenemiyor.

## <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Azure IOT Central uygulamanızda, önceki bölümde oluşturduğunuz cihaz şablonu gerçek bir cihaz ekleyin.

İçin "bir cihaz eklemek" öğreticideki yönergeleri izleyin [gerçek bir cihaz için bir bağlantı dizesi oluştur](tutorial-add-device.md#generate-connection-string). Aşağıdaki bölümde bu bağlantı dizesini kullanabilirsiniz:

### <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma

Aşağıdaki adımları uygulamaya eklenen gerçek cihaz uygulayan bir istemci uygulaması oluşturma işlemini göstermektedir. Burada Node.js uygulaması gerçek bir cihazı temsil eder. 

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
    var locLong = -122.1215;
    var locLat = 47.6740;
    var client = clientFromConnectionString(connectionString);
    ```

    Yer tutucu güncelleştirme `{your device connection string}` ile [cihaz bağlantı dizesini](tutorial-add-device.md#generate-connection-string). Bu örnekte, başlatma `targetTemperature` sıfır olarak cihazdaki geçerli okuma veya cihaz ikizinde arasında bir değer kullanabilirsiniz.

1. Telemetri, durumu, olay ve konum ölçümler, Azure IOT Central uygulamasına göndermek için dosyasına aşağıdaki işlevi ekleyin:

    ```javascript
    // Send device measurements.
    function sendTelemetry() {
      var temperature = targetTemperature + (Math.random() * 15);
      var humidity = 70 + (Math.random() * 10);
      var pressure = 90 + (Math.random() * 5);
      var fanmode = 0;
      var locationLong = locLong - (Math.random() / 100);
      var locationLat = locLat - (Math.random() / 100);
      var data = JSON.stringify({
        temperature: temperature,
        humidity: humidity,
        pressure: pressure,
        fanmode: (temperature > 25) ? "1" : "0",
        overheat: (temperature > 35) ? "ER123" : undefined,
        location: {
            lon: locationLong,
            lat: locationLat }
        });
      var message = new Message(data);
      client.sendEvent(message, (err, res) => console.log(`Sent message: ${message.getData()}` +
        (err ? `; error: ${err.toString()}` : '') +
        (res ? `; status: ${res.constructor.name}` : '')));
    }
    ```

1. Cihaz özellikleri, Azure IOT Central uygulamasına göndermek için dosyanıza aşağıdaki işlevi ekleyin:

    ```javascript
    // Send device reported properties.
    function sendDeviceProperties(twin, properties) {
      twin.properties.reported.update(properties, (err) => console.log(`Sent device properties: ${JSON.stringify(properties)}; ` +
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

1. IOT Central uygulamadan gönderilen bir geri sayım komutunu işlemek için aşağıdaki kodu ekleyin:

    ```javascript
    // Handle countdown command
    function onCountdown(request, response) {
      console.log('Received call to countdown');

      var countFrom = (typeof(request.payload.countFrom) === 'number' && request.payload.countFrom < 100) ? request.payload.countFrom : 10;

      response.send(200, (err) => {
        if (err) {
          console.error('Unable to send method response: ' + err.toString());
        } else {
          client.getTwin((err, twin) => {
            function doCountdown(){
              if ( countFrom >= 0 ) {
                var patch = {
                  countdown:{
                    value: countFrom
                  }
                };
                sendDeviceProperties(twin, patch);
                countFrom--;
                setTimeout(doCountdown, 2000 );
              }
            }

            doCountdown();
          });
        }
      });
    }
    ```

1. Azure IoT Central bağlantısını tamamlamak ve istemci kodundaki işlevleri bağlamak için aşağıdaki kodu ekleyin:

    ```javascript
    // Handle device connection to Azure IoT Central.
    var connectCallback = (err) => {
      if (err) {
        console.log(`Device could not connect to Azure IoT Central: ${err.toString()}`);
      } else {
        console.log('Device successfully connected to Azure IoT Central');

        // Create handler for countdown command
        client.onDeviceMethod('countdown', onCountdown);

        // Send telemetry measurements to Azure IoT Central every 1 second.
        setInterval(sendTelemetry, 1000);

        // Get device twin from Azure IoT Central.
        client.getTwin((err, twin) => {
          if (err) {
            console.log(`Error getting device twin: ${err.toString()}`);
          } else {
            // Send device properties once on device start up.
            var properties = {
              serialNumber: '123-ABC',
              manufacturer: 'Contoso'
            };
            sendDeviceProperties(twin, properties);

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

* Konumun görünümünde **ölçümleri** sayfası:

    ![Görünüm konumu ölçümleri](media/howto-connect-nodejs/viewlocation.png)

* Cihazınızın gönderen cihazın özellik değerlerini görüntülemek **özellikleri** sayfası. Cihaz bağlandığında cihaz özelliği kutucuk güncelleştirme:

    ![Cihaz özelliklerini görüntüleme](media/howto-connect-nodejs/viewproperties.png)

* Fan hız ve hedef sıcaklık gelen ayarlamak **ayarları** sayfası:

    ![Fan hızı ayarlama](media/howto-connect-nodejs/setfanspeed.png)

* Geri sayım komutunu çağırın **komutları** sayfası:

    ![Geri sayım komuta çağrı](media/howto-connect-nodejs/callcountdown.png)

## <a name="next-steps"></a>Sonraki adımlar

Genel bir Node.js istemcisi, Azure IOT Central uygulamasına bağlanmak öğrendiniz, önerilen sonraki adıma öğrenmektir nasıl [bir özel cihaz şablonu ayarlama](howto-set-up-template.md) kendi IOT cihazını için.
