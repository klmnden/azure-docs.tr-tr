---
title: 'Azure Cosmos DB tasarım deseni: sosyal medya uygulamalar | Microsoft Docs'
description: Sosyal ağlar için Azure Cosmos DB ve diğer Azure hizmetleriyle depolama esnekliğini yararlanarak tasarım deseni hakkında bilgi edinin.
keywords: Sosyal medya uygulamalar
services: cosmos-db
author: ealsur
manager: kfile
documentationcenter: ''
ms.assetid: 2dbf83a7-512a-4993-bf1b-ea7d72e095d9
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2017
ms.author: maquaran
ms.openlocfilehash: 53abefd4f3dd1f8da60b8b8efed1e7070b471383
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="going-social-with-azure-cosmos-db"></a>Azure Cosmos DB ile sosyal gitme
Yüksek düzeyde birbirine topluluğu içinde yaşayan anlamına gelir hayatta bir noktada, bir parçası haline gelir, bir **sosyal ağ**. Sosyal ağlar tutmak arkadaşlarınız, iş arkadaşlarınızı, aile olmanızı sağlar, veya bazen bizim tutku ortak ilgi alanlarına sahip kişilerle paylaşmak için kullanın.

Mühendisleri veya geliştiricilerin biz nasıl bu ağlar depolamak ve verilerimizi, birbirine hiç merak ettiniz veya bile oluşturmak veya belirli sayıda pazar için yeni bir sosyal ağ yourselves mimari için görevli. Ne zaman olan büyük soruyu ortaya çıkar: nasıl tüm bu verileri depolanır?

Şimdi kullanıcılarımızın ilgili medya gibi resim, video ya da hatta müzik makalelerle burada nakledebilirsiniz yeni ve parlak sosyal ağ, oluşturuyoruz varsayalım. Kullanıcıların gönderilerine yorum ve derecelendirmeleri noktaları verin. Kullanıcıların göreceği posta akışı olacaktır ve ana Web sitesi giriş sayfasında ile etkileşemeyebilirsiniz. Bu gerçekten karmaşık ses değil (başta), ancak basitleştirmek amacıyla, şimdi var. Durdur (biz ilişkileriyle etkilenen özel kullanıcı akışları içine inceleyin, ancak bu makalede amacı aşıyor).

Bu nedenle, nasıl depolarız bu ve nerede?

Birçoğu deneyimi olan SQL veritabanlarına sahip olabilir veya en azından kavramı [ilişkisel veri modelleme](https://en.wikipedia.org/wiki/Relational_model) ve şöyle bir şey çizim başlatma isteği olabilir:

![Göreli bir ilişkisel modeli gösteren diyagram](./media/social-media-apps/social-media-apps-sql.png) 

Mükemmel normalleştirilmiş ve asıl veri yapısı... ölçeklendirilmediğini. 

Me, my hayatta SQL veritabanları ile çalıştığınız, harika, ancak her düzeni, uygulama ve yazılım platformu gibi her senaryo için mükemmel değil yanlış olmaz.

Neden bu senaryoda en iyi seçenek SQL değil mi? Bu post bir Web sitesi veya uygulama, bir sorgu ile yapmak zorunda olabilirsiniz göstermek istediyseniz tek bir post yapısı bakalım... sadece tek tek posta, artık, dinamik olarak yükleyen ve ekran ve, görünen gönderileri akışı burada yapacağım görebileceğiniz resmi göstermek için 8 tablo birleştirme (!).

Elbette, bellek bilgisayar'ın bir SQL örneği ile yeterli güç sorguları binlerce bizim içeriği ancak gerçekten, neden olduğu sorundan daha basit bir çözüm varsa, hizmet vermek için bu birçok birleştirmeleri çözmek için kullanırız olabilir?

## <a name="the-nosql-road"></a>NoSQL yol
Bu makalede Azure'nın NoSQL veritabanı sosyal platformun verilerle modelleme içine yönlendirecek [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) gibi özellikler diğer Azure Cosmos DB yararlanarak sırasında uygun maliyetli bir şekilde [Gremlin grafik API'si](../cosmos-db/graph-introduction.md). Kullanarak bir [NoSQL](https://en.wikipedia.org/wiki/NoSQL) JSON biçiminde veri depolamak ve uygulama yaklaşımı [denormalization](https://en.wikipedia.org/wiki/Denormalization), daha önce karmaşık bizim post tek bir dönüştürülebilir [belge](https://en.wikipedia.org/wiki/Document-oriented_database):


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

Ve tek bir sorgu ile ve hiçbir birleştirmeleri edinilebilir. Bu çok daha basit ve kolay ve budget-wise, daha iyi sonucu elde etmek için daha az kaynak gerektirir.

Azure Cosmos DB yapar tüm özellikleri kendi otomatik dizin oluşturma ile hatta olabilen dizinlenir emin emin [özelleştirilmiş](indexing-policies.md). Şemasız yaklaşım bize kategorileri veya onlarla ilişkili diyez etiketlerini listesine sahip gönderileri istiyoruz farklı ve dinamik yapıları, belki de yarın ile belgeleri depolamak sağlar, hiçbir ek iş eklenen öznitelikler yeni belgelerle Cosmos DB işleyecek tarafımızca gereklidir.

Yalnızca diğer gönderileri (bizim nesne eşleme basitleştirir) üst özelliğe sahip olarak bir post açıklamaları işlenebilir. 

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

Ve tüm sosyal etkileşimler sayaçları ayrı bir nesne üzerinde depolanabilir:

    {
        "id":"dfe3-thf5-232s-dse4",
        "post":"ew12-res2-234e-544f",
        "comments":2,
        "likes":10,
        "points":200
    }

Akışlar oluşturma verilen ilgi sırasıyla post kimlikleri listesini tutabilir belgeler oluşturma yalnızca bir konudur:

    [
        {"relevance":9, "post":"ew12-res2-234e-544f"},
        {"relevance":8, "post":"fer7-mnb6-fgh9-2344"},
        {"relevance":7, "post":"w34r-qeg6-ref6-8565"}
    ]

Biz "en son" akış oluşturma tarihine göre sıralanmış gönderileri sahip, bu gönderileri olan bir "sıcak" akış daha son 24 saat içindeki yöntemlerine, biz followers ve ilgi alanları gibi mantığı göre her bir kullanıcı için özel bir akış bile uygulamak ve hala p listesini olacaktır osts. Sağlasa da, bu listeleri nasıl oluşturacağınızı olduğu, ancak okuma performans unhindered kalır. Biz bu listelerden birine elde sonra biz tek bir sorgu Cosmos DB kullanarak sorun [İŞLECİNDE](sql-api-sql-query.md#WhereClause) gönderileri sayfaların aynı anda elde etmek için.

Akış akışları kullanılarak oluşturulabilir [Azure App Services](https://azure.microsoft.com/services/app-service/) arka plan işlemleri: [Webjobs](../app-service/web-sites-create-web-jobs.md). Bir post oluşturulduktan sonra arka plan işleme kullanarak tetiklenebilir [Azure Storage](https://azure.microsoft.com/services/storage/) [sıraları](../storage/queues/storage-dotnet-how-to-use-queues.md) ve Webjobs kullanarak tetiklenen [Azure Webjobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki), uygulama kendi özel mantığına göre akışları içinde yayma gönderin. 

Noktaları ve yöntemlerine bir post üzerinden sonuçta tutarlı bir ortam oluşturmak için aynı tekniği kullanarak ertelenmiş bir şekilde işlenebilir.

Followers daha. Cosmos DB en fazla belge boyut sınırı vardır ve büyük belgeleri okuma/yazma, uygulamanızın ölçeklenebilirliğini etkileyebilir. Bu nedenle bu yapısına sahip bir belge olarak followers depolama düşünebilirsiniz:

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

Bu birkaç binlerce sahip bir kullanıcı için çalışabilir bazı ünlülerle bizim sıralar, bu yaklaşım işlem birleştirir, ancak followers neden büyük belge boyutuna ve sonunda belge boyutu ucun isabet.

Bunu çözmek için karma bir yaklaşım kullanabilirsiniz. Kullanıcı istatistikleri belgenin bir parçası olarak biz followers sayısı depolayabilirsiniz:

    {
        "id":"234d-sd23-rrf2-552d",
        "user": "dse4-qwe2-ert4-aad2",
        "followers":55230,
        "totalPosts":452,
        "totalPoints":11342
    }

Ve Azure Cosmos DB kullanarak followers gerçek grafiği depolanabilir [Gremlin grafik API'si](../cosmos-db/graph-introduction.md)oluşturmak için [köşeleri](http://mathworld.wolfram.com/GraphVertex.html) her kullanıcı için ve [kenarları](http://mathworld.wolfram.com/GraphEdge.html) "A-aşağıdaki-B" koru ilişkiler. Grafik API'si şimdi, yalnızca belirli bir kullanıcı followers elde ancak bile kişiler ortak önermek üzere daha karmaşık sorgular oluşturabilirsiniz. Biz içerik kategorileri kişilerin keyfini çıkarın veya gibi grafiğe eklerseniz, biz Akıllı İçerik bulma dahil deneyimleri weaving içerik öneren bu olanlar gibi biz izleyin veya takımı biz çok ortak olabilir kişi bulma başlatabilirsiniz.

Kullanıcı istatistikleri belge hala kartları kullanıcı Arabirimi veya hızlı profili önizlemeleri oluşturmak için kullanılabilir.

## <a name="the-ladder-pattern-and-data-duplication"></a>"Merdiveni" düzeni ve veri çoğaltma
Bir post başvuran JSON belgesinde fark etmiş olabileceğiniz gibi bir kullanıcı birden fazla oluşumu vardır. Ve, sağ, yani bu denormalization verilen bir kullanıcıyı temsil eden bilgiler birden fazla yerde mevcut olabilir tahmin.

Daha hızlı sorgular için izin vermek üzere veri çoğaltma yaptığımız. Bu yan etkiyi sorun bazı eylem tarafından bir kullanıcının veri değişiklikleri, biz tüm etkinlikleri bulmanız gerekiyorsa kendisine herhangi bir zamanda vermedi ve Tümünü Güncelleştir olmasıdır. Ses çok pratik, doğru değil mi?

Biz uygulamamızı her etkinlik için gösterilecek bir kullanıcı anahtarı özniteliklerini belirleyerek çözmek olacak. Neden biz görsel olarak bir post bizim uygulamada Göster ve yalnızca Oluşturanın adı ve resim Göster, tüm kullanıcı verileri "tarafından oluşturuldu" özniteliğinde depoluyor? Her açıklamasını yalnızca kullanıcının resmi gösteriyoruz, kendi bilgi kalan gerçekten gerekli değil. Burada bir şey "Merdiveni düzeni" çağrısı oyuna gelen olmasıdır.

Kullanıcı bilgilerini bir örnek olarak atalım:

    {
        "id":"dse4-qwe2-ert4-aad2",
        "name":"John",
        "surname":"Doe",
        "address":"742 Evergreen Terrace",
        "birthday":"1983-05-07",
        "email":"john@doe.com",
        "twitterHandle":"@john",
        "username":"johndoe",
        "password":"some_encrypted_phrase",
        "totalPoints":100,
        "totalPosts":24
    }

Bu bilgileri bakarak, biz kritik bilgilerin olduğu ve hangi "Merdiveni" böylece oluşturma değil hızlı bir şekilde tespit edebilirsiniz:

![Merdiveni düzeni diyagramı](./media/social-media-apps/social-media-apps-ladder.png)

En küçük adım UserChunk, en az bir kullanıcı tanımlayan bilgileri parçası olarak adlandırılır ve veri çoğaltma için kullanılır. Yalnızca "göstereceğiz" bilgileri çoğaltılan verilerin boyutunu azaltarak biz yoğun güncelleştirmeleri olasılığını azaltmak.

Orta adım kullanıcı olarak adlandırılır, Cosmos DB, en erişilen ve kritik performans bağımlı sorguların çoğu üzerinde kullanılan tam veri. UserChunk tarafından temsil edilen bilgileri içerir.

En büyük genişletilmiş kullanıcıdır. Kendi kullanımı (oturum açma işlemi gibi) nihai veya tüm kritik kullanıcı bilgilerini artı gerçekten hızlı bir şekilde okunacak gerektirmeyen diğer veri içerir. Bu veriler, Azure SQL Database veya Azure depolama tablolarında Cosmos DB dışında depolanabilir.

Neden biz kullanıcı bölme bile bu bilgileri ve farklı yerlerde depolamak? Çünkü bir performans bakış açısı, büyük belgeleri, costlier sorgular. Belgeleri tüm performans bağımlı sorguları, sosyal ağ için yapmak için ve diğer ek bilgileri gibi tam profil düzenleme, oturum açma bilgileri nihai senaryoları için depolama, veri araştırma kullanım analizi ve büyük veri için bile doğru bilgilerle ince koru girişimleri. Azure SQL veritabanı üzerinde çalıştığı için veri toplama için veri madenciliği yavaş ise, biz gerçekten umursamaz, biz sahip ilgilendiren ancak kullanıcılarımızın hızlı ve ince bir deneyim sahibi olduğunuzu. Cosmos DB üzerinde depolanan, bir kullanıcı şöyle olabilir:

    {
        "id":"dse4-qwe2-ert4-aad2",
        "name":"John",
        "surname":"Doe",
        "username":"johndoe"
        "email":"john@doe.com",
        "twitterHandle":"@john"
    }

Ve bir Post aşağıdaki gibidir:

    {
        "id":"1234-asd3-54ts-199a",
        "title":"Awesome post!",
        "date":"2016-01-02",
        "createdBy":{
            "id":"dse4-qwe2-ert4-aad2",
            "username":"johndoe"
        }
    }

Ve bir düzen öbek özniteliklerinden biri burada etkilenir duyduğunuzda, etkilenen belgeleri dizinli öznitelikleri noktası sorgularını kullanarak bulma daha kolaydır (seçin * FROM yazılarını p NEREYE p.createdBy.id "edited_user_id" ==) ve öbekler güncelleştiriliyor.

## <a name="the-search-box"></a>Arama kutusu
Kullanıcılar, çok miktarda içerik oluşturur. Ve biz aramak ve oluşturucuları uygulayın yoktur çünkü doğrudan kendi içerik akış, belki de olmayabilir içerik bulma olanağı sağlayabilir olmalı veya belki de numaralı yalnızca eski post 6 ay önce yaptığımız olduğunu bulmak çalışıyoruz.

Thankfully, ve Azure Cosmos DB de kullandığınızdan, biz kolayca bir arama motoru kullanarak uygulayabileceğiniz [Azure Search](https://azure.microsoft.com/services/search/) birkaç dakika ve tek satırlık bir kod (dışında açıktır, arama işlemi ve kullanıcı Arabirimi) yazarak olmadan.

Neden bu kadar kolay mi?

Azure arama uygulayan bunlar unsur [dizin oluşturucular](https://msdn.microsoft.com/library/azure/dn946891.aspx), arka plan, veri depoları içindeki bu kanca işler ve automagically ekleme, güncelleştirme veya dizinlerde nesnelerinizi kaldırma. Desteklenen bir [Azure SQL veritabanı dizin oluşturucular](https://blogs.msdn.microsoft.com/kaevans/2015/03/06/indexing-azure-sql-database-with-azure-search/), [Azure BLOB'ları dizin oluşturucular](../search/search-howto-indexing-azure-blob-storage.md) ve thankfully, [Azure Cosmos DB dizin oluşturucular](../search/search-howto-index-documentdb.md). Azure Search Cosmos DB bilgileriyle geçişini, her iki depolama bilgileri JSON biçiminde olarak basittir, biz yeterlidir [bizim dizin oluşturma](../search/search-create-index-portal.md) ve eşleme hangi özniteliklerin bizim belgelerden dizinli istiyoruz ve bu, içinde bir birkaç dakika (bizim veri boyutuna bağlıdır), tüm içeriği bağlı bulut altyapısı en iyi hizmet olarak arama çözümü tarafından aranacak kullanılabilir olacaktır. 

Azure Search hakkında daha fazla bilgi için ziyaret edebilirsiniz [Hitchhiker'ın Kılavuzu arama](https://blogs.msdn.microsoft.com/mvpawardprogram/2016/02/02/a-hitchhikers-guide-to-search/).

## <a name="the-underlying-knowledge"></a>Temel alınan bilgi
Büyür ve her gün büyür tüm bu içeriği depolamak sonra biz kendisini düşünüyorum bulabilirsiniz: tüm bu bilgilerin olan akış my kullanıcılardan ne yapabilirim?

Yanıt basittir: çalışma ve ondan bilgi alın.

Ancak, ne biz öğrenebilirsiniz? Birkaç kolay örnekler [düşünceleri analiz](https://en.wikipedia.org/wiki/Sentiment_analysis), içeriği önerilerine dayalı bir kullanıcının tercihlerini veya bizim sosyal ağ tarafından yayımlanan tüm içeriğin ailesi için güvenli olduğunu güvence altına alınır bile bir otomatik içerik denetleyici.

Sayfaya var, büyük olasılıkla bu desenleri ve basit veritabanları ve dosyaları dışında bilgi ayıklamak için bazı Doktora matematik Bilim içinde gerekir, ancak yanlış olacaktır düşündüğünüz.

[Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/), parçası [Cortana Intelligence Suite](https://www.microsoft.com/en/server-cloud/cortana-analytics-suite/overview.aspx), olan basit bir Sürükle ve bırak arabirimi algoritmaları kullanarak da iş akışları oluşturmanıza olanak sağlayan tam olarak yönetilen bulut hizmeti kod kendi algoritmaları [R](https://en.wikipedia.org/wiki/R_\(programming_language\)) veya bazı zaten oluşturulmuş ve API'leri gibi kullanıma hazır kullanın: [metin analizi](https://gallery.cortanaanalytics.com/MachineLearningAPI/Text-Analytics-2), [içerik denetleyici](https://www.microsoft.com/moderator) veya [önerileri](https://gallery.cortanaanalytics.com/MachineLearningAPI/Recommendations-2).

Bu Machine Learning senaryoların herhangi biri elde etmek için size kullanabilirsiniz [Azure Data Lake](https://azure.microsoft.com/services/data-lake-store/) farklı kaynaklardan bilgi alın ve kullanmak için [U-SQL](https://azure.microsoft.com/documentation/videos/data-lake-u-sql-query-execution/) bilgi işlem ve olabilir bir çıktı oluşturmak için Azure Machine Learning tarafından işlenmesi.

Başka bir seçenek kullanmaktır [Microsoft Bilişsel Hizmetler](https://www.microsoft.com/cognitive-services) kullanıcılarımızın içerik; yalnızca ki anlamak bunları daha iyi çözümlemek için (ile bunların yazma çözümleme aracılığıyla [metin Analytics API](https://www.microsoft.com/cognitive-services/en-us/text-analytics-api)), ancak aynı zamanda istenmeyen veya yetişkin içeriği algılayan ve buna göre hareket ile [bilgisayar görme API](https://www.microsoft.com/cognitive-services/en-us/computer-vision-api). Bilişsel hizmetler çok kullanmak için Machine Learning bilgi her türlü gerektirmeyen Giden kutusu çözümleri içerir.

## <a name="a-planet-scale-social-experience"></a>Planet ölçekli sosyal deneyimi
Bir son yoktur, ancak önemli konu t, adres gerekir: **ölçeklenebilirlik**. Daha fazla veri işlemeye gerektiğinden her bileşenin kendi başına ya da genişletilebileceğini önemlidir bir mimari tasarlarken veya daha büyük bir coğrafi kapsamı (veya her ikisi de!) sahip olmak istiyoruz. Thankfully, karmaşık bir görev elde olan bir **anahtar teslimi deneyimi** Cosmos DB ile.

Cosmos DB destekleyen [dinamik bölümleme](https://azure.microsoft.com/blog/10-things-to-know-about-documentdb-partitioned-collections/) out-of--box bölümleri göre otomatik olarak oluşturarak bir verilen **bölüm anahtarı** (belgelerinizi özniteliklerin biri olarak tanımlanır). Tasarım zamanında doğru bölüm anahtarı bitti tanımlama ve göz önünde bulundurarak [en iyi uygulamalar](../cosmos-db/partition-data.md#designing-for-partitioning) kullanılabilir; sosyal deneyimi söz konusu olduğunda, bölümleme stratejinizi (okuma aynı içinde sorgu yol hizalanması gerekir Bölüm arzu) ve yazma ("etkin nokta" birden çok bölüm üzerinde yazma yayarak kaçının). Bazı seçenekler şunlardır: kullanıcı tarafından; coğrafi bölgeye göre içerik kategoriye göre zamana bağlı bir anahtarı (gün/ay/hafta), temel bölümleri Tüm gerçekten nasıl verileri sorgulamak ve sosyal deneyiminizi göster bağımlı. 

Bir söz değerinde ilginç noktasıdır Cosmos DB sorgularınızı çalıştırın (de dahil olmak üzere [toplamalar](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/)), tüm bölümler saydam, verilerinizi büyüdükçe herhangi bir mantık eklemeniz gerekmez.

Saat ile sonunda trafik ve kaynak tüketimini büyüyecektir (cinsinden [RUs](request-units.md), veya istek birimleri) artmasına neden olur. Okuma ve daha sık, userbase büyüdükçe ve oluşturma ve daha fazla içerik okuma başlayacak şekilde yazma; özelliğini **, üretilen iş ölçeklendirme** önemlidir. Bizim RUs artırma çok kolay, size Azure portalında birkaç tıklama ile yapabilirsiniz ya da [API'si aracılığıyla komutları veren](https://docs.microsoft.com/rest/api/cosmos-db/replace-an-offer).

![Yükseltme ve bölüm anahtarı tanımlama](./media/social-media-apps/social-media-apps-scaling.png)

Şeyleri daha iyi almaya devam ederseniz olanlar başka bir bölge, ülke veya kıtada, platformunuz dikkat edin ve kullanıcıları, bir harika aniden kullanmaya başlayın!

Ancak bekleyin... yakında deneyimlerini platformunuz ile en iyi; değil unutmayın şu ana kadar işletimsel bölgenizi çıktığınızda gecikme korkunç ve bunları çıkmak için açıkça istemiyorsanız oldukları. Oluştu, kolay bir yol yalnızca **genel elinizin genişletme**... yoktur ancak!

Cosmos DB olanak tanır [verilerinizi genel çoğaltmak](../cosmos-db/tutorial-global-distribution-sql-api.md) ve saydam bir birkaç tıklama ve otomatik olarak kullanılabilir bölgelerden arasından, [istemci kodu](../cosmos-db/tutorial-global-distribution-sql-api.md). Bu aynı zamanda, olabilir gelir [birden çok yük devretme bölgeleri](regional-failover.md). 

Verilerinizi genel çoğalttığınızda, istemcilerinizin bunu yararlanabilir emin olmanız gerekir. Bir web ön uç veya belirtilmemelidir API'leri mobil istemcilerden kullanıyorsanız, dağıtabileceğiniz [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) ve tüm istenen bölgeler Azure App, genişletilmiş desteklemek için bir performans Yapılandırması kullanılarak hizmet kopyalama genel kapsamı. İstemcilerinizi ön uç veya API'leri eriştiğinizde, sırasıyla yerel Cosmos DB çoğaltma bağlanacak en yakın uygulama hizmeti, yönlendirilir.

![Sosyal platformunuz genel kapsamı ekleme](./media/social-media-apps/social-media-apps-global-replicate.png)

## <a name="conclusion"></a>Sonuç
Bu makalede, sosyal ağlara düşük maliyetli Hizmetleri ile tamamen Azure üzerinde oluşturma ve "Merdiveni" adlı bir çok katmanlı depolama çözümü ve veri dağıtım kullanımını görünümü teşvik eder mükemmel sonuçlar sağlama alternatifleri içine bazı ışık shed dener.

![Sosyal ağ için Azure services arasındaki etkileşim diyagramı](./media/social-media-apps/social-media-apps-azure-solution.png)

Gerçekte olduğu için bu tür senaryoların Gümüş hiçbir madde işareti, bize harika deneyimlerini oluşturmak izin harika Hizmetleri birleşimiyle oluşturulan synergy değil: harika bir sosyal uygulaması sağlamak için Azure Cosmos DB özgürlüğünü ve hızı Azure Search, depolama için esneklik değil bile dilden bağımsız uygulamalar ancak güçlü arka plan işlemleri ve Genişletilebilir Azure Storage barındırmak için Azure App Services ve Azure SQL veritabanı gibi bir ilk sınıf arama çözümü arkasında gösterimi büyük miktarlarda verinin ve Bilgi Bankası ve bizim işlemleri geri bildirim sağlayın ve bize yardımcı Intelligence oluşturmak için analitik gücünü Azure Machine Learning doğru içeriğe doğru kullanıcılara sunar.

## <a name="next-steps"></a>Sonraki adımlar
Cosmos DB için kullanım durumları hakkında daha fazla bilgi için bkz: [ortak Cosmos DB kullanım](use-cases.md).