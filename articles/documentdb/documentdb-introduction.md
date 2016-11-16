---
title: "Bir JSON veritabanı olan DocumentDB&quot;ye giriş | Microsoft Belgeleri"
description: "Bir NoSQL JSON veritabanı olan Azure DocumentDB hakkında bilgi edinin. Bu belge veritabanı büyük veri, esnek ölçeklenebilirlik ve yüksek kullanılabilirlik için oluşturulmuştur."
keywords: "json veritabanı, belge veritabanı"
services: documentdb
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 686cdd2b-704a-4488-921e-8eefb70d5c63
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/13/2016
ms.author: mimig
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 23a6be60d7bf8fa47589fffb5132a09994d33d4c


---
# <a name="introduction-to-documentdb-a-nosql-json-database"></a>DocumentDB'ye giriş: Bir NoSQL JSON Veritabanı
## <a name="what-is-documentdb"></a>DocumentDB nedir?
DocumentDB hızlı ve tahmin edilebilir performans, yüksek kullanılabilirlik, esnek ölçeklendirme, genel dağıtım ve geliştirme kolaylığı için oluşturulmuş tam olarak yönetilen bir NoSQL veritabanı hizmetidir. DocumentDB, şemasız bir NoSQL veritabanı olarak JSON verilerinde tutarlı bir şekilde düşük gecikme sürelerine sahip zengin ve tanıdık SQL sorgusu özellikleri sunar. Okuma işlemlerinizin %99’unun 10 milisaniyeden kısa bir sürede ve yazma işlemlerinizin %99’unun 15 milisaniyeden kısa bir sürede işlenmesini sağlar. Bu benzersiz avantajları DocumentDB’yi web, mobil, oyun, IoT ve sorunsuz ölçeklendirme ile küresel çoğaltma ihtiyacı olan pek çok diğer uygulama için ideal bir çözüm yapar.

## <a name="how-can-i-learn-about-documentdb"></a>DocumentDB hakkında nasıl bilgi edinebilirim?
DocumentDB hakkında bilgi edinmenin ve DocumentDB’yi çalışırken görmenin hızlı bir yolu şu üç adımı takip etmektir: 

1. DocumentDB kullanmanın avantajlarını tanıtan iki dakikalık [DocumentDB nedir?](https://azure.microsoft.com/documentation/videos/what-is-azure-documentdb/) videosunu izleyin.
2. Azure Portal'ı kullanarak DocumentDB kullanmaya nasıl başlayacağınızı vurgulayan üç dakikalık [Azure'da DocumentDB oluşturma](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) videosunu izleyin.
3. DocumentDB'de kullanılabilen zengin sorgulama işlevi hakkında bilgi edinmek için farklı etkinlikleri inceleyebileceğiniz [Query Playground](http://www.documentdb.com/sql/demo)'u ziyaret edin. Ardından, Korumalı Alan sekmesine giderek kendi SQL sorgularınızı çalıştırın ve DocumentDB'yi deneyin.

Ardından, konuyu daha da ayrıntılı ele alacağımız bu makaleye dönün.  

## <a name="what-capabilities-and-key-features-does-documentdb-offer"></a>DocumentDB sunduğu yetenekler ve önemli özellikler nelerdir?
Azure DocumentDB aşağıdaki temel işlevleri ve avantajları sunar:

* **Esnek bir şekilde ölçeklenebilir işleme ve depolama:** Uygulama gereksinimlerinizi karşılamak için DocumentDB JSON veritabanınızın ölçeğini kolayca artırın veya azaltın. Verileriniz düşük tahmin edilebilirliğe sahip gecikme süreleri sağlamak için katı hal disklerinde (SSD) depolanır. DocumentDB, JSON verilerini depolamak için koleksiyon adı verilen kapsayıcıları destekler; bu kapsayıcılar neredeyse sınırsız depolama boyutlarına ve sağlanan işlemeye ölçeklenebilir. Uygulamanız büyüdükçe, DocumentDB'yi tahmin edilebilir performansla sorunsuz ve esnek bir şekilde ölçeklendirebilirsiniz. 
* **Çok bölgeli çoğaltma:** DocumentDB, verilerinizi DocumentDB hesabınızla ilişkilendirdiğiniz tüm bölgelere şeffaf biçimde çoğaltır ve tutarlılık, kullanılabilirlik ve performansın hepsi için garantili bir denge sağlarken verilere genel erişim gerektiren uygulamalar geliştirmenize imkan tanır. DocumentDB çok girişli API’ler ile şeffaf bölgesel yük devretme ve verimlilik ile depolamayı dünya çapında elastik bir şekilde ölçeklendirme imkanı sağlar. [DocumentDB ile verileri küresel ölçekte dağıtma](documentdb-distribute-data-globally.md) bölümünde daha fazla bilgi edinin.
* **Tanıdık SQL söz dizimi ile geçici sorgular:** DocumentDB içinde heterojen JSON belgelerini depolayın ve tanıdık bir SQL söz dizimi aracılığıyla bu belgeleri sorgulayın. DocumentDB, tüm belge içeriğinin otomatik olarak dizinini oluşturmak için yüksek derecede eşzamanlı, kilitsiz, günlük yapılı bir dizin oluşturma teknolojisi kullanır. Böylece şema ipuçları, ikincil dizinler veya görünümler belirtmek gerekmeden gerçek zamanlı zengin sorgulara olanak sağlanır. Daha fazla bilgi için bkz. [DocumentDB'yi sorgulama](documentdb-sql-query.md). 
* **Veritabanı içinde JavaScript yürütme:** Standart JavaScript'i kullanarak uygulama mantığını saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler (UDF'ler) olarak ifade edin. Böylece uygulama ve veritabanı şeması arasındaki uyumsuzluk hakkında endişelenmeye gerek kalmadan uygulama mantığınızın veriler üzerinde çalışması sağlanır. DocumentDB, doğrudan veritabanı altyapısının içinde JavaScript uygulama mantığının tam işlem tabanlı olarak yürütülmesini sağlar. JavaScript derin tümleştirmesi YERLEŞTİRME, DEĞİŞTİRME, SİLME ve SEÇME işlemlerinin bir JavaScript programı içinden yalıtılmış bir işlem olarak yürütülmesini sağlar. Daha fazla bilgi için bkz. [DocumentDB sunucu tarafı programlama](documentdb-programming.md).
* **İnce ayarlanabilir tutarlılık düzeyleri:** Tutarlılık ve performans arasında en iyi dengeyi elde etmek için iyi tanımlanmış dört tutarlılık düzeyi arasından seçim yapın. DocumentDB sorgular ve okuma işlemleri için dört farklı tutarlılık düzeyi sunar: güçlü, sınırlanmış eskime durumu, oturum ve son. Bu ayrıntılı ve iyi tanımlanmış tutarlılık düzeyleri tutarlılık, kullanılabilirlik ve gecikme süresi arasında sağlam bir denge kurmanıza olanak sağlar. Daha fazla bilgi için bkz. [DocumentDB'de kullanılabilirlik ve performansı en üst düzeye çıkarmak için tutarlılık düzeylerini kullanma](documentdb-consistency-levels.md).
* **Tam olarak yönetilme:** Veritabanı ve makine kaynaklarını yönetme ihtiyacını ortadan kaldırın. Bu tam olarak yönetilen bir Microsoft Azure hizmeti olduğundan sanal makineleri yönetmeniz, yazılımları dağıtıp yapılandırmanız, ölçeklendirmeyi yönetmeniz veya karmaşık veri katmanı yükseltmeleriyle uğraşmanız gerekmez. Tüm veritabanları otomatik olarak yedeklenir ve bölgesel arızalara karşı korunur. İhtiyacınız oldukça kolaylıkla bir DocumentDB hesabı ve sağlama kapasitesi ekleyebilirsiniz, böylece veritabanınızı çalıştırmak ve yönetmek yerine uygulamanıza odaklanmanız sağlanır. 
* **Tasarımı gereği açık:** Var olan becerileri ve araçları kullanarak hızlı bir şekilde çalışmaya başlayın. DocumentDB'de programlama basittir, ulaşılabilirdir ve yeni araçları benimsemenizi veya JSON ya da JavaScript'e yönelik özel uzantılara bağlı kalmanızı gerektirmez. CRUD, sorgu ve JavaScript işleme dahil olmak üzere tüm veritabanı işlevlerine basit bir RESTful HTTP arabirimi üzerinden erişebilirsiniz. DocumentDB var olan biçimleri, dilleri ve standartları benimserken bunlara ek olarak yüksek değerde veritabanı işlevleri sunar.
* **Otomatik dizin oluşturma:** DocumentDB, varsayılan olarak veritabanındaki tüm belgelerin [otomatik olarak dizinini oluşturur](documentdb-indexing.md) ve herhangi bir şemayı ya da ikincil dizinlerin oluşturulmasını beklemez veya gerektirmez. Her şeyi dizine eklemek istemiyor musunuz? Merak etmeyin, [JSON dosyalarınızda yolları iptal de edebilirsiniz](documentdb-indexing-policies.md).

## <a name="a-namedatamanagementahow-does-documentdb-manage-data"></a><a name="data-management"></a>DocumentDB verileri nasıl yönetir?
Azure DocumentDB, JSON verilerini iyi tanımlanmış veritabanı kaynakları aracılığıyla yönetir. Bu kaynaklar yüksek kullanılabilirlik için çoğaltılır ve mantıksal URI'leri ile benzersiz olarak adreslenebilir. DocumentDB tüm kaynaklar için basit bir HTTP tabanlı RESTful programlama modeli sunar. 

DocumentDB veritabanı hesabı, size Azure DocumentDB erişimi sağlayan benzersiz bir ad alanıdır. Bir veritabanı hesabı oluşturabilmeniz için, öncelikle çeşitli Azure hizmetlerine erişim sağlayan bir Azure aboneliğinizin olması gerekir. 

DocumentDB içindeki tüm kaynaklar JSON belgeleri olarak modellenir ve depolanır. Kaynaklar meta veriler içeren JSON belgeleri olan öğeler şeklinde, aynı zamanda öğe koleksiyonları olan akışlar şeklinde yönetilir. Öğe kümeleri ilgili akışlarının içinde yer alır.

Aşağıdaki görüntüde DocumentDB kaynakları arasındaki ilişkiler gösterilmektedir:

![Bir NoSQL JSON veritabanı olan DocumentDB'de kaynaklar arasındaki hiyerarşi ilişkisi][1] 

Bir veritabanı hesabı, her biri saklı yordamlar, tetikleyiciler, UDF'ler, belgeler ve ilgili ekler içerebilen birden çok koleksiyonu kapsayan bir veritabanları kümesinden oluşur. Bir veritabanı çeşitli diğer koleksiyonlara, saklı yordamlara, tetikleyicilere, UDF'lere, belgelere veya eklere erişmek için her biri bir izinler kümesine sahip olan ilgili kullanıcıları da içerir. Veritabanları, kullanıcılar, izinler ve koleksiyonlar iyi bilinen şemalar sahip sistem tanımlı kaynaklardır; belgeler, saklı yordamlar, tetikleyiciler, UDF'ler ve eklerde ise rastgele ve kullanıcı tanımlı JSON içeriği bulunur.  

## <a name="a-namedevelopa-how-can-i-develop-apps-with-documentdb"></a><a name="develop"></a>DocumentDB ile nasıl uygulama geliştirebilirim?
Azure DocumentDB, HTTP/HTTPS istekleri yapabilen herhangi bir dilin çağırabildiği bir REST API'si aracılığıyla kaynaklarını kullanıma sunar. Ayrıca, DocumentDB birçok popüler dilde programlama kitaplıkları sunar. Bu kitaplıklar adresi önbelleğe alma, özel durum yönetimi, otomatik yeniden denemeler vb. gibi ayrıntıları işleyerek Azure DocumentDB ile çalışmayı birçok yönden basitleştirir. Kitaplıklar şu anda aşağıdaki diller ve platformlar için mevcuttur:  

| İndirme | Belgeler |
| --- | --- |
| [.NET SDK](http://go.microsoft.com/fwlink/?LinkID=402989) |[.NET kitaplığı](https://msdn.microsoft.com/library/azure/dn948556.aspx) |
| [Node.js SDK’sı](http://go.microsoft.com/fwlink/?LinkID=402990) |[Node.js kitaplığı](http://azure.github.io/azure-documentdb-node/) |
| [Java SDK](http://go.microsoft.com/fwlink/?LinkID=402380) |[Java kitaplığı](http://azure.github.io/azure-documentdb-java/) |
| [JavaScript SDK'sı](http://go.microsoft.com/fwlink/?LinkID=402991) |[JavaScript kitaplığı](http://azure.github.io/azure-documentdb-js/) |
| yok |[Sunucu tarafı JavaScript SDK'sı](http://azure.github.io/azure-documentdb-js-server/) |
| [Python SDK'sı](https://pypi.python.org/pypi/pydocumentdb) |[Python kitaplığı](http://azure.github.io/azure-documentdb-python/) |

DocumentDB temel oluşturma, okuma, güncelleştirme ve silme işlemlerinin ötesinde JSON belgelerini almak için zengin bir SQL sorgusu arabirimi ve JavaScript uygulama mantığının işlem tabanlı olarak yürütülmesi için sunucu tarafı desteği sağlar. Sorgu ve betik yürütme arabirimleri, REST API'lerinin yanı sıra tüm platform kitaplıkları aracılığıyla kullanılabilir. 

### <a name="sql-query"></a>SQL sorgusu
Azure DocumentDB kökü JavaScript türü sistemde bulunan bir SQL dilini kullanarak belgelerin sorgulanmasını ve ilişkisel, hiyerarşik ve uzamsal sorguları destekleyen ifadeleri destekler. DocumentDB sorgu dili, JSON belgelerini sorgulamak için basit ancak güçlü bir arabirimdir. Dil, ANSI SQL dil bilgisinin bir alt kümesini destekler ve JavaScript nesnesi, dizileri, nesne oluşturması ve işlev çağrısı için derin tümleştirme sağlar. DocumentDB geliştiriciden herhangi bir açık şema veya dizin oluşturma ipuçları almadan kendi sorgu modelini sağlar.

Kullanıcı Tanımlı İşlevler (UDF'ler) DocumentDB'ye kaydedilebilir ve bunlara bir SQL sorgusunun parçası olarak başvurulabilir, böylece dil bilgisi özel uygulama mantığını destekleyecek şekilde genişletilmiş olur. Bu UDF'ler JavaScript programları olarak yazılır ve veritabanı içinde yürütülür. 

DocumentDB .NET geliştiricileri için [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx)'sının parçası olarak bir LINQ sorgu sağlayıcısı da sunar. 

### <a name="transactions-and-javascript-execution"></a>İşlemler ve JavaScript yürütme
DocumentDB, uygulama mantığını tamamen JavaScript'te yazılmış adlandırılmış programlar olarak yazmanızı sağlar. Bu programlar bir koleksiyon için kaydedilir ve belirli bir koleksiyon içindeki belgelerde veritabanı işlemlerini yürütebilir. JavaScript bir tetikleyici, saklı yordam veya kullanıcı tanımlı işlev olarak yürütme için kaydedilebilir. Tetikleyiciler ve saklı yordamlar belgeleri oluşturabilir, okuyabilir, güncelleştirebilir ve silebilir ancak kullanıcı tanımlı işlevler, koleksiyona yazma erişimi olmaksızın sorgu yürütme mantığının parçası olarak yürütülür.

DocumentDB içinde JavaScript yürütme, ilişkisel veritabanı sistemleri tarafından desteklenen kavramlara göre modellenmiştir ve JavaScript Transact-SQL'in modern bir ardılı olarak kullanılır. Tüm JavaScript mantığı, anlık görüntü yalıtımıyla çevresel ACID işlemi içinde yürütülür. Yürütme sürecinde JavaScript bir özel durum oluşturursa tüm işlem iptal edilir.

## <a name="next-steps"></a>Sonraki adımlar
Zaten bir Azure hesabınız var mı? O halde, [bir DocumentDB veritabanı hesabı oluşturarak](documentdb-create-account.md) [Azure Portal](https://portal.azure.com/#gallery/Microsoft.DocumentDB)'da DocumentDB ile çalışmaya başlayabilirsiniz.

Azure hesabınız yok mu? Şunları yapabilirsiniz:

* Tüm Azure hizmetlerini denemek için size 30 gün ve 200 ABD doları sağlayan bir [Azure ücretsiz deneme sürümüne](https://azure.microsoft.com/free/) kaydolun. 
* Bir MSDN aboneliğiniz varsa herhangi bir Azure hizmetinde kullanmak üzere [aylık 150 ABD dolarını içeren ücretsiz Azure kredisi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) hakkınız bulunur. 

Daha fazla bilgi edinmeye hazır olduğunuzda sizin için kullanılabilir olan tüm öğrenme kaynaklarına gitmek için [öğrenme yolumuzu](https://azure.microsoft.com/documentation/learning-paths/documentdb/) ziyaret edin. 

[1]: ./media/documentdb-introduction/json-database-resources1.png




<!--HONumber=Nov16_HO2-->


