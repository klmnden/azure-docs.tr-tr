---
title: "SQL veritabanı olağanüstü durum kurtarma | Microsoft Docs"
description: "Bölgesel veri merkezi kesintisinden veya Azure SQL veritabanı etkin coğrafi çoğaltma ve coğrafi geri yükleme özelliklerini hatası bir veritabanını kurtarmak öğrenin."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: 4800960e-3f9d-40ce-9e55-fb7f2784c067
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: On Demand
ms.date: 04/14/2017
ms.author: sashan
ms.openlocfilehash: cbd54a2a309874c81d8384d789bebe4f94c97adf
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="restore-an-azure-sql-database-or-failover-to-a-secondary"></a>Bir Azure SQL Database veya yük devretme için ikincil bir geri yükleme
Azure SQL veritabanı bir kesintisi kurtarmak için aşağıdaki özellikleri sunar:

* [Etkin coğrafi çoğaltma](sql-database-geo-replication-overview.md)
* [Coğrafi geri yükleme](sql-database-recovery-using-backups.md#point-in-time-restore)

İş sürekliliği senaryoları ve bu senaryolar destekleyen özellikler hakkında bilgi edinmek için bkz: [iş sürekliliği](sql-database-business-continuity.md).

### <a name="prepare-for-the-event-of-an-outage"></a>Bir kesinti olayı için hazırlama
Aktif coğrafi çoğaltma veya coğrafi olarak yedekli yedeklemeler kullanarak başka bir veri bölgesi için Kurtarma ile başarı için bir sunucu hazırlamak gereken başka bir veri merkezinde yeni birincil sunucu olarak kesinti artırılması gereken ortaya yanı sıra iyi tanımlanmış adımlara sahip belgelenen ve kesintisiz bir kurtarma emin olmak için test edilmiştir. Hazırlık adımları içerir:

* Yeni birincil sunucu olarak başka bir bölgede mantıksal sunucu tanımlayın. Etkin coğrafi çoğaltma ile bu en az bir ve belki de her bir ikincil sunucu olacaktır. Coğrafi geri yükleme için bu genellikle bir sunucu olacaktır [eşleştirilmiş bölge](../best-practices-availability-paired-regions.md) veritabanınızın bulunduğu bölge için.
* Belirleyin ve isteğe bağlı olarak, üzerinde kullanıcıların yeni birincil veritabanına erişmek için gerekli sunucu düzeyinde güvenlik duvarı kural tanımlarsınız.
* Bağlantı dizeleri değiştirme veya DNS girdilerini değiştirerek gibi yeni birincil sunucu, kullanıcılara yönlendirmek için nasıl adımıdır belirler.
* Belirleyin ve isteğe bağlı olarak oluşturmak, yeni birincil sunucu ana veritabanındaki mevcut olması ve bu oturumlar varsa ana veritabanında uygun izinlere sahip olduğundan emin olun oturum açma bilgileri. Daha fazla bilgi için bkz: [SQL veritabanı güvenlik olağanüstü durum kurtarma işleminden sonra](sql-database-geo-replication-security-config.md)
* Yeni birincil veritabanına eşlemek için güncelleştirilmesi gerekir uyarı kuralları tanımlar.
* Belge geçerli birincil veritabanı üzerinde denetim yapılandırma
* Gerçekleştirmek bir [olağanüstü durum kurtarma ayrıntıya](sql-database-disaster-recovery-drills.md). Coğrafi geri yükleme için bir kesinti benzetimini yapmak için silebilir veya uygulama bağlantı hatası neden kaynak veritabanını yeniden adlandırın. Aktif coğrafi çoğaltma için bir kesinti benzetimini yapmak için web uygulaması veya sanal makine veritabanına veya yük devretme veritabanı bağlı uygulama bağlantısı hataları neden devre dışı bırakabilirsiniz.

## <a name="when-to-initiate-recovery"></a>Kurtarma zamanı
Kurtarma işlemi uygulama etkiler. SQL bağlantı dizesi veya DNS kullanarak yeniden yönlendirme değiştirilmesi gerektirir ve kalıcı veri kaybına neden olabilir. Bu nedenle, yalnızca kesinti uygulamanızın kurtarma süresi hedefi daha uzun son sırada yapılmalıdır. Üretim için uygulama dağıtıldığında normal uygulama sistem durumunu izleme gerçekleştirin ve kurtarma garanti olduğunu onaylanacak aşağıdaki veri noktaları kullanmanız gerekir:

1. Veritabanına kalıcı bağlantı hatası uygulama katmanından.
2. Azure portal geniş etkiyle bölgede bir olay hakkında bir uyarı gösterir.
3. Azure SQL veritabanı sunucusu, düşürülmüş olarak işaretlenir.

Kapalı kalma süresi ve olası iş yükümlülük, uygulama toleransını bağlı olarak aşağıdaki kurtarma seçeneklerine düşünebilirsiniz.

Kullanım [alma kurtarılabilir veritabanı](https://msdn.microsoft.com/library/dn800985.aspx) (*LastAvailableBackupDate*) en son coğrafi olarak çoğaltılmış geri yükleme noktası almak için.

## <a name="wait-for-service-recovery"></a>Servis kurtarma için bekleyin
Titizlikle hızla mümkün olduğunca ancak kök bağlı olarak neden olarak hizmet kullanılabilirliği geri yüklemek için Azure ekiplerin çalışma saat veya gün sürebilir.  Uygulamanızın önemli kapalı kalma süresi dayanabileceği, kurtarma için yalnızca bekleyebilirsiniz. Bu durumda, herhangi bir eyleminiz gereklidir. Geçerli Hizmet durumu görebilirsiniz bizim [Azure hizmet sağlığı panosunu](https://azure.microsoft.com/status/). Bölge kurtarma işleminden sonra uygulamanızın kullanılabilirlik geri yüklenir.

## <a name="fail-over-to-geo-replicated-secondary-database"></a>Coğrafi olarak çoğaltılmış ikincil veritabanına yük devri
Uygulamanızın kapalı kalma iş yükümlülük neden olabilir, uygulamanızda coğrafi olarak çoğaltılmış veritabanları kullanıyor olması gerekir. Bir kesinti durumunda farklı bir bölgede kullanılabilirlik hızlı bir şekilde geri uygulama olanak sağlar. Bilgi edinmek için nasıl [coğrafi çoğaltma yapılandırma](sql-database-geo-replication-portal.md).

Veritabanlarının kullanılabilirliğini geri yüklemek için desteklenen yöntemlerden birini kullanarak coğrafi olarak çoğaltılmış ikincil yük devretmeyi başlatmak gerekir.

Coğrafi olarak çoğaltılmış bir ikincil veritabanı devretmek için aşağıdaki kılavuzları birini kullanın:

* [Azure Portal kullanarak coğrafi olarak çoğaltılmış ikincil yük devri](sql-database-geo-replication-portal.md)
* [Coğrafi olarak çoğaltılmış ikincil bir yük devri PowerShell'i kullanma](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
* [T-SQL kullanarak coğrafi olarak çoğaltılmış ikincil yük devri](/sql/t-sql/statements/alter-database-azure-sql-database)

## <a name="recover-using-geo-restore"></a>Coğrafi geri yükleme kullanarak kurtarma
Uygulamanızın kapalı kalma süresi içinde iş yükümlülük sağlamazsa kullanabileceğiniz [coğrafi geri yükleme](sql-database-recovery-using-backups.md) uygulama veritabanlarını kurtarmak için bir yöntem olarak. En son coğrafi olarak yedekli yedeklemesinden veritabanının bir kopyasını oluşturur.

## <a name="configure-your-database-after-recovery"></a>Kurtarma işleminden sonra veritabanını Yapılandır
Bir kesintisinden kurtarma için coğrafi çoğaltma yük devretme veya coğrafi geri yükleme kullanıyorsanız, normal uygulama işlevi devam ettirilebilir böylece yeni veritabanları için bağlantı düzgün yapılandırıldığından emin olmanız gerekir. Bu, kurtarılan veritabanının üretim hazır hale getirmek için görevlerin bir listesi verilmektedir.

### <a name="update-connection-strings"></a>Bağlantı dizelerini güncelleştirmek
Kurtarılan veritabanı içinde farklı bir sunucuya yer alacağı olduğundan, bu sunucuya işaret edecek şekilde uygulamanızın bağlantı dizesini güncellemeniz gerekir.

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

