---
title: "Azure Cosmos DB sık sorulan sorular | Microsoft Docs"
description: "Azure Cosmos DB, genel olarak dağıtılmış, birden çok model veritabanı hizmeti hakkında sık sorulan soruların yanıtlarını alın. Kapasite, performans düzeyleri ve ölçeklendirme hakkında bilgi edinin."
keywords: "Sık sorulan sorular, documentdb, azure, Microsoft azure veritabanı soruları"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: b68d1831-35f9-443d-a0ac-dad0c89f245b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: mimig
ms.openlocfilehash: 0f45468616884a6866bd95ef53acab71b4fed06c
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="azure-cosmos-db-faq"></a>Azure Cosmos DB SSS
## <a name="azure-cosmos-db-fundamentals"></a>Azure Cosmos DB temelleri
### <a name="what-is-azure-cosmos-db"></a>Azure Cosmos DB nedir?
Azure Cosmos DB, şemasız verilerde zengin sorgulama sunan yapılandırılabilir ve güvenilir performans sağlanmasına yardımcı olan bir genel çoğaltılmış, birden çok model veritabanı hizmetidir ve hızlı geliştirme sağlar. Tüm güç tarafından yedeklenir ve Microsoft Azure'nın bir yönetilen platform sağlanır. 

Azure Cosmos DB web, mobil, oyun için doğru çözümdür ve tahmin edilebilir iş çıkarma, yüksek kullanılabilirlik, düşük gecikme süresi ve şemasız veri modeli IOT uygulamaları anahtar gereksinimleridir. Şema esnekliği ve zengin dizin oluşturma sağlar ve tümleşik JavaScript ile çok belgeli işlem desteğini içerir. 

Daha fazla veritabanı soruları yanıtlar ve dağıtma ve bu hizmeti kullanmak için yönergeleri [Azure Cosmos DB belge sayfasının] görmek için ((https://docs.microsoft.com/azure/cosmos-db/).

### <a name="what-happened-to-documentdb"></a>DocumentDB için ne oldu?
DocumentDB API desteklenen API'ları ve veri modelleri için Azure Cosmos DB biridir. Ayrıca, Azure Cosmos DB, grafik API'si (Önizleme), tablo API ve MongoDB API ile destekler. Daha fazla bilgi için bkz: [DocumentDB müşterilerden sorular](#moving-to-cosmos-db).

### <a name="how-do-i-get-to-my-documentdb-account-in-the-azure-portal"></a>Azure portalında my DocumentDB hesabına nasıl edinebilirim?
Azure portalında, sol bölmede Azure Cosmos DB simgesine tıklayın. Bir DocumentDB hesabı önce olsaydı, artık fatura değişiklik olmadan Azure Cosmos DB hesabınız var.

### <a name="what-are-the-typical-use-cases-for-azure-cosmos-db"></a>Azure Cosmos DB için genel kullanım örnekleri nelerdir?
Azure Cosmos DB yeni web, mobil, oyun için iyi bir seçenektir ve IOT uygulamaları burada otomatik ölçeğin, tahmin edilebilir performans, hızlı sırası milisaniye yanıt sürelerinin ve sorgulama şemasız verilerde önemlidir. Azure Cosmos DB kendisi, uygulama veri modellerinin sürekli yinelenmesini destekleme ve hızlı geliştirme için uygundur. Kullanıcı tarafından oluşturulan içeriği ve verileri yöneten uygulamalar [ortak kullanım durumları için Azure Cosmos DB](use-cases.md). 

### <a name="how-does-azure-cosmos-db-offer-predictable-performance"></a>Azure Cosmos DB tahmin edilebilir performansı nasıl sunar?
A [istek birimi](request-units.md) (RU) Azure Cosmos veritabanı işleme ölçü değil. 1 RU verimlilik bir 1 KB belge GET üretimini karşılık gelir. Her işlem Azure Cosmos DB, okuma, yazma, SQL sorguları ve saklı yordam yürütmeleri dahil olmak üzere, işlemi tamamlamak için gereken işlemeyi temel alan bir belirleyici RU değer içeriyor. CPU, IO, bellek ve bunların her birinin uygulama işlemenizi nasıl etkileyeceğini hakkında düşünmek yerine, tek bir RU ölçü açısından düşünebilirsiniz.

Saniye başına RUs bakımından sağlanan işleme sahip her Azure Cosmos DB kapsayıcısı ayırabilirsiniz. Her ölçekten uygulama için RU değerlerini ölçmek için istekleri ayrı ayrı kıyaslayabilir ve tüm istekler genelinde istek birimlerinin toplam işlemek için bir kapsayıcı sağlayın. Ayrıca, ölçeği veya uygulamanızın ihtiyaçları geliştikçe, kapsayıcının işleme ölçeklendirin. İstek birimleri hakkında daha fazla bilgi ve Yardım için kapsayıcı belirlemek bkz gereksinimlerini [üretilen iş gereksinimlerini tahmin etme](request-units.md#estimating-throughput-needs) deneyin [verimlilik hesaplayıcı](https://www.documentdb.com/capacityplanner). Terim *kapsayıcı* burada başvuran bir DocumentDB API koleksiyonu, grafik API'si grafik, MongoDB API koleksiyonu ve tablo API tabloya başvuruyor. 

### <a name="how-does-azure-cosmos-db-support-various-data-models-such-as-keyvalue-columnar-document-and-graph"></a>Azure Cosmos DB anahtar/değer, sütunlu, belge ve grafik gibi çeşitli veri modelleri nasıl destekler?

Anahtar/değer (tablo), sütunlu, belge ve tüm (atom, kaydeder ve dizileri) ARS nedeniyle yerel olarak desteklenen bu Azure Cosmos DB tasarım modelleri olan grafik verileri üzerine kurulmuştur. Atom, kaydeder ve dizilerini kolayca eşlenen ve çeşitli veri modelleri öngörülen. Kullanılabilir sağ şimdi (DocumentDB, MongoDB, tablo ve grafik API'leri) apı'leridir modelleri alt kümeleri için ve başkaları için ek veri modelleri belirli gelecekte kullanılabilir olacaktır.

Azure Cosmos DB bir şema belirsiz dizin oluşturma altyapısı herhangi bir şemayı ya da ikincil dizinlerin geliştiriciden gerek kalmadan alır tüm veriler otomatik olarak dizin oluşturma işlemi özellikli vardır. Hangi dizin ve sorgu alt sistemleri işleme depolama düzeninden ayırırsınız mantıksal dizin düzenleri (ters, sütunlu, ağaç) kümesi altyapısı kullanır. Cosmos DB, aynı zamanda bunları çekirdek veri modeli (1) ve (2) birden fazla veri modeli yerel olarak destekleyen benzersiz olarak özellikli kolaylaştırarak mantıksal dizin düzenleri verimli bir şekilde Çevir ve kablo protokolleri ve API kümesi genişletilebilir bir biçimde desteklemek için silebilir.

### <a name="is-azure-cosmos-db-hipaa-compliant"></a>Azure Cosmos DB HIPAA uyumlu mu?
Evet, Azure Cosmos DB HIPAA ile uyumlu değil. HIPAA, bağımsız olarak tanımlanabilen sağlık bilgilerinin kullanımı, açıklanması ve korunması için gereksinimler belirler. Daha fazla bilgi için bkz. [Microsoft Güven Merkezi](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### <a name="what-are-the-storage-limits-of-azure-cosmos-db"></a>Azure Cosmos DB depolama sınırları nelerdir?
Bir kapsayıcı Azure Cosmos DB'de depolayabilir veri toplam miktarı için bir sınır yoktur.

### <a name="what-are-the-throughput-limits-of-azure-cosmos-db"></a>Azure Cosmos DB işleme sınırları nelerdir?
Azure Cosmos DB içinde bir kapsayıcı destekleyen verimlilik toplam miktarı için bir sınır yoktur. İş yükünüzün kabaca eşit yeterince büyük sayıda bölüm anahtarı arasında dağıtmak için anahtar fikirdir bakın.

### <a name="how-much-does-azure-cosmos-db-cost"></a>Nasıl Azure Cosmos DB maliyeti nedir?
Ayrıntılar için başvurmak [Azure Cosmos fiyatlandırma ayrıntıları DB](https://azure.microsoft.com/pricing/details/cosmos-db/) sayfası. Azure Cosmos DB kullanım ücretleri, sağlanan kapsayıcıları, kapsayıcıları çevrimiçi olduğu saat sayısı sayısı tarafından belirlenir ve her kapsayıcı için sağlanan işleme. Terim *kapsayıcıları* burada DocumentDB API koleksiyonu, grafik API'si grafik, MongoDB API koleksiyonu ve tablo API tabloları başvuruyor. 

### <a name="is-a-free-account-available"></a>Ücretsiz bir hesap var mı?
Evet, hiçbir taahhüdü olmadan ücret ödemeden zaman sınırlı hesap için kaydolabilirsiniz. Kaydolmak için ziyaret [Azure Cosmos DB ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/) veya daha fazla okuma [Azure Cosmos DB ile ilgili SSS deneyin](#try-cosmos-db).

Azure'da yeniyseniz için kaydolabilirsiniz bir [ücretsiz Azure hesabına](https://azure.microsoft.com/free/), hangi size 30 gün ve ve tüm Azure hizmetlerini denemek için kredi. Visual Studio aboneliğiniz varsa, aynı zamanda hakkınız bulunur [ücretsiz Azure kredisi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) herhangi bir Azure hizmetinde kullanmak için. 

Aynı zamanda [Azure Cosmos DB öykünücüsü](local-emulator.md) geliştirmek ve bir Azure aboneliği oluşturmadan uygulamanızı yerel olarak ücretsiz, test etmek için. Uygulamanızı Azure Cosmos DB öykünücüsünde nasıl çalıştığını ile memnun kaldığınızda, bulutta bir Azure Cosmos DB hesabı kullanmaya geçiş yapabilirsiniz.

### <a name="how-can-i-get-additional-help-with-azure-cosmos-db"></a>Ek Yardım Azure Cosmos DB ile nasıl alabilirim?
Yardıma ihtiyacınız olursa, bize üzerinde ulaşmak [yığın taşması](http://stackoverflow.com/questions/tagged/azure-cosmosdb) veya [MSDN Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB), veya posta göndererek Azure Cosmos DB mühendislik ekibi ile bire bir sohbet zamanlama [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com). 

<a id="try-cosmos-db"></a>
## <a name="try-azure-cosmos-db-subscriptions"></a>Azure Cosmos DB abonelikleri deneyin

Şimdi bir zaman sınırlı Azure Cosmos DB deneyimi ücret ve taahhüt ücretsiz bir abonelik olmadan keyfini çıkarabilirsiniz. Bir Azure Cosmos DB deneyin abonelik için kaydolmak için Git [Azure Cosmos DB ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/). Bu abonelik ayrıdır [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/)ve Azure ücretsiz deneme sürümü veya bir Azure yanı sıra kullanılabilecek Ücretli aboneliği. 

Try Azure Cosmos DB abonelikleri Azure portalında sonraki kullanıcı kimliği ile ilişkili diğer abonelikler görünür. 

Aşağıdaki koşullar deneyin Azure Cosmos DB abonelikler için geçerlidir:

* Bir kapsayıcı SQL (DocumentDB API), Gremlin (grafik API'si) ve tablo hesapları için abonelik başına.
* MongoDB hesapları için abonelik başına en fazla 3 koleksiyonları.
* 10 GB depolama kapasitesi.
* Aşağıda, genel çoğaltma kullanılabilir [Azure bölgeleri](https://azure.microsoft.com/regions/): Orta ABD, Kuzey Avrupa ve Güneydoğu Asya
* En yüksek verimlilik 5 K RU/s.
* Abonelikler, 24 saat sonra süresi dolacak ve en fazla toplam 48 saat için genişletilebilir.
* Azure destek biletleri için Azure Cosmos DB deneyin hesapları oluşturulamıyor; Ancak, mevcut destek planları ile aboneleri için destek sağlanır. 

## <a name="set-up-azure-cosmos-db"></a>Azure Cosmos DB'yi yedekleyin ayarlayın
### <a name="how-do-i-sign-up-for-azure-cosmos-db"></a>Nasıl Azure Cosmos DB kaydolabilirim?
Azure portalında Azure Cosmos DB kullanılabilir. İlk olarak, Azure aboneliği için kaydolun. Oturum açtığınız sonra Azure Aboneliğinize bir DocumentDB API, grafik API'si (Önizleme), tablo API veya MongoDB API hesabı ekleyebilirsiniz.

### <a name="what-is-a-master-key"></a>Ana anahtar nedir?
Ana anahtar, bir hesaptaki tüm kaynaklara erişmeyi sağlayan bir güvenlik belirtecidir. Anahtara sahip kişiler okuma ve yazma erişimi veritabanı hesabındaki tüm kaynaklara. Ana anahtarları dağıtırken dikkatli olun. Birincil ana anahtar ve ikincil ana anahtar kullanılabilir **anahtarları** dikey [Azure portal][azure-portal]. Anahtarlar hakkında daha fazla bilgi için bkz: [görüntüleme, kopyalama ve yeniden oluşturma erişim tuşları](manage-account.md#keys).

### <a name="what-are-the-regions-that-preferredlocations-can-be-set-to"></a>PreferredLocations ayarlanabilir bölgeleri nelerdir? 
PreferredLocations değeri Cosmos DB kullanılabilir olduğu Azure bölgeleri için ayarlanabilir. Kullanılabilir bölgelerin bir listesi için bkz: [Azure bölgeleri](https://azure.microsoft.com/regions/).

### <a name="is-there-anything-i-should-be-aware-of-when-distributing-data-across-the-world-via-the-azure-datacenters"></a>Verileri Azure veri merkezleri aracılığıyla dünya çapında dağıtırken farkında olmalıdır bir şey var mı? 
Azure Cosmos DB varsa belirtildiği gibi tüm Azure bölgeler arasında [Azure bölgeleri](https://azure.microsoft.com/regions/) sayfası. Çekirdek hizmeti olduğundan, her yeni bir veri merkezine Azure Cosmos DB durum vardır. 

Bir bölge ayarladığınızda, Azure Cosmos DB sovereign ve kamu Bulutlar uyar unutmayın. Diğer bir deyişle, sovereign bir bölgede bir hesap oluşturursanız, o sovereign bölgesinin dışına çoğaltma yapamaz. Benzer şekilde, bir dış hesap sovereign diğer konumlardan içine çoğaltmayı etkinleştiremezsiniz. 

## <a name="develop-against-the-documentdb-api"></a>API Documentdb'de geliştirme

### <a name="how-do-i-start-developing-against-the-documentdb-api"></a>DocumentDB API karşı geliştirme nasıl başlamanız gerekir?
Microsoft DocumentDB API sağlanmıştır [Azure portal][azure-portal]. Önce Azure aboneliği için kaydolmanız gerekir. Azure aboneliği için kaydolduktan sonra DocumentDB API kapsayıcı Azure aboneliğiniz ekleyebilirsiniz. Bir Azure Cosmos DB hesap ekleme ile ilgili yönergeler için bkz: [Azure Cosmos DB veritabanı hesabı oluşturma](create-documentdb-dotnet.md#create-account). Bir DocumentDB hesabı geçmişte olsaydı, artık Azure Cosmos DB hesabınız var. 

.NET, Python, Node.js, JavaScript ve Java için [SDK'lar](documentdb-sdk-dotnet.md) kullanılabilir. Geliştiriciler ayrıca kullanabileceğiniz [RESTful HTTP API'lerini](/rest/api/documentdb/) çeşitli platformlardan ve dillerden Azure Cosmos DB kaynakları ile etkileşim kurmak için.

### <a name="can-i-access-some-ready-made-samples-to-get-a-head-start"></a>Head başlamak için hazır bazı örnekleri erişebilir mi?
DocumentDB API için örnek [.NET](documentdb-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md), ve [Python](documentdb-python-samples.md) SDK'ları Github'da bulunmaktadır.


### <a name="does-the-documentdb-api-database-support-schema-free-data"></a>DocumentDB API'si veritabanı şemasız verileri destekler mi?
Evet, DocumentDB API şema tanımları veya ipuçları olmadan rastgele JSON belgelerinin depolamak uygulamaları sağlar. Veriler Azure Cosmos DB SQL sorgu arabirimi yoluyla sorgu için hemen kullanılabilir.  

### <a name="does-the-documentdb-api-support-acid-transactions"></a>DocumentDB API ACID işlemlerini destekler mi?
Evet, DocumentDB API JavaScript saklı yordamları ve Tetikleyicileri ifade edilen belgeler arası işlemleri destekler. İşlemler her bir koleksiyonun içindeki tek bir bölümün kapsamındadır ve "tümü veya hiçbiri," olarak ACID semantiği ile yürütülen diğer eş zamanlı yürütme kodları ve kullanıcı isteklerinden ayrı. Sunucu tarafı JavaScript uygulama kodunun yürütülmesi özel durumlar varsa, tüm işlem geri alındı. İşlemler hakkında daha fazla bilgi için bkz: [veritabanı program işlemleri](programming.md#database-program-transactions).

### <a name="what-is-a-collection"></a>Koleksiyon nedir?
Bir koleksiyon, belgeler ve bunların ilişkili JavaScript uygulama mantığının grubudur. Faturalanabilir bir varlık, bir koleksiyondur nerede [maliyet](performance-levels.md) üretilen işi tarafından belirlenir ve depolama kullanılır. Koleksiyonlar bir veya daha fazla bölüm veya sunucuları kapsayabilir ve neredeyse sınırsız miktarda depolama veya işlemeyi işleyebilecek şekilde ölçeklendirilebilir.

Ayrıca fatura varlıklar için Azure Cosmos DB koleksiyonlarıdır. Her koleksiyon saatlik olarak faturalandırılır sağlanan işlemeyi temel alan ve kullanılan depolama alanı. Daha fazla bilgi için bkz: [Azure DB Cosmos fiyatlandırma](https://azure.microsoft.com/pricing/details/cosmos-db/). 

### <a name="how-do-i-create-a-database"></a>Veritabanı nasıl oluşturulur?
Kullanarak veritabanları oluşturabilirsiniz [Azure portal](https://portal.azure.com)açıklandığı gibi [bir koleksiyon Ekle](create-documentdb-dotnet.md#create-collection), bir, [Azure Cosmos DB SDK'ları](documentdb-sdk-dotnet.md), veya [REST API'leri](/rest/api/documentdb/). 

### <a name="how-do-i-set-up-users-and-permissions"></a>Kullanıcıları ve izinleri nasıl ayarlarım?
Aşağıdakilerden birini kullanarak, kullanıcılar ve izinler oluşturabilirsiniz [Cosmos DB API SDK'ları](documentdb-sdk-dotnet.md) veya [REST API'leri](/rest/api/documentdb/).  

### <a name="does-the-documentdb-api-support-sql"></a>DocumentDB API SQL destekliyor mu?
SQL sorgu dili Gelişmiş bir SQL tarafından desteklenen sorgu işlevi alt kümesidir. Azure Cosmos DB SQL sorgu dili zengin hiyerarşik ve ilişkisel işleçler ve genişletilebilirlik JavaScript tabanlı, kullanıcı tanımlı işlevler (UDF'ler) aracılığıyla sağlar. JSON dil bilgisi, hem Azure Cosmos DB otomatik dizin oluşturma teknikleri hem de Azure Cosmos DB SQL sorgu diyalekti tarafından kullanılan etiketli düğümleri ağaçlar JSON belgeleri modellenmesini sağlar. SQL dil bilgisinin kullanma hakkında daha fazla bilgi için bkz: [QueryDocumentDB] [ query] makalesi.

### <a name="does-the-documentdb-api-support-sql-aggregation-functions"></a>DocumentDB API SQL toplama işlevleri destekliyor mu?
DocumentDB API, düşük gecikme süreli toplama toplama işlevleri aracılığıyla herhangi bir ölçekte destekler `COUNT`, `MIN`, `MAX`, `AVG`, ve `SUM` SQL dil bilgisinin aracılığıyla. Daha fazla bilgi için bkz: [toplama işlevlerinin](documentdb-sql-query.md#Aggregates).

### <a name="how-does-the-documentdb-api-provide-concurrency"></a>DocumentDB API eşzamanlılığı nasıl sağlar?
DocumentDB API HTTP varlık etiketleri veya Etag'ler aracılığıyla iyimser eşzamanlılık denetimini (OCC) destekler. Her DocumentDB API kaynakları bir Etag'e sahiptir ve bir belgeyi her güncelleştirildiğinde ETag sunucu üzerinde ayarlanır. ETag üstbilgisi ve geçerli değeri tüm yanıt iletileri dahil edilir. Etag'ler IF-Match üst bilgisi bir kaynağın güncelleştirilmesi gerekip gerekmediğini karar vermek sunucu izin vermek için kullanılabilir. IF-Match değer karşı denetlenecek ETag değerdir. ETag değeri sunucunun ETag değeri eşleşirse, kaynak güncelleştirilir. ETag geçerli ise sunucu işlemi reddeder bir "HTTP 412 önkoşul hatası" yanıt kodu. İstemci daha sonra yeniden kaynak için geçerli ETag değeri almaya kaynak getirir. Ayrıca, Etag'ler If-None-Match üstbilgisi ile bir kaynağa yeniden getirin gerekli olup olmadığını belirlemek için kullanılabilir.

İyimser eşzamanlılık .NET içinde kullanmak için [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) sınıfı. .NET örnek için bkz: [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) github'da DocumentManagement örnekteki.

### <a name="how-do-i-perform-transactions-in-the-documentdb-api"></a>DocumentDB API içinde nasıl işlemler gerçekleştirebilir?
DocumentDB API JavaScript saklı yordamları ve Tetikleyicileri aracılığıyla dil ile tümleşik işlemleri destekler. Betiklerin içindeki tüm veritabanı işlemleri, anlık görüntü yalıtımı altında yürütülür. Tek bölümlü bir koleksiyon ise, yürütme koleksiyona kapsamlıdır. Koleksiyon bölümlendiğinde ise, yürütme koleksiyondaki aynı bölüm anahtarı değerine sahip belgelerde kapsamlıdır. Belge sürümlerinin anlık görüntüsü (ETag'ler) ise işlem başlangıcında alınır ve yalnızca betik başarılı olursa uygulanır. JavaScript bir hata oluşturursa işlem geri alınır. Daha fazla bilgi için bkz: [Azure Cosmos DB için sunucu tarafı JavaScript programlama](programming.md).

### <a name="how-can-i-bulk-insert-documents-into-cosmos-db"></a>Nasıl ı toplu belgeleri Cosmos Veritabanına ekleme?
Toplu belgeleri Azure Cosmos Veritabanına iki yoldan biriyle ekleme:

* Bölümünde açıklandığı gibi veri geçiş aracı [Azure Cosmos DB veritabanı geçiş aracını](import-data.md).
* Saklı yordamlar, açıklandığı gibi [Azure Cosmos DB için sunucu tarafı JavaScript programlama](programming.md).

### <a name="does-the-documentdb-api-support-resource-link-caching"></a>DocumentDB API desteği kaynak bağlantıyı önbelleğe almayı mu?
Evet, Azure Cosmos DB bir RESTful hizmeti olduğu için kaynak bağlantıları sabittir ve önbelleğe alınabilir. DocumentDB API istemciler herhangi bir kaynak benzeri belge veya koleksiyon yapılan okumalar için bir "If-None-Match" üst bilgisi belirtin ve sunucu sürümü değiştikten sonra kendi yerel kopyalarını güncelleştirin.

### <a name="is-a-local-instance-of-documentdb-api-available"></a>DocumentDB API yerel bir örneğini var mı?
Evet. [Azure Cosmos DB öykünücüsü](local-emulator.md) Cosmos DB hizmetinin yüksek doğruluk öykünmesi sağlar. Azure Cosmos JSON belgelerini sorgulamak için destek dahil olmak üzere sağlama DB'ye, aynı işlevselliği destekler ve koleksiyonları ölçekleme ve yürütme yordamları ve Tetikleyicileri depolanır. Geliştirmek ve Azure Cosmos DB öykünücüsü kullanarak uygulamaları test ve bunları Azure'da tek bir yapılandırma için Azure Cosmos DB bağlantı uç noktasına değişikliği yaparak genel bir ölçekte dağıtabilirsiniz.

## <a name="develop-against-the-api-for-mongodb"></a>Karşı API MongoDB için geliştirme
### <a name="what-is-the-azure-cosmos-db-api-for-mongodb"></a>Azure Cosmos DB API MongoDB için nedir?
Azure Cosmos DB API MongoDB için uygulamaların kolayca ve şeffaf bir şekilde varolan, topluluk tarafından desteklenen Apache MongoDB API'leri ve sürücüleri kullanarak yerel Azure Cosmos DB veritabanı altyapısı ile iletişim kurmasına olanak sağlayan bir uyumluluk katmanıdır. Geliştiriciler, Azure Cosmos DB yararlanmak uygulamaları oluşturmak için varolan MongoDB aracı zincirlerini ve yetenekleri artık kullanabilirsiniz. Geliştiriciler otomatik dizin oluşturma işlemi, yedekleme bakım, mali yedeklenmiş hizmet düzeyi sözleşmelerine (SLA) vb. dahil benzersiz yeteneklerini Azure Cosmos DB yararlanır.

### <a name="how-do-i-connect-to-my-api-for-mongodb-database"></a>My API MongoDB veritabanı için nasıl bağlanabiliyor?
MongoDB için head üzerinden için Azure Cosmos DB API'sine bağlanmak için en hızlı yoludur [Azure portal](https://portal.azure.com). Hesabınıza gidin ve ardından sol gezinti menüsünde tıklatın **Hızlı Başlangıç**. Hızlı Başlangıç, veritabanına bağlanmak için kod parçacıkları almak için en iyi yoludur. 

Azure Cosmos DB sıkı güvenlik gereksinimlerine ve standartlarını uygular. Azure Cosmos DB hesapları kimlik doğrulaması ve SSL üzerinden güvenli iletişim gerektiren, bu nedenle TLSv1.2 kullandığınızdan emin olun.

Daha fazla bilgi için bkz: [apı'nize MongoDB veritabanı için bağlantı](connect-mongodb-account.md).

### <a name="are-there-additional-error-codes-for-an-api-for-mongodb-database"></a>API MongoDB veritabanı için ek hata kodları var mı?
Ortak MongoDB hata kodları ek olarak, kendi özel hata kodlarını MongoDB API vardır:


| Hata               | Kod  | Açıklama  | Çözüm  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | Koleksiyon için sağlanan istek birimi oranı aştı ve daraltıldı tüketilen istek birimleri toplam sayısı. | Azure portalı koleksiyonundan verimini ölçekleme veya yeniden denemeden göz önünde bulundurun. |
| ExceededMemoryLimit | 16501 | Çok kiracılı bir hizmet işlemi istemcinin bellek birimi aştı. | İşlem daha kısıtlayıcı sorgu ölçütlerini aracılığıyla kapsamını azaltmak veya başvurun Destek'ten [Azure portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). <br><br>Örnek:  *&nbsp; &nbsp; &nbsp; &nbsp;db.getCollection('users').aggregate ([<br> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;{$match: {adı: "Herkesi"}}, <br> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;{$sort: {yaş: -1} }<br>&nbsp;&nbsp;&nbsp;&nbsp;])*) |

## <a name="develop-with-the-table-api"></a>Tablo API ile geliştirme

### <a name="how-can-i-use-the-table-api-offering"></a>Tablo API teklifinin nasıl kullanabilir miyim? 
Azure Cosmos DB tablo API kullanılabilir [Azure portal][azure-portal]. Önce Azure aboneliği için kaydolmanız gerekir. Oturum açtığınız sonra Azure Cosmos DB tablo API'si hesabınız Azure aboneliğinize ekleyin ve ardından tabloları hesabınıza ekleyin. 

Desteklenen diller ve ilişkili hızlı-başlamasını bulabilirsiniz [Azure Cosmos DB tablo API giriş](table-introduction.md).

### <a name="do-i-need-a-new-sdk-to-use-the-table-api"></a>Tablo API kullanmak için yeni bir SDK gerekiyor mu? 
Hayır, var olan depolama SDK'ları çalışması gerekir. Bir her zaman en son SDK'ları için en iyi desteği ve çoğu durumda üstün performans alır, ancak önerilir. Kullanılabilir dilleri listesini görmek [Azure Cosmos DB tablo API giriş](table-introduction.md).

### <a name="where-is-table-api-not-identical-with-azure-table-storage-behavior"></a>Burada tablo API Azure Table depolama davranışı ile aynı değil misiniz?
Azure Cosmos DB tablo API ile tabloları oluşturmak istediğiniz Azure Table depolama alanından gelen kullanıcıların bilincinde olmanız gereken bazı davranışı farklar vardır:

* Azure Cosmos DB tablo API garantili bir performans sağlamak için ayrılmış kapasite modeli kullanır, ancak bu tablo oluşturulduktan hemen kapasite kullanılmadığından olsa bile bir kapasite için ödeme yaptığını anlamına gelir. Azure Table storage'ı bir yalnızca gerçekten kullanılan kapasite ödeyen. Azure Table depolama 10 ikinci SLA sunarken SLA 99 yazma, bu 10 ms okuma tablo API neden sunabileceğiniz açıklamak için yardımcı olur ve 15 ms. Ancak sonuç olarak tüm istekler, kapasite SLA bunları için tüm istekleri işlemek kullanılabilir olduğundan emin olmak için maliyet para olmadan bile boş tablolar tablo API tablolarla Azure Cosmos DB tarafından sunulan.
* Azure Table storage ' oldukları gibi sorgu sonuçları tablosu API tarafından döndürülen bölüm anahtarı/satırın anahtar düzende sıralanır değil.
* Satır anahtarları yalnızca en fazla 255 bayt olabilir
* Toplu işlemleri yalnızca en çok 2 MB içerebilir
* CreateIfNotExists çağrıları sabit ve RUs tarafından ele alınan diğer tablo işlemleri ayrı bir yönetim kısıtlama tarafından kısıtlanan. Bu olanlar CreateIfNotExists çok sayıda yapmadan kısıtlanan ve sınır kendi RUs gelen değil çünkü hakkında herhangi bir şey yapmak yükleyemezsiniz anlamına gelir.
* CORS şu anda desteklenmiyor
* Azure Table depolama tablo adları büyük küçük harfe duyarlı değildir, ancak Azure Cosmos DB tablo API oldukları
* İkili alanları gibi kodlama bilgilerini Azure Cosmos DB'ın iç biçimlerinden bazıları şu anda bir beğenebileceğiniz olarak etkin değildir. Bu nedenle bu beklenmeyen sınırlamaları veri boyutuna neden olabilir. Örneğin, şu anda bir tablo varlığının tam 1 MB verinin boyutunu artırır kodlama için ikili verileri depolamak için kullanın uygulanamadı.

REST API bakımından Azure Cosmos DB tablo API'si tarafından desteklenmeyen uç noktalar/sorgu seçeneklerini sayısı vardır:
| REST yöntemleri | REST uç noktası/sorgu seçeneği | Belge URL'leri | Açıklama |
| ------------| ------------- | ---------- | ----------- |
| GET, PUT | /? restype =service@comp= özellikleri| [Tablo hizmeti özelliklerini ayarlama](https://docs.microsoft.com/rest/api/storageservices/set-table-service-properties) ve [tablo hizmeti özelliklerini alma](https://docs.microsoft.com/rest/api/storageservices/get-table-service-properties) | Bu uç noktaya CORS kuralları, depolama Analizi Yapılandırması ve günlüğe kaydetme ayarlarını belirlemek için kullanılır. CORS şu anda desteklenmiyor ve analizi ve günlüğe kaydetme Azure Cosmos veritabanı Azure depolama tabloları daha farklı bir şekilde ele |
| SEÇENEKLER | / < Tablo-resource-adı > | [Ön uçuş CORS tablo isteği](https://docs.microsoft.com/rest/api/storageservices/preflight-table-request) | Bu, Azure Cosmos DB şu anda desteklemediği CORS parçasıdır. |
| AL | /? restype =service@compİstatistiği = | [Tablo hizmeti istatistiklerini alın](https://docs.microsoft.com/rest/api/storageservices/get-table-service-stats) | Birincil ve ikincil kopya arasında veri çoğaltmak ne kadar hızlı bilgi sağlar. Çoğaltma yazma parçası olarak bu Cosmos DB'de gerekli değildir. |
| GET, PUT | /myTable? comp acl = | [Tablo ACL alma](https://docs.microsoft.com/rest/api/storageservices/get-table-acl) ve [tablo ACL ayarlayın](https://docs.microsoft.com/rest/api/storageservices/set-table-acl) | Bu alır ve paylaşılan erişim imzaları (SAS) yönetmek için kullanılan depolanmış erişim ilkeleri ayarlar. SAS desteklenmesine karşın, bunların ayarlayın ve farklı şekilde yönetilir. |

Ayrıca Azure Cosmos DB tablo API, yalnızca değil ATOM JSON biçimini destekler.

Azure Cosmos DB desteklerken paylaşılan erişim imzaları (SAS) var. bunu desteklemiyor, belirli özellikle bu yeni tablolar oluşturma hakkına gibi yönetim işlemlerini ilgili ilkelerdir.

İçin .NET SDK'sı özellikle var. bazı sınıfları ve Azure Cosmos DB şu anda desteklemediği yöntemleri

| Sınıf | Desteklenmeyen yöntemi |
|-------|-------- |
| CloudTableClient | \*ServiceProperties * |
|                  | \*ServiceStats * |
| CloudTable | İzinleri Ayarla * |
|            | GetPermissions * |
| TableServiceContext | * (Bu sınıf gerçekten kullanım dışıdır) |
| TableServiceEntity | " " |
| TableServiceExtensions | " " |
| TableServiceQuery | " " |

Bu farklılıklar, projeniz için bir sorun varsa temasa [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com) ve bize bildirin.

### <a name="how-do-i-provide-feedback-about-the-sdk-or-bugs"></a>SDK veya hatalar hakkında geri bildirim nasıl sağlıyor mu?
Geri bildiriminiz aşağıdaki yollardan biriyle paylaşabilirsiniz:

* [Uservoice](https://feedback.azure.com/forums/599062-azure-cosmos-db-table-api)
* [MSDN forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB)
* [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-cosmosdb)

### <a name="what-is-the-connection-string-that-i-need-to-use-to-connect-to-the-table-api"></a>Tablo API'sine bağlanmak için kullanılacak ihtiyacım bağlantı dizesi nedir?
Bağlantı dizesidir:
```
DefaultEndpointsProtocol=https;AccountName=<AccountNamefromCosmos DB;AccountKey=<FromKeysPaneofCosmosDB>;TableEndpoint=https://<AccountNameFromDocumentDB>.table.cosmosdb.azure.com
```
Bağlantı dizesi Azure Portalı'nda bağlantı dizesi sayfasından alabilirsiniz. 

### <a name="how-do-i-override-the-config-settings-for-the-request-options-in-the-net-sdk-for-the-table-api"></a>.NET SDK'sındaki isteği seçeneklerini tablo API için yapılandırma ayarlarını nasıl geçersiz kılma?
Yapılandırma ayarları hakkında daha fazla bilgi için bkz: [Azure Cosmos DB yetenekleri](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). Bazı ayarlar, istemci uygulaması appSettings bölümünde app.config aracılığıyla CreateCloudTableClient yöntemi ve diğer işlenir.

### <a name="are-there-any-changes-for-customers-who-are-using-the-existing-azure-table-storage-sdks"></a>Var olan Azure Table storage'ı SDK'ları kullanan müşteriler için herhangi bir değişiklik var mı?
yok. Var olan Azure Table storage'ı SDK'ları kullanan mevcut veya yeni müşteriler için bir değişiklik bulunmamaktadır. 

### <a name="how-do-i-view-table-data-that-is-stored-in-azure-cosmos-db-for-use-with-the-table-api"></a>Tablo API ile kullanmak için Azure Cosmos DB içinde depolanan tablo verileri nasıl görüntüleyebilirim? 
Veri göz atmak için Azure portalını kullanabilirsiniz. Tablo API kodunda veya sonraki yanıtında belirtilen araçları da kullanabilirsiniz. 

### <a name="which-tools-work-with-the-table-api"></a>Hangi Araçlar tablo API ile çalışır? 
Kullanabileceğiniz [Azure Storage Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).

Belirtilen biçiminde bir bağlantı dizesini almak için esneklik araçlarıyla daha önce yeni tablo API destekleyebilir. Tablo araçları listesi üzerinde sağlanan [Azure Storage istemci araçları](../storage/common/storage-explorers.md) sayfası. 

### <a name="do-powershell-or-azure-cli-work-with-the-table-api"></a>PowerShell veya Azure CLI tablo API ile çalışır?
İçin destek [PowerShell](table-powershell.md). Azure CLI desteği şu anda kullanılamıyor.

### <a name="is-the-concurrency-on-operations-controlled"></a>Eşzamanlılık denetlenen işlemlerde mi?
Evet, iyimser eşzamanlılık ETag mekanizması kullanımı aracılığıyla sağlanır. 

### <a name="is-the-odata-query-model-supported-for-entities"></a>OData sorgu modelini varlıklar için destekleniyor mu? 
Evet, tablo API OData sorgu ve LINQ sorgusu destekler. 

### <a name="can-i-connect-to-azure-table-storage-and-azure-cosmos-db-table-api-side-by-side-in-the-same-application"></a>Azure Table Storage ve Azure Cosmos DB tablo API aynı uygulamada yan yana bağlanabilir miyim? 
Evet, her bağlantı dizesi aracılığıyla kendi URI işaret CloudTableClient, iki ayrı ayrı örnekleri oluşturarak bağlanabilir.

### <a name="how-do-i-migrate-an-existing-azure-table-storage-application-to-this-offering"></a>Bu sunumun var olan bir Azure Table depolama uygulamayı nasıl geçişini?
[AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy) ve [Azure Cosmos DB veri geçiş aracı](import-data.md) desteklenen olan iki.

### <a name="how-is-expansion-of-the-storage-size-done-for-this-service-if-for-example-i-start-with-n-gb-of-data-and-my-data-will-grow-to-1-tb-over-time"></a>Örneğin, t ile başlatırsanız, bu hizmet için yapılan depolama boyutunu genişletme nasıl yapıldığını  *n*  GB veri ve verilerimi zamanla 1 TB'ye kadar büyüme? 
Azure Cosmos DB yatay ölçekleme kullanımı aracılığıyla sınırsız depolama sağlamak için tasarlanmıştır. Hizmet izlemek ve etkili bir şekilde depolama alanınızın artırın. 

### <a name="how-do-i-monitor-the-table-api-offering"></a>Tablo API teklifinin nasıl izlerim?
Tablo API kullanabilirsiniz **ölçümleri** istekleri ve depolama alanı kullanımı izlemek için bölmesi. 

### <a name="how-do-i-calculate-the-throughput-i-require"></a>I gerektiren verimlilik nasıl hesaplar?
İşlemler için gerekli olan TableThroughput hesaplamak için kapasite tahmin kullanabilirsiniz. Daha fazla bilgi için bkz: [tahmin istek birimleri ve veri depolama](https://www.documentdb.com/capacityplanner). Genel olarak, JSON olarak, varlığı temsil eder ve işlemleri için sayıları sağlayın. 

### <a name="can-i-use-the-table-api-sdk-locally-with-the-emulator"></a>Tablo API SDK'yı yerel olarak öykünücü ile kullanabilir miyim?
Şu anda değil.

### <a name="can-my-existing-application-work-with-the-table-api"></a>Varolan Uygulamam tablo API ile çalışabilir mi? 
Evet, aynı API desteklenir.

### <a name="do-i-need-to-migrate-my-existing-azure-table-storage-applications-to-the-sdk-if-i-do-not-want-to-use-the-table-api-features"></a>Tablo API özellikleri kullanmak istemediğiniz SDK var olan Azure Table depolama uygulamalarım geçirmeniz gerekiyor mu?
Hayır, oluşturabilir ve varolan Azure Table depolama varlıkları herhangi bir türde kesintisiz kullanın. Ancak, tablo API kullanmıyorsanız, otomatik dizin, ek tutarlılık seçeneği veya genel dağıtım kazancı olmaz. 

### <a name="how-do-i-add-replication-of-the-data-in-the-table-api-across-multiple-regions-of-azure"></a>Nasıl veri çoğaltma işlemi tablo API çağrısında birden çok Azure bölgeler arasında ekleyebilirim?
Cosmos DB Azure portal'ın kullanabilirsiniz [genel çoğaltma ayarları](tutorial-global-distribution-documentdb.md#portal) uygulamanız için uygun bölgeler eklemek için. Genel olarak dağıtılmış bir uygulama geliştirmek için PreferredLocation bilgi okuma düşük gecikme süresi sağlamak için yerel bölge kümesi ile uygulamanızı eklemelisiniz. 

### <a name="how-do-i-change-the-primary-write-region-for-the-account-in-the-table-api"></a>Tablo API hesap için birincil yazma bölge nasıl değişiyor?
Bir bölge ekleyin ve ardından gerekli bölgeye yük devri için Azure Cosmos DB genel çoğaltma portal bölmesinde kullanabilirsiniz. Yönergeler için bkz: [bölgeli Azure Cosmos DB hesaplarıyla geliştirme](regional-failover.md). 

### <a name="how-do-i-configure-my-preferred-read-regions-for-low-latency-when-i-distribute-my-data"></a>Verilerimi dağıttığınızda nasıl düşük gecikme süresi için tercih edilen my okuma bölgeler yapılandırırım? 
Yerel konumdan okuma yardımcı olmak için app.config dosyasında PreferredLocation tuşunu kullanın. LocationMode ayarlanmışsa, var olan uygulamalar için tablo API bir hata oluşturur. Bu bilgileri app.config dosyasından tablo API alır çünkü bu kodu kaldırın. Daha fazla bilgi için bkz: [Azure Cosmos DB yetenekleri](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="how-should-i-think-about-consistency-levels-in-the-table-api"></a>Tablo API'sindeki tutarlılık düzeyleri hakkında ne düşünüyorsunuz? 
Azure Cosmos DB iyi reasoned dengelemeler tutarlılık, kullanılabilirlik ve gecikme süresi arasında sağlar. Tablo düzeyinde sağ tutarlılık modelini seçin ve veri sorgularken tek tek istekte azure Cosmos DB tablo API geliştiricilerine beş tutarlılık düzeyi sunar. Bir istemci bağlandığında, tutarlılık düzeyi belirtebilirsiniz. CreateCloudTableClient consistencyLevel bağımsız değişkeni aracılığıyla düzeyini değiştirebilirsiniz. 

Tablo API ile "kendi yazma varsayılan olarak Bounded eskime durumu tutarlılığı ile okuma" Düşük gecikmeli okur sağlar. Daha fazla bilgi için bkz: [tutarlılık düzeylerini](consistency-levels.md). 

Varsayılan olarak, Azure Table storage bir bölgedeki güçlü tutarlılık ve ikincil konumlarda Eventual tutarlılık sağlar. 

### <a name="does-azure-cosmos-db-table-api-offer-more-consistency-levels-than-azure-table-storage"></a>Azure Cosmos DB tablo API Azure Table storage'den daha fazla tutarlılık düzeyleri sunar?
Evet, için dağıtılan yapı Azure Cosmos DB, yararlı hakkında daha fazla bilgi için bkz: [tutarlılık düzeylerini](consistency-levels.md). Tutarlılık düzeylerini garanti sağlamadığı güvenle kullanabilirsiniz. Daha fazla bilgi için bkz: [Azure Cosmos DB yetenekleri](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="when-global-distribution-is-enabled-how-long-does-it-take-to-replicate-the-data"></a>Genel dağıtım etkinleştirildiğinde, ne kadar veri çoğaltmak için sürer?
Azure Cosmos DB yerel bölge verilerde işlemi tamamlar ve diğer bölgelerinde hemen sağlasa da, milisaniye veri iter. Bu çoğaltma yalnızca gidiş dönüş süresi veri merkezinin üzerinde (RTT) bağlıdır. Genel dağıtım Özelliği Azure Cosmos DB'nin hakkında daha fazla bilgi için bkz: [Azure Cosmos DB: Azure ile ilgili genel dağıtılmış veritabanı hizmeti](distribute-data-globally.md).

### <a name="can-the-read-request-consistency-level-be-changed"></a>Okuma isteği tutarlılık düzeyi değiştirilebilir mi?
Azure Cosmos DB ile (tablo) kapsayıcı düzeyinde tutarlılık düzeyi ayarlayabilirsiniz. .NET SDK kullanarak, app.config dosyasında TableConsistencyLevel anahtarı için değer sağlayarak düzeyini değiştirebilirsiniz. Olası değerler şunlardır: güçlü, sınırlanmış eskime durumu, oturum, tutarlı öneki ve Eventual. Daha fazla bilgi için bkz: [veri ince ayarlanabilir tutarlılık düzeyleri Azure Cosmos veritabanı](consistency-levels.md). Anahtar, istek tutarlılık birden çok tablo ayarını adresindeki düzeyinde ayarlayamazsınız emin olur. Örneğin, tablo için tutarlılık düzeyi Eventual ve güçlü isteği tutarlılık düzeyinde ayarlayamazsınız. 

### <a name="how-does-the-table-api-handle-failover-if-a-region-goes-down"></a>Nasıl bir bölge kullanılamaz hale gelirse tablo API yük devretme ele alıyor? 
Tablo API Azure Cosmos DB'nin Genel dağıtılmış platformundan yararlanır. Uygulamanızı datacenter kapalı kalma süresi etkilenmemesini sağlamak için Azure Cosmos DB Portalı'ndaki hesap için en az bir daha fazla bölge etkinleştirin [bölgeli Azure Cosmos DB hesaplarıyla geliştirme](regional-failover.md). Portalı kullanarak bölge önceliğini ayarlayabilirsiniz [bölgeli Azure Cosmos DB hesaplarıyla geliştirme](regional-failover.md). 

Hesap için istediğiniz ve burada bu için bir yük devretme öncelik sağlayarak yük devredebildiğini kontrol sayıda bölgeleri ekleyebilirsiniz. Elbette, veritabanını kullanmak için bir uygulama var. çok vermeniz gerekir. Bunu yaptığınızda, müşterilerinizin kapalı kalma süresi karşılaşmazsınız. [En son .NET İstemci SDK](table-sdk-dotnet.md) otomatik homing olan ancak diğer SDK'ları değildir. Diğer bir deyişle, kapalı ve yeni bölge için otomatik olarak yük bölgeyi algılayabilir.

### <a name="is-the-table-api-enabled-for-backups"></a>Tablo API yedeklemeler için etkin mi?
Evet, tablo API Azure Cosmos DB platform yedeklemeler için yararlanır. Yedeklemeleri otomatik olarak yapılır. Daha fazla bilgi için bkz: [çevrimiçi yedekleme ve geri yükleme Azure Cosmos DB ile](online-backup-and-restore.md).

 
### <a name="does-the-table-api-index-all-attributes-of-an-entity-by-default"></a>Tablo API bir varlığın tüm öznitelikleri varsayılan dizin mu?
Evet, bir varlığın tüm öznitelikleri varsayılan olarak dizine alınır. Daha fazla bilgi için bkz: [Azure Cosmos DB: ilkeleri dizin](indexing-policies.md). 

### <a name="does-this-mean-i-do-not-have-to-create-multiple-indexes-to-satisfy-the-queries"></a>Değil sahibim sorguları karşılamak için birden çok dizin oluşturmak için bu ortalama mu? 
Evet, Azure Cosmos DB tablo API herhangi bir şema tanımı olmadan tüm özniteliklerin otomatik dizin oluşturma sağlar. Bu Otomasyon odak uygulama yerine dizin oluşturma ve yönetme için geliştiricilere boşaltır. Daha fazla bilgi için bkz: [Azure Cosmos DB: ilkeleri dizin](indexing-policies.md).

### <a name="can-i-change-the-indexing-policy"></a>Dizin oluşturma ilkesini değiştirebilir miyim?
Evet, dizin tanımı sağlayarak dizin oluşturma ilkesini değiştirebilirsiniz. Daha fazla bilgi için bkz: [Azure Cosmos DB yetenekleri](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). Düzgün kodlamak ve ayarları kaçış gerekir. 

olmayan - .NET SDK'ları için dizin oluşturma ilkesini yalnızca portalında ayarlanabilir **Veri Gezgini**, değiştirmek ve gidin istediğiniz belirli tablosuna gitmek **ölçek & ayarları**dizin oluşturma ilkesi -> İstenen değişikliği yapın ve ardından **kaydetmek**.

.NET SDK app.config dosyasında gönderilebilir:
```
{
  "indexingMode": "consistent",
  "automatic": true,
  "includedPaths": [
    {
      "path": "/somepath",
      "indexes": [
        {
          "kind": "Range",
          "dataType": "Number",
          "precision": -1
        },
        {
          "kind": "Range",
          "dataType": "String",
          "precision": -1
        } 
      ]
    }
  ],
  "excludedPaths": 
[
 {
      "path": "/anotherpath"
 }
]
}
```

### <a name="azure-cosmos-db-as-a-platform-seems-to-have-lot-of-capabilities-such-as-sorting-aggregates-hierarchy-and-other-functionality-will-you-be-adding-these-capabilities-to-the-table-api"></a>Bir platform olarak Azure Cosmos DB sıralama, toplamlar, hiyerarşi ve diğer işlevleri gibi özellikleri, pek çok görünüyor. Tablo API için bu özellikler ekleme? 
Tablo API Azure Table storage aynı sorgu işlevleri sağlar. Azure Cosmos DB, sıralama, toplamalar, Jeo-uzamsal sorgu, hiyerarşi ve çok çeşitli yerleşik işlevler de destekler. Biz gelecekteki hizmeti güncelleştirmesine tablo API ek işlevsellik sağlar. Daha fazla bilgi için bkz: [SQL sorgularını Azure Cosmos DB DocumentDB API için](../documentdb/documentdb-sql-query.md).
 
### <a name="when-should-i-change-tablethroughput-for-the-table-api"></a>Tablo API için zaman TableThroughput değiştiririm?
Aşağıdaki koşullardan herhangi biri geçerli olduğu durumlarda TableThroughput değiştirmeniz gerekir:
* Ayıklama, dönüştürme ve yükleme (ETL) veri gerçekleştiriyorsunuz veya kısa sürede çok miktarda veri yüklemek istediğiniz. 
* Daha fazla verimlilik arka uçtaki kapsayıcısından gerekir. Örneğin, kullanılan işleme sağlanan işleme büyük ve, kısıtlanan bakın. Daha fazla bilgi için bkz: [Azure Cosmos DB kapsayıcıları için kümesi işleme](set-throughput.md).

### <a name="can-i-scale-up-or-scale-down-the-throughput-of-my-table-api-table"></a>Ölçeği artırma veya miyim my tablo API tablosunun işleme ölçeğini? 
Evet, üretilen iş ölçeklendirmek için Azure Cosmos DB Portalı'nın ölçek bölmesini kullanabilirsiniz. Daha fazla bilgi için bkz: [kümesi işleme](set-throughput.md).

### <a name="is-a-default-tablethroughput-set-for-newly-provisioned-tables"></a>Varsayılan eylem TableThroughput için yeni sağlanan tabloları kümesi mi?
Evet, app.config aracılığıyla TableThroughput geçersiz kılmaz ve önceden oluşturulmuş bir kapsayıcı Azure Cosmos DB'de kullanmayın, hizmet 400 işleme ile bir tablo oluşturur.
 
### <a name="is-there-any-change-of-pricing-for-existing-customers-of-the-azure-table-storage-service"></a>Azure Table depolama hizmeti var olan müşteriler için fiyatlandırma herhangi bir değişiklik var mı?
yok. Var olan Azure Table depolama müşterileri için fiyatı değişiklik yoktur. 

### <a name="how-is-the-price-calculated-for-the-table-api"></a>Fiyat tablosu API için nasıl hesaplanır? 
Fiyat üzerinde ayrılmış TableThroughput bağlıdır. 

### <a name="how-do-i-handle-any-throttling-on-the-tables-in-table-api-offering"></a>Tablo API teklifi tablolarda herhangi azaltma nasıl işleneceğini? 
Sağlanan işleme kapasitesi temeldeki kapsayıcısı için istek oranı aşarsa, bir hata alıyorsunuz ve yeniden deneme ilkesi uygulayarak SDK çağrı yeniden dener.

### <a name="why-do-i-need-to-choose-a-throughput-apart-from-partitionkey-and-rowkey-to-take-advantage-of-the-table-api-offering-of-azure-cosmos-db"></a>Üretilen iş PartitionKey ve RowKey Azure Cosmos DB tablo API sunulması yararlanmak için dışında seçmek neden gerekiyor mu?
App.config dosyasında ya da Portalı aracılığıyla belirtmezseniz, azure Cosmos DB, kapsayıcı için bir varsayılan işleme ayarlar. 

Azure Cosmos DB, performansı ve gecikme süresi, üst sınır işlemi ile garantileri sağlar. Bu garantisi mümkün olduğunda altyapısı kiracının işlemlerini İdaresi uygulayabilirsiniz. TableThroughput ayarı platform bu kapasite ayırır ve işletimsel başarı güvence altına alır çünkü garantili üretilen iş ve gecikmeyi, edinmenizi sağlar. 

Üretilen iş belirtimi kullanarak, Özellikler esnek maliyet tasarrufu uygulamanızı mevsimselliğin yararlanabilir ve verimlilik ihtiyaçlarını karşılamak için değiştirebilirsiniz.

### <a name="azure-table-storage-has-been-very-inexpensive-for-me-because-i-pay-only-to-store-the-data-and-i-rarely-query-the-azure-cosmos-db-table-api-offering-seems-to-be-charging-me-even-though-i-have-not-performed-a-single-transaction-or-stored-anything-can-you-please-explain"></a>I yalnızca depolamak veri ve nadiren sorgu ödeme olduğundan azure Table storage benim için çok pahalı olmayan olmuştur. Azure Cosmos DB tablo API teklifi ı değil tek bir işlem gerçekleştirilen veya hiçbir şey depolanan olsa bile bana şarj gibi görünüyor. Lütfen açıklayabilir?

Azure Cosmos DB garanti kullanılabilirlik, gecikme ve verimlilik için Genel dağıtılmış, SLA tabanlı sistemiyle olacak şekilde tasarlanmıştır. Azure Cosmos veritabanı işleme ayırdığınızda, bu, diğer sistemler üretimini farklı olarak sağlanır. Azure Cosmos DB müşteriler, ikincil dizinler ve genel dağıtım gibi istediniz ek özellikler sağlar.  

### <a name="i-never-get-a-quota-full-notification-indicating-that-a-partition-is-full-when-i-ingest-data-into-azure-table-storage-with-the-table-api-i-do-get-this-message-is-this-offering-limiting-me-and-forcing-me-to-change-my-existing-application"></a>Hiçbir zaman (bir bölüm dolu belirten) bir "Kota tam" bildirim alıyorum zaman ı alma verileri Azure Table depolama alanına. Bu ileti tablo API ile alıyorum. Bana sınırlandırma ve bana varolan Uygulamam değiştirmek için zorlama tanıyor mu?

Azure Cosmos DB sınırsız ölçek gecikme süresi, performans, kullanılabilirlik ve tutarlılığı garanti sağlayan bir SLA tabanlı bir sistemdir. Garantili premium performans sağlamak için veri boyutu ve dizin yönetilebilir ve ölçeklenebilir olduğundan emin olun. Varlıklar veya bölüm anahtarı başına öğe sayısı 10 GB sınırını harika arama ve sorgu performansı sağladığımız sağlamaktır. Uygulamanızı Azure Storage için bile de ölçeklendirir emin olmak için öneririz, *değil* bölümde tüm bilgileri depolamak ve onu sorgulama hot bir bölüm oluşturun. 

### <a name="so-partitionkey-and-rowkey-are-still-required-with-the-table-api"></a>Bu nedenle PartitionKey ve RowKey tablo API ile hala gereklidir? 
Evet. Tablo API'ın yüzey alanını Azure Table depolama SDK'sı benzer olduğundan bölüm anahtarı verileri dağıtmak için etkili bir yol sağlar. Satır anahtarını Bu bölüm içinde benzersizdir. Satır anahtarını bulunması gerekir ve olduğu gibi standart SDK null olamaz. RowKey uzunluğu 255 bayttır ve PartitionKey uzunluğu 1 KB'tır. 

### <a name="what-are-the-error-messages-for-the-table-api"></a>Tablo API için hata iletileri nelerdir?
Hataların çoğu aynı olacak şekilde azure Table storage ve Azure Cosmos DB tablo API aynı SDK'ları kullanın.

### <a name="why-do-i-get-throttled-when-i-try-to-create-lot-of-tables-one-after-another-in-the-table-api"></a>Tabloları miktarda birbiri ardından tablo API oluşturmak çalıştığınızda neden ı kısıtlanan?
Azure Cosmos DB gecikme süresi, performans, kullanılabilirlik ve tutarlılık sağlayan bir SLA tabanlı bir sistemdir. Sağlanan sistem olduğundan, bu gereksinimleri güvence altına almak için kaynakları ayırır. Tabloları oluşturulmasını hızlı oranını algıladı ve daraltma. Tablo oluşturma hızında arayın ve az 5 dakika başına alt öneririz. Tablo API sağlanan sistem olduğunu unutmayın. Şu anda bunu sağlamak için ödeme başlar. 

## <a name="develop-against-the-graph-api-preview"></a>Grafik API'si (Önizleme) karşı geliştirin
### <a name="how-can-i-apply-the-functionality-of-graph-api-preview-to-azure-cosmos-db"></a>Nasıl ı grafik API'si (Önizleme) işlevselliğini Azure Cosmos DB uygulayabilir mi?
Grafik API'si (Önizleme) işlevselliğini uygulamak için bir uzantı kitaplığını kullanabilirsiniz. Bu kitaplık Microsoft Azure grafikleri denir ve NuGet üzerinde kullanılabilir. 

### <a name="it-looks-like-you-support-the-gremlin-graph-traversal-language-do-you-plan-to-add-more-forms-of-query"></a>Gremlin grafik geçişi dil desteği gibi görünüyor. Daha fazla form sorgusunun eklemek planlıyor musunuz?
Evet, diğer mekanizmaları sorgu için gelecekte ekleme planlıyoruz. 

### <a name="how-can-i-use-the-new-graph-api-preview-offering"></a>Grafik API'si (Önizleme) yenilik nasıl kullanabilir miyim? 
Başlamak için tamamlamak [grafik API'si](../cosmos-db/create-graph-dotnet.md) hızlı başlangıç makalesi.

<a id="cassandra"></a> 
## <a name="develop-with-the-apache-cassandra-api-preview"></a>Apache Cassandra API (Önizleme) ile geliştirme

### <a name="what-is-the-protocol-version-supported-in-the-private-preview-is-there-a-plan-to-support-other-protocols"></a>Özel önizleme olarak desteklenen protokol sürümü nedir? Diğer protokolleri desteklemek için bir plan var mı?
Apache Cassandra API Azure Cosmos DB için bugün CQL sürüm 4 destekler. Üzerinden diğer protokolleri destekleme hakkında görüş bildirmek isterseniz,'ın bize bildirin [uservoice geri bildirimleri](https://feedback.azure.com/forums/263030-azure-cosmos-db) veya e-posta Gönder [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com). 

### <a name="why-is-choosing-a-throughput-for-a-table-a-requirement"></a>Bir tablo için bir işleme gereksinimi seçme neden?
Azure Cosmos DB - tablodan nerede oluşturmak göre kapsayıcı için varsayılan işleme ayarlar portalı veya CQL. Azure Cosmos DB, performansı ve gecikme süresi, üst sınır işlemi ile garantileri sağlar. Bu garantisi mümkün olduğunda altyapısı kiracının işlemlerini İdaresi uygulayabilirsiniz. Ayar verimlilik platform bu kapasite ayırır ve işlem başarılı güvence altına alır çünkü garantili üretilen iş ve gecikmeyi, edinmenizi sağlar. Uygulamanızı mevsimselliğin yararlanabilir ve maliyet tasarrufu için işleme özellikler esnek değiştirebilirsiniz.

Üretilen iş kavramı açıklandığı [istek birimlerine Azure Cosmos veritabanı](request-units.md) makalesi. Bir tablo için işleme temel alınan fiziksel bölümleri arasında eşit olarak dağıtılır.  

### <a name="what-is-the-default-rus-of-table-when-created-through-cql-what-if-i-need-to-change-it"></a>Varsayılan RU/s CQL oluşturduğunuzda tablosunun nedir? Peki bunu değiştirmeniz gerekecek?
Azure Cosmos DB (RU/s) saniye başına istek birimleri, bir para birimi olarak verimlilik sağlamak için kullanır. CQL oluşturulan tabloları sahip 400 RU. Portaldan RU değiştirebilirsiniz. 

CQL
```
CREATE TABLE keyspaceName.tablename (user_id int PRIMARY KEY, lastname text) WITH cosmosdb_provisioned_throughput=1200
```

.NET
```csharp
int provisionedThroughput = 400;
var simpleStatement = new SimpleStatement($"CREATE TABLE {keyspaceName}.{tableName} (user_id int PRIMARY KEY, lastname text)");
var outgoingPayload = new Dictionary<string, byte[]>();
outgoingPayload["cosmosdb_provisioned_throughput"] = Encoding.UTF8.GetBytes(provisionedThroughput.ToString());
simpleStatement.SetOutgoingPayload(outgoingPayload); 
``` 

### <a name="what-happens-when-throughput-is-exceeded"></a>Üretilen iş aşıldığında ne olur? 
Azure Cosmos DB, performansı ve gecikme süresi, üst sınır işlemi ile garantileri sağlar. Bu garantisi mümkün olduğunda altyapısı kiracının işlemlerini İdaresi uygulayabilirsiniz. Bu platform bu kapasite ayırır ve işlem başarılı güvence altına alır çünkü garantili üretilen iş ve gecikmeyi, edinmenizi sağlar üretilen işi ayarlama konusunda olası dayalıdır. Bu kapasite aştıklarında, aşırı yüklenmiş kapasitenizi aşıldı olduğunu belirten hata iletisi alırsınız. 0x1001 aşırı: "İstek hızı büyük olduğu için" istek işlenemiyor. Juncture en bu hangi işlemleri görmek için gereklidir ve toplu bu soruna neden olur. Sağlanan kapasite portalındaki ölçümlerle aşan tüketilen kapasitesi hakkında bir fikir edinebilirsiniz. Kapasite neredeyse tüketilen sağlamak zorunda sonra tüm bölümleri arasında eşit olarak. Üretilen iş çoğunu bir bölüm tarafından tüketilen görürseniz, iş yükünün eğme sahip. 

Ölçümleri kullanılabilir gösterin, nasıl üretilen iş başına yedi gün ve saat, gün, üzerinden bölümleri arasında veya toplama kullanılıyor. Daha fazla bilgi için bkz: [izleme ve ölçümleri Azure Cosmos veritabanı ile hata ayıklama](use-metrics.md).

Tanılama günlüklerini içinde açıklanan [Azure Cosmos DB tanılama günlük](logging.md) makalesi.

### <a name="does-the-primary-key-map-to-the-partition-key-concept-of-azure-cosmos-db"></a>Bölüm anahtarı kavramı Azure Cosmos DB, birincil anahtar eşlemeye mu?
Evet, bölüm anahtarı varlık doğru konuma yerleştirmek için kullanılır. Azure Cosmos DB'de fiziksel bir bölüme depolanan sağ mantıksal bölüm bulmak için kullanılır. Bölümleme kavramı iyi içinde açıklanan [bölüm ve Azure Cosmos veritabanı ölçek](partition-data.md) makalesi. Temel Al hemen İşte bir mantıksal bölüm 10 GB sınırını bugün aşmaması gerektiğini. 

### <a name="what-happens-when-i-get-a-quota-full-notification-indicating-that-a-partition-is-full"></a>Bir bölüm gösteren bir "Kota tam" bildirimi alıyorum ne olur tam mi?
Azure Cosmos DB sınırsız ölçek gecikme süresi, performans, kullanılabilirlik ve tutarlılığı garanti sağlayan bir SLA tabanlı bir sistemdir. Buna ait Cassandra API çok sınırsız depolama alanı sağlar. Bu sınırsız depolama anahtar kavram bölümleme kullanarak verilerin yatay genişletme temel alır. Bölümleme kavramı iyi içinde açıklanan [bölüm ve Azure Cosmos veritabanı ölçek](partition-data.md) makalesi.

Varlıklar veya mantıksal bölüm başına öğe sayısı 10 GB sınırını için uyması. Uygulamanızı iyi ölçeklenir emin olmak için öneririz, *değil* bölümde tüm bilgileri depolamak ve onu sorgulama hot bir bölüm oluşturun. Verileri eğri -, olan, gelen yalnızca miktarda veri için bir bölüm anahtarı - yani, bu hata için birden fazla 10 GB. Veri depolama Portalı'nı kullanarak dağıtım bulabilirsiniz. Bu hatayı düzeltmek için yol için recrete tablosu ve veri daha iyi dağıtılması sağlayan ayrıntılı birincil (bölüm anahtarı) seçin.

### <a name="is-it-possible-to-use-cassandra-api-as-key-value-store-with-millions-or-billions-of-individual-partition-keys"></a>Milyonlarca veya tek tek bölüm anahtarlarını milyarlarca Cassandra API anahtar değeri deposu olarak kullanmak mümkün mü?
Azure Cosmos DB depolama ölçeklendirme tarafından sınırsız veri depolayabilirsiniz. Bu, üretimini bağımsızdır. Evet, Cassandra API depolamak ve doğru birincil bölüm anahtarı belirterek anahtar/değer almak için her zaman sadece kullanabilirsiniz. Bu tek tek anahtarları kendi mantıksal bölüm alın ve bir sorun olmadan fiziksel bölüm üzerinde sit. 

### <a name="is-it-possible-to-create-multiple-tables-with-apache-cassandra-api-of-azure-cosmos-db"></a>Apache Cassandra API, Azure Cosmos DB ile birden çok tablo oluşturmak mümkün mü?
Evet, crete için olası Apache Cassandra API ile birden çok tablo. Bu tablo her işleme ve depolama birimi olarak kabul edilir. 

### <a name="is-it-possible-to-create-multiple-tables-in-succession"></a>Art arda birden çok tablo oluşturmak mümkün mü?
Azure Cosmos DB, veri ve denetim düzlemi etkinlikler için yönetilen kaynak sistemidir. Koleksiyonları gibi tablolar işleme kapasitesi için sağlanan çalışma zamanı varlıkları kapsayıcılardır. Bu hızlı art arda kapsayıcılarında oluşturulmasını beklenen etkinliği değil ve kısıtlanan. Açılan/tabloları hemen - oluşturma testleri varsa boşluk Lütfen deneyin.

### <a name="what-is-maximum-number-of-tables-which-can-be-created"></a>En fazla oluşturulabilecek tablo sayısı nedir?
Tablo sayısı üzerinde fiziksel bir sınır yoktur, Lütfen bir e-postası gönder [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com) çok fazla sayıda normal 10s veya 100s oluşturulması gerekir (burada sürekli toplam boyutu 10 TB'lık veriyi aşıyor) tablosu varsa. 

### <a name="what-is-the-maximum--of-keyspace-which-we-can-create"></a>Yenilikler oluşturabiliriz keyspace maksimum sayısı 
Fiziksel bir sınır yoktur keyspaces sayısının meta verileri kapsayıcıları oldukları gibi Lütfen bir e-postası gönder [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com) herhangi bir nedenden dolayı çok fazla sayıda keyspaces varsa. 

### <a name="is-it-possible-to-bring-in-lot-of-data-after-starting-from-normal-table"></a>Normal tablosundan başlattıktan sonra çok sayıda veri getirmek mümkün mü? 
Depolama kapasitesi, otomatik olarak yönetilir ve daha fazla veri itme artar. Bu nedenle, yönetme ve sağlama düğümleri, vb. gerektiği kadar veri güvenle aktarabilirsiniz. 

### <a name="is-it-possible-to-supply-yaml-file-settings-to-configure-apache-casssandra-api-of-azure-cosmos-db-behavior"></a>Azure Cosmos DB, Apache Casssandra API davranışı yapılandırmak için yaml dosyası ayarları sağlamak mümkün mü?
Azure Cosmos DB, Apache Cassandra API bir platform hizmetidir. Protokol düzeyinde uyumluluk işlemleri yürütmek için sağlar. Koyma, yönetim, izleme ve yapılandırma karmaşıklığını gizler. Bir geliştirici/kullanıcı olarak kullanılabilirlik, silinmiş öğe, anahtar önbelleği, satır önbelleği, çiçek tomurcukları filtre ve diğer ayarları çok sayıda hakkında endişelenmeniz gerekmez. Azure Cosmos veritabanı Apache Cassandra API odaklar sağlama üzerinde okuma ve yazma performansı yapılandırma ve yönetim ek yükü gerektirir.

### <a name="will-apache-cassandra-api-for-azure-cosmos-db-support-node-additioncluster-statusnode-status-commands"></a>Azure Cosmos DB için Apache Cassandra API düğüm ekleme/küme durumu/düğüm durumu komutları destekleyecek mi?
Apache Cassandra API kapasite planlama, üretilen iş ve depolama için esneklik taleplerini çok kolay yanıt sağlayan bir platform hizmetidir. Azure Cosmos DB ile ihtiyacınız verimliliği sağlayın. Ardından, bu kez gün arasında herhangi bir sayıda yukarı ve aşağı düğümler ekleme/silme veya bunları yönetme hakkında endişelenmeden ölçeklendirebilirsiniz. Bu düğüm, küme yönetim aracı çok kullanmak gerekmez anlamına gelir. 

### <a name="what-happens-with-respect-to-various-config-settings-for-keyspace-creation-like-simplenetwork"></a>Keyspace oluşturma basit/ağ gibi çeşitli yapılandırma ayarları göre ne olur?
Azure Cosmos DB kutunun kullanılabilirlik ve düşük gecikme nedeniyle dışında genel dağıtım sağlar. Kurulum çoğaltmalar vb. için gerekli değildir. Tüm yazma işlemi her zaman burada performans garanti sağlarken yazma herhangi bir bölgede kaydedilen çekirdek olur.  

### <a name="what-happens-with-respect-to-various-settings-for-table-metadata-like-bloom-filter-caching-read-repair-change-gcgrace-compression-memtableflushperiod-etc"></a>Önbelleğe alma, çeşitli ayarları göre çiçek tomurcukları filtre gibi tablo meta verileri için olanlar onarım değişiklik, gc_grace, sıkıştırma memtable_flush_period vb. okuma?
Azure Cosmos DB okuma/yazma ve herhangi bir yapılandırma ayarlarını ve yanlışlıkla seçilebilmesini dokunmak için gerek kalmadan işleme için performans sağlar.  

### <a name="is-time-to-live-ttl-supported-for-cassandra-tables"></a>Yaşam süresi (TTL) Cassandra tablolar için destekleniyor mu? 
Evet, TTL desteklenir. 

### <a name="is-it-possible-to-monitor-node-status-replica-status-gc-and-os-parameters-earlier-with-various-tools-what-needs-to-be-monitored-now"></a>İzleyici düğümü durumu, çoğaltma durumunu, gc ve işletim sistemi parametrelerini çeşitli araçları ile daha önce mümkün mü? Şimdi izlenmesi neler gerekir?
Azure Cosmos DB üretkenliği artırmak ve altyapı izleme ve yönetme hakkında endişelenmeniz değil sağlamanıza yardımcı olan bir platform hizmetidir. Portal ölçümleri kısıtlanan ve artırır veya bu verimlilik azaltırsanız, bulmak için kullanılabilir olan işleme ilişkin halletmeniz yeterlidir. İzleyici [SLA](monitor-accounts.md).
Kullanım [ölçümleri](use-metrics.md) kullanım [tanılama günlükleri](logging.md).

### <a name="which-client-sdks-can-work-with-apache-cassandra-api-of-azure-cosmos-db"></a>Hangi istemci SDK'ları Apache Cassandra API, Azure Cosmos DB ile çalışabilir mi?
Özel önizleme Apache Cassandra SDK'ın istemcisinde CQLv3 kullanacağınız sürücüleri istemci programları için kullanılmıştır. Kullanın ya da sorunlar, karşılıklı, posta gönderin diğer sürücüleri varsa [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com). 

### <a name="is-composite-primary-key-supported"></a>Birleşik birincil anahtar destekleniyor mu?
Evet, bileşik bölüm anahtarı oluşturmak için normal sözdizimini kullanabilirsiniz. 

### <a name="can-i-use-sstable-loader-for-data-loading"></a>Veri yükleme için sstable yükleyicisi kullanabilir miyim?
Hayır, Önizleme süresince sstable yükleyicisi desteklenmiyor. 

### <a name="can-an-on-premises-cassandra-cluster-be-paired-with-azure-cosmos-dbs-apache-cassandra-api"></a>Bir şirket içi cassandra küme Azure Cosmos veritabanı Apache Cassandra API'si ile eşlenmiş?
Mevcut Azure Cosmos DB işlemlerinin ek yükü olmadan bulut ortamı için en iyi duruma getirilmiş bir deneyim sahiptir. Eşleştirme gerekiyorsa, lütfen posta gönderin [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com) senaryonuz açıklaması ile.

### <a name="does-cassandra-api-provide-full-backups"></a>Cassandra API tam yedeklemeler sağlar? 
Azure Cosmos DB dört saatlik aralıklarla bugün tüm API'leri geçen iki boş tam yedekleme sağlar. Bu, bir yedekleme zamanlaması vb. kurulum gerekmez sağlar. Bekletme ve sıklığı değiştirmek istiyorsanız, bir e-posta Gönder [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com) veya bir destek servis talebi Yükselt. Yedekleme özelliği hakkında bilgi sağlanır [otomatik çevrimiçi yedekleme ve geri yükleme Azure Cosmos DB ile](online-backup-and-restore.md) makalesi. 

### <a name="how-does-the-cassandra-api-account-handle-failover-if-a-region-goes-down"></a>Bir bölge kullanılamaz hale gelirse Cassandra API hesabını nasıl yük devretme ele alıyor? 
Azure Cosmos DB Cassandra API Azure Cosmos DB Genel dağıtılmış platformundan taşır. Uygulamanızı datacenter kapalı kalma süresi etkilenmemesini sağlamak için Azure Cosmos DB Portalı'ndaki hesap için en az bir daha fazla bölge etkinleştirin [bölgeli Azure Cosmos DB hesaplarıyla geliştirme](regional-failover.md). Portalı kullanarak bölge önceliğini ayarlayabilirsiniz [bölgeli Azure Cosmos DB hesaplarıyla geliştirme](regional-failover.md). 

Hesap için istediğiniz ve burada bu için bir yük devretme öncelik sağlayarak yük devredebildiğini kontrol sayıda bölgeleri ekleyebilirsiniz. Veritabanını kullanmak için bir uygulama var. çok sağlamanız gerekir. Bunu yaptığınızda, müşterilerinizin kapalı kalma süresi karşılaşmazsınız. 

### <a name="does-the-apache-cassandra-api-index-all-attributes-of-an-entity-by-default"></a>Apache Cassandra API bir varlığın tüm öznitelikleri varsayılan dizin mu?
Evet, bir varlığın tüm öznitelikleri varsayılan olarak Azure Cosmos DB dizine alınır. Daha fazla bilgi için bkz: [Azure Cosmos DB: ilkeleri dizin](indexing-policies.md). Tutarlı dizin ile garantili performans yararlarını alın ve kaydedilen dayanıklı çekirdek her zaman yazar. 

### <a name="does-this-mean-i-do-not-have-to-create-multiple-indexes-to-satisfy-the-queries"></a>Değil sahibim sorguları karşılamak için birden çok dizin oluşturmak için bu ortalama mu? 
Evet, Azure Cosmos DB herhangi bir şema tanımı olmadan tüm özniteliklerin otomatik dizin oluşturma sağlar. Bu Otomasyon odak uygulama yerine dizin oluşturma ve yönetme için geliştiricilere boşaltır. Daha fazla bilgi için bkz: [Azure Cosmos DB: ilkeleri dizin](indexing-policies.md).

### <a name="can-i-use-the-new-cassandra-api-sdk-locally-with-the-emulator"></a>Yeni Cassandra API SDK'yı yerel olarak öykünücü ile kullanabilir miyim?
Gelecekte bu özelliği destekleyen planlıyoruz. 

### <a name="azure-cosmos-db-as-a-platform-seems-to-have-lot-of-capabilities-such-as-changefeed-and-other-functionality-will-these-capabilities-be-added-to-the-cassandra-api"></a>Bir platform olarak Azure Cosmos DB changefeed ve diğer işlevleri gibi yeteneklerini pek çok görünüyor. Bu özellikler Cassandra API'sine eklenir? 
Apache Cassandra API Apache Cassandra aynı CQL işlevleri sağlar. Çeşitli özellikleri gelecekteki destekleme Uygulanabilirlik aramak planlayın.

### <a name="feature-x-of-regular-cassandra-api-is-not-working-as-today-where-can-the-feedback-be-provided"></a>Özellik x normal Cassandra API bugün olarak geri bildirim burada sağlanabilir çalışmıyor olabilir?
İle geribildirim sağlamak [uservoice geri bildirimleri](https://feedback.azure.com/forums/263030-azure-cosmos-db).

<a id="moving-to-cosmos-db"></a>
## <a name="questions-from-documentdb-customers"></a>DocumentDB müşterilerden sorular
### <a name="why-are-you-moving-to-azure-cosmos-db"></a>Neden Azure Cosmos DB taşıyor? 

Azure Cosmos DB sonraki büyük artık genel olarak dağıtılmış ölçekli bulut veritabanlarında ' dir. Bir DocumentDB müşteri olarak devrim niteliğinde sistem ve yetenekleri Azure Cosmos DB tarafından sunulan erişim sahip.

Azure Cosmos DB 2010 Microsoft içindeki büyük ölçekli uygulamalar oluşturmanın geliştiriciler tarafından karşıya kalınan adres "Proje Floransa" başlatılır. Biz bu teknoloji ilk nesil 2015'te Azure geliştiricilere Azure DocumentDB biçiminde kullanıma şekilde genel dağıtılmış uygulamalar oluşturmanın zorluklarından Microsoft'a benzersiz değil. 

O zamandan bu yana, eklenen yeni özellikler artık ve önemli yeni özellikler sunulmuştur. Azure Cosmos DB sonucudur. Bu sürümde bir parçası olarak verileriyle, DocumentDB müşterilerin Azure Cosmos DB müşteriler otomatik olarak ve sorunsuz bir şekilde haline gelir. Bu özellikler temel veritabanı motoru yanı sıra genel dağıtım, esnek ölçeklenebilirlik ve endüstri lideri, kapsamlı SLA alanlarıdır. Özellikle, biz verimli bir şekilde tüm popüler veri modelleri, türü sistemleri ve API'ları Azure Cosmos DB temel alınan veri modeline eşlemek için Azure Cosmos DB veritabanı altyapısı gelişim göstermiştir. 

Bu çalışmanın geçerli Geliştirici dönük göstergeleri yeni desteğidir [Gremlin](../cosmos-db/graph-introduction.md) ve [depolama API'leri tablo](../cosmos-db/table-introduction.md). Ve yalnızca başlangıç budur. Daha fazla performans ve küresel ölçekli depolama gelişmeleri, zaman içinde diğer popüler API'ler ve yeni veri modelleri eklemek planlıyoruz. 

DocumentDB noktasındaki önemlidir [SQL dialect](../documentdb/documentdb-sql-query.md) her zaman temel Azure Cosmos DB destekleyebileceği birçok API'ları yalnızca biri olmuştur. Azure Cosmos DB gibi tam olarak yönetilen bir hizmet kullanan geliştiriciler için yalnızca hizmet hizmeti tarafından sunulan API'leri arabirimidir. Hiçbir şey gerçekten var olan DocumentDB müşteriler için değiştirir. Azure Cosmos DB içinde tam olarak aynı SQL DocumentDB sunan API alın. Ve şimdi (ve gelecekte), daha önce erişilemez diğer capabilities erişebilirsiniz 

Bizim devam eden iş başka bir göstergeleri genel ve esnek ölçeklenebilirlik işleme ve depolama genişletilmiş temelidir. Biz, genel dağıtım alt sistemi birkaç temel geliştirmeler yapıldı. Birçok geliştirici dönük özelliklerden bir toplam beş iyi tanımlanmış tutarlılık modelleri yapar tutarlı önek tutarlılık modeli biridir. Biz, bunlar yetişkin birçok daha ilginç özellikleri serbest bırakır. 

### <a name="what-do-i-need-to-do-to-ensure-that-my-documentdb-resources-continue-to-run-on-azure-cosmos-db"></a>DocumentDB Kaynaklarım Azure Cosmos DB üzerinde çalışmaya devam etmesini sağlamak için yapmanız gerekenler nelerdir?

Tüm değişiklik gerekmez. DocumentDB kaynaklarınızı Azure Cosmos DB kaynakları sunulmuştur ve bu taşıma oluştuğunda hizmet kesintisine neden olmadan olmuştur.

### <a name="what-changes-do-i-need-to-make-for-my-app-to-work-with-azure-cosmos-db"></a>Uygulamam Azure Cosmos DB ile çalışacak biçimde yükseltilmesi hangi değişikliklerin gerekiyor mu?

Yapmak için bir değişiklik yoktur. Sınıfları, ad alanları ve NuGet paket adlarının değişmemiştir. Her zaman, SDK'ları en son özellikleri ve geliştirmeleri avantajlarından yararlanmak için güncel tutmanızı öneririz. 

### <a name="whats-changed-in-the-azure-portal"></a>Azure portalında Değiştirilenler?

DocumentDB portalında bir Azure hizmeti olarak artık görünür. Onun yerine aşağıdaki görüntüde gösterildiği gibi yeni bir Azure Cosmos DB simge olur. Önce oldukları ve verimlilik, değişiklik tutarlılık düzeyleri ve İzleyici SLA hala ölçeği gibi tüm koleksiyonlar kullanılabilir. Veri Gezgini (Önizleme) yeteneklerini geliştirilmiştir. Şimdi görüntüleyebilir ve belgeleri düzenleyebilir, oluşturma ve sorguları çalıştırmak ve saklı yordamlar, tetikleyiciler ve UDF ile aşağıdaki görüntüde gösterildiği gibi bir sayfadan çalışma: 

![Azure Cosmos DB koleksiyonları sayfası](./media/faq/cosmos-db-data-explorer.png)

### <a name="are-there-changes-to-pricing"></a>Fiyatlandırma değişiklikleri var mı?

Önceki Hayır, Azure Cosmos DB'de uygulamanızı çalıştırmanın maliyeti aynıdır.

### <a name="are-there-changes-to-the-slas"></a>SLA değişiklikler var mı?

Hayır, kullanılabilirlik, tutarlılık, gecikme ve verimlilik için SLA değiştirilmemiştir ve hala portalda görüntülenir. Daha fazla bilgi için bkz: [Azure Cosmos DB SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/).
   
![Örnek verilerle Yapılacaklar uygulama](./media/faq/azure-cosmosdb-portal-metrics-slas.png)


[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md
