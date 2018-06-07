---
title: Sağlama Node.js - Azure Uzaktan izleme için Böğürtlenli Pi | Microsoft Docs
description: Node.js içinde yazılmış bir uygulama kullanarak Uzaktan izleme Çözüm Hızlandırıcısı Raspberry Pi'yi aygıt bağlanmaya açıklar.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 01/24/2018
ms.author: dobett
ms.openlocfilehash: 78647612fc747ec328279536d82fb31bb4858688
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34626879"
---
# <a name="connect-your-raspberry-pi-device-to-the-remote-monitoring-solution-accelerator-nodejs"></a>Uzaktan izleme Çözüm Hızlandırıcısı için (Node.js) Raspberry Pi'yi Cihazınızı bağlama

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu öğretici, fiziksel bir aygıtı için Uzaktan izleme Çözüm Hızlandırıcısı bağlanmak nasıl gösterir. Bu öğreticide, en az kaynak kısıtlamaları ortamlar için iyi bir seçenek olan Node.js kullanın.

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

Henüz yapmadıysanız, Raspberry Pi'yi Node.js sürüm 4.0.0 veya sonraki bir sürümü yükleyin. Aşağıdaki adımlar, Raspberry Pi'yi Node.js v6 yükleme gösterir:

1. Kullanarak Raspberry Pi'yi bağlanmak `ssh`. Daha fazla bilgi için bkz: [SSH (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md) üzerinde [Raspberry Pi'yi Web sitesi](https://www.raspberrypi.org/).

1. Raspberry Pi'yi güncelleştirmek için aşağıdaki komutu kullanın:

    ```sh
    sudo apt-get update
    ```

1. Node.js olan herhangi bir yüklemesini, Raspberry Pi'yi kaldırmak için aşağıdaki komutları kullanın:

    ```sh
    sudo apt-get remove nodered -y
    sudo apt-get remove nodejs nodejs-legacy -y
    sudo apt-get remove npm  -y
    ```

1. Node.js v6, Raspberry Pi'yi yükleyip için aşağıdaki komutu kullanın:

    ```sh
    curl -sL https://deb.nodesource.com/setup_6.x | sudo bash -
    sudo apt-get install nodejs -y
    ```

1. Node.js v6.11.4 başarıyla yüklediniz doğrulamak için aşağıdaki komutu kullanın:

    ```sh
    node --version
    ```

## <a name="create-a-nodejs-solution"></a>Bir Node.js çözümü oluşturma

Kullanarak aşağıdaki adımları tamamlayın `ssh` Raspberry Pi'yi bağlantı:

1. Adlı bir klasör oluşturun `remotemonitoring` Raspberry Pi'yi, ev klasöründedir. Komut satırında bu klasöre gidin:

    ```sh
    cd ~
    mkdir remotemonitoring
    cd remotemonitoring
    ```

1. Karşıdan yüklemek ve örnek uygulaması tamamlamanız gereken paketleri yüklemek için aşağıdaki komutları çalıştırın:

    ```sh
    npm init
    npm install async azure-iot-device azure-iot-device-mqtt --save
    ```

1. İçinde `remotemonitoring` klasörünü adlı bir dosya oluşturun **remote_monitoring.js**. Bu dosyayı bir metin düzenleyicisinde açın. Raspberry Pi'yi üzerinde kullandığınız `nano` veya `vi` metin düzenleyicileri.

1. İçinde **remote_monitoring.js** dosya, aşağıdaki ekleyin `require` deyimleri:

    ```nodejs
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    var Message = require('azure-iot-device').Message;
    var async = require('async');
    ```

1. `require` deyimlerinden sonra aşağıdaki değişken bildirimlerini ekleyin. Yer tutucu değerini değiştirin `{device connection string}` Uzaktan izleme çözümünde sağladığınız cihaz için not ettiğiniz değere sahip:

    ```nodejs
    var connectionString = '{device connection string}';
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
    var deviceOnline = true;
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
      "Longitude": deviceLongitude,
      "Online": deviceOnline
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

1. Çözümden doğrudan yöntem çağrıları işlemek için aşağıdaki genel işlevi ekleyin. İşlev çağrıldı, ancak bu örnekte herhangi bir yolla cihaz değiştirmez doğrudan yöntemi hakkında bilgi görüntüler. Çözüm cihazlarda yapması doğrudan yöntemleri kullanır:

    ```nodejs
    function onDirectMethod(request, response) {
      // Implement logic asynchronously here.
      console.log('Simulated ' + request.methodName);

      // Complete the response
      response.send(200, request.methodName + ' was called on the device', function (err) {
        if (err) console.error('Error sending method response :\n' + err.toString());
        else console.log('200 Response to method \'' + request.methodName + '\' sent successfully.');
      });
    }
    ```

1. İşlemek için aşağıdaki işlevi ekleyin **FirmwareUpdate** doğrudan çözümden yöntem çağrıları. İşlev doğrudan yöntemi yükünde geçirilen parametreler doğrular ve bellenim güncelleştirme benzetimi zaman uyumsuz olarak çalışır:

    ```node.js
    function onFirmwareUpdate(request, response) {
      // Get the requested firmware version from the JSON request body
      var firmwareVersion = request.payload.Firmware;
      var firmwareUri = request.payload.FirmwareUri;
      
      // Ensure we got a firmware values
      if (!firmwareVersion || !firmwareUri) {
        response.send(400, 'Missing firmware value', function(err) {
          if (err) console.error('Error sending method response :\n' + err.toString());
          else console.log('400 Response to method \'' + request.methodName + '\' sent successfully.');
        });
      } else {
        // Respond the cloud app for the device method
        response.send(200, 'Firmware update started.', function(err) {
          if (err) console.error('Error sending method response :\n' + err.toString());
          else {
            console.log('200 Response to method \'' + request.methodName + '\' sent successfully.');

            // Run the simulated firmware update flow
            runFirmwareUpdateFlow(firmwareVersion, firmwareUri);
          }
        });
      }
    }
    ```

1. İlerleme durumu geri çözüme raporları bir uzun süre çalışan bellenim güncelleştirme akış benzetimini yapmak için aşağıdaki işlevi ekleyin:

    ```node.js
    // Simulated firmwareUpdate flow
    function runFirmwareUpdateFlow(firmwareVersion, firmwareUri) {
      console.log('Simulating firmware update flow...');
      console.log('> Firmware version passed: ' + firmwareVersion);
      console.log('> Firmware URI passed: ' + firmwareUri);
      async.waterfall([
        function (callback) {
          console.log("Image downloading from " + firmwareUri);
          var patch = {
            FirmwareUpdateStatus: 'Downloading image..'
          };
          reportUpdateThroughTwin(patch, callback);
          sleep(10000, callback);
        },
        function (callback) {
          console.log("Downloaded, applying firmware " + firmwareVersion);
          deviceOnline = false;
          var patch = {
            FirmwareUpdateStatus: 'Applying firmware..',
            Online: false
          };
          reportUpdateThroughTwin(patch, callback);
          sleep(8000, callback);
        },
        function (callback) {
          console.log("Rebooting");
          var patch = {
            FirmwareUpdateStatus: 'Rebooting..'
          };
          reportUpdateThroughTwin(patch, callback);
          sleep(10000, callback);
        },
        function (callback) {
          console.log("Firmware updated to " + firmwareVersion);
          deviceOnline = true;
          var patch = {
            FirmwareUpdateStatus: 'Firmware updated',
            Online: true,
            Firmware: firmwareVersion
          };
          reportUpdateThroughTwin(patch, callback);
          callback(null);
        }
      ], function(err) {
        if (err) {
          console.error('Error in simulated firmware update flow: ' + err.message);
        } else {
          console.log("Completed simulated firmware update flow");
        }
      });

      // Helper function to update the twin reported properties.
      function reportUpdateThroughTwin(patch, callback) {
        console.log("Sending...");
        console.log(JSON.stringify(patch, null, 2));
        client.getTwin(function(err, twin) {
          if (!err) {
            twin.properties.reported.update(patch, function(err) {
              if (err) callback(err);
            });      
          } else {
            if (err) callback(err);
          }
        });
      }

      function sleep(milliseconds, callback) {
        console.log("Simulate a delay (milleseconds): " + milliseconds);
        setTimeout(function () {
          callback(null);
        }, milliseconds);
      }
    }
    ```

1. Çözüme telemetri verileri göndermek için aşağıdaki kodu ekleyin. İstemci uygulama ileti şeması tanımlamak için iletiye özellikleri ekler:

    ```node.js
    function sendTelemetry(data, schema) {
      if (deviceOnline) {
        var d = new Date();
        var payload = JSON.stringify(data);
        var message = new Message(payload);
        message.properties.add('$$CreationTimeUtc', d.toISOString());
        message.properties.add('$$MessageSchema', schema);
        message.properties.add('$$ContentType', 'JSON');

        console.log('Sending device message data:\n' + payload);
        client.sendEvent(message, printErrorFor('send event'));
      } else {
        console.log('Offline, not sending telemetry');
      }
    }
    ```

1. Bir istemci örneği oluşturmak için aşağıdaki kodu ekleyin:

    ```nodejs
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. Aşağıdaki kodu ekleyin:

    * Bağlantıyı açın.
    * İstenen özellikleri için bir işleyici ayarlayın.
    * Bildirilen özellikleri gönderin.
    * Doğrudan yöntemleri için işleyiciler kaydedin. Örnek, bellenim güncelleştirme doğrudan yöntemi için ayrı bir işleyici kullanır.
    * Telemetri göndermeye başlayın.

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
              console.log('Twin state reported');
            });

            // Register handlers for all the method names we are interested in.
            // Consider separate handlers for each method.
            client.onDeviceMethod('Reboot', onDirectMethod);
            client.onDeviceMethod('FirmwareUpdate', onFirmwareUpdate);
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
