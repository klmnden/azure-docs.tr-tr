---
title: include dosyası
description: include dosyası
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 12/13/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: f48ad6ca74e6ce10148d66549fea16bc74015b2a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66171211"
---
Aşağıdaki tablo, Azure Service Bus mesajlaşması için belirli bir kota bilgileri listeler. Fiyatlandırma hakkında daha fazla bilgi ve diğer Service Bus kotaları için bkz. [Service Bus fiyatlandırma](https://azure.microsoft.com/pricing/details/service-bus/).

| Kota adı | `Scope` | Notlar | Değer |
| --- | --- | --- | --- |
| Azure aboneliği başına temel veya standart ad alanı sayısı |Ad Alanı |Sonraki istekleri için ek temel veya standart ad alanları, Azure portal tarafından reddedilir. |100|
| Azure aboneliği başına Premium ad alanı sayısı |Ad Alanı |Sonraki istekler ek Premium ad alanları için portal tarafından reddedilir. |25 |
| Kuyruk veya konu başlığı boyutu |Varlık |Kuyruk veya konu oluşturulduktan sonra tanımlanır. <br/><br/> Sonraki gelen iletileri reddedilir ve bir özel durum çağıran kod tarafından alınır. |1, 2, 3, 4 GB veya 5 GB.<br /><br />Premium SKU ve standart SKU ile [bölümleme](/azure/service-bus-messaging/service-bus-partitioning) etkinleştirildiğinde, kuyruk veya konu başlığı boyutu 80 GB olan. |
| Bir ad alanında eşzamanlı bağlantı sayısı |Ad Alanı |Bir ek bağlantı için sonraki istekler reddedilir ve bir özel durum çağıran kod tarafından alınır. REST işlemlerini eş zamanlı TCP bağlantısı sayılmaz. |NetMessaging: 1,000.<br /><br />AMQP: 5,000. |
| Eşzamanlı sayısı bir kuyruk, konu veya abonelik varlığı istekleri Al |Varlık |Sonraki alma istekleri reddedilir ve bir özel durum çağıran kod tarafından alınır. Bu birleşik kotanız eşzamanlı sayısı alma işlemlerinin bir konuya tüm abonelikler arasında. |5,000 |
| Konuları veya kuyruklar ad alanı başına sayısı |Ad Alanı |Yeni bir konu veya ad alanında kuyruk oluşturma sonraki istekler reddedilir. İle yapılandırılmış bir sonucu olarak [Azure portalında][Azure portal], bir hata iletisi oluşturulur. Adlı yönetim API'si, bir özel durum çağıran kod tarafından alınır. |1\.000 temel veya standart katman için. Bir ad alanındaki kuyrukları ve konuları sayısı 1000'e eşit veya daha az olmalıdır. <br/><br/>Premium katmanı için Mesajlaşma birimi (MU) başına 1000. Üst sınırı 4000 olabilir. |
| Sayısı [bölümlenmiş konuları veya kuyruklar](/azure/service-bus-messaging/service-bus-partitioning) ad alanı başına |Ad Alanı |Yeni bir bölümlenmiş konu veya ad alanında kuyruk oluşturma sonraki istekler reddedilir. İle yapılandırılmış bir sonucu olarak [Azure portalında][Azure portal], bir hata iletisi oluşturulur. Yönetim API'si, özel durum çağrılırsa **QuotaExceededException** çağıran kod tarafından alınır. |Temel ve standart katmanları: 100.<br/><br/>Bölümlenen varlıklar desteklenmeyen [Premium](../articles/service-bus-messaging/service-bus-premium-messaging.md) katmanı.<br/><br />Ad alanı başına 1.000 varlıkların kota doğru her bölümlenmiş bir kuyruk veya konuda sayar. |
| Herhangi bir Mesajlaşma varlığı yolu en büyük boyutunu: kuyruk veya konu başlığı |Varlık |- |260 karakter. |
| En büyük boyutunu herhangi bir Mesajlaşma varlığı ad: ad alanı, abonelik veya abonelik kuralı |Varlık |- |50 karakter. |
| En büyük boyutunu bir [ileti kimliği](/dotnet/api/microsoft.azure.servicebus.message.messageid) | Varlık |- | 128 |
| Bir iletinin en büyük boyutu [oturum kimliği](/dotnet/api/microsoft.azure.servicebus.message.sessionid) | Varlık |- | 128 |
| Bir kuyruk, konu veya abonelik varlığı için ileti boyutu |Varlık |Bu kotaları aşan gelen iletileri reddedilir ve bir özel durum çağıran kod tarafından alınır. |En büyük ileti boyutu: İçin 256 KB [standart katman](../articles/service-bus-messaging/service-bus-premium-messaging.md), 1 MB [Premium katmanı](../articles/service-bus-messaging/service-bus-premium-messaging.md). <br /><br />Sistem yükü nedeniyle, bu sınır bu değerleri küçüktür.<br /><br />En fazla üstbilgi boyutu: 64 KB.<br /><br />Özellik paketi üstbilgi özellikleri sayısı: **bayt/int MaxValue**.<br /><br />Özellik paketi özellik en büyük boyutu: Açık bir sınırlama yoktur. En fazla üstbilgi boyutu sınırlıdır. |
| Bir kuyruk, konu veya abonelik varlığı için ileti özelliği boyutu |Varlık | Özel durum **SerializationException** oluşturulur. |Her bir özellik için özellik boyutu en fazla ileti 32000 olabilir. Tüm özelliklerin toplam boyutu 64.000 aşamaz. Bu sınır için tüm üst geçerlidir [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage), sahip olduğu kullanıcı özelliklerini hem Sistem özellikleri gibi [SequenceNumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sequencenumber), [etiket](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label)ve [ MessageID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid). |
| Konu başına abonelik sayısı |Varlık |Konu için ek bir abonelik oluşturmak için sonraki istekler reddedilir. Sonuç olarak, portal üzerinden yapılandırdıysanız, bir hata iletisi gösterilir. Adlı yönetim API'si, bir özel durum çağıran kod tarafından alınır. |Standart ve Premium katmanı: Her abonelik 1.000 varlıkların kotaya diğer bir deyişle, kuyruklar, konular ve abonelikler, ad alanı başına sayar. |
| Konu başına SQL filtresi sayısı |Varlık |Ek filtreleri konuya oluşturulması için sonraki istekler reddedilir ve bir özel durum çağıran kod tarafından alınır. |2,000 |
| Konu başına bağıntı filtresi sayısı |Varlık |Ek filtreleri konuya oluşturulması için sonraki istekler reddedilir ve bir özel durum çağıran kod tarafından alınır. |100,000 |
| SQL filtresi veya Eylemler boyutu |Ad Alanı |Ek filtreler oluşturulması için sonraki istekler reddedilir ve bir özel durum çağıran kod tarafından alınır. |Filtre koşulu dize en fazla uzunluğu: 1024 (1 K).<br /><br />Kural eylemi dizenin maksimum uzunluğunu: 1024 (1 K).<br /><br />İfadeleri kural eylemi başına en fazla sayısı: 32. |
| Sayısı [Rootmanagesharedaccesskey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) ad alanı, kuyruk veya konu başına kuralları |Varlık, ad alanı |Ek kurallar oluşturmak için sonraki istekler reddedilir ve bir özel durum çağıran kod tarafından alınır. |En fazla kural sayısı: 12. <br /><br /> Tüm Kuyruklar ve konular bu ad alanındaki bir Service Bus ad alanınızdaki yapılandırılan kuralları uygulanır. |
| İleti başına işlem sayısı | İşlem | Ek gelen iletiler reddedilir ve belirten bir özel durum "100'den fazla ileti tek bir işlemde gönderemez" çağıran kod tarafından alınır. | 100 <br /><br /> Her ikisi için de **Send()** ve **SendAsync()** işlemleri. |

[Azure portal]: https://portal.azure.com
