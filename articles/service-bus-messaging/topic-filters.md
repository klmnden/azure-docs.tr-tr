---
title: Azure Service Bus konu filtreleri | Microsoft Docs
description: "Azure Service Bus konularını filtre"
services: service-bus-messaging
documentationcenter: 
author: clemensv
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: sethm
ms.openlocfilehash: 305c017bd49f233c10479e2c33ec8db72cae3aa7
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="topic-filters-and-actions"></a>Konu filtreleri ve eylemleri

Aboneler bir konusundan almak istediği hangi iletilerin tanımlayabilirsiniz. Bu iletiler bir veya daha fazla adlandırılmış abonelik kuralları biçiminde belirtilir. Her kural belirli iletileri ve seçili iletiyi açıklama ekler bir eylemi seçen bir koşulu oluşur. Her eşleşen Kural koşulu için abonelik eşleşen her kural için farklı bir şekilde açıklama iletinin bir kopyasını oluşturur.

Her yeni oluşturulan konu aboneliği ilk varsayılan aboneliği kuralı vardır. Kural için bir filtre koşulu açıkça belirtmezseniz, uygulanan filtredir **true** aboneliğinize seçilecek tüm iletileri etkinleştirir filtre. Varsayılan kural herhangi bir ilişkili ek açıklama eylemi yok.

Hizmet veri yolu üç filtre koşulları destekler:

-   *Boolean filtreleri* - **TrueFilter** ve **FalseFilter** her ikisi de tüm gelen iletileri neden (**true**) veya gelen iletileri hiçbiri (**yanlış**) için abonelik seçilmelidir.

-   *SQL filtreleri* - A **SqlFilter** gelen iletileri kullanıcı tarafından tanımlanan özellikler ve Sistem özellikleri karşı Aracısı hesaplanan SQL benzeri koşullu ifade tutar. Tüm sistem özellikleri ile eklenmelidir `sys.` da koşullu ifade. [SQL dil alt filtre koşulları için](service-bus-messaging-sql-filter.md) özellikleri (EXISTS) de null-değerleri (IS NULL), mantıksal değil/ve/veya ve ilişkisel işleçler ve basit sayısal aritmetik ve basit metin düzeni varlığı testleri ile benzer eşleştirme.

-   *Bağıntı filtresi* - A **CorrelationFilter** bir veya daha fazla gelen bir iletinin kullanıcı ve sistem özelliklerini karşı eşleşen koşulları tutar. Eşleştirilecek yaygın bir kullanımdır **correlationıd değeri** özelliği, ancak uygulama da seçebilirsiniz eşleştirilecek **ContentType**, **etiket**,  **MessageID**, **ReplyTo**, **ReplyToSessionId**, **SessionID**, **için**ve her kullanıcı tanımlı Özellikler. Bir özellik için bir gelen ileti değer bağıntı filtrede belirtilen değere eşit olduğunda bir eşleşme var. Dize ifadeler için karşılaştırma büyük/küçük harf duyarlıdır. Birden çok eşleşme özellikleri belirtirken, bunları birleştirir filtre filtre eşleşecek şekilde anlamı bir mantıksal ve koşulu olarak, tüm koşullara uyması gerekir.

Tüm filtreleri ileti özelliklerini değerlendirin. Filtreler, ileti gövdesi değerlendirilemiyor.

Karmaşık filtre kuralları, işlem kapasitesi gerektirir. Özellikle, SQL filtre kuralları kullanımını abonelik, konu ve ad alanı düzeyinde Genel alt ileti üretilen işi sonuçlanır. İşlemde çok daha verimlidir ve bu nedenle daha az etkiyle sahiptir mümkün olduğunda, uygulamaları bağıntı filtresi SQL benzeri filtreleri seçmeniz gerekir.

## <a name="actions"></a>Eylemler

SQL filtre koşulları ve yalnızca bu, ileti ekleme, kaldırma veya özellikler ve değerlerinin değiştirilmesi açıklayabilirsiniz bir eylem tanımlayabilirsiniz. Eylem [SQL benzeri ifade kullanan](service-bus-messaging-sql-filter.md) SQL güncelleştirme deyimi sözdizimi, geniş leans. Eylem, eşleşen sonra ve iletiyi konu ile seçilmeden önce ileti üzerinde gerçekleştirilir. İleti özelliklerine yapılan değişiklikler, aboneliğinize kopyalanan iletisi özeldir.

## <a name="usage-patterns"></a>Kullanım desenleri

En basit kullanım için bir konu her abonelik yayın bir desen sağlayan bir konu başlığına gönderilen her iletinin kopyasını alır senaryodur.

Filtreler ve eylemleri etkinleştirmek desenleri iki başka gruplar: bölümlendirme ve yönlendirme.

İletileri arasında birden fazla konu abonelikleri tahmin edilebilir ve birbirini dışlayan bir şekilde dağıtmak için kullandığı filtreleri bölümleme. Her bir genel veri alt kümesini tutun işlevsel olarak aynı bölmeler birçok farklı bağlamlarda işlemek için çıkışı bir sistem ölçeklendirilmiş bölümleme düzeni kullanılır; Örneğin, müşteri profil bilgileri. Bölümlendirme ile bir yayımcı iletiyi konu ile bölümleme model bilgisi gerek kalmadan gönderir. İleti sonra içinden, ardından bölümünün ileti işleyicisi tarafından alınabilmesi için doğru aboneliğin taşınır.

Yönlendirme filtreleri iletiler konu abonelikleri tahmin edilebilir bir şekilde dağıtmasına için kullanır ancak mutlaka özel. İle birlikte [otomatik iletme](service-bus-auto-forwarding.md) filtreleri, karmaşık yönlendirme oluşturmak için kullanılabilir konu özelliği, grafik bir Azure bölgesi içinde ileti dağıtım için bir hizmet veri yolu ad alanı içinde. Azure işlevleri veya Azure hizmet veri yolu ad alanları arasında bir köprü görevi gören Azure Logic Apps ile iş kolu uygulamaları doğrudan tümleştirmeye genel karmaşık topolojilerle oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus Mesajlaşma hizmeti hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [SQLFilter söz dizimi](service-bus-messaging-sql-filter.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)