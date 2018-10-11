---
title: Azure Yedekleme SSS
description: 'Kurtarma Hizmetleri kasaları, neleri yedekleyebilir, nasıl çalışır, şifreleme ve limitlerin dahil olduğu Azure Backup özellikleriyle ilgili yaygın soruların yanıtları. '
services: backup
author: markgalioto
manager: carmonm
keywords: yedekleme ve olağanüstü durum kurtarma; backup hizmeti
ms.service: backup
ms.topic: conceptual
ms.date: 8/2/2018
ms.author: markgal
ms.openlocfilehash: 561b5d3d9769b509e21a3005d771529f342e2171
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49067507"
---
# <a name="questions-about-the-azure-backup-service"></a>Azure Backup hizmetiyle ilgili sorular
Bu makalede, Azure Backup bileşenleri hakkında sık sorulan sorular yanıtlanmaktadır. Bazı yanıtlarda, kapsamlı bilgiler içeren makalelerin bağlantıları vardır. **Yorumlar**’a (sağda) tıklayarak Azure Backup hakkında soru sorabilirsiniz. Yorumlar bu makalenin altında görünür. Ayrıca Azure Backup hizmeti ile ilgili sorularınızı [tartışma forumunda](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) paylaşabilirsiniz.

Bu makaledeki bölümleri hızlı taramak için **bu makale altında** sağ taraftaki bağlantıları kullanın.


## <a name="recovery-services-vault"></a>Kurtarma hizmetleri kasası

### <a name="is-there-any-limit-on-the-number-of-vaults-that-can-be-created-in-each-azure-subscription-br"></a>Her bir Azure aboneliği için oluşturulan kasaların sayısına yönelik herhangi bir sınır var mıdır? <br/>
Evet. Azure Backup, abonelik başına desteklenen bölge başına 500 adede kadar kurtarma Hizmetleri kasası oluşturabilirsiniz. Daha fazla kasaya ihtiyacınız varsa başka bir abonelik oluşturun.

### <a name="are-there-limits-on-the-number-of-serversmachines-that-can-be-registered-against-each-vault-br"></a>Her bir kasa için kaydedilebilen sunucu/makine sayısına yönelik sınırlar var mıdır? <br/>
Kasa başına en fazla 1000 Azure sanal makineleri kaydedebilirsiniz. MAB Aracısı kullanıyorsanız, kasa başına en fazla 50 MAB aracıları kaydedebilirsiniz. Ve 50 MAB sunucuları/DPM sunucularını bir kasaya kaydedebilir.

### <a name="can-i-use-a-rest-api-to-query-the-size-of-protected-items-in-a-vault-br"></a>Bir kasadaki korumalı öğelerin boyutunu sorgulamak için bir REST API'si kullanabilir miyim? <br/>
Evet, makale [kullanımları - Listele kasaları tarafından](https://t.co/2lgIrIaF0J), Kurtarma Hizmetleri kasasından alınabilir bilgilerini listeler.

### <a name="if-my-organization-has-one-vault-how-can-i-isolate-one-servers-data-from-another-server-when-restoring-databr"></a>Kuruluşumun bir kasası varsa veri geri yükleme sırasında bir sunucunun verilerini başka bir sunucudan nasıl ayırabilirim?<br/>
Aynı kasaya kayıtlı tüm sunucular, *aynı parolayı kullanan* diğer sunucular tarafından yedeklenen verileri kurtarabilir. Yedekleme verilerini kuruluşunuzdaki diğer sunucularından ayırmak istediğiniz sunucularınız varsa bu sunucular için belirlenmiş bir parola kullanın. Örneğin, insan kaynakları sunucuları bir şifreleme parolası kullanırken, muhasebe sunucuları ve depolama sunucuları farklı birer şifreleme parolası kullanabilir.

### <a name="can-i-migrate-my-vault-between-subscriptions-br"></a>My kasamın abonelikler arasında geçişini sağlayabilir miyim? <br/>
Hayır. Kasa abonelik düzeyinde oluşturulur ve başka bir aboneliğe yeniden atanamaz.

### <a name="can-i-migrate-backup-data-to-another-vault-br"></a>Başka bir kasa için yedekleme verileri geçirebilir miyim? <br/>
Hayır. Bir kasada depolanan yedekleme verileri, farklı bir kasaya taşınamaz.

### <a name="can-i-change-from-grs-to-lrs-after-a-backup-br"></a>Bir yedeklemeden sonra GRS LRS için değiştirebilirim? <br/>
Hayır. Bir kurtarma Hizmetleri kasası, yalnızca depolanan yedeklemelere önce depolama seçenekleri değiştirebilirsiniz.

### <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-still-supported-br"></a>Kurtarma Hizmetleri kasaları Resource Manager tabanlıdır. Yedekleme kasaları hâlâ destekleniyor? <br/>
Yedekleme kasaları kurtarma Hizmetleri kasalarına dönüştürüldü. Bir kurtarma Hizmetleri kasasına yedekleme kasasına dönüştürülüp, ardından yedekleme kasası kurtarma Hizmetleri kasası için sizin için dönüştürüldü.

### <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Bir Backup kasasının Kurtarma Hizmetleri kasasına geçişini sağlayabilir miyim? <br/>
Tüm yedekleme kasaları kurtarma Hizmetleri kasalarına dönüştürüldü. Bir kurtarma Hizmetleri kasasına yedekleme kasasına dönüştürülüp, ardından yedekleme kasası kurtarma Hizmetleri kasası için sizin için dönüştürüldü.

## <a name="azure-backup-agent"></a>Azure Backup aracısı
Soruların ayrıntılı listesini [Azure dosya-klasör yedekleme hakkında SSS](backup-azure-file-folder-backup-faq.md) altında bulabilirsiniz.

## <a name="azure-vm-backup"></a>Azure VM yedeklemesi
Soruların ayrıntılı listesini [Azure VM yedeklemesi hakkında SSS](backup-azure-vm-backup-faq.md) altında bulabilirsiniz.

## <a name="back-up-vmware-servers"></a>VMware sunucularını yedekleme

### <a name="can-i-back-up-vmware-vcenter-servers-to-azure"></a>VMware vCenter sunucularını Azure'a yedekleyebilir miyim?
Evet. VMware vCenter ve ESXi’yi Azure’a yedeklemek için Azure Backup Sunucusu'nu kullanabilirsiniz. Desteklenen VMware sürümü hakkında daha fazla bilgi için [Azure Backup Sunucusu koruma matrisi](backup-mabs-protection-matrix.md) adlı makaleye bakın. Adım adım yönergeler için bkz. [VMware sunucusunu yedeklemek için Azure Backup Sunucusu’nu kullanma](backup-azure-backup-server-vmware.md).

### <a name="do-i-need-a-separate-license-to-recover-a-full-on-premises-vmwarehyper-v-cluster-from-dpm-or-azure-backup-serverbr"></a>DPM veya Azure Backup sunucusu tam şirket içi VMware/Hyper-V kümesi kurtarmak için ayrı bir lisans gerekiyor mu?<br/>
Vmware'den/Hyper-V koruması için lisanslama ayrı yoktur. System Center müşterisiyseniz, VMware Vm'leri korumak için DPM'yi kullanma. System Center müşteri değilseniz, VMware Vm'leri korumak için Azure Backup sunucusu (Kullandıkça Öde) kullanabilirsiniz.

## <a name="azure-backup-server-and-system-center-data-protection-manager"></a>Azure Backup Sunucusu ve System Center Data Protection Manager
### <a name="can-i-use-azure-backup-server-to-create-a-bare-metal-recovery-bmr-backup-for-a-physical-server-br"></a>Bir fiziksel sunucu için Tam Kurtarma (BMR) yedeklemesi oluşturmak üzere Azure Backup Sunucusu'nu kullanabilir miyim? <br/>
Evet.

### <a name="can-i-register-my-dpm-server-to-multiple-vaults-br"></a>DPM Sunucumu birden fazla kasaya kaydedebilir miyim? <br/>
Hayır. DPM veya MABS sunucuları yalnızca bir kasaya kaydedilebilir.

### <a name="which-version-of-system-center-data-protection-manager-is-supported"></a>System Center Data Protection Manager’ın hangi sürümü desteklenir?
[En son](http://aka.ms/azurebackup_agent) Azure Backup aracısını, System Center Data Protection Manager'ın (DPM) en son güncelleştirme paketi (UR) üzerine yüklemeniz önerilir.
- System Center DPM 2012 R2 için [güncelleştirme paketi 14](https://support.microsoft.com/help/4043315/update-rollup-14-for-system-center-2012-r2-data-protection-manager) en son güncelleştirmesidir.
- System Center DPM 2016, [güncelleştirme dökümü 2](https://support.microsoft.com/en-us/help/3209593) en son güncelleştirmesidir.

### <a name="i-have-installed-azure-backup-agent-to-protect-my-files-and-folders-can-i-install-system-center-dpm-to-protect-on-premises-applicationvm-workloads-to-azure"></a>Dosya ve klasörlerimi korumak için Azure Backup aracısını yükledim. Şirket içi uygulama/VM iş yüklerini azure'a korumak için System Center DPM yükleyebilirim?
Evet. Ancak, Azure Backup, System Center Data Protection Manager (DPM) ile kullanmak için önce DPM'yi yükleyin ve ardından Azure Backup aracısını yükleyin. Azure Backup bileşenlerinin bu sırada yüklenmesi, Azure Backup aracısının DPM ile çalışmasını sağlar. Azure Backup aracısının DPM yüklenmeden önce yüklenmesi önerilmez veya desteklenmez.

### <a name="can-i-use-dpm-to-back-up-apps-in-azure-stack"></a>Uygulamaları Azure Stack'te yedeklemek için DPM'yi kullanabilir miyim?
Hayır. Azure Stack korumak için Azure Backup kullanabilmenize rağmen Azure Backup şu anda Azure Stack'te uygulama yedeklemek için DPM'yi kullanarak desteklemez.

## <a name="how-azure-backup-works"></a>Azure Backup nasıl çalışır?
### <a name="if-i-cancel-a-backup-job-once-it-has-started-is-the-transferred-backup-data-deleted-br"></a>Bir yedekleme işini başlatıldıktan sonra iptal edersem aktarılan yedekleme verileri silinir mi? <br/>
Hayır. Yedekleme işi iptal edilmeden önce kasaya aktarılan tüm veriler kasada kalır. Azure Backup, yedekleme işlemi sırasında yedekleme verilerine zaman zaman denetim noktaları eklemek üzere bir denetim noktası mekanizması kullanır. Yedekleme verilerinde denetim noktaları bulunduğundan, sonraki yedekleme işlemi dosyaların bütünlüğünü doğrulayabilir. Bir sonraki yedekleme işi, daha önce yedeklenen verilerin üzerine artımlı olarak gerçekleşir. Artımlı yedekleme işlemlerinin yalnızca yeni veya değiştirilmiş verileri aktarması, bant genişliğinin daha iyi kullanılması anlamına gelir.

Bir Azure VM’ye yönelik bir yedekleme işini iptal ederseniz aktarılan tüm veriler yoksayılır. Bir sonraki yedekleme işi, son başarılı yedekleme işinden artımlı verileri aktarır.

### <a name="are-there-limits-on-when-or-how-many-times-a-backup-job-can-be-scheduledbr"></a>Bir yedekleme işinin ne zaman veya kaç kez zamanlanabileceğine yönelik sınırlar var mıdır?<br/>
Evet. Yedekleme işlerini Windows Server veya Windows iş istasyonları üzerinde günde en fazla üç kez çalıştırabilirsiniz. Yedekleme işlerini System Center DPM üzerinde en fazla günde iki kez çalıştırabilirsiniz. Bir yedekleme işini IaaS VM'ler için günde bir kez çalıştırabilirsiniz. Windows Server veya Windows iş istasyonu için zamanlama ilkesini, günlük veya haftalık zamanlamalar belirtmek için kullanın. System Center DPM ile günlük, haftalık, aylık ve yıllık zamanlamalar belirtebilirsiniz.

### <a name="why-is-the-size-of-the-data-transferred-to-the-recovery-services-vault-smaller-than-the-data-i-backed-upbr"></a>Kurtarma Hizmetleri kasasına aktarılan verilerin büyüklüğü neden yedeklediğim verilerden daha küçük?<br/>
 Azure Backup Aracısı veya SCDPM ya da Azure Backup Sunucusundan yedeklenen tüm veriler aktarılmadan önce sıkıştırılır ve şifrelenir. Sıkıştırma ve şifreleme uygulandıktan sonra kurtarma Hizmetleri kasasındaki veriler % 30-40 daha küçük ' dir.

### <a name="can-i-delete-individual-files-from-a-recovery-point-in-the-vaultbr"></a>Tek tek dosyaları kasasında bir kurtarma noktasından silebilir misiniz?<br/>
Hayır, Azure Backup, silme veya tek tek öğeleri depolanan yedeklerden temizleme desteklemiyor.

## <a name="what-can-i-back-up"></a>Neleri yedekleyebilirim?
### <a name="which-operating-systems-does-azure-backup-support-br"></a>Azure Backup hangi işletim sistemlerini destekler? <br/>
Azure Backup, Azure Backup Sunucusu ve System Center Data Protection Manager (DPM) kullanılarak korunan dosya ve klasörlerin yanı sıra iş yükü uygulamalarının yedeklenmesi için aşağıdaki listede yer alan işletim sistemlerini destekler.

| İşletim Sistemi | Platform | SKU |
|:--- | --- |:--- |
| Windows 8 ve en son SP'ler |64 bit |Enterprise, Pro |
| Windows 7 ve en son SP'ler |64 bit |Ultimate, Enterprise, Professional, Home Premium, Home Basic, Starter |
| Windows 8.1 ve en son SP'ler |64 bit |Enterprise, Pro |
| Windows 10 |64 bit |Enterprise, Pro, Home |
| Windows Server 2016 |64 bit |Standard, Datacenter, Essentials |
| Windows Server 2012 R2 ve en son SP'ler |64 bit |Standard, Datacenter, Foundation |
| Windows Server 2012 ve en son SP'ler |64 bit |Datacenter, Foundation, Standard |
| Windows Storage Server 2016 ve en son SP'ler |64 bit |Standard, Workgroup |
| Windows Storage Server 2012 R2 ve en son SP'ler |64 bit |Standard, Workgroup |
| Windows Storage Server 2012 ve en son SP'ler |64 bit |Standard, Workgroup |
| Windows Server 2012 R2 ve en son SP'ler |64 bit |Essential |
| Windows Server 2008 R2 SP1 |64 bit |Standard, Enterprise, Datacenter, Foundation |

**Azure VM yedeklemesi için:**

* **Linux**: Azure Backup, [Azure tarafından onaylanan bir dağıtım listesini](../virtual-machines/linux/endorsed-distros.md) (CoreOS Linux hariç) destekler.  Sanal makinede VM aracısı kullanılabilir olduğu ve Python desteği bulunduğu sürece diğer Kendi Linux’unu Getir dağıtımları da çalışabilir.
* **Windows Server**:  Windows Server 2008 R2’den eski sürümler desteklenmez.


### <a name="is-there-a-limit-on-the-size-of-each-data-source-being-backed-up-br"></a>Yedeklenmekte olan her veri kaynağının boyutuna yönelik bir sınır var mıdır? <br/>
Azure yedekleme, bir veri kaynağı için boyut sınırı zorlar, ancak kaynağı için limitler yüksektir. Ağustos 2015 itibarıyla, desteklenen işletim sistemlerinde veri kaynağı için boyut üst sınırı şu şekildedir:

| S.No | İşletim sistemi | En büyük veri kaynağı boyutu |
|:---:|:--- |:--- |
| 1 |Windows Server 2012 veya üzeri |54.400 GB |
| 2 |Windows 8 veya üzeri |54.400 GB |
| 3 |Windows Server 2008, Windows Server 2008 R2 |1700 GB |
| 4 |Windows 7 |1700 GB |

Aşağıdaki tabloda, her bir veri kaynağı boyutunun nasıl belirlendiği açıklanmaktadır.

| Veri kaynağı | Ayrıntılar |
|:---:|:--- |
| Birim |Bir sunucu veya istemci makinenin tek bir biriminden yedeklenmekte olan verilerin miktarı |
| Hyper-V sanal makine |Yedeklenmekte olan sanal makinenin tüm VHD'lerine ait verilerin toplamı |
| Microsoft SQL Server veritabanı |Yedeklenmekte olan tek bir SQL veritabanının boyutu |
| Microsoft SharePoint |Yedeklenmekte olan bir SharePoint grubu içinde içerik ve yapılandırma veritabanlarının toplamı |
| Microsoft Exchange |Yedeklenmekte olan bir Exchange sunucusundaki tüm Exchange veritabanlarının toplamı |
| BMR/Sistem Durumu |Yedeklenmekte olan makinenin BMR'sinin veya sistem durumunun her ayrı kopyası |

Azure Iaas VM yedekleme için her sanal makine 32 adede kadar veri diskleri olabilir ve her veri diski 4095 GB'a kadar olabilir.

### <a name="is-there-a-limit-on-the-amount-of-data-held-in-a-recovery-services-vault"></a>Bir kurtarma Hizmetleri kasasında tutulan veri miktarına bir sınır var mıdır?
Geri kadar bir kurtarma Hizmetleri kasası veri miktarına bir sınır yoktur.

## <a name="retention-policy-and-recovery-points"></a>Bekletme ilkesi ve kurtarma noktaları
### <a name="is-there-a-difference-between-the-retention-policy-for-dpm-and-windows-serverclient-that-is-on-windows-server-without-dpmbr"></a>DPM ve Windows Server'a/istemcisine yönelik bekletme ilkesi (DPM olmadan Windows Server'da) arasında bir fark var mıdır?<br/>
Hayır, hem DPM hem de Windows Server/istemcisi günlük, haftalık, aylık ve yıllık bekletme ilkelerine sahiptir.

### <a name="can-i-configure-my-retention-policies-selectively--that-is-configure-weekly-and-daily-but-not-yearly-and-monthlybr"></a>Miyim my bekletme ilkeleri ilkelerimi yani yapılandırma, haftalık ve günlük ancak yıllık ve aylık?<br/>
Evet, Azure Backup bekletme yapısı gereksinimlerinizi karşılayan bekletme ilkesini tanımlama konusunda tam esnekliğe sahip olmanızı sağlar.

### <a name="can-i-schedule-a-backup-at-6pm-and-specify-retention-policies-at-a-different-timebr"></a>Saat 18:00 için “bir yedekleme zamanlayıp” farklı bir saat için de "bekletme ilkeleri" belirtebilir miyim?<br/>
Hayır. Bekletme ilkeleri, yalnızca yedekleme noktalarında uygulanabilir. Aşağıdaki görüntüde, bekletme ilkesi 00:00 ve 18:00'de alınan yedeklemeler için belirtilmiştir. <br/>

![Yedekleme ve Bekletmeyi Zamanlama](./media/backup-azure-backup-faq/Schedule.png)
<br/>

### <a name="if-a-backup-is-retained-for-a-long-duration-does-it-take-more-time-to-recover-an-older-data-point-br"></a>Yedekleme uzun süre tutulursa daha eski bir veri noktasının kurtarılması daha uzun mu sürer? <br/>
Hayır, en eski veya en yeni noktanın kurtarılması için gereken süre aynıdır. Her kurtarma noktası bir tam nokta gibi davranır.

### <a name="if-each-recovery-point-is-like-a-full-point-does-it-impact-the-total-billable-backup-storagebr"></a>Her kurtarma noktası bir tam nokta gibiyse bu durum toplam faturalanabilir yedekleme alanını etkiler mi?<br/>
Genel uzun vadeli bekletme noktası ürünleri, yedekleme verilerini tam noktalar olarak depolar. Tam noktalar depolama açısından *verimsizdir* ancak daha kolay ve hızlı şekilde geri yüklenir. Artımlı kopyalar depolama açısından *verimlidir* ancak kurtarma süresini etkileyecek şekilde bir veri zincirini geri yüklemenizi gerektirir. Azure Backup alanı mimarisi, hızlı geri yükleme özelliğiyle optimum veri depolama olanağı sunarken aynı zamanda düşük depolama maliyetleri oluşturarak iki açıdan da avantaj sağlar. Bu veri depolama yaklaşımı, giriş ve çıkış bant genişliğinizin verimli şekilde kullanılmasını sağlar. Veri depolama alanı miktarı ve verileri kurtarmak için gereken süre minimum düzeyde tutulur. [Artımlı yedeklemenin](https://azure.microsoft.com/blog/microsoft-azure-backup-save-on-long-term-storage/) ne kadar verimli olduğu hakkında daha fazla bilgi edinin.

### <a name="is-there-a-limit-on-the-number-of-recovery-points-that-can-be-createdbr"></a>Oluşturulabilecek kurtarma noktalarının sayısına yönelik bir sınır var mıdır?<br/>
Korumalı bir örnek için en çok 9999 kurtarma noktası oluşturabilirsiniz. Korumalı örnek, verileri Azure’a yedeklemek için yapılandırılmış bir bilgisayar, sunucu (fiziksel veya sanal) veya iş yüküdür. Daha fazla bilgi için [Yedekleme ve elde tutma](./backup-introduction-to-azure-backup.md#backup-and-retention) ile [Korunan örnek nedir](./backup-introduction-to-azure-backup.md#what-is-a-protected-instance)? konularındaki açıklamalara bakın

### <a name="how-many-recoveries-can-i-perform-on-the-data-that-is-backed-up-to-azurebr"></a>Azure'a yedeklenen veriler üzerinde kaç kurtarma işlemi yapabilirim?<br/>
Azure Backup ile gerçekleştirilen kurtarma işlemlerinin sayısına yönelik bir sınır yoktur.

### <a name="when-restoring-data-do-i-pay-for-the-egress-traffic-from-azure-br"></a>Verileri geri yüklerken Azure'dan çıkış trafiği için ödeme yapacak mıyım? <br/>
Hayır. Kurtarma işlemleriniz ücretsizdir ve çıkış trafiği için sizden ücret tahsil edilmez.

### <a name="what-happens-when-i-change-my-backup-policy"></a>My yedekleme ilkesini değiştirdiğimde ne olur?
Yeni bir ilke uygulandığında yeni ilkenin zamanlama ve ardından. Bekletme süresi uzatıldıysa, yeni ilkeye göre tutulması için mevcut kurtarma noktaları işaretlenir. Bekletme süresi kısaltıldıysa, bunlar sonraki temizleme işleminde kesilmek üzere işaretlenir ve sonra silinir.

## <a name="azure-backup-encryption"></a>Azure Backup şifrelemesi
### <a name="is-the-data-sent-to-azure-encrypted-br"></a>Veriler Azure'a şifreli olarak mı gönderilir? <br/>
Evet. Veriler AES256 kullanılarak şirket içi sunucu/istemci/SCDPM makinesi üzerinde şifrelenir ve güvenli bir HTTPS bağlantısı üzerinden gönderilir.

### <a name="is-the-backup-data-on-azure-encrypted-as-wellbr"></a>Azure üzerindeki yedekleme verileri de şifreli midir?<br/>
Evet. Azure'a gönderilen veriler (bekleyen) şifreli olarak kalır. Microsoft herhangi bir noktada yedekleme verilerinin şifresini çözmez. Azure Backup bir Azure VM’yi yedeklerken sanal makinenin şifrelemesini kullanır. Örneğin, VM’niz Azure Disk Şifrelemesi veya başka bir şifreleme teknolojisi kullanılarak şifrelendiyse, Azure Backup verilerinizi korumak için bu şifrelemeyi kullanır.

### <a name="what-is-the-minimum-length-of-encryption-key-used-to-encrypt-backup-data-br"></a>Yedekleme verilerini şifrelemek için kullanılan şifreleme anahtarının minimum uzunluğu nedir? <br/>
Azure Backup aracısını kullanırken şifreleme anahtarı en az 16 karakter olmalıdır. Azure VM'lerde, Azure KeyVault tarafından kullanılan anahtarlar için boyut sınırlaması yoktur.

### <a name="what-happens-if-i-misplace-the-encryption-key-can-i-recover-the-data-or-can-microsoft-recover-the-data-br"></a>Şifreleme anahtarını kaybedersem ne olur? Verileri kurtarabilir miyim?/Microsoft, verileri kurtarabilir mi? <br/>
Yedekleme verilerini şifrelemek için kullanılan anahtar yalnızca müşterinin şirketinde bulunur. Microsoft, Azure üzerinde anahtarın bir kopyasını tutmaz ve anahtara erişim sahibi değildir. Müşteri, anahtarı kaybederse Microsoft, yedekleme verilerini kurtaramaz.
