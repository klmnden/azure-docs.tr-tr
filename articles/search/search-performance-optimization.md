---
title: Dağıtım stratejilerini ve performans - Azure Search iyileştirmek için en iyi uygulamalar
description: Teknikleri ve Azure Search performans ayarlama ve uygun ölçekte yapılandırmak için en iyi yöntemleri öğrenin.
author: LiamCavanagh
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 03/02/2019
ms.author: liamca
ms.custom: seodec2018
ms.openlocfilehash: 32352a857f0a74dc008dc1ad76b4a5951a36b956
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65024560"
---
# <a name="deployment-strategies-and-best-practices-for-optimizing-performance-on-azure-search"></a>Dağıtım stratejilerini ve Azure Arama performansını iyileştirmek için en iyi uygulamalar

Bu makalede ölçeklenebilirlik ve kullanılabilirlik için daha karmaşık gereksinimleri olan Gelişmiş senaryolar için en iyi uygulamaları açıklar. 

## <a name="develop-baseline-numbers"></a>Taban sayı geliştirin
İçin Arama performansını en iyi duruma getirme, sorgu yürütme süresini azaltmak üzerinde durmalısınız. Bunu yapmak için tipik sorgu yükünü nasıl göründüğünü bilmeniz gerekir. Aşağıdaki yönergeler, temel sorgu numaralarını geldiğinde yardımcı olabilir.

1. Çekme tamamlamak için bir hedef gecikme süresi (veya en uzun süreyi) tipik bir aranmasını almalıdır.
2. Oluşturma ve bir gerçek iş yükü arama hizmetinizde gecikme ücretler ölçmek için gerçekçi bir veri kümesi ile test edin.
3. Sorgu başına saniye (QPS) düşük bir sayı ile başlayıp ve sorgunun gecikme süresi, tanımlanan hedef gecikme düşene kadar testi içinde yürütülen sayı aşamalı olarak artırın. Uygulamanızın kullanımı arttıkça ölçeği planlamanıza yardımcı olması için önemli bir Kıyaslama budur.
4. Mümkün olduğunda, HTTP bağlantılarını yeniden kullanın. Azure Search .NET SDK'sı kullanıyorsanız, örneğini yeniden yani veya [Searchındexclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient) örneği ve REST API kullanıyorsanız, tek bir HttpClient yeniden kullanmamalısınız.
5. Bu arama dizininizi farklı kısımlarını gerçekleşir. Bu nedenle sorgu istekleri madde temini farklılık gösterir. Değişim önemlidir, çünkü aynı arama istekleri sürekli olarak çalışırsa verileri önbelleğe alma Ara daha farklı bir sorgu ayarlayabilir daha iyi performans sağlamak başlatılır.
6. Sorgu farklı türleri alabilmeniz sorgu isteği yapısını farklılık gösterir. Her arama sorgusu aynı düzeyde gerçekleştirir. Örneğin, bir belge arama veya arama öneri genellikle çok sayıda özellikleri ve filtreleri içeren bir sorgu hızlıdır. Test oluşturma üretimde beklediğiniz gibi çeşitli sorgular, yaklaşık aynı oranları içermelidir.  

Test iş yüklerini bu oluşturulurken, Azure arama, göz önünde bulundurmanız özelliklerinden bazıları vardır:

+ Mümkünse tek seferde çok sayıda arama sorguları göndererek hizmetinizi aşırı yükleme. Bu durumda, HTTP 503 yanıt kodları görürsünüz. Test sırasında bir 503 önlemek için daha fazla arama istekleri ekledikçe, gecikme süresi oranlarını farkları görmek için arama istekleri çeşitli aralıklarına başlatın.

+ Azure arama dizini oluşturma görevleri arka planda çalışmaz. Hizmetiniz, sorgu ve iş yüklerini eşzamanlı olarak dizinleme işliyorsa, giriş ya da sorgu testlerinizi dizin oluşturma işleri ya da kapalı yoğun saatler sırasında dizin oluşturma işleri çalıştırma seçenekleri keşfetmeye bu dikkate alın.

> [!NOTE]
> [Visual Studio Yük testi](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release) , kıyaslama gerçekleştirmek için gerçekten iyi bir şekilde Azure arama sorguları yürütmek için ihtiyacınız olacak kadar HTTP isteklerini yürütmek sağladığından test eder ve istekleri paralelleştirilmesini etkinleştirir.
> 
> 

## <a name="scaling-for-high-query-volume-and-throttled-requests"></a>Yüksek sorgu birimi için ölçeklendirme ve daraltılmış istekler
Çok fazla daraltılmış istekler alma ya da hedef gecikme süresi ücretlerinizi artan sorgu yük uzun gecikme süresi oranlarını iki yoldan biriyle azaltmak için göz atabilirsiniz:

1. **Çoğaltmaları artırın:**  Azure Search'ün Yük Dengeleme isteklerini birden fazla kopya karşı izin vererek, verilerin bir kopyasını gibi bir çoğaltmadır.  Tüm Yük Dengeleme ve yinelemeleri arasında veri çoğaltma Azure Search tarafından yönetilir ve herhangi bir zamanda hizmetiniz için ayrılan çoğaltmaların sayısı değiştirebilirsiniz.  En fazla 12 standart arama hizmeti çoğaltma ve temel arama hizmetinde 3 çoğaltma ayırabilirsiniz. Çoğaltmaları olabilir herhangi birinden ayarlanmış [Azure portalında](search-create-service-portal.md) veya [PowerShell](search-manage-powershell.md).
2. **Arama katmanı artırın:**  Azure arama geldiğinde bir [katmanları sayısı](https://azure.microsoft.com/pricing/details/search/) ve her biri bu katmanlar, çeşitli performans düzeylerini sunar.  Bazı durumlarda, çoğaltmaları kullanıma bile ayarlanma zaman olan katman yeterince düşük gecikme süresi oranlarını sağlayamayacağını kadar sorgular olabilir. Bu durumda, çok sayıda belge ve son derece yüksek sorgu iş yükleri ile senaryoları için uygun olan Azure Search S3 katmanı gibi daha yüksek arama katmanlardan birine yararlanarak göz önünde bulundurun isteyebilirsiniz.

## <a name="scaling-for-slow-individual-queries"></a>Yavaş her sorgu için ölçeklendirme
Başka bir nedenle yüksek gecikme süresi oranlarını tamamlanması çok uzun sürüyor, tek bir sorgudur. Bu durumda, çoğaltmalar ekleyerek yardımcı olmaz. İki şunlardır yardımcı olabilecek olası seçenekler:

1. **Bölümler artırmak** verilerinizi ek kaynaklar arasında bölmek için bir mekanizma bölümdür. İkinci bölüm ekleyerek veri ikiye ayırır, üçüncü bölüm, üç ve benzeri böler. Bir pozitif yan etkisi daha yavaş sorguları bazen daha hızlı bir şekilde paralel bilgi işlem nedeniyle, gerçekleştirmektir. Biz, belgeleri veya pek çok fazla sayıda belge sayılarını sağlayan özellikleri eşleşen sorgular gibi düşük seçiciliği sorguları paralelleştirme not almış. Belgeleri uygunluğunu puanlamak için önemli olan hesaplamayı gereklidir veya yardımcı olan belgeler, ek bölümler ekleyerek sayılarını sayma sorguları daha hızlı tamamlayın.  
   
   Standart arama hizmeti 12 olarak bölümlerde en fazla ve temel arama hizmetinde 1 bölüm olabilir.  Bölümler olabilir herhangi birinden ayarlanmış [Azure portalında](search-create-service-portal.md) veya [PowerShell](search-manage-powershell.md).

2. **Sınırı yüksek kardinalite alanlar:** Yüksek bir kardinalite alana bir modellenebilir veya önemli sayıda benzersiz değere sahip ve sonuç olarak, önemli miktarda kaynak sonuçları hesaplarken tüketir filtrelenebilir alanı oluşur. Belgeden belgeye değerleri çoğunu benzersiz olduğu için örneğin, bir ürün Kimliğine veya açıklamasına alan modellenebilir/filtrelenebilir olarak ayarlama yüksek kardinalite sayılacaktır. Mümkün olduğunda, yüksek bir kardinalite alanların sayısını sınırlayın.

3. **Arama katmanı artırın:**  Daha yüksek bir Azure arama katmanı, en fazla taşıma yavaş sorgu performansını artırmak için başka bir yolu olabilir. Her daha yüksek bir katman, daha hızlı CPU'lara ve ikisi için de bir pozitif sorgu performansı üzerindeki etkisi daha fazla bellek sağlar.

## <a name="scaling-for-availability"></a>Kullanılabilirlik için ölçeklendirme
Çoğaltmalar yalnızca sorgu gecikme süresini azaltmak ancak yüksek kullanılabilirlik için de izin verebilirsiniz. Tek bir çoğaltma düzenli kapalı kalma süresi nedeniyle sunucu yeniden başlatmaları yazılım güncelleştirmelerinden sonra veya meydana gelen diğer bakım olayları için beklemelisiniz.  Sonuç olarak, uygulamanızı yazma işlemleri (Dizin olayları) yanı sıra arama (sorgular) yüksek kullanılabilirlik gerektiriyorsa göz önünde bulundurmanız önemlidir. Azure arama, tüm Ücretli Arama teklifleri aşağıdaki özelliklere sahip SLA seçeneklerini sunar:

* salt okunur iş yüklerinin (sorgular) yüksek kullanılabilirlik için 2 çoğaltması
* okuma-yazma iş yüklerinin (sorgu ve dizin oluşturma) yüksek kullanılabilirlik için en az 3 çoğaltma

Bunun hakkında daha fazla ayrıntı için lütfen [Azure Search hizmet düzeyi sözleşmesi](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Çoğaltmalar verilerinizin kopyalarını olduğundan, birden fazla çoğaltma Azure Search yeniden başlatma işlemleri ve bakımı karşı bir çoğaltma, diğer çoğaltmalarda devam etmek için sorgu yürütmeye izin verirken makine sağlar. Buna karşılık, çoğaltmaları hemen alırsa, yinelemeler altında kullanılan bir kaynak olan varsayılarak sorgu performansında ücretler.

## <a name="scaling-for-geo-distributed-workloads-and-geo-redundancy"></a>Coğrafi olarak dağıtılmış iş yüklerini ve coğrafi olarak yedeklilik için ölçeklendirme
Coğrafi olarak dağıtılmış iş yükleri için Azure Search barındıran veri merkezindeki gölgeden uzak bulunan kullanıcılar daha yüksek gecikme süresi oranları sahip olur. Bu kullanıcılar için daha yakından yakınlık bölgelerde birden çok arama hizmetleri sağlamak için bir risk azaltma var. Azure arama, coğrafi olarak çoğaltılan Azure Search dizinlerini otomatikleştirilmiş bir yöntem şu anda bölgeler arasında sağlamaz, ancak bu işlemi uygulamak ve yönetmek basit hale getirmek kullanılan bazı teknik vardır. Bu, sonraki birkaç bölümde özetlenmiştir.

Arama Hizmetleri coğrafi olarak dağıtılmış bir dizi Bu örnekte görüldüğü gibi düşük gecikme süresi sağlayan Azure Search hizmeti için kullanıcının yönlendirildiği iki veya daha fazla bölgede iki veya daha fazla dizinleri sahip olmaktır:

   ![Bölgelere göre hizmetler arası sekmesinde][1]

### <a name="keeping-data-in-sync-across-multiple-azure-search-services"></a>Verilerini birden çok Azure Search hizmetinde eşitlenmiş durumda tutmanın
Kullanarak ya da oluşan dağıtılmış arama hizmetlerinizi eşitlenmiş tutmak için iki seçenek vardır [Azure Search dizin oluşturucu](search-indexer-overview.md) veya Push API (olarak da adlandırılan [Azure Search REST API'sine](https://docs.microsoft.com/rest/api/searchservice/)).  

### <a name="use-indexers-for-updating-content-on-multiple-services"></a>Birden çok hizmet içeriği güncelleştirmek için dizin oluşturucular kullanma

Bir hizmette, dizin oluşturucu zaten kullanıyorsanız aynı konumdan veri çekmek aynı veri kaynağı nesnesi kullanmak için ikinci bir hizmet ikinci bir dizin oluşturucusunu yapılandırabilirsiniz. Her bölgede her hizmetin kendi dizin oluşturucu ve bir hedef dizin vardır (search dizininizi paylaşılmaz, yinelenen veriler anlamına gelir), ancak her dizin oluşturucu, aynı veri kaynağına başvuruyor.

Mimari aşağıdaki gibi görünür, üst düzey bir görseli aşağıda verilmiştir.

   ![Tek veri kaynağı ile dağıtılmış bir dizin oluşturucu ve hizmet birleşimleri][2]

### <a name="use-rest-apis-for-pushing-content-updates-on-multiple-services"></a>Birden çok hizmetten içerik güncelleştirmeleri göndermek için REST API'lerini kullanma
Azure Search REST API'sine kullanıyorsanız [Azure Search dizininizi içerik anında](https://docs.microsoft.com/rest/api/searchservice/update-index), bir güncelleştirme gerekli olduğunda değişiklikler için tüm arama hizmetlerinin ileterek, çeşitli arama hizmetlerinizi eşitlenmiş tutabilirsiniz. Kodunuzda, burada bir arama hizmeti için bir güncelleştirme başarısız olur ancak diğer arama hizmetlerinin başarısız durumlarını işleyebildiğinden emin olun.

## <a name="leverage-azure-traffic-manager"></a>Azure Traffic Manager yararlanın
[Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) istekleri sonra birden fazla Azure Search Hizmetleri tarafından desteklenen birden çok coğrafi konumda bulunan Web sitelerine izin verir. Azure Search'ın kullanılabilir olduğundan emin olun ve kullanıcıları alternatif arama hizmetleri kesinti olması durumunda yönlendirmek için araştırma, Traffic Manager'ın avantajlarından biri. Ayrıca, arama istekleri aracılığıyla Azure Web siteleri yönlendirdiğinden, Azure Traffic Manager, Web sitesi oluşturan olduğu Bakiye durumları yüklenemedi, ancak Azure Search sağlar. Traffic Manager yararlanan hangi mimarisinin bir örnek aşağıda verilmiştir.

   ![Çapraz-sekmesi merkezi Traffic Manager ile bölgeye göre Hizmetleri][3]

## <a name="monitor-performance"></a>Performansı izleme
Azure Search, hizmetinizin performansını izleyin ve analiz olanağı sunar [trafik analizi arama](search-traffic-analytics.md). Bu işlevi etkinleştirmek ve istemci uygulamanızı izleme ekleyin, ardından analiz için işlenen ya da Power BI'da görselleştirilir. bir Azure depolama hesabı için tek tek arama işlemlerinin yanı sıra toplanan ölçümler isteğe bağlı olarak kaydedebilirsiniz. Performans istatistiklerini ortalama sayısı sorguları veya sorgu yanıt süreleri gibi ölçümleri yakalamaları bu şekilde sağlayın. Ayrıca, işlem günlüğü, belirli arama işlemleri detaylarına gitmek sağlar.

Trafik analizi, Azure Search açısından gecikme süresi oranlarını anlamak için kullanışlıdır. Günlüğe sorgu performans ölçümleri (out gönderildiğinde istenen zamandan) Azure Search'te tam olarak işlenmek üzere bir sorgu geçen süreyi dayalı olduğundan, bu gecikme sorunlarını Azure Search Hizmet tarafı veya çıkarmayı olup olmadığını belirlemek için kullanabilirsiniz hizmeti, ağ gecikmesi gelenler gibi IDE.  

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
