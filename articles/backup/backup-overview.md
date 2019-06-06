---
title: Azure Backup nedir?
description: Azure Backup hizmeti ve iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize nasıl katkı sağlayan bir genel bakış sağlar.
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: overview
ms.date: 04/24/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 9e926ca2625f98522652ae7e7d245ecf2ed576c4
ms.sourcegitcommit: 6932af4f4222786476fdf62e1e0bf09295d723a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66688718"
---
# <a name="what-is-azure-backup"></a>Azure Backup nedir?

Azure Backup hizmeti, Microsoft Azure bulutuna verileri yedekler. Şirket içi makinelerin ve iş yükleri ve Azure sanal makineleri (VM) yedekleme.


## <a name="why-use-azure-backup"></a>Azure Backup'ı neden kullanmalısınız?

Azure Backup şu önemli avantajlara sahiptir:

- **Şirket içi yedekleme boşaltma**: Azure Backup, şirket içi kaynaklarınızı buluta yedekleme için basit bir çözüm sunar. Alma kısa ve uzun vadeli yedekleme karmaşık dağıtmaya gerek kalmadan şirket yedekleme çözümleri.
- **Azure Iaas Vm'leri yedekleme**: Azure Backup, özgün verilerin yanlışlıkla edilmesine karşı koruma sağlamak için bağımsız ve ayrı yedeklemeler sağlar. Yedeklemeler ile yerleşik kurtarma noktaları yönetilen bir kurtarma Hizmetleri kasasında depolanır. Yapılandırma ve ölçeklenebilirlik basit, yedeklemeleri en iyi duruma getirilir ve gerektiğinde kolayca geri yükleyebilirsiniz.
- **Ölçeği kolayca** -herhangi bir bakım ve izleme maliyetleri ile yüksek kullanılabilirlik sunmak için Azure Backup kullanan temel alınan gücü ve sınırsız ölçek Azure bulutunun.
- **Sınırsız veri aktarımı alma**: Azure yedekleme, gelen veya giden veri aktarımı ve aktarılan veriler için ücret miktarını sınırlamaz.
    - Giden veriler, geri yükleme işlemi sırasında bir Kurtarma Hizmetleri kasasından aktarılan verileri tanımlar.
    - Büyük miktarda veriyi içeri aktarmak için Azure içeri/dışarı aktarma hizmetini kullanarak çevrimdışı ilk yedekleme yapıyorsanız, gelen verilerle ilişkili bir maliyeti yoktur.  [Daha fazla bilgi edinin](backup-azure-backup-import-export.md).
- **Verileri güvenli tutmak**: Azure Backup, veri aktarım ve bekleme sırasında güvenliğini sağlamak için çözümler sağlar.
- **Uygulamayla tutarlı yedekler almak**: Bir uygulamayla tutarlı yedekleme, kurtarma noktası yedek kopyayı geri yüklemek için tüm gerekli verileri içeren anlamına gelir. Azure Backup, verileri geri yüklerken ek düzeltmelere gerek kalmaması için uygulamayla tutarlı yedeklemeler yapılmasını sağlar. Uygulamayla tutarlı verilerin geri yüklenmesi, geri yükleme süresini azaltarak hizmetlerinizin kısa süre içinde çalışır hale gelmesini sağlar.
- **Kısa ve uzun süreli saklanması**: Kısa vadede ve uzun veri saklama için kurtarma Hizmetleri kasalarını kullanabilirsiniz. Azure, bir Kurtarma Hizmetleri kasasında verileri saklama süresini kısıtlamaz. Dilediğiniz sürece için tutabilirsiniz. Azure Backup, korunan her örnek için 9999 kurtarma noktası sınırına sahiptir. 
- **Otomatik depolama yönetimi** - Karma ortamlar genelde heterojen depolamaya (bazıları şirket içi, bazıları ise bulutta olan) ihtiyaç duyar. Azure Backup çözümünde, şirket içi depolama cihazlarının kullanımıyla ilişkili maliyetler yoktur. Azure yedekleme, otomatik olarak ayırır ve yedekleme depolama yönetir ve böylece yalnızca kullandığınız depolama alanı için ödeme yaparsınız-,-kullandıkça modeli kullanır. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/backup) fiyatlandırma hakkında daha fazla.
- **Birden çok depolama seçeneği** -Azure Backup iki tür depolama/verilerinizi yüksek oranda kullanılabilir tutmak için çoğaltma sunar.
    - [Yerel olarak yedekli depolama (LRS)](../storage/common/storage-redundancy-lrs.md) verilerinizi bir veri merkezinde bir depolama ölçek birimi (verilerinizin üç kopyasını oluşturur) üç kez çoğaltır. Verilerin tüm kopyaları aynı bölgenin içinde yer alır. LRS, verilerinizi yerel donanım hatalarına karşı korumak için düşük maliyetli bir seçenektir.
    - [Coğrafi olarak yedekli depolama (GRS)](../storage/common/storage-redundancy-grs.md) varsayılandır ve önerilen çoğaltma seçeneğidir. GRS, verilerinizi ikincil bir bölgeye (kaynak verilerin birincil konumundan yüzlerce kilometre uzakta) kopyalar. GRS’nin maliyeti LRS’den yüksektir ancak bölgesel kesintiler olsa bile daha yüksek veri dayanıklılık düzeyi sunar.


## <a name="whats-the-difference-between-azure-backup-and-azure-site-recovery"></a>Azure Backup ve Azure Site Recovery arasındaki fark nedir?

Azure Backup ve Azure Site Recovery Hizmetleri İşletmenizde bir iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. BCDR iki geniş amaçları oluşur:

- Kesintiler meydana geldiğinde iş verilerinizi güvenli ve kurtarılabilir tutun.
- Uygulamalarınızı ve iş yüklerini çalıştırmaya planlanmış ve Planlanmamış kapalı kalma süreleri tutun.

Her iki hizmet tamamlayıcı ancak farklı bir işlevsellik sağlar.

- **Azure Site Recovery**: Site Recovery, şirket içi makinelerin ve Azure Vm'leri için olağanüstü durum kurtarma çözümü sunar. Makineleri ikincil bir birincil konumdan çoğaltma. Olağanüstü durumla karşılaştığınızda, makineleri ikincil konuma yük devretme ve makinelere oradan erişebilirsiniz. Her şey çalışır duruma normalde yeniden üstündeyse başarısız birincil site kurtarılır arka makineleri.
- **Azure yedekleme**: Azure Backup hizmeti, şirket içi makinelerin ve Azure Vm'leri verileri yedekler. Veri yedeklenebilir ve dosyaları, klasörleri, makine sistem durumu yedeklemesi ve uygulama durumunu algılayan yedekleme de dahil olmak üzere ayrıntılı bir düzeyde, kurtarılır. Azure yedekleme, Site Recovery değerinden daha ayrıntılı bir düzeyde verileri işler. Örneğin, dizüstü bilgisayardaki sunu bozulduysa, sunuyu geri yüklemek için Azure Backup kullanın. Güvenli ve erişilebilir bir VM yapılandırma ve verileri tutmak istiyorsanız, Site RECOVERY'yi kullanabilirsiniz.  

BCDR gereksinimlerinizi şekil yardımcı olması için tablo noktalarını kullanın.

**Hedefi** | **Ayrıntılar** | **Karşılaştırma**
--- | --- | ---
**Veri yedekleme/saklama** | Yedekleme verileri korunur ve gün, ay veya yıl bile uyumluluk açısından bakıldığında gerekirse saklanan. | Azure yedekleme gibi yedekleme çözümleri, ince yedeklemek istediğiniz veri çekmek izin ve ince yedekleme ve bekletme ilkeleri ayarlayın.<br/><br/> Site Recovery, aynı ince ayar izin vermez.
**Kurtarma noktası hedefi (RPO)** | Kurtarma işleminin gerekli olduğu durumlarda kabul edilebilir veri kaybı miktarı. | Daha fazla değişken RPO yedeklemelere sahip.<br/><br/> Veritabanı Yedeklemeleri 15 dakika kadar düşük RPO'lar VM yedeklemeleri genellikle bir gün, bir RPO bulunur.<br/><br/> Site Recovery, düşük RPO'ya sağlar, bu çoğaltma sürekli veya sık, olduğundan kaynak ve çoğaltma kopyalama arasındaki delta küçük olmasını sağlayın.
**Kurtarma süresi hedefi (RTO)** |Bir geri yükleme veya kurtarma işlemini tamamlamak için geçen süre. | Daha büyük RPO nedeniyle, bir yedekleme çözümünün işlemesi gereken veri miktarı genellikle çok daha yüksektir; bu da daha uzun RTO'lara yol açar. Örneğin, bandın şirket dışı bir konumdan taşınması için harcanan süreye bağlı olarak, bantlardan veri geri yükleme işlemi birkaç gün sürebilir.

## <a name="what-backup-scenarios-are-supported"></a>Hangi yedekleme senaryoları desteklenir mi?

Azure yedekleme, hem şirket içi makinelerin ve Azure sanal makinelerini yedekleyebilirsiniz.

**Makine** | **Senaryoyu oluşturan yedekleme**
--- | ---
**Şirket içi yedekleme** |  (1) Azure Backup Microsoft Azure kurtarma Hizmetleri (MARS) aracısı şirket içi tek tek dosya ve sistem durumu yedekleme için Windows makineleri çalıştırın. <br/><br/>2) bir yedekleme sunucusuna (System Center Data Protection Manager (DPM) veya Microsoft Azure Backup sunucusu (MABS)) şirket içi makineleri yedekleme ve azure'da bir Azure yedekleme kurtarma Hizmetleri kasasına yedeklemek için backup sunucusu yapılandırın.
**Azure Vm'leri** | (1) tek tek Azure Vm'leri için yedeklemeyi etkinleştirin. Yedeklemeyi etkinleştirdiğinizde, Azure Backup uzantısı VM'de çalışan Azure VM aracısı yükler. Aracı, VM'nin tamamını yedekler.<br/><br/> (2) bir Azure sanal makinesinde MARS Aracısı çalıştırın. Bu VM üzerinde tek tek dosya ve klasörleri yedeklemek istiyorsanız kullanışlıdır.<br/><br/> 3) DPM sunucusuna veya MABS Azure'da çalışan bir Azure VM'yi yedekleme. Ardından DPM sunucusu/MABS kullanarak Azure Backup vault'a yedekleyin.


## <a name="why-use-a-backup-server"></a>Bir yedekleme sunucusu neden kullanmalısınız?
Makineleri ve uygulamaları MABS/DPM depolama alanı için yedekleme ve ardından DPM/MABS depolama Kasası'na yedekleme avantajları şunlardır:

- Azure'a yedekleme MABS/DPM, SQL Server, Exchange ve SharePoint gibi sık kullanılan uygulamaları için iyileştirilmiş uygulama durumunu algılayan yedeklemelerini sağlar ek dosya/klasör/birim yedeklemelerinin ve makine durumu yedeklemeleri (çıplak, sistem durumu).
- Şirket içi makineler için MARS aracısının yedeklemek istediğiniz her makineye yüklemeniz gerekmez. DPM/MABS koruma aracısını her makinede çalışır ve MARS aracısının MABS/DPM'de yalnızca çalıştırır.
- Daha fazla esneklik ve yedeklemeleri için ayrıntılı zamanlama seçenekleri var.
- Tek bir konsolda koruma gruplarına toplamak birden çok makineleri için yedeklemeleri yönetebilir. Bu uygulamalar birden çok makinede yük katmanlı halde bulunan ve birlikte yedeklemek istediğiniz özellikle yararlı olur.

Daha fazla bilgi edinin [nasıl yedekleme works](backup-architecture.md#architecture-back-up-to-dpmmabs) bir yedekleme sunucusu kullanırken ve [destek gereksinimlerini](backup-support-matrix-mabs-dpm.md) yedekleme sunucuları için.

## <a name="what-can-i-back-up"></a>Neleri yedekleyebilirim?

**Makine** | **Yedekleme yöntemi** | **Yedekleme**
--- | --- | ---
**Şirket içi Windows Vm'leri** | MARS Aracısı'nı çalıştırın | Dosyaları, klasörleri, sistem durumu yedekleme.<br/><br/> Linux makineleri desteklenmiyor.
**Şirket içi makineler** | DPM/MABS yedekleme | Yedekleme tarafından korunan herhangi bir şey [DPM](backup-support-matrix-mabs-dpm.md#supported-backups-to-dpm) veya [MABS](backup-support-matrix-mabs-dpm.md#supported-backups-to-mabs)dosya/klasör/paylaşımları/birimler ve uygulamaya özgü verileri dahil olmak üzere.
**Azure Vm'leri** | Çalıştırma Azure VM Aracısı yedekleme uzantısı | Tüm VM'yi yedekleme
**Azure Vm'leri** | MARS Aracısı'nı çalıştırın | Dosyaları, klasörleri, sistem durumu yedekleme.<br/><br/> Linux makineleri desteklenmiyor.
**Azure Vm'leri** | Azure'da çalışan MABS/DPM yedekleme | Yedekleme tarafından korunan herhangi bir şey [MABS](backup-support-matrix-mabs-dpm.md#supported-backups-to-mabs) veya [DPM](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix?view=sc-dpm-1807) dosya/klasör/paylaşımları/birimler ve uygulamaya özgü veriler dahil olmak üzere.

## <a name="what-backup-agents-do-i-need"></a>Hangi yedekleme aracıları ihtiyacım var?

**Senaryo** | **Aracı**
--- | ---
**Azure VM'lerini yedekleme** | Gerekli aracı yok. İlk Azure VM yedeklemesi çalıştırdığınızda Azure VM yedekleme için azure VM uzantısı yüklenir.<br/><br/> Windows ve Linux desteği için destek.
**Şirket içi Windows makinelerini yedekleme** | İndirin, yükleyin ve MARS aracısının doğrudan makinede çalıştırın.
**MARS Aracısı ile Azure Vm'lerini yedekleme** | İndirin, yükleyin ve MARS aracısının doğrudan makinede çalıştırın. MARS aracısının yedekleme uzantıyla birlikte çalıştırabilirsiniz.
**DPM/MABS için şirket içi makinelerin ve Azure sanal makinelerini yedekleme** | DPM veya MABS koruma aracısını korumak istediğiniz makinelerde çalıştırır. MARS aracısının DPM sunucusu/Azure'a yedeklemek için MABS çalışır.

## <a name="which-backup-agent-should-i-use"></a>Hangi yedekleme aracısı kullanmam gerekir?

**Backup** | **Çözüm** | **Sınırlama**
--- | --- | ---
**Tüm bir Azure VM'yi yedekleme istiyorum** | VM için yedeklemeyi etkinleştirin. Backup uzantısı Windows veya Linux Azure VM üzerinde otomatik olarak yapılandırılır. | Tüm VM yedeklenir <br/><br/> Windows VM'ler için uygulamayla tutarlı yedeklemedir. Linux için yedekleme dosyası tutarlıdır. Linux VM'ler için uygulama durumunu algılayan ihtiyacınız varsa, bu özel betiklerle yapılandırmanız gerekir.
**Azure VM'de belirli dosyaları/klasörleri yedeklemek üzere istiyorum** | MARS Aracısı VM üzerinde dağıtın.
**Şirket içi Windows makineleri doğrudan istiyorum** | MARS Aracısı makinesine yükleyin. | Dosyaları, klasörleri ve sistem durumu yedekleme Azure'a yedekleyebilirsiniz. Uygulama durumunu algılayan yedeklemelerini değildir.
**Doğrudan şirket içi Linux makinelerini yedekleme istiyorum** | DPM veya MABS Azure'a yedeklemek için dağıtmanız gerekebilir. | Linux ana yedeklemesi desteklenmiyor; yalnızca Hyper-V veya VMWare üzerinde barındırılan Linux Konuk makine yedekleyebilirsiniz.
**Şirket içinde çalışan uygulamaların yedeğini istiyorum** | Uygulama durumunu algılayan yedekleme için DPM veya MABS makineler korunmalıdır.
**Ayrıntılı ve esnek yedekleme ve kurtarma ayarlarını Azure Vm'leri için istiyorum** | Yedekleme Zamanlama ek esneklik ve dosya, klasör, birimler, uygulamalar ve sistem durumu geri yükleme ve koruma için tam esneklik için Azure'da çalışan MABS/DPM ile Azure sanal makinelerini koruyun.

## <a name="backup-and-retention"></a>Yedekleme ve bekletme

Azure Backup’ta, *korumalı örnek* başına 9999 kurtarma noktası (yedekleme kopyası veya anlık görüntü olarak da bilinir) sınırı vardır.

- Korumalı örnek, verileri Azure’a yedeklemek için yapılandırılmış bir bilgisayar, sunucu (fiziksel veya sanal) veya iş yüküdür. Verilerin yedek kopyası kaydedildiğinde örnek, korumalı hale gelir.
- Verilerin yedek kopyası, korumayı oluşturur. Kaynak veriler, kaybolmaları veya bozulmaları durumunda yedek kopya kullanılarak geri yüklenebilir.

Aşağıdaki tabloda, her bileşen için en fazla yedekleme sıklığını gösterir. Kurtarma noktalarının ne kadar hızla tükettiğiniz, yedekleme İlkesi yapılandırmanız belirler. Örneğin, her gün bir kurtarma noktası oluşturursanız; kurtarma noktalarınızı 27 yıl boyunca bitmeden tutabilirsiniz. Aylık kurtarma noktası alırsanız, kurtarma noktalarınızı 833 yıl süreyle saklayabilirsiniz. Backup hizmeti, kurtarma noktası üzerinde bir sona erme süresi ayarlamaz.

|  | Azure Backup aracısı | System Center DPM | Azure Backup Sunucusu | Azure IaaS VM Backup |
| --- | --- | --- | --- | --- |
| Yedekleme sıklığı<br/> (Kurtarma hizmetleri kasasına) |Günde üç yedekleme |Günde iki yedekleme |Günde iki yedekleme |Günde bir yedekleme |
| Yedekleme sıklığı<br/> (diske) |Uygulanamaz |SQL Server için 15 dakikada bir<br/><br/> Diğer iş yükleri için saatte bir |SQL Server için 15 dakikada bir<br/><br/> Diğer iş yükleri için saatte bir |Geçerli değil |
| Bekletme seçenekleri |Günlük, haftalık, aylık, yıllık |Günlük, haftalık, aylık, yıllık |Günlük, haftalık, aylık, yıllık |Günlük, haftalık, aylık, yıllık |
| Korumalı örnek başına en fazla kurtarma noktası |9999|9999|9999|9999|
| En uzun bekletme süresi |Yedekleme sıklığına bağlıdır |Yedekleme sıklığına bağlıdır |Yedekleme sıklığına bağlıdır |Yedekleme sıklığına bağlıdır |
| Yerel diskteki kurtarma noktaları |Geçerli değil | Dosya sunucuları için 64<br/><br/> Uygulama Sunucuları için 448 | Dosya sunucuları için 64<br/><br/> Uygulama Sunucuları için 448 |Geçerli değil |
| Banttaki kurtarma noktaları |Geçerli değil |Sınırsız |Geçerli değil |Geçerli değil |

## <a name="how-does-azure-backup-work-with-encryption"></a>Azure Backup ile şifreleme nasıl çalışır?

**Şifreleme** | **Şirket içi yedekleme** | **Azure VM'lerini yedekleme** | **Azure Vm'lerinde SQL yedekleme**
--- | --- | --- | ---
Bekleme sırasında şifreleme<br/> (Burada kalıcı ve depolanan verileri şifreleme) | Müşteri tarafından belirtilen parola verileri şifrelemek için kullanılır. | Azure [depolama hizmeti şifrelemesi (SSE)](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) kasasında depolanan verileri şifrelemek için kullanılır.<br/><br/> Yedekleme verileri depolamadan önce otomatik olarak şifreler. Azure depolama, almadan önce verilerin şifresini çözer. Müşteri tarafından yönetilen anahtarlar için SSE kullanımı şu anda desteklenmiyor.<br/><br/> Kullanan sanal makinelerini yedekleyebilirsiniz [Azure disk şifrelemesi (ADE)](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview) işletim sistemi ve veri disklerini şifrelemek için. Azure Backup, BEK ile şifrelenmiş VM'ler destekler-yalnızca ve her iki BEK ve [KEK](https://blogs.msdn.microsoft.com/cclayton/2017/01/03/creating-a-key-encrypting-key-kek/). Gözden geçirme [sınırlamaları](backup-azure-vms-encryption.md#encryption-support). | Azure Backup sunucusu ile veya SQL Server veritabanlarını yedekleme destekler [TDE](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) etkin. Backup, Azure tarafından yönetilen anahtarlar veya müşteri tarafından yönetilen anahtarlar (BYOK) ile TDE destekler.<br/><br/> Yedekleme, Yedekleme işleminin parçası olarak herhangi bir SQL şifreleme gerçekleştirmez.
Aktarım sırasında şifreleme<br/> (Bir konumdan diğerine taşınmasını verilerin şifrelenmesi) | Veriler AES256 kullanılarak ve azure'daki kasasına HTTPS üzerinden gönderilen şifrelenir. | Azure içinde verileri Azure depolama ve kasa arasındaki HTTPS tarafından korunur. Bu veriler, Azure omurga ağında kalır.<br/><br/> Dosya Kurtarma için iSCSI kasası ve Azure VM arasında aktarılan verilerin güvenliğini sağlar. Güvenli bir tünel iSCSI kanal korur. | Azure içinde verileri Azure depolama ve kasa arasındaki HTTPS tarafından korunur.<br/><br/> Dosya Kurtarma SQL için uygun değil.

## <a name="next-steps"></a>Sonraki adımlar

- [Gözden geçirme](backup-architecture.md) mimarisini ve bileşenlerini farklı yedekleme senaryoları için.
- [Doğrulama](backup-support-matrix.md) destek gereksinimleri ve sınırlamaları ve yedekleme için [Azure VM yedeklemesi](backup-support-matrix-iaas.md).

[green]: ./media/backup-introduction-to-azure-backup/green.png
[yellow]: ./media/backup-introduction-to-azure-backup/yellow.png
[red]: ./media/backup-introduction-to-azure-backup/red.png
