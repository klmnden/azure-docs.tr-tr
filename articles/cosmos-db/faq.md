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
ms.date: 11/02/2017
ms.author: mimig
ms.openlocfilehash: 091446fd45b09913dee70dbb4c7e5ebbca02819b
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="azure-cosmos-db-faq"></a>Azure Cosmos DB SSS
## <a name="azure-cosmos-db-fundamentals"></a>Azure Cosmos DB temelleri
### <a name="what-is-azure-cosmos-db"></a>Azure Cosmos DB nedir?
Azure Cosmos DB, şemasız verilerde zengin sorgulama sunan yapılandırılabilir ve güvenilir performans sağlanmasına yardımcı olan bir genel çoğaltılmış, birden çok model veritabanı hizmetidir ve hızlı geliştirme sağlar. Tüm güç tarafından yedeklenir ve Microsoft Azure'nın bir yönetilen platform sağlanır. 

Azure Cosmos DB web, mobil, oyun için doğru çözümdür ve tahmin edilebilir iş çıkarma, yüksek kullanılabilirlik, düşük gecikme süresi ve şemasız veri modeli IOT uygulamaları anahtar gereksinimleridir. Şema esnekliği ve zengin dizin oluşturma sağlar ve tümleşik JavaScript ile çok belgeli işlem desteğini içerir. 

Daha fazla veritabanı soruları yanıtlar ve dağıtma ve bu hizmeti kullanmaya ilişkin yönergeler için bkz: [Azure Cosmos DB belge sayfasının](https://azure.microsoft.com/documentation/services/cosmos-db/).

### <a name="what-happened-to-documentdb"></a>DocumentDB için ne oldu?
DocumentDB API desteklenen API'ları ve veri modelleri için Azure Cosmos DB biridir. Ayrıca, Azure Cosmos DB, grafik API'si (Önizleme), tablo API (Önizleme) ve MongoDB API ile destekler. Daha fazla bilgi için bkz: [DocumentDB müşterilerden sorular](#moving-to-cosmos-db).

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
Yardıma ihtiyacınız olursa, bize üzerinde ulaşmak [yığın taşması](http://stackoverflow.com/questions/tagged/azure-cosmosdb) veya [MSDN Forumu](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=AzureDocumentDB), veya posta göndererek Azure Cosmos DB mühendislik ekibi ile bire bir sohbet zamanlama [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com). 

<a id="try-cosmos-db"></a>
## <a name="try-azure-cosmos-db-subscriptions"></a>Azure Cosmos DB abonelikleri deneyin

Şimdi bir zaman sınırlı Azure Cosmos DB deneyimi ücret ve taahhüt ücretsiz bir abonelik olmadan keyfini çıkarabilirsiniz. Bir Azure Cosmos DB deneyin abonelik için kaydolmak için Git [Azure Cosmos DB ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/). Bu abonelik ayrıdır [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/)ve Azure ücretsiz deneme sürümü veya bir Azure yanı sıra kullanılabilecek Ücretli aboneliği. 

Try Azure Cosmos DB abonelikleri Azure portalında sonraki kullanıcı kimliği ile ilişkili diğer abonelikler görünür. 

Aşağıdaki koşullar deneyin Azure Cosmos DB abonelikler için geçerlidir:

* Bir kapsayıcı SQL (DocumentDB API), Gremlin (grafik API'si) ve tablo API hesapları için abonelik başına.
* MongoDB hesapları için abonelik başına en fazla 3 koleksiyonları.
* 10 GB depolama kapasitesi.
* Aşağıda, genel çoğaltma kullanılabilir [Azure bölgeleri](https://azure.microsoft.com/regions/): Orta ABD, Kuzey Avrupa ve Güneydoğu Asya
* En yüksek verimlilik 5 K RU/s.
* Abonelikler, 24 saat sonra süresi dolacak ve en fazla toplam 48 saat için genişletilebilir.
* Azure destek biletleri için Azure Cosmos DB deneyin hesapları oluşturulamıyor; Ancak, mevcut destek planları ile aboneleri için destek sağlanır. 

## <a name="set-up-azure-cosmos-db"></a>Azure Cosmos DB'yi yedekleyin ayarlayın
### <a name="how-do-i-sign-up-for-azure-cosmos-db"></a>Nasıl Azure Cosmos DB kaydolabilirim?
Azure portalında Azure Cosmos DB kullanılabilir. İlk olarak, Azure aboneliği için kaydolun. Oturum açtığınız sonra Azure Aboneliğinize bir DocumentDB API, grafik API'si (Önizleme), tablo API (Önizleme) veya MongoDB API hesabı ekleyebilirsiniz.

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

## <a name="develop-with-the-table-api-preview"></a>Tablo API (Önizleme) ile geliştirme

### <a name="terms"></a>Koşullar 
Yapı 2017 duyurdu bir tablo veri modeli tarafından Azure Cosmos DB sunumu premium Azure Cosmos DB tablo API (Önizleme) başvuruyor. 

### <a name="how-can-i-use-the-new-table-api-preview-offering"></a>Yeni Tablo API (Önizleme) sunumu nasıl kullanabilir miyim? 
Azure Cosmos DB tablo API kullanılabilir [Azure portal][azure-portal]. Önce Azure aboneliği için kaydolmanız gerekir. Oturum açtığınız sonra Azure Cosmos DB tablo API'si hesabınız Azure aboneliğinize ekleyin ve ardından tabloları hesabınıza ekleyin. 

Önizleme dönemi sırasında zaman [SDK'ları](../cosmos-db/table-sdk-dotnet.md) olan .NET için kullanılabilir, tamamlayarak başlayabiliriz [tablo API](../cosmos-db/create-table-dotnet.md) hızlı başlangıç makalesi.

### <a name="do-i-need-a-new-sdk-to-use-the-table-api-preview"></a>Tablo API (Önizleme) kullanmak için yeni bir SDK gerekiyor mu? 
Evet, [Windows Azure depolama Premium tablosu (Önizleme) SDK](https://www.nuget.org/packages/WindowsAzure.Storage-PremiumTable) NuGet üzerinde kullanılabilir olduğunu ve Azure Cosmos DB tablo API kullanmak için gereklidir. Ek bilgi edinilebilir [Azure Cosmos DB tablo .NET API: indirme ve sürüm notları](https://github.com/Microsoft/azure-docs-pr/cosmos-db/table-sdk-dotnet.md) sayfası. 

### <a name="how-do-i-provide-feedback-about-the-sdk-or-bugs"></a>SDK veya hatalar hakkında geri bildirim nasıl sağlıyor mu?
Geri bildiriminiz aşağıdaki yollardan biriyle paylaşabilirsiniz:

* [Uservoice](https://feedback.azure.com/forums/599062-azure-cosmos-db-table-api)
* [MSDN forumu](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=AzureDocumentDB)
* [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-cosmosdb)

### <a name="what-is-the-connection-string-that-i-need-to-use-to-connect-to-the-table-api-preview"></a>Tablo API (Önizleme) bağlanmak için kullanılacak ihtiyacım bağlantı dizesi nedir?
Bağlantı dizesidir:
```
DefaultEndpointsProtocol=https;AccountName=<AccountNamefromCosmos DB;AccountKey=<FromKeysPaneofCosmosDB>;TableEndpoint=https://<AccountNameFromDocumentDB>.documents.azure.com
```
Bağlantı dizesi, Azure portalında anahtarları sayfasından alabilirsiniz. 

### <a name="how-do-i-override-the-config-settings-for-the-request-options-in-the-new-table-api-preview"></a>Yeni Tablo API (Önizleme) istek seçeneklerinde yapılandırma ayarlarını nasıl geçersiz kılma?
Yapılandırma ayarları hakkında daha fazla bilgi için bkz: [Azure Cosmos DB yetenekleri](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). İstemci uygulaması appSettings bölümünde app.config için ekleyerek ayarlarını değiştirebilirsiniz.

    <appSettings>
        <add key="TableConsistencyLevel" value="Eventual|Strong|Session|BoundedStaleness|ConsistentPrefix"/>
        <add key="TableThroughput" value="<PositiveIntegerValue"/>
        <add key="TableIndexingPolicy" value="<jsonindexdefn>"/>
        <add key="TableUseGatewayMode" value="True|False"/>
        <add key="TablePreferredLocations" value="Location1|Location2|Location3|Location4>"/>....
    </appSettings>


### <a name="are-there-any-changes-for-customers-who-are-using-the-existing-azure-table-storage-sdk"></a>Var olan Azure Table storage'ı SDK kullanan müşteriler için herhangi bir değişiklik var mı?
yok. Var olan Azure Table storage'ı SDK'ları kullanan mevcut veya yeni müşteriler için bir değişiklik bulunmamaktadır. 

### <a name="how-do-i-view-table-data-that-is-stored-in-azure-cosmos-db-for-use-with-the-table-api-review"></a>Tablo API (revıew) ile kullanmak için Azure Cosmos DB içinde depolanan tablo verileri nasıl görüntüleyebilirim? 
Veri göz atmak için Azure portalını kullanabilirsiniz. Tablo API (Önizleme) kod veya sonraki yanıtında belirtilen araçları da kullanabilirsiniz. 

### <a name="which-tools-work-with-the-table-api-preview"></a>Hangi Araçlar tablo API (Önizleme) ile çalışır? 
Azure Gezgini (0.8.9) daha eski sürümünü kullanabilirsiniz.

Belirtilen biçiminde bir bağlantı dizesini almak için esneklik araçlarıyla daha önce yeni tablo API (Önizleme) destekler. Tablo araçları listesi üzerinde sağlanan [Azure Storage istemci araçları](../storage/common/storage-explorers.md) sayfası. 

### <a name="do-powershell-or-azure-cli-work-with-the-new-table-api-preview"></a>PowerShell veya Azure CLI yeni tablo API ile (Önizleme) çalışıyor mu?
Tablo API (Önizleme) için PowerShell ve Azure CLI desteği eklemek planlayın. 

### <a name="is-the-concurrency-on-operations-controlled"></a>Eşzamanlılık denetlenen işlemlerde mi?
Evet, iyimser eşzamanlılık ETag mekanizması kullanımı aracılığıyla sağlanır. 

### <a name="is-the-odata-query-model-supported-for-entities"></a>OData sorgu modelini varlıklar için destekleniyor mu? 
Evet, OData sorgu ve LINQ sorgusu tablo API (Önizleme) destekler. 

### <a name="can-i-connect-to-the-azure-table-storage-and-the-azure-cosmos-db-table-api-preview-side-by-side-in-the-same-application"></a>Azure Table depolama ve Azure Cosmos DB tablo API (Önizleme) yan yana aynı uygulamada bağlanabilir miyim? 
Evet, her bağlantı dizesi aracılığıyla kendi URI işaret CloudTableClient, iki ayrı ayrı örnekleri oluşturarak bağlanabilir.

### <a name="how-do-i-migrate-an-existing-azure-table-storage-application-to-this-new-offering"></a>Bu yeni sunum var olan bir Azure Table depolama uygulamayı nasıl geçişini?
Var olan tablo depolama verilerinizde Azure Cosmos DB tablo API yenilik yararlanmak için başvurun [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com). 

### <a name="what-is-the-roadmap-for-this-service-and-when-will-you-offer-other-standard-table-api-functionality"></a>Bu hizmet için yol haritası nedir ve ne zaman diğer standart tablo API işlevleri sunacaktır?
Biz doğru İST ilerlemeden gibi SAS belirteci, ServiceContext, istatistikleri, istemci tarafı şifreleme, analiz ve diğer özellikler için destek eklemeyi planlama Bize geri bildirim üzerinde verebilirsiniz [Uservoice](https://feedback.azure.com/forums/599062-azure-cosmos-db-table-api). 

### <a name="how-is-expansion-of-the-storage-size-done-for-this-service-if-for-example-i-start-with-n-gb-of-data-and-my-data-will-grow-to-1-tb-over-time"></a>Örneğin, t ile başlatırsanız, bu hizmet için yapılan depolama boyutunu genişletme nasıl yapıldığını  *n*  GB veri ve verilerimi zamanla 1 TB'ye kadar büyüme? 
Azure Cosmos DB yatay ölçekleme kullanımı aracılığıyla sınırsız depolama sağlamak için tasarlanmıştır. Hizmet izlemek ve etkili bir şekilde depolama alanınızın artırın. 

### <a name="how-do-i-monitor-the-table-api-preview-offering"></a>Tablo API (Önizleme) teklifinin nasıl izlerim?
Tablo API (Önizleme) kullanabilirsiniz **ölçümleri** istekleri ve depolama alanı kullanımı izlemek için Azure portalında bölmesi. 

### <a name="how-do-i-calculate-the-throughput-i-require"></a>I gerektiren verimlilik nasıl hesaplar?
İşlemler için gerekli olan TableThroughput hesaplamak için kapasite tahmin kullanabilirsiniz. Daha fazla bilgi için bkz: [tahmin istek birimleri ve veri depolama](https://www.documentdb.com/capacityplanner). Genel olarak, JSON olarak, varlığı temsil eder ve işlemleri için sayıları sağlayın. 

### <a name="can-i-use-the-new-table-api-preview-sdk-locally-with-the-emulator"></a>Yeni Tablo API (Önizleme) SDK öykünücüsü ile yerel olarak kullanabilir miyim?
Evet, yeni bir SDK kullandığınızda yerel öykünücü ile tablo API (Önizleme) kullanabilirsiniz. Yeni öykünücü indirmek için Git [yerel geliştirme ve sınama için Azure Cosmos DB öykünücüsünü kullanma](local-emulator.md). App.config StorageConnectionString değeri olması gerekir:

```
DefaultEndpointsProtocol=https;AccountName=localhost;AccountKey=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==;TableEndpoint=https://localhost:8081`. 
```

### <a name="can-my-existing-azure-table-storage-application-work-with-the-table-api-preview"></a>Var olan Azure Table depolama Uygulamam tablo API (Önizleme) ile çalışabilir mi? 
Yeni Tablo API (Önizleme)'ın yüzey alanını oluşturma, silme, güncelleştirme ve .NET API'sindeki sorgu işlemleri arasında var olan Azure Table depolama SDK'sı ile uyumludur. Tablo API (Önizleme) hem Bölüm anahtarı hem de satır anahtarını gerektirdiğinden satır anahtarı olduğundan emin olun. Ayrıca bu hizmet sunumu GA doğru ilerlemeden daha fazla SDK destek eklemeyi planlıyoruz.

### <a name="do-i-need-to-migrate-my-existing-azure-table-storage-applications-to-the-new-sdk-if-i-do-not-want-to-use-the-table-api-preview-features"></a>Tablo API (Önizleme) özellikleri kullanmak istemediğiniz için yeni bir SDK var olan Azure Table depolama uygulamalarım geçirmeniz gerekiyor mu?
Hayır, oluşturabilir ve varolan Azure Table depolama varlıkları herhangi bir türde kesintisiz kullanın. Ancak, yeni tablo API (Önizleme) kullanmıyorsanız, otomatik dizin, ek tutarlılık seçeneği veya genel dağıtım kazancı olmaz. 

### <a name="how-do-i-add-replication-of-the-data-in-the-table-api-preview-across-multiple-regions-of-azure"></a>Nasıl veri çoğaltma işlemi tablo API (Önizleme) birden çok Azure bölgeler arasında ekleyebilirim?
Cosmos DB Azure portal'ın kullanabilirsiniz [genel çoğaltma ayarları](tutorial-global-distribution-documentdb.md#portal) uygulamanız için uygun bölgeler eklemek için. Genel olarak dağıtılmış bir uygulama geliştirmek için PreferredLocation bilgi okuma düşük gecikme süresi sağlamak için yerel bölge kümesi ile uygulamanızı eklemelisiniz. 

### <a name="how-do-i-change-the-primary-write-region-for-the-account-in-the-table-api-preview"></a>Tablo API (Önizleme) hesap için birincil yazma bölge nasıl değişiyor?
Bir bölge ekleyin ve ardından gerekli bölgeye yük devri için Azure Cosmos DB genel çoğaltma portal bölmesinde kullanabilirsiniz. Yönergeler için bkz: [bölgeli Azure Cosmos DB hesaplarıyla geliştirme](regional-failover.md). 

### <a name="how-do-i-configure-my-preferred-read-regions-for-low-latency-when-i-distribute-my-data"></a>Verilerimi dağıttığınızda nasıl düşük gecikme süresi için tercih edilen my okuma bölgeler yapılandırırım? 
Yerel konumdan okuma yardımcı olmak için app.config dosyasında PreferredLocation tuşunu kullanın. LocationMode ayarlanmışsa, var olan uygulamalar için tablo API (Önizleme) bir hata oluşturur. Premium tablo API (Önizleme) bu bilgileri app.config dosyasından alır çünkü bu kodu kaldırın. Daha fazla bilgi için bkz: [Azure Cosmos DB yetenekleri](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="how-should-i-think-about-consistency-levels-in-the-table-api-preview"></a>Tablo API (Önizleme) de tutarlılık düzeyleri hakkında ne düşünüyorsunuz? 
Azure Cosmos DB iyi reasoned dengelemeler tutarlılık, kullanılabilirlik ve gecikme süresi arasında sağlar. Tablo düzeyinde sağ tutarlılık modelini seçin ve veri sorgularken tek tek istekte azure Cosmos DB tablo API (Önizleme) geliştiricilerine beş tutarlılık düzeyi sunar. Bir istemci bağlandığında, tutarlılık düzeyi belirtebilirsiniz. TableConsistencyLevel anahtarının değerini app.config ayarı aracılığıyla düzeyini değiştirebilirsiniz. 

Tablo API (Önizleme) ile "kendi yazma varsayılan olarak oturum tutarlılığı ile okuma" Düşük gecikmeli okur sağlar. Daha fazla bilgi için bkz: [tutarlılık düzeylerini](consistency-levels.md). 

Varsayılan olarak, Azure Table storage bir bölgedeki güçlü tutarlılık ve ikincil konumlarda Eventual tutarlılık sağlar. 

### <a name="does-the-azure-cosmos-db-table-api-offer-more-consistency-levels-than-azure-table-storage"></a>Azure Cosmos DB tablo API Azure Table storage'den daha fazla tutarlılık düzeyleri sunar?
Evet, için dağıtılan yapı Azure Cosmos DB, yararlı hakkında daha fazla bilgi için bkz: [tutarlılık düzeylerini](consistency-levels.md). Tutarlılık düzeylerini garanti sağlamadığı güvenle kullanabilirsiniz. Daha fazla bilgi için bkz: [Azure Cosmos DB yetenekleri](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="when-global-distribution-is-enabled-how-long-does-it-take-to-replicate-the-data"></a>Genel dağıtım etkinleştirildiğinde, ne kadar veri çoğaltmak için sürer?
Azure Cosmos DB yerel bölge verilerde işlemi tamamlar ve diğer bölgelerinde hemen sağlasa da, milisaniye veri iter. Bu çoğaltma yalnızca gidiş dönüş süresi veri merkezinin üzerinde (RTT) bağlıdır. Genel dağıtım Özelliği Azure Cosmos DB'nin hakkında daha fazla bilgi için bkz: [Azure Cosmos DB: Azure ile ilgili genel dağıtılmış veritabanı hizmeti](distribute-data-globally.md).

### <a name="can-the-read-request-consistency-level-be-changed"></a>Okuma isteği tutarlılık düzeyi değiştirilebilir mi?
Azure Cosmos DB ile (tablo) kapsayıcı düzeyinde tutarlılık düzeyi ayarlayabilirsiniz. SDK'yı kullanarak, app.config dosyasında TableConsistencyLevel anahtarı için değer sağlayarak düzeyini değiştirebilirsiniz. Olası değerler şunlardır: güçlü, sınırlanmış eskime durumu, oturum, tutarlı öneki ve Eventual. Daha fazla bilgi için bkz: [veri ince ayarlanabilir tutarlılık düzeyleri Azure Cosmos veritabanı](consistency-levels.md). Anahtar, istek tutarlılık birden çok tablo ayarını adresindeki düzeyinde ayarlayamazsınız emin olur. Örneğin, tablo için tutarlılık düzeyi Eventual ve güçlü isteği tutarlılık düzeyinde ayarlayamazsınız. 

### <a name="how-does-the-table-api-preview-account-handle-failover-if-a-region-goes-down"></a>Nasıl bir bölge kullanılamaz hale gelirse tablo API (Önizleme) hesap yük devretme ele alıyor? 
Azure Cosmos DB tablo API (Önizleme) Azure Cosmos DB Genel dağıtılmış platformundan taşır. Uygulamanızı datacenter kapalı kalma süresi etkilenmemesini sağlamak için Azure Cosmos DB Portalı'ndaki hesap için en az bir daha fazla bölge etkinleştirin [bölgeli Azure Cosmos DB hesaplarıyla geliştirme](regional-failover.md). Portalı kullanarak bölge önceliğini ayarlayabilirsiniz [bölgeli Azure Cosmos DB hesaplarıyla geliştirme](regional-failover.md). 

Hesap için istediğiniz ve burada bu için bir yük devretme öncelik sağlayarak yük devredebildiğini kontrol sayıda bölgeleri ekleyebilirsiniz. Elbette, veritabanını kullanmak için bir uygulama var. çok vermeniz gerekir. Bunu yaptığınızda, müşterilerinizin kapalı kalma süresi karşılaşmazsınız. SDK olan istemci otomatik olarak giriş. Diğer bir deyişle, kapalı ve yeni bölge için otomatik olarak yük bölgeyi algılayabilir.

### <a name="is-the-table-api-preview-enabled-for-backups"></a>Tablo API (Önizleme) yedeklemeler için etkin mi?
Evet, yedeklemeler için Azure Cosmos DB platformdan Azure Cosmos DB tablo API (Önizleme) taşır. Yedeklemeleri otomatik olarak yapılır. Daha fazla bilgi için bkz: [çevrimiçi yedekleme ve geri yükleme Azure Cosmos DB ile](online-backup-and-restore.md).

 
### <a name="does-the-table-api-preview-index-all-attributes-of-an-entity-by-default"></a>Tablo API (Önizleme) bir varlığın tüm öznitelikleri varsayılan dizin mu?
Evet, bir varlığın tüm öznitelikleri varsayılan olarak Azure Cosmos DB dizine alınır. Daha fazla bilgi için bkz: [Azure Cosmos DB: ilkeleri dizin](indexing-policies.md). 

### <a name="does-this-mean-i-do-not-have-to-create-multiple-indexes-to-satisfy-the-queries"></a>Değil sahibim sorguları karşılamak için birden çok dizin oluşturmak için bu ortalama mu? 
Evet, Azure Cosmos DB herhangi bir şema tanımı olmadan tüm özniteliklerin otomatik dizin oluşturma sağlar. Bu Otomasyon odak uygulama yerine dizin oluşturma ve yönetme için geliştiricilere boşaltır. Daha fazla bilgi için bkz: [Azure Cosmos DB: ilkeleri dizin](indexing-policies.md).

### <a name="can-i-change-the-indexing-policy"></a>Dizin oluşturma ilkesini değiştirebilir miyim?
Evet, dizin tanımı sağlayarak dizin oluşturma ilkesini değiştirebilirsiniz. Daha fazla bilgi için bkz: [Azure Cosmos DB yetenekleri](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). Düzgün kodlamak ve ayarları kaçış gerekir. 

Dize json biçiminde app.config dosyasında:
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
Önizleme'de, tablo API Azure Table storage aynı sorgu işlevleri sağlar. Azure Cosmos DB, sıralama, toplamalar, Jeo-uzamsal sorgu, hiyerarşi ve çok çeşitli yerleşik işlevler de destekler. Biz gelecekteki hizmeti güncelleştirmesine tablo API ek işlevsellik sağlar. Daha fazla bilgi için bkz: [SQL sorgularını Azure Cosmos DB DocumentDB API için](../documentdb/documentdb-sql-query.md).
 
### <a name="when-should-i-change-tablethroughput-for-the-table-api-preview"></a>Tablo API (Önizleme) ne zaman TableThroughput değiştiririm?
Aşağıdaki koşullardan herhangi biri geçerli olduğu durumlarda TableThroughput değiştirmeniz gerekir:
* Ayıklama, dönüştürme ve yükleme (ETL) veri gerçekleştiriyorsunuz veya kısa sürede çok miktarda veri yüklemek istediğiniz. 
* Daha fazla verimlilik arka uçtaki kapsayıcısından gerekir. Örneğin, kullanılan işleme sağlanan işleme büyük ve, kısıtlanan bakın. Daha fazla bilgi için bkz: [Azure Cosmos DB kapsayıcıları için kümesi işleme](set-throughput.md).

### <a name="can-i-scale-up-or-scale-down-the-throughput-of-my-table-api-preview-table"></a>Ölçeği artırma veya miyim my tablo API (Önizleme) tablosunun işleme ölçeğini? 
Evet, üretilen iş ölçeklendirmek için Azure Cosmos DB Portalı'nın ölçek bölmesini kullanabilirsiniz. Daha fazla bilgi için bkz: [kümesi işleme](set-throughput.md).

### <a name="is-a-default-tablethroughput-set-for-newly-provisioned-tables"></a>Varsayılan eylem TableThroughput için yeni sağlanan tabloları kümesi mi?
Evet, app.config aracılığıyla TableThroughput geçersiz kılmaz ve önceden oluşturulmuş bir kapsayıcı Azure Cosmos DB'de kullanmayın, hizmet 400 işleme ile bir tablo oluşturur.
 
### <a name="is-there-any-change-of-pricing-for-existing-customers-of-azure-table-storage"></a>Azure Table depolama mevcut müşteriler için fiyatlandırma herhangi bir değişiklik var mı?
yok. Var olan Azure Table depolama müşterileri için fiyatı değişiklik yoktur. 

### <a name="how-is-the-price-calculated-for-the-table-api-preview"></a>Fiyat tablosu için (Önizleme) API nasıl hesaplanır? 
Azure Cosmos DB tablo API (Önizleme) fiyat üzerinde ayrılmış TableThroughput bağlıdır. 

### <a name="how-do-i-handle-any-throttling-in-table-api-preview-offering"></a>Tablo API (Önizleme) Sunumda azaltma nasıl işleneceğini? 
Sağlanan işleme kapasitesi temeldeki kapsayıcısı için istek oranı aşarsa, bir hata alırsınız ve yeniden deneme ilkesi uygulayarak çağrı SDK'yı yeniden deneyecek.

### <a name="why-do-i-need-to-choose-a-throughput-apart-from-partitionkey-and-rowkey-to-take-advantage-of-the-azure-cosmos-db-table-api-preview"></a>Üretilen iş PartitionKey ve RowKey Azure Cosmos DB tablo API (Önizleme) yararlanmak için dışında seçmek neden gerekiyor mu?
Bir app.config dosyasında belirtmezseniz, azure Cosmos DB, kapsayıcı için bir varsayılan işleme ayarlar. 

Azure Cosmos DB, performansı ve gecikme süresi, üst sınır işlemi ile garantileri sağlar. Bu garantisi mümkün olduğunda altyapısı kiracının işlemlerini İdaresi uygulayabilirsiniz. TableThroughput ayarı platform bu kapasite ayırır ve işletimsel başarı güvence altına alır çünkü garantili üretilen iş ve gecikmeyi, edinmenizi sağlar. 

Üretilen iş belirtimi kullanarak, Özellikler esnek maliyet tasarrufu uygulamanızı mevsimselliğin yararlanabilir ve verimlilik ihtiyaçlarını karşılamak için değiştirebilirsiniz.

### <a name="azure-storage-sdk-has-been-very-inexpensive-for-me-because-i-pay-only-to-store-the-data-and-i-rarely-query-the-new-azure-cosmos-db-offering-seems-to-be-charging-me-even-though-i-have-not-performed-a-single-transaction-or-stored-anything-can-you-please-explain"></a>I yalnızca depolamak veri ve nadiren sorgu ödeme olduğundan azure depolama SDK'sı benim için çok pahalı olmayan olmuştur. Yeni Azure Cosmos DB sunum ı değil tek bir işlem gerçekleştirilen veya hiçbir şey depolanan olsa bile bana şarj gibi görünüyor. Lütfen açıklayabilir?

Azure Cosmos DB garanti kullanılabilirlik, gecikme ve verimlilik için Genel dağıtılmış, SLA tabanlı sistemiyle olacak şekilde tasarlanmıştır. Azure Cosmos veritabanı işleme ayırdığınızda, bu, diğer sistemler üretimini farklı olarak sağlanır. Azure Cosmos DB müşteriler, ikincil dizinler ve genel dağıtım gibi istediniz ek özellikler sağlar.  

### <a name="i-never-get-a-quota-full-notification-indicating-that-a-partition-is-full-when-i-ingest-data-into-azure-table-storage-with-the-table-api-preview-i-do-get-this-message-is-this-offering-limiting-me-and-forcing-me-to-change-my-existing-application"></a>Hiçbir zaman (bir bölüm dolu belirten) bir "Kota tam" bildirim alıyorum zaman ı alma verileri Azure Table depolama alanına. Bu ileti tablo API ile (Önizleme) alıyorum. Bana sınırlandırma ve bana varolan Uygulamam değiştirmek için zorlama tanıyor mu?

Azure Cosmos DB sınırsız ölçek gecikme süresi, performans, kullanılabilirlik ve tutarlılığı garanti sağlayan bir SLA tabanlı bir sistemdir. Garantili premium performans sağlamak için veri boyutu ve dizin yönetilebilir ve ölçeklenebilir olduğundan emin olun. Varlıklar veya bölüm anahtarı başına öğe sayısı 10 GB sınırını harika arama ve sorgu performansı sağladığımız sağlamaktır. Uygulamanızı Azure Storage için bile iyi ölçeklendirir emin olmak için öneririz, *değil* bölümde tüm bilgileri depolamak ve onu sorgulama hot bir bölüm oluşturun. 

### <a name="so-partitionkey-and-rowkey-are-still-required-with-the-new-table-api-preview"></a>Bu nedenle PartitionKey ve RowKey yeni tablo API ile (Önizleme) hala gereklidir? 
Evet. Tablo API (Önizleme)'ın yüzey alanını Azure Table depolama SDK'sı benzer olduğundan bölüm anahtarı verileri dağıtmak için etkili bir yol sağlar. Satır anahtarını Bu bölüm içinde benzersizdir. Satır anahtarını bulunması gerekir ve olduğu gibi standart SDK null olamaz. RowKey uzunluğu 255 bayt ve PartitionKey uzunluğunu 100 (en kısa sürede 1 KB ile artırılacak) bayt. 

### <a name="what-are-the-error-messages-for-the-table-api-preview"></a>Tablo API (Önizleme) için hata iletileri nelerdir?
Bu önizleme Azure Table storage ile uyumlu olduğundan, hataların çoğu hataları standart tablosundan eşleyin. 

### <a name="why-do-i-get-throttled-when-i-try-to-create-lot-of-tables-one-after-another-in-the-table-api-preview"></a>Tabloları miktarda birbiri ardından tablo API (Önizleme) oluşturmak çalıştığınızda neden ı kısıtlanan?
Azure Cosmos DB gecikme süresi, performans, kullanılabilirlik ve tutarlılık sağlayan bir SLA tabanlı bir sistemdir. Sağlanan sistem olduğundan, bu gereksinimleri güvence altına almak için kaynakları ayırır. Tabloları oluşturulmasını hızlı oranını algıladı ve daraltma. Tablo oluşturma hızında arayın ve az 5 dakika başına alt öneririz. Tablo API (Önizleme) sağlanan sistem olduğunu unutmayın. Şu anda bunu sağlamak için ödeme başlar. 

## <a name="develop-against-the-graph-api-preview"></a>Grafik API'si (Önizleme) karşı geliştirin
### <a name="how-can-i-apply-the-functionality-of-graph-api-preview-to-azure-cosmos-db"></a>Nasıl ı grafik API'si (Önizleme) işlevselliğini Azure Cosmos DB uygulayabilir mi?
Grafik API'si (Önizleme) işlevselliğini uygulamak için bir uzantı kitaplığını kullanabilirsiniz. Bu kitaplık Microsoft Azure grafikleri denir ve NuGet üzerinde kullanılabilir. 

### <a name="it-looks-like-you-support-the-gremlin-graph-traversal-language-do-you-plan-to-add-more-forms-of-query"></a>Gremlin grafik geçişi dil desteği gibi görünüyor. Daha fazla form sorgusunun eklemek planlıyor musunuz?
Evet, diğer mekanizmaları sorgu için gelecekte ekleme planlıyoruz. 

### <a name="how-can-i-use-the-new-graph-api-preview-offering"></a>Grafik API'si (Önizleme) yenilik nasıl kullanabilir miyim? 
Başlamak için tamamlamak [grafik API'si](../cosmos-db/create-graph-dotnet.md) hızlı başlangıç makalesi.

<a id="moving-to-cosmos-db"></a>
## <a name="questions-from-documentdb-customers"></a>DocumentDB müşterilerden sorular
### <a name="why-are-you-moving-to-azure-cosmos-db"></a>Neden Azure Cosmos DB taşıyor? 

Azure Cosmos DB sonraki büyük artık genel olarak dağıtılmış ölçekli bulut veritabanlarında ' dir. Bir DocumentDB müşteri olarak devrim niteliğinde sistem ve yetenekleri Azure Cosmos DB tarafından sunulan erişim sahip.

Azure Cosmos DB 2010 Microsoft içindeki büyük ölçekli uygulamalar oluşturmanın geliştiriciler tarafından karşıya kalınan adres "Proje Floransa" başlatılır. Biz bu teknoloji ilk nesil 2015'te Azure geliştiricilere Azure DocumentDB biçiminde kullanıma şekilde genel dağıtılmış uygulamalar oluşturmanın zorluklarından Microsoft'a benzersiz değil. 

O zamandan bu yana, eklenen yeni özellikler artık ve önemli yeni özellikler sunulmuştur. Azure Cosmos DB sonucudur. Bu sürümde bir parçası olarak verileriyle, DocumentDB müşterilerin Azure Cosmos DB müşteriler otomatik olarak ve sorunsuz bir şekilde haline gelir. Bu özellikler temel veritabanı motoru yanı sıra genel dağıtım, esnek ölçeklenebilirlik ve endüstri lideri, kapsamlı SLA alanlarıdır. Özellikle, biz verimli bir şekilde tüm popüler veri modelleri, türü sistemleri ve API'ları Azure Cosmos DB temel alınan veri modeline eşlemek için Azure Cosmos DB veritabanı altyapısı gelişim göstermiştir. 

Bu çalışmanın geçerli Geliştirici dönük göstergeleri yeni desteğidir [Gremlin](../cosmos-db/graph-introduction.md) ve [tablo API'leri](../cosmos-db/table-introduction.md). Ve yalnızca başlangıç budur. Daha fazla performans ve küresel ölçekli depolama gelişmeleri, zaman içinde diğer popüler API'ler ve yeni veri modelleri eklemek planlıyoruz. 

DocumentDB noktasındaki önemlidir [SQL dialect](../documentdb/documentdb-sql-query.md) her zaman temel Azure Cosmos DB destekleyebileceği birçok API'ları yalnızca biri olmuştur. Azure Cosmos DB gibi tam olarak yönetilen bir hizmet kullanan geliştiriciler için yalnızca hizmet hizmeti tarafından sunulan API'leri arabirimidir. Hiçbir şey gerçekten var olan DocumentDB müşteriler için değiştirir. Azure Cosmos DB içinde tam olarak aynı SQL DocumentDB sunan API alın. Ve şimdi (ve gelecekte), daha önce erişilemez diğer capabilities erişebilirsiniz 

Bizim devam eden iş başka bir göstergeleri genel ve esnek ölçeklenebilirlik işleme ve depolama genişletilmiş temelidir. Biz, genel dağıtım alt sistemi birkaç temel geliştirmeler yapıldı. Birçok geliştirici dönük özelliklerden bir toplam beş iyi tanımlanmış tutarlılık modelleri yapar tutarlı önek tutarlılık modeli biridir. Biz, bunlar yetişkin birçok daha ilginç özellikleri serbest bırakır. 

### <a name="what-do-i-need-to-do-to-ensure-that-my-documentdb-resources-continue-to-run-on-azure-cosmos-db"></a>DocumentDB Kaynaklarım Azure Cosmos DB üzerinde çalışmaya devam etmesini sağlamak için yapmanız gerekenler nelerdir?

Hiçbir değişiklik hiç olmanız gerekir. DocumentDB kaynaklarınızı Azure Cosmos DB kaynakları sunulmuştur ve bu taşıma oluştuğunda hizmet kesintisine neden olmadan olmuştur.

### <a name="what-changes-do-i-need-to-make-for-my-app-to-work-with-azure-cosmos-db"></a>Uygulamam Azure Cosmos DB ile çalışacak biçimde yükseltilmesi hangi değişikliklerin gerekiyor mu?

Yapmak için bir değişiklik yoktur. Sınıfları, ad alanları ve NuGet paket adlarının değişmemiştir. Her zaman, SDK'ları en son özellikleri ve geliştirmeleri avantajlarından yararlanmak için güncel tutmanızı öneririz. 

### <a name="whats-changed-in-the-azure-portal"></a>Azure portalında Değiştirilenler?

DocumentDB portalında bir Azure hizmeti olarak artık görünür. Onun yerine aşağıdaki görüntüde gösterildiği gibi yeni bir Azure Cosmos DB simge olur. Önce oldukları ve verimlilik, değişiklik tutarlılık düzeyleri ve İzleyici SLA hala ölçeği gibi tüm koleksiyonlar kullanılabilir. Veri Gezgini (Önizleme) yeteneklerini geliştirilmiştir. Şimdi görüntüleyebilir ve belgeleri düzenleyebilir, oluşturma ve sorguları çalıştırmak ve saklı yordamlar, tetikleyiciler ve UDF ile aşağıdaki görüntüde gösterildiği gibi bir sayfadan çalışma: 

![Azure Cosmos DB koleksiyonları dikey penceresi](./media/faq/cosmos-db-data-explorer.png)

### <a name="are-there-changes-to-pricing"></a>Fiyatlandırma değişiklikleri var mı?

Önceki Hayır, Azure Cosmos DB'de uygulamanızı çalıştırmanın maliyeti aynıdır.

### <a name="are-there-changes-to-the-slas"></a>SLA değişiklikler var mı?

Hayır, kullanılabilirlik, tutarlılık, gecikme ve verimlilik için SLA değiştirilmemiştir ve hala portalda görüntülenir. Daha fazla bilgi için bkz: [Azure Cosmos DB SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/).
   
![Örnek verilerle Yapılacaklar uygulama](./media/faq/azure-cosmosdb-portal-metrics-slas.png)


[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md
