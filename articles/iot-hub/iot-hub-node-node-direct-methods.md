---
title: "Azure IOT hub'ı doğrudan yöntemleri (düğüm) | Microsoft Docs"
description: "Azure IOT hub'ı doğrudan yöntemlerinin nasıl kullanılacağını. Node.js için Azure IOT SDK'ları doğrudan bir yöntem içeren bir sanal cihaz uygulamasının ve doğrudan yöntemini çağıran bir hizmet uygulaması uygulamak için kullanın."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: ea9c73ca-7778-4e38-a8f1-0bee9d142f04
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nberdy
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9a73c25724a239e56c3ea62a8452bb7c3a2b51be
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-direct-methods-on-your-iot-device-with-nodejs"></a>IOT cihazınızla Node.js doğrudan yöntemleri kullan
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

Bu öğreticinin sonunda iki Node.js konsol uygulamaları vardır:

* **CallMethodOnDevice.js**, sanal cihaz uygulamasının bir yöntemi çağırır ve yanıt görüntüler.
* **SimulatedDevice.js**, daha önce oluşturulan cihaz kimliğiyle IOT hub'ınıza bağlanır ve bulut tarafından çağrılan yöntemi yanıt verir.

> [!NOTE]
> [IoT Hub SDK'ları][lnk-hub-sdks] makalesi, hem cihazlarınızda hem de çözüm arka ucunuzda çalıştırılacak uygulamalar oluşturmak için kullanabileceğiniz Azure IoT SDK’ları hakkında bilgi içerir.
> 
> 

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Node.js 4.0.x sürümü veya sonraki bir sürüm.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma
Bu bölümde, bulut tarafından adlı bir yöntem yanıtlaması bir Node.js konsol uygulaması oluşturun.

1. **simulateddevice** adlı yeni bir boş klasör oluşturun. Komut isteminizde aşağıdaki komutu kullanarak **simulateddevice** klasöründe bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:
   
    ```
    npm init
    ```
2. Komut isteminizde **simulateddevice** klasöründe **azure-iot-device** Cihaz SDK paketini ve **azure-iot-device-mqtt** paketini yüklemek için aşağıdaki komutu çalıştırın:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Bir metin düzenleyicisi kullanarak **simulateddevice** klasöründe yeni bir **SimulatedDevice.js** dosyası oluşturun.
4. Aşağıdaki `require` deyimlerini **SimulatedDevice.js** dosyasının başlangıcına ekleyin:
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. Ekleme bir **connectionString** değişkeni ve oluşturmak için kullanın bir **DeviceClient** örneği. Değiştir **{cihaz bağlantı dizesi}** cihaz bağlantısı, üretildi dize *bir cihaz kimliği oluşturma* bölümü:
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. Yöntemi aygıtta uygulamak için aşağıdaki işlevi ekleyin:
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. Initialize yöntemi dinleyicisi IOT hub ve başlangıç bağlantı Aç:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```
8. **SimulatedDevice.js** dosyasını kaydedin ve kapatın.

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda yeniden deneme ilkelerini (bağlantı yeniden deneme), önerilen MSDN makalesinde uygulamalıdır [geçici hata işleme][lnk-transient-faults].
> 
> 

## <a name="call-a-method-on-a-device"></a>Bir cihazda bir yöntemini çağırın
Bu bölümde, sanal cihaz uygulamasının bir yöntemi çağırır ve yanıt görüntüleyen bir Node.js konsol uygulaması oluşturun.

1. Adlı yeni bir boş klasör oluşturun **callmethodondevice**. İçinde **callmethodondevice** klasörü, komut isteminde aşağıdaki komutu kullanarak bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:
   
    ```
    npm init
    ```
2. Komut isteminizde **callmethodondevice** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure-iothub** paketi:
   
    ```
    npm install azure-iothub --save
    ```
3. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **CallMethodOnDevice.js** dosyasını **callmethodondevice** klasör.
4. Aşağıdakileri ekleyin `require` deyimleri başlangıcında **CallMethodOnDevice.js** dosyası:
   
    ```
    'use strict';
   
    var Client = require('azure-iothub').Client;
    ```
5. Aşağıdaki değişken bildirimini ekleyin ve yer tutucu değerini hub'ınızın IoT Hub'ı bağlantı dizesiyle değiştirin:
   
    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```
6. IOT hub'ınıza bağlantıyı açmak için istemci oluşturun.
   
    ```
    var client = Client.fromConnectionString(connectionString);
    ```
7. Aygıt yöntemini çağırmak ve konsola aygıt yanıtı yazdırmak için aşağıdaki işlevi ekleyin:
   
    ```
    var methodParams = {
        methodName: methodName,
        payload: 'hello world',
        timeoutInSeconds: 30
    };
   
    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed to invoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```
8. Kaydet ve Kapat **CallMethodOnDevice.js** dosya.

## <a name="run-the-apps"></a>Uygulamaları çalıştırma
Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Bir komut isteminde **simulateddevice** klasörü, IOT Hub'ınızı gelen yöntem çağrıları için dinleme başlatmak için aşağıdaki komutu çalıştırın:
   
    ```
    node SimulatedDevice.js
    ```
   
    ![][7]
2. Bir komut isteminde **callmethodondevice** klasörü, IOT hub'ınızı izlemeye başlamak için aşağıdaki komutu çalıştırın:
   
    ```
    node CallMethodOnDevice.js 
    ```
   
    ![][8]
3. İleti ve yöntemi görüntü aygıttan yanıt olarak adlandırılan uygulama yazdırma tarafından yönteme tepki aygıt görürsünüz:
   
    ![][9]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Bu cihaz kimliğini bulut tarafından çağrılan yöntemler tepki vermek sanal cihaz uygulamasının sağlamak için kullanılır. Ayrıca aygıtta yöntemleri çağırır ve aygıttan yanıt görüntüleyen bir uygulama oluşturduğunuz. 

IoT Hub’ı kullanmaya başlamak ve diğer IoT senaryolarını keşfetmek için bkz:

* [IOT Hub ile çalışmaya başlama]
* [Birden çok aygıta işleri zamanla][lnk-devguide-jobs]

Çözüm ve zamanlama yöntemini çağıran birden fazla cihazda, IOT genişletmek öğrenmek için bkz: [zamanlama ve yayın işleri] [ lnk-tutorial-jobs] Öğreticisi.

<!-- Images. -->
[7]: ./media/iot-hub-node-node-direct-methods/run-simulated-device.png
[8]: ./media/iot-hub-node-node-direct-methods/run-callmethodondevice.png
[9]: ./media/iot-hub-node-node-direct-methods/methods-output.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[IOT Hub ile çalışmaya başlama]: iot-hub-node-node-getstarted.md
