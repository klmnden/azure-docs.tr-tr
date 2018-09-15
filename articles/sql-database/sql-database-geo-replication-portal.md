---
title: 'Azure portalı: SQL Database coğrafi çoğaltma | Microsoft Docs'
description: Azure portalı ve başlatma yük devretme, Azure SQL veritabanı için coğrafi çoğaltmayı yapılandırma
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 8faf6713a5fd8287b5f9e30976e80172c2c42f05
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2018
ms.locfileid: "45631302"
---
# <a name="configure-active-geo-replication-for-azure-sql-database-in-the-azure-portal-and-initiate-failover"></a>Azure portalı ve başlatma yük devretme Azure SQL veritabanı için etkin coğrafi çoğaltmayı yapılandırma

Bu makalede, SQL veritabanı için etkin coğrafi çoğaltmayı yapılandırma işlemini göstermektedir [Azure portalında](http://portal.azure.com) ve yük devretme başlatın.

Azure portalı ile yük devretmeyi başlatmak için bkz: [planlanmış veya planlanmamış bir yük devretme, Azure portalı ile Azure SQL veritabanı için başlatmak](sql-database-geo-replication-portal.md).

Azure portalını kullanarak etkin coğrafi çoğaltmayı yapılandırmak için aşağıdaki kaynak gerekir:

* Bir Azure SQL veritabanı: farklı bir coğrafi bölgeye çoğaltmak istediğiniz birincil veritabanı.

> [!Note]
Etkin coğrafi çoğaltma veritabanları aynı abonelikte olmalıdır.

## <a name="add-a-secondary-database"></a>İkincil bir veritabanı ekleyin
Aşağıdaki adımlar bir coğrafi çoğaltma ortaklığı yeni bir ikincil veritabanı oluşturur.  

Bir ikincil veritabanı eklemek için abonelik sahibi veya ortak sahibi olmalıdır.

İkincil veritabanı için birincil veritabanıyla aynı ada sahip ve varsayılan olarak, aynı hizmet düzeyi vardır. İkincil veritabanı, tek bir veritabanı veya elastik bir havuzdaki bir veritabanı olabilir. Daha fazla bilgi için [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md).
İkincil oluşturulan ve çekirdek değeri oluşturulmuş sonra veri yeni ikincil veritabanı için birincil veritabanından çoğaltmaya başlar.

> [!NOTE]
> Komutu, (örneğin, önceki bir coğrafi çoğaltma ilişkisini sonlandırma sonucu olarak) iş ortağı veritabanı zaten varsa başarısız olur.
> 

1. İçinde [Azure portalında](http://portal.azure.com), coğrafi çoğaltma için ayarlanacak istediğiniz veritabanına gözatın.
2. SQL veritabanı sayfasında, seçin **coğrafi çoğaltma**ve ardından ikincil veritabanı oluşturmak için bölge seçin. Birincil veritabanını barındıran bölgesi dışında herhangi bir bölgeyi seçebilirsiniz, ancak önerilir [eşleştirilmiş bölge](../best-practices-availability-paired-regions.md).
   
    ![Coğrafi çoğaltmayı yapılandırma](./media/sql-database-geo-replication-portal/configure-geo-replication.png)
3. Seçebilir veya ikincil veritabanı için fiyatlandırma katmanı ve sunucu yapılandırabilirsiniz.
   
    ![İkincil yapılandırın](./media/sql-database-geo-replication-portal/create-secondary.png)
4. İsteğe bağlı olarak, ikincil bir veritabanını elastik havuzlara da ekleyebilirsiniz. Bir havuzda ikincil veritabanı oluşturmak için tıklayın **elastik havuz** ve hedef sunucuda bir havuz seçin. Bir havuzu hedef sunucuda zaten mevcut olmalıdır. Bu iş akışı, bir havuzu oluşturmaz.
5. Tıklayın **Oluştur** ikincil eklemek için.
6. İkincil bir veritabanı oluşturulur ve dengeli dağıtım işlemi başlar.
   
    ![İkincil yapılandırın](./media/sql-database-geo-replication-portal/seeding0.png)
7. Dengeli dağıtım işlemi tamamlandıktan sonra ikincil veritabanı durumunu görüntüler.
   
    ![Dengeli Dağıtım tamamlandı](./media/sql-database-geo-replication-portal/seeding-complete.png)

## <a name="initiate-a-failover"></a>Bir yük devretme başlatın

İkincil veritabanının birincil veritabanı haline değiştirilebilir.  

1. İçinde [Azure portalında](http://portal.azure.com), birincil veritabanında coğrafi çoğaltma ortaklığı göz atın.
2. SQL veritabanı dikey penceresinde seçin **tüm ayarlar** > **coğrafi çoğaltma**.
3. İçinde **İKİNCİLLER** listesinde, yeni birincil veritabanı haline ve istediğiniz veritabanını seçin **yük devretme**.
   
    ![Yük devretme](./media/sql-database-geo-replication-failover-portal/secondaries.png)
4. Tıklayın **Evet** yük devretmeyi başlatmak için.

Komutu, ikincil veritabanı birincil rolünde hemen geçer. 

Rolleri geçiş sırasında bu sırada her iki veritabanı (0-25 saniye bazında) kullanılamaz ve kısa bir süre yoktur. Birden fazla ikincil veritabanı birincil veritabanının varsa komutu otomatik olarak diğer ikinciller yeni birincil veritabanına bağlanacak şekilde yeniden yapılandırır. Tüm işlem, normal koşullarda tamamlanması bir dakikadan kısa sürer. 

> [!NOTE]
> Bu komut, bir kesinti durumunda veritabanının Hızlı Kurtarma için tasarlanmıştır. Yük devretme (zorlamalı yük devretme) veri eşitleme tetikler.  Varsa birincil çevrimiçidir ve bazı verilerin kaybolması komutu verildiğinde işlem yürüten ortaya çıkabilir. 
> 
> 

## <a name="remove-secondary-database"></a>İkincil veritabanını Kaldır
Bu işlem kalıcı olarak ikincil veritabanı çoğaltma sonlandırır ve normal bir okuma / yazma veritabanı için ikincil rolü değiştirir. İkincil veritabanı bağlantısı kesildiğinde, komut başarılı olur, ancak ikincil mu bağlantı kadar sonra okuma-yazma olmayan bir duruma geri yüklenir.  

1. İçinde [Azure portalında](http://portal.azure.com), birincil veritabanında coğrafi çoğaltma ortaklığı göz atın.
2. SQL veritabanı sayfasında, seçin **coğrafi çoğaltma**.
3. İçinde **İKİNCİLLER** listesinde, coğrafi çoğaltma ortaklığı kaldırmak istediğiniz veritabanını seçin.
4. Tıklayın **çoğaltma durdurma**.
   
    ![İkincil Kaldır](./media/sql-database-geo-replication-portal/remove-secondary.png)
5. Bir onay penceresi açılır. Tıklayın **Evet** coğrafi çoğaltma ortaklığı veritabanını kaldırmak için. (Okuma / yazma veritabanı herhangi bir çoğaltma parçası olmayan ayarlayın.)

## <a name="next-steps"></a>Sonraki adımlar
* Etkin coğrafi çoğaltma hakkında daha fazla bilgi için bkz: [etkin coğrafi çoğaltma](sql-database-geo-replication-overview.md).
* İş sürekliliğine genel bakış ve senaryolar için bkz: [iş sürekliliğine genel bakış](sql-database-business-continuity.md).

