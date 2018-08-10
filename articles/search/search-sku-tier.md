---
title: Azure Search hizmeti için fiyatlandırma katmanı veya SKU seçin | Microsoft Docs
description: "Azure arama bu SKU'ların sağlanabilir: ücretsiz, temel ve standart, standart kullanılabildiği çeşitli kaynak yapılandırmaları ve kapasite düzeyleri."
services: search
author: HeidiSteen
manager: cgronlun
tags: azure-portal
ms.service: search
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: heidist
ms.openlocfilehash: f7cf471a69395cef0aef7d5dd2e3c77218bf97a3
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39715289"
---
# <a name="choose-a-pricing-tier-for-azure-search"></a>Azure arama için bir fiyatlandırma katmanı seçin

Azure Search'te bir [sağlanan hizmet](search-create-service-portal.md) belirli fiyatlandırma katmanı veya SKU. Seçenekleriniz **ücretsiz**, **temel**, veya **standart**burada **standart** yapılandırmaları ve kapasiteler içinde bulunabilir. 

Bu makalenin amacı, bir katman seçmenize yardımcı olmaktır. Tamamlayıcı niteliktedir [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/search/) ve [hizmet sınırları](search-limits-quotas-capacity.md) fatura kavramlar ve tüketim düzenlerine çeşitli katmanları ile ilişkili bir Özet sayfası. Ayrıca, en iyi hangi katmanın ihtiyaçlarınıza uygun anlamak için yinelemeli bir yaklaşım önerir. 

Katmanları kapasite değil özellikleri belirler. Çok düşük bir katmanın kapasitesini ettik, yeni bir hizmet yüksek katmandan sağlamanız gerekir ve ardından [dizinlerinizi yeniden](search-howto-reindex.md). Hiçbir aynı SKU bir hizmetten diğerine yerinde yükseltmesini yoktur.

Özellik kullanılabilirliği birincil katmanını göz önünde bulundurarak değil. Tüm katmanlarda, dahil olmak üzere **ücretsiz** katmanı, dizin oluşturucu desteği S3HD dışında özellik eşliği sunar. Ancak, dizin oluşturma ve kaynak kısıtlamaları etkili bir şekilde özellik kullanımı kapsamını sınırlayabilirsiniz. Örneğin, [bilişsel arama](cognitive-search-concept-intro.md) dizin sahip uzun süre çalışan becerileri, zaman aşımı ücretsiz bir hizmet veri kümesi çok daha düşük olacağını sürece.

> [!TIP]
> Çoğu müşteri başlayın **ücretsiz** katmanı için değerlendirme ve ardından ölçeğine geçin **standart** geliştirme için. Bir katmanı seçin sonra ve [bir arama hizmeti sağlama](search-create-service-portal.md), yapabilecekleriniz [çoğaltma ve bölüm sayısını artırmak](search-capacity-planning.md) performans ayarlama. Ne zaman ve neden kapasiteyi ayarlamak hakkında daha fazla bilgi için bkz. [performans ve iyileştirme konuları](search-performance-optimization.md).
>

## <a name="billing-concepts"></a>Faturalandırma kavramları

Katman seçimi için anlamanız gereken kavramlar kapasite tanımları, hizmet sınırları ve hizmet birimlerini içerir. 

### <a name="capacity"></a>Kapasite

Kapasite olarak yapılandırıldığı *çoğaltmaları* ve *bölümler*. 

+ Çoğaltmaları arama hizmetinin, her çoğaltma dizin yük dengeli bir kopyasını barındırdığı örnekleridir. Örneğin, 6 çoğaltma ile bir hizmeti, hizmete yüklenen her dizin 6 kopyasını içerir. 

+ Bölüm dizinleri depolamak ve aranabilir verileri'otomatik olarak böl: iki bölüm yarım, üç bölüm dizininizdeki Üçe bölme ve VS. Kapasite açısından *bölüm boyutu* birincil ayırt edici katmanlarda özelliğidir.

> [!NOTE]
> Tüm **standart** destek katmanları [esnek birleşimleri çoğaltma ve bölümler](search-capacity-planning.md#chart) destesinin [hızı veya depolama sisteminizin ağırlık](search-performance-optimization.md) Bakiye değiştirerek. **Temel** üç çoğaltmaları yedeklemek için yüksek kullanılabilirlik ancak yalnızca bir bölüm sunar. **Ücretsiz** katmanları olarak ayrılmış kaynaklarda sağlamaz: bilgi işlem kaynakları, birden çok ücretsiz hizmetler tarafından paylaşılır.

### <a name="search-units"></a>Arama birimleri

Anlamak için en önemli fatura kavram bir *arama birimi* (SU) olan Azure Search yönelik faturalandırma birimidir. Azure Search çoğaltmaları hem işlevi bölümlere bağlı olduğundan, biri veya diğeri faturalandırmak için anlam ifade etmez. Bunun yerine, her iki bileşik üzerinde üzerinden faturalandırılır. Formulaically, bir SU çoğaltma ve bir hizmet tarafından kullanılan bölümler ürünüdür: (R X P SU =). En az 1 SU (bir çoğaltma bir bölüm ile çarpılmış) ile her hizmeti başlatılır, ancak daha gerçekçi bir model 9 SUs faturalandırılır Yineleme 3, 3 bölümlü bir hizmet olabilir. 

Her katman aşamalı olarak daha yüksek kapasite sunmasına karşın, rest yedekte bulunduran bir kısmını çevrimiçi, toplam kapasite getirebilirsiniz. Faturalama bakımından, bölümler ve çoğaltmalar çevrimiçi, hesaplanmış, aslında ödersiniz belirleyen SU formülü kullanarak Getir sayısıdır.

Faturalandırma saatlik SU her bir katman farklı bir oran olması hızıdır. Her katmanın bulunabilir için derecelendirir [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/search/).

### <a name="limits"></a>Sınırlar

Dizin, dizin oluşturucular ve benzeri gibi ana bilgisayar kaynakları Hizmetleri. Her katman uygular [hizmet sınırları](search-limits-quotas-capacity.md) miktarı kaynakların oluşturabilirsiniz. Bu nedenle, dizinleri (ve diğer nesneler) sayısı için üst sınır ikinci ayırt edici katmanlarda özelliğidir. Her seçeneği portalında gözden geçirirken, dizinlerin sayısı sınırlamaları unutmayın. Uzmanlık becerileri, dizin oluşturucular ve veri kaynakları gibi diğer kaynaklar için dizin sınırları pegged.

## <a name="consumption-patterns"></a>Tüketim modelleri

Çoğu müşteri başlayın **ücretsiz** hizmeti, hangi süresiz olarak saklayın ve aşağıdakilerden birini seçin **standart** katmanları önemli geliştirme veya üretim iş yükleri için. 

![Azure search katmanları](./media/search-sku-tier/tiers.png "Azure Search'ü fiyatlandırma katmanları")

Her iki uçta **temel** ve **S3 HD** önemli, ancak alışılmadık tüketim düzenlerine için mevcut. **Temel** küçük üretim iş yükleri için olan: SLA, sunduğu ayrılmış kaynakları, yüksek oranda kullanılabilir, ancak 2 GB toplam belirtti uygun depolama. Bu katman müşterileri için tasarlanmış kimin tutarlı bir şekilde kullanılan kullanılabilir kapasitenin altında. Şimdiye kadar sonunda **S3 HD** iş yükleri için iş ortakları, ISV'lerin tipik olarak [çok kiracılı çözüm](search-modeling-multitenant-saas-applications.md), ya da çok sayıda küçük dizinler için çağırma herhangi bir yapılandırma. Bu genellikle Optional olduğunda **temel** veya **S3 HD** katmanı sağ uygun olduğu halde onay istiyorsanız için gönderebilir [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) veya [Azure başvurun Destek](https://azure.microsoft.com/support/options/) daha ayrıntılı yönergeler için.

Daha yaygın olarak kullanılan standart katmanlar için odak kaydırma **S1 S3** , bölüm boyutu dönüm noktalarını ve üst sınırlar sayıda dizin, dizin oluşturucular ve corollary kaynakları üzerinde kapasitesinin düzeyleri artan bir ilerleme olan:

|  | S1 | S2 | S3 |  |  |  |  |
|--|----|----|----|--|--|--|--|
| Bölüm boyutu|  25 GB | 100 GB | 250 GB |  |  |  |  |
| Dizin ve dizin oluşturucu sınırları| 50 | 200 | 200 |  |  |  |  |

**S1** seçeneği olarak ayrılmış kaynaklarda ve birden çok bölüm olduğunda ortak bir seçimdir. Hizmet başına en fazla 12 bölümler için 25 GB bölümleri ile sınırlamak üzerinde **S1** çoğaltmalar bölümlerin en üst düzeye toplam 300 GB olan (bkz [tahsis bölümleri ve çoğaltmalarını](search-capacity-planning.md#chart) daha dengeli için birleşimleri.)

Portal ve fiyatlandırma sayfalarını bölüm boyutu ve depolama odağı yerleştirir, ancak her bir katman için tüm işlem özelliklerini (disk kapasitesi, CPU hızı) fiyatı ile doğrusal olarak büyütün. Bir **S2** çoğaltma daha hızlı bir şekilde **S1**, ve **S3** hızlıdır **S2**. **S3** katmanları Kes orantısız daha hızlı bir g/ç genellikle doğrusal işlem fiyatlandırma deseni. G/ç performans sorunu olarak düşünüyorsanız bir **S3** alt katmanları daha fazlasını IOPS sağlar.

**S3** ve **S3 HD** yedeklenen aynı yüksek kapasiteli altyapı ancak her biri kendi üst sınırı farklı şekillerde ulaşır. **S3** daha küçük bir sayı çok büyük dizinlerde hedefler. Bu nedenle, kaynak veriye bağlı alt sınırı (her hizmet için 2,4 TB). **S3 HD** çok sayıda küçük dizinleri hedefler. 1.000 dizinlere **S3 HD** dizin kısıtlamalarını biçiminde sınıra ulaşana. Eğer bir **S3 HD** 1. 000'den fazla dizinleri gerektirir müşteri nasıl ilerleyeceğiniz hakkında bilgi için Microsoft Support başvurun.

> [!NOTE]
> Daha önce belge limitleri önemli bir unsur olan, ancak artık Ocak 2018'den sonra sağlanan birçok Azure Search hizmeti için geçerli değildir. Belge limitleri hala geçerli koşullar hakkında daha fazla bilgi için bkz. [hizmet sınırları: belge sınırları](search-limits-quotas-capacity.md#document-limits).
>

## <a name="evaluate-capacity"></a>Kapasite değerlendir

Kapasite ve hizmeti çalıştırmanın maliyetlerini el yakından gidin. Katmanları, iki düzeyde (depolama ve kaynakları) sınırlar büyük oranda yansıtmaktadır, hem hakkında çünkü hangi ulaşana ilk etkili sınırı biridir anlattık. 

İş gereksinimleri genellikle ihtiyacınız olacak dizinleri sayısı gerektirir. Örneğin, belgeler, büyük bir depo için genel bir dizin veya belki de bölgesi, uygulama veya iş talebini göre birden çok dizin.

Bir dizinin boyutunu belirlemek için gerekir [bir yapı](search-create-index-portal.md). Azure Search'te veri yapısı öncelikle olduğu bir [dizin ters](https://en.wikipedia.org/wiki/Inverted_index), kaynak verileri daha farklı özelliklere sahiptir. Tersine çevrilmiş bir dizin için boyutu ve karmaşıklığı içeriği, mutlaka içine akış veri miktarı belirlenir. Çok büyük artıklık ile büyük veri kaynağı, yüksek oranda değişken içeriğini içeren daha küçük bir veri kümesini daha küçük bir dizinde neden olabilir.  Bu nedenle, özgün veri kümesinin boyutuna bağlı dizin boyutu çıkarsanacak nadiren mümkündür.

### <a name="step-1-develop-rough-estimates-using-the-free-tier"></a>1. adım: ücretsiz katman kullanılarak kaba tahminleri geliştirin

Kapasitesini tahmin etmek için bir yaklaşım ise başlamak **ücretsiz** katmanı. Bu geri çağırma **ücretsiz** 50 MB depolama alanına ve saat dizin oluşturma, 2 dakika en fazla 3 dizin hizmeti sunar. Bu kısıtlamalar içeren bir öngörülen dizin boyutu tahmin etmek zor, ancak aşağıdaki örnekte, bir yaklaşım gösterilmektedir:

+ [Ücretsiz bir hizmet oluşturun](search-create-service-portal.md)
+ Küçük, temsili bir veri kümesi hazırlama (beş bin belgeleri ve örnek boyutunun yüzde onuna olduğunu varsayın)
+ [Bir başlangıç dizini oluşturun](search-create-index-portal.md) ve Portalı'nda boyutunu not edin (30 MB olduğunu varsayın)

Tüm belgeler dizine, örnek temsilcisi hem de veri kaynağının tamamı yüzde onuna eşit olduğu varsayılırsa, 30 MB dizin yaklaşık 300 MB olur. Bu ön numarasıyla kullanarak iki dizinler için (geliştirme ve üretim), depolama gereksinimleri 600 MB Toplam bütçeye çift bu tutar olabilir. Bu kolayca tarafından sağlanıyorsa **temel** var. başlar bu nedenle, katman.

### <a name="step-2-develop-refined-estimates-using-a-billable-tier"></a>2. adım: daraltılmış tahminleri Faturalandırılabilir katmandan kullanarak geliştirin

Bazı müşteriler büyük örnekleme ve işleme sürelerini uyum sağlayacak ve ardından dizin miktarı, boyutu ve sorgu birimleri gerçekçi tahminleri geliştirme sırasında geliştirme ayrılmış kaynaklarla başlatmak tercih eder. Başlangıçta, bir en iyi tahmin tahminine göre sağlanan bir hizmet ve Geliştirme projesinin geliştikçe ardından takımlar genellikle varolan hizmeti üzerinden veya öngörülen üretim iş yükleri için kapasite altında mı olduğunu. 

1. [Hizmet sınırları her katmanında gözden](https://docs.microsoft.com/en-us/azure/search/search-limits-quotas-capacity#index-limits) alt katmanları ihtiyacınız dizinleri miktarını destekleyip desteklemediğini belirlemek için. Arasında **temel**-**S1**- **S2** katmanları, dizin sınırları, 15-50-200, sırasıyla.

1. [Faturalanabilir bir katmanda bir hizmet oluşturma](search-create-service-portal.md):

    + Üzerinde düşük başlangıç **temel** veya **S1** , öğrenme eğrisi başında olması durumunda.
    + En yüksek Başlat **S2** ve hatta **S3**, büyük ölçekli dizin oluşturma ve sorgu Optional yükleniyor.

1. [Bir başlangıç dizini oluşturun](search-create-index-portal.md) kaynak verileri bir dizine nasıl çevirir belirlemek için. Dizin boyutu tahmin etmek için tek yolu budur.

1. [Depolama, hizmet sınırları, sorgu birimi ve gecikme süresi izleme](search-monitor-usage.md) portalında. Portal ikinci, daraltılmış sorgular ve arama gecikme süresi başına sorgu gösterir; her biri doğru katmanında olup olmadığını karar vermenize yardımcı olabilir. Portal ölçümleri yanı sıra ayrıntılı, geçişli tıklatma analizi gibi etkinleştirerek izlemeyi yapılandırabilirsiniz [trafik analizi arama](search-traffic-analytics.md). 

Sınırlarını depolama (bölümler) veya sınırlarını kaynaklar (dizin, dizin oluşturucular ve benzeri) tarafından kullanımı aracılığıyla hangisinin önce geldiğine bağlı ulaştığından dizin sayısına ve boyutuna eşit analiziniz için uygun. Portal, her ikisi de geçerli kullanım ve sınırlarını yan yana genel bakış sayfasında gösteren izlemenize yardımcı olur.

> [!NOTE]
> Belgeler fazlalık veriler içeriyorsa depolama gereksinimlerini aşırı inflated olabilir. İdeal olarak, belgeler, yalnızca arama deneyimi için ihtiyacınız olan verileri içerir. İkili veriler aranabilir olmayan ve ayrı olarak (belki de bir Azure tablo veya blob depolama alanında) bir alanda bir URL başvuru dış verileri tutmak için dizin ile depolanması gerekir. Tek bir belgenin en büyük boyutu 16 MB'tır (veya daha az ise bir istekte birden çok belge karşıya toplu olarak). [Azure Search'te hizmet sınırları](search-limits-quotas-capacity.md) daha fazla bilgi bulunur.
>

**Sorgu toplu konuları**

Sorguları saniye (QPS) performans ayarlama sırasında teklifleriyle kazandığında bir ölçüm, ancak gizliliğe çok yüksek sorgu toplu beklediğiniz sürece genellikle katmanını göz önünde bulundurarak değil.

Tüm standart katmanların Ek çoğaltmalar aracılığıyla daha hızlı sorgu döngü paralel işleme karşı ve ek bölümleri yüklemek için destek, bölüm çoğaltmalarını dengesi sunabilir. Hizmet sağlandıktan sonra performans için ayarlayabilirsiniz.

Strong beklediğiniz müşteri gizliliğe birimlerden daha yüksek katmanlarında, daha güçlü donanım tarafından desteklenen dikkate almanız gereken sorgu Sürdürülen. Bölümleri ve çoğaltmalarını çevrimdışı duruma getirin veya gerçekleştirmek bu sorgu birimlerin başarısız olduğunda bile daha düşük bir katman hizmetine geçin. Sorgu aktarım hızını hesaplamaya yönelik hakkında daha fazla bilgi için bkz. [Azure Search performans ve iyileştirme](search-performance-optimization.md).


**Hizmet düzeyi sözleşmeleri**

**Ücretsiz** katmanı ve önizleme özellikleri ile birlikte verilmez [hizmet düzeyi sözleşmeleri (SLA'lar)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). Hizmetiniz için yeterli yedeklilik sağlarken, Faturalanabilir tüm katmanlar için SLA etkili olur. İki veya daha fazla çoğaltma için sorgu (okuma) SLA gereklidir. Üç veya daha fazla çoğaltma, sorgu ve dizin oluşturma (okuma-yazma) SLA'sı için gereklidir. Bölüm sayısı, bir SLA husus değil. 

## <a name="tips-for-tier-evaluation"></a>Katman değerlendirme için ipuçları

+ Etkin dizinler oluşturmak nasıl ve hangi yenileme yöntemleri olan öğrenin az impactful. Öneririz [trafik analizi arama](search-traffic-analytics.md) sorgu etkinliği elde edilen içgörüleri için.

+ Sorgular oluşturabilir ve kullanım biçimlerini (yoğun olmayan saatlerde dizin sorgular çalışma saatleri) geçici olarak veri toplamak ölçümlerin sabitlenmesine ve gelecek hizmet kararları sağlama bildirmek için bu verileri kullanın. Pratik bir saatlik veya günlük düzeyinde olsa da, bölümler ve düzeyleri eylemde garanti altına almak yeterince uzun tutarsanız planlı değişiklikler sorgu birimler, ya da planlanmamış ancak sürekli değişiklikler karşılamak için kaynakları dinamik olarak ayarlayabilirsiniz.

+ Sağlama tek dezavantajı, Gerçek gereksinimler tahmini büyük olması durumunda bir hizmeti ayırma gerekebilir olduğunu unutmayın. Hizmet kesintisi yaşamamak için daha yüksek bir katmandan aynı abonelikte yeni bir hizmet oluşturmak ve çalıştırmak yan yana tüm uygulamaları ve istekleri yeni uç nokta hedef kadar.

## <a name="next-steps"></a>Sonraki adımlar

İle başlayan bir **ücretsiz** katmanı ve özelliklerini anlamak için verilerinizin bir alt kümesi kullanarak ilk dizini oluşturun. Azure Search'te veri yapısı bir ters dizini, boyut burada ve ters dizin karmaşıklığını içeriğe göre belirlenir. Yüksek düzeyde yedekli içeriği son derece düzensiz içeriği daha küçük bir dizin neden eğilimindedir unutmayın. Bu nedenle, bu dizin depolama gereksinimlerini belirleyen bir veri kümesi boyutu yerine içerik özellikleri olur.

Dizin boyutu, ilk hakkında bir fikir sonra [Faturalanabilir bir hizmeti sağlayın](search-create-service-portal.md) ya da bu makalede ele alınan katmanları birinde **temel** veya **standart** katmanı. Veri alt kümeleri üzerinde hiçbir yapay kısıtlama gevşeyin ve [dizininizi yeniden](search-howto-reindex.md) tüm gerçekten aranabilir olmasını istediğiniz verileri içerecek şekilde.

[Bölümleri ve çoğaltmalarını tahsis](search-capacity-planning.md) ihtiyaç duyduğunuz ölçek ve performans almak için gerektiği şekilde.

Performans ve kapasite ince ise gerçekleştirilir. Aksi takdirde, bir arama hizmeti daha yakından hizalar, ihtiyaçlarınıza göre farklı bir katmandan yeniden oluşturun.

> [!NOTE]
> Sorularınızı hakkında daha fazla yardım için postalayabilir [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) veya [Azure desteğine başvurun](https://azure.microsoft.com/support/options/).