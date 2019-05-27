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
ms.openlocfilehash: 38f7dd6eb1c4965eca003e5ba337ec5912a53420
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66148229"
---
Aşağıdaki tabloda için belirli sınırları ve kotalar listeler [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Event Hubs fiyatlandırması hakkında daha fazla bilgi için bkz: [Event Hubs fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/).

| Sınır | `Scope` | Notlar | Değer |
| --- | --- | --- | --- |
| Event Hubs ad alanlarını başına abonelik sayısı |Abonelik |- |100 |
| Event hubs ad alanı başına sayısı |Ad alanı |Yeni bir olay hub'ı oluşturmak için sonraki istekler reddedilir. |10 |
| Olay hub'ı başına bölüm sayısı |Varlık |- |32 |
| Her olay hub'ı tüketici grubu sayısı |Varlık |- |20 |
| AMQP bağlantıları ad alanı başına sayısı |Ad alanı |Bir ek bağlantı için sonraki istekler reddedilir ve bir özel durum çağıran kod tarafından alınır. |5,000 |
| Event Hubs olay üst sınırı|Varlık |- |1 MB |
| En büyük boyutu bir olay hub'ı adı |Varlık |- |50 karakteri |
| Dönem olmayan alıcılar bir tüketici grubu başına sayısı |Varlık |- |5 |
| Olay verilerinin en uzun bekletme süresi |Varlık |- |1-7 gün |
| En fazla üretilen iş birimi |Ad alanı |Verilerinizi kısıtlanabilir neden olur ve oluşturur aktarım hızı birimi sınırını aşan bir [sunucu meşgul özel durumu](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception). Çok sayıda üretilen iş birimleri bir standart katman için istemek için dosya bir [destek isteği](/azure/azure-supportability/how-to-create-azure-support-request). [Ek üretilen iş birimleri](../articles/event-hubs/event-hubs-auto-inflate.md) taahhüt edilen satın almaya başına 20 bloklar halinde kullanılabilir. |20 |
| Ad alanı başına yetkilendirme kuralları sayısı |Ad alanı|Yetkilendirme kuralı oluşturma sonraki istekler reddedilir.|12 |
| Getruntimeınformation metodu yapılan çağrı sayısı | Varlık | - | saniye başına 50 | 
