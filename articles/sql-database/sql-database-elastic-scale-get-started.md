---
title: "Esnek veritabanı araçları - Azure ile çalışmaya başlama | Microsoft Docs"
description: "Bir çalışma kolay örnek uygulaması da dahil olmak üzere Azure SQL veritabanı'nın esnek veritabanı araçlarını özelliği temel açıklaması."
services: sql-database
manager: craigg
author: anumjs
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 11/16/2017
ms.author: anjangsh
ms.openlocfilehash: 28ff3f6eee2316a078badcf29e6780f3844f3a54
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="get-started-with-elastic-database-tools"></a>Esnek veritabanı araçlarını kullanmaya başlama
Bu belge size geliştirici deneyimi tanıtır [esnek veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md) yardımcı olarak bir örnek uygulamayı çalıştırın. Örnek uygulama basit parçalı bir uygulama oluşturur ve Azure SQL veritabanı'nın esnek veritabanı araçları özelliği anahtar özelliklerini inceler. Kullanım örnekleri için odaklanır [parça eşleme Yönetim](sql-database-elastic-scale-shard-map-management.md), [veri bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md), ve [çok parça sorgulama](sql-database-elastic-scale-multishard-querying.md). Java yanı sıra .NET istemci kitaplığını kullanılabilir. 

## <a name="elastic-database-tools-for-java"></a>Java için esnek veritabanı araçları
### <a name="prerequisites"></a>Önkoşullar
* Bir Java Geliştirme Seti (JDK), sürüm 1,8 veya üstü
* [Maven](http://maven.apache.org/download.cgi)
* Azure veya yerel bir SQL Server örneğini bir mantıksal sunucu

### <a name="download-and-run-the-sample-app"></a>Örnek uygulamasını indirme ve çalıştırma
JAR dosyalarını oluşturmak ve örnek projeyle başlamak için aşağıdakileri yapın: 
1. Kopya [GitHub deposunu](https://github.com/Microsoft/elastic-db-tools-for-java) örnek uygulama ile birlikte istemci kitaplığı içeren. 

2. Düzen _./sample/src/main/resources/resource.properties_ dosya aşağıdakileri ayarlayın:
    * TEST_CONN_USER
    * TEST_CONN_PASSWORD
    * TEST_CONN_SERVER_NAME

3. Buna örnek proje oluşturmak için _. / örnek_ dizin, aşağıdaki komutu çalıştırın:

    ```
    mvn install
    ```
    
4. Örnek proje başlatmak için _. / örnek_ dizin, aşağıdaki komutu çalıştırın: 
    
    ```
    mvn -q exec:java "-Dexec.mainClass=com.microsoft.azure.elasticdb.samples.elasticscalestarterkit.Program"
    ```
    
5. İstemci kitaplığı özellikleri hakkında daha fazla bilgi edinmek için çeşitli seçeneklerle denemeler yapın. Örnek uygulama uygulama hakkında bilgi edinmek için kodu keşfetmek çekinmeyin.

    ![Progress-java][5]
    
Tebrikler! Başarıyla oluşturulmuş ve Azure SQL Database esnek veritabanı araçlarını kullanarak ilk parçalı uygulamanızı çalıştırın. SQL veritabanınıza bağlanmak ve örnek oluşturulan parça hızlı bakmak için Visual Studio veya SQL Server Management Studio kullanın. Yeni örnek parça veritabanları ve örnek oluşturduğu bir parça eşleme manager veritabanı fark edeceksiniz. 

İstemci Kitaplığı kendi Maven projesine eklemek için aşağıdaki bağımlılık POM dosyanıza ekleyin:

```xml
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>elastic-db-tools</artifactId> 
    <version>1.0.0</version> 
</dependency> 
```

## <a name="elastic-database-tools-for-net"></a>.NET için esnek veritabanı araçları 
### <a name="prerequisites"></a>Önkoşullar
* Visual Studio 2012 veya sonraki C# ile. Konumundaki ücretsiz sürümünü karşıdan [Visual Studio indirmeleri](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
* NuGet 2.7 veya üzeri. En son sürümü edinmek için bkz: [yükleme NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).

### <a name="download-and-run-the-sample-app"></a>Örnek uygulamasını indirme ve çalıştırma
Kitaplığı yüklemek için Git [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Kitaplığı aşağıdaki bölümde açıklanan örnek uygulama ile birlikte yüklenir.

İndirip örneği çalıştırmak için aşağıdaki adımları izleyin: 

1. Karşıdan [Azure SQL - Getting Started örnek için esnek DB Araçları](https://code.msdn.microsoft.com/windowsapps/Elastic-Scale-with-Azure-a80d8dc6) MSDN'den. Örnek seçtiğiniz bir konuma ayıklayın.

2. Bir proje oluşturmak için açık *ElasticScaleStarterKit.sln* çözümden *C#* dizin.

3. Örnek Proje çözüm açın *app.config* dosya. Ardından Azure SQL veritabanı sunucunuzun adını ve oturum açma bilgilerinizi (kullanıcı adı ve parola) eklemek için dosyayı'ndaki yönergeleri izleyin.

4. Derleme ve uygulamayı çalıştırın. İstendiğinde, çözümün NuGet paketlerini geri yüklemek Visual Studio etkinleştirin. Bu eylem Nuget'ten esnek veritabanı istemci kitaplığının en son sürümünü yükler.

5. İstemci kitaplığı özellikleri hakkında daha fazla bilgi edinmek için çeşitli seçeneklerle denemeler yapın. Uygulama konsolda alır gereken adımları çıkış ve arka planda kod keşfetmek çekinmeyin unutmayın.
   
    ![İlerleme Durumu][4]

Tebrikler! Başarıyla oluşturulmuş ve SQL Database esnek veritabanı araçlarını kullanarak ilk parçalı uygulamanızı çalıştırın. SQL veritabanınıza bağlanmak ve örnek oluşturulan parça hızlı bakmak için Visual Studio veya SQL Server Management Studio kullanın. Yeni örnek parça veritabanları ve örnek oluşturduğu bir parça eşleme manager veritabanı fark edeceksiniz.

> [!IMPORTANT]
> Böylece Azure ve SQL veritabanına güncelleştirmeleriyle eşitlenmesine, her zaman Management Studio en son sürümünü kullanmanızı öneririz. [SQL Server Management Studio’yu güncelleyin](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

## <a name="key-pieces-of-the-code-sample"></a>Kod örneği, temel parçalar
* **Eşlemelerini parça ve parça yönetme**: kod parça, aralıkları ve eşlemesinde ile çalışmaya nasıl gösterir *ShardManagementUtils.cs* dosya. Daha fazla bilgi için bkz: [parça eşleme Yöneticisi veritabanlarıyla genişletme](http://go.microsoft.com/?linkid=9862595).  

* **Veri bağımlı yönlendirme**: sağ parça hareketlerinin yönlendirme görüntülenir *DataDependentRoutingSample.cs* dosya. Daha fazla bilgi için bkz: [veri bağımlı yönlendirme](http://go.microsoft.com/?linkid=9862596). 

* **Birden çok parça sorgulama**: parça sorgulama gösterilen içinde *MultiShardQuerySample.cs* dosya. Daha fazla bilgi için bkz: [çok parça sorgulama](http://go.microsoft.com/?linkid=9862597).

* **Boş parça ekleme**: Yeni boş parça yinelemeli ekleme kodu tarafından gerçekleştirilen *CreateShardSample.cs* dosya. Daha fazla bilgi için bkz: [parça eşleme Yöneticisi veritabanlarıyla genişletme](http://go.microsoft.com/?linkid=9862595).

## <a name="other-elastic-scale-operations"></a>Diğer esnek ölçeklendirme işlemleri
* **Varolan bir parça bölme**: parça bölme yeteneği bölünmüş Birleştirme aracı tarafından sağlanır. Daha fazla bilgi için bkz: [ölçeklendirilmiş bulut veritabanları arasında verilerin taşınması](sql-database-elastic-scale-overview-split-and-merge.md).

* **Varolan parça birleştirme**: parça birleştirmeler, bölünmüş birleştirme aracını kullanarak da gerçekleştirilir. Daha fazla bilgi için bkz: [ölçeklendirilmiş bulut veritabanları arasında verilerin taşınması](sql-database-elastic-scale-overview-split-and-merge.md).   

## <a name="cost"></a>Maliyet
Esnek veritabanı araçlarını kitaplığı ücretsizdir. Esnek veritabanı araçlarını kullanın, hiçbir ek ücretler Azure kullanımınızı maliyetini ötesinde uygulanır. 

Örneğin, yeni veritabanları örnek uygulaması oluşturur. Bu özellik, maliyet, seçtiğiniz SQL veritabanı sürümü ve uygulamanızın Azure kullanım bağlıdır.

Fiyatlandırma bilgileri için bkz: [SQL veritabanı fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Sonraki adımlar
Esnek veritabanı araçları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* Kod örnekleri: 
  * Esnek veritabanı araçlarını ([.NET](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE), [Java](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-elasticdb-tools%22))
  * [Azure SQL - Entity Framework tümleştirme için esnek veritabanı araçları](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
  * [Betik Merkezi'nde parça esneklik](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
* Blog: [esnek genişleme Duyurusu](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
* Microsoft sanal Akademi: [genişleme kullanarak parçalama esnek veritabanı istemci kitaplığı Video ile uygulama](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554?l=lWyQhF1fC_6306218965) 
* Kanal 9: [esnek genişleme genel bakış videosu](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
* Tartışma forumu: [Azure SQL veritabanı Forumu](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
* Performansını ölçmek için: [parça eşleme Yöneticisi için performans sayaçları](sql-database-elastic-database-client-library.md)

<!--Anchors-->
[The Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run the Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png
[5]: ./media/sql-database-elastic-scale-get-started/java-client-library.PNG

