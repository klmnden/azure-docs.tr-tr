---
title: Azure Search Hizmeti - Azure arama için fiyatlandırma katmanı veya SKU seçin
description: "Azure Search'ü bu Sku'da sağlanabilir: Ücretsiz, temel ve standart ve standart, çeşitli kaynak yapılandırmaları ve kapasite düzeyleri kullanılabilir."
services: search
author: HeidiSteen
manager: cgronlun
tags: azure-portal
ms.service: search
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 00422209302bbcc2139be4f6b490f0bb2816c051
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65539269"
---
# <a name="choose-a-pricing-tier-for-azure-search"></a>Azure arama için bir fiyatlandırma katmanı seçin

Bir Azure Search Hizmeti oluşturduğunuzda bir [kaynak oluşturulduğu](search-create-service-portal.md) fiyatlandırma katmanı veya hizmet ömrü boyunca sabit SKU. Ücretsiz, temel, standart ve depolama için iyileştirilmiş Katmanlar içerir. Standart ve depolama için iyileştirilmiş, çeşitli yapılandırmaları ve kapasite ile kullanılabilir.

Hizmet değerlendirebilmeniz için çoğu müşteri ücretsiz katman ile başlayın. Bunlar ardından geliştirme ve üretim dağıtımları için daha yüksek katmanlardan birine yükseltin. Tüm kılavuzlarımız ve öğreticilerimizden yararlanarak kaynak kullanımı yoğun bilişsel arama yönelik olanlar dahil olmak üzere ücretsiz bir katmanı kullanarak tamamlayabilirsiniz.

> [!NOTE]
> Microsoft, test ve geri bildirim toplamak amacıyla bir deneme için indirimli fiyatlandırma bir Önizleme'de depolama için iyileştirilmiş hizmet katmanları şu an sunar. Bu katmanları genel olarak kullanılabilir olduğunda son fiyatlandırma daha sonra duyurulacaktır. Biz, üretim uygulamaları için bu katmanları kullanan karşı önerin.

Katmanları hizmeti (yerine özellikleri) barındıran donanım özellikleri yansıtır ve tarafından ayrılır:

+ Oluşturabileceğiniz dizinleri sayısı.
+ Boyutu ve hızlı bölümlerin (fiziksel depolama).

Ücretsiz katman dahil, tüm katmanlarda genellikle özellik eşliği sunmasına rağmen daha büyük iş yükleri daha yüksek katmanlara yönelik bir gereksinim okuyabilirsiniz. Örneğin, [yapay ZEKA ile Bilişsel hizmetler dizini oluşturma](cognitive-search-concept-intro.md) veri kümesini küçük olmadığı sürece uzun süre çalışan becerileri ücretsiz bir hizmet o zaman aşımına sahip.

> [!NOTE] 
> Özellik eşliği istisnası [dizin oluşturucular](search-indexer-overview.md), hangi S3 HD üzerinde mevcut değildir.
>

Bir katman içinde yapabilecekleriniz [çoğaltma ve bölüm kaynakları](search-capacity-planning.md) artırabilir veya ölçek azaltabilirsiniz. Biri veya her ikisi ile başlayın ve geçici olarak bir dizin oluşturma ağır iş yükü için işlem gücünü yükseltmek. Bir katman içinde kaynak düzeylerini ayarlama olanağı esneklik kazandırır ancak biraz da analiz karmaşık hale getirir. Daha fazla kaynak/kopyaları olan daha düşük bir katmana daha iyi değeri ve daha az kaynak ile daha yüksek bir katmana performans sunan görmek için denemeniz gerekebilir. Ne zaman ve neden kapasiteyi ayarlamak hakkında daha fazla bilgi için bkz: [performans ve iyileştirme konuları](search-performance-optimization.md).

## <a name="tiers-for-azure-search"></a>Azure Search için katmanları

Aşağıdaki tabloda kullanılabilir Katmanlar listelenmektedir. Üzerinde çeşitli katmanları hakkında daha fazla bilgi bulabilirsiniz [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/search/), [Azure Search'te hizmet sınırları](search-limits-quotas-capacity.md) makalesini inceleyin ve portalı sayfasında bir hizmet sağlama.

|Katman | Kapasite |
|-----|-------------|
|Ücretsiz | Diğer abonelerle paylaşılan. Ölçeklenebilir. Üç dizin ve 50 MB depolama alanı ile sınırlıdır. |
|Temel | Bilgi işlem kaynaklarını daha küçük ölçekli üretim iş yükleri için ayrılmış. Bir 2 GB'lık bölümü ve üç kopyaya kadar. |
|Standart 1 (S1) | S1 ve sonraki her düzeyde daha fazla depolama ve işleme kapasitesi makinelerle ayrılmış. S1 (hizmet başına en fazla 300 GB) ile 25 GB/bölüm bölüm boyutu var. |
|Standart 2 (S2 için) | S1, ancak 100 GB'lık bölümleri (ve 1,2 TB hizmet başına en fazla) ile benzer. |
|Standart 3 (S3 için) | 200 GB bölümlerle (hizmet başına en çok 2,4 TB). |
|Standart 3 yüksek yoğunluklu (S3 HD) | Yüksek yoğunluklu olan bir *modu barındırma* S3. Temel alınan donanım çok sayıda küçük dizinler için en iyi duruma getirilmiş ve çok kiracılı mimari senaryoları içindir. S3 HD, S3, ancak donanım optimize gibi çok sayıda küçük dizinleri üzerinde hızlı dosya okuma için aynı birim başına ücret sahiptir.|
|Depolama için iyileştirilmiş 1 (L1) | 1 TB bölümlerle (hizmet başına en çok 12 TB). |
|Depolama için iyileştirilmiş 2 (L2) | 2 TB bölümlerle (hizmet başına en fazla 24 TB). |

> [!NOTE] 
> Depolama için iyileştirilmiş katmanlar, daha düşük bir fiyatla TB başına standart katmanların değerinden daha büyük depolama kapasitesi sunar. Birincil artırabilen uygulamanıza özel gereksinimler için doğrulamalıdır daha yüksek sorgu gecikme olur.  Bu katmanının performans değerlendirmeleri hakkında daha fazla bilgi için bkz: [performans ve iyileştirme konuları](search-performance-optimization.md).
>

## <a name="how-billing-works"></a>Faturalandırma nasıl çalışır?

Azure Search'te ödemeniz üç yolu vardır ve sabit ve değişken bileşenleri vardır. Bu bölümde üç fatura bileşeni açıklanmaktadır: çekirdek hizmet maliyetleri, veri çıkış ücretlerini ve yapay ZEKA zenginleştirilmiş dizinleme.

### <a name="core-service-costs-fixed-and-variable"></a>Çekirdek hizmet maliyetleri (sabit ve değişken)

İçin kendi en düşük ücret ilk arama birimi (1 çoğaltma x 1 bölüm) hizmetidir. Bu en düşük hizmet ömrü boyunca sabittir hizmeti üzerinde herhangi bir şey bu yapılandırma'dan çalışmaz.

En düşük çoğaltmalar ve bölümler birbirinden bağımsız olarak ekleyebilirsiniz. Örneğin, yalnızca çoğaltmalar veya bölümler yalnızca ekleyebilirsiniz. Kapasite çoğaltmalar ve bölümler arasında artımlı bir artış değişkeni maliyet bileşenini olun.

Faturalandırma temel bir [formül (çoğaltmaları bölümler x x oranı)](#search-units). Fiyat, seçtiğiniz fiyatlandırma katmanına bağlıdır.

Aşağıdaki ekran görüntüsünde, ücretsiz, temel ve S1 birimi başına fiyatlandırma gösterilir. (S2, S3, L1 ve L2 gösterilmez.) Temel bir hizmeti oluşturursanız, aylık maliyetiniz için görüntülenen değeri ortalama *fiyat 1*. Standart bir hizmeti için aylık maliyetiniz için görüntülenen değeri ortalama *fiyat 2*. Hesaplama gücü ve depolama kapasitesini art arda her katmanında büyük olduğu için her katmanı için birim maliyetlerini artırabilir. Azure Search fiyatlar kullanılabilir [Azure fiyatlandırma sayfasını arama](https://azure.microsoft.com/pricing/details/search/).

![Birim başına fiyatlandırma](./media/search-sku-tier/per-unit-pricing.png "birim başına fiyat")

Bir arama çözümü maliyeti tahmin etmeye yönelik, fiyatlandırma aklınızda tutun ve kapasite doğrusal değildir. (Kapasite Katlama birden fazla maliyet iki katına çıkar.) Formül Works ilişkin bir örnek için bkz. [çoğaltmalar ve bölümler ayırma](search-capacity-planning.md#how-to-allocate-replicas-and-partitions).

#### <a name="billing-based-on-search-units"></a>Arama birimleri temel faturalandırma

Azure arama işlemleri için anlamak için en önemli fatura kavram *arama birimi* (SU). Çoğaltmalar ve bölümler dizin oluşturma ve sorgular için Azure Search bağlı olduğundan, onu yalnızca birini veya diğerini tarafından faturalandırmak için anlam ifade etmez. Bunun yerine, her iki bileşik üzerinde üzerinden faturalandırılır.

SU olan çarpımını *çoğaltmaları* ve *bölümleri* hizmeti tarafından kullanılan: **(R P x SU =)** .

Her hizmetin en az bir SU (bir çoğaltma bir bölüm ile çarpılmış) başlar. Herhangi bir hizmeti için en fazla 36 su ' dir. Bu maksimum birden çok yolla ulaşılabilir: 6 bölümler x 6 çoğaltmalar veya 3 bölümler x 12 çoğaltmalar, örneğin. Toplam Kapasite (9 SUs faturalandırılır. Örneğin, bir yineleme 3, 3 bölümlü hizmeti) sayısından az kullanımı yaygındır. Bkz: [bölüm ve çoğaltma birleşimlerini](search-capacity-planning.md#chart) geçerli birleşimleri için grafik.

SU saatlik faturalandırma tarifesi var. Her katman, giderek daha yüksek bir ücrete sahiptir. Daha yüksek katmanlarında daha büyük ve daha hızlı bölümleri ile gelir ve bu söz konusu katman için genel olarak daha yüksek bir saatlik ücret katkıda bulunur. Her katman fiyatlar görüntüleyebileceğiniz [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/search/) sayfası.

Çoğu müşteri, toplam kapasite çevrimiçi olarak yalnızca bir kısmını rest yedekte bulunduran getirin. , Bölümler ve çoğaltmalar çevrimiçi duruma sayısını faturalandırma için SU tarafından hesaplanan formül belirler saat bazında ödeme yaparsınız.

### <a name="data-egress-charges-during-indexing"></a>Dizin oluşturma sırasında veri çıkış ücretleri

Kullanarak [Azure Search dizin oluşturucularında](search-indexer-overview.md) hizmetlerinizi konumuna bağlı olarak, faturalandırma etkileyebilir. Tamamen verilerinizi aynı bölgede Azure Search Hizmeti oluşturursanız, veri çıkış ücretlerini ortadan kaldırabilir. İşte bazı bilgileri [bant genişliği fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/bandwidth/):

+ Microsoft Azure arama herhangi bir Azure hizmeti için gelen tüm veriler için veya tüm giden veri için ücret uygulamaz.

+ Multiservice çözümlerinde, tüm hizmetleri aynı bölgede olması durumunda kablo üzerinden geçmesini veri için ücret alınmaz.

Hizmetleri farklı bölgelerde bulunuyorsa için giden veri ücretleri uygulanır. Bu ücretler, Azure Search faturanıza aslında bir parçası değildir. Veri veya AI zenginleştirilmiş dizin oluşturucular veri çekmek için farklı bölgelerden kullanıyorsanız, genel faturanıza yansıtılan maliyetleri görürsünüz çünkü bunlar burada bahsedilen.

### <a name="ai-enriched-indexing-with-cognitive-services"></a>Yapay ZEKA-zenginleştirilmiş Bilişsel hizmetler ile dizinleme

İçin [yapay ZEKA ile Bilişsel hizmetler dizini oluşturma](cognitive-search-concept-intro.md), Faturalanabilir Azure Bilişsel hizmetler kaynağı, aynı bölgede Azure Search, Kullandıkça Öde işleme S0 fiyatlandırma katmanında eklemek planlamanız gerekir. Bilişsel Hizmetleri ekleme ile ilişkili hiçbir sabit bir ücret yoktur. Yalnızca gereksinim duyduğunuz işleme için ödeme yaparsınız.

Görüntü ayıklama belge çözme sırasında bir Azure Search ücretlendirme yapılır. Belgelerinizi ayıklanan görüntüleri sayısına göre faturalandırılır. Metin ayıklama şu anda ücretsiz olarak kullanılabilir.

Doğal dil işleme gibi diğer zenginleştirmelerinin dayalı [yerleşik bilişsel beceriler](cognitive-search-predefined-skills.md) ve Bilişsel hizmetler kaynağı göre faturalandırılır. Bilişsel hizmetler kullanarak doğrudan görev gerçekleştirilen gibi bunlar aynı fiyatı üzerinden faturalandırılırsınız. Daha fazla bilgi için [bir beceri kümesi ile bir Bilişsel hizmetler kaynağı ekleme](cognitive-search-attach-cognitive-services.md).

<a name="search-units"></a>

#### <a name="billing-for-image-extraction-in-cognitive-search"></a>Bilişsel arama görüntü ayıklama faturalandırması

Bilişsel arama işlem hattı dizinleme dosyalarından görüntüleri ayıklamak, Azure Search faturanızda bu işlem için ücretlendirilirsiniz. İçinde bir [dizin oluşturucu yapılandırmasını](https://docs.microsoft.com/rest/api/searchservice/create-indexer#indexer-parameters), **imageAction** görüntü ayıklama tetikler parametredir. Varsa **imageAction** "hiçbiri" (varsayılan), görüntü ayıklama için ücret ödemezsiniz ayarlanır.

Fiyatlandırma, değişikliğe tabi olduğu. Öğesinde belgelendirilen [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/search/) Azure Search için sayfa.

#### <a name="billing-for-built-in-skills-in-cognitive-search"></a>Bilişsel arama yerleşik yeteneklerinizi faturalandırması

Zenginleştirme işlem hattı ayarladığınızda herhangi [yerleşik yetenekler](cognitive-search-predefined-skills.md) işlem hattında kullanılan makine öğrenimi modelleri üzerinde temel alır. Bu modeller, Bilişsel hizmetler tarafından sağlanır. Dizin oluşturma sırasında bu modeller kullanmanız durumunda kaynağın doğrudan istenmesi durumunda olduğu gibi aynı fiyattan faturalandırılırsınız.

Örneğin, optik karakter tanıma (OCR) karşı taranan JPEG dosyaları kullanan bir işlem hattı sahip ve elde edilen metnini serbest biçimli arama sorguları için bir Azure Search dizinine gönderildiğinde varsayalım. Bir dizin oluşturucu ile dizin oluşturma işlem hattınızı verilebilir [OCR beceri](cognitive-search-skill-ocr.md), beceri yanıtlayabiliriz [Bilişsel hizmetler kaynağa bağlı](cognitive-search-attach-cognitive-services.md). Dizin Oluşturucu çalıştırdığınızda, OCR yürütme ücretleri Bilişsel kaynakları faturanızda görünür.

## <a name="tips-for-reducing-costs"></a>Maliyetleri azaltmak için ipuçları

Faturanızı azaltmak için hizmeti Kapat olamaz. Ayrılmış her zaman, özel kullanım ömrü boyunca hizmetiniz için ayrılan işletimsel, kaynaklardır. Çoğaltmalar ve bölümler hala kabul edilebilir performans sağlayan bir düzeye indirmenin faturanızı düşürebilir tek yolu olduğundan ve [SLA Uyumluluk](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Maliyetleri azaltmak için bir yolu, bir katman daha düşük bir saatlik ücret seçmektir. Saatlik ücretler S1, S2 veya S3 ücretlerinden daha düşük. Varsayılmıştır hizmetiniz, yük projeksiyonlar alt sonunda sağlayın ve bu hizmet aşıyorsa, daha büyük katmanlı ikinci bir hizmet oluşturabilir, ikinci hizmetine yeniden ve ilk silin.

Şirket içi sunucular için kapasite planlama yaptığınızdan varsa, yaygın "Gelecekteki büyümeyi işleyebilmesi için kaydolabileceksiniz" bilirsiniz. Belirli bir satın alma için özgürsünüz çünkü bir bulut hizmeti ile daha fazla maliyet tasarrufu agresif daha sonra amacınızın. Geçerli yeterli değilse, daha yüksek katmanlı bir hizmet için her zaman geçiş yapabilirsiniz.

### <a name="capacity"></a>Kapasite

Azure Search'te kapasite olarak yapılandırılmış *çoğaltmaları* ve *bölümler*.

+ Çoğaltmaları arama hizmeti örnekleridir. Her yineleme, bir dizinin yük dengeli bir kopya barındırır. Örneğin, hizmete yüklenen her dizin altı kopyasını altı hizmetiyle sahiptir.

+ Bölümler, dizinleri depolayın ve otomatik olarak aranabilir verileri bölün. İki bölüm dizininizi yarıya bölme, üç ve benzeri bölmek üç bölüm. Kapasite açısından *bölüm boyutu* katmanları arasında birincil ayırt edici özelliği.

> [!NOTE]
> Tüm standart ve depolama için iyileştirilmiş Katmanlar Destek [çoğaltmalar ve bölümler esnek birleşimlerini](search-capacity-planning.md#chart) , böylece [hızı ve depolama için en iyi duruma](search-performance-optimization.md) Bakiye değiştirerek. Temel katman, yüksek kullanılabilirlik için en fazla üç çoğaltma sunar ancak yalnızca bir bölümü vardır. Ücretsiz katman, ayrılmış kaynaklarda sağlamadığınızdan: bilgi işlem kaynakları, birden çok abone tarafından paylaşılır.

### <a name="more-about-service-limits"></a>Hizmet sınırları hakkında daha fazla bilgi

Ana bilgisayar kaynakları dizin ve dizin oluşturucu gibi hizmetleri. Her katman uygular [hizmet sınırları](search-limits-quotas-capacity.md) oluşturmak için kullanabileceğiniz kaynakları sayısı. Bu nedenle dizin (ve diğer nesneleri) sayısı ikinci ayırt edici katmanları arasında özelliğidir. Her seçeneği portalında gözden geçirirken, dizinlerin sayısına yönelik sınırlar unutmayın. Uzmanlık becerileri, dizin oluşturucular ve veri kaynakları gibi diğer kaynaklar için dizin sınırları yapıştırılmış.

## <a name="consumption-patterns"></a>Tüketim modelleri

Çoğu müşteri, bunlar süresiz olarak saklayın, ücretsiz hizmetle başlar ve önemli geliştirme veya üretim iş yükleri için standart veya depolama için iyileştirilmiş katmanlarından birini seçin.

![Azure Search'ü fiyatlandırma katmanları](./media/search-sku-tier/tiers.png "Azure Search fiyatlandırma katmanları")

Düşük ve yüksek uçlarında, önemli, ancak alışılmadık tüketim desenleri için temel ve S3 HD alır. Küçük bir üretim iş yükleri için temel'dir. SLA'ları, adanmış kaynaklar ve yüksek kullanılabilirlik sunar, ancak 2 GB toplam belirtti uygun depolama sağlar. Bu katman, sürekli olarak kullanılabilir kapasite underutilize müşteriler için mühendislik. Yüksek sonunda, S3 HD iş ortakları, iş yükleri için ISV'lerin, tipik olan [çok kiracılı çözüm](search-modeling-multitenant-saas-applications.md), ya da çok sayıda küçük dizinler için çağıran herhangi bir yapılandırma. Genellikle basit boş olduğundan veya S3 HD doğru katmanı gösterir. Onay istiyorsanız için gönderebilir [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) veya [Azure desteğine başvurun](https://azure.microsoft.com/support/options/) Kılavuzu.

Daha yaygın olarak kullanılan standart katmanların, S1, S3 aracılığıyla, artan kapasite düzeylerini ilerlemeyi yapın. Bölüm boyutu dönüm noktalarını ve dizin, dizin oluşturucular ve corollary kaynakları sayıda sınırlamaları vardır:

|  | S1 | S2 | S3 |  |  |  |  |
|--|----|----|----|--|--|--|--|
| Bölüm boyutu|  25 GB | 100 GB | 200 GB |  |  |  |  |
| Dizin ve dizin oluşturucu sınırları| 50 | 200 | 200 |  |  |  |  |

S1 ayrılmış kaynakları ve birden çok bölüm gereken müşteriler için ortak bir seçimdir. S1 bölümler 12 bölüme kadar ve 25 GB çoğaltmalar bir en üst düzeye bölümleri, bir hizmet başına sınır 300 GB sağlama sunar. (Bkz [tahsis bölümleri ve çoğaltmalarını](search-capacity-planning.md#chart) daha fazla bilgi için ayırmaları dengeli.)

Bölüm boyutu ve depolama portal ve fiyatlandırma sayfalarına odağı koyabilir, fakat, her bir katman için tüm özellikleri (disk kapasitesi, CPU hızı) işlem genellikle fiyatıyla doğrusal olarak artırın. S2 çoğaltma S1'den daha hızlıdır ve S3 S2 hızlıdır. S3 katmanlarından orantısız daha hızlı g/ç ile doğrusal işlem fiyatlandırma desenden bölün. G/ç iyileşti bekliyorsanız, S3 ile alt katmanları yapabileceğinizden daha fazla IOPS elde edebilirsiniz aklınızda bulundurun.

S3 ve S3 HD aynı yüksek kapasiteli altyapısı tarafından desteklenir, ancak farklı şekilde kendi sınırlarını ulaşmadan. S3 hedefleyen çok büyük dizinler, daha az sayıda kaynak bağlı kendi üst sınırı olacak şekilde (her hizmet için 2,4 TB). S3 HD çok sayıda küçük dizinleri hedefler. 1.000 dizinleri S3 HD dizin kısıtlamalarını biçiminde sınırlarını ulaşır. Bir S3 HD müşterisi olduğunuz ve 1. 000'den fazla dizinleri ihtiyacınız varsa, nasıl ilerleyeceğiniz hakkında daha fazla bilgi için Microsoft Support başvurun.

> [!NOTE]
> Belge limitleri tek seferde bir durum olan, ancak artık yeni hizmetler için geçerlidir. Belge limitleri hala uygulandığı koşullar hakkında daha fazla bilgi için bkz: [belge sınırları](search-limits-quotas-capacity.md#document-limits).
>

Depolama en iyi duruma getirilmiş katmanları, L1 ve L2, sorgunun gecikme süresi en aza en yüksek önceliğiniz değilse, ancak son kullanıcılar, göreli olarak az sayıda büyük veri gereksinimleri olan uygulamalar için idealdir.  

|  | L1 | L2 |  |  |  |  |  |
|--|----|----|--|--|--|--|--|
| Bölüm boyutu|  1 TB | 2 TB |  |  |  |  |  |
| Dizin ve dizin oluşturucu sınırları| 10 | 10 |  |  |  |  |  |

L2 iki kez toplam depolama kapasitesinin L1 sunar.  En fazla dizininizi gereken düşündüğünüz veri miktarına göre katmanınızı seçin. En fazla 12 TB için 1 TB'lik artışlarla L1 katmanı bölümleri ölçeği artırabilirsiniz. L2 bölümleri 2 TB'a her bölüm en fazla 24 TB'a kadar artırın.

## <a name="evaluating-capacity"></a>Kapasite değerlendiriliyor

Kapasite ve hizmeti çalıştırmanın maliyetlerini doğrudan ilişkilidir. Katmanları, iki düzeyde sınırları büyük oranda yansıtmaktadır: depolama ve kaynaklar. Hem hakkında çünkü sınırlarından hangisi önce ulaşmak etkili bir sınırdır almalısınız.

İş gereksinimleri genellikle dizin ihtiyacınız olacak sayısı dikte. Örneğin, belgeler için bir büyük havuz genel bir dizin gerekebilir. Veya, bölge, uygulama veya iş talebini göre birden çok dizin ihtiyacınız olabilir.

Bir dizinin boyutunu belirlemek için gerekir [bir yapı](search-create-index-portal.md). Azure Search'te veri yapısı öncelikle olduğu bir [dizin ters](https://en.wikipedia.org/wiki/Inverted_index) yapısı, kaynak verileri daha farklı özelliklere sahiptir. Tersine çevrilmiş bir dizin için boyutu ve karmaşıklığı içeriğe göre mutlaka içine akış veri miktarına göre belirlenir. Yüksek artıklık ile büyük veri kaynağı, yüksek oranda değişken içeriği içeren daha küçük bir veri kümesini daha küçük bir dizinde neden olabilir. Bu nedenle dizin boyutu özgün veri kümesinin boyutuna göre çıkarsanacak nadiren mümkündür.

> [!NOTE] 
> Dizinler ve depolama için gelecekteki gereksinimlerini tahmin etme gibi kararın düşünüyorsanız olsa da, bunun yapılması değer var. Çok düşük bir katmanın kapasitesini ettik, daha yüksek bir katmandan yeni bir hizmet sağlamak gerekir ve ardından [dizinlerinizi yeniden](search-howto-reindex.md). Herhangi bir SKU bir hizmetten diğerine yerinde yükseltmesini yoktur.
>

### <a name="step-1-develop-rough-estimates-by-using-the-free-tier"></a>1. Adım: Kaba tahminleri ücretsiz Katmanı'nı kullanarak geliştirme

Ücretsiz katmanı ile kapasitesini tahmin etmek için bir yaklaşım başlamaktır. Ücretsiz hizmet en fazla üç dizin, 50 MB depolama alanına ve saat dizin oluşturma, 2 dakika sunduğunu unutmayın. Bu bir öngörülen dizin boyutu bu kısıtlamaları ile tahmin etmek zor olabilir. Uygulayabileceğiniz bir yaklaşım şöyledir:

+ [Ücretsiz bir hizmet oluşturma](search-create-service-portal.md).
+ Küçük, temsili veri kümesi (örneğin, 5.000 belgeleri ve yüzde 10 örnek boyutu) hazırlayın.
+ [Bir başlangıç dizini oluşturun](search-create-index-portal.md) ve boyutunu (örneğin, 30 MB) portalında not edin.

Örnek, temsilci ve veri kaynağının tamamı % 10'u ise, tüm belgeler dizine yaklaşık 300 MB 30 MB dizin dönüşür. Bu ön numarasıyla kullanarak iki dizinler için (geliştirme ve üretim) bütçeye çift bu tutar olabilir. Bu, 600 MB Toplam depolama gereksinimleri sağlar. Burada başlar bu nedenle bu gereksinimi temel katmanı tarafından kolayca uyulmuş olur.

### <a name="step-2-develop-refined-estimates-by-using-a-billable-tier"></a>2. Adım: Daraltılmış tahminleri, Faturalandırılabilir katmandan'ı kullanarak geliştirme

Bazı müşteriler, daha büyük örnekleme ve işleme sürelerini uyum sağlayacak ve ardından dizin miktarı, boyutu ve sorgu birimleri gerçekçi tahminleri geliştirme sırasında geliştirme ayrılmış kaynakları başlatmak tercih eder. Başlangıçta, bir hizmet bir en iyi tahmin tahminine dayanarak sağlanır. Geliştirme projesinin geliştikçe daha sonra takımlar genellikle varolan hizmeti üzerinden veya öngörülen üretim iş yükleri için kapasite altında mı olduğunu.

1. [Hizmet sınırları her katmanında gözden](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity#index-limits) alt katmanları ihtiyacınız dizinleri sayısı destekleyip desteklemediğini belirlemek için. Temel, S1 ve S2 katmanlarda dizini 15, 50 ve 200, sırasıyla limitlerdir. Çok büyük dizinlerde düşük bir sayı desteklemek için tasarlanan olduğundan depolama için iyileştirilmiş katmanı 10 dizin sınırı vardır.

1. [Faturalanabilir bir katmanda bir hizmet oluşturma](search-create-service-portal.md):

    + Öğrenme eğrisi başında kullanıyorsanız temel veya S1, düşük başlangıç.
    + Büyük ölçekli dizin oluşturma ve sorgu yüklerini sahip olacak biliyorsanız, S2 veya S3 bile, en yüksek başlatın.
    + L1 veya L2, için iyileştirilmiş depolama ile büyük miktarda veri dizini oluşturma ve sorgu yükünü dahili bir iş uygulaması ile nispeten düşük olarak başlatın.

1. [Bir başlangıç dizini oluşturun](search-create-index-portal.md) kaynak verileri bir dizine nasıl çevirir belirlemek için. Dizin boyutu tahmin etmek için tek yolu budur.

1. [Depolama, hizmet sınırları, sorgu birimi ve gecikme süresi izleme](search-monitor-usage.md) portalında. Portal ikinci, daraltılmış sorgular ve arama gecikme süresi başına sorguları gösterir. Tüm bu değerleri doğru katmanını seçtiyseniz, karar vermenize yardımcı olabilir. Ayrıntılı geçişli tıklatma analizi gibi değerleri sağlayarak izleme de yapılandırabilirsiniz [trafik analizi arama](search-traffic-analytics.md).

Analiz için eşit oranda dizin sayısına ve boyutuna önemlidir. Sınırlarını depolama (bölümler) veya sınırlarını kaynaklar (dizin, dizin oluşturucular ve benzeri) tarafından kullanımı aracılığıyla hangisi önce gelirse ulaşıldığında olmasıdır. Portal, her ikisi de geçerli kullanım ve sınırlarını yan yana genel bakış sayfasında gösteren izlemenize yardımcı olur.

> [!NOTE]
> Belgeler fazlalık veriler içeriyorsa, depolama alanı gereksinimleri şişirileceğini. İdeal olarak, belgeler, yalnızca arama deneyimi için gereken verileri içerir. İkili veriler aranabilir değil ve ayrı olarak (belki de bir Azure tablo veya blob depolama alanında) depolanması gerekir. Bir alan, bir URL başvuru dış verileri tutmak için dizinde sonra eklenmelidir. Tek bir belgenin en büyük boyutu 16 MB'tır (veya toplu bir istekte birden çok belge karşıya olduğunuz daha az ise). Daha fazla bilgi için [Azure Search'te hizmet sınırları](search-limits-quotas-capacity.md).
>

**Sorgu toplu konuları**

Saniyedeki sorgu sayısı (QPS), performans ayarlama sırasında önemli bir ölçüm olduğu halde gizliliğe yüksek sorgu toplu bekliyorsanız, genellikle yalnızca bir katman husustur.

Standart katmanların çoğaltmalar ve bölümler dengesi sağlayabilir. Yük Dengeleme için çoğaltmalar ekleyerek sorgu amortisman artırabilir veya paralel işleme için bölümler ekleyin. Hizmet sağlandıktan sonra performans için daha sonra ayarlayabilirsiniz.

Gizliliğe sürekli yüksek sorgu birimlerden bekliyorsanız, daha güçlü donanım tarafından desteklenen daha yüksek standart katmanları düşünmelisiniz. Bölümleri ve çoğaltmalarını çevrimdışı duruma getirin ya da bu sorgu birimlerin oluşmaz gerekiyorsa daha düşük katmanlı hizmetine bile geçin. Sorgu aktarım hızını hesaplamaya yönelik hakkında daha fazla bilgi için bkz. [Azure Search performans ve iyileştirme](search-performance-optimization.md).

Depolama için iyileştirilmiş katmanlar, büyük veri iş yükleri, sorgu gecikme süresi gereksinimlerine daha az önemli olduğunda için daha fazla genel olarak kullanılabilir dizin depolaması desteklemek için kullanışlıdır. Yine, Yük Dengeleme ve ek bölümleri paralel işleme için Ek çoğaltmalar kullanmanız gerekir. Hizmet sağlandıktan sonra performans için daha sonra ayarlayabilirsiniz.

**Hizmet düzeyi sözleşmeleri**

Ücretsiz katman ve önizleme özellikleri sağlamayan [hizmet düzeyi sözleşmeleri (SLA'lar)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). Hizmetiniz için yeterli yedeklilik sağlarken, Faturalanabilir tüm katmanlar için SLA etkili olur. İki veya daha fazla çoğaltma için sorgu (okuma) SLA'lar olması gerekir. Sorgu ve dizin oluşturma (okuma-yazma) SLA'ları için üç veya daha fazla çoğaltma olması gerekir. Bölüm sayısı SLA'lar etkilemez.

## <a name="tips-for-tier-evaluation"></a>Katman değerlendirme için ipuçları

+ Verimli dizin oluşturmayı öğrenin ve hangi yenileme yöntemleri en az bir etkiye sahip öğrenin. Kullanım [trafik analizi arama](search-traffic-analytics.md) sorgu etkinliği dayalı Öngörüler elde etmek için.

+ Sorgular oluşturmak ölçümlerin sabitlenmesine ve kullanım düzenlerini (yoğun olmayan saatlerde dizin sorgular çalışma saatleri) geçici olarak veri toplama. Hizmet sağlama bildirmek için bu verileri kullanın. Günlük veya saatlik bir tempoda pratik olmasa da, bölümler ve sorgu birimlerdeki planlı değişiklikler karşılamak için kaynakları dinamik olarak ayarlayabilirsiniz. Düzeyleri eylemde garanti altına almak yeterince uzun tutarsanız planlanmamış ancak sürekli değişiklikler de barındırabilir.

+ Tek dezavantajı underprovisioning, Gerçek gereksinimler, Öngörüler büyük olması durumunda bir hizmeti ayırma gerekebilir olduğunu unutmayın. Hizmet kesintisi yaşamamak için daha yüksek bir katmandan aynı abonelikte yeni bir hizmet oluşturmak ve çalıştırmak yan yana tüm uygulamaları ve istekleri yeni uç nokta hedef kadar.

## <a name="next-steps"></a>Sonraki adımlar

Ücretsiz katman ile başlayın ve özelliklerini anlamak için verilerinizin bir alt kümesi kullanarak bir başlangıç dizini oluşturun. Ters dizin yapısı Azure Search'te veri yapısıdır. Büyüklüğü ve karmaşıklığı bir ters dizinin içeriğe göre belirlenir. Yüksek düzeyde yedekli içeriği son derece düzensiz içeriği daha küçük bir dizin neden eğilimindedir unutmayın. Veri kümesi boyutu yerine içerik özellikleri dizin depolama gereksinimlerini belirlemek için.

İlk tahmini, dizin boyutu sonra [Faturalanabilir bir hizmeti sağlayın](search-create-service-portal.md) bu makalede ele alınan katmanlardan birine üzerinde: Temel, standart veya depolama için iyileştirilmiş. Veri boyutlandırma üzerinde hiçbir yapay kısıtlama gevşeyin ve [dizininizi yeniden](search-howto-reindex.md) aranabilir olmasını istediğiniz tüm verileri dahil etmek için.

[Bölümleri ve çoğaltmalarını tahsis](search-capacity-planning.md) ihtiyaç duyduğunuz ölçek ve performans almak için gerektiği şekilde.

Performansı ve kapasiteyi bir sakınca yoktur, hazırsınız. Aksi takdirde, bir arama hizmeti daha yakından hizalar, ihtiyaçlarınıza göre farklı bir katmandan yeniden oluşturun.

> [!NOTE]
> Sorularınız varsa postalayabilir [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) veya [Azure desteğine başvurun](https://azure.microsoft.com/support/options/).
