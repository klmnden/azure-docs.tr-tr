---
title: Özel uç noktalar Azure IOT hub'ı Anlama | Microsoft Docs
description: Geliştirici Kılavuzu - özel uç noktalar için CİHAZDAN buluta iletileri yönlendirmek için yönlendirme sorgularını kullanarak.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/09/2018
ms.author: dobett
ms.openlocfilehash: af0b819c6c60835089c174a1f9f7c3a6215e362c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956977"
---
# <a name="use-message-routes-and-custom-endpoints-for-device-to-cloud-messages"></a>CİHAZDAN buluta iletiler için ileti yollarını ve özel uç noktaları kullanma

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

IOT hub'ı [ileti yönlendirme](iot-hub-devguide-routing-query-syntax.md) hizmet'e yönelik uç noktalar için CİHAZDAN buluta iletileri yönlendirmek kullanıcıların sağlar. Yönlendirme, bir sorgulama uç noktalar için yönlendirme önce verileri filtreleme yeteneği sağlar. Yapılandırdığınız her bir yönlendirme sorgu aşağıdaki özelliklere sahiptir:

| Özellik      | Açıklama |
| ------------- | ----------- |
| **Ad**      | Sorguyu tanımlayan benzersiz adı. |
| **Kaynak**    | Yapılması gereken veri akışı kaynağı. Örneğin, cihaz telemetrisi. |
| **Koşul** | İleti uygulama özellikleri, Sistem özellikleri, ileti gövdesi, cihaz ikizi etiketleri ve uç nokta için bir eşleşme olup olmadığını belirlemek için cihaz ikizi özelliklerini karşı çalışan yönlendirme sorgu için sorgu ifadesi. Bir sorgu oluşturma hakkında daha fazla bilgi için bkz [ileti yönlendirme sorgusu söz dizimi](iot-hub-devguide-routing-query-syntax.md) |
| **Uç noktası**  | IOT Hub sorguyla eşleşen iletileri göndereceği yeri uç nokta adı. IOT hub'ınız ile aynı bölgede bir uç nokta seçmenizi öneririz. |

Tek bir ileti birden çok yönlendirme sorgular, IOT Hub durumu ileti eşleşen her sorgu ile ilişkili uç sunar, koşula eşleşmiyor olabilir. IOT hub'ı da otomatik olarak ileti teslimini deduplicates bir ileti aynı hedefe sahip birden çok sorgu eşleşirse, yalnızca bir kez bu hedefe yazılmış olduğu için.

## <a name="endpoints-and-routing"></a>Uç noktalar ve yönlendirme

IOT hub'ı varsayılan sahip [yerleşik uç nokta][lnk-built-in]. Hub'ına aboneliğinizde diğer hizmetler arasında bağlantı kurularak iletileri yönlendirmek için özel uç noktaları oluşturabilirsiniz. IOT hub'ı Azure depolama kapsayıcıları, Event Hubs, Service Bus kuyrukları ve Service Bus konu başlıklarını şu anda özel uç noktalar destekler.

Yönlendirme ve özel uç noktaları kullandığınızda, herhangi bir sorgu eşleşmiyorsa iletileri yalnızca yerleşik uç noktasına gönderilir. Yerleşik uç noktasına da özel bir uç noktası için farklı iletileri ulaştırmak üzere, iletileri gönderen bir rota ekleyin **olayları** uç noktası.

> [!NOTE]
> IOT hub'ı yalnızca verileri Azure depolama kapsayıcıları bloblar için yazma destekler.

> [!WARNING]
> Service Bus kuyrukları ve konuları ile **oturumları** veya **yinelenen algılama** etkin özel uç noktalar desteklenmez.

IOT Hub'ında özel uç noktalar oluşturma hakkında daha fazla bilgi için bkz. [IOT Hub uç noktaları][lnk-devguide-endpoints].

Özel uç noktalar okuma hakkında daha fazla bilgi için bkz:

* Okuma [Azure depolama kapsayıcıları][lnk-getstarted-storage].
* Okuma [olay hub'ları][lnk-getstarted-eh].
* Okuma [Service Bus kuyruklarını][lnk-getstarted-queue].
* Okuma [Service Bus konu başlıklarını][lnk-getstarted-topic].

### <a name="next-steps"></a>Sonraki adımlar

* IOT Hub uç noktaları hakkında daha fazla bilgi için bkz: [IOT Hub uç noktaları][lnk-devguide-endpoints].
* Yönlendirme sorguları tanımlamak için kullandığınız sorgu dili hakkında daha fazla bilgi için bkz. [ileti yönlendirme sorgusu söz dizimi](iot-hub-devguide-routing-query-syntax.md).
* [Yolları kullanarak işlem IOT Hub CİHAZDAN buluta iletileri] [ lnk-d2c-tutorial] Öğreticisi, yönlendirme sorgular ve özel uç noktalar nasıl kullanılacağını gösterir.

[lnk-built-in]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-to-cloud]: iot-hub-devguide-messages-d2c.md
[lnk-devguide-query-language]: iot-hub-devguide-query-language.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-d2c-tutorial]: tutorial-routing.md
[lnk-getstarted-eh]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-getstarted-queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[lnk-getstarted-topic]: ../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md
[lnk-getstarted-storage]: ../storage/blobs/storage-blobs-introduction.md
