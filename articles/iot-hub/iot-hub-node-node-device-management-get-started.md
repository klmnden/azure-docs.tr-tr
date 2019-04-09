---
title: Azure IOT Hub cihaz yönetimini (Node) kullanmaya başlama | Microsoft Docs
description: IOT Hub cihaz Yönetimi uzak cihazı yeniden başlatmak için kullanma Node.js için Azure IOT SDK'sı, bir doğrudan yöntem içeren bir sanal cihaz uygulaması ve doğrudan yöntemini çağıran bir hizmet uygulaması'nı uygulamak için kullanın.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/25/2017
ms.openlocfilehash: 15166d3943bc72a2eeff3580cefdd264ecaba61d
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59258094"
---
# <a name="get-started-with-device-management-node"></a>Cihaz yönetimini (Node) kullanmaya başlama

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* Kullanım [Azure portalında](https://portal.azure.com) IOT Hub oluşturma ve IOT hub'ınızda bir cihaz kimliği oluşturma.

* Bu cihazı yeniden başlatır bir doğrudan yöntem içeren bir sanal cihaz uygulaması oluşturma. Doğrudan yöntemler buluttan çağrılır.

* IOT hub'ınız aracılığıyla sanal cihaz uygulaması, yeniden başlatma doğrudan yöntemi çağıran bir Node.js konsol uygulaması oluşturacaksınız.

Bu öğreticinin sonunda, iki Node.js konsol uygulamanız olacak:

* **dmpatterns_getstarted_device.js**, daha önce oluşturulan cihaz kimliğiyle IOT hub'ınızı bağlayan bir yeniden başlatma doğrudan yöntem alıp fiziksel sistemin yeniden başlatılması benzetimini yapar ve zamanı son yeniden başlatma için raporlar.

* **dmpatterns_getstarted_service.js**bir doğrudan yöntem sanal cihaz uygulamasında çağıran yanıt görüntüler ve görüntüler güncelleştirilmiş bildirilen özellikler.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Node.js 4.0.x sürümü veya sonraki bir sürüm. [Geliştirme ortamınızı hazırlama](https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md) Node.js Bu öğretici için Windows veya Linux'ta nasıl yükleneceğini açıklar.

* Etkin bir Azure hesabı. (Hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.)

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>IOT hub için bağlantı dizesi alma

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma

Bu bölümde, aşağıdaki adımları gerçekleştireceksiniz:

* Bulut tarafından çağrılan doğrudan bir yönteme yanıt veren bir Node.js konsol uygulaması oluşturma

* Sanal cihazı yeniden başlatma tetikleyin

* Cihaz ikizi sorgularının cihazları ve bunların en son ne zaman yeniden tanımlamak üzere bildirilen özellikleri kullanın

1. **manageddevice** adlı boş bir klasör oluşturun.  Komut isteminizde aşağıdaki komutu kullanarak **manageddevice** klasöründe bir package.json dosyası oluşturun.  Tüm varsayılanları kabul edin:
      
    ```
    npm init
    ```

2. Komut isteminizde **manageddevice** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure IOT cihaz** cihaz SDK paketini ve **azure-iot-device-mqtt** Paket:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

3. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **dmpatterns_getstarted_device.js** dosyası **manageddevice** klasör.

4. Aşağıdaki 'gerekli' başlangıcında deyimleri Ekle **dmpatterns_getstarted_device.js** dosyası:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Bir **connectionString** değişkeni ekleyin ve bir **İstemci** örneği oluşturmak için bunu kullanın.  Bağlantı dizesi, cihaz bağlantı dizesiyle değiştirin.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Doğrudan yöntemi cihazda uygulamak için aşağıdaki işlevi ekleyin
   
    ```
    var onReboot = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        // Report the reboot before the physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };
   
        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
   
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```

7. IOT hub'ınıza bağlantıyı açın ve doğrudan yöntem dinleyicisi başlatın:

   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```

8. Kaydet ve Kapat **dmpatterns_getstarted_device.js** dosya.

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda yeniden deneme ilkelerini (örneğin, bir üstel geri alma), makalesinde önerildiği uygulamalıdır [geçici hata işleme](/azure/architecture/best-practices/transient-faults).

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Bir doğrudan yöntem kullanarak cihaz üzerinde Uzaktan yeniden başlatma tetikleyin

Bu bölümde, bir cihazda doğrudan yöntem kullanarak uzaktan yeniden başlatma başlatan bir Node.js konsol uygulaması oluşturun. Uygulama, son yeniden başlatma zamanı bu cihaz için keşfetmek için cihaz ikizi sorgularını kullanır.

1. Adlı bir boş klasör oluşturun **triggerrebootondevice**. İçinde **triggerrebootondevice** klasöründe komut isteminizde aşağıdaki komutu kullanarak bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:
   
    ```
    npm init
    ```

2. Komut isteminizde **triggerrebootondevice** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure-iothub** cihaz SDK paketini ve **azure-iot-device-mqtt** Paket:
   
    ```
    npm install azure-iothub --save
    ```

3. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **dmpatterns_getstarted_service.js** dosyası **triggerrebootondevice** klasör.

4. Aşağıdaki 'gerekli' başlangıcında deyimleri Ekle **dmpatterns_getstarted_service.js** dosyası:

  
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Aşağıdaki değişken bildirimlerini ekleyin ve yer tutucu değerlerini değiştirin:

   
    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```

6. Hedef cihazı yeniden başlatmak için cihaz yöntemini çağırmak için aşağıdaki işlevi ekleyin:
   
    ```
    var startRebootDevice = function(twin) {
   
        var methodName = "reboot";
   
        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };
   
        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) {
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked the device to reboot.");  
            }
        });
    };
    ```

7. Cihaz için sorgulama ve son yeniden başlatma zamanı almak için aşağıdaki işlevi ekleyin:
   
    ```
    var queryTwinLastReboot = function() {
   
        registry.getTwin(deviceToReboot, function(err, twin){
   
            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device to report last reboot time.');
        });
    };
    ```

8. Sorgu ve yeniden başlatma doğrudan yöntem son kez yeniden başlatma tetikleyin işlevleri çağırmak için aşağıdaki kodu ekleyin:

   
    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```

9. Kaydet ve Kapat **dmpatterns_getstarted_service.js** dosya.

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Komut isteminde **manageddevice** klasörü için yeniden başlatma doğrudan yöntem dinleme başlamak için aşağıdaki komutu çalıştırın.

   
    ```
    node dmpatterns_getstarted_device.js
    ```

2. Komut isteminde **triggerrebootondevice** klasörü, Uzaktan yeniden başlatma ve son bulmak cihaz ikizi sorgusu zaman yeniden tetikleyicisi için şu komutu çalıştırın.

   
    ```
    node dmpatterns_getstarted_service.js
    ```

3. Doğrudan yöntem konsolunda cihaz yanıtı görürsünüz.

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]