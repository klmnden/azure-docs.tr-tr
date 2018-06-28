---
title: Cihaz üretici yazılımı güncelleştirme ile Azure IOT hub'ı (düğüm) | Microsoft Docs
description: Cihaz Yönetimi Azure IOT hub'ına aygıt üretici yazılımı güncelleştirmesi başlatmak için nasıl kullanılacağını. Sanal cihaz uygulaması ve bellenim güncelleştirme tetikleyen bir hizmet uygulaması uygulamak için Node.js için Azure IOT SDK'ları kullanın.
author: juanjperez
manager: cberlin
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/07/2017
ms.author: juanpere
ms.openlocfilehash: 0cd8c019cf9a65e0e72227ba99c1995a45ed4067
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "34634979"
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-nodenode"></a>Bir cihaz üretici yazılımı güncelleştirmesi (düğümü/düğümü) başlatmak için cihaz Yönetimi'ni kullanın
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

İçinde [aygıt Management'i kullanmaya başlama] [ lnk-dm-getstarted] Öğreticisi, nasıl kullanılacağını gördüğünüz [cihaz çifti] [ lnk-devtwin] ve [doğrudan yöntemleri ] [ lnk-c2dmethod] temelleri uzaktan bir aygıt yeniden başlatma. Bu öğretici aynı IOT hub'ı temelleri kullanır ve rehberlik sağlar ve bir uçtan uca sanal üretici yazılımı güncelleştirme yapmak nasıl gösterir.  Bu deseni bellenim güncelleştirme uygulamasında Intel Edison'u aygıt örnek için kullanılır.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* IOT hub'ınız aracılığıyla sanal cihaz uygulamasında firmwareUpdate doğrudan yöntemini çağıran bir Node.js konsol uygulaması oluşturun.
* Arabirimini uygulayan bir sanal cihaz uygulaması oluşturma bir **firmwareUpdate** doğrudan yöntemi. Bu yöntem bellenim görüntüsünü karşıdan yüklemek için bekleyeceği, bellenim görüntüyü indirir ve son bellenim görüntü geçerli bir çok aşama işlemi başlatır. Güncelleştirme her aşaması sırasında aygıt ilerlemesini bildirmek için bildirilen özelliklerini kullanır.

Bu öğreticinin sonunda iki Node.js konsol uygulamaları vardır:

**dmpatterns_fwupdate_service.js**yanıt görüntüler, sanal cihaz uygulamada, doğrudan bir yöntemi çağırır ve düzenli aralıklarla (her 500ms) görüntüler güncelleştirilmiş bildirilen özellikleri.

**dmpatterns_fwupdate_device.js**, daha önce oluşturulan cihaz kimliğiyle IOT hub'ınızı bağlayan firmwareUpdate doğrudan bir yöntem alır, bellenim güncelleştirme dahil benzetimini yapmak için çok durumlu bir işlem çalışır: görüntü için bekleniyor , yeni görüntüyü indirme ve son olarak görüntüyü uygulamadan indirin.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Node.js sürümünü 4.0.x veya sonraki bir sürümü <br/>  [Geliştirme ortamınızı hazırlama] [ lnk-dev-setup] Node.js Bu öğretici için Windows veya Linux'ta nasıl yükleneceğini açıklar.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

İzleyin [aygıt Management'i kullanmaya başlama](iot-hub-node-node-device-management-get-started.md) IOT hub'ınızı oluşturun ve IOT Hub bağlantı dizenizi almak için makale.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Doğrudan bir yöntem kullanarak aygıt bir uzak bellenim güncelleştirme Tetikle
Bu bölümde, bir cihazda uzaktan bellenim güncelleştirme başlatan bir Node.js konsol uygulaması oluşturun. Uygulama güncelleştirme başlatmak için doğrudan bir yöntem kullanır ve etkin bellenim güncelleştirme durumunu düzenli aralıklarla almak için cihaz çifti sorgularını kullanır.

1. Adlı bir boş klasör oluşturun **triggerfwupdateondevice**.  İçinde **triggerfwupdateondevice** klasörü, komut isteminde aşağıdaki komutu kullanarak bir package.json dosyası oluşturun.  Tüm varsayılanları kabul edin:
   
    ```
    npm init
    ```
2. Komut isteminizde **triggerfwupdateondevice** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure IOT hub** paketi:
   
    ```
    npm install azure-iothub --save
    ```
3. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **dmpatterns_getstarted_service.js** dosyasını **triggerfwupdateondevice** klasör.
4. Aşağıdaki 'İste' deyimleri başlangıcında eklemek **dmpatterns_getstarted_service.js** dosyası:
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. Aşağıdaki değişken bildirimlerini ekleme ve yer tutucu değerlerini değiştirin:
   
    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
6. Bulmak için aşağıdaki işlevi ekleyin ve özellik görüntü firmwareUpdate değerini bildirdi.
   
    ```
    var queryTwinFWUpdateReported = function() {
        registry.getTwin(deviceToUpdate, function(err, twin){
            if (err) {
              console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
            } else {
              console.log((JSON.stringify(twin.properties.reported.iothubDM.firmwareUpdate)) + "\n");
            }
        });
    };
    ```
7. Hedef aygıt yeniden firmwareUpdate yöntemini çağırmak için aşağıdaki işlevi ekleyin:
   
    ```
    var startFirmwareUpdateDevice = function() {
      var params = {
          fwPackageUri: 'https://secureurl'
      };
   
      var methodName = "firmwareUpdate";
      var payloadData =  JSON.stringify(params);
   
      var methodParams = {
        methodName: methodName,
        payload: payloadData,
        timeoutInSeconds: 30
      };
   
      client.invokeDeviceMethod(deviceToUpdate, methodParams, function(err, result) {
        if (err) {
          console.error('Could not start the firmware update on the device: ' + err.message)
        } 
      });
    };
    ```
8. Son olarak, aşağıdaki işlev bellenim güncelleştirme sırası ve düzenli olarak bildirilen özelliklerini gösteren başlatmak için kodu ekleyin:
   
    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
9. Kaydet ve Kapat **dmpatterns_fwupdate_service.js** dosya.

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-the-apps"></a>Uygulamaları çalıştırma
Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Komut isteminde **manageddevice** klasörü, yeniden başlatma doğrudan yöntemi için dinleme başlamak için aşağıdaki komutu çalıştırın.
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. Komut isteminde **triggerfwupdateondevice** klasörü, Uzaktan yeniden başlatma ve sorgu son bulmak cihaz çifti için yeniden başlatma zamanı tetikleyicisi için şu komutu çalıştırın.
   
    ```
    node dmpatterns_fwupdate_service.js
    ```
3. Konsolunda doğrudan yöntemi aygıt yanıta bakın.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, doğrudan bir yöntem bir cihazda uzaktan Bellenim güncelleştirmesini tetiklemek için bildirilen özellikleri bellenim güncelleştirme durumunu izlemek için kullanılır ve.

Çözüm ve zamanlama yöntemini çağıran birden fazla cihazda, IOT genişletmek öğrenmek için bkz: [zamanlama ve yayın işleri] [ lnk-tutorial-jobs] Öğreticisi.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
