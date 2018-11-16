---
title: Bir Azure Search dizini veya yenileme aranabilir içeriği yeniden | Microsoft Docs
description: Yeni öğeler eklemek, var olan öğeleri veya belgeleri güncelleştirin veya tam yeniden derleme veya kısmi artımlı bir Azure Search dizini yenilemek için dizin geçersiz belgelerde silme.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: heidist
ms.openlocfilehash: 0b346756349c483dea32ec31827a653bd9b777cf
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51705948"
---
# <a name="how-to-scale-out-indexing-in-azure-search"></a>Ölçeklendirme Azure Search'te dizin oluşturma nasıl

Veri birimleri büyütün veya işleme gereksinimleri gibi basit bulabilirsiniz [yeniden oluşturmayı ve işleri ölçeklemek](search-howto-reindex.md) yeterli değildir. 

Bir ilk adım olarak, artan taleplere toplantı doğrultusunda, artırmanız olan öneririz [ölçek ve kapasite](search-capacity-planning.md) mevcut hizmetiniz sınırları dahilinde. 

İkinci adım, kullanabiliyorsa [dizin oluşturucular](search-indexer-overview.md), ölçeklenebilir dizin oluşturma mekanizmaları ekler. Dizin oluşturucular paket düzenli aralıklarla dizin kullanıma olanak tanıyan yerleşik bir Zamanlayıcı ile gelir veya işleme 24 saat penceresinde ötesine genişletir. Ayrıca, veri kaynağı tanımlarının ile birlikte kullanıldığında, dizin oluşturucular veri bölümleme ve paralel olarak yürütmek için zamanlamaları kullanarak bir form paralellik derecesi elde etmenize yardımcı olur.

### <a name="scheduled-indexing-for-large-data-sets"></a>Büyük veri kümeleri için dizin oluşturma zamanlandı

Zamanlama, büyük veri kümeleri ve işlem hattında bilişsel arama görüntü analizi gibi yavaş çalışan analizleri işlemek için önemli bir mekanizmadır. Dizin Oluşturucu işleme bir 24 saatlik penceresi içinde çalışır. İşleme 24 saat içinde tamamlamak için başarısız olursa, Dizin Oluşturucu ile zamanlama davranışları kendi yararınıza çalışabilir. 

Tasarım gereği, genellikle bir sonraki zamanlanmış aralıkta devam etmeden önce tamamlanmasını bir işlemle dizin oluşturma başlatıldığında belirli aralıklarla zamanlanmış. Ancak, işleme aralığı içinde tamamlanmazsa, dizin oluşturucu (süre bitti çalıştığından) durdurur. Sonraki aralıkta nerede oluştuğunu, son kapalı, sistem tutma ile kaldığı işleme sürdürür izleyin. 

Dizin yüklemelerini birkaç gün genişleme için pratik anlamda, 24 saatlik zaman çizelgesinde dizin oluşturucu koyabilirsiniz. Sürdürür sonraki 24 saatlik stint için dizin oluştururken en son bilinen iyi belgeyi yeniden başlatır. Bu şekilde, bir dizin oluşturucu, bir dizi tüm işlenmemiş belgeleri işlenene kadar bir gün üzerinden yolu bir belge biriktirme listesi ile çalışabilir. Bu yaklaşımı hakkında daha fazla bilgi için bkz: [büyük veri kümelerini dizin oluşturma](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets)

<a name="parallel-indexing"></a>

## <a name="parallel-indexing"></a>Paralel dizin oluşturma

İkinci seçenek, paralel bir dizin oluşturma stratejisi ayarlamaktır. Olmayan-rutin için işlem bakımından yoğun işlem hattındaki bir bilişsel arama, taranan belgeleri üzerinde OCR gibi gereksinimleri dizin oluşturma stratejisi dizin paralel doğru yaklaşım, belirli bir hedefe yönelik olabilir. Bilişsel arama zenginleştirme hattında, görüntü analizi ve doğal dil işleme uzun. Paralel dizin sorgu istekleri aynı anda işleme değil bir hizmette büyük bir yavaş işleme İçerik gövdesi ile çalışmak için uygulanabilir bir seçenek olabilir. 

Paralel işleme için bir strateji, bu öğelere sahiptir:

+ Kaynak verileri birden çok kapsayıcı ya da aynı kapsayıcı içinde birden çok sanal klasörler arasında bölün. 
+ Mini her veri kümesine eşleme bir [tarih kaynak](https://docs.microsoft.com/rest/api/searchservice/create-data-source), kendi için eşleştirilmiş [dizin oluşturucu](https://docs.microsoft.com/rest/api/searchservice/create-indexer).
+ Bilişsel arama için aynı başvuru [beceri kümesi](https://docs.microsoft.com/rest/api/searchservice/create-skillset) her dizin oluşturucu tanımı.
+ Aynı hedef arama dizinine yazma. 
+ Tüm Dizin oluşturucuların aynı anda çalışacak şekilde zamanlayın.

> [!Note]
> Azure arama, ayrılmış çoğaltmalar veya bölümler belirli iş yükleri için desteklemez. Ağır eşzamanlı dizin oluşturma riskini zarar sisteminize sorgu performansını overburdening olduğu. Bir test ortamınız varsa, paralel var. ilk ödün anlamak için dizin oluşturma uygulayın.

## <a name="configure-parallel-indexing"></a>Paralel dizin oluşturmayı yapılandır

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