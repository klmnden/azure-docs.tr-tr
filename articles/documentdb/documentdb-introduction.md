<properties 
    pageTitle="Bir JSON veritabanı olan DocumentDB'ye giriş | Microsoft Azure" 
    description="Bir NoSQL JSON veritabanı olan Azure DocumentDB hakkında bilgi edinin. Bu belge veritabanı büyük veri, esnek ölçeklenebilirlik ve yüksek kullanılabilirlik için oluşturulmuştur." 
    keywords="json database, document database"
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

# DocumentDB'ye giriş: Bir NoSQL JSON Veritabanı

Azure DocumentDB hızlı ve tahmin edilebilir performans, yüksek kullanılabilirlik, otomatik ölçeklendirme ve geliştirme kolaylığı için oluşturulmuş tam olarak yönetilen bir NoSQL veritabanı hizmetidir. Esnek veri modeli, tutarlı düşük gecikme süreleri ve zengin sorgu özellikleri sayesinde web, mobil, oyun, IoT ve sorunsuz ölçeklendirme gerektiren diğer birçok uygulama için harika bir seçenektir.

Bu JSON veritabanı hakkında bilgi edinmenin ve bunu çalışırken görmenin hızlı bir yolu şu üç adımı takip etmektir: 

1. DocumentDB kullanmanın avantajlarını tanıtan iki dakikalık [DocumentDB nedir?](https://azure.microsoft.com/documentation/videos/what-is-azure-documentdb/) videosunu izleyin.
2. Azure Portal'ı kullanarak DocumentDB kullanmaya nasıl başlayacağınızı vurgulayan üç dakikalık [Azure'da DocumentDB oluşturma](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) videosunu izleyin.
3. DocumentDB'de kullanılabilen zengin sorgulama işlevi hakkında bilgi edinmek için farklı etkinlikleri inceleyebileceğiniz [Query Playground](http://www.documentdb.com/sql/demo)'u ziyaret edin. Ardından, Korumalı Alan sekmesine giderek kendi SQL sorgularınızı çalıştırın ve DocumentDB'yi deneyin.

Bunun sonrasında, daha derinlere ineceğimiz bu makaleye dönün ve aşağıdaki soruların yanıtlarını öğrenin:  

-   [DocumentDB nedir ve modern uygulamalara sağladığı değer nedir?](#what-is-azure-documentdb)
-   [DocumentDB'de verilerim nasıl yönetilir ve bunlara nasıl erişirim?](#data-management)
-   [DocumentDB kullanarak uygulamaları nasıl geliştiririm?](#develop)
-   [DocumentDB uygulaması oluşturmak için sonraki adımlarım nelerdir?](#next-steps)  

## Azure DocumentDB nedir?  

Modern uygulamalar çok büyük hacimdeki verileri hızlı bir şekilde üretir, tüketir ve bunlara yanıt verir. Bu uygulamalar ve de temel alınan veri şeması çok hızlı bir şekilde gelişir. Geliştiriciler buna yanıt olarak, uygulama veri modellerini ve yapılandırılmamış veri akışlarını hızla yineleme özelliğini koruyarak verileri depolamak ve işlemek için basit, hızlı ve ölçeklenebilir çözümler olan şemasız NoSQL belge veritabanlarını giderek daha fazla tercih etmektedir. Ancak birçok şemasız veritabanı karmaşık sorgulara ve işlem tabanlı işlemeye izin vermez, bu da veri yönetimini zorlaştırır. İşte bu noktada DocumentDB devreye girer. Microsoft, günümüzün uygulamalarına göre verileri yönetirken bu gereksinimleri karşılamak üzere DocumentDB'yi geliştirmiştir.

DocumentDB modern mobil, web, oyun ve IoT uygulamaları için tasarlanmış gerçek bir şemasız NoSQL veritabanı hizmetidir. DocumentDB tutarlı bir şekilde hızlı okuma ve yazma, şema esnekliği ve bir veritabanının ölçeğini isteğe bağlı olarak kolayca artırma ve azaltma olanağı sunar. Dizin oluşturduğu JSON belgeleri için herhangi bir şema varsaymaz veya gerektirmez. Varsayılan olarak, veritabanındaki tüm belgelerin otomatik olarak dizinini oluşturur ve herhangi bir şemayı ya da ikincil dizinlerin oluşturulmasını beklemez veya gerektirmez. DocumentDB SQL dilini kullanarak karmaşık geçici sorguları mümkün kılar, iyi tanımlanmış tutarlılık düzeylerini destekler ve saklı yordamlar, tetikleyiciler ve UDF'lerin tanıdık programlama modelini kullanarak JavaScript diliyle tümleşik, çok belgeli işlem gerçekleştirme sağlar. 

DocumentDB bir JSON veritabanı olarak, JSON belgelerini yerel bir şekilde destekleyip uygulama şemasının kolayca yinelenmesini sağlar ve anahtar değer, belge veya tablo veri modelleri gereksinimi olan uygulamaları destekler. DocumentDB JSON ve JavaScript'in yaygınlığını benimseyip uygulama tanımlı nesneler ve veritabanı şeması arasındaki uyumsuzluğu ortadan kaldırır. JavaScript derin tümleştirmesi, geliştiricilerin uygulama mantığını bir veritabanı işlemindeki veritabanı altyapısı içinde verimli şekilde ve doğrudan yürütmesini de sağlar. 

Azure DocumentDB aşağıdaki temel işlevleri ve avantajları sunar:

-   **Esnek bir şekilde ölçeklenebilir işleme ve depolama:** Uygulama gereksinimlerinizi karşılamak için DocumentDB JSON veritabanınızın ölçeğini kolayca artırın veya azaltın. Verileriniz düşük tahmin edilebilirliğe sahip gecikme süreleri sağlamak için katı hal disklerinde (SSD) depolanır. DocumentDB, JSON verilerini depolamak için koleksiyon adı verilen kapsayıcıları destekler; bu kapsayıcılar neredeyse sınırsız depolama boyutlarına ve sağlanan işlemeye ölçeklenebilir. Uygulamanız büyüdükçe, DocumentDB'yi tahmin edilebilir performansla sorunsuz ve esnek bir şekilde ölçeklendirebilirsiniz. 

-   **Tanıdık SQL söz dizimi ile geçici sorgular:** DocumentDB içinde heterojen JSON belgelerini depolayın ve tanıdık bir SQL söz dizimi aracılığıyla bu belgeleri sorgulayın. DocumentDB, tüm belge içeriğinin otomatik olarak dizinini oluşturmak için yüksek derecede eşzamanlı, kilitsiz, günlük yapılı bir dizin oluşturma teknolojisi kullanır. Böylece şema ipuçları, ikincil dizinler veya görünümler belirtmek gerekmeden gerçek zamanlı zengin sorgulara olanak sağlanır. Daha fazla bilgi için bkz. [DocumentDB'yi sorgulama](documentdb-sql-query.md). 

-   **Veritabanı içinde JavaScript yürütme:** Standart JavaScript'i kullanarak uygulama mantığını saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler (UDF'ler) olarak ifade edin. Böylece uygulama ve veritabanı şeması arasındaki uyumsuzluk hakkında endişelenmeye gerek kalmadan uygulama mantığınızın veriler üzerinde çalışması sağlanır. DocumentDB, doğrudan veritabanı altyapısının içinde JavaScript uygulama mantığının tam işlem tabanlı olarak yürütülmesini sağlar. JavaScript derin tümleştirmesi YERLEŞTİRME, DEĞİŞTİRME, SİLME ve SEÇME işlemlerinin bir JavaScript programı içinden yalıtılmış bir işlem olarak yürütülmesini sağlar. Daha fazla bilgi için bkz. [DocumentDB sunucu tarafı programlama](documentdb-programming.md).

-   **İnce ayarlanabilir tutarlılık düzeyleri:** Tutarlılık ve performans arasında en iyi dengeyi elde etmek için iyi tanımlanmış dört tutarlılık düzeyi arasından seçim yapın. DocumentDB sorgular ve okuma işlemleri için dört farklı tutarlılık düzeyi sunar: güçlü, sınırlanmış eskime durumu, oturum ve son. Bu ayrıntılı ve iyi tanımlanmış tutarlılık düzeyleri tutarlılık, kullanılabilirlik ve gecikme süresi arasında sağlam bir denge kurmanıza olanak sağlar. Daha fazla bilgi için bkz. [DocumentDB'de kullanılabilirlik ve performansı en üst düzeye çıkarmak için tutarlılık düzeylerini kullanma](documentdb-consistency-levels.md).

-   **Tam olarak yönetilme:** Veritabanı ve makine kaynaklarını yönetme ihtiyacını ortadan kaldırın. Bu tam olarak yönetilen bir Microsoft Azure hizmeti olduğundan sanal makineleri yönetmeniz, yazılımları dağıtıp yapılandırmanız, ölçeklendirmeyi yönetmeniz veya karmaşık veri katmanı yükseltmeleriyle uğraşmanız gerekmez. Tüm veritabanları otomatik olarak yedeklenir ve bölgesel arızalara karşı korunur. İhtiyacınız oldukça kolaylıkla bir DocumentDB hesabı ve sağlama kapasitesi ekleyebilirsiniz, böylece veritabanınızı çalıştırmak ve yönetmek yerine uygulamanıza odaklanmanız sağlanır. 

-   **Tasarımı gereği açık:** Var olan becerileri ve araçları kullanarak hızlı bir şekilde çalışmaya başlayın. DocumentDB'de programlama basittir, ulaşılabilirdir ve yeni araçları benimsemenizi veya JSON ya da JavaScript'e yönelik özel uzantılara bağlı kalmanızı gerektirmez. CRUD, sorgu ve JavaScript işleme dahil olmak üzere tüm veritabanı işlevlerine basit bir RESTful HTTP arabirimi üzerinden erişebilirsiniz. DocumentDB var olan biçimleri, dilleri ve standartları benimserken bunlara ek olarak yüksek değerde veritabanı işlevleri sunar.

Sorgu alma ve işlem tabanlı işleme gerektiren esnek veri kümelerini depolamak için DocumentDB'yi kullanabilirsiniz. Uygulama senaryoları arasında etkileşimli web, mobil ve oyun uygulamalarına ait kullanıcı verileri, aynı zamanda IoT cihazlarının oluşturduğu JSON verilerinin depolanması, alınması ve işlenmesi yer alır. DocumentDB internette ölçekli olarak çalışan uygulamalar için çok uygun olduğundan bir veritabanı herhangi bir hacimdeki JSON belgelerini depolayabilir.

##<a name="data-management"></a>Azure DocumentDB kaynakları
Azure DocumentDB iyi tanımlanmış veritabanı kaynakları aracılığıyla verileri yönetir. Bu kaynaklar yüksek kullanılabilirlik için çoğaltılır ve mantıksal URI'leri ile benzersiz olarak adreslenebilir. DocumentDB tüm kaynaklar için basit bir HTTP tabanlı RESTful programlama modeli sunar. 

DocumentDB veritabanı hesabı, size Azure DocumentDB erişimi sağlayan benzersiz bir ad alanıdır. Bir veritabanı hesabı oluşturabilmeniz için, öncelikle çeşitli Azure hizmetlerine erişim sağlayan bir Azure aboneliğinizin olması gerekir. 

DocumentDB içindeki tüm kaynaklar JSON belgeleri olarak modellenir ve depolanır. Kaynaklar meta veriler içeren JSON belgeleri olan öğeler şeklinde, aynı zamanda öğe koleksiyonları olan akışlar şeklinde yönetilir. Öğe kümeleri ilgili akışlarının içinde yer alır.

Aşağıdaki görüntüde DocumentDB kaynakları arasındaki ilişkiler gösterilmektedir:

![Bir NoSQL JSON veritabanı olan DocumentDB'de kaynaklar arasındaki hiyerarşi ilişkisi][1] 

Bir veritabanı hesabı, her biri saklı yordamlar, tetikleyiciler, UDF'ler, belgeler ve ilgili ekler içerebilen birden çok koleksiyonu kapsayan bir veritabanları kümesinden oluşur. Bir veritabanı çeşitli diğer koleksiyonlara, saklı yordamlara, tetikleyicilere, UDF'lere, belgelere veya eklere erişmek için her biri bir izinler kümesine sahip olan ilgili kullanıcıları da içerir. Veritabanları, kullanıcılar, izinler ve koleksiyonlar iyi bilinen şemalar sahip sistem tanımlı kaynaklardır; belgeler, saklı yordamlar, tetikleyiciler, UDF'ler ve eklerde ise rastgele ve kullanıcı tanımlı JSON içeriği bulunur.  

##<a name="develop"></a> Azure DocumentDB ile geliştirme
Azure DocumentDB, HTTP/HTTPS istekleri yapabilen herhangi bir dilin çağırabildiği bir REST API'si aracılığıyla kaynaklarını kullanıma sunar. Ayrıca, DocumentDB birçok popüler dilde programlama kitaplıkları sunar. Bu kitaplıklar adresi önbelleğe alma, özel durum yönetimi, otomatik yeniden denemeler vb. gibi ayrıntıları işleyerek Azure DocumentDB ile çalışmayı birçok yönden basitleştirir. Kitaplıklar şu anda aşağıdaki diller ve platformlar için mevcuttur:  

İndirme | Belgeler
--- | ---
[.NET SDK](http://go.microsoft.com/fwlink/?LinkID=402989) | [.NET kitaplığı](https://msdn.microsoft.com/library/azure/dn948556.aspx)
[Node.js SDK'sı](http://go.microsoft.com/fwlink/?LinkID=402990) | [Node.js kitaplığı](http://azure.github.io/azure-documentdb-node/)
[Java SDK](http://go.microsoft.com/fwlink/?LinkID=402380) | [Java kitaplığı](http://azure.github.io/azure-documentdb-java/)
[JavaScript SDK'sı](http://go.microsoft.com/fwlink/?LinkID=402991) | [JavaScript kitaplığı](http://azure.github.io/azure-documentdb-js/)
yok | [Sunucu tarafı JavaScript SDK'sı](http://azure.github.io/azure-documentdb-js-server/)
[Python SDK'sı](https://pypi.python.org/pypi/pydocumentdb) | [Python kitaplığı](http://azure.github.io/azure-documentdb-python/)

DocumentDB temel oluşturma, okuma, güncelleştirme ve silme işlemlerinin ötesinde JSON belgelerini almak için zengin bir SQL sorgusu arabirimi ve JavaScript uygulama mantığının işlem tabanlı olarak yürütülmesi için sunucu tarafı desteği sağlar. Sorgu ve betik yürütme arabirimleri, REST API'lerinin yanı sıra tüm platform kitaplıkları aracılığıyla kullanılabilir. 

### SQL sorgusu
Azure DocumentDB kökü JavaScript türü sistemde bulunan bir SQL dilini kullanarak belgelerin sorgulanmasını ve ilişkisel, hiyerarşik ve uzamsal sorguları destekleyen ifadeleri destekler. DocumentDB sorgu dili, JSON belgelerini sorgulamak için basit ancak güçlü bir arabirimdir. Dil, ANSI SQL dil bilgisinin bir alt kümesini destekler ve JavaScript nesnesi, dizileri, nesne oluşturması ve işlev çağrısı için derin tümleştirme sağlar. DocumentDB geliştiriciden herhangi bir açık şema veya dizin oluşturma ipuçları almadan kendi sorgu modelini sağlar.

Kullanıcı Tanımlı İşlevler (UDF'ler) DocumentDB'ye kaydedilebilir ve bunlara bir SQL sorgusunun parçası olarak başvurulabilir, böylece dil bilgisi özel uygulama mantığını destekleyecek şekilde genişletilmiş olur. Bu UDF'ler JavaScript programları olarak yazılır ve veritabanı içinde yürütülür. 

DocumentDB .NET geliştiricileri için [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx)'sının parçası olarak bir LINQ sorgu sağlayıcısı da sunar. 

### İşlemler ve JavaScript yürütme
DocumentDB, uygulama mantığını tamamen JavaScript'te yazılmış adlandırılmış programlar olarak yazmanızı sağlar. Bu programlar bir koleksiyon için kaydedilir ve belirli bir koleksiyon içindeki belgelerde veritabanı işlemlerini yürütebilir. JavaScript bir tetikleyici, saklı yordam veya kullanıcı tanımlı işlev olarak yürütme için kaydedilebilir. Tetikleyiciler ve saklı yordamlar belgeleri oluşturabilir, okuyabilir, güncelleştirebilir ve silebilir ancak kullanıcı tanımlı işlevler, koleksiyona yazma erişimi olmaksızın sorgu yürütme mantığının parçası olarak yürütülür.

DocumentDB içinde JavaScript yürütme, ilişkisel veritabanı sistemleri tarafından desteklenen kavramlara göre modellenmiştir ve JavaScript Transact-SQL'in modern bir ardılı olarak kullanılır. Tüm JavaScript mantığı, anlık görüntü yalıtımıyla çevresel ACID işlemi içinde yürütülür. Yürütme sürecinde JavaScript bir özel durum oluşturursa tüm işlem iptal edilir.

## Sonraki adımlar
Bir Azure hesabınız zaten varsa [bir DocumentDB veritabanı hesabı oluşturarak](documentdb-create-account.md) [Azure Portal](https://portal.azure.com/#gallery/Microsoft.DocumentDB)'da DocumentDB ile çalışmaya başlayabilirsiniz.

Bir Azure hesabınız yoksa şunları yapabilirsiniz:

- Tüm Azure hizmetlerini denemek için size 30 gün ve 200 ABD doları sağlayan bir [Azure ücretsiz deneme sürümüne](https://azure.microsoft.com/pricing/free-trial/) kaydolun. 
- Bir MSDN aboneliğiniz varsa herhangi bir Azure hizmetinde kullanmak üzere [aylık 150 ABD dolarını içeren ücretsiz Azure kredisi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) hakkınız bulunur. 

Daha fazla bilgi edinmeye hazır olduğunuzda sizin için kullanılabilir olan tüm öğrenme kaynaklarına gitmek için [öğrenme yolumuzu](https://azure.microsoft.com/documentation/learning-paths/documentdb/) ziyaret edin. 


[1]: ./media/documentdb-introduction/json-database-resources1.png
 



<!----HONumber=Jun16_HO2-->


