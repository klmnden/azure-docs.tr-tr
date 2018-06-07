---
title: Azure IOT Hub cihaz Yönetimi (düğüm) ile çalışmaya başlama | Microsoft Docs
description: IOT Hub cihaz yönetimine uzak aygıt yeniden başlatma işlemi başlatmak için nasıl kullanılacağını. Node.js için Azure IOT SDK'sı, doğrudan bir yöntem içeren bir sanal cihaz uygulamasının ve doğrudan yöntemini çağıran bir hizmet uygulaması uygulamak için kullanın.
author: juanjperez
manager: cberlin
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/25/2017
ms.author: juanpere
ms.openlocfilehash: 54658ea72ac8e32db45774e87e3ab177d68046fa
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34635959"
---
# <a name="get-started-with-device-management-node"></a>Aygıt Yönetimi (düğüm) ile çalışmaya başlama

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* IOT hub'ı oluşturun ve IOT hub'ınızda bir cihaz kimliği oluşturma için Azure Portalı'nı kullanın.
* Bu cihazı yeniden başlatır doğrudan bir yöntem içeren bir sanal cihaz uygulaması oluşturursunuz. Doğrudan yöntemleri buluttan çağrılır.
* IOT hub'ınız aracılığıyla sanal cihaz uygulama yeniden başlatma doğrudan yöntemini çağıran bir Node.js konsol uygulaması oluşturun.

Bu öğreticinin sonunda iki Node.js konsol uygulamaları vardır:

**dmpatterns_getstarted_device.js**, daha önce oluşturulan cihaz kimliğiyle IOT hub'ınızı bağlayan bir yeniden başlatma doğrudan yöntemi alır fiziksel yeniden başlatma taklit eder ve son yeniden başlatma zamanı raporlar.

**dmpatterns_getstarted_service.js**, yanıt görüntüler, sanal cihaz uygulamada, doğrudan bir yöntemi çağırır ve görüntüler güncelleştirilmiş rapor özellikleri.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Node.js sürümünü 4.0.x veya sonraki bir sürümü <br/>  [Geliştirme ortamınızı hazırlama] [ lnk-dev-setup] Node.js Bu öğretici için Windows veya Linux'ta nasıl yükleneceğini açıklar.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma
Bu bölümde şunları yapacaksınız:

* Bulut tarafından çağrılan doğrudan bir yönteme yanıt veren bir Node.js konsol uygulaması oluşturma
* Tetikleyici sanal cihaz yeniden başlatma
* Aygıtları ve bunların en son ne zaman yeniden tanımlamak için cihaz çifti sorgular etkinleştirmek için bildirilen özelliklerini kullanın

1. **manageddevice** adlı boş bir klasör oluşturun.  Komut isteminizde aşağıdaki komutu kullanarak **manageddevice** klasöründe bir package.json dosyası oluşturun.  Tüm varsayılanları kabul edin:
   
    ```
    npm init
    ```
2. Komut isteminizde **manageddevice** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure IOT cihaz** cihaz SDK'sı paketinin ve **azure-IOT-cihaz-mqtt** paketi:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **dmpatterns_getstarted_device.js** dosyasını **manageddevice** klasör.
4. Aşağıdaki 'İste' deyimleri başlangıcında eklemek **dmpatterns_getstarted_device.js** dosyası:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. Bir **connectionString** değişkeni ekleyin ve bir **İstemci** örneği oluşturmak için bunu kullanın.  Bağlantı dizesi, cihaz bağlantı dizesi ile değiştirin.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. Cihazda doğrudan yöntemi uygulamak için aşağıdaki işlevi ekleyin
   
    ```
    var onReboot = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
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
7. IOT hub'ınıza bağlantıyı açın ve doğrudan yöntemi dinleyicisi başlatın:
   
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
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. [Geçici Hata İşleme][lnk-transient-faults] adlı MSDN makalesinde önerildiği üzere, üretim kodunda yeniden deneme ilkelerini (üstel geri alma gibi) uygulamanız gerekir.

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Tetikleyici doğrudan bir yöntem kullanarak cihaz üzerinde Uzaktan yeniden başlatma
Bu bölümde, doğrudan bir yöntem kullanarak bir cihazda Uzaktan yeniden başlatma başlatan bir Node.js konsol uygulaması oluşturun. Uygulama cihaz çifti sorguları bu aygıtın son yeniden başlatma zamanını bulmak için kullanır.

1. Adlı bir boş klasör oluşturun **triggerrebootondevice**.  İçinde **triggerrebootondevice** klasörü, komut isteminde aşağıdaki komutu kullanarak bir package.json dosyası oluşturun.  Tüm varsayılanları kabul edin:
   
    ```
    npm init
    ```
2. Komut isteminizde **triggerrebootondevice** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure-iothub** cihaz SDK'sı paketinin ve **azure-IOT-cihaz-mqtt** paketi:
   
    ```
    npm install azure-iothub --save
    ```
3. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **dmpatterns_getstarted_service.js** dosyasını **triggerrebootondevice** klasör.
4. Aşağıdaki 'İste' deyimleri başlangıcında eklemek **dmpatterns_getstarted_service.js** dosyası:
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. Aşağıdaki değişken bildirimlerini ekleme ve yer tutucu değerlerini değiştirin:
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
6. Hedef aygıt yeniden başlatma için cihaz yöntemini çağırmak için aşağıdaki işlevi ekleyin:
   
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
7. Aygıt için sorgu ve son yeniden başlatma zamanı elde etmek için aşağıdaki işlevi ekleyin:
   
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
8. Yeniden başlatma doğrudan yöntemi ve sorgu son yeniden başlatma zamanını tetiklemek işlevleri çağırmak için aşağıdaki kodu ekleyin:
   
    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
9. Kaydet ve Kapat **dmpatterns_getstarted_service.js** dosya.

## <a name="run-the-apps"></a>Uygulamaları çalıştırma
Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Komut isteminde **manageddevice** klasörü, yeniden başlatma doğrudan yöntemi için dinleme başlamak için aşağıdaki komutu çalıştırın.
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. Komut isteminde **triggerrebootondevice** klasörü, Uzaktan yeniden başlatma ve sorgu son bulmak cihaz çifti için yeniden başlatma zamanı tetikleyicisi için şu komutu çalıştırın.
   
    ```
    node dmpatterns_getstarted_service.js
    ```
3. Konsolunda doğrudan yöntemi aygıt yanıta bakın.

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
