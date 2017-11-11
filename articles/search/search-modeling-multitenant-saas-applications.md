---
title: "Azure Search'te çoklu müşteri Mimarisi Modelleme | Microsoft Docs"
description: "Azure Search kullanırken çok kiracılı SaaS uygulamaları için ortak tasarım desenleri öğrenin."
services: search
manager: jhubbard
author: ashmaka
documentationcenter: 
ms.assetid: 72e9696a-553b-47dc-9e05-a82db0ebf094
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 11/09/2017
ms.author: ashmaka
ms.openlocfilehash: 622ae64e118dd2498aff0bf2e9f6c1dbfb0ab045
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="design-patterns-for-multitenant-saas-applications-and-azure-search"></a>Tasarım desenleri çok kiracılı SaaS uygulamaları ve Azure Search için
Çok kiracılı uygulama kimin göremez ya da başka bir kiracı veri paylaşımı kiracılar herhangi bir sayıda aynı Hizmetleri ve özellikleri sağlar biridir. Bu belge, Azure Search ile oluşturulan çok müşterili uygulamalar için Kiracı yalıtımı stratejileri açıklanır.

## <a name="azure-search-concepts"></a>Azure arama kavramları
Bir hizmet olarak arama çözümü olarak Azure Search zengin arama deneyimleri herhangi bir altyapı yönetme veya bir uzman bilgilerini alma olmadan olmadan uygulamaları eklemek için geliştiricilere sağlar. Veri Hizmeti karşıya ve bulutta depolanan. Azure Search API basit isteklerini kullanarak, veri sonra değiştirilen aranır ve. Hizmeti genel bir bakış bulunabilir [bu makalede](http://aka.ms/whatisazsearch). Tasarım modeli ele almadan önce Azure Search'te bazı kavramları anlamanız önemlidir.

### <a name="search-services-indexes-fields-and-documents"></a>Arama Hizmetleri, dizinleri, alanları ve belgeleri
Azure Search kullanırken, bir abone bir *search hizmeti*. Azure Search'e veri karşıya olarak depolanır bir *dizin* arama hizmeti içinde. Tek bir hizmet kapsamındaki dizin sayısı olabilir. Bir hizmet kapsamındaki dizinleri veritabanındaki tablolar benzetilebilir sırada veritabanları tanıdık kavramlarını kullanmak için arama hizmeti bir veritabanına benzetilebilir.

Her dizininin özelleştirilebilir bir dizi tanımlanmış kendi şeması, bir arama hizmeti içinde *alanları*. Verileri Azure Search dizini tek biçiminde eklenen *belgeleri*. Her belge için belirli bir dizin yüklenmelidir ve bu dizinin şema sığması gerekir. Azure Search kullanarak verileri ararken tam metin arama sorguları karşı belirli bir dizin verilir.  Bu veritabanı için bu kavramlar karşılaştırmak için alanları tablodaki sütunlar benzetilebilir ve belgeleri satırları benzetilebilir.

### <a name="scalability"></a>Ölçeklenebilirlik
Tüm Azure Search hizmeti standart [fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/search/) iki boyut ölçeklendirebilirsiniz: depolama ve kullanılabilirlik.

* *Bölümler* bir arama hizmeti depolama artırmak için eklenebilir.
* *Çoğaltmaları* bir arama hizmeti işleyebilir isteklerin verimini artırmak için bir hizmet eklenebilir.

Ekleme ve kaldırma bölümlerini ve çoğaltmaları veri miktarını büyür ve uygulama talepleri trafiği için arama hizmeti kapasitesini izin verir. Okuma elde etmek bir arama hizmeti için sırayla [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/), iki çoğaltma gerektirir. Okuma-yazma elde etmek bir hizmet için sırayla [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/), üç çoğaltmaları gerektirir.

### <a name="service-and-index-limits-in-azure-search"></a>Azure Search dizini ve hizmet sınırları
Birkaç farklı [fiyatlandırma katmanlarına](https://azure.microsoft.com/pricing/details/search/) Azure Search'te her katmanı farklı sahip [sınırlarını ve kotaları](search-limits-quotas-capacity.md). Bu sınırlar hizmet düzeyinde bazıları, bazı dizin düzeyinde değil ve bölüm düzeyinde bazılarıdır.

|  | Temel | Standard1 | Standard2 | Standard3 | Standard3 HD |
| --- | --- | --- | --- | --- | --- |
| Hizmeti başına en fazla yineleme |3 |12 |12 |12 |12 |
| Hizmet başına en fazla bölüm |1 |12 |12 |12 |3 |
| En fazla arama birimi (çoğaltmaları * bölümler) hizmeti başına |3 |36 |36 |36 |36 (en fazla 3 bölümler) |
| Hizmeti başına en fazla belgeleri |1 milyon |180 milyondan fazla |720 milyon |1.4 milyar |600 milyon |
| Hizmeti başına en fazla depolama |2 GB |300 GB |1,2 TB |2,4 TB |600 GB |
| Bölüm başına en fazla belgeleri |1 milyon |15 milyon |60 milyon |120 milyon |200 milyon |
| Bölüm başına en fazla depolama |2 GB |25 GB |100 GB |200 GB |200 GB |
| Hizmeti başına en fazla dizinler |5 |50 |200 |200 |3000 (en fazla 1000 dizinleri/bölüm) |

#### <a name="s3-high-density"></a>S3 Yüksek yoğunluklu '
Azure Search'ün S3 fiyatlandırma katmanında, özellikle çok müşterili senaryoları için tasarlanan yüksek yoğunluk (HD) modu için bir seçenek yoktur. Çoğu durumda, tek bir hizmeti altında küçük kiracılar Basitlik ve maliyet verimliliği avantajları elde etmek için çok sayıda desteklemek gereklidir.

S3 HD tek Arama Hizmeti Yönetimi altında tek bir hizmeti daha fazla dizinlerde barındırmak için özelliği için bölüm kullanan dizinleri ölçeği genişletme olanağı ticaret tarafından paketlenmiş için çok sayıda küçük dizinlerini sağlar.

Concretely, S3 hizmet kadar 1.4 milyar belgeleri birlikte barındırabilir 1 ve 200 dizinler arasında olabilir. S3 HD diğer yandan yalnızca 1 milyon belgeleri gitmek tek tek dizinler izin verir, ancak ile 200 milyon bölüm başına toplam belge sayısı (en fazla 3000 hizmeti başına) bölüm başına en fazla 1000 dizinleri işleyebilir (en çok hizmeti başına 600 milyon).

## <a name="considerations-for-multitenant-applications"></a>Çok müşterili uygulamalar için ilgili önemli noktalar
Çok müşterili uygulamalar gizlilik çeşitli kiracılar arasında belirli bir düzeyde koruyarak kaynakları kiracılar arasında etkili bir şekilde dağıtmanız gerekir. Bu tür bir uygulama mimarisi tasarlarken bazı noktalar vardır:

* *Kiracı yalıtımı:* uygulama geliştiricileri hiçbir Kiracı'nin yetkisiz veya istenmeyen diğer kiracıların verilere erişim sağlamak için uygun önlemleri gerekir. Veri gizliliği açısından, paylaşılan kaynakların ve gürültülü komşu korumadan etkin Yönetimi Kiracı yalıtımı stratejileri gerektirir.
* *Bulut kaynak maliyeti:* başka herhangi bir uygulama ile yazılım çözümleri maliyet rekabetçi bir çok kiracılı uygulama bileşeni olarak kalması gereken gibi.
* *Operations Kolaylığı:* çok müşterili mimarisi geliştirirken, uygulamanın işlemler ve karmaşıklık üzerindeki etkisini önemli bir konudur. Azure arama sahip bir [% 99,9 SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
* *Genel ayak izini:* çok müşterili uygulamalar etkili bir şekilde dünya çapında dağıtılan kiracılar hizmet gerekebilir.
* *Ölçeklenebilirlik:* uygulama geliştiricileri nasıl bunlar uygulama karmaşıklığını yeterince düşük düzeyde koruma ve kiracıların sayısı ve kiracılarınızın verileri ve iş yükü boyutu ile ölçeklendirmek için uygulama tasarlama arasındaki mutabakat göz önünde bulundurun gerekir.

Azure arama kiracılarınızın veri ve iş yükü ayırmak için kullanılan birkaç sınırlarını sunar.

## <a name="modeling-multitenancy-with-azure-search"></a>Azure Search ile çoklu müşteri Mimarisi Modelleme
Çok kiracılı bir senaryo, söz konusu olduğunda uygulama geliştiricisi bir veya daha fazla arama hizmetleri kullanır ve Hizmetleri, dizinler veya her ikisi arasında kiracıları bölün. Çok kiracılı bir senaryo modelleme varken, Azure arama birkaç ortak desenler kullanır:

1. *Kiracı başına dizin:* her bir kiracı kendi dizini içindeki diğer kiracılar ile paylaşılan bir arama hizmeti yok.
2. *Kiracı başına hizmet:* en yüksek düzeyde veri ve iş yükü ayrımı sunumu her bir kiracı kendi adanmış bir Azure Search Hizmeti sahip.
3. *Her ikisi de karışımını:* küçük kiracılar paylaşılan hizmetler içinde tek tek dizinler atanmış durumdayken daha büyük ve daha fazlasını etkin kiracılar ayrılmış Hizmetleri atanır.

## <a name="1-index-per-tenant"></a>1. Kiracı başına dizin
![Kiracı başına dizin modelinin portrayal](./media/search-modeling-multitenant-saas-applications/azure-search-index-per-tenant.png)

Bir kiracı başına dizin modelinde, her bir kiracı kendi dizin sahip olduğu birden çok kiracıya tek bir Azure Search Hizmeti kaplar.

Kiracılar, tüm istekleri arayın ve Azure Search'te dizin düzeyinde bir belge işlemleri verilen olduğundan veri yalıtımı elde edin. Uygulama katmanında hizmet düzeyinde kaynakları da tüm kiracılar arasında yönetirken uygun dizinleri çeşitli kiracılar trafiği yönlendirmek için gerek tanıma yoktur.

Kiracı başına dizin modelinin anahtar özniteliği için bir arama hizmeti uygulamanın kiracılar arasında kapasitesini oversubscribe için uygulama geliştiricisinin özelliğidir. Kiracılar iş yükü düzensiz bir dağılımına varsa, kiracılar en iyi birleşimi yüksek oranda etkin, yoğun bir kaynak Kiracı sayısı aynı anda daha az aktif kiracıların uzun bir kuyruk hizmet verirken uyum sağlamak için bir arama hizmetin dizinleri üzerinden dağıtılabilir. Dengelemeyi her Kiracı aynı anda yüksek oranda etkin olduğu durumlarda işlemek için model bağlanamaması ' dir.

Kiracı başına dizin model burada tüm Azure Search Hizmeti satın eylemli değişken maliyet modeli temelini ve daha sonra kiracılar ile doldurulur. Bu deneme ve ücretsiz hesaplar için atanacak kullanılmayan kapasiteyi sağlar.

Genel bir yer olan uygulamalar için Kiracı başına dizin modeli en etkili olmayabilir. Bir uygulamanın kiracılar dünya çapında dağıtılmışsa, ayrı bir hizmet maliyetleri her biri arasında çoğaltabilirsiniz her bölge için gerekli olabilir.

Azure arama tek tek dizinler ve dizinleri toplam sayısı için ölçek büyümesine olanak sağlar. Uygun fiyatlandırma, katmanı seçildiğinde, bölümler ve çoğaltmalar tek bir dizin hizmeti içinde depolama veya trafiği açısından aştığında tüm arama hizmetine eklenebilir.

Dizinleri toplam sayısı tek bir hizmet için çok fazla büyürse, yeni kiracılar sağlanması başka bir hizmete sahip. Dizinleri yeni hizmetler eklendikçe arama hizmetleri arasında taşınacak varsa, dizin verilerini Azure Search taşınması için bir dizin için izin vermediğinden el ile bir dizinden diğerine kopyalanması gerekir.

## <a name="2-service-per-tenant"></a>2. Kiracı başına hizmeti
![Kiracı başına hizmet modeli portrayal](./media/search-modeling-multitenant-saas-applications/azure-search-service-per-tenant.png)

Kiracı başına hizmet mimarisinde, her bir kiracı kendi Arama hizmeti vardır.

Bu modelde, uygulama, kiracılar için yalıtım düzeyi üst sınırına erişir. Her hizmet, depolama ve işleme ayrı API anahtarları yanı sıra arama isteği işlemek için ayrılmış.

Kaynakları çeşitli kiracılar iş yüklerinde paylaşılmayan gibi her bir kiracı büyük ayak izini sahip veya iş yükü çok az değişkenlik Kiracı Kiracı yüklü olduğu uygulamalar için Kiracı başına hizmet etkili bir seçim modelidir.

Kiracı modeli başına bir hizmet ayrıca bir tahmin edilebilir, sabit maliyet modelin avantajı sunar. Oluncaya kadar doldurmak için bir kiracı ancak Kiracı başına maliyet bir kiracı başına dizin modelinden daha yüksek olduğu tüm arama hizmeti yok önceden yatırım yoktur.

Kiracı başına hizmet modeli, genel bir yer olan uygulamalar için verimli bir seçimdir. Coğrafi olarak dağılmış kiracılarla her bir kiracının hizmet uygun bölgede sahip kolaydır.

Tek tek kiracılar kendi hizmet outgrow olduğunda bu deseni ölçeklendirme içinde zorluklar ortaya çıkar. Azure arama, böylece tüm verileri için yeni bir hizmet el ile kopyalanması zorunda bir arama hizmeti fiyatlandırma katmanı yükseltme şu anda desteklemiyor.

## <a name="3-mixing-both-models"></a>3. Her iki modeli karıştırma
Çoklu müşteri Mimarisi Modelleme için başka bir desen dizin başına Kiracı ve Kiracı başına hizmeti stratejileri karıştırma.

Daha az etkin, daha küçük kiracılar uzun kuyruğu paylaşılan hizmetinde dizinleri kaplar sırada iki desenleri birleştirilmesinden ayrılmış Hizmetleri bir uygulamanın en büyük kiracılar kaplar. Bu model, en büyük kiracılar hizmetinden tutarlı bir şekilde yüksek performanslı herhangi gürültülü komşu daha küçük kiracılar korumak için sağlamanın sahip olmanızı sağlar.

Ancak, bu stratejinin uygulanması hangi kiracılar paylaşılan bir hizmet bir dizinde karşı adanmış bir hizmet gerektirecektir tahmin etmeye içinde öngörü kullanır. Bu çoklu müşteri mimarisi modeller her ikisi de yönetmek için gereken uygulama karmaşıklığını artırır.

## <a name="achieving-even-finer-granularity"></a>Sağlanması bile daha hassas ayrıntı düzeyi
Azure Search çok müşterili senaryolarda model oluşturmak için yukarıdaki tasarım desenleri her Kiracı uygulamasının tüm örneğini olduğu Tekdüzen bir kapsam varsayalım. Ancak, uygulamaları bazen çok sayıda küçük kapsamı işleyebilir.

Hizmet başına Kiracı ve Kiracı başına dizin modelleri yeterince küçük kapsamları emin değilseniz, hatta daha hassas bir ayrıntı düzeyi derecesini elde etmek için bir dizin model mümkündür.

Farklı istemci uç noktaları için farklı davranır tek bir dizin için bir alan için olası her istemci belirli bir değer atayan bir dizine eklenebilir. Bir istemci çağırır sorgu veya bir dizini değiştirmek için Azure Search her zaman istemci uygulaması kodu kullanarak Azure Search'ün bu alan için uygun değer belirtir. [filtre](https://msdn.microsoft.com/library/azure/dn798921.aspx) sorgu zaman yeteneği.

Bu yöntem ayrı kullanıcı hesaplarını, ayrı izin düzeyleri işlevselliğini elde etmek için kullanılabilir ve hatta tamamen uygulamaları ayırın.

> [!NOTE]
> Yukarıda açıklanan yaklaşım kullanarak birden çok kiracıya hizmet vermek için tek bir dizin yapılandırmak için arama sonuçlarını alaka etkiler. Tüm kiracılar veri terim sıklığı gibi temel istatistikleri ilgi puanları dahil edilmiş şekilde arama ilgi puanları Kiracı düzeyi kapsamı olmayan bir dizin düzeyinde kapsamda hesaplanır.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Azure arama seçimdir bir ilgi çekici pek çok uygulama [hizmetin sağlam özellikleri hakkında daha fazla](http://aka.ms/whatisazsearch). Çok müşterili uygulamalar için çeşitli tasarım desenleri, değerlendirirken [çeşitli fiyatlandırma katmanlarına](https://azure.microsoft.com/pricing/details/search/) ve ilgili [hizmet sınırları](search-limits-quotas-capacity.md) en iyi uygulama iş yükleri ve mimarileri ölçekteki uyacak şekilde Azure Search uyarlamak için.

Azure Search ve çok müşterili senaryoları hakkında herhangi bir sorunuz için yönlendirilebilir azuresearch_contact@microsoft.com.

