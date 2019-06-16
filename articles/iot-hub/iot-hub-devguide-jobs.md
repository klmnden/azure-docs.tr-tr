---
title: Azure IOT hub'ı işleri anlama | Microsoft Docs
description: Geliştirici Kılavuzu - birden fazla cihazda çalıştırılacak işleri zamanlama, IOT hub'ınıza bağlı. İşler, etiketler ve istenen özelliklerini güncelleştirmek ve birden fazla cihazda doğrudan metotları çağırma.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 147dd0f454bd85673bcba5cd6148c5da9716c580
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65409045"
---
# <a name="schedule-jobs-on-multiple-devices"></a>Birden fazla cihazda işleri zamanlama

Azure IOT hub'ı etkinleştirir yapı taşları gibi bir dizi [cihaz ikizi özelliklerini ve etiketlerini](iot-hub-devguide-device-twins.md) ve [doğrudan yöntemler](iot-hub-devguide-direct-methods.md). Genellikle, arka uç uygulamaları güncelleştirme ve IOT cihazlarını toplu ve zamanlanan tarihte ile etkileşim kurmak cihaz yöneticilerin ve operatörlerin olanak tanır. İşleri zamanlanan saatte cihaz ikizi güncelleştirmeleri ve bir dizi cihazda karşı doğrudan metotları yürütme. Örneğin, operatör başlatır ve bir grup için yapı işlemlerini kesintiye uğratan olmazdı birer 43 ve 3 kat oluşturmadaki cihazı yeniden başlatmak için bir iş izleyen bir arka uç uygulaması kullanmanız gerekir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Planlamak ve ilerlemeyi izlemek gerektiğinde aşağıdaki etkinliklerin herhangi bir dizi cihazda işlemleriyle göz önünde bulundurun:

* İstenen özellikleri güncelleştirme
* Etiketleri güncelleştirin
* Doğrudan metotları çağırma

## <a name="job-lifecycle"></a>İş yaşam döngüsü

İşleri çözüm arka ucu tarafından başlatılan ve IOT Hub tarafından korunur. Hizmet kullanıma yönelik bir URI aracılığıyla bir işi başlatabilirsiniz (`PUT https://<iot hub>/jobs/v2/<jobID>?api-version=2018-06-30`) ve hizmeti kullanıma yönelik bir URI aracılığıyla yürütülürken bir işin ilerlemesi için sorgu (`GET https://<iot hub>/jobs/v2/<jobID?api-version=2018-06-30`). Bir iş başlatıldıktan sonra işleri çalıştırma durumunu yenilemek için bir iş sorgusu çalıştırın.

> [!NOTE]
> Bir işi başlattığınızda, özellik adları ve değerleri yalnızca US-ASCII yazdırılabilir içerebilir alfasayısal karakterler, aşağıdaki, tüm hariç: `$ ( ) < > @ , ; : \ " / [ ] ? = { } SP HT`

## <a name="jobs-to-execute-direct-methods"></a>Doğrudan yöntemler çalıştırılacak işleri

Aşağıdaki kod parçacığını yürütmeye yönelik HTTPS 1.1 istek ayrıntılarını gösterir bir [doğrudan yöntemini](iot-hub-devguide-direct-methods.md) bir iş kullanarak cihazları bir dizi:

```
PUT /jobs/v2/<jobId>?api-version=2018-06-30

Authorization: <config.sharedAccessSignature>
Content-Type: application/json; charset=utf-8

{
    "jobId": "<jobId>",
    "type": "scheduleDeviceMethod",
    "cloudToDeviceMethod": {
        "methodName": "<methodName>",
        "payload": <payload>,
        "responseTimeoutInSeconds": methodTimeoutInSeconds
    },
    "queryCondition": "<queryOrDevices>", // query condition
    "startTime": <jobStartTime>,          // as an ISO-8601 date string
    "maxExecutionTimeInSeconds": <maxExecutionTimeInSeconds>
}
```

Sorgu koşulu, bir tek bir cihaz kimliği veya cihaz aşağıdaki örneklerde gösterildiği gibi kimlikleri listesi de olabilir:

```
"queryCondition" = "deviceId = 'MyDevice1'"
"queryCondition" = "deviceId IN ['MyDevice1','MyDevice2']"
"queryCondition" = "deviceId IN ['MyDevice1']"
```

[IOT Hub sorgu dili](iot-hub-devguide-query-language.md) ek ayrıntılı IOT Hub sorgu dili kapsar.

Aşağıdaki kod parçacığı, istek ve yanıt testMethod tüm cihazlarda contoso-hub-1'adlı bir doğrudan yöntem çağırma için zamanlanan bir işin gösterir:

```
PUT https://contoso-hub-1.azure-devices.net/jobs/v2/job01?api-version=2018-06-30 HTTP/1.1
Authorization: SharedAccessSignature sr=contoso-hub-1.azure-devices.net&sig=68iv------------------------------------v8Hxalg%3D&se=1556849884&skn=iothubowner
Content-Type: application/json; charset=utf-8
Host: contoso-hub-1.azure-devices.net
Content-Length: 317

{
    "jobId": "job01",
    "type": "scheduleDeviceMethod",
    "cloudToDeviceMethod": {
        "methodName": "testMethod",
        "payload": {},
        "responseTimeoutInSeconds": 30
    },
    "queryCondition": "*", 
    "startTime": "2019-05-04T15:53:00.077Z",
    "maxExecutionTimeInSeconds": 20
}

HTTP/1.1 200 OK
Content-Length: 65
Content-Type: application/json; charset=utf-8
Vary: Origin
Server: Microsoft-HTTPAPI/2.0
Date: Fri, 03 May 2019 01:46:18 GMT

{"jobId":"job01","type":"scheduleDeviceMethod","status":"queued"}
```

## <a name="jobs-to-update-device-twin-properties"></a>Cihaz ikizi özelliklerini güncelleştirmek için işlemler

Aşağıdaki kod parçacığında, bir iş kullanarak cihaz ikizi özelliklerini güncelleştirmek için HTTPS 1.1 istek ayrıntılarını gösterir:

```
PUT /jobs/v2/<jobId>?api-version=2018-06-30

Authorization: <config.sharedAccessSignature>
Content-Type: application/json; charset=utf-8

{
    "jobId": "<jobId>",
    "type": "scheduleUpdateTwin",
    "updateTwin": <patch>                 // Valid JSON object
    "queryCondition": "<queryOrDevices>", // query condition
    "startTime": <jobStartTime>,          // as an ISO-8601 date string
    "maxExecutionTimeInSeconds": <maxExecutionTimeInSeconds>
}
```

> [!NOTE]
> *UpdateTwin* özellik için geçerli etag eşleşme gerekir; Örneğin, `etag="*"`.

Aşağıdaki kod parçacığı, istek ve yanıt için test-contoso-hub-1 üzerinde'cihaz için cihaz ikizi özelliklerini güncelleştirmek için zamanlanmış bir işi gösterir:

```
PUT https://contoso-hub-1.azure-devices.net/jobs/v2/job02?api-version=2018-06-30 HTTP/1.1
Authorization: SharedAccessSignature sr=contoso-hub-1.azure-devices.net&sig=BN0U-------------------------------------RuA%3D&se=1556925787&skn=iothubowner
Content-Type: application/json; charset=utf-8
Host: contoso-hub-1.azure-devices.net
Content-Length: 339

{
    "jobId": "job02",
    "type": "scheduleUpdateTwin",
    "updateTwin": {
      "properties": {
        "desired": {
          "test1": "value1"
        }
      },
     "etag": "*"
     },
    "queryCondition": "deviceId = 'test-device'",
    "startTime": "2019-05-08T12:19:56.868Z",
    "maxExecutionTimeInSeconds": 20
}

HTTP/1.1 200 OK
Content-Length: 63
Content-Type: application/json; charset=utf-8
Vary: Origin
Server: Microsoft-HTTPAPI/2.0
Date: Fri, 03 May 2019 22:45:13 GMT

{"jobId":"job02","type":"scheduleUpdateTwin","status":"queued"}
```

## <a name="querying-for-progress-on-jobs"></a>Devam eden işler üzerinde sorgulama

Aşağıdaki kod parçacığı için işleri sorgulamak için HTTPS 1.1 istek ayrıntılarını gösterir:

```
GET /jobs/v2/query?api-version=2018-06-30[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]

Authorization: <config.sharedAccessSignature>
Content-Type: application/json; charset=utf-8
```

Yanıttan continuationToken sağlanır.

Her bir cihaz kullanarak iş yürütme durumunu sorgulayabilirsiniz [cihaz ikizleri, işler ve ileti yönlendirme için IOT Hub sorgu dili](iot-hub-devguide-query-language.md).

## <a name="jobs-properties"></a>İş özellikleri

Aşağıdaki liste, sorgulanırken işleri veya iş sonuçları için kullanılabilir ilgili açıklamalar ve özelliklerini gösterir.

| Özellik | Açıklama |
| --- | --- |
| **jobId** |Uygulama için İş Kimliği sağlanmadı. |
| **startTime** |Uygulama, işin başlangıç saati (ISO 8601) sağlanır. |
| **endTime** |IOT Hub, iş tamamlandığında (ISO 8601) tarihi sağlanır. Yalnızca iş 'Tamamlandı' durumuna ulaştıktan sonra geçerli. |
| **type** |İşleri türleri: |
| | **scheduleUpdateTwin**: Bir dizi istenen özellikleri veya etiketleri güncelleştirmek için kullanılan bir proje. |
| | **scheduleDeviceMethod**: Cihaz ikizlerini kümesi üzerinde bir cihaz yöntemini çağırmak için kullanılan bir proje. |
| **status** |İşin geçerli durumu. Durum için olası değerler: |
| | **Bekleyen**: Zamanlanmış ve iş hizmeti tarafından işlenmek üzere bekleniyor. |
| | **Zamanlanmış**: Gelecekteki bir zamanı için zamanlandı. |
| | **Çalışan**: Şu anda etkin iş. |
| | **İptal**: İş iptal edildi. |
| | **Başarısız**: İş başarısız oldu. |
| | **Tamamlanan**: İşi tamamlandı. |
| **deviceJobStatistics** |İşin yürütme hakkındaki istatistiklerdir. |
| | **deviceJobStatistics** özellikleri: |
| | **deviceJobStatistics.deviceCount**: İşteki cihaz sayısı. |
| | **deviceJobStatistics.failedCount**: İş başarısız olduğu cihaz sayısı. |
| | **deviceJobStatistics.succeededCount**: İş yeri başarılı cihazların sayısı. |
| | **deviceJobStatistics.runningCount**: İşi çalışmakta olan cihazların sayısı. |
| | **deviceJobStatistics.pendingCount**: İşi çalıştırmak için bekleyen cihaz sayısı. |

### <a name="additional-reference-material"></a>Ek başvuru malzemesi

IOT Hub Geliştirici Kılavuzu'nda olan diğer başvuru konularını içerir:

* [IOT Hub uç noktaları](iot-hub-devguide-endpoints.md) her IOT hub'ı ortaya koyan çalışma zamanı ve yönetim işlemleri için çeşitli uç noktaları açıklar.

* [Azaltma ve kotalar](iot-hub-devguide-quotas-throttling.md) IOT Hub hizmeti ve hizmetin kullandığınızda beklenir azaltma davranışını uygulanan kotalar açıklar.

* [Azure IOT cihaz ve hizmet SDK'ları](iot-hub-devguide-sdks.md) çeşitli dil IOT hub'ı ile etkileşim kuran hem cihaz hem de hizmet uygulamaları geliştirirken kullanabileceğiniz SDK'ları listeler.

* [Cihaz ikizleri, işler ve ileti yönlendirme için IOT Hub sorgu dili](iot-hub-devguide-query-language.md) IOT Hub sorgu dili açıklar. IOT Hub'ından, cihaz ikizleri ve işler hakkında bilgi almak için bu sorgu dili kullanın.

* [IOT hub'ı MQTT desteği](iot-hub-mqtt-support.md) ve MQTT protokolünü için IOT hub'ı desteği hakkında daha fazla bilgi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede açıklanan kavramları bazıları denemek için aşağıdaki IOT hub'ı öğreticiye bakın:

* [İşleri zamanlama ve yayınlama](iot-hub-node-node-schedule-jobs.md)