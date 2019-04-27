---
title: Azure önbelleği için Redis nedir? | Microsoft Docs
description: Azure önbelleği için Redis nedir ve bunu yaygın olarak nasıl kullanıldığını öğrenin.
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: overview
ms.date: 03/26/2018
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 9d789572abf0545eb51b357da091e5a1d712eab2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60831444"
---
# <a name="what-is-azure-cache-for-redis"></a>Azure önbelleği için Redis nedir

Azure önbelleği için Redis popüler yazılım tabanlı [Redis](https://redis.io/). Genellikle, arka uç veri depolarını yoğun bir şekilde bağımlı olan sistemlerin performans ve ölçeklenebilirliğini artırmak için önbellek olarak kullanılır. Performans, sık erişilen veriler uygulamaya yakın bir konumdaki hızlı depolama alanına geçici olarak kopyalanarak iyileştirilir. İle [Azure önbelleği için Redis](https://redis.io/), bu hızlı bulunduğu bellek içi ile Azure önbelleği için Redis diskten yerine bir veritabanı tarafından yüklenen depolamadır.

Azure önbelleği için Redis bellek içi veri yapısı deposu, ilişkisel olmayan dağıtılmış bir veritabanı ve ileti Aracısı da kullanılabilir. Redis altyapısının düşük gecikmeli, yüksek aktarım hızına sahip performansından yararlanılarak uygulama performansı iyileştirilir.

Redis, güvenli ve adanmış bir Azure önbelleği için Redis için erişmenizi için azure önbellek, azure'da barındırılan ve içinde veya Azure dışındaki herhangi bir uygulama tarafından erişilebilen, Microsoft tarafından yönetilen.

## <a name="why-use-azure-cache-for-redis"></a>Neden Azure önbelleği için Redis kullanmalısınız?

Azure önbelleği için Redis uygulama mimarisi desteklemek için veya uygulama performansını artırmak için kullanıldığı birçok ortak desen vardır. En yaygın olanlarından bazıları şunlardır:

| Desen      | Açıklama                                        |
| ------------ | -------------------------------------------------- |
| [Edilgen Önbellek](cache-web-app-cache-aside-leaderboard.md) | Bir veritabanı büyük olabileceği için, tüm veritabanının bir önbelleğe yüklenmesi önerilen bir yaklaşım değildir. Veri öğelerini önbelleğe yalnızca gerekli olduğunda yüklemek için [cache-aside](https://docs.microsoft.com/azure/architecture/patterns/cache-aside) düzeninin kullanılması yaygındır. Sistem arka uç verilerinde değişiklikler yaptığında da önbelleği güncelleyebilir ve diğer istemcilerle dağıtabilir. Ek olarak, sistem veri öğeleri için bir süre sonu ayarlayabilir ya da veri güncelleştirmelerinin önbelleğe yeniden yüklenmesine neden olacak bir çıkarma ilkesi kullanabilir.|
| [İçeriği Önbelleğe Alma](cache-aspnet-output-cache-provider.md) | Çoğu web sayfası üst bilgi, alt bilgi, araç çubuğu, menü vb. içeren şablonlardan oluşturulur. Bu web sayfaları aslında çok sık değişmez ve dinamik olarak oluşturulmamalıdır. Bir bellek içi önbellek, Azure önbelleği için gibi kullanarak, Redis, web sunucularınızın hızlı erişim için arka uç veri depoları karşılaştırıldığında statik içeriği bu tür olanağı sunar. Bu düzen, içeriği dinamik olarak oluşturmak için gerekecek işleme süresini ve sunucu yükünü azaltır. Bunun yapılması, web sunucularının daha iyi yanıt vermesini sağlar ve yükleri işlemek için gereken sunucu sayısını azaltmanızı sağlayabilir. Azure önbelleği için Redis, Redis çıktı önbelleği ASP.NET ile bu desenin desteklenmesi amacıyla sağlayıcısı sağlar.|
| [Kullanıcı oturumunu önbelleğe alma](cache-aspnet-session-state-provider.md) | Bu düzen yaygın olarak alışveriş sepetleri ve bir web uygulamasının kullanıcı tanımlama bilgileriyle ilişkilendirmek isteyebileceği diğer kullanıcı geçmişi türündeki bilgilerle birlikte kullanılır. Bir tanımlama bilgisinde çok fazla bilgi depolamak, tanımlama bilgisi boyutu arttıkça ve tanımlama bilgisi her istek ile birlikte geçirilip doğrulandıkça performansı olumsuz yönde etkileyebilir. Tipik bir çözüm, bir arka uç veritabanında verileri sorgulamak üzere tanımlama bilgisinin anahtar olarak kullanılmasıdır. Bir bellek içi önbellek, bilgileri bir kullanıcıyla ilişkilendirir Azure Cache, Redis için gibi kullanarak tam ilişkisel veritabanıyla etkileşim daha hızlıdır. |
| İş ve ileti sıraya alma | Uygulamalar istek aldığında genellikle istekle ilişkili işlemlerin yürütülmesi ek zaman alır. Daha uzun süre çalışan işlemleri bir sıraya ekleyerek ertelemek ve daha sonra, muhtemelen başka bir sunucu ile işleme almak yaygın bir düzendir. Bu iş erteleme yöntemine görevi sıraya alma adı verilir. Görev sıralarını desteklemek için tasarlanmış birçok yazılım bileşeni mevcuttur. Redis için azure önbellek de iyi dağıtılmış bir sıra gibi bu amaca hizmet eder.|
| Dağıtılmış işlemler | Uygulamaların tek bir işlem (atomik) olarak bir arka uç veri deposuna karşı bir dizi komut yürütebilmesi genel bir gereksinimdir. Tüm komutlar başarılı olmalı veya tümü ilk durumuna geri döndürülmelidir. Azure önbelleği için Redis destekleyen şeklinde tek bir işlem olarak bir batch komutları yürütülürken [işlemleri](https://redis.io/topics/transactions). |

## <a name="azure-cache-for-redis-offerings"></a>Azure önbelleği için Redis teklifleri

Redis için Azure Cache aşağıdaki katmanlarda kullanılabilir:

| Katman | Açıklama |
|---|---|
Temel | Tek düğümlü bir önbellek. Bu katman birden fazla bellek boyutunu (250 MB - 53 GB) destekler. Geliştirme/test ve kritik olmayan iş yükleri için ideal bir katmandır. Temel katmanda hizmet düzeyi sözleşmesi (SLA) yoktur |
| Standart | Microsoft tarafından yönetilen iki düğümlü bir birincil/ikincil yapılandırmada çoğaltılan ve yüksek kullanılabilirlik SLA’sı (%99,9) ile sunulan önbellek. |
| Premium | Premium katmanı, Kurumsal kullanıma hazır katmandır. Premium katmanı Önbellekler daha fazla özelliği destekler ve düşük gecikmeyle daha yüksek aktarım hızına sahiptir. Premium katmanındaki önbellekler, Temel veya Standart Katmanına kıyasla daha iyi performans sağlayan daha güçlü donanımlara dağıtılır. Bu avantaj, aynı boyuttaki bir önbellek için aktarım hızının, Premium katmanında Standart katmanına göre daha yüksek olacağı anlamına gelir |

> [!TIP]
> Boyut, işleme ve premium önbelleklere sahip bant genişliği hakkında daha fazla bilgi için bkz. [Azure önbelleği için Redis SSS](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use).
>

Önbelleğiniz oluşturulduktan sonra ölçeğini daha yüksek bir katmana artırabilirsiniz. Ölçeğin daha düşük bir katmana indirilmesi desteklenmez. Ölçeklendirme hakkında adım adım yönergeler için bkz: [ölçek Azure Cache, Redis için nasıl](cache-how-to-scale.md) ve [bir ölçeklendirme işlemi otomatik hale getirmek nasıl](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

### <a name="feature-comparison"></a>Özellik Karşılaştırması

[Azure önbelleği için Redis fiyatlandırma](https://azure.microsoft.com/pricing/details/cache/) sayfası, her bir katman ayrıntılı bir karşılaştırmasını sağlar. Aşağıdaki tablo, katmana göre desteklenen özelliklerden bazılarını açıklamaya yardımcı olur:

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

* [ASP.NET Web uygulaması Hızlı Başlangıç](cache-web-app-howto.md) bir Azure önbelleği için Redis kullanan basit bir ASP.NET web uygulaması oluşturma.
* [.NET Hızlı Başlangıç](cache-dotnet-how-to-use-azure-redis-cache.md) bir Azure önbelleği için Redis kullanan bir .NET uygulaması oluşturun.
* [.NET core Hızlı Başlangıç](cache-dotnet-core-quickstart.md) bir Azure önbelleği için Redis kullanan bir .NET Core uygulaması oluşturma.
* [Node.js Hızlı Başlangıç](cache-nodejs-get-started.md) bir Azure önbelleği için Redis kullanan basit bir Node.js uygulaması oluşturun.
* [Java Hızlı Başlangıç](cache-java-get-started.md) bir Azure önbelleği için Redis kullanan basit bir Java uygulaması oluşturun.
* [Python hızlı](cache-python-get-started.md) bir Azure önbelleği için Redis kullanan bir Python uygulaması oluşturma.
