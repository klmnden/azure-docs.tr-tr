---
title: "Parça eşleme sorunları düzeltmek için kurtarma Yöneticisi'ni kullanarak | Microsoft Docs"
description: "Parça Haritalar ile sorunları çözmek için RecoveryManager sınıfı kullanın"
services: sql-database
manager: craigg
author: stevestein
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 10/25/2016
ms.author: sstein
ms.openlocfilehash: 45dc16c7bf54f34c89f2e9208a7cee06de03c0da
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="using-the-recoverymanager-class-to-fix-shard-map-problems"></a>RecoveryManager sınıfı ile parça eşleme sorunlarını düzeltme
[RecoveryManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.aspx) sınıfı ADO.Net uygulamaları kolayca algılamak ve genel parça eşleme (GSM) parçalı veritabanı ortamında yerel parça eşleme (LSM) arasındaki tutarsızlıkları düzeltmek olanağı sağlar. 

GSM ve LSM parçalı bir ortamda her veritabanı eşleme izler. Bazen, bir kesme GSM ve LSM arasında oluşur. Bu durumda, algılamak ve sonu onarmak için RecoveryManager sınıfını kullanın.

RecoveryManager sınıfı parçası olan [esnek veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md). 

![Parça eşleme][1]

Terim tanımları için bkz: [esnek veritabanı araçlarını sözlüğü](sql-database-elastic-scale-glossary.md). Anlamak için nasıl **ShardMapManager** kullanılan veri parçalı bir çözümde yönetmek için bkz: [parça eşleme Yönetim](sql-database-elastic-scale-shard-map-management.md).

## <a name="why-use-the-recovery-manager"></a>Kurtarma Yöneticisi'ni neden kullanılır?
Parçalı veritabanı ortamında, bir kiracı veritabanı başına ve sunucu başına birçok veritabanı yok. Ayrıca olabilir pek çok sunucu ortamında. Çağrılar doğru sunucu ve veritabanı yönlendirilebilir şekilde her veritabanı parça eşlemesinde eşlenir. Veritabanları göre izlenen bir **parçalama anahtar**, ve her parça atanan bir **anahtar değerlerin**. Örneğin, bir parçalama anahtar müşteri adları "D" "F" gösterebilir Tüm parça (diğer adıyla veritabanları) ve bunların eşleme aralıkları eşlenmesini içerdiği **genel parça eşleme (GSM)**. Her veritabanı olarak bilinen parça üzerinde yer alan aralıkları haritasını de içeren **yerel parça eşleme (LSM)**. Bir uygulama için bir parça bağlandığında, eşleme uygulama hızlı alma için önbelleğe alınır. LSM önbelleğe alınmış verileri doğrulamak için kullanılır. 

GSM ve LSM aşağıdaki nedenlerle eşitlenmemiş hale gelebilir:

1. Bağlantı aralığı artık kullanın veya bir parça yeniden adlandırma düşünülen bir parça silme. Bir parça silme sonuçlanıyor bir **parça eşleme yalnız**. Benzer şekilde, yeniden adlandırılmış bir veritabanını yalnız bırakılmış parça eşleme neden olabilir. Değişikliğin amacı, bağlı olarak parça kaldırılması gerekebilir veya parça konumun güncelleştirilmesi gerekiyor. Silinen bir veritabanını kurtarmak için bkz: [silinen bir veritabanını geri](sql-database-recovery-using-backups.md).
2. Bir yük devretme coğrafi olayı oluşur. Devam etmek için bir sunucu adını ve veritabanı adını parça eşleme Yöneticisi'nde uygulama güncelleştirin ve sonra bir parça eşlemindeki tüm parça parça eşleme ayrıntılarını güncelleştirin. Bir yük devretme coğrafi ise, böyle bir kurtarma mantık yük devretme iş akışı içinde otomatik olarak. Kurtarma eylemleri otomatikleştirme coğrafi etkinleştirilmiş veritabanları için uyumlu bir yönetilebilirlik sağlar ve el ile İnsan Eylemler önler. Veri Merkezi kesintisinden ise bir veritabanını kurtarmak için seçenekler hakkında bilgi edinmek için bkz: [iş sürekliliği](sql-database-business-continuity.md) ve [olağanüstü durum kurtarma](sql-database-disaster-recovery.md).
3. Bir parça veya ShardMapManager veritabanının bir önceki noktası bileşenini duruma geri yüklenir. Yedeklemeler kullanarak zaman kurtarma noktası hakkında bilgi edinmek için [Yedekleme kullanarak kurtarma](sql-database-recovery-using-backups.md).

Azure SQL veritabanı esnek veritabanı araçlarını, coğrafi çoğaltma ve geri yükleme hakkında daha fazla bilgi için aşağıdakilere bakın: 

* [Genel Bakış: SQL veritabanı ile iş devamlılığı ve veritabanı olağanüstü durum kurtarma bulut](sql-database-business-continuity.md) 
* [Esnek veritabanı araçlarını kullanmaya başlama](sql-database-elastic-scale-get-started.md)  
* [ShardMap Yönetimi](sql-database-elastic-scale-shard-map-management.md)

## <a name="retrieving-recoverymanager-from-a-shardmapmanager"></a>Bir ShardMapManager RecoveryManager alınıyor
İlk adım bir RecoveryManager örneği oluşturmaktır. [GetRecoveryManager yöntemi](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getrecoverymanager.aspx) geçerli kurtarma Yöneticisi döndürür [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) örneği. Parça eşleme bulunan tüm tutarsızlıkları çözmek için önce belirli parça harita RecoveryManager almanız gerekir. 

   ```
    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString,  
             ShardMapManagerLoadPolicy.Lazy);
             RecoveryManager rm = smm.GetRecoveryManager(); 
   ```

Bu örnekte, RecoveryManager ShardMapManager başlatılır. Bir ShardMap içeren ShardMapManager de zaten başlatılmış. 

Bu uygulama kodu parça eşleme değiştirdiğinde bu yana (önceki örnekte, smmConnectionString) Fabrika yönteminde kullanılan kimlik bilgileri bağlantı dizesi tarafından başvurulan GSM veritabanı üzerinde okuma-yazma izinlerine sahip kimlik bilgileri olmalıdır. Bu kimlik bilgileri, veri bağımlı yönlendirme bağlantıları'nı açmak için kullanılan kimlik bilgileri genellikle farklıdır. Daha fazla bilgi için bkz: [esnek veritabanı istemci kimlik bilgilerini kullanarak](sql-database-elastic-scale-manage-credentials.md).

## <a name="removing-a-shard-from-the-shardmap-after-a-shard-is-deleted"></a>Bir parça silindikten sonra bir parça ShardMap kaldırma
[DetachShard yöntemi](https://msdn.microsoft.com/library/azure/dn842083.aspx) verilen parça parça eşlemesinden ayırır ve parça ile ilişkili eşlemeleri siler.  

* Konum parametresi parça, özellikle sunucu adını ve veritabanı adını ayrılmakta parça konumdur. 
* Parça eşleme adı shardMapName parametresidir. Bu yalnızca olan birden çok parça eşlemesi aynı parça eşleme Yöneticisi tarafından yönetilen gereklidir. İsteğe bağlı. 


> [!IMPORTANT]
> Eminseniz, aralığın güncelleştirilmiş eşlemesi için boş ise yalnızca bu tekniği kullanın. Denetimleri kodunuzda dahil etmek en iyisidir yukarıdaki yöntemleri taşınan, aralığı için veri denetlemez.
>

Bu örnek parça parça eşlemesinden kaldırır. 

   ```
   rm.DetachShard(s.Location, customerMap);
   ``` 

Parça eşleme parça silinmesini önce GSM parça konumda yansıtır. Parça silindiği için bu kasıtlı ve parçalama anahtar aralığı artık kullanımda varsayılır. Aksi durumda, zaman içinde nokta geri yükleme yürütebilir. bir önceki noktası zaman parça kurtarılır. (Bu durumda, parça tutarsızlıklar algılamak için aşağıdaki bölümü gözden geçirin.) Kurtarmak için bkz: [zaman kurtarma noktası](sql-database-recovery-using-backups.md).

Veritabanı silme kasıtlı varsayıldığından son yönetim temizleme eylemi parça parça eşleme Yöneticisi'nde girişe silmektir. Bu, istemeden beklenmiyor bir aralık bilgi yazmasını uygulama engeller.

## <a name="to-detect-mapping-differences"></a>Eşleme farklılıkları algılamak için
[DetectMappingDifferences yöntemi](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.detectmappingdifferences.aspx) seçer ve her iki parça eşlemeleri (GSM ve LSM) eşlemesi uzlaştırır parça eşlemeleri (yerel veya genel) gerçekte kaynağı olarak döndürür.

   ```
   rm.DetectMappingDifferences(location, shardMapName);
   ```

* *Konumu* sunucu adını ve veritabanı adını belirtir. 
* *ShardMapName* parametredir parça eşleme adı. Bu yalnızca olan birden çok parça eşlemesi aynı parça eşleme Yöneticisi tarafından yönetilen varsa gerekli. İsteğe bağlı. 

## <a name="to-resolve-mapping-differences"></a>Eşleme farkları gidermek için
[ResolveMappingDifferences yöntemi](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.resolvemappingdifferences.aspx) parça eşlemeleri (yerel veya genel) birini gerçekte kaynağı olarak seçer ve her iki parça eşlemeleri (GSM ve LSM) eşlemesi uzlaştırır.

   ```
   ResolveMappingDifferences (RecoveryToken, MappingDifferenceResolution.KeepShardMapping);
   ```

* *RecoveryToken* parametre eşlemeleri GSM ve belirli parça LSM arasındaki farkları numaralandırır. 
* [MappingDifferenceResolution numaralandırma](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.mappingdifferenceresolution.aspx) parça eşlemeleri arasındaki farkı çözmek için yöntem belirtmek için kullanılır. 
* **MappingDifferenceResolution.KeepShardMapping** is recommended that when the LSM contains the accurate mapping and therefore the mapping in the shard should be used. Bir yük devretme ise genellikle böyledir: parça şimdi yeni bir sunucuda bulunuyor. Parça (RecoveryManager.DetachShard yöntemi kullanılarak) GSM kaldırılmalıdır olduğundan, bir eşleme üzerinde GSM artık yok. Bu nedenle, LSM parça eşleme yeniden oluşturmak için kullanılması gerekir.

## <a name="attach-a-shard-to-the-shardmap-after-a-shard-is-restored"></a>Bir parça geri yüklendikten sonra bir parça ShardMap ekleme
[AttachShard yöntemi](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.attachshard.aspx) verilen parça parça eşlemesi ekler. Parça eşleme tutarsızlıkları algılar ve eşlemeleri parça parça geri yükleme noktasında eşleşecek şekilde güncelleştirir. Zaman içinde nokta geri yükleme damgasıyla eklenen yeni bir veritabanı için varsayılan olarak bu yana veritabanı (parça geri önce) özgün veritabanı adı, yansıtmak üzere de adlandırılır varsayılır. 

   ```
   rm.AttachShard(location, shardMapName)
   ``` 

* *Konumu* parametredir sunucu adını ve veritabanı adını iliştirilmekte parça. 
* *ShardMapName* parametredir parça eşleme adı. Bu yalnızca olan birden çok parça eşlemesi aynı parça eşleme Yöneticisi tarafından yönetilen gereklidir. İsteğe bağlı. 

Bu örnek önceki noktası-zaman bir yakın zamanda geri parça eşleme bir parça ekler. (Yani LSM içinde parça için eşleme) parça geri olduğundan, büyük olasılıkla tutarsız GSM parça girişle kalır. Bu örnek kod dışında parça geri ve veritabanını özgün adına yeniden adlandırıldı. Geri yüklenen olduğundan, güvenilen eşleme eşleme LSM içinde olduğu varsayılır. 

   ```
   rm.AttachShard(s.Location, customerMap); 
   var gs = rm.DetectMappingDifferences(s.Location); 
      foreach (RecoveryToken g in gs) 
       { 
       rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping); 
       } 
   ```

## <a name="updating-shard-locations-after-a-geo-failover-restore-of-the-shards"></a>Bir coğrafi-yük devretme (geri yükleme) parça parça konumları güncelleştiriliyor
Bir yük devretme coğrafi varsa, ikincil veritabanı yazma erişilebilir oluşturulur ve yeni birincil veritabanı haline gelir. Sunucu ve büyük olasılıkla (yapılandırmanıza bağlı olarak), veritabanı adını, özgün birincil sunucudan farklı olabilir. Bu nedenle GSM ve LSM parça eşleme girdileri düzeltilmelidir. Benzer şekilde, zaman içinde veritabanı farklı bir ad veya konum veya daha önceki bir noktaya geri yüklenirse, bu tutarsızlıkları parça eşlemelerinin neden olabilir. Parça eşleme Yöneticisi'nin doğru veritabanı açık bağlantıları dağıtım işleme. Dağıtım parça eşleme ve uygulamaları isteği hedefidir parçalama anahtarının değerini veriler temel alır. Coğrafi-yük devretme sonrasında, bu bilgileri doğru sunucu adını, veritabanı adının ve kurtarılan veritabanının parça eşleme ile güncelleştirilmesi gerekir. 

## <a name="best-practices"></a>En iyi uygulamalar
Coğrafi yük devretme ve kurtarma genellikle kasıtlı olarak Azure SQL veritabanları iş sürekliliği özellikleri birini kullanan uygulaması bir bulut Yöneticisi tarafından yönetilen işlemleridir. İş sürekliliği planlama işlemleri, yordamlar ve işletme işlemleri kesintisiz devam etmesini sağlamak için ölçümleri gerektirir. GSM ve LSM güncel tutulduğundan emin olmak için bu iş akışı içinde kullanılmalıdır RecoveryManager sınıfı bir parçası olarak kullanılabilir yöntemleri gerçekleştirilen kurtarma eylemi temel. GSM ve LSM bir yük devretme olayından sonra doğru bilgileri yansıtacak düzgün şekilde sağlamak için beş temel adımı vardır. Bu adımları yürütmek için uygulama kodu mevcut araçlar ve iş akışı tümleştirilebilir. 

1. RecoveryManager ShardMapManager alın. 
2. Eski parça parça eşlemesinden kullanımdan çıkarın.
3. Yeni parça parça konuma dahil olmak üzere parça eşlemeye ekleyin.
4. GSM LSM arasındaki eşlemesinde tutarsızlıklarını algıla. 
5. GSM ve LSM güvenen LSM arasındaki farklılıkları giderin. 

Bu örnekte aşağıdaki adımları gerçekleştirir:

1. Parça parça konumlarını yük devretme olayından önce gösterecek parça eşleme kaldırır.
2. Parça parça ("Configuration.SecondaryServer" yeni sunucu adı aynı veritabanı adında ancak parametredir) yeni parça konumlar yansıtma eşlemesi ekler.
3. GSM ve her parça LSM arasındaki eşleme farkları algılayarak kurtarma belirteçlerini alır. 
4. Her parça LSM eşlemesinden güvenerek tutarsızlıklar çözümler. 
   
   ```
   var shards = smm.GetShards(); 
   foreach (shard s in shards) 
   { 
     if (s.Location.Server == Configuration.PrimaryServer) 
   
         { 
          ShardLocation slNew = new ShardLocation(Configuration.SecondaryServer, s.Location.Database); 
   
          rm.DetachShard(s.Location); 
   
          rm.AttachShard(slNew); 
   
          var gs = rm.DetectMappingDifferences(slNew); 
   
          foreach (RecoveryToken g in gs) 
            { 
               rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping); 
            } 
        } 
    } 
   ```

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-database-recovery-manager/recovery-manager.png

