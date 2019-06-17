---
title: En iyi uygulamalar için Azure işlevleri | Microsoft Docs
description: Azure işlevleri için en iyi uygulamaları ve desenleri öğrenin.
services: functions
documentationcenter: na
author: wesmc7777
manager: jeconnoc
keywords: Azure işlevleri, desenler, en iyi uygulama, İşlevler, olay işleme, Web kancaları, dinamik işlem, sunucusuz mimari
ms.assetid: 9058fb2f-8a93-4036-a921-97a0772f503c
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/16/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 30b187676f0c1fb03b7124d93b3991b0e32d61ae
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62104686"
---
# <a name="optimize-the-performance-and-reliability-of-azure-functions"></a>Azure işlevleri'nin güvenilirliği ve performansı en iyi duruma getirme

Bu makalede performans ve güvenilirliğini geliştirmek için rehberlik sağlanır, [sunucusuz](https://azure.microsoft.com/solutions/serverless/) işlev uygulamaları. 

## <a name="general-best-practices"></a>Genel en iyi yöntemler

En iyi oluşturma ve Azure işlevleri'ni kullanarak sunucusuz çözümlerinizi mimari aşağıda verilmiştir.

### <a name="avoid-long-running-functions"></a>İşlevleri uzun süre çalışan kaçının

Büyük ve uzun süre çalışan işlevler beklenmeyen zaman aşımı sorunlara neden olabilir. Bir işlevin birçok Node.js bağımlılıklar nedeniyle büyüyebilir. Bağımlılıkları alma beklenmeyen zaman aşımlarına neden artan yükleme süreleri de neden olabilir. Bağımlılıklar hem de açık ve örtük olarak yüklenir. Kodunuz tarafından yüklenen tek bir modülü kendi ek modüller yükleyebilir.  

Mümkün olduğunda, yeniden düzenleme büyük işlevleri daha küçük işlevine, iş birlikte ayarlar ve hızlı yanıtlar döndürür. Örneğin, bir Web kancası veya HTTP tetikleyici işlevi belirli bir süre içinde bir bildirim yanıt gerektirebilir; anlık bir yanıt gerektirecek şekilde için Web kancaları yaygındır. HTTP tetikleyicisi yükü kuyruğu tetikleme işlevi tarafından işlenecek bir kuyruğun içine geçirebilirsiniz. Bu yaklaşım, gerçek iş ertele ve anlık bir yanıt döndürür olanak tanır.


### <a name="cross-function-communication"></a>İşlev iletişim arası

[Dayanıklı işlevler](durable/durable-functions-concepts.md) ve [Azure Logic Apps](../logic-apps/logic-apps-overview.md) durumu geçişleri ve birden çok işlevi arasındaki iletişim yönetmek için tasarlanmıştır.

Dayanıklı işlevler veya Logic Apps ile birden çok işlevleri tümleştirmek için kullanılıyorsa, genellikle çapraz işlevi iletişimi için depolama kuyruklarını kullanma bir en iyi uygulamadır.  Ana nedeni, depolama kuyruklarına ucuz ve sağlamak için çok daha kolay olmasıdır. 

Tek bir depolama kuyruğuna iletiler 64 KB boyutunda sınırlıdır. İşlevleri arasında daha büyük iletiler geçirmek gerekiyorsa, 256 KB'a kadar standart katmandaki bir Azure Service Bus kuyruk iletisi desteklemek için kullanılabilir boyutları ve Premium katmanda 1 MB'a kadar.

Service Bus konu başlıklarına ileti işleme önce filtreleme gerektiriyorsa yararlı olur.

Olay hub'ları, yüksek hacimli iletişim desteklemek kullanışlıdır.


### <a name="write-functions-to-be-stateless"></a>Durum bilgisiz olarak işlevler yazma 

İşlevleri durum bilgisiz ve mümkünse etkili. Tüm gerekli durum bilgilerini verilerinizle ilişkilendirin. Örneğin, işlenmekte olan büyük olasılıkla haritamın ilişkili bir sipariş `state` üyesi. Bir işlev, işlev durum bilgisiz kalırken, durumunu temel alan bir sipariş işlem gerçekleştirilemedi. 

Idempotent işlevleri Zamanlayıcı tetikleyici ile özellikle önerilir. Kesinlikle günde bir kez çalıştırılması gereken bir şey varsa, gün içinde dilediğiniz zaman ile aynı sonuçları çalıştırabilmeniz gibi yazın. Belirli bir gün için hiçbir iş olduğunda işlev çıkabilirsiniz. Ayrıca bir önceki tamamlanması başarısız çalıştırırsanız, sonraki çalıştırma kaldığı yukarı seçmeniz gerekir.


### <a name="write-defensive-functions"></a>Savunma işlevler yazma

İşlevinizi dilediğiniz zaman bir özel durum karşılaşabilirsiniz varsayılır. Sonraki yürütme sırasında bir önceki başarısız noktadan devam etme olanağı ile işlevlerinizi tasarlayın. Aşağıdaki eylemleri gerektiren bir senaryo düşünün:

1. Bir db 10.000 satır için sorgu.
2. Her biri için bir kuyruk iletisi oluşturmak daha fazla işlemek için bir satır aşağı satır.
 
Nasıl karmaşık sisteminize bağlı olarak, size sahip olabilir: hatalı davranmakta söz konusu aşağı akış hizmetleriyle ağ kesintileri veya kota sınırları ulaşıldığında, vb. Bunların tümü, işlevinizi herhangi bir zamanda etkileyebilir. İşlevleriniz için hazırlıklı olmak için tasarım gerekir.

İlgili öğelerin 5.000 işlem için bir kuyruğun içine ekledikten sonra bir hata oluşması durumunda, kodunuzu nasıl tepki vermez? Tamamladığınıza göre bir kümedeki öğeleri izleyin. Aksi halde, sonraki açışınızda tekrar bunları ekleyebilirsiniz. Bu, iş akışınızı üzerinde önemli bir etkiye sahip olabilir. 

Kuyruk öğesi zaten işlendi, işlevinizi bir İşlemsiz olmasını sağlar.

Azure işlevleri platform kullandığınız bileşenleri için zaten sağlanan savunma önlemleri yararlanın. Örneğin, **zehirli iletileri işleme** belgelerindeki [Azure depolama kuyruğu Tetikleyicileri ve bağlamaları](functions-bindings-storage-queue.md#trigger---poison-messages). 

## <a name="scalability-best-practices"></a>Ölçeklenebilirlik en iyi uygulamalar

Nasıl örnekleri işlev uygulamanızın ölçeğini etkileyen bir dizi etkene vardır. Ayrıntılar için belgelerinde verilmiştir [işlevi ölçeklendirme](functions-scale.md).  Bir işlev uygulaması, en iyi ölçeklenebilirlik sağlamak için en iyi yöntemlerden bazıları aşağıda verilmiştir.

### <a name="share-and-manage-connections"></a>Paylaşın ve bu bağlantıları yönetme

Mümkün olduğunda, dış kaynaklara yeniden kullanın.  Bkz: [Azure işlevleri'nde bağlantılarını yönetme](./manage-connections.md).

### <a name="dont-mix-test-and-production-code-in-the-same-function-app"></a>Test ve üretim kodu aynı işlev uygulamasında bir arada kullanmayın

Bir işlev uygulaması işlevlerinde kaynakları paylaşır. Örneğin, bellek paylaşılır. Üretim ortamında bir işlev uygulaması kullanıyorsanız, test ile ilgili işlevler ve kaynaklar için eklemeyin. Bu, üretim kodu yürütme sırasında beklenmeyen ek yükü neden olabilir.

Üretim işlev uygulamalarınızı yüklemek dikkatli olun. Bellek, uygulamadaki her bir işlevin üzerinden ortalaması alınır.

Birden çok .NET işlevlerde başvurulan paylaşılan bir derleme varsa, ortak paylaşılan bir klasöre yerleştirin. Bütünleştirilmiş kod aşağıdaki örneğe benzer bir ifade ile C# betik (.csx) kullanıyorsanız, başvuru: 

    #r "..\Shared\MyAssembly.dll". 

Aksi takdirde, yanlışlıkla işlevler arasında farklı şekilde davranan birden çok test sürümleri aynı ikilinin dağıtmak kolaydır.

Ayrıntılı günlük üretim kodunda kullanmayın. Bu olumsuz bir etkisi vardır.

### <a name="use-async-code-but-avoid-blocking-calls"></a>Zaman uyumsuz kodu kullanır ancak çağrıları engellemekten kaçınacak

Zaman uyumsuz programlama önerilen bir en iyi yöntemdir. Bununla birlikte, her zaman başvuran kaçının `Result` özelliği veya arama `Wait` metodunda bir `Task` örneği. Bu yaklaşım için iş parçacığı tükenmesi neden olabilir.

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

### <a name="receive-messages-in-batch-whenever-possible"></a>Mümkün olduğunda batch'de ileti alma

Olay hub'ı gibi bazı Tetikleyicileri toplu iletiler üzerinde tek bir çağrı alma etkinleştirin.  Toplu ileti işleme, çok daha iyi performans sahiptir.  Maksimum toplu işlem boyutu yapılandırabilirsiniz `host.json` dosya içinde ayrıntılı olarak [host.json başvurusu belgeleri](functions-host-json.md)

C# işlevlerine bir türü kesin belirlenmiş diziye türünü değiştirebilirsiniz.  Örneğin, yerine, `EventData sensorEvent` yöntem imzası olabilir `EventData[] sensorEvent`.  Diğer diller için açıkça kardinalite özelliği ayarlayın gerekir, `function.json` için `many` toplu işleme etkinleştirmek için [burada gösterildiği gibi](https://github.com/Azure/azure-webjobs-sdk-templates/blob/df94e19484fea88fc2c68d9f032c9d18d860d5b5/Functions.Templates/Templates/EventHubTrigger-JavaScript/function.json#L10).

### <a name="configure-host-behaviors-to-better-handle-concurrency"></a>Konak davranışlarını daha iyi eşzamanlılık işlemek için yapılandırma

`host.json` İşlev uygulaması dosyasında konak çalışma ve tetikleyici davranışlarını yapılandırmasını sağlar.  Toplu işlem davranışları yanı sıra birkaç tetikleyici için eşzamanlılık yönetebilirsiniz.  Genellikle bu seçenekler değerleri ayarlama, her bir örneği ölçek çağrılan işlevlerin gereksinimlerini karşılamak için uygun şekilde yardımcı olabilir.

İçinde uygulama uygulamasında tüm işlevleri arasında ayarları hosts dosyasında bir *tek örnek* işlevin. Örneğin, bir işlev uygulaması ile 2 HTTP işlevler vardı ve eş zamanlı istek 25 olarak ayarlamanızı, ya da HTTP tetikleyicisi isteğine paylaşılan 25 eş zamanlı istekler sayılacaktır.  Bu işlev uygulaması için 10 örneğe ölçeği, 2 işlevleri etkili bir şekilde 250 eş zamanlı istek izin (10 örneğe * örnek başına 25 eş zamanlı istek).

**HTTP eşzamanlılık konak seçenekleri**

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

Diğer ana bilgisayar yapılandırma seçenekleri bulunabilir [ana bilgisayar yapılandırma belgesindeki](functions-host-json.md).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure işlevleri'nde bağlantılarını yönetme](manage-connections.md)
* [Azure App Service en iyi uygulamaları](../app-service/app-service-best-practices.md)
