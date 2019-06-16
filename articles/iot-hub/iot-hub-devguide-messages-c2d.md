---
title: Azure IOT Hub, bulut-cihaz Mesajlaşma anlama | Microsoft Docs
description: Bu Geliştirici Kılavuzu, bulut-cihaz ile IOT hub'ınıza ileti kullanmayı açıklar. İleti yaşam döngüsü ve yapılandırma seçenekleri hakkında bilgiler içerir.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/15/2018
ms.openlocfilehash: 0fc1b65a4ba1c8a3d76b48206d6a4703035e05bc
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67055335"
---
# <a name="send-cloud-to-device-messages-from-an-iot-hub"></a>Bir IOT hub'ından bulut buluttan cihaza iletileri gönderme

Bir cihaz uygulamasına, çözüm arka ucu tek yönlü bildirimleri göndermek için cihazınız için IOT hub'ından bulut-cihaz iletilerini gönderin. Azure IOT Hub tarafından desteklenen diğer bulut-cihaz seçenekleri için bkz [bulut buluttan cihaza iletişim Kılavuzu](iot-hub-devguide-c2d-guidance.md).

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bulut-cihaz iletilerini yönelik hizmet uç noktası aracılığıyla gönderdiğiniz */iletileri/devicebound*. Bir cihaz, ardından cihaza özel uç noktası aracılığıyla iletileri alan */devices/ {DeviceID} / iletileri/devicebound*.

Bulut-cihaz her iletinin tek bir cihaz hedeflemek için IOT hub'ınızın ayarlar **için** özelliğini */devices/ {DeviceID} / iletileri/devicebound*.

Her bir cihaz kuyruk en çok 50 bulut-cihaz iletilerini içerir. Bir hata aynı cihaz sonuçları daha fazla ileti göndermeyi denemek için.

## <a name="the-cloud-to-device-message-life-cycle"></a>Bulut buluttan cihaza iletinin yaşam döngüsü

En az bir kez mesaj teslimatı sağlamak için IOT hub'ınızın cihaz başına sıralardaki bulut-cihaz iletilerini devam ettirir. IOT hub iletileri kuyruktan kaldırmak cihazları açıkça onaylamanız gerekir *tamamlama*. Bu yaklaşım, bağlantı ve aygıt hatalarına karşı dayanıklılık garanti eder.

Aşağıdaki diyagramda yaşam döngüsü durumu grafiği görüntülenir:

![Bulut buluttan cihaza iletinin yaşam döngüsü](./media/iot-hub-devguide-messages-c2d/lifecycle.png)

IOT hub hizmeti bir cihaza ileti gönderdiğinde hizmet ileti durumu ayarlar *sıraya*. Bir cihazın ne zaman isteyen *alma* bir ileti, IOT hub'ı *kilitleri* durumu ayarlayarak iletiyi *görünmez*. Bu durum diğer iş parçacıklarını diğer iletiler almaya başlamak için cihazda izin verir. Bir cihaz iş parçacığı bir iletisinin işlenmesi tamamlandığında, IOT hub tarafından bildirir *Tamamlanıyor* ileti. IOT hub'ı sonra durumu ayarlar *tamamlandı*.

Bir cihaz da yapabilirsiniz:

* *Reddetme* ayarlamak için IOT hub neden olan ileti *kullanılmayan lettered* durumu. Message Queuing Telemetry Transport (MQTT) protokolü üzerinden bağlanan cihazların bulut-cihaz iletilerini reddedemezsiniz.

* *İptal* kümesine durumu ile geri kuyrukta ileti yerleştirmek IOT hub neden olan ileti *sıraya*. Bulut-cihaz iletilerini bağlanan ve MQTT protokolünü cihazları iptal edilemez.

Bir iş parçacığı, IOT hub'ı bildirmeden bir iletiyi işlemek başarısız olabilir. Bu durumda, iletileri otomatik olarak durumundan *görünmez* geri durum *sıraya* sonra durum bir *görünürlük* zaman aşımı (veya *kilit* zaman aşımı). Bu zaman aşımı varsayılan değerini bir dakikadır.

**En fazla teslimat sayısı** özelliği IOT hub'ında en fazla kaç kez bir ileti arasında geçiş belirler *sıraya* ve *görünmez* durumları. Bu geçiş sayısı sonra IOT hub'ı iletiye durumunu ayarlar *kullanılmayan lettered*. Benzer şekilde, IOT hub ileti durumu ayarlar *kullanılmayan lettered* sonra sona erme zamanı. Daha fazla bilgi için [yaşam süresi](#message-expiration-time-to-live).

[IOT Hub ile bulut-cihaz iletilerini göndermek nasıl](iot-hub-csharp-csharp-c2d.md) makale buluttan bulut-cihaz iletilerini göndermek ve bunları bir cihazda almak nasıl gösterir.

İleti kaybı uygulama mantığı etkilemez, bir cihaz, normalde bir bulut-cihaz iletiyi tamamlar. Cihaz ileti içeriği yerel olarak kalıcı veya bir işlemi başarıyla yürütüldü, bunun bir örneği olabilir. İleti geçici bilgi kaybı, uygulamanın işlevselliğini etkileyen mıydı, ayrıca gerçekleştirmek. Bazı durumlarda, uzun süre çalışan görevler için şunları yapabilirsiniz:

* Cihazın yerel depolama görev açıklamasında kaydetti sonra bulut buluttan cihaza iletinin tamamlayın.

* Bir veya daha fazla cihaz-bulut iletileri ile çözüm arka ucu, görevin ilerleme çeşitli aşamalarında bildirin.

## <a name="message-expiration-time-to-live"></a>İleti süre sonu (yaşam süresi)

Her bulut buluttan cihaza iletinin sona erme süresini vardır. Bu süresi aşağıdakilerden birini ayarlanır:

* **ExpiryTimeUtc** hizmetinde özelliği
* Varsayılan kullanarak IOT hub *yaşam süresi* bir IOT hub'ı özellik olarak belirtilir

Bkz: [bulut-cihaz yapılandırma seçenekleri](#cloud-to-device-configuration-options).

İleti süre sonu avantajlarından yararlanın ve bağlantısı kesilmiş cihazlar için iletileri göndermekten kaçınmanız en yaygın yolu kısa ayarlamaktır *yaşam süresi* değerleri. Bu yaklaşım, cihaz bağlantısı durum koruma olarak aynı sonucu veren, ancak daha etkilidir. İleti bildirimler istediğinde, hangi cihazlar IOT hub'ı bildirir:

* İleti almak kullanabilirsiniz.
* Çevrimiçi olmayan ya da başarısız oldu.

## <a name="message-feedback"></a>İleti geri bildirim

Hizmet, bulut buluttan cihaza ileti gönderme, ileti son durumuyla ilgili ileti başına geri bildirim teslim isteğinde bulunabilirsiniz. Ayarlayarak bunu **iothub-ack** uygulama özelliği aşağıdaki dört değerden biri olarak gönderilen bulut-cihaz iletisi:

| ACK özellik değeri | Davranış |
| ------------ | -------- |
| Yok     | IOT hub'ı geri bildirim iletisi (varsayılan davranış) oluşturmaz. |
| pozitif | Bulut-cihaz ileti ulaşırsa *tamamlandı* durumunda, IOT hub'ı geri bildirim iletisi oluşturur. |
| Negatif | Bulut-cihaz ileti ulaşırsa *kullanılmayan lettered* durumunda, IOT hub'ı geri bildirim iletisi oluşturur. |
| tam     | IOT hub'ı, her iki durumda da bir geri bildirim iletisi oluşturmayacaktır. |

Varsa **Ack** değer *tam*ve geri bildirim iletisi almaz, geri bildirim iletisi süresinin dolduğunu anlamına gelir. Hizmet, özgün iletinin ne olduğunu bilemezsiniz. Uygulamada, bir hizmet süresi dolmadan önce bu geri bildirim işleyebildiğinden emin olmalısınız. Bir hata oluşursa maksimum süre tekrar hizmet edinebilmek için zamanda bırakan iki gün çalışıyor.

İçinde anlatıldığı gibi [uç noktaları](iot-hub-devguide-endpoints.md), IOT hub'ı yönelik hizmet uç noktası aracılığıyla geri bildirim teslim */messages/servicebound/feedback*, iletileri olarak. Geri bildirim alma semantiği bulut-cihaz iletilerini ile aynıdır. Mümkün olduğunda, aşağıdaki biçime sahip tek bir ileti, ileti geri bildirim toplu:

| Özellik     | Açıklama |
| ------------ | ----------- |
| EnqueuedTime | Geri bildirim iletisi hub tarafından ne zaman alındı belirten bir zaman damgası |
| UserId       | `{iot hub name}` |
| contentType  | `application/vnd.microsoft.iothub.feedback.json` |

Gövdesi bir JSON ile seri hale getirilmiş kayıtları, aşağıdaki özelliklere sahip her dizisidir:

| Özellik           | Açıklama |
| ------------------ | ----------- |
| EnqueuedTimeUtc    | İleti sonucunu ne zaman oluştuğunu gösteren zaman damgası (örneğin, hub'ı geri bildirim iletisini veya özgün iletinin süresi doldu) |
| Originalmessageıd  | *MessageID* bulut-cihaz iletinin ilişkili olduğu için bu geri bildirim bilgileri |
| statusCode         | IOT hub tarafından oluşturulan geri bildirim iletileri kullanılan bir gerekli dize: <br/> *Başarılı* <br/> *Süresi doldu* <br/> *DeliveryCountExceeded* <br/> *Reddedildi* <br/> *Temizlendi* |
| Açıklama        | Dize değerlerini *StatusCode* |
| DeviceId           | *DeviceID* hedef cihazın bu geri bildirim parçası ilgili olduğu bulut-cihaz ileti |
| DeviceGenerationId | *DeviceGenerationId* hedef cihazın bu geri bildirim parçası ilgili olduğu bulut-cihaz ileti |

Bulut-cihaz ileti kendi geri bildirim özgün mesajla ilişkilendirebilmesi hizmet belirtmelisiniz bir *MessageID*.

Geri bildirim iletisinin gövdesi, aşağıdaki kodda gösterilmiştir:

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
| defaultTtlAsIso8601       | Bulut-cihaz iletilerini için varsayılan TTL | ISO_8601 aralığı 2 güne kadar (en az 1 dakika); Varsayılan: 1 saat |
| MaxDeliveryCount          | Bulut-cihaz cihaz başına kuyruklar için en yüksek teslimat sayısı | 1-100; Varsayılan: 10 |
| feedback.ttlAsIso8601     | Bağlı hizmet geri bildirim iletileri için bekletme | ISO_8601 aralığı 2 güne kadar (en az 1 dakika); Varsayılan: 1 saat |
| feedback.maxDeliveryCount | Geri bildirim kuyruğu için en yüksek teslimat sayısı | 1-100; Varsayılan: 100 |

Bu yapılandırma seçenekleri ayarlama hakkında daha fazla bilgi için bkz. [oluşturma IOT hub'ları](iot-hub-create-through-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

Bulut-cihaz iletilerini almak için kullanabileceğiniz SDK'lar hakkında daha fazla bilgi için bkz. [Azure IOT SDK'ları](iot-hub-devguide-sdks.md).

Bulut-cihaz iletilerini alma kullanıma denemek için bkz [Gönder bulut-cihaz](iot-hub-csharp-csharp-c2d.md) öğretici.
