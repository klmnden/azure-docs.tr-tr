---
title: Parça eşleme yöneticisi için performans sayaçları
description: ShardMapManager sınıf ve veri bağımlı Yönlendirme performans sayaçları
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
ms.date: 03/31/2018
ms.openlocfilehash: f98c09a7e51fa729ef4a940e5f3c03de55d8dfd2
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52875289"
---
# <a name="performance-counters-for-shard-map-manager"></a>Parça eşleme yöneticisi için performans sayaçları
Performansını yakalayabilirsiniz bir [parça eşleme Yöneticisi](sql-database-elastic-scale-shard-map-management.md), özellikle kullanılırken [verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md). Sayaçları Microsoft.Azure.SqlDatabase.ElasticScale.Client sınıfı yöntemleri ile oluşturulur.  

Sayaçları performansını izlemek için kullanılan [verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md) operations. Bu sayaçlar, "Elastik veritabanı: Shard Management" kategorisi altında Performans İzleyicisi'nde erişilebilir.

**En son sürümü:** Git [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Ayrıca bkz: [en yeni elastik veritabanı istemci kitaplığı kullanmak için bir uygulamayı yükseltme](sql-database-elastic-scale-upgrade-client-library.md).

## <a name="prerequisites"></a>Önkoşullar
* Performans kategorisi ve sayaçları oluşturmak için kullanıcı yerel bir parçası olmalıdır **Yöneticiler** grubunu uygulamayı barındıran makinede.  
* Bir performans sayacı örneği oluşturmak ve sayaçları güncelleştirmek için kullanıcının ya da bir üyesi olmanız gerekir **Yöneticiler** veya **Performance Monitor Users** grubu. 

## <a name="create-performance-category-and-counters"></a>Performans kategorisi ve sayaçları oluşturma
Sayaçlarınızı oluşturma işlemleri CreatePeformanceCategoryAndCounters yöntemini çağırabilirsiniz. [ShardMapManagementFactory sınıfı](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx). Yalnızca yönetici yöntem yürütebilirsiniz: 

    ShardMapManagerFactory.CreatePerformanceCategoryAndCounters()  

Ayrıca [bu](https://gallery.technet.microsoft.com/scriptcenter/Elastic-DB-Tools-for-Azure-17e3d283) metodunu yürütmek için PowerShell Betiği. Yöntemi aşağıdaki performans sayaçları oluşturur:  

* **Eşlemeleri önbelleğe**: parça eşlemesi için önbelleğe alınmış eşlemeleri sayısı.
* **DDR işlemi/sn**: parça eşlemesi için veri bağımlı yönlendirme işlemlerinin hızıdır. Bu sayaç, bir çağrı zaman güncelleştirilir [OpenConnectionForKey()](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) başarılı bir bağlantı hedef parçaya sonuçlanıyor. 
* **Arama Önbelleği İsabetli Okuma/sn eşleme**: başarılı bir önbellek parça eşlemesindeki eşlemeleri için arama işlemlerinin hızıdır. 
* **Arama önbellek isabetsizliği/sn eşleme**: eşlemeleri parça eşlemesindeki başarısız önbellek arama işlemlerinin hızıdır.
* **Eşlemeleri eklendiğinde veya Önbellek/sn**: hangi eşlemeleri eklenir veya parça eşlemesi için güncelleştirilmiş içinde önbellek fiyatı. 
* **Önbellek/sn eşlemeleri kaldırıldı**: hangi eşlemeleri kaldırılıyor parça eşlemesi için önbelleğinden oranı. 

Performans sayaçları, işlem başına her önbelleğe alınmış parça eşlemesi için oluşturulur.  

## <a name="notes"></a>Notlar
Aşağıdaki olaylar, performans sayaçları oluşturma tetikleyin:  

* Başlatma [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) ile [istekli yükleme](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerloadpolicy.aspx), her parça eşlemesi ShardMapManager içerir. Bunlar [GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx?f=255&MSPPError=-2147217396#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerFactory.GetSqlShardMapManager%28System.String,Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerLoadPolicy%29) ve [TryGetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx) yöntemleri.
* Parça eşleme başarılı arama (kullanarak [GetShardMap()](https://msdn.microsoft.com/library/azure/dn824215.aspx), [GetListShardMap()](https://msdn.microsoft.com/library/azure/dn824212.aspx) veya [GetRangeShardMap()](https://msdn.microsoft.com/library/azure/dn824173.aspx)). 
* Parça eşlemesi CreateShardMap() kullanarak başarıyla oluşturuldu.

Performans sayaçlarını eşlemeleri ve parça eşlemesi üzerinde gerçekleştirilen tüm önbellek işlemleri tarafından güncelleştirilir. Performans sayaçları örneği silinmesine DeleteShardMap () sonuç kullanarak parça eşlemesi başarıyla kaldırılması.  

## <a name="best-practices"></a>En iyi uygulamalar
* Performans kategorisi ve sayaçları oluşturulmasını ShardMapManager nesnesinin oluşturulmasını önce yalnızca bir kez gerçekleştirilmesi gerekir. Her komutun yürütülmesi CreatePerformanceCategoryAndCounters() (tüm örnekleri tarafından bildirilen veri kaybı) önceki sayaçları temizler ve yeni bir tane oluşturur.  
* Performans sayacı örneklerinin, işlem başına oluşturulur. Herhangi bir uygulama kilitlenme veya önbellekten bir parça eşlemesini kaldırma işlemi performans sayaçları örneklerin silinmesine neden olacak.  

### <a name="see-also"></a>Ayrıca bkz.
[Elastik Veritabanı özelliklerine genel bakış](sql-database-elastic-scale-introduction.md)  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->

