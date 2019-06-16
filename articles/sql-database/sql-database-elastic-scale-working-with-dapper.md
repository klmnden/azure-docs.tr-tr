---
title: Dapper ile esnek veritabanı istemci kitaplığını kullanma | Microsoft Docs
description: Dapper ile esnek veritabanı istemci kitaplığı kullanılarak.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: c6ca7637c8e251fa29781503ffc18227c51bb4da
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60335304"
---
# <a name="using-elastic-database-client-library-with-dapper"></a>Dapper ile esnek veritabanı istemci kitaplığı kullanma
Bu belge uygulamalar üzerinde Dapper kullanır ancak ayrıca Kucak isteyen geliştiricilere yöneliktir [esnek veritabanı araçları](sql-database-elastic-scale-introduction.md) kendi veri katmanı genişletmek için bu uygulama parçalama uygulamalar oluşturmak için.  Bu belge Dapper tabanlı uygulamalar, elastik veritabanı araçları ile tümleştirme gereken değişiklikler gösterilmektedir. Elastik veritabanı parça yönetimi ve Dapper ile verilere bağımlı yönlendirme buradaki kazanmasının olur. 

**Örnek kod**: [Azure SQL veritabanı - Dapper tümleştirme için esnek veritabanı araçlarını](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-e19fc77f).

Tümleştirme **Dapper** ve **DapperExtensions** ile esnek veritabanı istemci kitaplığı Azure SQL veritabanı için kolaydır. Uygulamalarınızı oluşturma değiştirme ve, yeni açarak verilere bağımlı yönlendirme kullanabilirsiniz [SqlConnection](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) nesneleri [OpenConnectionForKey](https://msdn.microsoft.com/library/azure/dn807226.aspx) çağırmanıza [istemci kitaplığı ](https://msdn.microsoft.com/library/azure/dn765902.aspx). Yeni bir bağlantı oluşturulur ve açık olduğunda bu yalnızca uygulamanızdaki değişiklikleri sınırlar. 

## <a name="dapper-overview"></a>Dapper genel bakış
**Dapper** bir nesne-ilişkisel eşleyicisidir. Bu, .NET nesnelerini ilişkisel veritabanına (ve tersi) uygulamanızın eşler. Elastik veritabanı istemci kitaplığı nasıl Dapper tabanlı uygulamalarıyla tümleştirebilirsiniz örnek kodun ilk bölümünü göstermektedir. Örnek kodu ikinci bölümü, hem Dapper hem de DapperExtensions kullanırken tümleştirmek gösterilmektedir.  

Dapper Eşleyici İşlevler, yürütme veya veritabanını sorgulama için gönderen T-SQL bildirimleri basitleştirin, veritabanı bağlantıları üzerinde genişletme yöntemleri sağlar. Örneğin Dapper .NET nesnelerinizi ve parametreler için SQL deyimlerinin arasında eşleme kolaylaştırır **yürütme** çağrıları, veya .NET nesneleri kullanarak sonuçları SQL sorgularınızı tüketmeye **sorgu** Dapper gelen çağrıları. 

DapperExtensions kullanırken, artık SQL deyimlerini sağlamanız gerekir. Uzantı yöntemleri gibi **GetList** veya **Ekle** veritabanı bağlantısı üzerinden arka planda SQL deyimleri oluşturma.

Dapper hem de DapperExtensions başka bir avantajı, uygulama veritabanı bağlantısı oluşturulmasını denetleyen içindir. Bu aracıları veritabanı bağlantıları parçacıklara veritabanlarına eşleme temel esnek veritabanı istemci kitaplığı ile etkileşime yardımcı olur.

Dapper derlemeleri almak için bkz. [Dapper dot net](https://www.nuget.org/packages/Dapper/). Dapper uzantıları için bkz: [DapperExtensions](https://www.nuget.org/packages/DapperExtensions).

## <a name="a-quick-look-at-the-elastic-database-client-library"></a>Elastik veritabanı istemci kitaplığı hızlı bir bakış
Elastik veritabanı istemci kitaplığı ile uygulama verilerinizi adlı bölümlerini tanımladığınız *parçacıklara*, bunları veritabanlarıyla eşleyin ve onlar tarafından belirleme *parçalama anahtarları*. Gerekir ve bu veritabanları arasında parçacıklara dağıtmak kadar veritabanı olabilir. Parçalama anahtarı değerleri veritabanlarına eşleme kitaplığın API'leri tarafından sağlanan bir parça eşlemesi tarafından depolanır. Bu özellik adında **parça eşleme Yönetimi**. Parça eşlemesi, ayrıca bir parçalama anahtarı taşıma istekler için veritabanı bağlantı aracısı olarak işlev görür. Bu özellik olarak adlandırılır **verilere bağımlı yönlendirme**.

![Parça eşlemeleri ve verilere bağımlı yönlendirme][1]

Parça eşleme Yöneticisi kullanıcı veritabanlarında eşzamanlı parçacık yönetim işlemlerini olay meydana gelir gerektiğinde oluşabilecek parçacık verilerin tutarsız görünümleri korur. Bunu yapmak için veritabanı bağlantıları kitaplığı ile oluşturulmuş bir uygulama için parça eşlemesi aracı. Parça yönetim işlemlerini parçacık etkileyebilir, bu bir veritabanı bağlantısı otomatik olarak sonlandırmak parça Haritası işlevini sağlar. 

Dapper için bağlantılar oluşturmak için geleneksel biçimini kullanmak yerine, kullanmanız gereken [OpenConnectionForKey yöntemi](https://msdn.microsoft.com/library/azure/dn824099.aspx). Bu, tüm doğrulama gerçekleşir ve verileri parçalar arasında taşındığında bağlantılar düzgün bir şekilde yönetilir sağlar.

### <a name="requirements-for-dapper-integration"></a>Dapper tümleştirme gereksinimleri
Elastik veritabanı istemci kitaplığı ve Dapper API'leri ile çalışırken, aşağıdaki özellikleri korumak istediğiniz:

* **Ölçeği genişletme**: Veritabanları kapasite gereksinimlerini karşılamak için gerektiği şekilde parçalı uygulama uygulamanın veri katmanından ekleyip istiyoruz. 
* **Tutarlılık**: Parçalama kullanarak uygulama ölçeklendirilir, verilere bağımlı yönlendirme gerçekleştirmek gerekir. Bunu yapmak için kitaplık verilere bağımlı yönlendirme yeteneklerini kullanılacak istiyoruz. Özellikle, doğrulama saklamak istediğiniz ve parça eşleme Yöneticisi Bozulması veya yanlış sorgu sonuçları önlemek için aracılı bağlantılar tarafından sağlanan tutarlılığını garanti eder. Bu, belirli bir parçacık bağlantı reddedildi veya (örneğin) bir parçacık ayırma/birleştirme API'lerini kullanarak farklı bir parçaya şu anda taşınırsa durduruldu sağlar.
* **Eşleme nesnesi**: Dapper uygulama sınıfları ve temel alınan veritabanı yapılarını arasında çeviri yapmak için sağladığı eşlemeleri kolaylık elde tutmak istiyoruz. 

Aşağıdaki bölümde dayalı uygulamalar için bu gereksinimleri yönelik yönergeler sağlanmaktadır **Dapper** ve **DapperExtensions**.

## <a name="technical-guidance"></a>Teknik rehberlik
### <a name="data-dependent-routing-with-dapper"></a>Dapper ile verilere bağımlı yönlendirme
Dapper ile uygulama oluşturmayı ve temel alınan veritabanı bağlantılarını açmak için genelde sorumludur. .NET koleksiyonlarına türü t Dapper gerçekleştirir eşleme T-SQL sonuç sütunları t türü nesneler için uygulama tarafından belirli bir türe T, Dapper sorgu sonuçlarını döndürür Benzer şekilde, Dapper .NET nesnelerini SQL değerleri veya veri işleme dili (DML) deyimleri için parametreleri eşler. Dapper normal üzerinde genişletme yöntemleri aracılığıyla bu işlevselliği sunar [SqlConnection](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) ADO .NET SQL istemci kitaplıklarından nesne. DDR için esnek ölçeklendirme API'leri tarafından döndürülen SQL bağlantısı normal ayrıca [SqlConnection](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) nesneleri. Bu basit bir SQL istemcisi bağlantı de olduğu gibi doğrudan Dapper uzantıları üzerinden istemci kitaplığının DDR API'si tarafından döndürülen tür kullanacak olanak tanır.

Bu gözlemler kullanımı için Dapper ile esnek veritabanı istemci kitaplığı aracılı kolay olun.

Bu kod örneği (eşlik eden örnekten) burada parçalama anahtarı doğru parçaya Bağlantı Aracısı için uygulama kitaplığı tarafından sağlanan yaklaşımı da gösterir.   

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                     key: tenantId1,
                     connectionString: connStrBldr.ConnectionString,
                     options: ConnectionOptions.Validate))
    {
        var blog = new Blog { Name = name };
        sqlconn.Execute(@"
                      INSERT INTO
                            Blog (Name)
                            VALUES (@name)", new { name = blog.Name }
                        );
    }

Çağrı [OpenConnectionForKey](https://msdn.microsoft.com/library/azure/dn807226.aspx) API, varsayılan oluşturma ve SQL istemci bağlantısı açılırken değiştirir. [OpenConnectionForKey](https://msdn.microsoft.com/library/azure/dn807226.aspx) çağrı verilere bağımlı yönlendirme için gerekli olan bağımsız değişkenleri alır: 

* Verilere bağımlı yönlendirme erişmek için parça eşlemesi arabirimleri
* Parçalama anahtarı parçacık tanımlamak için
* Kimlik bilgilerini (kullanıcı adı ve parola) parçaya bağlanma

Parça eşleme nesnesi belirtilen parçalama anahtarı için parçacık tutan parça bir bağlantı oluşturur. Elastik veritabanı istemci API'leri Ayrıca kendi tutarlılık garantisi uygulamak için bağlantı etiketi. Çağrısından sonra [OpenConnectionForKey](https://msdn.microsoft.com/library/azure/dn807226.aspx) normal SQL istemci bağlantı nesnesi, sonraki çağrı döndürür **yürütme** Dapper şuradan genişleme metodu standart Dapper uygulama izler.

Sorguları hemen hemen aynı şekilde çalışır - ilk bağlantıyı kullanarak açmak [OpenConnectionForKey](https://msdn.microsoft.com/library/azure/dn807226.aspx) istemci API'si. Ardından .NET nesnelerini SQL Sorgunuzun sonuçlarını eşleştirmek için normal Dapper genişletme yöntemleri kullanın:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId1,
                    connectionString: connStrBldr.ConnectionString,
                    options: ConnectionOptions.Validate ))
    {
           // Display all Blogs for tenant 1
           IEnumerable<Blog> result = sqlconn.Query<Blog>(@"
                                SELECT *
                                FROM Blog
                                ORDER BY Name");

           Console.WriteLine("All blogs for tenant id {0}:", tenantId1);
           foreach (var item in result)
           {
                Console.WriteLine(item.Name);
            }
    }

Unutmayın **kullanarak** DDR bağlantı kapsamlarla tenantId1 nerede tutulur bir parçaya bloğu içinde tüm veritabanı işlemleri engelleyin. Sorgu, geçerli parça üzerinde depolanan blogları, ancak başka bir parça üzerinde depolanan olanları değil yalnızca döndürür. 

## <a name="data-dependent-routing-with-dapper-and-dapperextensions"></a>Dapper ile DapperExtensions verilere bağımlı yönlendirme
Dapper bir veritabanı uygulamaları geliştirirken daha kullanışlı ve veritabanından Özet sağlayan ek uzantı ekosistemi ile birlikte gelir. DapperExtensions örneğidir. 

Uygulamanızda DapperExtensions kullanarak veritabanı bağlantıları nasıl oluşturulduğunu ve yönetilen değiştirmez. Bunu hala bağlantıları uygulamanın sorumluluğundadır ve normal SQL istemci bağlantı nesneleri genişletme yöntemleri tarafından beklenir. Bağımlı olduğumuz [OpenConnectionForKey](https://msdn.microsoft.com/library/azure/dn807226.aspx) yukarıda ana hatlarıyla. Aşağıdaki kod örnekleri gösterdiği gibi tek değişiklik, artık T-SQL deyimleri yazmanıza olmasıdır:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2,
                    connectionString: connStrBldr.ConnectionString,
                    options: ConnectionOptions.Validate))
    {
           var blog = new Blog { Name = name2 };
           sqlconn.Insert(blog);
    }

Ve sorgu için kod örneği aşağıdadır: 

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2,
                    connectionString: connStrBldr.ConnectionString,
                    options: ConnectionOptions.Validate))
    {
           // Display all Blogs for tenant 2
           IEnumerable<Blog> result = sqlconn.GetList<Blog>();
           Console.WriteLine("All blogs for tenant id {0}:", tenantId2);
           foreach (var item in result)
           {
               Console.WriteLine(item.Name);
           }
    }

### <a name="handling-transient-faults"></a>Geçici hataların işlenmesi
Microsoft Patterns ve uygulamalar ekibi yayımlanan [geçici hata işleme uygulama bloğu](https://msdn.microsoft.com/library/hh680934.aspx) uygulama geliştiricilerin bulutta çalışırken karşılaşılan yaygın geçici hata koşulları azaltılmasına yardımcı olmak için. Daha fazla bilgi için [Perseverance, tüm Triumphs gizli anahtarı: Geçici hata işleme uygulama bloğu kullanarak](https://msdn.microsoft.com/library/dn440719.aspx).

Geçici hata kitaplığı, geçici hataların karşı korumak için kod örneği kullanır. 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
    {
       using (SqlConnection sqlconn =
          shardingLayer.ShardMap.OpenConnectionForKey(tenantId2, connStrBldr.ConnectionString, ConnectionOptions.Validate))
          {
              var blog = new Blog { Name = name2 };
              sqlconn.Insert(blog);
          }
    });

**SqlDatabaseUtils.SqlRetryPolicy** Yukarıdaki kod olarak tanımlanan bir **SqlDatabaseTransientErrorDetectionStrategy** 10 ve 5 saniye ile bir yeniden deneme sayısı, yeniden denemeler arasındaki süre bekleyin. İşlem kullanıyorsanız, yeniden deneme kapsamı geri geçici bir hata durumunda işlem başlangıcına gider emin emin olun.

## <a name="limitations"></a>Sınırlamalar
Bu belgede özetlenen yaklaşımları birkaç sınırlama dahildir:

* Bu belge için örnek kod, parçalar arasında şema yönetme göstermemiz gerekmez.
* Bir istek alındığında, tüm veritabanı işlemleri istek tarafından sağlanan parçalama anahtarı tarafından tanımlandığı gibi tek bir parçanın içinde bulunur varsayıyoruz. Bir parçalama anahtarı kullanılabilir hale getirmek mümkün değildir, ancak bu varsayımı her zaman, örneğin, tutmaz. Bunu ele almak için elastik veritabanı istemci kitaplığı içerir. [MultiShardQuery sınıfı](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardexception.aspx). Sınıfı birden çok parça sorgulama için bir bağlantı Özet uygular. MultiShardQuery Dapper ile birlikte bu belgenin kapsamı dışındadır kullanmaktır.

## <a name="conclusion"></a>Sonuç
Dapper ile DapperExtensions uygulamalar, kolayca Azure SQL veritabanı için esnek Veritabanı Araçları'ndan yararlanabilir. Bu belgede özetlenen adımları oluşturma değiştirme ve, yeni açarak verilere bağımlı yönlendirme için bu uygulamaların Aracı'nın özellik kullanmaya [SqlConnection](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) nesneleri [ OpenConnectionForKey](https://msdn.microsoft.com/library/azure/dn807226.aspx) elastik veritabanı istemci Kitaplığı'nın çağrısı. Bu yeni bağlantıları burada oluşturulur ve açılır bu yerlerin gerekli uygulama değişiklikleri sınırlar. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-working-with-dapper/dapperimage1.png
