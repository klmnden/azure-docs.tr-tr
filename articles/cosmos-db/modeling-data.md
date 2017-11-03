---
title: "Belge verileri için bir NoSQL veritabanı modelleme | Microsoft Docs"
description: "NoSQL veritabanları için veriler modelleme hakkında bilgi edinin"
keywords: Veri modelleme
services: cosmos-db
author: arramac
manager: jhubbard
editor: mimig1
documentationcenter: 
ms.assetid: 69521eb9-590b-403c-9b36-98253a4c88b5
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2016
ms.author: arramac
ms.openlocfilehash: 16c387fe574243544cf54cf283c7713ddcaa1942
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="modeling-document-data-for-nosql-databases"></a>Belge verileri NoSQL veritabanları için modelleme
Azure Cosmos DB gibi şemasız veritabanı yaparken veri modelinizi değişiklikler kucaklamaya Süper kolay hala bazı zaman düşünmeye verilerinizle ilgili harcadığınız. 

Nasıl veri depolanması gittiği? Uygulamanızı nasıl almak ve verileri sorgulamak için geçiyor? Uygulamanızı koyu okuma veya yazma ağır mi? 

Bu makaleyi okuduktan sonra aşağıdaki soruları yanıtlayın mümkün olacaktır:

* Nasıl bir belge veritabanı belgede dikkat etmeniz?
* Veri modellemesi nedir ve neden ı dikkat etmelisiniz? 
* Bir belge veritabanındaki modelleme verileri ilişkisel bir veritabanına farklı mı?
* Veri ilişkileri ilişkisel olmayan bir veritabanında nasıl express?
* Ne zaman ı verileri katıştır ve ne zaman veri bağlamayın?

## <a name="embedding-data"></a>Veri katıştırma
Varlıklarınızı davranmanız Azure Cosmos DB gibi bir belge deposundaki verileri modelleme başlattığınızda deneyin **kendi içinde bulunan belgeleri** JSON'da temsil.

Biz daha yakından inceleyin önce çok daha çok, bize birkaç adımı yeniden alın ve nasıl biz bize çoğunu bilginiz konu ilişkisel bir veritabanındaki bir şey model bir göz gerekir. Aşağıdaki örnek, bir kişinin ilişkisel bir veritabanına nasıl depolanabilir gösterir. 

![İlişkisel veritabanı modeli](./media/documentdb-modeling-data/relational-data-model.png)

İlişkisel veritabanları ile çalışırken, biz normalleştirmek için normalleştirin, normalleştirin yıldır öğrettin.

Verilerinizi genellikle normalleştirme bir kişi gibi bir varlık alma ve onu içinde veri ayrık parçalarını çiğnemekten içerir. Yukarıdaki örnekte, bir kişi, birden çok kişi ayrıntı kaydı gibi birden çok adres kaydı olabilir. Biz bile bir adım daha fazla gidin ve başka çıkartarak kişi ayrıntılarını ortak bölün alanları ister bir türü. Aynı adresi için burada her kaydı gibi bir türe sahip *giriş* veya *iş* 

Kılavuzluk yapar verileri normalleştirmek için olduğunda **yedek verileri depolamak kaçının** her kaydetmek ve bunun yerine veri bakın. Bu örnekte, iletişim ayrıntılarını ve adresleri olan bir kişi okumak için BİRLEŞİMLER, verilerinizi çalışma zamanında etkili bir şekilde toplamak için kullanmanız gerekebilir.

    SELECT p.FirstName, p.LastName, a.City, cd.Detail
    FROM Person p
    JOIN ContactDetail cd ON cd.PersonId = p.Id
    JOIN ContactDetailType on cdt ON cdt.Id = cd.TypeId
    JOIN Address a ON a.PersonId = p.Id

Tek bir kişinin kendi ilgili kişi ayrıntıları ve adreslerinin güncelleştirme yazma işlemleri, birçok tek tek tablolar arasında gerektirir. 

Şimdi biz bir belge veritabanında kendi içinde bulunan bir varlık aynı verilere nasıl saptayacaktır bir bakalım.

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
            {"email: "thomas@andersen.com"},
            {"phone": "+1 555 555-5555", "extension": 5555}
        ] 
    }

Şimdi sahibiz yaklaşımı yukarıdaki kullanarak **Normalleştirilmemiş** kişi kayıt where biz **katıştırılmış** kendi ilgili kişi ayrıntıları ve adresleri gibi bu kişinin içinde tek bir JSON belgesi ilgili tüm bilgileri.
Biz sabit bir şemaya sınırlı değil çünkü ek olarak, biz farklı şekillerdeki kişi ayrıntılarını tamamen olması gibi şeyler için esnekliğe sahip olursunuz. 

Tam kişi kaydı veritabanından alma işlemi tek bir koleksiyon ve tek bir belgenin okuma tek bir sunulmuştur. Bir kişi kaydı, iletişim ayrıntılarını ve adresleri güncelleştirmek tek bir belgenin karşı tek bir yazma işlemi de kullanılır.

Veri denormalizing tarafından daha az sorgular ve ortak işlemleri tamamlamak için güncelleştirmeleri vermek uygulamanız gerekebilir. 

### <a name="when-to-embed"></a>Ne zaman katıştırma
Genel olarak, katıştırılmış veri kullanmak ne zaman modelleri:

* Vardır **içeren** varlıklar arasındaki ilişkiler.
* Vardır **bir birkaç** varlıklar arasındaki ilişkiler.
* Katıştırılmış veri, **değişmeyen**.
* Var. katıştırılmış veri olmaz büyümesine **bağlı olmadan**.
* Ekli veriler var. **tam sayı** bir belgedeki verileri için.

> [!NOTE]
> Veri modelleri sağlayan daha iyi normal genellikle dışı **okuma** performans.
> 
> 

### <a name="when-not-to-embed"></a>Ne zaman değil katıştırma
Her şeyi denormalize ve tüm verileri tek bir belgeye katıştırmak için bir belge veritabanında altın kural olmakla birlikte, bu kaçınılmalıdır bazı durumlarda neden olabilir.

Bu JSON parçacığı alın.

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

Bu, biz tipik bir blog veya CMS, sistem modelleme varsa gibi ne katıştırılmış açıklamaları post varlıkla görünür olabilir. Bu örnek sorun açıklamaları dizi olmasıdır **sınırsız**, tek bir post olabilir yorum sayısını (pratik) sınır olmadığını anlamına gelir. Belgenin boyutunu önemli ölçüde büyüdükçe bu bir sorun olacaktır.

Kablo yanı sıra okuma ve ölçekli olarak belge güncelleştirme üzerinden veri iletme yeteneği belgesinin boyutu büyüdükçe etkilenir.

Bu durumda aşağıdaki model dikkate alınması gereken daha iyi olur.

    Post document:
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

    Comment documents:
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

Bu model en son üç sahip bir sabit ile bir dizi olan posta kendisini katıştırılmış açıklamaları bağlı bu süre. Diğer açıklamaları 100 açıklamaları toplu işlemler için gruplandırılan ve ayrı belgelerde depolanır. Bir kerede 100 açıklamaları yükleyen kullanıcı kurgusal uygulamamız izin verdiği için toplu iş boyutu 100 olarak seçildi.  

Katıştırılmış veri belgeler arasında genellikle kullanıldığında ve sık sık değişiklik yapacağınız katıştırma veri iyi bir fikir olduğu başka bir durumdur. 

Bu JSON parçacığı alın.

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

Bu kişinin stok Portföy temsil eder. Her Portföy belgeye stok bilgileri katıştırmak seçtiniz. Burada ilgili verileri sık değişen bir ortamda, uygulama ticaret, sık sık değişen verileri katıştırma stock gibi bir hisse senedi ticareti her zaman her Portföy belge sürekli güncelleştirdiğiniz anlamına zordur.

Hisse senedi *zaza* tek bir kez yüzlerce ticareti gün ve binlerce kullanıcıyı sahip olabilir *zaza* kendi Portföy üzerinde. Yukarıdaki birçok kez binlerce Portföy belgeleri güncelleştirmek için olurdu gibi bir veri modeli ile bir sisteme önde gelen her gün, çok iyi ölçeklendirme çalışmaz. 

## <a id="Refer"></a>Veri başvurma
Bu nedenle, veri katıştırma sorunsuz şekilde için çoğu zaman çalışır ancak verilerinizi denormalizing değerinde olandan daha fazla sorunlara neden olduğunda senaryoları olduğunu işaretlenmemiştir. Bu nedenle ne şimdi yapmak istersiniz? 

İlişkisel veritabanları varlıklar arasındaki ilişkiler oluşturabileceğiniz tek yer değildir. Bir belge veritabanında gerçekten başka belgelerde verilerle ilgili bir belgedeki bilgi olabilir. Şimdi, ı bile bir dakika için size bir ilişkisel veritabanı Azure Cosmos veritabanı ya da herhangi bir belge veritabanı için daha uygun sistemler oluşturabilir, ancak basit ilişkileri ince ve çok kullanışlı olabilir advocating değil. 

Daha önce bir stok Portföy örneği kullanmak için seçtik JSON aşağıdaki ancak bu kez biz katıştırmak yerine Portföy stok öğede bakın. Stok öğesi güncelleştirilmesi gerekiyor yalnızca belge gün içinde sık değiştiğinde bu şekilde, tek stok belge olur. 

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


Bu yaklaşımın hemen bir dezavantajı, ancak uygulamanızı kişinin Portföy görüntülenirken tutulan her hisse senedi hakkındaki bilgileri görüntülemek için gerekli olup olmadığını olan; Bu durumda, birden çok dönüşleri stok her belge için bilgileri yüklemek için veritabanına yapmanız gerekir. Gün içinde sık gerçekleşir, ancak büyük olasılıkla daha az bu belirli sistem performansına etkisi okuma işlemleri üzerindeki sırayla tehlikeye yazma işlemleri verimliliğini artırmak için bir karar burada yaptık.

> [!NOTE]
> Normalleştirilmiş veri modelleri **daha fazla gidiş dönüş gerektiren** sunucuya.
> 
> 

### <a name="what-about-foreign-keys"></a>Yabancı anahtarlar nasıldır?
Şu anda bir kısıtlama kavramına olduğundan, yabancı anahtar veya aksi takdirde belgelerde sahip herhangi bir arası belge ilişki etkili bir şekilde "zayıf bağlantılar" ve veritabanı tarafından doğrulanmaz. Bir belge gerçekten var başvurma veri emin olmak istiyorsanız, bu, uygulamanızda veya sunucu tarafı Tetikleyiciler veya Azure Cosmos DB saklı yordamları kullanarak yapmanız gerekir.

### <a name="when-to-reference"></a>Başvuru zamanı
Genel olarak, normalleştirilmiş verileri kullanmak ne zaman modelleri:

* Temsil eden **bir çok** ilişkiler.
* Temsil eden **çok-** ilişkiler.
* İlgili verileri **sık sık değişen**.
* Başvurulan veri olabilir **sınırsız**.

> [!NOTE]
> Genellikle normalleştirme sağlar daha iyi **yazma** performans.
> 
> 

### <a name="where-do-i-put-the-relationship"></a>İlişki burada put?
İlişkinin büyüme başvuru depolamak için hangi belgede belirlemenize yardımcı olacaktır.

Biz Yayımcılar ve books modellerin JSON aşağıdaki bakarsanız.

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
    {"id": "1000", "name": "Deep Dive in to Azure Cosmos DB" }

Yayımcı başına books sayısı sınırlı büyümesiyle küçükse, yayımcı belge içindeki kitap başvuru depolama yararlı olabilir. Yayımcı başına books sayısı sınırsız ise, ancak, ardından bu veri modeli Yukarıdaki örnek yayımcı belge olduğu değişebilir, büyüyen dizilerine sunulmasını sağlar. 

Bir bit geçici şeyler geçiş hala aynı verileri temsil eder, ancak şimdi bu büyük değişebilir koleksiyonları önler modeli neden olur.

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
    {"id": "1000","name": "Deep Dive in to Azure Cosmos DB", "pub-id": "mspress"}

Yukarıdaki örnekte, biz yayımcı belge üzerinde sınırsız koleksiyonu bırakılan. Bunun yerine yalnızca sahip olduğumuz bir yayımcı her kitap belge üzerinde referansı.

### <a name="how-do-i-model-manymany-relationships"></a>Çok: ilişkileri nasıl model?
İlişkisel bir veritabanındaki *çok:* ilişkileri genellikle yalnızca diğer tablolardan kayıtları birlikte katılma birleştirme tablolarla modellenir. 

![Tabloları birleştirme](./media/documentdb-modeling-data/join-table.png)

Belgeleri kullanarak aynı şeyi çoğaltabilir ve aşağıdakine benzer bir veri modeli oluşturmak için isteği duyabilirsiniz.

    Author documents: 
    {"id": "a1", "name": "Thomas Andersen" }
    {"id": "a2", "name": "William Wakefield" }

    Book documents:
    {"id": "b1", "name": "Azure Cosmos DB 101" }
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users" }
    {"id": "b3", "name": "Taking over the world one JSON doc at a time" }
    {"id": "b4", "name": "Learn about Azure Cosmos DB" }
    {"id": "b5", "name": "Deep Dive in to Azure Cosmos DB" }

    Joining documents: 
    {"authorId": "a1", "bookId": "b1" }
    {"authorId": "a2", "bookId": "b1" }
    {"authorId": "a1", "bookId": "b2" }
    {"authorId": "a1", "bookId": "b3" }

Bu çalışır. Ancak, bir ya da yazar kendi kitaplarıyla yüklenirken veya kitap yazarı ile yükleme her zaman en az iki ek sorgular veritabanında gerektirir. Bir sorgu katılma belgeye ve birleştirilen gerçek belge getirmek için başka bir sorgu. 

Tüm bu birleştirme tablo yaptığını yapıştırma, birlikte veri iki parça sonra neden tamamen kaldırın?
Aşağıdakileri göz önünde bulundurun.

    Author documents:
    {"id": "a1", "name": "Thomas Andersen", "books": ["b1, "b2", "b3"]}
    {"id": "a2", "name": "William Wakefield", "books": ["b1", "b4"]}

    Book documents: 
    {"id": "b1", "name": "Azure Cosmos DB 101", "authors": ["a1", "a2"]}
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users", "authors": ["a1"]}
    {"id": "b3", "name": "Learn about Azure Cosmos DB", "authors": ["a1"]}
    {"id": "b4", "name": "Deep Dive in to Azure Cosmos DB", "authors": ["a2"]}

Artık bir yaz güncelleştirmeniz varsa hemen yazılmış hangi books bilebilirim ve yüklenen bir kitap belge sahipse, buna karşılık yazarları kimliklerini bilebilirim. Bu sunucunun numarası gidiş dönüşleri yapmak için uygulamanızın sahip birleştirme tablosu azaltma karşı Ara sorgu kaydeder. 

## <a id="WrapUp"></a>Karma veri modelleri
Biz şimdi inceledik katıştırma (veya denormalizing) ve başvuru (veya normalleştirme) veri her varsa bunların upsides ve her güvenlik ihlalleri anlatıldığı gibi. 

Her zaman aşağıdakilerden biri olması gerekmez ya da biraz karışık şeyler Korkmuş yok. 

Uygulamanızın belirli kullanım desenlerini ve burada karıştırma katıştırılmış durumlar olabilir iş yükleri göre ve başvurulan veri mantıklı ve daha az sunucusu ile basit uygulama mantığını adayına dönüşleri iyi düzeyde performans hala korurken yuvarlamak.

Aşağıdaki JSON göz önünde bulundurun. 

    Author documents: 
    {
        "id": "a1",
        "firstName": "Thomas",
        "lastName": "Andersen",        
        "countOfBooks": 3,
         "books": ["b1", "b2", "b3"],
        "images": [
            {"thumbnail": "http://....png"}
            {"profile": "http://....png"}
            {"large": "http://....png"}
        ]
    },
    {
        "id": "a2",
        "firstName": "William",
        "lastName": "Wakefield",
        "countOfBooks": 1,
        "books": ["b1"],
        "images": [
            {"thumbnail": "http://....png"}
        ]
    }

    Book documents:
    {
        "id": "b1",
        "name": "Azure Cosmos DB 101",
        "authors": [
            {"id": "a1", "name": "Thomas Andersen", "thumbnailUrl": "http://....png"},
            {"id": "a2", "name": "William Wakefield", "thumbnailUrl": "http://....png"}
        ]
    },
    {
        "id": "b2",
        "name": "Azure Cosmos DB for RDBMS Users",
        "authors": [
            {"id": "a1", "name": "Thomas Andersen", "thumbnailUrl": "http://....png"},
        ]
    }

Burada size burada diğer varlıklar verilerden en üst düzey belgede gömülü, ancak diğer veri başvurulan katıştırılmış modeli (çoğunlukla) uyguladıysanız. 

Kitap belge bakarsanız, birkaç görebiliriz biz yazarlar dizisi baktığınızda alanları ilginç. Var olan bir *kimliği* alan kullanırız geri normalleştirilmiş bir model standart uygulamada bir yazar belge başvurmak için alan olduğu ancak biz de sahip *adı* ve *thumbnailUrl*. Biz yalnızca ile takılmış *kimliği* ve herhangi bir ek bilgi almak için uygulama sol "bağlantı" kullanarak ilgili Yazar belgeden gereklidir, ancak uygulamamız görüntülenen her defteriyle yazar adı ve küçük resim görüntülediğinden biz gidiş dönüş sunucu listesini Defteri'nde başına denormalizing tarafından kaydedebilirsiniz **bazı** veri yazar.

Emin yazar adı değiştirilmiş ya da size bir güncelleştirme gitmek zorunda kendi photo güncelleştirmek istedikleri her kitap hiç yayımlanmadan ancak yazarlar adlarını sıklıkla, değişmeyen duymadığını uygulamamız için bu kabul edilebilir tasarım kararı.  

Örnekte vardır **toplamalar'önceden hesaplanan** değerleri pahalı işleme bir okuma üzerinde işlem kaydedilemedi. Örnekte, bazı Yazar belgede katıştırılmış veri olduğundan çalışma zamanında hesaplanan veri. Yeni bir rehberi yayımlanan her zaman bir kitap belge oluşturulur **ve** countOfBooks alanı için belirli bir yazarın mevcut Kitap belgelerini sayısına dayalı bir hesaplanmış değere ayarlanır. Bu iyileştirme biz okuma en iyi duruma getirmek için yazma üzerinde hesaplamalar yapmak burada destekleyebilir okuma ağır sistemlerinde iyi olacaktır.

Azure Cosmos DB desteklediği için önceden hesaplanan alanları ile bir model sahip olma yeteneği mümkün hale getirilir **Çoklu belge işlemler**. Birçok NoSQL depoları belgeler arasında işlemleri yapın ve bu nedenle "her zaman her şeyi, bu sınırlama nedeniyle katıştırmak" gibi tasarım kararları advocate. Azure Cosmos DB ile sunucu tarafı Tetikleyicileri ya da books ekleyebileceğiniz ve yazarlar ACID işlemi içinde tüm güncelleştirme saklı yordamları kullanabilirsiniz. Sizin artık **sahip** her şeyi yalnızca verilerinizi tutarlı olmaya devam ettiğinden emin olmak için bir belgeye katıştırmak için.

## <a name="NextSteps"></a>Sonraki adımlar
Bu makaledeki büyük paketler olan şemasız bir dünyada modelleme verileri olarak şimdiye kadar önemli olduğunu anlamak için. 

Yalnızca olduğundan ekranda verileri bir parçasını temsil etmek için tek bir yolu, verilerinizi model tek bir yolu yoktur. Uygulamanız ve nasıl oluşturur, anlamak için kullanmak ve gerekir verileri işleyecek. Ardından, bazı Burada sunulan yönergeleri uygulayarak, uygulamanızın anında gereksinimlerine yönelik bir model oluşturma hakkında ayarlayabilirsiniz. Uygulamalarınızı değiştirmeniz gerektiğinde, değiştirmek ve veri modelinizi kolayca gelişmesi kapsayacak şekilde bir şemasız veritabanı esnekliğini yararlanabilirsiniz. 

Azure Cosmos DB hakkında daha fazla bilgi için hizmetin bakın [belgelerine](https://azure.microsoft.com/documentation/services/cosmos-db/) sayfası. 

Parça için verilerinizi birden çok bölüm arasında nasıl başvurmak için anlamak için [bölümleme veri Azure Cosmos veritabanı](documentdb-partition-data.md). 
