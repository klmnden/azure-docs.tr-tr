---
title: "Bir SKU seçin veya Azure arama için fiyatlandırma katmanı | Microsoft Docs"
description: "Azure arama sırasında bu SKU'ları sağlanabilir: ücretsiz, temel ve standart, standart olduğu çeşitli kaynak yapılandırmaları ve kapasite düzeyleri kullanılabilir."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 8d4b7bca-02a5-43ee-b3f8-03551dfb32fd
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/24/2016
ms.author: heidist
ms.openlocfilehash: f9f3a7b2369818791ffac1c8eeccef45216c2ff0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="choose-a-sku-or-pricing-tier-for-azure-search"></a>Azure Search için SKU veya fiyatlandırma katmanı seçme
Azure Search'te bir [hizmet sağlandığına](search-create-service-portal.md) belirli fiyatlandırma katmanı veya SKU. Seçenekleriniz **serbest**, **temel**, veya **standart**, burada **standart** birden çok yapılandırmaları ve kapasiteleri kullanılabilir.

Bu makalenin amacı, bir katman seçmenize yardımcı olmaktır. Çok düşük olacak şekilde bir katmanın kapasitesini öyle, daha yüksek katman en yeni bir hizmet sağlamak ve dizinleri yeniden yüklemeniz gerekir. Herhangi bir SKU aynı hizmetinden başka bir yerinde yükseltmesini yoktur.

> [!NOTE]
> Bir katman seçtikten sonra ve [bir arama hizmeti sağlamak](search-create-service-portal.md), çoğaltma artırabilir ve bölüm hizmet içinde sayar. Yönergeler için bkz [ölçeklendirme sorgu ve dizin oluşturma iş yükleri için kaynak düzeylerini](search-capacity-planning.md).
>
>

## <a name="how-to-approach-a-pricing-tier-decision"></a>Yaklaşım bir fiyatlandırma katmanı karar nasıl
Azure Search'te katman kapasitesi, Özellik kullanılabilirliği belirler. Genellikle, özellikleri her katmanını Önizleme özellikleri dahil olmak üzere, kullanılabilir. S3 HD dizin oluşturucular desteği tek istisnası.

> [!TIP]
> Her zaman sağlamanızı öneririz bir **serbest** hizmet (abonelik, süre sonu olmayan her bir) böylece kendi kullanıma hazır hafif projeleri için. Kullanım **serbest** ; ikinci bir Faturalanabilir hizmeti oluşturma hizmeti sınama ve değerlendirme için **temel** veya **standart** katmanı üretim ya da daha büyük test iş yükleri için.
>
>

Kapasite ve hizmet çalıştırmanın maliyetlerini el eldeki gidin. Bu makaledeki bilgileri, doğru dengeyi SKU sunar karar vermenize yardımcı olabilir, ancak herhangi bunu kullanışlı olması için en az kaba tahminleri şu gerekir:

* Sayısını ve boyutunu dizinler oluşturmak için planlama
* Sayısını ve boyutunu belgeleri karşıya yüklemek için
* Bazı fikir sorgu biriminin bakımından sorguları başına ikinci (QPS)

Bir sabit sınır sayısı dizinler veya bir hizmet belgelerde veya hizmeti tarafından kullanılan kaynakları (depolama veya çoğaltmaları) aracılığıyla maksimum sınırlarına ulaştığından sayısını ve boyutunu önemlidir. Hizmetiniz için gerçek sınırı hangisi ilk kullanılır: kaynakları veya nesneler.

Tahminler el de, aşağıdaki adımları işlemini basitleştirmek:

* **1. adım** kullanılabilir seçenekler hakkında bilgi edinmek için aşağıdaki SKU açıklamaları gözden geçirin.
* **2. adım** bir ön kararını ulaşması için aşağıdaki soruları yanıtlayın.
* **3. adım** kararınız depolama sabit sınırlara gözden geçirme ve fiyatlandırma sonlandır.

## <a name="sku-descriptions"></a>SKU açıklamaları
Aşağıdaki tabloda, her katman açıklamalarını sağlar.

| Katman | Birincil senaryolar |
| --- | --- |
| **Boş** |Değerlendirme, araştırma ya da küçük iş yükleri için kullanılan ücret ödemeden, paylaşılan bir hizmet. Diğer aboneleriyle paylaşıldığından, sorgu işleme ve dizin oluşturma başka Kime ve hizmetinin kullandığı göre değişir. Kapasite Küçük (50 MB 10.000 ile 3 dizinlerini belgeleri veya her). |
| **Temel** |Ayrılmış donanım üzerinde küçük üretim iş yükleri. Yüksek oranda kullanılabilir. Kapasite en fazla 3 çoğaltmaları ve 1 bölüm (2 GB) toplamıdır. |
| **S1** |Standart 1 (12) bölümler ve çoğaltmalar (12), ayrılmış donanım üzerinde Orta üretim iş yükleri için kullanılan esnek birleşimlerini destekler. Bölümleri ve en çok 36 Faturalanabilir arama birimi sayısı tarafından desteklenen birleşimleri yinelemede ayırabilirsiniz. Bu düzeyde bölümleri 25 GB ve QPS saniyede yaklaşık 15 sorgular. |
| **S2** |Standart 2 büyük üretim iş yükleri aynı 36 arama birimi S1 ancak büyük boyutlu bölümler ve çoğaltmalar kullanarak çalıştırır. Bu düzeyde bölümleri 100 GB ve QPS saniyede yaklaşık 60 sorgular. |
| **S3** |Standart 3 orantılı olarak büyük üretim iş yükleri daha yüksek son sistemlerinde 12 bölümleri veya 12 çoğaltmaları kadar yapılandırmalarını altında 36 arama birimi çalıştırır. Bu düzeyde bölümleri 200 GB ve QPS saniye başına birden fazla 60 sorgular. |
| **S3 HD** |Standart 3 yüksek yoğunluklu, çok sayıda küçük dizinler için tasarlanmıştır. 200 GB olarak her 3 adede kadar bölümlere sahip olabilir. QPS, saniye başına birden fazla 60 sorgudur. |

> [!NOTE]
> Çoğaltma ve bölüm üst sınırlar, çıkışı, ne maksimum nominal değerde gelir daha etkili bir alt limit uygular (36 birim başına en fazla), arama birimi olarak faturalandırılır. Örneğin, en fazla 12 çoğaltmaları kullanmak için en fazla 3 bölümleri olabilir (12 * 3 = 36 birimleri). Benzer şekilde, en fazla bölüm kullanmak için 3 çoğaltmaları azaltın. Bkz: [ölçeklendirme sorgu ve iş yüklerini Azure Search'te dizin oluşturma için kaynak düzeylerini](search-capacity-planning.md) izin verilen birleşimleri grafikte için.
>
>

## <a name="review-limits-per-tier"></a>Katman başına sınırları gözden geçirin
Aşağıdaki grafikte sınırlamaları uygulanmaya alt kümesidir [Azure Search hizmet sınırları](search-limits-quotas-capacity.md). SKU karar etkisi en olası neden olan faktörleri listeler. Aşağıdaki sorular gözden geçirirken Bu grafiğe başvurabilir.

| Kaynak | Ücretsiz | Temel | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Hizmet Düzeyi Sözleşmesi (SLA) |Hayır <sup>1</sup> |Evet |Evet |Evet |Evet |Evet |
| Dizin sınırları |3 |5 |50 |200 |200 |1000 <sup>2</sup> |
| Belge sınırları |toplam 10.000 |hizmeti başına 1 milyon |Bölüm başına 15 milyon |Bölüm başına 60 milyon |Bölüm başına 120 milyon |Dizin başına 1 milyon |
| En fazla bölümleri |Yok |1 |12 |12 |12 |3 <sup>2</sup> |
| Bölüm boyutu |50 MB Toplam |Hizmeti başına 2 GB |Bölüm başına 25 GB |(Hizmeti başına en fazla 1.2 TB) kadar bölüm başına 100 GB |(Hizmeti başına en fazla 2.4 TB) kadar bölüm başına 200 GB |200 GB (kadar hizmeti başına en fazla 600 GB) |
| En fazla yineleme |Yok |3 |12 |12 |12 |12 |
| Saniye başına sorguları |Yok |Çoğaltma başına yaklaşık 3 |Çoğaltma başına yaklaşık 15 |Çoğaltma başına yaklaşık 60 |Çoğaltma başına yaklaşık >60 |Çoğaltma başına yaklaşık >60 |

<sup>1</sup> ücretsiz katmanı ve önizleme özellikleri hizmet düzeyi sözleşmelerine (SLA) ile gelen değil. Hizmetiniz için yeterli artıklık sağladığınızda tüm Faturalanabilir katmanları için SLA etkili olur. İki veya daha fazla çoğaltmaları için (okuma) sorgu SLA gereklidir. Üç veya daha fazla çoğaltmalar, sorgu ve dizin oluşturma (okuma-yazma) SLA için gereklidir. Bölüm sayısı bir SLA önem verilmez. 

<sup>2</sup> S3 ve S3 HD aynı yüksek kapasiteli altyapısı tarafından yedeklenmiş ancak her biri farklı şekillerde maksimum sınırına ulaştığında. S3 çok büyük dizinler daha az sayıda hedefler. Bu nedenle, kaynak bağlı kendi üst sınır: (her hizmet için 2.4 TB). S3 HD çok sayıda çok küçük dizinleri hedefler. 1.000 dizinleri S3 HD sınırlarına dizin kısıtlamalarını biçiminde ulaşır. 1. 000'den fazla dizinler gerektiren bir S3 HD müşteriyi olup olmadığını nasıl ilerleyeceğiniz hakkında bilgi için Microsoft Support başvurun.

## <a name="eliminate-skus-that-dont-meet-requirements"></a>Gereksinimlerine uymayan SKU'ları ortadan kaldırma
Aşağıdaki sorular, iş yükü doğru SKU seçim adresindeki gelmesini yardımcı olabilir.

1. Elinizde **hizmet düzeyi sözleşmesi (SLA)** gereksinimleri? Herhangi bir Faturalanabilir katmanı kullanabilirsiniz (temel üzerinde yukarı), ancak hizmetiniz artıklık için yapılandırmanız gerekir. İki veya daha fazla çoğaltmaları için (okuma) sorgu SLA gereklidir. Üç veya daha fazla çoğaltmalar, sorgu ve dizin oluşturma (okuma-yazma) SLA için gereklidir. Bölüm sayısı bir SLA önem verilmez.
2. **Kaç tane dizinler** gerektiriyor mu? Bir SKU karar Finansman büyük değişkenlerinden biri her SKU tarafından desteklenen dizinleri sayısıdır. Dizin alt fiyatlandırma katmanlarına belirtmeyecektir farklı düzeylerde desteğidir. Dizin sayısı gereksinimlerine SKU karar birincil bir etken olabilir.
3. **Kaç tane belgeleri** her dizine yüklenir? Sayısını ve boyutunu belgelerin dizinini nihai boyutunu belirler. Dizin tahmini boyutunu tahmin edebilirsiniz varsayıldığında, bu boyut dizini depolamak için gereken bölümleri sayısına göre genişletilmiş SKU başına bölüm boyutu karşı numarayı karşılaştırabilirsiniz.
4. **Beklenen sorgu yükünü nedir**? Depolama gereksinimleri anladım sonra sorgu iş yükleri göz önünde bulundurun. S2 ve her iki S3 SKU'ları eşdeğer yakın verimlilik sağlar, ancak SLA gereksinimleri herhangi Önizleme SKU'ları kural.
5. S2 veya S3 katmanı düşünüyorsanız, gerekip gerekmediğini belirleme [dizin oluşturucular](search-indexer-overview.md). Dizin oluşturucular henüz S3 HD katmanı için kullanılabilir değildir. Alternatif bir yaklaşım bir gönderme modeli dizin güncelleştirmeler için bir veri kümesi için bir dizin göndermek için uygulama kodu yazma burada kullanmaktır.

Müşterilerin çoğu, yukarıdaki soruların yanıtlarını göre veya belirli bir SKU çıkarabilirsiniz. Hala ile gitmek için hangi SKU emin değilseniz, MSDN veya StackOverflow forumları sorularınızı ya da daha ayrıntılı yönergeler için Azure desteğine başvurun.

## <a name="decision-validation-does-the-sku-offer-sufficient-storage-and-qps"></a>Karar doğrulama: SKU yeterli depolama alanı ve QPS sunar?
Son adım olarak, yeniden ziyaret [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/search/) ve [hizmet sınırları hizmeti başına ve dizin başına bölümlerde](search-limits-quotas-capacity.md) , tahminleri aboneliği ve hizmet sınırları karşı bir kez daha denetleyin.

Fiyat veya depolama gereksinimleri sınırların dışında olması durumunda iş yüklerini birden çok daha küçük hizmetleri arasında (örneğin) yeniden düzenlemeniz isteyebilirsiniz. Daha ayrıntılı düzeyi, dizinleri daha küçük olacak şekilde yeniden tasarlamanız veya sorguları daha verimli hale getirmek için filtreleri kullanın.

> [!NOTE]
> Belgeleri yabancı veri içeriyorsa depolama gereksinimleri aşırı inflated olabilir. İdeal olarak, belgeler, yalnızca aranabilir verileri veya meta veriler içerir. İkili veriler yapılamayan ve ayrı olarak (belki de bir Azure tablo veya blob depolama alanında) dış veriler için bir URL başvuru tutmak için dizinde bir alanla depolanması gerekir. Tek bir belgenin en büyük boyutu 16 MB (veya daha az ise, toplu bir istek birden çok belgeyi karşıya yükleme). Bkz: [hizmet sınırları Azure Search'te](search-limits-quotas-capacity.md) daha fazla bilgi için.
>
>

## <a name="next-step"></a>Sonraki adım
SKU sağ uygun olan öğrendikten sonra bu adımlara devam:

* [Portalda bir arama hizmeti oluşturma](search-create-service-portal.md)
* [Bölümler ve çoğaltmalar hizmetinizi ölçeklendirmek için değiştirme](search-capacity-planning.md)
