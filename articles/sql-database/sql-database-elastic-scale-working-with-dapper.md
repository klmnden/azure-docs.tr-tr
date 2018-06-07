---
title: Esnek veritabanı istemci kitaplığı ile Dapper kullanılarak | Microsoft Docs
description: Esnek veritabanı istemci kitaplığı ile Dapper kullanıyor.
services: sql-database
manager: craigg
author: stevestein
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: 6619f2dfe5f58cd23dbd0ffe6e2b545b803f3acc
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34647937"
---
# <a name="using-elastic-database-client-library-with-dapper"></a>Esnek veritabanı istemci kitaplığı Dapper ile kullanma
Uygulamaları derleme Dapper kullanır, ancak aynı zamanda kag'yi geliştiriciler için bu belgedir [esnek veritabanı araçları](sql-database-elastic-scale-introduction.md) kendi veri katmanı genişletmek için bu uygulama parçalama uygulamaları oluşturmak için.  Bu belge Dapper tabanlı uygulamalar, esnek veritabanı araçları ile tümleştirmek için gerekli olan değişiklikleri gösterilmektedir. Esnek veritabanı parça yönetimi ve veri bağımlı Dapper ile yönlendirme oluşturma üzerinde bizim odak noktasıdır. 

**Örnek kod**: [Azure SQL veritabanı - Dapper tümleştirme için esnek veritabanı araçlarını](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-e19fc77f).

Tümleştirme **Dapper** ve **DapperExtensions** esnek veritabanı ile Azure SQL veritabanı için istemci kitaplığı kolaydır. Uygulamalarınızı oluşturma değiştirme ve, yeni açarak veri bağımlı yönlendirme kullanabilirsiniz [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) kullanılacak nesneleri [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) çağırmanıza [istemci kitaplığı ](http://msdn.microsoft.com/library/azure/dn765902.aspx). Yeni bağlantı oluşturulur ve açık olduğunda bu değişiklikler yalnızca, uygulamanızda sınırlar. 

## <a name="dapper-overview"></a>Dapper genel bakış
**Dapper** bir nesne ilişkisel Eşleyici olduğu. .NET nesneleri uygulamanızdan bir ilişkisel veritabanı (ve tersi) eşler. Esnek veritabanı istemci kitaplığı Dapper tabanlı uygulamalarla nasıl tümleştirebilir örnek kod ilk bölümü gösterir. Örnek kod ikinci bölümü, Dapper ve DapperExtensions kullanırken tümleştirmek göstermektedir.  

Dapper Eşleyici işlevindeki yürütme veya veritabanını sorgulamak için gönderen T-SQL deyimlerini basitleştirmek veritabanı bağlantılarını genişletme yöntemleri sağlar. Örneğin, Dapper .NET nesneleri ve SQL deyimleri için parametreleri arasında eşleme kolaylaştırır **yürütme** çağrıları, ya da SQL sorgularınızı sonuçlarını kullanarak .NET nesnelerini kullanmak için **sorgu** Dapper gelen çağrıları. 

DapperExtensions kullanırken, artık SQL deyimlerini sağlamanız gerekir. Genişletme yöntemleri gibi **GetList** veya **Ekle** veritabanı bağlantısı üzerinden SQL deyimlerini arka planda oluşturun.

Dapper ve ayrıca DapperExtensions başka bir avantajı, uygulama veritabanı bağlantısı oluşturulmasını denetler ' dir. Bu aracıların veritabanlarına shardlets eşleme bağlantıları veritabanı esnek veritabanı istemci kitaplığı ile etkileşim yardımcı olur.

Dapper derlemeler almak için bkz: [Dapper dot net](http://www.nuget.org/packages/Dapper/). Dapper uzantıları için bkz: [DapperExtensions](http://www.nuget.org/packages/DapperExtensions).

## <a name="a-quick-look-at-the-elastic-database-client-library"></a>Esnek veritabanı istemci kitaplığı hızlı bir bakış
Adlı uygulama verilerinizi bölümlerini tanımladığınız esnek veritabanı istemci kitaplığı ile *shardlets*veritabanlarına harita ve onlar tarafından tanımlamak *parçalama anahtarları*. Gerekir ve bu veritabanları arasında shardlets dağıtmak sayıda veritabanı olabilir. Parçalama anahtar değerlerin veritabanlarına kitaplığın API'leri tarafından sağlanan bir parça eşleme tarafından depolanır. Bu özellik adında **parça eşleme Yönetim**. Parça eşleme, ayrıca bir parçalama anahtar taşımak istekleri için veritabanı bağlantılarını aracısı olarak görev yapar. Bu özellik olarak adlandırılır **veri bağımlı yönlendirme**.

![Parça eşlemeleri ve veri bağımlı yönlendirme][1]

Parça eşleme Yöneticisi eşzamanlı shardlet yönetim işlemlerini veritabanlarına gerçekleştiği yüklendiğinde oluşabilecek shardlet veri tutarsız görünümleri kullanıcıları korur. Bunu yapmak için veritabanı bağlantılarını kitaplığı ile oluşturulmuş bir uygulama için parça eşlemeleri Aracısı. Parça yönetim işlemlerini shardlet etkileyebilir, bu otomatik olarak bir veritabanı bağlantısı sonlandırılamadı parça eşleme işlevselliği sağlar. 

Dapper bağlantılarında oluşturmak için geleneksel yol kullanmak yerine, kullanmanız gereken [OpenConnectionForKey yöntemi](http://msdn.microsoft.com/library/azure/dn824099.aspx). Bu, tüm doğrulama gerçekleşir ve herhangi bir veri parça arasında taşındığında bağlantıları düzgün yönetilen sağlar.

### <a name="requirements-for-dapper-integration"></a>Dapper tümleştirme gereksinimleri
Esnek veritabanı istemci kitaplığı ve Dapper API'leri ile çalışırken, aşağıdaki özellikleri korumak istediğiniz:

* **Ölçeği genişletme**: veritabanları kapasite gereksinimlerini karşılamak için gerekli olarak parçalı uygulama uygulamasının veri katmanı ekleyip istiyoruz. 
* **Tutarlılık**: uygulama parçalama kullanılarak ölçeklenir olduğundan, veri bağımlı yönlendirme gerçekleştirmeniz gerekir. Bunu yapmak için kitaplık veri bağımlı yönlendirme özelliklerini kullanmak istiyoruz. Özellikle, doğrulama korumak istediğiniz ve parça eşleme Yöneticisi aracılığıyla bozulma veya yanlış sorgu sonuçları önlemek için aracılı bağlantılar tarafından sağlanan tutarlılığı garanti altına alır. Bu, belirli bir shardlet bağlantı reddedildi veya (örneğin) shardlet bölünmüş/Merge API'lerini kullanarak farklı bir parça şu anda taşınırsa durduruldu sağlar.
* **Nesne eşleme**: uygulama sınıfları ve temel veritabanı yapılarını arasında çeviri yapmak Dapper tarafından sağlanan eşlemeleri kolaylık korumak istiyoruz. 

Aşağıdaki bölümde temel uygulamalar için bu gereksinimleri için yönergeler sağlanmaktadır **Dapper** ve **DapperExtensions**.

## <a name="technical-guidance"></a>Teknik Kılavuzu
### <a name="data-dependent-routing-with-dapper"></a>Veri bağımlı Dapper ile yönlendirme
Dapper ile uygulama oluşturma ve temel veritabanı bağlantılarını açma genellikle sorumludur. T-SQL sonuç satırları t türü nesnelere eşleme türü T. Dapper .NET koleksiyonları gerçekleştirir gibi T türü uygulama tarafından verilen, Dapper sorgu sonuçlarını döndürür Benzer şekilde, Dapper SQL değerleri veya veri işleme dili (DML) deyimleri parametrelerini .NET nesneleri eşler. Dapper normal üzerinde genişletme yöntemleri aracılığıyla bu işlevselliği sunar [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) ADO .NET SQL istemci kitaplıklarından nesnesi. DDR için esnek ölçeklendirme API'leri tarafından döndürülen SQL bağlantısı normal de [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) nesneleri. Bu bize Ayrıca basit SQL istemci bağlantısı olarak doğrudan Dapper uzantılar istemci kitaplığının DDR API tarafından döndürülen tür üzerinden kullanmayı sağlar.

Bu gözlemleri Dapper için esnek veritabanı istemci kitaplığı tarafından aracılı bağlantılar kullanmak basit olun.

Bu kod örneği (eşlik eden örnekten) burada parçalama anahtar sağ parça Bağlantı Aracısı uygulamaya kitaplığına sağladığı yaklaşımı açıklar.   

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

Çağrı [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) API varsayılan oluşturulması ve SQL istemci bağlantısı açılırken yerini alır. [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) çağrısı veri bağımlı yönlendirme için gerekli olan bağımsız değişkenler alır: 

* Veri bağımlı yönlendirme arabirimlerini erişmek için parça eşleme
* Shardlet tanımlamak için parçalama anahtarı
* (Kullanıcı adı ve parola) parça bağlanmak için kimlik bilgileri

Parça harita nesnesi belirtilen parçalama anahtar shardlet tutan parça bir bağlantı oluşturur. Esnek veritabanı istemci API Ayrıca kendi tutarlılığı garanti uygulamak için bağlantı etiketi. Çağrısından sonra [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) normal SQL istemci bağlantısı nesnesi, sonraki çağrı döndürür **yürütme** Dapper uzantısı yönteminden standart Dapper uygulama izler.

İş çok benzer şekilde sorguları - ilk bağlantıyı kullanarak açmak [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) API istemciden. Ardından SQL Sorgunuzun sonuçlarını .NET nesneleri eşleştirmek için normal Dapper genişletme yöntemleri kullanın:

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

Unutmayın **kullanarak** tenantId1 nerede tutulur bir parça bloğuna içindeki tüm veritabanı işlemleri DDR bağlantı kapsamlarla engelleyin. Sorgu, geçerli parça üzerinde depolanan bloglar, ancak başka bir parça üzerinde depolanan olanları değil yalnızca döndürür. 

## <a name="data-dependent-routing-with-dapper-and-dapperextensions"></a>Veri bağımlı Dapper ve DapperExtensions ile yönlendirme
Dapper bir daha kullanışlı ve soyutlama veritabanından veritabanı uygulamaları geliştirirken sağlayabilir ek uzantıları ekosistemi ile birlikte gelir. DapperExtensions bir örnektir. 

Uygulamanızda DapperExtensions kullanarak veritabanı bağlantılarını nasıl oluşturulduğunu ve yönetilen değiştirmez. Bunu hala Bağlantıları'nı açmak için uygulamanın sorumluluğundadır ve normal SQL istemci bağlantısı nesneleri genişletme yöntemleri tarafından beklenir. Üzerinde güvenebilirsiniz [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) yukarıda açıklandığı şekilde. Aşağıdaki kod örnekleri gösterdiği gibi tek değişiklik, artık T-SQL deyimleri yazmanıza sahip olabilir:

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

### <a name="handling-transient-faults"></a>Geçici hata işleme
Microsoft Patterns & yöntemler takım yayımlanan [geçici hata işleme uygulama bloğu](http://msdn.microsoft.com/library/hh680934.aspx) uygulama geliştiricileri bulutta çalışırken karşılaşılan yaygın geçici hata koşulları azaltmaya yardımcı olmak için. Daha fazla bilgi için bkz: [Perseverance, tüm Triumphs gizliliği: geçici hata işleme uygulama bloğu kullanarak](http://msdn.microsoft.com/library/dn440719.aspx).

Kod örneği geçici hataları karşı korumak için geçici hata kitaplığı kullanır. 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
    {
       using (SqlConnection sqlconn = 
          shardingLayer.ShardMap.OpenConnectionForKey(tenantId2, connStrBldr.ConnectionString, ConnectionOptions.Validate))
          {
              var blog = new Blog { Name = name2 };
              sqlconn.Insert(blog);
          }
    });

**SqlDatabaseUtils.SqlRetryPolicy** Yukarıdaki kod olarak tanımlanan bir **SqlDatabaseTransientErrorDetectionStrategy** bir yeniden deneme sayısı 10 ve 5 saniye ile yeniden denemeler arasındaki süre bekleyin. İşlemler kullanıyorsanız, yeniden deneme kapsamınızı geri geçici bir hata durumunda işlem başına kalacağı emin olun.

## <a name="limitations"></a>Sınırlamalar
Bu belgede özetlenen yaklaşımlar birkaç sınırlama oluşturulmasını gerektirir:

* Bu belge için örnek kod, şema parça yönetme gösterilmemiştir.
* Bir istek göz önüne alındığında, tüm veritabanı işleme istek tarafından sağlanan parçalama anahtarı tarafından tanımlandığı gibi tek bir parça içinde bulunur varsayıyoruz. Bir parçalama anahtarı kullanılabilir hale mümkün değilse, ancak bu varsayım her zaman, örneğin, tutmaz. Bu sorunu çözmek için esnek veritabanı istemci kitaplığı içerir [MultiShardQuery sınıfı](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardexception.aspx). Sınıf birkaç parça sorgulama için bir bağlantı Özet uygular. MultiShardQuery Dapper ile birlikte bu belgenin kapsamı dışındadır kullanmaktır.

## <a name="conclusion"></a>Sonuç
Dapper ve DapperExtensions kullanarak uygulamaları kolayca Azure SQL veritabanı için esnek Veritabanı Araçları'ndan yararlanabilirsiniz. Bu belgede özetlenen adımları bu uygulamaları aracın yetenek oluşturma değiştirme ve, yeni açarak veri bağımlı yönlendirmek için kullanabilir [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) kullanılacak nesneleri [ OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) esnek veritabanı istemci kitaplığı çağrısı. Bu yeni bağlantı burada oluşturulan ve açılan bu yerlerin gerekli uygulama değişiklikleri sınırlar. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-working-with-dapper/dapperimage1.png
