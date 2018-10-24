---
title: include dosyası
description: include dosyası
services: event-hubs
author: sethmanheim
ms.service: event-hubs
ms.topic: include
ms.date: 02/26/2018
ms.author: sethm
ms.custom: include file
ms.openlocfilehash: a49ce4e997a21e0db707c851f5ea46817bcb642e
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49960207"
---
Aşağıdaki tabloda için belirli sınırları ve kotalar listeler [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Event Hubs fiyatlandırması hakkında daha fazla bilgi için bkz: [Event Hubs fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/).

| Sınır | Kapsam | Notlar | Değer |
| --- | --- | --- | --- | --- |
| Event Hubs ad alanlarını başına abonelik sayısı |Abonelik |- |1000 |
| Event hubs ad alanı başına sayısı |Ad Alanı |Yeni bir olay hub'ı oluşturmak için sonraki istekler reddedilir. |10 |
| Olay hub'ı başına bölüm sayısı |Varlık |- |32 |
| Her olay hub'ı tüketici grubu sayısı |Varlık |- |20 |
| AMQP bağlantıları ad alanı başına sayısı |Ad Alanı |Bir ek bağlantı için sonraki istekler reddedilir ve bir özel durum çağıran kod tarafından alınır. |5.000 |
| Event Hubs olay üst sınırı|Varlık |- |256 KB |
| En büyük boyutu bir olay hub'ı adı |Varlık |- |50 karakteri |
| Dönem olmayan alıcılar bir tüketici grubu başına sayısı |Varlık |- |5 |
| Olay verilerinin en uzun bekletme süresi |Varlık |- |1-7 gün |
| En fazla üretilen iş birimi |Ad Alanı |Verilerinizi kısıtlanabilir neden olur ve oluşturur aktarım hızı birimi sınırını aşan bir  **[ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception)**. Çok sayıda üretilen iş birimleri için standart bir talep edebilir dosyalama katmanın bir [destek isteği](/azure/azure-supportability/how-to-create-azure-support-request). [Ek üretilen iş birimleri](../articles/event-hubs/event-hubs-auto-inflate.md) taahhüt edilen satın almaya başına 20 bloklar halinde kullanılabilir. |20 |
| Ad alanı başına yetkilendirme kuralları sayısı |Ad Alanı|Yetkilendirme kuralı oluşturma sonraki istekler reddedilir.|12 |
