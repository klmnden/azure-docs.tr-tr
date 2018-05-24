---
title: Azure Redis Önbelleği nedir? | Microsoft Docs
description: Azure Redis Cache’ın ne olduğu ve yaygın olarak nasıl kullanıldığı hakkında bilgi edinin.
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: overview
ms.date: 03/26/2018
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: 8f477282e49104e9b034e11656ff50c2a67545f7
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="what-is-azure-redis-cache"></a>Azure Redis Cache nedir?

Azure Redis Cache, popüler açık kaynak [Redis önbelleğini](https://redis.io/) temel alır. Genellikle, arka uç veri depolarını yoğun bir şekilde bağımlı olan sistemlerin performans ve ölçeklenebilirliğini artırmak için önbellek olarak kullanılır. Performans, sık erişilen veriler uygulamaya yakın bir konumdaki hızlı depolama alanına geçici olarak kopyalanarak iyileştirilir. [Redis önbelleği](https://redis.io/) ile bu hızlı depolama bir veritabanı tarafından diskten yüklenmek yerine Redis Cache ile birlikte bellek içinde bulunur.

Azure Redis Cache, bir bellek içi veri yapısı deposu, dağıtılmış bir ilişkisel olmayan veritabanı ve ileti aracısı olarak da kullanılabilir. Redis altyapısının düşük gecikmeli, yüksek aktarım hızına sahip performansından yararlanılarak uygulama performansı iyileştirilir.

Azure Redis Cache, Microsoft tarafından yönetilip Azure içinde barındırılan ve Azure içindeki herhangi bir uygulamanın erişimine açık olan, güvenli, ayrılmış bir Redis önbelleğine erişmenizi sağlar.

## <a name="why-use-azure-redis-cache"></a>Neden Azure Redis Cache kullanılmalıdır?

Uygulama mimarisini desteklemek veya uygulama performansını iyileştirmek için Redis Cache kullanılan çok sayıda yaygın düzen vardır. En yaygın olanlarından bazıları şunlardır:

| Desen      | Açıklama                                        |
| ------------ | -------------------------------------------------- |
| [Edilgen Önbellek](cache-web-app-cache-aside-leaderboard.md) | Bir veritabanı büyük olabileceği için, tüm veritabanının bir önbelleğe yüklenmesi önerilen bir yaklaşım değildir. Veri öğelerini önbelleğe yalnızca gerekli olduğunda yüklemek için [cache-aside](https://docs.microsoft.com/azure/architecture/patterns/cache-aside) düzeninin kullanılması yaygındır. Sistem arka uç verilerinde değişiklikler yaptığında da önbelleği güncelleyebilir ve diğer istemcilerle dağıtabilir. Ek olarak, sistem veri öğeleri için bir süre sonu ayarlayabilir ya da veri güncelleştirmelerinin önbelleğe yeniden yüklenmesine neden olacak bir çıkarma ilkesi kullanabilir.|
| [İçeriği Önbelleğe Alma](cache-aspnet-output-cache-provider.md) | Çoğu web sayfası üst bilgi, alt bilgi, araç çubuğu, menü vb. içeren şablonlardan oluşturulur. Bu web sayfaları aslında çok sık değişmez ve dinamik olarak oluşturulmamalıdır. Azure Redis Cache gibi bellek içi bir önbellek kullanmak, arka uç veri depolarınıza kıyasla bu tür bir statik içeriğe web sunucularınız için hızlı erişim sağlar. Bu düzen, içeriği dinamik olarak oluşturmak için gerekecek işleme süresini ve sunucu yükünü azaltır. Bunun yapılması, web sunucularının daha iyi yanıt vermesini sağlar ve yükleri işlemek için gereken sunucu sayısını azaltmanızı sağlayabilir. Azure Redis Cache, bu düzeni ASP.NET ile desteklemeye yardımcı olmak üzere Redis Çıktı Önbelleği Sağlayıcısı’nı kullanır.|
| [Kullanıcı oturumunu önbelleğe alma](cache-aspnet-session-state-provider.md) | Bu düzen yaygın olarak alışveriş sepetleri ve bir web uygulamasının kullanıcı tanımlama bilgileriyle ilişkilendirmek isteyebileceği diğer kullanıcı geçmişi türündeki bilgilerle birlikte kullanılır. Bir tanımlama bilgisinde çok fazla bilgi depolamak, tanımlama bilgisi boyutu arttıkça ve tanımlama bilgisi her istek ile birlikte geçirilip doğrulandıkça performansı olumsuz yönde etkileyebilir. Tipik bir çözüm, bir arka uç veritabanında verileri sorgulamak üzere tanımlama bilgisinin anahtar olarak kullanılmasıdır. Bilgileri bir kullanıcıyla ilişkilendirmek için Azure Redis Cache gibi bellek içi bir önbelleğin kullanılması, tam bir ilişkisel veritabanı ile etkileşimde bulunmaktan çok daha hızlıdır. |
| İş ve ileti sıraya alma | Uygulamalar istek aldığında genellikle istekle ilişkili işlemlerin yürütülmesi ek zaman alır. Daha uzun süre çalışan işlemleri bir sıraya ekleyerek ertelemek ve daha sonra, muhtemelen başka bir sunucu ile işleme almak yaygın bir düzendir. Bu iş erteleme yöntemine görevi sıraya alma adı verilir. Görev sıralarını desteklemek için tasarlanmış birçok yazılım bileşeni mevcuttur. Redis Cache, dağıtılmış bir sıra olarak bu amaca da hizmet eder.|
| Dağıtılmış işlemler | Uygulamaların tek bir işlem (atomik) olarak bir arka uç veri deposuna karşı bir dizi komut yürütebilmesi genel bir gereksinimdir. Tüm komutlar başarılı olmalı veya tümü ilk durumuna geri döndürülmelidir. Redis Cache, bir komut toplu işini [İşlemler](https://redis.io/topics/transactions) biçiminde tek bir işlem olarak yürütmeyi destekler. |

## <a name="azure-redis-cache-offerings"></a>Azure Redis Cache teklifleri

Azure Redis Cache aşağıdaki katmanlarda kullanılabilir:

| Katman | Açıklama |
|---|---|
Temel | Tek düğümlü bir önbellek. Bu katman birden fazla bellek boyutunu (250 MB - 53 GB) destekler. Geliştirme/test ve kritik olmayan iş yükleri için ideal bir katmandır. Temel katmanda hizmet düzeyi sözleşmesi (SLA) yoktur |
| Standart | Microsoft tarafından yönetilen iki düğümlü bir birincil/ikincil yapılandırmada çoğaltılan ve yüksek kullanılabilirlik SLA’sı (%99,9) ile sunulan önbellek. |
| Premium | Premium katmanı, Kurumsal kullanıma hazır katmandır. Premium katmanı Önbellekler daha fazla özelliği destekler ve düşük gecikmeyle daha yüksek aktarım hızına sahiptir. Premium katmanındaki önbellekler, Temel veya Standart Katmanına kıyasla daha iyi performans sağlayan daha güçlü donanımlara dağıtılır. Bu avantaj, aynı boyuttaki bir önbellek için aktarım hızının, Premium katmanında Standart katmanına göre daha yüksek olacağı anlamına gelir |

> [!TIP]
> Premium önbelleklerde boyut, aktarım hızı ve bant genişliği hakkında daha fazla bilgi için bkz. [Azure Redis Cache SSS](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).
>

Önbelleğiniz oluşturulduktan sonra ölçeğini daha yüksek bir katmana artırabilirsiniz. Ölçeğin daha düşük bir katmana indirilmesi desteklenmez. Adım adım ölçeklendirme yönergeleri için bkz. [Azure Redis Cache’ı Ölçeklendirme](cache-how-to-scale.md) ve [Bir ölçeklendirme işlemini otomatik hale getirme](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

### <a name="feature-comparision"></a>Özellik Karşılaştırması

[Redis Cache Fiyatlandırma](https://azure.microsoft.com/pricing/details/cache/) sayfasında her bir katmanın ayrıntılı karşılaştırması verilmiştir. Aşağıdaki tablo, katmana göre desteklenen özelliklerden bazılarını açıklamaya yardımcı olur:

| Özellik Açıklaması | Premium | Standart | Temel |
| ------------------- | :-----: | :------: | :---: |
| [Hizmet Düzeyi Sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/cache/v1_0/) |✔|✔|-|
| [Redis veri kalıcılığı](cache-how-to-premium-persistence.md) |✔|-|-|
| [Redis kümesi](cache-how-to-premium-clustering.md) |✔|-|-|
| [Güvenlik duvarı kuralları ile güvenlik](cache-configure.md#firewall) |✔|✔|✔|
| [VNet ile geliştirilmiş güvenlik ve yalıtım](cache-how-to-premium-vnet.md) |✔|-|-|
| [İçeri/Dışarı Aktarma](cache-how-to-import-export-data.md) |✔|-|-|
| [Güncelleştirmeleri zamanlama](cache-administration.md#schedule-updates) |✔|-|-|
| [Coğrafi çoğaltma](cache-how-to-geo-replication.md) |✔|-|-|
| [Yeniden başlatma](cache-administration.md#reboot) |✔|✔|✔|

## <a name="next-steps"></a>Sonraki adımlar

* [ASP.NET Web Uygulaması Hızlı Başlangıcı](cache-web-app-howto.md) Azure Redis Cache kullanan basit bir ASP.NET web uygulaması oluşturun.
* [.NET Hızlı Başlangıcı](cache-dotnet-how-to-use-azure-redis-cache.md) Azure Redis Cache kullanan bir .NET uygulaması oluşturun.
* [Node.js Hızlı Başlangıcı](cache-nodejs-get-started.md) Azure Redis Cache kullanan basit bir Node.js uygulaması oluşturun.
* [Java Hızlı Başlangıcı](cache-java-get-started.md) Azure Redis Cache kullanan basit bir Java uygulaması oluşturun.
* [Python Hızlı Başlangıcı](cache-python-get-started.md) Azure Redis Cache kullanan bir Python uygulaması oluşturun.