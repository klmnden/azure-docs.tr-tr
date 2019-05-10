---
title: Azure Search Hizmeti - Azure arama için fiyatlandırma katmanı veya SKU seçin
description: "Azure arama bu SKU'ların sağlanabilir: Ücretsiz, temel ve standart, burada standart çeşitli kaynak yapılandırmaları ve kapasite düzeyleri kullanılabilir."
services: search
author: HeidiSteen
manager: cgronlun
tags: azure-portal
ms.service: search
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 5b9e0dfb88c979618ce4cc5ed56e372cb0f65608
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65472714"
---
# <a name="choose-a-pricing-tier-for-azure-search"></a>Azure arama için bir fiyatlandırma katmanı seçin

Azure Search'te bir [kaynak oluşturulduğu](search-create-service-portal.md) fiyatlandırma katmanı veya hizmet ömrü boyunca sabit SKU. Katmanlar **ücretsiz**, **temel**, **standart**, veya **depolama için iyileştirilmiş**.  **Standart** ve **depolama için iyileştirilmiş** çeşitli yapılandırmaları ve kapasiteler içinde kullanılabilir. 

Çoğu müşteri başlayın **ücretsiz** katmanı için değerlendirme ve geliştirme ile üretim dağıtımları için daha yüksek Ücretli katmanlardan birine ölçeğine geçin. Üzerindeki tüm Hızlı başlangıçlar ve öğreticilerle tamamlayabilirsiniz **ücretsiz** katmanı, kaynak kullanımı yoğun bilişsel arama için dahil olmak üzere.

> [!NOTE]
> Depolama için iyileştirilmiş hizmet katmanları şu an geri bildirim toplamak amacıyla test ve deneme amaçları için indirimli fiyatlandırma önizleme olarak kullanılabilir. Bu katmanları genel olarak kullanılabilir olduğunda son fiyatlandırma daha sonra duyurulacaktır. Biz, üretim uygulamaları için bu katmanları kullanan karşı önerin.

Katmanları hizmeti (yerine özellikleri) barındıran donanım özellikleri yansıtır ve tarafından ayrılır:

+ Oluşturabileceğiniz dizin sayısı
+ Boyutuna ve hızına bölümleri (fiziksel depolama)

Tüm katmanları olsa da dahil olmak üzere **ücretsiz** katman, genellikle özellik eşliği teklif daha büyük iş yükleri, daha yüksek katmanlara yönelik gereksinimleri dikte. Örneğin, [Bilişsel hizmetler ile yapay ZEKA dizin](cognitive-search-concept-intro.md) veri kümesini küçük özelleştirmede sürece uzun süre çalışan becerileri ücretsiz bir hizmet o zaman aşımına sahip.

> [!NOTE] 
> Özellik eşliği istisnası [dizin oluşturucular](search-indexer-overview.md), hangi S3HD üzerinde mevcut değildir.
>

Bir katman içinde yapabilecekleriniz [çoğaltma ve bölüm kaynakları](search-capacity-planning.md) artırabilir veya ölçek azaltabilirsiniz. Biri veya her ikisi ile başlayın ve geçici olarak bir dizin oluşturma ağır iş yükü için işlem gücünü yükseltmek. Bir katman içinde kaynak düzeylerini ayarlama olanağı esneklik kazandırır ancak biraz da analiz karmaşık hale getirir. Daha düşük bir katmana daha yüksek kaynak/yinelemeler ile daha iyi değeri ve daha düşük kaynak ile daha yüksek bir katmana performans sunar görmek için denemeniz gerekebilir. Ne zaman ve neden kapasiteyi ayarlamak hakkında daha fazla bilgi için bkz: [performans ve iyileştirme konuları](search-performance-optimization.md).

## <a name="tiers-for-azure-search"></a>Azure Search için katmanları

Aşağıdaki tabloda kullanılabilir Katmanlar listelenmektedir. Katman bilgileri diğer kaynakları dahil [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/search/), [hizmet ve veri sınırları](search-limits-quotas-capacity.md)ve bir hizmet sağlanırken portal sayfası.

|Katman | Kapasite |
|-----|-------------|
|Ücretsiz | Diğer abonelerle paylaşılan. 3 dizin ve 50 MB depolama için sınırlı ölçeklenemez. |
|Temel | Bilgi işlem kaynaklarını daha küçük ölçekli üretim iş yükleri için ayrılmış. Bir 2 GB'lık bölümü ve üç kopyaya kadar. |
|Standart 1 (S1) | Yedekleme, daha fazla depolama ve işleme kapasitesi her düzeyde olan ayrılmış makineye S1 üzerinde. Bölüm, 25 GB/bölüm (hizmet başına en fazla 300 GB) S1 boyutudur. |
|Standart 2 (S2 için) | Benzer S1 ancak 100 GB/bölüm (hizmet başına en fazla 1,2 TB) |
|Standart 3 (S3 için) | 200 GB/bölüm (hizmet başına en fazla 2,4 TB) |
|Standart 3 yüksek yoğunluklu (S3-HD) | Yüksek yoğunluklu olan bir *modu barındırma* S3. Temel alınan donanım, çok sayıda küçük dizinler, çoklu müşteri mimarisi senaryolarına yönelik için optimize edilmiştir. S3 ancak donanım optimize gibi çok sayıda küçük dizinleri üzerinde hızlı dosya okuma için aynı birim başına ücret S3 HD sahiptir.|
|Depolama için iyileştirilmiş 1 (L1) | 1 TB/bölüm (hizmet başına en fazla 12 TB) |
|Depolama için iyileştirilmiş 2 (L2) | 2 TB/bölüm (hizmet başına en fazla 24 TB) |

> [!NOTE] 
> Depolama için iyileştirilmiş katmanlar, daha düşük bir fiyatla TB başına standart katmanların değerinden daha büyük depolama kapasitesi sunar. Birincil artırabilen uygulamanıza özel gereksinimler için doğrulamalıdır daha yüksek sorgu gecikme olur.  Bu katmanının performans değerlendirmeleri hakkında daha fazla bilgi için bkz: [performans ve iyileştirme konuları](search-performance-optimization.md).
>

## <a name="how-billing-works"></a>Faturalandırma nasıl çalışır?

Azure Search, Azure Search'te ödemeniz üç yolu vardır ve sabit ve değişken bileşeni vardır. Bu bölümde her fatura bileşenini sırayla incelenir.

### <a name="1-core-service-costs-fixed-and-variable"></a>1. Çekirdek hizmet maliyetleri (sabit ve değişken)

Hizmetinde, ilk arama birimi (1 çoğaltma x 1 bölüm) en düşük ücretlendirme yapılır ve hizmet üzerinde herhangi bir şey bu yapılandırma'dan çalıştırılamaz hizmet ömrü boyunca sabit bu miktar. 

En düşük çoğaltmalar ve bölümler birbirinden bağımsız olarak ekleyebilirsiniz. Örneğin, yalnızca çoğaltmalar veya bölümler yalnızca ekleyebilirsiniz. Kapasite çoğaltmalar ve bölümler arasında artımlı bir artış değişkeni maliyet bileşeni oluşturur. 

Faturalandırma temel bir [formül (çoğaltmaları bölümler x x oranı)](#search-units). Ücreti, seçtiğiniz fiyatlandırma katmanına bağlıdır.

Aşağıdaki ekran görüntüsünde, ücretsiz, temel ve S1 için birim fiyatlandırma gösterilir (S2, S3, L1 ve L2 gösterilmez). Oluşturduysanız bir **temel**, **standart**, veya **depolama için iyileştirilmiş** hizmet, aylık maliyetiniz ortalama için görüntülenen değeri *fiyat 1*ve *fiyat 2* sırasıyla. İşlem gücü ve depolama kapasitesini art arda her katmanında büyük olduğundan birim maliyetlerini her katman için artar. Azure Search fiyatlar yayımlanan [Azure fiyatlandırma sayfasını arama](https://azure.microsoft.com/pricing/details/search/).

![Birim başına](./media/search-sku-tier/per-unit-pricing.png "birim başına")

Bir arama çözümü maliyeti, fiyatlandırma ve kapasite (maliyet double'birden fazla kapasite Katlama) doğrusal olmadığına dikkat edin. Formül Works ilişkin bir örnek için bkz. ["çoğaltmalar ve bölümler tahsis etme"](search-capacity-planning.md#how-to-allocate-replicas-and-partitions).


### <a name="2-data-egress-charges-during-indexing"></a>2. Dizin oluşturma sırasında veri çıkış ücretleri

Kullanım [Azure Search dizin oluşturucularında](search-indexer-overview.md) Hizmetleri bulunduğu yere göre faturalara neden olabilir. Tamamen verilerinizi aynı bölgede Azure Search Hizmeti oluşturursanız, veri çıkış ücretlerini ortadan kaldırabilir. Aşağıdaki noktaları arasındadır [bant genişliği fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/bandwidth/).

+ Microsoft gelen tüm veriler için herhangi bir Azure hizmeti için veya tüm giden veri için Azure arama doldurmaz.

+ Birden çok hizmet çözümlerinde tüm hizmetleri aynı bölgede olması durumunda kablo üzerinden geçmesini veriler için ücretlendirme yoktur.

Hizmetleri farklı bölgelerde bulunuyorsa için giden veri ücretleri uygulanır. Giderlerin Azure Search faturanızı başına uzatılmasında parçası olmayan, ancak veri veya AI zenginleştirilmiş dizin oluşturucular veri çekmek için farklı bölgelerden kullanıyorsanız, bu maliyetleri genel faturanıza yansıtılan görürsünüz çünkü bunlar aşağıda belirtilmiştir. 

### <a name="3-ai-enriched-indexing-using-cognitive-services"></a>3. Yapay ZEKA-zenginleştirilmiş Bilişsel hizmetler kullanarak dizin oluşturma

İçin [Bilişsel hizmetler ile yapay ZEKA dizin](cognitive-search-concept-intro.md), aynı bölgede Azure Search, Kullandıkça Öde işleme S0 fiyatlandırma katmanında Faturalanabilir bir Bilişsel hizmetler kaynağı iliştirilirken planlamanız gerekir. "Bilişsel Hizmetleri ekleme ile ilişkili hiçbir sabit ücret" yoktur. Yalnızca gereksinim duyduğunuz işleme için ödeme yaparsınız.

Görüntü ayıklama belge çözme sırasında olan bir Azure Search ücret faturalandırılır, belgeleri ayıklanan görüntülerin göre. Metin ayıklama şu anda ücretsiz olarak kullanılabilir. 

Doğal dil işleme gibi diğer zenginleştirmelerinin dayalı [yerleşik bilişsel beceriler](cognitive-search-predefined-skills.md) Bilişsel hizmetler kullanarak doğrudan görev gerçekleştirilen gibi aynı hızda bir Bilişsel hizmetler kaynağı göre faturalandırılır. Daha fazla bilgi için [bir beceri kümesi ile bir Bilişsel hizmetler kaynağı ekleme](cognitive-search-attach-cognitive-services.md).

<a name="search-units"></a>

### <a name="billing-based-on-search-units"></a>Arama birimleri temel faturalandırma

Azure arama işlemleri için en önemli fatura kavramı anlamak için olan bir *arama birimi* (SU). Çoğaltmalar ve bölümler dizin oluşturma ve sorgular için Azure Search bağlı olduğundan, onu yalnızca birini veya diğerini tarafından faturalandırmak için anlam ifade etmez. Bunun yerine, her iki bileşik üzerinde üzerinden faturalandırılır. 

SU olan çarpımını *çoğaltma* ve *bölümleri* hizmeti tarafından kullanılan: **`(R X P = SU)`**

Her hizmetin en az bir SU (bir çoğaltma bir bölüm ile çarpılmış) başlar. En büyük herhangi bir hizmet için birden çok yolla sağlanabilir 36 su şöyledir: 6 bölümler x 6 çoğaltmalar veya 3 bölümler x 12 çoğaltmalar. Daha azını toplam kapasite kullanımı yaygındır. 9 SUs faturalandırılır. Örneğin, bir yineleme 3, 3 bölümlü hizmeti. Gözden geçirebilirsiniz [bu grafik](search-capacity-planning.md#chart) geçerli birleşimleri bir bakışta görmek için.

Fatura oranı **SU başına saatlik**, giderek daha yüksek fiyatlarla sahip her bir katman ile. Genel olarak daha yüksek bir saatlik ücret söz konusu katman için katkıda bulunan, daha büyük ve daha hızlı bölümleri olan daha yüksek katmanlarında sunulur. Her katmanın bulunabilir için derecelendirir [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/search/). 

Çoğu müşteri, toplam kapasite çevrimiçi olarak yalnızca bir kısmını rest yedekte bulunduran getirin. Faturalama bakımından, bölümler ve çoğaltmalar çevrimiçi, hesaplanmış, aslında saatlik olarak ödersiniz belirleyen SU formülü kullanarak Getir sayısıdır.

### <a name="billing-for-image-extraction-in-cognitive-search"></a>Bilişsel arama görüntü ayıklama faturalandırması

Bilişsel arama işlem hattı dizinleme dosyalarından görüntüleri ayıklıyorsanız, Azure Search faturanızda bu işlem için ücretlendirilirsiniz. Görüntü ayıklama tetikleyen parametre **imageAction** içinde bir [dizin oluşturucu yapılandırmasını](https://docs.microsoft.com/rest/api/searchservice/create-indexer#indexer-parameters). Varsa **imageAction** ayarlanır hiçbiri için (varsayılan), ücretsizdir görüntü ayıklama için.

Fiyatlandırma, değiştirilebilir, ancak her zaman öğesinde belgelendirilen [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/search/) Azure Search için sayfa. 

### <a name="billing-for-built-in-skills-in-cognitive-search"></a>Bilişsel arama yerleşik yeteneklerinizi faturalandırması

Zenginleştirme işlem hattı ayarladığınızda herhangi [yerleşik yetenekler](cognitive-search-predefined-skills.md) işlem hattında kullanılan makine öğrenimi modelleri üzerinde temel alır. Bu model, Bilişsel hizmetler tarafından sağlanır. Dizin oluşturma sırasında bu modelleri kullanımını doğrudan kaynak istenen gibi aynı fiyattan faturalandırılır.

Örneğin, burada elde edilen metnini serbest biçimli arama sorguları için bir Azure Search dizinine gönderildiğinde taranmış görüntü JPEG dosyaları karşı optik karakter tanıma (OCR) oluşan bir işlem hattı varsayalım. Bir dizin oluşturucu ile dizin oluşturma işlem hattınızı verilebilir [OCR beceri](cognitive-search-skill-ocr.md), beceri yanıtlayabiliriz [Bilişsel hizmetler kaynağa bağlı](cognitive-search-attach-cognitive-services.md). Dizin Oluşturucu çalıştırdığınızda, ücretleri OCR yürütme için Bilişsel kaynakları faturanızda görünür.

## <a name="tips-for-reducing-costs"></a>Maliyetleri azaltmak için ipuçları

Fatura düşürmek için hizmeti Kapat olamaz. İşletimsel 24-hizmetinizin ömrü, özel kullanım için ayrılan 7, ayrılmış kaynaklardır. Çoğaltmalar ve bölümler hala kabul edilebilir performans sağlayan düşük bir düzeyde azaltarak tek yolu bir fatura daha düşük olan ve [SLA Uyumluluk](https://azure.microsoft.com/support/legal/sla/search/v1_0/). 

Maliyetleri azaltmak için bir düzeyi daha düşük bir saatlik ücret bir katman seçmektir. Saatlik ücretler S1, S2 veya S3 ücretlerinden daha düşük. Hizmet aşıyorsa, daha düşük, yük projeksiyonlar sonunda amaçlayan bir hizmet sağlama varsayılarak, daha büyük katmanlı ikinci bir hizmet oluşturma, bu ikinci hizmetine yeniden ve ilk silin. 

Kapasite planlaması şirket içi sunucularda yaptığınız varsa, yaygın "Gelecekteki büyümeyi işleyebilmeniz kaydolabileceksiniz" olduğunu bilirsiniz. Ancak, belirli bir satın alma için kilitli çünkü bir bulut hizmeti ile daha fazla maliyet tasarrufu agresif daha sonra amacınızın. Geçerli yetersizse, daha yüksek katmanlı bir hizmet için her zaman geçiş yapabilirsiniz.

### <a name="capacity-drill-down"></a>Kapasite detaya gitme

Azure Search'te kapasite olarak yapılandırılmış *çoğaltmaları* ve *bölümler*. 

+ Çoğaltmaları arama hizmetinin, her çoğaltma dizin yük dengeli bir kopyasını barındırdığı örnekleridir. Örneğin, 6 çoğaltma ile bir hizmeti, hizmete yüklenen her dizin 6 kopyasını içerir. 

+ Bölüm dizinleri depolamak ve aranabilir verileri'otomatik olarak böl: iki bölüm yarım, üç bölüm dizininizdeki Üçe bölme ve VS. Kapasite açısından *bölüm boyutu* birincil ayırt edici katmanlarda özelliğidir.

> [!NOTE]
> Tüm **standart** ve **depolama için iyileştirilmiş** destek katmanları [esnek birleşimleri çoğaltma ve bölümler](search-capacity-planning.md#chart) destesinin [sisteminiz hızı için ağırlık veya Depolama](search-performance-optimization.md) Bakiye değiştirerek. **Temel** üç çoğaltmaları yedeklemek için yüksek kullanılabilirlik ancak yalnızca bir bölüm sunar. **Ücretsiz** katmanları olarak ayrılmış kaynaklarda sağlamaz: bilgi işlem kaynakları, birden çok abone tarafından paylaşılır.

### <a name="more-about-service-limits"></a>Hizmet sınırları hakkında daha fazla bilgi

Dizin, dizin oluşturucular ve benzeri gibi ana bilgisayar kaynakları Hizmetleri. Her katman uygular [hizmet sınırları](search-limits-quotas-capacity.md) miktarı kaynakların oluşturabilirsiniz. Bu nedenle, dizinleri (ve diğer nesneler) sayısı için üst sınır ikinci ayırt edici katmanlarda özelliğidir. Her seçeneği portalında gözden geçirirken, dizinlerin sayısı sınırlamaları unutmayın. Uzmanlık becerileri, dizin oluşturucular ve veri kaynakları gibi diğer kaynaklar için dizin sınırları pegged.

## <a name="consumption-patterns"></a>Tüketim modelleri

Çoğu müşteri başlayın **ücretsiz** hizmeti, hangi süresiz olarak saklayın ve aşağıdakilerden birini seçin **standart** veya **depolama için iyileştirilmiş** katmanları için önemli geliştirme veya Üretim iş yükleri. 

![Azure search katmanları](./media/search-sku-tier/tiers.png "Azure Search'ü fiyatlandırma katmanları")

Her iki uçta **temel** ve **S3 HD** önemli, ancak alışılmadık tüketim düzenlerine için mevcut. **Temel** küçük üretim iş yükleri için olan: SLA, sunduğu ayrılmış kaynakları, yüksek oranda kullanılabilir, ancak 2 GB toplam belirtti uygun depolama. Bu katman müşterileri için tasarlanmış kimin tutarlı bir şekilde kullanılan kullanılabilir kapasitenin altında. Şimdiye kadar sonunda **S3 HD** iş yükleri için iş ortakları, ISV'lerin tipik olarak [çok kiracılı çözüm](search-modeling-multitenant-saas-applications.md), ya da çok sayıda küçük dizinler için çağırma herhangi bir yapılandırma. Bu genellikle Optional olduğunda **temel** veya **S3 HD** katmanı sağ uygun olduğu halde onay istiyorsanız için gönderebilir [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) veya [Azure başvurun Destek](https://azure.microsoft.com/support/options/) daha ayrıntılı yönergeler için.

Daha yaygın olarak kullanılan standart katmanlar için odak kaydırma **S1 S3** , bölüm boyutu dönüm noktalarını ve üst sınırlar sayıda dizin, dizin oluşturucular ve corollary kaynakları üzerinde kapasitesinin düzeyleri artan bir ilerleme olan:

|  | S1 | S2 | S3 |  |  |  |  |
|--|----|----|----|--|--|--|--|
| Bölüm boyutu|  25 GB | 100 GB | 200 GB |  |  |  |  |
| Dizin ve dizin oluşturucu sınırları| 50 | 200 | 200 |  |  |  |  |

**S1** seçeneği olarak ayrılmış kaynaklarda ve birden çok bölüm olduğunda ortak bir seçimdir. Hizmet başına en fazla 12 bölümler için 25 GB bölümleri ile sınırlamak üzerinde **S1** çoğaltmalar bölümlerin en üst düzeye toplam 300 GB olan (bkz [tahsis bölümleri ve çoğaltmalarını](search-capacity-planning.md#chart) daha dengeli için birleşimleri.)

Portal ve fiyatlandırma sayfalarını bölüm boyutu ve depolama odağı yerleştirir, ancak her bir katman için tüm işlem özelliklerini (disk kapasitesi, CPU hızı) fiyatı ile doğrusal olarak büyütün. Bir **S2** çoğaltma daha hızlı bir şekilde **S1**, ve **S3** hızlıdır **S2**. **S3** katmanları Kes orantısız daha hızlı bir g/ç genellikle doğrusal işlem fiyatlandırma deseni. G/ç performans sorunu olarak düşünüyorsanız bir **S3** alt katmanları daha fazlasını IOPS sağlar.

**S3** ve **S3 HD** yedeklenen aynı yüksek kapasiteli altyapı ancak her biri kendi üst sınırı farklı şekillerde ulaşır. **S3** daha küçük bir sayı çok büyük dizinlerde hedefler. Bu nedenle, kaynak veriye bağlı alt sınırı (her hizmet için 2,4 TB). **S3 HD** çok sayıda küçük dizinleri hedefler. 1.000 dizinlere **S3 HD** dizin kısıtlamalarını biçiminde sınıra ulaşana. Eğer bir **S3 HD** 1. 000'den fazla dizinleri gerektirir müşteri nasıl ilerleyeceğiniz hakkında bilgi için Microsoft Support başvurun.

> [!NOTE]
> Daha önce belge limitleri önemli bir unsur olan, ancak artık yeni hizmetler için geçerlidir. Koşullar altında belge limitleri hala geçerli hakkında daha fazla bilgi için bkz. [hizmet sınırları: belge sınırları](search-limits-quotas-capacity.md#document-limits).
>

Depolama için optimize edilmiş katmanları **L1 L2**, büyük veri gereksinimleri, ancak son kullanıcılar, sorgunun gecikme süresi en aza olduğu en yüksek önceliğiniz görece az sayıda olan uygulamalar için idealdir.  

|  | L1 | L2 |  |  |  |  |  |
|--|----|----|--|--|--|--|--|
| Bölüm boyutu|  1 TB | 2 TB |  |  |  |  |  |
| Dizin ve dizin oluşturucu sınırları| 10 | 10 |  |  |  |  |  |

*L2* genel depolama kapasitesi iki kez sunan bir *L1*.  En fazla dizininizi gereken düşündüğünüz veri miktarına göre katmanınızı seçin.  *L1* katmanı altında ölçek yukarı 1 TB'lik artışlarla 12 TB maksimum olarak bölümler sırada *L2* 2 TB'a her bölüm en fazla 24 TB'a kadar artırın.

## <a name="evaluate-capacity"></a>Kapasite değerlendir

Kapasite ve hizmeti çalıştırmanın maliyetlerini el yakından gidin. Katmanları, iki düzeyde (depolama ve kaynakları) sınırlar büyük oranda yansıtmaktadır, hem hakkında çünkü hangi ulaşana ilk etkili sınırı biridir anlattık. 

İş gereksinimleri genellikle ihtiyacınız olacak dizinleri sayısı gerektirir. Örneğin, belgeler, büyük bir depo için genel bir dizin veya belki de bölgesi, uygulama veya iş talebini göre birden çok dizin.

Bir dizinin boyutunu belirlemek için gerekir [bir yapı](search-create-index-portal.md). Azure Search'te veri yapısı öncelikle olduğu bir [dizin ters](https://en.wikipedia.org/wiki/Inverted_index), kaynak verileri daha farklı özelliklere sahiptir. Tersine çevrilmiş bir dizin için boyutu ve karmaşıklığı içeriği, mutlaka içine akış veri miktarı belirlenir. Çok büyük artıklık ile büyük veri kaynağı, yüksek oranda değişken içeriğini içeren daha küçük bir veri kümesini daha küçük bir dizinde neden olabilir. Bu nedenle, özgün veri kümesinin boyutuna bağlı dizin boyutu çıkarsanacak nadiren mümkündür.

> [!NOTE] 
> Dizinler ve depolama için gelecekteki gereksinimlerini tahmin etme gibi kararın düşünüyorsanız olsa da, bunun yapılması değer var. Çok düşük bir katmanın kapasitesini ettik, yeni bir hizmet yüksek katmandan sağlamanız gerekir ve ardından [dizinlerinizi yeniden](search-howto-reindex.md). Hiçbir aynı SKU bir hizmetten diğerine yerinde yükseltmesini yoktur.
>

### <a name="step-1-develop-rough-estimates-using-the-free-tier"></a>1. Adım: Ücretsiz katman kullanılarak kaba tahminleri geliştirin

Kapasitesini tahmin etmek için bir yaklaşım ise başlamak **ücretsiz** katmanı. Bu geri çağırma **ücretsiz** 50 MB depolama alanına ve saat dizin oluşturma, 2 dakika en fazla 3 dizin hizmeti sunar. Bu kısıtlamalar içeren bir öngörülen dizin boyutu tahmin etmek zor, ancak aşağıdaki örnekte, bir yaklaşım gösterilmektedir:

+ [Ücretsiz bir hizmet oluşturun](search-create-service-portal.md)
+ Küçük, temsili bir veri kümesi hazırlama (beş bin belgeleri ve örnek boyutunun yüzde onuna olduğunu varsayın)
+ [Bir başlangıç dizini oluşturun](search-create-index-portal.md) ve Portalı'nda boyutunu not edin (30 MB olduğunu varsayın)

Tüm belgeler dizine, örnek temsilcisi hem de veri kaynağının tamamı yüzde onuna eşit olduğu varsayılırsa, 30 MB dizin yaklaşık 300 MB olur. Bu ön numarasıyla kullanarak iki dizinler için (geliştirme ve üretim), depolama gereksinimleri 600 MB Toplam bütçeye çift bu tutar olabilir. Bu kolayca tarafından sağlanıyorsa **temel** var. başlar bu nedenle, katman.

### <a name="step-2-develop-refined-estimates-using-a-billable-tier"></a>2. Adım: Daraltılmış tahminleri Faturalandırılabilir katmandan kullanarak geliştirin

Bazı müşteriler büyük örnekleme ve işleme sürelerini uyum sağlayacak ve ardından dizin miktarı, boyutu ve sorgu birimleri gerçekçi tahminleri geliştirme sırasında geliştirme ayrılmış kaynaklarla başlatmak tercih eder. Başlangıçta, bir en iyi tahmin tahminine göre sağlanan bir hizmet ve Geliştirme projesinin geliştikçe ardından takımlar genellikle varolan hizmeti üzerinden veya öngörülen üretim iş yükleri için kapasite altında mı olduğunu. 

1. [Hizmet sınırları her katmanında gözden](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity#index-limits) alt katmanları ihtiyacınız dizinleri miktarını destekleyip desteklemediğini belirlemek için. Arasında **temel**-**S1**-**S2** katmanları, dizin sınırları, 15-50-200, sırasıyla.  **Depolama için iyileştirilmiş** katmanı, çok büyük dizinlerde düşük bir sayı desteklemek için tasarımcı olduğundan 10 dizin sınırı vardır.

1. [Faturalanabilir bir katmanda bir hizmet oluşturma](search-create-service-portal.md):

    + Üzerinde düşük başlangıç **temel** veya **S1** , öğrenme eğrisi başında olması durumunda.
    + En yüksek Başlat **S2** ve hatta **S3**, büyük ölçekli dizin oluşturma ve sorgu Optional yükleniyor.
    + Depolama için iyileştirilmiş, adresindeki **L1** veya **L2**, büyük miktarda veri dizin ve sorgu yükünü dahili bir iş uygulaması gibi nispeten düşük.

1. [Bir başlangıç dizini oluşturun](search-create-index-portal.md) kaynak verileri bir dizine nasıl çevirir belirlemek için. Dizin boyutu tahmin etmek için tek yolu budur.

1. [Depolama, hizmet sınırları, sorgu birimi ve gecikme süresi izleme](search-monitor-usage.md) portalında. Portal ikinci, daraltılmış sorgular ve arama gecikme süresi başına sorgu gösterir; bunların tümü doğru katmanını seçtiyseniz, karar vermenize yardımcı olabilir. Portal ölçümleri yanı sıra ayrıntılı, geçişli tıklatma analizi gibi etkinleştirerek izlemeyi yapılandırabilirsiniz [trafik analizi arama](search-traffic-analytics.md). 

Sınırlarını depolama (bölümler) veya sınırlarını kaynaklar (dizin, dizin oluşturucular ve benzeri) tarafından kullanımı aracılığıyla hangisinin önce geldiğine bağlı ulaştığından dizin sayısına ve boyutuna eşit analiziniz için uygun. Portal, her ikisi de geçerli kullanım ve sınırlarını yan yana genel bakış sayfasında gösteren izlemenize yardımcı olur.

> [!NOTE]
> Belgeler fazlalık veriler içeriyorsa depolama gereksinimlerini aşırı inflated olabilir. İdeal olarak, belgeler, yalnızca arama deneyimi için ihtiyacınız olan verileri içerir. İkili veriler aranabilir olmayan ve ayrı olarak (belki de bir Azure tablo veya blob depolama alanında) bir alanda bir URL başvuru dış verileri tutmak için dizin ile depolanması gerekir. Tek bir belgenin en büyük boyutu 16 MB'tır (veya daha az ise bir istekte birden çok belge karşıya toplu olarak). [Azure Search'te hizmet sınırları](search-limits-quotas-capacity.md) daha fazla bilgi bulunur.
>

**Sorgu toplu konuları**

Sorguları saniye (QPS) performans ayarlama sırasında teklifleriyle kazandığında bir ölçüm, ancak gizliliğe yüksek sorgu toplu beklenenden sürece genellikle katmanını göz önünde bulundurarak değil.

Standart katmanların Ek çoğaltmalar aracılığıyla daha hızlı sorgu döngü paralel işleme karşı ve ek bölümleri yüklemek için destek, bölüm çoğaltmalarını dengesi teslim edebilirsiniz. Hizmet sağlandıktan sonra performans için ayarlayabilirsiniz.

Gizliliğe Sürdürülen güçlü sorgu birimlerden beklediğiniz müşterilerin daha yüksek düşünün **standart** katmanları, daha güçlü donanım tarafından desteklenir. Bölümleri ve çoğaltmalarını çevrimdışı duruma getirin veya gerçekleştirmek bu sorgu birimlerin başarısız olduğunda bile daha düşük bir katman hizmetine geçin. Sorgu aktarım hızını hesaplamaya yönelik hakkında daha fazla bilgi için bkz. [Azure Search performans ve iyileştirme](search-performance-optimization.md).

Depolama katmanları sorgu gecikme gereksinimleri esnek biraz olduğu daha fazla genel dizin depolaması kullanılabilir destekleyen büyük veri iş yüklerini doğru yalın iyileştirilmiştir.  Paralel işleme karşı ve ek bölümleri yüklemek için hala bu ek çoğaltmalar havuzlamanızı. Hizmet sağlandıktan sonra performans için ayarlayabilirsiniz.

**Hizmet düzeyi sözleşmeleri**

**Ücretsiz** katmanı ve önizleme özellikleri ile birlikte verilmez [hizmet düzeyi sözleşmeleri (SLA'lar)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). Hizmetiniz için yeterli yedeklilik sağlarken, Faturalanabilir tüm katmanlar için SLA etkili olur. İki veya daha fazla çoğaltma için sorgu (okuma) SLA gereklidir. Üç veya daha fazla çoğaltma, sorgu ve dizin oluşturma (okuma-yazma) SLA'sı için gereklidir. Bölüm sayısı, bir SLA husus değil. 

## <a name="tips-for-tier-evaluation"></a>Katman değerlendirme için ipuçları

+ Etkin dizinler oluşturmak nasıl ve hangi yenileme yöntemleri olan öğrenin az impactful. Öneririz [trafik analizi arama](search-traffic-analytics.md) sorgu etkinliği elde edilen içgörüleri için.

+ Sorgular oluşturabilir ve kullanım biçimlerini (yoğun olmayan saatlerde dizin sorgular çalışma saatleri) geçici olarak veri toplamak ölçümlerin sabitlenmesine ve gelecek hizmet kararları sağlama bildirmek için bu verileri kullanın. Pratik bir saatlik veya günlük temposu adresindeki olsa da, bölümler ve düzeyleri eylemde garanti altına almak yeterince uzun tutarsanız planlı değişiklikler sorgu birimler, ya da planlanmamış ancak sürekli değişiklikler karşılamak için kaynakları dinamik olarak ayarlayabilirsiniz.

+ Sağlama tek dezavantajı, Gerçek gereksinimler tahmini büyük olması durumunda bir hizmeti ayırma gerekebilir olduğunu unutmayın. Hizmet kesintisi yaşamamak için daha yüksek bir katmandan aynı abonelikte yeni bir hizmet oluşturmak ve çalıştırmak yan yana tüm uygulamaları ve istekleri yeni uç nokta hedef kadar.

## <a name="next-steps"></a>Sonraki adımlar

İle başlayan bir **ücretsiz** katmanı ve özelliklerini anlamak için verilerinizin bir alt kümesi kullanarak ilk dizini oluşturun. Azure Search'te veri yapısı bir ters dizini, boyut burada ve ters dizin karmaşıklığını içeriğe göre belirlenir. Yüksek düzeyde yedekli içeriği son derece düzensiz içeriği daha küçük bir dizin neden eğilimindedir unutmayın. Bu nedenle, bu dizin depolama gereksinimlerini belirleyen bir veri kümesi boyutu yerine içerik özellikleri olur.

Dizin boyutu, ilk hakkında bir fikir sonra [Faturalanabilir bir hizmet sağlama](search-create-service-portal.md) ya da bu makalede ele alınan katmanları birinde **temel**, **standart**, veya **Depolama için iyileştirilmiş** katmanı. Veri boyutlandırma üzerinde hiçbir yapay kısıtlama gevşeyin ve [dizininizi yeniden](search-howto-reindex.md) tüm gerçekten aranabilir olmasını istediğiniz verileri içerecek şekilde.

[Bölümleri ve çoğaltmalarını tahsis](search-capacity-planning.md) ihtiyaç duyduğunuz ölçek ve performans almak için gerektiği şekilde.

Performans ve kapasite ince ise gerçekleştirilir. Aksi takdirde, bir arama hizmeti daha yakından hizalar, ihtiyaçlarınıza göre farklı bir katmandan yeniden oluşturun.

> [!NOTE]
> Sorularınızı hakkında daha fazla yardım için postalayabilir [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) veya [Azure desteğine başvurun](https://azure.microsoft.com/support/options/).
