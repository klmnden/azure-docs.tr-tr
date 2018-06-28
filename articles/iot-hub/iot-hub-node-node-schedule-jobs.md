---
title: Zamanlama işlerini ile Azure IOT hub'ı (düğüm) | Microsoft Docs
description: Birden çok aygıta doğrudan bir yöntemi çağırmak için bir Azure IOT Hub işini zamanlamak nasıl. Sanal cihaz uygulamaları ve işi çalıştırmak için bir hizmet uygulaması uygulamak için Node.js için Azure IOT SDK'ları kullanın.
author: juanjperez
manager: cberlin
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 10/06/2017
ms.author: juanpere
ms.openlocfilehash: 42deb210c55cd4a6c2aa2c7757ed87f8f706c58f
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "34634116"
---
# <a name="schedule-and-broadcast-jobs-node"></a>Zamanlama ve yayın işleri (düğüm)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IOT Hub oluşturma ve zamanlama ve milyonlarca cihaza güncelleştirme işleri izlemek için bir arka uç uygulaması sağlayan tam olarak yönetilen bir hizmettir.  İşlerini için aşağıdaki eylemler kullanılabilir:

* İstenen özellikleri güncelleştirme
* Güncelleştirme etiketleri
* Doğrudan yöntemleri çağırma

Kavramsal olarak, bir işi şu eylemlerden birini sarmalar ve bir cihaz çifti sorgu tarafından tanımlanan bir dizi aygıtlar, yürütme ilerlemesini izler.  Örneğin, bir arka uç uygulaması, bir iş 10.000 cihaz, cihaz çifti sorgu tarafından belirtilen ve gelecek bir zamanda zamanlanmış bir yeniden başlatma yöntemini çağırmak için kullanabilirsiniz.  Bu cihazların her biri almak ve yeniden başlatma yöntemini çalıştırmak gibi uygulama sonra ilerleme durumunu izleyebilirsiniz.

Bu makaleler, bu özelliklerin her biri hakkında daha fazla bilgi edinin:

* Cihaz çifti ve özellikleri: [cihaz çiftlerini ile çalışmaya başlama] [ lnk-get-started-twin] ve [öğretici: cihaz çifti özellikleri kullanma][lnk-twin-props]
* Doğrudan yöntemleri: [IOT Hub Geliştirici Kılavuzu - doğrudan yöntemleri] [ lnk-dev-methods] ve [Öğreticisi: doğrudan yöntemleri][lnk-c2d-methods]

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* Etkinleştirir doğrudan bir yönteme sahip bir Node.js sanal cihaz uygulaması oluşturma **lockDoor**, hangi çağrılabilir çözüm arka ucu tarafından.
* Çağıran bir Node.js konsol uygulaması oluşturma **lockDoor** yöntemi bir işi ve güncelleştirmeleri cihaz işi kullanarak istenen özelliklerini kullanan sanal cihaz uygulamasında doğrudan.

Bu öğreticinin sonunda iki Node.js uygulamalarını vardır:

**simDevice.js**, hangi cihaz kimliğiyle IOT hub'ınıza bağlanır ve alan bir **lockDoor** doğrudan yöntemi.

**scheduleJobService.js**, sanal cihaz uygulamasının doğrudan bir yöntemi çağırır ve cihaz çifti 's güncelleştirmeleri bir işi kullanarak özelliklerini istenen.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Node.js sürümünü 4.0.x veya sonraki bir sürümü <br/>  [Geliştirme ortamınızı hazırlama] [ lnk-dev-setup] Node.js Bu öğretici için Windows veya Linux'ta nasıl yükleneceğini açıklar.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma
Bu bölümde, oluşturduğunuz sanal tetikler bulut tarafından adlı doğrudan bir yöntem yanıtlaması bir Node.js konsol uygulaması **lockDoor** yöntemi.

1. Adlı yeni bir boş klasör oluşturun **simDevice**.  İçinde **simDevice** klasörü, komut isteminde aşağıdaki komutu kullanarak bir package.json dosyası oluşturun.  Tüm varsayılanları kabul edin:
   
    ```
    npm init
    ```
2. Komut isteminizde **simDevice** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure IOT cihaz** cihaz SDK'sı paketinin ve **azure-IOT-cihaz-mqtt** paketi:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Bir metin düzenleyicisi kullanarak yeni bir oluşturma **simDevice.js** dosyasını **simDevice** klasör.
4. Aşağıdaki 'İste' deyimleri başlangıcında eklemek **simDevice.js** dosyası:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. Bir **connectionString** değişkeni ekleyin ve bir **İstemci** örneği oluşturmak için bunu kullanın.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. İşlemek için aşağıdaki işlevi ekleyin **lockDoor** yöntemi.
   
    ```
    var onLockDoor = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        console.log('Locking Door!');
    };
    ```
7. İşleyicisi kaydetmek için aşağıdaki kodu ekleyin **lockDoor** yöntemi.
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub. Register handler for lockDoor direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```
8. Kaydet ve Kapat **simDevice.js** dosya.

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. [Geçici Hata İşleme][lnk-transient-faults] adlı MSDN makalesinde önerildiği üzere, üretim kodunda yeniden deneme ilkelerini (üstel geri alma gibi) uygulamanız gerekir.
> 
> 

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a>İşlerini doğrudan bir yöntemi çağırmak ve bir cihaz çifti'nın özelliklerini güncelleştirmek için zamanla
Bu bölümde, bir uzak başlatan bir Node.js konsol uygulaması oluşturma **lockDoor** bir aygıtta doğrudan bir yöntem kullanarak ve cihaz çifti'nın özelliklerini güncelleştirir.

1. Adlı yeni bir boş klasör oluşturun **scheduleJobService**.  İçinde **scheduleJobService** klasörü, komut isteminde aşağıdaki komutu kullanarak bir package.json dosyası oluşturun.  Tüm varsayılanları kabul edin:
   
    ```
    npm init
    ```
2. Komut isteminizde **scheduleJobService** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure-iothub** cihaz SDK'sı paketinin ve **azure-IOT-cihaz-mqtt** paketi:
   
    ```
    npm install azure-iothub uuid --save
    ```
3. Bir metin düzenleyicisi kullanarak yeni bir oluşturma **scheduleJobService.js** dosyasını **scheduleJobService** klasör.
4. Aşağıdaki 'İste' deyimleri başlangıcında eklemek **dmpatterns_gscheduleJobServiceetstarted_service.js** dosyası:
   
    ```
    'use strict';
   
    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```
5. Aşağıdaki değişken bildirimlerini ekleme ve yer tutucu değerlerini değiştirin:
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var queryCondition = "deviceId IN ['myDeviceId']";
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  300;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
6. İş yürütme izlemek için kullanılan aşağıdaki işlevi ekleyin:
   
    ```
    function monitorJob (jobId, callback) {
        var jobMonitorInterval = setInterval(function() {
            jobClient.getJob(jobId, function(err, result) {
            if (err) {
                console.error('Could not get job status: ' + err.message);
            } else {
                console.log('Job: ' + jobId + ' - status: ' + result.status);
                if (result.status === 'completed' || result.status === 'failed' || result.status === 'cancelled') {
                clearInterval(jobMonitorInterval);
                callback(null, result);
                }
            }
            });
        }, 5000);
    }
    ```
7. Aygıt yöntemini çağırır işini zamanlamak için aşağıdaki kodu ekleyin:
   
    ```
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        responseTimeoutInSeconds: 15 // Timeout after 15 seconds if device is unable to process method
    };
   
    var methodJobId = uuid.v4();
    console.log('scheduling Device Method job with id: ' + methodJobId);
    jobClient.scheduleDeviceMethod(methodJobId,
                                queryCondition,
                                methodParams,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule device method job: ' + err.message);
        } else {
            monitorJob(methodJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor device method job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
8. Cihaz çifti güncelleştirmek için işini zamanlamak için aşağıdaki kodu ekleyin:
   
    ```
    var twinPatch = {
       etag: '*', 
       properties: {
           desired: {
               building: '43', 
               floor: 3
           }
       }
    };
   
    var twinJobId = uuid.v4();
   
    console.log('scheduling Twin Update job with id: ' + twinJobId);
    jobClient.scheduleTwinUpdate(twinJobId,
                                queryCondition,
                                twinPatch,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule twin update job: ' + err.message);
        } else {
            monitorJob(twinJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor twin update job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
9. Kaydet ve Kapat **scheduleJobService.js** dosya.

## <a name="run-the-applications"></a>Uygulamaları çalıştırma
Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Komut isteminde **simDevice** klasörü, yeniden başlatma doğrudan yöntemi için dinleme başlamak için aşağıdaki komutu çalıştırın.
   
    ```
    node simDevice.js
    ```
2. Komut isteminde **scheduleJobService** klasörü, kapısını kilitleme ve twin güncelleştirmek için işleri tetiklemek için aşağıdaki komutu çalıştırın
   
    ```
    node scheduleJobService.js
    ```
3. Konsolunda doğrudan yöntemi aygıt yanıta bakın.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir cihaz ve cihaz çifti'nın özelliklerini güncelleştirmek için doğrudan bir yöntem zamanlamak için bir iş kullanılır.

IOT Hub ve Uzaktan gibi cihaz yönetim düzenleri hava bellenim güncelleştirme kullanmaya Başlarken devam etmek için bkz:

[Öğretici: bir üretici yazılımı güncelleştirmesi yapma][lnk-fwupdate]

IOT Hub ile çalışmaya başlama devam etmek için bkz: [Azure IOT Edge ile çalışmaya başlama][lnk-iot-edge].

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
