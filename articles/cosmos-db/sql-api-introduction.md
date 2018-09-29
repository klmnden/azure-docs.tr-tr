---
title: "Azure Cosmos DB: SQL API'sine Giriş | Microsoft Docs"
description: SQL ve JavaScript kullanarak düşük gecikme süreleriyle çok büyük hacimlerdeki JSON belgelerini depolayıp sorgulamak için Azure Cosmos DB’yi nasıl kullanabileceğinizi öğrenin.
keywords: json veritabanı, belge veritabanı
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: na
ms.topic: overview
ms.date: 05/22/2017
ms.author: rafats
ms.openlocfilehash: 76545c9953cff944c515e887a6a4214b9c76c501
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47038536"
---
# <a name="introduction-to-azure-cosmos-db-sql-api"></a>Azure Cosmos DB: SQL API’si

[Azure Cosmos DB](introduction.md), Microsoft'un görev açısından kritik uygulamalar için genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Azure Cosmos DB tarafından [kullanıma hazır genel dağıtım](distribute-data-globally.md), dünya çapında [aktarım hızı ve depolama için elastik ölçeklendirme](partition-data.md), 99. yüzdebirlik dilimde tek haneli milisaniyelik gecikme süreleri, [beş iyi tanımlanmış tutarlılık düzeyi](consistency-levels.md) ve garantili yüksek kullanılabilirlik olanakları sağlanır ve bunların tamamı [sektör lideri SLA’lar](https://azure.microsoft.com/support/legal/sla/cosmos-db/) ile desteklenir. Azure Cosmos DB, şema ve dizin yönetimiyle ilgilenmenize gerek kalmadan [otomatik olarak verilerin dizinini oluşturur](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). Çok modelli olan bu hizmet belge, anahtar-değer, grafik ve sütunlu veri modellerini destekler.

![Azure SQL API](./media/sql-api-introduction/cosmosdb-sql-api.png) 

SQL API’si ile Azure Cosmos DB, şemasız JSON verileri üzerinde tutarlı düşük gecikme süreleriyle zengin ve kullanımı kolay [SQL sorgu işlevleri](sql-api-sql-query.md) sağlar. Bu makalede Azure Cosmos DB SQL API’sine genel bakış ve çok büyük hacimlerdeki JSON verilerini depolamak, milisaniyelik gecikme süresi içinde sorgulamak ve şemayı kolayca geliştirmek için nasıl kullanılabileceği hakkında bilgiler verilmektedir. 

## <a name="what-capabilities-and-key-features-does-azure-cosmos-db-offer"></a>Azure Cosmos DB’nin sunduğu yetenekler ve önemli özellikler nelerdir?
Azure Cosmos DB, SQL API’si üzerinden aşağıdaki temel işlevleri ve avantajları sunar:

* **Esnek bir şekilde ölçeklenebilir aktarım hızı ve depolama:** Uygulama gereksinimlerinizi karşılamak için JSON veritabanınızın ölçeğini kolayca artırın veya azaltın. Verileriniz düşük tahmin edilebilirliğe sahip gecikme süreleri sağlamak için katı hal disklerinde (SSD) depolanır. Azure Cosmos DB, JSON verilerini depolamak için koleksiyon adı verilen kapsayıcıları destekler; bu kapsayıcılar neredeyse sınırsız depolama boyutlarına ve sağlanan aktarım hızına ölçeklenebilir. Uygulamanız büyüdükçe, Azure Cosmos DB'yi tahmin edilebilir performansla sorunsuz ve esnek bir şekilde ölçeklendirebilirsiniz. 


* **Çok bölgeli çoğaltma:** Azure Cosmos DB, verilerinizi Azure Cosmos DB hesabınızla ilişkilendirdiğiniz tüm bölgelere şeffaf biçimde çoğaltır ve tutarlılık, kullanılabilirlik ve performansın hepsi için garantili bir denge sağlarken verilere genel erişim gerektiren uygulamalar geliştirmenize imkan tanır. Azure Cosmos DB çok girişli API’ler ile şeffaf bölgesel yük devretme olanağının yanı sıra aktarım hızını ve depolamayı dünya çapında elastik bir şekilde ölçeklendirme imkanı sağlar. [Azure Cosmos DB ile verileri küresel ölçekte dağıtma](distribute-data-globally.md) bölümünde daha fazla bilgi edinin.

* **Tanıdık SQL söz dizimi ile geçici sorgular:** Heterojen JSON belgelerini depolayın ve bilindik bir SQL söz dizimi aracılığıyla bu belgeleri sorgulayın. Azure Cosmos DB, tüm belge içeriğinin otomatik olarak dizinini oluşturmak için yüksek derecede eşzamanlı, kilitsiz, günlük yapılı bir dizin oluşturma teknolojisi kullanır. Böylece şema ipuçları, ikincil dizinler veya görünümler belirtmek gerekmeden gerçek zamanlı zengin sorgulara olanak sağlanır. [Azure Cosmos DB’yi sorgulama](sql-api-sql-query.md) bölümünden daha fazla bilgi edinin. 
* **Veritabanı içinde JavaScript yürütme:** Standart JavaScript'i kullanarak uygulama mantığını saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler (UDF'ler) olarak ifade edin. Böylece uygulama ve veritabanı şeması arasındaki uyumsuzluk hakkında endişelenmeye gerek kalmadan uygulama mantığınızın veriler üzerinde çalışması sağlanır. SQL API’si, doğrudan veritabanı altyapısının içinde JavaScript uygulama mantığının tam işlem tabanlı olarak yürütülmesini sağlar. JavaScript derin tümleştirmesi YERLEŞTİRME, DEĞİŞTİRME, SİLME ve SEÇME işlemlerinin bir JavaScript programı içinden yalıtılmış bir işlem olarak yürütülmesini sağlar. Daha fazla bilgi için bkz. [SQL sunucu tarafı programlama](programming.md).

* **İnce ayarlanabilir tutarlılık düzeyleri:** Tutarlılık ve performans arasında en iyi dengeyi elde etmek için iyi tanımlanmış beş tutarlılık düzeyi arasından seçim yapın. Azure Cosmos DB sorgular ve okuma işlemleri için beş farklı tutarlılık düzeyi sunar: güçlü, sınırlanmış eskime durumu, oturum, tutarlı ön ek ve son. Bu ayrıntılı ve iyi tanımlanmış tutarlılık düzeyleri tutarlılık, kullanılabilirlik ve gecikme süresi arasında sağlam bir denge kurmanıza olanak sağlar. Daha fazla bilgi için bkz. [Kullanılabilirlik ve performansı en üst düzeye çıkarmak için tutarlılık düzeylerini kullanma](consistency-levels.md).

* **Tam olarak yönetilme:** Veritabanı ve makine kaynaklarını yönetme ihtiyacını ortadan kaldırın. Bu tam olarak yönetilen bir Microsoft Azure hizmeti olduğundan sanal makineleri yönetmeniz, yazılımları dağıtıp yapılandırmanız, ölçeklendirmeyi yönetmeniz veya karmaşık veri katmanı yükseltmeleriyle uğraşmanız gerekmez. Tüm veritabanları otomatik olarak yedeklenir ve bölgesel arızalara karşı korunur. İhtiyacınız oldukça kolaylıkla bir Azure Cosmos DB hesabı ve sağlama kapasitesi ekleyebilirsiniz, böylece veritabanınızı çalıştırmak ve yönetmek yerine uygulamanıza odaklanmanız sağlanır. 

* **Tasarımı gereği açık:** Var olan becerileri ve araçları kullanarak hızlı bir şekilde çalışmaya başlayın. SQL API'sinde programlama basittir, ulaşılabilirdir ve yeni araçları benimsemenizi veya JSON ya da JavaScript'e yönelik özel uzantılara bağlı kalmanızı gerektirmez. CRUD, sorgu ve JavaScript işleme dahil olmak üzere tüm veritabanı işlevlerine basit bir RESTful HTTP arabirimi üzerinden erişebilirsiniz. SQL API’si var olan biçimleri, dilleri ve standartları benimserken bunlara ek olarak yüksek değerde veritabanı işlevleri sunar.

* **Otomatik dizin oluşturma:** Azure Cosmos DB, varsayılan olarak veritabanındaki tüm belgelerin otomatik olarak dizinini oluşturur ve herhangi bir şemayı ya da ikincil dizinlerin oluşturulmasını beklemez veya gerektirmez. Her şeyi dizine eklemek istemiyor musunuz? Merak etmeyin, [JSON dosyalarınızda yolları iptal de edebilirsiniz](indexing-policies.md).

* **Değişiklik akışı desteği:** Değişiklik akışı, bir Azure Cosmos DB koleksiyonunda düzenlenme sırasına göre sıralanmış bir belge listesi sağlar. Bu akış, verileri çoğaltmak, API çağrıları tetiklemek veya güncelleştirmelerde akış işleme gerçekleştirmek için veri değişikliklerini dinlemek amacıyla kullanılabilir. Değişiklik akışı otomatik olarak etkinleştirilir ve kullanımı kolaydır: [Değişiklik akışı hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/cosmos-db/change-feed). 

## <a name="data-management"></a>SQL API’si ile verileri nasıl yönetebilirsiniz?
SQL API’si, JSON verilerinin iyi tanımlanmış veritabanı kaynakları aracılığıyla yönetilmesine yardımcı olur. Bu kaynaklar yüksek kullanılabilirlik için çoğaltılır ve mantıksal URI'leri ile benzersiz olarak adreslenebilir. SQL API’si, tüm kaynaklar için basit bir HTTP tabanlı RESTful programlama modeli sunar. 


Azure Cosmos DB veritabanı hesabı, size Azure Cosmos DB erişimi sağlayan benzersiz bir ad alanıdır. Bir veritabanı hesabı oluşturabilmeniz için, öncelikle çeşitli Azure hizmetlerine erişim sağlayan bir Azure aboneliğinizin olması gerekir. 

Azure Cosmos DB içindeki tüm kaynaklar JSON belgeleri olarak modellenir ve depolanır. Kaynaklar meta veriler içeren JSON belgeleri olan öğeler şeklinde, aynı zamanda öğe koleksiyonları olan akışlar şeklinde yönetilir. Öğe kümeleri ilgili akışlarının içinde yer alır.

Aşağıdaki görüntüde Azure Cosmos DB kaynakları arasındaki ilişkiler gösterilmektedir:

![Azure Cosmos DB’de kaynaklar arasındaki hiyerarşi ilişkisi][1] 

Bir veritabanı hesabı, her biri saklı yordamlar, tetikleyiciler, UDF'ler, belgeler ve ilgili ekler içerebilen birden çok koleksiyonu kapsayan bir veritabanları kümesinden oluşur. Bir veritabanı çeşitli diğer koleksiyonlara, saklı yordamlara, tetikleyicilere, UDF'lere, belgelere veya eklere erişmek için her biri bir izinler kümesine sahip olan ilgili kullanıcıları da içerir. Veritabanları, kullanıcılar, izinler ve koleksiyonlar iyi bilinen şemalar sahip sistem tanımlı kaynaklardır; belgeler, saklı yordamlar, tetikleyiciler, UDF'ler ve eklerde ise rastgele ve kullanıcı tanımlı JSON içeriği bulunur.  

## <a name="develop"></a>SQL API’si ile nasıl uygulama geliştirebilirim?

Azure Cosmos DB, HTTP/HTTPS istekleri yapabilen herhangi bir dil tarafından çağrılabilen REST API'ler aracılığıyla kaynaklarını kullanıma sunar. Ayrıca, SQL API’si için birçok popüler dilde programlama kitaplıkları sunuyoruz. İstemci kitaplıkları, adresi önbelleğe alma, özel durum yönetimi, otomatik yeniden denemeler vb. gibi ayrıntıları işleyerek API ile çalışmayı birçok yönden basitleştirir. Kitaplıklar şu anda aşağıdaki diller ve platformlar için mevcuttur:  

| İndirme | Belgeler |
| --- | --- |
| [.NET SDK](http://go.microsoft.com/fwlink/?LinkID=402989) |[.NET kitaplığı](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) |
| [Node.js SDK’sı](http://go.microsoft.com/fwlink/?LinkID=402990) |[Node.js kitaplığı](https://github.com/Azure/azure-cosmosdb-node) |
| [Java SDK](http://go.microsoft.com/fwlink/?LinkID=402380) |[Java kitaplığı](/java/api/com.microsoft.azure.documentdb) |
| [JavaScript SDK'sı](https://github.com/Azure/azure-cosmos-js) |[JavaScript kitaplığı](https://github.com/Azure/azure-cosmos-js) |
| yok |[Sunucu tarafı JavaScript SDK'sı](https://github.com/Azure/azure-cosmosdb-js-server) |
| [Python SDK'sı](https://pypi.python.org/pypi/pydocumentdb) |[Python kitaplığı](https://github.com/Azure/azure-cosmos-python) |
| yok | [MongoDB API’si](mongodb-introduction.md)


[Azure Cosmos DB Öykünücüsü](local-emulator.md)’nü kullanarak Azure aboneliği oluşturmadan veya masraf yapmadan uygulamanızı SQL API’si ile yerel ortamda geliştirip test edebilirsiniz. Uygulamanızın öykünücüdeki performansından memnun olduğunuzda bulut üzerinde Azure Cosmos DB hesabı kullanmaya başlayabilirsiniz.

SQL API’si temel oluşturma, okuma, güncelleştirme ve silme işlemlerinin ötesinde JSON belgelerini almak için zengin bir SQL sorgusu arabirimi ve JavaScript uygulama mantığının işlem tabanlı olarak yürütülmesi için sunucu tarafı desteği sağlar. Sorgu ve betik yürütme arabirimleri, REST API'lerinin yanı sıra tüm platform kitaplıkları aracılığıyla kullanılabilir. 

### <a name="sql-query"></a>SQL sorgusu
Azure Cosmos DB, kökü JavaScript türü sistemde bulunan bir SQL dilini kullanarak belgelerin sorgulanmasını ve ilişkisel, hiyerarşik ve uzamsal sorguları destekleyen ifadeleri destekler. Azure Cosmos sorgu dili, JSON belgelerini sorgulamak için basit ancak güçlü bir arabirimdir. Dil, ANSI SQL dil bilgisinin bir alt kümesini destekler ve JavaScript nesnesi, dizileri, nesne oluşturması ve işlev çağrısı için derin tümleştirme sağlar. SQL API’si, geliştiriciden herhangi bir açık şema veya dizin oluşturma ipuçları almadan kendi sorgu modelini sağlar.

Kullanıcı Tanımlı İşlevler (UDF'ler) SQL API'sine kaydedilebilir ve bunlara bir SQL sorgusunun parçası olarak başvurulabilir, böylece dil bilgisi özel uygulama mantığını destekleyecek şekilde genişletilmiş olur. Bu UDF'ler JavaScript programları olarak yazılır ve veritabanı içinde yürütülür. 

.NET geliştiricileri için SQL API [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx)'sı bir LINQ sorgu sağlayıcısı da sunar. 

### <a name="transactions-and-javascript-execution"></a>İşlemler ve JavaScript yürütme
SQL API’si, uygulama mantığını tamamen JavaScript'te yazılmış adlandırılmış programlar olarak yazmanızı sağlar. Bu programlar bir koleksiyon için kaydedilir ve belirli bir koleksiyon içindeki belgelerde veritabanı işlemlerini yürütebilir. JavaScript bir tetikleyici, saklı yordam veya kullanıcı tanımlı işlev olarak yürütme için kaydedilebilir. Tetikleyiciler ve saklı yordamlar belgeleri oluşturabilir, okuyabilir, güncelleştirebilir ve silebilir ancak kullanıcı tanımlı işlevler, koleksiyona yazma erişimi olmaksızın sorgu yürütme mantığının parçası olarak yürütülür.

Cosmos DB içinde JavaScript yürütme, ilişkisel veritabanı sistemleri tarafından desteklenen kavramlara göre modellenmiştir ve JavaScript Transact-SQL'in modern bir ardılı olarak kullanılır. Tüm JavaScript mantığı, anlık görüntü yalıtımıyla çevresel ACID işlemi içinde yürütülür. Yürütme sürecinde JavaScript bir özel durum oluşturursa tüm işlem iptal edilir.

## <a name="next-steps"></a>Sonraki adımlar
Zaten bir Azure hesabınız var mı? Bundan sonra, hesap oluşturma ve Cosmos DB ile çalışmaya başlama konusunda size kılavuzluk edecek [hızlı başlangıçlarımızı](../cosmos-db/create-sql-api-dotnet.md) takip ederek Azure Cosmos DB’yi kullanmaya başlayabilirsiniz.

[1]: ./media/sql-api-introduction/json-database-resources1.png

