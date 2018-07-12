---
title: Cihaz üretici yazılımı güncelleştirmesi ile Azure IOT hub'ı (Node) | Microsoft Docs
description: Cihaz üretici yazılımı güncelleştirme başlatmak için Azure IOT Hub cihaz Yönetimi kullanma Node.js için Azure IOT SDK'ları, bir sanal cihaz uygulaması ve üretici yazılımı güncelleştirmesi tetikleyen bir hizmet uygulaması'nı uygulamak için kullanın.
author: juanjperez
manager: cberlin
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/07/2017
ms.author: juanpere
ms.openlocfilehash: 0cd8c019cf9a65e0e72227ba99c1995a45ed4067
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38452439"
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-nodenode"></a>Cihaz üretici yazılımı güncelleştirme (düğüm/Node) başlatmak için cihaz Yönetimi'ni kullanın
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

İçinde [cihaz yönetimini kullanmaya başlama] [ lnk-dm-getstarted] eğitmen, nasıl kullanılacağını gördüğünüz [cihaz ikizi] [ lnk-devtwin] ve [doğrudan yöntemler ] [ lnk-c2dmethod] ilkel bir cihazı Uzaktan yeniden başlatmak için. Bu öğretici aynı IOT hub'ı temelleri kullanır ve rehberlik sağlar ve bir uçtan uca sanal üretici yazılımı güncelleştirme işlemini gösterir.  Bu düzen, bellenim güncelleştirme uygulamasında Intel Edison cihaz örnek için kullanılır.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* IOT hub'ınız aracılığıyla sanal cihaz uygulamasında firmwareUpdate doğrudan yöntemi çağıran bir Node.js konsol uygulaması oluşturacaksınız.
* Uygulayan bir sanal cihaz uygulaması oluşturma bir **firmwareUpdate** doğrudan yöntemi. Bu yöntem, üretici yazılımı görüntüsünü indirmeye bekler, üretici yazılımı görüntüsünü indirir ve son olarak üretici yazılımı görüntüsünü geçerlidir çok aşamalı bir işlem başlatır. Güncelleştirmenin her aşamasında cihaz ilerlemesini bildirmek üzere bildirilen özellikleri kullanır.

Bu öğreticinin sonunda, iki Node.js konsol uygulamanız olacak:

**dmpatterns_fwupdate_service.js**bir doğrudan yöntem sanal cihaz uygulamasında çağıran yanıt görüntüler ve düzenli aralıklarla (her 500ms) görüntüler güncelleştirilmiş bildirilen özellikler.

**dmpatterns_fwupdate_device.js**, daha önce oluşturulan cihaz kimliğiyle IOT hub'ınıza bağlanan firmwareUpdate bir doğrudan yöntem alır, üretici yazılımı güncelleştirme dahil benzetimini yapmak için çok durumlu bir işlem yapması çalıştırır: görüntüsü için bekleniyor İndirme, yeni görüntüyü indirme ve son olarak görüntüsü uygulanıyor.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Node.js sürümü 4.0.x sürümü veya sonraki bir sürümü <br/>  [Geliştirme ortamınızı hazırlama] [ lnk-dev-setup] Node.js Bu öğretici için Windows veya Linux'ta nasıl yükleneceğini açıklar.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

İzleyin [cihaz yönetimini kullanmaya başlama](iot-hub-node-node-device-management-get-started.md) IOT hub'ınızı oluşturması ve IOT Hub bağlantı dizesini almak için makale.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Bir doğrudan yöntem kullanarak cihaz üzerinde bir uzak üretici yazılımı güncelleştirmesini tetikleme
Bu bölümde, bir cihazda uzak üretici yazılımı güncelleştirme başlatan bir Node.js konsol uygulaması oluşturun. Uygulamayı bir doğrudan yöntem güncelleştirmeyi başlatmak için ve cihaz çifti sorguları etkin üretici yazılımı güncelleştirme durumunu düzenli aralıklarla almak için kullanır.

1. Adlı bir boş klasör oluşturun **triggerfwupdateondevice**.  İçinde **triggerfwupdateondevice** klasöründe komut isteminizde aşağıdaki komutu kullanarak bir package.json dosyası oluşturun.  Tüm varsayılanları kabul edin:
   
    ```
    npm init
    ```
2. Komut isteminizde **triggerfwupdateondevice** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure IOT hub** paket:
   
    ```
    npm install azure-iothub --save
    ```
3. Bir metin düzenleyicisi kullanarak oluşturduğunuz bir **dmpatterns_getstarted_service.js** dosyası **triggerfwupdateondevice** klasör.
4. Aşağıdaki 'gerekli' başlangıcında deyimleri Ekle **dmpatterns_getstarted_service.js** dosyası:
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. Aşağıdaki değişken bildirimlerini ekleyin ve yer tutucu değerlerini değiştirin:
   
    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
6. Bulmak için aşağıdaki işlevi ekleyin ve görüntü firmwareUpdate değeri bildirilen özellik.
   
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
7. Hedef cihaz yeniden firmwareUpdate yöntemini çağırmak için aşağıdaki işlevi ekleyin:
   
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
8. Son olarak üretici yazılımı güncelleştirme dizisi ve düzenli olarak bildirilen özellikler gösteren başlatmak için kodu aşağıdaki işlevi ekleyin:
   
    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
9. Kaydet ve Kapat **dmpatterns_fwupdate_service.js** dosya.

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-the-apps"></a>Uygulamaları çalıştırma
Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Komut isteminde **manageddevice** klasörü için yeniden başlatma doğrudan yöntem dinleme başlamak için aşağıdaki komutu çalıştırın.
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. Komut isteminde **triggerfwupdateondevice** klasörü, Uzaktan yeniden başlatma ve son bulmak cihaz ikizi sorgusu zaman yeniden tetikleyicisi için şu komutu çalıştırın.
   
    ```
    node dmpatterns_fwupdate_service.js
    ```
3. Doğrudan yöntem konsolunda cihaz yanıtı görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir cihazda bir uzak üretici yazılımı güncelleştirmesini tetikleme için kullanılan bir doğrudan yöntem ve bildirilen özellikleri üretici yazılımı güncelleştirme durumunu izlemek için kullanılır.

IOT çözümü ve zamanlama yöntemi çağıran birden çok cihazda genişletmek öğrenmek için bkz [işleri zamanlama ve yayınlama] [ lnk-tutorial-jobs] öğretici.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
