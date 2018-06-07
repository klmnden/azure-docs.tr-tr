---
title: Bir Azure Search dizini veya yenileme Aranabilir içeriğin yeniden | Microsoft Docs
description: Yeni öğeler eklemek, varolan öğeleri veya belgeleri güncelleştirin veya eski bir tam yeniden veya kısmi artımlı bir Azure Search dizini yenilemek için dizin oluşturma belgelerde silme.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: heidist
ms.openlocfilehash: cb99096c1217fca1527b17946dc12390ddf3f62c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655811"
---
# <a name="how-to-scale-out-indexing-in-azure-seearch"></a>Azure Seearch genişleme dizin nasıl

Veri birimleri büyütün veya işleme gereksinimleri gibi basit bulabilirsiniz [yeniden oluşturmayı ve işleri yeniden dizin oluşturmaya](search-howto-reindex.md) yeterli değildir. 

Artan taleplere toplantı doğrultusunda ilk adım olarak, artırmanız olan öneririz [ölçek ve kapasite](search-capacity-planning.md) mevcut hizmetiniz sınırları içinde. 

İkinci adım kullanabilirsiniz, [dizin oluşturucular](search-indexer-overview.md), ölçeklenebilir dizin oluşturma için mekanizmaları ekler. Dizin oluşturucular düzenli aralıklarla dizin çıkışı paket imkan tanıyan yerleşik bir zamanlayıcı gelir veya 24 saatlik bir aralık ötesinde işleme genişletin. Ayrıca, veri kaynağı tanımlarının ile eşlenmiş, dizin oluşturucular veri bölümlendirme ve paralel olarak yürütmek için zamanlamaları kullanarak bir form paralellik elde yardımcı olur.

### <a name="scheduled-indexing-for-large-data-sets"></a>Büyük veri kümeleri için dizin oluşturma zamanlanmış

Zamanlama, büyük veri kümeleri ve görüntü analiz bilişsel arama ardışık düzeninde gibi yavaş çalışan çözümlemeler işlemek için önemli bir mekanizmadır. Dizin Oluşturucu işleme 24 saatlik bir aralık içinde çalışır. Son 24 saat içinde başarısız işlem, dizin oluşturucu zamanlaması davranışları kendi yararınıza çalışabilir. 

Tasarım gereği, genellikle bir sonraki zamanlanmış aralıkta devam etmeden önce tamamlanmasını bir iş ile dizin oluşturma başlatıldığında belirli aralıklarla zamanlanmış. Ancak, işleme aralığı içinde tamamlanmazsa (süreniz doldu çünkü) dizin oluşturucu durdurur. Sonraki aralıkta, nerede oluştuğunu, son kapalı, sistem tutma ile kaldığı işlemeyi sürdürür izler. 

Birkaç gün kapsayıcı dizini yükler için pratik terimleriyle dizin oluşturucu 24 saatlik zamanlamaya koyabilirsiniz. Sonraki 24 saat stint sürdürür dizin oluşturulurken, en son bilinen iyi belgeyi yeniden başlatır. Bu şekilde, bir dizin oluşturucu, tüm işlenmemiş belgeleri işlenene kadar gün bir dizi üzerinden belge biriktirme listesi kendi aşamalardaki çalışabilir. Bu yaklaşımı hakkında daha fazla bilgi için bkz: [büyük veri kümeleri dizin oluşturma](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets)

<a name="parallel-indexing"></a>

## <a name="parallel-indexing"></a>Paralel dizin oluşturma

Stratejisi dizin paralel kümesi ikinci bir seçimdir. Olmayan-yordamı, pkı'ya yoğun bilişsel arama düzenindeki taranmış belgeler üzerinde OCR gibi gereksinimleri dizin stratejisi dizin paralel sağ yaklaşım, belirli bir amaç için olabilir. Bilişsel arama iyileştirmesini ardışık düzeninde, görüntü çözümleme ve doğal dil işleme uzun süredir çalışıp. Paralel sorgu isteği aynı anda işlenmemesinin bir hizmette dizin yavaş işleme içeriğin büyük bir gövde ile çalışmak için uygulanabilir bir seçenek olabilir. 

Bir strateji paralel işleme Bu öğe vardır:

+ Kaynak verileri birden çok kapsayıcı ya da aynı kapsayıcı içinde birden çok sanal klasörler arasında böler. 
+ Her mini veri kümesine eşleme bir [tarih kaynak](https://docs.microsoft.com/rest/api/searchservice/create-data-source), eşleştirilmiş, kendi [dizin oluşturucu](https://docs.microsoft.com/rest/api/searchservice/create-indexer).
+ Bilişsel arama için aynı başvuru [skillset](https://docs.microsoft.com/rest/api/searchservice/create-skillset) her dizin oluşturucu tanımında.
+ Aynı hedef search dizinine yazma. 
+ Aynı anda çalıştırmak için tüm dizin oluşturucuların zamanlayın.

> [!Note]
> Azure arama ayrılması çoğaltmaları veya belirli iş yükleri için bölümleri desteklemez. Yoğun eşzamanlı dizin riskini zarar sisteminize sorgu performansını overburdening. Bir sınama ortamınız varsa, paralel var. önce dengelemeden anlamak için dizin oluşturma işlemi uygulayın.

## <a name="configure-parallel-indexing"></a>Paralel dizin oluşturmayı yapılandır

Dizin oluşturucular için kapasite işleme geniş bir dizin oluşturucu alt sisteminde arama hizmeti tarafından kullanılan her hizmet birimi (SU) için temel alır. Birden çok eşzamanlı dizin oluşturucular, Azure arama hizmetleri en az iki çoğaltma sahip temel veya standart katmanda bulunan sağlanan mümkündür. 

1. İçinde [Azure portal](https://portal.azure.com), arama hizmeti Panonuzda **genel bakış** sayfasında, onay **fiyatlandırma katmanı** uyum paralel dizin onaylamak için. Temel ve standart katmanları, birden çok çoğaltma sunar.

2. İçinde **ayarları** > **ölçek**, [çoğaltmaları artırmak](search-capacity-planning.md) paralel işleme: her dizin oluşturucu iş yükü için ek bir çoğaltma. Varolan bir sorguyu birim için yeterli sayıda bırakın. Dizin oluşturma için sorgu iş yükleri ödün iyi kolaylığını değil.

3. Azure Search'te dizin oluşturucular ulaşabileceği bir düzeyinde birden çok kapsayıcı halinde veri dağıtın. Bu, Azure SQL Database, Azure Blob Depolama alanında birden çok kapsayıcı ya da birden çok koleksiyon birden çok tablo olabilir. Her bir tablo veya kapsayıcı için bir veri kaynağı nesnesi tanımlayın.

4. Oluşturun ve paralel olarak çalıştırmak için birden çok dizin oluşturucu zamanla:

   + Bir hizmet altı yinelemelerle varsayalım. Altı dizin oluşturucular, tüm veri kümesinin 6 yönlü bölme için veri kümesinin tek altıncı içeren bir veri kaynağına eşlenen her birini yapılandırın. 

   + Her dizin oluşturucu aynı dizine gelin. Bilişsel arama iş yükleri için her dizin oluşturucu için aynı skillset gelin.

   + Her dizin oluşturucu tanımı içinde aynı çalışma zamanı yürütme düzeni zamanlayın. Örneğin, `"schedule" : { "interval" : "PT8H", "startTime" : "2018-05-15T00:00:00Z" }` 2018-05-15 sekiz saatlik aralıklarla çalıştıran tüm dizin oluşturucuların üzerinde bir zamanlama oluşturur.

Zamanlanan saatte tüm dizin oluşturucuların veri yükleme (bilişsel arama ardışık düzen yapılandırılmışsa) enrichments uygulama ve dizine yazma yürütülmesine başlar. Azure arama dizini güncelleştirmeler için kilit yok. Eşzamanlı yazma belirli bir yazma ilk denemede başarısız olursa yeniden deneme ile yönetilir.

> [!Note]
> Çoğaltmaları artırılması, dizin boyutu büyük ölçüde artırmak için öngörülen varsa bölüm sayısı artırmayı düşünün. Bölümler dilimler dizinli içeriğinin depolamak; Daha fazla bölüm vardır, daha küçük diliminin her biri depolamak vardır.

## <a name="see-also"></a>Ayrıca bkz.

+ [Dizin Oluşturucu’ya genel bakış](search-indexer-overview.md)
+ [Portal'da dizin oluşturma](search-import-data-portal.md)
+ [Azure SQL veritabanı dizin oluşturucu](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [Azure Cosmos DB dizinleyici](search-howto-index-cosmosdb.md)
+ [Azure Blob Depolama dizin oluşturucu](search-howto-indexing-azure-blob-storage.md)
+ [Azure Tablo Depolama dizin oluşturucu](search-howto-indexing-azure-tables.md)
+ [Azure Search'te güvenlik](search-security-overview.md)