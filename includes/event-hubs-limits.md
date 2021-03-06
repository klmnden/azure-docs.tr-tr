---
title: include dosyası
description: include dosyası
services: event-hubs
author: sethmanheim
ms.service: event-hubs
ms.topic: include
ms.date: 05/22/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: b105fb14608d53c5c2ef469ab44e211ccdf4d3c8
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67812496"
---
Aşağıdaki tabloda için belirli sınırları ve kotalar listeler [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Event Hubs fiyatlandırması hakkında daha fazla bilgi için bkz: [Event Hubs fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/).

| Sınır | `Scope` | Notlar | Value |
| --- | --- | --- | --- |
| Event Hubs ad alanlarını başına abonelik sayısı |Subscription |- |100 |
| Event hubs ad alanı başına sayısı |Ad Alanı |Yeni bir olay hub'ı oluşturmak için sonraki istekler reddedilir. |10 |
| Olay hub'ı başına bölüm sayısı |Varlık |- |32 |
| Her olay hub'ı tüketici grubu sayısı |Varlık |- |20 |
| AMQP bağlantıları ad alanı başına sayısı |Ad Alanı |Bir ek bağlantı için sonraki istekler reddedilir ve bir özel durum çağıran kod tarafından alınır. |5,000 |
| Event Hubs olay üst sınırı|Varlık |- |1 MB |
| En büyük boyutu bir olay hub'ı adı |Varlık |- |50 karakteri |
| Dönem olmayan alıcılar bir tüketici grubu başına sayısı |Varlık |- |5 |
| Olay verilerinin en uzun bekletme süresi |Varlık |- |1-7 gün |
| En fazla üretilen iş birimi |Ad Alanı |Verilerinizi kısıtlanabilir neden olur ve oluşturur aktarım hızı birimi sınırını aşan bir [sunucu meşgul özel durumu](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception). Çok sayıda üretilen iş birimleri bir standart katman için istemek için dosya bir [destek isteği](/azure/azure-supportability/how-to-create-azure-support-request). [Ek üretilen iş birimleri](../articles/event-hubs/event-hubs-auto-inflate.md) taahhüt edilen satın almaya başına 20 bloklar halinde kullanılabilir. |20 |
| Ad alanı başına yetkilendirme kuralları sayısı |Ad Alanı|Yetkilendirme kuralı oluşturma sonraki istekler reddedilir.|12 |
| Getruntimeınformation metodu yapılan çağrı sayısı | Varlık | - | saniye başına 50 | 
| Sanal ağ (VNet) ve IP yapılandırma kuralları sayısı | Varlık | - | 128 | 

### <a name="event-hubs-dedicated---quotas-and-limits"></a>Event Hubs Dedicated - kotalar ve sınırlar
Event Hubs Dedicated teklifi, bir aylık fiyatla, en az 4 saatlik kullanım ücreti alınır. Adanmış katmanı, zorlu iş yüklerine sahip müşteriler için tüm özellikleri standart plan, ancak Kurumsal ölçek kapasitesi ve sınırları ile sunar. 

| Özellik | Sınırlar |
| --- | ---|
| Bant genişliği |  20 cu |
| Ad Alanları | 50 CU başına |
| Event Hubs |  ad alanı başına 1000 |
| Giriş olayları | Dahil |
| İleti Boyutu | 1 milyon bayt |
| Bölümler | CU başına 2000 |
| Tüketici grupları | Sınır, CU başına 1000 olay hub'ı başına |
| Aracılı bağlantılar | 100 bin adet dahil |
| İleti Saklama | 7 gün (çok yakında 90 gün saklama) yedekleme CU 10 TB dahil |
| Capture | Dahil |
