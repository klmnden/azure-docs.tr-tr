---
title: Azure depolama tablolar sorgular için Tasarım | Microsoft Docs
description: Azure tablo depolaması sorguları için tabloları tasarlayın.
services: storage
documentationcenter: na
author: MarkMcGeeAtAquent
manager: kfile
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/23/2018
ms.author: sngun
ms.openlocfilehash: b8d2033b0b29caddf165f4b582c7d0578109480c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661072"
---
# <a name="design-for-querying"></a>Sorgulama için Tasarım
Tablo hizmeti çözümleri yoğun, yazma yoğun veya ikisinin bir karışımı okuyabilirsiniz. Bu makalede okuma işlemleri verimli bir şekilde desteklemek için tablo hizmeti tasarlarken de hesaba katılmalıdır gerekenler odaklanır. Genellikle, desteklediği işlemleri verimli bir şekilde okuma tasarım de yazma işlemleri için verimli olur. Ancak, desteklemek için tasarlama yazdığınızda, işlemler, makalesinde açıklanan de hesaba katılmalıdır gereken ek noktalar vardır [veri değişikliği için tasarım](table-storage-design-for-modification.md).

"Sorguları Uygulamam tablo hizmetinden ihtiyacı olan verileri almak için yürütme gerekecek mi?" sormak için veri etkin şekilde okumanızı sağlamak için tablo hizmeti çözümünüzü tasarlamak için iyi bir başlangıç noktası değil  

> [!NOTE]
> Tablo hizmeti ile zor ve pahalı daha sonra değiştirmeye olduğundan tasarım doğru Önden almak önemlidir. Örneğin, bir ilişkisel veritabanı genellikle dizinler yalnızca mevcut bir veritabanına ekleyerek performans sorunlarına yönelik mümkündür: Bu tablo hizmeti ile bir seçenek değil.  
> 
> 

Bu bölümde sorgulamak için tabloları tasarlarken çözülmesi gereken anahtar durumlara odaklanmaktadır. Bu bölümde yer alan konuları içerir:

* [PartitionKey ve RowKey seçiminizi sorgu performansını nasıl etkilediğini](#how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance)
* [Uygun bir PartitionKey seçme](#choosing-an-appropriate-partitionkey)
* [Tablo hizmeti için en iyi duruma getirme sorguları](#optimizing-queries-for-the-table-service)
* [Tablo hizmetinde verileri sıralama](#sorting-data-in-the-table-service)

## <a name="how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance"></a>PartitionKey ve RowKey seçiminizi sorgu performansını nasıl etkilediğini
Aşağıdaki örneklerde, tablo hizmetinde şu yapıda çalışan varlıkları depolamak varsayılmaktadır (örnekler çoğunu atlayın **zaman damgası** özelliği daha anlaşılır olması için):  

| *Sütun adı* | *Veri türü* |
| --- | --- |
| **PartitionKey** (bölüm adı) |Dize |
| **RowKey** (çalışan kimliği) |Dize |
| **FirstName** |Dize |
| **Soyadı** |Dize |
| **geçerlilik süresi** |Tamsayı |
| **EmailAddress** |Dize |

Makaleyi [Azure Table depolama genel bakış](table-storage-overview.md) bazı Azure tablo Hizmeti sorgu tasarlama üzerinde doğrudan etkisi olan temel özellikleri açıklanmaktadır. Bunlar, tablo hizmeti sorguları tasarlamak için aşağıdaki genel yönergeleri sonuçlanır. Daha fazla bilgi için tablo hizmetinden REST API'si, aşağıdaki örneklerde kullanılan filtresi sözdizimi olduğuna dikkat edin [sorgu varlıklar](https://docs.microsoft.com/rest/api/storageservices/Query-Entities).  

* A ***noktası sorgusu*** kullanmak için en verimli arama ve yüksek hacimli aramaları veya en düşük gecikme gerektiren aramalar için kullanılacak önerilir. Böyle bir sorguyu her ikisi de belirterek tek bir varlık çok verimli bir şekilde bulmak için dizinler kullanabilir **PartitionKey** ve **RowKey** değerleri. Örneğin: $filter (PartitionKey eq 'Satış') = ve (RowKey eq '2')  
* İkinci en iyi olan bir ***aralık sorgusu*** kullanan **PartitionKey** ve bir dizi filtreleri **RowKey** birden fazla varlık döndürülecek değer. **PartitionKey** değer belirli bir bölüm tanımlar ve **RowKey** değerleri o bölümün varlıklarda kümesini tanımlayın. Örneğin: $filter PartitionKey eq 'Satış ve RowKey ge'nın ' ve RowKey lt 'T ='  
* Üçüncü iyi bir ***bölüm tarama*** kullanan **PartitionKey** ve başka bir anahtar dışı özelliği ve, filtreleri, birden fazla varlık döndürebilir. **PartitionKey** değer belirli bir bölüm tanımlar ve değerlerini özellik o bölümün varlıklarda alt kümeleri için seçin. Örneğin: $filter PartitionKey eq 'Satış' ve LastName eq 'Smith' =  
* A ***tablo tarama*** içermemesi **PartitionKey** ve tüm tablonuzu sırayla eşleşen tüm varlıkların oluşturan bölümlerin arar çok verimli değildir. Olsun veya olmasın, filtre kullanır, bağımsız olarak bir tablo taraması gerçekleştirecek **RowKey**. Örneğin: $filter LastName eq 'Can' =  
* Birden çok varlık döndüren sorgular bunları içinde sıralanmış dönmek **PartitionKey** ve **RowKey** sırası. Varlıklar istemci yeniden ayırma önlemek için seçin bir **RowKey** en yaygın sıralama düzenini tanımlar.  

Bu kullanarak not bir "**veya**" dayalı bir filtre belirtmek için **RowKey** değerleri bir bölüm tarama sonuçları ve aralığı sorgu olarak işlenmez. Bu nedenle, filtreleri gibi kullanan sorguları kaçınmanız gerekir: $filter PartitionKey eq 'Satış' ve RowKey eq '121' = (veya RowKey eq '322')  

Verimli sorgularını yürütmek için depolama istemci kitaplığı kullanan istemci-tarafı kod örnekleri için bkz:  

* [Depolama istemci kitaplığı kullanılarak noktası sorgu yürütme](table-storage-design-patterns.md#executing-a-point-query-using-the-storage-client-library)
* [LINQ kullanarak birden çok varlık alma](table-storage-design-patterns.md#retrieving-multiple-entities-using-linq)
* [Sunucu tarafı projeksiyonu](table-storage-design-patterns.md#server-side-projection)  

Birden çok varlık türleri aynı tabloda depolanan işleyebilir istemci-tarafı kod örnekleri için bkz:  

* [Heterojen varlık türleri ile çalışma](table-storage-design-patterns.md#working-with-heterogeneous-entity-types)  

## <a name="choosing-an-appropriate-partitionkey"></a>Uygun bir PartitionKey seçme
Tercih ettiğiniz **PartitionKey** (tutarlılığını sağlamak için) EGTs kullanımını etkinleştirmek için gereken varlıklarınızı (ölçeklenebilir bir çözüm sağlamak için) birden çok bölüm arasında dağıtmak için gereksinim karşı dengelemeniz.  

Bir extreme adresindeki tüm varlıkları tek bir bölüm saklayabilirsiniz, ancak bu çözümünüzü ölçeklenebilirliğini sınırlayabilir ve tablo hizmeti Yük Dengeleme isteklerini engellemek. Diğer uçta bu yüksek oranda ölçeklenebilir olur ve Yük Dengeleme isteklerini tablo hizmetine sağlar, ancak hangi, varlık Grup hareketleri kullanmasını önler bölüm başına tek bir varlık saklayabilirsiniz.  

İdeal **PartitionKey** verimli sorguları kullanmanıza olanak sağlayan ve çözümünüzü ölçeklenebilir olduğundan emin olmak için yeterli bölümleri olan biridir. Genellikle, varlıklarınızı varlıklarınızı yeterli bölümler dağıtır uygun bir özellik olduğunu bulabilirsiniz.

> [!NOTE]
> Örneğin, kullanıcılar veya çalışanlar ilgili bilgileri depoladığı bir sistemde, iyi PartitionKey UserID olabilir. Bölüm anahtarı olarak belirli bir kullanıcı kimliği kullanan birden fazla varlık sahip. Bir kullanıcı hakkında verilerini depolayan her varlığın tek bir bölümde gruplandırılır ve bu nedenle bu varlıkları hala yüksek oranda ölçeklenebilir devam ederken varlık Grup hareketleri erişilebilir.
> 
> 

Tercih ettiğiniz hususlar vardır **PartitionKey** nasıl, ekleme, güncelleştirme ve varlıkları silmek için ilişkilidir. Daha fazla bilgi için bkz: [tablolar için veri değişikliği tasarlama](table-storage-design-for-modification.md).  

## <a name="optimizing-queries-for-the-table-service"></a>Tablo hizmeti için en iyi duruma getirme sorguları
Tablo hizmeti otomatik olarak kullanarak, varlıklarınızı dizinler **PartitionKey** ve **RowKey** tek bir kümelenmiş dizin, bu nedenle sorguları noktası neden değerler kullanmak için en etkili. Ancak, vardır kümelenmiş dizini dışında hiçbir dizinler **PartitionKey** ve **RowKey**.

Çoğu tasarımları birden çok ölçüte dayalı varlıkların aramasını etkinleştirmek için gereksinimleri karşılaması gerekir. Örneğin, e-postalar, temel çalışan varlıkları bulma çalışan kimliği ya da son adı. Desenler açıklanan [tablo Tasarım desenleri](table-storage-design-patterns.md) gereksinim bu tür adres ve tablo hizmetinde ikincil dizinler sağlamaz olgu çözümüne yolları açıklanmaktadır:  

* [İçi bölüm ikincil dizin düzeni](table-storage-design-patterns.md#intra-partition-secondary-index-pattern) -her varlık kullanan birden çok kopyalarını farklı depolama **RowKey** değerlerini (aynı bölüm) etkinleştirmek hızlı ve verimli aramalar ve farklı kullanarak diğer sıralamalar **RowKey** değerleri.  
* [İkincil dizin arası bölüm düzeni](table-storage-design-patterns.md#inter-partition-secondary-index-pattern) -her varlık kullanan birden çok kopyalarını farklı depolama **RowKey** değerleri de, bölümler ayrı veya içinde hızlı ve verimli aramaları ve diğer sıralama etkinleştirmek için tabloları ayırın farklı kullanarak siparişleri **RowKey** değerleri.  
* [Dizin varlıkları düzeni](table-storage-design-patterns.md#index-entities-pattern) -varlıklar listesi Döndür verimli aramalar etkinleştirmek için dizin varlıkları korumak.  

## <a name="sorting-data-in-the-table-service"></a>Tablo hizmetinde verileri sıralama
Tablo hizmeti göre artan düzende sıralandı varlıklar döndürüyor **PartitionKey** ve sonra **RowKey**. Bu anahtarları dize değerlerini ve sayısal değerleri doğru sıralamak emin olmak için sabit bir uzunluğa Dönüştür sıfırlarla doldurur. Örneğin, çalışan kimlik değeri olarak kullanırsanız, **RowKey** bir tamsayı değeri çalışan kimliği dönüştürmeniz gerekir **123** için **00000123**.  

Birçok uygulama farklı siparişler sıralanmış veri kullanımı için gereksinimler vardır: Örneğin, çalışanlar ada göre ya da tarih katılarak sıralama. Aşağıdaki desenleri nasıl sıralamalar varlıklarınızı için alternatif adres:  

* [İçi bölüm ikincil dizin düzeni](table-storage-design-patterns.md#intra-partition-secondary-index-pattern) - hızlı etkinleştirmek için (aynı bölümde) farklı RowKey değerleri kullanarak her bir varlık birden çok kopyasını depolamak ve verimli aramaları ve diğer sıralama siparişleri farklı RowKey değerleri kullanarak.  
* [İkincil dizin arası bölüm düzeni](table-storage-design-patterns.md#inter-partition-secondary-index-pattern) - hızlı etkinleştirmek için ayrı tablolarda ayrı bölümlerdeki farklı RowKey değerleri kullanarak her bir varlık birden çok kopyasını depolamak ve verimli aramaları ve diğer sıralama siparişleri farklı RowKey değerleri kullanarak .
* [Günlük tail düzeni](table-storage-design-patterns.md#log-tail-pattern) -almak *n* kullanılarak bir bölüm için en son eklenen varlıklar bir **RowKey** ters tarihi ve saati sipariş sıralar değeri.  

## <a name="next-steps"></a>Sonraki adımlar

- [Tablo Tasarım desenleri](table-storage-design-patterns.md)
- [Modelleme ilişkileri](table-storage-design-modeling.md)
- [Tablo verileri şifrele](table-storage-design-encrypt-data.md)
- [Veri değişikliği için Tasarım](table-storage-design-for-modification.md)
