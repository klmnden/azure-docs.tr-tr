<properties 
    pageTitle="DocumentDB Veritabanı Soruları - Sık Sorulan Sorular | Microsoft Azure" 
    description="JSON için bir NoSQL belge veritabanı hizmeti olan Azure DocumentDB hakkında sık sorulan soruların yanıtlarını alın. Kapasite, performans düzeyleri ve ölçeklendirme hakkındaki veritabanı sorularını yanıtlayın." 
    keywords="Database questions, frequently asked questions, documentdb, azure, Microsoft azure"
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="03/30/2016" 
    ms.author="mimig"/>


#DocumentDB hakkında sık sorulan sorular

## Microsoft Azure DocumentDB temelleri hakkında veritabanı soruları

### Microsoft Azure DocumentDB nedir? 
Microsoft Azure DocumentDB, Microsoft Azure'ın gücü ve erişiminden destek alan bir yönetilen platform yoluyla şemasız verilerde zengin sorgulama sunan, yapılandırılabilir ve güvenilir performans sağlanmasına yardımcı olan ve hızlı gelişmeyi mümkün kılan bir yüksek düzeyde ölçeklenebilen bir hizmet olarak NoSQL belge veritabanıdır. Tahmin edilebilir işlemenin, düşük gecikme süresinin ve şemasız veri modelinin önemli gereksinimler olduğu web, mobil, oyun ve IoT uygulamaları için DocumentDB doğru çözümdür. DocumentDB, yerel bir JSON veri modeli aracılığıyla şema esnekliği ve zengin dizin oluşturma sağlar ve tümleşik JavaScript ile çok belgeli işlem desteğini içerir.  
  
Bu hizmeti dağıtma ve kullanma hakkında daha fazla veritabanı sorusu, yanıtı ve yönergesi için bkz. [DocumentDB belge sayfası](https://azure.microsoft.com/documentation/services/documentdb/).

### DocumentDB ne tür bir veritabanıdır?
DocumentDB, verileri JSON biçiminde depolayan bir NoSQL belge yönelimli veritabanıdır.  DocumentDB zengin bir DocumentDB [SQL sorgu dil bilgisi](documentdb-sql-query.md) aracılığıyla sorgulanabilen, iç içe geçmiş ve kendi içinde veri yapılarını destekler. DocumentDB [saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler](documentdb-programming.md) aracılığıyla sunucu tarafı JavaScript için yüksek performanslı işlem tabanlı işleme sağlar. Veritabanı aynı zamanda ilişkili [performans düzeyleriyle](documentdb-performance-levels.md) birlikte geliştirici tarafından ince ayar yapılabilir tutarlılık düzeylerini destekler.
 
### DocumentDB veritabanlarında ilişkisel veritabanı (RDBMS) gibi tablolar var mı?
Hayır, DocumentDB verileri JSON belgesi koleksiyonlarında depolar.  DocumentDB kaynakları hakkında bilgi için bkz. [DocumentDB kaynak modeli ve kavramları](documentdb-resources.md). 

### DocumentDB veritabanları şemasız verileri destekler mi?
Evet, DocumentDB şema tanımları veya ipuçları olmadan rastgele JSON belgelerinin uygulamalar tarafından depolanmasına izin verir. Veriler, DocumentDB SQL sorgu arabirimi yoluyla sorgu için hemen kullanılabilir.   

### DocumentDB ACID işlemlerini destekler mi?
Evet, DocumentDB JavaScript saklı yordamları ve tetikleyicileri olarak ifade edilen belgeler arası işlemleri destekler. İşlemler her bir koleksiyonun içindeki tek bir bölümün kapsamındadır ve diğer eş zamanlı yürütme kodları ve kullanıcı isteklerinden ayrı olarak, ACID semantiği ile tümü veya hiçbiri yoluyla yürütülür.  Sunucu tarafı JavaScript uygulama kodunun yürütülmesinde özel durumlar oluşursa işlemin tümü geri alınır. 

### DocumentDB için genel kullanım örnekleri nelerdir?  
DocumentDB otomatik ölçeğin, tahmin edilebilir performansın, milisaniye yanıt sürelerinin hızla sıralanmasının sıralanmasını ve şemasız verileri sorgulama işlevinin önemli olduğu yeni web, mobil, oyun ve IoT uygulamaları için iyi bir seçimdir. DocumentDB, uygulama veri modellerinin sürekli yinelenmesini destekleme ve hızlı geliştirme için uygundur. Kullanıcı tarafından oluşturulan içeriği ve verileri yöneten uygulamalar [DocumentDB için yaygın kullanım örnekleridir](documentdb-use-cases.md).  

### DocumentDB tahmin edilebilir bir performansı nasıl sunar?
İstek Birimi (RU), DocumentDB'deki üretilen iş ölçüsüdür. 1 RU, 1 KB'lık belgenin GET'inin işlemesine karşılık gelir. DocumentDB'de okuma, yazma, SQL sorguları ve saklı yordam yürütmeleri dahil olmak üzere her işlemin, işlemi tamamlamak için gereken işlemeyi temel alan bir belirleyici İstek Birimi değeri bulunur. CPU, IO, bellek ve bunların her birinin uygulama işlemenizi nasıl etkilediği hakkında düşünmek yerine, tek bir İstek Birimi ölçümünü göz önüne alarak değerlendirme yapabilirsiniz.

Her DocumentDB koleksiyonu, saniye başına işlemenin İstek Birimlerine göre, sağlanan işlemeyle saklanabilir. Her ölçekten uygulama için, istek birimi değerlerini ölçmek amacıyla istekleri ayrı ayrı kıyaslayabilir ve tüm istekler genelinde istek birimlerinin genel toplamının işlenmesi için koleksiyonlar sağlayabilirsiniz. Ayrıca, uygulamanızın ihtiyaçları geliştikçe koleksiyonunuzun işleme ölçeğini artırabilir veya azaltabilirsiniz. İstek birimleri hakkında daha fazla bilgi ve koleksiyonunuzun ihtiyaçlarını belirleme konusunda yardım için lütfen [Performans ve Kapasiteyi Yönetme](documentdb-manage.md)'yi okuyun.

### DocumentDB HIPAA ile uyumlu mu?
Evet, DocumentDB HIPAA ile uyumludur. HIPAA, bağımsız olarak tanımlanabilen sağlık bilgilerinin kullanımı, açıklanması ve korunması için gereksinimler belirler. Daha fazla bilgi için bkz. [Microsoft Güven Merkezi](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### DocumentDB'nin depolama sınırları nelerdir? 
DocumentDB'de bir koleksiyonun depolayabileceği toplam veri miktarının teorik olarak bir sınırı yoktur. Tek bir koleksiyon içinde 250 GB'tan fazla veri depolamak istiyorsanız hesap kotanızın yükseltilmesi için lütfen [desteğe başvurun](documentdb-increase-limits.md). 

### DocumentDB'nin işleme sınırları nelerdir? 
İş yükünüzün yeterince çok sayıda bölüm anahtarı arasında kabaca eşit bir şekilde dağıtılabilmesi durumunda, DocumentDB'de bir koleksiyonun destekleyebileceği toplam işleme miktarının teorik olarak bir sınırı yoktur. Koleksiyon veya hesap başına 250.000 istek birimi/saniyeyi aşmak istiyorsanız hesap kotanızın yükseltilmesi için lütfen [desteğe başvurun](documentdb-increase-limits.md). 

### Microsoft Azure DocumentDB'nin maliyeti nedir?
Ayrıntılı bilgi için lütfen [DocumentDB fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/documentdb/) sayfasına bakın. DocumentDB kullanım ücretleri, kullanılan koleksiyon sayısına, koleksiyonların çevrimiçi olduğu saat sayısına ve her bir koleksiyon için harcanan depolama ve sağlanan işlemeye göre belirlenir. 

### Ücretsiz bir hesap var mı?
Azure'da yeniyseniz tüm Azure hizmetlerini denemek için size 30 gün ve 200 ABD doları sağlayan bir [ücretsiz Azure hesabına](https://azure.microsoft.com/pricing/free-trial/) kaydolabilirsiniz. Alternatif olarak, bir Visual Studio aboneliğiniz varsa herhangi bir Azure hizmetinde kullanmak üzere [aylık 150 ABD dolarını içeren ücretsiz Azure kredisi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) hakkınız bulunur.  

### DocumentDB için nasıl ek yardım alabilirim?
Yardıma ihtiyacınız olursa [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-documentdb)'dan ve [Azure DocumentDB MSDN Geliştirici Forumları](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB)'ndan bize ulaşın veya [DocumentDB mühendislik ekibi ile bire bir sohbet](http://www.askdocdb.com/) planlayın. En son DocumentDB haberleri ve özellikleri hakkında güncel kalmak için bizi [Twitter](https://twitter.com/DocumentDB)'da takip edin.

## Microsoft Azure DocumentDB'yi ayarlama

### Microsoft Azure DocumentDB'ye nasıl kaydolurum?
Microsoft Azure DocumentDB'yi [Azure Portal][azure-portal]'da bulabilirsiniz.  Öncelikle Microsoft Azure aboneliği için kaydolmanız gerekir.  Microsoft Azure aboneliği için kaydolduktan sonra, Azure aboneliğinize bir DocumentDB hesabı ekleyebilirsiniz. DocumentDB hesabı ekleme hakkında yönergeler için bkz. [DocumentDB veritabanı hesabı oluşturma](documentdb-create-account.md).   

### Ana anahtar nedir?
Ana anahtar, bir hesaptaki tüm kaynaklara erişmeyi sağlayan bir güvenlik belirtecidir. Anahtara sahip kişiler, veritabanı hesabındaki tüm kaynaklara okuma ve yazma erişimine sahiptir. Ana anahtarları dağıtırken dikkatli olun. Birincil ana anahtar ve ikincil ana anahtar, [Azure Portal][azure-portal] **Anahtarlar **dikey penceresinde bulunur. Anahtarlar hakkında daha fazla bilgi için bkz. [Erişim tuşlarını görüntüleme, kopyalama ve yeniden oluşturma](documentdb-manage-account.md#keys).

### Veritabanı nasıl oluşturulur?
[DocumentDB veritabanı oluşturma](documentdb-create-database.md)'da açıklandığı gibi [Azure Portal]()'ı kullanarak, [DocumentDB SDK'larından](documentdb-sdk-dotnet.md) biriyle veya [REST API'leri](https://msdn.microsoft.com/library/azure/dn781481.aspx) aracılığıyla veritabanları oluşturabilirsiniz.  

### Koleksiyon nedir?
Koleksiyon, JSON belgeleri ve ilişkili JavaScript uygulama mantığının bir kapsayıcısıdır. Koleksiyon, [maliyetin](documentdb-performance-levels.md) koleksiyonla ilişkili performans düzeyine göre belirlendiği faturalanabilir bir varlıktır. Koleksiyonlar bir veya daha fazla bölümü/sunucuyu kapsayabilir ve neredeyse sınırsız miktarda depolama veya işlemeyi işleyebilecek şekilde ölçeklendirilebilir.

Koleksiyonlar, DocumentDB için de faturalama varlıklarıdır. Her bir koleksiyon, sağlanan işleme ve kullanılan depolama alanına göre saatlik olarak faturalandırılır. Daha fazla bilgi için bkz. [DocumentDB fiyatlandırma](https://azure.microsoft.com/pricing/details/documentdb/).  

### Kullanıcıları ve izinleri nasıl ayarlarım?
[DocumentDB SDK'larından](documentdb-sdk-dotnet.md) birini kullanarak veya [REST API'leri](https://msdn.microsoft.com/library/azure/dn781481.aspx) aracılığıyla kullanıcılar ve izinler oluşturabilirsiniz.   

## Microsoft Azure DocumentDB'de geliştirme hakkında veritabanı soruları

### DocumentDB'de geliştirmeye nasıl başlarım?
.NET, Python, Node.js, JavaScript ve Java için [SDK'lar](documentdb-sdk-dotnet.md) kullanılabilir.  Geliştiriciler, çeşitli platformlardan ve dillerden DocumentDB kaynaklarıyla etkileşim kurmak için [RESTful HTTP API'lerini](https://msdn.microsoft.com/library/azure/dn781481.aspx) de kullanabilir. 

DocumentDB [.NET](documentdb-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md) ve [Python](documentdb-python-samples.md) SDK'ları için örnekler GitHub'da bulunmaktadır.

### DocumentDB SQL destekliyor mu?
DocumentDB SQL sorgu dili, SQL tarafından desteklenen gelişmiş bir sorgu işlevi alt kümesidir. DocumentDB SQL sorgu dili, JavaScript tabanlı kullanıcı tanımlı işlevler (UDF'ler) aracılığıyla zengin hiyerarşik ve ilişkisel işleçler ve genişletilebilirlik sağlar. JSON dil bilgisi, JSON belgelerinin ağaç düğümleri olarak etiketli ağaçlar şeklinde modellenmesini sağlar, bu da hem DocumentDB'nin otomatik dizin oluşturma teknikleri hem de DocumentDB'nin SQL sorgu diyalekti tarafından kullanılır.  SQL dil bilgisinin nasıl kullanılacağı hakkında ayrıntılar için lütfen [DocumentDB'yi sorgulama][sorgu] makalesine bakın.

### DocumentDB tarafından desteklenen veri türleri nelerdir?
DocumentDB'de desteklenen temel veri türleri, JSON ile aynıdır. JSON Dizeler, Sayılar (IEEE754 çift duyarlık) ve true, false ve Null olmak üzere Boole değerlerinden oluşan basit türde bir sisteme sahiptir.  DateTime, Guid, Int64 ve Geometry gibi daha karmaşık olan veri türleri, { } işleci kullanılarak iç içe geçmiş nesnelerin ve [ ] işleci kullanılarak dizilerin oluşturulması yoluyla hem JSON hem de DocumentDB'de temsil edilebilir. 

### DocumentDB eşzamanlılığı nasıl sağlar?
DocumentDB, HTTP varlık etiketleri veya ETag'ler aracılığıyla iyimser eşzamanlılık denetimini (OCC) destekler. Tüm DocumentDB kaynakları bir ETag'e sahiptir ve DocumentDB istemcileri yazma isteklerine en son okuma sürümlerini ekler. ETag geçerli ise değişiklik uygulanır. Değer dışarıdan değiştirilmişse sunucu bir "HTTP 412 Önkoşul hatası" yanıt koduyla yazmayı reddeder. İstemcilerin kaynağın en son sürümünü okumaları ve isteği yeniden denemeleri gerekir. 

### DocumentDB'de nasıl işlem gerçekleştiririm?
DocumentDB, JavaScript saklı yordamları ve tetikleyicileri aracılığıyla dil ile tümleşik işlemleri destekler. Betiklerin içindeki tüm veritabanı işlemleri, tek bölümlü bir koleksiyon olduğu zaman koleksiyon kapsamındaki anlık görüntü yalıtımında, koleksiyon bölümlendiğinde ise bir koleksiyondaki aynı bölüm anahtarı değerine sahip belgelerde yürütülür. Belge sürümlerinin anlık görüntüsü (ETag'ler) ise işlem başlangıcında alınır ve yalnızca betik başarılı olursa uygulanır. JavaScript bir hata oluşturursa işlem geri alınır. Daha ayrıntılı bilgi için bkz. [DocumentDB sunucu tarafı programlama](documentdb-programming.md).

### Belgeleri DocumentDB'ye toplu olarak nasıl yerleştirebilirim? 
DocumentDB'ye belgeleri toplu olarak yerleştirmenin üç yolu vardır:

- Veri geçiş aracı, açıklama için bkz. [DocumentDB'ye veri aktarma](documentdb-import-data.md).
- Azure Portal'daki Belge Gezgini, açıklama için bkz. [Belge Gezgini ile belgeleri toplu ekleme](documentdb-view-json-document-explorer.md#BulkAdd).
- Saklı yordamlar, açıklama için bkz. [DocumentDB sunucu tarafı programlama](documentdb-programming.md).

### DocumentDB kaynak bağlantıyı önbelleğe almayı destekliyor mu?
Evet, DocumentDB bir RESTful hizmeti olduğu için kaynak bağlantıları sabittir ve önbelleğe alınabilir. DocumentDB istemcileri, belge veya koleksiyon gibi herhangi bir kaynakta yapılan okumalar için bir "If-None-Match" üst bilgisi belirtebilir ve yalnızca sunucu sürümü değiştiğinde kendi yerel kopyalarını güncelleştirebilir. 




[azure-portal]: https://portal.azure.com
[sorgu]: documentdb-sql-query.md
 


<!---HONumber=Jun16_HO2-->


