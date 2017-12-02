---
title: "En iyi uygulamalar için Azure işlevleri | Microsoft Docs"
description: "Azure işlevleri için en iyi yöntemler ve yaklaşımlar öğrenin."
services: functions
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: 
tags: 
keywords: "Azure işlevleri, desenleri, en iyi uygulama, işlemler ve olay işleme, Web kancalarını, dinamik işlem, sunucusuz mimarisi"
ms.assetid: 9058fb2f-8a93-4036-a921-97a0772f503c
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/16/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 739e820a44194af984750932d6023c90fcd11e42
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="optimize-the-performance-and-reliability-of-azure-functions"></a>Azure işlevleri güvenilirliğini ve performansını en iyi duruma getirme

Bu makalede performansı ve güvenilirliği geliştirmek için yönergeler sağlar, [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) işlev uygulamalar. 

## <a name="general-best-practices"></a>Genel en iyi yöntemler

Derleme ve Azure işlevleri kullanarak sunucusuz çözümlerinizi mimari nasıl en iyi uygulamalar şunlardır:

### <a name="avoid-long-running-functions"></a>İşlevler uzun süre çalışan kaçının

Büyük, uzun süre çalışan işlevler beklenmeyen zaman aşımı sorunlara neden olabilir. Bir işlev pek çok Node.js bağımlılık nedeniyle büyüyebilir. Bağımlılıklar alma beklenmeyen zaman aşımlarına neden artan yükleme süreleri de neden olabilir. Bağımlılıklar her ikisi de açıkça ve örtülü olarak yüklenir. Kodunuz tarafından yüklenen tek bir modül ek modülleri yükleyebilir.  

Mümkün olduğunda, daha küçük işlevine düzenleme büyük işlevlerin o iş birlikte ayarlar ve yanıtları hızlı döndürür. Örneğin, bir Web kancası veya HTTP tetikleyicisi işlevi belirli bir süre içinde bir bildirim yanıt gerektirebilir; hemen bir yanıt istemek için Web kancası için yaygın bir sorundur. Bir kuyruk tetikleyici işlev tarafından işlenmek üzere bir kuyruğuna HTTP tetikleyicisi yükü geçirebilirsiniz. Bu yaklaşım, gerçek iş erteleneceği ve hemen bir yanıt döndürür olanak sağlar.


### <a name="cross-function-communication"></a>İşlev iletişimi arası

[Dayanıklı işlevleri](durable-functions-overview.md) ve [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md) durumu geçişleri ve birden çok işlevleri arasındaki iletişimi yönetmek için tasarlanmıştır.

Dayanıklı işlevleri veya Logic Apps birden çok işlevleri ile tümleştirmek için kullanılıyorsa, genellikle işlevi iletişim arası depolama kuyruklarında kullanmak için en iyi uygulama içindir.  Ana nedeni, depolama kuyrukları daha ucuz ve sağlamak için çok daha kolay olmasıdır. 

Bir depolama kuyruğu tek bir ileti boyutu 64 KB sınırlıdır. Büyük iletileri işlevleri arasında geçirmek gerekiyorsa, kuyruk iletisi desteklemek için kullanılabilecek bir Azure hizmet veri yolu en fazla 256 KB boyutları.

Service Bus konu başlıklarını ileti işleme önce filtreleme gerektiriyorsa yararlı olur.

Olay hub'ları, yüksek hacimli iletişimini desteklemek faydalıdır.


### <a name="write-functions-to-be-stateless"></a>Durum bilgisiz olmasını işlevler yazma 

İşlevler durum bilgisiz ve mümkünse ıdempotent. Tüm gerekli durum bilgilerini verilerinizle ilişkilendirin. Örneğin, işlenmekte olan büyük olasılıkla sahip olabilir ilişkili bir sipariş `state` üyesi. Bir işlev işlevi durum bilgisiz devam ederken bu durumuna bağlı bir sipariş işlem gerçekleştirilemedi. 

Idempotent işlevleri ile Zamanlayıcı Tetikleyicileri özellikle önerilir. Kesinlikle günde bir kez çalıştırmanız gereken bir şey varsa, gün sırasında her zaman aynı sonucu ile çalıştırabilmeniz için örneğin, okuma ve yazma. Belirli bir gün için hiçbir iş olduğunda işlevi çıkabilirsiniz. Ayrıca bir önceki çalıştırma tamamlamak için başarısız oldu, sonraki çalıştırma kaldığı yerden yukarı seçmeniz gerekir.


### <a name="write-defensive-functions"></a>Savunma işlevler yazma

İşlevinizi dilediğiniz zaman bir özel durum karşılaşabileceği varsayalım. Sonraki yürütme sırasında bir önceki başarısız noktasından devam etme olanağı ile işlevlerinizi tasarlayın. Aşağıdaki eylemleri gerektiren bir senaryo düşünün:

1. Bir db 10.000 satır için sorgu.
2. Bunların her biri için bir kuyruk iletisi oluşturmak daha fazla işlem satırları aşağı satır.
 
Nasıl karmaşık sisteminize bağlı olarak, size sahip olabilir: hatalı davranmakta söz konusu aşağı akış hizmetleriyle ağ kesintileri veya kota sınırları ulaşıldığında, vb. Tüm bunların herhangi bir zamanda işlevinizi etkileyebilir. İşlevlerinizi için hazırlanması tasarlamanız gerekir.

İşleme kuyruğuna bu öğelerinin 5.000 ekledikten sonra bir hata oluşursa, kodunuzu nasıl tepki vermez? Öğeleri, tamamladığınız kümesindeki izler. Aksi takdirde, bunları yeniden başlattığınızda ekleyebilirsiniz. Bu, iş akışınızı üzerinde ciddi bir etkisi olabilir. 

Bir kuyruk öğesi zaten işlenmiş ise Hayır op olmasını işlevinizi izin verir.

Azure işlevleri platform kullandığınız bileşenleri için zaten sağlanan savunma önlemleri yararlanın. Örneğin, **zarar iletileri işleme** belgelerindeki [Azure depolama kuyruğu Tetikleyicileri ve bağlamaları](functions-bindings-storage-queue.md#trigger---poison-messages). 

## <a name="scalability-best-practices"></a>Ölçeklenebilirlik en iyi uygulamalar

Nasıl işlevi uygulamanızı örneklerini ölçeklendirme etkileyen faktörler mevcuttur. Ayrıntılar için belgelere sağlanan [işlevi ölçeklendirme](functions-scale.md).  Bir işlev uygulaması, en iyi ölçeklenebilirlik sağlamak için bazı en iyi uygulamalar şunlardır:

### <a name="dont-mix-test-and-production-code-in-the-same-function-app"></a>Aynı işlev uygulaması test ve üretim kodunda bir arada kullanmayın

Bir işlev uygulaması işlevlerinde kaynakları paylaşır. Örneğin, bellek paylaşılır. Bir işlev uygulaması üretimde kullanıyorsanız, test ilgili işlevleri ve kaynaklar için eklemeyin. Üretim kodu yürütme sırasında beklenmeyen yükünü neden olabilir.

Üretim işlevi uygulamalarınızda yük dikkatli olun. Bellek uygulamadaki her işlev üzerinden ortalaması alınır.

Birden çok .net işlevlerde başvurulan paylaşılan bir derleme varsa, ortak bir paylaşılan klasöre yerleştirin. C# komut dosyaları (.csx) kullanıyorsanız, aşağıdaki örneğe benzer bir deyim derleme başvurusu: 

    #r "..\Shared\MyAssembly.dll". 

Aksi takdirde, yanlışlıkla işlevleri arasında farklı şekilde davranan birden çok test sürümünü aynı ikili dağıtılması kolaydır.

Ayrıntılı günlük üretim kodunda kullanmayın. Olumsuz bir etkisi vardır.

### <a name="use-async-code-but-avoid-blocking-calls"></a>Zaman uyumsuz kod kullanır ancak çağrıları engelleme kaçının

Zaman uyumsuz programlama önerilen en iyi uygulamadır. Bununla birlikte, her zaman başvuran kaçının `Result` özelliği veya arama `Wait` yöntemi bir `Task` örneği. Bu yaklaşım, iş parçacığı tükenmesine yol açabilir.

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

### <a name="receive-messages-in-batch-whenever-possible"></a>Mümkün olduğunda toplu işlemde ileti alma

Olay hub'ı gibi bazı tetikleyiciler tek çağırma iletilerde toplu almayı etkinleştirin.  Toplu ileti işleme çok daha iyi performans sahiptir.  Maksimum toplu iş boyutu yapılandırabilirsiniz `functions.json` dosya içinde ayrıntılı olarak [host.json başvuru belgeleri](functions-host-json.md)

C# işlevleri için kesin türü belirtilmiş bir dizi türünü değiştirebilirsiniz.  Örneğin, yerine, `EventData sensorEvent` yöntem imzası olabilir `EventData[] sensorEvent`.  Diğer diller için açıkça kardinalite özelliği kümesinde gerekir, `function.json` için `many` toplu işleme etkinleştirmek için [aşağıda gösterildiği gibi](https://github.com/Azure/azure-webjobs-sdk-templates/blob/df94e19484fea88fc2c68d9f032c9d18d860d5b5/Functions.Templates/Templates/EventHubTrigger-JavaScript/function.json#L10).

### <a name="configure-host-behaviors-to-better-handle-concurrency"></a>Daha iyi eşzamanlılık işlemek için ana bilgisayar davranışları yapılandırın

`host.json` İşlev uygulaması dosyasında konak çalışma zamanı ve tetikleyici davranışları yapılandırmasını sağlar.  Davranışları toplu işleme yanı sıra Tetikleyiciler sayısı için eşzamanlılık yönetebilirsiniz.  Genellikle bu seçeneklerinde değerlerini ayarlama her örneği ölçek çağrılan işlevleri gereksinimlerini karşılamak için uygun şekilde yardımcı olur.

İçinde uygulama uygulama içinde tüm işlevleri arasında hosts dosyasını ayarlarında bir *tek örnek* işlevinin. Örneğin, bir işlev uygulaması 2 HTTP işlevlere sahip olan ve eş zamanlı istekler 25 olarak ayarlamak, her iki HTTP tetikleyici için bir istek paylaşılan 25 eş zamanlı istekler sayar.  Bu işlev uygulaması 10 örneklerine ölçeği, 2 işlevleri 250 eşzamanlı istek etkili bir şekilde olanak tanır (10 örnekleri * örneği başına 25 eşzamanlı istek).

**HTTP eşzamanlılık konak seçenekleri**

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

Diğer ana bilgisayar yapılandırma seçeneklerini bulunabilir [ana bilgisayar yapılandırma belgesindeki](functions-host-json.md).

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için aşağıdaki kaynaklara bakın:

Azure işlevleri Azure App Service kullandığından, ayrıca uygulama hizmeti yönergelerini farkında olmalıdır.
* [Patterns and Practices HTTP performans iyileştirmeleri](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/)
