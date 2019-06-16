---
title: Tek bir hizmet - Azure Search içerik yalıtımı için çok kiracılı mimari modelleme
description: Azure Search kullanarak çok kiracılı SaaS uygulamaları için sık karşılaşılan tasarım desenleri öğrenin.
manager: jlembicz
author: LiamCavanagh
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: liamca
ms.custom: seodec2018
ms.openlocfilehash: 58d7ca65a14f9f774b19796c9beae2a7c84102ad
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61288731"
---
# <a name="design-patterns-for-multitenant-saas-applications-and-azure-search"></a>Çok kiracılı SaaS uygulamaları ve Azure Search için desenler tasarlama
Çok müşterili uygulamalarda herhangi bir sayıda göremez veya diğer bir kiracı veri paylaşımı Kiracı aynı Hizmetleri ve özellikleri sağlayan biridir. Bu belge, Azure Search ile derlenen çok kiracılı uygulamalar için Kiracı yalıtımı stratejileri açıklanır.

## <a name="azure-search-concepts"></a>Azure arama kavramları
Bir hizmet olarak arama çözümü olan Azure Search, geliştiricilerin herhangi bir altyapı yönetme veya Uzman bilgileri alma olma olmadan uygulamalara zengin arama deneyimleri eklemek olanak tanır. Veriler hizmetine yüklenir ve ardından bulutta depolanır. Basit istekler için Azure Search API kullanarak veri sonra değiştirilen Aranan ve. Hizmetine genel bir bakış bulunabilir [bu makalede](https://aka.ms/whatisazsearch). Tasarım desenleri ele almadan önce Azure Search'te bazı kavramları anlamak önemlidir.

### <a name="search-services-indexes-fields-and-documents"></a>Hizmetleri, dizinler, alanların ve belgeleri arayın
Azure arama'yı kullanarak, bir abone olduğu bir *arama hizmetinizi*. Azure Search'e veri karşıya yüklenmiş gibi depolanan bir *dizin* arama hizmetinde. Tek bir hizmet içinde dizin sayısını olabilir. Bir veritabanı içinde tablo hizmetinden dizinleri benzetilebilir sırada tanıdık veritabanı kavramlarını kullanmak için arama hizmeti bir veritabanına benzetilebilir.

Her dizini bir arama hizmetindeki özelleştirilebilir bir dizi tanımlanmış kendi şemasını sahip *alanları*. Veri biçiminde bireysel bir Azure Search dizinine eklenir *belgeleri*. Her belge için belirli bir dizinden yüklenmelidir ve bu dizinin şema sığması gerekir. Azure arama'yı kullanarak verileri ararken tam metin arama sorguları karşı belirli bir dizinden verilir.  Bu veritabanı için bu kavramlar karşılaştırmak için bir tablodaki sütun alanları benzetilebilir ve belgeleri satırlara benzetilebilir.

### <a name="scalability"></a>Ölçeklenebilirlik
Herhangi bir Azure Search hizmeti standart [fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/search/) iki boyutta ölçeklendirebilirsiniz: depolama ve kullanılabilirlik.

* *Bölümler* bir arama hizmeti, depolama alanını artırmak için eklenebilir.
* *Çoğaltmaları* bir arama hizmeti işleyebileceği isteklerin verimini artırmak için bir hizmet eklenebilir.

Bölümler ve çoğaltmalar, ekleme ve kaldırma kapasitesini veri miktarı ile büyütün ve uygulama taleplerini trafik için arama hizmetini izin verir. Okuma elde etmek bir arama hizmeti için sırayla [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/), yinelemeler gerektirir. Okuma-yazma elde etmek bir hizmet için sırayla [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/), üç kopyaya gerektirir.

### <a name="service-and-index-limits-in-azure-search"></a>Azure Search hizmeti ve dizin sınırları
Birkaç farklı [fiyatlandırma katmanları](https://azure.microsoft.com/pricing/details/search/) Azure Search'te, her katman farklı sahip [limitler ve kotalar](search-limits-quotas-capacity.md). Bu sınırlar hizmet düzeyinde bazıları dizin düzeyinde bazıları ve bölüm düzeyinde bazılarıdır.

|  | Temel | Standard1 | Standard2 | Standart3 | Standart3 HD |
| --- | --- | --- | --- | --- | --- |
| Hizmet başına en fazla yineleme |3 |12 |12 |12 |12 |
| Hizmet başına en fazla bölüm |1 |12 |12 |12 |3 |
| En fazla arama birimi (çoğaltmalar * bölümler) hizmet başına |3 |36 |36 |36 |36 (en fazla 3 bölümler) |
| Hizmet başına en fazla depolama |2 GB |300 GB |1,2 TB |2,4 TB |600 GB |
| Bölüm başına en fazla depolama |2 GB |25 GB |100 GB |200 GB |200 GB |
| Hizmet başına en fazla dizin |5 |50 |200 |200 |3000 (en fazla 1000 dizin/bölüm) |

#### <a name="s3-high-density"></a>S3 Yüksek yoğunluklu '
Azure Search'ün fiyatlandırma, S3 katmanında özellikle çok kiracılı senaryoları için tasarlanan yüksek yoğunluklu (HD) mod için bir seçenek yoktur. Çoğu durumda, çok sayıda avantaj Basitlik ve maliyet verimliliği elde etmek için tek bir hizmet altındaki küçük kiracılar desteklemek gereklidir.

S3 HD tek Arama Hizmeti Yönetimi altında tek bir hizmette daha fazla dizinleri barındırmak için bölümleri için özelliği'ni kullanarak dizinler ölçeğini genişletme olanağı alım-satım heyecanla birçok küçük dizinleri olanak tanır.

Concretely, S3 hizmeti birlikte kadar 1.4 milyar belgeleri barındırabilir 1 ila 200 dizinler arasında olabilir. S3 HD diğer taraftan tek dizin 1 milyona kadar belgeyi yalnızca Git izin, ancak bölüm başına 200 milyon toplam belge sayısı ile (hizmet başına 3000) en fazla bölüm başına en fazla 1000 dizin işleyebilir (hizmet başına 600 milyona kadar).

## <a name="considerations-for-multitenant-applications"></a>Çok müşterili uygulamalar için dikkat edilmesi gerekenler
Çok kiracılı uygulamalar bazı çeşitli kiracılar arasında gizlilik düzeyini korurken kaynakları kiracılar arasında etkili bir şekilde dağıtmanız gerekir. Bu tür bir uygulama mimarisi tasarlanırken bazı önemli noktalar vardır:

* *Kiracı yalıtımı:* Uygulama geliştiriciler, Kiracı yetkisiz veya istenmeyen diğer kiracıların verileri erişimi olduğundan emin olun için uygun önlemleri almanız gerekir. Veri gizliliği açısından, paylaşılan kaynaklar ve gürültücü Komşuları korumadan etkili Yönetimi Kiracı yalıtımı stratejileri gerektirir.
* *Bulut kaynak maliyeti:* Diğer herhangi bir uygulamada olduğu gibi yazılım çözümlerini maliyet rekabetçi bir çok kiracılı bir uygulama bileşeni olarak kalması gerekir.
* *İşlemleri Kolaylığı:* Çok kiracılı mimari geliştirirken, uygulamanın işlemler ve karmaşıklık üzerindeki etkisini önemli bir noktadır. Azure Search'ü sahip bir [% 99,9 oranında SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
* *Global Ayak izi:* Çok kiracılı uygulamaları etkili bir şekilde dünya çapında dağıtılan kiracılara hizmet gerekebilir.
* *Ölçeklenebilirlik:* Uygulama geliştiricileri nasıl, kiracıların sayısı ve kiracıların verileri ve iş yükü boyutu ile ölçek için uygulama tasarlama ve uygulama karmaşıklığını yeterince düşük düzeyde koruma arasında mutabık göz önünde bulundurmanız gerekir.

Azure arama, kiracıların verileri ve iş yükü ayırmak için kullanılan bazı sınırlar sunar.

## <a name="modeling-multitenancy-with-azure-search"></a>Azure Search ile çok kiracılı mimari modelleme
Çok kiracılı bir senaryo, söz konusu olduğunda uygulama geliştiricisinin bir veya daha fazla arama hizmetlerini kullanır ve kiracıları Hizmetleri, dizinleri veya her ikisi arasında bölün. Azure arama, çok kiracılı bir senaryo modelleme, bazı ortak desenleri sahiptir:

1. *Kiracı başına dizin:* Her kiracının kendi dizin diğer kiracılar ile paylaşılan bir arama hizmeti vardır.
2. *Kiracı başına hizmeti:* Her kiracının kendi adanmış bir Azure Search Hizmeti sunarak yüksek düzeyde veri ve iş yükü ayrımı vardır.
3. *Her ikisinin karışımı:* Paylaşılan Hizmetler içinde tek bir dizin daha küçük kiracılar atanmış durumdayken daha büyük, daha fazla etkin kiracılar adanmış Hizmetleri atanır.

## <a name="1-index-per-tenant"></a>1. Kiracı başına dizin
![Kiracı başına dizin modelinin bir portrayal](./media/search-modeling-multitenant-saas-applications/azure-search-index-per-tenant.png)

Bir kiracı başına dizin modelinde, her kiracının kendi dizin sahip olduğu tek bir Azure Search hizmeti birden çok kiracının kaplar.

Kiracılar, tüm arama istekleri ve belge işlemlerini Azure Search'te bir dizin düzeyinde verilir çünkü veri yalıtımı elde edin. Uygulama Katmanı'de hizmet düzeyinde kaynakları tüm kiracılar genelinde yönetirken uygun dizinleri çeşitli kiracılar trafiği yönlendirmek için gereksinim tanıma yoktur.

Kiracı başına dizin modelinin bir anahtar öznitelik için bir arama hizmeti uygulamanın kiracılar arasında kapasitesini oversubscribe için uygulama geliştiricisinin yeteneğidir. Aynı anda daha az uzun bir kuyruğu verirken son derece aktif, kaynak kullanımı yoğun kiracıların sayısı uyum sağlamak için bir arama hizmetinin dizinleri arasında kiracılar bir dağılımına iş yükü varsa, kiracılar en iyi birleşimi dağıtılabilir Etkin kiracılar. Her Kiracı aynı anda yüksek oranda etkin olduğu durumlarda model yükleyememesine dengedir.

Kiracı başına dizin modeli temel burada tüm Azure Search Hizmeti satın ön değişken maliyet modeli sağlar ve daha sonra kiracılar ile doldurulur. Bu deneme sürümleri ve ücretsiz hesaplar için atanacak kullanılmayan kapasite sağlar.

Bir global Ayak izi ile uygulamalar için Kiracı başına dizin modeli en etkili olmayabilir. Dünya çapında dağıtılan bir uygulamanın kiracıları, ayrı bir hizmet maliyetleri her biri arasında çoğaltabilirsiniz. her bölge için gerekli olabilir.

Azure Search'ü tek dizin ve dizin toplam sayısı için ölçek büyütmenize izin verir. Uygun bir fiyatlandırma katmanı seçilen, tek bir dizin hizmetinde depolama ya da trafiği açısından çok büyük büyürken bölümler ve çoğaltmalar için tüm arama hizmeti eklenebilir.

Dizinleri toplam sayısı, tek bir hizmet için çok fazla büyürse, başka bir hizmet yeni kiracılar sağlanması gerekir. Dizinleri yeni hizmetler eklendikçe arama hizmetleri arasında taşınması gerekiyorsa, dizin verilerini Azure arama için dizin taşınacak izin vermediğinden el ile bir dizini diğerine kopyalanması gerekir.

## <a name="2-service-per-tenant"></a>2. Kiracı başına hizmeti
![Kiracı başına hizmet modelinin bir portrayal](./media/search-modeling-multitenant-saas-applications/azure-search-service-per-tenant.png)

Bir kiracı başına hizmet mimarisinde her kiracının kendi Arama hizmeti vardır.

Bu modelde, uygulama yalıtımı, kiracılar için en üst düzeyini elde eder. Her hizmeti, depolamayı ve aktarım hızını ayrı API anahtarları yanı sıra arama isteği işlemek için ayırmıştır.

Çeşitli kiracılar iş yüklerinde kaynakları paylaşılmayan burada her Kiracı büyük bir boyut ya da iş yükü az değişkenlik Kiracı Kiracı uygulamalar için Kiracı başına hizmet modeli etkili bir seçim aynıdır.

Kiracılı model başına bir hizmet, ayrıca öngörülebilir ve sabit maliyet modeli avantajı sunar. Oluncaya kadar doldurmak için bir kiracı Kiracı başına maliyet, bir kiracı başına dizin modelinden daha yüksektir, ancak tüm arama hizmetinde önceden hiçbir yatırım yoktur.

Kiracı başına hizmet modeli, bir global Ayak izi olan uygulamalar için verimli bir seçimdir. Coğrafi olarak dağıtılmış kiracıyla ilgili bölgede her bir kiracının hizmet sağlamak kolaydır.

Tek tek kiracılar kendi hizmet aşıyorsa, bu düzen ölçeklendirme karşılaşılan durumlardan kaynaklanır. Azure arama, tüm verileri yeni bir hizmet için el ile kopyalanması bu nedenle, bir arama hizmeti, fiyatlandırma katmanını yükseltme şu anda desteklemiyor.

## <a name="3-mixing-both-models"></a>3. Her iki modeli karıştırma
Çok kiracılı mimari modelleme başka bir düzen, Kiracı başına dizini hem Kiracı başına hizmeti stratejileri karıştırma.

Paylaşılan hizmet olarak daha az etkin, daha küçük kiracılar uzun kuyruğunu dizinleri kaplayabilir ancak iki deseni birleştirilmesinden adanmış Hizmetleri bir uygulamanın en büyük kiracılar kaplayabilir. Bu model, en büyük kiracılar herhangi gürültücü Komşuları daha küçük kiracılar korunmasına yardımcı olma sırasında sürekli olarak yüksek performanslı hizmetinden sahip olmasını sağlar.

Ancak, bu strateji uygulama kaynaklansın hangi kiracıya özel bir hizmettir ve paylaşılan bir hizmette bir dizini gerektirecek tahmin etmeye yönelik olarak kullanır. Bu çok kiracılı mimari modeller her ikisi de yönetmek için gereken uygulama karmaşıklığını artırır.

## <a name="achieving-even-finer-granularity"></a>Daha fazla ayrıntı elde edin
Azure Search çok kiracılı senaryolarda modellemek için yukarıdaki tasarım desenleri, her Kiracı uygulamanın tamamını bir örneği olduğu Tekdüzen bir kapsamı varsayar. Ancak, uygulamalar, bazen çok sayıda küçük kapsamı işleyebilir.

Hizmet başına Kiracı ve Kiracı başına dizin modelleri yeterince küçük kapsamları emin değilseniz, bir bile zahmetli ayrıntı düzeyi elde etmek için bir dizin model mümkündür.

Farklı istemci uç noktaları için farklı davranır tek bir dizin için olası her istemci için belirli bir değer atayan bir dizin bir alan eklenebilir. Bir istemci çağrıları sorgu ya da bir dizini değiştirmek için Azure Search her zaman uygun değeri kullanarak Azure Search'ün bu alan için istemci uygulaması kodu belirtir [filtre](https://msdn.microsoft.com/library/azure/dn798921.aspx) sorgu zamanında yeteneği.

Bu yöntem, ayrı kullanıcı hesaplarını, ayrı izin düzeyleri, işlevselliği elde etmek için kullanılabilir ve hatta tamamen uygulamaları ayırmak.

> [!NOTE]
> Yukarıda açıklanan yaklaşımı kullanarak birden çok kiracıya hizmet vermek için tek bir dizinde yapılandırmak için arama sonuçlarının ilgi düzeyi etkiler. Terim sıklığı gibi temel istatistikleri ilgi puanları, tüm kiracıların verileri dahil şekilde arama ilgi puanları bir dizin düzeyinde kapsamda bir kiracı düzeyinde kapsamı hesaplanır.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Azure arama, çoğu uygulama için ilgi çekici bir seçim [hizmetin güçlü özellikleri hakkında daha fazla](https://aka.ms/whatisazsearch). Çok müşterili uygulamalar için çeşitli tasarım desenleri değerlendirilirken göz önünde bulundurun [çeşitli fiyatlandırma katmanları](https://azure.microsoft.com/pricing/details/search/) ve ilgili [hizmet sınırları](search-limits-quotas-capacity.md) en iyi Azure Search, uygulama uyacak şekilde uyarlamak için iş yükleri ve her boyuttaki mimariler.

Azure Search ve çok kiracılı senaryoları hakkında sorularınız için yönlendirilebilir azuresearch_contact@microsoft.com.

