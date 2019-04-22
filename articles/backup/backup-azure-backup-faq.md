---
title: Azure Yedekleme SSS
description: 'Hakkında sık sorulan sorulara yanıtlar: Kurtarma Hizmetleri kasaları, neleri yedekleyebilir, nasıl çalışır, şifreleme ve sınırları dahil olmak üzere azure yedekleme özellikleri. '
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: raynew
ms.openlocfilehash: 0981f4d5d9d5fcb243fc7ead6f4b529c096935d0
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58885885"
---
# <a name="azure-backup---frequently-asked-questions"></a>Azure Backup - sık sorulan sorular
Bu makalede, Azure Backup hizmeti hakkında sık sorulan sorular yanıtlanmaktadır.

## <a name="recovery-services-vault"></a>Kurtarma hizmetleri kasası

### <a name="is-there-any-limit-on-the-number-of-vaults-that-can-be-created-in-each-azure-subscription"></a>Her bir Azure aboneliği için oluşturulan kasaların sayısına yönelik herhangi bir sınır var mıdır?
Evet. Azure Backup'ın desteklenen bir bölge başına abonelik başına en çok 500 kurtarma Hizmetleri kasası oluşturabilirsiniz. Daha fazla kasaya ihtiyacınız varsa başka bir abonelik oluşturun.

### <a name="are-there-limits-on-the-number-of-serversmachines-that-can-be-registered-against-each-vault"></a>Her bir kasa için kaydedilebilen sunucu/makine sayısına yönelik sınırlar var mıdır?
Kasa başına 1000'e kadar Azure sanal makineleri kaydedebilirsiniz. Microsoft Azure Yedekleme aracısı kullanıyorsanız, kasa başına en fazla 50 MAB aracıları kaydedebilirsiniz. Ve 50 MAB sunucuları/DPM sunucularını bir kasaya kaydedebilir.

### <a name="if-my-organization-has-one-vault-how-can-i-isolate-data-from-different-servers-in-the-vault-when-restoring-data"></a>Kuruluşumun bir kasası varsa nasıl farklı sunuculardaki kasasındaki verileri verileri geri yüklerken ayırabilirim?
Birlikte kurtarmak istediğiniz sunucu verilerini yedekleme aynı parolayı kullanmalıdır. Belirli bir sunucu veya sunuculara kurtarma yalıtmak isterseniz, bu sunucu veya yalnızca sunucular için bir parola kullanın. Örneğin, insan kaynakları sunucuları bir şifreleme parolası kullanırken, muhasebe sunucuları ve depolama sunucuları farklı birer şifreleme parolası kullanabilir.

### <a name="can-i-move-my-vault-between-subscriptions"></a>My kasa abonelikler arasında taşıyabilir miyim?
Evet. Bu başvuran bir kurtarma Hizmetleri kasasına taşımak için [makale](backup-azure-move-recovery-services-vault.md)

### <a name="can-i-move-backup-data-to-another-vault"></a>Başka bir kasa için yedekleme verileri taşıyabilir miyim?
Hayır. Bir kasada depolanan yedekleme verileri, farklı bir kasaya taşınamaz.

### <a name="can-i-change-from-grs-to-lrs-after-a-backup"></a>Bir yedeklemeden sonra GRS LRS için değiştirebilirim?
Hayır. Bir kurtarma Hizmetleri kasası, yalnızca depolanan yedeklemelere önce depolama seçenekleri değiştirebilirsiniz.

### <a name="can-i-do-an-item-level-restore-ilr-for-vms-backed-up-to-a-recovery-services-vault"></a>Bir kurtarma Hizmetleri kasasına yedeklenen sanal makineler için bir öğe düzeyinde geri yükleme (ILR) yapabilirim?
- ILR Azure VM yedeklemesinde yedeklenen Azure Vm'leri için desteklenir. Daha fazla bilgi edinmek, [makale](backup-azure-restore-files-from-vm.md)
- Çevrimiçi kurtarma noktaları, şirket içi Vm'leri Azure backup sunucusu veya System Center DPM tarafından yedeklenen için ILR desteklenmiyor.


## <a name="azure-backup-agent"></a>Azure Backup aracısı

### <a name="where-can-i-find-common-questions-about-the-azure-backup-agent-for-azure-vm-backup"></a>Azure VM yedeklemesi için Azure Backup Aracısı hakkında sık sorulan sorular nereden bulabilirim?

- Aracı için bu okuma Azure Vm'lerinde çalışan [SSS](backup-azure-vm-backup-faq.md).
- Kullanılacak aracı için bu Azure dosya klasörü yedekleme okuma [SSS](backup-azure-file-folder-backup-faq.md).


## <a name="vmware-and-hyper-v-backup"></a>VMware ve Hyper-V yedekleme

### <a name="can-i-back-up-vmware-vcenter-servers-to-azure"></a>VMware vCenter sunucularını Azure'a yedekleyebilir miyim?
Evet. VMware vCenter Server ve ESXi konaklarının azure'a yedeklemek için Azure Backup Sunucusu'nu kullanabilirsiniz.

- [Daha fazla bilgi edinin](backup-mabs-protection-matrix.md) desteklenen sürümleri hakkında.
- [Bu adımları](backup-azure-backup-server-vmware.md) bir VMware sunucusunu yedeklemek için.

### <a name="do-i-need-a-separate-license-to-recover-an-full-on-premises-vmwarehyper-v-cluster"></a>Bir tam şirket içi VMware/Hyper-V kümesi kurtarmak için ayrı bir lisans gerekiyor mu?
Vmware'den/Hyper-V koruması için lisanslama ayrı yoktur.

- System Center müşterisiyseniz, VMware Vm'leri korumak için System Center Data Protection Manager (DPM) kullanın.
- System Center müşteri değilseniz, VMware Vm'leri korumak için Azure Backup sunucusu (Kullandıkça Öde) kullanabilirsiniz.

## <a name="dpm-and-azure-backup-server-backup"></a>DPM ve Azure Backup sunucusu yedekleme

### <a name="which-dpm-versions-are-supported"></a>DPM hangi sürümleri destekleniyor?
Desteklenen DPM sürümleri özetlenir [destek matrisi](backup-azure-dpm-introduction.md#prerequisites-and-limitations). En son DPM güncelleştirmelerini yükleme ve çalıştırma öneririz [en son sürümü](https://aka.ms/azurebackup_agent) Azure Backup aracısının DPM sunucusunda.

### <a name="can-i-register-the-server-to-multiple-vaults"></a>Sunucunun birden fazla kasaya kaydedebilir miyim?
Hayır. Bir DPM veya Azure Backup sunucusu yalnızca bir kasaya kaydedilebilir.

### <a name="can-i-use-azure-backup-server-to-create-a-bare-metal-recovery-bmr-backup-for-a-physical-server-br"></a>Bir fiziksel sunucu için Tam Kurtarma (BMR) yedeklemesi oluşturmak üzere Azure Backup Sunucusu'nu kullanabilir miyim? <br/>
Evet.

### <a name="can-i-use-dpm-to-back-up-apps-in-azure-stack"></a>Uygulamaları Azure Stack'te yedeklemek için DPM'yi kullanabilir miyim?
Hayır. Uygulamaları Azure Stack'te yedeklemek için DPM'yi kullanarak Azure Backup desteklemiyor, Azure Stack korumak için Azure Backup'ı kullanabilirsiniz.

### <a name="if-ive-installed-azure-backup-agent-to-protect-my-files-and-folders-can-i-install-system-center-dpm-to-back-up-on-premises-workloads-to-azure"></a>Dosya ve klasörlerimi korumak için Azure Backup aracısını yükledim, şirket içi iş yüklerini Azure'a yedeklemek için System Center DPM'yi yükleyebilir miyim?
Evet. Ancak önce DPM'yi ayarlayın ve ardından Azure Backup aracısını yüklemeniz gerekir.  Bileşenlerinin bu sırada yüklenmesi, Azure Backup aracısının DPM ile çalışmasını sağlar. Aracıyı DPM yüklenmeden önce yüklenmesi önerilmez veya değil.

## <a name="general-backup"></a>Genel yedekleme

### <a name="are-there-limits-on-backup-scheduling"></a>Yedekleme Zamanlama sınırları vardır?
Evet.
- Windows Server veya Windows günde üç kez makinelere yedekleyebilirsiniz. Günlük veya haftalık zamanlamalar için zamanlama ilkesini ayarlayabilirsiniz.
- Günde DPM'yi yedekleyebilirsiniz. Günlük, haftalık, aylık ve yıllık için zamanlama ilkesini ayarlayabilirsiniz.
- Günde bir Azure sanal makinelerini yedekleyebilirsiniz.

### <a name="what-operating-systems-are-supported-for-backup"></a>Yedekleme için desteklenen işletim sistemleri?
Azure Backup, dosyaları ve Azure Backup sunucusu ve DPM tarafından korunan uygulamalar ve klasörleri yedeklemek için bu işletim sistemlerini destekler.

**OS** | **SKU** | **Ayrıntılar**
--- | --- | ---
İş istasyonu | |
Windows 10 64 bit | Enterprise, Pro, Home | Makineler en son hizmet paketleri ve güncelleştirmeler çalıştırıyor olmalıdır.
Windows 8.1 64 bit | Enterprise, Pro | Makineler en son hizmet paketleri ve güncelleştirmeler çalıştırıyor olmalıdır.
Windows 8 64 bit | Enterprise, Pro | Makineler en son hizmet paketleri ve güncelleştirmeler çalıştırıyor olmalıdır.
Windows 7 64-bit | Ultimate, Enterprise, Professional, Home Premium, Home Basic, Starter | Makineler en son hizmet paketleri ve güncelleştirmeler çalıştırıyor olmalıdır.
Sunucu | |
Windows Server 2019 64 bit | Standard, Datacenter, Essentials | En son hizmet paketleri/güncelleştirmelerle.
Windows Server 2016 64 bit | Standard, Datacenter, Essentials | En son hizmet paketleri/güncelleştirmelerle.
Windows Server 2012 R2 64 bit | Standard, Datacenter, Foundation | En son hizmet paketleri/güncelleştirmelerle.
Windows Server 2012 64 bit | Datacenter, Foundation, Standard | En son hizmet paketleri/güncelleştirmelerle.
Windows Storage Server 2016 64 bit | Standard, Workgroup | En son hizmet paketleri/güncelleştirmelerle.
Windows Storage Server 2012 R2 64 bit | Standart, çalışma grubu, gerekli | En son hizmet paketleri/güncelleştirmelerle.
Windows Storage Server 2012 64 bit | Standard, Workgroup | En son hizmet paketleri/güncelleştirmelerle.
Windows Server 2008 R2 SP1 64 bit | Standard, Enterprise, Datacenter, Foundation | En son güncelleştirmeleri ile.
Windows Server 2008 64 bit | Standard, Enterprise, Datacenter | En son güncelleştirmeler yüklü.

Azure VM Linux yedeklemeleri için Azure yedeklemeyi destekler [Azure tarafından desteklenen dağıtım listesini](../virtual-machines/linux/endorsed-distros.md), Core OS Linux ve 32-bit işletim sistemi hariç. Diğer Getir kendi Linux dağıtımları VM Aracısı VM üzerinde kullanılabilir olduğu sürece çalışır ve Python desteği bulunduğu.


### <a name="are-there-size-limits-for-data-backup"></a>Veri yedekleme boyutu sınırlar var mıdır?
Boyutları sınırlamaları aşağıdaki gibidir:

İşletim sistemi/makine | Veri kaynağı boyutu sınırı
--- | --- 
Windows 8 veya üzeri | 54.400 GB
Windows 7 |1700 GB
Windows Server 2012 veya üzeri | 54.400 GB
Windows Server 2008, Windows Server 2008 R2 | 1700 GB
Azure VM | 16 veri diski<br/><br/> 4095 GB'a kadar veri diski

### <a name="how-is-the-data-source-size-determined"></a>Belirlenen veri kaynağı boyutu nasıl mi?
Aşağıdaki tabloda, her bir veri kaynağı boyutunun nasıl belirlendiği açıklanmaktadır.

**Veri kaynağı** | **Ayrıntılar**
--- | ---
Birim |Tek bir birime yedeklenen VM'ye gelen verilerin miktarını yedeklendi.
SQL Server veritabanı |Yedeklenmekte olan tek SQL veritabanının boyutunu.
SharePoint | Yedeklenmekte olan bir SharePoint grubu içinde içerik ve yapılandırma veritabanlarının toplamı.
Exchange |Yedeklenmekte olan bir Exchange sunucusundaki tüm Exchange veritabanlarının toplamı.
BMR/sistem durumu |Yedeklenmekte olan makinenin BMR'yi veya sistem durumunu tek tek her kopyası.

### <a name="is-there-a-limit-on-the-amount-of-data-backed-up-using-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası kullanılarak yedeklenen veri miktarına bir sınır var mıdır?
Kurtarma Hizmetleri kasası kullanarak yedekleme veri miktarına bir sınır yoktur.

### <a name="why-is-the-size-of-the-data-transferred-to-the-recovery-services-vault-smaller-than-the-data-selected-for-backup"></a>Neden veri boyutu, yedekleme için seçilen verileri daha küçük kurtarma Hizmetleri kasasına aktarılır?
Azure Backup aracısını, DPM, yedeklenen verileri ve Azure Backup sunucusu sıkıştırılır ve aktarılmadan önce şifrelenir. İle sıkıştırma ve şifreleme uygulandığında, kasasındaki veriler % 30-40 daha küçük.

### <a name="can-i-delete-individual-files-from-a-recovery-point-in-the-vault"></a>Tek tek dosyaları kasasında bir kurtarma noktasından silebilir misiniz?
Hayır, Azure Backup, silme veya tek tek öğeleri depolanan yedeklerden temizleme desteklemiyor.

### <a name="if-i-cancel-a-backup-job-after-it-starts-is-the-transferred-backup-data-deleted"></a>Başladıktan sonra bir yedekleme işini iptal, aktarılan yedekleme verileri silindi mi?
Hayır. Yedekleme işinin önce kasaya aktarılan tüm veriler kasada kalır iptal edildi.

- Azure Backup, yedekleme işlemi sırasında yedekleme verilerine zaman zaman denetim noktaları eklemek üzere bir denetim noktası mekanizması kullanır.
- Yedekleme verilerinde denetim noktaları bulunduğundan, sonraki yedekleme işlemi dosyaların bütünlüğünü doğrulayabilir.
- Bir sonraki yedekleme işi, daha önce yedeklenen verilerin üzerine artımlı olarak gerçekleşir. Artımlı yedekleme işlemlerinin yalnızca yeni veya değiştirilmiş verileri aktarması, bant genişliğinin daha iyi kullanılması anlamına gelir.

Bir Azure VM’ye yönelik bir yedekleme işini iptal ederseniz aktarılan tüm veriler yoksayılır. Bir sonraki yedekleme işi, son başarılı yedekleme işinden artımlı verileri aktarır.

## <a name="retention-and-recovery"></a>Elde tutma ve kurtarma

### <a name="are-the-retention-policies-for-dpm-and-windows-machines-without-dpm-the-same"></a>DPM olmayan DPM ve Windows makineleri için bekletme ilkeleri aynı mı?
Evet, her ikisi de günlük, haftalık, aylık ve yıllık bekletme ilkelerine sahiptir.

### <a name="can-i-customize-retention-policies"></a>Bekletme ilkeleri özelleştirebilirim?
Evet, özelleştirdikten ilkeleri. Örneğin, haftalık ve günlük bekletme gereksinimlerini, ancak yıllık ve aylık yapılandırabilirsiniz.

### <a name="can-i-use-different-times-for-backup-scheduling-and-retention-policies"></a>Farklı bir kez Yedekleme Zamanlama ve bekletme ilkeleri için kullanabilir miyim?
Hayır. Bekletme ilkeleri, yalnızca yedekleme noktalarında uygulanabilir. Örneğin, bu görüntüleri, 12: 00 ve 18: 00 alınan yedeklemeler için bekletme ilkesini gösterir.

![Yedeklemeyi ve Bekletmeyi Zamanlama](./media/backup-azure-backup-faq/Schedule.png)


### <a name="if-a-backup-is-kept-for-a-long-time-does-it-take-more-time-to-recover-an-older-data-point-br"></a>Bir yedekleme uzun bir süre tutulur, eski bir veri noktasının kurtarılması daha uzun sürer? <br/>
Hayır. En eski kurtarma süresi veya en yeni noktanın aynıdır. Her kurtarma noktası bir tam nokta gibi davranır.

### <a name="if-each-recovery-point-is-like-a-full-point-does-it-impact-the-total-billable-backup-storage"></a>Her kurtarma noktası bir tam nokta gibiyse bu durum toplam faturalanabilir yedekleme alanını etkiler mi?
Genel uzun vadeli bekletme noktası ürünleri, yedekleme verilerini tam noktalar olarak depolar.

    - Tam noktalar depolama açısından *verimsizdir* ancak daha kolay ve hızlı şekilde geri yüklenir.
    - Artımlı kopyalar depolama olan *verimli* ancak bir zincir, kurtarma süresini etkileyecek verileri geri yüklemek ihtiyaç duyduğunuz

Azure Backup alanı mimarisi, hızlı geri yükleme özelliğiyle optimum veri depolama olanağı sunarken aynı zamanda düşük depolama maliyetleri oluşturarak iki açıdan da avantaj sağlar. Bu, giriş ve çıkış bant genişliğiniz verimli bir şekilde kullanılmasını sağlar. Veri depolama ve verileri kurtarmak için gereken süre miktarını, bir en düşük düzeyde tutulur. Daha fazla bilgi edinin [artımlı yedeklemeler](https://azure.microsoft.com/blog/microsoft-azure-backup-save-on-long-term-storage/).

### <a name="is-there-a-limit-on-the-number-of-recovery-points-that-can-be-created"></a>Oluşturulabilecek kurtarma noktalarının sayısına yönelik bir sınır var mıdır?
Korumalı bir örnek için en çok 9999 kurtarma noktası oluşturabilirsiniz. Korumalı örnek, bir bilgisayar, sunucu (fiziksel veya sanal) veya Azure'a yedekler iş yükü ' dir.

- Daha fazla bilgi edinin [yedekleme ve bekletme](./backup-introduction-to-azure-backup.md#backup-and-retention).
- Hakkında bilgi edinin [korumalı örnekler](./backup-introduction-to-azure-backup.md#what-is-a-protected-instance)?

### <a name="how-many-times-can-i-recovery-data-thats-backed-up-to-azure"></a>Kaç kez kullanabilir miyim Azure'a yedeklenen verileri kurtarma?
Azure Backup ile gerçekleştirilen kurtarma işlemlerinin sayısına yönelik bir sınır yoktur.

### <a name="when-restoring-data-do-i-pay-for-the-egress-traffic-from-azure"></a>Verileri geri yüklerken Azure'dan çıkış trafiği için ödeme yapacak mıyım?
Hayır. Kurtarma işlemi ücretsizdir ve çıkış trafiği için ücretlendirilmezsiniz.

### <a name="what-happens-when-i-change-my-backup-policy"></a>My yedekleme ilkesini değiştirdiğimde ne olur?
Yeni bir ilke uygulandığında yeni ilkenin zamanlama ve ardından.

- Bekletme süresi uzatıldıysa, yeni ilkeye göre tutulması için mevcut kurtarma noktaları işaretlenir.
- Bekletme süresi kısaltıldıysa, bunlar sonraki temizleme işleminde kesilmek üzere işaretlenir ve sonra silinir.

## <a name="encryption"></a>Şifreleme

### <a name="is-the-data-sent-to-azure-encrypted"></a>Veriler Azure'a şifreli olarak mı gönderilir?
Evet. Veriler AES256 kullanılarak şirket içi makinede şifrelenir. Veriler güvenli bir HTTPS bağlantısı üzerinden gönderilir. Buluta gönderilen verileri yalnızca depolama ve kurtarma hizmeti arasında HTTPS bağlantısı korunur. iSCSI protokolü kurtarma hizmeti ve kullanıcı makine arasında aktarılan verilerin güvenliğini sağlar. Güvenli bir tünel iSCSI kanalı korumak için kullanılır.

### <a name="is-the-backup-data-on-azure-encrypted-as-well"></a>Azure üzerindeki yedekleme verileri de şifreli midir?
Evet. Azure'da bekleyen şifrelenmiş verilerdir.

- Şirket içi yedekleme için Azure'a yedeklerken sağladığınız parolayı kullanarak bekleyen şifreleme sağlanır.
- Azure Vm'leri için şifrelenmiş depolama hizmeti şifrelemesi (SSE) kullanarak bekleyen verilerdir.

Microsoft herhangi bir noktada yedekleme verilerinin şifresini çözmez.

### <a name="what-is-the-minimum-length-of-encryption-the-key-used-to-encrypt-backup-data"></a>Şifreleme anahtarı yedekleme verilerini şifrelemek için kullanılan minimum uzunluğu nedir?
Azure Backup aracısını kullanırken şifreleme anahtarı en az 16 karakter olmalıdır. Azure VM'lerde, Azure KeyVault tarafından kullanılan anahtarlar için boyut sınırlaması yoktur.

### <a name="what-happens-if-i-misplace-the-encryption-key-can-i-recover-the-data-can-microsoft-recover-the-data"></a>Şifreleme anahtarını kaybedersem ne olur? Veri kurtarma gerçekleştirebilir miyim? Microsoft, verileri kurtarabilir miyim?
Yedekleme verilerini şifrelemek için kullanılan anahtar yalnızca sitesinde mevcuttur. Microsoft, Azure üzerinde anahtarın bir kopyasını tutmaz ve anahtara erişim sahibi değildir. Anahtar misplace durumunda Microsoft Yedekleme verilerini kurtaramaz.

## <a name="next-steps"></a>Sonraki adımlar

Diğer SSS'leri okuyun:

- [Sık sorulan sorular](backup-azure-vm-backup-faq.md) Azure VM yedeklemeleri hakkında.
- [Sık sorulan sorular](backup-azure-file-folder-backup-faq.md) Azure Backup Aracısı hakkında
