---
title: Oluşturma Kafka etkin Azure Event Hubs | Microsoft Docs
description: Oluşturma bir Kafka Azure portalını kullanarak Azure Event Hubs ad etkin
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: dotnet
ms.topic: article
ms.date: 05/07/2018
ms.author: shvija
ms.openlocfilehash: 4f1d21be3c19dfbc764485fea47b6d4cb2171b3c
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33942553"
---
# <a name="create-kafka-enabled-event-hubs"></a>Oluşturma Kafka etkin olay hub'ları

Azure Event Hubs, saniye başına milyonlarca olayı alır ve gerçek zamanlı analiz ve görselleştirme için düşük gecikme süresi ve yüksek verimlilik sağlar (PaaS) hizmet olarak Platform akış büyük bir veri ' dir.

Azure Event Hubs Kafka ekosistemlerini için bir uç nokta sağlar. Bu uç noktaya yerel olarak anlamak, olay hub'ları ad alanı sağlayan [Apache Kafka](https://kafka.apache.org/intro) ileti protokolü ve API'leri. Bu özellik ile Protokolü istemcileriniz değiştirme veya kendi kümeleri çalıştıran Kafka konularda olduğu gibi Event Hubs ile iletişim kurabilir. Event Hubs Kafka ekosistemlerini desteklediği için [Apache Kafka sürümleri 1.0](https://kafka.apache.org/10/documentation.html) ve daha sonra.

Bu makalede bir olay hub'ları ad alanı oluşturma ve bağlantı alma dizesi Kafka Kafka uygulamalara bağlanmak için gereken etkin olay hub'ları.

## <a name="prerequisites"></a>Önkoşullar

Bir Azure aboneliğiniz yoksa oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) başlamadan önce.

## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Oluşturma olay hub'ları ad alanı bir Kafka etkin

1. Oturum [Azure portal][Azure portal], tıklatıp **kaynak oluşturma** en üst ekranın sol.

2. Event Hubs için arama ve burada gösterilen seçenekleri seçin:
    
    ![Olay hub'ları için Portalı'nda arama](./media/event-hubs-create-kafka-enabled/event-hubs-create-event-hubs.png)
 
3. **Ad alanı oluşturma**, göre benzersiz bir ad sağlamak ve ad Kafka etkinleştirin. **Oluştur**’a tıklayın.
    
    ![Ad alanı oluşturma](./media/event-hubs-create-kafka-enabled/create-kafka-namespace.png)
 
4. Ad alanı, üzerinde oluşturulduktan sonra **ayarları** sekmesini tıklatın, **paylaşılan erişim ilkeleri** bağlantı dizesini almak için.

    ![Paylaşılan Erişim İlkeleri'ni tıklatın](./media/event-hubs-create/create-event-hub7.png)

5. Varsayılan seçebilirsiniz **RootManageSharedAccessKey**, veya yeni bir ilke ekleme. İlke adını ve bağlantı dizesini kopyalayın. 
    
    ![Bir ilke seçin](./media/event-hubs-create/create-event-hub8.png)
 
6. Bu bağlantı dizesi Kafka uygulama yapılandırmanıza ekleyebilirsiniz.

Şimdi, Event Hubs'a Kafka protokolünü kullanan, uygulamalardan olaylarını akışını sağlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs hakkında daha fazla bilgi için aşağıdaki bağlantıları ziyaret edin:

* [Olay hub'ları bir akışa, Kafka uygulamalardan](event-hubs-quickstart-kafka-enabled-event-hubs.md)
* [Kafka ekosistemi için olay hub'ları hakkında bilgi edinin](event-hubs-for-kafka-ecosystem-overview.md)
* [Olay hub'ları hakkında bilgi edinin](event-hubs-what-is-event-hubs.md)


[Azure portal]: https://portal.azure.com/
