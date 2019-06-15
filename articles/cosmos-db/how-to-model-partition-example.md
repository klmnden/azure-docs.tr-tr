---
title: Azure Cosmos DB'de gerçek örneği kullanarak model ve bölüm verilerini
description: Model ve Azure Cosmos DB Core API'si kullanarak bir gerçek örnek bölümü hakkında bilgi edinin
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/23/2019
ms.author: thweiss
ms.openlocfilehash: 4bb99c8cbec88d23f9297dcbe8b13cc69cd0006c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67070676"
---
# <a name="how-to-model-and-partition-data-on-azure-cosmos-db-using-a-real-world-example"></a>Azure Cosmos DB'de gerçek örneği kullanarak model ve bölüm verilerini

Bu makalede gibi çeşitli Azure Cosmos DB kavramlarla ilgili derlemeleri [veri modelleme](modeling-data.md), [bölümleme](partitioning-overview.md), ve [sağlanan aktarım hızı](request-units.md) nasıl başa çıkabileceğiniz gerçek dünyaya göstermek için Veri alıştırma tasarlayın.

Genellikle ilişkisel veritabanları ile çalışıyorsanız, büyük olasılıkla alışkanlıkları ve bir veri modeli tasarlama konusunda intuitions oluşturdunuz. Belirli kısıtlamalar, aynı zamanda Azure Cosmos DB benzersiz gücü, nedeniyle bu en iyi uygulamaların en iyi Çevir yoktur ve yetersiz çözümleriyle sürükleyin. Bu makalenin amacı, tam bir gerçek kullanım örneği, varlık birlikte bulundurma ve kapsayıcı bölümleme modelleme öğesinden Azure Cosmos DB üzerinde modelleme işlemi boyunca rehberlik etmektir.

## <a name="the-scenario"></a>Senaryo

Bu alıştırma için bir blog oluşturma platformu etki alanını düşünün kullanacağız burada *kullanıcılar* oluşturabilirsiniz *gönderileri*. Kullanıcılar aynı zamanda *gibi* ve ekleme *açıklamaları* bu gönderiler için.

> [!TIP]
> Biz bazı sözcükler vurguladığınız *italik*; bu sözcükler "şey" modelimizi işlemek için olacaktır türünü belirleyin.

Daha fazla gereksinimleri bizim belirtimine ekleme:

- Son oluşturulan gönderilerin bir akış ön sayfa görüntüler,
- Bir kullanıcı için tüm gönderileri, bir gönderi için tüm yorumları ve bir gönderi için tüm beğenilerin getirmek,
- Gönderileri yazarlar kullanıcı adı ve kaç yorumlar ve beğeniler sahip oldukları sayısı ile döndürülür,
- Yorumlar ve beğeniler Ayrıca bunları oluşturan kullanıcılar kullanıcı adı ile döndürülür,
- Listeler olarak görüntülediğinde, yalnızca içeriklerini kesilmiş özetini sunmak gönderilerin.

## <a name="identify-the-main-access-patterns"></a>Ana erişim düzenlerini belirleyen

Başlamak için bazı yapısı bizim ilk belirtimine bizim çözümün erişim desenlerini tanımlayarak sunuyoruz. Bir veri modeli için Azure Cosmos DB tasarlarken hangi istekleri modelimizi modeli bu istekleri verimli bir şekilde görecek emin olmak için hizmet gerekecektir anlamak önemlidir.

Genel işlem takibini kolaylaştırmak için size farklı isteklere komutları veya sorguyu bazı sözcük ödünç kategorilere [CQRS](https://en.wikipedia.org/wiki/Command%E2%80%93query_separation#Command_query_responsibility_segregation) komutları nerede yazma istekleri (sistemi güncelleştirmek için niyetlerini) diğer bir deyişle, ve sorgular, salt okunur isteklerdir.

Platformumuzu kullanıma gerekecektir istekleri listesi aşağıda verilmiştir:

- **[C1]**  Oluştur/Düzenle kullanıcı
- **[S1]**  Bir kullanıcı alma
- **[C2]**  Oluştur/Düzenle bir gönderi
- **[S2]**  Posta al
- **[Q3]**  Kısa form kullanıcının posta listesi
- **[C3]**  Bir açıklama oluşturun
- **[4]**  Post'ın açıklamaları listesi
- **[C4]**  Gibi bir gönderi
- **[S5]**  Post's beğenilerin listesi
- **[S6]**  Listesi *x* en son gönderiler oluşturulan kısa form (akış)

Her bir varlık (kullanıcı, post, vb.) yer alır, hakkındaki ayrıntıları bu aşama düşündük henüz. Bu adım, genellikle nasıl kişilikleri İngilizceye tablolar, sütunlar, yabancı anahtarlar vb. açısından ekleyeceğimi gerekir çünkü ilişkisel bir depo tasarlarken tackled için ilk olanları arasında yer almıştır. Bu yazma sırasında herhangi bir şema zorunlu olmayan bir belge veritabanı içeriğiyle daha az olur.

Neden başından başlayarak bizim erişim düzenlerini belirleyen önemli olduğu ana nedeni, bu isteklerin listesini test paketimiz olabilir çünkü olmasıdır. Biz veri modelimizi yineleme her zaman, her bir isteğin Git eder ve kendi performans ve ölçeklenebilirlik'i denetleyin.

## <a name="v1-a-first-version"></a>V1: İlk sürümü

Biz iki kapsayıcılarla başlayın: `users` ve `posts`.

### <a name="users-container"></a>Kullanıcılar kapsayıcısı

Bu kapsayıcı yalnızca kullanıcı öğeleri depolar:

    {
      "id": "<user-id>",
      "username": "<username>"
    }

Biz bu kapsayıcı tarafından bölüm `id`, yani bu kapsayıcı her mantıksal bölüm yalnızca bir öğe içeriyor.

### <a name="posts-container"></a>Gönderileri kapsayıcı

Bu kapsayıcı barındıran iletileri, açıklamalar ve beğeni:

    {
      "id": "<post-id>",
      "type": "post",
      "postId": "<post-id>",
      "userId": "<post-author-id>",
      "title": "<post-title>",
      "content": "<post-content>",
      "creationDate": "<post-creation-date>"
    }

    {
      "id": "<comment-id>",
      "type": "comment",
      "postId": "<post-id>",
      "userId": "<comment-author-id>",
      "content": "<comment-content>",
      "creationDate": "<comment-creation-date>"
    }

    {
      "id": "<like-id>",
      "type": "like",
      "postId": "<post-id>",
      "userId": "<liker-id>",
      "creationDate": "<like-creation-date>"
    }

Biz bu kapsayıcı tarafından bölüm `postId`, bu kapsayıcıdaki her bir mantıksal bölüm içereceğini posta, bu gönderi için tüm yorumları ve bu gönderi için tüm beğenilerin anlamına gelir.

Ekledik Not bir `type` öğeleri özelliğinde depolanan üç tür varlıklar arasında ayrım yapmak için bu kapsayıcıdaki bu kapsayıcı konakları.

İlgili veri katıştırmak yerine başvurmak ayrıca seçtik (denetleyin [Bu bölümde](modeling-data.md) Bu kavramlar hakkında ayrıntılı bilgi için) için:

- bir kullanıcı oluşturabilir, kaç gönderileri için üst sınır yoktur,
- gönderileri rasgele uzun olabilir,
- kaç tane yorumlar ve beğeniler post olabilir için üst sınır yoktur,
- post güncelleştirmek zorunda kalmadan bir gönderi için bir açıklama veya LIKE eklemek mümkün olmasını istiyoruz.

## <a name="how-well-does-our-model-perform"></a>Modelimizi nasıl gerçekleştirir?

Bu bizim ilk sürüm ölçeklenebilirliği ve performansı değerlendirmek için zaman sunulmuştur. Her biri daha önce tanımlanan istekleri için biz, gecikme süresi ve kaç tane istek ölçü birimi kullandığı. Bu ölçüm, kullanıcı başına ve en fazla 25 yorumlar ve gönderi başına 100 beğenilerin 5 ile 50 gönderileri birlikte 100.000 kullanıcı içeren işlevsiz bir veri kümesi karşı yapılır.

### <a name="c1-createedit-a-user"></a>[C1] Kullanıcı Oluştur/Düzenle

Bu istek size yalnızca oluşturmak veya bir öğeyi güncelleştirme uygulamak oldukça basittir `users` kapsayıcı. İstek düzgün şekilde performanstan tüm bölümler arasında yayılır `id` bölüm anahtarı.

![Tek bir öğe Kullanıcılar kapsayıcısına yazma](./media/how-to-model-partition-example/V1-C1.png)

| **Gecikme süresi** | **RU ücreti** | **Performans** |
| --- | --- | --- |
| 7 ms | 5.71 RU | ✅ |

### <a name="q1-retrieve-a-user"></a>[S1] Bir kullanıcı alma

Bir kullanıcı alma yapılır karşılık gelen öğeden okuyarak `users` kapsayıcı.

![Tek bir öğe kullanıcılar kapsayıcısından alınırken](./media/how-to-model-partition-example/V1-Q1.png)

| **Gecikme süresi** | **RU ücreti** | **Performans** |
| --- | --- | --- |
| 2 ms | 1 RU | ✅ |

### <a name="c2-createedit-a-post"></a>[C2] Bir gönderi Oluştur/Düzenle

Benzer şekilde **[C1]** , biz yalnızca yazmak zorunda `posts` kapsayıcı.

![Tek bir öğe için gönderileri kapsayıcı yazma](./media/how-to-model-partition-example/V1-C2.png)

| **Gecikme süresi** | **RU ücreti** | **Performans** |
| --- | --- | --- |
| 9 ms | 8.76 RU | ✅ |

### <a name="q2-retrieve-a-post"></a>[S2] Posta Al

Belge karşılık gelen alarak başlangıç `posts` kapsayıcı. Ancak bu yeterli olmaz, bizim belirtimi uyarınca de post kullanıcının Yazanın kullanıcı adını toplamak sahibiz ve kaç açıklama sayısını ve kaç yazıya beğeni sahip verilmesi için 3 ek SQL sorguları gerektirir.

![Bir postayı almaya ve ek verileri toplama](./media/how-to-model-partition-example/V1-Q2.png)

Her bir ek sorgu performansı ve ölçeklenebilirliği en üst düzeye çıkarmak istediğimiz tam olarak olan ilgili kapsayıcısının bölüm anahtarına göre filtreler. Ancak sonuçta biz, bir sonraki yinelemede geliştirmek böylece tek bir gönderi döndürmek için dört işlemleri gerçekleştirmek sunuyoruz.

| **Gecikme süresi** | **RU ücreti** | **Performans** |
| --- | --- | --- |
| 9 ms | 19.54 RU | ⚠ |

### <a name="q3-list-a-users-posts-in-short-form"></a>[Q3] Kısa form kullanıcının posta listesi

İlk olarak, belirli bir kullanıcıya karşılık gelen postaları getiren bir SQL sorgusu ile istenen gönderileri alınacak sahibiz. Ancak biz de yazar kullanıcı adı ve açıklama sayıları toplamak için ek sorguları göndermek zorunda ve beğeni.

![Bir kullanıcı için tüm gönderileri almaya ve ek verilerini toplama](./media/how-to-model-partition-example/V1-Q3.png)

Bu uygulama, çok sayıda dezavantajları sunar:

- yorumlar ve beğeniler sayısı toplama sorguları ilk sorgu tarafından döndürülen her bir gönderi için verilmesi gerekiyor.
- Ana sorguda bölüm anahtarına göre filtre `posts` kapsayıcı arasında bir yayma ve bölüm tarama için önde gelen kapsayıcı.

| **Gecikme süresi** | **RU ücreti** | **Performans** |
| --- | --- | --- |
| 130 ms | 619.41 RU | ⚠ |

### <a name="c3-create-a-comment"></a>[C3] Bir açıklama oluşturun

Karşılık gelen öğe yazarak bir açıklama oluşturulur `posts` kapsayıcı.

![Tek bir öğe için gönderileri kapsayıcı yazma](./media/how-to-model-partition-example/V1-C2.png)

| **Gecikme süresi** | **RU ücreti** | **Performans** |
| --- | --- | --- |
| 7 ms | 8.57 RU | ✅ |

### <a name="q4-list-a-posts-comments"></a>[4] Post'ın açıklamaları listesi

Size bu gönderi için tüm yorumları getiren bir sorgu ile başlayın ve yine de her bir açıklama için ayrı ayrı toplam kullanıcı adları için gerekiyor.

![Bir gönderi için tüm yorumları almaya ve ek verilerini toplama](./media/how-to-model-partition-example/V1-Q4.png)

Ana sorguda kapsayıcının bölüm anahtarına göre filtre olsa da, kullanıcı adlarını ayrı olarak toplama genel performansını penalizes. Biz, daha sonra geliştirmek.

| **Gecikme süresi** | **RU ücreti** | **Performans** |
| --- | --- | --- |
| 23 ms | 27.72 RU | ⚠ |

### <a name="c4-like-a-post"></a>[C4] Bir iletiyi beğenme

Olduğu gibi **[C3]** , karşılık gelen öğe ile oluştururuz `posts` kapsayıcı.

![Tek bir öğe için gönderileri kapsayıcı yazma](./media/how-to-model-partition-example/V1-C2.png)

| **Gecikme süresi** | **RU ücreti** | **Performans** |
| --- | --- | --- |
| 6 ms | 7.05 RU | ✅ |

### <a name="q5-list-a-posts-likes"></a>[S5] Post's beğenilerin listesi

Olduğu gibi **[4]** , size bu gönderi için beğenilerin sorgu sonra kullanıcıların kullanıcı adlarını toplama.

![Bir post ve ek verilerini toplamak için beğeni, tüm alınıyor](./media/how-to-model-partition-example/V1-Q5.png)

| **Gecikme süresi** | **RU ücreti** | **Performans** |
| --- | --- | --- |
| 59 ms | 58.92 RU | ⚠ |

### <a name="q6-list-the-x-most-recent-posts-created-in-short-form-feed"></a>[S6] Kısa form (akış) oluşturulan x en son gönderiler listesi

Biz sorgulayarak en son gönderiler fetch `posts` oluşturma tarihi daha sonra toplam kullanıcı adları ve yorumlar ve beğeniler sayısı her gönderi için azalan bir kapsayıcı.

![En son gönderiler almaya ve ek verilerini toplama](./media/how-to-model-partition-example/V1-Q6.png)

Bir kez daha, ilk sorgumuzu bölüm anahtarına göre filtre uygulamaz `posts` kapsayıcı maliyetli yayma tetikler. Bu tek hedef çok daha büyük bir sonuç kümesi ve sonuçları sıralamak gibi daha büyük bir `ORDER BY` yan tümcesi, istek birimleri daha pahalı kılar.

| **Gecikme süresi** | **RU ücreti** | **Performans** |
| --- | --- | --- |
| 306 ms | 2063.54 RU | ⚠ |

## <a name="reflecting-on-the-performance-of-v1"></a>V1'ın performansını yansıtma

Biz önceki bölümde karşılaşılan performans sorunlarını bakarak, iki ana sınıf sorunları belirleyebilir:

- döndürülecek ihtiyacımız tüm verileri toplamak için verilmesi için birden çok sorgu gerektiren bazı istekler
- Bazı sorgular bölüm anahtarını bilir, bizim ölçeklenebilirlik yavaşlatır bir yayma için önde gelen hedefledikleri kapsayıcıları filtre yok.

Şimdi her ilkinden başlayarak, bu sorunları çözün.

## <a name="v2-introducing-denormalization-to-optimize-read-queries"></a>V2'DE: Salt okunur sorguların iyileştirilmesine normalleştirilmişlikten çıkarma ile tanışın

İlk istek sonuçlarını içermediğinden döndürmek için ihtiyacımız olan veriler neden bazı durumlarda ek istekler verecek sahibiz neden olmasıdır. Azure Cosmos DB gibi ilişkisel olmayan veri deposu ile çalışırken, bu tür bir sorun bizim veri kümesi arasında verileri normal durumdan çıkarmayı yaygın olarak çözülür.

Bizim örneğimizde, biz post'ın yazar ve açıklama sayısını sayısı kullanıcı adı eklemek için post öğelerini beğeni:

    {
      "id": "<post-id>",
      "type": "post",
      "postId": "<post-id>",
      "userId": "<post-author-id>",
      "userUsername": "<post-author-username>",
      "title": "<post-title>",
      "content": "<post-content>",
      "commentCount": <count-of-comments>,
      "likeCount": <count-of-likes>,
      "creationDate": "<post-creation-date>"
    }

Biz de açıklamayı değiştirin ve bunları oluşturan kullanıcının kullanıcı adı eklenecek öğeler ister:

    {
      "id": "<comment-id>",
      "type": "comment",
      "postId": "<post-id>",
      "userId": "<comment-author-id>",
      "userUsername": "<comment-author-username>",
      "content": "<comment-content>",
      "creationDate": "<comment-creation-date>"
    }

    {
      "id": "<like-id>",
      "type": "like",
      "postId": "<post-id>",
      "userId": "<liker-id>",
      "userUsername": "<liker-username>",
      "creationDate": "<like-creation-date>"
    }

### <a name="denormalizing-comment-and-like-counts"></a>Açıklama normal durumdan çıkarmayı ve ister sayıları

Elde etmek istediğimiz bir açıklama veya LIKE eklediğimiz her seferinde, biz de artırma olan `commentCount` veya `likeCount` karşılık gelen gönderisinde. Olarak bizim `posts` kapsayıcı bölümlenmiş tarafından `postId`, yeni bir öğe (açıklama satırı yapın veya ister) ve aynı mantıksal bölümde sit, karşılık gelen post. Sonuç olarak, kullanabiliriz bir [saklı yordamı](stored-procedures-triggers-udfs.md) bu işlemi gerçekleştirmek için.

Artık yorum oluştururken ( **[C3]** ), yalnızca içinde yeni bir öğe eklemek yerine `posts` kapsayıcı diyoruz aşağıdaki saklı yordamı bu kapsayıcıdaki:

```javascript
function createComment(postId, comment) {
  var collection = getContext().getCollection();

  collection.readDocument(
    `${collection.getAltLink()}/docs/${postId}`,
    function (err, post) {
      if (err) throw err;

      post.commentCount++;
      collection.replaceDocument(
        post._self,
        post,
        function (err) {
          if (err) throw err;

          comment.postId = postId;
          collection.createDocument(
            collection.getSelfLink(),
            comment
          );
        }
      );
    })
}
```

Bu saklı yordamı, post ve yeni yorum gövdesi Kimliğini sonra parametre alır:

- posta alır.
- artırır `commentCount`
- post değiştirir
- Yeni Yorum ekler

Saklı yordamlar atomik işlemler gerçekleştirilirken, sağlanır değerini `commentCount` ve açıklama gerçek sayısını her zaman eşitlenmiş durumda kalır.

Beğeni artırmak yeni ekleme sırasında da benzer bir saklı yordam diyoruz `likeCount`.

### <a name="denormalizing-usernames"></a>Normal durumdan çıkarmayı kullanıcı adları

Kullanıcı adları, kullanıcılar yalnızca farklı bölümler, ancak farklı bir kapsayıcıya sit gibi farklı bir yaklaşım gerektirir. Biz, bölümler ve kapsayıcılar arasında verileri normalleştirilmişlikten çıkarmak varsa, kaynak kapsayıcının kullanabiliriz [değişiklik akışını](change-feed.md).

Bizim kullandığımız değişiklik akışını `users` kullanıcılar, kullanıcı adlarını güncelleştirdiğinizde tepki vermek için kapsayıcı. Bu durum oluştuğunda, biz değiştirme üzerinde başka bir saklı yordam çağırarak yaymak `posts` kapsayıcı:

![Kullanıcı adlarını gönderileri kapsayıcıya normal durumdan çıkarmayı](./media/how-to-model-partition-example/denormalization-1.png)

```javascript
function updateUsernames(userId, username) {
  var collection = getContext().getCollection();
  
  collection.queryDocuments(
    collection.getSelfLink(),
    `SELECT * FROM p WHERE p.userId = '${userId}'`,
    function (err, results) {
      if (err) throw err;

      for (var i in results) {
        var doc = results[i];
        doc.userUsername = username;

        collection.upsertDocument(
          collection.getSelfLink(),
          doc);
      }
    });
}
```

Bu saklı yordamı, kullanıcının yeni kullanıcı adı ve kullanıcı kimliği sonra parametre alır:

- eşleşen tüm öğeleri getirir `userId` (olabilen gönderimleri, açıklamaları veya beğeni)
- her biri bu öğeleri için
  - değiştirir `userUsername`
  - öğesini değiştirir

> [!IMPORTANT]
> Bu saklı yordamı her bölüme yürütülecek gerektiğinden bu işlem maliyetlidir `posts` kapsayıcı. Çoğu kullanıcı kayıt sırasında uygun bir kullanıcı adı seçin ve bu güncelleştirme çok nadir olarak çalışacak şekilde şimdiye kadar değişmez varsayıyoruz.

## <a name="what-are-the-performance-gains-of-v2"></a>V2, gelen performans artışı nelerdir?

### <a name="q2-retrieve-a-post"></a>[S2] Posta Al

Bizim normalleştirilmişlikten çıkarma yerinde olduğuna göre yalnızca bu isteği işlemek için tek bir öğe getirilecek sahibiz.

![Tek bir öğe gönderileri kapsayıcısından alınırken](./media/how-to-model-partition-example/V2-Q2.png)

| **Gecikme süresi** | **RU ücreti** | **Performans** |
| --- | --- | --- |
| 2 ms | 1 RU | ✅ |

### <a name="q4-list-a-posts-comments"></a>[4] Post'ın açıklamaları listesi

Burada yine, biz kullanıcı adları ve son bölüm anahtarına göre filtreleyen tek bir sorgu ile getirilen ek istekler yedek.

![Bir gönderi için tüm yorumları alınıyor](./media/how-to-model-partition-example/V2-Q4.png)

| **Gecikme süresi** | **RU ücreti** | **Performans** |
| --- | --- | --- |
| 4 ms | 7.72 RU | ✅ |

### <a name="q5-list-a-posts-likes"></a>[S5] Post's beğenilerin listesi

Beğenilerin listelerken tam aynı durum.

![Tüm almak için bir gönderme beğeni](./media/how-to-model-partition-example/V2-Q5.png)

| **Gecikme süresi** | **RU ücreti** | **Performans** |
| --- | --- | --- |
| 4 ms | 8.92 RU | ✅ |

## <a name="v3-making-sure-all-requests-are-scalable"></a>V3: Tüm istekleri sağlamaktan ölçeklenebilir

Bizim genel performans geliştirmeleri arama, hala var. tamamen iyileştirdik henüz iki isteği: **[Q3]** ve **[S6]** . Bunlar, hedefledikleri kapsayıcılar bölüm anahtarına göre filtre yoksa sorgular ile ilgili isteklerdir.

### <a name="q3-list-a-users-posts-in-short-form"></a>[Q3] Kısa form kullanıcının posta listesi

Bu istek zaten ek sorgular ödemek zorunda kalmamasını sağlar, V2'de sunulan iyileştirmeler faydalanır.

![Bir kullanıcı için tüm gönderileri alınıyor](./media/how-to-model-partition-example/V2-Q3.png)

Ancak geri kalan sorgu hala bölüm anahtarına göre filtreleme değildir `posts` kapsayıcı.

Bu durumu hakkında düşünmek gerçekten basit yoludur:

1. Bu istek *sahip* filtrelemek üzere `userId` belirli bir kullanıcı için tüm gönderileri getirilecek istediğimizden
1. İyi karşı yürütülür çünkü gerçekleştirmez `posts` tarafından bölümlenmemiş kapsayıcı `userId`
1. Belirgin belirten, biz performans bu isteği bir kapsayıcı karşı yürüterek ister misiniz, *olduğu* tarafından bölümlenmiş `userId`
1. Zaten bu tür bir kapsayıcı sahibiz ettik: `users` kapsayıcı!

Biz tüm gönderimleri çoğaltarak normalleştirilmişlikten çıkarma ikinci bir düzeyini tanıtmak için `users` kapsayıcı. Bunu yapmadan etkili bir şekilde bir kopyasını farklı bir boyutta yalnızca bölümlenmiş, bizim gönderileri tarafından almak için daha verimli hale getirir aldığımız kendi `userId`.

`users` Kapsayıcısı artık 2 öğe türlerini içerir:

    {
      "id": "<user-id>",
      "type": "user",
      "userId": "<user-id>",
      "username": "<username>"
    }

    {
      "id": "<post-id>",
      "type": "post",
      "postId": "<post-id>",
      "userId": "<post-author-id>",
      "userUsername": "<post-author-username>",
      "title": "<post-title>",
      "content": "<post-content>",
      "commentCount": <count-of-comments>,
      "likeCount": <count-of-likes>,
      "creationDate": "<post-creation-date>"
    }

Aşağıdakilere dikkat edin:

- ekledik bir `type` gönderilerinden, kullanıcıları ayırt etmek için kullanıcı öğesi alan
- de ekledik bir `userId` ile gereksiz kullanıcı öğesini alanındaki `id` ancak alandır olarak gerekli `users` kapsayıcı tarafından bölümlenmiş artık `userId` (ve `id` daha önce)

Bu normalleştirilmişlikten çıkarma elde etmek için bir kez daha değişiklik akışı kullanırız. Biz bu kez, üzerinde değişiklik akışını react `posts` herhangi bir yeni veya güncelleştirilmiş gönderiyi gönderileceği kapsayıcı `users` kapsayıcı. Ve gönderileri listeleme tam içeriklerini döndürülecek gerektirmediğinden, biz bunları işlemde kısaltabilirsiniz.

![Kullanıcılar kapsayıcısına normal durumdan çıkarmayı gönderileri](./media/how-to-model-partition-example/denormalization-2.png)

Biz şimdi sorgumuzu için yönlendirebilir `users` kapsayıcısı, kapsayıcının bölüm anahtarına göre filtreleme.

![Bir kullanıcı için tüm gönderileri alınıyor](./media/how-to-model-partition-example/V3-Q3.png)

| **Gecikme süresi** | **RU ücreti** | **Performans** |
| --- | --- | --- |
| 4 ms | 6.46 RU | ✅ |

### <a name="q6-list-the-x-most-recent-posts-created-in-short-form-feed"></a>[S6] Kısa form (akış) oluşturulan x en son gönderiler listesi

Biz burada benzer bir durum ile uğraşmak zorunda: bile ilave sorgulardan sparing V2'de tanıtılan normalleştirilmişlikten çıkarma tarafından gereksiz sol sonra kalan sorgu kapsayıcının bölüm anahtarına göre filtre değil:

![En son gönderiler alınıyor](./media/how-to-model-partition-example/V2-Q6.png)

Bu isteğin performans ve ölçeklenebilirliği en üst düzeye aynı yaklaşımı, bu yalnızca bir bölüm İsabetleri gerektirir. Biz yalnızca sınırlı sayıda öğeleri döndürmek olduğundan bu parametrelerin budur; Bizim blog platformun giriş sayfasını doldurmak için yalnızca 100 en son gönderiler tüm veri kümesi üzerinden gösterilecek şekilde sayfalara bölmek zorunda kalmadan almak gerekiyor.

Bu son istek iyileştirmek için biz üçüncü bir kapsayıcı bizim tasarım tanıtmak için tamamen sunan bu istek için ayrılmış. Biz bu yeni bizim gönderimleri normalleştirmesini `feed` kapsayıcı:

    {
      "id": "<post-id>",
      "type": "post",
      "postId": "<post-id>",
      "userId": "<post-author-id>",
      "userUsername": "<post-author-username>",
      "title": "<post-title>",
      "content": "<post-content>",
      "commentCount": <count-of-comments>,
      "likeCount": <count-of-likes>,
      "creationDate": "<post-creation-date>"
    }

Bu kapsayıcı tarafından bölümlenmiş `type`, her zaman olacak `post` bizim öğeleri. Bu, tüm bu kapsayıcı öğeler aynı bölümde şaşıracaksınız sağlanır.

Normalleştirilmişlikten çıkarma elde etmek için yalnızca daha önce yeni bir kapsayıcı gönderimleri gönderileceği ekledik işlem hattı akış değişiklik yeteneklerinizi sahibiz. Aklınızda bulundurun gereken önemli bir unsur, yalnızca 100 en son gönderiler depolarız emin olmak ihtiyacımız olan; Aksi takdirde, kapsayıcı içeriğini bir bölüm en büyük boyutunu büyütün. Bu çağrı yaparak yapılır bir [sonrası tetikleyici](stored-procedures-triggers-udfs.md#triggers) kapsayıcıda her bir belge eklendiğinde:

![Akış kapsayıcı içine normal durumdan çıkarmayı gönderileri](./media/how-to-model-partition-example/denormalization-3.png)

Koleksiyon keser sonrası tetikleyici gövdesi şu şekildedir:

```javascript
function truncateFeed() {
  const maxDocs = 100;
  var context = getContext();
  var collection = context.getCollection();

  collection.queryDocuments(
    collection.getSelfLink(),
    "SELECT VALUE COUNT(1) FROM f",
    function (err, results) {
      if (err) throw err;

      processCountResults(results);
    });

  function processCountResults(results) {
    // + 1 because the query didn't count the newly inserted doc
    if ((results[0] + 1) > maxDocs) {
      var docsToRemove = results[0] + 1 - maxDocs;
      collection.queryDocuments(
        collection.getSelfLink(),
        `SELECT TOP ${docsToRemove} * FROM f ORDER BY f.creationDate`,
        function (err, results) {
          if (err) throw err;

          processDocsToRemove(results, 0);
        });
    }
  }

  function processDocsToRemove(results, index) {
    var doc = results[index];
    if (doc) {
      collection.deleteDocument(
        doc._self,
        function (err) {
          if (err) throw err;

          processDocsToRemove(results, index + 1);
        });
    }
  }
}
```

İçin yeni sorgumuzu yönlendirecek şekilde son adımdır `feed` kapsayıcı:

![En son gönderiler alınıyor](./media/how-to-model-partition-example/V3-Q6.png)

| **Gecikme süresi** | **RU ücreti** | **Performans** |
| --- | --- | --- |
| 9 ms | 16.97 RU | ✅ |

## <a name="conclusion"></a>Sonuç

Şimdi göz bizim tasarımın farklı sürümlere göre ekledik genel performans ve ölçeklendirilebilirlik geliştirmeleri vardır.

| | V1 | V2 | V3 |
| --- | --- | --- | --- |
| **[C1]** | 7 ms / 5.71 RU | 7 ms / 5.71 RU | 7 ms / 5.71 RU |
| **[Q1]** | 2 ms / 1 RU | 2 ms / 1 RU | 2 ms / 1 RU |
| **[C2]** | 9 ms / 8.76 RU | 9 ms / 8.76 RU | 9 ms / 8.76 RU |
| **[Q2]** | 9 ms / 19.54 RU | 2 ms / 1 RU | 2 ms / 1 RU |
| **[Q3]** | 130 ms / 619.41 RU | 28 ms / 201.54 RU | 4 ms / 6.46 RU |
| **[C3]** | 7 ms / 8.57 RU | 7 ms / 15.27 RU | 7 ms / 15.27 RU |
| **[4]** | 23 ms / 27.72 RU | 4 ms / 7.72 RU | 4 ms / 7.72 RU |
| **[C4]** | 6 ms / 7.05 RU | 7 ms / 14.67 RU | 7 ms / 14.67 RU |
| **[Q5]** | 59 ms / 58.92 RU | 4 ms / 8.92 RU | 4 ms / 8.92 RU |
| **[Q6]** | 306 ms / 2063.54 RU | 83 ms / 532.33 RU | 9 ms / 16.97 RU |

### <a name="we-have-optimized-a-read-heavy-scenario"></a>Okuma yoğunluklu senaryo iyileştirdik

Biz (komutlar) yazma isteklerini çoğaltamaz (sorgular) okuma isteklerinin performansını geliştirmeye yönelik çalışmalarımız yoğunlaşmıştır fark etmiş olabilirsiniz. Çoğu durumda, yazma işlemlerini daha hesaplama açısından pahalı ve uzun iyileştirilene yapan sonraki normalleştirilmişlikten çıkarma değişiklik akışlarından aracılığıyla şimdi Tetikle.

Bu bir blog oluşturma platformu (gibi en sosyal uygulamalar) sunmak için sahip okuma isteklerinin miktarı genellikle kat yazma isteklerini tutardan daha yüksek olduğu anlamına gelir okuma-ağır, olduğundan emin olarak düzeltilir. Yazma isteklerine izin çalıştırılacak daha pahalı hale getirmek için mantıklı şekilde okuma isteklerinin ucuz ve daha iyi gerçekleştiriliyor.

Biz en aşırı iyileştirme uyguladığımız, bakarsanız **[S6]** 2000 + için yalnızca 17 RU RU oluştu; Bu gönderiler maliyetiyle öğesi başına yaklaşık 10 RU, normal durumdan çıkarmayı alanımız. Biz oluşturmayı veya güncelleştirmeleri gönderilerin daha çok fazla akış isteklere hizmet gibi bu normalleştirilmişlikten çıkarma maliyetini genel tasarruf dikkate alarak göz ardı edilebilir.

### <a name="denormalization-can-be-applied-incrementally"></a>Normalleştirilmişlikten çıkarma artımlı olarak uygulanabilir

Biz bu makalede incelediniz ölçeklenebilirlik geliştirmeleri normalleştirilmişlikten çıkarma ve veri çoğaltma veri kümesi arasında içerir. Bu iyileştirmeler 1 günde bir yere koymak gerekmez, bu unutulmamalıdır. Bölüm anahtarları üzerinde filtre sorgularını uygun ölçekte daha iyi performans, ancak bölümler arası sorgular nadiren bağlanan veya sınırlı bir veri kümesine karşı çağrılırlarsa tümüyle kabul edilebilir. Bu geliştirmeler, daha sonra kullanmak üzere yeni bir prototip oluştururken, veya taban küçük ve denetlenen kullanıcı bir ürünle başlatılıyor, büyük olasılıkla yedek; önemli olan için ise [İzleyici](use-metrics.md) bunları almak için zamanı açmadıklarını ve ne zaman karar verebilmek için modelinizin performans.

Tüm bunları güncelleştirmeleri kalıcı olarak başka bir kapsayıcı deposunda güncelleştirmeleri dağıtmak için kullanırız değişiklik akışı. Bu, sisteminize zaten çok fazla veri olsa bile tüm güncelleştirmeleri tek seferlik bir yakalama işlemi önyükleme normalleştirilmişlikten çıkarılmış görünümler ve kapsayıcı oluşturulduktan sonra isteği mümkün kılar.

## <a name="next-steps"></a>Sonraki adımlar

Pratik veri modelleme ve bölümleme bu giriş sonra kapsamına kavramlarını gözden geçirmek için aşağıdaki makaleleri denetlemekte isteyebilirsiniz:

- [Veritabanları, kapsayıcılar ve öğeleri ile çalışma](databases-containers-items.md)
- [Azure Cosmos DB'de bölümleme](partitioning-overview.md)
- [Azure Cosmos DB'de akış değiştirme](change-feed.md)
