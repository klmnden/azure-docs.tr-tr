---
title: Azure IOT Hub ileti biçimi anlama | Microsoft Docs
description: Geliştirici Kılavuzu - biçimini ve IOT Hub iletilerini beklenen içeriğini açıklar.
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/13/2018
ms.author: asrastog
ms.openlocfilehash: 49a28c120ae71224195edcdb3809335aeea1fa3a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61364328"
---
# <a name="create-and-read-iot-hub-messages"></a>IoT Hub iletilerini oluşturma ve okuma

Protokoller üzerinde sorunsuz birlikte çalışabilirlik desteklemek için IOT Hub cihaz'e yönelik tüm protokoller için ortak bir ileti biçimine tanımlar. Bu ileti biçimi için kullanılan [CİHAZDAN buluta yönlendirme](iot-hub-devguide-messages-d2c.md) ve [bulut-cihaz](iot-hub-devguide-messages-c2d.md) iletileri. 

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

IOT Hub CİHAZDAN buluta ileti gönderme akış bir Mesajlaşma deseni kullanılarak uygular. IOT Hub'ınızın CİHAZDAN buluta iletileri gibi daha fazla [Event Hubs](/azure/event-hubs/) *olayları* daha [Service Bus](/azure/service-bus-messaging/) *iletileri* yoktur yüksek hacimli, birden çok okuyucular tarafından okunan hizmeti aracılığıyla geçirme olayları.

Bir IOT Hub iletisi oluşur:

* Önceden belirlenen bir dizi *Sistem Özellikleri* aşağıda listelenen.

* Bir dizi *uygulama özellikleri*. Uygulamayı tanımlayan dize özellikleri ve ileti gövdesi seri durumdan gerek olmadan erişim sözlüğü. IOT hub'ı hiçbir zaman bu özellikleri değiştirir.

* Donuk bir ikili gövdesi.

Özellik adlarını ve değerlerini içerebilir ASCII alfasayısal karakterler, artı ``{'!', '#', '$', '%, '&', ''', '*', '+', '-', '.', '^', '_', '`', '|', '~'}`` CİHAZDAN buluta iletileri HTTPS kullanarak iletişim kuralı veya Bulut buluttan cihaza iletileri gönderme gönderdiğinizde.

CİHAZDAN buluta iletileri IOT Hub ile aşağıdaki özelliklere sahiptir:

* CİHAZDAN buluta iletileri, dayanıklı ve bir IOT hub'ın varsayılan saklama **iletiler/olaylar** yedi güne kadar uç noktası.

* CİHAZDAN buluta iletileri en fazla 256 KB olabilir ve gönderen iyileştirmek için toplu olarak gruplandırılabilir. Toplu en fazla 256 KB olabilir.

* IOT hub'ı rastgele bölümleme izin vermez. CİHAZDAN buluta iletileri bölümlenmiş kendi kaynak tabanlı **DeviceID**.

* İçinde anlatıldığı gibi [IOT hub'a erişimi denetleme](iot-hub-devguide-security.md), IOT Hub cihaz başına kimlik doğrulaması ve erişim denetimi sağlar.

Kodlama ve kodunu çözme farklı protokolleriyle gönderilen iletiler hakkında daha fazla bilgi için bkz. [Azure IOT SDK'ları](iot-hub-devguide-sdks.md).

Aşağıdaki tabloda, IOT Hub iletilerini Sistem özelliklerinde kümesini listeler.

| Özellik | Açıklama | Kullanıcı ayarlanabilir mi? |
| --- | --- | --- |
| ileti kimliği |İstek-yanıt desenlerinde kullanılan ileti için kullanıcı ayarlanabilir bir tanımlayıcı. Biçim: ASCII 7 bit alfasayısal karakterlerin büyük küçük harfe duyarlı bir dize (en çok 128 karakterden uzun) + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. | Evet |
| sıra numarası |Her bulut buluttan cihaza iletiye IOT Hub tarafından atanmış bir sayı (benzersiz cihaz kuyruk). | C2D iletileri için Hayır '; Evet, aksi takdirde. |
| - |Belirtilen bir hedef [bulut-cihaz](iot-hub-devguide-c2d-guidance.md) iletileri. | C2D iletileri için Hayır '; Evet, aksi takdirde. |
| süre sonu mutlak |Tarih ve saat ileti sonu. | Evet |
| iothub-enqueuedtime |Tarih ve saat [bulut-cihaz](iot-hub-devguide-c2d-guidance.md) ileti, IOT Hub tarafından alındı. | C2D iletileri için Hayır '; Evet, aksi takdirde. |
| Bağıntı Kimliği |Genellikle istek, istek-yanıt desenleri MessageID içeren bir yanıt iletisi bir dize özelliği. | Evet |
| Kullanıcı Kimliği |İletileri kaynağını belirtmek için kullanılan bir kimliği. İletileri IOT Hub tarafından oluşturulduğunda ayarlanır `{iot hub name}`. | Hayır |
| iothub-ack |Geri bildirim iletisi Oluşturucusu. Bu özellik bulut-cihaz iletilerini cihaz tarafından sonucunda iletinin Tüketim geri bildirim iletileri oluşturmak için IOT hub'ı istemek için kullanılır. Olası değerler: **hiçbiri** (varsayılan): hiçbir geri bildirim iletisi oluşturulur, **pozitif**: ileti tamamlanmışsa, bir geri bildirim iletisi **negatif**: alma bir iletinin süresi (veya en yüksek teslimat sayısı ulaşıldı varsa) ve cihaz tarafından tamamlanmasını olmadan geri bildirim iletisi veya **tam**: pozitif ve negatif. <!-- robinsh For more information, see [Message feedback][lnk-feedback].--> | Evet |
| ıothub bağlantı cihaz kimliği |CİHAZDAN buluta iletileri IOT Hub tarafından ayarlanmış bir kimlik. İçerdiği **DeviceID** iletiyi gönderen cihazın. | D2C iletileri için Hayır '; Evet, aksi takdirde. |
| iothub-bağlantı-kimlik doğrulama-oluşturma kimliği |CİHAZDAN buluta iletileri IOT Hub tarafından ayarlanmış bir kimlik. İçerdiği **Generationıd** (olarak başına [cihaz kimlik özelliklerini](iot-hub-devguide-identity-registry.md#device-identity-properties)) iletiyi gönderen cihazın. | D2C iletileri için Hayır '; Evet, aksi takdirde. |
| ıothub bağlantı kimlik doğrulama yöntemi |CİHAZDAN buluta iletileri IOT Hub tarafından ayarlanmış bir kimlik doğrulama yöntemi. Bu özellik, iletinin gönderme cihazın kimliğini doğrulamak için kullanılan kimlik doğrulama yöntemi hakkında bilgi içerir. <!-- ROBINSH For more information, see [Device to cloud anti-spoofing][lnk-antispoofing].--> | D2C iletileri için Hayır '; Evet, aksi takdirde. |
| ıothub oluşturma zamanı utc | Tarih ve saat ileti bir cihazda oluşturuldu. Bir cihaz, bu değeri açıkça ayarlamanız gerekir. | Evet |

## <a name="message-size"></a>İleti boyutu

IOT Hub ileti boyutu, bir protokol belirsiz şekilde, yalnızca gerçek yükü dikkate ölçer. Bayt cinsinden boyut aşağıdakilerden toplamı hesaplanır:

* Gövde boyutu bayt cinsinden.
* İleti sistemi özelliklerinin değerlerinin bayt cinsinden boyutu.
* Tüm kullanıcı özellik adlarının ve değerlerinin bayt cinsinden boyutu.

Bayt cinsinden boyutu dize uzunluğu eşittir özellik adlarını ve değerlerini ASCII karakter ile sınırlıdır.

## <a name="anti-spoofing-properties"></a>Sahtekarlığına karşı koruma özellikleri

Tüm Damgalar cihaz IOT hub'ı, CİHAZDAN buluta iletileri kimlik sahtekarlığı önlemek için aşağıdaki özelliklere sahip iletileri:

* **ıothub bağlantı cihaz kimliği**
* **iothub-connection-auth-generation-id**
* **iothub-connection-auth-method**

İlk iki içeren **DeviceID** ve **Generationıd** kaynak cihazın olarak başına [cihaz kimlik özelliklerini](iot-hub-devguide-identity-registry.md#device-identity-properties).

**İothub-bağlantı-kimlik doğrulama-method** özelliği, aşağıdaki özelliklere sahip bir seri hale getirilmiş JSON nesnesi içerir:

```json
{
  "scope": "{ hub | device }",
  "type": "{ symkey | sas | x509 }",
  "issuer": "iothub"
}
```

## <a name="next-steps"></a>Sonraki adımlar

* IOT hub'ında ileti boyutu sınırları hakkında daha fazla bilgi için bkz: [IOT Hub kotaları ve azaltma](iot-hub-devguide-quotas-throttling.md).

* Oluşturun ve IOT hub'ı çeşitli programlama dillerinde iletileri okumak öğrenmek için bkz: [hızlı Başlangıçlar](quickstart-send-telemetry-node.md).
