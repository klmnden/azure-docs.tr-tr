---
title: "Azure IOT Hub dosya karşıya yükleme anlama | Microsoft Docs"
description: "Geliştirici Kılavuzu - IOT hub'ı yüklemeyi yönetmek için dosya karşıya yükleme özelliği bir Azure depolama blob kapsayıcısına bir aygıttan dosyaları kullanın."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a0427925-3e40-4fcd-96c1-2a31d1ddc14b
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 75a6b9bc3ecfe6d6901bb38e312d62333f38daf1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="upload-files-with-iot-hub"></a>IOT Hub ile dosyaları karşıya yükleme

İçinde ayrıntılı olarak [IOT Hub uç noktaları] [ lnk-endpoints] makale, bir cihaz başlatabilir dosyanın karşıya bir aygıt'e yönelik uç noktası aracılığıyla bir bildirim göndererek (**/devices/ {DeviceID} / dosyaları**). Bir cihaz IOT hub'ı bir karşıya yükleme tamamlanana bildirdiğinde, IOT hub'ı aracılığıyla dosya karşıya yükleme bildirim iletisi gönderir **/messages/servicebound/filenotifications** service'e yönelik uç noktası.

Aracılığı yapmaktan IOT Hub kendisi aracılığıyla iletileri yerine, IOT hub'ı bir dağıtıcı ilişkili bir Azure depolama hesabı yerine görür. Bir aygıtın depolama belirteci cihaz karşıya istediği dosyasına özel IOT hub'dan ister. Dosya depolama alanına karşıya SAS URI'sini aygıt kullanır ve karşıya yükleme tamamlandığında cihaz IOT Hub'ına tamamlanma bir bildirim gönderir. IOT hub'ı dosya karşıya yükleme tamamlandı ve ardından hizmet dönük dosya bildirim uç noktası için bir dosya karşıya yükleme bildirim iletisi ekler denetler.

Bir CİHAZDAN IOT Hub'ına bir dosyayı karşıya yüklemeden önce hub'ınızı tarafından yapılandırmalısınız [bir Azure Storage ilişkilendirme] [ lnk-associate-storage] için hesap.

Cihazınızı böylece [karşıya yükleme başlatma] [ lnk-initialize] ve ardından [IOT hub'ı bildir] [ lnk-notify] karşıya yükleme tamamlandığında. İsteğe bağlı olarak, bir cihaz IOT hub'ı yükleme tamamlandığını bildirir, hizmet oluşturabilir bir [bildirim iletisi][lnk-service-notification].

### <a name="when-to-use"></a>Kullanılması gereken durumlar

Karşıya dosya yükleme, ortam dosyaları ve aralıklı bağlı cihazlar tarafından karşıya veya bant genişliğini korumak amacıyla Sıkıştırılmış büyük telemetri toplu göndermek için kullanın.

Başvurmak [cihaz bulut iletişimi Kılavuzu] [ lnk-d2c-guidance] IF bildirilen özellikleri, cihaz bulut iletilerini veya karşıya dosya yükleme kullanarak arasında şüpheli.

## <a name="associate-an-azure-storage-account-with-iot-hub"></a>Bir Azure Storage hesabı IOT Hub ile ilişkilendirme

Dosya karşıya yükleme işlevselliği kullanmak için öncelikle bir Azure Storage hesabı IOT Hub'ına bağlamanız gerekir. Aracılığıyla bu görevi tamamlayabilirsiniz [Azure portal][lnk-management-portal], program aracılığıyla aracılığıyla veya [IOT hub'ı kaynak sağlayıcısı REST API'leri] [ lnk-resource-provider-apis]. IOT Hub'ınıza bir Azure Storage hesabı ilişkilendirdikten sonra hizmet bir cihaza bir SAS URI'sini cihaz dosya karşıya yükleme isteği başlattığında döndürür.

> [!NOTE]
> [Azure IOT SDK'ları] [ lnk-sdks] SAS URI'sini alma, dosyayı karşıya yükleme ve tamamlanan karşıya yükleme, IOT Hub bildiren otomatik olarak işler.


## <a name="initialize-a-file-upload"></a>Bir dosyayı karşıya yükleme başlatma
IOT hub'ı bir SAS URI'sini depolama için bir dosyayı karşıya yüklemeyi istemek özellikle aygıtları için bir uç nokta vardır. Dosya karşıya yükleme işlemini başlatmak için cihaz bir POST isteği gönderir `{iot hub}.azure-devices.net/devices/{deviceId}/files` aşağıdaki JSON gövdesi ile:

```json
{
    "blobName": "{name of the file for which a SAS URI will be generated}"
}
```

IOT Hub cihaz dosyayı karşıya yüklemeyi kullanır aşağıdaki veriler döndürür:

```json
{
    "correlationId": "somecorrelationid",
    "hostName": "contoso.azure-devices.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a>Kullanım dışı: karşıya dosya yükleme GET ile başlatma

> [!NOTE]
> Bu bölümde IOT hub'dan bir SAS URI'sini alma için kullanım dışı bırakılan işlevsellik açıklanmaktadır. Daha önce açıklanan POST yöntemini kullanın.

IOT hub'ı dosya karşıya yükleme, tamamlanan karşıya yükleme IOT hub'ını bildirmek için SAS URI'sini için depolama ve diğer Al birine desteklemek için iki REST uç nokta vardır. Cihaz IOT hub'ına bir GET göndererek dosya karşıya yükleme işlemi başlatır `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`. IOT hub'ı döndürür:

* Bir SAS karşıya yüklenecek URI dosyasına özgüdür.
* Karşıya yükleme tamamlandıktan sonra kullanılması için bir bağıntı kimliği.

## <a name="notify-iot-hub-of-a-completed-file-upload"></a>IOT hub'ı bir tamamlanmış dosyanın karşıya yüklenmesi bildir

Aygıt dosyası Azure depolama SDK'ları kullanarak depolama alanına yüklemek için sorumludur. Karşıya yükleme tamamlandığında, cihaz bir POST isteği gönderir `{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` aşağıdaki JSON gövdesi ile:

```json
{
    "correlationId": "{correlation ID received from the initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

Değeri `isSuccess` dosyası başarıyla karşıya yüklendi olup olmadığını Boolean temsil eden bir değil. Durum kodunu `statusCode` depolama, dosyayı karşıya yükleme durumu ve `statusDescription` karşılık gelen `statusCode`.

## <a name="reference-topics"></a>Başvuru konuları:

Aşağıdaki başvuru konuları aygıttan dosyaları karşıya yükleme hakkında daha fazla bilgi sağlar.

## <a name="file-upload-notifications"></a>Dosya karşıya yükleme bildirimleri

Bir cihaz IOT hub'ı bir karşıya yükleme tamamlandığını bildirir, isteğe bağlı olarak, IOT hub'ı dosyasının adını ve depolama konumunu içeren bir bildirim iletisi oluşturabilir.

İçinde anlatıldığı gibi [uç noktaları][lnk-endpoints], IOT hub'ı sunan bir hizmet'e yönelik uç noktası aracılığıyla dosya karşıya yükleme bildirimleri (**/messages/servicebound/fileuploadnotifications**) iletileri olarak. Dosya karşıya yükleme bildirimlerini alma anlamları bulut-cihaz iletilerini aynıdır ve aynı olan [ileti yaşam döngüsü][lnk-lifecycle]. Dosya karşıya yükleme bildirim uç noktasından alınan her ileti, aşağıdaki özelliklere sahip bir JSON kaydıdır:

| Özellik | Açıklama |
| --- | --- |
| EnqueuedTimeUtc |Bildirim ne zaman oluşturulduğunu belirten bir zaman damgası. |
| Cihaz kimliği |**DeviceID** olan dosya karşıya cihaz. |
| BlobUri |Yüklenen dosya URI'si. |
| BlobName |Karşıya yüklenen dosyanın adı. |
| LastUpdatedTime |Dosyanın en son güncelleştirildiği belirten bir zaman damgası. |
| BlobSizeInBytes |Karşıya yüklenen dosyanın boyutu. |

**Örnek**. Bu örnek, bir dosya gövdesi karşıya bildirim iletisi gösterir.

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

Her IOT hub'ı dosya karşıya yükleme bildirimleri için aşağıdaki yapılandırma seçeneklerini gösterir:

| Özellik | Açıklama | Aralık ve varsayılan |
| --- | --- | --- |
| **enableFileUploadNotifications** |Dosya karşıya yükleme bildirimlerini dosya bildirimleri uç noktası için yazılmış olup olmadığını denetler. |Bool. Varsayılan: True. |
| **fileNotifications.ttlAsIso8601** |Dosya karşıya yükleme bildirimleri için varsayılan TTL değeri. |ISO_8601 aralığı ' 48 H kadar (en az 1 dakika). Varsayılan: 1 saat. |
| **fileNotifications.lockDuration** |Dosya karşıya yükleme bildirimleri sırası süresince kilitleyin. |5 ile 300 saniye (en az 5 saniye). Varsayılan: 60 saniye sayısı. |
| **fileNotifications.maxDeliveryCount** |Dosya için maksimum teslimat sayısı bildirim sırası karşıya yükleyin. |1 ile 100. Varsayılan: 100. |

## <a name="additional-reference-material"></a>Ek başvuru bilgileri

IOT Hub Geliştirici Kılavuzu'ndaki diğer başvuru konuları içerir:

* [IOT Hub uç noktaları] [ lnk-endpoints] her IOT hub'ı çalışma zamanı ve yönetim işlemleri için kullanıma sunan çeşitli uç noktaları açıklar.
* [Azaltma ve kotaları] [ lnk-quotas] kotaları açıklar ve IOT hub'ı hizmete uygulamak davranışları azaltma.
* [Azure IOT cihaz ve hizmet SDK'ları] [ lnk-sdks] çeşitli dil IOT Hub ile etkileşim hem cihaz hem de hizmet uygulamaları geliştirirken kullanabilir SDK'ları listeler.
* [IOT hub'ı sorgu dili] [ lnk-query] IOT Hub'ından, cihaz çiftlerini ve işleri hakkında bilgi almak için kullanabileceğiniz sorgu dili açıklar.
* [IOT Hub MQTT Destek] [ lnk-devguide-mqtt] IOT hub'ı desteği hakkında daha fazla bilgi için MQTT Protokolü sağlar.

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ı kullanarak cihazları dosyaları karşıya yüklemek öğrendiniz artık aşağıdaki IOT Hub Geliştirici Kılavuzu konuları ilgilenen olabilir:

* [IOT Hub cihaz kimliklerini yönetme][lnk-devguide-identities]
* [IOT Hub'ına erişimi denetleme][lnk-devguide-security]
* [Cihaz çiftlerini durumu ve yapılandırmaları eşitlemek için kullanın][lnk-devguide-device-twins]
* [Bir cihazda doğrudan bir yöntem çağırma][lnk-devguide-directmethods]
* [Birden çok aygıta işleri zamanla][lnk-devguide-jobs]

Bu makalede açıklanan kavramları bazıları denemek istiyorsanız, aşağıdaki IOT hub'ı öğreticide ilgilenen olabilir:

* [IOT Hub ile bulut aygıtlardan dosyaları karşıya yükleme yapma][lnk-fileupload-tutorial]

[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-management-portal]: https://portal.azure.com
[lnk-fileupload-tutorial]: iot-hub-csharp-csharp-file-upload.md
[lnk-associate-storage]: iot-hub-devguide-file-upload.md#associate-an-azure-storage-account-with-iot-hub
[lnk-initialize]: iot-hub-devguide-file-upload.md#initialize-a-file-upload
[lnk-notify]: iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload
[lnk-service-notification]: iot-hub-devguide-file-upload.md#file-upload-notifications
[lnk-lifecycle]: iot-hub-devguide-messages-c2d.md#the-cloud-to-device-message-lifecycle
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
