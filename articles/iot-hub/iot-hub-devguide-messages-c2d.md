---
title: Azure IOT Hub, bulut-cihaz Mesajlaşma anlama | Microsoft Docs
description: Geliştirici Kılavuzu - bulut-cihaz IOT Hub ile ileti kullanma. İleti yaşam döngüsü ve yapılandırma seçenekleri hakkında bilgi içerir.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/15/2018
ms.openlocfilehash: b0c1b877a9468ce9c3b851bce62cb87c64c04260
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65472730"
---
# <a name="send-cloud-to-device-messages-from-iot-hub"></a>IoT Hub’dan buluttan cihaza iletileri gönderme

Cihaz uygulamasına, çözüm arka ucu tek yönlü bildirimleri göndermek için Cihazınızı IOT hub'ından bulut cihazları iletileri gönderir. IOT Hub tarafından desteklenen diğer bulut-cihaz seçenekleri için bkz [bulut buluttan cihaza iletişim Kılavuzu](iot-hub-devguide-c2d-guidance.md).

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Yönelik hizmet uç noktası aracılığıyla bulut buluttan cihaza iletileri gönderme (**/iletileri/devicebound**). Bir cihaz, ardından cihaza özel uç noktası aracılığıyla iletileri alan (**/devices/ {DeviceID} / iletileri/devicebound**).

Bulut-cihaz her iletinin tek bir cihaz hedeflemek için IOT hub'ı ayarlar **için** özelliğini **/devices/ {DeviceID} / iletileri/devicebound**.

Her bir cihaz kuyruk en fazla 50 bulut-cihaz iletilerini içerir. Aynı cihaz sonuçları hata için daha fazla ileti gönderilmeye çalışılıyor.

## <a name="the-cloud-to-device-message-lifecycle"></a>Bulut-cihaz ileti yaşam döngüsü

En az bir kez mesaj teslimatı sağlamak için IOT Hub cihaz başına sıralardaki bulut-cihaz iletilerini devam ettirir. Gereken cihazları açıkça kabul *tamamlama* IOT Hub'ı sıradan çıkarmak için. Bu yaklaşım, bağlantı ve aygıt hatalarına karşı dayanıklılık garanti eder.

Aşağıdaki diyagramda, IOT Hub'ında bulut buluttan cihaza iletinin yaşam döngüsü durumu grafik gösterir.

![Bulut-cihaz ileti yaşam döngüsü](./media/iot-hub-devguide-messages-c2d/lifecycle.png)

IOT Hub hizmeti bir cihaza ileti gönderdiğinde hizmet ileti durumu ayarlar **sıraya**. Ne zaman bir cihaz isteyen *alma* bir ileti, IOT hub'ı *kilitleri* ileti (durumu ayarlayarak **görünmez**), almaya başlamak cihazda başka iş parçacıklarının sağlar diğer iletiler. Bir cihaz iş parçacığı bir iletisinin işlenmesi tamamlandığında, IOT Hub tarafından bildirir *Tamamlanıyor* ileti. IOT hub'ı daha sonra ayarlar durumu **tamamlandı**.

Ayrıca, bir cihaz da tercih edebilirsiniz:

* *Reddetme* IOT Hub'ı ayarlamak için neden olan ileti **kullanılmayan lettered** durumu. Bulut-cihaz iletilerini bağlanan ve MQTT protokolünü cihazları reddedemezsiniz.

* *İptal* kümesine durumu ile geri kuyrukta ileti yerleştirmek IOT Hub neden olan ileti **sıraya**. Bulut-cihaz iletilerini bağlanan ve MQTT protokolünü cihazları iptal edilemez.

Bir iş parçacığı, IOT Hub'ı bildirmeden bir iletiyi işlemek başarısız olabilir. Bu durumda, iletileri otomatik olarak durumundan **görünmez** geri durum **sıraya** sonra durum bir *görünürlük (veya kilit) zaman aşımı*. Bir dakikalık zaman aşımı varsayılan değerdir.

**En fazla teslimat sayısı** özelliği IOT Hub'ında en fazla kaç kez bir ileti arasında geçiş belirler **sıraya** ve **görünmez** durumları. Bu geçiş sayısı sonra IOT hub'ı iletiye durumunu ayarlar **kullanılmayan lettered**. Benzer şekilde, IOT Hub ileti durumu ayarlar **kullanılmayan lettered** sona erme zamanı sonra (bkz [yaşam süresi](#message-expiration-time-to-live)).

[IOT Hub ile bulut-cihaz iletilerini göndermek nasıl](iot-hub-csharp-csharp-c2d.md) buluttan bulut-cihaz iletilerini göndermek ve bunları bir cihazda almak nasıl gösterir.

Genellikle, ileti kaybı uygulama mantığı etkilemez, bir cihaz bir bulut-cihaz iletiyi tamamlar. Örneğin, bir işlem olduğunda cihaz iletinin içerik yerel olarak kalıcı veya başarıyla yürütüldü. İleti geçici bilgi kaybı olan, uygulamanın işlevselliğini etkileyebilir değil, ayrıca gerçekleştirmek. Bazı durumlarda, uzun süre çalışan görevler için şunları yapabilirsiniz:

* Görev tanımı yerel depolama alanında kalıcı hale sonra bulut buluttan cihaza iletinin tamamlayın.

* Bir veya daha fazla cihaz-bulut iletileri ile çözüm arka ucu, görevin ilerleme çeşitli aşamalarında bildirin.

## <a name="message-expiration-time-to-live"></a>İleti süre sonu (yaşam süresi)

Her bulut buluttan cihaza iletinin sona erme süresini vardır. Bu süre biri tarafından ayarlanır:

* **ExpiryTimeUtc** hizmetinde özelliği.
* IOT Hub'ın varsayılan değeri kullanmanın *yaşam süresi* bir IOT hub'ı özelliği olarak belirtildi.

Bkz: [bulut-cihaz yapılandırma seçenekleri](#cloud-to-device-configuration-options).

İleti süre sonu avantajlarından yararlanın ve önlemek için yaygın bir yolu bağlantısı kesilmiş cihazlar için gönderme kısa değerler yaşam süresi ayarlamaktır. Bu yaklaşım daha verimli olmanın yanı sıra cihaz bağlantı durumunun koruma aynı sonucu elde eder. İleti onayları istediğinde, hangi cihazlar IOT hub'ı bildirir:

* İleti almak kullanabilirsiniz.
* Çevrimiçi olmayan ya da başarısız oldu.

## <a name="message-feedback"></a>İleti geri bildirim

Hizmet, bulut buluttan cihaza ileti gönderme, ileti son durumu hakkında bir ileti başına geri bildirim teslim isteğinde bulunabilirsiniz. Bu ayarı gerçekleştirilir `iothub-ack` uygulama özelliği şu değerlerden birini gönderilen C2D iletisi:

| ACK özellik değeri | Davranış |
| ------------ | -------- |
| **Yok**     | IOT hub'ı geri bildirim iletisi (varsayılan davranış) oluşturmaz. |
| **Pozitif** | Bulut-cihaz ileti ulaşırsa **tamamlandı** durumunda, IOT hub'ı geri bildirim iletisi oluşturur. |
| **Negatif** | Bulut-cihaz ileti ulaşırsa **kullanılmayan lettered** durumunda, IOT hub'ı geri bildirim iletisi oluşturur. |
| **Tam**     | IOT hub'ı, her iki durumda da bir geri bildirim iletisi oluşturmayacaktır. |

Varsa **Ack** olduğu **tam**ve geri bildirim iletisi almaz, geri bildirim iletisi dolduğunu anlamına gelir. Hizmet, özgün iletinin ne olduğunu bilemezsiniz. Uygulamada, bir hizmet süresi dolmadan önce bu geri bildirim işleyebildiğinden emin olmalısınız. Bir hata oluşursa maksimum süre sonu zamanı tekrar hizmet edinebilmek için zamanda bırakan iki gün çalışıyor.

İçinde anlatıldığı gibi [uç noktaları](iot-hub-devguide-endpoints.md), IOT Hub hizmeti'e yönelik uç noktası aracılığıyla geri bildirim sunar (**/messages/servicebound/feedback**) iletileri. Geri bildirim alma semantiği bulut-cihaz iletilerini ile aynıdır. Mümkün olduğunda, aşağıdaki biçime sahip tek bir ileti, ileti geri bildirim toplu:

| Özellik     | Açıklama |
| ------------ | ----------- |
| EnqueuedTime | Geri bildirim iletisi hub tarafından ne zaman alındı belirten bir zaman damgası. |
| UserId       | `{iot hub name}` |
| ContentType  | `application/vnd.microsoft.iothub.feedback.json` |

Gövdesi bir JSON ile seri hale getirilmiş kayıtları, aşağıdaki özelliklere sahip her dizisidir:

| Özellik           | Açıklama |
| ------------------ | ----------- |
| EnqueuedTimeUtc    | İleti sonucunu ne zaman oluştuğunu gösteren zaman damgası. Örneğin, hub'ı geri bildirim iletisini veya özgün iletinin süresi dolmuş. |
| Originalmessageıd  | **MessageID** bu geri bildirim bilgilerini ilgili olduğu bulut-cihaz ileti. |
| statusCode         | Zorunlu dize. Geri bildirim iletileri IOT Hub tarafından üretilen kullanılır. <br/> 'Başarılı' şeklindedir <br/> 'Süresi dolmuş' <br/> 'DeliveryCountExceeded' <br/> 'Reddedildi' <br/> 'Temizlendi' |
| Açıklama        | Dize değerlerini **StatusCode**. |
| DeviceId           | **DeviceID** hedef cihazın bu geri bildirim parçası ilgili olduğu bulut-cihaz ileti. |
| DeviceGenerationId | **DeviceGenerationId** hedef cihazın bu geri bildirim parçası ilgili olduğu bulut-cihaz ileti. |

Hizmet belirtmelisiniz bir **MessageID** kendi geri bildirim özgün mesajla ilişkilendirebilmesi bulut-cihaz ileti.

Aşağıdaki örnek, bir geri bildirim iletisinin gövdesini gösterir.

```json
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0,
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

## <a name="cloud-to-device-configuration-options"></a>Bulut-cihaz yapılandırma seçenekleri

Her IOT hub, bulut-cihaz Mesajlaşma için aşağıdaki yapılandırma seçeneklerini sunar:

| Özellik                  | Açıklama | Aralığı ve varsayılan |
| ------------------------- | ----------- | ----------------- |
| defaultTtlAsIso8601       | Bulut-cihaz iletilerini için varsayılan TTL değeri. | 2B (en az 1 dakika içinde) en fazla ISO_8601 aralığı. Varsayılan: 1 saat. |
| MaxDeliveryCount          | Bulut-cihaz cihaz başına kuyruklar için en yüksek teslimat sayısı. | 1-100. Varsayılan: 10. |
| feedback.ttlAsIso8601     | Bağlı hizmet geri bildirim iletileri için elde tutma. | 2B (en az 1 dakika içinde) en fazla ISO_8601 aralığı. Varsayılan: 1 saat. |
| feedback.maxDeliveryCount |Geri bildirim kuyruğu için en yüksek teslimat sayısı. | 1-100. Varsayılan: 100. |

Bu yapılandırma seçenekleri ayarlama hakkında daha fazla bilgi için bkz. [oluşturma IOT hub'ları](iot-hub-create-through-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

Bulut-cihaz iletilerini almak için kullanabileceğiniz SDK'lar hakkında bilgi için bkz. [Azure IOT SDK'ları](iot-hub-devguide-sdks.md).

Bulut-cihaz iletilerini alma kullanıma denemek için bkz [Gönder bulut-cihaz](iot-hub-csharp-csharp-c2d.md) öğretici.