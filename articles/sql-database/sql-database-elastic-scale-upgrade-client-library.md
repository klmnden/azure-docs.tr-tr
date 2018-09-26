---
title: En son elastik veritabanı istemci kitaplığını yükseltme | Microsoft Docs
description: Nuget yükseltme elastik veritabanı istemci kitaplığını kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-scale
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: 9fb29b18397be83f5dc56464b3366d91c47f43b3
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47160800"
---
# <a name="upgrade-an-app-to-use-the-latest-elastic-database-client-library"></a>En yeni elastik veritabanı istemci kitaplığı kullanmak için bir uygulamayı yükseltme
Yeni sürümlerini [elastik veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md) NuGetand Visual Studio'daki NuGetPackage Yöneticisi arabirimi aracılığıyla kullanılabilir. Yükseltmeleri hata düzeltmelerini içeren ve istemci Kitaplığı'nın yeni özelliklerini destekler.

**En son sürümü:** Git [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Yeni kitaplığıyla uygulamanızı yeniden gibi yeni özellikleri desteklemek için Azure SQL veritabanlarınızda depolanan parça eşleme Yöneticisi meta verilerinizi mevcut değiştirin.

Adımları sırayla gerçekleştirilmesi meta veri nesnesi güncelleştirildiğinde, istemci Kitaplığı'nın eski sürümleri artık eski sürümü meta veri nesnelerinin yükseltmeden sonra oluşturulmaz anlamına gelir, ortamınızda mevcut olmasını sağlar.   

## <a name="upgrade-steps"></a>Yükseltme adımları
**1. Uygulamalarınızı yükseltin.** Visual Studio'da indirin ve tüm kitaplığını kullanan geliştirme projelerinizi en son istemci kitaplığı sürümü başvuru; Daha sonra yeniden oluşturun ve dağıtın. 

* Visual Studio çözümünüzü seçin **Araçları** --> **NuGet Paket Yöneticisi** -->  **çözüm için NuGet paketlerini Yönet**. 
* (Visual Studio 2013) Sol bölmede bulunan seçin **güncelleştirmeleri**ve ardından **güncelleştirme** paket düğmesinde **Azure SQL veritabanı elastik ölçeklendirme istemci kitaplığını** penceresinde görünür.
* (Visual Studio 2015) Filtre kutusunun kümesine **kullanılabilir yükseltme**. Güncelleştirmek ve paketi seçin **güncelleştirme** düğmesi.
* (Visual Studio 2017) İletişim kutusunun üstündeki seçin **güncelleştirmeleri**. Güncelleştirmek ve paketi seçin **güncelleştirme** düğmesi.
* Oluşturun ve dağıtın. 

**2. Komut dosyalarınızı yükseltin.** Kullanıyorsanız **PowerShell** parçalar yönetmek için betikleri [yeni kitaplık sürümünü indirin](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) ve kendisinden betikleri yürüttüğünüz dizinine kopyalayın. 

**3. Ayırma-birleştirme hizmetinizi yükseltin.** Parçalı verileri, elastik veritabanı bölme-birleştirme aracını kullanırsanız [indirin ve aracının en son sürümünü dağıtmak](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/). Hizmet bulunabilir yükseltme adımları ayrıntılı [burada](sql-database-elastic-scale-overview-split-and-merge.md). 

**4. Parça eşleme Yöneticisi veritabanlarınızı yükseltme**. Azure SQL veritabanı'nda, parça eşlemesi destekleyici meta veriler yükseltin.  PowerShell veya C# kullanarak Bunu gerçekleştirmenin iki yolu vardır. Her iki seçenek aşağıda gösterilmektedir.

***1. seçenek: PowerShell kullanarak meta verileri yükseltme***

1. Nuget'ten en yeni komut satırı yardımcı programını indirin [burada](http://nuget.org/nuget.exe) ve bir klasöre kaydedin. 
2. Bir komut istemi açın ve sonra da aynı klasöre gidin, komutu Yürüt: `nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`
3. Az önce örneğin indirdiğiniz yeni istemci DLL sürümü içeren alt klasörüne gidin: `cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`
4. Esnek veritabanı istemci yükseltme scriptlet'dan indirin [betik Merkezi](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9), DLL içeren klasöre kaydedin.
5. Bu klasördeki "PowerShell.\upgrade.ps1" Komut İstemi'nden çalıştırın ve yönergeleri izleyin.

***2. seçenek: C# kullanarak meta verileri yükseltme***

Alternatif olarak, ShardMapManager açar, tüm parçalar yinelenir ve yöntemleri çağırarak meta veri yükseltmesi gerçekleştiren bir Visual Studio uygulama oluşturma [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) ve [UpgradeGlobalStore ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) Bu örnekte olduğu gibi: 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 

    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

Bu teknikler meta verileri yükseltme için birden çok kez zarar uygulanabilir. Eski bir sürüm zaten güncelleştirdikten sonra yanlışlıkla bir parça oluşturur, örneğin, yükseltmeyi yeniden en son meta veri sürümü altyapınız mevcut olduğundan emin olmak için tüm parçalar arasında çalıştırabilirsiniz. 

**Not:** istemci Kitaplığı'nın yeni sürümleri yayımlanan tarih Azure SQL DB ve tam tersini parça eşleme Yöneticisi meta verileri'nın önceki sürümleriyle çalışmaya devam eder.   Ancak, en son istemci bazı yeni özelliklerden yararlanmak için meta verileri yükseltilmesi gerekiyor.   Meta verileri yükseltme herhangi bir kullanıcı verileri veya uygulamaya özgü verileri, oluşturulan ve parça eşleme Yöneticisi tarafından kullanılan yalnızca nesneleri etkilemez unutmayın.  Ve uygulamaları aracılığıyla yukarıda açıklanan yükseltme sırasını çalışmaya devam eder. 

## <a name="elastic-database-client-version-history"></a>Elastik veritabanı istemci sürüm geçmişi
Sürüm geçmişi için Git [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png

