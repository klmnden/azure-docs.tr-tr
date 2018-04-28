---
title: 'Azure portal: SQL Database coğrafi çoğaltma | Microsoft Docs'
description: Azure portalı ve başlatma yük devretme Azure SQL veritabanı için coğrafi çoğaltma yapılandırma
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: article
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: 89839d18e90a75f81e78edcb7f54e77a31ea4886
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="configure-active-geo-replication-for-azure-sql-database-in-the-azure-portal-and-initiate-failover"></a>Aktif coğrafi çoğaltma Azure portal ve başlatma yük devretme Azure SQL veritabanı için yapılandırma

Bu makalede, SQL veritabanında için etkin coğrafi çoğaltma yapılandırma gösterilmektedir [Azure portal](http://portal.azure.com) ve yük devretme başlatın.

Azure portal ile yük devretme başlatmak için bkz: [planlanmış veya planlanmamış bir yük devretme, Azure portalı ile Azure SQL veritabanı için başlatmak](sql-database-geo-replication-portal.md).

Aktif coğrafi çoğaltma Azure portalını kullanarak yapılandırmak için aşağıdaki kaynak gerekir:

* Azure SQL veritabanını: farklı bir coğrafi bölgeye çoğaltmak istediğiniz birincil veritabanı.

> [!Note]
Aktif coğrafi çoğaltma veritabanları aynı abonelikte arasında olmalıdır.

## <a name="add-a-secondary-database"></a>İkincil bir veritabanı ekleyin
Aşağıdaki adımlar bir coğrafi çoğaltma ortaklığı yeni ikincil bir veritabanı oluşturun.  

İkincil bir veritabanı eklemek için abonelik sahibi veya ortak sahibi olmalıdır.

İkincil veritabanını birincil veritabanı ile aynı ada ve varsayılan olarak, aynı hizmet düzeyinde sahiptir. İkincil veritabanı tek bir veritabanı veya veritabanı esnek havuzdaki olabilir. Daha fazla bilgi için bkz: [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [vCore tabanlı satın alma modeli (Önizleme)](sql-database-service-tiers-vcore.md).
İkincil oluşturulan ve dağıtılan sonra verileri yeni ikincil veritabanına birincil veritabanından çoğaltmaya başlar.

> [!NOTE]
> (Örneğin, bir önceki coğrafi çoğaltma ilişkisi sonlandırma sonucunda) ortak veritabanı zaten var. komutu başarısız olur.
> 

1. İçinde [Azure portal](http://portal.azure.com), coğrafi çoğaltma için ayarlamak istediğiniz veritabanına gözatın.
2. SQL veritabanı sayfasında seçin **coğrafi çoğaltma**ve ardından ikincil veritabanını oluşturmak için bölge seçin. Birincil veritabanını barındıran bölgesi dışında herhangi bir bölgeyi seçebilirsiniz, ancak öneririz [eşleştirilmiş bölge](../best-practices-availability-paired-regions.md).
   
    ![Coğrafi çoğaltmayı yapılandırma](./media/sql-database-geo-replication-portal/configure-geo-replication.png)
3. Seçin veya sunucuyu ve fiyatlandırma katmanı ikincil veritabanı için yapılandırın.
   
    ![İkincil yapılandırın](./media/sql-database-geo-replication-portal/create-secondary.png)
4. İsteğe bağlı olarak, ikincil bir veritabanını bir esnek havuz ekleyebilirsiniz. Bir havuzda ikincil veritabanını oluşturmak için tıklatın **esnek havuz** ve hedef sunucudaki bir havuz seçin. Bir havuzu hedef sunucuda zaten mevcut olmalıdır. Bu iş akışı bir havuzu oluşturmaz.
5. Tıklatın **oluşturma** ikincil kopya eklemek için.
6. İkincil veritabanı oluşturulur ve dengeli dağıtım işlemi başlar.
   
    ![İkincil yapılandırın](./media/sql-database-geo-replication-portal/seeding0.png)
7. Dengeli dağıtım işlemi tamamlandığında, ikincil veritabanı durumunu görüntüler.
   
    ![Tam üretme](./media/sql-database-geo-replication-portal/seeding-complete.png)

## <a name="initiate-a-failover"></a>Bir yük devretme başlatın

İkincil veritabanının birincil olmasını değiştirilebilir.  

1. İçinde [Azure portal](http://portal.azure.com), coğrafi çoğaltma ortaklığı birincil veritabanında konumuna göz atın.
2. SQL veritabanı dikey seçin **tüm ayarları** > **coğrafi çoğaltma**.
3. İçinde **İKİNCİLLER** listesinde, yeni birincil hale tıklatıp istediğiniz veritabanını seçin **yük devretme**.
   
    ![Yük devretme](./media/sql-database-geo-replication-failover-portal/secondaries.png)
4. Tıklatın **Evet** yük devretmeyi başlatmak için.

Komut, ikincil veritabanı birincil rolde hemen geçirir. 

Rolleri geçiş sırasında hangi sırasında her iki veritabanı (0-25 saniye terabayt) kullanılamaz kısa bir süre yoktur. Birincil veritabanında birden fazla ikincil veritabanı varsa, komut yeni birincil bağlanmak için diğer ikincil kopya otomatik olarak yeniden yapılandırır. Tüm işlemi normal koşullarda tamamlanması bir dakikadan az zamanınızı. 

> [!NOTE]
> Bu komut, bir kesinti durumunda veritabanının Hızlı Kurtarma için tasarlanmıştır. Yük devretme (yük devretme zorlanır) veri eşitleme tetikler.  Varsa birincil çevrimiçi olduğunu ve bazı veri kaybı komutu verildiğinde işlem yürüten ortaya çıkabilir. 
> 
> 

## <a name="remove-secondary-database"></a>İkincil veritabanını Kaldır
Bu işlem kalıcı olarak ikincil veritabanı için çoğaltma sonlandırır ve normal bir okuma-yazma veritabanı rolü ikincil değiştirir. İkincil veritabanı bağlantısını bozuksa, komut başarılı olur, ancak ikincil mu bağlantı kadar sonra okuma-yazma olmayan bir duruma geri yüklenir.  

1. İçinde [Azure portal](http://portal.azure.com), coğrafi çoğaltma ortaklığı birincil veritabanında konumuna göz atın.
2. SQL veritabanı sayfasında seçin **coğrafi çoğaltma**.
3. İçinde **İKİNCİLLER** listesinde, coğrafi çoğaltma ortaklığı kaldırmak istediğiniz veritabanını seçin.
4. Tıklatın **çoğaltmayı durdurma**.
   
    ![İkincil Kaldır](./media/sql-database-geo-replication-portal/remove-secondary.png)
5. Bir onay penceresi açılır. Tıklatın **Evet** coğrafi çoğaltma ortaklığı veritabanını kaldırmak için. (Bu bir okuma-yazma veritabanına herhangi çoğaltma'nın parçası olmayan ayarlanır.)

## <a name="next-steps"></a>Sonraki adımlar
* Aktif coğrafi çoğaltma hakkında daha fazla bilgi için bkz: [aktif coğrafi çoğaltma](sql-database-geo-replication-overview.md).
* İş sürekliliğine genel bakış ve senaryolar için bkz: [iş sürekliliğine genel bakış](sql-database-business-continuity.md).

