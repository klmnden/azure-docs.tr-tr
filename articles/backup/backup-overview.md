---
title: Azure Backup nedir?
description: Azure Backup hizmeti ve iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize parçası olarak dağıtma hakkında genel bir bakış sağlar.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: overview
ms.date: 04/05/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 5408f920a16860972dca6450d5e51152048bbf82
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60254692"
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
- **Verileri güvenli tutmak**:
    - Şirket içi, Taşınmakta olan veriler AES256 kullanılarak şirket içi makinede şifrelenir. İletilen veriler, depolama ve yedekleme HTTPS tarafından korunur. İSCSI protokolü, yedekleme ve kullanıcı makine arasında aktarılan verilerin güvenliğini sağlar. Güvenli bir tünel iSCSI kanalı korumak için kullanılır.
    - Şirket içi için Azure yedekleme, azure'daki şifreli yedekleme sağladığınız parolayı kullanarak bekleyen verilerdir. Hiçbir zaman aktarılan veya Azure'da depolanan anahtar ve parola. Verileri geri yüklemeniz gerekirse, şifreleme parolası veya anahtarı yalnızca sizde olur.
    - Azure Vm'leri için veriler şifrelenir sıfırlama sırasında depolama hizmeti şifrelemesi (SSE) kullanma. Yedekleme verileri depolamadan önce otomatik olarak şifreler. Azure depolama, almadan önce verilerin şifresini çözer.
    - Backup, Azure Azure Disk şifrelemesi (ADE) kullanılarak şifrelenmiş VM'ler de destekler. [Daha fazla bilgi edinin](backup-azure-vms-introduction.md#encryption-of-azure-vm-backups).
- **Uygulamayla tutarlı yedekler almak**: Bir uygulamayla tutarlı yedekleme, kurtarma noktası yedek kopyayı geri yüklemek için tüm gerekli verileri içeren anlamına gelir. Azure Backup, verileri geri yüklerken ek düzeltmelere gerek kalmaması için uygulamayla tutarlı yedeklemeler yapılmasını sağlar. Uygulamayla tutarlı verilerin geri yüklenmesi, geri yükleme süresini azaltarak hizmetlerinizin kısa süre içinde çalışır hale gelmesini sağlar.
- **Kısa ve uzun süreli saklanması**: Kısa vadede ve uzun veri saklama için kurtarma Hizmetleri kasalarını kullanabilirsiniz. Azure, bir Kurtarma Hizmetleri kasasında verileri saklama süresini kısıtlamaz. Dilediğiniz sürece için tutabilirsiniz. Azure Backup, korunan her örnek için 9999 kurtarma noktası sınırına sahiptir. [Daha fazla bilgi edinin](backup-introduction-to-azure-backup.md#backup-and-retention)bu sınırın yedekleme gereksinimlerinizi nasıl etkileyeceği hakkında.
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
**Şirket içi Windows makinelerini yedekleme** | İndirme, yükleme ve MARS aracısının doğrudan makinede çalıştırın.
**MARS Aracısı ile Azure Vm'leri yedekleme** | İndirin, yükleyin ve MARS aracısının doğrudan makinede çalıştırın. MARS aracısının yedekleme uzantıyla birlikte çalıştırabilirsiniz.
**DPM/MABS için şirket içi makinelerin ve Azure sanal makinelerini yedekleme** | DPM veya MABS koruma aracısını korumak istediğiniz makinelerde çalıştırır. MARS aracısının DPM sunucusu/Azure'a yedeklemek için MABS çalışır.

## <a name="which-backup-agent-should-i-use"></a>Hangi yedekleme aracısı kullanmam gerekir?

**Backup** | **Çözüm** | **Sınırlama**
--- | --- | ---
**Tüm bir Azure VM'yi yedekleme istiyorum** | VM için yedeklemeyi etkinleştirin. Backup uzantısı Windows veya Linux Azure VM üzerinde otomatik olarak yapılandırılır. | Tüm VM yedeklenir <br/><br/> Windows VM'ler için uygulamayla tutarlı yedeklemedir. Linux için yedekleme dosyası tutarlıdır. Linux VM'ler için uygulama durumunu algılayan gerekiyorsa bu özel betiklerle yapılandırmanız gerekir.
**Azure VM'de belirli dosyaları/klasörleri yedeklemek üzere istiyorum** | MARS Aracısı VM üzerinde dağıtın.
**Şirket içi Windows makineleri doğrudan istiyorum** | MARS Aracısı makinesine yükleyin. | Dosyaları, klasörleri ve sistem durumu yedekleme Azure'a yedekleyebilirsiniz. Uygulama durumunu algılayan yedeklemelerini değildir.
**Doğrudan şirket içi Linux makinelerini yedekleme istiyorum** | DPM veya MABS Azure'a yedeklemek için dağıtmanız gerekebilir. | Linux ana yedeklemesi desteklenmiyor, Hyper-V veya VMWare üzerinde barındırılan yalnızca yedekleme Linux Konuk makine olabilir.
**Şirket içinde çalışan uygulamaların yedeğini istiyorum** | Uygulama durumunu algılayan yedekleme için DPM veya MABS makineler korunmalıdır.
**Ayrıntılı ve esnek yedekleme ve kurtarma ayarlarını Azure Vm'leri için istiyorum** | Yedekleme Zamanlama ek esneklik ve dosya, klasör, birimler, uygulamalar ve sistem durumu geri yükleme ve koruma için tam esneklik için Azure'da çalışan MABS/DPM ile Azure sanal makinelerini koruyun.


## <a name="next-steps"></a>Sonraki adımlar

- [Gözden geçirme](backup-architecture.md) mimarisini ve bileşenlerini farklı yedekleme senaryoları için.
- [Doğrulama](backup-support-matrix.md) desteklenen özellikler ve ayarlar için yedekleme.

[green]: ./media/backup-introduction-to-azure-backup/green.png
[yellow]: ./media/backup-introduction-to-azure-backup/yellow.png
[red]: ./media/backup-introduction-to-azure-backup/red.png
