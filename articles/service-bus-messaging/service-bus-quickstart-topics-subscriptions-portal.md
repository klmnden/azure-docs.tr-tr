---
title: Hızlı Başlangıç - Azure portalını kullanarak Service Bus konuları ve abonelikleri oluşturun | Microsoft Docs
description: Bu hızlı başlangıçta, Azure portalını kullanarak bir Service Bus konusu ve bu konuya abonelik oluşturma işlemini öğrenin.
services: service-bus-messaging
author: spelluru
manager: timlt
ms.service: service-bus-messaging
ms.topic: quickstart
ms.date: 04/15/2019
ms.author: spelluru
ms.openlocfilehash: e943dbff71fdbd98773587c8ac5a62dd081b49a2
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64698848"
---
# <a name="quickstart-use-the-azure-portal-to-create-a-service-bus-topic-and-subscriptions-to-the-topic"></a>Hızlı Başlangıç: Bir Service Bus konusu ve konu için Abonelik oluşturmak için Azure portalını kullanma
Bu hızlı başlangıçta, Service Bus konusu oluşturun ve sonra bu konuya abonelik oluşturmak için Azure portalını kullanın. 

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Service Bus konuları ve abonelikleri nelerdir?
Service Bus konuları ve abonelikleri *publish/subscribe* mesajlaşma iletişim modelini destekler. Konular ve abonelikler kullanıldığında, dağıtılmış uygulamanın bileşenleri birbirleriyle doğrudan iletişim kurmazlar; bunun yerine bir aracı gibi davranan bir konu aracılığıyla iletileri değiş tokuş eder.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

Service Bus kuyrukları, her ileti tek bir tüketici tarafından işlenir kullanılmasının konuları ve abonelikleri bir yayımlama/abone olma modelini kullanarak iletişimin bir-çok biçimi sağlayın. Bir konuya birden fazla abonelik kaydedilebilir. Bir konuya ileti gönderildiğinde, bundan sonra, bağımsız olarak ele almak/işlemek amacıyla her abonelik için kullanılabilir hale getirilir. Bir konuya abone olunması, konuya gönderilmiş olan iletilerin kopyaların alan sanal kuyruğa benzer. İsteğe bağlı olarak, bir konu filtreleyin veya hangi konu aboneliklerinin bir konuya hangi mesajları kısıtlayabilirsiniz olanak tanıyan bir abonelik başına temelinde filtre kuralları da kaydedebilirsiniz.

Service Bus konuları ve Abonelikleri, çok sayıda kullanıcılar ve uygulamalar arasında çok sayıda iletileri işlemek üzere ölçeği sağlar.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-create-topics-three-subscriptions-portal](../../includes/service-bus-create-topics-three-subscriptions-portal.md)]



## <a name="next-steps"></a>Sonraki adımlar
Bir abonelik aracılığıyla bu iletileri alıp bir konuya ileti göndermek nasıl öğrenmek için şu makaleye bakın: içindekiler tablosunda programlama dilini seçin. 

> [!div class="nextstepaction"]
> [Yayımla ve abone ol iletileri](service-bus-dotnet-how-to-use-topics-subscriptions.md)
