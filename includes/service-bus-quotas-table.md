---
title: include dosyası
description: include dosyası
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 08/29/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 1116d6037a149b379bd96d6b208ca306a38c7a03
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2018
ms.locfileid: "52288704"
---
Aşağıdaki tablo, Service Bus mesajlaşması için belirli bir kota bilgileri listeler. Fiyatlandırma hakkında daha fazla bilgi ve diğer Service Bus kotaları için bkz. [hizmet veri yolu fiyatlandırması](https://azure.microsoft.com/pricing/details/service-bus/) genel bakış.

| Kota adı | Kapsam | Notlar | Değer |
| --- | --- | --- | --- | --- |
| Azure aboneliği başına temel / standart ad alanı sayısı |Ad Alanı |Sonraki istekler ek temel / standart ad alanları için portal tarafından reddedilir. |100|
| Azure aboneliği başına premium ad alanı sayısı |Ad Alanı |Sonraki istekler ek premium ad alanları için portal tarafından reddedilir. |10 |
| Kuyruk/konu boyutu |Varlık |Kuyruk/konu oluşturulduktan sonra tanımlanır. <br/><br/> Sonraki gelen iletileri reddedilir ve bir özel durum çağıran kod tarafından alınır. |1, 2, 3, 4 GB veya 5 GB.<br /><br />Standart yanı sıra Premium SKU, [bölümleme](/azure/service-bus-messaging/service-bus-partitioning) maksimum kuyruk/konu boyutu 80 GB etkinleştirilmiştir. |
| Bir ad alanında eşzamanlı bağlantı sayısı |Ad Alanı |Bir ek bağlantı için sonraki istekler reddedilir ve bir özel durum çağıran kod tarafından alınır. REST işlemlerini eş zamanlı TCP bağlantısı sayılmaz. |NetMessaging: 1.000<br /><br />AMQP: 5.000 |
| Eşzamanlı sayısı istekler kuyruk/konu/abonelik varlığı alır |Varlık |Sonraki alma istekleri reddedilir ve bir özel durum çağıran kod tarafından alınır. Bu birleşik kotanız eşzamanlı sayısı alma işlemlerinin bir konuya tüm abonelikler arasında. |5.000 |
| Konuları/kuyruklar ad alanı başına sayısı |Ad Alanı |Yeni bir konu veya ad alanında kuyruk oluşturma sonraki istekler reddedilir. İle yapılandırılmış bir sonucu olarak [Azure portalında][Azure portal], bir hata iletisi oluşturulur. Adlı yönetim API'si, bir özel durum çağıran kod tarafından alınır. |1000 (temel/standart katman). Bir ad alanındaki kuyrukları ve konuları sayısı 1000'e eşit veya daha az olmalıdır. <br/><br/>Premium katmanı için Mesajlaşma unit(MU) başına 1000. Üst sınırı 4000'dir. |
| Sayısı [bölümlenmiş konuları/kuyruklar](/azure/service-bus-messaging/service-bus-partitioning) ad alanı başına |Ad Alanı |Yeni bir bölümlenmiş konu veya ad alanında kuyruk oluşturma sonraki istekler reddedilir. İle yapılandırılmış bir sonucu olarak [Azure portalında][Azure portal], bir hata iletisi oluşturulur. Yönetim API'si, çağrılırsa bir **QuotaExceededException** özel durumu çağıran kod tarafından alındı. |Temel ve standart katmanları - 100<br/><br/>Bölümlenen varlıklar içinde desteklenmiyor [Premium](../articles/service-bus-messaging/service-bus-premium-messaging.md) katmanı.<br/><br />Ad alanı başına 1000 varlıkların kota doğrultusunda her bölümlenmiş bir kuyruk veya konuda sayar. |
| Herhangi bir Mesajlaşma varlığı yolu en büyük boyutunu: kuyruk veya konu başlığı |Varlık |- |260 karakter |
| En büyük boyutunu herhangi bir Mesajlaşma varlığı ad: ad alanı, abonelik veya abonelik kuralı |Varlık |- |50 karakteri |
| Bir iletinin en büyük boyutu [oturum kimliği](/dotnet/api/microsoft.azure.servicebus.message.sessionid) | Varlık |- | 128 |
| Bir kuyruk/konu/abonelik varlığı için ileti boyutu |Varlık |Bu kotaları aşan gelen iletileri reddedilir ve bir özel durum çağıran kod tarafından alınır. |En büyük ileti boyutu: 256 KB ([standart katman](../articles/service-bus-messaging/service-bus-premium-messaging.md)) / 1 MB ([Premium katmanı](../articles/service-bus-messaging/service-bus-premium-messaging.md)). <br /><br />Sistem yükü nedeniyle, bu sınır bu değerleri küçüktür.<br /><br />En fazla üst bilgi boyut: 64 KB<br /><br />Özellik paketi üstbilgi özellikleri sayısı: **bayt/int MaxValue**<br /><br />Özellik paketi özellik üst sınırı: açık bir sınır yoktur. En fazla üstbilgi boyutu sınırlıdır. |
| Bir kuyruk/konu/abonelik varlığı için ileti özelliği boyutu |Varlık |A **SerializationException** özel durum oluşturulur. |En fazla ileti özelliği her bir özellik için tüm özelliklerin toplam boyutu 32 K. 64 k aşamaz boyutudur Bu sınır için tüm üst geçerlidir [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage), her ikisi de sahip kullanıcı özelliklerinin yanı sıra Sistem Özellikleri (gibi [SequenceNumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sequencenumber), [etiket](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label), [ MessageID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid), vb.). |
| Konu başına abonelik sayısı |Varlık |Konu için ek bir abonelik oluşturmak için sonraki istekler reddedilir. Sonuç olarak, portal üzerinden yapılandırdıysanız, bir hata iletisi gösterilir. Yönetim API'SİNDEN çağrılırsa bir özel durum çağıran kod tarafından alınır. |Standart katman - her abonelik, ad alanı başına 1000 varlıkları (kuyruklar, konular ve abonelikler) kotaya sayar. <br/> <br/> Premium Katman - 2. 000 ' |
| Konu başına SQL filtresi sayısı |Varlık |Ek filtreleri konuya oluşturulması için sonraki istekler reddedilir ve bir özel durum çağıran kod tarafından alınır. |2,000 |
| Konu başına bağıntı filtresi sayısı |Varlık |Ek filtreleri konuya oluşturulması için sonraki istekler reddedilir ve bir özel durum çağıran kod tarafından alınır. |100.000 |
| SQL filtreleri/Eylemler boyutu |Ad Alanı |Ek filtreler oluşturulması için sonraki istekler reddedilir ve bir özel durum çağıran kod tarafından alınır. |Filtre koşulu dizenin en fazla uzunluk: 1024 (1 K).<br /><br />Kural eylemi dizenin en fazla uzunluk: 1024 (1 K).<br /><br />En fazla sayıda ifade başına kural eylemi: 32. |
| Sayısı [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) ad alanı, kuyruk veya konu başına kuralları |Varlık, ad alanı |Ek kurallar oluşturmak için sonraki istekler reddedilir ve bir özel durum çağıran kod tarafından alınır. |En fazla kural sayısı: 12. <br /><br /> Tüm Kuyruklar ve konular bu ad alanındaki bir Service Bus ad alanınızdaki yapılandırılan kuralları uygulanır. |
| İleti başına işlem sayısı | İşlem | Ek gelen iletiler reddedilir ve belirten bir özel durum "100'den fazla ileti tek bir işlemde gönderemez" çağıran kod tarafından alınır. | 100 <br /><br /> Her ikisi için de **Send()** ve **SendAsync()** işlemleri. |

[Azure portal]: https://portal.azure.com