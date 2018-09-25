---
title: Azure IOT Hub ileti biçimi anlama | Microsoft Docs
description: Geliştirici Kılavuzu - descibes biçimi ve IOT Hub iletilerini beklenen içerik.
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/13/2018
ms.author: asrastog
ms.openlocfilehash: edea20343c2a261902c082dbc5c96b78db6b470d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973229"
---
# <a name="create-and-read-iot-hub-messages"></a>IoT Hub iletilerini oluşturma ve okuma

Protokoller üzerinde sorunsuz birlikte çalışabilirlik desteklemek için IOT Hub cihaz'e yönelik tüm protokoller için ortak bir ileti biçimine tanımlar. Bu ileti biçimi için kullanılan [CİHAZDAN buluta yönlendirme](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c) ve [bulut-cihaz](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-c2d) iletileri. 

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

IOT Hub CİHAZDAN buluta ileti gönderme akış bir Mesajlaşma deseni kullanılarak uygular. IOT Hub'ınızın CİHAZDAN buluta iletileri gibi daha fazla [Event Hubs](https://docs.microsoft.com/azure/event-hubs/) *olayları* daha [Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/) *iletileri* yoktur yüksek hacimli, birden çok okuyucular tarafından okunan hizmeti aracılığıyla geçirme olayları.

Bir IOT Hub iletisi oluşur:
* Önceden belirlenen bir dizi *Sistem Özellikleri* aşağıda listelenen.
* Bir dizi *uygulama özellikleri*. Uygulamayı tanımlayan dize özellikleri ve ileti gövdesi seri durumdan gerek olmadan erişim sözlüğü. IOT hub'ı hiçbir zaman bu özellikleri değiştirir.
* Donuk bir ikili gövdesi.

Özellik adlarını ve değerlerini içerebilir ASCII alfasayısal karakterler, artı ```{'!', '#', '$', '%, '&', "'", '*', '+', '-', '.', '^', '_', '`', '|', '~'}``` CİHAZDAN buluta iletileri HTTPS kullanarak iletişim kuralı veya Bulut buluttan cihaza iletileri gönderme gönderdiğinizde.

CİHAZDAN buluta iletileri IOT Hub ile aşağıdaki özelliklere sahiptir:

* CİHAZDAN buluta iletileri, dayanıklı ve bir IOT hub'ın varsayılan saklama **iletiler/olaylar** yedi güne kadar uç noktası.
* CİHAZDAN buluta iletileri en fazla 256 KB olabilir ve gönderen iyileştirmek için toplu olarak gruplandırılabilir. Toplu en fazla 256 KB olabilir.
* IOT hub'ı rastgele bölümleme izin vermez. CİHAZDAN buluta iletileri bölümlenmiş kendi kaynak tabanlı **DeviceID**.
* İçinde anlatıldığı gibi [IOT hub'a erişimi denetleme](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security) bölümde, IOT Hub cihaz başına kimlik doğrulaması ve erişim denetimi sağlar.

Kodlama ve kodunu çözme farklı protokolleriyle gönderilen iletiler hakkında daha fazla bilgi için bkz. [Azure IOT SDK'ları](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks).

Aşağıdaki tabloda, IOT Hub iletilerini Sistem özelliklerinde kümesini listeler.

| Özellik | Açıklama | Kullanıcı ayarlanabilir mi? |
| --- | --- | --- |
| MessageId |İstek-yanıt desenlerinde kullanılan ileti için kullanıcı ayarlanabilir bir tanımlayıcı. Biçim: Bir büyük küçük harfe duyarlı dize (en çok 128 karakterden uzun), ASCII 7 bit alfasayısal karakterlerin + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. | Evet |
| Sıra numarası |Her bulut buluttan cihaza iletiye IOT Hub tarafından atanmış bir sayı (benzersiz cihaz kuyruk). | C2D iletileri için Hayır '; Evet, aksi takdirde. |
| Alıcı |İletileri [bulut-cihaz] [lnk-c2d] belirtilen bir hedef. | C2D iletileri için Hayır '; Evet, aksi takdirde. |
| ExpiryTimeUtc |Tarih ve saat ileti sonu. | Evet |
| EnqueuedTime |Tarih ve saat IOT Hub tarafından [bulut-cihaz] [lnk-c2d] iletisi alındı. | C2D iletileri için Hayır '; Evet, aksi takdirde. |
| CorrelationId |Genellikle istek, istek-yanıt desenleri MessageID içeren bir yanıt iletisi bir dize özelliği. | Evet |
| UserId |İletileri kaynağını belirtmek için kullanılan bir kimliği. İletileri IOT Hub tarafından oluşturulduğunda ayarlanır `{iot hub name}`. | Hayır |
| ACK |Geri bildirim iletisi Oluşturucusu. Bu özellik bulut-cihaz iletilerini cihaz tarafından sonucunda iletinin Tüketim geri bildirim iletileri oluşturmak için IOT hub'ı istemek için kullanılır. Olası değerler: **hiçbiri** (varsayılan): hiçbir geri bildirim iletisi oluşturulur, **pozitif**: ileti tamamlanmışsa, bir geri bildirim iletisi **negatif**: alma bir iletinin süresi (veya en yüksek teslimat sayısı ulaşıldı varsa) ve cihaz tarafından tamamlanmasını olmadan geri bildirim iletisi veya **tam**: pozitif ve negatif. Daha fazla bilgi için bkz. [ileti geri bildirim] [lnk-geri bildirim]. | Evet |
| ConnectionDeviceId |CİHAZDAN buluta iletileri IOT Hub tarafından ayarlanmış bir kimlik. İçerdiği **DeviceID** iletiyi gönderen cihazın. | D2C iletileri için Hayır '; Evet, aksi takdirde. |
| ConnectionDeviceGenerationId |CİHAZDAN buluta iletileri IOT Hub tarafından ayarlanmış bir kimlik. İçerdiği **Generationıd** (göre [cihaz kimliği properties][lnk-device-properties]) iletiyi gönderen cihazın. | D2C iletileri için Hayır '; Evet, aksi takdirde. |
| ConnectionAuthMethod |CİHAZDAN buluta iletileri IOT Hub tarafından ayarlanmış bir kimlik doğrulama yöntemi. Bu özellik, iletinin gönderme cihazın kimliğini doğrulamak için kullanılan kimlik doğrulama yöntemi hakkında bilgi içerir. Daha fazla bilgi için bkz [CİHAZDAN yanıltma buluta] [lnk-antispoofing]. | D2C iletileri için Hayır '; Evet, aksi takdirde. |
| CreationTimeUtc | Tarih ve saat ileti bir cihazda oluşturuldu. Bir cihaz, bu değeri açıkça ayarlamanız gerekir. | Evet |

## <a name="message-size"></a>İleti boyutu

IOT Hub ileti boyutu, bir protokol belirsiz şekilde, yalnızca gerçek yükü dikkate ölçer. Bayt cinsinden boyut aşağıdakilerden toplamı hesaplanır:

* Gövde boyutu bayt cinsinden.
* İleti sistemi özelliklerinin değerlerinin bayt cinsinden boyutu.
* Tüm kullanıcı özellik adlarının ve değerlerinin bayt cinsinden boyutu.

Bayt cinsinden boyutu dize uzunluğu eşittir özellik adlarını ve değerlerini ASCII karakter ile sınırlıdır.

## <a name="anti-spoofing-properties"></a>Sahtekarlığına karşı koruma özellikleri

Tüm Damgalar cihaz IOT hub'ı, CİHAZDAN buluta iletileri kimlik sahtekarlığı önlemek için aşağıdaki özelliklere sahip iletileri:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

İlk iki içeren **DeviceID** ve **Generationıd** kaynak cihazın olarak başına [cihaz kimlik özelliklerini](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-identity-registry#device-identity-properties).

**ConnectionAuthMethod** özelliği, aşağıdaki özelliklere sahip bir seri hale getirilmiş JSON nesnesi içerir:

```json
{
  "scope": "{ hub | device }",
  "type": "{ symkey | sas | x509 }",
  "issuer": "iothub"
}
```

## <a name="next-steps"></a>Sonraki adımlar

* IOT hub'ında ileti boyutu sınırları hakkında daha fazla bilgi için bkz: [IOT Hub kotaları ve azaltma](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-quotas-throttling).
* Oluşturun ve IOT hub'ı çeşitli programlama dillerinde iletileri okumak öğrenmek için bkz: [hızlı Başlangıçlar](https://docs.microsoft.com/azure/iot-hub/quickstart-send-telemetry-node).