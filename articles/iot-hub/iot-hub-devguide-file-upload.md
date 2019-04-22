---
title: Azure IOT hub'ı dosya karşıya yükleme anlama | Microsoft Docs
description: Geliştirici Kılavuzu - karşıya yükleme yönetmek için IOT Hub'ın karşıya dosya yükleme özelliğiyle bir CİHAZDAN bir Azure depolama blob kapsayıcısına dosyaları kullanın.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 11/07/2018
ms.openlocfilehash: 217d348eacab30b90e06fe805d9cdb0cf32349ac
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59050739"
---
# <a name="upload-files-with-iot-hub"></a>IOT Hub ile dosyaları karşıya yükleme

Ayrıntılarıyla açıklandığı gibi [IOT Hub uç noktaları](iot-hub-devguide-endpoints.md) makale, bir cihaz başlayabileceğini dosyanın karşıya bir cihaz'e yönelik uç noktası bir bildirim göndererek (**/devices/ {DeviceID} / dosyaları**). Bir cihaz IOT hub'ı bir karşıya yükleme tamamlandıktan bildirdiğinde, IOT Hub aracılığıyla bir dosyayı karşıya yükleme bildirim iletisi gönderir. **/messages/servicebound/filenotifications** yönelik hizmet uç noktası.

Aracılığını destekleyen kendini IOT Hub aracılığıyla iletileri yerine, IOT hub'ı bir dağıtıcısıyla ilişkili Azure depolama hesabınız yerine görür. Bir cihaz IOT hub'dan cihaz yüklemek isteyen dosyanın belirli bir depolama belirteci ister. Dosya depolama alanına yüklemek için SAS URI'sini cihaz kullanır ve karşıya yükleme tamamlandığında, cihaz bildirim tamamlama IOT Hub'ına gönderir. IOT Hub, dosyayı karşıya yükleme tamamlandı ve ardından hizmeti kullanıma yönelik dosya bildirim uç noktası için bir dosya karşıya yükleme bildirim iletisi ekler denetler.

Bir CİHAZDAN IOT Hub'ına bir dosyayı karşıya yüklemeden önce hub'ınıza tarafından yapılandırmalısınız [bir Azure depolama ilişkilendirme](iot-hub-devguide-file-upload.md#associate-an-azure-storage-account-with-iot-hub) için hesap.

Cihazınızı böylece [karşıya yükleme başlatmak](iot-hub-devguide-file-upload.md#initialize-a-file-upload) ardından [IOT hub'ı bildirim](iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload) karşıya yükleme tamamlandığında. İsteğe bağlı olarak, bir cihaz IOT hub'ı karşıya yükleme tamamlandıktan bildirdiğinde, hizmeti oluşturabilir bir [bildirim iletisi](iot-hub-devguide-file-upload.md#file-upload-notifications).

### <a name="when-to-use"></a>Kullanılması gereken durumlar

Karşıya dosya yükleme, medya dosyaları ve aralıklı olarak bağlanan cihazlarla tarafından karşıya veya bant genişliğinden tasarruf etmek sıkıştırılmış toplu işlemleri büyük telemetri göndermek için kullanın.

Başvurmak [CİHAZDAN buluta iletişim Kılavuzu](iot-hub-devguide-d2c-guidance.md) bildirilen özellikleri, CİHAZDAN buluta iletileri ya da Karşıya Dosya Yükleme'nın kullanımı arasındaki şüpheli ise.

## <a name="associate-an-azure-storage-account-with-iot-hub"></a>IOT hub'ı ile bir Azure depolama hesabı ilişkilendirin

Dosya karşıya yükleme işlevselliği kullanmak için bir Azure depolama hesabı IOT Hub'ına bağlamanız gerekir. Azure portalından veya programlama yoluyla aracılığıyla bu görevi tamamlayabilirsiniz [IOT hub'ı kaynak sağlayıcısı REST API'leri](/rest/api/iothub/iothubresource). Bir Azure depolama hesabı ile IOT Hub'ınıza ilişkilendirdiğiniz sonra hizmet cihaz bir dosya karşıya yükleme isteği başlatıldığında bir cihaza bir SAS URI döndürür.

[Dosya cihazınızdan IOT Hub ile buluta yükleme](iot-hub-csharp-csharp-file-upload.md) izlenecek tam yol dosya karşıya yükleme işleminin nasıl yapılır kılavuzları sağlar. Bu nasıl yapılır kılavuzlarından Azure portalında bir depolama hesabı, bir IOT hub ile ilişkilendirmek için nasıl kullanılacağını gösterir.

> [!NOTE]
> [Azure IOT SDK'ları](iot-hub-devguide-sdks.md) SAS URI'sini alma, dosyayı karşıya yükledikten ve tamamlanan bir karşıya yükleme, IOT hub'bildiren otomatik olarak işler.

## <a name="initialize-a-file-upload"></a>Karşıya dosya yüklemeyi Başlat
IOT hub'ı bir dosyayı karşıya yüklemek için bir SAS URI depolama istemek özellikle cihazlar için bir uç nokta vardır. Dosya karşıya yükleme işlemini başlatmak için cihaz bir POST isteği gönderir `{iot hub}.azure-devices.net/devices/{deviceId}/files` aşağıdaki JSON gövdesi ile:

```json
{
    "blobName": "{name of the file for which a SAS URI will be generated}"
}
```

IOT Hub cihaz dosyayı karşıya yüklemek için kullandığı aşağıdaki veriler döndürür:

```json
{
    "correlationId": "somecorrelationid",
    "hostName": "yourstorageaccount.blob.core.windows.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a>Kullanım dışı: Dosyanın karşıya bir GET ile başlatılamıyor

> [!NOTE]
> Bu bölümde, IOT Hub'ından SAS URI'sini alma için kullanım dışı işlevler açıklanmaktadır. Daha önce açıklanan POST yöntemini kullanın.

IOT hub'ı dosya karşıya yükleme, tamamlanan bir karşıya yükleme, IOT hub'ı bildirmek için SAS URI'si için depolama ve diğer kazanmak için desteklemek için iki REST uç noktası vardır. Cihaz IOT hub'da bir GET göndererek dosya karşıya yükleme işlemi başlar `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`. IOT hub'ı döndürür:

* SAS karşıya yüklenecek URI'si dosyasına özgüdür.

* Karşıya yükleme tamamlandıktan sonra kullanılacak bir bağıntı kimliği.

## <a name="notify-iot-hub-of-a-completed-file-upload"></a>Tamamlanmış bir dosyayı karşıya yükleme, IOT hub'bildir

Cihaz Azure depolama SDK'larını kullanarak depolama alanına yüklenir. Karşıya yükleme tamamlandığında, cihaz bir POST isteği gönderir. `{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` aşağıdaki JSON gövdesi ile:

```json
{
    "correlationId": "{correlation ID received from the initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

Değerini `isSuccess` dosyası başarıyla karşıya yüklendi olup olmadığını gösteren bir Boole değeri. Durum kodunu `statusCode` depolama, dosyanın karşıya yükleme durumu ve `statusDescription` karşılık gelen `statusCode`.

## <a name="reference-topics"></a>Başvuru konuları:

Aşağıdaki başvuru konuları, dosyaları bir CİHAZDAN karşıya yükleme hakkında daha fazla bilgi sağlar.

## <a name="file-upload-notifications"></a>Dosya karşıya yükleme bildirimleri

İsteğe bağlı olarak, bir cihaz IOT hub'ı bir karşıya yükleme tamamlandıktan bildirdiğinde, IOT hub'ı bir bildirim iletisi oluşturur. Bu ileti dosyasının adını ve depolama konumunu içerir.

İçinde anlatıldığı gibi [uç noktaları](iot-hub-devguide-endpoints.md), IOT Hub hizmeti'e yönelik uç nokta üzerinden dosya karşıya yükleme bildirimleri sunar (**/messages/servicebound/fileuploadnotifications**) iletileri. Dosya karşıya yükleme bildirimleri alma semantiği bulut-cihaz iletilerini ile aynıdır ve aynı [ileti yaşam döngüsü](iot-hub-devguide-messages-c2d.md#the-cloud-to-device-message-lifecycle). Dosya karşıya yükleme bildirim uç noktasından alınan her ileti, aşağıdaki özelliklere sahip bir JSON kaydıdır:

| Özellik | Açıklama |
| --- | --- |
| EnqueuedTimeUtc |Bildirimin ne zaman oluşturulduğunu belirten bir zaman damgası. |
| DeviceId |**DeviceID** dosya karşıya cihazın. |
| BlobUri |Karşıya yüklenen dosya URI'si. |
| BlobName |Karşıya yüklenen dosya adı. |
| LastUpdatedTime |Dosyanın en son ne zaman güncelleştirildiğini belirten bir zaman damgası. |
| BlobSizeInBytes |Karşıya yüklenen dosyanın boyutu. |

**Örnek**. Bu örnek, bir dosya gövdesinin karşıya yükleme, bildirim iletisini gösterir.

```json
{
    "deviceId":"mydevice",
    "blobUri":"https://{storage account}.blob.core.windows.net/{container name}/mydevice/myfile.jpg",
    "blobName":"mydevice/myfile.jpg",
    "lastUpdatedTime":"2016-06-01T21:22:41+00:00",
    "blobSizeInBytes":1234,
    "enqueuedTimeUtc":"2016-06-01T21:22:43.7996883Z"
}
```

## <a name="file-upload-notification-configuration-options"></a>Dosya karşıya yükleme bildirim yapılandırma seçenekleri

Her IOT hub aşağıdaki yapılandırma seçeneklerinin bildirimleri karşıya dosya vardır:

| Özellik | Açıklama | Aralığı ve varsayılan |
| --- | --- | --- |
| **enableFileUploadNotifications** |Dosya karşıya yükleme bildirim dosyası bildirimleri uç noktaya yazılıp yazılmayacağını denetler. |Bool. Varsayılan: TRUE. |
| **fileNotifications.ttlAsIso8601** |Varsayılan TTL dosya karşıya yükleme bildirimleri. |ISO_8601 aralığı ' 48 saat kadar (en az 1 dakika). Varsayılan: 1 saat. |
| **fileNotifications.lockDuration** |Kilit süresi için dosya karşıya yükleme bildirim sırası. |5 ile 300 saniye (en az 5 saniye). Varsayılan: 60 saniye. |
| **fileNotifications.maxDeliveryCount** |En fazla teslim sayısı bildirim sırası karşıya yükleyin. |1-100. Varsayılan: 100. |

## <a name="additional-reference-material"></a>Ek başvuru malzemesi

IOT Hub Geliştirici Kılavuzu'nda olan diğer başvuru konularını içerir:

* [IOT Hub uç noktaları](iot-hub-devguide-endpoints.md) çalışma zamanı ve yönetim işlemleri için çeşitli IOT hub uç noktaları açıklar.

* [Azaltma ve kotalar](iot-hub-devguide-quotas-throttling.md) kotaları açıklar ve IOT Hub hizmetine geçerli davranışlara azaltma.

* [Azure IOT cihaz ve hizmet SDK'ları](iot-hub-devguide-sdks.md) çeşitli dil IOT hub'ı ile etkileşim kuran hem cihaz hem de hizmet uygulamaları geliştirirken kullanabileceğiniz SDK'ları listeler.

* [IOT Hub sorgu dili](iot-hub-devguide-query-language.md) IOT Hub'ından, cihaz ikizleri ve işler hakkında bilgi almak için kullanabileceğiniz bir sorgu dili açıklar.

* [IOT hub'ı MQTT desteği](iot-hub-mqtt-support.md) ve MQTT protokolünü için IOT hub'ı desteği hakkında daha fazla bilgi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ı kullanarak cihazlardan karşıya dosya yükleme öğrendiniz artık aşağıdaki IOT Hub Geliştirici Kılavuzu konuları ilginizi çekebilir:

* [IOT hub cihaz kimliklerini yönetme](iot-hub-devguide-identity-registry.md)

* [IoT Hub’a erişimi denetleme](iot-hub-devguide-security.md)

* [Durum ve yapılandırmaları eşitlemek için cihaz ikizlerini kullanma](iot-hub-devguide-device-twins.md)

* [Bir cihazda doğrudan yöntem çağırma](iot-hub-devguide-direct-methods.md)

* [Birden fazla cihazda işleri zamanlama](iot-hub-devguide-jobs.md)

Bu makalede açıklanan kavramları bazıları denemek için aşağıdaki IOT hub'ı öğreticiye bakın:

* [IOT Hub ile buluta cihazlardan dosya karşıya yükleme](iot-hub-csharp-csharp-file-upload.md)