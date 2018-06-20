---
title: Azure Search hizmeti için bir fiyatlandırma katmanı veya SKU seçin | Microsoft Docs
description: "Azure arama sırasında bu SKU'ları sağlanabilir: ücretsiz, temel ve standart, standart olduğu çeşitli kaynak yapılandırmaları ve kapasite düzeyleri kullanılabilir."
services: search
author: HeidiSteen
manager: cgronlun
tags: azure-portal
ms.service: search
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: heidist
ms.openlocfilehash: 241d24746d82a359b4bbf4febbbaaf91180dd23e
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36210933"
---
# <a name="choose-a-pricing-tier-for-azure-search"></a>Azure Arama'ya ilişkin bir fiyatlandırma katmanı seçin

Azure Search'te bir [hizmet sağlandığına](search-create-service-portal.md) belirli fiyatlandırma katmanı veya SKU. Seçenekleriniz **serbest**, **temel**, veya **standart**, burada **standart** birden çok yapılandırmaları ve kapasiteleri kullanılabilir. 

Bu makalenin amacı, bir katman seçmenize yardımcı olmaktır. Bu ek niteliğindedir [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/search/) ve [hizmet sınırları](search-limits-quotas-capacity.md) fatura kavramları ve tüketim desenlerini çeşitli katmanları ile ilişkili bir Özet sayfası. Ayrıca, en iyi hangi katmanda ihtiyaçlarınıza uygun anlamak için yinelemeli bir yaklaşım önerir. 

Katmanları kapasite, değil özellikleri belirler. Çok düşük olacak şekilde bir katmanın kapasitesini öyle, daha yüksek katman en yeni bir hizmet sağlamanız gerekir ve ardından [dizinleri yeniden](search-howto-reindex.md). Herhangi bir SKU aynı hizmetinden başka bir yerinde yükseltmesini yoktur.

Özellik kullanılabilirliği birincil katmanı önem verilmez. Tüm katmanlar dahil olmak üzere, **serbest** katmanı, S3HD dizin oluşturucu desteği dışında özellik eşliği sunar. Ancak, dizin oluşturma ve kaynak kısıtlamaları etkili bir şekilde özelliği kullanım kapsamını sınırlayabilirsiniz. Örneğin, [bilişsel arama](cognitive-search-concept-intro.md) dizin oluşturma sahip uzun süre çalışan becerileri o zaman aşımına boş bir hizmette sürece çok küçük olacak şekilde veri kümesi olur.

> [!TIP]
> Müşterilerin çoğu başlayın **serbest** katmanı için değerlendirme ve için Mezun **standart** geliştirme. Bir katman seçtikten sonra ve [bir arama hizmeti sağlamak](search-create-service-portal.md), yapabilecekleriniz [çoğaltma ve bölüm sayısını artırmak](search-capacity-planning.md) performans ayarlama. Ne zaman ve neden kapasite ayarlamak hakkında daha fazla bilgi için bkz: [performans ve en iyi duruma getirme konuları](search-performance-optimization.md).
>

## <a name="billing-concepts"></a>Faturalama kavramları

Katmanı seçimi için anlamanız gereken kavramlar kapasite tanımları, hizmet sınırları ve servis birimleri içerir. 

### <a name="capacity"></a>Kapasite

Kapasite olarak yapılandırıldığı *çoğaltmaları* ve *bölümleri*. Çoğaltmaları arama hizmeti her çoğaltma dizin yük dengeli bir kopyasını barındırdığı örnekleridir. Örneğin, bir hizmet 6 yinelemelerle hizmetinde yüklenen her dizin 6 kopyalarını sahiptir. Bölümler dizinleri depolamak ve otomatik olarak, aranabilir verileri böl: iki bölüm yarım, üç bölüm dizininizdeki Üçe bölme ve benzeri. Kapasite, bakımından *bölüm boyutu* birincil farklılaştırıcı katmanları arasında bir özelliktir.

> [!NOTE]
> Tüm **standart** katmanlarını Destek [esnek birleşimleri çoğaltma ve bölümleri](search-capacity-planning.md#chart) yapabilmeniz [hızı veya depolama sisteminizi ağırlık](search-performance-optimization.md) Bakiye değiştirerek. **Temel** üç çoğaltmaları yedeklemek için yüksek kullanılabilirlik ancak yalnızca bir bölüm sunar. **Ücretsiz** katmanları özel kaynakları sağlamaz: kaynakları birden çok ücretsiz hizmetler tarafından paylaşılan bilgisayar.

### <a name="limits"></a>Sınırlar

Dizinler, dizin oluşturucular ve diğerleri gibi ana bilgisayar kaynakları Hizmetleri. Her katman uygular [hizmet sınırları](search-limits-quotas-capacity.md) miktarı kaynakların oluşturabilirsiniz. Bu nedenle, dizinleri (ve diğer nesneler) sayısına bir sınır ikinci farklılaştırıcı katmanları arasında özelliğidir. Her seçenek portalında gözden geçirirken, dizin sayısı sınırlamaları unutmayın. Dizin Oluşturucular, veri kaynakları ve skillsets, gibi başka kaynaklar için dizin sınırları pegged.

### <a name="search-units"></a>Arama birimleri

Anlamak için en önemli fatura kavram bir *arama birimi* (SU) olduğu Azure Search yönelik faturalandırma birimidir. Azure Search çoğaltmaları ve bölümleri işlevine bağımlı olduğundan dolayı bir veya diğer faturalandırmak doesn't make Sense. Bunun yerine, faturalama hem bileşik üzerinde temel alır. Formulaically, bir SU çoğaltma ve bir hizmet tarafından kullanılan bölümlerini üründür: (R X P SU =). En azından, her hizmetin 1 SU (bir çoğaltma tarafından bir bölüm çarpılmış) ile başlayan, ancak daha gerçekçi bir model 9 SUs faturalandırılır 3-çoğaltma, 3-partition hizmet olabilir. 

Her katman aşamalı olarak daha yüksek kapasite sunmasına karşın, rest yedekte bulunduran bir kısmı çevrimiçi toplam kapasite kullanıma sunabilirsiniz. Fatura açısından, bölümler ve çoğaltmalar çevrimiçi, hesaplanan ne, aslında ücret ödersiniz belirleyen SU formülü kullanarak Getir, sayısıdır.

Faturalama saatlik SU farklı bir oran olması her katmanı ile hızıdır. Her katman üzerinde bulunabilir derecelendirir [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/search/).

## <a name="consumption-patterns"></a>Tüketim desenleri

Müşterilerin çoğu başlayın **serbest** aşağıdakilerden birini seçin ve hizmeti, hangi süresiz olarak saklamak **standart** önemli geliştirme veya üretim iş yükleri için katmanları. 

![Azure arama katmanları](./media/search-sku-tier/tiers.png "fiyatlandırma katmanlarına Azure arama")

Her iki uçta **temel** ve **S3 HD** önemli ancak alışılmadık tüketim desenler için mevcut. **Temel** küçük üretim iş yükleri için olan: SLA, sunduğu ayrılmış kaynakları, yüksek kullanılabilirlik, ancak uygun depolama, 2 GB toplam belirtti. Bu katman müşterileri için tasarlanmış olan tutarlı bir şekilde kullanılan kullanılabilir kapasitenin altında. Şimdiye kadar sonunda **S3 HD** iş yükleri için iş ortakları, ISV tipik [çok müşterili çözümleri](search-modeling-multitenant-saas-applications.md), ya da çok sayıda küçük dizinler için çağırma herhangi bir yapılandırma. Bu genellikle Optional olduğunda **temel** veya **S3 HD** katmanı sağ uygun olmakla birlikte, onay istiyorsanız için nakledebilirsiniz [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) veya [Azure başvurun Destek](https://azure.microsoft.com/support/options/) daha ayrıntılı yönergeler için.

Yaygın olarak kullanılan standart katmanları için odak kaydırma **S1 S3** , bölüm boyutu ton noktalarında ve üst sınırlar sayıda dizinler, dizin oluşturucular ve corollary kaynaklar üzerinde kapasite düzeylerini artan ilerlemeyi şunlardır:

|  | S1 | S2 | S3 |  |  |  |  |
|--|----|----|----|--|--|--|--|
| Bölüm boyutu|  25 GB | 100 GB | 250 GB |  |  |  |  |
| Dizin ve dizin oluşturucu sınırları| 50 | 200 | 200 |  |  |  |  |

**S1** özel kaynakları ve birden çok bölüm zorunlu olduğunda ortak bir seçimdir. Hizmet başına 25 GB en fazla 12 bölümleri için daha fazla bölüm ile sınırlamak üzerinde **S1** çoğaltmaları bölümleri en üst düzeye 300 GB Toplam ise (bkz [tahsis bölümler ve çoğaltmalar](search-capacity-planning.md#chart) daha dengeli için kompozisyonlarınıza.)

Portal ve fiyatlandırma sayfaları put odağı bölüm boyutu ve depolama, ancak her katman için tüm özellikleri (disk kapasitesi, hız, CPU'lar) işlem fiyatıyla doğrusal olarak büyütün. Bir **S2** çoğaltma daha hızlı bir şekilde **S1**, ve **S3** hızlıdır **S2**. **S3** katmanları sonu orantısız daha hızlı bir g/ç genellikle doğrusal işlem fiyatlandırma deseni. G/ç performans sorunu olarak öngörüyorsanız bir **S3** alt katmanları'den çok daha fazla IOPS sağlar.

**S3** ve **S3 HD** yedeklenen aynı yüksek kapasiteli altyapı ancak her biri kendi sınırı farklı şekillerde ulaştığında. **S3** çok büyük dizinler daha az sayıda hedefler. Bu nedenle, kaynak bağlı kendi üst sınır: (her hizmet için 2.4 TB). **S3 HD** çok sayıda çok küçük dizinleri hedefler. 1.000 dizinleri adresindeki **S3 HD** sınırlarına dizin kısıtlamalarını biçiminde ulaşır. Kullanıyorsanız bir **S3 HD** 1. 000'den fazla dizinler gerektiren müşteri nasıl ilerleyeceğiniz hakkında bilgi için Microsoft Support başvurun.

> [!NOTE]
> Daha önce belge limitleri önemli bir unsur olan, ancak artık sonra Ocak 2018 sağlanan çoğu Azure arama hizmetleri için geçerlidir. Belge limitleri hala geçerli durumları hakkında daha fazla bilgi için bkz: [hizmet sınırları: belge sınırları](search-limits-quotas-capacity.md#document-limits).
>

## <a name="evaluate-capacity"></a>Kapasite değerlendir

Kapasite ve hizmet çalıştırmanın maliyetlerini el eldeki gidin. Katmanları zorunlu tuttukları (depolama ve kaynaklar), iki düzeyde sınırları hakkında her ikisi de çünkü hangi ulaşana ilk etkili sınırı biridir düşündüğünüz şekilde. 

İş gereksinimleri genellikle ihtiyacınız olacak dizin sayısı dikte. Örneğin, belgelerin büyük bir havuz için genel bir dizin veya belki de bölge, uygulama veya iş talebini göre birden çok dizin.

Bir dizin boyutunu belirlemek için zorunda [bir yapı](search-create-index-portal.md). Azure Search'te veri yapısı öncelikle olan bir [dizin ters](https://en.wikipedia.org/wiki/Inverted_index), kaynak verileri daha farklı özelliklere sahip. Ters bir dizin için boyutu ve karmaşıklığı, içerik, mutlaka içine akış veri miktarı tarafından belirlenir. Yoğun artıklık ile büyük veri kaynağı, yüksek oranda değişken içerik içeren küçük bir veri kümesi'den küçük bir dizinde neden olabilir.  Bu nedenle, özgün veri kümesi boyutuna göre dizin boyutu Infer nadiren mümkündür.

### <a name="preliminary-estimates-using-the-free-tier"></a>Ücretsiz katmanı kullanarak ön tahminleri

Kapasite tahmin etmek için bir yaklaşım ise başlaması **serbest** katmanı. Sözcüğünün **serbest** hizmeti depolama 50 MB ve zaman dizin oluşturma, 2 dakika olarak en fazla 3 dizinleri sunar. Bu kısıtlamaların tahmini dizin boyutuyla tahmin etmek için bu zor olabilir, ancak bir yaklaşım aşağıdaki örnek gösterilmektedir:

+ [Ücretsiz bir hizmet oluşturun](search-create-service-portal.md)
+ Küçük, temsili bir veri kümesi hazırlama (beş bin belgeler ve yüzde onluk örnek boyutu varsayılır)
+ [Bir başlangıç dizini derleme](search-create-index-portal.md) ve Portalı'nda boyutunu not edin (30 MB varsayılır)

Tüm belgeler yeniden dizinlenir örnek temsilcisi ve tüm veri kaynağının yüzde onluk olduğu varsayılarak, 30 MB dizin yaklaşık 300 MB haline gelir. Bu ön numarasıyla sayesinde 600 MB Toplam depolama gereksinimleri için iki dizinler için (geliştirme ve üretim) bütçeye çift bu tutar olabilir. Bu kolayca tarafından sağlanıyorsa **temel** katmanı vardır başlarsınız şekilde.

### <a name="advanced-estimates-using-a-billable-tier"></a>Faturalanabilir bir katmanı kullanarak gelişmiş tahmin eder

Bazı müşteriler büyük örnekleme ve işlem süreleri uyum ve ardından dizin miktarı, boyutu ve sorgu birimleri gerçekçi tahminleri geliştirme sırasında geliştirmek özel kaynakları ile başlatmak tercih edilir. Başlangıçta, bir en iyi tahmin tahminine dayanarak bir hizmet sağlanır ve geliştirme projesi geliştikçe sonra takımlar genellikle varolan hizmeti üzerinden tahmini üretim iş yükleri için kapasite altında mı olduğunu bilin. 

1. [Her katman konumundaki hizmet sınırları gözden](https://docs.microsoft.com/en-us/azure/search/search-limits-quotas-capacity#index-limits) alt katmanları ihtiyacınız dizinleri miktarını destekleyip desteklemediğini belirlemek için. Üzerinden **temel**-**S1**- **S2** katmanları, dizin sınırları 15-50-200, sırasıyla şunlardır.

1. [Bir hizmet Faturalanabilir bir katman oluşturmak](search-create-service-portal.md):

    + Düşük üzerinde Başlat **temel** veya **S1** öğrenme eğrisi başında varsa.
    + En yüksek Başlat **S2** ve hatta **S3**, büyük ölçekli dizin oluşturma ve sorgu yükleri Optional varsa.

1. [Bir başlangıç dizini derleme](search-create-index-portal.md) kaynak verileri bir dizine nasıl çevirir belirlemek için. Bu dizin boyutunu tahmin etmek için tek yoludur.

1. [Depolama, hizmet sınırları, sorgu toplu ve gecikme izleme](search-monitor-usage.md) Portalı'nda. Portal sorguları ikinci, daraltılmış sorgular ve arama gecikme her gösterir; bunların tümü doğru katmanını olup olmadığını karar vermenize yardımcı olabilir. Portal ölçümleri yanı sıra, derin, geçişli tıklatma analiz gibi etkinleştirerek izlemeyi yapılandırabilirsiniz [trafiği analytics arama](search-traffic-analytics.md). 

Maksimum sınırları depolama (bölümler) ya da kaynaklar (dizinler, dizin oluşturucular ve benzeri) maksimum sınırları tam kullanımı aracılığıyla hangisi önce gelirse ulaştığından dizin sayısını ve boyutunu çözümleme için eşit oranda ilgilidir. Portal, her ikisi de, geçerli kullanım ve maksimum sınırları yan yana genel bakış sayfasında gösteren izlemenize yardımcı olur.

> [!NOTE]
> Belgeleri yabancı veri içeriyorsa depolama gereksinimleri aşırı inflated olabilir. İdeal olarak, belgeler yalnızca arama deneyimi için gereken verileri içerir. İkili veriler yapılamayan ve ayrı olarak (belki de bir Azure tablo veya blob depolama alanında) dış veriler için bir URL başvuru tutmak için dizinde bir alanla depolanması gerekir. Tek bir belgenin en büyük boyutu 16 MB (veya daha az ise, toplu bir istek birden çok belgeyi karşıya yükleme). [Hizmet sınırları Azure Search'te](search-limits-quotas-capacity.md) daha fazla bilgi bulunur.
>

**Sorgu toplu konuları**

Saniyede sorgular (QPS) performans ayarlama sırasında teklifleriyle kazanır ölçümüdür ancak outset çok yüksek sorgu toplu beklediğiniz sürece genellikle bir katmanı önem verilmez.

Paralel işleme karşı ve ek bölümleri yüklemek için ek çoğaltmaları üzerinden daha hızlı bir sorgu döngü destekleme bölümlere, tüm standart katmanları çoğaltmaları dengesi sunabilir. Hizmet sağlandıktan sonra performans için ayarlayabilirsiniz.

Strong beklediğiniz müşteri outset birimlerden daha güçlü donanım tarafından desteklenen daha yüksek katmanları dikkate almanız gereken sorgu Sürdürülen. Bölümler ve çoğaltmalar çevrimdışı duruma getirin veya gerçekleştirmeye bu sorguyu birimlerin başarısız olduğunda bile bir alt katmanı hizmetine geçin. Sorgu işleme hesaplama hakkında daha fazla bilgi için bkz: [Azure Search performans ve en iyi duruma getirme](search-performance-optimization.md).


**Hizmet düzeyi sözleşmeleri**

**Serbest** katmanı ve önizleme özellikleri değil ile birlikte gelir [hizmet düzeyi sözleşmelerine (SLA)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). Hizmetiniz için yeterli artıklık sağladığınızda tüm Faturalanabilir katmanları için SLA etkili olur. İki veya daha fazla çoğaltmaları için (okuma) sorgu SLA gereklidir. Üç veya daha fazla çoğaltmalar, sorgu ve dizin oluşturma (okuma-yazma) SLA için gereklidir. Bölüm sayısı bir SLA önem verilmez. 

## <a name="tips-for-tier-evaluation"></a>Katman değerlendirme için ipuçları

+ Verimli dizin oluşturma ve hangi yenileme yöntemlerini olan bilgi en az impactful. Öneririz [trafiği analytics arama](search-traffic-analytics.md) sorgu faaliyete elde edilen Öngörüler için.

+ Sorguları oluşturmak ve kullanım düzenlerini (yoğun olmayan saatlerde dizin sorguları) iş saatleri sırasında geçici veri toplamak ölçümleri izin ve gelecekteki hizmeti kararları sağlama bildirmek için bu verileri kullanın. Pratik bir saatlik veya günlük düzeyinde karşın, bölümler ve düzeylerini garanti eylemin gerçekleştirilmesi için yeterince uzun tutarsanız planlanan değişiklikler sorgu birimler, veya planlanmamış ancak sürekli değişiklikleri karşılamak için kaynakları dinamik olarak ayarlayabilirsiniz.

+ Altında sağlama yalnızca dezavantajı Gerçek gereksinimler tahmini fazla olması durumunda bir hizmeti kesmeden gerekebilir olduğunu unutmayın. Hizmet kesintisi yaşamamak için aynı abonelikte daha yüksek bir katmanı en yeni bir hizmet oluşturmak ve çalıştırın yan yana tüm uygulamaları ve istekleri yeni uç nokta hedef kadar.

## <a name="next-steps"></a>Sonraki adımlar

İle başlayan bir **serbest** katmanı ve onun özelliklerini anlamak için verilerin bir alt kümesini kullanarak bir başlangıç dizini oluşturun. Azure Search'te veri yapısı bir ters dizini, boyut olduğunda ve ters dizin karmaşıklığını içerik tarafından belirlenir. Yüksek oranda gereksiz içeriği yüksek oranda düzensiz içeriği daha küçük bir dizin sonucunda eğilimindedir unutmayın. Bu nedenle, bu dizin depolama gereksinimleri belirler veri kümesi boyutunu yerine içerik özellikleri gösterir.

Dizin boyutu, ilk hakkında bir fikir edindikten sonra [Faturalanabilir bir hizmet sağlamak](search-create-service-portal.md) ya da bu makalede ele alınan katmanları birinde **temel** veya **standart** katmanı. Veri alt kümelerini yapay herhangi kısıtlamalar hafifletin ve [dizininizi yeniden](search-howto-reindex.md) tüm gerçekte aranabilir olmasını istediğiniz verileri içerecek şekilde.

[Bölümler ve çoğaltmalar tahsis](search-capacity-planning.md) performansı ve ölçeği ihtiyaç duyduğunuz almak için gerektiği gibi.

Performans ve kapasite ince varsa, yapılır. Aksi takdirde, bir arama hizmeti daha yakından hizalar gereksinimlerinizi farklı bir katmana yeniden oluşturun.

> [!NOTE]
> Sorularınızı daha fazla yardım için deftere [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) veya [Azure desteğine başvurun](https://azure.microsoft.com/support/options/).