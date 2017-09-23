---
title: "Azure Event Hubs nedir ve neden kullanılır | Microsoft Docs"
description: "Azure Event Hubs’a genel bakış ve giriş - Web sitesi, uygulama ve cihazlardan bulut ölçekli telemetri alma"
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: sethm; babanisa
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 1a6bf0a0352e6d9e3a22586ac825558d12e1307a
ms.contentlocale: tr-tr
ms.lasthandoff: 06/28/2017

---
# <a name="what-is-event-hubs"></a>Event Hubs nedir?

Azure Event Hubs saniyede milyonlarca olay alıp işleyebilen, ölçeklenebilirlik yüzeyi yüksek bir veri akışı platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Düşük gecikme ile yoğun ölçekte [yayımlama-abonelik özellikleri](https://msdn.microsoft.com/library/aa560414.aspx) sağlayabilen Event Hubs, Büyük Veri için "kestirme yol" işlevi görür.

## <a name="why-use-event-hubs"></a>Event Hubs’ı neden kullanmalıyım?

Event Hubs olay ve telemetri işleme özellikler onu özellikle aşağıdaki durumlar için yararlı hale getirir:

* Uygulama izleme
* Kullanıcı deneyimi veya iş akışı işleme
* Nesnelerin İnterneti (IoT) senaryoları

Örneğin, Event Hubs mobil uygulamalarda davranış izleme, web gruplarından trafik bilgileri alma, konsol oyunlarında oyun içi olay yakalama veya sanayi makinelerinden ya da bağlı taşıtlardan telemetri verileri toplama olanağı sağlar.

## <a name="azure-event-hubs-overview"></a>Azure Event Hubs’a genel bakış

Event Hubs’ın çözüm mimarilerinde oynadığı genel rol, bir olay ardışık düzeni için "ön kapı" olarak görev yapmalarıdır ve çoğunlukla *olay alıcı* olarak adlandırılırlar. Olay yutucu, bir olay akışının üretimini ilgili olayların kullanılmasına ayıran, olay yayımcıları ile olay tüketicileri arasında duran bir bileşen veya hizmettir. Aşağıdaki şekilde bu mimari gösterilir:

![Event Hubs](./media/event-hubs-what-is-event-hubs/event_hubs_full_pipeline.png)

Event Hubs, ileti akışı işleme olanağı sağlar ancak geleneksel kurumsal mesajlaşmadan farklı özelliklere sahiptir. Event Hubs özellikleri, yüksek işleme ve olay işleme senaryoları üzerine inşa edilmiştir. Bu nedenle Event Hubs, [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) mesajlaşmadan da farklıdır ve konu başlıkları gibi [Service Bus mesajlaşma](/azure/service-bus-messaging/) varlıkları için sunulan bazı özellikleri uygulamaz.

## <a name="event-hubs-features"></a>Event Hubs özellikleri

Event Hubs şu temel öğeleri içerir:

- [**Olay oluşturucuları/yayımcıları**](event-hubs-features.md#event-publishers): Bir olay hub'ına veri gönderen varlıktır. Olay AMQP 1.0 veya HTTPS üzerinden yayımlanır.
- [**Yakala**](event-hubs-features.md#capture): Event Hubs akış verilerini yakalamanıza ve Azure Blob depolama hesabında depolamanıza olanak tanır.
- [**Bölümler**](event-hubs-features.md#partitions): Her tüketicinin, olay akışının yalnızca belirli bir alt kümesini veya bölümünü okumasına olanak sağlar.
- [**SAS belirteçleri**](event-hubs-features.md#sas-tokens): Olay yayımcısını tanımlar ve yayımcı için kimlik doğrulama gerçekleştirir.
- [**Olay tüketicileri**](event-hubs-features.md#event-consumers): Bir olay hub'ından olay verilerini okuyan bir varlıktır. Olay tüketicileri AMQP 1.0 üzerinden bağlanır. 
- [**Tüketici grupları**](event-hubs-features.md#consumer-groups): Her çoklu tüketim uygulamasına, olay akışının ayrı bir görünümünü sağlar; böylece, söz konusu tüketicilerin bağımsız olarak işlem gerçekleştirmesine olanak sağlar.
- [**İşleme birimleri**](event-hubs-features.md#capacity): Önceden satın alınan kapasite birimleridir. Tek bir bölüm en fazla 1 işleme biriminden oluşan ölçeğe sahiptir.

Bu ve diğer Event Hubs özellikleriyle ilgili teknik bilgiler için bkz. [Event Hubs özelliklerine genel bakış](event-hubs-features.md). 

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs ayrıntılı fiyatlandırma bilgileri için bkz. [Event Hubs Fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/).

Event Hubs hakkında daha fazla bilgi için şu bağlantıları ziyaret edin:

* [Event Hubs öğreticisi](event-hubs-dotnet-standard-getstarted-send.md) ile çalışmaya başlayın
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [Event Hubs kullanan örnek uygulamalar](https://github.com/Azure/azure-event-hubs/tree/master/samples)
 
 


