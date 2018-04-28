---
title: Azure arama performans ve en iyi duruma getirme konuları | Microsoft Docs
description: Azure Search performansı ayarlamak ve en iyi ölçeği yapılandırın
author: LiamCavanagh
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 05/01/2017
ms.author: liamca
ms.openlocfilehash: 89c0352723f1ed00784250b566902028af853d10
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="azure-search-performance-and-optimization-considerations"></a>Azure arama performans ve en iyi duruma getirme konuları
Harika arama deneyimi, birçok mobil için başarı ve web uygulamaları için bir anahtardır. Gayrimenkul araba Pazar çevrimiçi katalog için kullanılan için hızlı arama ve ilgili sonuçlar müşteri deneyimini etkiler. Bu belge, ölçeklenebilirlik, karmaşık gereksinimlerine Gelişmiş senaryolar için özellikle Azure Search dışında en çok dilli alma için en iyi uygulamaları destekleyen Bul Yardım veya özel bir derecelendirme yöneliktir.  Ayrıca, bu belgede iç Ayrıntılar özetlemektedir ve gerçek müşteri uygulamalarında etkili yaklaşımlar kapsar.

## <a name="performance-and-scale-tuning-for-search-services"></a>Performans ve ölçek arama hizmetleri için ayarlama
Biz tüm arama motorları Bing ve Google ve bunlar sunar yüksek performans gibi için kullanılır.  Sonuç olarak, müşteriler arama etkin web veya mobil uygulama kullandığınızda, benzer performans özellikleri istedikleri.  İçin arama performansı en iyi duruma getirme, en iyi yaklaşımın tamamlamak ve sonuçları döndüren bir sorgu gereken süredir gecikme üzerinde odaklanmaya biridir.  İçin arama gecikme süresini en iyi duruma getirme zaman için önemlidir:

1. Bir hedef gecikme (veya en fazla süreyi), tipik bir arama isteği tamamlamak için yapması gereken seçin.
2. Oluşturun ve bu gecikme oranları ölçmek için gerçekçi bir veri kümesi ile arama hizmetinizi karşı gerçek bir iş yükü test edin.
3. Sorgular (QPS) saniyede az sayıda başlayın ve sorgu gecikmesi tanımlanan hedef gecikme düşene kadar testinde yürütülen numarası büyümeye devam eder.  Uygulamanızın kullanımını büyüdükçe için ölçek planlamanıza yardımcı olması için önemli bir Kıyaslama budur.
4. Mümkün olduğunda, HTTP bağlantılarını yeniden kullanabilirsiniz.  Azure Search .NET SDK'sı kullanıyorsanız, bu örneğini yeniden gelir veya [Searchındexclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient) örneği ve REST API kullanıyorsanız, tek bir HttpClient yeniden kullanmamalısınız.

Bunlar oluşturulurken test iş yükleri, Azure Search göz önünde bulundurmanız bazı özellikleri vardır:

1. Azure Search hizmetinizin kullanılabilir kaynakları çok, aynı anda birçok arama sorgular için anında iletme için mümkündür.  Bu durumda, HTTP 503 yanıt kodları görürsünüz.  Bu nedenle, daha fazla arama istekleri ekledikçe gecikme oranları farklılıkları görmek için arama istekleri çeşitli aralıklarıyla başlatmak en iyisidir.
2. Azure Search içerik karşıya genel performans ve Azure Search Hizmeti gecikme etkiler.  Kullanıcıların aramaları gerçekleştirirken veri göndermesini bekliyorsanız, bu iş yükünü hesaba testlerinizde almanız önemlidir.
3. Her arama sorgusu aynı performans düzeylerinde gerçekleştirir.  Örneğin, bir belge arama veya arama öneri genellikle çok sayıda modelleri ve filtreleri içeren bir sorgu daha hızlı gerçekleştirir.  Testlerinizi oluştururken dikkate görmeyi beklediğiniz çeşitli sorguları olabilmesi en iyisidir.  
4. Arama istekleri çeşitlemesi önemlidir, çünkü aynı arama istekleri sürekli olarak çalıştırırsanız verileri önbelleğe alma daha farklı bir sorgu ayarlayabilir daha görüntü daha iyi performans sağlamak başlatılır.

> [!NOTE]
> [Visual Studio Yük testi](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release) , kıyaslama gerçekleştirmek için gerçekten iyi bir yöntem testleri, Azure arama sorguları yürütmek için gereken HTTP isteklerini yürütmek üzere verdiğinden ve istekleri paralelleştirme sağlar.
> 
> 

## <a name="scaling-azure-search-for-high-query-rates-and-throttled-requests"></a>Azure Search yüksek sorgu hızları için ölçekleme ve istekleri kısıtlanan
Çok fazla daraltılmış isteklerini almak ya da hedef gecikme ücretlerinizi artırılmış sorgu yük aşan gecikme oranları iki yoldan biriyle azaltmak için arayabilirsiniz:

1. **Artış çoğaltmaları:** Yük Dengeleme isteklerini birden çok kopya karşı Azure Search izin vererek, verilerin bir kopyasını bir çoğaltma gibidir.  Tüm Yük Dengeleme ve çoğaltmalar arasında verileri çoğaltmasını Azure arama tarafından yönetilir ve herhangi bir zamanda hizmetiniz için ayrılan çoğaltmaların sayısı değiştirebilirsiniz.  Standard arama hizmeti olarak 12 çoğaltmaları ve temel arama hizmeti 3 yinelemede kadar ayırabilirsiniz. Çoğaltmaları olabilir herhangi birinden ayarlanmış [Azure portal](search-create-service-portal.md) veya [PowerShell](search-manage-powershell.md).
2. **Artış arama katmanı:** Azure arama geldiğinde bir [katmanları sayısı](https://azure.microsoft.com/pricing/details/search/) ve bu katmanların her çeşitli performans düzeylerini sunar.  Bazı durumlarda bile çıkışı çoğaltmaları kullanımının zaman bulunduğunuzu katmanı yeterince düşük gecikme süresi oranları sağlayamayacağını çok fazla sayıda sorgular olabilir.  Bu durumda, bir çok sayıda belgeleri ve son derece yüksek sorgu iş yükleri ile senaryoları için uygundur Azure arama S3 katmanı gibi daha yüksek arama katmanı yararlanarak göz önünde bulundurun isteyebilirsiniz.

## <a name="scaling-azure-search-for-slow-individual-queries"></a>Azure Search yavaş tek tek sorgular için ölçeklendirme
Başka bir nedeni neden gecikme oranları yavaş olabilir, tamamlanması çok uzun sürüyor tek bir sorgudan olmasıdır.  Bu durumda, çoğaltmaları ekleme gecikme hızlarını artırır değil.  Bu durumda kullanılabilen iki seçenek vardır:

1. **Bölümler artırmak** bir bölüm verilerinizi ek kaynaklar arasında bölmek için bir mekanizmadır.  İkinci bir bölüm eklediğinizde, bu nedenle, verilerinizi ikiye böl.  Üçüncü bir bölüm dizininizi üç vb. ayırır.  Bu da bazı durumlarda, yavaş sorguları daha hızlı hesaplama paralelleştirme nedeniyle gerçekleştireceğini etkisi yoktur.  İş de son derece düşük seçiciliği sorgular olan sorguları ile bu paralelleştirme burada anlatıldığı bazı örnekler verilmiştir.  Bu, çok sayıda belge eşleşen veya olduğunu sayıları belgeleri çok sayıda sağlamak gerektiği zaman sorguları oluşur.  Çok fazla hesaplama belgeleri uygunluğunu puan veya belgeleri sayıda saymak için gerekli olduğundan, ek hesaplama sağlamak için ek bölümleri ekleme yardımcı olabilir.  
   
   En çok Standard arama hizmeti olarak 12 bölüme ve 1 temel arama hizmeti bölümünde olabilir.  Bölümler olabilir herhangi birinden ayarlanmış [Azure portal](search-create-service-portal.md) veya [PowerShell](search-manage-powershell.md).
2. **Sınır yüksek nicelik alanlar:** modellenebilir veya önemli sayıda benzersiz değere sahip ve sonuç olarak, çok fazla kaynak üzerinde sonuçları işlem alan filtrelenebilir alan yüksek kardinalite alan oluşur.   Belge değerleri belgeye çoğunu benzersiz olduğundan Örneğin, bir ürün kimliği veya açıklama alanı modellenebilir/filtrelenebilir olarak ayarlamak için yüksek kardinalite olmaması anlamına gelir. Mümkün olduğunda, yüksek kardinalite alan sayısını sınırlayın.
3. **Artış arama katmanı:** kadar daha yüksek bir Azure Search katmanı yavaş sorguların performansını artırmak için başka bir yolu olabilir taşıma.  Her daha yüksek katman daha hızlı CPU'lar ve sorgu performansına pozitif bir etkisi olan daha fazla bellek de sağlar.

## <a name="scaling-for-availability"></a>Kullanılabilirlik için ölçeklendirme
Çoğaltmaları değil yalnızca sorgu gecikmesi azaltmaya yardımcı olmak ancak yüksek kullanılabilirlik için de izin verebilirsiniz.  Tek bir çoğaltma ile ortaya çıkar diğer bakım olayları veya yazılım güncelleştirmelerini sonra sunucu yeniden başlatma nedeniyle düzenli kapalı kalma süresini beklemelisiniz.  Sonuç olarak, uygulamanızı yazma (dizin oluşturma olayları) yanı sıra aramaları (sorgular), yüksek kullanılabilirlik gerektiriyorsa dikkate almak önemlidir.  Azure arama aşağıdaki özelliklere sahip tüm Ücretli Arama tekliflerini SLA seçenekleri sunar:

* salt okunur iş yüklerinin (sorgular) yüksek kullanılabilirlik için 2 çoğaltma
* okuma-yazma iş yüklerinin (sorgu ve dizin oluşturma) yüksek kullanılabilirlik için 3 veya daha fazla yineleme

Bunun hakkında daha fazla ayrıntı için lütfen ziyaret [Azure Search hizmet düzeyi sözleşmesi](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Çoğaltmaları verilerinizin kopyasını olduğundan, birden çok çoğaltma yeniden başlatmalar ve Bakım bir çoğaltma karşı birer birer karşı diğer çoğaltmaları yürütülecek devam etmek sorguları verirken makine Azure Search sağlar.  Bu nedenle, bu kapalı kalma süresi artık daha az verisinin bir kopyası karşı yürütülecek sorguları nasıl etkileyebilir göz önüne almanız gerekir.

## <a name="scaling-geo-distributed-workloads-and-provide-geo-redundancy"></a>Coğrafi olarak dağıtılan iş yükleri ölçekleme ve coğrafi artıklık sağlamak
Coğrafi olarak dağıtılan iş yükleri için Azure Search hizmetinizin barındırıldığı veri merkezi gölgeden uzak bulunan kullanıcılara yüksek gecikme hızları olduğunu bulabilirsiniz.  Bu nedenle, bu kullanıcılar için daha yakın bir yerde konumlandırıldığında olan bölgeler birden çok arama hizmetleri sağlamak önemlidir.  Azure Search'te Azure Search dizinlerini coğrafi çoğaltma otomatik olarak yöntemi şu anda bölgeler arasında sağlamaz, ancak bu işlem uygulamak ve yönetmek basit hale getirmek kullanılabilir bazı teknikler vardır. Bu sonraki birkaç bölümlerde özetlenmiştir.

Bu örnekte görüldüğü gibi en düşük gecikme sağlayan Azure Search hizmeti için bir kullanıcı burada yönlendirilir iki veya daha fazla bölgelerdeki iki veya daha fazla dizinler kullanılabilir olması için arama hizmetleri coğrafi olarak dağıtılan bir dizi belirtilir:

   ![Bölgeye göre hizmetleri arası sekmesinde][1]

### <a name="keeping-data-in-sync-across-multiple-azure-search-services"></a>Veriler arasında birden fazla Azure arama hizmetleri eşitlenmiş şekilde kalmasının
Ya da kullanarak oluşan dağıtılmış arama hizmetlerinizi eşitlenmiş tutmak için iki seçenek vardır [Azure Search dizin oluşturucusunu](search-indexer-overview.md) veya itme API (olarak da adlandırılan [Azure Search REST API'sini](https://docs.microsoft.com/rest/api/searchservice/)).  

### <a name="azure-search-indexers"></a>Azure Search'te dizin oluşturucular
Azure Search dizin oluşturucusunu kullanıyorsanız, zaten veri değişiklikleri merkezi bir veri deposu Azure SQL DB ya da Azure Cosmos DB gibi alma. Yeni bir arama hizmeti oluşturduğunuzda, yalnızca bu aynı veri deposuna işaret bu hizmet için yeni bir Azure Search dizin oluşturucusunu oluşturabilir. Yeni değişiklikler veri deposuna gelen olduğunda bu şekilde, bunlar daha sonra çeşitli dizin oluşturucu tarafından dizinlenir.  

Bu mimari aşağıdaki gibi görünür, örnek aşağıda verilmiştir.

   ![Tek veri kaynağıyla dağıtılmış dizin oluşturucu ve hizmet birleşimler][2]

### <a name="push-api"></a>Anında iletme API
Azure Search itme API kullanıyorsanız [güncelleştirme Azure Search dizininizi içeriği](https://docs.microsoft.com/rest/api/searchservice/update-index), bir güncelleştirme gerekli olduğunda değişiklikler için tüm arama hizmetlerinin ileterek, çeşitli arama hizmetleri eşitlenmiş tutabilirsiniz.  Bunu yaparken bir arama hizmeti için bir güncelleştirme başarısız olduğu ve bir veya daha fazla güncelleştirme başarılı durumlarında emin olmak önemlidir.

## <a name="leveraging-azure-traffic-manager"></a>Yararlanmayı Azure trafik Yöneticisi
[Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) sonra birden fazla Azure arama hizmetleri tarafından desteklenen birden çok coğrafi konumda bulunan Web siteleri için rota isteklere izin verir.  Kullanılabilir olduğundan emin olun ve kullanıcıları alternatif arama hizmetleri kapalı kalma süresi durumunda yönlendirmek için Azure Search araştırma, trafik Yöneticisi'nin avantajlarından biri.  Ayrıca, arama istekleri aracılığıyla Azure Web siteleri yönlendirme, Azure trafik Yöneticisi, Web sitesi yukarı olduğu Bakiye durumlarda yüklemek için ancak Azure Search sağlar.  Trafik Yöneticisi yararlanır hangi mimarisi bir örneği burada verilmiştir.

   ![Çapraz-sekmesinde, bölgeye göre Hizmetleri Merkezi Traffic Manager ile][3]

## <a name="monitoring-performance"></a>Performans izleme
Azure arama çözümlemek ve hizmeti aracılığıyla performansını izleme olanağı sağlar [arama trafiği Analytics (STA)](search-traffic-analytics.md). STA isteğe bağlı olarak tek tek arama işlemlerinin yanı sıra toplanan ölçümler sonra analiz için işlenen ya da Power BI'da görselleştirilen bir Azure Storage hesabı oturum açabilirsiniz.  STA ölçümleri'ni kullanarak performans istatistikleri sorgular veya sorgusu yanıt sürelerini ortalama sayısı gibi gözden geçirebilirsiniz.  Ayrıca, işlem günlüğü, belirli arama işlemlerinin ayrıntılara ayrıntıya olanak sağlar.

STA gecikme hızları, Azure Search açısından anlamak için önemli bir araçtır.  Bir sorgu Azure Search'te (out gönderildiğinde istenen zamandan) tam olarak işlenmesi için geçen süreyi oturum sorgu performans ölçümleri dayalı olduğundan, bu gecikme sorunlarına Azure Search Hizmet tarafı ya da aşımı ayarlarına olup olmadığını belirlemek için kullanabilirsiniz IDE hizmetinin gibi ağ gecikmesi uğradıysa.  

## <a name="next-steps"></a>Sonraki adımlar
Her biri için fiyatlandırma katmanlarına ve Hizmetleri sınırları hakkında daha fazla bilgi için bkz: [hizmet sınırları Azure Search'te](search-limits-quotas-capacity.md).

Ziyaret [kapasite planlaması](search-capacity-planning.md) bölüm ve çoğaltma birleşimlerini hakkında daha fazla bilgi edinmek için.

Daha fazla ayrıntıya gitme performansı ve nasıl bu makalede açıklanan iyileştirmeler uygulanacağı bazı gösterileri görmek için için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 
> 

<!--Image references-->
[1]: ./media/search-performance-optimization/geo-redundancy.png
[2]: ./media/search-performance-optimization/scale-indexers.png
[3]: ./media/search-performance-optimization/geo-search-traffic-mgr.png
