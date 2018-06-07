---
title: En son esnek veritabanı istemci kitaplığına yükseltme | Microsoft Docs
description: Nuget yükseltme esnek veritabanı istemci kitaplığına kullanın.
services: sql-database
manager: craigg
author: stevestein
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: 56500a21822f7c7122a20cb676b9ad307de49c87
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34647019"
---
# <a name="upgrade-an-app-to-use-the-latest-elastic-database-client-library"></a>En son esnek veritabanı istemci kitaplığı kullanmak için bir uygulama yükseltme
Yeni sürümlerini [esnek veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md) NuGetand Visual Studio NuGetPackage Yöneticisi arabiriminde aracılığıyla kullanılabilir. Yükseltmeler istemci kitaplığı yeni özellikler için destek ve hata düzeltmeleri içerir.

**En son sürümü için:** Git [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Uygulamanızı yeni kitaplığı ile yeniden gibi yeni özellikleri desteklemek için Azure SQL veritabanlarının depolanan parça eşleme Yöneticisi meta verileriniz varolan değiştirin.

Adımları sırayla gerçekleştirilmesi meta veri nesnesi güncelleştirildiğinde, istemci kitaplığının eski sürümleri artık eski sürümü meta veri nesnelerinin yükseltmeden sonra oluşturulmaz anlamına gelir, ortamınızda mevcut olmasını sağlar.   

## <a name="upgrade-steps"></a>Yükseltme adımları
**1. Uygulamalarınızı yükseltin.** Visual Studio'da karşıdan yükle ve en son istemci kitaplığı sürümü tüm kitaplığı kullanarak geliştirme projelerinizi başvuru; Daha sonra yeniden oluşturun ve dağıtın. 

* Visual Studio çözümünüzde seçin **Araçları** --> **NuGet Paket Yöneticisi** -->  **çözüm için NuGet paketlerini Yönet**. 
* (Visual Studio 2013) Sol panelinde seçin **güncelleştirmeleri**ve ardından **güncelleştirme** paket düğmesinde **Azure SQL Database esnek ölçek istemci Kitaplığı** penceresinde görünür.
* (Visual Studio 2015) Filtre kutusuna kümesine **kullanılabilir yükseltme**. Güncelleştirme ve tıklatın istediğiniz paketi seçin **güncelleştirme** düğmesi.
* (Visual Studio 2017) İletişim kutusunun üstündeki seçin **güncelleştirmeleri**. Güncelleştirme ve tıklatın istediğiniz paketi seçin **güncelleştirme** düğmesi.
* Derleme ve dağıtma. 

**2. Komut dosyalarınızı yükseltin.** Kullanıyorsanız **PowerShell** parça, yönetmek için betikler [yeni kitaplık sürümü yüklemek](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) ve kendisinden, yürütme komut dosyaları dizinine kopyalayın. 

**3. Bölünmüş birleştirme hizmetinizi yükseltin.** Parçalı veriler yeniden düzenlemek için esnek veritabanı bölünmüş birleştirme aracını kullanırsanız [indirin ve aracının en son sürümünü dağıtmak](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/). Hizmet bulunabilir yükseltme adımlarını ayrıntılı [burada](sql-database-elastic-scale-overview-split-and-merge.md). 

**4. Parça eşleme Yöneticisi veritabanlarınızı yükseltme**. Azure SQL veritabanı'nda, parça eşlemeleri destekleme meta verileri yükseltin.  Bu, PowerShell veya C# kullanarak gerçekleştirmenin iki yolu vardır. Her iki seçenek aşağıda verilmiştir.

***Seçenek 1: Meta veri PowerShell kullanarak yükseltme***

1. Son komut satırı yardımcı programı Nuget'ten için karşıdan [burada](http://nuget.org/nuget.exe) ve bir klasöre kaydedin. 
2. Bir komut istemi açın, aynı klasöre gidin ve komutu yürütün: `nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`
3. Yalnızca, örneğin yüklediğiniz yeni istemci DLL sürümü içeren alt klasöre gidin: `cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`
4. Esnek veritabanı istemci yükseltme Resimli'nden indirin [Komut Merkezi](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9)ve dll dosyasını içeren klasöre kaydedin.
5. Bu klasörü "PowerShell.\upgrade.ps1" komut isteminden çalıştırın ve yönergeleri izleyin.

***Seçenek 2: C# kullanarak meta verileri yükseltme***

Alternatif olarak, meta veri yükseltme yöntemlerini çağırarak gerçekleştirir, ShardMapManager açar ve tüm parça tekrarlanan bir Visual Studio uygulaması oluşturma [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) ve [UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) Bu örnekte olduğu gibi: 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 

    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

Bu teknikler meta verileri yükseltmeler için birden çok kez zarar uygulanabilir. Örneğin, eski bir sürüm zaten güncelleştirdikten sonra yanlışlıkla bir parça oluşturur, yükseltme son meta veri sürümü, altyapınız mevcut olduğundan emin olmak için tüm parça genelinde yeniden komutunu çalıştırabilirsiniz. 

**Not:** istemci Kitaplığı'nın yeni sürümleri yayımlanan tarih için Azure SQL DB ve tam tersini parça eşleme Yöneticisi meta verileri önceki sürümleri ile çalışmaya devam eder.   Ancak, en son istemcisinde bazı yeni özelliklerden yararlanmak için meta veri yükseltilmesi gerekiyor.   Meta veri yükseltme tüm kullanıcı verileri ve uygulamaya özgü verileri, oluşturulan ve parça eşleme Yöneticisi tarafından kullanılan nesneler yalnızca etkilemez unutmayın.  Ve uygulamaları aracılığıyla yukarıda açıklanan yükseltme sırası çalışmaya devam eder. 

## <a name="elastic-database-client-version-history"></a>Esnek veritabanı istemci sürüm geçmişi
Sürüm geçmişi için Git [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png

