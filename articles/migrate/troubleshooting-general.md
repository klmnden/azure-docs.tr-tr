---
title: Azure geçişi sorunlarını giderme | Microsoft Docs
description: Azure geçişi hizmeti ve sorun giderme ipuçları için sık karşılaşılan bilinen sorunlara ilişkin genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: raynew
ms.openlocfilehash: ca0931810fd78ce4cc684ad307efeb866cee3353
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47165306"
---
# <a name="troubleshoot-azure-migrate"></a>Azure Geçişi sorunlarını giderme

## <a name="troubleshoot-common-errors"></a>Sık karşılaşılan hataları giderme

[Azure geçişi](migrate-overview.md) Azure'a geçiş için şirket içi iş yüklerini değerlendirir. Dağıtma ve Azure Geçişi'ni kullanarak sorunları gidermek için bu makaleyi kullanın.

### <a name="migration-project-creation-failed-with-error-requests-must-contain-user-identity-headers"></a>Geçiş projesi oluşturma başarısız oldu, hata *istekler kullanıcı kimlik üst bilgileri içermelidir*

Kuruluşunuzun Azure Active Directory (Azure AD) kiracısına erişim sahip olmayan kullanıcılar için bu sorun oluşabilir. Azure AD kiracısı için ilk kez bir kullanıcı eklendiğinde, bu derse kiracıya katılmak için bir e-posta daveti alır. Kullanıcıların e-postasına gidip kiracıya başarıyla eklenin daveti kabul etmesi gerekir. E-postayı görmek bulamıyorsanız, Kiracı erişimi olan ve belirtilen adımları kullanarak daveti yeniden gönder isteyin zaten bir kullanıcıya ulaşın [burada](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator#resend-invitations-to-guest-users).

Davet e-posta alındığında, e-posta açın ve e-posta daveti kabul etmek için bağlantıya tıklayın gerekir. Bunu yaptıktan sonra Azure portalında oturum açmak gereken ve oturum açma yeniden, tarayıcıyı yenilemeyi çalışmaz. Ardından, geçiş projesi oluşturmayı deneyebilirsiniz.

### <a name="performance-data-for-disks-and-networks-adapters-shows-as-zeros"></a>Sıfır olarak diskler ve ağ bağdaştırıcıları için performans verilerini gösterir

VCenter sunucusundaki istatistik ayarı düzeyini küçüktür üç ayarlanırsa, bu durum oluşabilir. Düzey üç veya daha yüksek, vCenter işlem, depolama ve ağ için VM performans geçmişini saklar. VCenter küçüktür düzeyi üç için depolama ve ağ verileri, ancak yalnızca CPU ve bellek verileri depolar. Bu senaryoda, Azure Geçişi'nde performans verileri gösterildiği gibi sıfır ve Azure geçişi, diskler ve ağ şirket içi makinelerden toplanan meta verileri için boyut önerisi sağlar.

Disk ve ağ performans verileri toplamayı etkinleştirmek için istatistik ayarları düzeyini 3 olarak değiştirin. Ardından, en az ortamınızı keşfedin ve onu değerlendirmek için bir gün bekleyin.

### <a name="i-installed-agents-and-used-the-dependency-visualization-to-create-groups-now-post-failover-the-machines-show-install-agent-action-instead-of-view-dependencies"></a>Yüklenen aracıları ve bağımlılık görselleştirmesi grupları oluşturmak için kullanılan bildirimi. Şimdi yük devretme, "aracı yükleme" eylem "Bağımlılıkları görüntüle" yerine makineleri Göster gönderin.
* POST planlanmış veya planlanmamış yük devretme, şirket içi makineleri kapatılır ve eşdeğer makineler çalışmaya Azure'da başlar. Bu makineler, farklı bir MAC adresi alın. Bunlar olup kullanıcı veya şirket içi IP adresi korumak seçiminize bağlı olarak farklı bir IP adresi al. MAC ve IP adresleri farklıysa, Azure geçişi ile herhangi bir hizmet eşlemesi bağımlılık verileri şirket içi makinelerin ilişkilendirmez ve bağımlılıklarını görüntüleme yerine aracıları yüklemek için kullanıcıya sorar.
* Test yük devretme sonrası beklendiği gibi şirket içi makineleri açık kalır. Azure'da küme çalışmaya başladıktan eşdeğer makineler farklı MAC adresini ve farklı IP adresini almanızdan. Kullanıcı bu makinelerden giden Log Analytics trafiği engeller sürece, Azure geçişi şirket içi makineler hiçbir hizmet eşlemesi bağımlılık verilerle ilişkilendirmez ve bağımlılıklarını görüntüleme yerine aracıları yüklemek için kullanıcıya sorar.

### <a name="i-specified-an-azure-geography-while-creating-a-migration-project-how-do-i-find-out-the-exact-azure-region-where-the-discovered-metadata-would-be-stored"></a>Ben Azure coğrafyası, bir geçiş projesi olduğunu nasıl bulabilirim burada bulunan meta verileri depolanan tam Azure bölgesinde oluşturulurken belirtilen?

Gidebilirsiniz **Essentials** konusundaki **genel bakış** meta verilerin depolandığı konumun tam tanımlamak için proje sayfası. Konumun Azure geçişi tarafından coğrafyada rastgele seçilir ve üzerinde değişiklik yapamazsınız. Yalnızca belirli bir bölgede bir proje oluşturmak istiyorsanız, geçiş projesi oluşturabilir ve istediğiniz bölgeyi geçirmek için REST API'lerini kullanabilirsiniz.

   ![Proje konumu](./media/troubleshooting-general/geography-location.png)

## <a name="collector-errors"></a>Toplayıcı hataları

### <a name="deployment-of-collector-ova-failed"></a>OVA Toplayıcı dağıtımı başarısız oldu

OVA dağıtmak için vSphere web istemcisi kullanıyorsanız bu OVA kısmen karşıdan yüklediyseniz veya tarayıcı nedeniyle gerçekleşebilir. İndirme işleminin tamamlandığından emin olun ve farklı bir tarayıcı ile OVA dağıtmayı deneyin.

### <a name="collector-is-not-able-to-connect-to-the-internet"></a>Toplayıcı, internet'e bağlanmak mümkün değildir.

Kullanmakta olduğunuz makine bir proxy'nin arkasında olduğunda bu durum oluşabilir. Ara sunucu gerekiyorsa yetkilendirme kimlik bilgilerini sağladığınızdan emin olun.
Herhangi bir URL tabanlı güvenlik duvarı proxy'si kullanıyorsanız giden bağlantıyı denetlemek için bu URL'ler gerekli izin verilenler listesine emin olun:

**URL** | **Amacı**  
--- | ---
*.portal.azure.com | Azure hizmeti bağlantısını denetleyin ve zaman eşitleme doğrulamak için gereken sorunlar.
*.oneget.org | Gerekli powershell indirmek için vCenter Powerclı modülü temel.

**Toplayıcı, bir sertifika doğrulama hatası nedeniyle internet'e bağlanılamıyor**

Bu, Internet'e bağlanmak için bir araya giren bir proxy kullanıyorsanız ve Toplayıcı sanal makinesi proxy sertifikasını aktarılmamış olabilir. Ayrıntılı adımları kullanarak proxy sertifikasını içeri aktarabilirsiniz [burada](https://docs.microsoft.com/azure/migrate/concepts-collector#internet-connectivity).

**Toplayıcı Proje kimliği kullanarak projesine bağlanamıyor ve anahtar portaldan kopyaladım.**

Kopyalanır ve doğru bilgileri yapıştırılan emin olun. Sorunu gidermek için Microsoft Monitoring Agent (MMA) yükleyin ve MMA'yı projeye şu şekilde bağlanabildiğinizi doğrulayın:

1. Toplayıcı VM üzerinde indirme [MMA](https://go.microsoft.com/fwlink/?LinkId=828603).
2. Yüklemeyi başlatmak için indirilen dosyayı çift tıklatın.
3. Kur'da, üzerinde **Hoş Geldiniz** sayfasında **sonraki**. **Lisans Koşulları** sayfasında **Kabul Ediyorum**’a tıklayarak lisansı kabul edin.
4. İçinde **hedef klasör**, saklamak veya varsayılan yükleme klasörünü değiştirin > **sonraki**.
5. İçinde **Aracı Kurulum Seçenekleri**seçin **Azure Log Analytics** > **sonraki**.
6. Tıklayın **Ekle** yeni bir Log Analytics çalışma alanı eklemek için. Proje kimliği ve kopyaladığınız anahtarını yapıştırın. Ardından **İleri**'ye tıklayın.
7. Aracıyı projesine bağlanabildiğinizi doğrulayın. Erişilemiyorsa, ayarları doğrulayın. Aracı bağlanabilir ancak Toplayıcı kullanamazsanız, desteğe başvurun.


### <a name="error-802-date-and-time-synchronization-error"></a>802. hata: Tarih ve saat eşitleme hatası

Sunucu saati eşitleme dışı olabilir zamanla beş dakikadan fazla tarafından. Saatin Toplayıcı geçerli zamanı, aşağıdaki şekilde eşleştirmek için VM üzerinde değiştirin:

1. VM üzerinde bir yönetici komut istemi açın.
2. Saat dilimi denetlemek için w32tm /tz çalıştırın.
3. Zaman eşitlenecek w32tm/resync çalıştırın.

### <a name="vmware-powercli-installation-failed"></a>VMware Powerclı yüklenemedi

Azure geçişi toplayıcısı Powerclı indirir ve gerecinde yükler. Powerclı yüklemesindeki hata nedeniyle ulaşılamaz uç noktaları Powerclı depo için olabilir. Sorun gidermek için Powerclı Toplayıcıya aşağıdaki VM el ile yüklemeyi deneyin:

1. Windows PowerShell'i Yönetici modunda açın.
2. C:\ProgramFiles\ProfilerService\VMWare\Scripts\ dizine gidin
3. InstallPowerCLI.ps1 betiği çalıştırın

### <a name="error-unhandledexception-internal-error-occured-systemiofilenotfoundexception"></a>Hata UnhandledException Dahili hata oluştu: System.IO.FileNotFoundException

Bu, 1.0.9.5'ten önceki Toplayıcı sürümlerinde görülen bir sorundur. Toplayıcı sürümü 1.0.9.2 veya 1.0.8.59 gibi GA öncesi bir sürüm kullanıyorsanız bu sorunla karşılaşırsınız. [Ayrıntılı yanıt için burada verilen forum bağlantısını](https://social.msdn.microsoft.com/Forums/azure/en-US/c1f59456-7ba1-45e7-9d96-bae18112fb52/azure-migrate-connect-to-vcenter-server-error?forum=AzureMigrate) izleyin.

[Sorunu düzeltmek için Toplayıcınızı yükseltin](https://aka.ms/migrate/col/checkforupdates).

### <a name="error-unabletoconnecttoserver"></a>Hata UnableToConnectToServer

VCenter Server "Servername.com:9443" hatası nedeniyle bağlantı kurulamıyor: hiçbir uç noktası konumunda dinleme https://Servername.com:9443/sdk iletiyi kabul.

Toplayıcı gerecini'nın en son sürümünü, aksi halde, yükseltme gerecine denetleyin [en son sürümü](https://docs.microsoft.com/azure/migrate/concepts-collector#how-to-upgrade-collector).

Sorunu hala en son sürümde olursa, Toplayıcı makinesi belirtilen bağlantı noktası yanlış ya da belirtilen vCenter Server adını çözümleyemiyor nedeni olabilir. Bağlantı noktası belirtilmezse, varsayılan olarak, Toplayıcı bağlantı noktası numarası 443 bağlanmak çalışacaktır.

1. Toplayıcı makinesinden sunucuadı.com ping atmayı deneyin.
2. 1 adım başarısız olursa, IP adresi üzerinden vCenter Server’a bağlanmayı deneyin.
3. vCenter’a bağlanmak için doğru bağlantı noktasını belirleyin.
4. Son olarak vCenter Server’ın çalışır durumda olup olmadığını denetleyin.

## <a name="troubleshoot-dependency-visualization-issues"></a>Bağımlılık görselleştirme sorunlarını giderme

### <a name="i-installed-the-microsoft-monitoring-agent-mma-and-the-dependency-agent-on-my-on-premises-vms-but-the-dependencies-are-now-showing-up-in-the-azure-migrate-portal"></a>Şirket içi Vm'lerimi Microsoft Monitoring Agent (MMA) ve bağımlılık aracısını yükledim ancak bağımlılıkları artık Azure geçişi Portalı'nda gösterildiğini.

Aracıları yükledikten sonra Azure geçişi Portalı'nda bağımlılıkları görüntülemek için 15-30 dakika genellikle alır. 30 dakikadan uzun süre bekledi, MMA aracısını takip ederek OMS çalışma alanına konuşabilir olmasına aşağıdaki adımları:

Windows VM için:
1. Git **Denetim Masası** ve başlatma **Microsoft İzleme Aracısı**
2. Git **Azure Log Analytics (OMS)** açılır MMA özelliklerinde sekmesi
3. Emin **durumu** çalışma yeşil için.
4. Durum yeşil, değilse çalışma alanı kaldırıp MMA için yeniden eklemeyi deneyin.
        ![MMA durumu](./media/troubleshooting-general/mma-status.png)

Linux VM için MMA ve bağımlılık aracısını yükleme komutlarını başarılı olun.

### <a name="what-are-the-operating-systems-supported-by-mma"></a>MMA'yı tarafından desteklenen işletim sistemleri nelerdir?

MMA'yı tarafından desteklenen Windows işletim sistemleri listesi [burada](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems).
MMA'yı tarafından desteklenen Linux işletim sistemleri listesi [burada](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

### <a name="what-are-the-operating-systems-supported-by-dependency-agent"></a>Bağımlılık aracısı tarafından desteklenen işletim sistemleri nelerdir?

Bağımlılık aracısı tarafından desteklenen Windows işletim sistemleri listesi [burada](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems).
Bağımlılık aracısı tarafından desteklenen Linux işletim sistemleri listesi [burada](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems).

### <a name="i-am-unable-to-visualize-dependencies-in-azure-migrate-for-more-than-one-hour-duration"></a>Bir saat süresinden daha fazla bilgi için Azure Geçişi'ndeki bağımlılıkları görselleştirin oluşturamıyorum?
Azure geçişi en fazla bir saatlik süre için bağımlılıklar görmenize olanak tanır. Azure geçişi belirli bir tarihe kadar son bir ay için geçmişte dönün olanak tanısa da için bağımlılıkları görselleştirebilirsiniz en fazla süre 1 saate kadar ' dir. Örneğin, Dün için bağımlılıkları görüntülemek için bağımlılık Haritası saati süresi işlevleri kullanabilirsiniz ancak yalnızca bir için bir saat penceresinde görüntüleyebilirsiniz.

### <a name="i-am-unable-to-visualize-dependencies-for-groups-with-more-than-10-vms"></a>10'dan fazla Vm'leri gruplar için bağımlılıkları görselleştirme oluşturamıyorum?
Yapabilecekleriniz [grupları için bağımlılıkları görselleştirme](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) sahip yukarı 10 VM için 10'dan fazla vm'lerle grubunuz varsa öneririz, grupta küçük kullanıcı gruplarına bölün ve bağımlılıklarını görselleştirin.

## <a name="troubleshoot-readiness-issues"></a>Hazır olma durumu sorunlarını giderme

**Sorunu** | **Fix**
--- | ---
Desteklenmeyen önyükleme türü | Azure, EFI Önyükleme türündeki sanal makineleri desteklemez. Bir geçiş çalıştırmadan önce için BIOS önyükleme türü dönüştürmek için önerilir. <br/><br/>Kullanabileceğiniz [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/tutorial-migrate-on-premises-to-azure) , sanal Makinenin önyükleme türü için BIOS geçişi sırasında olarak tür VM'ler geçişini yapmak için.
Koşullu olarak desteklenen Windows işletim sistemi | İşletim sistemi, destek sonu tarihi geçti ve bir özel destek anlaşması (CSA) gerekiyor [destek Azure'da](https://aka.ms/WSosstatement), Azure'a geçiş yapmadan önce işletim sistemini yükseltmeyi düşünün.
Desteklenmeyen Windows İşletim Sistemi | Azure yalnızca destekler [seçili Windows işletim sistemi sürümleri](https://aka.ms/WSosstatement), Azure'a geçiş yapmadan önce makinenin işletim sistemini yükseltmeyi düşünün.
Koşullu olarak desteklenen Linux işletim sistemi | Azure yalnızca onayladığı [seçili Linux işletim sistemi sürümleri](../virtual-machines/linux/endorsed-distros.md), Azure'a geçiş yapmadan önce makinenin işletim sistemini yükseltmeyi düşünün.
Desteklenmeyen Linux İşletim Sistemi | Makine Azure'da önyüklenebilir ancak Azure tarafından işletim sistemi desteği sağlanmaz, işletim sistemine yükseltmeniz önerilir bir [Linux sürümü onaylı](../virtual-machines/linux/endorsed-distros.md) Azure'a geçirmeden önce
Bilinmeyen işletim sistemi | Sanal makinenin işletim sistemi 'Diğer' vCenter Server'da, hangi nedeniyle VM'nin Azure için hazır olma Azure geçişi tanımlanamıyor belirtildi. Makinede çalışan işletim sistemi olduğundan emin olun [desteklenen](https://aka.ms/azureoslist) makineyi geçirmeden önce Azure tarafından.
İşletim sistemi bit genişliği desteklenmiyor | 32 bit işletim sistemi ile Vm'leri Azure'da önyükleme, ancak 32-bit sanal makinenin işletim sistemini yükseltmek için önerilen için 64-bit Azure'a geçiş yapmadan önce.
Visual Studio aboneliği gerektirir. | Makinelerin bulunduğu bir Windows istemci işletim sistemi çalıştırılıyor yalnızca Visual Studio aboneliği desteklenir.
Gerekli depolama performansı için VM bulunamadı. | Azure VM desteği makine için gerekli depolama performansı (IOPS/işleme) aşıyor. Geçiş işleminden önce makine için depolama gereksinimlerini azaltır.
Gerekli ağ performansı için VM bulunamadı. | Azure VM desteği makine için gerekli ağ performansı (daraltma/genişletme) aşıyor. Makine için ağ gereksinimlerini azaltır.
VM belirtilen fiyatlandırma katmanında bulunamadı. | Fiyatlandırma katmanını standart olarak ayarlanır, Azure'a geçiş yapmadan önce VM'nin downsizing göz önünde bulundurun. Temel boyutlandırma katmanı ise değerlendirmenin fiyatlandırma katmanını standart olarak değiştirmeyi düşünün.
VM belirtilen konumda bulunamadı. | Geçiş işleminden önce farklı bir hedef konum kullanın.
Bir veya daha fazla disk uyumsuz. | VM'ye bir veya daha fazla disk Azure gereksinimlerini karşılamıyor. Sanal Makineye eklenmiş her disk için disk boyutunu < 4 TB olduğundan emin olun, aksi durumda, Azure'a geçiş yapmadan önce disk boyutunu küçültmek. Her disk tarafından ihtiyaç duyduğu performansın (IOPS/işleme) Azure tarafından desteklendiğinden emin olmak [yönetilen sanal makine diskleri](https://docs.microsoft.com/azure/azure-subscription-service-limits#storage-limits).   
Bir veya daha fazla ağ bağdaştırıcısı uyumsuz. | Makine geçişten önce kullanılmayan ağ bağdaştırıcılarını kaldırın.
Disk sayısı, sınırı aşıyor | Makine geçişten önce kullanılmayan diskleri kaldırın.
Disk boyutu, sınırı aşıyor | Azure destekler disklerle boyutu en fazla 4 TB. Geçiş işleminden önce 4 TB'den için diskleri daraltır.
Disk, belirtilen konumda kullanılamıyor | Geçiş yapmadan önce disk, hedef konumda olduğundan emin olun.
Disk, belirtilen çoğaltma için kullanılamıyor | Disk değerlendirmesi ayarları (varsayılan olarak LRS) tanımlanan yedeklilik depolama türü kullanmanız gerekir.
Bir iç hata nedeniyle disk uygunluğu belirlenemedi | Grup için yeni bir değerlendirme oluşturmayı deneyin.
Gerekli çekirdeklere ve belleğe sahip VM bulunamadı | Azure, uygun bir VM türüne ince uygulanamadı. Geçiş yapmadan önce bellek ve şirket içi makinenin çekirdek sayısını azaltın.
Bir iç hata nedeniyle VM uygunluğu belirlenemedi. | Grup için yeni bir değerlendirme oluşturmayı deneyin.
Bir iç hata nedeniyle bir veya daha fazla disk uygunluğu belirlenemedi. | Grup için yeni bir değerlendirme oluşturmayı deneyin.
Bir iç hata nedeniyle bir veya daha fazla ağ bağdaştırıcısının uygunluğu belirlenemedi. | Grup için yeni bir değerlendirme oluşturmayı deneyin.


## <a name="collect-logs"></a>Günlük toplama

**Toplayıcı VM üzerinde nasıl günlüklerini toplama?**

Günlük varsayılan olarak etkindir. Günlükleri bulunduğu konum aşağıda verilmiştir:

- C:\Profiler\ProfilerEngineDB.sqlite
- C:\Profiler\Service.log
- C:\Profiler\WebApp.log

Olay izleme için Windows toplamak için aşağıdakileri yapın:

1. Toplayıcı VM üzerinde bir PowerShell komut penceresi açın.
2. Çalıştırma **EventLog Get - günlükadı uygulama | export-csv eventlog.csv**.

**Portal ağ trafik günlüklerini nasıl topluyor mu?**

1. Tarayıcıyı açın ve gidin ve oturum açma [portalına](https://portal.azure.com).
2. Geliştirici Araçları'nı başlatmak için F12 tuşuna basın. Gerekirse ayarı temizlemek **Gezinti girdilerini temizleyin**.
3. Tıklayın **ağ** sekmesini ve ağ trafiği Yakalamayı Başlat:
 - Chrome'da, seçin **Koru günlük**. Kayıt otomatik olarak başlayacaktır. Kırmızı bir daire trafik yakalama verdiğini gösterir. Görünmüyorsa, Siyah daire başlatmak için tıklayın
 - Edge/IE, kaydı otomatik olarak başlayacaktır. Bu gereksinimleri karşılamıyorsa yeşil YÜRÜT düğmesine tıklayın.
4. Hatayı yeniden oluşturmaya çalışın.
5. Kaydetme hatası karşılaştığınız sonra kaydı durdurmak ve kayıtlı etkinliği bir kopyasını kaydedin:
 - Chrome'da, sağ tıklatıp **içerikle HAR olarak Kaydet**. Bu, zıps ve günlükleri .har dosyası olarak dışarı aktarır.
 - Edge/IE, tıklayın **dışarı aktarma yakalanan trafiği** simgesi. Bu, zıps ve günlük dışarı aktarır.
6. Gidin **konsol** için uyarıları veya hataları denetlemek için sekmesinde. Konsol günlüğüne kaydetmek için:
 - Chrome'da, konsol günlüğüne herhangi bir yere sağ tıklayın. Seçin **Kaydet**, dışarı aktarma ve günlük zip.
 - Edge/IE, sağ tıklatın ve hataları **Tümünü Kopyala**.
7. Geliştirici Araçları'nı kapatın.

## <a name="collector-error-codes-and-recommended-actions"></a>Toplayıcı hata kodları ve Önerilen Eylemler

|           |                                |                                                                               |                                                                                                       |                                                                                                                                            |
|-----------|--------------------------------|-------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Hata Kodu | Hata adı                      | İleti                                                                       | Olası nedenler                                                                                        | Önerilen eylem                                                                                                                          |
| 601       | CollectorExpired               | Toplayıcının süresi doldu.                                                        | Toplayıcının Süresi Doldu.                                                                                    | Lütfen toplayıcının yeni bir sürümünü indirip yeniden deneyin.                                                                                      |
| 751       | UnableToConnectToServer        | '%Name;' adlı vCenter Server'a şu hata nedeniyle bağlanılamıyor: %ErrorMessage;     | Daha fazla ayrıntı için hata iletisini inceleyin.                                                             | Hatayı giderip yeniden deneyin.                                                                                                           |
| 752       | InvalidvCenterEndpoint         | '%Name;' adlı sunucu bir vCenter Server değil.                                  | vCenter Server ayrıntılarını sağlayın.                                                                       | Doğru vCenter Server ayrıntılarıyla işlemi yeniden deneyin.                                                                                   |
| 753       | InvalidLoginCredentials        | '%Name;' adlı vCenter Server'a şu hata nedeniyle bağlanılamıyor: %ErrorMessage; | Geçersiz oturum açma kimlik bilgileri nedeniyle vCenter Server bağlantısı başarısız oldu.                             | Sağlanan oturum açma kimlik bilgilerinin doğru olduğundan emin olun.                                                                                    |
| 754       | NoPerfDataAvaialable           | Performans verileri kullanılamıyor.                                               | VCenter Server'da istatistik düzeyini denetleyin. Performans verilerinin kullanılabilmesi için 3 ayarlanması gerekir. | İstatistik Düzeyini 3 (5 dakika, 30 dakika ve 2 saatlik süre için) olarak değiştirin ve en az bir gün bekledikten sonra yeniden deneyin.                   |
| 756       | NullInstanceUUID               | InstanceUUID değeri null olan bir makine ile karşılaşıldı                                  | vCenter Server uygun olmayan bir nesneye sahip olabilir.                                                      | Hatayı giderip yeniden deneyin.                                                                                                           |
| 757       | VMNotFound                     | Sanal makine bulunamadı                                                  | Sanal makine silinmiş olabilir: %VMID;                                                                | vCenter envanterinin kapsamı belirlenirken seçilen sanal makinelerin keşif sırasında mevcut olduğundan emin olun                                      |
| 758       | GetPerfDataTimeout             | VCenter isteği zaman aşımına uğradı. İleti % Message;                                  | vCenter Server kimlik bilgileri yanlış                                                              | VCenter sunucusu kimlik bilgilerini denetleyin ve vCenter Server'ın erişilebilir olduğundan emin olun. İşlemi yeniden deneyin. Sorun devam ederse desteğe başvurun. |
| 759       | VmwareDllNotFound              | VMWare.Vim DLL bulunamadı.                                                     | PowerCLI düzgün bir şekilde yüklenmedi.                                                                   | Lütfen Powerclı düzgün yüklü olup olmadığını denetleyin. İşlemi yeniden deneyin. Sorun devam ederse desteğe başvurun.                               |
| 800       | ServiceError                   | Azure Geçişi Toplayıcısı hizmeti çalışmıyor.                               | Azure Geçişi Toplayıcısı hizmeti çalışmıyor.                                                       | Hizmeti services.msc kullanarak başlatın ve işlemi yeniden deneyin.                                                                             |
| 801       | PowerCLIError                  | VMware PowerCLI yüklenemedi.                                          | VMware PowerCLI yüklenemedi.                                                                  | İşlemi yeniden deneyin. Sorun devam ederse kendiniz yükleyin ve işlemi yeniden deneyin.                                                   |
| 802       | TimeSyncError                  | Saat, İnternet saat sunucusuyla eşitlenmemiş.                            | Saat, İnternet saat sunucusuyla eşitlenmemiş.                                                    | Makinedeki saatin saat dilimine göre doğru ayarlandığından emin olun ve işlemi yeniden deneyin.                                 |
| 702       | OMSInvalidProjectKey           | Geçersiz proje anahtarı belirtildi.                                                | Geçersiz proje anahtarı belirtildi.                                                                        | İşlemi doğru proje anahtarıyla yeniden deneyin.                                                                                              |
| 703       | OMSHttpRequestException        | İstek gönderilirken hata oluştu. İleti % Message;                                | Proje kimliği ile anahtarını denetleyerek uç noktanın erişilebilir olduğundan emin olun.                                       | İşlemi yeniden deneyin. Sorun devam ederse, Microsoft Destek'e başvurun.                                                                     |
| 704       | OMSHttpRequestTimeoutException | HTTP isteği zaman aşımına uğradı. İleti % Message;                                     | Uç noktanın erişilebilir olduğundan emin olmak için proje kimliği ve anahtarını kontrol edin.                                       | İşlemi yeniden deneyin. Sorun devam ederse, Microsoft Destek'e başvurun.                                                                     |
