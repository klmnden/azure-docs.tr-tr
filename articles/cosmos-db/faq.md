---
title: Azure Cosmos DB hakkında sık sorulan sorular | Microsoft Docs
description: Azure Cosmos DB, küresel olarak dağıtılmış çok modelli veritabanı hizmeti hakkında sık sorulan soruların yanıtlarını alın. Kapasite, performans düzeyleri ve ölçeklendirme hakkında bilgi edinin.
keywords: Sık sorulan sorular, documentdb, azure, Microsoft azure, veritabanı soruları
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: sngun
ms.openlocfilehash: 30ebe4f990dc65e53c34673f0948d3aa2240385c
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37859709"
---
# <a name="azure-cosmos-db-faq"></a>Azure Cosmos DB ile ilgili SSS
## <a name="azure-cosmos-db-fundamentals"></a>Azure Cosmos DB ile ilgili temel bilgiler
### <a name="what-is-azure-cosmos-db"></a>Azure Cosmos DB nedir?
Azure Cosmos DB, şemadan bağımsız veriler üzerinde zengin sorgulama sunan, yapılandırılabilir ve güvenilir bir performans sunmaya yardımcı olur ve hızlı geliştirme sağlar bir genel olarak çoğaltılan, çok modelli veritabanı hizmetidir. Tüm gücü tarafından desteklenen ve Microsoft Azure'nın yönetilen bir platform sağlanır. 

Azure Cosmos DB web, mobil, oyun için doğru çözümdür ve öngörülebilir üretilen iş hacmi, yüksek oranda kullanılabilirlik ve düşük gecikme süresi ve şemasız veri modelinin IOT uygulamaları anahtar koşulları yer almaktadır. Şema esnekliği ve zengin dizin oluşturma sağlar ve tümleşik JavaScript ile çok belgeli işlem desteğini içerir. 

Daha fazla veritabanı soruları yanıtlar ve dağıtma ve bu hizmeti kullanmaya ilişkin yönergeler için bkz. [Azure Cosmos DB belgeleri sayfasının](https://docs.microsoft.com/azure/cosmos-db/).

### <a name="what-happened-to-the-documentdb-api"></a>DocumentDB API'si için ne oldu?

Azure Cosmos DB DocumentDB API'sine veya SQL (DocumentDB) API artık Azure Cosmos DB SQL API bilinir. DocumentDB API'si ile oluşturulan uygulamalarınızı çalıştırmaya devam etmek için herhangi bir ayarı değiştirmek gerekmez. İşlevselliği aynı kalacak.

Bir DocumentDB API'si hesabınız önce varsa, artık herhangi bir değişiklik fatura ile bir SQL API hesabı var. 

### <a name="what-happened-to-azure-documentdb-as-a-service"></a>Azure DocumentDB hizmeti olarak ne oldu?

Azure DocumentDB hizmetinin artık Azure Cosmos DB hizmetini bir parçası ve SQL API'si biçiminde ortaya çıkmaktadır. Azure DocumentDB üzerinde oluşturulan uygulamalar hiçbir değişiklik yapmadan Azure Cosmos DB SQL API'sine karşı çalışır. Ayrıca, Azure Cosmos DB Graph API, tablo API'si, MongoDB API'sini ve Cassandra API'si (Önizleme) destekler.

### <a name="what-are-the-typical-use-cases-for-azure-cosmos-db"></a>Azure Cosmos DB için genel kullanım örnekleri nelerdir?
Azure Cosmos DB için yeni web, mobil, oyun, iyi bir seçimdir ve IOT uygulamaları burada otomatik ölçeğin, tahmin edilebilir performans, hızlı sırası milisaniye yanıt sürelerinin ve sorgu şemadan bağımsız veriler üzerinde önemlidir. Azure Cosmos DB kendisi, uygulama veri modellerinin sürekli yineleme destekleme ve hızlı geliştirme için uygundur. Kullanıcı tarafından oluşturulan içeriği ve verileri yöneten uygulamalar [Azure Cosmos DB için ortak kullanım durumları](use-cases.md). 

### <a name="how-does-azure-cosmos-db-offer-predictable-performance"></a>Azure Cosmos DB tahmin edilebilir performansı nasıl sunar?
A [istek birimi](request-units.md) (RU) olan bir Azure Cosmos DB'de aktarım hızı ölçümü. 1 RU aktarım hızı için 1 KB boyutundaki bir belgeyi GET işlemesine karşılık gelir. Okuma, yazma, SQL sorguları ve saklı yordam yürütmeleri dahil olmak üzere Azure Cosmos DB'deki her işlemin, işlemi tamamlamak için gereken aktarım hızına göre belirleyici bir RU değerine sahiptir. CPU, GÇ, bellek ve bunların uygulama işlemesini nasıl etkilediğini hakkında düşünmek yerine tek RU ölçü açısından düşünebilirsiniz.

Her Azure Cosmos DB kapsayıcısı RU saniye başına aktarım hızı açısından sağlanan aktarım hızı ile ayırabilirsiniz. Her ölçekten uygulama için RU değerlerini ölçmek için istekleri ayrı ayrı kıyaslayabilir ve tüm istekler genelinde istek birimlerinin toplam işlemek için bir kapsayıcı sağlayın. Ayrıca, ölçeği büyütün veya ihtiyaçları, uygulama geliştikçe, kapsayıcının aktarım hızını ölçeklendirin. İstek birimleri hakkında daha fazla bilgi ve Yardım için kapsayıcı belirleme bkz gereksinimlerini [aktarım hızı gereksinimlerini tahmin etme](request-units.md#estimating-throughput-needs) deneyin [aktarım hızı hesaplayıcı](https://www.documentdb.com/capacityplanner). Terim *kapsayıcı* burada başvuran bir SQL API'si koleksiyonu, Graph API'si grafiği, MongoDB API koleksiyon ve tablo API'si tablo ifade eder. 

### <a name="how-does-azure-cosmos-db-support-various-data-models-such-as-keyvalue-columnar-document-and-graph"></a>Azure Cosmos DB anahtar/değer, sütun, belge ve graf gibi çeşitli veri modellerini nasıl destekler?

Anahtar/değer (tablo) sütunlu, belge ve graf verilerini (atom, kayıt ve dizileri) ARS nedeniyle desteklendiğinden, Azure Cosmos DB tasarım modelleri nelerdir oluşturulmuştur. Atom, kayıt ve dizileri kolayca eşlenen ve çeşitli veri modelleri için öngörülen. Apı'lerdir kullanılabilir şu anda (SQL, MongoDB, tablo ve graf API'lerini) modellerinin bir alt kümesi için ve başkaları için ek veri modelleri belirli gelecekte kullanıma sunulacaktır.

Azure Cosmos DB, bir şema şemadan bağımsız dizinleme altyapısı otomatik olarak tüm verilerin herhangi bir şema veya ikincil dizinler ' geliştiriciden gerek kalmadan alır, şemalardan sahiptir. Altyapı, dizin ve sorgu işleme alt sistemlerin depolama düzeni ayırın (ters, sütunlu, ağacı) mantıksal dizin düzenleri kümesi kullanır. Cosmos DB kablo protokolleri ve API'leri bir dizi Genişletilebilir biçimde destekler ve verimli bir şekilde çekirdek veri modeli (1) ve (2) birçok veri modelini yerel olarak destekleyen benzersiz olarak özellikli getirerek mantıksal dizin düzenleri çevirmek olanağı da vardır.

### <a name="is-azure-cosmos-db-hipaa-compliant"></a>Azure Cosmos DB HIPAA ile uyumlu mu?
Evet, Azure Cosmos DB, HIPAA uyumlu olduğundan. HIPAA, bağımsız olarak tanımlanabilen sağlık bilgilerinin kullanımı, açıklanması ve korunması için gereksinimler belirler. Daha fazla bilgi için bkz. [Microsoft Güven Merkezi](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### <a name="what-are-the-storage-limits-of-azure-cosmos-db"></a>Azure Cosmos DB depolama sınırları nelerdir?
Toplam Azure Cosmos DB içinde bir kapsayıcı depolayan veri miktarının sınırı yoktur.

### <a name="what-are-the-throughput-limits-of-azure-cosmos-db"></a>Azure Cosmos DB işleme sınırları nelerdir?
Azure Cosmos DB içinde bir kapsayıcı destekleyen bir aktarım hızı toplam miktarı sınırı yoktur. İş yükünüz kabaca eşit bir şekilde bölüm anahtarlarını yeterince çok sayıda arasında dağıtmak için anahtar fikirdir bakın.

### <a name="are-direct-and-gateway-connectivity-modes-encrypted-"></a>Doğrudan ve ağ geçidi bağlantı modları şifrelenir? 
Evet her iki mod tam olarak her zaman şifrelenir. 

### <a name="how-much-does-azure-cosmos-db-cost"></a>Azure Cosmos DB nin ücreti ne kadardır?
Ayrıntılar için başvurmak [fiyatlandırma ayrıntıları Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/) sayfası. Azure Cosmos DB kullanım ücretleri, sağlanan kapsayıcılar, kapsayıcıları çevrimiçi saat sayısına göre belirlenir ve her kapsayıcı için sağlanan aktarım hızı. Terim *kapsayıcıları* burada SQL API koleksiyonu, Graph API'si grafiği, MongoDB API koleksiyon ve tablo API'si tabloları ifade eder. 

### <a name="is-a-free-account-available"></a>Ücretsiz bir hesap var mı?
Evet, taahhüt ücret ödemeden bir süre sınırlı hesabı için kaydolabilirsiniz. Kaydolmak için ziyaret [Azure Cosmos DB'yi ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/) veya daha fazla bilgi [Azure Cosmos DB ile ilgili SSS deneyin](#try-cosmos-db).

Azure'da yeniyseniz için kaydolabilirsiniz bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/), size sağlayan 30 gün ve ve tüm Azure hizmetlerini denemek için kredi. Visual Studio aboneliğiniz varsa, aynı zamanda uygun olduğunuz [ücretsiz Azure KREDİLERİ](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) herhangi bir Azure hizmetinde kullanmak için. 

Ayrıca [Azure Cosmos DB öykünücüsü'nü](local-emulator.md) geliştirmek ve uygulamanız için yerel bir Azure aboneliği oluşturmadan ücretsiz olarak test etmek için. Uygulamanızın Azure Cosmos DB Öykünücüsü’ndeki performansından memnun olduğunuzda bulut üzerinde Azure Cosmos DB hesabı kullanmaya başlayabilirsiniz.

### <a name="how-can-i-get-additional-help-with-azure-cosmos-db"></a>Azure Cosmos DB ile ilgili ek Yardım nasıl alabilirim?

Teknik soru sormak için bu iki soru birine ve forumları yanıtlayın:
* [MSDN forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurecosmosdb)
* [Yığın Taşması](http://stackoverflow.com/questions/tagged/azure-cosmosdb). Yığın taşması soru programlama için idealdir. Sorunuzu olduğundan emin olun [konuyla](https://stackoverflow.com/help/on-topic) ve [soru işaretini kaldırın ve yanıt verilemeyen yapmak mümkün olduğunca çok ayrıntı sağlamak](https://stackoverflow.com/help/how-to-ask). 

Yeni özellikleri istemek için yeni bir istek oluşturmak [Uservoice](https://feedback.azure.com/forums/263030-azure-cosmos-db).

Hesabınızla ilgili bir sorun gidermek için Azure portalda bir [destek isteği](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) oluşturun.

Diğer sorular adresinden ekibine gönderilebilir [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com); ancak bu bir teknik destek diğer adı değildir. 

<a id="try-cosmos-db"></a>
## <a name="try-azure-cosmos-db-subscriptions"></a>Azure Cosmos DB abonelikleri deneyin

Şimdi, abone olmadan, taahhüt vermeden ve ücretsiz bir süre sınırlı Azure Cosmos DB deneyiminin keyfini çıkarabilirsiniz. Azure Cosmos DB'yi deneyin abonelik için kaydolmak için Git [Azure Cosmos DB'yi ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/). Bu abonelik ayrıdır [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/)ve bir Azure ücretsiz deneme sürümü veya bir Azure yanı sıra kullanılabilir Ücretli aboneliği. 

Try Azure Cosmos DB abonelikleri Azure portalında ileri kullanıcı kimliğinizi ile ilişkili diğer abonelikler görünür. 

Azure Cosmos DB'yi deneyin abonelikler için aşağıdaki koşullar geçerlidir:

* SQL, Gremlin (grafik API'si) ve tablo hesaplarını için abonelik başına bir kapsayıcı.
* MongoDB hesabı için abonelik başına en fazla 3 koleksiyonu.
* 10 GB depolama kapasitesi.
* Aşağıdaki genel çoğaltma kullanılabilir [Azure bölgeleri](https://azure.microsoft.com/regions/): Orta ABD, Kuzey Avrupa ve Güneydoğu Asya
* En fazla aktarım hızı 5 K RU/sn.
* Abonelikleri, 24 saat sonra süresi dolacak ve en fazla toplam 48 saat için genişletilebilir.
* Azure Cosmos DB'yi deneyin hesapları için Azure destek bileti oluşturulamıyor; Bununla birlikte, mevcut destek planları ile aboneleri için destek sağlanır. 

## <a name="set-up-azure-cosmos-db"></a>Azure Cosmos DB'yi yedekleyin ayarlayın
### <a name="how-do-i-sign-up-for-azure-cosmos-db"></a>Azure Cosmos DB için nasıl kaydolabilirim?
Azure Cosmos DB, Azure portalında kullanılabilir. İlk olarak, bir Azure aboneliği için kaydolun. Açtıktan sonra Azure aboneliğinizde bir SQL API'si, Graph API, tablo API'si, MongoDB API'si veya Cassandra API hesabı ekleyebilirsiniz.

### <a name="what-is-a-master-key"></a>Ana anahtar nedir?
Ana anahtar, bir hesaptaki tüm kaynaklara erişmeyi sağlayan bir güvenlik belirtecidir. Anahtara sahip kişiler, okuma ve yazma, veritabanı hesabındaki tüm kaynaklara erişimi. Ana anahtarları dağıtırken dikkatli olun. Birincil ana anahtar ve ikincil ana anahtar mevcuttur **anahtarları** dikey [Azure portalında][azure-portal]. Anahtarları hakkında daha fazla bilgi için bkz. [görüntüleme, kopyalama ve yeniden oluşturma erişim anahtarlarını](manage-account.md#keys).

### <a name="what-are-the-regions-that-preferredlocations-can-be-set-to"></a>PreferredLocations ayarlanabilir bölgeleri nelerdir? 
PreferredLocations değeri Cosmos DB kullanılabildiği Azure bölgelerini birine ayarlanabilir. Kullanılabilir bölgelerin listesi için bkz. [Azure bölgeleri](https://azure.microsoft.com/regions/).

### <a name="is-there-anything-i-should-be-aware-of-when-distributing-data-across-the-world-via-the-azure-datacenters"></a>Verileri Azure veri merkezleri aracılığıyla dünya genelinde dağıtırken dikkat etmem bir şey var mı? 
Azure Cosmos DB, mevcut belirtildiği gibi tüm Azure bölgelerinde [Azure bölgeleri](https://azure.microsoft.com/regions/) sayfası. Çekirdek hizmet olduğundan, her yeni bir veri merkezine bir Azure Cosmos DB faaliyet. 

Bir bölge kümesi, Azure Cosmos DB bağımsız ve kamu bulutlarında uyar unutmayın. Diğer bir deyişle, bir hesap oluşturursanız, bir [bağımsız bölge](https://azure.microsoft.com/global-infrastructure/), dışında çoğaltamazsınız [bağımsız bölge](https://azure.microsoft.com/global-infrastructure/). Benzer şekilde, bir dış hesaptan bağımsız diğer konumlara çoğaltma etkinleştirilemiyor. 

### <a name="is-it-possible-to-switch-from-container-level-throughput-provisioning-to-database-level-throughput-provisioning-or-vice-versa"></a>Kapsayıcı düzeyi akışından geçmek mümkün veritabanı düzeyi aktarım hızı sağlama için sağlanıyor? Ya da tam tersi

Kapsayıcı ve veritabanı düzeyi aktarım hızı sağlama ayrı teklifleri ve ya da bunların arasında geçiş gerektiren geçirme kaynaktan hedef veri. Yeni bir veritabanı veya yeni bir koleksiyon oluşturun ve ardından verileri kullanarak geçirmek için anlamına gelir [toplu Yürütücü Kitaplığı](bulk-executor-overview.md) veya [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md).


## <a name="develop-against-the-sql-api"></a>SQL API'si ile programlama geliştirin

### <a name="how-do-i-start-developing-against-the-sql-api"></a>SQL API'si ile programlama geliştirmeye nasıl başlarım?
Öncelikle, bir Azure aboneliği için kaydolmalısınız. Bir Azure aboneliğine kaydolduktan sonra Azure aboneliğinizde bir SQL API'si kapsayıcı ekleyebilirsiniz. Bir Azure Cosmos DB hesabını ekleme ile ilgili yönergeler için bkz: [bir Azure Cosmos DB veritabanı hesabı oluşturma](create-sql-api-dotnet.md#create-account). 

[SDK'ları](sql-api-sdk-dotnet.md) .NET, Python, Node.js, JavaScript ve Java için kullanılabilir. Geliştiriciler ayrıca kullanabileceğiniz [RESTful HTTP API'lerini](/rest/api/cosmos-db/) çeşitli platformlar ve diller Azure Cosmos DB kaynaklarıyla etkileşim kurmak için.

### <a name="can-i-access-some-ready-made-samples-to-get-a-head-start"></a>Avantajlı bir başlangıç için bazı kullanıma hazır örnekler erişebilirim?
SQL API'si için örnekleri [.NET](sql-api-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](sql-api-nodejs-samples.md), ve [Python](sql-api-python-samples.md) SDK'ları Github'da bulunmaktadır.


### <a name="does-the-sql-api-database-support-schema-free-data"></a>SQL API'si veritabanı şemasız verileri destekler mi?
Evet, SQL API'si, şema tanımları veya ipuçları olmadan rastgele JSON belgelerinin depolamak uygulamalar sağlar. Veriler, Azure Cosmos DB SQL sorgu arabirimi yoluyla sorgu için hemen kullanılabilir.  

### <a name="does-the-sql-api-support-acid-transactions"></a>SQL API'si, ACID işlemlerini destekler mi?
Evet, SQL API'si, JavaScript saklı yordamları ve Tetikleyicileri olarak ifade edilen belgeler arası işlemleri destekler. İşlemler her bir kapsayıcı içindeki tek bir bölüm için kapsamlı ve "tümü veya hiçbiri," olarak, ACID semantiği ile yürütülen diğer eş zamanlı yürütme kodları ve kullanıcı isteklerinden. Sunucu tarafı JavaScript uygulama kodunun yürütülmesini özel durumlar oluşursa işlemin tümü geri alınır. İşlemler hakkında daha fazla bilgi için bkz. [programı işlemleri veritabanına](programming.md#database-program-transactions).

### <a name="what-is-a-container"></a>Bir kapsayıcı nedir?
Bir grup belgeleri ve bunların ilişkili JavaScript uygulama mantığının bir kapsayıcıdır. Faturalanabilir bir varlık kapsayıcıdır burada [maliyet](performance-levels.md) kullanılan depolama ve aktarım hızı tarafından belirlenir. Kapsayıcılar, bir veya daha fazla bölümleri veya sunucuları kapsayabilir ve neredeyse sınırsız miktarda depolama veya işlemeyi işleyebilecek şekilde ölçeklendirilebilir. 

* SQL ve MongoDB API hesabı, kapsayıcı bir koleksiyona eşler. 
* Cassandra ve tablo API'si hesapları için bir kapsayıcı bir tabloya eşler. 
* Gremlin API hesapları için bir kapsayıcı için bir grafik eşler. 

Ayrıca Azure Cosmos DB için fatura varlıkları kapsayıcılardır. Her kapsayıcı, sağlanan aktarım hızına göre saatlik olarak faturalandırılır ve da kullanılan depolama alanı. Daha fazla bilgi için [Azure Cosmos DB fiyatlandırma](https://azure.microsoft.com/pricing/details/cosmos-db/). 

### <a name="how-do-i-create-a-database"></a>Veritabanı nasıl oluşturulur?
Kullanarak veritabanları oluşturabilirsiniz [Azure portalında](https://portal.azure.com)anlatılan şekilde [koleksiyon ekleyin](create-sql-api-dotnet.md#create-collection), bir'ın [Azure Cosmos DB SDK'ları](sql-api-sdk-dotnet.md), veya [REST API'leri](/rest/api/cosmos-db/). 

### <a name="how-do-i-set-up-users-and-permissions"></a>Kullanıcıları ve izinleri nasıl ayarlarım?
Kullanıcıları ve izinleri birini kullanarak oluşturabileceğiniz [Cosmos DB API SDK'ları](sql-api-sdk-dotnet.md) veya [REST API'leri](/rest/api/cosmos-db/).  

### <a name="does-the-sql-api-support-sql"></a>SQL API'si SQL destekliyor mu?
SQL API hesabı tarafından desteklenen SQL sorgu dili, SQL Server tarafından desteklenen sorgu işlevselliği Gelişmiş bir alt kümesidir. Azure Cosmos DB SQL sorgu dili, zengin hiyerarşik ve ilişkisel işleçler ve JavaScript tabanlı, kullanıcı tanımlı işlevler (UDF'ler) aracılığıyla genişletilebilirlik sağlar. JSON dil bilgisi, JSON belgelerini hem Azure Cosmos DB otomatik dizin oluşturma teknikleri hem de Azure Cosmos DB SQL sorgu diyalekti tarafından kullanılan etiketli düğümler etiketli modelleme sağlar. SQL dil bilgisi kullanma hakkında daha fazla bilgi için bkz: [SQL sorgusu] [ query] makalesi.

### <a name="does-the-sql-api-support-sql-aggregation-functions"></a>SQL API'si SQL toplama işlevleri destekliyor mu?
Toplama işlevleri aracılığıyla herhangi bir ölçekte düşük gecikme süreli toplama SQL API'sini destekleyen `COUNT`, `MIN`, `MAX`, `AVG`, ve `SUM` SQL dil bilgisi aracılığıyla. Daha fazla bilgi için [toplama işlevleri](sql-api-sql-query.md#Aggregates).

### <a name="how-does-the-sql-api-provide-concurrency"></a>SQL API'si eşzamanlılığı nasıl sağlar?
SQL API'si, HTTP varlık etiketleri veya Etag'ler aracılığıyla iyimser eşzamanlılık denetimini (OCC) destekler. Her bir SQL API'si kaynakları bir Etag'e sahiptir ve bir belgeyi her güncelleştirildiğinde ETag sunucu üzerinde ayarlanır. ETag üstbilgi ve geçerli değeri, tüm yanıt iletilerine dahil edilir. Etag'ler IF-Match üst bilgisi bir kaynağın güncelleştirilmesi gerekip gerekmediğini karar vermek sunucu izin vermek için kullanılabilir. IF-Match karşı denetlenmesi için ETag değeri değerdir. ETag değeri, sunucunun ETag değeri eşleşirse, kaynak güncelleştirilir. ETag geçerli ise sunucu işlemi reddetti. bir "HTTP 412 önkoşul hatası" yanıt kodu. Ardından istemci kaynak için geçerli ETag değeri almak için kaynak yeniden getirir. Ayrıca, Etag'ler If-None-Match üst bilgisi ile bir kaynak yeniden getirmek gerekli olup olmadığını belirlemek için kullanılabilir.

. NET'te iyimser eşzamanlılık kullanmak için [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) sınıfı. Bir .NET örneği için bkz. [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) DocumentManagement örneğindeki GitHub üzerinde.

### <a name="how-do-i-perform-transactions-in-the-sql-api"></a>SQL API'SİNDE nasıl işlemleri yaptınız?
SQL API'si, JavaScript saklı yordamları ve Tetikleyicileri aracılığıyla dil ile tümleşik işlemleri destekler. Betiklerin içindeki tüm veritabanı işlemleri, anlık görüntü yalıtımı altında yürütülür. Tek bölümlü bir koleksiyon olduğundan, yürütme koleksiyona kapsamlıdır. Koleksiyon bölümlere ayrılmışsa, yürütme koleksiyondaki aynı bölüm anahtarı değerine sahip belgelerde kapsamlıdır. Belge sürümlerinin anlık görüntüsü (ETag'ler) ise işlem başlangıcında alınır ve yalnızca betik başarılı olursa uygulanır. JavaScript bir hata oluşturursa işlem geri alınır. Daha fazla bilgi için [Azure Cosmos DB için sunucu tarafı JavaScript programlama](programming.md).

### <a name="how-can-i-bulk-insert-documents-into-cosmos-db"></a>Nasıl miyim toplu belgeleri Cosmos DB'ye ekleme?
Toplu belgeleri Azure Cosmos DB'ye aşağıdaki yollardan biriyle ekleme:

* Bölümünde anlatıldığı gibi toplu Yürütücü aracı [kullanarak toplu Yürütücü .NET kitaplığı](bulk-executor-dot-net.md) ve [kullanarak toplu Yürütücü Java kitaplığı](bulk-executor-java.md)
* Bölümünde anlatıldığı gibi veri geçiş aracı [Azure Cosmos DB için veritabanı geçiş aracı](import-data.md).
* Saklı yordamlar, açıklandığı [Azure Cosmos DB için sunucu tarafı JavaScript programlama](programming.md).

### <a name="i-have-setup-my-container-to-use-lazy-indexing-i-see-that-my-queries-do-not-return-expected-results"></a>Kurulum yavaş dizinleme kullanılacak my kapsayıcı sahibim, Sorgularım beklenen sonuçları döndürmeyen görüyorum. 
Yavaş dizin, dizin oluşturma bölümünde açıklandığı gibi bu davranışlara neden olabilir. Tüm uygulamalar için tutarlı bir dizin her zaman kullanmalısınız. 


### <a name="does-the-sql-api-support-resource-link-caching"></a>SQL API desteği kaynak bağlantıyı önbelleğe almayı mu?
Evet, Azure Cosmos DB bir RESTful hizmeti olduğu için kaynak bağlantıları sabittir ve önbelleğe alınabilir. SQL API istemciler, herhangi bir kaynak gibi belge veya koleksiyon yapılan okumalar için bir "If-None-Match" üst bilgisi belirtin ve sunucu sürümü değiştikten sonra kendi yerel kopyalarını güncelleştirin.

### <a name="is-a-local-instance-of-sql-api-available"></a>SQL API'si, yerel bir örnek var mı?
Evet. [Azure Cosmos DB öykünücüsü'nü](local-emulator.md) bir Cosmos DB hizmetinin yüksek kaliteli öykünmesi sağlar. Azure Cosmos oluşturmak ve JSON belgelerini sorgulamak için destek dahil olmak üzere sağlama DB'ye, aynı işlevselliği destekler ve saklı yordamları ve Tetikleyicileri koleksiyonların ölçeklendirilmesiyle ve yürütme. Geliştirme ve Azure Cosmos DB öykünücüsü'nü kullanarak uygulamaları test etmek ve tek bir yapılandırma için Azure Cosmos DB bağlantı uç noktası için değişiklik yaparak global bir ölçekte Azure'a dağıtın.

### <a name="why-are-long-floating-point-values-in-a-document-rounded-when-viewed-from-data-explorer-in-the-portal"></a>Neden uzun kayan nokta belgeye portalında Veri Gezgini görüntülendiğinde yuvarlanmış değerler. 
Bu JavaScript sınırlamasıdır. JavaScript IEEE 754 belirtildiği gibi çift duyarlıklı kayan nokta biçimi numaraları kullanır ve sayı - arasında güvenli bir şekilde temsil edebilir (253 - 1) ve 253-1 (yani, 9007199254740991) yalnızca.

### <a name="where-are-permissions-allowed-in-the-object-hierarchy"></a>Burada izinleri nesne hiyerarşisinde izin veriliyor mu?

İzinler ResourceTokens kullanarak oluşturma kapsayıcı düzeyinde ve alt öğelerini (örneğin, belgeler, ekleri) izin verilir. Bu veritabanını bir izin oluşturmak çalışan gelir veya hesap düzeyi şu anda verilmez.


## <a name="develop-against-the-api-for-mongodb"></a>MongoDB için API karşı geliştirin
### <a name="what-is-the-azure-cosmos-db-api-for-mongodb"></a>Azure Cosmos DB MongoDB için API nedir?
MongoDB için Azure Cosmos DB API uygulamaları kolayca ve şeffaf bir şekilde var olan ve topluluk tarafından desteklenen Apache MongoDB API'leri ve sürücüleri kullanarak yerel Azure Cosmos DB veritabanı altyapısı ile iletişim kurmasına olanak sağlayan bir uyumluluk katmanıdır. Geliştiriciler artık, Azure Cosmos DB yararlanan uygulamalar oluşturmak için var olan MongoDB takımlarına ve becerileri kullanabilirsiniz. Otomatik dizin oluşturma işlemi, yedekleme bakım, finansal destekli bir hizmet düzeyi sözleşmeleri (SLA'lar) vb. içeren benzersiz özelliklerinden Azure Cosmos DB, geliştiricilerin yararlanır.

### <a name="how-do-i-connect-to-my-api-for-mongodb-database"></a>MongoDB veritabanı benim API'sine nasıl bağlanabilirim?
MongoDB için baş üzerinden için Azure Cosmos DB API'sine bağlanmak için en hızlı yolu [Azure portalında](https://portal.azure.com). Hesabınıza gidin ve ardından, sol gezinti menüsünde **Hızlı Başlangıç**. Hızlı Başlangıç, veritabanınıza bağlanmak için kod parçacıkları almak için en iyi bir yoludur. 

Azure Cosmos DB, katı güvenlik gereksinimleri ve standartları zorunlu kılar. Azure Cosmos DB hesapları kimlik doğrulaması ve SSL ile güvenli bağlantı gerekir, bu nedenle TLSv1.2 kullandığınızdan emin olun.

Daha fazla bilgi için [API'nize MongoDB veritabanına bağlan](connect-mongodb-account.md).

### <a name="are-there-additional-error-codes-for-an-api-for-mongodb-database"></a>MongoDB veritabanı için bir API için ek hata kodları vardır?
Ek olarak ortak MongoDB hata kodları, MongoDB API'sini kendi belirli hata kodlarıyla sahiptir:


| Hata               | Kod  | Açıklama  | Çözüm  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | İstek birimleri tüketilen toplam sayısı, koleksiyon için sağlanan istek birimi oranını aşmış ve kısıtlanan. | Bir kapsayıcı veya bir dizi kapsayıcılar için Azure portal veya deneniyor yeniden atanan aktarım hızını ölçeklendirmeyi düşünün. |
| ExceededMemoryLimit | 16501 | Çok kiracılı bir hizmet, istemcinin bellek tahsisat işlem eşiğini aştı. | Destek ile iletişime geçin veya daha kısıtlayıcı bir sorgu ölçütü ile bir işlem kapsamını azaltın [Azure portalında](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). <br><br>Örnek:  *&nbsp; &nbsp; &nbsp; &nbsp;db.getCollection('users').aggregate ([<br> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;{$match: {adı: "Andy"}}, <br> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;{$sort: {yaş: -1} }<br>&nbsp;&nbsp;&nbsp;&nbsp;])*) |

## <a name="develop-with-the-table-api"></a>Tablo API'si ile geliştirme

### <a name="how-can-i-use-the-table-api-offering"></a>Tablo API'si teklifi nasıl kullanabilirim? 
Azure Cosmos DB tablo API'si kullanılabilir [Azure portalında][azure-portal]. Öncelikle, bir Azure aboneliği için kaydolmalısınız. Açtıktan sonra Azure Aboneliğinize bir Azure Cosmos DB tablo API'si hesabı eklemek ve ardından tabloları hesabınıza ekleyin. 

Desteklenen diller ve ilişkili hızlı başlangıçlar, bulabilirsiniz [Azure Cosmos DB tablo API'sine giriş](table-introduction.md).

### <a name="do-i-need-a-new-sdk-to-use-the-table-api"></a>Tablo API'sini kullanmak için yeni bir SDK ihtiyacım var? 
Hayır, var olan depolama SDK'ları çalışması gerekir. Bir her zaman en son SDK'ları için en iyi destek ve çoğu durumda üstün performans alır, ancak önerilir. Kullanılabilir diller listesini [Azure Cosmos DB tablo API'sine giriş](table-introduction.md).

### <a name="where-is-table-api-not-identical-with-azure-table-storage-behavior"></a>Burada tablo API'si ile Azure tablo depolama davranışı aynı değildir?
Azure Cosmos DB tablo API'si ile tablolar oluşturmak istediğiniz Azure tablo Depolama'dan gelen kullanıcılar bilincinde olmanız gereken bazı davranış farkları vardır:

* Azure Cosmos DB tablo API'si, garantili performans sağlamak için ayrılmış bir kapasite modeli kullanır. ancak bu tablo oluşturulduktan hemen sonra kapasite kullanılmıyor olsa bile bir kapasite için ödeme yaptığını anlamına gelir. Azure tablo depolama ile bir yalnızca gerçekten kullanılan kapasiteyi öder. Azure tablo depolama 10 ikinci bir SLA sunarken bu 10 MS'den okuma tablo API'si neden sunabilir açıklamak için yardımcı olur ve 15 MS'nin SLA 99. yüzdebirlik dilimde yazın. Ancak cihazını Maliyet para kapasite SLA onlara tüm istekleri işlemek kullanılabilir olduğundan emin olmak için tüm istekleri olmadan bile boş tablolar ile tablo API'si tablolar, Azure Cosmos DB tarafından sunulan.
* Azure tablo Depolama'da olduğu gibi tablo API'si tarafından döndürülen sorgu sonuçları bölüm anahtarı/satır anahtarı sırasına sıralı değildir.
* Satır anahtarı yalnızca en fazla 255 bayt olabilir
* Toplu işlemleri yalnızca en fazla 2 MB içerebilir
* CORS şu anda desteklenmiyor
* Azure tablo depolama tablo adları büyük küçük harfe duyarlı değildir, ancak bunlar Azure Cosmos DB tablo API'si
* Azure Cosmos DB'nin iç biçimleri gibi ikili alanlar kodlama bilgi için bazıları şu anda bir dilediğiniz kadar verimli değildir. Bu nedenle bu beklenmeyen sınırlamaları veri boyutuna göre neden olabilir. Örneğin, şu anda bir tablo varlığın tam 1 MB kodlama veri boyutu artar çünkü ikili verileri depolamak için kullanamadık.
* Varlık özellik adı 'Id' şu anda desteklenmiyor
* TableQuery TakeCount 1000 sınırlı değildir.

REST API açısından birçok Azure Cosmos DB tablo API'si tarafından desteklenmeyen uç noktalar/sorgu seçenek vardır:
| REST yöntemleri | REST uç noktası/sorgu seçeneği | Belge URL'leri | Açıklama |
| ------------| ------------- | ---------- | ----------- |
| YERLEŞTİRME, ALIN | /? restype =service@compözellikleri| [Tablo hizmeti özelliklerini ayarla](https://docs.microsoft.com/rest/api/storageservices/set-table-service-properties) ve [tablo hizmeti özelliklerini alma](https://docs.microsoft.com/rest/api/storageservices/get-table-service-properties) | Bu uç nokta, CORS kuralları, depolama Analizi Yapılandırması ve günlüğe kaydetme ayarlarını ayarlamak için kullanılır. Analiz ve günlüğe kaydetme daha Azure depolama tabloları, Azure Cosmos DB'de farklı işlenme ve CORS şu anda desteklenmiyor |
| SEÇENEKLER | / < resource-tablo-adı > | [Uçuş öncesi CORS tablo isteği](https://docs.microsoft.com/rest/api/storageservices/preflight-table-request) | Bu Azure Cosmos DB şu anda desteklemediği CORS bir parçasıdır. |
| GET | /? restype =service@compistatistikleri = | [Tablo hizmeti istatistikleri alın](https://docs.microsoft.com/rest/api/storageservices/get-table-service-stats) | Birincil ve ikincil veritabanı arasında verilerin çoğaltılması ne kadar hızlı bilgi sağlar. Çoğaltma yazma parçası olarak bu Cosmos DB'de gerekli değildir. |
| YERLEŞTİRME, ALIN | /mytable?comp=acl | [Tablodaki ACL](https://docs.microsoft.com/rest/api/storageservices/get-table-acl) ve [ACL tablosu ayarlama](https://docs.microsoft.com/rest/api/storageservices/set-table-acl) | Bu, alır ve paylaşılan erişim imzaları (SAS) yönetmek için kullanılan saklı erişim ilkeleri ayarlar. SAS karşın, bunlar ayarlayın ve farklı bir şekilde yönetilebilir. |

Buna ek olarak Azure Cosmos DB tablo API'si, JSON biçiminde değil ATOM yalnızca destekler.

Azure Cosmos DB desteklese de paylaşılan erişim imzaları (SAS) var. bunu desteklemiyor, belirli özel olarak bu yeni tablolar oluşturmak üzere sağa gibi yönetim işlemlerini ilgili ilkeleridir.

.NET SDK'sı için özellikle var. bazı sınıflar ve Azure Cosmos DB şu anda desteklemediği yöntemleri

| Sınıf | Desteklenmeyen yöntemi |
|-------|-------- |
| CloudTableClient | \*ServiceProperties * |
|                  | \*ServiceStats * |
| CloudTable | İzinleri Ayarla * |
|            | GetPermissions * |
| TableServiceContext | * (Bu sınıf, aslında bırakılmıştır) |
| TableServiceEntity | " " |
| TableServiceExtensions | " " |
| TableServiceQuery | " " |

Bu farklılıklar, projeniz için bir sorun varsa lütfen iletişime geçin [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com) ve bunu bize bildirin.

### <a name="how-do-i-provide-feedback-about-the-sdk-or-bugs"></a>Geri bildirim SDK'sı veya hatalar hakkında nasıl sağlar?
Geri bildiriminiz aşağıdaki yollardan biriyle paylaşabilirsiniz:

* [Uservoice](https://feedback.azure.com/forums/599062-azure-cosmos-db-table-api)
* [MSDN forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurecosmosdb)
* [Yığın Taşması](http://stackoverflow.com/questions/tagged/azure-cosmosdb). Yığın taşması soru programlama için idealdir. Sorunuzu olduğundan emin olun [konuyla](https://stackoverflow.com/help/on-topic) ve [soru işaretini kaldırın ve yanıt verilemeyen yapmak mümkün olduğunca çok ayrıntı sağlamak](https://stackoverflow.com/help/how-to-ask).

### <a name="what-is-the-connection-string-that-i-need-to-use-to-connect-to-the-table-api"></a>Bağlantı dizesini tablo API'sine bağlanmak için kullanılacak ihtiyacım nedir?
Bağlantı dizesidir:
```
DefaultEndpointsProtocol=https;AccountName=<AccountNamefromCosmos DB;AccountKey=<FromKeysPaneofCosmosDB>;TableEndpoint=https://<AccountName>.table.cosmosdb.azure.com
```
Bağlantı dizesini Azure portalında bağlantı dizesi sayfasından alabilirsiniz. 

### <a name="how-do-i-override-the-config-settings-for-the-request-options-in-the-net-sdk-for-the-table-api"></a>Tablo API'si için .NET SDK'sı istek seçenekleri yapılandırma ayarlarını nasıl geçersiz?
Yapılandırma ayarları hakkında daha fazla bilgi için bkz. [Azure Cosmos DB özellikleri](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). Bazı ayarlar Createcloudtableclient'a yöntemi ve diğer istemci uygulamasındaki appSettings bölümündeki app.config aracılığıyla işlenir.

### <a name="are-there-any-changes-for-customers-who-are-using-the-existing-azure-table-storage-sdks"></a>Mevcut Azure tablo depolama SDK'larını kullanan müşteriler için herhangi bir değişiklik vardır?
Yok. Mevcut Azure tablo depolama SDK'ları kullanarak mevcut veya yeni müşteriler için bir değişiklik bulunmamaktadır. 

### <a name="how-do-i-view-table-data-that-is-stored-in-azure-cosmos-db-for-use-with-the-table-api"></a>Tablo API'si ile kullanmak için Azure Cosmos DB'de depolanan tablo verilerini nasıl görüntüleyebilirim? 
Veri göz atmak için Azure portalını kullanabilirsiniz. Tablo API'si kod veya sonraki yanıt bahsedilen araçlarını kullanabilirsiniz. 

### <a name="which-tools-work-with-the-table-api"></a>Hangi araçları, tablo API'si ile çalışıyor? 
Kullanabileceğiniz [Azure Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).

Belirtilen biçimde bir bağlantı dizesi esnekliğine araçlarla daha önce yeni tablo API'sini destekler. Tablo araçları listesi üzerinde sağlanan [Azure depolama istemci araçları](../storage/common/storage-explorers.md) sayfası. 

### <a name="is-the-concurrency-on-operations-controlled"></a>Eşzamanlılık işlemleri kontrol mi?
Evet, iyimser eşzamanlılık ETag mekanizmasının kullanımı sağlanır. 

### <a name="is-the-odata-query-model-supported-for-entities"></a>OData sorgu modeli, varlıklar için destekleniyor mu? 
Evet, OData sorgu ve LINQ sorgusu tablo API'sini destekler. 

### <a name="can-i-connect-to-azure-table-storage-and-azure-cosmos-db-table-api-side-by-side-in-the-same-application"></a>Azure tablo depolama ve Azure Cosmos DB tablo API'si aynı uygulamada yan yana bağlanabilir miyim? 
Evet, her bağlantı dizesi aracılığıyla kendi URI'sine işaret CloudTableClient iki ayrı örneklerini oluşturarak bağlanabilirsiniz.

### <a name="how-do-i-migrate-an-existing-azure-table-storage-application-to-this-offering"></a>Bu teklife mevcut Azure tablo depolama uygulamanın nasıl geçirebilirim?
[AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy) ve [Azure Cosmos DB veri geçiş aracı](import-data.md) desteklenen olan iki.

### <a name="how-is-expansion-of-the-storage-size-done-for-this-service-if-for-example-i-start-with-n-gb-of-data-and-my-data-will-grow-to-1-tb-over-time"></a>Örneğin miyim ile başlatın, bu hizmet için yapılan depolama boyutunu genişletme nasıl olduğunu *n* GB veri ve verilerimi zaman içinde 1 TB ile büyütün? 
Azure Cosmos DB yatay ölçekleme kullanım aracılığıyla sınırsız depolama sağlamak için tasarlanmıştır. Hizmet, izleyin ve etkili bir şekilde depolamanızı artırın. 

### <a name="how-do-i-monitor-the-table-api-offering"></a>Tablo API'si teklifi nasıl izleyebilirim?
Tablo API'sini kullanabilirsiniz **ölçümleri** istekleri ve depolama alanı kullanımı izleme bölmesinde. 

### <a name="how-do-i-calculate-the-throughput-i-require"></a>Nasıl gerekli kılarım aktarım hızını hesaplar?
Kapasite estimator işlemler için gerekli olan TableThroughput hesaplamak için kullanabilirsiniz. Daha fazla bilgi için [tahmin istek birimleri ve veri depolama](https://www.documentdb.com/capacityplanner). Genel olarak, JSON olarak, bir varlığı temsil eder ve sayıları, işlemleri için sağlama. 

### <a name="can-i-use-the-table-api-sdk-locally-with-the-emulator"></a>Öykünücü ile tablo API'si SDK'yı yerel olarak kullanabilirim?
Şu anda değil.

### <a name="can-my-existing-application-work-with-the-table-api"></a>Uygulamam var olan tablo API'si ile çalışabilir mi? 
Evet, aynı API'yi desteklenir.

### <a name="do-i-need-to-migrate-my-existing-azure-table-storage-applications-to-the-sdk-if-i-do-not-want-to-use-the-table-api-features"></a>Tablo API'si özellikleri kullanmak istemediğiniz SDK'sı için mevcut Azure tablo depolama uygulamalarımı geçirmeniz gerekiyor mu?
Hayır, oluşturabilir ve mevcut Azure tablo depolama varlıkları herhangi bir türde kesinti olmadan kullanın. Ancak, tablo API'si kullanmıyorsanız, otomatik dizin, ek tutarlılık seçeneği veya genel dağıtım fayda sağlayamaz. 

### <a name="how-do-i-add-replication-of-the-data-in-the-table-api-across-multiple-regions-of-azure"></a>Nasıl veri çoğaltma işlemi tablo API'SİNDE Azure bölgelerinde ekleyebilirim?
Azure Cosmos DB portal'ın kullanabileceğiniz [genel çoğaltma ayarları](tutorial-global-distribution-sql-api.md#portal) uygulamanız için uygun olan bölgeler eklenecek. Global olarak dağıtılmış bir uygulama geliştirmek için okuma gecikme süresi düşük sağlamak için yerel bölge kümesi PreferredLocation bilgilerle uygulamanızı eklemelisiniz. 

### <a name="how-do-i-change-the-primary-write-region-for-the-account-in-the-table-api"></a>Tablo API'si hesabı için birincil yazma bölgesini nasıl değiştirebilirim?
Azure Cosmos DB küresel çoğaltma portalındaki bölmesi, bir bölge eklemek ve ardından gerekli bölgeye yük devretmek için kullanabilirsiniz. Yönergeler için [çok bölgeli Azure Cosmos DB hesapları ile geliştirme](regional-failover.md). 

### <a name="how-do-i-configure-my-preferred-read-regions-for-low-latency-when-i-distribute-my-data"></a>Verilerim dağıtabilirim, düşük gecikme süresi için tercih edilen my okuma bölgeleri nasıl yapılandırabilirim? 
Yerel konum yardımcı olmak için app.config dosyasında PreferredLocation anahtarı kullanın. LocationMode ayarlanmışsa var olan uygulamalar için tablo API'si bir hata oluşturur. Tablo API'si bu bilgileri app.config dosyasından alır çünkü bu kodu kaldırın. Daha fazla bilgi için [Azure Cosmos DB özellikleri](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="how-should-i-think-about-consistency-levels-in-the-table-api"></a>Tablo API'si tutarlılık düzeyleri hakkında ne düşünüyorsunuz? 
Azure Cosmos DB tutarlılık, kullanılabilirlik ve gecikme süresi arasında iyi reasoned dengelemeler sağlar. Tablo düzeyinde doğru tutarlılık modelini seçin ve verileri sorgularken bireysel isteğinde bulunmak için azure Cosmos DB tablo API'si geliştiriciler için beş tutarlılık düzeyi sunar. Bir istemci bağlandığında, tutarlılık düzeyi belirleyebilirsiniz. Createcloudtableclient'a consistencyLevel bağımsız değişkeni aracılığıyla düzeyini değiştirebilirsiniz. 

Tablo API'si ile "kendi yazma sınırlanmış eskime durumu tutarlılık varsayılan olarak okuma" Düşük gecikmeli okuma sağlar. Daha fazla bilgi için [tutarlılık düzeyleri](consistency-levels.md). 

Varsayılan olarak, Azure tablo depolama, bir bölge içinde güçlü tutarlılık ve ikincil konumda nihai tutarlılık sağlar. 

### <a name="does-azure-cosmos-db-table-api-offer-more-consistency-levels-than-azure-table-storage"></a>Azure Cosmos DB tablo API'si, Azure tablo Depolama'den daha fazla tutarlılık düzeyi sunar?
Evet, Azure Cosmos DB dağıtılmış yapısını Avantajı hakkında daha fazla bilgi için bkz. [tutarlılık düzeyleri](consistency-levels.md). Tutarlılık düzeyleri için garanti sağlandığından, bunları güvenle kullanabilirsiniz. Daha fazla bilgi için [Azure Cosmos DB özellikleri](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="when-global-distribution-is-enabled-how-long-does-it-take-to-replicate-the-data"></a>Genel dağıtım etkinleştirildiğinde, ne kadar veri çoğaltmak için sürer?
Azure Cosmos DB yerel bölgede büyük veri işleme ve diğer bölgelere hemen birkaç milisaniye verileri gönderir. Bu çoğaltma, yalnızca gidiş geliş saati datacenter'ın (RTT) bağlıdır. Azure Cosmos DB'nin genel dağıtım özelliği hakkında daha fazla bilgi için bkz. [Azure Cosmos DB: Azure üzerindeki bir Global olarak dağıtılmış veritabanı hizmeti](distribute-data-globally.md).

### <a name="can-the-read-request-consistency-level-be-changed"></a>Okuma isteği tutarlılık düzeyi değiştirilebilir mi?
Azure Cosmos DB ile kapsayıcı düzeyinde tutarlılık düzeyi (tablo) ayarlayabilirsiniz. .NET SDK kullanarak, app.config dosyasında TableConsistencyLevel anahtar için değeri sağlayarak düzeyini değiştirebilirsiniz. Olası değerler şunlardır: güçlü, sınırlanmış eskime durumu, oturum, tutarlı ön ek ve nihai. Daha fazla bilgi için [Azure Cosmos DB'de veri ayarlanabilir tutarlılık düzeyleri](consistency-levels.md). İstek tutarlılık birden çok tablo ayarı en düzeyinde ayarlayamazsınız, anahtar olur. Örneğin, nihai ve istek tutarlılık düzeyi güçlü tutarlılık düzeyi tablosu için ayarlanamıyor. 

### <a name="how-does-the-table-api-handle-failover-if-a-region-goes-down"></a>Bir bölgede bir arıza yaşanırsa nasıl tablo API'si, yük devretme işliyor? 
Tablo API'si, Azure Cosmos DB Global olarak dağıtılmış platformu yararlanır. Uygulamanızın veri merkezi kapalı kalma süresi etkilenmemesini sağlamak için Azure Cosmos DB portalında hesabı için en az bir daha fazla bölge etkinleştirme [çok bölgeli Azure Cosmos DB hesapları ile geliştirme](regional-failover.md). Portalı kullanarak bölgenin önceliğini ayarlayabilirsiniz [çok bölgeli Azure Cosmos DB hesapları ile geliştirme](regional-failover.md). 

Hesabınız için istediğiniz ve burada bunun için bir yük devretme öncelik sağlayarak devredebilir denetlemek çok bölgesini ekleyebilirsiniz. Elbette, veritabanını kullanmak üzere bir uygulamayı çok vermeniz gerekir. Bunu yaptığınızda, müşterilerinizin kesintiyle karşılaşmayacaksınız. [En son .NET İstemci SDK'sı](table-sdk-dotnet.md) otomatik olan giriş ancak diğer SDK'lar değildir. Diğer bir deyişle, kapalı ve otomatik olarak yeni bölgeye yük devretmek bölge tespit edebilirsiniz.

### <a name="is-the-table-api-enabled-for-backups"></a>Tablo API'si, yedeklemeler için etkin mi?
Evet, Azure Cosmos DB platformunun yedeklemeler için tablo API'sini kullanır. Yedeklemeleri otomatik olarak yapılır. Daha fazla bilgi için [çevrimiçi yedekleme ve geri yükleme Azure Cosmos DB ile](online-backup-and-restore.md).

 
### <a name="does-the-table-api-index-all-attributes-of-an-entity-by-default"></a>Tablo API'si, bir varlığın tüm öznitelikleri varsayılan dizini?
Evet, bir varlığın tüm öznitelikleri varsayılan olarak dizine eklenir. Daha fazla bilgi için [Azure Cosmos DB: dizin oluşturma ilkeleri](indexing-policies.md). 

### <a name="does-this-mean-i-do-not-have-to-create-multiple-indexes-to-satisfy-the-queries"></a>Ben zorunda değilsiniz sorguları gerçekleştirmek için birden çok dizin oluşturma anlamına mı geliyor? 
Evet, Azure Cosmos DB tablo API'si, herhangi bir şema tanımı olmadan tüm özniteliklerin otomatik dizin oluşturma sağlar. Bu Otomasyon odak dizin oluşturma ve yönetim yerine uygulama geliştiricilerine serbest bırakır. Daha fazla bilgi için [Azure Cosmos DB: dizin oluşturma ilkeleri](indexing-policies.md).

### <a name="can-i-change-the-indexing-policy"></a>Dizin oluşturma ilkesini değiştirebilirim?
Evet, dizin tanımını sağlayarak dizin oluşturma ilkesini değiştirebilirsiniz. Daha fazla bilgi için [Azure Cosmos DB özellikleri](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). Düzgün bir şekilde kodlama ve ayarları kaçış gerekir. 

olmayan - .NET SDK'ları için dizin oluşturma ilkesini adresindeki portalında yalnızca ayarlanabilir **Veri Gezgini**, değiştirin ve ardından gitmek istediğiniz belirli tablosuna gidin **ölçek ve ayarlar**dizin oluşturma ilkesi -> İstenen değişikliği yapın ve ardından **Kaydet**.

.NET SDK'sından app.config dosyasında gönderilebilir:
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

### <a name="azure-cosmos-db-as-a-platform-seems-to-have-lot-of-capabilities-such-as-sorting-aggregates-hierarchy-and-other-functionality-will-you-be-adding-these-capabilities-to-the-table-api"></a>Azure Cosmos DB platformu olarak yetenekleri, sıralama, toplamalar, hiyerarşi ve diğer işlevleri gibi pek çok gibi görünüyor. Tablo API'si için bu özellikler ekliyor olacağız? 
Tablo API'si, Azure tablo depolaması aynı sorgu işlevlerini sağlar. Azure Cosmos DB ayrıca sıralama, toplamalar, jeo-uzamsal sorgu, hiyerarşi ve çok çeşitli yerleşik işlevleri de destekler. Gelecekteki bir hizmet güncelleştirmesinde tablo API'SİNDE ek işlevsellik sağlayacağız. Daha fazla bilgi için [SQL sorguları](sql-api-sql-query.md).
 
### <a name="when-should-i-change-tablethroughput-for-the-table-api"></a>Tablo API'si için ne zaman TableThroughput değiştirebilirim?
Aşağıdaki koşullardan biri geçerli olduğu durumlarda TableThroughput değiştirmeniz gerekir:
* Bir ayıklama, dönüştürme ve yükleme (ETL) veri gerçekleştiriyorsunuz veya kısa bir sürede çok sayıda veri karşıya yüklemek istediğiniz. 
* Kapsayıcı veya bir dizi kapsayıcıları arka uçta daha fazla aktarım hızı gerekir. Örneğin, kullanılan aktarım hızını sağlanan aktarım hızı büyük ve kaynaklarınızın azaltılıp görürsünüz. Daha fazla bilgi için [Azure Cosmos DB kapsayıcıları için aktarım hızı ayarlama](set-throughput.md).

### <a name="can-i-scale-up-or-scale-down-the-throughput-of-my-table-api-table"></a>Ölçeği artırma veya miyim tablo API'si tablomun aktarım hızı ölçeklendirme? 
Evet, aktarım hızını ölçeklendirme için Azure Cosmos DB portal'ın ölçek bölmesini kullanabilirsiniz. Daha fazla bilgi için [aktarım hızı ayarlama](set-throughput.md).

### <a name="is-a-default-tablethroughput-set-for-newly-provisioned-tables"></a>Yeni sağlanan tablolar için TableThroughput bir varsayılan mı?
Evet, hizmet app.config aracılığıyla TableThroughput geçersiz kılmaz ve önceden oluşturulmuş kapsayıcı Azure Cosmos DB'de kullanmayın, aktarım hızı 400 ile bir tablo oluşturur.
 
### <a name="is-there-any-change-of-pricing-for-existing-customers-of-the-azure-table-storage-service"></a>Fiyatlandırma Azure tablo depolama hizmetinin mevcut müşteriler için herhangi bir değişiklik var mı?
Yok. Mevcut Azure tablo depolama müşterileri için fiyat değişiklik yoktur. 

### <a name="how-is-the-price-calculated-for-the-table-api"></a>Fiyat, tablo API'si için nasıl hesaplanır? 
Fiyat üzerinde ayrılmış TableThroughput bağlıdır. 

### <a name="how-do-i-handle-any-rate-limiting-on-the-tables-in-table-api-offering"></a>Tablo API'si teklifi tablolarda hiçbir oran sınırlandırma nasıl yapabilirim? 
İstek hızı, temel alınan kapsayıcı için sağlanan aktarım hızı kapasitesi veya bir dizi kapsayıcıları aşıyorsa, bir hata iletisi ve yeniden deneme ilkesi uygulayarak SDK'sı çağrı yeniden denenir.

### <a name="why-do-i-need-to-choose-a-throughput-apart-from-partitionkey-and-rowkey-to-take-advantage-of-the-table-api-offering-of-azure-cosmos-db"></a>PartitionKey ve RowKey Azure Cosmos DB'nin tablo API'si teklifi yararlanmak için ayrı bir aktarım hızı seçmek neden ihtiyacım var?
App.config dosyasında ya da portal aracılığıyla belirtmezseniz, azure Cosmos DB kapsayıcınız için bir varsayılan aktarım hızını ayarlar. 

Azure Cosmos DB, performans ve işlem için üst sınır olan gecikme süresi garantileri sağlar. Bu garanti altyapısı kiracının operasyonlarda idare zorunlu kılabilir mümkündür. TableThroughput ayarlanması, platform bu kapasite ayırır ve işletimsel başarı garanti eder garantili aktarım hızı ve gecikme süresi, almasını sağlar. 

Aktarım hızı belirtimi kullanarak esnek bir şekilde bunu uygulamanızın mevsimsellik avantajı, aktarım hızı gereksinimlerini karşılamak ve maliyet tasarrufu için değiştirebilirsiniz.

### <a name="azure-table-storage-has-been-very-inexpensive-for-me-because-i-pay-only-to-store-the-data-and-i-rarely-query-the-azure-cosmos-db-table-api-offering-seems-to-be-charging-me-even-though-i-have-not-performed-a-single-transaction-or-stored-anything-can-you-please-explain"></a>Azure tablo depolama, yalnızca depolamak verileri ve nadiren sorgu öderim çünkü benim için çok düşük maliyetli olmuştur. Azure Cosmos DB tablo API'si teklifi, ben değil tek bir işlem gerçekleştirilen veya herhangi bir şey depolanan olsa bile bana şarj gibi görünüyor. Lütfen açıklayabilir?

Azure Cosmos DB, kullanılabilirlik, gecikme süresi ve aktarım hızı için Garantisi ile Global olarak dağıtılmış, SLA tabanlı bir sistem olacak şekilde tasarlanmıştır. Azure Cosmos DB'de aktarım hızı ayırdığınızda, bu, diğer sistemler aktarım hızı garanti edilir. Azure Cosmos DB, ikincil dizinler ve küresel dağıtım gibi müşterilerin istediniz ek özellikler sağlar.  

### <a name="i-never-get-a-quota-full-notification-indicating-that-a-partition-is-full-when-i-ingest-data-into-azure-table-storage-with-the-table-api-i-do-get-this-message-is-this-offering-limiting-me-and-forcing-me-to-change-my-existing-application"></a>(Bir bölümün tam olduğunu belirten) hiçbir zaman tam bir kota alabilirim"bildirim zaman miyim alma verileri Azure tablo depolama alanına. Bu ileti tablo API'si ile alabilirim. Bu benim sınırlama ve bana mevcut Uygulamam değiştirmek için zorlama sunar?

Azure Cosmos DB, sınırsız ölçek, gecikme süresi, aktarım hızı, kullanılabilirlik ve tutarlılık garantileri sağlar bir SLA tabanlı bir sistemdir. Garantili premium performans için veri boyutu ve dizin yönetilebilir ve ölçeklenebilir olduğundan emin olun. Varlıkları ya da bölüm anahtarı başına öğe sayısı 10 GB sınırına harika arama ve sorgu performansını sağladığımız sağlamaktır. Uygulamanızın Azure depolama için bile de ölçeklendirilebileceğinden emin olmak için önerilir, *değil* tüm bilgileri bir bölümde depolama ve sorgulama sık erişimli bir bölüm oluşturun. 

### <a name="so-partitionkey-and-rowkey-are-still-required-with-the-table-api"></a>Bu nedenle PartitionKey ve RowKey tablo API'si ile hala gereklidir? 
Evet. Tablo API'SİNİ'ın yüzey alanını, Azure tablo depolama SDK'sı benzer olduğundan, bölüm anahtarı verileri dağıtmaya yönelik etkili bir yol sağlar. Satır anahtarı, bu bölüm içinde benzersizdir. Satır anahtarı, mevcut olması gerekir ve olduğu gibi standart SDK null olamaz. RowKey uzunluğu 255 bayttır ve PartitionKey uzunluğu 1 KB'tır. 

### <a name="what-are-the-error-messages-for-the-table-api"></a>Tablo API'si için hata iletileri nelerdir?
Hataların çoğu aynı olacak şekilde azure tablo depolama ve Azure Cosmos DB tablo API'si aynı SDK'ları kullanın.

### <a name="why-do-i-get-throttled-when-i-try-to-create-lot-of-tables-one-after-another-in-the-table-api"></a>Birçok tabloları birbiri ardına tablo API'SİNDE oluşturmaya çalıştığınızda neden miyim kısıtlanmazsınız?
Azure Cosmos DB, gecikme süresi, aktarım hızı, kullanılabilirlik ve tutarlılık garantileri sağlar bir SLA tabanlı bir sistemdir. Sağlanan sistem olduğundan, bu gereksinimleri güvence altına almak için kaynakları ayırır. Tablo oluşturma Hızlı oranını algıladı ve kısıtlanmış. Tabloların oluşturulma oranı arayın ve dakika başına 5'ten daha düşük öneririz. Tablo API'si sağlanan sistem olduğunu unutmayın. Sağlama süre için ödeme başlar. 

## <a name="develop-against-the-graph-api"></a>Graph API karşı geliştirin
### <a name="how-can-i-apply-the-functionality-of-graph-api-to-azure-cosmos-db"></a>Nasıl miyim Graph API işlevlerini Azure Cosmos DB'ye uygulayabilir miyim?
Graph API işlevlerini uygulamak için bir uzantı Kitaplığı'nı kullanabilirsiniz. Bu kitaplık, Microsoft Azure grafikleri denir ve NuGet üzerinde kullanılabilir. 

### <a name="it-looks-like-you-support-the-gremlin-graph-traversal-language-do-you-plan-to-add-more-forms-of-query"></a>Gremlin graf çapraz dil desteği gibi görünüyor. Sorgu, daha fazla formlar eklemek üzere planlıyor musunuz?
Evet, sorgu için başka mekanizmalar gelecekte eklemeyi planlıyoruz. 

### <a name="how-can-i-use-the-new-graph-api-offering"></a>Yeni Graph API'si teklifi nasıl kullanabilirim? 
Başlamak için tamamlamak [Graph API'si](../cosmos-db/create-graph-dotnet.md) hızlı başlangıç makalesi.

<a id="cassandra"></a> 
## <a name="develop-with-the-apache-cassandra-api-preview"></a>Apache Cassandra API'si (Önizleme) ile geliştirin

### <a name="what-is-the-protocol-version-supported-in-the-private-preview-is-there-a-plan-to-support-other-protocols"></a>Özel önizleme sürümünde desteklenen protokol sürümü nedir? Diğer protokoller desteklemek için bir plan var mı?
Azure Cosmos DB Apache Cassandra API'si, bugün CQL sürüm 4 destekler. Diğer protokoller destekleme hakkında Geribildiriminiz varsa bize [uservoice geri bildirimi](https://feedback.azure.com/forums/263030-azure-cosmos-db) veya bir e-posta Gönder [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com). 

### <a name="why-is-choosing-a-throughput-for-a-table-a-requirement"></a>Bir tablo için bir aktarım hızı bir gereksinim seçmektir neden?
Azure Cosmos DB, durum - tablosundan oluşturduğunuz kapsayıcınız için varsayılan aktarım hızını ayarlar portal'ı veya CQL. Azure Cosmos DB, performans ve işlem için üst sınır olan gecikme süresi garantileri sağlar. Bu garanti altyapısı kiracının operasyonlarda idare zorunlu kılabilir mümkündür. Ayar aktarım hızı, platform bu kapasite ayırır ve garanti işlem başarılı olduğundan garantili aktarım hızı ve gecikme süresi, almasını sağlar. Uygulamanızın mevsimsellik yararlanabilir ve maliyet tasarrufu için işlem hacmi esnek bir şekilde değiştirebilirsiniz.

Aktarım hızı kavramı açıklanan [Azure Cosmos DB'de istek birimleri](request-units.md) makalesi. Aktarım hızı bir tablo için temel alınan fiziksel bölümler arasında eşit olarak dağıtılır.  

### <a name="what-is-the-default-rus-of-table-when-created-through-cql-what-if-i-need-to-change-it"></a>' % S'varsayılan RU/sn CQL oluştururken tablosunun nedir? Peki bunu değiştirmem gerekecek?
Azure Cosmos DB, aktarım hızı sağlamak için istek birimi (RU/sn) saniyede bir para birimi olarak kullanır. CQL ile oluşturulan tablolarda sahip 400 RU. Portaldan RU değiştirebilirsiniz. 

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

### <a name="what-happens-when-throughput-is-exceeded"></a>Aktarım hızını aştı ne olur? 
Azure Cosmos DB, performans ve işlem için üst sınır olan gecikme süresi garantileri sağlar. Bu garanti altyapısı kiracının operasyonlarda idare zorunlu kılabilir mümkündür. Platform bu kapasite ayırır ve garanti işlem başarılı olduğundan garantili aktarım hızı ve gecikme süresi elde etmeniz sağlanır aktarım hızı ayarlama göre bu mümkündür. Bu kapasiteyi aştığında, aşırı yüklenmiş kapasite aşıldı olduğunu belirten hata iletisi alırsınız. 0x1001 aşırı: "İstek oranı büyük olduğundan" istek işlenemiyor. Juncture en bu hangi işlemleri görmek için gereklidir ve toplu bu sorunu neden olur. Tüketilen kapasite ölçümleri Portal ile sağlanan kapasiteyi aşan hakkında bir fikir edinebilirsiniz. Kapasite neredeyse tüketilen emin olmak gereken sonra tüm temel alınan bölümler arasında eşit. Aktarım hızı çoğunu bir bölüm tarafından kullanılan görürseniz, iş yükü eğriltme sahip. 

Kullanılabilir ölçümler gösteren, nasıl işleme başına yedi gün ve saat, gün, üzerinden bölümler arasında veya toplama kullanılıyor. Daha fazla bilgi için [izleme ve Azure Cosmos DB'de ölçümlerle hata ayıklama](use-metrics.md).

Tanılama günlükleri içinde açıklanan [Azure Cosmos DB tanılama günlüğüne kaydetme](logging.md) makalesi.

### <a name="does-the-primary-key-map-to-the-partition-key-concept-of-azure-cosmos-db"></a>Azure Cosmos DB bölüm anahtarı kavramına birincil anahtar eşleme mu?
Evet, bölüm anahtarı, varlık doğru konuma yerleştirmek için kullanılır. Azure Cosmos DB içinde bir fiziksel bölüm üzerinde depolanan sağ mantıksal bölüm bulmak için kullanılır. Bölümleme kavramı da açıklanan [bölümleme ve ölçeklendirme Azure Cosmos DB'de](partition-data.md) makalesi. Temel sınav zamanı hemen İşte bir mantıksal bölüm 10 GB sınırı bugün aşmamalıdır olduğunu. 

### <a name="what-happens-when-i-get-a-quota-full-notification-indicating-that-a-partition-is-full"></a>Bir bölüm tam olduğunu gösteren tam bir kota alabilirim ne olur"bildirim?
Azure Cosmos DB, sınırsız ölçek, gecikme süresi, aktarım hızı, kullanılabilirlik ve tutarlılık garantileri sağlar bir SLA tabanlı bir sistemdir. Cassandra API çok veri sınırsız depolama sağlar. Bu sınırsız depolama, veri bölümleme anahtar kavram kullanarak yatay genişletme dayanır. Bölümleme kavramı da açıklanan [bölümleme ve ölçeklendirme Azure Cosmos DB'de](partition-data.md) makalesi.

Varlıklar veya mantıksal bölüm başına öğe sayısı 10 GB sınırına için uymanız gerekir. Uygulamanızın iyi ölçeklendirilebileceğinden emin olmak için önerilir, *değil* tüm bilgileri bir bölümde depolama ve sorgulama sık erişimli bir bölüm oluşturun. Gelen verileri dengesiz -, yani, yalnızca çok fazla veri için bir bölüm anahtarı - başka bir deyişle, bu hata için 10 GB'den fazla. Depolama portalı kullanarak veri dağılımını bulabilirsiniz. Bu hatayı düzeltmek için yol için recrete tablosu ve daha iyi veri dağılımını sağlayan ayrıntılı birincil (bölüm anahtarı) seçin.

### <a name="is-it-possible-to-use-cassandra-api-as-key-value-store-with-millions-or-billions-of-individual-partition-keys"></a>Milyonlarca veya tek tek bölüm anahtarları milyarlarca ile Cassandra API anahtar değeri deposu olarak kullanmak mümkündür?
Azure Cosmos DB, depolama ölçeklendirerek sınırsız veri depolayabilirsiniz. Bu, aktarım hızını bağımsızdır. Evet, Cassandra API depolamak ve anahtar/değer doğru birincil bölüm anahtarı belirterek almak için her zaman sadece kullanabilirsiniz. Bu tek tek anahtarları kendi mantıksal bölüm almak ve sorunları olmadan fiziksel bölüm üzerine durur. 

### <a name="is-it-possible-to-create-multiple-tables-with-apache-cassandra-api-of-azure-cosmos-db"></a>Apache Cassandra API'si, Azure Cosmos DB ile birden çok tablo oluşturulabilir mi?
Evet, tabletlerde için olası Apache Cassandra API'si ile birden çok tablo. Bu tablo, aktarım hızı ve depolama birimi olarak kabul edilir. 

### <a name="is-it-possible-to-create-multiple-tables-in-succession"></a>Art arda birden fazla tabloyu oluşturulabilir mi?
Azure Cosmos DB, hem verileri hem de denetim düzlemi etkinlikler için yönetilen kaynak sistemidir. Koleksiyonlar gibi tablo, belirli bir işleme kapasitesi için sağlanan çalışma zamanında varlıklar kapsayıcılardır. Bu sayfayı hızlı bir şekilde kapsayıcılarda oluşturulmasını beklenen etkinliği değil ve kısıtlanmış. Bırakma/tabloları hemen - oluşturma, testleri varsa, aralarında boşluk Lütfen deneyin.

### <a name="what-is-maximum-number-of-tables-which-can-be-created"></a>En fazla oluşturulabilecek tablo sayısı nedir?
Tablo sayısı fiziksel bir sınır yoktur, Lütfen bir e-postası gönder [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com) normal 10s veya 100'lük bloklar oluşturulması gerekir (burada sürekli toplam boyutu 10 TB veri aşıyor) tabloları çok fazla sayıda varsa. 

### <a name="what-is-the-maximum--of-keyspace-which-we-can-create"></a>Yenilikler oluşturabiliriz anahtar alanı için en yüksek sayısı 
Meta veri kapsayıcıları oldukları gibi fiziksel keyspaces sayısı sınırlı değildir, Lütfen bir e-postası gönder [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com) herhangi bir nedenden dolayı keyspaces çok sayıda varsa. 

### <a name="is-it-possible-to-bring-in-lot-of-data-after-starting-from-normal-table"></a>Bu normal tablosundan başlattıktan sonra çok sayıda veri getirmek mümkün mü? 
Depolama kapasitesi, otomatik olarak yönetilir ve daha fazla veri gönderme artar. Bu nedenle, yönetme ve sağlama düğümleri vb. ihtiyacınız olan verilere erişmelerini güvenle içeri aktarabilirsiniz. 

### <a name="is-it-possible-to-supply-yaml-file-settings-to-configure-apache-casssandra-api-of-azure-cosmos-db-behavior"></a>Azure Cosmos DB, Apache Casssandra API davranışı yapılandırmak için yaml dosyası ayarları sağlamak mümkündür?
Azure Cosmos DB, Apache Cassandra API'si, bir platform hizmetidir. Bu işlemleri yürütmek için protokol düzeyinde uyumluluk sunar. Yerine, yönetim, izleme ve yapılandırma karmaşıklığını gizler. Geliştirici/kullanıcı olarak kullanılabilirlik, silinmiş öğe işareti, anahtar önbellek, satır önbellek, çiçek tomurcukları filtre ve çeşitli diğer ayarlar hakkında endişelenmeniz gerekmez. Azure Cosmos DB Apache Cassandra API'si odaklar sağlama üzerinde okuma ve yazma performansını yapılandırma ve yönetim ek yükü gerektirir.

### <a name="will-apache-cassandra-api-for-azure-cosmos-db-support-node-additioncluster-statusnode-status-commands"></a>Azure Cosmos DB Apache Cassandra API'si, düğüm ekleme/küme durumu/node durumu komutları desteklenecek?
Apache Cassandra API'si kapasite planlama, barındırmamıza aktarım hızı ve depolama için esneklik taleplerini yanıtlama getiren bir platform hizmetidir. Azure Cosmos DB ile ihtiyacınız olan aktarım hızı sağlayın. Ardından, bu kez gün arasında herhangi bir sayıda yukarı ve aşağı düğümleri ekleme/silme veya bunları yönetme hakkında endişelenmenize gerek kalmadan ölçeklendirebilirsiniz. Bu düğüm, küme yönetim aracını kullanmak gerekmez anlamına gelir. 

### <a name="what-happens-with-respect-to-various-config-settings-for-keyspace-creation-like-simplenetwork"></a>Anahtar oluşturma gibi basit/ağ için çeşitli yapılandırma ayarlarını göre ne olur?
Azure Cosmos DB, kullanılabilirlik ve düşük gecikme süresi nedeniyle hazır genel dağıtım sağlar. Kurulum çoğaltmalar vb. için gerekli değildir. Her zaman tüm yazma işlemlerini performans garantisi sağlarken yazma burada herhangi bir bölge içinde çekirdek arızaya olur.  

### <a name="what-happens-with-respect-to-various-settings-for-table-metadata-like-bloom-filter-caching-read-repair-change-gcgrace-compression-memtableflushperiod-etc"></a>Önbelleğe alma, çeşitli ayarları göre çiçek tomurcukları filtre gibi tablo meta verileri için ne olduğunu okuyun onarım değişiklik, gc_grace, sıkıştırma memtable_flush_period vb.?
Azure Cosmos DB okumayı/yazmayı ve aktarım hızı için tüm yapılandırma ayarlarını ve bunları düzenleme yanlışlıkla temas gerek kalmadan performans sağlar.  

### <a name="is-time-to-live-ttl-supported-for-cassandra-tables"></a>Yaşam süresi (TTL) Cassandra tablolar için destekleniyor mu? 
Evet, TTL desteklenir. 

### <a name="is-it-possible-to-monitor-node-status-replica-status-gc-and-os-parameters-earlier-with-various-tools-what-needs-to-be-monitored-now"></a>İzleyici düğümü durumu, çoğaltma durumu, gc ve çeşitli araçlarla daha önce işletim sistemi parametrelerini mümkündür? Artık izlenmesi ne gerekiyor?
Azure Cosmos DB, üretkenliği artırmak ve altyapı izleme ve yönetme hakkında endişe duymamanızı yardımcı olan bir platform hizmetidir. Portal ölçümleri kısıtlanan ve artırır veya bu aktarım hızı azaltırsanız, bulmak için kullanılabilir olan aktarım hızı ölçeklendirilmesini yeterlidir. İzleyici [SLA'lar](monitor-accounts.md).
Kullanım [ölçümleri](use-metrics.md) kullanım [tanılama günlükleri](logging.md).

### <a name="which-client-sdks-can-work-with-apache-cassandra-api-of-azure-cosmos-db"></a>Hangi istemci SDK'ları, Apache Cassandra API'si, Azure Cosmos DB ile çalışabilir mi?
Özel önizleme Apache Cassandra SDK'ın istemci sürücüleri CQLv3 kullanan istemci programları için kullanıldı. Diğer sürücüleri kullanabilir veya sorunları yaşıyorsanız, e-posta Gönder varsa [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com). 

### <a name="is-composite-partition-key-supported"></a>Bileşik bölüm anahtarı destekleniyor mu?
Evet, bileşik bir bölüm anahtarı oluşturmak için normal söz dizimini kullanabilirsiniz. 

### <a name="can-i-use-sstable-loader-for-data-loading"></a>Sstable yükleyici veri yükleme için kullanabilir miyim?
Hayır, Önizleme süresince sstable yükleyici desteklenmiyor. 

### <a name="can-an-on-premises-cassandra-cluster-be-paired-with-azure-cosmos-dbs-apache-cassandra-api"></a>Bir şirket içi cassandra kümesi, Azure Cosmos DB Apache Cassandra API'si ile eşleştirilmiş?
Mevcut Azure Cosmos DB operasyonların işleriyle bulut ortamı için en iyi duruma getirilmiş bir deneyimi vardır. Eşleştirme ihtiyacınız varsa lütfen e-posta gönderin [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com) senaryonuz açıklamasını içeren.

### <a name="does-cassandra-api-provide-full-backups"></a>Cassandra API tam yedeklemeler sağlar? 
Azure Cosmos DB, dört saat aralığında tüm API'leri bugün gerçekleştirilecek iki ücretsiz tam yedeklemeler sağlar. Bu, bir yedekleme zamanlaması vb. Kurulumu gerekmez sağlar. Bekletme ve sıklığı değiştirmek istiyorsanız, bir e-posta Gönder [ askcosmosdbcassandra@microsoft.com ](mailto:askcosmosdbcassandra@microsoft.com) veya bir destek talebi oluşturun. Yedekleme özelliği hakkında bilgi sağlanır [otomatik çevrimiçi yedekleme ve geri yükleme Azure Cosmos DB ile](online-backup-and-restore.md) makalesi. 

### <a name="how-does-the-cassandra-api-account-handle-failover-if-a-region-goes-down"></a>Bir bölgede bir arıza yaşanırsa nasıl Cassandra API hesabı, yük devretme işliyor? 
Azure Cosmos DB Cassandra API'si, Azure Cosmos DB Global olarak dağıtılmış platformdan taşır. Uygulamanızın veri merkezi kapalı kalma süresi etkilenmemesini sağlamak için Azure Cosmos DB portalında hesabı için en az bir daha fazla bölge etkinleştirme [çok bölgeli Azure Cosmos DB hesapları ile geliştirme](regional-failover.md). Portalı kullanarak bölgenin önceliğini ayarlayabilirsiniz [çok bölgeli Azure Cosmos DB hesapları ile geliştirme](regional-failover.md). 

Hesabınız için istediğiniz ve burada bunun için bir yük devretme öncelik sağlayarak devredebilir denetlemek çok bölgesini ekleyebilirsiniz. Bir veritabanını kullanmak için bir uygulamayı çok vermeniz gerekir. Bunu yaptığınızda, müşterilerinizin kesintiyle karşılaşmayacaksınız. 

### <a name="does-the-apache-cassandra-api-index-all-attributes-of-an-entity-by-default"></a>Apache Cassandra API'SİNİN bir varlığın tüm öznitelikleri varsayılan olarak dizin mu?
Evet, bir varlığın tüm öznitelikleri varsayılan olarak Azure Cosmos DB tarafından dizine eklenir. Daha fazla bilgi için [Azure Cosmos DB: dizin oluşturma ilkeleri](indexing-policies.md). Tutarlı dizin ile garantili performans avantajlarını edinin ve dayanıklı çekirdek kaydedilen her zaman yazar. 

### <a name="does-this-mean-i-do-not-have-to-create-multiple-indexes-to-satisfy-the-queries"></a>Ben zorunda değilsiniz sorguları gerçekleştirmek için birden çok dizin oluşturma anlamına mı geliyor? 
Evet, Azure Cosmos DB, herhangi bir şema tanımı olmadan tüm özniteliklerin otomatik dizin oluşturma sağlar. Bu Otomasyon odak dizin oluşturma ve yönetim yerine uygulama geliştiricilerine serbest bırakır. Daha fazla bilgi için [Azure Cosmos DB: dizin oluşturma ilkeleri](indexing-policies.md).

### <a name="can-i-use-the-new-cassandra-api-sdk-locally-with-the-emulator"></a>Öykünücü ile yeni Cassandra API SDK'yı yerel olarak kullanabilirim?
Gelecekte bu özelliği desteklemeyi planlıyoruz. 

### <a name="azure-cosmos-db-as-a-platform-seems-to-have-lot-of-capabilities-such-as-changefeed-and-other-functionality-will-these-capabilities-be-added-to-the-cassandra-api"></a>Azure Cosmos DB platformu olarak çok sayıda changefeed ve diğer işlevler gibi özelliklere sahip gibi görünüyor. Bu özellikler Cassandra API'sine eklenecek mi? 
Apache Cassandra API'si aynı Apache Cassandra CQL işlevselliği sağlar. Çeşitli özellikler gelecekte destekleme Uygulanabilirlik bak planlıyoruz.

### <a name="feature-x-of-regular-cassandra-api-is-not-working-as-today-where-can-the-feedback-be-provided"></a>Normal Cassandra API'sinin x özelliği, geri bildirim burada sağlanabilir bugün olarak çalışmıyor?
Aracılığıyla geri bildirim sağlamak [uservoice geri bildirimi](https://feedback.azure.com/forums/263030-azure-cosmos-db).

[azure-portal]: https://portal.azure.com
[query]: sql-api-sql-query.md
