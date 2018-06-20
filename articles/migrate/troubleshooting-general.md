---
title: Azure geçiş sorunlarını giderme | Microsoft Docs
description: Azure geçirmek hizmet ve sorun giderme ipuçları için sık karşılaşılan bilinen sorunlar genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: 896e918f6031f3bc6b925a2ecdfa2a5c82f00e0b
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36228263"
---
# <a name="troubleshoot-azure-migrate"></a>Azure Geçişi sorunlarını giderme

## <a name="troubleshoot-common-errors"></a>Sık karşılaşılan hataları giderme

[Azure geçirme](migrate-overview.md) geçiş Azure için şirket içi iş yüklerini değerlendirir. Dağıtma ve Azure geçişi kullanırken sorunları gidermek için bu makaleyi kullanın.

### <a name="migration-project-creation-failed-with-error-requests-must-contain-user-identity-headers"></a>Geçiş proje oluşturma başarısız oldu, hata *istekleri, kullanıcı kimliği üst bilgileri içermelidir*

Bu sorun, kuruluşunuzun Azure Active Directory (Azure AD) Kiracı erişimi olmayan kullanıcılar için oluşabilir. Bir kullanıcı ilk kez bir Azure AD kiracısı eklendiğinde klasöründe Kiracı katılmak için bir e-posta davet alır. Kullanıcıların e-postasına gidip Kiracı için başarıyla eklendi için daveti kabul etmesi gerekir. Zaten Kiracı erişimi ve davet belirtilen adımları kullanarak yeniden göndermeyi isteyin bir kullanıcıyla e-posta görmek erişemiyorsanız ulaşmak [burada](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator#resend-invitations-to-guest-users).

Davet e-posta alındıktan sonra e-posta açın ve daveti kabul etmek için e-postadaki bağlantıya gerekir. Bunu yaptıktan sonra Azure portalında oturum açmanız gerekir ve oturum açma yeniden, tarayıcıyı yenilemeyi çalışmaz. Ardından, geçiş proje oluşturmayı deneyebilirsiniz.

### <a name="performance-data-for-disks-and-networks-adapters-shows-as-zeros"></a>Sıfır olarak diskler ve ağ bağdaştırıcıları için performans verilerini gösterir

VCenter sunucusu istatistikleri ayarı düzeyinde değerinden üç ayarlandıysa, bu durum oluşabilir. Düzey üç veya daha yüksek, vCenter işlem, depolama ve ağ için VM performans geçmişini saklar. Küçüktür düzeyi üç için vCenter depolama ve ağ verileri, ancak yalnızca CPU ve bellek verileri depolamaz. Bu senaryoda, performans verileri gösterildiği gibi Azure geçirmek sıfır ve Azure geçiş diskleri ve şirket içi makinelerden toplanan meta verileri temel ağlar için boyutu öneri sağlar.

Disk ve ağ performans verileri koleksiyonunu etkinleştirmek için üç istatistikleri ayarları düzeyini değiştirin. Daha sonra en az bir gün onu değerlendirmek ortamınız bulmak için bekleyin.

### <a name="i-installed-agents-and-used-the-dependency-visualization-to-create-groups-now-post-failover-the-machines-show-install-agent-action-instead-of-view-dependencies"></a>Yüklenen aracıları ve bağımlılık görselleştirme grupları oluşturmak için kullanılan bildirimi. Artık yük devretme, "Görünüm bağımlılıkları" yerine "aracı yükleme" eylemi makineler Göster sonrası
* POST planlanmış veya planlanmamış bir yük devretme şirket içi makineler kapalıdır ve eşdeğer makineler çalışmaya Azure'da başlar. Bu makineleri farklı bir MAC adresi alın. Bunlar Sunucusu'ndan olup kullanıcı veya şirket içi IP adresini korumak seçiminize göre göre farklı bir IP adresi. MAC ve IP adreslerini farklıysa, Azure geçirmek şirket içi makineler tüm hizmet Haritası bağımlılık verilerle ilişkilendirmez ve bağımlılıklarını görüntüleme yerine aracıları yüklemek için kullanıcıya sorar.
* Test yük devretme sonrası beklendiği gibi şirket içi makineler açık kalır. Eşdeğer makineleri Azure üzerinde çalışmaya başlar, farklı bir MAC adresi edinmeli ve farklı bir IP adresi elde. Kullanıcı bu makinelere giden günlük analizi trafiği engelleyen sürece, Azure geçirmek şirket içi makineler tüm hizmet Haritası bağımlılık verilerle ilişkilendirmez ve bağımlılıklarını görüntüleme yerine aracıları yüklemek için kullanıcıya sorar.

## <a name="collector-errors"></a>Toplayıcı hataları

### <a name="deployment-of-collector-ova-failed"></a>Toplayıcı OVA dağıtımı başarısız oldu

OVA dağıtmak için vSphere web istemcisi kullanıyorsanız bu OVA kısmen indirdiyseniz, veya tarayıcı nedeniyle meydana gelmiş olabilir. Yükleme işleminin tamamlandığından emin olun ve farklı bir tarayıcı ile OVA dağıtmayı deneyin.

### <a name="collector-is-not-able-to-connect-to-the-internet"></a>Toplayıcı internet'e bağlanabiliyor değil

Bu durum, kullanmakta olduğunuz makine bir proxy'nin arkasında olduğunda ortaya çıkar. Proxy gerekiyorsa yetkilendirme kimlik bilgilerini sağladığınızdan emin olun.
Herhangi bir URL tabanlı bir güvenlik duvarı proxy kullanıyorsanız giden bağlantıyı denetlemek için bu URL'leri gerekli beyaz liste ile emin olun:

**URL** | **Amacı**  
--- | ---
*.portal.azure.com | Azure hizmetiyle bağlantısını denetleyin ve zaman eşitlemesini doğrulamak için gerekli verir.
*.oneget.org | Gerekli powershell indirmek için vCenter Powerclı modülü temel.

**Toplayıcı Proje kimliği kullanarak proje bağlanamaz ve anahtar ı portaldan kopyalanır.**

Kopyalanır ve doğru bilgileri yapıştırılan emin olun. Sorunu gidermek için Microsoft İzleme Aracısı'nı (MMA) yükleyin ve MMA projeye şu şekilde bağlanabildiğinizi doğrulayın:

1. Toplayıcı VM üzerinde karşıdan [MMA](https://go.microsoft.com/fwlink/?LinkId=828603).
2. Yüklemeyi başlatmak için indirilen dosyayı çift tıklatın.
3. Kur'da, üzerinde **Hoş Geldiniz** sayfasında, **sonraki**. **Lisans Koşulları** sayfasında **Kabul Ediyorum**’a tıklayarak lisansı kabul edin.
4. İçinde **hedef klasörü**, saklamak veya varsayılan yükleme klasörünü değiştirmek > **sonraki**.
5. İçinde **aracı Kur Seçenekleri**seçin **Azure günlük analizi** > **sonraki**.
6. Tıklatın **Ekle** yeni bir günlük analizi çalışma alanı eklemek için. Proje kimliği ve kopyaladığınız anahtarını yapıştırın. Ardından **İleri**'ye tıklayın.
7. Aracı projeye bağlanabildiğinizi doğrulayın. Başaramazsa ayarlarını doğrulayın. Aracı bağlanabilirsiniz ancak Toplayıcı kullanamazsanız, desteğe başvurun.


### <a name="error-802-date-and-time-synchronization-error"></a>802. hata: Tarih ve saat eşitleme hatası

Sunucu saati eşitleme olabilir geçerli saatle beş dakikadan fazla ile. Geçerli saat gibi eşleşecek şekilde VM toplayıcısında saatin değiştirin:

1. VM bir yönetici komut istemi açın.
2. Saat dilimi denetlemek için w32tm /tz çalıştırın.
3. Zaman eşitleme için w32tm/resync çalıştırın.

### <a name="vmware-powercli-installation-failed"></a>VMware Powerclı yüklemesi başarısız oldu

Azure geçirme Toplayıcı Powerclı indirir ve aygıtındaki yükler. Powerclı yüklemede hata Powerclı depo ulaşılamaz uç noktaları nedeniyle olabilir. Sorun giderme Powerclı Toplayıcı aşağıdaki adımı kullanarak VM el ile yüklemeyi deneyin:

1. Windows PowerShell'i Yönetici modunda açın
2. C:\ProgramFiles\ProfilerService\VMWare\Scripts\ dizinine gidin
3. InstallPowerCLI.ps1 komut dosyasını çalıştır

### <a name="error-unhandledexception-internal-error-occured-systemiofilenotfoundexception"></a>Hata UnhandledException iç hata oluştu: System.IO.FileNotFoundException

Bu, 1.0.9.5'ten önceki Toplayıcı sürümlerinde görülen bir sorundur. Toplayıcı sürümü 1.0.9.2 veya 1.0.8.59 gibi GA öncesi bir sürüm kullanıyorsanız bu sorunla karşılaşırsınız. [Ayrıntılı yanıt için burada verilen forum bağlantısını](https://social.msdn.microsoft.com/Forums/azure/en-US/c1f59456-7ba1-45e7-9d96-bae18112fb52/azure-migrate-connect-to-vcenter-server-error?forum=AzureMigrate) izleyin.

[Sorunu düzeltmek için Toplayıcınızı yükseltin](https://aka.ms/migrate/col/checkforupdates).

### <a name="error-unabletoconnecttoserver"></a>Hata UnableToConnectToServer

VCenter Server "Servername.com:9443" hatası nedeniyle bağlantı kurulamadı: adresinde dinlemede hiçbir uç noktası https://Servername.com:9443/sdk ileti kabul.

Toplayıcı Gereci en son sürümünü çalıştırıyorsanız, değilseniz Gereci yükseltme denetleyin [en son sürümünü](https://docs.microsoft.com/azure/migrate/concepts-collector#how-to-upgrade-collector).

Sorun hala en son sürümde olursa, Toplayıcı makine vCenter sunucusu adı belirtilen veya belirtilen bağlantı noktası yanlış çözümleyemiyor nedeni olabilir. Bağlantı noktası belirtilmezse, varsayılan olarak 443 numaralı bağlantı noktası numarası bağlanmak Toplayıcı dener.

1. Toplayıcı makineden sunucuadı.com ping işlemi yapmayı deneyin.
2. 1 adım başarısız olursa, IP adresi üzerinden vCenter Server’a bağlanmayı deneyin.
3. vCenter’a bağlanmak için doğru bağlantı noktasını belirleyin.
4. Son olarak vCenter Server’ın çalışır durumda olup olmadığını denetleyin.

## <a name="troubleshoot-readiness-issues"></a>Hazır olma durumu sorunlarını giderme

**Sorunu** | **Fix**
--- | ---
Desteklenmeyen önyükleme türü | Azure VM'ler EFI Önyükleme türüyle desteklemez. Bir geçiş çalıştırmadan önce için BIOS önyükleme türüne dönüştürmek için önerilir. <br/><br/>Kullanabileceğiniz [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/tutorial-migrate-on-premises-to-azure) , VM önyükleme türünü geçiş sırasında BIOS'a dönüştürecektir gibi böyle VM'ler geçişini yapmak için.
Koşullu desteklenen Windows işletim sistemi | İşletim sistemi sonunu destek tarihi geçtikten ve bir özel destek sözleşmesi (CSA) için gereken [destek Azure'da](https://aka.ms/WSosstatement), Azure'a geçirmeden önce işletim sistemi yükseltme yapmayı düşünün.
Desteklenmeyen Windows İşletim Sistemi | Azure destekleyen yalnızca [seçili Windows işletim sistemi sürümleri](https://aka.ms/WSosstatement), Azure'a geçirmeden önce makine işletim sistemini yükseltme yapmayı düşünün.
Koşullu olarak desteklenen Linux işletim sistemi | Azure Symantec'in yalnızca [seçili Linux işletim sistemi sürümleri](../virtual-machines/linux/endorsed-distros.md), Azure'a geçirmeden önce makine işletim sistemini yükseltme yapmayı düşünün.
Desteklenmeyen Linux İşletim Sistemi | Makine Azure'da önyükleme, ancak hiçbir işletim sistemi desteği Azure tarafından sağlanan, işletim sistemine yükseltin bir [Linux sürüm destekli](../virtual-machines/linux/endorsed-distros.md) Azure'a geçirmeden önce
Bilinmeyen işletim sistemi | VM işletim sistemi 'Diğer' vCenter Server'da, hangi nedeniyle VM Azure hazırlık Azure geçirmek tanımlayamıyor belirtildi. Makinede çalışan işletim sistemi olduğundan emin olun [desteklenen](https://aka.ms/azureoslist) makineyi geçirmeden önce Azure tarafından.
İşletim sistemi bit genişliği desteklenmiyor | 32-bit işletim sistemi ile sanal makineleri Azure'da önyükleme, ancak 32-bit VM işletim sistemini yükseltmek için önerilen için 64-bit Azure'a geçirmeden önce.
Visual Studio aboneliği gerektirir. | Makine içeren bir Windows istemci işletim sistemi içinde çalıştığı yalnızca Visual Studio abonelikte desteklenmiyor.
VM için gerekli depolama performansı bulunamadı. | Makine için gerekli depolama performansı (IOPS/üretilen iş) Azure VM destek aşıyor. Geçiş işleminden önce makine için depolama gereksinimlerini azaltır.
VM için gerekli ağ performansını bulunamadı. | Makine için gereken ağ performansı (çıkış) Azure VM destek aşıyor. Makine için ağ gereksinimlerini azaltır.
VM belirtilen fiyatlandırma katmanında bulunamadı. | Fiyatlandırma katmanı standart olarak ayarlanırsa, Azure'a geçirmeden önce VM downsizing göz önünde bulundurun. Boyutlandırma katmanı Basic ise, değerlendirme fiyatlandırma katmanı standart olarak değiştirmeyi düşünün.
VM belirtilen konumda bulunamadı. | Geçişten önce farklı bir hedef konum kullanın.
Bir veya daha fazla uygun diskler. | VM'ye bağlı bir veya daha fazla disk Azure gereksinimleri karşılamıyor. VM'ye bağlı her disk için disk boyutu < 4 TB olduğundan emin olun, aksi durumda, Azure'a geçirmeden önce disk boyutunu küçültmek. Her disk tarafından gerekli performans (IOPS/üretilen iş) Azure tarafından desteklendiğinden emin olun [yönetilen sanal makine disklerini](https://docs.microsoft.com/azure/azure-subscription-service-limits#storage-limits).   
Bir veya daha fazla uygun ağ bağdaştırıcıları. | Geçişten önce makineden kullanılmayan ağ bağdaştırıcılarını kaldırın.
Disk sayısı, sınırı aşıyor | Kullanılmayan disklerin geçiş işleminden önce makine kaldırın.
Disk boyutu, sınırı aşıyor | Azure destekleyen sahip bir disk boyutu en fazla 4 TB. Disklerin geçiş işleminden önce 4 TB değerinden küçük küçültün.
Disk, belirtilen konumda kullanılamıyor | Geçirmeden önce hedef konumda disk olduğundan emin olun.
Disk, belirtilen çoğaltma için kullanılamıyor | Disk değerlendirme ayarlar (varsayılan olarak LRS) tanımlı artıklık depolama türünü kullanmanız gerekir.
Bir iç hata nedeniyle disk uygunluğu belirlenemedi | Grubu için yeni bir değerlendirme oluşturmayı deneyin.
Gerekli çekirdeklere ve belleğe sahip VM bulunamadı | Azure, uygun bir VM türüne ince uygulanamadı. Geçirmeden önce bellek ve şirket içi makineyi çekirdek sayısını azaltın.
İç hata nedeniyle VM uygunluğu belirleyemedi. | Grubu için yeni bir değerlendirme oluşturmayı deneyin.
İç hata nedeniyle bir veya daha fazla disk uygunluğuna belirleyemedi. | Grubu için yeni bir değerlendirme oluşturmayı deneyin.
İç hata nedeniyle bir veya daha fazla ağ bağdaştırıcısı uygunluğuna belirleyemedi. | Grubu için yeni bir değerlendirme oluşturmayı deneyin.


## <a name="collect-logs"></a>Günlüklerini toplayın

**VM toplayıcısında nasıl günlükleri toplamak?**

Günlüğe kaydetme varsayılan olarak etkindir. Günlükleri aşağıdaki gibi bulunur:

- C:\Profiler\ProfilerEngineDB.sqlite
- C:\Profiler\Service.log
- C:\Profiler\WebApp.log

Windows için olay izleme toplamak için aşağıdakileri yapın:

1. VM toplayıcısı üzerinde bir PowerShell komut penceresi açın.
2. Çalıştırma **Get-EventLog - günlükadı uygulama | export-csv eventlog.csv**.

**Portal ağ trafiği günlüklerini nasıl topluyor mu?**

1. Tarayıcıyı açın ve gidin ve oturum açma [portalı](https://portal.azure.com).
2. Geliştirici Araçları başlatmak için F12 tuşuna basın. Gerekirse, ayarı temizlemek **temizleyin Gezinti girişlerde**.
3. Tıklatın **ağ** sekmesini tıklatın ve ağ trafiğini yakalama Başlat:
 - Chrome'seçin **Koru günlük**. Kayıt otomatik olarak başlamanız gerekir. Kırmızı bir daire trafiği yakalama yapılıyor gösterir. Görünmüyorsa, siyah bir daire başlatmak için tıklatın
 - Edge/IE'de kaydı otomatik olarak başlamanız gerekir. Seçili değilse, yeşil Oynat düğmesini tıklatın.
4. Hata yeniden oluşturmayı deneyin.
5. Kaydedilirken hatayla sonra durdurmanıza ve kaydedilen etkinliği bir kopyasını kaydedin:
 - Chrome'sağ tıklatın ve **içerikle HAR Kaydet**. Bu zips ve günlükleri .har dosyası olarak dışarı aktarır.
 - Edge/IE'de tıklatın **verme yakalanan trafiği** simgesi. Bu zips ve günlük dışa aktarır.
6. Gidin **konsol** sekmesi tüm uyarılar veya hatalar için denetleyin. Konsol günlüğe kaydetmek için:
 - Chrome'konsol günlüğünde herhangi bir yere sağ tıklayın. Seçin **Farklı Kaydet**, aktarın ve günlük zip.
 - Edge/IE'de sağ tıklatın ve hataları **tüm kopyalayın**.
7. Geliştirici Araçları'nı kapatın.

## <a name="collector-error-codes-and-recommended-actions"></a>Toplayıcı hata kodları ve Önerilen Eylemler

|           |                                |                                                                               |                                                                                                       |                                                                                                                                            |
|-----------|--------------------------------|-------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Hata Kodu | Hata adı                      | İleti                                                                       | Olası nedenler                                                                                        | Önerilen eylem                                                                                                                          |
| 601       | CollectorExpired               | Toplayıcının süresi doldu.                                                        | Toplayıcının Süresi Doldu.                                                                                    | Lütfen toplayıcının yeni bir sürümünü indirip yeniden deneyin.                                                                                      |
| 751       | UnableToConnectToServer        | '%Name;' adlı vCenter Server'a şu hata nedeniyle bağlanılamıyor: %ErrorMessage;     | Daha fazla ayrıntı için hata iletisini inceleyin.                                                             | Hatayı giderip yeniden deneyin.                                                                                                           |
| 752       | InvalidvCenterEndpoint         | '%Name;' adlı sunucu bir vCenter Server değil.                                  | vCenter Server ayrıntılarını sağlayın.                                                                       | Doğru vCenter Server ayrıntılarıyla işlemi yeniden deneyin.                                                                                   |
| 753       | InvalidLoginCredentials        | '%Name;' adlı vCenter Server'a şu hata nedeniyle bağlanılamıyor: %ErrorMessage; | Geçersiz oturum açma kimlik bilgileri nedeniyle vCenter Server bağlantısı başarısız oldu.                             | Sağlanan oturum açma kimlik bilgilerinin doğru olduğundan emin olun.                                                                                    |
| 754       | NoPerfDataAvaialable           | Performans verileri kullanılamıyor.                                               | VCenter sunucusu istatistikleri düzeyini denetleyin. Performans verileri kullanılabilir olması için 3 ayarlanması gerekir. | İstatistik Düzeyini 3 (5 dakika, 30 dakika ve 2 saatlik süre için) olarak değiştirin ve en az bir gün bekledikten sonra yeniden deneyin.                   |
| 756       | NullInstanceUUID               | InstanceUUID değeri null olan bir makine ile karşılaşıldı                                  | vCenter Server uygun olmayan bir nesneye sahip olabilir.                                                      | Hatayı giderip yeniden deneyin.                                                                                                           |
| 757       | VMNotFound                     | Sanal makine bulunamadı                                                  | Sanal makine silinmiş olabilir: %VMID;                                                                | vCenter envanterinin kapsamı belirlenirken seçilen sanal makinelerin keşif sırasında mevcut olduğundan emin olun                                      |
| 758       | GetPerfDataTimeout             | VCenter istek zaman aşımına uğradı. İleti % Message;                                  | vCenter Server kimlik bilgileri yanlış                                                              | VCenter sunucusu kimlik bilgilerini denetleyin ve bu vCenter sunucusunun erişilebilir olduğundan emin olun. İşlemi yeniden deneyin. Sorun devam ederse, desteğe başvurun. |
| 759       | VmwareDllNotFound              | VMWare.Vim DLL bulunamadı.                                                     | PowerCLI düzgün bir şekilde yüklenmedi.                                                                   | Lütfen Powerclı düzgün yüklü olup olmadığını denetleyin. İşlemi yeniden deneyin. Sorun devam ederse, desteğe başvurun.                               |
| 800       | ServiceError                   | Azure Geçişi Toplayıcısı hizmeti çalışmıyor.                               | Azure Geçişi Toplayıcısı hizmeti çalışmıyor.                                                       | Hizmeti services.msc kullanarak başlatın ve işlemi yeniden deneyin.                                                                             |
| 801       | PowerCLIError                  | VMware PowerCLI yüklenemedi.                                          | VMware PowerCLI yüklenemedi.                                                                  | İşlemi yeniden deneyin. Sorun devam ederse, el ile yükleyin ve işlemi yeniden deneyin.                                                   |
| 802       | TimeSyncError                  | Saat, İnternet saat sunucusuyla eşitlenmemiş.                            | Saat, İnternet saat sunucusuyla eşitlenmemiş.                                                    | Makinedeki saatin saat dilimine göre doğru ayarlandığından emin olun ve işlemi yeniden deneyin.                                 |
| 702       | OMSInvalidProjectKey           | Geçersiz proje anahtarı belirtildi.                                                | Geçersiz proje anahtarı belirtildi.                                                                        | İşlemi doğru proje anahtarıyla yeniden deneyin.                                                                                              |
| 703       | OMSHttpRequestException        | İsteği gönderilirken hata oluştu. İleti % Message;                                | Proje kimliği ile anahtarını denetleyerek uç noktanın erişilebilir olduğundan emin olun.                                       | İşlemi yeniden deneyin. Sorun devam ederse, Microsoft Destek'e başvurun.                                                                     |
| 704       | OMSHttpRequestTimeoutException | HTTP isteği zaman aşımına uğradı. İleti % Message;                                     | Uç noktanın erişilebilir olduğundan emin olmak için proje kimliği ve anahtarını kontrol edin.                                       | İşlemi yeniden deneyin. Sorun devam ederse, Microsoft Destek'e başvurun.                                                                     |
