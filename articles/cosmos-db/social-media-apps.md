---
title: 'Azure Cosmos DB tasarım deseni: Sosyal medya uygulamaları'
description: Sosyal ağlar için Azure Cosmos DB ve diğer Azure hizmetleriyle depolama esnekliğinden yararlanarak bir tasarım modeli hakkında bilgi edinin.
author: ealsur
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: maquaran
ms.openlocfilehash: 45e27b37ca7a1718674914fbe9203b7dc64475b1
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342099"
---
# <a name="going-social-with-azure-cosmos-db"></a>Azure Cosmos DB ile iletişim

İçinde bir yüksek düzeyde birbirine society yaşayan anlamına gelir hayatta belirli bir noktada, bir parçası haline gelir, bir **sosyal ağ**. Sosyal ağlar arkadaşlarınız, iş arkadaşlarınızın, ailesi sürdürebilecek tutmak veya bazen Tutkunuzu ortak ilgi alanlarına kişilerle paylaşmak için kullanın.

Mühendislerin veya geliştiriciler olarak, bu ağlar depolamak ve nasıl bağlantı verilerinizi merak etmiş olabilirsiniz. Veya, bile oluşturmak veya yeni bir sosyal ağ belirli sayıda pazar için Mimari görevli. Bu durumda önemli soruyu ortaya çıkar: Tüm bu veriler nasıl depolanır?

Kullanıcıların resimler, videolar veya hatta müzik gibi ilgili medya makalelerle gönderebileceği yeni ve shiny bir sosyal ağ oluşturduğunuzu düşünün. Kullanıcılar gönderilerine yorum ve derecelendirmeleri puan verin. Kullanıcıların görebileceği ve etkileşim ana Web sitesinin giriş sayfasında gönderilerin bir akış olacaktır. Bu yöntem ilk ancak kolaylık açısından en karmaşık ses değil, şimdi burada durdurun. (Özel kullanıcı akışları ilişkileri tarafından etkilenen içine delve, ancak bu makalenin hedefi gider.)

Bu nedenle, nasıl bu verileri depoladığınız ve nerede?

SQL veritabanlarında deneyimine sahip veya bir kavramı sahip [ilişkisel verileri modelleme](https://en.wikipedia.org/wiki/Relational_model). Bir şey gibi çizim başlatılabilir:

![Göreli bir ilişkisel modeli gösteren diyagram](./media/social-media-apps/social-media-apps-sql.png)

Ölçeklendirilemez mükemmel normalleştirilmiş ve asıl veri yapısı ….

Benim Hayatımı tüm SQL veritabanları ile çalıştım, yanlış elde etmezsiniz. Bunlar harika, ancak her desen, uygulama ve yazılım platformu gibi her senaryo için uygun değil.

Neden bu senaryoda en iyi seçenek SQL değil mi? Tek bir gönderi yapısını göz atalım. Bir Web sitesi veya uygulama post göstermek istiyorsanız bir sorgu ile yalnızca tek bir gönderi göstermek için sekiz tables(!) katılarak... yapmak sahip. Artık bir akışı dinamik olarak yükleme ve ekranda görünen gönderilerin resim ve burada yapacağım görebilirsiniz.

Binlerce sorgusu, içerik sunmak için çok sayıda birleştirmeleri çözmek için yeterli güç ile devasa bir SQL örneği kullanabilirsiniz. Ancak daha basit bir çözüm varsa, neden olur?

## <a name="the-nosql-road"></a>NoSQL yol

Bu makalede, Azure'nın NoSQL veritabanı ile sosyal platformun veri modelleme içine yüklenmesinde size [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hesaplı bir şekilde. Ayrıca, gibi diğer Azure Cosmos DB özelliklerinin nasıl kullanılacağını anlatır [Gremlin API](../cosmos-db/graph-introduction.md). Kullanarak bir [NoSQL](https://en.wikipedia.org/wiki/NoSQL) yaklaşım, verileri JSON biçiminde depolamak ve uygulama [normalleştirilmişlikten çıkarma](https://en.wikipedia.org/wiki/Denormalization), daha önce karmaşık post tek bir dönüştürülebilir [belge](https://en.wikipedia.org/wiki/Document-oriented_database):

    {
        "id":"ew12-res2-234e-544f",
        "title":"post title",
        "date":"2016-01-01",
        "body":"this is an awesome post stored on NoSQL",
        "createdBy":User,
        "images":["https://myfirstimage.png","https://mysecondimage.png"],
        "videos":[
            {"url":"https://myfirstvideo.mp4", "title":"The first video"},
            {"url":"https://mysecondvideo.mp4", "title":"The second video"}
        ],
        "audios":[
            {"url":"https://myfirstaudio.mp3", "title":"The first audio"},
            {"url":"https://mysecondaudio.mp3", "title":"The second audio"}
        ]
    }

Ve bunu tek bir sorgu ile hiçbir birleştirmeleri edinmiş. Bu sorgu çok basit ve kolay anlaşılan ve budget-wise, daha iyi bir sonuç elde etmek için daha az kaynak gerektirir.

Azure Cosmos DB, otomatik dizin oluşturma ile tüm özellikleri dizinlenir emin olur. Otomatik dizin oluşturma bile olabilir [özelleştirilmiş](index-policy.md). Şemadan bağımsız bir yaklaşım farklı ve dinamik yapıları ile belgelerini depolamanıza olanak sağlar. Belki de yarın gönderilerin listesi kategorileri veya diyez etiketlerini ilişkili olmasını istiyor musunuz? Cosmos DB, bizim tarafımızdan gerekli ek çalışma yapmadan yeni eklenen öznitelikler belgelerle işleyecektir.

Diğer gönderiler üst özelliğine sahip olarak bir post hakkındaki yorumları işlenebilir. (Bu uygulama, nesne eşleme basitleştirir.)

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

Akışlar oluşturma verilen ilgi sipariş kimlikleri gönderiyle listesini içerebilir belge oluşturma adımlarından oluşur:

    [
        {"relevance":9, "post":"ew12-res2-234e-544f"},
        {"relevance":8, "post":"fer7-mnb6-fgh9-2344"},
        {"relevance":7, "post":"w34r-qeg6-ref6-8565"}
    ]

Oluşturma tarihine göre sıralanmış gönderiler ile "son" bir akışa sahip olabilir. Veya bu son 24 saat içindeki daha beğenilerin gönderilerinize "sıcak" bir akışa sahip olabilirsiniz. Özel bir akış izleyicilerinizle ve ilgi alanları gibi mantıksal göre her bir kullanıcı için bile uygulayabilirsiniz. Gönderi listesini olmaya. Sağlasa da, bu listeleri oluşturmak nasıl olduğu halde okuma performansını unhindered kalır. Bu listelerden birine edindiğiniz sonra Cosmos DB kullanarak tek bir sorgu alınmamış [anahtar SÖZCÜĞÜ](sql-query-keywords.md#in) teker teker gönderilerin sayfaları almak için.

Akış akışları kullanılarak oluşturulabilir. [Azure uygulama hizmetleri](https://azure.microsoft.com/services/app-service/) arka plan işlemleri: [Webjobs](../app-service/webjobs-create.md). Bir gönderi oluşturulduğunda, arka plan işlemesi kullanarak tetiklenebilir [Azure depolama](https://azure.microsoft.com/services/storage/) [kuyrukları](../storage/queues/storage-dotnet-how-to-use-queues.md) ve kullanarak Tetiklenmiş Web işleri [Azure Webjobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki), uygulama kendi özel mantığı temelinde akışlar içinde yayma gönderin.

Puan ve beğeniler post üzerinden sonunda tutarlı bir ortam oluşturmak için aynı tekniği kullanarak ertelenmiş bir şekilde işlenebilir.

Takipçileri daha. Cosmos DB belge boyutu sınırı vardır ve büyük belge okuma/yazma uygulamanızı ölçeklenebilirliğini etkileyebilir. Bu nedenle bu yapıya sahip bir belge olarak takipçileri depolama düşünebilirsiniz:

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

Bu yapı için birkaç binlerce kullanıcıyla işe yarayabilir takipçileri. Bazı ünlü sıralamalara sahip katılırsa, ancak bu yaklaşım, büyük belge boyutuna olmasına neden olur ve sonunda belge boyutu sınırı neden olabilir.

Bu sorunu çözmek için karma bir yaklaşım kullanabilirsiniz. Kullanıcı istatistikleri belgeyi bir parçası olarak takipçi sayısı depolayabilir:

    {
        "id":"234d-sd23-rrf2-552d",
        "user": "dse4-qwe2-ert4-aad2",
        "followers":55230,
        "totalPosts":452,
        "totalPoints":11342
    }

Azure Cosmos DB kullanarak takipçileri gerçek grafiği depolayabilirsiniz [Gremlin API](../cosmos-db/graph-introduction.md) oluşturmak için [grafiğinize köşe](http://mathworld.wolfram.com/GraphVertex.html) her kullanıcı için ve [kenarlar](http://mathworld.wolfram.com/GraphEdge.html) "A-aşağıdaki-B" koru ilişkiler. Gremlin API'si ile belirli bir kullanıcının takipçileri Al ve kişileri ortak önermek için daha karmaşık sorgular oluşturun. İçerik kategorileri kişilerin keyfini çıkarın veya gibi Graph'a eklerseniz, Akıllı İçerik bulmayı içerir deneyimleri weaving, bu kişilerin gibi izleyin veya bulma kişiler içeriği common ile olabilir önerme başlayabilirsiniz.

Kullanıcı istatistikleri belgeyi yine de kullanıcı Arabirimi veya hızlı profili önizlemeler kartları oluşturmak için kullanılabilir.

## <a name="the-ladder-pattern-and-data-duplication"></a>"Merdiveni" deseni ve veri çoğaltma

Bir postayı başvuran JSON belgesinde fark etmiş olabilirsiniz gibi pek çok tekrarı kullanıcı vardır. Ve, bu yinelenen bir kullanıcı tarafından verilen bu normalleştirilmişlikten çıkarma, açıklayan bilgileri birden fazla yerde bulunabilir anlamına doğru tahmin.

Daha hızlı sorgular için izin vermek için veri çoğaltma ücretler. Bu yan etkiyi sorun bazı eylemler tarafından bir kullanıcının veri değişiklikleri tüm etkinlikleri bulunacak ihtiyacınız varsa kullanıcı hiç olmadığı kadar olmadı ve tüm bunları güncelleştirin olmasıdır. Sağa pratik, ses değil mi?

Uygulamanızdaki her etkinlik için gösteren bir kullanıcı en önemli niteliklerinden tanımlayarak çözmek dağıtacağız. Neden görsel olarak bir post uygulamanızda Göster ve yalnızca oluşturan kişinin adını ve resim Göster, tüm kullanıcı verilerini "oluşturan" özniteliği depoluyor? Yalnızca kullanıcının resmi Göster her açıklama, kullanıcının bilgileri geri kalanını gerçekten gerekmez. Burada bir şey "Merdiveni deseni" çağırmalıyım dahil olur olmasıdır.

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

En küçük adım bir UserChunk en az bir kullanıcıyı tanımlayan bilgileri parçası olarak adlandırılır ve veri çoğaltma için kullanılır. Yalnızca, "göstereceğiz" bilgileri için yinelenen veri boyutunu azaltarak, büyük güncelleştirmelerin olasılığını azaltmak.

İkinci adım, kullanıcı adı verilir. Cosmos DB, en çok erişilen ve kritik performans bağımlı sorguların çoğu üzerinde kullanılacak tam verilerdir. Bir UserChunk tarafından temsil edilen bilgiler içerir.

Genişletilmiş Kullanıcı büyüktür. Kritik kullanıcı bilgilerini ve hızlı bir şekilde okunur gerekmez diğer veriler içerir veya oturum açma işlemi gibi nihai kullanımı vardır. Bu veriler, Cosmos DB, Azure SQL veritabanı veya Azure depolama tabloları dışında depolanabilir.

Neden, kullanıcı bölme ve hatta farklı yerlerde bu bilgileri depolamak? Çünkü bir performans açısından, daha büyük belgelere, costlier sorgular. Belgeler, sosyal ağ için tüm performans bağlı sorgular yapmak için doğru bilgileri ile ince tutun. Tam profil düzenleme, oturum açma bilgileri ve kullanım analizi ve büyük veri girişimler için veri madenciliği Store nihai senaryoları için diğer ek bilgileri ister. Azure SQL veritabanı'nda çalıştığından veri madenciliği için veri toplama yavaş ise, gerçekten umursamaz. Sahip ilgilendiriyor ancak kullanıcılarınız hızlı ve ince bir deneyimi vardır. Cosmos DB'de depolanan kullanıcı, şu kod gibi görünür:

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

Bir öbek özniteliği burada etkilenen bir düzenleme ortaya çıkar, etkilenen belgeleri kolayca bulabilirsiniz. Dizinli öznitelikleri gibi noktası sorguları kullanmanız yeterlidir `SELECT * FROM posts p WHERE p.createdBy.id == "edited_user_id"`ve ardından öbekleri güncelleştirin.

## <a name="the-search-box"></a>Arama kutusu

Kullanıcılar, çok içerik oluşturur. Creators izlemeyin olduğundan, doğrudan kendi içerik akışlarında, belki de olmayabilecek içeriği bulmak ve arama olanağı sağlayabilir olmalıdır ve belki de, yalnızca eski post altı ay önce yaptığınız bulmaya çalıştığınız.

Azure Cosmos DB kullandığımızdan, kolayca bir arama motoru kullanarak uygulayabileceğiniz [Azure Search](https://azure.microsoft.com/services/search/) arama işlemi ve kullanıcı Arabirimi dışındaki herhangi bir kod yazmaya gerek kalmadan birkaç dakika içinde.

Neden bu işlem çok kolay oluyor?

Azure Search'ü uygular, çağrı [dizin oluşturucular](https://msdn.microsoft.com/library/azure/dn946891.aspx), arka plan işlemleri, veri depoları içindeki bu kanca ve automagically ekleme, güncelleştirme veya nesnelerinizi dizinlerde kaldırın. Destekledikleri bir [Azure SQL veritabanı dizin oluşturucular](https://blogs.msdn.microsoft.com/kaevans/2015/03/06/indexing-azure-sql-database-with-azure-search/), [Azure blob dizin oluşturucuları](../search/search-howto-indexing-azure-blob-storage.md) ve ne [Azure Cosmos DB dizinleyici](../search/search-howto-index-documentdb.md). Cosmos DB bilgileri Azure Search için geçişin oldukça basittir. Tek yapmanız gereken şekilde hem teknolojiler bilgileri JSON biçiminde depolamak [dizininizi oluşturma](../search/search-create-index-portal.md) ve istediğiniz dizinli belgelerinizi öznitelikleri eşleyin. Bu kadar! Verilerinizin boyutuna bağlı olarak, tüm içeriğinizi üzerine dakika içinde en iyi hizmet olarak arama çözümü bulut altyapısı tarafından aratılmak üzere kullanılabilir.

Azure arama hakkında daha fazla bilgi için ziyaret edebilirsiniz [arama Hitchhiker'ın Kılavuzu](https://blogs.msdn.microsoft.com/mvpawardprogram/2016/02/02/a-hitchhikers-guide-to-search/).

## <a name="the-underlying-knowledge"></a>Temel alınan bilgi

Büyür ve her gün büyüdükçe tüm bu içerik depolama sonrasında, düşünme bulabilirsiniz: Tüm bu akış bilgileri ile my kullanıcıların neler yapabilirim?

Yanıt oldukça basittir: İş ve buradan edinin yerleştirin.

Ancak ne öğrenebilir? Birkaç kolay örnekler [yaklaşım analizi](https://en.wikipedia.org/wiki/Sentiment_analysis), içerik tabanlı kullanıcı tercihlerine göre öneriler veya, sosyal ağ tarafından yayımlanan içerik emin yapan bile bir otomatik content moderator ailesi için güvenlidir.

Büyük olasılıkla, kancalandı aldım, bu desenleri ve basit veritabanları ve dosyaları dışında bilgi ayıklamak için bazı Biyofizik matematik Bilimleri gerekir, ancak yanlış olurdu düşünecek.

[Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/)in bir parçası olan [Cortana Intelligence Suite](https://social.technet.microsoft.com/wiki/contents/articles/36688.introduction-to-cortana-intelligence-suite.aspx), içinde basit bir Sürükle ve bırak arabirimi algoritmalarını kullanarak iş akışları oluşturun, kendi algoritmalar koduolanaksağlayantamolarakyönetilenbuluthizmeti[ R](https://en.wikipedia.org/wiki/R_\(programming_language\)), veya bazı zaten oluşturulmuş ve kullanıma hazır API'leri gibi kullanın: [Metin analizi](https://gallery.cortanaanalytics.com/MachineLearningAPI/Text-Analytics-2), [Content Moderator, veya [önerileri](https://gallery.azure.ai/Solution/Recommendations-Solution).

Bu makine öğrenimi senaryoların herhangi birini elde etmek için kullanabileceğiniz [Azure Data Lake](https://azure.microsoft.com/services/data-lake-store/) farklı kaynaklardan bilgi alacaksınız. Ayrıca [U-SQL](https://azure.microsoft.com/documentation/videos/data-lake-u-sql-query-execution/) bilgi işlem ve Azure Machine Learning tarafından işlenebilen bir çıktı oluşturur.

Başka bir seçenek kullanmaktır [Azure Bilişsel Hizmetler](https://www.microsoft.com/cognitive-services) kullanıcılarınız yalnızca anlamanız bunları daha iyi içerik; analiz etmek için (ile bunların yazma analiz aracılığıyla [metin analizi API'si](https://www.microsoft.com/cognitive-services/en-us/text-analytics-api)), ancak Ayrıca istenmeyen veya yetişkin içeriği Algıla ve buna göre hareket ile [görüntü işleme API'si](https://www.microsoft.com/cognitive-services/en-us/computer-vision-api). Bilişsel hizmetler kullanmak için Machine Learning bilgi herhangi bir türden gerektirmeyen çok sayıda kullanıma hazır çözümler sunar.

## <a name="a-planet-scale-social-experience"></a>Çok büyük ölçekli bir sosyal deneyimi

Son yoktur, ancak önemli bir makale miyim, ele almalıdır: **ölçeklenebilirlik**. Bir mimari tasarlarken, her bileşen kendi üzerinde ölçeklendirmeniz gerekir. Daha büyük bir coğrafi kapsama sahip olmasını istediğiniz veya daha fazla veriyi işlemek sonunda gerekir. Her iki görevi gerçekleştirmekten ne olduğunu bir **yapmanız** Cosmos DB ile.

Cosmos DB, dinamik bölümleme,-hazır destekler. Bölümler göre otomatik olarak oluşturur bir verilen **bölüm anahtarı**, belgelerinizi özniteliği olarak tanımlanır. Tasarım zamanında doğru bölüm anahtarı tanımlayarak yapılmalıdır. Daha fazla bilgi için [Azure Cosmos DB'de bölümleme](partitioning-overview.md).

Sosyal deneyimi için bölümleme stratejisinde, sorgu ve yol ile hizalamanız gerekir. (Örneğin, aynı bölüm içindeki okuma istenen ve çok sayıda bölüme yazma işlemleri yayılma tarafından "etkin nokta" kaçının.) Bazı seçenekler şunlardır: bölümler içerik kategorisi, coğrafi bölge veya kullanıcı tarafından zamana bağlı bir anahtar (gün/ay/hafta) bağlı. Tüm gerçekten nasıl verileri sorgulamak ve sosyal deneyiminizi verileri gösterme bağlıdır.

Cosmos DB, sorgularınızı çalışır (dahil olmak üzere [toplamalar](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/)) tüm bölümler arasında saydam bir şekilde, bu nedenle gerekmeyen verileriniz arttıkça, herhangi bir mantık ekleyin.

Sonunda trafik ve kaynak tüketiminize zamanla çok (ölçülen [RU](request-units.md), veya istek birimi) artacaktır. Okuma ve kullanıcı tabanınız büyüdükçe daha sık yazma. Kullanıcı tabanı, oluşturma ve daha fazla içerik okuma başlayacaktır. Bu nedenle yeteneğini **aktarım hızınızı ölçeklendirme** yaşamsal önem taşır. RU artırmak kolaydır. Azure portalı ya da birkaç tıklamayla yapın [API komutları verme](https://docs.microsoft.com/rest/api/cosmos-db/replace-an-offer).

![Bir bölüm anahtarı tanımlayarak ve büyütme](./media/social-media-apps/social-media-apps-scaling.png)

Şeyleri daha iyi almaya devam ederseniz ne olur? Kullanıcılar başka bir bölge, ülke veya Kıta aşırı platformunuz dikkat edin ve kullanmaya başlamak varsayalım. Hangi harika şaşkınlık!

Ancak şu durumda bekleyin! Yakında platformunuz yaşadıkları deneyimleri en iyi olmayan farkında olun. Şu ana kadar işletimsel bölgenizi uzağa gecikme saltanatı oldukları. Ayrıca, çıkmak için bunları açıkça istemezsiniz. Vardı, kolay bir yol yalnızca **küresel erişiminizi genişletmek**? Yok!

Cosmos DB sayesinde [verilerinizi dünya çapında çoğaltın](../cosmos-db/tutorial-global-distribution-sql-api.md) ve şeffaf bir şekilde birkaç tıklamayla ve otomatik olarak ile kullanılabilir bölgelerden arasından, [istemci kodu](../cosmos-db/tutorial-global-distribution-sql-api.md). Bu işlem ayrıca, bulunabileceği anlamına gelir [birden çok yük devretme bölgeleri](high-availability.md).

Verilerinizi dünya çapında çoğaltın, müşterilerinizin Avantajdan sürebilir emin olmanız gerekir. Bir web ön ucu kullanıyorsanız veya dağıtabileceğiniz API'leri mobil istemcilerden erişim, [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) ve Azure App Service, genişletilmiş desteklemek için bir performans yapılandırması kullanarak tüm istenen bölgeler üzerinde kopyalama genel Kapsam. İstemcileriniz, ön uç veya API'leri eriştiğinizde, bunların hangi sırayla, Cosmos DB yerel çoğaltmaya bağlanacak en yakın App Service'e, yönlendirilmiş.

![Sosyal platformunuz için genel kapsamı ekleme](./media/social-media-apps/social-media-apps-global-replicate.png)

## <a name="conclusion"></a>Sonuç

Bu makalede, sosyal ağlar, düşük maliyetli Hizmetleri ile Azure'da tamamen oluşturma alternatifleri içine bazı ışık sheds. sonuçları, "Merdiveni" adlı bir çok katmanlı depolama çözümü ve veri dağıtım kullanımını teşvik tarafından sunar.

![Sosyal ağ için Azure Hizmetleri arasındaki etkileşim diyagramı](./media/social-media-apps/social-media-apps-azure-solution.png)

Gerçekte bu tür senaryolara için Gümüş madde işareti yok olmasıdır. Harika deneyimler oluşturmak üzere bize izin harika hizmetler birleşimiyle oluşturulan synergy olduğu: hız ve Azure Cosmos DB'nin harika sosyal bir uygulama olan bir birinci sınıf arama çözümü olan Azure Search gibi arkasında zeka sağlamak için arama özgürlüğü büyük miktarda veri ve analiz için Azure Machine Learning gücünü depolamak için Azure App Services'ı değil bile dilden uygulamaları ancak güçlü bir arka plan işlemleri ve Genişletilebilir Azure depolama barındırmak için Azure SQL veritabanı ve esneklik Bilgi Bankası oluşturun ve, işlemler için geri bildirim sağlamak ve bize yardımcı bilgiler doğru içeriği doğru kullanıcılara sunmak.

## <a name="next-steps"></a>Sonraki adımlar

Cosmos DB için kullanım örnekleri hakkında daha fazla bilgi edinmek için [Cosmos DB genel kullanım örnekleri](use-cases.md).
