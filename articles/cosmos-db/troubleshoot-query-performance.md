---
title: Tanılama ve Azure Cosmos DB kullanırken sorgu sorunlarını giderme
description: Tanımlamak, tanılamak ve Azure Cosmos DB SQL sorgu sorunlarını giderme hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: troubleshooting
ms.date: 07/10/2019
ms.author: girobins
ms.subservice: cosmosdb-sql
ms.reviewer: sngun
ms.openlocfilehash: 079e8677febfe6683d4f0e60a0e7ba6b06ea549d
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67835828"
---
# <a name="troubleshoot-query-performance-for-azure-cosmos-db"></a>Azure Cosmos DB için sorgu performansı sorunlarını giderme
Bu makalede tanımlamak, tanılamak ve Azure Cosmos DB SQL sorgu sorunlarını giderme konusunda anlatılmaktadır. Azure Cosmos DB sorgular için en iyi performans elde etmek için aşağıdaki sorun giderme adımlarını izleyin. 

## <a name="collocate-clients-in-same-azure-region"></a>İstemcilerin aynı Azure bölgesinde ISS'de 
En düşük gecikmeyi çağıran uygulama için aynı Azure bölgesindeki sağlanan Azure Cosmos DB uç nokta olarak bulunduğu sağlayarak gerçekleştirilir. Kullanılabilir bölgelerin listesi için bkz. [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/#services) makalesi.

## <a name="check-consistency-level"></a>Tutarlılık düzeyi denetimi
[Tutarlılık düzeyi](consistency-levels.md) performans ve ücretleri etkileyebilir. Tutarlılık düzeyi, verilen senaryo için uygun olduğundan emin olun. Daha fazla ayrıntı için bkz: [tutarlılık düzeyi seçme](consistency-levels-choosing.md).

## <a name="log-query-metrics"></a>Sorgu ölçümleri günlüğe kaydedin
Kullanım `QueryMetrics` yavaş veya pahalı sorgu giderilir. 

  * Ayarlama `FeedOptions.PopulateQueryMetrics = true` olmasını `QueryMetrics` yanıt.
  * `QueryMetrics` sınıfında aşırı yüklenmiş bir `.ToString()` dize gösterimini almak için çağrılan işlev `QueryMetrics`. 
  * Ölçümleri, diğerlerinin yanında aşağıdaki Öngörüler türetmek için kullanılabilir: 
  
      * Olup herhangi belirli bir sorgu işlem hattındaki bileşeninin (yüzlerce milisaniye ya da daha fazla sırayla) tamamlamak için anormal olarak uzun sürdü. 

          * Bakmak `TotalExecutionTime`.
          * Varsa `TotalExecutionTime` sorgusunu uçtan uca yürütme süresi'dan küçük ve istemci tarafı veya ağda zaman harcandığını. İki Azure bölgesi ve istemci birlikte bulunan denetleyin.
      
      * (Çıkış belge sayısı alınan belge sayısından daha az ise) belgeleri hatalı pozitif sonuçları gerçekleştirildiğini analiz.  

          * Bakmak `Index Utilization`.
          * `Index Utilization` = (Döndürülen belgeler sayısı / yüklenen sayısı belgeler)
          * Döndürülen belge sayısı yüklenen sayıdan daha az ise, hatalı pozitif sonuçları çözümlenmekte.
          * Dar filtrelerle alınmasını belge sayısını sınırlayın.  

      * Tek gidiş dönüş fared (bkz `Partition Execution Timeline` gelen dize gösterimini `QueryMetrics`). 
      * Olup sorgu yüksek istek yükü kullanılan. 

Daha fazla ayrıntı için bkz: [SQL sorgusu yürütme ölçümlerini edinme](profile-sql-api-query.md) makalesi.
      
## <a name="tune-query-feed-options-parameters"></a>Sorgu akış seçenekleri parametrelerini ayarlama 
Sorgu performansı isteğin öne [akış seçenekleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.feedoptions?view=azure-dotnet) parametreleri. Ayar deneyin seçenekleri aşağıda:

  * Ayarlama `MaxDegreeOfParallelism` -1 ilk ve ardından farklı değerler arasında karşılaştırma performans. 
  * Ayarlama `MaxBufferedItemCount` -1 ilk ve ardından farklı değerler arasında karşılaştırma performans. 
  * Ayarlama `MaxItemCount` -1.

Farklı değerler performansını karşılaştırılırken, 2, 4, 8, 16 ve diğerleri gibi değerler deneyin.
 
## <a name="read-all-results-from-continuations"></a>Tüm sonuçları gelen devamlılığını okuyun
Tüm sonuçları gitmiyor düşünüyorsanız, tam olarak devamlılığın boşaltma emin olun. Diğer bir deyişle, devamlılık belirteci elde etmek üzere daha fazla belge sahipken sonuçları okuma tutun.

Tam olarak boşaltma aşağıdaki desenlerden birini kullanarak gerçekleştirilebilir:

  * Devamlılık boş durumdayken işleme sonuçlarını devam edin.
  * Sorgu daha fazla sonuç varken işleme devam edin. 

    ```csharp
    // using AsDocumentQuery you get access to whether or not the query HasMoreResults
    // If it does, just call ExecuteNextAsync until there are no more results
    // No need to supply a continuation token here as the server keeps track of progress
    var query = client.CreateDocumentQuery<Family>(collectionLink, options).AsDocumentQuery();
    while (query.HasMoreResults)
    {
        foreach (Family family in await query.ExecuteNextAsync())
        {
            families.Add(family);
        }
    }
    ```

## <a name="choose-system-functions-that-utilize-index"></a>Dizin kullanan sistem işlevlerini seçin
İfade bir dize değerleri aralığına çevrilebilir, dizinin kullanabilir; Aksi takdirde, işlem gerçekleştirilemiyor. 

Dizinden yararlanabilecek dize işlevleri listesi aşağıda verilmiştir: 
    
  * STARTSWITH (str_expr, str_expr) 
  * Sol (str_expr, num_expr) str_expr = 
  * (Str_expr, num_expr, num_expr) alt DİZENİN ilk num_expr 0 ise, ancak yalnızca str_expr = 
    
    Birkaç sorgu örnekleri şunlardır: 
    
    ```sql

    -- If there is a range index on r.name, STARTSWITH will utilize the index while ENDSWITH won't 
    SELECT * 
    FROM c 
    WHERE STARTSWITH(c.name, 'J') AND ENDSWITH(c.name, 'n')

    ```
    
    ```sql

    -- LEFT will utilize the index while RIGHT won't 
    SELECT * 
    FROM c 
    WHERE LEFT(c.name, 2) = 'Jo' AND RIGHT(c.name, 2) = 'hn'

    ```

  * Sistem işlevleri filtredeki kaçının (veya WHERE yan tümcesi) dizin tarafından sunulan değil. Bazı örnekler gibi sistem işlevlerini içerir, üst, alt içerir.
  * Mümkün olduğunda sorguları bölüm anahtarı filtrelemesi kullanacak şekilde yazın.
  * Yüksek performanslı elde etmek için büyük/küçük filtrede çağırma sorgular kullanmayın. Bunun yerine, değerler ekleme sırasında büyük/küçük harfleri normalleştirmek. Değerlerin her biri için istenen büyük/küçük harf değeri eklemek veya istenen büyük/küçük harf değeri ve özgün değeri ekleyin. 

    Örneğin:
    
    ```sql

    SELECT * FROM c WHERE UPPER(c.name) = "JOE"

    ```
    
    Bu durum için deposu "ALİ" büyük harfli veya "ALi" özgün değeri hem de "ALİ" depolayın. 
    
    JSON veri büyük/küçük harf normalleştirilmiş sorgu olur:
    
    ```sql

    SELECT * FROM c WHERE c.name = "JOE"

    ```

    İkinci sorgu, dönüştürmeler her değeri "ALİ" için değerleri karşılaştırmak için gerçekleştirme gerektirmez daha fazla performansa sahip olacaktır.

Daha fazla sistem işlevi için bilgi [sistem işlevlerini](sql-query-system-functions.md) makalesi.

## <a name="check-indexing-policy"></a>Dizin oluşturma ilkesini denetleme
Doğrulamak için geçerli [dizin oluşturma ilkesi](index-policy.md) idealdir:

  * Sorgularda kullanılan tüm JSON yolları daha hızlı okuma için dizin oluşturma İlkesi'nde dahil olduklarından emin olun.
  * Daha fazla yüksek performanslı yazma işlemleri için sorgularda kullanılmaz yolları hariç tut.

Daha fazla ayrıntı için bkz: [nasıl yönetme dizin oluşturma ilkesi](how-to-manage-indexing-policy.md) makalesi.

## <a name="spatial-data-check-ordering-of-points"></a>Uzamsal veriler: Kontrol noktalarını sıralama
İçinde bir Çokgen noktalarının saat yönünün tersi düzende belirtilmesi gerekir. Bir çokgenin belirtilen saat yönünde sırayla bölgesinde tersini temsil eder.

## <a name="optimize-join-expressions"></a>Birleştirme ifadeleri en iyi duruma getirme
`JOIN` ifadeler, büyük arası ürünlere kaymasını genişletebilirsiniz. Ne zaman mümkün olan daha küçük bir arama sorgusunu daha dar bir filtre boşluk.

Birden çok değerli alt sorgularda en iyi duruma getirebilir `JOIN` doğrulamaları her seçin-çok ifade sonra yerine tüm çapraz birleşimlerde sonra gönderme by ifadeleri `WHERE` yan tümcesi. Ayrıntılı bir örnek için bkz. [birleştirme ifadeleri en iyi duruma getirme](https://docs.microsoft.com/azure/cosmos-db/sql-query-subquery#optimize-join-expressions) makalesi.

## <a name="optimize-order-by-expressions"></a>ORDER BY ifadeleri en iyi duruma getirme 
`ORDER BY` alanları seyrek veya dizin ilkesinde dahil değil ise, sorgu performansı düşebilir.

  * Zamanı gibi seyrek alanlar için arama alanını mümkün olduğunca filtrelerle azaltın. 
  * Tek bir özellik için `ORDER BY`, dizin ilkesinde özellik içerir. 
  * Birden çok özellik için `ORDER BY` ifadeleri tanımlamak bir [bileşik dizin](https://docs.microsoft.com/azure/cosmos-db/index-policy#composite-indexes) sıralanan alanları.  

## <a name="many-large-documents-being-loaded-and-processed"></a>Yüklenen ve işlenen çok sayıda büyük belgeleri
Saat ve bir sorgu için gerekli RU'ları yalnızca yanıt boyutuna bağlı değildir, ayrıca sorgu işleme işlem hattı tarafından yapılan iş bağımlı oldukları. Saat ve RU, tüm sorgu işleme işlem hattı tarafından yapılan iş miktarı ile orantılı olarak artar. Daha fazla iş büyük dosyalar için gerçekleştirilir, bu nedenle daha fazla zaman ve RU yüklemek ve büyük belgeleri işlemek için gereklidir.

## <a name="low-provisioned-throughput"></a>Sağlanan aktarım hızının düşük olmasını
Sağlanan aktarım hızı, iş yükü işleyebileceğinden emin olun. Etkilenen koleksiyonlar için RU bütçe artırın.

## <a name="try-upgrading-to-the-latest-sdk-version"></a>En son SDK sürümüne yükseltmeyi deneyin
En son SDK'sı bakın belirlemek için [SDK'sını indirin ve sürüm notları](sql-api-sdk-dotnet.md) makalesi.

## <a name="next-steps"></a>Sonraki adımlar
RU sorgu ölçün, sorgularınızı ve daha fazlasını ayarlamanıza olanak tanıyan bir yürütme istatistikleri alma konusunda belgelere bakın:

* [.NET SDK kullanarak SQL sorgusu yürütme ölçümleri alma](profile-sql-api-query.md)
* [Azure Cosmos DB ile sorgu performansını ayarlama](sql-api-sql-query-metrics.md)
* [.NET SDK'sı için performans ipuçları](performance-tips.md)