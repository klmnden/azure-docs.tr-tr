---
title: SQL veritabanı olağanüstü durum kurtarma | Microsoft Docs
description: Bir veritabanını bölgesel veri merkezi kesintisi veya Azure SQL veritabanı etkin coğrafi çoğaltma ve coğrafi geri yükleme özelliklerini hata kurtarma hakkında bilgi edinin.
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
ms.date: 01/25/2019
ms.openlocfilehash: 1e1bc92c684bf6ddbb7dc4ff0f882ad61ddeb27e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61075821"
---
# <a name="restore-an-azure-sql-database-or-failover-to-a-secondary"></a>Geri yükleme için ikincil bir Azure SQL veritabanı veya yük devretme

Azure SQL veritabanını kesintiden kurtarma için aşağıdaki özellikleri sunar:

- [Etkin coğrafi çoğaltma](sql-database-active-geo-replication.md)
- [Otomatik yük devretme grupları](sql-database-auto-failover-group.md)
- [Coğrafi geri yükleme](sql-database-recovery-using-backups.md#point-in-time-restore)
- [Bölgesel olarak yedekli veritabanları](sql-database-high-availability.md)

İş sürekliliği senaryoları ve bu senaryolar destekleyen özellikler hakkında bilgi edinmek için [iş sürekliliği](sql-database-business-continuity.md).

> [!NOTE]
> Bölgesel olarak yedekli Premium veya iş açısından kritik veritabanları veya havuzları kullanıyorsanız, kurtarma işlemini otomatik hale getirilmiştir ve bu yazıda geri kalanı için geçerli değildir.

> [!NOTE]
> Birincil ve ikincil veritabanları aynı hizmet katmanı için gereklidir. Aynı işlem boyutu (Dtu veya sanal çekirdekler) olarak birincil ile ikincil veritabanı oluşturulduğunu de önemle tavsiye edilir. Daha fazla bilgi için [yükseltme veya indirgeme birincil veritabanı olarak](sql-database-active-geo-replication.md#upgrading-or-downgrading-a-primary-database).

> [!NOTE]
> Bir veya birden çok yük devretme grubu yük devretme işlemlerini birden fazla veritabanını yönetmek için kullanın.
> Mevcut bir coğrafi çoğaltma ilişkisi için yük devretme grubuna eklerseniz, coğrafi-ikincil aynı hizmet katmanına ve işlem boyutu birincil olarak yapılandırıldığından emin olun. Daha fazla bilgi için [birden fazla veritabanının saydam ve Eşgüdümlü yük devretmeyi etkinleştirmek için otomatik yük devretme grupları kullanma](sql-database-auto-failover-group.md).

## <a name="prepare-for-the-event-of-an-outage"></a>Kesinti olayı için hazırlama

Yük devretme grupları veya coğrafi olarak yedekli yedeklemeler kullanarak başka bir veri bölgesine Kurtarma ile başarı için bir sunucu hazırlamanız gerekir. başka bir veri merkezinde kesinti yeni birincil sunucu için gereken belgelenen iyi tanımlanmış adımlara sahip olarak ortaya ve sorunsuz bir kurtarma için test. Hazırlama adımları şunlardır:

- SQL veritabanı sunucusu yeni birincil sunucu, başka bir bölgede belirleyin. Coğrafi geri yükleme için bu genellikle bir sunucu olan [eşleştirilmiş bölge](../best-practices-availability-paired-regions.md) veritabanınızın bulunduğu bölge için. Bu, coğrafi geri yükleme işlemleri sırasında ek trafik maliyetini ortadan kaldırır.
- Tanımlamak ve isteğe bağlı olarak tanımlayın, üzerinde kullanıcıların yeni birincil veritabanına erişmek için gerekli sunucu düzeyi IP güvenlik duvarı kuralları.
- Bağlantı dizeleri değiştirme veya DNS girişlerini değiştirerek gibi yeni birincil sunucu, kullanıcılara yönlendirmek için nasıl yükleyeceksiniz belirleyin.
- Belirleyin ve oluşturma isteğe bağlı olarak oturumların asıl veritabanında yeni birincil sunucuda mevcut olması gerekir ve bu oturum açma bilgileri varsa ana veritabanında uygun izinlere sahip olduğunuzdan emin olun. Daha fazla bilgi için [olağanüstü durum kurtarma sonrasında SQL veritabanı güvenliği](sql-database-geo-replication-security-config.md)
- Yeni birincil veritabanına eşlemek için güncelleştirilmesi gereken uyarı kuralları tanımlar.
- Belge geçerli birincil veritabanı üzerindeki denetim yapılandırması
- Gerçekleştirmek bir [olağanüstü durum kurtarma tatbikatı](sql-database-disaster-recovery-drills.md). Coğrafi geri yükleme için kesinti simülasyonu yapma silin veya kaynak veritabanı, uygulama bağlantı hatası neden yeniden adlandırın. Yük devretme grupları kullanarak kesinti simülasyonu yapma web uygulaması veya sanal makine bir veritabanı veya veritabanı yük devretme bağlı uygulama bağlantısı hataları neden devre dışı bırakabilirsiniz.

## <a name="when-to-initiate-recovery"></a>Kurtarma işlemini başlatmada ne zaman

Kurtarma işlemi, uygulamanın etkiler. SQL bağlantı dizesi veya DNS kullanarak yeniden yönlendirme değiştirilmesi gerekir ve kalıcı veri kaybına neden. Bu nedenle, yalnızca kesinti uygulamanızın kurtarma süresi hedefi daha uzun süre yararlanabilmenize olasılığı olduğunda yapılmalıdır. Uygulama üretime dağıtıldığında, uygulama durumunu düzenli aralıklarla izleme gerçekleştirme ve kurtarma garanti altına alınmadıkça onaylanacak aşağıdaki veri noktaları kullanmalısınız:

1. Veritabanına uygulama katmanından kalıcı bağlantı hatası.
2. Azure portal bölgede geniş bir etkiye sahip bir olay hakkında bir uyarı gösterir.

> [!NOTE]
> Yük devretme grupları kullanarak ve otomatik yük devretme seçtiyseniz, kurtarma işlemi otomatik ve şeffaf uygulamaya olur.

Kapalı kalma süresi ve olası iş sınırlandırılması için uygulama tolerans aralığınız bağlı olarak aşağıdaki kurtarma seçeneklerine göz önünde bulundurabilirsiniz.

Kullanım [alın, kurtarılabilir veritabanı](https://msdn.microsoft.com/library/dn800985.aspx) (*LastAvailableBackupDate*) en son coğrafi olarak çoğaltılmış bir geri yükleme noktası alınamıyor.

## <a name="wait-for-service-recovery"></a>Hizmet kurtarma için bekleme

Hızlı bir şekilde mümkün olduğunca ancak bağlı kök neden olarak hizmet kullanılabilirliği yapıyorduk geri yüklemek için Azure takımlar iş saatler veya günler sürebilir.  Uygulamanızı önemli kapalı kalma süresi toleransına sahipse kurtarma için yalnızca bekleyebilirsiniz. Bu durumda, sizin herhangi bir eylemi gerekli değildir. Geçerli hizmet durumunu görebilirsiniz bizim [Azure hizmet durumu Panosu](https://azure.microsoft.com/status/). Bölge kurtarma işleminden sonra uygulamanızın kullanılabilirlik geri kazanılır.

## <a name="fail-over-to-geo-replicated-secondary-server-in-the-failover-group"></a>Coğrafi çoğaltmalı ikincil sunucuya Yük devretme grubu yük devretme

Uygulamanızın kapalı kalma süresi iş yükümlülük neden olabilir, yük devretme grupları kullanarak. Ancak, kullanılabilirlik farklı bir bölgede kesinti durumunda hızlıca geri yüklemek uygulamayı etkinleştirir. Bir öğretici için bkz. [coğrafi olarak dağıtılmış bir veritabanı uygulama](sql-database-implement-geo-distributed-database.md).

Kullanılabilirlik veritabanlarının geri yüklemek için desteklenen yöntemlerden birini kullanarak ikincil sunucuya Yük devretme başlatma gerekir.

Coğrafi çoğaltmalı ikincil veritabanına yük devretmek için aşağıdaki kılavuzları kullanın:

- [Azure portalını kullanarak bir coğrafi olarak çoğaltılmış ikincil sunucuya Yük devretme](sql-database-geo-replication-portal.md)
- [PowerShell kullanarak ikincil sunucuya Yük devretme](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
- [Transact-SQL (T-SQL) kullanarak bir ikincil sunucuya Yük devretme](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#e-failover-to-a-geo-replication-secondary)

## <a name="recover-using-geo-restore"></a>Coğrafi geri yükleme kullanarak kurtarma

Uygulamanızın kapalı kalma süresi iş yükümlülük sağlamazsa kullanabileceğiniz [coğrafi geri yükleme](sql-database-recovery-using-backups.md) , uygulama veritabanlarını kurtarmak için bir yöntem olarak. Bunu, en son coğrafi olarak yedekli bir yedekten veritabanının bir kopyasını oluşturur.

## <a name="configure-your-database-after-recovery"></a>Kurtarma işleminden sonra veritabanını Yapılandır

Kesintiden kurtarma için coğrafi geri yükleme kullanıyorsanız, böylece normal uygulama işlevi sürdürülebilir yeni veritabanlarına bağlantı düzgün yapılandırıldığından emin olmanız gerekir. Kurtarılan veritabanı üretim hazır hale getirmek için görev listesi budur.

### <a name="update-connection-strings"></a>Bağlantı dizelerini güncelleştir

Kurtarılan veritabanı içinde farklı bir sunucuya bulunduğundan, bu sunucuya işaret edecek şekilde uygulamanızın bağlantı dizesini güncelleştirmeniz gerekiyor.

Uygun geliştirme dilini bağlantı dizeleri değiştirme hakkında daha fazla bilgi için bkz, [bağlantı kitaplığı](sql-database-libraries.md).

### <a name="configure-firewall-rules"></a>Güvenlik duvarı kuralları yapılandırma

Sunucusunda ve veritabanı yapılandırılmış güvenlik duvarı kuralları, birincil sunucu ve birincil veritabanının yapılandırılmış eşleştiğinden emin olmanız gerekir. Daha fazla bilgi için [nasıl yapılır: Güvenlik Duvarı ayarlarını (Azure SQL veritabanı) yapılandırma](sql-database-configure-firewall-settings.md).

### <a name="configure-logins-and-database-users"></a>Oturum açma bilgileri ve veritabanı kullanıcılarını yapılandırma

Uygulamanız tarafından kullanılan tüm oturum açma bilgileri, kurtarılan veritabanınızı barındıran sunucuda mevcut emin olmanız gerekir. Daha fazla bilgi için [coğrafi çoğaltma için Güvenlik Yapılandırması](sql-database-geo-replication-security-config.md).

> [!NOTE]
> Yapılandırma ve olağanüstü durum kurtarma tatbikatı sırasında sunucu güvenlik duvarı kuralları ve oturum açma bilgileri (ve izinlerini) test gerekir. Bu sunucu düzeyinde nesneleri ve yapılandırmalarını kesinti sırasında kullanılabilir olmayabilir.

### <a name="setup-telemetry-alerts"></a>Telemetri uyarıları ayarlama

Kurtarılan veritabanı ve farklı sunucu eşlemek için var olan uyarı kuralı ayarlarınızı güncelleştirilir emin olmanız gerekir.

Veritabanı uyarı kuralları hakkında daha fazla bilgi için bkz: [uyarı bildirimleri alma](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) ve [hizmet durumu izleme](../monitoring-and-diagnostics/insights-service-health.md).

### <a name="enable-auditing"></a>Denetimi etkinleştirme

Denetim, veritabanına erişmek için gerekli olursa, veritabanı kurtarma işleminden sonra Denetim etkinleştirmeniz gerekir. Daha fazla bilgi için [veritabanı denetimi](sql-database-auditing.md).

## <a name="next-steps"></a>Sonraki adımlar

- Veritabanı otomatik yedeklemeler Azure SQL hakkında bilgi edinmek için bkz: [SQL veritabanı otomatik yedekleme](sql-database-automated-backups.md)
- İş sürekliliği tasarım ve kurtarma senaryoları hakkında bilgi edinmek için [sürekliliği senaryoları](sql-database-business-continuity.md)
- Otomatik yedekleme, kurtarma için kullanma hakkında bilgi edinmek için [hizmet tarafından başlatılan yedeklemelerden veritabanını geri yükleme](sql-database-recovery-using-backups.md)
