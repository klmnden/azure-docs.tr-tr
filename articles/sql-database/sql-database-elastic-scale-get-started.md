---
title: Elastik veritabanı araçları - Azure ile çalışmaya başlama | Microsoft Docs
description: Bir kolayca çalışma örnek uygulamasını dahil olmak üzere, Azure SQL veritabanı'nın elastik veritabanı araçları özelliğini temel açıklaması.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anumjs
ms.author: anjangsh
ms.reviewer: sstein
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 7c2e3b2bea7ddef3dfbf69f85b499d9a60158689
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60585023"
---
# <a name="get-started-with-elastic-database-tools"></a>Esnek veritabanı araçlarını kullanmaya başlayın

Bu belge, geliştirici deneyimini tanıtır [elastik veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md) yardımcı olarak örnek uygulamasını çalıştırın. Örnek uygulama, basit bir parçalı uygulama oluşturur ve Azure SQL veritabanı'nın elastik veritabanı araçları özelliği'nin önemli özelliklerini açıklar. Kullanım durumları için odaklanır [parça eşleme Yönetimi](sql-database-elastic-scale-shard-map-management.md), [verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md), ve [çok parçalı sorgulama](sql-database-elastic-scale-multishard-querying.md). İstemci Kitaplığı, Java yanı sıra .NET için kullanılabilir. 

## <a name="elastic-database-tools-for-java"></a>Java için esnek veritabanı araçları

### <a name="prerequisites"></a>Önkoşullar

* Bir Java Developer Kit (JDK), sürüm 1.8 veya sonraki bir sürümü
* [Maven](https://maven.apache.org/download.cgi)
* Azure'da veya yerel bir SQL Server örneğini bir SQL veritabanı sunucusu

### <a name="download-and-run-the-sample-app"></a>Örnek uygulamasını indirme ve çalıştırma

Örnek projeyle birlikte kullanmaya başlayın ve JAR dosyaları oluşturmak için aşağıdakileri yapın: 
1. Kopya [GitHub deposu](https://github.com/Microsoft/elastic-db-tools-for-java) istemci kitaplığı örnek uygulaması ile birlikte içeren. 

2. Düzen _./sample/src/main/resources/resource.properties_ dosyasını aşağıdakileri ayarlayın:
    * TEST_CONN_USER
    * TEST_CONN_PASSWORD
    * TEST_CONN_SERVER_NAME

3. Örnek Proje içinde oluşturulacak _. / sample_ dizin, aşağıdaki komutu çalıştırın:

    ```
    mvn install
    ```
    
4. Örnek proje başlatmak için _. / sample_ dizin, aşağıdaki komutu çalıştırın: 
    
    ```
    mvn -q exec:java "-Dexec.mainClass=com.microsoft.azure.elasticdb.samples.elasticscalestarterkit.Program"
    ```
    
5. İstemci kitaplığı özellikleri hakkında daha fazla bilgi edinmek için çeşitli seçeneklerle denemeler yapın. Örnek uygulamayı uygulama hakkında bilgi edinmek için kodu keşfedin çekinmeyin.

    ![Progress-java][5]
    
Tebrikler! Başarıyla oluşturulan ve Azure SQL veritabanı elastik veritabanı araçları kullanarak ilk parçalı uygulamanızı çalıştırın. SQL veritabanı'na bağlanma ve örnek oluşturulan parçalar hızlı göz atın, Visual Studio veya SQL Server Management Studio'yu kullanın. Yeni örnek parça veritabanları ve örnek oluşturduğu bir parça eşleme Yöneticisi veritabanını göreceksiniz. 

İstemci Kitaplığı kendi Maven projesine eklemek için POM dosyanızın aşağıdaki bağımlılığı ekleyin:

```xml
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>elastic-db-tools</artifactId> 
    <version>1.0.0</version> 
</dependency> 
```

## <a name="elastic-database-tools-for-net"></a>.NET için esnek veritabanı araçları

### <a name="prerequisites"></a>Önkoşullar

* Visual Studio 2012 veya üzeri ile C#. Ücretsiz bir sürümde indirme [Visual Studio indirmeleri](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
* NuGet 2.7 veya sonraki bir sürümü. En son sürümü edinmek için bkz. [NuGet yükleme](https://docs.nuget.org/docs/start-here/installing-nuget).

### <a name="download-and-run-the-sample-app"></a>Örnek uygulamasını indirme ve çalıştırma

Kitaplığı yüklemek için Git [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Kitaplığı, aşağıdaki bölümde açıklanan örnek uygulaması ile birlikte yüklenir.

İndirme ve çalıştırma örneği için bu adımları izleyin: 

1. İndirme [Azure SQL - kullanmaya başlama örneği için elastik veritabanı araçları](https://code.msdn.microsoft.com/windowsapps/Elastic-Scale-with-Azure-a80d8dc6) MSDN'den. Örnek seçtiğiniz bir konuma ayıklayın.

2. Bir proje oluşturmak için açık *ElasticScaleStarterKit.sln* çözümünden *C#* dizin.

3. Örnek Proje çözüm açın *app.config* dosya. Ardından Azure SQL veritabanı sunucunuzun adını ve oturum açma bilgilerinizi (kullanıcı adı ve parola) eklemek için dosyasındaki yönergeleri izleyin.

4. Derleme ve uygulamayı çalıştırın. İstendiğinde, çözümün NuGet paketlerini geri yüklemek Visual Studio etkinleştirin. Bu eylem Nuget'ten elastik veritabanı istemci Kitaplığı'nın en son sürümünü yükler.

5. İstemci kitaplığı özellikleri hakkında daha fazla bilgi edinmek için çeşitli seçeneklerle denemeler yapın. Konsolda geçen adımları çıktı ve arka planda kod keşfedin çekinmeyin unutmayın.
   
    ![İlerleme durumu][4]

Tebrikler! Başarıyla oluşturulan ve SQL veritabanı elastik veritabanı araçları kullanarak ilk parçalı uygulamanızı çalıştırın. SQL veritabanı'na bağlanma ve örnek oluşturulan parçalar hızlı göz atın, Visual Studio veya SQL Server Management Studio'yu kullanın. Yeni örnek parça veritabanları ve örnek oluşturduğu bir parça eşleme Yöneticisi veritabanını göreceksiniz.

> [!IMPORTANT]
> Böylece, Azure ve SQL veritabanı güncelleştirmeleriyle kalmasını her zaman Management Studio'nun en son sürümünü kullanmanızı öneririz. [SQL Server Management Studio’yu güncelleyin](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="key-pieces-of-the-code-sample"></a>Kod örneği temel

* **Parça ve parça yönetme eşler**: Kod parçaları, aralıkları ve eşlemeler ile nasıl çalışılacağını göstermektedir *ShardManagementUtils.cs* dosya. Daha fazla bilgi için [veritabanları parça eşleme Yöneticisi ile ölçeklendirme](https://go.microsoft.com/?linkid=9862595).  

* **Verilere bağımlı yönlendirme**: İşlemlerin uygun parçaya yönlendirmesi gösterilir *DataDependentRoutingSample.cs* dosya. Daha fazla bilgi için [verilere bağımlı yönlendirme](https://go.microsoft.com/?linkid=9862596). 

* **Birden çok parça üzerinde sorgulama**: Parçalar arasında sorgulama içinde gösterilmiştir *MultiShardQuerySample.cs* dosya. Daha fazla bilgi için [çok parçalı sorgulama](https://go.microsoft.com/?linkid=9862597).

* **Boş bir parça ekleme**: Yeni boş parçalar yinelemeli ekleme kodu tarafından gerçekleştirilen *CreateShardSample.cs* dosya. Daha fazla bilgi için [veritabanları parça eşleme Yöneticisi ile ölçeklendirme](https://go.microsoft.com/?linkid=9862595).

## <a name="other-elastic-scale-operations"></a>Diğer elastik ölçeklendirme işlemleri

* **Mevcut bir parça bölme**: Ayırma-Birleştirme aracı tarafından parçalara bölmek olanağı sağlanır. Daha fazla bilgi için [ölçeği genişletilen bulut veritabanları arasında veri taşıma](sql-database-elastic-scale-overview-split-and-merge.md).

* **Varolan parçaların birleştirme**: Parça birleştirmeleri bölme-birleştirme aracını kullanarak da gerçekleştirilir. Daha fazla bilgi için [ölçeği genişletilen bulut veritabanları arasında veri taşıma](sql-database-elastic-scale-overview-split-and-merge.md).   

## <a name="cost"></a>Maliyet

Elastik veritabanı araçları kitaplığı ücretsizdir. Esnek veritabanı araçlarını kullandığınızda, Azure kullanımınızı maliyetini ötesinde herhangi bir ek ücret uygulanır. 

Örneğin, örnek uygulama, yeni veritabanları oluşturur. Bu özellik maliyeti, seçtiğiniz SQL Database sürümünü ve uygulamanızın Azure kullanımını bağlıdır.

Fiyatlandırma bilgileri için bkz: [SQL veritabanı fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Sonraki adımlar

Esnek veritabanı araçları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* Kod örnekleri: 
  * Esnek veritabanı araçlarını ([.NET](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE), [Java](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-elasticdb-tools%22))
  * [Azure SQL - Entity Framework tümleştirmesi için esnek veritabanı araçları](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
  * [Betik Merkezi'nde parça esnekliği](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
* Blog: [Esnek ölçek Duyurusu](https://azure.microsoft.com/blog/20../../introducing-elastic-scale-preview-for-azure-sql-database/)
* Kanal 9: [Esnek ölçek genel bakış videosu](https://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
* Tartışma forumu: [Azure SQL veritabanının Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
* Performansını ölçmek için: [Parça eşleme yöneticisi için performans sayaçları](sql-database-elastic-database-client-library.md)

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

