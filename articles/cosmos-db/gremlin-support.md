---
title: Azure Cosmos DB Gremlin destek | Microsoft Docs
description: Apache TinkerPop Gremlin dilden hakkında bilgi edinin. Hangi özellikler ve adımları Azure Cosmos DB'de kullanılabilir olduğunu öğrenin
services: cosmos-db
documentationcenter: ''
author: LuisBosquez
manager: jhubbard
editor: ''
tags: ''
ms.assetid: 6016ccba-0fb9-4218-892e-8f32a1bcc590
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 01/02/2018
ms.author: lbosq
ms.openlocfilehash: 453e11c31a01b6ce8e77deda89725ecd53fd2db9
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="azure-cosmos-db-gremlin-graph-support"></a>Azure Cosmos DB Gremlin grafik desteği
Azure Cosmos DB destekleyen [Apache Tinkerpop'ın](http://tinkerpop.apache.org) grafik geçişi dil [Gremlin](http://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps), grafik varlıkları oluşturma ve grafik sorgu işlemleri gerçekleştirmek için bir grafik API'si olan. Grafik varlıkları (köşeleri ve kenarları) oluşturmak, bu varlıkların içinde özelliklerini değiştirmek, sorgular ve çapraz geçişlerine gerçekleştirmek ve varlıkları silmek için Gremlin dil kullanabilirsiniz. 

Azure Cosmos DB Kurumsal kullanıma hazır özellikler grafik veritabanlarına getirir. Bu genel dağıtım, depolama ve işleme, tahmin edilebilir tek basamaklı milisaniyelik gecikme ölçeklendirme bağımsız içerir okuma iki veya daha fazla Azure bölgeleri kapsayıcı veritabanı hesapları için kullanılabilirlik SLA'ları, otomatik dizin. Azure Cosmos DB TinkerPop/Gremlin desteklediğinden, başka bir grafik veritabanı kod değişikliklerini yapmak zorunda kalmadan kullanılarak yazılmış uygulamaları kolayca geçirebilirsiniz. Ayrıca, Gremlin desteği sayesinde, Azure Cosmos DB sorunsuz bir şekilde TinkerPop etkin analitik çerçeveler gibi bütünleşir [Apache Spark GraphX](http://spark.apache.org/graphx/). 

Bu makalede, biz Gremlin hızlı bir kılavuz sağlar ve Gremlin özellikleri ve grafik API'si tarafından desteklenen adımları numaralandırır.

## <a name="gremlin-by-example"></a>Örneğe göre gremlin
Bir örnek grafik nasıl sorguları Gremlin ifade edilebilir anlamak için kullanalım. Aşağıdaki şekilde, kullanıcılar, ilgi alanları ve bir grafik biçiminde aygıtlar hakkındaki verileri yöneten bir iş uygulaması gösterilmiştir.  

![Kişilerin, cihazların ve ilgi gösteren örnek veritabanı](./media/gremlin-support/sample-graph.png) 

Bu grafik (Gremlin içinde "etiketi" olarak adlandırılır) aşağıdaki köşe türleri vardır:

- Kişiler: Grafikte bir kez deneme, Thomas ve Ben üç kişilerin var
- İlgi: Kendi ilgi alanlarına, bu örnekte, futbol oyun
- Aygıtlar: kullanımı cihazları
- İşletim sistemleri: Çalıştıran cihazlar üzerinde sistemleri

Biz aşağıdaki sınır türlerinin/etiketler aracılığıyla bu varlıklar arasındaki ilişkiler temsil eder:

- Bilir: Örneğin, "Thomas deneme bilir"
- : Kişileri ilgi bizim grafikte temsil etmek için örneğin, "Ben futbol ilginizi ilgilenmektedir"
- RunsOS: Dizüstü bilgisayar Windows işletim sistemi çalıştırır.
- Kullanır: bir kişinin hangi cihaz göstermek kullanır. Örneğin, bir kez deneme 77 seri numarasıyla Motorola telefon kullanır.

Bu grafik kullanarak karşı bazı işlemler şimdi çalıştırmak [Gremlin konsol](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console). Tercih ettiğiniz (Java, Node.js, Python veya .NET) platform Gremlin sürücüleri kullanarak bu işlemler gerçekleştirebilir.  Azure Cosmos DB'de desteklenen adresindeki Ara önce sözdizimi hakkında bilgi edinmek için birkaç örnek bakalım.

İlk CRUD bakalım. Aşağıdaki Gremlin deyimi "Thomas" köşe grafiği ekler:

```
:> g.addV('person').property('id', 'thomas.1').property('firstName', 'Thomas').property('lastName', 'Andersen').property('age', 44)
```

Ardından, aşağıdaki Gremlin deyimi bir "bilir" kenar Thomas ve bir kez deneme arasındaki ekler.

```
:> g.V('thomas.1').addE('knows').to(g.V('robin.1'))
```

Aşağıdaki sorgu "kişi" köşeleri ilk adlarının azalan sırada döndürür:
```
:> g.V().hasLabel('person').order().by('firstName', decr)
```

Burada yanıtlamanız gerektiğinde grafikleri bekliyoruz "hangi işletim sistemleri Thomas arkadaşların kullanıyor musunuz?" gibi soruları. Grafikten bu bilgileri almak için bu basit Gremlin geçişi çalıştırabilirsiniz:

```
:> g.V('thomas.1').out('knows').out('uses').out('runsos').group().by('name').by(count())
```
Şimdi ne Azure Cosmos DB Gremlin geliştiricileri için sağladığı konumundaki bakalım.

## <a name="gremlin-features"></a>Gremlin özellikleri
TinkerPop grafik teknolojileri çeşitli kapsayan bir standarttır. Bu nedenle, hangi özelliklerin bir grafik sağlayıcısı tarafından sağlanan tanımlamak için standart terimleri vardır. Azure Cosmos DB kalıcı, yüksek eşzamanlılık, birden çok sunuculara veya kümelere bölümlenebilir yazılabilir grafik veritabanı sağlar. 

Aşağıdaki tabloda Azure Cosmos DB tarafından uygulanan TinkerPop özellikler listelenmektedir: 

| Kategori | Azure Cosmos DB uygulama |  Notlar | 
| --- | --- | --- |
| Grafik özellikleri | Kalıcılığı ve ConcurrentAccess sağlar. İşlemleri desteklemek için tasarlanmış | Bilgisayar yöntemlerini Spark bağlayıcısı aracılığıyla uygulanabilir. |
| Değişken özellikleri | Boolean, tamsayı, bayt destekler çift, tamsayı, uzun, dize Float | İlkel türler destekler, veri modeli aracılığıyla karmaşık türler ile uyumlu |
| Köşe özellikleri | RemoveVertices, MetaProperties, AddVertices, MultiProperties, StringIds, UserSuppliedIds, AddProperty, RemoveProperty destekler  | Oluşturma, değiştirme ve silme köşeleri destekler |
| Köşe özellik özellikleri | StringIds, UserSuppliedIds, AddProperty, RemoveProperty, BooleanValues, ByteValues, DoubleValues, FloatValues, IntegerValues, LongValues, StringValues | Oluşturma, değiştirme ve silme köşe özelliklerini destekler |
| Edge özellikleri | AddEdges, RemoveEdges, StringIds, UserSuppliedIds, AddProperty, RemoveProperty | Oluşturma, değiştirme ve silme kenarları destekler |
| Özellik özellikleri | Özellikler, BooleanValues, ByteValues, DoubleValues, FloatValues, IntegerValues, LongValues, StringValues | Oluşturma, değiştirme ve silme kenar özellikleri destekler |

## <a name="gremlin-wire-format-graphson"></a>Gremlin kablo biçimi: GraphSON

Azure Cosmos DB kullanır [GraphSON biçimi](https://github.com/thinkaurelius/faunus/wiki/GraphSON-Format) sonuçları Gremlin işlemlerinden döndürülürken. GraphSON köşeleri, kenarları ve JSON kullanarak özelliklerini (tek ve birden çok değerli) temsil eden Gremlin standart biçimidir. 

Örneğin, aşağıdaki kod parçacığını bir köşe GraphSON gösterimini gösterir *istemciye döndürülen* Azure Cosmos DB'den. 

```json
  {
    "id": "a7111ba7-0ea1-43c9-b6b2-efc5e3aea4c0",
    "label": "person",
    "type": "vertex",
    "outE": {
      "knows": [
        {
          "id": "3ee53a60-c561-4c5e-9a9f-9c7924bc9aef",
          "inV": "04779300-1c8e-489d-9493-50fd1325a658"
        },
        {
          "id": "21984248-ee9e-43a8-a7f6-30642bc14609",
          "inV": "a8e3e741-2ef7-4c01-b7c8-199f8e43e3bc"
        }
      ]
    },
    "properties": {
      "firstName": [
        {
          "value": "Thomas"
        }
      ],
      "lastName": [
        {
          "value": "Andersen"
        }
      ],
      "age": [
        {
          "value": 45
        }
      ]
    }
  }
```

Köşe için GraphSON tarafından kullanılan özellikleri şunlardır:

| Özellik | Açıklama |
| --- | --- |
| id | Köşe kimliği. (Birleşimi _partition varsa değerle) benzersiz olmalıdır |
| Etiket | Köşe etiketi. İsteğe bağlı ve varlık türünü belirtmek için kullanılan budur. |
| type | Grafik olmayan belgelerden köşeleri ayırt etmek için kullanılır |
| properties | Köşe ile ilişkili kullanıcı tanımlı özellik paketi. Her bir özellik, birden çok değerlere sahip olabilir. |
| _partition (yapılandırılabilir) | Köşe bölüm anahtarı. Kullanılabilir birden çok sunucuya grafikleri genişleme için |
| outE | Bu, bir köşe kenarlarından çıkışı bir listesini içerir. Köşe bitişik bilgilerle depolama çapraz geçişlerine hızlı yürütülmesini sağlar. Kenarları etiketlerine göre gruplandırılır. |

Ve kenar grafiği, diğer bölümlerine gezinme yardımcı olmak için aşağıdaki bilgileri içerir.

| Özellik | Açıklama |
| --- | --- |
| id | Kenar kimliği. (Birleşimi _partition varsa değerle) benzersiz olmalıdır |
| Etiket | Kenar etiketi. Bu özellik isteğe bağlıdır ve kullanılan ilişki türü açıklamak için kullanılır. |
| Stok | Bu bir kenar için köşeleri listesi içerir. Edge bitişik bilgilerini depolamak için çapraz geçişlerine Hızlı yürütme sağlar. Köşeleri etiketlerine göre gruplandırılır. |
| properties | Edge ile ilişkili kullanıcı tanımlı özellikleri paketi. Her bir özellik, birden çok değerlere sahip olabilir. |

Her bir özellik, bir dizi birden çok değer depolayabilirsiniz. 

| Özellik | Açıklama |
| --- | --- |
| değer | Özelliğinin değeri

## <a name="gremlin-partitioning"></a>Gremlin bölümlendirme

Azure Cosmos DB'de grafikleri ölçeklendirebilirsiniz kapsayıcılara depolanan bağımsız olarak bakımından depolama üretilen işi (normalleştirilmiş istekleri / saniye cinsinden). Her kapsayıcı isteğe bağlı bir tanımlamanız gerekir, ancak ilgili veriler için bir mantıksal bölüm sınır belirler bölüm anahtar özelliği önerilir. Her köşe/kenar olmalıdır bir `id` Bu bölüm anahtarı değerini varlıkları için benzersizdir özelliği. Ayrıntıları ele alınmaktadır [Azure Cosmos DB'de bölümleme](partition-data.md).

Gremlin işlemleri Azure Cosmos DB birden çok bölüm span grafik verileri arasında sorunsuz çalışır. Ancak, bir filtre sorgularında, birçok farklı değerleri, ve benzer sıklığını erişimine sahip bu değerleri gibi sık kullanılan bir bölüm anahtarı, grafiklerde seçmek için önerilir. 

## <a name="gremlin-steps"></a>Gremlin adımları
Artık Azure Cosmos DB tarafından desteklenen Gremlin adımları bakalım. Gremlin üzerinde tam bir başvuru için bkz: [TinkerPop başvuru](http://tinkerpop.apache.org/docs/current/reference).

| Adım | Açıklama | TinkerPop 3.2 belgeleri |
| --- | --- | --- |
| `addE` | İki köşeleri arasında kenar ekler | [addE step](http://tinkerpop.apache.org/docs/current/reference/#addedge-step) |
| `addV` | Grafiğe bir köşe ekler | [addV step](http://tinkerpop.apache.org/docs/current/reference/#addvertex-step) |
| `and` | Tüm çapraz geçişlerine bir değer döndürmesi sağlar | [ve adım](http://tinkerpop.apache.org/docs/current/reference/#and-step) |
| `as` | Bir adım için bir değişken atamak için bir adım modülatörü | [adım olarak](http://tinkerpop.apache.org/docs/current/reference/#as-step) |
| `by` | İle kullanılan bir adım modülatörü `group` ve `order` | [Adım](http://tinkerpop.apache.org/docs/current/reference/#by-step) |
| `coalesce` | Bir sonuç döndürür ilk geçişi döndürür | [Adım birleşim](http://tinkerpop.apache.org/docs/current/reference/#coalesce-step) |
| `constant` | Sabit bir değer döndürür. İle kullanılan `coalesce`| [Sabit adım](http://tinkerpop.apache.org/docs/current/reference/#constant-step) |
| `count` | Geçişi sayımını döndürür | [Count adım](http://tinkerpop.apache.org/docs/current/reference/#count-step) |
| `dedup` | Yinelenen değerleri kaldırılmış değerleri döndürür | [Yinelenenleri kaldırma adım](http://tinkerpop.apache.org/docs/current/reference/#dedup-step) |
| `drop` | Değerleri (köşe/kenar) bırakır | [bırakma adımı](http://tinkerpop.apache.org/docs/current/reference/#drop-step) |
| `fold` | Toplama sonuçları hesaplar bir engel görür| [Katlama adım](http://tinkerpop.apache.org/docs/current/reference/#fold-step) |
| `group` | Grupları belirtilen etiketlerini esas alan değerleri| [Grup adımı](http://tinkerpop.apache.org/docs/current/reference/#group-step) |
| `has` | Özellikler, köşe ve kenarları filtrelemek için kullanılır. Destekler `hasLabel`, `hasId`, `hasNot`, ve `has` çeşitleri. | [adım vardır](http://tinkerpop.apache.org/docs/current/reference/#has-step) |
| `inject` | Değerleri bir akışa ekleme| [Adım ekleme](http://tinkerpop.apache.org/docs/current/reference/#inject-step) |
| `is` | Boole ifadesi kullanarak bir filtre gerçekleştirmek için kullanılan | [Adım](http://tinkerpop.apache.org/docs/current/reference/#is-step) |
| `limit` | Geçişi öğelerin sayısını sınırlamak için kullanılır| [sınır adım](http://tinkerpop.apache.org/docs/current/reference/#limit-step) |
| `local` | Geçişi, bir alt sorgu için benzer bir bölümünü yerel sarmalar | [Yerel adım](http://tinkerpop.apache.org/docs/current/reference/#local-step) |
| `not` | Bir filtre değilleme üretmek için kullanılan | [Adım değil](http://tinkerpop.apache.org/docs/current/reference/#not-step) |
| `optional` | Bir sonuç döndürürse belirtilen geçişi sonucunu döndürür başka arama öğeyi döndürür | [İsteğe bağlı adım](http://tinkerpop.apache.org/docs/current/reference/#optional-step) |
| `or` | Çapraz geçişlerine en az biri bir değer döndürür sağlar | [veya adımı](http://tinkerpop.apache.org/docs/current/reference/#or-step) |
| `order` | Belirtilen sıralama düzeni sonuçlarında döndürür | [Sipariş adım](http://tinkerpop.apache.org/docs/current/reference/#order-step) |
| `path` | Geçişi tam yolunu döndürür | [yol adım](http://tinkerpop.apache.org/docs/current/reference/#path-step) |
| `project` | Bir eşleme özelliklerini projeleri | [project step](http://tinkerpop.apache.org/docs/current/reference/#project-step) |
| `properties` | Belirtilen etiket için özellikleri döndürür | [özellikleri adım](http://tinkerpop.apache.org/docs/current/reference/#properties-step) |
| `range` | Belirtilen değer aralığı için filtreleri| [Aralık adım](http://tinkerpop.apache.org/docs/current/reference/#range-step) |
| `repeat` | Adım belirtilen sayıda için yineler. Döngü için kullanılan | [adımı yineleyin](http://tinkerpop.apache.org/docs/current/reference/#repeat-step) |
| `sample` | Örnek sonuçları geçişi kullanılan | [Örnek adım](http://tinkerpop.apache.org/docs/current/reference/#sample-step) |
| `select` | Proje sonuçları geçişi kullanılan |  [adım seçin](http://tinkerpop.apache.org/docs/current/reference/#select-step) | |
| `store` | Engelleyici olmayan toplamalar gelen geçişi için kullanılır | [Mağaza adım](http://tinkerpop.apache.org/docs/current/reference/#store-step) |
| `tree` | Bir ağaç içine köşe toplama yolları | [Ağaç adım](http://tinkerpop.apache.org/docs/current/reference/#tree-step) |
| `unfold` | Yineleyici bir adım olarak unroll| [Adım unfold](http://tinkerpop.apache.org/docs/current/reference/#unfold-step) |
| `union` | Birden çok çapraz geçişlerine sonuçlarından birleştirme| [birleşim adım](http://tinkerpop.apache.org/docs/current/reference/#union-step) |
| `V` | Köşeleri ve kenarları arasında çapraz geçişlerine için gerekli olan adımları içerir `V`, `E`, `out`, `in`, `both`, `outE`, `inE`, `bothE`, `outV`, `inV` , `bothV`, ve `otherV` için | [vertex steps](http://tinkerpop.apache.org/docs/current/reference/#vertex-steps) |
| `where` | Geçişi sonuçlarını filtrelemek için kullanılır. Destekler `eq`, `neq`, `lt`, `lte`, `gt`, `gte`, ve `between` işleçleri  | [Burada adım](http://tinkerpop.apache.org/docs/current/reference/#where-step) |

Azure Cosmos DB tarafından sağlanan yazma iyileştirilmiş altyapısı otomatik köşeleri ve kenarları içindeki tüm özelliklerinin varsayılan dizin oluşturmayı destekler. Bu nedenle, sorgular filtreleriyle, sorguları, sıralama, aralık veya herhangi bir özellikte toplamalar dizinden işlenir ve verimli bir şekilde hizmet. Üzerinde şu incelemeyi Azure Cosmos veritabanı dizin oluşturma nasıl çalıştığıyla ilgili daha fazla bilgi için bkz [şema belirsiz dizin](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf).

## <a name="next-steps"></a>Sonraki adımlar
* Graph uygulaması oluşturmaya başlamak [bizim SDK'ları kullanma](create-graph-dotnet.md) 
* Daha fazla bilgi edinmek [grafik destek](graph-introduction.md) Azure Cosmos veritabanı
