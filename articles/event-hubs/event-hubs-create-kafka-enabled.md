---
title: Oluşturma Kafka etkin Azure Event Hubs | Microsoft Docs
description: Oluşturma bir Kafka, Azure portalını kullanarak Azure Event Hubs ad alanı etkin
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: dotnet
ms.topic: article
ms.date: 05/07/2018
ms.author: shvija
ms.openlocfilehash: 7a05a89fdf99efb6470ee9c8695f349cf22b8ebb
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37442136"
---
# <a name="create-kafka-enabled-event-hubs"></a>Oluşturma Kafka etkin olay hub'ları

Azure Event Hubs, büyük bir akış platformu olarak saniye başına milyonlarca olayı alır ve gerçek zamanlı analiz ve görselleştirme için düşük gecikme ve yüksek performans sağlayan bir hizmet (PaaS) veri.

Kafka ekosistemlerini için Azure Event Hubs ile bir uç nokta sağlar. Event Hubs ad alanınızın yerel olarak anlamak Bu uç noktayı etkinleştirir [Apache Kafka](https://kafka.apache.org/intro) iletisi protokolü ve API'ler. Bu özellik sayesinde ile Kafka konularını Protokolü istemcilerinize değiştirme veya kendi kümelerini çalıştırmak gibi event hubs ile iletişim kurabilir. Event Hubs için Kafka ekosistemlerini destekler [Apache Kafka sürümleri 1.0](https://kafka.apache.org/10/documentation.html) ve daha sonra.

Bu makalede bir Event Hubs ad alanı oluşturma ve Kafka özellikli bir olay hub'ları Kafka uygulamalarına bağlanmak için gerekli bağlantı dizesini alın.

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Oluşturma bir Kafka Event Hubs ad alanı etkin

1. Oturum [Azure portalında][Azure portal], tıklatıp **kaynak Oluştur** , ekranın sol üst köşesindeki.

2. Event Hubs için arama yapın ve burada gösterilen Seçenekler'i seçin:
    
    ![Event Hubs için portalda da arayabilirsiniz.](./media/event-hubs-create-kafka-enabled/event-hubs-create-event-hubs.png)
 
3. Benzersiz bir ad belirtin ve ad alanı üzerinde Kafka etkinleştirin. **Oluştur**’a tıklayın.
    
    ![Ad alanı oluşturma](./media/event-hubs-create-kafka-enabled/create-kafka-namespace.png)
 
4. Ad alanı, üzerinde oluşturulduktan sonra **ayarları** sekmesini tıklatın **paylaşılan erişim ilkeleri** bağlantı dizesini almak için.

    ![Paylaşılan erişim ilkeleri](./media/event-hubs-create/create-event-hub7.png)

5. Varsayılan seçebilirsiniz **RootManageSharedAccessKey**, ya da yeni bir ilke ekleyin. İlke adına tıklayın ve bağlantı dizesini kopyalayın. 
    
    ![Bir ilke seçin](./media/event-hubs-create/create-event-hub8.png)
 
6. Bu bağlantı dizesini Kafka uygulama yapılandırmanıza ekleyin.

Şimdi, Event Hubs'a Kafka protokolünü kullanan uygulamalarınızdan olayları akışını yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs hakkında daha fazla bilgi için şu bağlantıları ziyaret edin:

* [Kafka uygulamalarınızdan Event hubs'ta Stream](event-hubs-quickstart-kafka-enabled-event-hubs.md)
* [Kafka ekosistemi için Event Hubs hakkında bilgi edinin](event-hubs-for-kafka-ecosystem-overview.md)
* [Event Hubs hakkında bilgi edinin](event-hubs-what-is-event-hubs.md)


[Azure portal]: https://portal.azure.com/
