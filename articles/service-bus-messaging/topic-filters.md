---
title: Azure Service Bus konu başlığı filtreleri | Microsoft Docs
description: Azure Service Bus konu başlıklarını Filtrele
services: service-bus-messaging
documentationcenter: ''
author: clemensv
manager: timlt
editor: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2018
ms.author: spelluru
ms.openlocfilehash: 41af53dbfbb5c863007a332445a2f184fcbcbf81
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60332236"
---
# <a name="topic-filters-and-actions"></a>Konu başlığı filtreleri ve eylemleri

Aboneler, bir konu başlığından hangi iletileri almak istediklerini tanımlayabilir. Bu iletiler bir veya daha fazla adlandırılmış abonelik kuralları biçiminde belirtilir. Her kural belirli iletileri ve seçili ileti açıklama ekler eylemi seçen bir koşulu oluşur. Eşleşen her kural koşulu için abonelik, iletinin, her eşleşme kuralı için farklı şekilde açıklama eklenebilecek bir kopyasını üretir.

Her yeni oluşturulan konu aboneliği bir varsayılan başlangıç aboneliği kuralı vardır. Uygulanan filtre kuralı için bir filtre koşulu açıkça belirtmezseniz olan **true** aboneliğe seçilecek tüm iletileri filtre. Varsayılan kural herhangi bir ilişkili ek açıklama eylemi yok.

Service Bus üç filtre koşulları destekler:

-   *Boolean filtreleri* - **TrueFilter** ve **FalseFilter** ya da tüm gelen iletileri neden (**true**) veya gelen iletileri hiçbiri (**false**) için abonelik seçilmelidir.

-   *SQL filtresi* - **SqlFilter** Aracısı gelen iletileri kullanıcı tanımlı özellikler ve Sistem özellikleri karşı değerlendirilen SQL benzeri bir koşullu ifade tutan. Tüm sistem özellikleri ile önek eklenmelidir `sys.` koşullu ifadede. [Filtre koşulları için SQL dil alt kümesi](service-bus-messaging-sql-filter.md) testleri özellikleri bulunup bulunmadığını (`EXISTS`), null değerleri gibi yanı (`IS NULL`), mantıksal değil/ve/veya ilişkisel işleçler, basit sayısal aritmetik ve basit metin ile desen eşleştirme `LIKE`.

-   *Bağıntı filtresi* - **SQL filtresidir** bir veya daha fazla gelen iletinin kullanıcı ve sistem özellik karşı eşleşen koşullar kümesini içerir. Eşleştirilecek yaygın olan **Correlationıd** özelliği ancak uygulama de seçebilir eşleştirilecek **ContentType**, **etiket**,  **MessageID**, **ReplyTo**, **ReplyToSessionId**, **SessionID**, **için**ve her kullanıcı tanımlı özellikleri. Bir özellik için değer gelen iletinin bağıntı filtrede belirtilen değere eşit olduğunda bir eşleşme varsa. Dize ifadeleri için karşılaştırma büyük/küçük harf duyarlıdır. Birden çok eşleşme özellikleri belirtirken, bunları birleştirir filtre filtre eşleştirilecek anlamına gelen bir mantıksal ve koşulu olarak, tüm koşullara uyması gerekir.

Tüm filtreleri, ileti özelliklerini değerlendirin. Filtreler, ileti gövdesi değerlendirilemiyor.

Karmaşık filtre kuralları işlem kapasitesi gerektirir. Özellikle, abonelik, konu ve ad alanı düzeyinde Genel alt ileti işleme hızı SQL filtre kuralları kullanımını sonuçlanır. İşleme, çok daha verimlidir ve bu nedenle daha az etkisi sahiptir olduğundan mümkün olduğunda, uygulamaları bağıntı filtresi SQL benzeri filtreleri seçmeniz gerekir.

## <a name="actions"></a>Eylemler

SQL filtresi koşullarla ileti ekleme, kaldırma veya özellikleri ve değerlerini değiştirerek açıklama ekleyebilirsiniz bir eylem tanımlayabilirsiniz. Eylem [SQL benzeri bir ifade kullanır](service-bus-messaging-sql-filter.md) SQL UPDATE deyimi sözdizimi, gevşek leans. Eylem iletide, eşleşen sonra ve aboneliğe ileti seçilmeden önce gerçekleştirilir. İleti özelliklerini değişiklikleri aboneliğe kopyalanan iletisi özeldir.

## <a name="usage-patterns"></a>Kullanım desenleri

En basit kullanım için bir konu her abonelik bir yayın desen sağlayan bir konu başlığına gönderilen her iletinin bir kopyasını alır senaryodur.

Filtreleri ve eylemleri desenlerinin iki daha fazla Grup etkinleştir: bölümleme ve yönlendirme.

İletileri arasında birçok konu abonelikleri tahmin edilebilir ve birbirini dışlayan bir şekilde dağıtmak için kullandığı filtreleri bölümleme. Bir sistem her bir genel veri alt kümesini tutmak işlevsel olarak aynı bölmeleri birçok farklı bağlamlardaki işlemek için kullanıma ölçeklendirilir bölümleme düzeni kullanılır; Örneğin, müşteri profili bilgileri. Bölümlendirme ile yayımcı ileti bir konu başlığında bölümleme modelinin herhangi bir Bilgi Bankası gerek kalmadan gönderir. İletiyi daha sonra bunu ardından bölümün ileti işleyicisi tarafından alınabilmesi için doğru aboneliğin taşınır.

Yönlendirme filtreleri iletiler konu abonelikleri tahmin edilebilir bir biçimde dağıtmasına için kullanır ancak mutlaka özel. İle birlikte [otomatik iletme](service-bus-auto-forwarding.md) konu filtreleri, karmaşık yönlendirme oluşturmak için kullanılabilir özelliği, grafik bir Azure bölgesi içinde ileti dağıtım için bir Service Bus ad alanı içinde. Azure işlevleri veya Azure Service Bus ad alanları arasında bir köprü işlevi gören Azure Logic Apps ile doğrudan tümleştirme satır iş kolu uygulamalarına genel karmaşık topolojileri oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [SQLFilter söz dizimi](service-bus-messaging-sql-filter.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)