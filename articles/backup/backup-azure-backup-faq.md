<properties
   pageTitle="Azure Backup ile ilgili SSS | Microsoft Azure"
   description="Yedekleme hizmeti, yedekleme aracısı, yedekleme ve bekletme, kurtarma, güvenlik ve yedekleme ile olağanüstü durum kurtarma ile ilgili diğer sık sorulan soruların yanıtları."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="yedekleme ve olağanüstü durum kurtarma; backup hizmeti"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="08/29/2016"
     ms.author="trinadhk; giridham; arunak; markgal; jimpark;"/>


# <a name="azure-backup-service--faq"></a>Azure Backup hizmeti - SSS

> [AZURE.SELECTOR]
- [Klasik mod için Backup ile ilgili SSS](backup-azure-backup-faq.md)
- [Resource Manager modu için Backup ile ilgili SSS](backup-azure-backup-ibiza-faq.md)

Bu makale, Azure Backup hizmeti ile ilgili sık sorulan soruların (ve yanıtlarının) listesinden oluşmaktadır. Topluluğumuz, soruları hızlı bir şekilde yanıtlar ve bir sorunun sıklıkla sorulması durumunda söz konusu soruyu bu makaleye ekleriz. Soruların yanıtları genellikle başvuru veya destek bilgileri sağlar. Azure Backup ile ilgili sorularınızı, bu makalenin veya ilgili bir makalenin Disqus bölümünde sorabilirsiniz. Ayrıca Azure Backup hizmeti ile ilgili sorularınızı [tartışma forumunda](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) paylaşabilirsiniz.


## <a name="what-is-the-list-of-supported-operating-systems-from-which-i-can-back-up-to-azure-using-azure-backup?"></a>Azure Backup hizmetini kullanarak Azure'a yedekleme yapabileceğim desteklenen işletim sistemlerinin listesi ne şekildedir? <br/>
Azure Backup, Azure Backup Sunucusu ve SCDPM kullanarak dosya-klasör yedekleme ve uygulama yedekleme için aşağıdaki listede bulunan işletim sistemlerini destekler. 

| İşletim Sistemi        | Platform           | SKU  |
| :------------- |-------------| :-----|
| Windows 8 ve en son SP'ler      | 64 bit | Enterprise, Pro |
| Windows 7 ve en son SP'ler      | 64 bit | Ultimate, Enterprise, Professional, Home Premium, Home Basic, Starter |
| Windows 8.1 ve en son SP'ler | 64 bit      |    Enterprise, Pro |
| Windows 10      | 64 bit | Enterprise, Pro, Home |
|Windows Server 2012 R2 ve en son SP'ler| 64 bit| Standard, Datacenter, Foundation|
|Windows Server 2012 ve en son SP'ler|    64 bit| Datacenter, Foundation, Standard|
|Windows Storage Server 2012 R2 ve en son SP'ler  |64 bit|    Standard, Workgroup|
|Windows Storage Server 2012 ve en son SP'ler |64 bit |Standard, Workgroup
|Windows Server 2012 R2 ve en son SP'ler  |64 bit|    Essential|
|Windows Server 2008 R2 SP1 |64 bit|    Standard, Enterprise, Datacenter, Foundation|
|Windows Server 2008 SP2    |64 bit|    Standard, Enterprise, Datacenter, Foundation|

Azure VM yedeklemesi için,

- **Linux**: Azure Backup, Core OS Linux hariç [Azure tarafından onaylanan bir dağıtım listesini](../virtual-machines/virtual-machines-linux-endorsed-distros.md) destekler.  Sanal makinede VM aracısı kullanılabilir olduğu ve Python desteği bulunduğu sürece diğer Kendi Linux’unu Getir dağıtımları da çalışabilir.
- **Windows Server**:  Windows Server 2008 R2’den eski sürümler desteklenmez.

## <a name="where-can-i-download-the-latest-azure-backup-agent?"></a>En son Azure Backup aracısını nereden indirebilirim? <br/>
Windows Server, System Center DPM veya Windows istemcisini yedeklemeye yönelik en son aracıyı [buradan](http://aka.ms/azurebackup_agent) indirebilirsiniz. Bir sanal makineyi yedeklemek istiyorsanız VM Aracısı'nı (otomatik olarak uygun uzantıyı yükler) kullanın. VM Aracısı, Azure galerisinden oluşturulan sanal makineler üzerinde zaten mevcuttur.

## <a name="which-version-of-scdpm-server-is-supported?"></a>SCDPM sunucusunun hangi sürümü desteklenir? <br/>
[En son](http://aka.ms/azurebackup_agent) Azure Backup aracısını, SCDPM'nin (Ağustos 2016'dan itibaren UR11) en son güncelleştirme paketi üzerine yüklemeniz önerilir

## <a name="when-configuring-the-azure-backup-agent,-i-am-prompted-to-enter-the-vault-credentials.-do-vault-credentials-expire?"></a>Azure Backup aracısını yapılandırırken kasa kimlik bilgilerini girmem isteniyor. Kasa kimlik bilgilerinin süresi dolar mı?
Evet, kasa kimlik bilgilerinin süresi 48 saat sonra dolar. Dosyanın süresi dolarsa Azure portalında oturum açın ve kasa kimlik bilgileri dosyalarını kasanızdan indirin. 

## <a name="is-there-any-limit-on-the-number-of-vaults-that-can-be-created-in-each-azure-subscription?"></a>Her bir Azure aboneliği için oluşturulan kasaların sayısına yönelik herhangi bir sınır var mıdır? <br/>
Evet. Eylül 2016’den itibaren abonelik başına 25 yedekleme kasası oluşturabilirsiniz. Her abonelikte, Azure Backup hizmetinin desteklenen her bir bölgesi için en fazla 25 Kurtarma Hizmetleri kasası oluşturabilirsiniz. Daha fazla kasaya ihtiyacınız varsa yeni bir abonelik oluşturun.

## <a name="are-there-any-limits-on-the-number-of-servers/machines-that-can-be-registered-against-each-vault?"></a>Her bir kasa için kaydedilebilen sunucu/makine sayısına yönelik herhangi bir sınır var mıdır? <br/>
Evet, kasa başına en fazla 50 makine kaydedebilirsiniz. Azure IaaS sanal makineleri için sınır, kasa başına 200 VM'dir. Daha fazla makine kaydetmeniz gerekirse yeni bir kasa oluşturun.

## <a name="how-do-i-register-my-server-to-another-datacenter?"></a>Sunucumu başka bir veri merkezine nasıl kaydederim?<br/>
Yedekleme verileri, kasanın kayıtlı olduğu veri merkezine gönderilir. Veri merkezini değiştirmenin en kolay yolu, aracıyı kaldırmak ve aracıyı yeniden yükleyip istenilen veri merkezine ait yeni bir kasa kaydetmektir.

## <a name="what-happens-if-i-rename-a-windows-server-that-is-backing-up-data-to-azure?"></a>Azure'a veri yedekleyen bir Windows sunucusunu yeniden adlandırırsam ne olur?<br/>
Bir sunucuyu yeniden adlandırdığınızda, geçerli olarak yapılandırılmış olan tüm yedeklemeler durdurulur.
Sunucunun yeni adını Backup kasasına kaydetmeniz gerekir. Yeni bir kayıt oluşturduğunuzda, ilk yedekleme işlemi artımlı yedekleme değil, tam yedekleme olur. Daha önce eski sunucu adıyla kasaya yedeklenen verileri kurtarmanız gerekiyorsa **Veri Kurtarma** sihirbazındaki [**Başka bir sunucu**](backup-azure-restore-windows-server.md#recover-to-an-alternate-machine) seçeneğini kullanarak verileri kurtarabilirsiniz.

## <a name="what-types-of-drives-can-i-backup-files-and-folders-from?"></a>Ne tür sürücülerden dosya ve klasör yedekleyebilirim? <br/>
Aşağıdaki sürücüler/birimler yedeklenemez:

- Çıkarılabilir Medya: Sürücünün yedekleme öğesi kaynağı olarak kullanılabilmesi için sabit olarak bildirilmesi gerekir.
- Salt Okunur Birimler: Birimin çalışması için birim gölge kopyası hizmetine (VSS) yönelik olarak yazılabilir olması gerekir.
- Çevrimdışı Birimler: Birimin çalışması için VSS'ye yönelik olarak çevrimiçi olması gerekir.
- Ağ paylaşımı: Birimin çevrimiçi yedekleme kullanılarak yedeklenebilmesi için sunucuya yönelik olarak yerel olması gerekir.
- Bitlocker korumalı birimler: Yedeklemenin gerçekleşebilmesi için birimin kilidinin açık olması gerekir.
- Dosya Sistemi Tanımlaması: NTFS, çevrimiçi yedekleme hizmetinin bu sürümü için desteklenen tek dosya sistemidir.

## <a name="what-file-and-folder-types-can-i-back-up-from-my-server?"></a>Sunucumdan hangi dosya ve klasör hangi türlerini yedekleyebilirim?<br/>
Aşağıdaki türler desteklenir:

- Şifreli
- Sıkıştırılmış
- Seyrek
- Sıkıştırılmış + Seyrek
- Sabit Bağlantılar: Desteklenmez, atlanır
- Yeniden Ayrıştırma Noktası: Desteklenmez, atlanır
- Şifreli + Sıkıştırılmış: Desteklenmez, atlanır
- Şifreli + Seyrek: Desteklenmez, atlanır
- Sıkıştırılmış Akış: Desteklenmez, atlanır
- Seyrek Akış: Desteklenmez, atlanır

## <a name="what's-the-minimum-size-requirement-for-the-cache-folder?"></a>Önbellek klasörü için minimum boyut gereksinimini nedir? <br/>
Önbellek klasörünün boyutu, yedeklediğiniz veri miktarını belirler. Önbellek klasörü, veri depolama için gerekli olan alanın % 5'ini oluşturmalıdır.

## <a name="if-my-organization-has-one-vault,-how-can-i-isolate-one-server's-data-from-another-server-when-restoring-data?"></a>Kuruluşumun bir kasası varsa veri geri yükleme sırasında bir sunucunun verilerini başka bir sunucudan nasıl ayırabilirim?<br/>
Aynı kasaya kayıtlı tüm sunucular, *aynı parolayı kullanan* diğer sunucular tarafından yedeklenen verileri kurtarabilir. Yedekleme verilerini kuruluşunuzdaki diğer sunucularından ayırmak istediğiniz sunucularınız varsa bu sunucular için belirlenmiş bir parola kullanın. Örneğin, insan kaynakları sunucuları bir şifreleme parolası kullanırken, muhasebe sunucuları ve depolama sunucuları farklı birer şifreleme parolası kullanabilir.

## <a name="can-i-“migrate”-my-backup-data-or-vault-between-subscriptions?"></a>Yedekleme verilerimin veya kasamın abonelikler arasında "geçişini" sağlayabilir miyim? <br/>
Hayır. Kasa abonelik düzeyinde oluşturulur ve oluşturulduktan sonra başka bir aboneliğe yeniden atanamaz.

## <a name="does-the-azure-backup-agent-work-on-a-server-that-uses-windows-server-2012-deduplication?"></a>Azure Backup Aracısı, Windows Server 2012 yinelenenleri kaldırma özelliğini kullanan bir sunucu üzerinde çalışır mı? <br/>
Evet. Aracı hizmeti, yedekleme işlemini hazırlarken yinelenenleri kaldırma işlemi uygulanmış verileri normal verilere dönüştürür. Ardından verileri yedekleme için en iyi duruma getirir, verileri şifreler ve daha sonra, şifreli verileri çevrimiçi yedekleme hizmetine gönderir.

## <a name="if-i-cancel-a-backup-job-once-it-has-started,-is-the-transferred-backup-data-deleted?"></a>Bir yedekleme işini başlatıldıktan sonra iptal edersem aktarılan yedekleme verileri silinir mi? <br/>
Hayır. Yedekleme kasası, iptal noktasına kadar aktarılmış olan yedeklenmiş verileri depolar. Azure Backup, yedekleme işlemi sırasında yedekleme verilerine zaman zaman denetim noktaları eklemek üzere bir denetim noktası mekanizması kullanır. Yedekleme verilerinde denetim noktaları bulunduğundan, sonraki yedekleme işlemi dosyaların bütünlüğünü doğrulayabilir. Tetiklenen sonraki yedekleme, daha önce yedeklenmiş olan veriler üzerinde artımlı olarak gerçekleşecektir. Artımlı yedekleme, bant genişliğinin daha iyi kullanılmasını sağlar; böylece aynı verileri tekrar tekrar aktarmanız gerekmez.

Azure VM yedeklemesinde iş iptal edildikten sonra aktarılan veriler göz ardı edilir ve yeni yedekleme artımlı verileri önceki başarılı yedekleme işinden aktarır. 

## <a name="why-am-i-seeing-the-warning-"azure-backups-have-not-been-configured-for-this-server"-even-though-i-had-scheduled-regular-backups-previously?"></a>Daha önce düzenli yedeklemeler zamanlamış olmama karşın neden "Azure Yedeklemeleri bu sunucu için yapılandırılmamış" uyarısını görüyorum? <br/>
Bu uyarı, yerel sunucuda depolanan yedekleme zamanlaması ayarları, yedekleme kasasında depolanan ayarlarla aynı olmadığında oluşur. Sunucu ya da ayarlar bilinen bir iyi duruma getirilerek kurtarıldığında, yedekleme zamanlamaları eşitlemesini kaybedebilir. Bu uyarıyı alırsanız [yedekleme ilkesini yeniden yapılandırın](backup-azure-manage-windows-server.md) ve ardından yerel sunucuyu Azure ile yeniden eşitlemek için **Run Back Up Now (Yedeklemeyi Şimdi Çalıştır)** işlemini uygulayın.

## <a name="what-firewall-rules-should-be-configured-for-azure-backup?"></a>Azure Backup için hangi güvenlik duvarı kuralları yapılandırılmalıdır? <br/>
Verilerin şirket içinden Azure'a ve iş yükünden Azure'a sorunsuz şekilde korunması için güvenlik duvarınızın aşağıdaki URL'ler ile iletişim kurmasına izin vermeniz önerilir:

- www.msftncsi.com
- \*.Microsoft.com
- \*.WindowsAzure.com
- \*.microsoftonline.com
- \*.windows.net

## <a name="can-i-install-the-azure-backup-agent-on-an-azure-vm-already-backed-by-the-azure-backup-service-using-the-vm-extension?"></a>Azure Backup aracısını, önceden VM uzantısı kullanılarak Azure Backup hizmeti tarafından yedeklenmiş olan bir Azure VM üzerine yükleyebilir miyim? <br/>
Kesinlikle. Azure Backup, VM uzantısını kullanan Azure VM'ler için VM düzeyinde yedekleme sağlar. Azure Backup aracısını, bir Konuk Windows işletim sistemine yükleyerek bu konuk işletim sistemi üzerindeki dosya ve klasörleri koruyabilirsiniz.

## <a name="can-i-install-the-azure-backup-agent-on-an-azure-vm-to-back-up-files-and-folders-present-on-temporary-storage-provided-by-the-azure-vm?"></a>Azure Backup aracısını bir Azure VM'ye yükleyerek mevcut dosya ve klasörleri Azure VM tarafından sağlanan geçici depolama alanına yedekleyebilir miyim? <br/>
Azure Backup aracısını, bir Konuk Windows işletim sistemine yükleyebilir ve dosya ve klasörleri geçici depolama alanına yedekleyebilirsiniz. Ancak geçici depolama verileri silindikten sonra yedeklemelerin başarısız olacağını lütfen unutmayın. Ayrıca, geçici depolama verilerinin silinmiş olması durumunda, yalnızca geçici olmayan depolama alanına geri yükleme gerçekleştirebilirsiniz.

## <a name="i-have-installed-azure-backup-agent-to-protect-my-files-and-folders.-can-i-now-install-scdpm-to-work-with-azure-backup-agent-to-protect-on-premises-application/vm-workloads-to-azure?"></a>Dosya ve klasörlerimi korumak için Azure Backup aracısını yükledim. Artık Azure'a yönelik şirket içi uygulama/VM iş yüklerini korumak için Azure Backup aracısıyla çalışmak üzere SCDPM'yi yükleyebilir miyim? <br/>
Azure Backup'ı SCDPM ile kullanmak için ilk olarak SCDPM'yi ve ancak bundan sonra Azure Backup aracısını yüklemeniz önerilir. Bu, Azure Backup aracısının SCDPM ile tümleştirmesinin sorunsuz şekilde gerçekleştirilmesini sağlar ve dosyaların/klasörlerin, Azure'a yönelik uygulama iş yüklerinin ve VM'lerin doğrudan SCDPM yönetim konsolundan korunmasına olanak tanır. Yukarıda belirtilen amaçlar doğrultusunda Azure Backup'ın yüklenmesinin ardından SCDPM'nin yüklenmesi önerilmez veya desteklenmez.

## <a name="what-is-the-length-of-file-path-that-can-be-specified-as-part-of-azure-backup-policy-using-azure-backup-agent?"></a>Azure Backup aracısını kullanan Azure Yedekleme ilkesinin bir bölümü olarak belirtilebilecek dosya yolunun uzunluğu nedir? <br/>  
Azure Backup aracısı NTFS kullanır. [Dosya yolu uzunluğu belirtimi, Windows API ile sınırlıdır](https://msdn.microsoft.com/library/aa365247.aspx#fully_qualified_vs._relative_paths). Windows API tarafından belirtilenlerden daha uzun dosya yollarına sahip olan dosyaların yedeklenmesi için müşteriler yedekleme dosyalarının üst klasörünü veya disk sürücüsünü yedeklemeyi seçebilir.  

## <a name="what-characters-are-allowed-in-file-path-of-azure-backup-policy-using-azure-backup-agent?"></a>Azure Backup aracısını kullanan Azure Yedekleme ilkesinin dosya yolunda hangi karakterlere izin verilir? <br>  
 Azure Backup aracısı NTFS kullanır. [NTFS destekli karakterleri](https://msdn.microsoft.com/library/aa365247.aspx#naming_conventions) dosya belirtiminin bir parçası olarak etkinleştirir.  

## <a name="can-i-use-azure-backup-server-to-create-a-bare-metal-recovery-(bmr)-backup-for-a-physical-server?"></a>Bir fiziksel sunucu için Tam Kurtarma (BMR) yedeklemesi oluşturmak üzere Azure Backup Sunucusu'nu kullanabilir miyim? <br/>
Evet.

## <a name="can-i-configure-the-backup-service-to-send-mail-if-a-backup-job-fails?"></a>Bir yedekleme işi başarısız olursa posta göndermek için Backup hizmetini yapılandırabilir miyim? <br/>
Evet, Backup hizmeti bir PowerShell betiği ile kullanılabilen çeşitli olay tabanlı uyarılara sahiptir. Tam açıklama için bkz. [Uyarı bildirimleri](backup-azure-manage-vms.md#alert-notifications).

## <a name="is-there-a-limit-on-the-size-of-each-data-source-being-backed-up?"></a>Yedeklenmekte olan her veri kaynağının boyutuna yönelik bir sınır var mıdır? <br/>
Kasa düzeyinde yedekleyebileceğiniz veri miktarına ilişkin bir sınırlama olmamasına karşın Azure Backup en büyük veri kaynağı boyutuyla ilgili bir kısıtlama uygulamaktadır (tüm uygulamalarda bu limitler çok yüksektir). Ağustos 2015'ten itibaren, desteklenen işletim sistemleri için en büyük veri kaynağı boyutu şu şekildedir:

|S.No | İşletim sistemi |  En büyük veri kaynağı boyutu |
| :-------------: |:-------------| :-----|
|1| Windows Server 2012 veya üzeri| 54400 GB|
|2| Windows 8 veya üzeri| 54400 GB|
|3| Windows Server 2008, Windows Server 2008 R2 | 1700 GB|
|4| Windows 7 | 1700 GB|

Aşağıdaki tabloda, her bir veri kaynağı boyutunun nasıl belirlendiği açıklanmaktadır.

|   Veri kaynağı  |   Ayrıntılar |
| :-------------: |:-------------|
|Birim |Bir sunucu veya istemci makinenin tek bir biriminden yedeklenmekte olan verilerin miktarı|
|Hyper-V sanal makine | Yedeklenmekte olan sanal makinenin tüm VHD'lerine ait verilerin toplamı|
|Microsoft SQL Server veritabanı | Yedeklenmekte olan tek bir SQL veritabanının boyutu |
|Microsoft SharePoint |Yedeklenmekte olan bir SharePoint grubu içinde içerik ve yapılandırma veritabanlarının toplamı|
|Microsoft Exchange |Yedeklenmekte olan bir Exchange sunucusundaki tüm Exchange veritabanlarının toplamı|
|BMR/Sistem Durumu |Yedeklenmekte olan makinenin BMR'sinin veya sistem durumunun her ayrı kopyası|

## <a name="are-there-limits-on-the-number-of-times-a-backup-job-can-be-scheduled-per-day?"></a>Bir yedekleme işinin günde kaç kez zamanlanabileceğine yönelik sınırlar var mıdır?<br/>
Evet, yedekleme işlerini Windows Server veya Windows istemcisi üzerinde günde en fazla üç kez çalıştırabilirsiniz. Yedekleme işlerini System Center DPM üzerinde günde en fazla iki kez çalıştırabilirsiniz. Bir yedekleme işini IaaS VM'ler için günde bir kez çalıştırabilirsiniz.

## <a name="is-there-a-difference-between-the-scheduling-policy-for-dpm-and-windows-server-(i.e.-on-windows-server-without-dpm)?"></a>DPM ve Windows Server'a yönelik zamanlama ilkesi (DPM olmadan Windows Server'da) arasında bir fark var mıdır? <br/>
Evet. DPM'yi kullanarak, günlük, haftalık, aylık ve yıllık zamanlamalar belirtebilirsiniz. Windows Server (DPM olmadan) yalnızca günlük ve haftalık zamanlamalar belirtmenize olanak tanır.

## <a name="is-there-a-difference-between-the-retention-policy-for-dpm-and-windows-server/client-(i.e.-on-windows-server-without-dpm)?"></a>DPM ve Windows Server'a/istemcisine yönelik bekletme ilkesi (DPM olmadan Windows Server'da) arasında bir fark var mıdır?<br/>
Hayır, hem DPM hem de Windows Server/istemcisi günlük, haftalık, aylık ve yıllık bekletme ilkelerine sahiptir.

## <a name="can-i-configure-my-retention-policies-selectively-–-i.e.-configure-weekly-and-daily-but-not-yearly-and-monthly?"></a>Bekletme ilkelerimi seçerek yapılandırabilir miyim? Başka bir deyişle, haftalık ve günlük yapılandırmaya olanak tanırken yıllık ve aylık yapılandırmayı engelleyebilir miyim?<br/>
Evet, Azure Backup bekletme yapısı gereksinimlerinizi karşılayan bekletme ilkesini tanımlama konusunda tam esnekliğe sahip olmanızı sağlar.

## <a name="can-i-“schedule-a-backup”-at-6pm-and-specify-“retention-policies”-at-a-different-time?"></a>Saat 18:00 için "bir yedekleme zamanlayıp" farklı bir saat için de "bekletme ilkeleri" belirtebilir miyim?<br/>
Hayır. Bekletme ilkeleri, yalnızca yedekleme noktalarında uygulanabilir. Aşağıdaki görüntüde, bekletme ilkesi 00:00 ve 18:00'de alınan yedeklemeler için belirtilmiştir. <br/>

![Yedekleme ve Bekletmeyi Zamanlama](./media/backup-azure-backup-faq/Schedule.png)
<br/>

## <a name="is-an-incremental-copy-transferred-for-the-retention-policies-scheduled?"></a>Zamanlanmış bekletme ilkeleri için bir artımlı kopya aktarılır mı? <br/>
Hayır, artımlı kopya, yedekleme zamanlaması sayfasında belirtilen süreye göre gönderilir. Bekletilebilen noktalar, bekletme ilkesi temel alınarak belirlenir.

## <a name="if-a-backup-is-retained-for-a-long-duration,-does-it-take-more-time-to-recover-an-older-data-point?"></a>Yedekleme uzun süre tutulursa daha eski bir veri noktasının kurtarılması daha uzun mu sürer? <br/>
 Hayır, en eski veya en yeni noktanın kurtarılması için gereken süre aynıdır. Her kurtarma noktası bir tam nokta gibi davranır.

## <a name="if-each-recovery-point-is-like-a-full-point,-does-it-impact-the-total-billable-backup-storage?"></a>Her kurtarma noktası bir tam nokta gibiyse bu durum toplam faturalanabilir yedekleme alanını etkiler mi?<br/>
Genel uzun vadeli bekletme noktası ürünleri, yedekleme verilerini tam noktalar olarak depolar. Tam noktalar depolama açısından *verimsizdir* ancak daha kolay ve hızlı şekilde geri yüklenir. Artımlı kopyalar depolama açısından *verimlidir* ancak kurtarma süresini etkileyecek şekilde bir veri zincirini geri yüklemenizi gerektirir. Azure Backup alanı mimarisi, hızlı geri yükleme özelliğiyle optimum veri depolama olanağı sunarken aynı zamanda düşük depolama maliyetleri oluşturarak iki açıdan da avantaj sağlar. Bu veri depolama yaklaşımı, giriş ve çıkış bant genişliğinizin verimli şekilde kullanılmasını sağlar. Veri depolama alanı miktarı ve verileri kurtarmak için gereken süre minimum düzeyde tutulur. [Artımlı yedekleme](https://azure.microsoft.com/blog/microsoft-azure-backup-save-on-long-term-storage/) tasarrufunun ne kadar verimli olduğu hakkında daha fazla bilgi edinin. 

## <a name="is-there-a-limit-on-the-number-of-recovery-points-that-can-be-created?"></a>Oluşturulabilecek kurtarma noktalarının sayısına yönelik bir sınır var mıdır?<br/>
Hayır. Kurtarma noktalarına yönelik sınırları kaldırdık. İstediğiniz sayıda kurtarma noktası oluşturabilirsiniz.

## <a name="why-is-the-amount-of-data-transferred-in-backup-not-equal-to-the-amount-of-data-i-backed-up?"></a>Yedeklemede aktarılan veri miktarı neden yedeklenen veri miktarına eşit değil?<br/>
 Azure Backup Aracısı veya SCDPM ya da Azure Backup Sunucusundan yedeklenen tüm veriler aktarılmadan önce sıkıştırılır ve şifrelenir. Sıkıştırma ve şifreleme uygulandıktan sonra yedekleme kasasındaki veriler % 30-40 daha küçük hale gelir.

## <a name="is-there-a-way-to-adjust-the-amount-of-bandwidth-used-by-the-backup-service?"></a>Backup hizmeti tarafından kullanılan bant genişliği miktarını ayarlamanın bir yolu var mıdır?<br/>
 Evet, bant genişliğini ayarlamak için Backup Aracısı'ndaki **Özellikleri Değiştir** seçeneğini kullanın. Bant genişliği miktarını ve bu bant genişliğini kullanma zamanlarınızı ayarlayın. Daha fazla bilgi için bkz. [Ağ Azaltma](../backup-configure-vault.md#enable-network-throttling).

## <a name="my-internet-bandwidth-is-limited-for-the-amount-of-data-i-need-to-back-up.-is-there-a-way-i-can-move-data-to-a-certain-location-with-a-large-network-pipe-and-push-that-data-into-azure?"></a>İnternet bant genişliğim, yedeklemem gereken veri miktarı için sınırlı durumda. Verileri büyük bir ağ kanalı ile belirli bir konuma taşıyıp bu verileri Azure'a gönderebilmemin bir yolu var mıdır? <br/>
Standart çevrimiçi yedekleme işlemini kullanarak verileri Azure'a yedekleyebilir veya verileri Azure'da blob depolamaya aktarmak için Azure İçeri/Dışarı Aktarma hizmetini kullanabilirsiniz. Yedekleme verilerini Azure depolama alanına almanın başka bir yolu yoktur. Azure İçeri/Dışarı Aktarma hizmetini Azure Backup ile kullanma hakkında bilgi için lütfen [Çevrimdışı Yedekleme iş akışı](backup-azure-backup-import-export.md) makalesine bakın.

## <a name="how-many-recoveries-can-i-perform-on-the-data-that-is-backed-up-to-azure?"></a>Azure'a yedeklenen veriler üzerinde kaç kurtarma işlemi yapabilirim?<br/>
Azure Backup ile gerçekleştirilen kurtarma işlemlerinin sayısına yönelik bir sınır yoktur.

## <a name="do-i-have-to-pay-for-the-egress-traffic-from-azure-data-center-during-recoveries?"></a>Kurtarma sırasında Azure veri merkezi çıkış trafiği için ödeme yapmam gerekir mi?<br/>
 Hayır. Kurtarma işlemleriniz ücretsizdir ve çıkış trafiği için sizden ücret tahsil edilmez.

## <a name="is-the-data-sent-to-azure-encrypted?"></a>Veriler Azure'a şifreli olarak mı gönderilir? <br/>
Evet. Veriler AES256 kullanılarak şirket içi sunucu/istemci/SCDPM makinesi üzerinde şifrelenir ve güvenli bir HTTPS bağlantısı üzerinden gönderilir.

## <a name="is-the-backup-data-on-azure-encrypted-as-well?"></a>Azure üzerindeki yedekleme verileri de şifreli midir?<br/>
 Evet. Azure'a gönderilen veriler (bekleyen) şifreli olarak kalır. Microsoft herhangi bir noktada yedekleme verilerinin şifresini çözmez. Azure Backup, Azure VM yedeklemesi için sanal makinenin şifrelemesini kullanır; örneğin, sanal makineniz Azure Disk Şifrelemesi veya başka bir şifreleme teknolojisi kullanılarak şifreleniyorsa Azure Backup verilerinizin güvenliğini sağlamak için bu şifrelemeyi kullanır. 

## <a name="what-is-the-minimum-length-of-encryption-key-used-to-encrypt-backup-data?"></a>Yedekleme verilerini şifrelemek için kullanılan şifreleme anahtarının minimum uzunluğu nedir? <br/>
 Şifreleme anahtarı en az 16 karakterden oluşmalıdır.

## <a name="what-happens-if-i-misplace-the-encryption-key?-can-i-recover-the-data-(or)-can-microsoft-recover-the-data?"></a>Şifreleme anahtarını kaybedersem ne olur? Verileri kurtarabilir miyim?/Microsoft, verileri kurtarabilir mi? <br/>
Yedekleme verilerini şifrelemek için kullanılan anahtar yalnızca müşterinin şirketinde bulunur. Microsoft, Azure üzerinde anahtarın bir kopyasını tutmaz ve anahtara erişim sahibi değildir. Müşteri, anahtarı kaybederse Microsoft, yedekleme verilerini kurtaramaz.

## <a name="how-do-i-change-the-cache-location-specified-for-the-azure-backup-agent?"></a>Azure Backup aracısı için belirtilen önbellek konumunu nasıl değiştiririm?<br/>
 Önbellek konumunu değiştirmek için aşağıdaki madde listesini sırasıyla izleyin.
- Yükseltilmiş komut isteminde aşağıdaki komutu çalıştırarak Backup altyapısını durdurun:

  ```PS C:\> Net stop obengine```

- Dosyaları taşımayın. Bunun yerine, önbellek alanı klasörünü yeterli alana sahip farklı bir sürücüye kopyalayın. Yedeklemelerin yeni önbellek alanı ile çalıştığı onaylandıktan sonra özgün önbellek alanı kaldırılabilir.

- Aşağıdaki kayıt defteri girdilerini yeni önbellek alanı klasörünün yolu ile güncelleştirin.<br/>

|Kayıt defteri yolu | Kayıt Defteri Anahtarı | Değer |
| ------ | ------- | ------|
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` | ScratchLocation | *Yeni önbellek klasörü konumu* |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` | ScratchLocation | *Yeni önbellek klasörü konumu* |

- Yükseltilmiş komut isteminde aşağıdaki komutu çalıştırarak Backup altyapısını yeniden başlatın:

  ```PS C:\> Net start obengine```

  Yedekleme oluşturma yeni önbellek konumunda başarıyla tamamlandıktan sonra, özgün önbellek klasörünü kaldırabilirsiniz.

## <a name="where-can-i-put-the-cache-folder-for-the-azure-backup-agent-to-work-as-expected?"></a>Azure Backup Aracısı'nın beklendiği şekilde çalışması için önbellek klasörünü nereye koyabilirim?<br/>
Önbellek klasörü için aşağıdaki konumlar önerilmez:

- Ağ paylaşımı veya Çıkarılabilir Medya: Önbellek klasörü, çevrimiçi yedekleme kullanılarak yedeklenmesi gereken sunucu için yerel olmalıdır. Ağ konumlarını veya USB sürücüleri gibi çıkarılabilir medyalar desteklenmez.
- Çevrimdışı Birimler: Önbellek klasörü, Azure Backup Aracısı kullanılarak gerçekleştirilecek beklenen yedekleme için çevrimiçi olmalıdır.

## <a name="are-there-any-attributes-of-the-cache-folder-that-are-not-supported?"></a>Önbellek klasörünün desteklenmeyen herhangi bir özniteliği var mıdır?<br/>
 Aşağıdaki öznitelikler veya bunların bileşimleri, önbellek klasörü için desteklenmez:

- Şifreli
- Yinelenenleri kaldırma işlemi uygulanmış
- Sıkıştırılmış
- Seyrek
- Yeniden Ayrıştırma Noktası

Azure Backup aracısının beklenen şekilde çalışması için önbellek klasörünün veya meta veri VHD'sinin yukarıdaki özniteliklere sahip olmaması önerilir.



<!--HONumber=Oct16_HO3-->


