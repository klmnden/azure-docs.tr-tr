---
title: Dizin büyük veri kümesi yerleşik dizin oluşturucular - Azure Search kullanma
description: Büyük veri dizini oluşturma veya işlem bakımından yoğun toplu iş modunda dizin oluşturma, kaynak ve zamanlanmış, paralel ve dağıtılmış dizin oluşturma teknikleri stratejileri öğrenin.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.topic: conceptual
ms.date: 12/19/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 8923c94409dcf079179ed0464046e39ef7654c4c
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65949834"
---
# <a name="how-to-index-large-data-sets-in-azure-search"></a>Büyük veri kümeleri Azure Search dizinleme

Veri birimleri büyütmek ya da işleme gereksinimlerine gibi varsayılan dizin stratejileri artık pratik bulabilirsiniz. Azure arama için zamanlanmış ve dağıtılmış iş yükleri için özel kaynak dizin oluşturucu kullanarak verileri karşıya yükleme isteği, nasıl yapı gelen arasında değişen büyük veri kümelerini destekleme için birçok yaklaşım vardır.

Büyük veriler için aynı teknikleri, uzun süre çalışan işlemler için de geçerlidir. Adımlar özellikle özetlenen [paralel dizin](#parallel-indexing) işlem bakımından yoğun dizin oluşturma, görüntü analizi veya doğal dil işleme gibi yararlıdır [bilişsel arama işlem hatları](cognitive-search-concept-intro.md).

## <a name="batch-indexing"></a>Toplu dizin oluşturma

Birden çok belge veya kayıtları tek bir istek göndermek için daha büyük bir veri kümesi dizinini oluşturmak için basit mekanizmaları birisidir. Tüm yükü 16 MB altında olduğu sürece, bir toplu karşıya yükleme işleminde en fazla 1000 belge bir isteği işleyebilir. Varsayılarak [ekleme veya güncelleştirme belgeleri REST API](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents), 1000 istek gövdesi belgelerde paketlemeniz gerekir.

Toplu dizin oluşturma, REST veya .NET kullanarak tek tek istekler için ya da dizin oluşturucular aracılığıyla uygulanır. Birkaç dizin oluşturucular farklı sınırlar altında çalışır. Özellikle, Azure Blob dizin toplu iş boyutu 10 belgeleri ortalama belge büyük dolayı ayarlar. Temel dizin oluşturucular için [dizin oluşturucu REST API oluşturma](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer ), ayarlayabileceğiniz `BatchSize` verilerinizi özelliklerini daha iyi eşleşmesi için bu ayarı özelleştirmek için bağımsız değişken. 

> [!NOTE]
> Belge boyutunu tutmak, istekten sorgulanamayan verileri dışlamak unutmayın. Resimler ve diğer ikili veriler doğrudan aranabilir değil ve dizinde depolanan olmamalıdır. Arama sonuçlarında sorgulanamayan veri tümleştirmek için bir URL kaynağına başvuru depolar aranabilir olmayan bir alanı tanımlamanız gerekir.

## <a name="add-resources"></a>Kaynak Ekle

Biri sağlanan hizmetleri [standart fiyatlandırma katmanları](search-sku-tier.md) getiren kapasite hem depolama hem de iş yüklerini (sorguları veya dizin), çoğu zaman yeterince [bölüm ve çoğaltma sayısını artırma ](search-capacity-planning.md) destekleme daha büyük veri kümeleri için belirgin bir çözüm. En iyi sonuçlar için her iki kaynakları gerekir: depolama ve veri alımı iş çoğaltmaları bölümler.

Artan çoğaltmalar ve bölümler maliyetlerinizi artırabilir, Faturalanabilir olayların içindir, ancak sürekli olarak en yüksek yük altında dizin sürece, dizin oluşturma işlemi boyunca ölçek eklemek ve dizin oluşturma konusunda olduktan sonra kaynak düzeylerini aşağı ayarlayın bitti.

## <a name="use-indexers"></a>Dizin oluşturucular kullanma

[Dizin oluşturucular](search-indexer-overview.md) aranabilir içeriği için dış veri kaynaklarına gezinme için kullanılır. Büyük ölçekli dizin oluşturma işlemi için özel olarak tasarlanmış olmakla birlikte, çeşitli dizin oluşturucu özelliklerini daha büyük veri kümelerini destekleme için yararlıdır:

+ Zamanlayıcıları, böylece size, zaman içinde dağılmış, düzenli aralıklarla dizin kullanıma paket olanak tanır.
+ Zamanlanmış dizin son bilinen durdurma noktadan sürdürebilirsiniz. Bir veri kaynağı bir 24 saatlik pencerede tamamen gezinilemiyor, dizin oluşturucu günde iki yerde kaldığı, dizin oluşturma devam edecek.
+ Daha küçük bağımsız veri kaynaklarında veri bölümleme paralel işlemeye olanak tanır. Büyük bir veri kümesini daha küçük veri kümeleri halinde bölün ve ardından sıralanabilir birden çok veri kaynağı tanımlarının paralel olarak oluşturun.

> [!NOTE]
> Bir dizin oluşturucu yaklaşımı kullanarak yalnızca azure'da seçili veri kaynakları için uygun olacak şekilde dizin oluşturucular veri kaynağı-özgü şunlardır: [SQL veritabanı](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), [Blob Depolama](search-howto-indexing-azure-blob-storage.md), [tablo depolama](search-howto-indexing-azure-tables.md), [Cosmos DB](search-howto-index-cosmosdb.md).

## <a name="scheduled-indexing"></a>Dizin oluşturma zamanlandı

Dizin Oluşturucu ile zamanlama gibi bilişsel arama ardışık düzeninde görüntü analizi yavaş çalışan işlemlerin yanı sıra büyük veri kümeleri, işlem için önemli bir mekanizmadır. Dizin Oluşturucu işleme bir 24 saatlik penceresi içinde çalışır. İşleme 24 saat içinde tamamlamak için başarısız olursa, Dizin Oluşturucu ile zamanlama davranışları kendi yararınıza çalışabilir. 

Tasarım gereği, genellikle bir sonraki zamanlanmış aralıkta devam etmeden önce tamamlanmasını bir işlemle dizin oluşturma başlatıldığında belirli aralıklarla zamanlanmış. Ancak, işleme aralığı içinde tamamlanmazsa, dizin oluşturucu (süre bitti çalıştığından) durdurur. Sonraki aralıkta nerede oluştuğunu, son kapalı, sistem tutma ile kaldığı işleme sürdürür izleyin. 

Dizin yüklemelerini birkaç gün genişleme için pratik anlamda, 24 saatlik zaman çizelgesinde dizin oluşturucu koyabilirsiniz. Sürdürür sonraki 24 saat döngüsü için dizin oluştururken en son bilinen iyi belgeyi yeniden başlatır. Bu şekilde, bir dizin oluşturucu, bir dizi tüm işlenmemiş belgeleri işlenene kadar bir gün üzerinden yolu bir belge biriktirme listesi ile çalışabilir. Bu yaklaşımı hakkında daha fazla bilgi için bkz: [Azure Blob depolamada büyük veri kümelerini dizin](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets). Genel zamanlamaları ayarlama hakkında daha fazla bilgi için bkz. [dizin oluşturucu REST API oluşturma](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer#request-syntax).

<a name="parallel-indexing"></a>

## <a name="parallel-indexing"></a>Paralel dizin oluşturma

Dizin oluşturma stratejisi paralel, burada her veri kaynağı tanımını verilerin bir alt kümesini belirtir birden çok veri kaynağına uyum içinde yönetebileceğiniz, dizin üzerinde temel alır. 

Yordamı olmayan, işlem bakımından yoğun dizin gereksinimleri için - bilişsel arama işlem hattı, görüntü analizi veya doğal dil işleme - taranan belgeleri üzerinde OCR gibi dizin oluşturma stratejisi paralel genellikle tamamlamaya doğru yaklaşımdır bir uzun süre çalışan işlem en kısa sürede. Kaldırın veya sorgu isteği azaltmak, paralel sorguları aynı anda işleme olmayan bir hizmette dizin oluşturma, en iyi stratejisi yavaş işleme içeriğin büyük bir gövde ile çalışmak için seçenektir. 

Paralel işleme Bu öğe vardır:

+ Birden çok kapsayıcı veya aynı kapsayıcı içinde birden çok sanal klasörler arasında kaynak verileri alt bölümlere ayırır. 
+ Mini her veri kümesi kendi eşleme [veri kaynağı](https://docs.microsoft.com/rest/api/searchservice/create-data-source), kendi için eşleştirilmiş [dizin oluşturucu](https://docs.microsoft.com/rest/api/searchservice/create-indexer).
+ Bilişsel arama için aynı başvuru [beceri kümesi](https://docs.microsoft.com/rest/api/searchservice/create-skillset) her dizin oluşturucu tanımı.
+ Aynı hedef arama dizinine yazma. 
+ Tüm Dizin oluşturucuların aynı anda çalışacak şekilde zamanlayın.

> [!NOTE]
> Azure arama, ayrılmış çoğaltmalar veya bölümler belirli iş yükleri için desteklemez. Ağır eşzamanlı dizin oluşturma riskini zarar sisteminize sorgu performansını overburdening olduğu. Bir test ortamınız varsa, paralel var. ilk ödün anlamak için dizin oluşturma uygulayın.

### <a name="how-to-configure-parallel-indexing"></a>Paralel dizin oluşturma yapılandırma

Dizin oluşturucular için işleme kapasitesini gevşek bir dizin oluşturucu alt sisteminde arama hizmetiniz tarafından kullanılan her bir hizmet birimi (SU) için temel alır. Birden çok eşzamanlı dizin oluşturucular en az iki çoğaltmaları olan temel veya standart katmanlarda sağlanan Azure Search Hizmetleri mümkündür. 

1. İçinde [Azure portalında](https://portal.azure.com), search hizmeti Panonuzda **genel bakış** sayfasında **fiyatlandırma katmanı** onaylamak uyum paralel dizin oluşturma için. Temel ve standart katmanları birden çok çoğaltma sunar.

2. İçinde **ayarları** > **ölçek**, [çoğaltmaları artırmak](search-capacity-planning.md) paralel işleme: her dizin oluşturucu iş yükü için ek bir çoğaltma. Varolan bir sorgu birimi için yeterli sayıda bırakın. Dizin oluşturma için sorgu iş yükleri ödün iyi bir denge değil.

3. Azure Search dizin oluşturucularında ulaşabileceği bir düzeyinde birden çok kapsayıcılarına veri dağıtın. Bu, birden çok tablodan Azure SQL veritabanı, Azure Blob Depolama alanında birden çok kapsayıcı veya birden çok koleksiyon olabilir. Her bir tablo ya da kapsayıcı için bir veri kaynağı nesnesi tanımlayın.

4. Oluşturma ve paralel olarak çalıştırmak için birden çok dizin oluşturucu zamanlayabilirsiniz:

   + Altı hizmetiyle varsayılır. Altı dizin oluşturucular, altıda tüm veri kümesinin 6 yönlü bölme için veri kümesi içeren bir veri kaynağı için eşlenen her birini yapılandırın. 

   + Her dizin oluşturucu, aynı dizine işaret edin. Bilişsel arama iş yükleri için her dizin oluşturucu için aynı becerilerine gelin.

   + Her dizin oluşturucu tanımı içinde aynı çalışma zamanı yürütme düzeni zamanlayın. Örneğin, `"schedule" : { "interval" : "PT8H", "startTime" : "2018-05-15T00:00:00Z" }` 2018-05-15 üzerinde tüm dizin oluşturucular, sekiz saatlik aralıklarla çalışan bir zamanlama oluşturur.

Planlanan zamanda tüm dizin oluşturucuların yürütme, veri yükleme, (bir bilişsel arama işlem hattı yapılandırılmışsa) zenginleştirmelerinin uygulama ve dizine yazmaya başlayın. Azure arama dizini güncelleştirmeleri kilitlemez. Eşzamanlı yazma belirli bir yazma ilk denemede başarısız olursa yeniden deneme ile yönetilir.

> [!Note]
> Çoğaltmaları artırılması, dizin boyutu büyük ölçüde artırmak için öngörülen, bölüm sayısını artırmayı düşünün. Bölüm dilimleri dizinlenmiş içeriği depolar; Daha fazla bölüm sahip olduğunuz, depolamak daha küçük dilim her birine sahiptir.

## <a name="see-also"></a>Ayrıca bkz.

+ [Dizin Oluşturucu’ya genel bakış](search-indexer-overview.md)
+ [Portalda dizin oluşturma](search-import-data-portal.md)
+ [Azure SQL veritabanı dizin oluşturucu](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [Azure Cosmos DB dizinleyici](search-howto-index-cosmosdb.md)
+ [Azure Blob Depolama dizin oluşturucu](search-howto-indexing-azure-blob-storage.md)
+ [Azure Tablo Depolama dizin oluşturucu](search-howto-indexing-azure-tables.md)
+ [Azure Search'te güvenlik](search-security-overview.md)
