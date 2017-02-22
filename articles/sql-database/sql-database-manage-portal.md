---
title: "Azure SQL Veritabanını Azure Portalı kullanarak yönetme | Microsoft Belgeleri"
description: "Bulutta Azure Portalı kullanan bir ilişkisel veritabanını Azure Portalı kullanarak yönetme konusunda hızlı başvuru bilgileri."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 3a56e9de-c21a-40ba-9a35-958172cb4e5b
ms.service: sql-database
ms.custom: overview
ms.devlang: NA
ms.workload: data-management
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.date: 01/10/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 420b2153f6115dd712d3033e30f11f79b18cd80f
ms.openlocfilehash: be89a2799af3bdc2938f73e3d54f00f81d9ab9cd


---
# <a name="manage-azure-sql-databases-using-the-azure-portal"></a>Azure SQL Veritabanlarını Azure Portalı kullanarak yönetme
> [!div class="op_single_selector"]
> * [Azure portal](sql-database-manage-portal.md)
> * [SSMS](sql-database-manage-azure-ssms.md)
> * [PowerShell](sql-database-manage-powershell.md)
> 
> 

[Azure portalı](https://portal.azure.com/), Azure SQL veritabanlarını ve sunucularını oluşturmanızı, izlemenizi ve yönetmenizi sağlar. Bu makalede sık kullanılan görevlerle ilgili kısa açıklamalara ve ayrıntılı bilgilerin bağlantılarına yer verilmektedir.

> [!TIP]
> Sunucu oluşturma, sunucu tabanlı güvenlik duvarı oluşturma, sunucu özelliklerini görüntüleme, SQL Server Management Studio'yu kullanarak bağlanma, ana veritabanını sorgulama, örnek veritabanı ve boş veritabanı oluşturma, veritabanı özelliklerini sorgulama, SQL Server Management Studio kullanarak bağlanma ve örnek veritabanını sorgulama adımlarını gösteren bir öğreticiye ihtiyacınız varsa bkz. [Başlangıç Öğreticisi](sql-database-get-started.md).

## <a name="view-your-azure-sql-databases-servers-and-pools"></a>Azure SQL veritabanlarınızı, sunucularınızı ve havuzlarınızı görüntüleme
Kullanılabilir SQL Veritabanı hizmetlerini görüntülemek için **Diğer hizmetler**'e tıklayın ve arama kutusuna **SQL** yazın:

![SQL Veritabanı](./media/sql-database-manage-portal/sql-services.png)

## <a name="how-do-i-create-or-view-azure-sql-databases"></a>Azure SQL veritabanlarını nasıl oluşturabilir veya görüntüleyebilirim?
**SQL veritabanları** dikey penceresini açmak için **SQL veritabanları**'na ve çalışmak istediğiniz veritabanına tıklayın veya **+Ekle**'ye tıklayarak bir SQL veritabanı oluşturun. Ayrıntılar için bkz. [Azure portalını kullanarak dakikalar içinde bir SQL veritabanı oluşturma](sql-database-get-started.md).

![SQL veritabanları](./media/sql-database-manage-portal/sql-databases.png)

## <a name="how-do-i-create-or-view-azure-sql-servers"></a>Azure SQL sunucularını nasıl oluşturabilir veya görüntüleyebilirim?
**SQL sunucuları** dikey penceresini açmak için **SQL sunucuları**'na ve çalışmak istediğiniz sunucuya tıklayın veya **+Ekle**'ye tıklayarak bir SQL sunucusu oluşturun. Ayrıntılar için bkz. [Azure portalını kullanarak dakikalar içinde bir SQL veritabanı oluşturma](sql-database-get-started.md).

![SQL sunucuları](./media/sql-database-manage-portal/sql-servers.png)

## <a name="how-do-i-create-or-view-sql-elastic-pools"></a>SQL elastik havuzlarını nasıl oluşturabilir veya görüntüleyebilirim?
**SQL elastik havuzları** dikey penceresini açmak için **SQL elastik havuzları**'na ve çalışmak istediğiniz havuza tıklayın veya **+Ekle**'ye tıklayarak bir havuz oluşturun. Ayrıntılar için bkz. [Azure portalıyla yeni bir elastik havuz oluşturma](sql-database-elastic-pool-create-portal.md).

![SQL elastik havuzları](./media/sql-database-manage-portal/elastic-pools.png)

## <a name="how-do-i-update-or-view-sql-database-settings"></a>SQL veritabanı ayarlarını nasıl güncelleştirebilir veya görüntüleyebilirim?
Veritabanı ayarlarını görüntülemek veya güncelleştirmek için SQL veritabanı dikey penceresinde istediğiniz ayara tıklayın:

![SQL veritabanı ayarları](./media/sql-database-manage-portal/settings.png)

## <a name="how-do-i-find-a-sql-databases-fully-qualified-server-name"></a>Bir SQL veritabanının tam sunucu adını nasıl bulabilirim?
Veritabanınızın sunucu adını görüntülemek için **SQL veritabanı** dikey penceresinde **Genel bakış**'a tıklayın ve sunucu adını not edin:

![SQL veritabanı ayarları](./media/sql-database-manage-portal/server-name.png)

## <a name="how-do-i-manage-firewall-rules-to-control-access-to-my-sql-server-and-database"></a>SQL sunucuma ve veritabanıma erişimi denetlemek için güvenlik duvarı kurallarını nasıl yönetebilirim?
Güvenlik duvarı kurallarını görüntülemek, oluşturmak veya güncelleştirmek için **SQL veritabanı** dikey penceresinde **Sunucu güvenlik duvarı ayarla**'ya tıklayın. Ayrıntılar için bkz. [Azure portalını kullanarak Azure SQL Veritabanı sunucu düzeyinde güvenlik duvarı kuralı yapılandırma](sql-database-configure-firewall-settings.md).

![güvenlik duvarı kuralları](./media/sql-database-manage-portal/sql-database-firewall.png)

## <a name="how-do-i-change-my-sql-database-service-tier-or-performance-level"></a>SQL veritabanımın hizmet katmanını veya performans düzeyini nasıl değiştirebilirim?
Bir SQL veritabanının hizmet katmanını veya performans düzeyini güncelleştirmek için **SQL veritabanı** dikey penceresinde **Fiyatlandırma katmanı (DTU'ları ölçeklendirme)** seçeneğine tıklayın. Ayrıntılar için bkz. [Bir SQL veritabanının hizmet katmanını ve performans düzeyini (fiyatlandırma katmanını) değiştirme](sql-database-scale-up.md).

![fiyatlandırma katmanları](./media/sql-database-manage-portal/pricing-tier.png)

## <a name="how-do-i-configure-auditing-and-threat-detection-for-a-sql-database"></a>Bir SQL veritabanı için denetim ve tehdit algılamayı nasıl yapılandırabilirim?
Bir SQL veritabanı için denetim ve tehdit algılamayı yapılandırmak için **SQL veritabanı** dikey penceresinde **Denetim ve Tehdit algılama**'ya tıklayın. Ayrıntılar için bkz. [SQL veritabanı denetimini kullanmaya başlama](sql-database-auditing-get-started.md) ve [SQL Veritabanı Tehdit Algılanmayı kullanmaya başlama](sql-database-threat-detection-get-started.md).

## <a name="how-do-i-configure-dynamic-data-masking-for-a-sql-database"></a>Bir SQL veritabanı için dinamik veri maskelemeyi nasıl yapılandırabilirim?
Bir SQL veritabanında dinamik veri maskelemeyi yapılandırmak için **SQL veritabanı** dikey penceresinde **Dinamik veri maskeleme**'ye tıklayın. Ayrıntılar için bkz. [SQL Veritabanı Dinamik Veri Maskelemeyi kullanmaya başlama](sql-database-dynamic-data-masking-get-started.md).

## <a name="how-do-i-configure-transparent-data-encryption-tde-for-a-sql-database"></a>Bir SQL veritabanı için saydam veri şifrelemesini (TDE) nasıl yapılandırabilirim?
Bir SQL veritabanında saydam veri şifrelemesini yapılandırmak için **SQL veritabanı** dikey penceresinde **Saydam veri şifrelemesi**'ne tıklayın. Ayrıntılar için bkz. [Portalı kullanarak bir veritabanında TDE'yi etkinleştirme](https://msdn.microsoft.com/library/dn948096#Anchor_1).

## <a name="how-do-i-view-or-change-the-max-size-of-a-sql-database"></a>Bir SQL veritabanının maksimum boyutunu nasıl görüntüleyebilir veya değiştirebilirim?
Bir SQL veritabanının boyutunu görüntülemek veya değiştirmek için **SQL veritabanı** dikey penceresinde **Veritabanı boyutu**'na tıklayın. Bir veritabanının maksimum boyutunu güncelleştirmek için hizmet katmanını veya performans düzeyini değiştirin. Ayrıntılar için bkz. [Bir SQL veritabanının hizmet katmanını ve performans düzeyini (fiyatlandırma katmanını) değiştirme](sql-database-scale-up.md).

## <a name="how-do-i-monitor-and-improve-the-performance-of-a-sql-database"></a>Bir SQL veritabanının performansını nasıl izleyebilir ve geliştirebilirim?
Bir SQL veritabanının performans özelliklerini izlemek ve geliştirmek için **SQL veritabanı** dikey penceresinde **Performansa genel bakış**'a tıklayın. Ayrıntılar için bkz. [SQL Veritabanı Performans Öngörüleri](sql-database-performance.md).

## <a name="how-do-i-configure-geo-replication"></a>Coğrafi Çoğaltmayı nasıl yapılandırabilirim?
Bir SQL veritabanında Coğrafi Çoğaltmayı kurmak için **SQL veritabanı** dikey penceresinde **Coğrafi Çoğaltma**'ya tıklayın. Ayrıntılar için bkz. [Azure portalıyla Azure SQL Veritabanı için Coğrafi Çoğaltmayı yapılandırma](sql-database-geo-replication-portal.md).

## <a name="how-do-i-fail-over-to-a-geo-replicated-sql-database"></a>Coğrafi çoğaltma etkin SQL veritabanında nasıl yük devretme gerçekleştirebilirim?
Coğrafi çoğaltmalı ikincil veritabanına yük devretmek için **SQL veritabanı** dikey penceresinde **Coğrafi Çoğaltma**'ya ve ardından **Yük devri**'ne tıklayın. Ayrıntılar için bkz. [Azure portalıyla Azure SQL Veritabanı için planlı veya plansız yük devretme başlatma](sql-database-geo-replication-failover-portal.md).

## <a name="how-do-i-copy-a-sql-database"></a>Bir SQL veritabanını nasıl kopyalayabilirim?
Bir SQL veritabanını kopyalamak için **SQL veritabanı** dikey penceresinde **Kopyala**'ya tıklayın. Ayrıntılar için bkz. [Azure portalını kullanarak Azure SQL veritabanını kopyalama](sql-database-copy-portal.md).

![SQL veritabanı ayarları](./media/sql-database-manage-portal/sql-database-copy.png)

## <a name="how-do-i-archive-an-azure-sql-database-to-a-bacpac-file"></a>Bir Azure SQL veritabanını BACPAC dosyasına nasıl arşivleyebilirim?
Bir SQL veritabanından BACPAC dosyası oluşturmak için **SQL veritabanı** dikey penceresinde **Dışarı aktar**'a tıklayın. Ayrıntılar için bkz. [Azure portalını kullanarak bir Azure SQL veritabanını BACPAC dosyasına arşivleme](sql-database-export.md).

![SQL veritabanını dışarı aktarma](./media/sql-database-manage-portal/sql-database-export.png)

## <a name="how-do-i-restore-a-sql-database-to-a-previous-point-in-time"></a>Bir SQL veritabanını daha önceki bir noktaya nasıl geri yükleyebilirim?
Bir SQL veritabanını geri yüklemek için **SQL veritabanı** dikey penceresinde **Geri yükle**'ye tıklayın. Ayrıntılar için bkz. [Azure portalı ile bir Azure SQL Veritabanı’nı önceki bir zamana geri yükleme](sql-database-point-in-time-restore.md).

![SQL veritabanı ayarları](./media/sql-database-manage-portal/sql-database-restore.png)

## <a name="how-do-i-create-an-azure-sql-database-from-a-bacpac-file"></a>Bir BACPAC dosyasından nasıl bir Azure SQL veritabanı oluşturabilirim?
Bir BACPAC dosyasından bir SQL veritabanı oluşturmak için **SQL sunucusu** dikey penceresinde **Veritabanını içeri aktar**'a tıklayın. Ayrıntılar için bkz. [Azure SQL veritabanı oluşturmak için bir BACPAC dosyasını içeri aktarma](sql-database-import.md).

![SQL sunucusu](./media/sql-database-manage-portal/server-commands.png)

## <a name="how-do-i-restore-a-deleted-sql-database"></a>Silinen bir SQL veritabanını nasıl geri yükleyebilirim?
Silinen bir SQL veritabanını geri yüklemek için **SQL sunucusu** dikey penceresinde (silinen veritabanının bulunduğu SQL sunucusu) **Silinen veritabanları**'na tıklayın. Ayrıntılar için bkz. [Silinen bir Azure SQL veritabanını Azure portalını kullanarak geri yükleme](sql-database-restore-deleted-database-portal.md).

## <a name="how-do-i-delete-a-sql-database"></a>Bir SQL veritabanını nasıl silebilirim?
Bir SQL veritabanını silmek için **SQL veritabanı** dikey penceresinde **Sil**'e tıklayın. 

![SQL veritabanı ayarları](./media/sql-database-manage-portal/sql-database-delete.png)

## <a name="additional-resources"></a>Ek kaynaklar
* [SQL Veritabanı](sql-database-technical-overview.md)
* [Azure portalıyla bir elastik havuzu izleme ve yönetme](sql-database-elastic-pool-manage-portal.md)




<!--HONumber=Dec16_HO3-->


