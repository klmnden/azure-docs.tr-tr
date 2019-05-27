---
title: İstek birimleri ve maliyet Azure Cosmos DB'de sorguları çalıştırmak için en iyi duruma getirme
description: Bir sorgu için istek birimi ücreti değerlendirmek ve sorgu performansı ve maliyet açısından en iyi duruma getirme hakkında bilgi edinin.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: rimman
ms.openlocfilehash: 2d1ac054abf4bb8228bdb5cc20d79cb751af7a33
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65967448"
---
# <a name="optimize-query-cost-in-azure-cosmos-db"></a>Azure Cosmos DB'de sorgu gerçekleştirerek

Azure Cosmos DB veritabanı işlemleri bir kapsayıcı içindeki öğeleri üzerinde çalışacağı ilişkisel ve hiyerarşik sorgular da dahil olmak üzere zengin bir özellik kümesi sunar. Bu işlemlerden her biriyle ilişkilendirilmiş maliyet, CPU, GÇ ve işlemi tamamlamak için gerekli belleğe göre değişir. Hakkında düşünmek ve donanım kaynaklarını yönetmek yerine, bir istek Birimi'ni (RU), bir isteğe hizmet vermek için çeşitli veritabanı işlemlerini gerçekleştirmek için gereken kaynaklar için tek ölçü olarak düşünebilirsiniz. Bu makalede, bir sorgu için istek birimi ücreti değerlendirmek ve sorgu performansı ve maliyet açısından en iyi duruma getirmek açıklar. 

Azure Cosmos DB'de sorguları genellikle hızlı/en verimli daha yavaş/daha az verimli aktarım hızı açısından için şu şekilde sıralanır:  

* ALMA işlemi tek bir bölüm anahtarı ve öğe anahtarı.

* Bir filtre yan tümcesi içinde tek bir bölüm anahtarı ile sorgulama.

* Bir eşitlik veya aralık filtre yan tümcesi olmadan herhangi bir özellikte sorgulayın.

* Bir filtre içermeyen sorgulayın.

Bir veya birden çok bölümdeki verileri okuma sorguları, daha yüksek gecikme uygulanır ve daha yüksek sayıda istek birimleri kullanma. Her bölüm tüm özellikleri için otomatik dizin oluşturma olduğundan, sorgu dizinden verimli bir şekilde sunulabilir. Birden çok bölüm paralellik seçenekleri kullanarak daha hızlı kullanan sorguları yapabilirsiniz. Bölümlendirme ve bölüm anahtarları hakkında daha fazla bilgi için bkz: [Azure Cosmos DB'de bölümleme](partitioning-overview.md).

## <a name="evaluate-request-unit-charge-for-a-query"></a>Bir sorgu için istek birimi ücretine ek olarak değerlendir

Azure Cosmos kapsayıcılarınızı veya depolanan bazı verileri bir kez oluşturun ve, sorguları çalıştırmak için Azure portalında Veri Gezgini'ni kullanabilirsiniz. Veri Gezgini'ni kullanarak, sorguları maliyetini de alabilirsiniz. Bu yöntem gerçek ücretleri tipik sorgular ve sisteminizin desteklediği işlemleri ile ilgili bir fikir verir.

SDK'ları kullanarak sorguları maliyetini programlı olarak da edinebilirsiniz. Herhangi bir işlem yükü ölçmek için gibi oluştururken, güncelleştirme veya silme İnceleme `x-ms-request-charge` REST API'si kullanılırken başlığı. .NET veya Java SDK'sı kullanıyorsanız `RequestCharge` isteği ücret alınacak eşdeğer özelliğin bir özelliktir ve bu özellik ResourceResponse veya FeedResponse içinde yok.

```csharp
// Measure the performance (request units) of writes 
ResourceResponse<Document> response = await client.CreateDocumentAsync(collectionSelfLink, myDocument); 

Console.WriteLine("Insert of an item consumed {0} request units", response.RequestCharge); 

// Measure the performance (request units) of queries 
IDocumentQuery<dynamic> queryable = client.CreateDocumentQuery(collectionSelfLink, queryString).AsDocumentQuery(); 

while (queryable.HasMoreResults) 
     { 
          FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>(); 
          Console.WriteLine("Query batch consumed {0} request units", queryResponse.RequestCharge); 
     }
```

## <a name="factors-influencing-request-unit-charge-for-a-query"></a>Bir sorgu için istek birimi ücreti etkileyen faktörler

Sorgular için istek birimleri, bir dizi faktöre bağlıdır. Örneğin, Azure Cosmos öğe sayısını yüklendi/döndürülen, arama ve dizin sorgu derleme karşı sayısı vb. ayrıntıları zaman. Azure Cosmos DB, aynı veri yürütüldüğünde aynı sorgu her zaman istek birimleri bile tekrar yürütme ile aynı sayıda tüketecektir garanti eder. Sorgu yürütme ölçümleri kullanarak sorguyu profili istek birimleri nasıl harcanan bir iyi fikir verir.  

Bazı durumlarda, 200 ve 429 yanıtları ve değişken istek birimleri cinsinden sorguları kullanılabilir RU'ları üzerinde temel mümkün olduğunca hızlı çalışır çünkü Disk bellekli bir yürütme sorgu, bir dizi görebilirsiniz. Birden çok birden/sunucu ve istemci arasındaki gelişlerin yuvarlak bir sorgu yürütme görebilirsiniz. Örneğin, 10.000 öğeleri döndürülmesi birden çok sayfa her sayfada gerçekleştirilen hesaplama göre ücretlendirilir. Bu sayfada topladığımızda için sorgunun tamamını elde edebileceğiniz gibi aynı sayıda RU almanız gerekir.  

## <a name="metrics-for-troubleshooting"></a>Sorun giderme için ölçümleri

Performans ve kullanıcı tanımlı işlevler (UDF'ler) sorgular tarafından genellikle kullanılan aktarım hızı, işlev gövdesinde bağlıdır. Sorgu yürütme UDF ve tüketilen, RU sayısını harcandığını ne kadar süre öğrenmek için en kolay yolu olan sorgu ölçümlerini etkinleştirerek. .NET SDK'sı kullanıyorsanız, SDK tarafından döndürülen örnek sorgu ölçümler şunlardır:

```bash
Retrieved Document Count                 :               1              
Retrieved Document Size                  :           9,963 bytes        
Output Document Count                    :               1              
Output Document Size                     :          10,012 bytes        
Index Utilization                        :          100.00 %            
Total Query Execution Time               :            0.48 milliseconds 
  Query Preparation Times 
    Query Compilation Time               :            0.07 milliseconds 
    Logical Plan Build Time              :            0.03 milliseconds 
    Physical Plan Build Time             :            0.05 milliseconds 
    Query Optimization Time              :            0.00 milliseconds 
  Index Lookup Time                      :            0.06 milliseconds 
  Document Load Time                     :            0.03 milliseconds 
  Runtime Execution Times 
    Query Engine Execution Time          :            0.03 milliseconds 
    System Function Execution Time       :            0.00 milliseconds 
    User-defined Function Execution Time :            0.00 milliseconds 
  Document Write Time                    :            0.00 milliseconds 
  Client Side Metrics 
    Retry Count                          :               1              
    Request Charge                       :            3.19 RUs  
```

## <a name="best-practices-to-cost-optimize-queries"></a>Sorguları en iyi yöntemler maliyetini en iyi duruma getirme 

Aşağıdaki en iyi maliyet sorgularında iyileştirilmesi sırasında göz önünde bulundurun:

* **Birden çok varlık türleri birlikte bulundurma**

   Kapsayıcıları tek ya da daha küçük bir dizi içinde birden çok varlık türleri birlikte bulundurmanıza olanak deneyin. Bu yöntemin avantajları yalnızca fiyatlandırma açısından, aynı zamanda sorgu yürütme ve işlemler için verir. Sorgular tek bir kapsayıcıya kapsamına eklenir; ve saklı yordamlar/Tetikleyicileri aracılığıyla birden çok kayıt üzerinden atomik işlemler tek bir kapsayıcıdaki bir bölüm anahtarı kapsamına eklenir. Aynı kapsayıcı içindeki varlıklar birlikte bulundurma ağ sayısını azaltabilirsiniz kayıtlarda ilişkilerini çözmek için gidiş dönüş. Böylece uçtan uca performansını artırır, daha büyük bir veri kümesi için birden çok kayıt atomik işlemler sağlar ve sonuç olarak maliyetlerini düşürür. Kapsayıcıları tek ya da daha küçük bir dizi içinde birden çok varlık türleri birlikte bulundurma genellikle var olan bir uygulamayı geçiş yaptığınız ve - herhangi bir kod değişikliği yapmak istiyor musunuz senaryonuz için zor olduğundan, ardından sağlama düşünmelisiniz veritabanı düzeyinde aktarım hızı.  

* **Ölçün ve için alt istek birimi/saniye kullanım ayarlayın.**

   Kaç tane istek birimi (RU) bir işlem için kullanılan bir sorgu karmaşıklığı etkiler. Koşullar, koşullarına, UDF'ler sayısı ve boyutu kaynak veri kümesi yapısı sayısı. Tüm bu faktörler sorgu işlemlerinin maliyetini etkiler. 

   İstek üstbilgisinde döndürülen istek yükü, belirli bir sorgu maliyetini gösterir. Örneğin, bir sorgu 1000 1 KB'lık öğeleri döndürürse, işlemin maliyeti 1000'dir. Bu nedenle, bir saniye içinde sonraki istekleri hız sınırı önce yalnızca iki tür isteklere sunucunun geliştirir. Daha fazla bilgi için [istek birimi](request-units.md) makale ve istek birimi hesaplayıcı. 

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde Azure Cosmos DB'de maliyet iyileştirmesi hakkında daha fazla bilgi edinmek için sonraki geçebilirsiniz:

* Daha fazla bilgi edinin [nasıl çalıştığını fiyatlandırma Azure Cosmos](how-pricing-works.md)
* Daha fazla bilgi edinin [en iyi duruma getirme için geliştirme ve test etme](optimize-dev-test.md)
* Daha fazla bilgi edinin [Azure Cosmos DB faturanızı anlama](understand-your-bill.md)
* Daha fazla bilgi edinin [aktarım hızı maliyeti en iyi duruma getirme](optimize-cost-throughput.md)
* Daha fazla bilgi edinin [depolama maliyetini en iyi duruma getirme](optimize-cost-storage.md)
* Daha fazla bilgi edinin [okuma ve yazma işlemleri maliyetini en iyi duruma getirme](optimize-cost-reads-writes.md)
* Daha fazla bilgi edinin [çok bölgeli Azure Cosmos hesapları maliyetini en iyi duruma getirme](optimize-cost-regions.md)
* Daha fazla bilgi edinin [Azure Cosmos DB ayrılan kapasite](cosmos-db-reserved-capacity.md)

