---
title: SQL veritabanı olağanüstü durum kurtarma | Microsoft Docs
description: Bölgesel veri merkezi kesintisinden veya Azure SQL veritabanı etkin coğrafi çoğaltma ve coğrafi geri yükleme özelliklerini hatası bir veritabanını kurtarmak öğrenin.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: article
ms.date: 04/04/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 6ac4b26e1b014da792791ca657c9f51230a135b5
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="restore-an-azure-sql-database-or-failover-to-a-secondary"></a>Bir Azure SQL Database veya yük devretme için ikincil bir geri yükleme
Azure SQL veritabanı bir kesintisi kurtarmak için aşağıdaki özellikleri sunar:

* [Etkin coğrafi çoğaltma ve yük devretme gruplar](sql-database-geo-replication-overview.md)
* [coğrafi geri yükleme](sql-database-recovery-using-backups.md#point-in-time-restore)
* [Bölge olarak yedekli veritabanları](sql-database-high-availability.md)

İş sürekliliği senaryoları ve bu senaryolar destekleyen özellikler hakkında bilgi edinmek için bkz: [iş sürekliliği](sql-database-business-continuity.md).

> [!NOTE]
> Bölge olarak yedekli Premium ya da iş kritik veritabanları veya havuzları (Önizleme) kullanıyorsanız, kurtarma işlemini otomatik hale getirilmiştir ve bu yazıda kalan geçerli değildir. 

### <a name="prepare-for-the-event-of-an-outage"></a>Bir kesinti olayı için hazırlama
Yük devretme grupları veya coğrafi olarak yedekli yedeklemeler kullanarak başka bir veri bölgesi için Kurtarma ile başarı için bir sunucu hazırlamak gereken başka bir veri merkezinde yeni birincil sunucu olarak kesinti artırılması gereken yanı sıra belgelenen iyi tanımlanmış adımlara sahip ortaya ve bir düzgün kurtarma emin olmak için test. Hazırlık adımları içerir:

* Yeni birincil sunucu olarak başka bir bölgede mantıksal sunucu tanımlayın. Coğrafi geri yükleme için bu genellikle bir sunucu olan [eşleştirilmiş bölge](../best-practices-availability-paired-regions.md) veritabanınızın bulunduğu bölge için. Bu, coğrafi geri yükleme işlemleri sırasında ek trafiği maliyetini ortadan kaldırır.
* Belirleyin ve isteğe bağlı olarak, üzerinde kullanıcıların yeni birincil veritabanına erişmek için gerekli sunucu düzeyinde güvenlik duvarı kural tanımlarsınız.
* Bağlantı dizeleri değiştirme veya DNS girdilerini değiştirerek gibi yeni birincil sunucu, kullanıcılara yönlendirmek için nasıl adımıdır belirler.
* Belirleyin ve isteğe bağlı olarak oluşturmak, yeni birincil sunucu ana veritabanındaki mevcut olması ve bu oturumlar varsa ana veritabanında uygun izinlere sahip olduğundan emin olun oturum açma bilgileri. Daha fazla bilgi için bkz: [SQL veritabanı güvenlik olağanüstü durum kurtarma işleminden sonra](sql-database-geo-replication-security-config.md)
* Yeni birincil veritabanına eşlemek için güncelleştirilmesi gereken uyarı kuralları tanımlar.
* Belge geçerli birincil veritabanı üzerinde denetim yapılandırma
* Gerçekleştirmek bir [olağanüstü durum kurtarma ayrıntıya](sql-database-disaster-recovery-drills.md). Coğrafi geri yükleme için bir kesinti benzetimini yapmak için silebilir veya uygulama bağlantı hatası neden kaynak veritabanını yeniden adlandırın. Yük devretme gruplarını kullanarak bir kesinti benzetimini yapmak için web uygulaması veya sanal makine veritabanına veya yük devretme veritabanı bağlı uygulama bağlantısı hataları neden devre dışı bırakabilirsiniz.

## <a name="when-to-initiate-recovery"></a>Kurtarma zamanı
Kurtarma işlemi uygulama etkiler. SQL bağlantı dizesi veya DNS kullanarak yeniden yönlendirme değiştirilmesi gerektirir ve kalıcı veri kaybına neden olabilir. Bu nedenle, yalnızca kesinti uygulamanızın kurtarma süresi hedefi daha uzun son sırada yapılmalıdır. Üretim için uygulama dağıtıldığında normal uygulama sistem durumunu izleme gerçekleştirin ve kurtarma garanti olduğunu onaylanacak aşağıdaki veri noktaları kullanmanız gerekir:

1. Veritabanına kalıcı bağlantı hatası uygulama katmanından.
2. Azure portal geniş etkiyle bölgede bir olay hakkında bir uyarı gösterir.

> [!NOTE]
> Yük devretme grupları kullanıyorsanız ve otomatik yük devretme seçtiyseniz kurtarma otomatik ve şeffaf uygulama işlemidir. 

Kapalı kalma süresi ve olası iş yükümlülük, uygulama toleransını bağlı olarak aşağıdaki kurtarma seçeneklerine düşünebilirsiniz.

Kullanım [alma kurtarılabilir veritabanı](https://msdn.microsoft.com/library/dn800985.aspx) (*LastAvailableBackupDate*) en son coğrafi olarak çoğaltılmış geri yükleme noktası almak için.

## <a name="wait-for-service-recovery"></a>Servis kurtarma için bekleyin
Titizlikle hızla mümkün olduğunca ancak kök bağlı olarak neden olarak hizmet kullanılabilirliği geri yüklemek için Azure ekiplerin çalışma saat veya gün sürebilir.  Uygulamanızın önemli kapalı kalma süresi dayanabileceği, kurtarma için yalnızca bekleyebilirsiniz. Bu durumda, herhangi bir eyleminiz gereklidir. Geçerli Hizmet durumu görebilirsiniz bizim [Azure hizmet sağlığı panosunu](https://azure.microsoft.com/status/). Bölge kurtarma işleminden sonra uygulamanızın kullanılabilirlik geri kazanılır.

## <a name="fail-over-to-geo-replicated-secondary-server-in-the-failover-group"></a>Coğrafi olarak çoğaltılmış ikincil sunucu yük devretme grubunda yük
Uygulamanızın kapalı kalma iş yükümlülük neden olabilir, yük devretme grupları kullanmanız. Bir kesinti durumunda farklı bir bölgede kullanılabilirlik hızlı bir şekilde geri uygulama sağlar. Bilgi edinmek için nasıl [yük devretme gruplarını yapılandırma](sql-database-geo-replication-portal.md).

Veritabanlarının kullanılabilirliğini geri yüklemek için desteklenen yöntemlerden birini kullanarak ikincil sunucu yük devretme başlatın gerekir.

Coğrafi olarak çoğaltılmış bir ikincil veritabanı devretmek için aşağıdaki kılavuzları birini kullanın:

* [Azure Portalı'nı kullanarak bir coğrafi olarak çoğaltılmış ikincil sunucuya yük devri](sql-database-geo-replication-portal.md)
* [PowerShell kullanarak ikincil sunucuya yük devri](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)

## <a name="recover-using-geo-restore"></a>Coğrafi geri yükleme kullanarak kurtarma
Uygulamanızın kapalı kalma süresi içinde iş yükümlülük sağlamazsa kullanabileceğiniz [coğrafi geri yükleme](sql-database-recovery-using-backups.md) uygulama veritabanlarını kurtarmak için bir yöntem olarak. En son coğrafi olarak yedekli yedeklemesinden veritabanının bir kopyasını oluşturur.

## <a name="configure-your-database-after-recovery"></a>Kurtarma işleminden sonra veritabanını Yapılandır
Bir kesintisinden kurtarma için coğrafi geri yükleme kullanıyorsanız, normal uygulama işlevi devam ettirilebilir böylece yeni veritabanları için bağlantı düzgün yapılandırıldığından emin olmanız gerekir. Bu, kurtarılan veritabanının üretim hazır hale getirmek için görevlerin bir listesi verilmektedir.

### <a name="update-connection-strings"></a>Bağlantı dizelerini güncelleştir
Kurtarılan veritabanı içinde farklı bir sunucuya bulunduğundan, bu sunucuya işaret edecek şekilde uygulamanızın bağlantı dizesini güncellemeniz gerekir.

Bağlantı dizeleri değiştirme hakkında daha fazla bilgi için uygun geliştirme dilini bkz, [bağlantı kitaplığı](sql-database-libraries.md).

### <a name="configure-firewall-rules"></a>Güvenlik duvarı kurallarını yapılandırma
Server ve veritabanı üzerinde yapılandırılmış güvenlik duvarı kuralları birincil sunucu ve birincil veritabanı üzerinde yapılandırılmış eşleştiğinden emin olmanız gerekir. Daha fazla bilgi için bkz: [nasıl yapılır: Güvenlik Duvarı ayarlarını yapılandırma (Azure SQL veritabanı)](sql-database-configure-firewall-settings.md).

### <a name="configure-logins-and-database-users"></a>Oturum açma ve veritabanı kullanıcıları yapılandırın
Uygulamanız tarafından kullanılan tüm oturumları, kurtarılan veritabanı barındırma sunucusu üzerinde mevcut olduğundan emin olmanız gerekir. Daha fazla bilgi için bkz: [coğrafi çoğaltma için Güvenlik Yapılandırması](sql-database-geo-replication-security-config.md).

> [!NOTE]
> Yapılandırma ve sunucunun güvenlik duvarı kuralları, oturum açma bilgileri (ve onların izinlerini) bir olağanüstü durum kurtarma ayrıntıya sırasında test gerekir. Bu sunucu düzeyi nesneleri ve bunların yapılandırma sırasında kesinti kullanılamayabilir.
> 
> 

### <a name="setup-telemetry-alerts"></a>Kurulum telemetri uyarıları
Kurtarılan veritabanı ve farklı sunucu eşlemek için var olan uyarı kuralı ayarlarınızı güncelleştirildiğinden emin olmanız gerekir.

Veritabanı uyarı kuralları hakkında daha fazla bilgi için bkz: [uyarı bildirimleri alma](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) ve [izleme hizmeti durumu](../monitoring-and-diagnostics/insights-service-health.md).

### <a name="enable-auditing"></a>Denetimi etkinleştirme
Denetim, veritabanına erişmek için gerekli ise, veritabanı kurtarma işleminden sonra Denetim etkinleştirmeniz gerekir. Daha fazla bilgi için bkz: [veritabanı denetimi](sql-database-auditing.md).

## <a name="next-steps"></a>Sonraki adımlar
* Veritabanı Yedeklemeleri otomatik Azure SQL hakkında bilgi edinmek için bkz: [SQL veritabanı otomatik yedeklemeler](sql-database-automated-backups.md)
* İş sürekliliği tasarım ve kurtarma senaryoları hakkında bilgi edinmek için [sürekliliği senaryoları](sql-database-business-continuity.md)
* Kurtarma için otomatik yedeklemeler kullanma hakkında bilgi edinmek için bkz: [bir veritabanı hizmeti tarafından başlatılan yedeklerden geri yükleme](sql-database-recovery-using-backups.md)

