---
title: Parça eşleme yöneticisi için performans sayaçları
description: ShardMapManager sınıf ve veri bağımlı Yönlendirme performans sayaçları
services: sql-database
manager: craigg
author: stevestein
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: 9c134ee96f7749529ab665df041cfc51c979acde
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34647332"
---
# <a name="performance-counters-for-shard-map-manager"></a>Parça eşleme yöneticisi için performans sayaçları
Performansını yakalamak için bir [parça eşleme Yöneticisi](sql-database-elastic-scale-shard-map-management.md), özellikle kullanırken [veri bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md). Sayaçları Microsoft.Azure.SqlDatabase.ElasticScale.Client sınıfı yöntemleri ile oluşturulur.  

Sayaçları performansını izlemek için kullanılan [veri bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md) işlemleri. Bu sayaçlar, "Esnek veritabanı: Parça Yönetimi" kategorisi altında Performans İzleyicisi'nde erişilebilir.

**En son sürümü için:** Git [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Ayrıca bkz. [son esnek veritabanı istemci kitaplığı kullanmak için bir uygulama yükseltme](sql-database-elastic-scale-upgrade-client-library.md).

## <a name="prerequisites"></a>Önkoşullar
* Performans kategorisi ve sayaçları oluşturmak için kullanıcının yerel bir parçası olması gerekir **Yöneticiler** uygulamayı barındıran makinede grup.  
* Bir performans sayacı örneği oluşturmak ve sayaçları güncelleştirmek için kullanıcı ya da bir üyesi olmalıdır **Yöneticiler** veya **Performance Monitor Users** grubu. 

## <a name="create-performance-category-and-counters"></a>Performans kategorisi ve sayaçları oluşturma
Sayaçları oluşturmak için CreatePeformanceCategoryAndCounters yöntemini çağırın [ShardMapManagmentFactory sınıfı](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx). Yalnızca bir yönetici yöntemi yürütebilir: 

    ShardMapManagerFactory.CreatePerformanceCategoryAndCounters()  

Aynı zamanda [bu](https://gallery.technet.microsoft.com/scriptcenter/Elastic-DB-Tools-for-Azure-17e3d283) yöntemi yürütmek için PowerShell Betiği. Yöntemi, aşağıdaki performans sayaçları oluşturur:  

* **Eşlemeleri önbelleğe**: parça eşleme için önbelleğe alınmış eşlemeleri sayısı.
* **DDR işlemi/sn**: parça harita veri bağımlı yönlendirme işlemlerinin hızıdır. Bu sayaç bir çağrı onayladığında [OpenConnectionForKey()](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) hedef parça başarılı bir bağlantı sonuçlanıyor. 
* **Arama Önbelleği İsabetli Okuma/sn eşleme**: parça eşlemesindeki eşlemeleri başarılı önbellek arama işlemlerinin hızıdır. 
* **Arama önbellek isabetsizliği/sn eşleme**: parça eşlemesindeki eşlemeleri başarısız önbellek arama işlemlerinin hızıdır.
* **Eşleme eklendi veya Önbellek/sn güncelleştirildi**: hangi eşlemeleri eklenir veya parça harita içinde güncelleştirilmiş önbelleğine hızda. 
* **Eşlemeleri kaldırılan Önbellek/sn**: hangi eşlemeleri kaldırılır parça harita önbelleğinden oranı. 

Performans sayaçları, işlem başına her önbelleğe alınmış parça eşleme için oluşturulur.  

## <a name="notes"></a>Notlar
Aşağıdaki olaylar, performans sayaçlarını oluşturulmasını tetikler:  

* Başlatma [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) ile [istekli yükleme](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerloadpolicy.aspx), ShardMapManager herhangi parça eşlemeler içeriyorsa. Bunlar [GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx?f=255&MSPPError=-2147217396#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerFactory.GetSqlShardMapManager%28System.String,Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerLoadPolicy%29) ve [TryGetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx) yöntemleri.
* Parça eşleme başarılı arama (kullanarak [GetShardMap()](https://msdn.microsoft.com/library/azure/dn824215.aspx), [GetListShardMap()](https://msdn.microsoft.com/library/azure/dn824212.aspx) veya [GetRangeShardMap()](https://msdn.microsoft.com/library/azure/dn824173.aspx)). 
* Parça eşleme CreateShardMap() kullanarak başarılı oluşturma.

Performans sayaçları, parça eşleme ve eşlemeleri gerçekleştirilen tüm önbellek işlemleri tarafından güncelleştirilir. Başarılı kaldırma DeleteShardMap () sonuç performans sayaçları örneği silinmesine kullanarak parça eşleme.  

## <a name="best-practices"></a>En iyi uygulamalar
* Performans kategorisi ve sayaçları oluşturulmasını ShardMapManager nesne oluşturması işleminden önce yalnızca bir kez gerçekleştirilmesi gerekir. Her komutun yürütülmesi için CreatePerformanceCategoryAndCounters() (tüm örnekleri tarafından bildirilen veri kaybı) önceki sayaçları temizler ve yenilerini oluşturur.  
* Performans sayacı örnekleri her işlem için oluşturulur. Herhangi bir uygulama kilitlenme veya önbellekten parça eşleme kaldırılmasını performans sayaçları örnekleri silinmesine neden olacak.  

### <a name="see-also"></a>Ayrıca bkz.
[Elastik Veritabanı özelliklerine genel bakış](sql-database-elastic-scale-introduction.md)  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->

