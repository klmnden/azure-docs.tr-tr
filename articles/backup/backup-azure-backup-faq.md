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
     ms.date="07/01/2016"
     ms.author="trinadhk; giridham; arunak; markgal; jimpark;"/>

# Azure Backup hizmeti - SSS

> [AZURE.SELECTOR]
- [Klasik mod için Backup ile ilgili SSS](backup-azure-backup-faq.md)
- [ARM modu için Backup ile ilgili SSS](backup-azure-backup-ibiza-faq.md)

Bu makale, Azure Backup hizmeti ile ilgili sık sorulan soruların (ve yanıtlarının) listesinden oluşmaktadır. Topluluğumuz, soruları hızlı bir şekilde yanıtlar ve bir sorunun sıklıkla sorulması durumunda söz konusu soruyu bu makaleye ekleriz. Soruların yanıtları genellikle başvuru veya destek bilgileri sağlar. Azure Backup ile ilgili sorularınızı, bu makalenin veya ilgili bir makalenin Disqus bölümünde sorabilirsiniz. Ayrıca Azure Backup hizmeti ile ilgili sorularınızı [tartışma forumunda](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) paylaşabilirsiniz.

## Yükleme ve Yapılandırma
**S1. Azure Backup hizmetini kullanarak Azure'a yedekleme yapabileceğim desteklenen işletim sistemlerinin listesi ne şekildedir?** <br/>
Y1. Azure Backup aşağıdaki listede bulunan işletim sistemlerini destekler


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

**S2. En son Azure Backup aracısını nereden indirebilirim?** <br/>
Y2. Windows Server, System Center DPM veya Windows istemcisini yedeklemeye yönelik en son aracıyı [buradan](http://aka.ms/azurebackup_agent) indirebilirsiniz. Bir sanal makineyi yedeklemek istiyorsanız VM Aracısı'nı (otomatik olarak uygun uzantıyı yükler) kullanın. VM Aracısı, Azure galerisinden oluşturulan sanal makineler üzerinde zaten mevcuttur.

**S3. SCDPM sunucusunun hangi sürümü desteklenir?** <br/>
Y3. [En son](http://aka.ms/azurebackup_agent) Azure Backup aracısını, SCDPM'nin (Temmuz 2015'ten itibaren UR6) en son güncelleştirme paketi üzerine yüklemeniz önerilir

**S4. Azure Yedekleme aracısını yapılandırırken **kasa kimlik bilgilerini** girmem isteniyor. Kasa kimlik bilgilerinin süresi dolar mı?
Y4. Evet, kasa kimlik bilgilerinin süresi 48 saat sonra dolar. Dosyanın süresi dolarsa Azure portalında oturum açın ve kasa kimlik bilgileri dosyalarını Yedekleme kasanızdan indirin.

**S5. Her bir Azure aboneliği için oluşturulan yedekleme kasalarının sayısına yönelik herhangi bir sınır var mıdır?** <br/>
Y5. Evet. Temmuz 2015'ten itibaren, abonelik başına 25 kasa oluşturabilirsiniz. Daha fazla kasaya ihtiyacınız varsa yeni bir abonelik oluşturun.

**S6. Kasayı bir fatura varlığı olarak mı görmeliyim?** <br/>
Y6. Her kasa için ayrıntılı bir fatura almak mümkün olsa da Azure aboneliğini kesinlikle fatura varlığı olarak değerlendirmenizi öneririz. Bu, tüm hizmetler arasında tutarlıdır ve daha kolay yönetilir.

**S7. Her bir kasa için kaydedilebilen sunucu/makine sayısına yönelik herhangi bir sınır var mıdır?** <br/>
Y7. Evet, kasa başına en fazla 50 makine kaydedebilirsiniz. Azure IaaS sanal makineleri için sınır, kasa başına 200 VM'dir. Daha fazla makine kaydetmeniz gerekirse yeni bir kasa oluşturun.

**S8. Bir Windows server'dan/istemcisinden veya SCDPM sunucusundan yedeklenebilecek veri miktarına yönelik herhangi bir sınır var mıdır?** <br/>
Y8. Hayır.

**S9. Sunucumu başka bir veri merkezine nasıl kaydederim?**<br/>
Y9. Backup verileri, kayıtlı olduğu Backup Hizmeti'nin veri merkezine gönderilir. Veri merkezini değiştirmenin en kolay yolu, aracıyı kaldırmak ve aracıyı yeniden yükleyip yeni bir veri merkezine kaydetmektir.

**S10. Azure'a veri yedekleyen bir Windows sunucusunu yeniden adlandırırsam ne olur?**<br/>
Y10. Bir sunucuyu yeniden adlandırdığınızda, geçerli olarak yapılandırılmış olan tüm yedeklemeler durdurulur.
Sunucunun yeni adını Backup kasasına kaydetmeniz gerekir. Yeni bir kayıt oluşturduğunuzda, ilk yedekleme işlemi artımlı yedekleme değil, tam yedekleme olur. Daha önce eski sunucu adıyla kasaya yedeklenen verileri kurtarmanız gerekiyorsa **Veri Kurtarma** sihirbazındaki [**Başka bir sunucu**](backup-azure-restore-windows-server.md#recover-to-an-alternate-machine) seçeneğini kullanarak verileri kurtarabilirsiniz.


**S11. Ne tür sürücülerden dosya ve klasör yedekleyebilirim?** <br/>
Y11. Aşağıdaki sürücüler/birimler yedekleme olamaz:

- Çıkarılabilir Medya: Sürücünün yedekleme öğesi kaynağı olarak kullanılabilmesi için sabit olarak bildirilmesi gerekir.
- Salt Okunur Birimler: Birimin çalışması için birim gölge kopyası hizmetine (VSS) yönelik olarak yazılabilir olması gerekir.
- Çevrimdışı Birimler: Birimin çalışması için VSS'ye yönelik olarak çevrimiçi olması gerekir.
- Ağ paylaşımı: Birimin çevrimiçi yedekleme kullanılarak yedeklenebilmesi için sunucuya yönelik olarak yerel olması gerekir.
- Bitlocker korumalı birimler: Yedeklemenin gerçekleşebilmesi için birimin kilidinin açık olması gerekir.
- Dosya Sistemi Tanımlaması: NTFS, çevrimiçi yedekleme hizmetinin bu sürümü için desteklenen tek dosya sistemidir.

**S12. Sunucumdan hangi dosya ve klasör hangi türlerini yedekleyebilirim?**<br/>
Y12. Aşağıdaki türler desteklenir:

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

**S13. Önbellek klasörü için minimum boyut gereksinimini nedir?** <br/>
Y13. Önbellek klasörünün boyutu, yedeklediğiniz veri miktarını belirler. Önbellek klasörü, veri depolama için gerekli olan alanın % 5'ini oluşturmalıdır.

**S14. Kuruluşumun bir yedekleme kasası varsa veri geri yükleme sırasında bir sunucunun verilerini başka bir sunucudan nasıl ayırabilirim?**<br/>
Y14. Aynı kasaya kayıtlı tüm sunucular, *aynı parolayı kullanan* diğer sunucular tarafından yedeklenen verileri kurtarabilir. Yedekleme verilerini kuruluşunuzdaki diğer sunucularından ayırmak istediğiniz sunucularınız varsa bu sunucular için belirlenmiş bir parola kullanın. Örneğin, insan kaynakları sunucuları bir şifreleme parolası kullanırken, muhasebe sunucuları ve depolama sunucuları farklı birer şifreleme parolası kullanabilir.

**S15. Yedekleme verilerimin abonelikler arasında "geçişini" sağlayabilir miyim?** <br/>
Y15: Hayır

**S16. Yedekleme kasamın abonelikler arasında "geçişini" sağlayabilir miyim?** <br/>
Y16: Hayır Kasa abonelik düzeyinde oluşturulur ve oluşturulduktan sonra başka bir aboneliğe yeniden atanamaz.

**S17. Azure Backup Aracısı, Windows Server 2012 yinelenenleri kaldırma özelliğini kullanan bir sunucu üzerinde çalışır mı?** <br/>
Y17: Evet. Aracı hizmeti, yedekleme işlemini hazırlarken yinelenenleri kaldırma işlemi uygulanmış verileri normal verilere dönüştürür. Ardından verileri yedekleme için en iyi duruma getirir, verileri şifreler ve daha sonra, şifreli verileri çevrimiçi yedekleme hizmetine gönderir.

**S18. Bir yedekleme işini başlatıldıktan sonra iptal edersem aktarılan yedekleme verileri silinir mi?** <br/>
Y18: Hayır. Yedekleme kasası, iptal noktasına kadar aktarılmış olan yedeklenmiş verileri depolar. Azure Backup, yedekleme işlemi sırasında yedekleme verilerine zaman zaman denetim noktaları eklemek üzere bir denetim noktası mekanizması kullanır. Yedekleme verilerinde denetim noktaları bulunduğundan, sonraki yedekleme işlemi dosyaların bütünlüğünü doğrulayabilir. Tetiklenen sonraki yedekleme, daha önce yedeklenmiş olan veriler üzerinde artımlı olarak gerçekleşecektir. Artımlı yedekleme, bant genişliğinin daha iyi kullanılmasını sağlar; böylece aynı verileri tekrar tekrar aktarmanız gerekmez.

**S19. Daha önce düzenli yedeklemeler zamanlamış olmama karşın neden "Azure Yedeklemeleri bu sunucu için yapılandırılmamış" uyarısını görüyorum?** <br/>
Y19: Bu uyarı, yerel sunucuda depolanan yedekleme zamanlaması ayarları, yedekleme kasasında depolanan ayarlarla aynı olmadığında oluşur. Sunucu ya da ayarlar bilinen bir iyi duruma getirilerek kurtarıldığında, yedekleme zamanlamaları eşitlemesini kaybedebilir. Bu uyarıyı alırsanız [yedekleme ilkesini yeniden yapılandırın](backup-azure-manage-windows-server.md) ve ardından yerel sunucuyu Azure ile yeniden eşitlemek için **Run Back Up Now (Yedeklemeyi Şimdi Çalıştır)** işlemini uygulayın.

**S20. Azure Backup için hangi güvenlik duvarı kuralları yapılandırılmalıdır?** <br/>
Y20. Verilerin şirket içinden Azure'a ve iş yükünden Azure'a sorunsuz şekilde korunması için güvenlik duvarınızın aşağıdaki URL'ler ile iletişim kurmasına izin vermeniz önerilir:

- www.msftncsi.com
- \*.Microsoft.com
- \*.WindowsAzure.com
- \*.microsoftonline.com
- \*.windows.net

**S21. Azure Backup aracısını, önceden VM uzantısı kullanılarak Azure Backup hizmeti tarafından yedeklenmiş olan bir Azure VM üzerine yükleyebilir miyim?** <br/>
Y21. Kesinlikle. Azure Backup, VM uzantısını kullanan Azure VM'ler için VM düzeyinde yedekleme sağlar. Azure Backup aracısını, bir Konuk Windows işletim sistemine yükleyerek bu konuk işletim sistemi üzerindeki dosya ve klasörleri koruyabilirsiniz.

**S22. Azure Backup aracısını bir Azure VM'ye yükleyerek mevcut dosya ve klasörleri Azure VM tarafından sağlanan geçici depolama alanına yedekleyebilir miyim?** <br/>
Y22. Azure Backup aracısını, bir Konuk Windows işletim sistemine yükleyebilir ve dosya ve klasörleri geçici depolama alanına yedekleyebilirsiniz. Ancak geçici depolama verileri silindikten sonra yedeklemelerin başarısız olacağını lütfen unutmayın. Ayrıca, geçici depolama verilerinin silinmiş olması durumunda, yalnızca geçici olmayan depolama alanına geri yükleme gerçekleştirebilirsiniz.

**S23. Dosya ve klasörlerimi korumak için Azure Backup aracısını yükledim. Artık Azure'a yönelik şirket içi uygulama/VM iş yüklerini korumak için Azure Backup aracısıyla çalışmak üzere SCDPM'yi yükleyebilir miyim?** <br/>
Y23. Azure Backup'ı SCDPM ile kullanmak için ilk olarak SCDPM'yi ve ancak bundan sonra Azure Backup aracısını yüklemeniz önerilir. Bu, Azure Backup aracısının SCDPM ile tümleştirmesinin sorunsuz şekilde gerçekleştirilmesini sağlar ve dosyaların/klasörlerin, Azure'a yönelik uygulama iş yüklerinin ve VM'lerin doğrudan SCDPM yönetim konsolundan korunmasına olanak tanır. Yukarıda belirtilen amaçlar doğrultusunda Azure Backup'ın yüklenmesinin ardından SCDPM'nin yüklenmesi önerilmez veya desteklenmez.

**S24. Azure Backup aracısını kullanan Azure Yedekleme ilkesinin bir bölümü olarak belirtilebilecek dosya yolunun uzunluğu nedir?** <br/>  
Y24. Azure Backup aracısı NTFS kullanır. [Dosya yolu uzunluğu belirtimi, Windows API ile sınırlıdır](https://msdn.microsoft.com/library/aa365247.aspx#fully_qualified_vs._relative_paths). Windows API tarafından belirtilenlerden daha uzun dosya yollarına sahip olan dosyaların yedeklenmesi için müşteriler yedekleme dosyalarının üst klasörünü veya disk sürücüsünü yedeklemeyi seçebilir.  

**S25. Azure Backup aracısını kullanan Azure Yedekleme ilkesinin dosya yolunda hangi karakterlere izin verilir?** <br>  
Y25. Azure Backup aracısı NTFS kullanır. [NTFS destekli karakterleri](https://msdn.microsoft.com/library/aa365247.aspx#naming_conventions) dosya belirtiminin bir parçası olarak etkinleştirir.  

**S26. Bir fiziksel sunucu için Tam Kurtarma (BMR) yedeklemesi oluşturmak üzere Azure Backup Sunucusu'nu kullanabilir miyim?** <br/>
Y26. Evet.

**S27. Bir yedekleme işi başarısız olursa posta göndermek için Backup hizmetini yapılandırabilir miyim?** <br/>
Y27. Evet, Backup hizmeti bir PowerShell betiği ile kullanılabilen çeşitli olay tabanlı uyarılara sahiptir. Tam açıklama için bkz. [Uyarı bildirimleri](backup-azure-manage-vms.md#alert-notifications).



## Yedekleme ve Bekletme
**S1. Yedeklenmekte olan her veri kaynağının boyutuna yönelik bir sınır var mıdır?** <br/>
Y1. Ağustos 2015'ten itibaren, desteklenen işletim sistemleri için en büyük veri kaynağı boyutu şu şekildedir:

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

**S2. Bir yedekleme işinin günde kaç kez zamanlanabileceğine yönelik sınırlar var mıdır?**<br/>
Y2. Evet, yedekleme işlerini Windows Server veya Windows istemcisi üzerinde günde en fazla üç kez çalıştırabilirsiniz. Yedekleme işlerini System Center DPM üzerinde günde en fazla iki kez çalıştırabilirsiniz. Bir yedekleme işini IaaS VM'ler için günde bir kez çalıştırabilirsiniz.

**S3. DPM ve Windows Server'a yönelik zamanlama ilkesi (DPM olmadan Windows Server'da) arasında bir fark var mıdır?** <br/>
Y3. Evet. DPM'yi kullanarak, günlük, haftalık, aylık ve yıllık zamanlamalar belirtebilirsiniz. Windows Server (DPM olmadan) yalnızca günlük ve haftalık zamanlamalar belirtmenize olanak tanır.

**S4. DPM ve Windows Server'a/istemcisine yönelik bekletme ilkesi (DPM olmadan Windows Server'da) arasında bir fark var mıdır?**<br/>
Y4. Hayır, hem DPM hem de Windows Server/istemcisi günlük, haftalık, aylık ve yıllık bekletme ilkelerine sahiptir.

**S5. Bekletme ilkelerimi seçerek yapılandırabilir miyim? Başka bir deyişle, haftalık ve günlük yapılandırmaya olanak tanırken yıllık ve aylık yapılandırmayı engelleyebilir miyim?**<br/>
Y5. Evet, Azure Backup bekletme yapısı gereksinimlerinizi karşılayan bekletme ilkesini tanımlama konusunda tam esnekliğe sahip olmanızı sağlar.

**S6. Saat 18:00 için "bir yedekleme zamanlayıp" farklı bir saat için de "bekletme ilkeleri" belirtebilir miyim?**<br/>
Y6. Hayır. Bekletme ilkeleri, yalnızca yedekleme noktalarında uygulanabilir. Aşağıdaki görüntüde, bekletme ilkesi 00:00 ve 18:00'de alınan yedeklemeler için belirtilmiştir. <br/>

![Yedeklemeyi ve Bekletmeyi Zamanlama](./media/backup-azure-backup-faq/Schedule.png)
<br/>

**S7. Zamanlanmış bekletme ilkeleri için bir artımlı kopya aktarılır mı?** <br/>
Y7. Hayır, artımlı kopya, yedekleme zamanlaması sayfasında belirtilen süreye göre gönderilir. Bekletilebilen noktalar, bekletme ilkesi temel alınarak belirlenir.

**S8. Yedekleme uzun süre tutulursa daha eski bir veri noktasının kurtarılması daha uzun mu sürer?** <br/>
Y8. Hayır, en eski veya en yeni noktanın kurtarılması için gereken süre aynıdır. Her kurtarma noktası bir tam nokta gibi davranır.

**S9. Her kurtarma noktası bir tam nokta gibiyse bu durum toplam faturalanabilir yedekleme alanını etkiler mi?**<br/>
Y9.  Genel uzun vadeli bekletme noktası ürünleri, yedekleme verilerini tam noktalar olarak depolar. Tam noktalar depolama açısından *verimsizdir* ancak daha kolay ve hızlı şekilde geri yüklenir. Artımlı kopyalar depolama açısından *verimlidir* ancak kurtarma süresini etkileyecek şekilde bir veri zincirini geri yüklemenizi gerektirir. Azure Backup alanı mimarisi, hızlı geri yükleme özelliğiyle optimum veri depolama olanağı sunarken aynı zamanda düşük depolama maliyetleri oluşturarak iki açıdan da avantaj sağlar. Bu veri depolama yaklaşımı, giriş ve çıkış bant genişliğinizin verimli şekilde kullanılmasını sağlar. Veri depolama alanı miktarı ve verileri kurtarmak için gereken süre minimum düzeyde tutulur.

**S10. Oluşturulabilecek kurtarma noktalarının sayısına yönelik bir sınır var mıdır?**<br/>
Y10. Hayır. Kurtarma noktalarına yönelik sınırları kaldırdık. İstediğiniz sayıda kurtarma noktası oluşturabilirsiniz.

**S11. Yedeklemede aktarılan veri miktarı neden yedeklenen veri miktarına eşit değil?**<br/>
Y11. Yedeklenen tüm veriler aktarılmadan önce sıkıştırılır ve şifrelenir. Sıkıştırma ve şifreleme uygulandıktan sonra yedekleme kasasındaki veriler % 30-40 daha küçük hale gelir.

**S12. Backup hizmeti tarafından kullanılan bant genişliği miktarını ayarlamanın bir yolu var mıdır?**<br/>
Y12. Evet, bant genişliğini ayarlamak için Backup Aracısı'ndaki **Özellikleri Değiştir** seçeneğini kullanın. Bant genişliği miktarını ve bu bant genişliğini kullanma zamanlarınızı ayarlayın. Daha fazla bilgi için bkz. [Ağ Azaltma](../backup-configure-vault.md#enable-network-throttling).

**S13. İnternet bant genişliğim, yedeklemem gereken veri miktarı için sınırlı durumda. Verileri büyük bir ağ kanalı ile belirli bir konuma taşıyıp bu verileri Azure'a gönderebilmemin bir yolu var mıdır?** <br/>
Y13. Standart çevrimiçi yedekleme işlemini kullanarak verileri Azure'a yedekleyebilir veya verileri Azure'da blob depolamaya aktarmak için Azure İçeri/Dışarı Aktarma hizmetini kullanabilirsiniz. Yedekleme verilerini Azure depolama alanına almanın başka bir yolu yoktur. Azure İçeri/Dışarı Aktarma hizmetini Azure Backup ile kullanma hakkında bilgi için lütfen [Çevrimdışı Yedekleme iş akışı](backup-azure-backup-import-export.md) makalesine bakın.


## Kurtarma
**S1. Azure'a yedeklenen veriler üzerinde kaç kurtarma işlemi yapabilirim?**<br/>
Y1. Azure Backup ile gerçekleştirilen kurtarma işlemlerinin sayısına yönelik bir sınır yoktur.

**S2. Kurtarma sırasında Azure veri merkezi çıkış trafiği için ödeme yapmam gerekir mi?**<br/>
Y2. Hayır. Kurtarma işlemleriniz ücretsizdir ve çıkış trafiği için sizden ücret tahsil edilmez.

## Güvenlik
**S1. Veriler Azure'a şifreli olarak mı gönderilir?** <br/>
Y1. Evet. Veriler AES256 kullanılarak şirket içi sunucu/istemci/SCDPM makinesi üzerinde şifrelenir ve güvenli bir HTTPS bağlantısı üzerinden gönderilir.

**S2. Azure üzerindeki yedekleme verileri de şifreli midir?**<br/>
Y2. Evet. Azure'a gönderilen veriler (bekleyen) şifreli olarak kalır. Microsoft herhangi bir noktada yedekleme verilerinin şifresini çözmez.

**S3. Yedekleme verilerini şifrelemek için kullanılan şifreleme anahtarının minimum uzunluğu nedir?** <br/>
Y3. Şifreleme anahtarı en az 16 karakterden oluşmalıdır.

**S4. Şifreleme anahtarını kaybedersem ne olur? Verileri kurtarabilir miyim?/Microsoft, verileri kurtarabilir mi?** <br/>
Y4. Yedekleme verilerini şifrelemek için kullanılan anahtar yalnızca müşterinin şirketinde bulunur. Microsoft, Azure üzerinde anahtarın bir kopyasını tutmaz ve anahtara erişim sahibi değildir. Müşteri, anahtarı kaybederse Microsoft, yedekleme verilerini kurtaramaz.
 

## Yedekleme önbelleği

**S1. Azure Backup aracısı için belirtilen önbellek konumunu nasıl değiştiririm?**<br/>
Y1. Önbellek konumunu değiştirmek için aşağıdaki madde listesini sırasıyla izleyin.
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

**S2. Azure Backup Aracısı'nın beklendiği şekilde çalışması için önbellek klasörünü nereye koyabilirim?**<br/>
Y2. Önbellek klasörü için aşağıdaki konumlar önerilmez:

- Ağ paylaşımı veya Çıkarılabilir Medya: Önbellek klasörü, çevrimiçi yedekleme kullanılarak yedeklenmesi gereken sunucu için yerel olmalıdır. Ağ konumlarını veya USB sürücüleri gibi çıkarılabilir medyalar desteklenmez.
- Çevrimdışı Birimler: Önbellek klasörü, Azure Backup Aracısı kullanılarak gerçekleştirilecek beklenen yedekleme için çevrimiçi olmalıdır.

**S3. Önbellek klasörünün desteklenmeyen herhangi bir özniteliği var mıdır?**<br/>
Y3. Aşağıdaki öznitelikler veya bunların bileşimleri, önbellek klasörü için desteklenmez:

- Şifreli
- Yinelenenleri kaldırma işlemi uygulanmış
- Sıkıştırılmış
- Seyrek
- Yeniden Ayrıştırma Noktası

Azure Backup aracısının beklenen işlevi için önbellek klasörünün veya meta veri VHD'sinin yukarıdaki özniteliklere sahip olmaması önerilir.



<!--HONumber=Aug16_HO1-->


