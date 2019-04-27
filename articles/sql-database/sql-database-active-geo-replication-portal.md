---
title: 'Azure portalı: SQL Database coğrafi çoğaltma | Microsoft Docs'
description: Başlatma yük devretme ve Azure portalını kullanarak Azure SQL veritabanı tek veya havuza alınmış bir veritabanı için coğrafi çoğaltmayı yapılandırma
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 02/13/2019
ms.openlocfilehash: 8bada96c648881a9943176c45115627a829fcc58
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60864142"
---
# <a name="configure-active-geo-replication-for-azure-sql-database-in-the-azure-portal-and-initiate-failover"></a>Azure portalı ve başlatma yük devretme Azure SQL veritabanı için etkin coğrafi çoğaltmayı yapılandırma

Bu makalede nasıl yapılacağı gösterilmektedir [tek ve havuza alınmış veritabanları için etkin coğrafi çoğaltma](sql-database-active-geo-replication.md#active-geo-replication-terminology-and-capabilities) Azure SQL veritabanı'nı kullanarak [Azure portalında](https://portal.azure.com) ve yük devretme başlatın.

Tek ve havuza alınmış veritabanlarıyla otomatik yük devretme grupları hakkında daha fazla bilgi için bkz. [en iyi uygulamalar ile tek ve havuza alınmış veritabanları yük devretme grupları kullanma](sql-database-auto-failover-group.md#best-practices-of-using-failover-groups-with-single-databases-and-elastic-pools). Yönetilen örnek (Önizleme) ile otomatik yük devretme grupları hakkında daha fazla bilgi için bkz. [en iyi uygulamalar, yönetilen örnekleriyle yük devretme grupları kullanarak](sql-database-auto-failover-group.md#best-practices-of-using-failover-groups-with-managed-instances).

## <a name="prerequisites"></a>Önkoşullar

Azure portalını kullanarak etkin coğrafi çoğaltmayı yapılandırmak için aşağıdaki kaynak gerekir:

* Bir Azure SQL veritabanı: Farklı bir coğrafi bölgeye çoğaltmak istediğiniz birincil veritabanı.

> [!Note]
> Azure portalını kullanarak, yalnızca birincil olarak aynı abonelik içindeki ikincil bir veritabanı oluşturabilirsiniz. İkincil veritabanı farklı bir abonelikte olması gerekir, kullanın [veritabanı REST API oluşturma](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) veya [ALTER veritabanı Transact-SQL API](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql).

## <a name="add-a-secondary-database"></a>İkincil bir veritabanı ekleyin

Aşağıdaki adımlar bir coğrafi çoğaltma ortaklığı yeni bir ikincil veritabanı oluşturur.  

Bir ikincil veritabanı eklemek için abonelik sahibi veya ortak sahibi olmalıdır.

İkincil veritabanı için birincil veritabanıyla aynı ada sahip ve varsayılan olarak, aynı hizmet katmanı ve işlem boyutu vardır. İkincil veritabanı, tek bir veritabanı veya havuza alınmış bir veritabanı olabilir. Daha fazla bilgi için [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md).
İkincil oluşturulan ve çekirdek değeri oluşturulmuş sonra veri yeni ikincil veritabanı için birincil veritabanından çoğaltmaya başlar.

> [!NOTE]
> Komutu, (örneğin, önceki bir coğrafi çoğaltma ilişkisini sonlandırma sonucu olarak) iş ortağı veritabanı zaten varsa başarısız olur.

1. İçinde [Azure portalında](https://portal.azure.com), coğrafi çoğaltma için ayarlanacak istediğiniz veritabanına gözatın.
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

1. İçinde [Azure portalında](https://portal.azure.com), birincil veritabanında coğrafi çoğaltma ortaklığı göz atın.
2. SQL veritabanı dikey penceresinde seçin **tüm ayarlar** > **coğrafi çoğaltma**.
3. İçinde **İKİNCİLLER** listesinde, yeni birincil veritabanı haline ve istediğiniz veritabanını seçin **yük devretme**.

    ![yük devretme](./media/sql-database-geo-replication-failover-portal/secondaries.png)
4. Tıklayın **Evet** yük devretmeyi başlatmak için.

Komutu, ikincil veritabanı birincil rolünde hemen geçer. Bu işlem genellikle 30 saniye içinde ya da daha az tamamlamanız gerekir.

Rolleri geçiş sırasında bu sırada her iki veritabanı (0-25 saniye bazında) kullanılamaz ve kısa bir süre yoktur. Birden fazla ikincil veritabanı birincil veritabanının varsa komutu otomatik olarak diğer ikinciller yeni birincil veritabanına bağlanacak şekilde yeniden yapılandırır. Tüm işlem, normal koşullarda tamamlanması bir dakikadan kısa sürer.

> [!NOTE]
> Bu komut, bir kesinti durumunda veritabanının Hızlı Kurtarma için tasarlanmıştır. Yük devretme (zorlamalı yük devretme) veri eşitleme tetikler.  Varsa birincil çevrimiçidir ve bazı verilerin kaybolması komutu verildiğinde işlem yürüten ortaya çıkabilir.

## <a name="remove-secondary-database"></a>İkincil veritabanını Kaldır

Bu işlem kalıcı olarak ikincil veritabanı çoğaltma sonlandırır ve normal bir okuma / yazma veritabanı için ikincil rolü değiştirir. İkincil veritabanı bağlantısı kesildiğinde, komut başarılı olur, ancak ikincil mu bağlantı kadar sonra okuma-yazma olmayan bir duruma geri yüklenir.  

1. İçinde [Azure portalında](https://portal.azure.com), birincil veritabanında coğrafi çoğaltma ortaklığı göz atın.
2. SQL veritabanı sayfasında, seçin **coğrafi çoğaltma**.
3. İçinde **İKİNCİLLER** listesinde, coğrafi çoğaltma ortaklığı kaldırmak istediğiniz veritabanını seçin.
4. Tıklayın **çoğaltma durdurma**.

    ![İkincil Kaldır](./media/sql-database-geo-replication-portal/remove-secondary.png)
5. Bir onay penceresi açılır. Tıklayın **Evet** coğrafi çoğaltma ortaklığı veritabanını kaldırmak için. (Okuma / yazma veritabanı herhangi bir çoğaltma parçası olmayan ayarlayın.)

## <a name="next-steps"></a>Sonraki adımlar

* Etkin coğrafi çoğaltma hakkında daha fazla bilgi için bkz: [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md).
* Otomatik Yük devretme grupları hakkında daha fazla bilgi için bkz: [otomatik yük devretme grupları](sql-database-auto-failover-group.md)
* İş sürekliliğine genel bakış ve senaryolar için bkz: [iş sürekliliğine genel bakış](sql-database-business-continuity.md).
