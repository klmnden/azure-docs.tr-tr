---
title: Uzaktan izleme node.js'de - Azure cihaz sağlama | Microsoft Docs
description: Node.js'de yazılmış bir Web uygulaması kullanarak Uzaktan izleme çözüm Hızlandırıcısını için bir cihaz bağlamak açıklar.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 01/24/2018
ms.author: dobett
ms.openlocfilehash: fdb2bed76a8e23a6034a57b3a5f1358c26e9e990
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61450287"
---
# <a name="connect-your-device-to-the-remote-monitoring-solution-accelerator-nodejs"></a>Cihazınızı Uzaktan izleme çözüm hızlandırıcısına (Node.js) bağlama

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu öğreticide, gerçek bir cihaz Uzaktan izleme çözüm hızlandırıcısına bağlamayı gösterilmektedir. Bu öğreticide, Node.js, en az kaynak kısıtlamalarıyla ortamlar için iyi bir seçenek olduğu kullanın.

Bir cihazın benzetimini gerçekleştirme isterseniz, bkz. [oluşturma ve test yeni bir simülasyon cihazı](iot-accelerators-remote-monitoring-create-simulated-device.md).

## <a name="create-a-nodejs-solution"></a>Bir Node.js çözümü oluşturun

Emin [Node.js](https://nodejs.org/) sürüm 4.0.0 veya üzeri geliştirme makinenizde yüklü. Çalıştırabileceğiniz `node --version` sürümü denetlemek için komut satırına.

1. Adlı bir klasör oluşturun `remotemonitoring` geliştirme makinenizde. Komut satırı ortamınızda bu klasöre gidin.

1. Örnek uygulamayı tamamlamanız gereken paketleri indirme ve yükleme için aşağıdaki komutları çalıştırın:

    ```cmd/sh
    npm init
    npm install async azure-iot-device azure-iot-device-mqtt --save
    ```

1. İçinde `remotemonitoring` klasöründe adlı bir dosya oluşturun **remote_monitoring.js**. Bu dosyayı bir metin düzenleyicisinde açın.

1. İçinde **remote_monitoring.js** dosyasında, aşağıdaki ekleyin `require` ifadeleri:

    ```javascript
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var Message = require('azure-iot-device').Message;
    var async = require('async');
    ```

1. `require` deyimlerinden sonra aşağıdaki değişken bildirimlerini ekleyin. Yer tutucu değerini değiştirin `{device connection string}` cihaz için belirtilen değer ile'Uzaktan izleme çözümünde sağlanan:

    ```javascript
    var connectionString = '{device connection string}';
    ```

1. Bazı temel telemetri verileri tanımlamak için aşağıdaki değişkenleri ekleyin:

    ```javascript
    var temperature = 50;
    var temperatureUnit = 'F';
    var humidity = 50;
    var humidityUnit = '%';
    var pressure = 55;
    var pressureUnit = 'psig';
    ```

1. Bazı özellik değerlerini tanımlamak için aşağıdaki değişkenleri ekleyin:

    ```javascript
    var schema = "real-chiller;v1";
    var deviceType = "RealChiller";
    var deviceFirmware = "1.0.0";
    var deviceFirmwareUpdateStatus = "";
    var deviceLocation = "Building 44";
    var deviceLatitude = 47.638928;
    var deviceLongitude = -122.13476;
    var deviceOnline = true;
    ```

1. Çözüme göndermek için bildirilen özellikleri tanımlamak için aşağıdaki değişkeni ekleyin. Bu özellikler Web kullanıcı Arabiriminde görüntülemek için meta verileri içerir:

    ```javascript
    var reportedProperties = {
      "SupportedMethods": "Reboot,FirmwareUpdate,EmergencyValveRelease,IncreasePressure",
      "Telemetry": {
        [schema]: ""
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

1. İşlem sonuçlarını yazdırmak için aşağıdaki yardımcı işlevi ekleyin:

    ```javascript
    function printErrorFor(op) {
        return function printError(err) {
            if (err) console.log(op + ' error: ' + err.toString());
        };
    }
    ```

1. Telemetri değerleri rastgele seçmek için kullanılacak aşağıdaki yardımcı işlevi ekleyin:

     ```javascript
     function generateRandomIncrement() {
         return ((Math.random() * 2) - 1);
     }
     ```

1. Çözümden doğrudan yöntem çağrıları işlemek için aşağıdaki genel işlevi ekleyin. İşlev çağrıldı, ancak bu örnekte, cihazın herhangi bir şekilde değiştirmez doğrudan yöntem hakkında bilgi görüntüler. Çözüm, cihazlarda işlem yapmak için doğrudan yöntemleri kullanır:

     ```javascript
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

1. İşlemek için aşağıdaki işlevi ekleyin **FirmwareUpdate** doğrudan çözümünden yöntem çağrıları. İşlev doğrudan yöntem yükteki geçirilen parametreler doğrular ve bir üretici yazılımı güncelleştirme simülasyonu zaman uyumsuz olarak çalışır:

     ```javascript
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

1. İlerlemeyi çözüme geri raporlar bir uzun süre çalışan üretici yazılımı güncelleştirme akış benzetimini yapmak için aşağıdaki işlevi ekleyin:

     ```javascript
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

1. Çözüme telemetri verileri göndermek için aşağıdaki kodu ekleyin. İstemci uygulaması, iletinin ileti şeması tanımlamak için özellikleri ekler:

     ```javascript
     function sendTelemetry(data, schema) {
       if (deviceOnline) {
         var d = new Date();
         var payload = JSON.stringify(data);
         var message = new Message(payload);
         message.properties.add('iothub-creation-time-utc', d.toISOString());
         message.properties.add('iothub-message-schema', schema);

         console.log('Sending device message data:\n' + payload);
         client.sendEvent(message, printErrorFor('send event'));
       } else {
         console.log('Offline, not sending telemetry');
       }
     }
     ```

1. Bir istemci örneği oluşturmak için aşağıdaki kodu ekleyin:

     ```javascript
     var client = Client.fromConnectionString(connectionString, Protocol);
     ```

1. Aşağıdaki kodu ekleyin:

    * Bağlantıyı açın.
    * İstenen özellikleri için bir işleyici ayarlayın.
    * Bildirilen özellikleri gönderir.
    * Doğrudan yöntemler için işleyiciler kaydedin. Örnek, üretici yazılımı güncelleştirme doğrudan yöntemi için ayrı bir işleyici kullanır.
    * Telemetri göndermeye başlaması.

      ```javascript
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
        var sendDeviceTelemetry = setInterval(function () {
          temperature += generateRandomIncrement();
          pressure += generateRandomIncrement();
          humidity += generateRandomIncrement();
          var data = {
            'temperature': temperature,
            'temperature_unit': temperatureUnit,
            'humidity': humidity,
            'humidity_unit': humidityUnit,
            'pressure': pressure,
            'pressure_unit': pressureUnit
          };
          sendTelemetry(data, schema)
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

1. Örnek uygulamayı başlatmak için komut isteminde aşağıdaki komutu çalıştırın:

     ```cmd/sh
     node remote_monitoring.js
     ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
