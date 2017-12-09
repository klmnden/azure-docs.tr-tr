---
title: "Esnek veritabanı araçlarını kullanmaya başlama | Microsoft Docs"
description: "Esnek veritabanı araçları özelliği Çalıştır kolay örnek uygulaması da dahil olmak üzere Azure SQL veritabanı'nın temel açıklaması."
services: sql-database
documentationcenter: 
manager: jstrauss
author: anumjs
editor: CarlRabeler
ms.assetid: b6911f8d-2bae-4d04-9fa8-f79a3db7129d
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: anjangsh
ms.openlocfilehash: e7e072e310cabc2c4520df7e9f4f9e45b8218998
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2017
---
# <a name="get-started-with-elastic-database-tools"></a>Esnek veritabanı araçlarını kullanmaya başlama
Bu belge size geliştirici deneyimi tanıtır [esnek veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md) yardımcı olarak bir örnek uygulamayı çalıştırın. Örnek uygulama basit parçalı bir uygulama oluşturur ve esnek veritabanı araçlarını anahtar özelliklerini inceler. Kullanım örnekleri için odaklanır [parça eşleme Yönetim](sql-database-elastic-scale-shard-map-management.md), [veri bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md), ve [çok parça sorgulama](sql-database-elastic-scale-multishard-querying.md). Java yanı sıra .net istemci kitaplığını kullanılabilir. 

## <a name="elastic-database-tools-for-java"></a>Java için esnek veritabanı araçları
### <a name="prerequisites"></a>Ön koşullar
* Bir Java Geliştirme Seti (JDK), v 1.8 veya üzeri
* [Maven](http://maven.apache.org/download.cgi)
* Azure veya yerel SQL Server'ın bir mantıksal sunucu

### <a name="download-and-run-the-sample-app"></a>Örnek uygulamasını indirme ve çalıştırma
JAR dosyalarını oluşturmak ve örnek projeyle başlamak için aşağıdaki adımları izleyin: 
1. Kopya [GitHub deposunu](https://github.com/Microsoft/elastic-db-tools-for-java) örnek uygulama ile birlikte istemci kitaplığı içeren. 
2. Düzen _./sample/src/main/resources/resource.properties_ dosya aşağıdakileri ayarlayın.
    * TEST_CONN_USER
    * TEST_CONN_PASSWORD
    * TEST_CONN_SERVER_NAME
3. Gelen _. / örnek_ dizin, örnek proje oluşturmak için aşağıdaki komutu çalıştırın.<br>

    ```
    mvn install
    ```
    
4. Gelen _. / örnek_ dizin, örnek proje başlatmak için aşağıdaki komutu çalıştırın. 
    
    ```
    mvn -q exec:java "-Dexec.mainClass=com.microsoft.azure.elasticdb.samples.elasticscalestarterkit.Program"
    ```
    
5. İstemci kitaplığı özellikleri hakkında daha fazla bilgi edinmek için farklı seçeneklerle denemeler yapın. Örnek uygulama uygulama hakkında bilgi edinmek için kodu keşfetmek çekinmeyin.

    ![İlerleme-java][5]
    
Tebrikler! Başarıyla oluşturulmuş ve SQL Database esnek veritabanı araçlarını kullanarak ilk parçalı uygulamanızı çalıştırın. SQL veritabanınıza bağlanmak ve örnek oluşturulan parça hızlı bakmak için Visual Studio veya SQL Server Management Studio kullanın. Yeni örnek parça veritabanları ve örnek oluşturduğu bir parça eşleme manager veritabanı fark edeceksiniz. İstemci Kitaplığı kendi Maven projesine eklemek için aşağıdaki bağımlılık POM dosyanıza ekleyin.<br>

```xml
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>elastic-db-tools</artifactId> 
    <version>1.0.0</version> 
</dependency> 
```

## <a name="elastic-database-tools-for-net"></a>.Net için esnek veritabanı araçları 
### <a name="prerequisites"></a>Ön koşullar
* Visual Studio 2012 veya sonraki C# ile. Konumundaki ücretsiz sürümünü karşıdan [Visual Studio indirmeleri](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
* NuGet 2.7 veya üzeri. En son sürümü edinmek için bkz: [yükleme NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).

### <a name="download-and-run-the-sample-app"></a>Örnek uygulamasını indirme ve çalıştırma
Kitaplığı yüklemek için Git [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Kitaplığı aşağıdaki bölümde açıklanan örnek uygulama ile birlikte yüklenir.

İndirip örneği çalıştırmak için aşağıdaki adımları izleyin: 

1. Karşıdan [Azure SQL - Getting Started örnek için esnek DB Araçları](https://code.msdn.microsoft.com/windowsapps/Elastic-Scale-with-Azure-a80d8dc6) MSDN'den. Örnek seçtiğiniz bir konuma ayıklayın.

2. Bir proje oluşturmak için açık **ElasticScaleStarterKit.sln** çözümden **C#** dizin.

3. Örnek Proje çözüm açın **app.config** dosya. Ardından Azure SQL veritabanı sunucunuzun adını ve oturum açma bilgilerinizi (kullanıcı adı ve parola) eklemek için dosyayı'ndaki yönergeleri izleyin.

4. Derleme ve uygulamayı çalıştırın. İstendiğinde, çözümün NuGet paketlerini geri yüklemek Visual Studio etkinleştirin. Bu esnek veritabanı istemci kitaplığının en son sürümünü Nuget'ten indirir.

5. İstemci kitaplığı özellikleri hakkında daha fazla bilgi edinmek için farklı seçeneklerle denemeler yapın. Uygulama konsolda alır adımları çıkış ve arka planda kod keşfetmek çekinmeyin unutmayın.
   
    ![İlerleme][4]

Tebrikler--başarıyla oluşturulmuş ve SQL Database esnek veritabanı araçlarını kullanarak ilk parçalı uygulamanızı çalıştırın. SQL veritabanınıza bağlanmak ve örnek oluşturulan parça hızlı bakmak için Visual Studio veya SQL Server Management Studio kullanın. Yeni örnek parça veritabanları ve örnek oluşturduğu bir parça eşleme manager veritabanı fark edeceksiniz.

> [!IMPORTANT]
> Böylece Azure ve SQL veritabanına güncelleştirmeleriyle eşitlenmesine, her zaman Management Studio en son sürümünü kullanmanızı öneririz. [SQL Server Management Studio’yu güncelleyin](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

## <a name="key-pieces-of-the-code-sample"></a>Kod örneği, temel parçalar
* **Eşlemelerini parça ve parça yönetme**: kod parça, aralıkları ve eşlemeleri dosyasındaki çalışmak nasıl gösterir **ShardManagementUtils.cs**. Daha fazla bilgi için bkz: [parça eşleme Yöneticisi veritabanlarıyla genişletme](http://go.microsoft.com/?linkid=9862595).  

* **Veri bağımlı yönlendirme**: sağ parça hareketlerinin yönlendirme görüntülenir **DataDependentRoutingSample.cs**. Daha fazla bilgi için bkz: [veri bağımlı yönlendirme](http://go.microsoft.com/?linkid=9862596). 

* **Birden çok parça sorgulama**: parça sorgulama dosyasında gösterilen **MultiShardQuerySample.cs**. Daha fazla bilgi için bkz: [çok parça sorgulama](http://go.microsoft.com/?linkid=9862597).

* **Boş parça ekleme**: yinelemeli ekleme yeni boş parça dosyasındaki kodu tarafından gerçekleştirilen **CreateShardSample.cs**. Daha fazla bilgi için bkz: [parça eşleme Yöneticisi veritabanlarıyla genişletme](http://go.microsoft.com/?linkid=9862595).

## <a name="other-elastic-scale-operations"></a>Diğer esnek ölçeklendirme işlemleri
* **Varolan bir parça bölme**: parça bölme özelliği tarafından sağlanan **bölünmüş Birleştirme aracı**. Daha fazla bilgi için bkz: [ölçeklendirilmiş bulut veritabanları arasında verilerin taşınması](sql-database-elastic-scale-overview-split-and-merge.md).

* **Varolan parça birleştirme**: parça birleştirmeler, kullanarak da gerçekleştirilir **bölünmüş Birleştirme aracı**. Daha fazla bilgi için bkz: [ölçeklendirilmiş bulut veritabanları arasında verilerin taşınması](sql-database-elastic-scale-overview-split-and-merge.md).   

## <a name="cost"></a>Maliyet
Esnek veritabanı araçlarını ücretsizdir. Esnek veritabanı araçlarını kullandığınızda, hiçbir ek bir ücret Azure kullanımınızı maliyetini en üstünde almadığınız. 

Örneğin, yeni veritabanları örnek uygulaması oluşturur. Bu maliyet, seçtiğiniz SQL veritabanı sürümü ve uygulamanızın Azure kullanım bağlıdır.

Fiyatlandırma bilgileri için bkz: [SQL veritabanı fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Sonraki adımlar
Esnek veritabanı araçları hakkında daha fazla bilgi için aşağıdaki sayfalarına bakın:

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

