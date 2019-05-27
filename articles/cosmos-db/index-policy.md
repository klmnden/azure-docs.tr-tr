---
title: Azure Cosmos DB dizinleme ilkeleri
description: Yapılandırma ve varsayılan dizinleme ilkesinin için otomatik dizin oluşturma ve Azure Cosmos DB'de daha yüksek performans değiştirme hakkında bilgi edinin.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: thweiss
ms.openlocfilehash: c45beb3ed6f87e95d171e2299c533b4be2827f27
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65954044"
---
# <a name="indexing-policies-in-azure-cosmos-db"></a>Azure Cosmos DB'de dizinleme ilkeleri

Azure Cosmos DB'de her kapsayıcı, kapsayıcının öğelerini nasıl sıralanması gerektiğini belirleyen bir dizin oluşturma ilkesi vardır. Varsayılan dizinleme ilkesinin için yeni kapsayıcılar dizinleri her özelliği için herhangi bir dize veya sayı aralığı dizinleri zorlamayı her öğesi, oluşturulan ve herhangi bir GeoJSON nesne için uzamsal dizin noktası yazın. Bu, dizin oluşturma ve önceden dizin yönetimi hakkında düşünmek zorunda kalmadan yüksek sorgu performansı elde etmek sağlar.

Bazı durumlarda gereksinimlerinize daha iyi uyacak şekilde otomatik bu davranışı geçersiz kılmak isteyebilirsiniz. Bir kapsayıcının dizin oluşturma ilkesini ayarlayarak özelleştirebileceğiniz kendi *dizin oluşturma modu*ve dahil edilecek veya hariç *özellik yolları*.

## <a name="indexing-mode"></a>Dizin oluşturma modu

Azure Cosmos DB iki dizin oluşturma modunu destekler:

- **Tutarlı**: Bir kapsayıcının dizin oluşturma ilkesi için Consistent ayarlarsanız, oluşturmak, güncelleştirmek veya öğeleri silme gibi dizin zaman uyumlu olarak güncelleştirilir. Bu tutarlılık okuma sorgularınızın anlamına gelir [hesabı için yapılandırılan tutarlılık](consistency-levels.md).

- **Hiçbiri**: Bir kapsayıcının dizin oluşturma ilkesini hiçbiri olarak ayarlandı, dizin oluşturma etkin, kapsayıcıdaki devre dışı bırakılır. Bir kapsayıcı olarak saf bir anahtar-değer deposu ikincil dizinleri gerek kalmadan kullanıldığında, bu yaygın olarak kullanılır. Toplu ekleme işlemlerinin hızlandırma da yardımcı olabilir.

## <a name="including-and-excluding-property-paths"></a>Dahil etme ve dışlama özellik yolları

Özel bir dizin oluşturma ilkesini açıkça dahil veya dizine elmadan hariç özelliği yol belirtebilirsiniz. Dizine alınmış yollarının sayısını en iyi duruma, kapsayıcı tarafından kullanılan depolama miktarını azaltın ve yazma işlemlerinin gecikme süresini iyileştirip. Bu yolları aşağıdaki tanımlanan [dizinleme genel bakış bölümünde açıklanan yöntemi](index-overview.md#from-trees-to-property-paths) aşağıdaki eklemelerle:

- skaler bir değer (dize veya sayı) giden yol şununla biter `/?`
- bir diziden öğeleri açıklanmıştır birlikte aracılığıyla `/[]` gösterimi (yerine `/0`, `/1` vs.)
- `/*` düğümü altındaki tüm öğeleri eşleştirmek için joker karakter kullanılabilir

Aynı örneği yeniden alma:

    {
        "locations": [
            { "country": "Germany", "city": "Berlin" },
            { "country": "France", "city": "Paris" }
        ],
        "headquarters": { "country": "Belgium", "employees": 250 }
        "exports": [
            { "city": "Moscow" },
            { "city": "Athens" }
        ]
    }

- `headquarters`'s `employees` yolu `/headquarters/employees/?`
- `locations`' `country` yolu `/locations/[]/country/?`
- yolu herhangi bir şey `headquarters` olduğu `/headquarters/*`

Bir yol açıkça dizin oluşturma İlkesi'nde dahil edilirse, ayrıca hangi dizin türleri yol ve her dizin türü, bu dizin için geçerli veri türü için uygulanması tanımlamak vardır:

| Dizin türü | İzin verilen hedef veri türleri |
| --- | --- |
| Aralık | Dize veya sayı |
| Uzamsal | Point, LineString veya Çokgen |

Örneğin, biz içerebilir `/headquarters/employees/?` yolu belirleyen bir `Range` dizin uygulanması bu yolda hem `String` ve `Number` değerleri.

### <a name="includeexclude-strategy"></a>Stratejisi Ekle/Dışla

Tüm dizin oluşturma ilkesini kök yolunu içermek zorundadır `/*` bir dahil veya hariç tutulan bir yolu olarak.

- Seçmeli olarak sıralanması gerekmez yolları hariç tutmak için kök yolu içerir. Azure Cosmos DB proaktif olarak modelinize eklediğiniz herhangi bir yeni özelliği dizin sağlayan gibi önerilen yaklaşımdır.
- Seçmeli olarak sıralanması gereken yolları dahil etmek için kök yolu hariç tutun.

- İçeren normal karakterler içeren yolların: alfasayısal karakterler ve _ (alt çizgi), çift tırnak (örneğin, "/ path /?") etrafında yolu dizeyi atlatmaya zorunda değilsiniz. Diğer özel karakterleri içeren yolları için çift tırnak işareti geçici yol dizesi atlamanız gerekir (örneğin, "/\"yolu abc\"/?"). Özel karakterler yolunuzda bekliyorsanız, güvenliği her yolun çizgilerden kaçınabilirsiniz. İşlevsel olarak her yolun Vs özel karakterlere sahip olanları kaçış olmadığını fark yapmaz.

Bkz: [Bu bölümde](how-to-manage-indexing-policy.md#indexing-policy-examples) ilkesi örnekleri dizinini oluşturmak için.

## <a name="composite-indexes"></a>Bileşik dizinler

Bu sorgular `ORDER BY` iki veya daha fazla özellik bir bileşik dizin gerektirir. Bileşik dizinler çoklu tarafından yalnızca şu anda kullanıldığından `ORDER BY` sorgular. Yapmanız gerekenler bu nedenle varsayılan olarak, bileşik dizin tanımlanan [Bileşik dizinler eklemek](how-to-manage-indexing-policy.md#composite-indexing-policy-examples) gerektiğinde.

Bir bileşik dizin tanımlarken aşağıdakileri belirtmeniz gerekir:

- İki veya daha fazla özellik yolu. Önemli olan konuya hangi özelliğinde yollardır sırası tanımlı.
- (Azalan veya artan) sırası.

Bileşik dizinler kullanılırken aşağıdaki önemli noktalar kullanılır:

- Bileşik dizin yolları ORDER BY yan tümcesindeki özellikler dizisini eşleşmiyorsa, bileşik dizin sorgu ardından desteklemez

- Bileşik dizin yolları (azalan veya artan) sırasını da ORDER BY yan tümcesi sırayla eşleşmelidir.

- Bileşik dizin bir ORDER BY tümcesi ters sırada olan tüm yollarında da destekler.

Aşağıdaki örnek bir bileşik dizin özelliklerini tanımlanan yere göz önünde bulundurun b ve c:

| **Bileşik dizin**     | **Örnek `ORDER BY` sorgu**      | **Dizin tarafından destekleniyor mu?** |
| ----------------------- | -------------------------------- | -------------- |
| ```(a asc, b asc)```         | ```ORDER BY  a asc, bcasc```        | ```Yes```            |
| ```(a asc, b asc)```          | ```ORDER BY  b asc, a asc```        | ```No```             |
| ```(a asc, b asc)```          | ```ORDER BY  a desc, b desc```      | ```Yes```            |
| ```(a asc, b asc)```          | ```ORDER BY  a asc, b desc```       | ```No```             |
| ```(a asc, b asc, c asc)``` | ```ORDER BY  a asc, b asc, c asc``` | ```Yes```            |
| ```(a asc, b asc, c asc)``` | ```ORDER BY  a asc, b asc```        | ```No```            |

Tüm gerekli sunabilmeniz için dizin oluşturma ilkenizi özelleştirmelisiniz `ORDER BY` sorgular.

## <a name="modifying-the-indexing-policy"></a>Dizin oluşturma ilkesini değiştirme

Bir kapsayıcının dizin oluşturma ilkesini dilediğiniz zaman güncelleştirilebilir [Azure portalı veya desteklenen Sdk'lardan birini kullanarak](how-to-manage-indexing-policy.md). Eski dizinden bir dönüştürme (ek depolama alanı işlemi sırasında kullanılan şekilde) çevrimiçi ortamda ve yerinde gerçekleştirilen yeni bir dizin oluşturma ilkesini güncelleştirme tetikler. Eski ilkenin dizini yazma kullanılabilirliği veya kapsayıcıdaki sağlanmış olan aktarım hızı etkilemeden Yeni ilkeye verimli bir şekilde dönüştürülür. Dizin dönüştürme zaman uyumsuz bir işlemdir ve tamamlamak için gereken süreyi sağlanan aktarım hızı, öğe sayısı ve boyutuna bağlıdır. 

> [!NOTE]
> Yeniden dizin oluşturma işlemi devam ederken sorgular eşleşen sonuçları döndürmeyebilir ve herhangi bir hata döndürüyor olmadan bunu yapar. Bu dizin dönüştürme tamamlanana kadar sorgu sonuçları tutarlı olmayabileceği anlamına gelir. Dizin dönüştürme ilerlemesini izlemek mümkündür [Sdk'lardan birini kullanarak](how-to-manage-indexing-policy.md).

Yeni dizin ilkenin modu için Consistent ayarlarsanız, dizin dönüştürme işlemi sürerken başka bir dizin oluşturma ilkesi değişikliğini uygulanabilir. Çalışan bir dizine dönüştürme dizinleme ilkesinin modu (Bu dizin hemen kaldıracağız) hiçbiri olarak ayarlayarak iptal edilebilir.

## <a name="indexing-policies-and-ttl"></a>Dizin oluşturma ilkeleri ve TTL

[Özelliği için-yaşam süresi (TTL)](time-to-live.md) , açık kapsayıcı üzerindeki etkin olması için dizin oluşturmayı gerektirir. Bunun anlamı:

- Dizin oluşturma modu None olarak ayarlandığı bir kapsayıcı TTL etkinleştirmek mümkün değildir,
- TTL etkinleştirdiğiniz yok bir kapsayıcı için dizin oluşturma modu ayarlamak mümkün değildir.

Burada listelenecek hiçbir özellik yolu gerekiyor, ancak TTL gereklidir senaryoları için bir dizin oluşturma ilkesi ile kullanabilirsiniz:

- bir dizin oluşturma modu için Consistent, ve
- dahil edilen yol, ve
- `/*` yalnızca hariç tutulan yolu.

## <a name="obsolete-attributes"></a>Artık kullanılmayan öznitelikleri

Dizin oluşturma ilkeleri ile çalışırken, artık kullanım dışıdır aşağıdaki öznitelikleri karşılaşabilirsiniz:

- `automatic` Bir Boole değeri bir dizin oluşturma ilkesini kökünde tanımlanır. Artık yok sayılır ve ayarlanabilir `true`, kullanmakta olduğunuz aracı gerektirdiğinde.
- `precision` bir sayı içerdiği yolları için dizin düzeyinde tanımlanır. Artık yok sayılır ve ayarlanabilir `-1`, kullanmakta olduğunuz aracı gerektirdiğinde.
- `hash` artık aralığı türü tarafından değiştirilen bir dizin türüdür.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler de dizinleme hakkında daha fazla bilgi edinin:

- [Dizin oluşturma genel bakış](index-overview.md)
- [Dizin oluşturma ilkesini yönetme](how-to-manage-indexing-policy.md)
