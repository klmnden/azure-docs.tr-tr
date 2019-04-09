---
title: Azure Cosmos DB dizinleme ilkeleri
description: Azure Cosmos DB'de dizinleme nasıl çalıştığını anlayın. Yapılandırma ve otomatik dizin oluşturma ve daha yüksek performans için dizin oluşturma ilkesini değiştirme hakkında bilgi edinin.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: rimman
ms.openlocfilehash: 6998db1679e67f8ac4bf7c81ea9373c66a9618ee
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59278573"
---
# <a name="index-policy-in-azure-cosmos-db"></a>Azure Cosmos DB'de dizin İlkesi

Varsayılan dizinleme ilkesinin bir Azure Cosmos kapsayıcısı üzerinde aşağıdaki parametreleri yapılandırarak geçersiz kılabilirsiniz:

* **Dahil edilecek veya dizinden öğeleri ve yolları hariç**: Hariç tutma veya eklediğinizde veya bir kapsayıcı içindeki öğeleri değiştirin, dizinde belirli öğeler içerir. Ayrıca dahil edebilir veya kapsayıcıda dizine belirli yollar/özellikleri hariç. Yolları içerebilir joker karakter düzenleri, örneğin, *.

* **Dizin türleri yapılandırma**: Buna ek olarak dizinlenmiş yolları aralığı için dizinler, diğer türleri gibi ekleyebileceğiniz uzamsal.

* **Dizin modu yapılandırma**: Bir kapsayıcı dizin oluşturma ilkesini kullanarak, farklı bir dizin oluşturma modu gibi yapılandırabileceğiniz *Consistent* veya *hiçbiri*.

## <a name="indexing-modes"></a>Dizin oluşturma modları

Azure Cosmos DB, dizin oluşturma ilkesi aracılığıyla bir Azure Cosmos kapsayıcısı üzerinde yapılandırabileceğiniz iki dizin oluşturma modunu destekler:

* **Tutarlı**: Bir Azure Cosmos kapsayıcının İlkesi ayarlanırsa *Consistent*, belirli bir kapsayıcı sorgulamaları noktası okuma için belirtmiş aynı tutarlılık düzeyi izleyin (örneğin, güçlü, bağımlı eskime, oturum veya son). 

  Dizinin öğeleri güncelleştirme zaman uyumlu olarak güncelleştirilir. Örneğin, INSERT, Değiştir, güncelleştirme ve silme işlemleri bir öğe üzerinde dizin güncelleştirme neden olur. Tutarlı dizin oluşturma, yazma üretimi etkilemeden tutarlı sorguları destekler. Dizine dahil "yolları" yazma üretimi azalma bağlıdır ve "tutarlılık düzeyi." Tutarlı bir dizin oluşturma modu dizin tüm güncelleştirmeleri ile güncel tutmak ve hemen sorgular sunmak için tasarlanmıştır.

* **Hiçbiri**: None kendisiyle ilişkilendirilmiş hiçbir dizin dizin moduna sahip olan bir kapsayıcı. Bu, Azure Cosmos veritabanı, bir anahtar-değer depolama alanı olarak kullanılır ve yalnızca kimliği özelliği tarafından erişilen öğeleri yaygın olarak kullanılır.

  > [!NOTE]
  > Dizin oluşturma modu olarak yapılandırma bir *hiçbiri* mevcut tüm dizinleri bırakmayı yan etkisi vardır. Erişim desenleri ID gerektir ya da yalnızca kendi bağlantı varsa bu seçeneği kullanmanız gerekir.

Sorgu tutarlılık düzeyleri benzer normal okuma işlemleri için korunur. Azure Cosmos veritabanı, sahip kapsayıcı sorgularsanız hata döndürür bir *hiçbiri* dizin oluşturma modu. Açık aracılığıyla taramaları olarak sorguları yürütebilir **x-ms-documentdb-enable-tarama** üst bilgisinde REST API veya **EnableScanInQuery** seçeneği .NET SDK kullanarak istek. ORDER BY şu anda desteklenmiyor ile gibi bazı sorgu özellikleri **EnableScanInQuery**, bunlar karşılık gelen bir dizin zorunlu kılabilir.

## <a name="modifying-the-indexing-policy"></a>Dizin oluşturma ilkesini değiştirme

Azure Cosmos DB'de dizin oluşturma ilkesini bir kapsayıcının herhangi bir zamanda güncelleştirebilirsiniz. Bir dizin oluşturma ilkesini bir Azure Cosmos kapsayıcısı üzerinde değişiklik dizini şeklinde bir değişiklik neden olabilir. Bu değişiklik, sıralanabilir yolları, kendi duyarlık ve dizin tutarlılık modelini etkiler. Dizin oluşturma ilkesi etkili bir şekilde bir değişiklik bir dönüşümünü eski dizinin yeni bir dizin gerektirir.

### <a name="index-transformations"></a>Dizin dönüşümleri

Tüm dizin dönüştürmeleri çevrimiçi gerçekleştirilir. Dizin eski ilkeyi öğeleri yazma kullanılabilirliği veya kapsayıcıdaki sağlanmış olan aktarım hızı etkilemeden yeni ilke verimli bir şekilde dönüştürülür. Tutarlılığını okuma ve yazma REST API, SDK'ları, veya kullanılarak gerçekleştirilen işlemleri saklı yordamları ve Tetikleyicileri dizin dönüştürme sırasında etkilenmez.

Dizin oluşturma ilkesini değiştirme zaman uyumsuz bir işlemdir ve işlemi tamamlamak için geçen süre öğeleri, sağlanan aktarım hızı ve öğelerin boyutunu sayısına bağlıdır. Yeniden dizin oluşturma işlemi devam ederken değiştirilmekte olan dizin kullanmak için sorguları görülüyorsa sorgunuzu tüm eşleşen sonuçları döndürmeyebilir ve sorgular, herhangi bir hata/hata döndürmez. Yeniden dizin oluşturma işlemi devam ederken, sorguları bağımsız olarak dizin oluşturma modu yapılandırma son tutarlılık sağlar. Dizin sonra dönüştürme tamamlandıktan, tutarlı sonuçlar görmeye devam edecektir. Bu arabirimler gibi REST API, SDK'ları, veya saklı yordamları ve Tetikleyicileri tarafından verilen sorguları için geçerlidir. Dizin dönüştürme, zaman uyumsuz olarak belirli çoğaltmalar için kullanılabilir yedek kaynakları kullanarak çoğaltmalarındaki arka planda gerçekleştirilir.

Tüm dizin dönüştürmeleri yerinde yapılır. Azure Cosmos DB, dizin iki kopyasını tutmaz. Bu nedenle hiçbir ek disk alanı gerekli veya dizin dönüştürme gerçekleşirken, kapsayıcılarda kullanılan.

Dizin oluşturma ilkesini değiştirdiğinizde değişiklikler yeni dizine eski dizinden taşımak için uygulanır ve dizin oluşturma modu yapılandırmalarında öncelikle temel alır. Dizin oluşturma modu yapılandırmaları, dahil edilen/Dışlanan yollar, dizin türü ve duyarlık gibi diğer özellikleri karşılaştırıldığında önemli bir rol oynar.

Eski ve yeni ilkeleri kullanım dizin **Consistent** dizin oluşturma, Azure Cosmos veritabanı bir çevrimiçi dizin dönüşümü gerçekleştirir. Dönüştürme işlemi devam ederken, tutarlı bir dizin oluşturma moduna sahip başka bir dizin oluşturma ilkesi değişikliği uygulanamıyor. Dizin, dizin oluşturma modu None taşıdığınızda, hemen bırakılır. Hiçbiri taşıma, devam eden dönüştürme iptal edin ve farklı bir dizin oluşturma ilkesi ile yeni başlangıç istediğinizde yararlıdır.

## <a name="modifying-the-indexing-policy---examples"></a>Dizin oluşturma ilkesi - örnekler değiştirme

Bir dizin oluşturma ilkesini güncelleştirmek istediğiniz zaman en yaygın kullanım örnekleri şunlardır:

* Normal işlem sırasında tutarlı sonuçlar için ancak geri döner istiyorsanız **hiçbiri** dizin oluşturma modunda toplu veri alır.

* Geçerli Azure Cosmos kapsayıcılarınızı dizin oluşturma özellikleri kullanmaya başlamak istiyorsanız. Örneğin, Jeo-uzamsal sorgulama, uzamsal dizin türü veya ORDER BY gerektiren kullanma / dizesi aralık dizin türü gerektiren aralık sorguları dize.

* El ile istediğiniz dizine ve bunları iş yüklerinize ayarlamak için zaman içinde değiştirmek için Özellikler'i seçin.

* Sorgu performansını artırmak için veya tüketilen depolama alanı azaltmak için dizin oluşturma duyarlılık ayarlamak istiyorsanız.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler de dizinleme hakkında daha fazla bilgi edinin:

* [Dizin oluşturma genel bakış](index-overview.md)
* [Dizin türleri](index-types.md)
* [Dizin yolları](index-paths.md)
* [Dizin oluşturma ilkesini yönetme](how-to-manage-indexing-policy.md)
