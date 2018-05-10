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
ms.openlocfilehash: f38054eaf2829149a496f840366b6f2f9e03e12b
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="how-to-rebuild-an-azure-search-index"></a>Azure Search dizini yeniden oluşturmak nasıl

Bir dizin değişiklikleri Azure Search hizmetinizde dizin fiziksel bir ifade değiştirilmesine yapısını yeniden oluşturma. Buna karşılık, bir dizin yenileme katkıda bulunan bir dış veri kaynağına ait en son değişiklikleri almak için bir yalnızca içerik güncelleştirmesidir. Bu makalede yönü dizinleri güncelleştirme yapısal olarak ve vermeyiz nasıl sağlar.

Dizin güncelleştirmeleri hizmeti düzeyinde okuma-yazma izinleri gereklidir. Tam yeniden veya artımlı içeriğin güncelleştirme seçenekleri belirterek parametrelerle dizinini için REST veya .NET API'lerini programlı olarak çağırabilirsiniz. 

Genellikle, bir dizin için isteğe bağlı güncelleştirmelerdir. Ancak, kaynak özgü kullanarak doldurulmuş dizinler için [dizin oluşturucular](search-indexer-overview.md), yerleşik bir Zamanlayıcı'yı kullanabilirsiniz. Zamanlayıcı belge yenileme ne olursa olsun aralığı ve ihtiyaç duyduğunuz düzeni kadar her 15 dakikada sıklıkta destekler. Daha hızlı bir yenileme hızı dizin güncelleştirmelerini el ile belki de aynı anda hem dış veri kaynağı hem de Azure Search dizini güncelleştirme işlemlerde çift yazma aracılığıyla iletme gerektirir.

## <a name="full-rebuilds"></a>Tam yeniden

Güncelleştirmeleri birçok türleri için tam bir yeniden oluşturma gereklidir. Tam bir yeniden oluşturma, hem veri hem de dış veri kaynaklarından dizini yeniden tarafından meta veriler, bir dizinin silinmesi ifade eder. Program aracılığıyla, [silmek](https://docs.microsoft.com/rest/api/searchservice/delete-index), [oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index), ve [yeniden](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) yeniden oluşturmak için dizini. 

Sonrası yeniden oluşturma, sorgu desenlerine, test ve temel alınan içeriği değişirse profilleri Puanlama, değişim sorgu sonuçlarında bekleyebileceğiniz istiyorsanız unutmayın.

## <a name="when-to-rebuild"></a>Ne zaman yeniden oluşturmak için

Dizin şemaları flux bir durumda olduğunda plan sık, tam üzerinde etkin geliştirme sırasında yeniden oluşturur.

| Değiştirme | Durum yeniden oluşturma|
|--------------|---------------|
| Veri türü, bir alanın adını değiştirin veya kendi [dizin öznitelikleri](https://docs.microsoft.com/rest/api/searchservice/create-index) | Bir alan tanımı genellikle değiştirme doğurur bunlar dışında bir yeniden cezası [dizin öznitelikleri](https://docs.microsoft.com/rest/api/searchservice/create-index): alınabilir, SearchAnalyzer, SynonymMaps. Kendi dizini yeniden oluşturmak zorunda kalmadan, varolan bir alana alınabilir, SearchAnalyzer ve SynonymMaps öznitelikler ekleyebilirsiniz.|
| Bir alan ekleyin | Rebuild üzerinde katı gereksinimi yoktur. Varolan dizinlenmiş belgeleri yeni alan için bir null değer verilir. Gelecekteki bir arat üzerinde Azure Search tarafından eklenen null değerlere kaynak veri değerleri değiştirin. |
| Bir alanı silme | Bu gibi durumlarda, bir alanın doğrudan bir Azure Search dizini silemezsiniz. Bunun yerine, Yoksay kullanmadan önlemek için "silinmiş" alanı, uygulamanızın olması gerekir. Fiziksel olarak alan tanımı ve içeriği dizinde söz konusu alan atlar bir şema kullanarak dizininizi yeniden açana kadar kalır.|

> [!Note]
> Katmanları geçiş yaparsanız yeniden de gereklidir. Belirli bir noktada üzerinde daha fazla kapasite karar verirseniz, hiçbir yerinde yükseltme yoktur. Yeni bir hizmet yeni kapasite noktada oluşturulmalı ve dizinleri sıfırdan yeni hizmette oluşturulmalıdır. 

## <a name="partial-or-incremental-indexing"></a>Kısmi veya artımlı dizin oluşturma

Bir dizin üretimde eklendiğinde, artımlı, genellikle hiçbir discernable hizmet kesilmelerini ile dizin oluşturma için odak geçer. Kısmi veya artımlı dizin katkıda bulunan bir veri kaynağında içerik durumunu yansıtacak biçimde arama dizini içeriği eşitleyen bir yalnızca içerik iş yükü var. Eklenen veya kaynak silinen bir belge eklenemez veya dizini silindi. Kod içinde arama [ekleme, güncelleştirme veya silme belgeleri](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) işlemi veya .NET eşdeğer.

> [!Note]
> Dış veri kaynaklarına gezinme dizin oluşturucular kullanırken, artımlı dizin oluşturma işlemi için değişiklik izleme mekanizmaları kaynak sistemlerinde yararlanılabilir. İçin [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection), `lastModified` alanı kullanılır. Üzerinde [Azure Table depolama](search-howto-indexing-azure-tables.md#incremental-indexing-and-deletion-detection), `timestamp` aynı amaca hizmet eder. Benzer şekilde, her ikisi de [Azure SQL veritabanı dizin oluşturucu](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#capture-new-changed-and-deleted-rows) ve [Azure Cosmos DB dizin oluşturucu](search-howto-index-cosmosdb.md#indexing-changed-documents) satır güncelleştirmeleri bayrak eklemek için alanları sahiptir. Dizin oluşturucular hakkında daha fazla bilgi için bkz: [dizin oluşturucu genel bakış](search-indexer-overview.md).

## <a name="scale-out-indexing"></a>Dizin genişletme

Veri birimleri büyütün veya işleme gereksinimleri, bulabilirsiniz, basit yeniden oluşturur ve tamamlanmasından işleri yeterli değildir. Artan taleplere toplantı doğrultusunda ilk adım olarak, artırmanız olan öneririz [ölçek ve kapasite](search-capacity-planning.md) mevcut hizmetiniz sınırları içinde. 

Kullanabilirsiniz, [dizin oluşturucular](search-indexer-overview.md), ek genişleme mekanizmaları kullanılabilir olur. Dizin oluşturucular düzenli aralıklarla dizin çıkışı paket imkan tanıyan yerleşik bir zamanlayıcı gelir veya 24 saatlik bir aralık ötesinde işleme genişletin. Ayrıca, veri kaynağı tanımlarının ile eşlenmiş, dizin oluşturucular veri bölümlendirme ve paralel olarak yürütmek için zamanlamaları kullanarak bir form paralellik elde yardımcı olur.

### <a name="scheduled-indexing-for-large-data-sets"></a>Büyük veri kümeleri için dizin oluşturma zamanlanmış

Zamanlama, büyük veri kümeleri ve görüntü analiz bilişsel arama ardışık düzeninde gibi yavaş çalışan çözümlemeler işlemek için önemli bir mekanizmadır. Dizin Oluşturucu işleme 24 saatlik bir aralık içinde çalışır. Son 24 saat içinde başarısız işlem, dizin oluşturucu zamanlaması davranışları kendi yararınıza çalışabilir. 

Tasarım gereği, genellikle bir sonraki zamanlanmış aralıkta devam etmeden önce tamamlanmasını bir iş ile dizin oluşturma başlatıldığında belirli aralıklarla zamanlanmış. Ancak, işleme aralığı içinde tamamlanmazsa (süreniz doldu çünkü) dizin oluşturucu durdurur. Sonraki aralıkta, nerede oluştuğunu, son kapalı, sistem tutma ile kaldığı işlemeyi sürdürür izler. 

Birkaç gün kapsayıcı dizini yükler için pratik terimleriyle dizin oluşturucu 24 saatlik zamanlamaya koyabilirsiniz. Sonraki 24 saat stint sürdürür dizin oluşturulurken, en son bilinen iyi belgeyi yeniden başlatır. Bu şekilde, bir dizin oluşturucu, tüm işlenmemiş belgeleri işlenene kadar gün bir dizi üzerinden belge biriktirme listesi kendi aşamalardaki çalışabilir. Bu yaklaşımı hakkında daha fazla bilgi için bkz: [büyük veri kümeleri dizin oluşturma](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets)

<a name="parallel-indexing"></a>

### <a name="parallel-indexing"></a>Paralel dizin oluşturma

Stratejisi dizin paralel kümesi ikinci bir seçimdir. Olmayan-yordamı, pkı'ya yoğun bilişsel arama düzenindeki taranmış belgeler üzerinde OCR gibi gereksinimleri dizin stratejisi dizin paralel sağ yaklaşım, belirli bir amaç için olabilir. Bilişsel arama iyileştirmesini ardışık düzeninde, görüntü çözümleme ve doğal dil işleme uzun süredir çalışıp. Paralel sorgu isteği aynı anda işlenmemesinin bir hizmette dizin yavaş işleme içeriğin büyük bir gövde ile çalışmak için uygulanabilir bir seçenek olabilir. 

Bir strateji paralel işleme Bu öğe vardır:

+ Kaynak verileri birden çok kapsayıcı ya da aynı kapsayıcı içinde birden çok sanal klasörler arasında böler. 
+ Her mini veri kümesine eşleme bir [tarih kaynak](https://docs.microsoft.com/rest/api/searchservice/create-data-source), eşleştirilmiş, kendi [dizin oluşturucu](https://docs.microsoft.com/rest/api/searchservice/create-indexer).
+ Bilişsel arama için aynı başvuru [skillset](ref-create-skillset.md) her dizin oluşturucu tanımında.
+ Aynı hedef search dizinine yazma. 
+ Aynı anda çalıştırmak için tüm dizin oluşturucuların zamanlayın.

> [!Note]
> Azure arama ayrılması çoğaltmaları veya belirli iş yükleri için bölümleri desteklemez. Yoğun eşzamanlı dizin riskini zarar sisteminize sorgu performansını overburdening. Bir sınama ortamınız varsa, paralel var. önce dengelemeden anlamak için dizin oluşturma işlemi uygulayın.

### <a name="configure-parallel-indexing"></a>Paralel dizin oluşturmayı yapılandır

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