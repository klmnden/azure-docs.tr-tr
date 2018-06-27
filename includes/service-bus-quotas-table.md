---
title: include dosyası
description: include dosyası
services: service-bus-messaging
author: sethmanheim
ms.service: service-bus-messaging
ms.topic: include
ms.date: 06/04/2018
ms.author: sethm
ms.custom: include file
ms.openlocfilehash: 0ff1e31e52c7db5c41f92cb9e4cb1a17f28dea6f
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34806409"
---
Aşağıdaki tabloda Service Bus Mesajlaşma hizmeti için özel kota bilgileri listeler. Fiyatlandırma hakkında daha fazla bilgi ve diğer Service Bus kotaları için bkz [Service Bus fiyatlandırma](https://azure.microsoft.com/pricing/details/service-bus/) genel bakış.

| Kota adı | Kapsam | Notlar | Değer |
| --- | --- | --- | --- | --- |
| Temel / standart ad alanları Azure abonelik başına en fazla sayısı |Ad Alanı |Sonraki istekleri ek temel / standart ad alanları için portal tarafından reddedilir. |100|
| Premium ad Azure abonelik başına en fazla sayısı |Ad Alanı |Sonraki istekleri ek premium ad alanları için portal tarafından reddedilir. |10 |
| Sıra/konu boyutu |Varlık |Sıra/konu oluşturma sırasında tanımlanır. <br/><br/> Sonraki gelen iletileri reddedilir ve arama kodun bir özel durum aldı. |1, 2, 3, 4 GB ya da 5 GB.<br /><br />Standart yanı sıra Premium SKU, [bölümleme](/azure/service-bus-messaging/service-bus-partitioning) en büyük sıra/konu başlığı boyutu 80 GB etkinleştirilmiştir. |
| Bir ad alanında eşzamanlı bağlantı sayısı |Ad Alanı |Sonraki istekleri için ek bağlantıları reddedilir ve arama kodun bir özel durum aldı. REST işlemlerini eşzamanlı TCP bağlantılarını doğru sayılmaz. |NetMessaging: 1.000<br /><br />AMQP: 5.000 |
| Bir konu/kuyruk/aboneliği varlık isteklerinde alma eşzamanlı sayısı |Varlık |Sonraki alma isteği reddedilir ve arama kodun bir özel durum aldı. Bu kota birleşik uygulanır eşzamanlı sayısı alma işlemlerinin bir konuyla ilgili tüm abonelikleri arasında. |5.000 |
| Ad alanı başına konuları/sıraların sayısı |Ad Alanı |Yeni konu veya ad alanında kuyruk oluşturma sonraki istekler reddedilir. Aracılığıyla yapılandırdıysanız sonuç olarak, [Azure portal][Azure portal], bir hata iletisi oluşturulur. Yönetim API'si çağırdıysanız, bir özel durum çağrıyı yapan kod tarafından alınır. |10,000<br /><br />Konular artı bir ad alanı kuyruklarda toplam sayısı 10. 000'e eşit veya daha az olmalıdır. |
| Sayısı [bölümlenmiş konuları/sıraları](/azure/service-bus-messaging/service-bus-partitioning) başına ad alanı |Ad Alanı |Yeni bölümlenmiş konu veya ad alanında kuyruk oluşturma sonraki istekler reddedilir. Aracılığıyla yapılandırdıysanız sonuç olarak, [Azure portal][Azure portal], bir hata iletisi oluşturulur. Yönetim API'si, çağrıldıklarında bir **QuotaExceededException** çağıran kodla özel durum aldı. |Temel ve standart katmanları - 100<br/><br/>Bölümlenen varlıklar desteklenmiyor [Premium](../articles/service-bus-messaging/service-bus-premium-messaging.md) katmanı.<br/><br />Ad alanı başına 10.000 varlık kota doğrultusunda her bölümlenmiş kuyruk veya konu sayar. |
| Her Mesajlaşma varlığı yolunun en büyük boyut: kuyruk veya konu başlığı |Varlık |- |260 karakter |
| Her Mesajlaşma varlığı adının en büyük boyutu: ad alanı, abonelik veya abonelik kuralı |Varlık |- |50 karakter |
| Bir iletinin en büyük boyutunu [SessionID](/dotnet/api/microsoft.azure.servicebus.message.sessionid) | Varlık |- | 128 |
| Bir konu/kuyruk/aboneliği varlık için ileti boyutu |Varlık |Bu kotalar aşan gelen iletileri reddedilir ve arama kodun bir özel durum aldı. |Maksimum ileti boyutu: 256 KB ([standart katmanı](../articles/service-bus-messaging/service-bus-premium-messaging.md)) / 1 MB ([Premium katmanı](../articles/service-bus-messaging/service-bus-premium-messaging.md)). <br /><br />Sistem yükü nedeniyle, bu sınır bu değerleri küçüktür.<br /><br />En fazla üstbilgi boyutu: 64 KB<br /><br />Özellik paketi üstbilgi özelliklerinde sayısı: **bayt/int MaxValue**<br /><br />Özellik paketi özellik üst sınırı: açık bir sınırlama yoktur. En fazla üstbilgi boyutu sınırlıdır. |
| Bir konu/kuyruk/aboneliği varlık için ileti özelliği boyutu |Varlık |A **SerializationException** özel durum oluşturulur. |En fazla ileti özelliği her bir özellik için tüm özellikleri 32 k toplam boyutu 64 k aşamaz boyutudur Bu sınır için tüm üstbilgisi geçerli [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage), her ikisi de olan kullanıcı özelliklerinin yanı sıra Sistem Özellikleri (gibi [SequenceNumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sequencenumber), [etiket](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label), [ MessageID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid), vb.). |
| Konu başına abonelik sayısı |Varlık |Konu için ek abonelik oluşturmak için sonraki istekler reddedilir. Sonuç olarak, portal yapılandırdıysanız, bir hata iletisi gösterilir. Yönetimi API'SİNDEN çağrıldıklarında bir özel durum çağrıyı yapan kod tarafından alınır. |2,000 |
| Konu başına SQL filtresi sayısı |Varlık |Oluşturma konusunda ek filtreler, sonraki istekler reddedilir ve arama kodun bir özel durum aldı. |2,000 |
| Konu başına bağıntı filtresi sayısı |Varlık |Oluşturma konusunda ek filtreler, sonraki istekler reddedilir ve arama kodun bir özel durum aldı. |100,000 |
| SQL filtreleri/Eylemler boyutu |Ad Alanı |Ek filtreler oluşturulması için sonraki istekleri reddedilir ve arama kodun bir özel durum aldı. |Filtre koşulu dizenin en fazla uzunluğu: 1024 (1 K).<br /><br />Kural eylemi dizenin en fazla uzunluğu: 1024 (1 K).<br /><br />İfadeler kural eylemi başına en fazla: 32. |
| Sayısı [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) her ad alanı, kuyruk veya konu kuralları |Varlık, ad alanı |Ek kuralları oluşturulması için sonraki istekleri reddedilir ve arama kodun bir özel durum aldı. |En fazla kural sayısı: 12. <br /><br /> Tüm Kuyruklar ve konular bu ad alanında bir hizmet veri yolu ad alanı üzerinde yapılandırılmış olan kurallar uygulanır. |
| İşlem başına ileti sayısı | İşlem | Ek gelen iletileri reddedilir ve belirten bir özel durum "100'den fazla ileti tek bir işlemde gönderemez" çağrıyı yapan kod tarafından alınır. | 100 <br /><br /> Her ikisi için de **Send()** ve **SendAsync()** işlemleri. |

[Azure portal]: https://portal.azure.com