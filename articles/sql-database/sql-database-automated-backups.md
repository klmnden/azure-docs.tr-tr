---
title: Otomatik, coğrafi olarak yedekli Azure SQL veritabanı yedeklemeleri | Microsoft Docs
description: SQL veritabanı otomatik olarak birkaç dakikada bir yerel veritabanı yedeği oluşturur ve coğrafi yedeklilik için Azure okuma erişimli coğrafi olarak yedekli depolama kullanır.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: article
ms.workload: Active
ms.date: 04/04/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: ab1793621950fd57d3f0be545772d85b32f5d7b8
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="learn-about-automatic-sql-database-backups"></a>Otomatik SQL veritabanını yedekleme hakkında bilgi edinin

SQL veritabanı otomatik olarak veritabanı yedeklerini oluşturur ve coğrafi yedeklilik sağlamak için Azure okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) kullanır. Bu yedeklemeler otomatik olarak ve ek ücret ödemeden oluşturulur. Bunları durum hale getirmek için herhangi bir şey yapmanız gerekmez. Verilerinizin yanlışlıkla Bozulması veya silme korumak için veritabanı yedeklemeleri tüm iş sürekliliği ve olağanüstü durum kurtarma stratejisi, önemli bir parçasıdır. Kendi depolama kapsayıcısı yedeklemeleri tutmak istiyorsanız, uzun vadeli yedekleme bekletme ilkesi yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Uzun süreli saklama](sql-database-long-term-retention.md).

## <a name="what-is-a-sql-database-backup"></a>Bir SQL veritabanı yedeği nedir?

SQL veritabanı oluşturmak için SQL Server teknolojisini kullanan [tam](https://msdn.microsoft.com/library/ms186289.aspx), [fark](https://msdn.microsoft.com/library/ms175526.aspx), ve [işlem günlüğü](https://msdn.microsoft.com/library/ms191429.aspx) zaman içinde nokta amacıyla yedeklerini geri (PITR). İşlem günlüğü yedeklemeleri her 5-10 dakika performans düzeyi ve veritabanı etkinliği miktarı göre sıklık genellikle gerçekleşir. İşlem günlüğü yedeklemeleri, tam ve değişim yedeklemeleri ile bir veritabanı bir belirli noktası zaman içinde veritabanını barındıran aynı sunucuya geri yüklemek izin verin. Bir veritabanını geri yüklediğinizde, hizmetin hangi tam, fark ve işlem günlüğü yedeklemeleri geri yüklenmesi gerekiyor rakamlar.


Bu yedeklemeler için kullanabilirsiniz:

* Bir veritabanını bir nokta zaman için saklama dönemi içinde geri yükleyin. Bu işlem, özgün veritabanı ile aynı sunucuda yeni bir veritabanı oluşturur.
* Silinen bir veritabanını silindi veya tüm süresini saklama dönemi içinde geri yükleyin. Silinen bir veritabanını yalnızca özgün veritabanının oluşturulduğu aynı sunucudan geri yüklenebilir.
* Bir veritabanını geri yüklemek için başka bir coğrafi bölge. Bu, sunucu ve veritabanı erişemediğinde coğrafi olağanüstü durumdan kurtarmanıza olanak tanır. Dünyanın varolan tüm Server'da yeni bir veritabanı oluşturur. 
* Bir veritabanı, veritabanı ile uzun vadeli bir bekletme ilkesi yapılandırdıysanız belirli uzun vadeli yedekten geri yükleyin. Bu veritabanının uyumluluk isteği karşılamak için veya uygulama eski bir sürümünü çalıştırdığı için eski bir sürümüne geri yüklemenize olanak sağlar. Bkz: [uzun vadeli bekletme](sql-database-long-term-retention.md).
* Bir geri yükleme gerçekleştirmek için bkz: [veritabanını yedeklerden geri](sql-database-recovery-using-backups.md).

> [!NOTE]
> Azure depolama alanında terimi *çoğaltma* dosyaları bir konumdan diğerine kopyalanmasına başvuruyor. SQL'in *veritabanı çoğaltması* birden fazla ikincil veritabanları birincil veritabanı ile eşitlenmiş tutmak için başvuruyor. 
> 

## <a name="how-often-do-backups-happen"></a>Yedeklemelerin ne sıklıkta meydana?
Tam veritabanı yedeklemeleri, haftalık, artımlı veritabanı yedeklemeleri genellikle her birkaç saat ve işlem günlüğü yedeklemeleri genellikle 5-10 dakikada bir gerçekleşir durum gerçekleşir. Hemen bir veritabanı oluşturulduktan sonra ilk tam yedeklemede zamanlanır. Genellikle 30 dakika içinde tamamlanır ancak veritabanı önemli boyutunu olduğunda uzun sürebilir. Örneğin, ilk yedekleme geri yüklenen veritabanı veya veritabanı kopyası üzerinde daha uzun sürebilir. İlk tam yedeklemeden sonra başka yedeklemelerinin de tümünü otomatik olarak zamanlanmış ve arka planda sessizce yönetilen. Genel sistem iş yükünü dengeleyen gibi tüm veritabanı yedeklemeleri tam zamanlamasını SQL veritabanı hizmeti tarafından belirlenir. 

Yedekleme depolama coğrafi çoğaltma Azure Storage çoğaltma zamanlamaya göre gerçekleşir.

## <a name="how-long-do-you-keep-my-backups"></a>Ne kadar süreyle yedeklerim tutarım?
Her SQL veritabanını yedekleme temel alan bir bekletme dönemi içeren [hizmet katmanı](sql-database-service-tiers.md) veritabanı. Bir veritabanında saklama süresi:


* Temel hizmet katmanı 7 gündür.
* Standart hizmet katmanında 35 gün olur.
* Premium Hizmet katmanını 35 gün olur.
* Genel amaçlı katmanı maksimum 35 gün ile yapılandırılabilir (varsayılan olarak 7 gün) *
* İş kritik Katmanı (Önizleme) maksimum 35 gün ile yapılandırılabilir (varsayılan olarak 7 gün) *

\* Önizleme sırasında yedeklemelerin saklama dönemi yapılandırılabilir değildir ve 7 gün için sabit.

Daha kısa bekletme olan bir veritabanına uzun yedeklemeleri bekletme veritabanıyla dönüştürürseniz, hedef katmanı saklama süresinden daha eski tüm var olan yedekleri artık kullanılamaz.

Daha uzun bir süre olan bir veritabanına daha kısa bir bekletme dönemi veritabanıyla yükseltirseniz, SQL veritabanı var olan yedekleri daha uzun bekletme süresine ulaşılana kadar tutar. 

Bir veritabanı silerseniz, SQL veritabanı yedeklemeleri için çevrimiçi bir veritabanı misiniz aynı şekilde korur. Örneğin, bir yedi günlük tutma süresine sahip bir temel veritabanı silme varsayalım. Dört gün önce yapılmışsa bir yedekleme için üç gün daha kaydedilir.

> [!IMPORTANT]
> SQL veritabanlarını barındıran Azure SQL server silerseniz, sunucuya ait tüm veritabanlarının da silinir ve kurtarılamaz. Silinen bir sunucuya geri yükleyemezsiniz.
> 

## <a name="how-to-extend-the-backup-retention-period"></a>Yedekleme bekletme süresini uzatmayı nasıl?

Uygulamanızın yedekleri maksimum PITR yedekleme saklama süresinden daha uzun süre boyunca kullanılabilir olduğunu gerektiriyorsa, uzun vadeli yedekleme bekletme ilkesi tekil veritabanları (LTR İlkesi) yapılandırabilirsiniz. Bu yerleşik BT saklama süresi en fazla 10 yıl maksimum 35 gün genişletmenizi sağlar. Daha fazla bilgi için bkz. [Uzun süreli saklama](sql-database-long-term-retention.md).

Azure portalı veya API kullanarak bir veritabanına LTR İlkesi ekledikten sonra haftalık tam veritabanı yedeklemeleri uzun vadeli bekletme (LTR depolama) ayrı bir RA-GRS depolama kapsayıcısını otomatik olarak kopyalanır. Veritabanınız ile TDE şifrelenmişse yedeklemeleri bekleyen otomatik olarak şifrelenir. SQL veritabanı otomatik olarak kendi zaman damgası ve LTR İlkesi göre süresi dolan Yedeklerinizin silinmesine neden olur. İlke ayarladıktan sonra yedekleme zamanlaması yönetmek ya da eski dosyaları temizleme hakkında endişelenmeniz gerekmez. Görüntüleme, geri yüklemek veya bu yedeklemeler silmek için Azure portal veya PowerShell kullanabilirsiniz.

## <a name="are-backups-encrypted"></a>Yedeklemeleri şifrelenir?

TDE bir Azure SQL veritabanı için etkinleştirildiğinde, yedeklemeler de şifrelenir. Tüm yeni Azure SQL veritabanları, varsayılan olarak etkin TDE ile yapılandırılır. TDE hakkında daha fazla bilgi için bkz: [saydam veri şifrelemesi ile Azure SQL veritabanı](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

## <a name="are-the-automatic-backups-compliant-with-gdpr"></a>Otomatik yedekleme GDPR ile uyumlu olan?
Yedekleme genel veri koruma düzenleme (GDPR) tabi olan kişisel veriler içeriyorsa, verileri yetkisiz erişimden korumak için Gelişmiş güvenlik önlemleri uygulamak için gereklidir. İle GDPR uymak için yedeklemelere erişmek zorunda kalmadan verileri sahiplerinin verileri isteklerini yönetmek için bir yöntem gerekir.  Kısa vadeli yedeklemeleri için yedekleme kısaltmak için bir çözüm olabilir zaman olan penceresi altında 30 gün için izin verilen veri erişim isteklerini tamamlamak için.  Uzun vadeli yedeklemeleri gerekli olduğunda, yedeklemelerin yalnızca "pseudonymized" verileri depolamak için önerilir. Bir kişi hakkındaki verileri silindi veya güncelleştirilmesi gerekiyorsa, örneğin, bu silme veya var olan yedekleri güncelleştirme gerektirmez. GDPR en iyi uygulamalar hakkında daha fazla bilgi bulabilirsiniz [GDPR uyumluluk için veri yönetimi](https://info.microsoft.com/DataGovernanceforGDPRCompliancePrinciplesProcessesandPractices-Registration.html).

## <a name="next-steps"></a>Sonraki adımlar

- Verilerinizin yanlışlıkla Bozulması veya silme korumak için veritabanı yedeklemeleri tüm iş sürekliliği ve olağanüstü durum kurtarma stratejisi, önemli bir parçasıdır. Diğer Azure SQL Database iş sürekliliği çözümleri hakkında bilgi edinmek için [iş sürekliliğine genel bakış](sql-database-business-continuity.md).
- Azure portalını kullanarak zaman içinde bir noktaya geri yüklemenizi bkz [veritabanı Azure portalını kullanarak zaman içinde bir noktaya geri](sql-database-recovery-using-backups.md).
- PowerShell kullanarak zaman içinde bir noktaya geri yüklemenizi bkz [veritabanı PowerShell kullanarak zaman içinde bir noktaya geri](scripts/sql-database-restore-database-powershell.md).
- Otomatik Azure portalını kullanarak bir Azure kurtarma Hizmetleri kasasına yedekleme yapılandırmak, yönetmek ve uzun vadeli bekletme geri yüklemek için bkz: [Azure portalını kullanarak uzun vadeli yedekleme bekletme yönetmek](sql-database-long-term-backup-retention-configure.md).
- Otomatik PowerShell kullanarak bir Azure kurtarma Hizmetleri kasasına yedekleme yapılandırmak, yönetmek ve uzun vadeli bekletme geri yüklemek için bkz: [uzun vadeli yedekleme bekletme PowerShell kullanarak yönetme](sql-database-long-term-backup-retention-configure.md).
