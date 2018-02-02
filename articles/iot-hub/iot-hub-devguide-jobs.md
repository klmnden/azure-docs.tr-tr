---
title: "Azure IOT Hub işleri anlama | Microsoft Docs"
description: "Geliştirici Kılavuzu - birden çok cihaz üzerinde çalışmasına işlerini zamanlama IOT hub'ına bağlı. İşler, etiketler ve istenen özelliklerini güncelleştirmek ve birden çok aygıta doğrudan yöntemleri çağırma."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: fe78458f-4f14-4358-ac83-4f7bd14ee8da
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: 7e0af40b2fd5bbb12d5565765aae4026922aec5c
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="schedule-jobs-on-multiple-devices"></a>Birden fazla cihazda işleri zamanlama

Azure IOT hub'ı etkinleştirir yapı taşları gibi çeşitli [cihaz çifti özellikleri ve etiketleri] [ lnk-twin-devguide] ve [doğrudan yöntemleri][lnk-dev-methods].  Genellikle, arka uç uygulamaları cihaz yöneticileri ve işleçleri güncelleştirmek ve IOT cihazları toplu ve zamanlanan saatte etkileşim etkinleştirir.  İşler, zamanlanmış bir saatte cihaz çifti güncelleştirmeleri ve bir cihaz kümesi karşı doğrudan yöntemleri yürütün.  Örneğin, bir işleç başlatır ve bir grup oluşturma işlemlerini kesintiye uğratan olmayacaktır zaman 43 ve 3 kat oluşturmanın cihazı yeniden başlatmak için bir iş izleyen bir arka uç uygulaması kullanırsınız.

Zamanlamak ve ilerleme durumunu izlemek gerektiğinde işleri şu etkinlikler herhangi bir cihaz kümesi üzerinde kullanmayı dikkate alın:

* İstenen özellikleri güncelleştirme
* Güncelleştirme etiketleri
* Doğrudan yöntemleri çağırma

## <a name="job-lifecycle"></a>İş yaşam döngüsü
İşlerini çözüm arka ucu tarafından başlatılan ve IOT Hub tarafından korunur.  Bir hizmet dönük URI ile bir işi başlatabilirsiniz (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-11-14`) ve bir hizmet dönük URI aracılığıyla yürütülen bir işin ilerleme için sorgu (`{iot hub}/jobs/v2/<jobId>?api-version=2016-11-14`). Bir iş başlatıldıktan işlerin durumunu yenilemek için bir iş sorgu çalıştırın.

> [!NOTE]
> Bir işi başlattığınızda özellik adları ve değerleri yalnızca US-ASCII yazdırılabilir içerebilir herhangi aşağıdaki kümesindeki dışında alfasayısal: `$ ( ) < > @ , ; : \ " / [ ] ? = { } SP HT`.

## <a name="jobs-to-execute-direct-methods"></a>İşlerini doğrudan bir yöntem yürütülemez
Aşağıdaki kod parçacığını yürütmek HTTPS 1.1 isteği ayrıntıları gösteren bir [doğrudan yöntemi] [ lnk-dev-methods] bir işi kullanarak cihazları bir dizi:

    PUT /jobs/v2/<jobId>?api-version=2016-11-14

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            responseTimeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // query condition
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }

Sorgu koşulu de tek bir cihaz kimliği veya aygıt aşağıdaki örneklerde gösterildiği gibi kimlikleri listesini olabilir:

```
queryCondition = "deviceId = 'MyDevice1'"
queryCondition = "deviceId IN ['MyDevice1','MyDevice2']"
queryCondition = "deviceId IN ['MyDevice1']
```
[IOT Hub sorgu dili] [ lnk-query] IOT hub'ı sorgu dili ek ayrıntılı kapsar.

## <a name="jobs-to-update-device-twin-properties"></a>Cihaz çifti özelliklerini güncelleştirmek için işlemler
Aşağıdaki kod parçacığında, bir iş kullanarak cihaz çifti özelliklerini güncelleştirmek için HTTPS 1.1 istek ayrıntılarını gösterir:

    PUT /jobs/v2/<jobId>?api-version=2016-11-14
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // query condition
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }

## <a name="querying-for-progress-on-jobs"></a>İşlerini ilerleme sorgulama
Aşağıdaki kod parçacığında HTTPS 1.1 istek ayrıntılarını gösterir [işleri sorgulama][lnk-query]:

    GET /jobs/v2/query?api-version=2016-11-14[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

ContinuationToken yanıttan sağlanır.  

## <a name="jobs-properties"></a>İşlerini özellikleri
Aşağıdaki listede, özelliklerini ve sorgulanırken işleri veya iş sonuçları için kullanılabilir karşılık gelen açıklamaları gösterir.

| Özellik | Açıklama |
| --- | --- |
| **jobId** |Uygulama Kimliği iş için sağlanan. |
| **startTime** |Uygulama için iş başlangıç zamanı (ISO 8601) sağlanan. |
| **endTime** |IOT hub'ı zaman iş tamamlandı (ISO 8601) tarihi sağlanır. Yalnızca iş 'Tamamlandı' durumuna ulaştıktan sonra geçerli. |
| **türü** |İşlerini türleri: |
| | **scheduledUpdateTwin**: İstenen özellikleri veya etiketleri kümesi güncelleştirmek için kullanılan bir işi. |
| | **scheduledDeviceMethod**: cihaz çiftlerini kümesi üzerinde bir aygıt yöntemi çağırmak için kullanılan bir işi. |
| **durumu** |İşin geçerli durumu. Durum için olası değerler: |
| | **Bekleyen**: Zamanlanmış ve iş hizmeti tarafından çekilmesi bekleniyor. |
| | **Zamanlanmış**: gelecekteki bir zamanı için zamanlanan. |
| | **çalışan**: şu anda etkin iş. |
| | **İptal**: işi iptal edildi. |
| | **başarısız**: işi başarısız oldu. |
| | **Tamamlanan**: işi tamamlandı. |
| **deviceJobStatistics** |İş yürütme hakkındaki istatistiklerdir. |
| | **deviceJobStatistics** özellikleri: |
| | **deviceJobStatistics.deviceCount**: cihazları iş sayısı. |
| | **deviceJobStatistics.failedCount**: iş başarısız olduğu cihaz sayısı. |
| | **deviceJobStatistics.succeededCount**: Burada iş başarılı cihaz sayısı. |
| | **deviceJobStatistics.runningCount**: işi çalışmakta olan aygıt sayısı. |
| | **deviceJobStatistics.pendingCount**: işi çalıştırmak için beklemede olan aygıt sayısı. |

### <a name="additional-reference-material"></a>Ek başvuru bilgileri
IOT Hub Geliştirici Kılavuzu'ndaki diğer başvuru konuları içerir:

* [IOT Hub uç noktaları] [ lnk-endpoints] her IOT hub'ı çalışma zamanı ve yönetim işlemleri için kullanıma sunan çeşitli uç noktaları açıklar.
* [Azaltma ve kotaları] [ lnk-quotas] IOT Hub hizmeti ve azaltma davranışı hizmetini kullandığınızda beklediğiniz uygulama kotaları açıklar.
* [Azure IOT cihaz ve hizmet SDK'ları] [ lnk-sdks] çeşitli dil IOT Hub ile etkileşim hem cihaz hem de hizmet uygulamaları geliştirirken kullanabilir SDK'ları listeler.
* [IOT Hub cihaz çiftlerini, işler ve ileti yönlendirme için sorgu dili] [ lnk-query] IOT hub'ı sorgu dili açıklar. Bu sorgu dili, IOT Hub'ından, cihaz çiftlerini ve işleri hakkında bilgi almak için kullanın.
* [IOT Hub MQTT Destek] [ lnk-devguide-mqtt] IOT hub'ı desteği hakkında daha fazla bilgi için MQTT Protokolü sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede açıklanan kavramları bazıları denemek için aşağıdaki IOT hub'ı öğretici bakın:

* [Zamanlama ve yayın işleri][lnk-jobs-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-node-node-schedule-jobs.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md
