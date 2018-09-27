---
title: 'Azure Cosmos DB tasarım deseni: sosyal medya uygulamaları | Microsoft Docs'
description: Sosyal ağlar için Azure Cosmos DB ve diğer Azure hizmetleriyle depolama esnekliğinden yararlanarak bir tasarım modeli hakkında bilgi edinin.
keywords: Sosyal medya uygulamaları
services: cosmos-db
author: ealsur
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: maquaran
ms.openlocfilehash: 5c916f847bf5098145c3ed14fad87c7669d916c8
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47222702"
---
# <a name="going-social-with-azure-cosmos-db"></a>Azure Cosmos DB ile iletişim
İçinde bir yüksek düzeyde birbirine society yaşayan anlamına gelir hayatta belirli bir noktada, bir parçası haline gelir, bir **sosyal ağ**. Sosyal ağlar arkadaşlarınız, iş arkadaşlarınızın, ailesi sürdürebilecek tutmak veya bazen Tutkunuzu ortak ilgi alanlarına kişilerle paylaşmak için kullanın.

Mühendislerin veya geliştiriciler olarak, bu ağlar depolamak ve nasıl bağlantı verilerinizi merak etmiş olabilirsiniz veya bile oluşturmak veya belirli sayıda pazar için yeni bir sosyal ağ yourselves mimari görevli. Bu durumda önemli soruyu ortaya çıkar: nasıl tüm bu veriler depolanır?

Kullanıcılarınız gibi resimler, videolar veya hatta müzik ilgili medya makalelerle gönderebileceği bir yeni ve shiny sosyal ağ, oluşturmakta olduğunuz varsayalım. Kullanıcılar gönderilerine yorum ve derecelendirmeleri puan verin. Kullanıcıların göreceği gönderilerin bir akış olacaktır ve ana Web sitesinin giriş sayfasında ile etkileşemeyebilirsiniz. Bu karmaşık ses değil (başlangıçta), ancak basitleştirmek amacıyla, şimdi burada Durdur (ilişkileri tarafından etkilenen özel kullanıcı akışları ile delve, ancak bu makalenin hedefi aşıyor).

Bu nedenle, nasıl depoladığınız bu ve nerede?

Birçoğunuzun SQL veritabanlarında deneyimine sahip veya en az kavramı vardır [ilişkisel verileri modelleme](https://en.wikipedia.org/wiki/Relational_model) ve şunun gibi çizim başlatmak için fikri size cazip olabilir:

![Göreli bir ilişkisel modeli gösteren diyagram](./media/social-media-apps/social-media-apps-sql.png) 

Mükemmel normalleştirilmiş ve asıl veri yapısı... Bu biçimde ölçeklendirilemez. 

Benim Hayatımı tüm SQL veritabanları ile çalıştım, harika olduklarından, ancak her desen, uygulama ve yazılım platformu gibi her senaryo için mükemmel değil yanlış elde etmezsiniz.

Neden bu senaryoda en iyi seçenek SQL değil mi? Bir sorgu ile yapmanız gerekir, bu postayı bir Web sitesi veya uygulaması göstermek istiyorsanız tek bir gönderi yapısını bakalım... Bir tek gönderi, şimdi dinamik olarak yükleme ve ekran ve, görünen gönderileri akışı göstereceğim burada görebilirsiniz resim göstermek için sekiz tablo birleştirmelerde (!) tam.

Elbette, devasa bir SQL örneği yeterli gücüyle binlerce sorgusu bu sayıda birleştirme içeriğinizi ancak gerçek anlamda, daha basit bir çözüm varsa, neden olduğu hizmet ile çözmeyi kullanabilirsiniz?

## <a name="the-nosql-road"></a>NoSQL yol
Bu makalede Azure'nın NoSQL veritabanı ile sosyal platformun veri modelleme içine yönlendirecektir [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) gibi özellikler diğer Azure Cosmos DB kullanırken uygun maliyetli bir şekilde [Gremlin API](../cosmos-db/graph-introduction.md). Kullanarak bir [NoSQL](https://en.wikipedia.org/wiki/NoSQL) yaklaşım, verileri JSON biçiminde depolamak ve uygulama [normalleştirilmişlikten çıkarma](https://en.wikipedia.org/wiki/Denormalization), daha önce karmaşık post tek bir dönüştürülebilir [belge](https://en.wikipedia.org/wiki/Document-oriented_database):


    {
        "id":"ew12-res2-234e-544f",
        "title":"post title",
        "date":"2016-01-01",
        "body":"this is an awesome post stored on NoSQL",
        "createdBy":User,
        "images":["http://myfirstimage.png","http://mysecondimage.png"],
        "videos":[
            {"url":"http://myfirstvideo.mp4", "title":"The first video"},
            {"url":"http://mysecondvideo.mp4", "title":"The second video"}
        ],
        "audios":[
            {"url":"http://myfirstaudio.mp3", "title":"The first audio"},
            {"url":"http://mysecondaudio.mp3", "title":"The second audio"}
        ]
    }

Ve tek bir sorgu ile hiçbir birleştirmeleri alınabilir. Bu çok daha basit ve kolay anlaşılan ve budget-wise, daha iyi bir sonuç elde etmek için daha az kaynak gerektirir.

Azure Cosmos DB tüm özellikleri, otomatik dizin oluşturma ile hatta olabilen dizinlenir emin emin yapar [özelleştirilmiş](indexing-policies.md). Şemadan bağımsız bir yaklaşım bize gönderilerin listesi kategorileri veya diyez etiketlerini ilişkili olmasını istediğiniz farklı ve dinamik yapıları, hatta yarın ile belgelerini depolamanıza olanak tanır, Cosmos DB, yeni eklenen öznitelikler hiçbir ek iş belgelerle işleyecek Bizim tarafımızdan gereklidir.

Diğer gönderiler (Bu, nesne eşleme basitleştirir) üst özelliğine sahip olarak bir post hakkındaki yorumları işlenebilir. 

    {
        "id":"1234-asd3-54ts-199a",
        "title":"Awesome post!",
        "date":"2016-01-02",
        "createdBy":User2,
        "parent":"ew12-res2-234e-544f"
    }

    {
        "id":"asd2-fee4-23gc-jh67",
        "title":"Ditto!",
        "date":"2016-01-03",
        "createdBy":User3,
        "parent":"ew12-res2-234e-544f"
    }

Ve tüm sosyal etkileşimleri sayaçları ayrı bir nesne üzerinde depolanabilir:

    {
        "id":"dfe3-thf5-232s-dse4",
        "post":"ew12-res2-234e-544f",
        "comments":2,
        "likes":10,
        "points":200
    }

Akışlar oluşturma verilen ilgi sipariş post kimlikleri listesini içerebilir belge oluşturma adımlarından oluşur:

    [
        {"relevance":9, "post":"ew12-res2-234e-544f"},
        {"relevance":8, "post":"fer7-mnb6-fgh9-2344"},
        {"relevance":7, "post":"w34r-qeg6-ref6-8565"}
    ]

Oluşturma tarihine göre sıralanmış gönderileri "son" stream sahip, bu gönderiler ile birlikte bir "sıcak" stream son 24 saat içindeki fazla beğeni, takipçi ve ilgi alanları gibi mantıksal göre her bir kullanıcı için özel bir akış bile uygulayabilirsiniz ve listesini olmaya  gönderir. Sağlasa da, bu listeleri oluşturmak nasıl olduğu halde okuma performansını unhindered kalır. Bu listelerden birine edindiğiniz sonra Cosmos DB kullanarak tek bir sorgu alınmamış [İŞLECİNDE](sql-api-sql-query.md#WhereClause) gönderilerin sayfaları aynı anda elde edilir.

Akış akışları kullanılarak oluşturulabilir. [Azure uygulama hizmetleri](https://azure.microsoft.com/services/app-service/) arka plan işlemleri: [Webjobs](../app-service/web-sites-create-web-jobs.md). Bir gönderi oluşturulduğunda, arka plan işlemesi kullanarak tetiklenebilir [Azure depolama](https://azure.microsoft.com/services/storage/) [kuyrukları](../storage/queues/storage-dotnet-how-to-use-queues.md) ve kullanarak Tetiklenmiş Web işleri [Azure Webjobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki), uygulama kendi özel mantığı temelinde akışlar içinde yayma gönderin. 

Puan ve beğeniler post üzerinden sonunda tutarlı bir ortam oluşturmak için aynı tekniği kullanarak ertelenmiş bir şekilde işlenebilir.

Takipçileri daha. Cosmos DB belge en fazla boyut sınırı vardır ve büyük belge okuma/yazma uygulamanızı ölçeklenebilirliğini etkileyebilir. Bu nedenle bu yapıya sahip bir belge olarak takipçileri depolama düşünebilirsiniz:

    {
        "id":"234d-sd23-rrf2-552d",
        "followersOf": "dse4-qwe2-ert4-aad2",
        "followers":[
            "ewr5-232d-tyrg-iuo2",
            "qejh-2345-sdf1-ytg5",
            //...
            "uie0-4tyg-3456-rwjh"
        ]
    }

Bu birkaç binlerce kullanıcı için çalışabilir ancak bazı ünlü sıralamalar, bu yaklaşım işlem birleştirir, takipçi, müşteri adayı, büyük belge boyutuna ve sonunda belge boyutu sınırı neden olabilir.

Bunu çözmek için karma bir yaklaşım kullanabilirsiniz. Kullanıcı istatistikleri belgeyi bir parçası olarak takipçi sayısı depolayabilir:

    {
        "id":"234d-sd23-rrf2-552d",
        "user": "dse4-qwe2-ert4-aad2",
        "followers":55230,
        "totalPosts":452,
        "totalPoints":11342
    }

Ve Azure Cosmos DB kullanarak takipçileri gerçek grafiği depolanabilir [Gremlin API](../cosmos-db/graph-introduction.md)oluşturmak için [grafiğinize köşe](http://mathworld.wolfram.com/GraphVertex.html) her kullanıcı için ve [kenarlar](http://mathworld.wolfram.com/GraphEdge.html) "A-aşağıdaki-B" koru ilişkiler. Gremlin API şimdi, yalnızca belirli bir kullanıcının takipçilerini elde ancak bile kişileri ortak önermek için daha karmaşık sorgular oluşturun. İçerik kategorileri kişilerin keyfini çıkarın veya gibi Graph'a eklerseniz, Akıllı İçerik bulmayı içerir deneyimleri weaving içeriği önermesini bu olanlar gibi takip ettiğiniz veya kişilerin kimlerle çok ortak sahip olabileceğiniz bulma başlatabilirsiniz.

Kullanıcı istatistikleri belgeyi yine de kullanıcı Arabirimi veya hızlı profili önizlemeler kartları oluşturmak için kullanılabilir.

## <a name="the-ladder-pattern-and-data-duplication"></a>"Merdiveni" deseni ve veri çoğaltma
Bir postayı başvuran JSON belgesinde fark etmiş olabilirsiniz gibi bir kullanıcı birden çok defa geçmelerine vardır. Ve, verilen bu normalleştirilmişlikten çıkarma, bir kullanıcıyı temsil eden bilgi birden fazla yerde mevcut olabilir yani doğru tahmin.

Daha hızlı sorgular için izin vermek üzere, veri çoğaltma ücretler. Bu yan etkisi olan bazı eylemler tarafından bir kullanıcının veri değişiklikleri tüm etkinlikleri bulunacak ihtiyacınız varsa kendisinin hiç olmadığı kadar yaptığınız ve tüm bunları güncelleştirin, bir sorundur. Sağa pratik, ses değil mi?

Uygulamanızdaki her etkinlik için gösteren bir kullanıcı anahtarı özniteliklerini tanımlayarak çözmek için oluşturacaksınız. Neden görsel olarak bir post uygulamanızda Göster ve yalnızca oluşturan kişinin adını ve resim Göster, tüm kullanıcı verilerini "oluşturan" özniteliği depoluyor? Her bir açıklama için yalnızca kullanıcının resmi gösterir, kalan bilgileri gerçekten gerekmez. Burada bir şey "Merdiveni deseni" çağırmalıyım dönüştürülerek olmasıdır.

Kullanıcı bilgilerini örnek olarak alalım:

    {
        "id":"dse4-qwe2-ert4-aad2",
        "name":"John",
        "surname":"Doe",
        "address":"742 Evergreen Terrace",
        "birthday":"1983-05-07",
        "email":"john@doe.com",
        "twitterHandle":"\@john",
        "username":"johndoe",
        "password":"some_encrypted_phrase",
        "totalPoints":100,
        "totalPosts":24
    }

Bu bilgileri bakarak kritik bilgilerin olduğu ve hangi "Merdiveni" Bu nedenle oluşturma değil hızla algılayabilir:

![Merdiveni düzeni diyagramı](./media/social-media-apps/social-media-apps-ladder.png)

En küçük adım bir UserChunk en az bir kullanıcıyı tanımlayan bilgileri parçası olarak adlandırılır ve veri çoğaltma için kullanılır. Yalnızca, "gösterilir" bilgileri için yinelenen verilerin boyutunu azaltarak, büyük güncelleştirmelerin olasılığını azaltmak.

İkinci adım, kullanıcı adı verilir, Cosmos DB, en çok erişilen ve kritik performans bağımlı sorguların çoğu kullanılacak tam veri. Bir UserChunk tarafından temsil edilen bilgiler içerir.

Genişletilmiş Kullanıcı büyüktür. Tüm kritik kullanıcı bilgilerini artı gerçekten hızlı bir şekilde okunacak gerektirmeyen diğer veri içerir veya onun kullanımı (oturum açma işlemi gibi) nihai. Bu veriler, Cosmos DB, Azure SQL veritabanı veya Azure depolama tabloları dışında depolanabilir.

Neden, kullanıcı bölme ve hatta farklı yerlerde bu bilgileri depolamak? Çünkü bir performans açısından, daha büyük belgelere, costlier sorgular. Belgeleri, sosyal ağ için tüm performans bağlı sorgular yapmak için ve diğer ek bilgileri gibi tam bir profil düzenleme, oturum açma bilgileri nihai senaryoları depolamak, kullanım analizi ve büyük veriler için veri madenciliği bile doğru bilgilerle ince koru girişim. Azure SQL veritabanı'nda çalıştığından veri madenciliği için veri toplama yavaş olması durumunda, gerçekten İlgilenmiyor, size sahip ilgilendiriyor ancak kullanıcılarınız hızlı ve ince bir deneyimi vardır. Cosmos DB'de depolanan kullanıcı, şöyle görünür:

    {
        "id":"dse4-qwe2-ert4-aad2",
        "name":"John",
        "surname":"Doe",
        "username":"johndoe"
        "email":"john@doe.com",
        "twitterHandle":"\@john"
    }

Ve bir gönderi şöyle görünmelidir:

    {
        "id":"1234-asd3-54ts-199a",
        "title":"Awesome post!",
        "date":"2016-01-02",
        "createdBy":{
            "id":"dse4-qwe2-ert4-aad2",
            "username":"johndoe"
        }
    }

Ve burada öbeğin özniteliklerinden biri etkilenir bir düzenleme ortaya çıkar, dizinli öznitelikleri noktası sorgularını kullanarak etkilenen belgeleri bulmak daha kolaydır (seçin * FROM gönderileri p nerede p.createdBy.id "edited_user_id" ==) ve ardından öbekleri güncelleştiriliyor.

## <a name="the-search-box"></a>Arama kutusu
Kullanıcılar, çok içerik oluşturur. Arama ve oluşturucuları izlemeyin olduğundan, doğrudan kendi içerik akışlarında, belki de olmayabilecek içerik bulma olanağı sağlayabilir olmalıdır ve belki de yalnızca eski post altı ay önce yaptığınız bulmaya çalıştığınız.

Ne ve Azure Cosmos DB kullandığından, kolayca bir arama motoru kullanarak uygulayabileceğiniz [Azure Search](https://azure.microsoft.com/services/search/) birkaç dakika ve tek satırlık bir kod (diğer daha açıktır, arama işlemi ve kullanıcı Arabirimi) yazarak olmadan.

Neden bu kadar kolay mı?

Azure Search'ü uygular, çağrı [dizin oluşturucular](https://msdn.microsoft.com/library/azure/dn946891.aspx), arka plan işlemleri, veri depoları içindeki bu kanca ve automagically ekleme, güncelleştirme veya nesnelerinizi dizinlerde kaldırın. Destekledikleri bir [Azure SQL veritabanı dizin oluşturucular](https://blogs.msdn.microsoft.com/kaevans/2015/03/06/indexing-azure-sql-database-with-azure-search/), [Azure blob dizin oluşturucuları](../search/search-howto-indexing-azure-blob-storage.md) ve ne [Azure Cosmos DB dizinleyici](../search/search-howto-index-documentdb.md). Cosmos DB için Azure Search bilgilerinin geçişi her iki depolama bilgileri JSON biçiminde olarak basit, tek yapmanız gereken [dizininizi oluşturma](../search/search-create-index-portal.md) eşlemek istediğiniz dizinli belgelerinizi hangi öznitelikleri ve, olan birkaç dakika içinde (veri boyutuna bağlıdır), tüm içeriğinizi, üzerine bulut altyapısında en iyi hizmet olarak arama çözümü tarafından aranacak kullanılabilir olacaktır. 

Azure arama hakkında daha fazla bilgi için ziyaret edebilirsiniz [arama Hitchhiker'ın Kılavuzu](https://blogs.msdn.microsoft.com/mvpawardprogram/2016/02/02/a-hitchhikers-guide-to-search/).

## <a name="the-underlying-knowledge"></a>Temel alınan bilgi
Büyür ve her gün büyüdükçe tüm bu içerik depolama sonrasında, düşünme bulabilirsiniz: tüm bu akış bilgileri ile my kullanıcıların neler yapabilirim?

Yanıt oldukça basittir: İş ve buradan edinin yerleştirin.

Peki ne öğrenebilir? Birkaç kolay örnekler [yaklaşım analizi](https://en.wikipedia.org/wiki/Sentiment_analysis), öneriler temel bir kullanıcının tercihlerini veya sosyal ağınız tarafından yayımlanan tüm içeriklerde ailesi için güvenli olduğunu güvence altına alınır bile bir otomatik content moderator içerik.

Büyük olasılıkla, kancalandı aldım, bu desenleri ve basit veritabanları ve dosyaları dışında bilgi ayıklamak için bazı Biyofizik matematik Bilimleri gerekir, ancak yanlış olurdu düşünecek.

[Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/)in bir parçası olan [Cortana Intelligence Suite](https://social.technet.microsoft.com/wiki/contents/articles/36688.introduction-to-cortana-intelligence-suite.aspx), içinde basit bir Sürükle ve bırak arabirimi algoritmalarını kullanarak iş akışları oluşturun, kendi algoritmalar koduolanaksağlayantamolarakyönetilenbuluthizmeti[ R](https://en.wikipedia.org/wiki/R_\(programming_language\)) veya bazı zaten oluşturulmuş ve kullanıma hazır API'leri gibi kullanın: [metin analizi](https://gallery.cortanaanalytics.com/MachineLearningAPI/Text-Analytics-2), [Content Moderator veya [önerileri](https://gallery.azure.ai/Solution/Recommendations-Solution).

Bu makine öğrenimi senaryoların herhangi birini elde etmek için kullanabileceğiniz [Azure Data Lake](https://azure.microsoft.com/services/data-lake-store/) farklı kaynaklardan bilgi alma ve [U-SQL](https://azure.microsoft.com/documentation/videos/data-lake-u-sql-query-execution/) bilgi işlem ve için bir çıktı oluşturmak için Azure Machine Learning tarafından işlenmesi.

Başka bir seçenek kullanmaktır [Microsoft Bilişsel Hizmetler](https://www.microsoft.com/cognitive-services) kullanıcılarınız yalnızca anlamanız bunları daha iyi içerik; analiz etmek için (ile bunların yazma analiz aracılığıyla [metin analizi API'si](https://www.microsoft.com/cognitive-services/en-us/text-analytics-api)), ancak Ayrıca istenmeyen veya yetişkin içeriği Algıla ve buna göre hareket ile [görüntü işleme API'si](https://www.microsoft.com/cognitive-services/en-us/computer-vision-api). Bilişsel hizmetler kullanmak için Machine Learning bilgi herhangi bir türden gerektirmeyen çok sayıda kullanıma hazır çözümler sunar.

## <a name="a-planet-scale-social-experience"></a>Çok büyük ölçekli bir sosyal deneyimi
Son yoktur, ancak önemli bir makale miyim, ele almalıdır: **ölçeklenebilirlik**. Daha fazla veri işleme gerektiği için her bileşen kendi başına, ya da ölçeklendirebileceğinizi önemlidir bir mimari tasarlarken veya daha büyük bir coğrafi kapsamı (veya her ikisi de!) sahip olmak istememiz. Karmaşık bir görevi gerçekleştirmekten ne olduğunu bir **yapmanız** Cosmos DB ile.

Cosmos DB destekleyen [dinamik bölümlemeyi](https://azure.microsoft.com/blog/10-things-to-know-about-documentdb-partitioned-collections/) ,-hazır bölümlere göre otomatik olarak oluşturarak bir verilen **bölüm anahtarı** (belgelerinizde özniteliklerinden biri olarak tanımlanır). Tasarım zamanında doğru bölüm anahtarının bitti tanımlama ve göz önünde bulundurarak [en iyi uygulamalar](../cosmos-db/partition-data.md#designing-for-partitioning) kullanılabilir; sosyal bir deneyimle karşılaşacak (okuma aynı sorgu yolunu bölümleme stratejisinde hizalanması gerekir Bölüm arzu) ve yazma ("etkin nokta" birden çok bölüm üzerinde yazma yayarak kaçının). Bazı seçenekler şunlardır: zamana bağlı bir anahtar (gün/ay/hafta); kullanıcı tarafından coğrafi bölgeye göre içerik kategoriye göre bölüm Tüm gerçekten nasıl, verileri sorgulamak ve sosyal deneyiminizi Göster bağlıdır. 

Bir değer bahseden ilgi çekici nokta Cosmos DB sorgularınız çalışabilmesini (dahil olmak üzere [toplamalar](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/)) tüm bölümler arasında saydam bir şekilde, verileriniz arttıkça herhangi bir mantık eklemeniz gerekmez.

Zaman, sonunda trafik ve kaynak tüketiminize büyüyecektir (ölçülen [RU](request-units.md), veya istek birimi) artacaktır. Okuma ve daha sık kullanıcı tabanınızı büyüdükçe ve oluşturma ve daha fazla içerik okuma başlayacak yazma; yeteneğini **aktarım hızınızı ölçeklendirme** yaşamsal önem taşır. Azure portalında birkaç tıklamayla yapın ya da, RU artırmak kolaydır, [API komutları verme](https://docs.microsoft.com/rest/api/cosmos-db/replace-an-offer).

![Bir bölüm anahtarı tanımlayarak ve büyütme](./media/social-media-apps/social-media-apps-scaling.png)

Şeyleri daha iyi almaya devam ederseniz ne olur ve kullanıcıların başka bir bölge, ülke veya Kıta, platformunuza dikkat edin ve bu harika şaşkınlık kullanmaya başlayın!

Ancak şu durumda bekleyin... platformunuz yaşadıkları deneyimleri en iyi; değil hemen fark şu ana kadar uzağa işletimsel bölgenizi saltanatı gecikme süresi ve bunları çıkmak için açıkça istemiyorsanız değildirler. Vardı, kolay bir yol yalnızca **küresel erişiminizi genişletmek**... yoktur ancak!

Cosmos DB sayesinde [verilerinizi dünya çapında çoğaltın](../cosmos-db/tutorial-global-distribution-sql-api.md) ve şeffaf bir şekilde birkaç tıklamayla ve otomatik olarak ile kullanılabilir bölgelerden arasından, [istemci kodu](../cosmos-db/tutorial-global-distribution-sql-api.md). Ayrıca, olabilir yani [birden çok yük devretme bölgeleri](regional-failover.md). 

Verilerinizi dünya çapında çoğaltın, müşterilerinizin Avantajdan sürebilir emin olmanız gerekir. Bir web ön ucu kullanıyorsanız veya dağıtabileceğiniz API'leri mobil istemcilerden erişim, [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) ve Azure App Service, genişletilmiş desteklemek için bir performans yapılandırması kullanarak tüm istenen bölgeler üzerinde kopyalama genel Kapsam. İstemcileriniz, ön uç veya API'leri eriştiğinizde, sırasıyla yerel bir Cosmos DB kopyasına bağlanır, en yakın uygulama hizmeti, yönlendirilir.

![Sosyal platformunuz için genel kapsamı ekleme](./media/social-media-apps/social-media-apps-global-replicate.png)

## <a name="conclusion"></a>Sonuç
Bu makalede, sosyal ağlar, düşük maliyetli Hizmetleri ile Azure'da tamamen oluşturma ve harika sonuçlar "Merdiveni" adlı bir çok katmanlı depolama çözümü ve veri dağıtım kullanımını teşvik tarafından sağlama alternatifleri içine bazı ışık boşaltmaktır dener.

![Sosyal ağ için Azure Hizmetleri arasındaki etkileşim diyagramı](./media/social-media-apps/social-media-apps-azure-solution.png)

Gerçekte olduğu için bu tür senaryolara Gümüş madde işareti yok, harika deneyimleri oluşturmanıza olanak tanır harika hizmetler birleşimiyle oluşturulan synergy olduğu: hız ve Azure Cosmos DB'nin harika bir sosyal uygulama sağlamak için arama özgürlüğü Azure Search, depolamak için esneklik değil bile dilden uygulamaları ancak güçlü bir arka plan işlemleri ve Genişletilebilir Azure depolama barındırmak için Azure App Services ve Azure SQL veritabanı gibi birinci sınıf arama çözümü arkasında gösterimi büyük miktarda veri ve bilgi ve işlemlerinizi için geri bildirim sağlamak ve bize yardımcı bilgiler oluşturmak için Azure Machine Learning analitik gücü doğru içeriği doğru kullanıcılara sunun.

## <a name="next-steps"></a>Sonraki adımlar
Cosmos DB için kullanım örnekleri hakkında daha fazla bilgi edinmek için [Cosmos DB genel kullanım örnekleri](use-cases.md).
