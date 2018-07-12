---
title: Node.js kullanarak bir cihazı bağlayın | Microsoft Docs
description: Bir cihaz, node.js'de yazılmış bir Web uygulaması kullanarak Azure IOT paketi önceden yapılandırılmış Uzaktan izleme çözümüne bağlanmak açıklar.
services: ''
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: fc50a33f-9fb9-42d7-b1b8-eb5cff19335e
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 87a2e97638508eef1d90a219cfb38d1fcac81d55
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38723885"
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a>Cihazınızı Uzaktan izleme önceden yapılandırılmış çözüme (Node.js) bağlama
[!INCLUDE [iot-suite-v1-selector-connecting](../../includes/iot-suite-v1-selector-connecting.md)]

## <a name="create-a-nodejs-sample-solution"></a>Node.js örnek bir çözüm oluşturma

Bu Node.js sürümü 0.11.5 sağlayın ya da üzeri geliştirme makinenizde yüklü. Çalıştırabileceğiniz `node --version` sürümü denetlemek için komut satırına.

1. Adlı bir klasör oluşturun **RemoteMonitoring** geliştirme makinenizde. Komut satırı ortamınızda bu klasöre gidin.

1. Örnek uygulamayı tamamlamanız gereken paketleri indirme ve yükleme için aşağıdaki komutları çalıştırın:

    ```
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. İçinde **RemoteMonitoring** klasöründe adlı bir dosya oluşturun **remote_monitoring.js**. Bu dosyayı bir metin düzenleyicisinde açın.

1. İçinde **remote_monitoring.js** dosyasında, aşağıdaki ekleyin `require` ifadeleri:

    ```nodejs
    'use strict';

    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

1. `require` deyimlerinden sonra aşağıdaki değişken bildirimlerini ekleyin. [Cihaz Kimliği] ve [Cihaz Anahtarı] yer tutucu değerlerini uzaktan izleme çözümü panosunda not ettiğiniz cihaz değerleriyle değiştirin. Çözüm panosundaki IoT Hub Ana Bilgisayar Adını [IoTHub Adı] yerine girin. Örneğin, IoT Hub Ana Bilgisayar Adınız **contoso.azure-devices.net** şeklindeyse [IoTHub Adı] yerine **contoso** yazın:

    ```nodejs
    var connectionString = 'HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]';
    var deviceId = ConnectionString.parse(connectionString).DeviceId;
    ```

1. Bazı temel telemetri verileri tanımlamak için aşağıdaki değişkenleri ekleyin:

    ```nodejs
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    ```

1. İşlem sonuçlarını yazdırmak için aşağıdaki yardımcı işlevi ekleyin:

    ```nodejs
    function printErrorFor(op) {
        return function printError(err) {
            if (err) console.log(op + ' error: ' + err.toString());
        };
    }
    ```

1. Telemetri değerleri rastgele seçmek için kullanılacak aşağıdaki yardımcı işlevi ekleyin:

    ```nodejs
    function generateRandomIncrement() {
        return ((Math.random() * 2) - 1);
    }
    ```

1. Aşağıdaki tanım ekleyin **Deviceınfo** cihazın gönderdiği Başlangıç nesnesi:

    ```nodejs
    var deviceMetaData = {
        'ObjectType': 'DeviceInfo',
        'IsSimulatedDevice': 0,
        'Version': '1.0',
        'DeviceProperties': {
            'DeviceID': deviceId,
            'HubEnabledState': 1
        }
    };
    ```

1. Aşağıdaki cihaz ikizinde tanımı bildirilen değerler. Bu tanım, cihazın desteklediği direkt yöntemlerle açıklamalarını içerir:

    ```nodejs
    var reportedProperties = {
        "Device": {
            "DeviceState": "normal",
            "Location": {
                "Latitude": 47.642877,
                "Longitude": -122.125497
            }
        },
        "Config": {
            "TemperatureMeanValue": 56.7,
            "TelemetryInterval": 45
        },
        "System": {
            "Manufacturer": "Contoso Inc.",
            "FirmwareVersion": "2.22",
            "InstalledRAM": "8 MB",
            "ModelNumber": "DB-14",
            "Platform": "Plat 9.75",
            "Processor": "i3-9",
            "SerialNumber": "SER99"
        },
        "Location": {
            "Latitude": 47.642877,
            "Longitude": -122.125497
        },
        "SupportedMethods": {
            "Reboot": "Reboot the device",
            "InitiateFirmwareUpdate--FwPackageURI-string": "Updates device Firmware. Use parameter FwPackageURI to specifiy the URI of the firmware file"
        },
    }
    ```

1. İşlemek için aşağıdaki işlevi ekleyin **yeniden** doğrudan yöntem çağrısı:

    ```nodejs
    function onReboot(request, response) {
        // Implement actual logic here.
        console.log('Simulated reboot...');

        // Complete the response
        response.send(200, "Rebooting device", function(err) {
            if(!!err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```

1. İşlemek için aşağıdaki işlevi ekleyin **Initiatefirmwareupdate** doğrudan yöntem çağrısı. Başlatır üretici yazılımını güncelleştirmek cihaz üzerinde zaman uyumsuz olarak ve üretici yazılımı görüntüsünü indirmek için konumunu belirtmek için bir parametre doğrudan bu yöntemi kullanır:

    ```nodejs
    function onInitiateFirmwareUpdate(request, response) {
        console.log('Simulated firmware update initiated, using: ' + request.payload.FwPackageURI);

        // Complete the response
        response.send(200, "Firmware update initiated", function(err) {
            if(!!err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });

        // Add logic here to perform the firmware update asynchronously
    }
    ```

1. Bir istemci örneği oluşturmak için aşağıdaki kodu ekleyin:

    ```nodejs
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. Aşağıdaki kodu ekleyin:

    * Bağlantıyı açın.
    * Gönderme **Deviceınfo** nesne.
    * İstenen özellikleri için bir işleyici ayarlayın.
    * Bildirilen özellikleri gönderir.
    * Doğrudan yöntemler için işleyiciler kaydedin.
    * Telemetri göndermeye başlaması.

    ```nodejs
    client.open(function (err) {
        if (err) {
            printErrorFor('open')(err);
        } else {
            console.log('Sending device metadata:\n' + JSON.stringify(deviceMetaData));
            client.sendEvent(new Message(JSON.stringify(deviceMetaData)), printErrorFor('send metadata'));

            // Create device twin
            client.getTwin(function(err, twin) {
                if (err) {
                    console.error('Could not get device twin');
                } else {
                    console.log('Device twin created');

                    twin.on('properties.desired', function(delta) {
                        console.log('Received new desired properties:');
                        console.log(JSON.stringify(delta));
                    });

                    // Send reported properties
                    twin.properties.reported.update(reportedProperties, function(err) {
                        if (err) throw err;
                        console.log('twin state reported');
                    });

                    // Register handlers for direct methods
                    client.onDeviceMethod('Reboot', onReboot);
                    client.onDeviceMethod('InitiateFirmwareUpdate', onInitiateFirmwareUpdate);
                }
            });

            // Start sending telemetry
            var sendInterval = setInterval(function () {
                temperature += generateRandomIncrement();
                externalTemperature += generateRandomIncrement();
                humidity += generateRandomIncrement();

                var data = JSON.stringify({
                    'DeviceID': deviceId,
                    'Temperature': temperature,
                    'Humidity': humidity,
                    'ExternalTemperature': externalTemperature
                });

                console.log('Sending device event data:\n' + data);
                client.sendEvent(new Message(data), printErrorFor('send event'));
            }, 5000);

            client.on('error', function (err) {
                printErrorFor('client')(err);
                if (sendInterval) clearInterval(sendInterval);
                client.close(printErrorFor('client.close'));
            });
        }
    });
    ```

1. Değişiklikleri kaydetmek **remote_monitoring.js** dosya.

1. Örnek uygulamayı başlatmak için komut isteminde aşağıdaki komutu çalıştırın:
   
    ```
    node remote_monitoring.js
    ```

[!INCLUDE [iot-suite-v1-visualize-connecting](../../includes/iot-suite-v1-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdk-node
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
