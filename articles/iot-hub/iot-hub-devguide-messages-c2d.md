---
title: Azure IOT Hub bulut cihaz Mesajlaşma anlama | Microsoft Docs
description: Geliştirici Kılavuzu - bulut cihaz IOT Hub ile Mesajlaşma kullanma. İleti yaşam döngüsü ve yapılandırma seçenekleri hakkında bilgi içerir.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: ''
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/15/2018
ms.author: dobett
ms.openlocfilehash: 670cf45a48ca4b72576cedddd4678c0d569401cd
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="send-cloud-to-device-messages-from-iot-hub"></a>IOT Hub'ından bulut cihaza ileti gönderme

Çözüm arka ucu cihaz uygulamasının tek yönlü bildirimleri göndermek için bulut aygıtları iletileri IOT hub'ından aygıtınıza gönderin. IOT Hub tarafından desteklenen diğer bulut aygıtları seçenekleri tartışma için bkz [bulut-cihaz iletişimi Kılavuzu][lnk-c2d-guidance].

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bir hizmet'e yönelik uç noktası aracılığıyla bulut cihaz göndermek (**/iletileri/devicebound**). Bir aygıt sonra bir cihaza özel uç noktası aracılığıyla iletileri alır (**/devices/ {DeviceID} / iletileri/devicebound**).

IOT hub'ı tek bir cihazı her bulut cihaz ileti hedef ayarlar **için** özelliğine **/devices/ {DeviceID} / iletileri/devicebound**.

Her cihaz sıra en çok 50 bulut-cihaz iletilerini tutar. Aynı aygıt sonuçları hata daha fazla ileti göndermeye çalışıyor.

## <a name="the-cloud-to-device-message-lifecycle"></a>Bulut cihaz ileti yaşam döngüsü

En az bir kere ileti teslimi güvence altına almak için IOT Hub cihaz başına sıralardaki bulut-cihaz iletilerini devam ettirir. Aygıtları gerekir açıkça onaylarsınız *tamamlama* IOT Hub'ın sıradan kaldırmak için. Bu yaklaşım bağlantısı ve aygıt hatalarına karşı dayanıklılık güvence altına alır.

Aşağıdaki diyagramda IOT Hub ' bulut cihaza iletinin yaşam döngüsü durumu grafik gösterir.

![Bulut cihaz ileti yaşam döngüsü][img-lifecycle]

IOT Hub hizmeti bir cihaza ileti gönderdiğinde, hizmet ileti durumu ayarlar **sıraya alınan**. Bir aygıtın ne zaman istemektedir *almak* IOT hub'ı bir ileti *kilitleri* ileti (durumu ayarlayarak **görünmez**), diğer iletileri almaya başlamak aygıtta başka bir iş parçacığı sağlar. Bir aygıt iş parçacığı bir ileti işlenmesi tamamlandığında, IOT Hub tarafından bildirir *Tamamlanıyor* ileti. IOT hub'ı sonra ayarlar durumu **tamamlandı**.

Bir aygıt de seçebilirsiniz:

* *Reddetme* IOT Hub'ı ayarlamak için neden olan ileti **kullanılmayan lettered** durumu. MQTT protokolü üzerinden bağlanan aygıtları bulut-cihaz iletilerini reddedemezsiniz.
* *Abandon* ileti kümesine durumuyla kuyruğa geri koymak için IOT Hub neden olan ileti **sıraya alınan**. MQTT protokolü üzerinden bağlanan aygıtları bulut-cihaz iletilerini iptal edilemez.

Bir iş parçacığı IOT hub'ı bildirmeden bir iletiyi işlemek başarısız olabilir. Bu durumda, ileti otomatik olarak gelen geçiş **görünmez** geri durum **sıraya alınan** sonra durum bir *görünürlük (veya kilidi) zaman aşımı*. Bir dakika zaman aşımı varsayılan değerdir.

**Maksimum teslimat sayısı** IOT hub'ındaki özelliği belirler en fazla kaç kez bir ileti arasında geçiş **sıraya alınan** ve **görünmez** durumları. Geçişleri sayıdaki sonra IOT hub'ı iletiye durumunu ayarlar **kullanılmayan lettered**. Benzer şekilde, IOT Hub için bir ileti durumunu ayarlar **kullanılmayan lettered** kendi sona erme zamanı sonra (bkz [yaşam süresi][lnk-ttl]).

[IOT Hub ile bulut-cihaz iletilerini göndermek nasıl] [ lnk-c2d-tutorial] buluttan bulut-cihaz iletilerini göndermek ve bunları bir aygıtta almak nasıl gösterir.

Genellikle, ileti kaybı uygulama mantığını etkilemez sırada bir aygıt bir bulut cihaz ileti tamamlar. Örneğin, bir işlem olduğunda aygıt iletinin içerik yerel olarak kalıcı veya başarıyla yürütüldü. İleti, ayrıca, kaybı, uygulamanın işlevselliğini etkileyen değil, geçici bilgilerini taşımak. Bazı durumlarda, uzun süre çalışan görevler için şunları yapabilirsiniz:

* Görev tanımı yerel depolama kalıcı sonra bulut cihaz iletisi tamamlayın.
* Çözüm arka ucu ile bir veya daha fazla cihaz-bulut iletileri görevin ilerlemesini çeşitli aşamalarında bildirin.

## <a name="message-expiration-time-to-live"></a>İleti kullanım süresi sonu (yaşam süresi)

Her bulut cihaz iletinin sona erme süresi vardır. Bu süre biri tarafından ayarlanır:

* **ExpiryTimeUtc** hizmet özelliği.
* IOT Hub'ın varsayılan kullanılarak *yaşam süresi* bir IOT Hub özelliği olarak belirtilmiş.

Bkz: [bulut cihaz yapılandırma seçenekleri][lnk-c2d-configuration].

İleti süresinin sona ermesi yararlanabilir ve önlemek için genel bir bağlantısı kesilmiş aygıtları için ileti gönderme kısa değerler yaşam süresini ayarlamak için yoludur. Bu yaklaşım daha verimli devam ederken aygıt bağlantı durumunu koruma aynı sonucu elde eder. İleti onayları istediğinde, IOT Hub cihazları olan sizi uyarır:

* İletileri almak kullanabilirsiniz.
* Çevrimiçi olmayan ya da başarısız oldu.

## <a name="message-feedback"></a>İleti geri bildirim

Bir bulut cihaz ileti gönderirken hizmet ileti son durumu ile ilgili her ileti geri bildirim teslimini isteyebilirler.

| ACK özelliği | Davranış |
| ------------ | -------- |
| **Pozitif** | Bulut cihaz iletisi ulaşırsa **tamamlandı** durumunda, IOT hub'ı bir geri bildirim iletisi oluşturur. |
| **Negatif** | Bulut cihaz iletisi ulaşırsa **kullanılmayan lettered** durumunda, IOT hub'ı bir geri bildirim iletisi oluşturur. |
| **tam**     | IOT Hub, her iki durumda da bir geri bildirim iletisi oluşturur. |

Varsa **Ack** olan **tam**ve geri bildirim iletisi almadığınız, geri bildirim iletisi dolduğunu anlamına gelir. Hizmet, özgün iletiye ne bilemezsiniz. Uygulamada, bir hizmet süresi dolmadan önce bu geri bildirim işleyebilir emin olmalısınız. Bir hata oluşursa maksimum süre sonu zamanı hizmet almak için süre bırakır iki gün yeniden çalışıyor.

İçinde anlatıldığı gibi [uç noktaları][lnk-endpoints], IOT hub'ı sunan bir hizmet'e yönelik uç noktası aracılığıyla geribildirim (**/messages/servicebound/feedback**) iletileri olarak. Geri bildirim alma semantiğini bulut-cihaz iletilerini ile aynıdır. Mümkün olduğunda, ileti geri bildirim aşağıdaki biçime sahip tek bir iletide toplu hale:

| Özellik     | Açıklama |
| ------------ | ----------- |
| EnqueuedTime | Geri bildirim iletisi hub tarafından ne zaman alındı belirten bir zaman damgası. |
| UserId       | `{iot hub name}` |
| ContentType  | `application/vnd.microsoft.iothub.feedback.json` |

Gövde bir JSON serileştirilmiş dizisi kayıtları, her biri aşağıdaki özelliklere sahip olan:

| Özellik           | Açıklama |
| ------------------ | ----------- |
| EnqueuedTimeUtc    | Ne zaman ileti sonucu olduğunu belirten bir zaman damgası. Örneğin, hub geri bildirim iletisi aldı veya özgün iletinin süresi doldu. |
| OriginalMessageId  | **MessageID** bu geri bildirim bilgilerini ilgili olduğu bulut cihaz iletisi. |
| StatusCode         | Gerekli dize. IOT Hub tarafından oluşturulan geri bildirim iletileri kullanılır. <br/> 'Başarılı' <br/> 'Süresi' <br/> 'DeliveryCountExceeded' <br/> 'Reddedildi' <br/> 'Temizlendi' |
| Açıklama        | Dize değerlerini **StatusCode**. |
| DeviceId           | **DeviceID** hedef cihaz bu aldığımız geri Bildirimlerden biri ilgili olduğu bulut cihaz iletisi. |
| DeviceGenerationId | **DeviceGenerationId** bu aldığımız geri Bildirimlerden biri ilgili olduğu bulut cihaz iletisinin hedef cihaz. |

Hizmet belirtmelisiniz bir **MessageID** kendi geri bildirim özgün iletiyle ilişkilendirmek bulut cihaz ileti.

Aşağıdaki örnek, bir geri bildirim iletisinin gövdesinde gösterir.

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

## <a name="cloud-to-device-configuration-options"></a>Bulut cihaz yapılandırma seçenekleri

Her IOT hub'ı bulut cihaz Mesajlaşma için aşağıdaki yapılandırma seçeneklerini gösterir:

| Özellik                  | Açıklama | Aralık ve varsayılan |
| ------------------------- | ----------- | ----------------- |
| defaultTtlAsIso8601       | Bulut-cihaz iletilerini için varsayılan TTL değeri. | ISO_8601 aralığı kadar 2B (en az 1 dakika). Varsayılan: 1 saat. |
| maxDeliveryCount          | Maksimum teslimat sayısı her aygıt için bulut cihaz sıralar. | 1 ile 100. Varsayılan: 10. |
| feedback.ttlAsIso8601     | Hizmet bağlı geri bildirim iletileri için bekletme. | ISO_8601 aralığı kadar 2B (en az 1 dakika). Varsayılan: 1 saat. |
| feedback.maxDeliveryCount |Geri bildirim kuyruğu en yüksek teslimat sayısı. | 1 ile 100. Varsayılan: 100. |

Bu yapılandırma seçenekleri ayarlama hakkında daha fazla bilgi için bkz: [oluşturma IOT hub'ları][lnk-portal].

## <a name="next-steps"></a>Sonraki adımlar

Bulut cihaz iletileri almak için kullanabileceğiniz SDK'ları hakkında bilgi için bkz: [Azure IOT SDK'ları][lnk-sdks].

Bulut-cihaz iletilerini alma çıkışı denemek için bkz: [Gönder bulut cihaz] [ lnk-c2d-tutorial] Öğreticisi.

[img-lifecycle]: ./media/iot-hub-devguide-messages-c2d/lifecycle.png

[lnk-portal]: iot-hub-create-through-portal.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-ttl]: #message-expiration-time-to-live
[lnk-c2d-configuration]: #cloud-to-device-configuration-options
[lnk-lifecycle]: #message-lifecycle
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
