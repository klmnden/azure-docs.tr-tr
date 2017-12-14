---
title: "Sağlama Node.js - Azure Uzaktan izleme için Böğürtlenli Pi | Microsoft Docs"
description: "Node.js içinde yazılmış bir uygulaması kullanarak Azure IOT paketi önceden yapılandırılmış Uzaktan izleme çözümü Raspberry Pi'yi aygıt bağlanmaya açıklar."
services: iot-suite
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: fc50a33f-9fb9-42d7-b1b8-eb5cff19335e
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: d2cc72f26568a362049f24323ffa598b6012d77f
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="connect-your-raspberry-pi-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a>Uzaktan izleme önceden yapılandırılmış çözümü (Node.js) Raspberry Pi'yi Cihazınızı bağlama

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu öğretici, fiziksel bir aygıtı için Uzaktan izleme önceden yapılandırılmış çözümü bağlanmak nasıl gösterir. Bu öğreticide, en az kaynak kısıtlamaları ortamlar için iyi bir seçenek olan Node.js kullanın.

### <a name="required-hardware"></a>Gerekli donanım

Komut satırı Raspberry Pi'yi üzerinde uzaktan bağlanmak etkinleştirmek için bir masaüstü bilgisayar.

[Microsoft IOT Starter Kit Raspberry Pi 3](https://azure.microsoft.com/develop/iot/starter-kits/) veya eşdeğer bileşenleri. Bu öğretici Seti'nden aşağıdaki öğeleri kullanır:

- Böğürtlenli Pi 3
- MicroSD kartı (ile NOOBS)
- Bir USB Mini kablosu
- Ethernet kablosu

### <a name="required-desktop-software"></a>Gerekli masaüstü yazılımı

Komut satırı Raspberry Pi'yi üzerinde uzaktan erişim sağlamak için Masaüstü makinenizde SSH istemcisi gerekir.

- Windows, bir SSH istemcisi içermez. Kullanmanızı öneririz [PuTTY](http://www.putty.org/).
- Çoğu Linux dağıtımları ve Mac OS komut satırı SSH yardımcı programı içerir. Daha fazla bilgi için bkz: [SSH kullanarak Linux veya Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

### <a name="required-raspberry-pi-software"></a>Gerekli Raspberry Pi'yi yazılım

Henüz yapmadıysanız, Raspberry Pi'yi Node.js sürüm 4.0.0 veya sonraki bir sürümü yükleyin. Aşağıdaki adımlar, Raspberry Pi'yi Node.js v6.11.4 yükleme gösterir:

1. Kullanarak Raspberry Pi'yi bağlanmak `ssh`. Daha fazla bilgi için bkz: [SSH (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md) üzerinde [Raspberry Pi'yi Web sitesi](https://www.raspberrypi.org/).

1. Raspberry Pi'yi güncelleştirmek için aşağıdaki komutu kullanın:

    ```sh
    sudo apt-get update
    ```

1. Raspberry Pi'yi Node.js ikilileri indirmek için aşağıdaki komutu kullanın:

    ```sh
    wget https://nodejs.org/dist/v6.11.4/node-v6.11.4-linux-armv7l.tar.gz
    ```

1. İkili dosyaları yüklemek için aşağıdaki komutu kullanın:

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.11.4-linux-armv7l.tar.gz
    ```

1. Node.js v6.11.4 başarıyla yüklediniz doğrulamak için aşağıdaki komutu kullanın:

    ```sh
    node --version
    ```

## <a name="create-a-nodejs-solution"></a>Bir Node.js çözümü oluşturma

Kullanarak aşağıdaki adımları tamamlayın `ssh` Raspberry Pi'yi bağlantı:

1. Adlı bir klasör oluşturun `RemoteMonitoring` Raspberry Pi'yi, ev klasöründedir. Komut satırında bu klasöre gidin:

    ```sh
    cd ~
    mkdir RemoteMonitoring
    cd RemoteMonitoring
    ```

1. Karşıdan yüklemek ve örnek uygulaması tamamlamanız gereken paketleri yüklemek için aşağıdaki komutları çalıştırın:

    ```sh
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. İçinde `RemoteMonitoring` klasörünü adlı bir dosya oluşturun **remote_monitoring.js**. Bu dosyayı bir metin düzenleyicisinde açın. Raspberry Pi'yi üzerinde kullandığınız `nano` veya `vi` metin düzenleyicileri.

1. İçinde **remote_monitoring.js** dosya, aşağıdaki ekleyin `require` deyimleri:

    ```nodejs
    'use strict';

    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

1. `require` deyimlerinden sonra aşağıdaki değişken bildirimlerini ekleyin. Yer tutucu değerlerini değiştirmek `{Device Id}` ve `{Device Key}` aygıt için not ettiğiniz değerlerle Uzaktan izleme çözümünde sağlandı. Değiştirmek için IOT Hub ana bilgisayar adına çözümden kullanmak `{IoTHub Name}`. Örneğin, IOT Hub ana bilgisayar adı ise `contoso.azure-devices.net`, yerine `{IoTHub Name}` ile `contoso`:

    ```nodejs
    var connectionString = 'HostName={IoTHub Name}.azure-devices.net;DeviceId={Device Id};SharedAccessKey={Device Key}';
    var deviceId = ConnectionString.parse(connectionString).DeviceId;
    ```

1. Bazı temel telemetri verileri tanımlamak için aşağıdaki değişkenleri ekleyin:

    ```nodejs
    var temperature = 50;
    var temperatureUnit = 'F';
    var humidity = 50;
    var humidityUnit = '%';
    var pressure = 55;
    var pressureUnit = 'psig';
    ```

1. Bazı özellik değerleri tanımlamak için aşağıdaki değişkenleri ekleyin:

    ```nodejs
    var temperatureSchema = 'chiller-temperature;v1';
    var humiditySchema = 'chiller-humidity;v1';
    var pressureSchema = 'chiller-pressure;v1';
    var interval = "00:00:05";
    var deviceType = "Chiller";
    var deviceFirmware = "1.0.0";
    var deviceFirmwareUpdateStatus = "";
    var deviceLocation = "Building 44";
    var deviceLatitude = 47.638928;
    var deviceLongitude = -122.13476;
    ```

1. Çözüme göndermek için bildirilen özelliklerini tanımlamak için aşağıdaki değişkeni ekleyin. Bu özellikler yöntemleri açıklamak için meta verileri içerir ve telemetri aygıt kullanır:

    ```nodejs
    var reportedProperties = {
      "Protocol": "MQTT",
      "SupportedMethods": "Reboot,FirmwareUpdate,EmergencyValveRelease,IncreasePressure",
      "Telemetry": {
        "TemperatureSchema": {
          "Interval": interval,
          "MessageTemplate": "{\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\"}",
          "MessageSchema": {
            "Name": temperatureSchema,
            "Format": "JSON",
            "Fields": {
              "temperature": "Double",
              "temperature_unit": "Text"
            }
          }
        },
        "HumiditySchema": {
          "Interval": interval,
          "MessageTemplate": "{\"humidity\":${humidity},\"humidity_unit\":\"${humidity_unit}\"}",
          "MessageSchema": {
            "Name": humiditySchema,
            "Format": "JSON",
            "Fields": {
              "humidity": "Double",
              "humidity_unit": "Text"
            }
          }
        },
        "PressureSchema": {
          "Interval": interval,
          "MessageTemplate": "{\"pressure\":${pressure},\"pressure_unit\":\"${pressure_unit}\"}",
          "MessageSchema": {
            "Name": pressureSchema,
            "Format": "JSON",
            "Fields": {
              "pressure": "Double",
              "pressure_unit": "Text"
            }
          }
        }
      },
      "Type": deviceType,
      "Firmware": deviceFirmware,
      "FirmwareUpdateStatus": deviceFirmwareUpdateStatus,
      "Location": deviceLocation,
      "Latitude": deviceLatitude,
      "Longitude": deviceLongitude
    }
    ```

1. İşlem sonuçları yazdırmak için aşağıdaki yardımcı işlevi ekleyin:

    ```nodejs
    function printErrorFor(op) {
        return function printError(err) {
            if (err) console.log(op + ' error: ' + err.toString());
        };
    }
    ```

1. Telemetri değerleri rastgele seçmek için kullanmak için aşağıdaki yardımcı işlevi ekleyin:

    ```nodejs
    function generateRandomIncrement() {
        return ((Math.random() * 2) - 1);
    }
    ```

1. Çözümden doğrudan yöntem çağrıları işlemek için aşağıdaki işlevi ekleyin. Çözüm cihazlarda yapması doğrudan yöntemleri kullanır:

    ```nodejs
    function onDirectMethod(request, response) {
      // Implement logic asynchronously here.
      console.log('Simulated ' + request.methodName);

      // Complete the response
      response.send(200, request.methodName + ' was called on the device', function (err) {
        if (!!err) {
          console.error('An error ocurred when sending a method response:\n' +
            err.toString());
        } else {
          console.log('Response to method \'' + request.methodName +
            '\' sent successfully.');
        }
      });
    }
    ```

1. Çözüme telemetri verileri göndermek için aşağıdaki kodu ekleyin. İstemci uygulama ileti şeması tanımlamak için iletiye özellikleri ekler:

    ```node.js
    function sendTelemetry(data, schema) {
      var d = new Date();
      var payload = JSON.stringify(data);
      var message = new Message(payload);
      message.properties.add('$$CreationTimeUtc', d.toISOString());
      message.properties.add('$$MessageSchema', schema);
      message.properties.add('$$ContentType', 'JSON');

      console.log('Sending device message data:\n' + payload);
      client.sendEvent(message, printErrorFor('send event'));
    }
    ```

1. Bir istemci örneği oluşturmak için aşağıdaki kodu ekleyin:

    ```nodejs
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. Aşağıdaki kodu ekleyin:

    - Bağlantıyı açın.
    - İstenen özellikleri için bir işleyici ayarlayın.
    - Bildirilen özellikleri gönderin.
    - Doğrudan yöntemleri için işleyiciler kaydedin.
    - Telemetri göndermeye başlayın.

    ```nodejs
    client.open(function (err) {
      if (err) {
        printErrorFor('open')(err);
      } else {
        // Create device Twin
        client.getTwin(function (err, twin) {
          if (err) {
            console.error('Could not get device twin');
          } else {
            console.log('Device twin created');

            twin.on('properties.desired', function (delta) {
              // Handle desired properties set by solution
              console.log('Received new desired properties:');
              console.log(JSON.stringify(delta));
            });

            // Send reported properties
            twin.properties.reported.update(reportedProperties, function (err) {
              if (err) throw err;
              console.log('twin state reported');
            });

            // Register handlers for all the method names we are interested in.
            // Consider separate handlers for each method.
            client.onDeviceMethod('Reboot', onDirectMethod);
            client.onDeviceMethod('FirmwareUpdate', onDirectMethod);
            client.onDeviceMethod('EmergencyValveRelease', onDirectMethod);
            client.onDeviceMethod('IncreasePressure', onDirectMethod);
          }
        });

        // Start sending telemetry
        var sendTemperatureInterval = setInterval(function () {
          temperature += generateRandomIncrement();
          var data = {
            'temperature': temperature,
            'temperature_unit': temperatureUnit
          };
          sendTelemetry(data, temperatureSchema)
        }, 5000);

        var sendHumidityInterval = setInterval(function () {
          humidity += generateRandomIncrement();
          var data = {
            'humidity': humidity,
            'humidity_unit': humidityUnit
          };
          sendTelemetry(data, humiditySchema)
        }, 5000);

        var sendPressureInterval = setInterval(function () {
          pressure += generateRandomIncrement();
          var data = {
            'pressure': pressure,
            'pressure_unit': pressureUnit
          };
          sendTelemetry(data, pressureSchema)
        }, 5000);

        client.on('error', function (err) {
          printErrorFor('client')(err);
          if (sendTemperatureInterval) clearInterval(sendTemperatureInterval);
          if (sendHumidityInterval) clearInterval(sendHumidityInterval);
          if (sendPressureInterval) clearInterval(sendPressureInterval);
          client.close(printErrorFor('client.close'));
        });
      }
    });
    ```

1. Değişiklikleri kaydetmek **remote_monitoring.js** dosya.

1. Örnek uygulamayı başlatmak için üzerinde Raspberry Pi'yi, komut isteminde aşağıdaki komutu çalıştırın:

    ```sh
    node remote_monitoring.js
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
