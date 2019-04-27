---
title: Apache Kafka etkin olay hub'ı - Azure Event Hubs oluşturma | Microsoft Docs
description: Bu makalede, Azure portalını kullanarak Azure Event Hubs ad alanı bir Apache Kafka oluşturmak için bir kılavuz etkin sağlar.
services: event-hubs
documentationcenter: .net
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.devlang: dotnet
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: bahariri
ms.openlocfilehash: 125da95349fce0e75b44b5619baba28d34a74be1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60822493"
---
# <a name="create-apache-kafka-enabled-event-hubs"></a>Apache Kafka etkin event hubs'ı oluşturma

Azure Event Hubs, büyük bir akış platformu olarak saniye başına milyonlarca olayı alır ve gerçek zamanlı analiz ve görselleştirme için düşük gecikme ve yüksek performans sağlayan bir hizmet (PaaS) veri.

Azure Event Hubs ile bir Kafka uç noktası sağlar. Event Hubs ad alanınızın yerel olarak anlamak Bu uç noktayı etkinleştirir [Apache Kafka](https://kafka.apache.org/intro) iletisi protokolü ve API'ler. Bu özellik sayesinde ile Kafka konularını Protokolü istemcilerinize değiştirme veya kendi kümelerini çalıştırmak gibi event hubs ile iletişim kurabilir. Event hubs'ı destekleyen [Apache Kafka sürümleri 1.0](https://kafka.apache.org/10/documentation.html) ve daha sonra.

Bu makalede bir Event Hubs ad alanı oluşturma ve Kafka özellikli bir olay hub'ları Kafka uygulamalarına bağlanmak için gerekli bağlantı dizesini alın.

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Kafka etkin Event Hubs ad alanı oluşturma

1. Oturum [Azure portalında][Azure portal], tıklatıp **kaynak Oluştur** , ekranın sol üst köşesindeki.

2. Event Hubs araması yapın ve burada gösterilen seçenekleri belirleyin:
    
    ![Portalda Event Hubs arama](./media/event-hubs-create-kafka-enabled/event-hubs-create-event-hubs.png)
 
3. Benzersiz bir ad belirtin ve ad alanında Kafka'yı etkinleştirin. **Oluştur**’a tıklayın.
    
    ![Ad alanı oluşturma](./media/event-hubs-create-kafka-enabled/create-kafka-namespace.jpg)
 
4. Ad alanı oluşturulduktan sonra **Ayarlar** sekmesinde **Paylaşılan erişim ilkeleri**'ne tıklayarak bağlantı dizesini alın.

    ![Paylaşılan erişim ilkeleri’ne tıklayın.](./media/event-hubs-create/create-event-hub7.png)

5. Varsayılan **RootManageSharedAccessKey** ilkesini seçebilir veya yeni bir ilke ekleyebilirsiniz. İlke adına tıklayın ve bağlantı dizesini kopyalayın. 
    
    ![İlke seçme](./media/event-hubs-create/create-event-hub8.png)
 
6. Bu bağlantı dizesini Kafka uygulaması yapılandırmanıza ekleyin.

Artık Kafka protokolünü kullanan uygulamalarınızdaki olayların akışını Event Hubs'a yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs hakkında daha fazla bilgi için şu bağlantıları ziyaret edin:

* [Kafka uygulamalarınızdan Event Hubs'a akış yapma](event-hubs-quickstart-kafka-enabled-event-hubs.md)
* [Kafka için Event Hubs hakkında bilgi edinin](event-hubs-for-kafka-ecosystem-overview.md)
* [Event Hubs hakkında bilgi edinin](event-hubs-what-is-event-hubs.md)


[Azure portal]: https://portal.azure.com/
