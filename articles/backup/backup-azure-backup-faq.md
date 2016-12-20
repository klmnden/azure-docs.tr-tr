---
title: Azure Backup ile ilgili SSS | Microsoft Belgeleri
description: "Yedekleme hizmeti, yedekleme aracısı, yedekleme ve bekletme, kurtarma, güvenlik ve yedekleme ile olağanüstü durum kurtarma ile ilgili diğer sık sorulan soruların yanıtları."
services: backup
documentationcenter: 
author: markgalioto
manager: jwhit
editor: 
keywords: "yedekleme ve olağanüstü durum kurtarma; backup hizmeti"
ms.assetid: 1011bdd6-7a64-434f-abd7-2783436668d7
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/16/2016
ms.author: trinadhk; giridham; arunak; markgal; jimpark;
translationtype: Human Translation
ms.sourcegitcommit: be06f1eca1848ff6d00661cfc1166797649a98a4
ms.openlocfilehash: cb45e7113073d19c1dc3e305d7b69373bd38d84f


---
# <a name="azure-backup-service--faq"></a>Azure Backup hizmeti - SSS
Bu makale, Azure Backup hizmeti ile ilgili sık sorulan soruların (ve yanıtlarının) listesinden oluşmaktadır. Topluluğumuz, soruları hızlı bir şekilde yanıtlar ve bir sorunun sıklıkla sorulması durumunda söz konusu soruyu bu makaleye ekleriz. Soruların yanıtları genellikle başvuru veya destek bilgileri sağlar. Azure Backup ile ilgili sorularınızı, bu makalenin veya ilgili bir makalenin Disqus bölümünde sorabilirsiniz. Ayrıca Azure Backup hizmeti ile ilgili sorularınızı [tartışma forumunda](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) paylaşabilirsiniz.

## <a name="what-is-the-list-of-supported-operating-systems-from-which-i-can-back-up-to-azure-using-azure-backup-br"></a>Azure Backup hizmetini kullanarak Azure'a yedekleme yapabileceğim desteklenen işletim sistemlerinin listesi ne şekildedir? <br/>
Azure Backup, Azure Backup Sunucusu ve SCDPM kullanılarak korunan dosya ve klasörlerin yanı sıra iş yükü uygulamalarının yedeklenmesi için aşağıdaki listede yer alan işletim sistemlerini destekler.

| İşletim Sistemi | Platform | SKU |
|:--- | --- |:--- |
| Windows 8 ve en son SP'ler |64 bit |Enterprise, Pro |
| Windows 7 ve en son SP'ler |64 bit |Ultimate, Enterprise, Professional, Home Premium, Home Basic, Starter |
| Windows 8.1 ve en son SP'ler |64 bit |Enterprise, Pro |
| Windows 10 |64 bit |Enterprise, Pro, Home |
| Windows Server 2012 R2 ve en son SP'ler |64 bit |Standard, Datacenter, Foundation |
| Windows Server 2012 ve en son SP'ler |64 bit |Datacenter, Foundation, Standard |
| Windows Storage Server 2012 R2 ve en son SP'ler |64 bit |Standard, Workgroup |
| Windows Storage Server 2012 ve en son SP'ler |64 bit |Standard, Workgroup |
| Windows Server 2012 R2 ve en son SP'ler |64 bit |Essential |
| Windows Server 2008 R2 SP1 |64 bit |Standard, Enterprise, Datacenter, Foundation |
| Windows Server 2008 SP2 |64 bit |Standard, Enterprise, Datacenter, Foundation |

Azure VM yedeklemesi için,

* **Linux**: Azure Backup, [Azure tarafından onaylanan bir dağıtım listesini](../virtual-machines/virtual-machines-linux-endorsed-distros.md) (CoreOS Linux hariç) destekler.  Sanal makinede VM aracısı kullanılabilir olduğu ve Python desteği bulunduğu sürece diğer Kendi Linux’unu Getir dağıtımları da çalışabilir.
* **Windows Server**:  Windows Server 2008 R2’den eski sürümler desteklenmez.

## <a name="where-can-i-download-the-latest-azure-backup-agent-br"></a>En son Azure Backup aracısını nereden indirebilirim? <br/>
Windows Server, System Center DPM veya Windows istemcisini yedeklemeye yönelik en son aracıyı [buradan](http://aka.ms/azurebackup_agent) indirebilirsiniz. Bir sanal makineyi yedeklemek istiyorsanız VM Aracısı'nı (otomatik olarak uygun uzantıyı yükler) kullanın. VM Aracısı, Azure galerisinden oluşturulan sanal makineler üzerinde zaten mevcuttur.

## <a name="which-version-of-scdpm-server-is-supported-br"></a>SCDPM sunucusunun hangi sürümü desteklenir? <br/>
[En son](http://aka.ms/azurebackup_agent) Azure Backup aracısını, SCDPM'nin (Ağustos 2016'dan itibaren UR11) en son güncelleştirme paketi üzerine yüklemeniz önerilir

## <a name="when-configuring-the-azure-backup-agent-i-am-prompted-to-enter-the-vault-credentials-do-vault-credentials-expire"></a>Azure Backup aracısını yapılandırırken kasa kimlik bilgilerini girmem isteniyor. Kasa kimlik bilgilerinin süresi dolar mı?
Evet, kasa kimlik bilgilerinin süresi 48 saat sonra dolar. Dosyanın süresi dolarsa Azure portalında oturum açın ve kasa kimlik bilgileri dosyalarını kasanızdan indirin.

## <a name="is-there-any-limit-on-the-number-of-vaults-that-can-be-created-in-each-azure-subscription-br"></a>Her bir Azure aboneliği için oluşturulan kasaların sayısına yönelik herhangi bir sınır var mıdır? <br/>
Evet. Eylül 2016’den itibaren abonelik başına 25 yedekleme kasası oluşturabilirsiniz. Her abonelikte, Azure Backup hizmetinin desteklenen her bir bölgesi için en fazla 25 Kurtarma Hizmetleri kasası oluşturabilirsiniz. Daha fazla kasaya ihtiyacınız varsa yeni bir abonelik oluşturun.

## <a name="are-there-any-limits-on-the-number-of-serversmachines-that-can-be-registered-against-each-vault-br"></a>Her bir kasa için kaydedilebilen sunucu/makine sayısına yönelik herhangi bir sınır var mıdır? <br/>
Evet, kasa başına en fazla 50 makine kaydedebilirsiniz. Azure IaaS sanal makineleri için sınır, kasa başına 200 VM'dir. Daha fazla makine kaydetmeniz gerekirse başka bir kasa oluşturun.

## <a name="how-do-i-register-my-server-to-another-datacenterbr"></a>Sunucumu başka bir veri merkezine nasıl kaydederim?<br/>
Yedekleme verileri, kasanın kayıtlı olduğu veri merkezine gönderilir. Veri merkezini değiştirmenin en kolay yolu, aracıyı kaldırmak ve aracıyı yeniden yükleyip istenilen veri merkezine ait yeni bir kasa kaydetmektir.

## <a name="what-happens-if-i-rename-a-windows-server-that-is-backing-up-data-to-azurebr"></a>Azure'a veri yedekleyen bir Windows sunucusunu yeniden adlandırırsam ne olur?<br/>
Bir sunucuyu yeniden adlandırdığınızda, geçerli olarak yapılandırılmış olan tüm yedeklemeler durdurulur.
Sunucunun yeni adını Backup kasasına kaydedin. Yeni adı kasaya kaydettiğinizde, ilk yedekleme işlemi *tam* yedekleme olur. Daha önce eski sunucu adıyla kasaya yedeklenen verileri kurtarmanız gerekiyorsa **Veri Kurtarma** sihirbazındaki [**Başka bir sunucu**](backup-azure-restore-windows-server.md#recover-to-an-alternate-machine) seçeneğini kullanarak verileri kurtarabilirsiniz.

## <a name="what-types-of-drives-can-i-backup-files-and-folders-from-br"></a>Ne tür sürücülerden dosya ve klasör yedekleyebilirim? <br/>
Aşağıdaki sürücüler/birimler yedeklenemez:

* Çıkarılabilir Medya: Sürücünün yedekleme öğesi kaynağı olarak kullanılabilmesi için sabit olarak bildirilmesi gerekir.
* Salt Okunur Birimler: Birimin çalışması için birim gölge kopyası hizmetine (VSS) yönelik olarak yazılabilir olması gerekir.
* Çevrimdışı Birimler: Birimin çalışması için VSS'ye yönelik olarak çevrimiçi olması gerekir.
* Ağ paylaşımı: Birimin çevrimiçi yedekleme kullanılarak yedeklenebilmesi için sunucuya yönelik olarak yerel olması gerekir.
* Bitlocker korumalı birimler: Yedeklemenin gerçekleşebilmesi için birimin kilidinin açık olması gerekir.
* Dosya Sistemi Tanımlaması: NTFS, çevrimiçi yedekleme hizmetinin bu sürümü için desteklenen tek dosya sistemidir.

## <a name="what-file-and-folder-types-can-i-back-up-from-my-serverbr"></a>Sunucumdan hangi dosya ve klasör hangi türlerini yedekleyebilirim?<br/>
Aşağıdaki türler desteklenir:

* Şifreli
* Sıkıştırılmış
* Seyrek
* Sıkıştırılmış + Seyrek
* Sabit Bağlantılar: Desteklenmez, atlanır
* Yeniden Ayrıştırma Noktası: Desteklenmez, atlanır
* Şifreli + Sıkıştırılmış: Desteklenmez, atlanır
* Şifreli + Seyrek: Desteklenmez, atlanır
* Sıkıştırılmış Akış: Desteklenmez, atlanır
* Seyrek Akış: Desteklenmez, atlanır

## <a name="whats-the-minimum-size-requirement-for-the-cache-folder-br"></a>Önbellek klasörü için minimum boyut gereksinimini nedir? <br/>
Önbellek klasörünün boyutu, yedeklediğiniz veri miktarını belirler. Önbellek klasörü, veri depolama için gerekli olan alanın % 5'ini oluşturmalıdır.

## <a name="if-my-organization-has-one-vault-how-can-i-isolate-one-servers-data-from-another-server-when-restoring-databr"></a>Kuruluşumun bir kasası varsa veri geri yükleme sırasında bir sunucunun verilerini başka bir sunucudan nasıl ayırabilirim?<br/>
Aynı kasaya kayıtlı tüm sunucular, *aynı parolayı kullanan* diğer sunucular tarafından yedeklenen verileri kurtarabilir. Yedekleme verilerini kuruluşunuzdaki diğer sunucularından ayırmak istediğiniz sunucularınız varsa bu sunucular için belirlenmiş bir parola kullanın. Örneğin, insan kaynakları sunucuları bir şifreleme parolası kullanırken, muhasebe sunucuları ve depolama sunucuları farklı birer şifreleme parolası kullanabilir.

## <a name="can-i-migrate-my-backup-data-or-vault-between-subscriptions-br"></a>Yedekleme verilerimin veya kasamın abonelikler arasında "geçişini" sağlayabilir miyim? <br/>
Hayır. Kasa abonelik düzeyinde oluşturulur ve oluşturulduktan sonra başka bir aboneliğe yeniden atanamaz.

## <a name="does-the-azure-backup-agent-work-on-a-server-that-uses-windows-server-2012-deduplication-br"></a>Azure Backup Aracısı, Windows Server 2012 yinelenenleri kaldırma özelliğini kullanan bir sunucu üzerinde çalışır mı? <br/>
Evet. Aracı hizmeti, yedekleme işlemini hazırlarken yinelenenleri kaldırma işlemi uygulanmış verileri normal verilere dönüştürür. Ardından verileri yedekleme için en iyi duruma getirir, verileri şifreler ve daha sonra, şifreli verileri çevrimiçi yedekleme hizmetine gönderir.

## <a name="if-i-cancel-a-backup-job-once-it-has-started-is-the-transferred-backup-data-deleted-br"></a>Bir yedekleme işini başlatıldıktan sonra iptal edersem aktarılan yedekleme verileri silinir mi? <br/>
Hayır. İşlemin iptal edildiği andan önce kasaya aktarılan tüm veriler kasada kalır. Azure Backup, yedekleme işlemi sırasında yedekleme verilerine zaman zaman denetim noktaları eklemek üzere bir denetim noktası mekanizması kullanır. Yedekleme verilerinde denetim noktaları bulunduğundan, sonraki yedekleme işlemi dosyaların bütünlüğünü doğrulayabilir. Bir sonraki yedekleme işi, daha önce yedeklenen verilerin üzerine artımlı olarak gerçekleşir. Artımlı yedekleme işlemlerinin yalnızca yeni veya değiştirilmiş verileri aktarması, bant genişliğinin daha iyi kullanılması anlamına gelir.

Bir Azure VM’ye yönelik bir yedekleme işini iptal ederseniz aktarılan tüm veriler yoksayılır. Bir sonraki yedekleme işi, son başarılı yedekleme işinden artımlı verileri aktarır.

## <a name="why-am-i-seeing-the-warning-azure-backups-have-not-been-configured-for-this-server-even-though-i-had-scheduled-regular-backups-previously-br"></a>Daha önce düzenli yedeklemeler zamanlamış olmama karşın neden "Azure Yedeklemeleri bu sunucu için yapılandırılmamış" uyarısını görüyorum? <br/>
Bu uyarı, yerel sunucuda depolanan yedekleme zamanlaması ayarları, yedekleme kasasında depolanan ayarlarla aynı olmadığında oluşur. Sunucu ya da ayarlar bilinen bir iyi duruma getirilerek kurtarıldığında, yedekleme zamanlamaları eşitlemesini kaybedebilir. Bu uyarıyı alırsanız [yedekleme ilkesini yeniden yapılandırın](backup-azure-manage-windows-server.md) ve ardından yerel sunucuyu Azure ile yeniden eşitlemek için **Run Back Up Now (Yedeklemeyi Şimdi Çalıştır)** işlemini uygulayın.

## <a name="what-firewall-rules-should-be-configured-for-azure-backup-br"></a>Azure Backup için hangi güvenlik duvarı kuralları yapılandırılmalıdır? <br/>
Verilerin şirket içinden Azure'a ve iş yükünden Azure'a sorunsuz şekilde korunması için güvenlik duvarınızın aşağıdaki URL'ler ile iletişim kurmasına izin vermeniz önerilir:

* www.msftncsi.com
* \*.Microsoft.com
* \*.WindowsAzure.com
* \*.microsoftonline.com
* \*.windows.net

## <a name="can-i-install-the-azure-backup-agent-on-an-azure-vm-already-backed-by-the-azure-backup-service-using-the-vm-extension-br"></a>Azure Backup aracısını, önceden VM uzantısı kullanılarak Azure Backup hizmeti tarafından yedeklenmiş olan bir Azure VM üzerine yükleyebilir miyim? <br/>
Kesinlikle. Azure Backup, VM uzantısını kullanan Azure VM'ler için VM düzeyinde yedekleme sağlar. Konuk Windows işletim sistemi üzerindeki dosya ve klasörleri korumak için Azure Backup aracısını bu konuk işletim sistemine yükleyin.

## <a name="can-i-install-the-azure-backup-agent-on-an-azure-vm-to-back-up-files-and-folders-present-on-temporary-storage-provided-by-the-azure-vm-br"></a>Azure Backup aracısını bir Azure VM'ye yükleyerek mevcut dosya ve klasörleri Azure VM tarafından sağlanan geçici depolama alanına yedekleyebilir miyim? <br/>
Evet. Azure Backup aracısını Konuk Windows işletim sistemine yükleyin ve dosya ve klasörleri geçici depolama alanına yedekleyin. Ancak, geçici depolama verileri silindikten sonra yedeklemelerin başarısız olacağını unutmayın. Ayrıca, geçici depolama verilerinin silinmiş olması durumunda, yalnızca geçici olmayan depolama alanına geri yükleme gerçekleştirebilirsiniz.

## <a name="i-have-installed-azure-backup-agent-to-protect-my-files-and-folders-can-i-now-install-scdpm-to-work-with-azure-backup-agent-to-protect-on-premises-applicationvm-workloads-to-azure-br"></a>Dosya ve klasörlerimi korumak için Azure Backup aracısını yükledim. Artık Azure'a yönelik şirket içi uygulama/VM iş yüklerini korumak için Azure Backup aracısıyla çalışmak üzere SCDPM'yi yükleyebilir miyim? <br/>
Azure Backup’ı System Center Data Protection Manager (DPM) ile birlikte kullanmak için önce DPM’yi, ardından Azure Backup aracısını yükleyin. Azure Backup bileşenlerinin bu sırada yüklenmesi, Azure Backup aracısının DPM ile çalışmasını sağlar. Azure Backup aracısının DPM yüklenmeden önce yüklenmesi önerilmez veya desteklenmez.

## <a name="what-is-the-length-of-file-path-that-can-be-specified-as-part-of-azure-backup-policy-using-azure-backup-agent-br"></a>Azure Backup aracısını kullanan Azure Yedekleme ilkesinin bir bölümü olarak belirtilebilecek dosya yolunun uzunluğu nedir? <br/>
Azure Backup aracısı NTFS kullanır. [Dosya yolu uzunluğu belirtimi, Windows API ile sınırlıdır](https://msdn.microsoft.com/library/aa365247.aspx#fully_qualified_vs._relative_paths). Windows API tarafından izin verilenden daha uzun dosya yollarına sahip dosyaları yedeklerken, yedekleme dosyalarının üst klasörünü veya disk sürücüsünü yedeklemeyi seçebilirsiniz.  

## <a name="what-characters-are-allowed-in-file-path-of-azure-backup-policy-using-azure-backup-agent-br"></a>Azure Backup aracısını kullanan Azure Yedekleme ilkesinin dosya yolunda hangi karakterlere izin verilir? <br>
 Azure Backup aracısı NTFS kullanır. [NTFS destekli karakterleri](https://msdn.microsoft.com/library/aa365247.aspx#naming_conventions) dosya belirtiminin bir parçası olarak etkinleştirir.  

## <a name="can-i-use-azure-backup-server-to-create-a-bare-metal-recovery-bmr-backup-for-a-physical-server-br"></a>Bir fiziksel sunucu için Tam Kurtarma (BMR) yedeklemesi oluşturmak üzere Azure Backup Sunucusu'nu kullanabilir miyim? <br/>
Evet.

## <a name="can-i-configure-the-backup-service-to-send-mail-if-a-backup-job-fails-br"></a>Bir yedekleme işi başarısız olursa posta göndermek için Backup hizmetini yapılandırabilir miyim? <br/>
Evet, Backup hizmeti bir PowerShell betiği ile kullanılabilen çeşitli olay tabanlı uyarılara sahiptir. Tam açıklama için bkz. [Bildirimleri yapılandırma](backup-azure-monitor-vms.md#configure-notifications)

## <a name="is-there-a-limit-on-the-size-of-each-data-source-being-backed-up-br"></a>Yedeklenmekte olan her veri kaynağının boyutuna yönelik bir sınır var mıdır? <br/>
Bir kasaya yedekleyebileceğiniz veri miktarı konusunda sınır yoktur. Azure Backup, veri kaynağı için en fazla boyut kısıtlaması uygular ancak limitler oldukça yüksektir. Ağustos 2015 itibarıyla, desteklenen işletim sistemlerinde veri kaynağı için boyut üst sınırı şu şekildedir:

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

## <a name="are-there-limits-on-the-number-of-times-a-backup-job-can-be-scheduled-per-daybr"></a>Bir yedekleme işinin günde kaç kez zamanlanabileceğine yönelik sınırlar var mıdır?<br/>
Evet, yedekleme işlerini Windows Server veya Windows istemcisi üzerinde günde en fazla üç kez çalıştırabilirsiniz. Yedekleme işlerini System Center DPM üzerinde günde en fazla iki kez çalıştırabilirsiniz. Bir yedekleme işini IaaS VM'ler için günde bir kez çalıştırabilirsiniz.

## <a name="is-there-a-difference-between-the-scheduling-policy-for-dpm-and-windows-server-ie-on-windows-server-without-dpm-br"></a>DPM ve Windows Server'a yönelik zamanlama ilkesi (DPM olmadan Windows Server'da) arasında bir fark var mıdır? <br/>
Evet. DPM'yi kullanarak, günlük, haftalık, aylık ve yıllık zamanlamalar belirtebilirsiniz. Windows Server (DPM olmadan) yalnızca günlük ve haftalık zamanlamalar belirtmenize olanak tanır.

## <a name="is-there-a-difference-between-the-retention-policy-for-dpm-and-windows-serverclient-ie-on-windows-server-without-dpmbr"></a>DPM ve Windows Server'a/istemcisine yönelik bekletme ilkesi (DPM olmadan Windows Server'da) arasında bir fark var mıdır?<br/>
Hayır, hem DPM hem de Windows Server/istemcisi günlük, haftalık, aylık ve yıllık bekletme ilkelerine sahiptir.

## <a name="can-i-configure-my-retention-policies-selectively-ie-configure-weekly-and-daily-but-not-yearly-and-monthlybr"></a>Bekletme ilkelerimi seçerek yapılandırabilir miyim? Başka bir deyişle, haftalık ve günlük yapılandırmaya olanak tanırken yıllık ve aylık yapılandırmayı engelleyebilir miyim?<br/>
Evet, Azure Backup bekletme yapısı gereksinimlerinizi karşılayan bekletme ilkesini tanımlama konusunda tam esnekliğe sahip olmanızı sağlar.

## <a name="can-i-schedule-a-backup-at-6pm-and-specify-retention-policies-at-a-different-timebr"></a>Saat 18:00 için "bir yedekleme zamanlayıp" farklı bir saat için de "bekletme ilkeleri" belirtebilir miyim?<br/>
Hayır. Bekletme ilkeleri, yalnızca yedekleme noktalarında uygulanabilir. Aşağıdaki görüntüde, bekletme ilkesi 00:00 ve 18:00'de alınan yedeklemeler için belirtilmiştir. <br/>

![Yedekleme ve Bekletmeyi Zamanlama](./media/backup-azure-backup-faq/Schedule.png)
<br/>

## <a name="is-an-incremental-copy-transferred-for-the-retention-policies-scheduled-br"></a>Zamanlanmış bekletme ilkeleri için bir artımlı kopya aktarılır mı? <br/>
Hayır, artımlı kopya, yedekleme zamanlaması sayfasında belirtilen süreye göre gönderilir. Bekletilebilen noktalar, bekletme ilkesi temel alınarak belirlenir.

## <a name="if-a-backup-is-retained-for-a-long-duration-does-it-take-more-time-to-recover-an-older-data-point-br"></a>Yedekleme uzun süre tutulursa daha eski bir veri noktasının kurtarılması daha uzun mu sürer? <br/>
 Hayır, en eski veya en yeni noktanın kurtarılması için gereken süre aynıdır. Her kurtarma noktası bir tam nokta gibi davranır.

## <a name="if-each-recovery-point-is-like-a-full-point-does-it-impact-the-total-billable-backup-storagebr"></a>Her kurtarma noktası bir tam nokta gibiyse bu durum toplam faturalanabilir yedekleme alanını etkiler mi?<br/>
Genel uzun vadeli bekletme noktası ürünleri, yedekleme verilerini tam noktalar olarak depolar. Tam noktalar depolama açısından *verimsizdir* ancak daha kolay ve hızlı şekilde geri yüklenir. Artımlı kopyalar depolama açısından *verimlidir* ancak kurtarma süresini etkileyecek şekilde bir veri zincirini geri yüklemenizi gerektirir. Azure Backup alanı mimarisi, hızlı geri yükleme özelliğiyle optimum veri depolama olanağı sunarken aynı zamanda düşük depolama maliyetleri oluşturarak iki açıdan da avantaj sağlar. Bu veri depolama yaklaşımı, giriş ve çıkış bant genişliğinizin verimli şekilde kullanılmasını sağlar. Veri depolama alanı miktarı ve verileri kurtarmak için gereken süre minimum düzeyde tutulur. [Artımlı yedekleme](https://azure.microsoft.com/blog/microsoft-azure-backup-save-on-long-term-storage/) tasarrufunun ne kadar verimli olduğu hakkında daha fazla bilgi edinin.

## <a name="is-there-a-limit-on-the-number-of-recovery-points-that-can-be-createdbr"></a>Oluşturulabilecek kurtarma noktalarının sayısına yönelik bir sınır var mıdır?<br/>
Hayır. Kurtarma noktalarına yönelik sınırları kaldırdık. İstediğiniz sayıda kurtarma noktası oluşturabilirsiniz.

## <a name="why-is-the-amount-of-data-transferred-in-backup-not-equal-to-the-amount-of-data-i-backed-upbr"></a>Yedeklemede aktarılan veri miktarı neden yedeklenen veri miktarına eşit değil?<br/>
 Azure Backup Aracısı veya SCDPM ya da Azure Backup Sunucusundan yedeklenen tüm veriler aktarılmadan önce sıkıştırılır ve şifrelenir. Sıkıştırma ve şifreleme uygulandıktan sonra yedekleme kasasındaki veriler % 30-40 daha küçük hale gelir.

## <a name="is-there-a-way-to-adjust-the-amount-of-bandwidth-used-by-the-backup-servicebr"></a>Backup hizmeti tarafından kullanılan bant genişliği miktarını ayarlamanın bir yolu var mıdır?<br/>
 Evet, bant genişliğini ayarlamak için Backup Aracısı'ndaki **Özellikleri Değiştir** seçeneğini kullanın. Bant genişliği miktarını ve bu bant genişliğini kullanma zamanlarınızı ayarlayabilirsiniz. Adım adım yönergeler için, “Resource Manager dağıtım modelini kullanarak Windows Server’ı veya bir istemciyi Azure’a yedekleme” (“Back up a Windows Server or client to Azure using the Resource Manager deployment model”) makalesindeki **[Ağ kapasitesi azaltmayı etkinleştirme (Enable network throttling)](backup-configure-vault.md#enable-network-throttling)** bölümüne bakın.

## <a name="my-internet-bandwidth-is-limited-for-the-amount-of-data-i-need-to-back-up-is-there-a-way-i-can-move-data-to-a-certain-location-with-a-large-network-pipe-and-push-that-data-into-azure-br"></a>İnternet bant genişliğim, yedeklemem gereken veri miktarı için sınırlı durumda. Verileri büyük bir ağ kanalı ile belirli bir konuma taşıyıp bu verileri Azure'a gönderebilmemin bir yolu var mıdır? <br/>
Standart çevrimiçi yedekleme işlemini kullanarak verileri Azure'a yedekleyebilir veya verileri Azure'da blob depolamaya aktarmak için Azure İçeri/Dışarı Aktarma hizmetini kullanabilirsiniz. Yedekleme verilerini Azure depolama alanına almanın başka bir yolu yoktur. Azure İçeri/Dışarı Aktarma hizmetini Azure Backup ile kullanma hakkında bilgi için lütfen [Çevrimdışı Yedekleme iş akışı](backup-azure-backup-import-export.md) makalesine bakın.

## <a name="how-many-recoveries-can-i-perform-on-the-data-that-is-backed-up-to-azurebr"></a>Azure'a yedeklenen veriler üzerinde kaç kurtarma işlemi yapabilirim?<br/>
Azure Backup ile gerçekleştirilen kurtarma işlemlerinin sayısına yönelik bir sınır yoktur.

## <a name="do-i-have-to-pay-for-the-egress-traffic-from-azure-data-center-during-recoveriesbr"></a>Kurtarma sırasında Azure veri merkezi çıkış trafiği için ödeme yapmam gerekir mi?<br/>
 Hayır. Kurtarma işlemleriniz ücretsizdir ve çıkış trafiği için sizden ücret tahsil edilmez.

## <a name="is-the-data-sent-to-azure-encrypted-br"></a>Veriler Azure'a şifreli olarak mı gönderilir? <br/>
Evet. Veriler AES256 kullanılarak şirket içi sunucu/istemci/SCDPM makinesi üzerinde şifrelenir ve güvenli bir HTTPS bağlantısı üzerinden gönderilir.

## <a name="is-the-backup-data-on-azure-encrypted-as-wellbr"></a>Azure üzerindeki yedekleme verileri de şifreli midir?<br/>
 Evet. Azure'a gönderilen veriler (bekleyen) şifreli olarak kalır. Microsoft herhangi bir noktada yedekleme verilerinin şifresini çözmez. Azure Backup bir Azure VM’yi yedeklerken sanal makinenin şifrelemesini kullanır. Örneğin, VM’niz Azure Disk Şifrelemesi veya başka bir şifreleme teknolojisi kullanılarak şifrelendiyse, Azure Backup verilerinizi korumak için bu şifrelemeyi kullanır.

## <a name="what-is-the-minimum-length-of-encryption-key-used-to-encrypt-backup-data-br"></a>Yedekleme verilerini şifrelemek için kullanılan şifreleme anahtarının minimum uzunluğu nedir? <br/>
 Şifreleme anahtarı en az 16 karakterden oluşmalıdır.

## <a name="what-happens-if-i-misplace-the-encryption-key-can-i-recover-the-data-or-can-microsoft-recover-the-data-br"></a>Şifreleme anahtarını kaybedersem ne olur? Verileri kurtarabilir miyim?/Microsoft, verileri kurtarabilir mi? <br/>
Yedekleme verilerini şifrelemek için kullanılan anahtar yalnızca müşterinin şirketinde bulunur. Microsoft, Azure üzerinde anahtarın bir kopyasını tutmaz ve anahtara erişim sahibi değildir. Müşteri, anahtarı kaybederse Microsoft, yedekleme verilerini kurtaramaz.

## <a name="how-do-i-change-the-cache-location-specified-for-the-azure-backup-agentbr"></a>Azure Backup aracısı için belirtilen önbellek konumunu nasıl değiştiririm?<br/>
 Önbellek konumunu değiştirmek için aşağıdaki madde listesini sırasıyla izleyin.

* Yükseltilmiş komut isteminde aşağıdaki komutu çalıştırarak Backup altyapısını durdurun:

  ```PS C:\> Net stop obengine```
* Dosyaları taşımayın. Bunun yerine, önbellek alanı klasörünü yeterli alana sahip farklı bir sürücüye kopyalayın. Yedeklemelerin yeni önbellek alanı ile çalıştığı onaylandıktan sonra özgün önbellek alanı kaldırılabilir.
* Aşağıdaki kayıt defteri girdilerini yeni önbellek alanı klasörünün yolu ile güncelleştirin.<br/>

| Kayıt defteri yolu | Kayıt Defteri Anahtarı | Değer |
| --- | --- | --- |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` |ScratchLocation |*Yeni önbellek klasörü konumu* |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` |ScratchLocation |*Yeni önbellek klasörü konumu* |

* Yükseltilmiş komut isteminde aşağıdaki komutu çalıştırarak Backup altyapısını yeniden başlatın:

  ```PS C:\> Net start obengine```

  Yedekleme oluşturma yeni önbellek konumunda başarıyla tamamlandıktan sonra, özgün önbellek klasörünü kaldırabilirsiniz.

## <a name="where-can-i-put-the-cache-folder-for-the-azure-backup-agent-to-work-as-expectedbr"></a>Azure Backup Aracısı'nın beklendiği şekilde çalışması için önbellek klasörünü nereye koyabilirim?<br/>
Önbellek klasörü için aşağıdaki konumlar önerilmez:

* Ağ paylaşımı veya Çıkarılabilir Medya: Önbellek klasörü, çevrimiçi yedekleme kullanılarak yedeklenmesi gereken sunucu için yerel olmalıdır. Ağ konumlarını veya USB sürücüleri gibi çıkarılabilir medyalar desteklenmez.
* Çevrimdışı Birimler: Önbellek klasörü, Azure Backup Aracısı kullanılarak gerçekleştirilecek beklenen yedekleme için çevrimiçi olmalıdır.

## <a name="are-there-any-attributes-of-the-cache-folder-that-are-not-supportedbr"></a>Önbellek klasörünün desteklenmeyen herhangi bir özniteliği var mıdır?<br/>
 Aşağıdaki öznitelikler veya bunların bileşimleri, önbellek klasörü için desteklenmez:

* Şifreli
* Yinelenenleri kaldırma işlemi uygulanmış
* Sıkıştırılmış
* Seyrek
* Yeniden Ayrıştırma Noktası

Önbellek klasörü ve meta veri VHD’si, Azure Backup aracısı için gerekli özniteliklere sahip değildir.

## <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Kurtarma Hizmetleri kasaları Resource Manager tabanlıdır. Backup kasaları (Klasik mod) hala destekleniyor mu? <br/>
Evet, Yedekleme kasaları hâlâ destekleniyor. [Klasik portalda](https://manage.windowsazure.com) Backup kasaları oluşturun. [Azure portalında](https://portal.azure.com) Kurtarma Hizmetleri kasaları oluşturun. Ancak gelecekteki tüm geliştirmeler yalnızca Kurtarma Hizmetleri kasasında kullanılabileceği için, kurtarma hizmetleri kasası oluşturmanızı öneririz.

## <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Bir Backup kasasının Kurtarma Hizmetleri kasasına geçişini sağlayabilir miyim? <br/>
Ne yazık ki hayır, şu an için bir Backup kasasının içeriğinin Kurtarma Hizmetleri kasasına geçişini sağlayamazsınız. Bu işlevi eklemeye yönelik çalışmalarımız devam ediyor ancak işlev şu anda kullanılamıyor.

## <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Kurtarma Hizmetleri kasaları, klasik VM’leri mi Resource Manager tabanlı VM’leri mi destekler? <br/>
Kurtarma Hizmetleri kasaları iki modeli de destekler.  Klasik portalda oluşturulan bir klasik VM’yi ya da Azure portalında oluşturulan bir Resource Manager VM’sini bir Kurtarma Hizmetleri kasasına yedekleyebilirsiniz.

## <a name="i-have-backed-up-my-classic-vms-in-a-backup-vault-can-i-migrate-my-vms-from-classic-mode-to-resource-manager-mode-and-protect-them-in-a-recovery-services-vault"></a>Klasik VM’lerimi bir yedekleme kasasına yedekledim. VM’lerimi klasik moddan Resource Manager moduna geçirip bunları bir Kurtarma Hizmetleri kasasında koruyabilir miyim?
Bir VM’yi klasikten Resource Manager moduna taşıdığınızda yedekleme kasasındaki klasik VM kurtarma noktaları, Kurtarma Hizmetleri kasasına otomatik olarak geçirilmez. VM yedeklerinizi aktarmak için bu adımları izleyin:

1. Yedekleme kasasında **Korunan Öğeler** sekmesine gidin ve VM’yi seçin. [Korumayı Durdur](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines)’a tıklayın. *İlişkili yedekleme verilerini sil* seçeneğini **işaretlenmemiş** olarak bırakın.
2. Sanal makineyi, klasik moddan Resource Manager moduna geçirin. Sanal makine için karşılık gelen depolama ve ağın da Resource Manager moduna geçirildiğinden emin olun.
3. Bir Kurtarma Hizmetleri kasası oluşturun ve kasa panosunun üstündeki **Yedekle** eylemini kullanarak, geçirilen sanal makinede yedeklemeyi yapılandırın. Bir VM’yi Kurtarma Hizmetleri kasasına yedekleme hakkında ayrıntılı bilgi için [Azure VM’leri bir Kurtarma Hizmetleri kasasıyla koruma](backup-azure-vms-first-look-arm.md) başlıklı makaleye bakın.



<!--HONumber=Nov16_HO3-->


