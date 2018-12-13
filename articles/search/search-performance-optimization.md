---
title: Azure arama performans ve iyileştirme dikkat edilmesi gerekenler - Azure Search
description: Teknikleri ve Azure Search performans ayarlama ve uygun ölçekte yapılandırmak için en iyi yöntemleri öğrenin.
author: LiamCavanagh
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 05/01/2017
ms.author: liamca
ms.custom: seodec2018
ms.openlocfilehash: 0a98e7f05e766d47a5ea9293409a74a6fafbf837
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53310276"
---
# <a name="azure-search-performance-and-optimization-considerations"></a>Azure arama performans ve iyileştirme konuları
Mükemmel arama deneyimi, başarı için çok sayıda mobil ve web uygulamaları için bir anahtardır. Emlak araba marketlerden çevrimiçi katalog için kullanılan için hızlı arama ve ilgili sonuçların müşteri deneyimini etkiler. Bu belge, Yardım desteklemek için ölçeklenebilirlik, karmaşık gereksinimleri olan Gelişmiş senaryolar için özellikle Azure Search verilerinizden en çok dilli almak en iyi uygulamaları keşfedin veya özel derecelendirme yöneliktir.  Ayrıca, bu belge, iç işlevleri özetler ve gerçek müşteri uygulamaları etkili bir şekilde iş yaklaşımlar ele alınmaktadır.

## <a name="performance-and-scale-tuning-for-search-services"></a>Performans ve ölçek arama hizmetlerinin ayarlama
Biz tüm arama motorlarına Bing, Google ve sundukları yüksek performanslı gibi kullanılır.  Sonuç olarak, müşteriler, arama özellikli bir web veya mobil uygulamanızı kullandığınızda, benzer performans özelliklerini beklediği.  İçin Arama performansını en iyi duruma getirme, en iyi yaklaşım bir sorgu tamamlamak ve sonuçları döndürmek için gereken süre anlamına gelmektedir gecikme süresine odaklanmak için biridir.  Arama gecikme süresi için en iyi duruma getirme zaman için önemlidir:

1. Çekme tamamlamak için bir hedef gecikme süresi (veya en uzun süreyi) tipik bir aranmasını almalıdır.
2. Oluşturma ve bir gerçek iş yükü arama hizmetinizde gecikme ücretler ölçmek için gerçekçi bir veri kümesi ile test edin.
3. Sorgular / saniye (QPS) az sayıda başlayın ve tanımlanan hedef gecikme sorgunun gecikme süresi düşene kadar testi içinde yürütülen sayısı artacaktır.  Uygulamanızın kullanımı arttıkça ölçeği planlamanıza yardımcı olması için önemli bir Kıyaslama budur.
4. Mümkün olduğunda, HTTP bağlantılarını yeniden kullanın.  Azure Search .NET SDK'sı kullanıyorsanız, örneğini yeniden yani veya [Searchındexclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient) örneği ve REST API kullanıyorsanız, tek bir HttpClient yeniden kullanmamalısınız.

Test iş yüklerini bu oluşturulurken, Azure arama, göz önünde bulundurmanız özelliklerinden bazıları vardır:

1. Azure Search hizmetinizin kullanılabilir kaynakları aşırı yüklenebilir tek seferde çok sayıda arama sorguları için anında iletme için mümkündür.  Bu durumda, HTTP 503 yanıt kodları görürsünüz.  Bu nedenle, daha fazla arama istekleri ekledikçe, gecikme süresi oranlarını farkları görmek için arama istekleri çeşitli aralıklarına başlatmak en iyisidir.
2. Azure Search içerik karşıya yükleme, genel performans ve Azure Search Hizmeti gecikme süresini etkiler.  Kullanıcıların arama yaparken, veri göndermek bekliyorsanız, bu iş yükünü hesaba testlerinizde yararlanmak önemlidir.
3. Her arama sorgusu aynı performans düzeylerinde gerçekleştirir.  Örneğin, bir belge arama veya arama öneri genellikle çok sayıda özellikleri ve filtreleri içeren bir sorgu daha hızlı gerçekleştirir.  Testlerinizi oluştururken dikkate görmeyi beklediğiniz çeşitli sorgular ele en iyisidir.  
4. Arama istekleri çeşitlemesi önemlidir, çünkü aynı arama istekleri sürekli olarak çalışırsa verileri önbelleğe alma Ara daha farklı bir sorgu ayarlayabilir daha iyi performans sağlamak başlatılır.

> [!NOTE]
> [Visual Studio Yük testi](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release) , kıyaslama gerçekleştirmek için gerçekten iyi bir şekilde Azure arama sorguları yürütmek için ihtiyacınız olacak kadar HTTP isteklerini yürütmek sağladığından test eder ve istekleri paralelleştirilmesini etkinleştirir.
> 
> 

## <a name="scaling-azure-search-for-high-query-rates-and-throttled-requests"></a>Azure Search yüksek sorgu hızları için ölçeklendirme ve istekler daraltıldı
Çok fazla daraltılmış istekler alıyorsanız veya daha yüksek sorgu yük hedef gecikme süresi ücretlerinizi aşan, gecikme süresi oranlarını iki yoldan biriyle azaltmak için göz atabilirsiniz:

1. **Çoğaltmaları artırın:**  Azure Search'ün Yük Dengeleme isteklerini birden fazla kopya karşı izin vererek, verilerin bir kopyasını gibi bir çoğaltmadır.  Tüm Yük Dengeleme ve yinelemeleri arasında veri çoğaltma Azure Search tarafından yönetilir ve herhangi bir zamanda hizmetiniz için ayrılan çoğaltmaların sayısı değiştirebilirsiniz.  En fazla 12 standart arama hizmeti çoğaltma ve temel arama hizmetinde 3 çoğaltma ayırabilirsiniz. Çoğaltmaları olabilir herhangi birinden ayarlanmış [Azure portalında](search-create-service-portal.md) veya [PowerShell](search-manage-powershell.md).
2. **Arama katmanı artırın:**  Azure arama geldiğinde bir [katmanları sayısı](https://azure.microsoft.com/pricing/details/search/) ve her biri bu katmanlar, çeşitli performans düzeylerini sunar.  Bazı durumlarda, çoğaltmaları kullanıma bile ayarlanma zaman olan katman yeterince düşük gecikme süresi oranlarını sağlayamayacağını kadar sorgular olabilir.  Bu durumda, çok sayıda belge ve son derece yüksek sorgu iş yükleri ile senaryoları için oldukça uygundur Azure Search S3 katmanı gibi daha yüksek arama katmanlardan birine yararlanarak göz önünde bulundurun isteyebilirsiniz.

## <a name="scaling-azure-search-for-slow-individual-queries"></a>Azure Search yavaş her sorgu için ölçeklendirme
Başka bir nedenle neden gecikme süresi oranlarını yavaş olabilir, tamamlanması çok uzun sürüyor, tek bir sorgudan ' dir.  Bu durumda, çoğaltmalar ekleyerek gecikme süresi oranlarını artırır değil.  Bu durumda kullanılabilir iki seçenek vardır:

1. **Bölümler artırmak** verilerinizi ek kaynaklar arasında bölmek için bir mekanizma bölümdür.  İkinci bir bölümü eklediğinizde, bu nedenle, verilerinizi ikiye ayırma.  Bir üçüncü bölüm dizininizi üç vb. böler.  Bu ayrıca, bazı durumlarda, yavaş sorguları daha hızlı hesaplama paralelleştirilmesini nedeniyle gerçekleştireceğini etkisi vardır.  Burada da son derece düşük seçiciliği sorguların sorgularla çalışmak bu paralelleştirme gördük bazı örnekler vardır.  Bu, birçok belgelerle eşleşmesini veya model oluşturma sayıları belgeleri çok sayıda sağlamak gerektiğinde sorguları oluşur.  Çok sayıda hesaplama belgeleri uygunluğunu puanlamak için veya belge sayılarını saymak için gerekli olduğundan, ek hesaplama sağlamak için ek bölümler eklemek yardımcı olabilir.  
   
   Standart arama hizmeti 12 olarak bölümlerde en fazla ve temel arama hizmetinde 1 bölüm olabilir.  Bölümler olabilir herhangi birinden ayarlanmış [Azure portalında](search-create-service-portal.md) veya [PowerShell](search-manage-powershell.md).
2. **Sınırı yüksek kardinalite alanlar:** Yüksek bir kardinalite alan modellenebilir veya önemli sayıda benzersiz değere sahip ve sonuç olarak, kaynaklar üzerinde sonuçları hesaplamak için çok fazla alan filtrelenebilir alanı oluşur.   Örneğin, çoğu belgeden belgeye değerlerin benzersiz olduğundan bir ürün Kimliğine veya açıklamasına alan modellenebilir/filtrelenebilir olarak ayarlamak için yüksek kardinalite hale getirir. Mümkün olduğunda, yüksek bir kardinalite alanların sayısını sınırlayın.
3. **Arama katmanı artırın:**  Daha yüksek bir Azure arama katmanı, en fazla taşıma yavaş sorgu performansını artırmak için başka bir yolu olabilir.  Her daha yüksek bir katman, daha hızlı CPU'lar ve sorgu performansı üzerindeki olumlu bir etkisi olan daha fazla bellek de sağlar.

## <a name="scaling-for-availability"></a>Kullanılabilirlik için ölçeklendirme
Çoğaltmalar yalnızca sorgu gecikme süresini azaltmak ancak yüksek kullanılabilirlik için de izin verebilirsiniz.  Tek bir çoğaltma düzenli kapalı kalma süresi nedeniyle sunucu yeniden başlatmaları yazılım güncelleştirmelerinden sonra veya meydana gelen diğer bakım olayları için beklemelisiniz.  Sonuç olarak, uygulamanızı yazma işlemleri (Dizin olayları) yanı sıra arama (sorgular) yüksek kullanılabilirlik gerektiriyorsa göz önünde bulundurmanız önemlidir.  Azure arama, tüm Ücretli Arama teklifleri aşağıdaki özelliklere sahip SLA seçeneklerini sunar:

* salt okunur iş yüklerinin (sorgular) yüksek kullanılabilirlik için 2 çoğaltması
* okuma-yazma iş yüklerinin (sorgu ve dizin oluşturma) yüksek kullanılabilirlik için en az 3 çoğaltma

Bunun hakkında daha fazla ayrıntı için lütfen [Azure Search hizmet düzeyi sözleşmesi](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Çoğaltmalar verilerinizin kopyalarını olduğundan, birden fazla çoğaltma karşı diğer kopyalarla yürütülecek devam etmek sorgular sağlarken bir zaman yeniden başlatma işlemleri ve bakımı karşı bir çoğaltma makinesi Azure Search sağlar.  Bu nedenle, bu kapalı kalma süresi artık bir daha az veri kopyası karşı yürütülecek olan sorgu nasıl etkileyebilir göz önünde bulundurmanız gerekir.

## <a name="scaling-geo-distributed-workloads-and-provide-geo-redundancy"></a>Coğrafi olarak dağıtılmış iş yüklerini ölçeklendirebilir ve coğrafi olarak yedeklilik sağlayın
Coğrafi olarak dağıtılmış iş yükleri için Azure Search hizmetinizin barındırıldığı veri merkezine gölgeden uzak bulunan kullanıcılara daha yüksek gecikme süresi oranları olduğunu bulabilirsiniz.  Bu nedenle, bu kullanıcılar için daha yakından yakınlık bulunan bölgeler birden çok arama hizmetleri sağlamak önemlidir.  Azure arama, coğrafi olarak çoğaltılan Azure Search dizinlerini otomatikleştirilmiş bir yöntem şu anda bölgeler arasında sağlamaz, ancak bu işlemi uygulamak ve yönetmek basit hale getirmek kullanılan bazı teknik vardır. Bu, sonraki birkaç bölümde özetlenmiştir.

Bu örnekte görüldüğü gibi düşük gecikme süresi sağlayan Azure Search hizmeti için bir kullanıcı nereye yönlendirileceğini iki veya daha fazla bölgede iki veya daha fazla dizinler için amacı, arama hizmetleri coğrafi olarak dağıtılmış bir dizi verilmiştir:

   ![Bölgelere göre hizmetler arası sekmesinde][1]

### <a name="keeping-data-in-sync-across-multiple-azure-search-services"></a>Verilerini birden çok Azure Search hizmetinde eşitlenmiş durumda tutmanın
Kullanarak ya da oluşan dağıtılmış arama hizmetlerinizi eşitlenmiş tutmak için iki seçenek vardır [Azure Search dizin oluşturucu](search-indexer-overview.md) veya Push API (olarak da adlandırılan [Azure Search REST API'sine](https://docs.microsoft.com/rest/api/searchservice/)).  

### <a name="azure-search-indexers"></a>Azure Search dizin Oluşturucularında
Azure arama dizin oluşturucusu kullanıyorsanız, zaten veri değişikliklerini Azure SQL DB veya Azure Cosmos DB gibi merkezi bir veri deposu alma. Yeni bir arama hizmeti oluşturduğunuzda, yalnızca bu aynı veri deposuna işaret eden hizmet için yeni bir Azure Search dizin oluşturucu oluşturabilir. Yeni değişiklikler veri deposuna gelen olduğunda bu şekilde, bunlar daha sonra çeşitli dizin oluşturucu tarafından dizine alınır.  

Mimari aşağıdaki gibi görünür bir örnek aşağıda verilmiştir.

   ![Tek veri kaynağı ile dağıtılmış bir dizin oluşturucu ve hizmet birleşimleri][2]

### <a name="push-api"></a>Push API
Azure arama Push API kullanıyorsanız [Azure Search dizininizi içerikte güncelleştirme](https://docs.microsoft.com/rest/api/searchservice/update-index), bir güncelleştirme gerekli olduğunda değişiklikler için tüm arama hizmetlerinin ileterek, çeşitli arama hizmetlerinizi eşitlenmiş tutabilirsiniz.  Bunu yaparken burada bir arama hizmeti için bir güncelleştirme başarısız olur ve bir veya daha fazla güncelleştirme başarılı durumlarını işleyebildiğinden emin olmak önemlidir.

## <a name="leveraging-azure-traffic-manager"></a>Azure Traffic Manager'ı kullanma
[Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) istekleri sonra birden fazla Azure Search Hizmetleri tarafından desteklenen birden çok coğrafi konumda bulunan Web sitelerine izin verir.  Azure Search'ın kullanılabilir olduğundan emin olun ve kullanıcıları alternatif arama hizmetleri kesinti olması durumunda yönlendirmek için araştırma, Traffic Manager'ın avantajlarından biri.  Ayrıca, arama istekleri aracılığıyla Azure Web siteleri yönlendirdiğinden, Azure Traffic Manager, Web sitesi oluşturan olduğu Bakiye durumları yüklenemedi, ancak Azure Search sağlar.  Traffic Manager yararlanan hangi mimarisinin bir örnek aşağıda verilmiştir.

   ![Çapraz-sekmesi merkezi Traffic Manager ile bölgeye göre Hizmetleri][3]

## <a name="monitoring-performance"></a>Performans izleme
Azure Search, hizmetinizin performansını izleyin ve analiz olanağı sunar [arama trafiği analizi (STA)](search-traffic-analytics.md). STA isteğe bağlı olarak tek tek arama işlemlerinin yanı sıra toplanan ölçümler sonra analiz için işlenen veya Power BI'da görselleştirilir. bir Azure depolama hesabı oturum açabilirsiniz.  STA ölçümleri kullanarak performans istatistikleri sorguları veya sorgu yanıt sürelerini ortalama sayısı gibi gözden geçirebilirsiniz.  Ayrıca, işlem günlüğü, belirli arama işlemleri detaylarına gitmek sağlar.

STA, gecikme süresi oranlarını, Azure Search açısından anlamak için değerli bir araçtır.  Günlüğe sorgu performans ölçümleri (out gönderildiğinde istenen zamandan) Azure Search'te tam olarak işlenmek üzere bir sorgu geçen süreyi dayalı olduğundan, bu gecikme sorunlarını Azure Search Hizmet tarafı veya çıkarmayı olup olmadığını belirlemek için kullanabilirsiniz hizmeti, ağ gecikmesi gelenler gibi IDE.  

## <a name="next-steps"></a>Sonraki adımlar
Her biri için fiyatlandırma katmanları ve hizmet sınırları hakkında daha fazla bilgi için bkz: [Azure Search'te hizmet sınırları](search-limits-quotas-capacity.md).

Ziyaret [kapasite planlaması](search-capacity-planning.md) birleşimleri bölüm ve çoğaltma hakkında daha fazla bilgi edinmek için.

Performans ve bazı chef'in bu makalede ele alınan iyileştirmeler uygulamak nasıl daha fazla ayrıntılı için şu videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 
> 

<!--Image references-->
[1]: ./media/search-performance-optimization/geo-redundancy.png
[2]: ./media/search-performance-optimization/scale-indexers.png
[3]: ./media/search-performance-optimization/geo-search-traffic-mgr.png
