---
title: Parça eşleme sorunlarını gidermek için kurtarma Yöneticisi'ni kullanarak | Microsoft Docs
description: RecoveryManager sınıfı ile parça eşlemesi sorunları çözmek için kullanın
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
ms.date: 01/03/2019
ms.openlocfilehash: 1bab1ed9e2a24b0a84f4327d47a910934319b397
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61475901"
---
# <a name="using-the-recoverymanager-class-to-fix-shard-map-problems"></a>RecoveryManager sınıfı ile parça eşleme sorunlarını düzeltme

[RecoveryManager](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager) sınıfı ADO.NET uygulamaları kolayca algılayıp genel parça eşleme (GSM) parçalı veritabanlarını ortamında yerel parça eşlemesinin (LSM) arasındaki tutarsızlıkları düzeltmek olanağı sağlar.

Parçalı bir ortamda her bir veritabanı eşleme LSM ve GSM izleyin. Bazen, bir kesme LSM GSM arasında gerçekleşir. Bu durumda, algılamak ve sonu onarmak için RecoveryManager sınıfı kullanın.

RecoveryManager sınıfı parçasıdır [elastik veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md).

![Parça eşlemesi][1]

Terim tanımları için bkz: [esnek veritabanı araçları sözlüğü](sql-database-elastic-scale-glossary.md). Anlamak için nasıl **ShardMapManager** kullanılan veri parçalı bir çözümde yönetmek için bkz: [parça eşleme Yönetimi](sql-database-elastic-scale-shard-map-management.md).

## <a name="why-use-the-recovery-manager"></a>Neden kurtarma Yöneticisi'ni kullanın

Parçalı veritabanını ortamında, bir kiracı başına veritabanı ve sunucu başına çok sayıda veritabanı yoktur. Olabilir pek çok sunucu ortamında. Doğru sunucu ve veritabanı için çağrıları yönlendirilebilir için her veritabanı parça eşlemesinde eşlenir. Veritabanlarına göre izlenen bir **parçalama anahtarı**, ve her parça atanmış bir **anahtar değer aralığının**. Örneğin, bir parçalama anahtarı müşteri adları "D" "F" için temsil edebilir Tüm parçaları (veritabanlarının olarak da bilinir) ve kendi eşleme aralıkları eşleme içerdiği **genel parça eşleme (GSM)**. Her veritabanı bir harita olarak da bilinen parça yer alan aralıkların de içeren **yerel parça eşlemesinin (LSM)**. Bir uygulama bir parçaya bağlandığında, eşleme için hızlı alma uygulaması önbelleğe alınır. LSM önbelleğe alınmış verileri doğrulamak için kullanılır.

Aşağıdaki nedenlerle LSM ve GSM eşitlenmemiş hale gelebilir:

1. Aralığı artık kullanın veya bir parça yeniden adlandırma düşünülen parça silme işlemi. Bir parça silme sonuçlanıyor bir **parça eşleme yalnız bırakılmış**. Benzer şekilde, yeniden adlandırılmış bir veritabanı yalnız bırakılmış parça eşleme neden olabilir. Amacına bağlı olarak değişiklik, parçanın kaldırılması gerekebilir veya parça konumunda güncelleştirilmesi gerekiyor. Silinen bir veritabanını kurtarmak için bkz: [silinen bir veritabanını geri yükleme](sql-database-recovery-using-backups.md).
2. Coğrafi olarak yük devretme olayı oluşur. Devam etmek için bir gerekir sunucu adını ve uygulamadaki parça eşleme Yöneticisi veritabanı adı güncelleştirilene ve sonra tüm parçalarda parça eşlemesi için parça eşlemesi ayrıntıları. Bir coğrafi olarak yük devretme varsa, böyle bir kurtarma mantık yük devretme iş akışını otomatik hale getirilmelidir. Kurtarma eylemleri otomatikleştirme coğrafi etkin hale getirilmiş veritabanları için uyumlu bir yönetilebilirlik sağlar ve İnsan el ile yapılan Eylemler önler. Bir veri merkezi arızasına ise bir veritabanını kurtarmak için seçenekler hakkında bilgi edinmek için [iş sürekliliği](sql-database-business-continuity.md) ve [olağanüstü durum kurtarma](sql-database-disaster-recovery.md).
3. Bir parça veya ShardMapManager veritabanının bir önceki noktası bileşenini duruma geri yüklenir. Yedeklemeleri kullanarak zaman kurtarma noktası hakkında bilgi edinmek için bkz. [yedeklemeleri kullanarak kurtarma](sql-database-recovery-using-backups.md).

Azure SQL veritabanı elastik veritabanı araçları, coğrafi çoğaltma ve geri yükleme hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Genel Bakış: SQL veritabanı ile iş sürekliliği ve veritabanı olağanüstü durum kurtarma bulut](sql-database-business-continuity.md)
* [Esnek veritabanı araçlarını kullanmaya başlayın](sql-database-elastic-scale-get-started.md)  
* [ShardMap Yönetimi](sql-database-elastic-scale-shard-map-management.md)

## <a name="retrieving-recoverymanager-from-a-shardmapmanager"></a>Bir ShardMapManager RecoveryManager alınıyor

İlk adım bir RecoveryManager örneği oluşturmaktır. [GetRecoveryManager yöntemi](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getrecoverymanager) geçerli kurtarma Yöneticisi döndürür [ShardMapManager](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager) örneği. Parça eşlemesi bulunan tüm tutarsızlıkları gidermenin ilk belirli parça eşlemesi için RecoveryManager alınması gerekir.

   ```java
    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnectionString,  
             ShardMapManagerLoadPolicy.Lazy);
             RecoveryManager rm = smm.GetRecoveryManager();
   ```

Bu örnekte, ShardMapManager RecoveryManager başlatılır. Bir ShardMap içeren ShardMapManager de zaten başlatıldı.

Bu uygulama kodu parça Haritası işleyen olduğundan, bağlantı tarafından başvurulan GSM veritabanı üzerinde okuma / yazma izinlerine sahip olan kimlik bilgilerini (yukarıdaki örnekte, smmConnectionString) Fabrika yönteminde kullanılan kimlik bilgileri olmalıdır dize. Bu kimlik bilgileri, verilere bağımlı yönlendirme bağlantılarını açmak için kullanılan kimlik bilgileri genellikle farklıdır. Daha fazla bilgi için [elastik veritabanı istemci kimlik bilgilerini kullanarak](sql-database-elastic-scale-manage-credentials.md).

## <a name="removing-a-shard-from-the-shardmap-after-a-shard-is-deleted"></a>Bir parça silindikten sonra bir parça ShardMap kaldırılıyor

[DetachShard yöntemi](https://docs.microsoft.com/previous-versions/azure/dn842083(v=azure.100)) verilen parça parça eşlemesinden ayırır ve parça ile ilişkili eşlemeleri siler.  

* Konum, özellikle sunucu adını ve veritabanı adını ayrılmakta parça parça konumunda parametredir.
* Parça eşleme adı shardMapName parametredir. Bu sadece, birden çok parça eşlemesi aynı parça eşleme Yöneticisi tarafından yönetildiğinde gerekli. İsteğe bağlı.

> [!IMPORTANT]
> Eminseniz, güncelleştirilmiş eşlemesi için aralığı boş ise yalnızca bu tekniği kullanın. Kodunuzda denetimler şunlardır en iyisidir yukarıdaki yöntemleri taşınan, aralığı için veri denetlemez.

Bu örnekte parça parça eşlemden kaldırır.

   ```java
   rm.DetachShard(s.Location, customerMap);
   ```

Parça eşlemesi parçanın silmeden önce GSM parça konumunda yansıtır. Parça silindiğinden, bu, bilinçli ve parçalama anahtar aralığı artık kullanılmıyor kabul edilir. Aksi durumda, zaman içinde nokta geri yükleme yürütebilirsiniz. bir önceki-belirli bir noktaya parça kurtarılır. (Bu durumda, parça tutarsızlıklar algılamak için aşağıdaki bölümü gözden geçirin.) Kurtarmak için bkz: [zaman kurtarma noktasında](sql-database-recovery-using-backups.md).

Veritabanı silme işlemi kasıtlı varsayıldığından son yönetim temizleme eylemi parça parça eşleme Yöneticisi'nin girişine silmektir. Bu uygulama yanlışlıkla beklenmiyor bir aralığı için bilgi yazmasını engeller.

## <a name="to-detect-mapping-differences"></a>Eşleme farkları algılamak için

[DetectMappingDifferences yöntemi](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.detectmappingdifferences) seçer ve her iki parça eşlemesi (GSM ve LSM) eşleşmeleri uzlaştırır parça eşlemesi (yerel veya genel) gerçekte kaynağı olarak döndürür.

   ```java
   rm.DetectMappingDifferences(location, shardMapName);
   ```

* *Konumu* sunucu adını ve veritabanı adını belirtir.
* *ShardMapName* parametredir parça eşleme adı. Bu sadece, birden çok parça eşlemesi aynı parça eşleme Yöneticisi tarafından yönetilen, gerekli. İsteğe bağlı.

## <a name="to-resolve-mapping-differences"></a>Eşleme farklılıkları çözmeye

[ResolveMappingDifferences yöntemi](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.resolvemappingdifferences) parça eşlemesi (yerel veya genel) birini gerçeklik kaynağı olarak seçer ve her iki parça eşlemesi (GSM ve LSM) eşleşmeleri uzlaştırır.

   ```java
   ResolveMappingDifferences (RecoveryToken, MappingDifferenceResolution.KeepShardMapping);
   ```

* *RecoveryToken* parametre eşlemeleri GSM ve belirli parça için LSM arasındaki farklılıkları numaralandırır.
* [MappingDifferenceResolution numaralandırma](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.mappingdifferenceresolution) parça eşlemeleri arasındaki farkı çözmek için yöntem belirtmek için kullanılır.
* **MappingDifferenceResolution.KeepShardMapping** doğru eşleme LSM içerdiğinde ve bu nedenle parça eşlemesindeki kullanılmalıdır önerilir. Bir yük devretme ise genellikle böyledir: parça artık yeni bir sunucuda yer alır. (RecoveryManager.DetachShard yöntemi kullanılarak) GSM parça kaldırılmalıdır olduğundan, bir eşleme üzerinde GSM artık yok. Bu nedenle, LSM parça eşlemesini yeniden oluşturmak için kullanılması gerekir.

## <a name="attach-a-shard-to-the-shardmap-after-a-shard-is-restored"></a>Bir parça geri yüklendikten sonra ShardMap için parça ekleme

[AttachShard yöntemi](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.attachshard) verilen parça parça eşlemesine ekler. Parça eşleme tutarsızlıkları algılar ve eşlemeleri parça parça geri noktasında eşleşecek şekilde güncelleştirir. Zaman içinde nokta geri yüklemesi, eklenen zaman damgasına sahip yeni bir veritabanı varsayılan olarak bu yana (parça kurulduğu önce) özgün veritabanı adı yansıtacak şekilde veritabanı da yeniden adlandırılır varsayılır.

   ```java
   rm.AttachShard(location, shardMapName)
   ```

* *Konumu* parametresi, sunucu adını ve veritabanı adını iliştirilmekte parça.
* *ShardMapName* parametredir parça eşleme adı. Bu sadece, birden çok parça eşlemesi aynı parça eşleme Yöneticisi tarafından yönetildiğinde gerekli. İsteğe bağlı.

Bu örnek, bir önceki noktası bileşenini saatten kısa bir süre önce geri yüklendi parça eşlemesine bir parça ekler. (Yani LSM parçada eşlemesi) parça geri olduğundan, potansiyel olarak GSM parça girişi ile tutarsız. Bu örnek kod dışında parça geri ve veritabanının özgün adına yeniden adlandırıldı. Geri yüklenen olduğundan, güvenilen eşleme LSM eşlemede bir varsayılır.

   ```java
   rm.AttachShard(s.Location, customerMap);
   var gs = rm.DetectMappingDifferences(s.Location);
      foreach (RecoveryToken g in gs)
       {
       rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping);
       }
   ```

## <a name="updating-shard-locations-after-a-geo-failover-restore-of-the-shards"></a>Bir coğrafi olarak yük devretme (Kurtarma) parça parça konumlarını güncelleştiriliyor

Bir coğrafi olarak yük devretme varsa, ikincil veritabanı yazma erişilebilir oluşturulur ve yeni birincil veritabanı haline gelir. Sunucu ve büyük olasılıkla (yapılandırmanıza bağlı olarak), veritabanı adını, özgün birincil farklı olabilir. Bu nedenle LSM ve GSM parçada eşleme girişleri düzeltilmesi gerekir. Benzer şekilde, zaman içinde veritabanını farklı bir ad veya konum ya da daha önceki bir noktaya geri yüklenirse, bu tutarsızlıklar parça eşlemesi neden olabilir. Parça eşleme Yöneticisi doğru veritabanına açık bağlantılar dağılımını yönetir. Dağıtım parça eşlemesi ve hedefi olan uygulamaları istek parçalama anahtarı değerini verileri temel alır. Bir coğrafi olarak yük devretme, bu bilgiler doğru sunucu adını, veritabanı adı ve kurtarılan veritabanının parça eşleme ile güncelleştirilmelidir.

## <a name="best-practices"></a>En iyi uygulamalar

Coğrafi olarak yük devretme ve kurtarma genellikle bir Azure SQL veritabanı iş sürekliliği özellikleri birini kasıtlı olarak kullanan uygulama bulut Yöneticisi tarafından yönetilen işlemlerdir. İş sürekliliği planlama, işlemler, yordamları ve işle ilgili işlemlerin kesintisiz devam etmesini sağlamak için ölçüler gerektirir. RecoveryManager sınıfı parçası LSM ve GSM güncel tutulduğundan emin olmak için bu iş akışı içinde kullanılması gereken olarak kullanılabilen yöntemler gerçekleştirilen kurtarma eylemlere göre. Bir yük devretme olayından sonra doğru bilgileri LSM ve GSM yansıtacak düzgün şekilde sağlamak için beş temel adımı vardır. Bu adımları yürütmek için uygulama kodu, var olan araçlarınız ve iş akışı tümleştirilebilir.

1. RecoveryManager ShardMapManager alın.
2. Eski parça parça eşlemesinden çıkarın.
3. Yeni parça parça konuma dahil olmak üzere parça eşlemesine ekleyin.
4. Eşlemesinde GSM LSM arasındaki tutarsızlıkları algılayın.
5. GSM LSM güvenen LSM arasındaki farklar çözümleyin.

Bu örnek, aşağıdaki adımları gerçekleştirir:

1. Parça parça konumlarını yük devretme olayından önce gösterecek parça eşlemesi kaldırır.
2. Parçalar ("Configuration.SecondaryServer" yeni sunucu adı aynı veritabanı adında ancak parametredir) yeni parça konumlarını yansıtan parça eşlemesi ekler.
3. GSM ve her parça için LSM arasındaki eşleme farkları algılayarak kurtarma belirteçlerini alır.
4. Her parçanın LSM eşlemesinden güvenerek tutarsızlıklar çözümler.

   ```java
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
