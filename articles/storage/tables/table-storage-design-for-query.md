---
title: Azure depolama tabloları, sorguları tasarlama | Microsoft Docs
description: Sorgular, Azure tablo depolaması için tablolar tasarlayın.
services: storage
author: MarkMcGeeAtAquent
ms.service: storage
ms.topic: article
ms.date: 04/23/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: 97373f6f0138d3ed8028ed4327b7e6cf90ad76a7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60325876"
---
# <a name="design-for-querying"></a>Sorgulama için tasarım
Tablo hizmeti çözümleri, yoğun, yazma yoğun veya ikisinin bir karışımı okunabilir. Bu makalede, okuma işlemleri verimli bir şekilde destekleyin, tablo hizmeti tasarlarken göz önünde ayı gerekenler odaklanır. Genellikle, desteklediği işlemleri verimli bir şekilde okuyun bir tasarım de yazma işlemleri için verimli olur. Ancak, destekleyecek şekilde tasarlama yazdığınızda makalesinde açıklanan işlem, göz önünde işlenmesi için ek hususlar vardır [veri değişikliği için tasarım](table-storage-design-for-modification.md).

"Sorguları tablo hizmetinden ihtiyacı olan verileri almak için yürütmek uygulamamı gerekir?" isteyebilir, verimli bir şekilde veri okumanızı sağlamak için tablo hizmeti çözümünüzü tasarlama için iyi bir başlangıç noktası olan  

> [!NOTE]
> Tablo hizmeti ile zor ve daha sonra değiştirmek pahalı olduğundan tasarım doğru Önden almak önemlidir. Örneğin, ilişkisel bir veritabanında genellikle yalnızca mevcut bir veritabanına dizinleri ekleyerek performans sorunlarını gidermek için mümkündür: Bu tablo hizmeti ile bir seçenek değildir.  
> 
> 

Bu bölümde tablolarınızı sorgulamak için tasarlarken çözülmesi gereken önemli sorunları ele alınmaktadır. Bu bölümde ele alınan konular:

* [PartitionKey ve RowKey seçiminizi sorgu performansı nasıl etkiler](#how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance)
* [Uygun bir PartitionKey seçme](#choosing-an-appropriate-partitionkey)
* [Tablo hizmeti için en iyi duruma getirme sorguları](#optimizing-queries-for-the-table-service)
* [Tablo hizmeti verileri sıralama](#sorting-data-in-the-table-service)

## <a name="how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance"></a>PartitionKey ve RowKey seçiminizi sorgu performansı nasıl etkiler
Aşağıdaki örneklerde, tablo hizmeti aşağıdaki yapıya sahip çalışan varlıkları depolamak varsayılmaktadır (örneklerin çoğu atlamak **zaman damgası** açıklık için özellik):  

| *Sütun adı* | *Veri türü* |
| --- | --- |
| **PartitionKey** (bölüm adı) |Dize |
| **RowKey** (çalışan kimliği) |Dize |
| **FirstName** |Dize |
| **Soyadı** |Dize |
| **Geçerlilik süresi** |Tamsayı |
| **EmailAddress** |String |

Makaleyi [Azure tablo depolamaya genel bakış](table-storage-overview.md) Azure tablo hizmeti için sorgu tasarlama üzerinde doğrudan bir etkisi olan anahtar özelliklerinden bazılarını açıklar. Bunlar, tablo hizmeti sorguları tasarlamaya yönelik aşağıdaki genel yönergeleri sonuçlanır. Daha fazla bilgi için tablo hizmeti REST API, aşağıdaki örneklerde kullanılan filtre söz dizimi olduğuna dikkat edin [varlıkları sorgulayın](https://docs.microsoft.com/rest/api/storageservices/Query-Entities).  

* A ***noktası sorgusu*** kullanılacak en verimli arama ve yüksek hacimli aramaları veya en düşük gecikme gerektiren aramalar için kullanılması önerilir. Böyle bir sorguyu her ikisi de belirterek tek bir varlık çok verimli bir şekilde bulmak için dizinleri kullanabilirsiniz **PartitionKey** ve **RowKey** değerleri. Örneğin: $filter = (PartitionKey eq 'Satış') ve (RowKey eq '2')  
* İkinci en iyi olan bir ***aralık sorgusu*** kullanan **PartitionKey** ve bir dizi filtreleri **RowKey** birden fazla varlık döndürülecek değer. **PartitionKey** değer belirli bir bölüm tanımlar ve **RowKey** değerleri bu bölümdeki varlıkları kümesini belirleyin. Örneğin: $filter PartitionKey eq 'Satışları'değerine ve RowKey ge'nin' ve RowKey lt 'T ='  
* Üçüncü iyi bir ***bölüm tarama*** kullanan **PartitionKey** ve başka bir anahtar olmayan özellik ve bu filtreler, birden fazla varlık döndürebilir. **PartitionKey** değeri belirli bir bölüm tanımlar ve özellik değerleri için bu bölümdeki varlıkları kümesini seçin. Örneğin: $filter PartitionKey eq 'Satış' ve LastName eq 'Smith' =  
* A ***tablo tarama*** içermemesi **PartitionKey** ve tüm tablonuzda sırayla eşleşen tüm varlıkların oluşturan bölümlerin arar çok yetersizdir. Olsun veya olmasın, filtre kullanır, bağımsız olarak bir tablo taraması gerçekleştirecek **RowKey**. Örneğin: $filter LastName eq 'Jones' =  
* Birden çok varlık döndüren sorgular onları sıralanmış olarak döndürür **PartitionKey** ve **RowKey** sırası. Maksimum istemci varlıklarda önlemek için seçin bir **RowKey** , en yaygın sıralama düzenini tanımlar.  

Bu Not bir "**veya**" dayalı bir filtre belirtmek için **RowKey** değerleri bölüm tarama sonuçları ve bir aralık sorgusu işlenmez. Bu nedenle, aşağıdaki gibi filtreler kullanan sorguları kaçınmanız gerekir: $filter PartitionKey eq 'Satış' ve RowKey eq '121' = (veya RowKey eq '322')  

Verimli sorgular çalıştırmak için depolama istemcisi kitaplığı kullanan istemci tarafı kod örnekleri için bkz:  

* [Depolama istemcisi Kitaplığı'nı kullanarak bir nokta sorgu yürütme](table-storage-design-patterns.md#executing-a-point-query-using-the-storage-client-library)
* [LINQ kullanarak birden çok varlıkları alma](table-storage-design-patterns.md#retrieving-multiple-entities-using-linq)
* [Sunucu tarafı projeksiyonu](table-storage-design-patterns.md#server-side-projection)  

Aynı tabloda depolanan birden fazla varlık türleri işleyebilir istemci tarafı kod örnekleri için bkz:  

* [Heterojen varlık türleri ile çalışma](table-storage-design-patterns.md#working-with-heterogeneous-entity-types)  

## <a name="choosing-an-appropriate-partitionkey"></a>Uygun bir PartitionKey seçme
Tercih ettiğiniz **PartitionKey** EGTs (tutarlılık sağlamak üzere) kullanımını etkinleştirmek için gereken karşı varlıklarınızı (ölçeklenebilir bir çözüm sağlamak üzere) birden çok bölümler arasında dağıtmak için ihtiyaç dengelemeniz.  

Bir üst düzey, tek bir bölümde tüm varlıklarınızı depolayabilir, ancak bu çözümünüzü ölçeklenebilirliğini sınırlandırabilir ve tablo hizmeti, Yük Dengeleme isteklerini airdrop önler. Diğer uçta, yüksek oranda ölçeklenebilir olması ve istek yükünü dengelemek için tablo hizmeti sağlayan, ancak bu, varlık grubu işlemleri kullanmasını önler bölüm başına tek bir varlık olarak depolayabilir.  

İdeal **PartitionKey** verimli sorgular kullanmanıza olanak sağlar ve çözümünüzün ölçeklenebilir olduğundan emin olmak için yeterli bölümleri olan biridir. Genellikle, varlıklarınızı varlıklarınızı yeterli bölümler arasında dağıtır. uygun bir özellik olduğunu bulabilirsiniz.

> [!NOTE]
> Örneğin, kullanıcılar veya çalışanlar hakkında bilgileri depolayan bir sistemde iyi PartitionKey UserID olabilir. Bölüm anahtarı olarak belirli bir kullanıcı kimliği kullanan çeşitli varlıklara olabilir. Bir kullanıcı hakkında daha fazla veri depolar her varlığın tek bir bölümde gruplandırılır ve bu nedenle bu varlıkları hala yüksek oranda ölçeklenebilir olmanın yanı sıra varlık grubu işlemleri erişilebilir.
> 
> 

Kendi seçtiğiniz ek hususlar vardır **PartitionKey** nasıl, ekleyecek, güncelleştirme ve varlıkları silmek üzere ilgilidir. Daha fazla bilgi için [veri değişikliği için tabloları tasarlama](table-storage-design-for-modification.md).  

## <a name="optimizing-queries-for-the-table-service"></a>Tablo hizmeti için en iyi duruma getirme sorguları
Tablo hizmeti kullanarak varlıklarınızı otomatik olarak dizinleyen **PartitionKey** ve **RowKey** tek bir kümelenmiş dizin, bu nedenle sorguları noktası nedeni değerler kullanılacak en verimli. Ancak, vardır kümelenmiş dizini dışındaki hiçbir dizinler **PartitionKey** ve **RowKey**.

Birçok tasarımı birden çok ölçüte bağlı varlıkların aramasını etkinleştirmek için gereksinimleri karşılaması gerekir. Örneğin, e-postasına erişmelerini göre çalışan varlık bulma çalışan kimliği veya son adı. Desenler açıklanan [tablo Tasarım desenleri](table-storage-design-patterns.md) bu tür bir gereksinimi ele ve tablo hizmeti ikincil dizinler sağlamaz olgu etrafında çalışma yollarını açıklar:  

* [İçi bölüm ikincil dizin düzeni](table-storage-design-patterns.md#intra-partition-secondary-index-pattern) -Store kullanarak her varlığın birden çok kopyalarını farklı **RowKey** değerlerini (aynı bölüme) etkinleştirmek hızlı ve verimli aramalar ve farklı kullanarak alternatif sıralama düzenleri **RowKey** değerleri.  
* [İkincil dizin arası bölüm düzeni](table-storage-design-patterns.md#inter-partition-secondary-index-pattern) -Store kullanarak her varlığın birden çok kopyalarını farklı **RowKey** değerleri bölüm'de ayrı veya içinde tablolar, hızlı ve verimli aramalar ve alternatif sıralama etkinleştirmek için ayrı farklı kullanarak siparişler **RowKey** değerleri.  
* [Dizin varlıklarını düzeni](table-storage-design-patterns.md#index-entities-pattern) -varlıklar listesi döndüren verimli aramalar etkinleştirmek için dizin varlıklarını korumak.  

## <a name="sorting-data-in-the-table-service"></a>Tablo hizmeti verileri sıralama
Tablo hizmeti göre artan düzende sıralanmış varlıklar döndürüyor **PartitionKey** göre ve ardından **RowKey**. Bu anahtarları dize değerlerini ve sayısal değerleri doğru sıralamak emin olmak için bunları dönüştürmek için sabit uzunluk ve bunları sıfır ile doldurur. Örneğin, çalışan kimliği değeri olarak kullanırsanız **RowKey** bir tamsayı değeri olan çalışan kimliği dönüştürmelisiniz **123** için **00000123**.  

Çoğu uygulama farklı sırada sıralanmış veri kullanmak için gereksinimler vardır: Örneğin, çalışanlar ada göre ya da tarih katılarak sıralama. Aşağıdaki desenleri nasıl alternatif sıralama düzenleri varlıklarınızı için adres:  

* [İçi bölüm ikincil dizin düzeni](table-storage-design-patterns.md#intra-partition-secondary-index-pattern) - hızlı etkinleştirmek için (aynı bölümde) farklı RowKey değerleri kullanarak her varlığın birden çok kopyasını Store ve verimli aramalar ve alternatif sıralama siparişleri farklı RowKey değerlerini kullanarak.  
* [İkincil dizin arası bölüm düzeni](table-storage-design-patterns.md#inter-partition-secondary-index-pattern) - hızlı etkinleştirmek için ayrı bölümlerde ayrı tablolarda farklı RowKey değerleri kullanarak her varlığın birden çok kopyasını Store ve verimli aramalar ve alternatif sıralama siparişleri farklı RowKey değerlerini kullanarak .
* [Günlük kuyruğu deseni](table-storage-design-patterns.md#log-tail-pattern) -almak *n* varlıkları kullanarak bir bölüm için en son eklenen bir **RowKey** geriye doğru tarih ve saat sipariş sıralar değeri.  

## <a name="next-steps"></a>Sonraki adımlar

- [Tablo Tasarım desenleri](table-storage-design-patterns.md)
- [İlişkileri modelleme](table-storage-design-modeling.md)
- [Tablo verilerini şifreleme](table-storage-design-encrypt-data.md)
- [Veri değişikliği için Tasarım](table-storage-design-for-modification.md)
