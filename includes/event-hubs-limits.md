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
ms.openlocfilehash: cd64bdabc2b7b34687296c855c27882925d80f63
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58124429"
---
Aşağıdaki tabloda için belirli sınırları ve kotalar listeler [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Event Hubs fiyatlandırması hakkında daha fazla bilgi için bkz: [Event Hubs fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/).

| Sınır | Kapsam | Notes | Değer |
| --- | --- | --- | --- |
| Event Hubs ad alanlarını başına abonelik sayısı |Abonelik |- |1000 |
| Event hubs ad alanı başına sayısı |İsim uzayı |Yeni bir olay hub'ı oluşturmak için sonraki istekler reddedilir. |10 |
| Olay hub'ı başına bölüm sayısı |Varlık |- |32 |
| Her olay hub'ı tüketici grubu sayısı |Varlık |- |20 |
| AMQP bağlantıları ad alanı başına sayısı |İsim uzayı |Bir ek bağlantı için sonraki istekler reddedilir ve bir özel durum çağıran kod tarafından alınır. |5,000 |
| Event Hubs olay üst sınırı|Varlık |- |1 MB |
| En büyük boyutu bir olay hub'ı adı |Varlık |- |50 karakteri |
| Dönem olmayan alıcılar bir tüketici grubu başına sayısı |Varlık |- |5 |
| Olay verilerinin en uzun bekletme süresi |Varlık |- |1-7 gün |
| En fazla üretilen iş birimi |İsim uzayı |Verilerinizi kısıtlanabilir neden olur ve oluşturur aktarım hızı birimi sınırını aşan bir [sunucu meşgul özel durumu](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception). Çok sayıda üretilen iş birimleri bir standart katman için istemek için dosya bir [destek isteği](/azure/azure-supportability/how-to-create-azure-support-request). [Ek üretilen iş birimleri](../articles/event-hubs/event-hubs-auto-inflate.md) taahhüt edilen satın almaya başına 20 bloklar halinde kullanılabilir. |20 |
| Ad alanı başına yetkilendirme kuralları sayısı |İsim uzayı|Yetkilendirme kuralı oluşturma sonraki istekler reddedilir.|12 |
