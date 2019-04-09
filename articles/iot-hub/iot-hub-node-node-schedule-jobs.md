---
title: Azure IOT hub'ı (Node) ile işleri zamanlama | Microsoft Docs
description: Birden fazla cihazda doğrudan yöntem çağırmak için bir Azure IOT hub'ı işini zamanlamak nasıl. Node.js için Azure IOT SDK'ları, sanal cihaz uygulamaları ve işi çalıştırmak için bir hizmet uygulaması'nı uygulamak için kullanın.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 10/06/2017
ms.openlocfilehash: 1f357ed60e9d9f020d5a80ac9349eb65577521e7
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59267608"
---
# <a name="schedule-and-broadcast-jobs-node"></a>Zamanlama ve yayınlama işleri (Node)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IOT Hub oluşturma ve zamanlama ve milyonlarca cihaza güncelleştirme işleri izlemek bir arka uç uygulaması sağlayan tam olarak yönetilen bir hizmettir.  İşleri için aşağıdaki eylemler kullanılabilir:

* İstenen özellikleri güncelleştirme
* Etiketleri güncelleştirin
* Doğrudan metotları çağırma

Kavramsal olarak, bir işi bu eylemlerden biri, sarmalar ve bir cihaz ikizi sorgu tarafından tanımlanan bir dizi cihazda karşı yürütme ilerleme durumunu izler.  Örneğin, bir arka uç uygulaması, bir işi 10.000 cihaz, cihaz ikizi sorgu tarafından belirtilen ve gelecekteki zaman için programlanmış bir yeniden başlatma yöntemini çağırmak için kullanabilirsiniz. Bu cihazların her biri alın ve yeniden başlatma yöntemi uygulamak, uygulamanın sonra ilerleme durumunu izleyebilirsiniz.

Bu makaleler, bu özelliklerin her biri hakkında daha fazla bilgi edinin:

* Cihaz ikizi ve özellikleri: [Cihaz ikizlerini kullanmaya başlama](iot-hub-node-node-twin-getstarted.md) ve [Öğreticisi: Cihaz ikizi özelliklerini kullanma](tutorial-device-twins.md)

* Doğrudan yöntemler: [IOT Hub Geliştirici Kılavuzu - doğrudan yöntemler](iot-hub-devguide-direct-methods.md) ve [Öğreticisi: doğrudan yöntemler](quickstart-control-device-node.md)

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* Sahip sağlayan bir doğrudan yöntem Node.js sanal cihaz uygulaması oluşturma **lockDoor**, hangi çağrılabilir çözüm arka ucu tarafından.

* Çağıran bir Node.js konsol uygulaması oluşturacaksınız **lockDoor** yöntemi bir iş ve güncelleştirmeleri kullanarak bir cihaz iş istenen özellikleri kullanarak sanal cihaz uygulamasında doğrudan.

Bu öğreticinin sonunda iki Node.js uygulamaları vardır:

* **simDevice.js**, cihaz kimliğiyle IOT hub'ınıza bağlanır ve aldığı kaydeden bir **lockDoor** doğrudan yöntemi.

* **scheduleJobService.js**, sanal cihaz uygulamasında bir doğrudan yöntem çağrıları ve cihaz ikizinin güncelleştirmeleri kullanarak bir proje özelliklerini istenen.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Node.js sürümü 4.0.x sürümü veya üzeri [geliştirme ortamınızı hazırlama](https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md) Node.js Bu öğretici için Windows veya Linux'ta nasıl yükleneceğini açıklar.

* Etkin bir Azure hesabı. (Hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.)

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>IOT hub için bağlantı dizesi alma

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma

Bu bölümde, bir sanal tetikler bulut tarafından çağrılan doğrudan bir yönteme yanıt veren bir Node.js konsol uygulaması oluşturma **lockDoor** yöntemi.

1. Adlı yeni bir boş klasör oluşturun **simDevice**.  İçinde **simDevice** klasöründe komut isteminizde aşağıdaki komutu kullanarak bir package.json dosyası oluşturun.  Tüm varsayılanları kabul edin:

   ```
   npm init
   ```

2. Komut isteminizde **simDevice** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure IOT cihaz** cihaz SDK paketini ve **azure-iot-device-mqtt** paket:
   
   ```
   npm install azure-iot-device azure-iot-device-mqtt --save
   ```

3. Bir metin düzenleyicisi kullanarak yeni bir oluşturma **simDevice.js** dosyası **simDevice** klasör.

4. Aşağıdaki 'gerekli' başlangıcında deyimleri Ekle **simDevice.js** dosyası:
   
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
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        console.log('Locking Door!');
    };
    ```

7. İşleyicisini kaydetmek için aşağıdaki kodu ekleyin **lockDoor** yöntemi.

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
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda yeniden deneme ilkelerini (örneğin, bir üstel geri alma), makalesinde önerildiği uygulamalıdır [geçici hata işleme](/azure/architecture/best-practices/transient-faults).
>

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a>Bir doğrudan yöntem çağırma ve bir cihaz ikizinin özelliklerini güncelleştirmek için işleri zamanlama

Bu bölümde, bir uzak başlatan bir Node.js konsol uygulaması oluşturma **lockDoor** bir cihazda doğrudan yöntem kullanarak ve cihaz ikizinin özelliklerini güncelleştirir.

1. Adlı yeni bir boş klasör oluşturun **scheduleJobService**.  İçinde **scheduleJobService** klasöründe komut isteminizde aşağıdaki komutu kullanarak bir package.json dosyası oluşturun.  Tüm varsayılanları kabul edin:

    ```
    npm init
    ```

2. Komut isteminizde **scheduleJobService** klasörü yüklemek için aşağıdaki komutu çalıştırın, **azure-iothub** cihaz SDK paketini ve **azure-iot-device-mqtt** Paket:
   
    ```
    npm install azure-iothub uuid --save
    ```

3. Bir metin düzenleyicisi kullanarak yeni bir oluşturma **scheduleJobService.js** dosyası **scheduleJobService** klasör.

4. Aşağıdaki 'gerekli' başlangıcında deyimleri Ekle **dmpatterns_gscheduleJobServiceetstarted_service.js** dosyası:
   
    ```
    'use strict';
   
    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```

5. Aşağıdaki değişken bildirimlerini ekleyin ve yer tutucu değerlerini değiştirin:
   
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

7. Cihaz yöntemi çağıran işini zamanlamak için aşağıdaki kodu ekleyin:
   
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

8. Cihaz ikizi güncelleştirilecek işini zamanlamak için aşağıdaki kodu ekleyin:
   
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

1. Komut isteminde **simDevice** klasörü için yeniden başlatma doğrudan yöntem dinleme başlamak için aşağıdaki komutu çalıştırın.
   
    ```
    node simDevice.js
    ```

2. Komut isteminde **scheduleJobService** klasörü, işlerinin kapısını kilitleme ve ikiz güncelleştirmesi tetiklemek için aşağıdaki komutu çalıştırın
   
    ```
    node scheduleJobService.js
    ```

3. Doğrudan yöntem konsolunda cihaz yanıtı görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir işi bir doğrudan yöntem bir cihaz ve cihaz ikizinin özelliklerini güncelleştirme için zamanlamak için kullanılır.

IOT Hub ve cihaz yönetim modellerini uzaktan gibi ile hava üretici yazılımı güncelleştirme başlamak için bkz: [Öğreticisi: Üretici yazılımlarını güncelleştirme nasıl](tutorial-firmware-update.md).

IOT Hub kullanmaya başlamaya devam etmek için bkz: [Azure IOT Edge'i kullanmaya başlama](../iot-edge/tutorial-simulate-device-linux.md).
