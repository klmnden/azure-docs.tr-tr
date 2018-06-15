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
ms.openlocfilehash: ab4c5b98ed9f6fcc8c271797db2d81dcc7ec4449
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
ms.locfileid: "29717039"
---
Aşağıdaki tabloda kotaları listeler ve özel sınırlar [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Event Hubs fiyatlandırması hakkında daha fazla bilgi için bkz: [Event Hubs fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/).

| Sınır | Kapsam | Notlar | Değer |
| --- | --- | --- | --- | --- |
| Olay hub'ad alanı başına sayısı |Ad Alanı |Yeni bir olay hub'ı oluşturulması için sonraki istekleri kabul edilmeyecek. |10 |
| Olay hub'ı başına bölüm sayısı |Varlık |- |32 |
| Olay hub'ı her tüketici grupları sayısı |Varlık |- |20 |
| Ad alanı başına AMQP bağlantı sayısı |Ad Alanı |Sonraki istekleri için ek bağlantıları reddedilir ve arama kodun bir özel durum aldı. |5.000 |
| Event Hubs olay en büyük boyutu|Varlık |- |256 KB |
| Bir olay hub'ı adının en büyük boyutu |Varlık |- |50 karakter |
| Dönem olmayan alıcılar bir tüketici grubu başına sayısı |Varlık |- |5 |
| Olay verilerini maksimum bekleme süresi |Varlık |- |1-7 gün |
| En fazla üretilen iş birimi |Ad Alanı |Verilerinizi kısıtlanan neden olur ve oluşturur işleme birimi sınırını aşan bir  **[ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception)**. Üretilen iş birimleri için standart bir daha çok sayıda istek katmanı dosyalama tarafından bir [destek isteği](/azure/azure-supportability/how-to-create-azure-support-request). [Ek üretilen iş birimleri](../articles/event-hubs/event-hubs-auto-inflate.md) 20 taahhüt satın alma temelinde bloklarını mevcuttur. |20 |
| Ad alanı başına yetkilendirme kuralı sayısı |Ad Alanı|Yetkilendirme kuralı oluşturma sonraki istekleri kabul edilmeyecek.|12 |
