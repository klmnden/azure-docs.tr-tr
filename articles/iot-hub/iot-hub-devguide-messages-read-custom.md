---
title: Özel uç noktalar Azure IOT hub'ı Anlama | Microsoft Docs
description: Geliştirici Kılavuzu - özel uç noktalar için CİHAZDAN buluta iletileri yönlendirmek için yönlendirme sorgularını kullanarak.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/09/2018
ms.openlocfilehash: e5e92c40cef15e99431dc9652820c71e87935f67
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61244353"
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

IOT hub'ı varsayılan sahip [yerleşik uç nokta](iot-hub-devguide-messages-read-builtin.md). Hub'ına aboneliğinizde diğer hizmetler arasında bağlantı kurularak iletileri yönlendirmek için özel uç noktaları oluşturabilirsiniz. IOT hub'ı Azure depolama kapsayıcıları, Event Hubs, Service Bus kuyrukları ve Service Bus konu başlıklarını şu anda özel uç noktalar destekler.

Yönlendirme ve özel uç noktaları kullandığınızda, herhangi bir sorgu eşleşmiyorsa iletileri yalnızca yerleşik uç noktasına gönderilir. Yerleşik uç noktasına da özel bir uç noktası için farklı iletileri sunmak için yerleşik olarak iletiler gönderen bir yol ekleyin. **olayları** uç noktası.

> [!NOTE]
> * IOT hub'ı yalnızca verileri Azure depolama kapsayıcıları bloblar için yazma destekler.
> * Service Bus kuyrukları ve konuları ile **oturumları** veya **yinelenen algılama** etkin özel uç noktalar desteklenmez.

IOT Hub'ında özel uç noktalar oluşturma hakkında daha fazla bilgi için bkz. [IOT Hub uç noktaları](iot-hub-devguide-endpoints.md).

Özel uç noktalar okuma hakkında daha fazla bilgi için bkz:

* Okuma [Azure depolama kapsayıcıları](../storage/blobs/storage-blobs-introduction.md).

* Okuma [olay hub'ları](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).

* Okuma [Service Bus kuyruklarını](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).

* Okuma [Service Bus konu başlıklarını](../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md).

## <a name="next-steps"></a>Sonraki adımlar

* IOT Hub uç noktaları hakkında daha fazla bilgi için bkz: [IOT Hub uç noktaları](iot-hub-devguide-endpoints.md).

* Yönlendirme sorguları tanımlamak için kullandığınız sorgu dili hakkında daha fazla bilgi için bkz. [ileti yönlendirme sorgusu söz dizimi](iot-hub-devguide-routing-query-syntax.md).

* [Yolları kullanarak işlem IOT Hub CİHAZDAN buluta iletileri](tutorial-routing.md) Öğreticisi, yönlendirme sorgular ve özel uç noktalar nasıl kullanılacağını gösterir.