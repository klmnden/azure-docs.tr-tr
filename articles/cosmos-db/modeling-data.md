---
title: Azure Cosmos DB'de veri modelleme
titleSuffix: Azure Cosmos DB
description: NoSQL veritabanlarında veri modelleme, ilişkisel bir veritabanı ve belge veritabanını veri modelleme arasındaki farklar hakkında bilgi edinin.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: rimman
ms.custom: rimman
ms.openlocfilehash: 47d519523c7ffd1c0b6329d6b4eb12b052466b35
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657368"
---
# <a name="data-modeling-in-azure-cosmos-db"></a>Azure Cosmos DB'de modelleme verileri

Gibi Azure Cosmos DB, şemasız veritabanları, süper kolay bir şekilde depolama, yapılandırılmamış ve yarı yapılandırılmış verileri Sorgulama yaparken, performans ve ölçeklenebilirlik açısından hizmet ve en düşük, en iyi şekilde yararlanmak için veri modelinizi hakkında bazı zaman düşünmeye ayırması gerektiğini maliyeti.

Nasıl veri depolanacak geçiyor? Uygulamanızı nasıl almak ve veri sorgulamak için geçiyor? Uygulamanız okuma yoğunluklu mu yazma yoğunluklu mi?

Bu makaleyi okuduktan sonra aşağıdaki soruları yanıtlamak mümkün olacaktır:

* Veri modelleme nedir ve neden miyim dikkat etmelisiniz?
* Azure Cosmos DB'de modelleme verileri ilişkisel bir veritabanı için farklı mı?
* İlişkisel olmayan bir veritabanında veri ilişkileri nasıl express?
* Ben veri siteme ne zaman ve ne zaman veri bağlamayın?

## <a name="embedding-data"></a>Veri ekleme

Azure Cosmos DB'de veri modelleme başlattığınızda varlıklarınızı değerlendirilecek deneyin **müstakil öğeleri** JSON belgeleri olarak temsil edilir.

Karşılaştırma için öncelikle şu ilişkisel veritabanı içindeki veriler nasıl model görelim. Aşağıdaki örnek, bir kişinin ilişkisel bir veritabanında depolanabilir nasıl gösterir.

![İlişkisel veritabanı modeli](./media/sql-api-modeling-data/relational-data-model.png)

İlişkisel veritabanları ile çalışırken, tüm verilerinizi Normalleştir stratejisidir. Verilerinizi genellikle normalleştirme bir kişi gibi bir varlık alma ve ayrık bileşenlerine bozucu içerir. Yukarıdaki örnekte, bir kişi, birden çok adresi kayıtları yanı sıra birden çok kişi ayrıntı kaydı olabilir. İletişim ayrıntıları daha fazla bozulmuş başka ayıklayarak ortak alanları ister bir tür. Aynı adrese geçerlidir, her bir kayıt türü olabilir *giriş* veya *iş*.

Kılavuzluk yapar verileri normalleştirmek için olduğunda **yedekli veri depolanmasını önlemek** her kaydı ve bunun yerine veriye başvurmaktadır. Tüm kişi ayrıntıları ve adresleri olan bir kişi okumak için bu örnekte, çalışma zamanında BİRLEŞTİRMELER verilerinizi etkili bir şekilde geri oluşturun (veya normalleştirilmişlikten çıkarmak için) kullanmanız gerekir.

    SELECT p.FirstName, p.LastName, a.City, cd.Detail
    FROM Person p
    JOIN ContactDetail cd ON cd.PersonId = p.Id
    JOIN ContactDetailType cdt ON cdt.Id = cd.TypeId
    JOIN Address a ON a.PersonId = p.Id

Yazma işlemleri, tek bir kişi, iletişim ayrıntılarını ve adresleri ile güncelleştirilmesi arasında bireysel birçok tabloları gerektirir.

Artık size Azure Cosmos DB'de kendi içinde bir varlık olarak aynı verileri modelleyecek nasıl göz atalım.

    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "addresses": [
            {
                "line1": "100 Some Street",
                "line2": "Unit 1",
                "city": "Seattle",
                "state": "WA",
                "zip": 98012
            }
        ],
        "contactDetails": [
            {"email": "thomas@andersen.com"},
            {"phone": "+1 555 555-5555", "extension": 5555}
        ]
    }

Size yukarıda bir yaklaşım kullanarak **normalleştirilmişlikten çıkarılmış** göre kişinin kaydı **katıştırma** tüm ilgili bilgileri, kişi ayrıntıları ve adresleri gibi bu kişiye içine bir *tek JSON* belge.
Ayrıca, biz için sabit bir şema sınırlı değil çünkü biz farklı şekiller kişi ayrıntılarını tamamen sahip gibi şeyleri esnekliğine sahipsiniz.

Veritabanından bir tam kişi kaydı alınırken, artık bir **tek okuma işlemi** tek bir kapsayıcıya karşı ve tek bir öğe. Bir kişi kaydı, iletişim ayrıntılarını ve adresleri ile güncelleştirilmesi ise ayrıca bir **tek bir yazma işlemi** karşı tek bir öğe.

Veri normal durumdan çıkarmayı, daha az sorgular ve bunun yaygın işlemlerin tamamlanması güncelleştirmeleri uygulamanız gerekebilir.

### <a name="when-to-embed"></a>Ekleme zamanı

Genel olarak, katıştırılmış veri kullanın ne zaman modelleri:

* Vardır **bulunan** varlıklar arasında ilişkiler.
* Vardır **bir birkaç** varlıklar arasında ilişkiler.
* Katıştırılmış veri, **seyrek değişen**.
* Değil büyüyecektir katıştırılmış veri **bağlı olmadan**.
* Katıştırılmış veriler var. **birlikte sık Sorgulanmış**.

> [!NOTE]
> Genellikle, veri modelleri sağlamak daha iyi çıkarılmışsa **okuma** performans.

### <a name="when-not-to-embed"></a>Ne zaman değil ekleme

Azure Cosmos DB'de kural karşısında her şeyi normalleştirilmişlikten çıkarmak ve tüm veriler tek bir öğede olsa da bu kaçınılmalıdır bazı durumlar için açabilir.

Bu JSON parçacığı yararlanın.

    {
        "id": "1",
        "name": "What's new in the coolest Cloud",
        "summary": "A blog post by someone real famous",
        "comments": [
            {"id": 1, "author": "anon", "comment": "something useful, I'm sure"},
            {"id": 2, "author": "bob", "comment": "wisdom from the interwebs"},
            …
            {"id": 100001, "author": "jane", "comment": "and on we go ..."},
            …
            {"id": 1000000001, "author": "angry", "comment": "blah angry blah angry"},
            …
            {"id": ∞ + 1, "author": "bored", "comment": "oh man, will this ever end?"},
        ]
    }

Bu, biz tipik bir blog veya CMS, sistem modelleme, gibi ne yorumlar embedded post varlıkla görünür olabilir. Bu örnekte sorun açıklamaları dizisi olmasıdır **sınırsız**, herhangi tek bir posta olabilir açıklama sayısını (pratik) sınır olmadığını anlamına gelir. Öğenin boyutunu sonsuz büyük bağlı olarak bu bir sorun olabilir.

Öğenin boyutunu kablo yanı sıra okuma ve uygun ölçekte, öğe güncelleştirme üzerinden veri iletmek için özelliği büyüdükçe etkilenir.

Bu durumda, aşağıdaki veri modeli dikkate alınması gereken daha iyi olur.

    Post item:
    {
        "id": "1",
        "name": "What's new in the coolest Cloud",
        "summary": "A blog post by someone real famous",
        "recentComments": [
            {"id": 1, "author": "anon", "comment": "something useful, I'm sure"},
            {"id": 2, "author": "bob", "comment": "wisdom from the interwebs"},
            {"id": 3, "author": "jane", "comment": "....."}
        ]
    }

    Comment items:
    {
        "postId": "1"
        "comments": [
            {"id": 4, "author": "anon", "comment": "more goodness"},
            {"id": 5, "author": "bob", "comment": "tails from the field"},
            ...
            {"id": 99, "author": "angry", "comment": "blah angry blah angry"}
        ]
    },
    {
        "postId": "1"
        "comments": [
            {"id": 100, "author": "anon", "comment": "yet more"},
            ...
            {"id": 199, "author": "bored", "comment": "will this ever end?"}
        ]
    }

Bu modelde sabit bir öznitelik kümesini sahip olan bir dizi post kapsayıcı katıştırılmış üç en son açıklamalar yok. Diğer açıklamaları 100 yorumların toplu gruplandırılan ve ayrı öğeleri olarak depolanır. Kurgusal uygulamamız bir kerede 100 açıklamaları yükleyen kullanıcı izin verdiğinden, toplu iş boyutu 100 seçildi.  

Katıştırılmış veri öğeleri arasında sık sık kullanılır ve sık sık değişir katıştırma verileri yararlı olduğu başka bir durumdur.

Bu JSON parçacığı yararlanın.

    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "holdings": [
            {
                "numberHeld": 100,
                "stock": { "symbol": "zaza", "open": 1, "high": 2, "low": 0.5 }
            },
            {
                "numberHeld": 50,
                "stock": { "symbol": "xcxc", "open": 89, "high": 93.24, "low": 88.87 }
            }
        ]
    }

Bu, bir kişinin stok Portföy temsil eder. Her Portföy belgesine hisse senedi bilgi eklemek seçtik. İlgili veriler sıklıkla değişiyorsa olduğu bir ortamda uygulama alım-satım, sık sık değişen veriler Katıştırılıyor stok gibi bir hisse senedi satılan her zaman her Portföy belge sürekli olarak güncelleştirdiğiniz anlamına zordur.

Hisse senedi *zaza* yüzlerce kez tek bir satılan gün ve binlerce kullanıcıya sahip olabilir *zaza* Portföyüne üzerinde. Yukarıdaki birçok kez yüzlerce Portföy belgeleri güncelleştirmek için gerekir gibi bir veri modeli ile bir sisteme önde gelen her gün, iyi ölçeklendirme olmaz.

## <a name="referencing-data"></a>Başvuru verileri

Veriler Katıştırılıyor güzelce için çoğu durumda çalışır ancak bazı senaryolarda verilerinizi normal durumdan çıkarmayı değer olandan daha fazla sorunlara neden. Bu nedenle biz şimdi ne yapacaksınız?

İlişkisel veritabanları, varlıklar arasındaki ilişkilerin oluşturabileceğiniz tek yer değildir. Bir belge veritabanında bir belgedeki başka belgelerde verilerle ilgili bilgi olabilir. Azure Cosmos DB ilişkisel bir veritabanında veya başka bir belge veritabanında daha uygun sistemler oluşturmaya önerilmez, ancak basit ilişkiler bir sakınca yoktur ve yararlı olabilir.

Daha önce bir stok Portföyünden örneği kullanmak için seçtik aşağıdaki JSON ancak bu kez katıştırmak yerine Portföy stok öğede diyoruz. Stok öğesi gün içinde sık güncelleştirilmesi gereken yalnızca belge değiştiğinde bu şekilde, tek bir hisse senedi belge olur.

    Person document:
    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "holdings": [
            { "numberHeld":  100, "stockId": 1},
            { "numberHeld":  50, "stockId": 2}
        ]
    }

    Stock documents:
    {
        "id": "1",
        "symbol": "zaza",
        "open": 1,
        "high": 2,
        "low": 0.5,
        "vol": 11970000,
        "mkt-cap": 42000000,
        "pe": 5.89
    },
    {
        "id": "2",
        "symbol": "xcxc",
        "open": 89,
        "high": 93.24,
        "low": 88.87,
        "vol": 2970200,
        "mkt-cap": 1005000,
        "pe": 75.82
    }

Bu yaklaşımın hemen bir dezavantajı, ancak uygulamanızı bir kişinin Portföy görüntülenirken tutulan her hisse senedi hakkındaki bilgileri gösterecek şekilde gerekli olup olmadığını olan; Bu durumda veritabanına stok her belge için bilgileri yüklemek için birden fazla dönüş yapmak gerekir. Burada, gün içinde sık gerçekleşir, ancak büyük olasılıkla bu belirli bir sistemi performansı üzerinde daha az etkisi olan okuma işlemleri üzerindeki sırayla tehlikeye yazma işlemlerinin verimliliğini artırmak için bir karar yaptık.

> [!NOTE]
> Normalleştirilmiş veri modelleri **daha fazla gidiş dönüş gerektirebilir** sunucu.

### <a name="what-about-foreign-keys"></a>Yabancı anahtarlar hakkında neler diyeceksiniz?

Şu anda bir kısıtlama kavramı olduğundan, yabancı anahtar veya aksi takdirde belgelerde sahip belge arası ilişkileri etkili bir şekilde "zayıf bağlantılar" ve veritabanı tarafından doğrulanmaz. Bir belge gerçekten var başvuran veri emin olmak, uygulamanızdaki veya sunucu tarafı Tetikleyicileri veya saklı yordamları Azure Cosmos DB üzerinde kullanarak bunu gerekir.

### <a name="when-to-reference"></a>Zaman başvurmak için

Genel olarak, normalleştirilmiş verileri kullanma ne zaman modelleri:

* Temsil eden **-çok** ilişkileri.
* Temsil eden **çoktan çoğa** ilişkileri.
* İlgili verileri **sık sık değişen**.
* Başvurulan veri olabilir **sınırsız**.

> [!NOTE]
> Genellikle normalleştirme sağlayan daha iyi **yazma** performans.

### <a name="where-do-i-put-the-relationship"></a>İlişki nereye yerleştirmeniz gerekir?

İlişkinin büyüme, başvuruyu depolanacağı hangi belge belirlemenize yardımcı olur.

Biz, Yayımcılar ve kitapları modeller JSON yanıtına aşağıdaki bakarsanız.

    Publisher document:
    {
        "id": "mspress",
        "name": "Microsoft Press",
        "books": [ 1, 2, 3, ..., 100, ..., 1000]
    }

    Book documents:
    {"id": "1", "name": "Azure Cosmos DB 101" }
    {"id": "2", "name": "Azure Cosmos DB for RDBMS Users" }
    {"id": "3", "name": "Taking over the world one JSON doc at a time" }
    ...
    {"id": "100", "name": "Learn about Azure Cosmos DB" }
    ...
    {"id": "1000", "name": "Deep Dive into Azure Cosmos DB" }

Yayımcı başına books sayısı ile sınırlı büyüme küçükse, ardından yayımcı belge içinde kitap başvuru depolamak yararlı olabilir. Yayımcı başına books sayısını sınırsız olarak ise, ancak daha sonra bu veri modeli Yukarıdaki örnek yayımcı belgeyi olduğu gibi değişebilir, büyüyen dizilerine sunulmasını sağlar.

Bir bit etrafında şeyler geçiş hala aynı verileri temsil eder, ancak artık bu büyük değişmez koleksiyonlara kaçınır modeli neden olur.

    Publisher document:
    {
        "id": "mspress",
        "name": "Microsoft Press"
    }

    Book documents:
    {"id": "1","name": "Azure Cosmos DB 101", "pub-id": "mspress"}
    {"id": "2","name": "Azure Cosmos DB for RDBMS Users", "pub-id": "mspress"}
    {"id": "3","name": "Taking over the world one JSON doc at a time"}
    ...
    {"id": "100","name": "Learn about Azure Cosmos DB", "pub-id": "mspress"}
    ...
    {"id": "1000","name": "Deep Dive into Azure Cosmos DB", "pub-id": "mspress"}

Yukarıdaki örnekte, biz yayımcı belge üzerinde sınırsız koleksiyon bırakılan. Bunun yerine yalnızca yayımcı başvuru her kitap belgeyi sahibiz.

### <a name="how-do-i-model-manymany-relationships"></a>Çok: çok ilişkileri nasıl model?

İlişkisel bir veritabanındaki *çok: çok* ilişkiler genellikle yalnızca diğer tablodaki kayıtların birlikte katılın birleştirme tablolarla modellenir.

![Tabloları birleştirme](./media/sql-api-modeling-data/join-table.png)

Belgeleri kullanarak aynı şeyi çoğaltmak, aşağıdakine benzer bir veri modeli oluşturmak için fikri size cazip olabilir.

    Author documents:
    {"id": "a1", "name": "Thomas Andersen" }
    {"id": "a2", "name": "William Wakefield" }

    Book documents:
    {"id": "b1", "name": "Azure Cosmos DB 101" }
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users" }
    {"id": "b3", "name": "Taking over the world one JSON doc at a time" }
    {"id": "b4", "name": "Learn about Azure Cosmos DB" }
    {"id": "b5", "name": "Deep Dive into Azure Cosmos DB" }

    Joining documents:
    {"authorId": "a1", "bookId": "b1" }
    {"authorId": "a2", "bookId": "b1" }
    {"authorId": "a1", "bookId": "b2" }
    {"authorId": "a1", "bookId": "b3" }

Bu işe yarar. Ancak, kendi kitaplarıyla yazar ya da yükleme veya yazarı ile bir kitap yüklerken her zaman veritabanında en az iki ek sorgular gerekir. Katılma belge ve birleştirilen gerçek belge getirmek için başka bir sorgu için bir sorgu.

Tüm bu birleşim tablosundan yapıyor yapıştırma, birlikte iki veri parçasını ardından neden tamamen kaldırın?
Aşağıdakileri göz önünde bulundurun.

    Author documents:
    {"id": "a1", "name": "Thomas Andersen", "books": ["b1, "b2", "b3"]}
    {"id": "a2", "name": "William Wakefield", "books": ["b1", "b4"]}

    Book documents:
    {"id": "b1", "name": "Azure Cosmos DB 101", "authors": ["a1", "a2"]}
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users", "authors": ["a1"]}
    {"id": "b3", "name": "Learn about Azure Cosmos DB", "authors": ["a1"]}
    {"id": "b4", "name": "Deep Dive into Azure Cosmos DB", "authors": ["a2"]}

Artık, ı yazar varsa hemen yazılmış hangi kitapları biliyorum ve ben yüklenen bir kitap belge varsa buna karşılık yazarları kimliklerini biliyorum. Bu, sunucu sayısını gidiş dönüşleri uygulamanızın yapması gereken tablo birleştirme azaltma karşı Ara sorgu kaydeder.

## <a name="hybrid-data-models"></a>Karma veri modelleri

Biz artık inceledik katıştırma (veya normal durumdan çıkarmayı) ve veri başvuru (veya normalleştirilmesi), her sahip kullanıcıların upsides ve her ödün anlatıldığı gibi.

Her zaman ya da olması gerekmez ya da biraz karışık şeyler Korkmuş çekinmeyin.

Uygulamanızın belirli kullanım desenleri ve iş yüklerini burada karıştırma katıştırılmış durumlar olabilir göre başvurulan veri anlamlı ve iyi bir performans düzeyi faydalanırken, sağlama daha az sunucu ile basit uygulama mantığı gelişlerin yuvarlak.

Aşağıdaki JSON göz önünde bulundurun.

    Author documents:
    {
        "id": "a1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "countOfBooks": 3,
        "books": ["b1", "b2", "b3"],
        "images": [
            {"thumbnail": "https://....png"}
            {"profile": "https://....png"}
            {"large": "https://....png"}
        ]
    },
    {
        "id": "a2",
        "firstName": "William",
        "lastName": "Wakefield",
        "countOfBooks": 1,
        "books": ["b1"],
        "images": [
            {"thumbnail": "https://....png"}
        ]
    }

    Book documents:
    {
        "id": "b1",
        "name": "Azure Cosmos DB 101",
        "authors": [
            {"id": "a1", "name": "Thomas Andersen", "thumbnailUrl": "https://....png"},
            {"id": "a2", "name": "William Wakefield", "thumbnailUrl": "https://....png"}
        ]
    },
    {
        "id": "b2",
        "name": "Azure Cosmos DB for RDBMS Users",
        "authors": [
            {"id": "a1", "name": "Thomas Andersen", "thumbnailUrl": "https://....png"},
        ]
    }

Burada (çoğunlukla) burada diğer varlıkların verilerden en üst düzey belgeye gömülü, ancak diğer veri başvurulan ekli model izlenen.

Kitap belge bakarsanız, birkaç görebiliriz yazarlar dizisi baktığımızda, alanlar ilginç. Var olan bir `id` alan alanın geri standart uygulama normalleştirilmiş bir modelde bir yazar belgesi başvurmak için kullanıyoruz ancak biz de sahip `name` ve `thumbnailUrl`. Biz ile takılmış `id` ve ilgili Yazar belge "bağlantı" kullanılarak gerekli herhangi bir ek bilgi almak için uygulama ekledi ancak uygulamamız yazarın adı ve her bir kitap ile küçük bir resim görüntüler. Biz kaydedebilir gidiş dönüş kitap bir listedeki her sunucuya normal durumdan çıkarmayı tarafından görüntülenen **bazı** veri yazar.

Emin olun, yazarın adı değiştirilmiş ya da fotoğrafını güncelleştirme istedikleri Git ve sürekli yayınlanmış ancak uygulamamız için yazarlar adları genellikle değişmez varsayımına dayanarak her kitabın güncelleştirmek gerekir, bu kabul edilebilir bir tasarım kararıdır.  

Örnekte, vardır **toplamalar'önceden hesaplanan** okuma işlemi pahalı işleme kaydetmek için değerler. Örnekte, yazar belgeye gömülü verilerin bazıları, çalışma zamanında hesaplanır verilerdir. Yeni bir kitap her yayımlandığında bir kitap belge oluşturulur **ve** countOfBooks alanı belirli bir yazar için mevcut kitap belgelerin göre hesaplanan bir değere ayarlanır. Bu iyileştirme biz okuma iyileştirmek için yazma işlemleri üzerinde hesaplamalar yapmak burada gücünüze okuma yoğun sistemleri yararlı olabilir.

Azure Cosmos DB desteklediğinden önceden hesaplanan alanlar modeliyle yeteneğini mümkün hale getirilir **çok belgeli işlemler**. Birçok NoSQL deposu, belgeler arasında işlemleri yapmak ve tasarım kararları, "her zaman her şeyi, bu sınırlama nedeniyle ekleme" gibi bu nedenle Danışmanı. Azure Cosmos DB ile sunucu tarafı Tetikleyiciler veya books ekleyebileceğiniz ve yazarlar bir ACID işlemi içinde tüm güncelleştirme saklı yordamları kullanabilirsiniz. Sizin artık **sahip** her şeyi yalnızca verilerinizi tutarlı kalmasından emin olmak için bir belgeye gömülecek.

## <a name="distinguishing-between-different-document-types"></a>Farklı belge türleri arasında ayrım

Bazı senaryolarda, aynı koleksiyonda farklı belge türlerinin karışımı isteyebilirsiniz; birden çok, istediğiniz zaman bu karşılaşılır ilgili belgeler için de aynı oturmak [bölüm](partitioning-overview.md). Örneğin, hem books put kitap aynı koleksiyonda incelemeler ve tarafından bölümleme `bookId`. Böyle bir durumda, genellikle belgelerinizi bunları ayırt etmek için kendi türünü tanımlayan bir alan eklemek istersiniz.

    Book documents:
    {
        "id": "b1",
        "name": "Azure Cosmos DB 101",
        "bookId": "b1",
        "type": "book"
    }

    Review documents:
    {
        "id": "r1",
        "content": "This book is awesome",
        "bookId": "b1",
        "type": "review"
    },
    {
        "id": "r2",
        "content": "Best book ever!",
        "bookId": "b1",
        "type": "review"
    }

## <a name="next-steps"></a>Sonraki adımlar

Şemadan bağımsız bir dünyada modelleme verileri olarak şimdiye kadar önemli olduğunu anlamak için bu makaledeki büyük paketler var.

Yalnızca olmadığından bir ekranda veri parçasını temsil etmek için tek bir yolu, verilerinizi modellemek için tek bir yolu yoktur. Uygulamanızı ve nasıl oluşturacak, anlamak için kullanma ve verileri işlemek. Ardından, burada sunulan yönergeler uygulanarak, uygulamanızın anında ihtiyaçlarını gideren bir model oluşturma hakkında ayarlayabilirsiniz. Uygulamalarınızı değiştirmeniz gerektiğinde değiştirebilir ve veri modelinizin bir kolayca gelişmek benimsemek için bir şemasız veritabanı esnekliğinden yararlanabilir.

Azure Cosmos DB hakkında daha fazla bilgi için hizmetin başvuran [belgeleri](https://azure.microsoft.com/documentation/services/cosmos-db/) sayfası.

Bir parçaya verilerinizin birden çok bölüm nasıl bakın anlamak için [Azure Cosmos DB'de bölümleme veri](sql-api-partition-data.md).
