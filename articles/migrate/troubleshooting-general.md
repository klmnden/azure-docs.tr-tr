---
title: Azure geçişi sorunlarını giderme | Microsoft Docs
description: Azure geçişi hizmeti ve sorun giderme ipuçları için sık karşılaşılan bilinen sorunlara ilişkin genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: raynew
ms.openlocfilehash: dff3c96cf3ac8eea7c1160ee1834cc70390c0333
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58652646"
---
# <a name="troubleshoot-azure-migrate"></a>Azure Geçişi sorunlarını giderme

## <a name="troubleshoot-common-errors"></a>Sık karşılaşılan hataları giderme

[Azure geçişi](migrate-overview.md) Azure'a geçiş için şirket içi iş yüklerini değerlendirir. Dağıtma ve Azure Geçişi'ni kullanarak sorunları gidermek için bu makaleyi kullanın.

### <a name="i-am-using-the-ova-that-continuously-discovers-my-on-premises-environment-but-the-vms-that-are-deleted-in-my-on-premises-environment-are-still-being-shown-in-the-portal"></a>Sürekli olarak şirket içi ortamımın bulur OVA kullanıyorum, ancak şirket içi ortamımın silinir Vm'leri hala portalında gösterilir.

Sürekli bulma Gereci yalnızca performans verilerini sürekli olarak toplar, şirket içi ortamda (yani VM ekleme, silme, disk ekleme vb.) herhangi bir yapılandırma değişikliği algılamaz. Şirket içi ortamda bir yapılandırma değişikliği gerçekleşirse değişikliklerin portala yansıması için aşağıdakileri yapabilirsiniz:

- Ayrıca öğeleri (VM'ler, diskler ve çekirdek vb.): Azure portalında bu değişiklikleri yansıtacak şekilde gereç keşiften durdurun ve yeniden başlatın. Bu, değişikliklerin Azure Geçişi projesinde güncelleştirilmesini sağlar.

   ![Keşfi durdur](./media/troubleshooting-general/stop-discovery.png)

- VM silme: Bulma durdurup bile gereç tasarlandığı şekilde nedeniyle, VM'ler silinmesini yansıtılmaz. Bunun nedeni takip eden keşiflerin eski keşiflerin üzerine yazılması yerine bunlara eklenmesidir. Bu durumda grubunuzdan kaldırarak ve değerlendirmeyi yeniden hesaplayarak portaldaki VM’yi yoksayabilirsiniz.

### <a name="deletion-of-azure-migrate-projects-and-associated-log-analytics-workspace"></a>Azure geçişi projeleri ve ilişkili Log Analytics çalışma alanını silme

Bir Azure geçişi projesi sildiğinizde grupları ve değerlendirmeler ile birlikte geçiş projesini siler. Projeye bir Log Analytics çalışma alanı eklediyseniz, ancak bunu otomatik olarak Log Analytics çalışma alanı silinmez. Aynı Log Analytics çalışma alanı için birden çok kullanım örnekleri kullanılabilir olmasıdır. De Log Analytics çalışma alanını silmek istiyorsanız, el ile yapmanız gerekir.

1. Projeye bağlı Log Analytics çalışma alanına göz atın.
   a. Geçiş projesi henüz silmemeniz, bağlantıyı çalışma alanına proje genel bakış sayfasından temel bileşenler bölümünde bulabilirsiniz.

   ![LA çalışma](./media/troubleshooting-general/LA-workspace.png)

   b. Geçiş projesi zaten silinmişse tıklayın **kaynak grupları** sol bölmesinde Azure portalında ve çalışma alanı oluşturuldu ve buna göz atamaz kaynak grubuna gidin.
2. Yönergeleri izleyerek [bu makaledeki](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace) çalışma alanını silmek için.

### <a name="migration-project-creation-failed-with-error-requests-must-contain-user-identity-headers"></a>Geçiş projesi oluşturma başarısız oldu, hata *istekler kullanıcı kimlik üst bilgileri içermelidir*

Kuruluşunuzun Azure Active Directory (Azure AD) kiracısına erişim sahip olmayan kullanıcılar için bu sorun oluşabilir. Azure AD kiracısı için ilk kez bir kullanıcı eklendiğinde, bu derse kiracıya katılmak için bir e-posta daveti alır. Kullanıcıların e-postasına gidip kiracıya başarıyla eklenin daveti kabul etmesi gerekir. E-postayı görmek bulamıyorsanız, Kiracı erişimi olan ve belirtilen adımları kullanarak daveti yeniden gönder isteyin zaten bir kullanıcıya ulaşın [burada](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator#resend-invitations-to-guest-users).

Davet e-posta alındığında, e-posta açın ve e-posta daveti kabul etmek için bağlantıya tıklayın gerekir. Bunu yaptıktan sonra Azure portalında oturum açmak gereken ve oturum açma yeniden, tarayıcıyı yenilemeyi çalışmaz. Ardından, geçiş projesi oluşturmayı deneyebilirsiniz.

### <a name="i-am-unable-to-export-the-assessment-report"></a>Değerlendirme raporunu dışarı aktaramıyor istiyorum

Portaldan değerlendirme raporunu dışarı bulamıyorsanız, kullanmayı deneyin için değerlendirme raporu indirme URL'sini almak için REST API altında.

1. Yükleme *armclient* (, zaten yüklü değilse), bu bilgisayarda:

   a. Bir yönetici komut istemi penceresinde aşağıdaki komutu çalıştırın: ```@powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"```

   b. Bir yönetici Windows PowerShell penceresinde aşağıdaki komutu çalıştırın: ```choco install armclient```

2. Azure geçişi REST API'sini kullanarak değerlendirme raporu için indirme URL'sini alma

   a.    Bir yönetici Windows PowerShell penceresinde aşağıdaki komutu çalıştırın: ```armclient login```

        This opens the Azure login pop-up where you need to sign in to Azure.

   b.    Aynı PowerShell penceresinde değerlendirme raporu (aşağıda istek URI parametreleri örnek API uygun değerlerle değiştirin) indirme URL'sini almak için aşağıdaki komutu çalıştırın

       ```armclient POST https://management.azure.com/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/Microsoft.Migrate/projects/{projectName}/groups/{groupName}/assessments/{assessmentName}/downloadUrl?api-version=2018-02-02```

      Örnek istek ve çıktı:

      ```PS C:\WINDOWS\system32> armclient POST https://management.azure.com/subscriptions/8c3c936a-c09b-4de3-830b-3f5f244d72e9/r
   esourceGroups/ContosoDemo/providers/Microsoft.Migrate/projects/Demo/groups/contosopayroll/assessments/assessment_11_16_2
   018_12_16_21/downloadUrl?api-version=2018-02-02
   {
   "assessmentReportUrl": "https://migsvcstoragewcus.blob.core.windows.net/4f7dddac-f33b-4368-8e6a-45afcbd9d4df/contosopayrollassessment_11_16_2018_12_16_21?sv=2016-05-31&sr=b&sig=litQmHuwi88WV%2FR%2BDZX0%2BIttlmPMzfVMS7r7dULK7Oc%3D&st=2018-11-20T16%3A09%3A30Z&se=2018-11-20T16%3A19%3A30Z&sp=r",
   "expirationTime": "2018-11-20T22:09:30.5681954+05:30"```

3. Yanıttan URL'sini kopyalayın ve değerlendirme raporunu indirmek için bir tarayıcıda açın.

4. Rapor indirme işlemi tamamlandığınızda Excel indirilen klasöre göz atın ve dosyayı görüntülemek için Excel'de açmak için kullanın.

### <a name="performance-data-for-cpu-memory-and-disks-is-showing-up-as-zeroes"></a>CPU, bellek ve disk için performans verilerini sıfır olarak gösteriyor

Azure geçişi, şirket içi VM'lerin performans verilerini toplamak için şirket içi ortamı sürekli olarak profiller. Ortamınızın bulma yeni başlattıysanız, performans verileri toplama yapılacak en az bir gün beklemeniz gerekir. Değerlendirme, bir gün için beklemenize gerek kalmadan oluşturulursa performans ölçümleri sıfır olarak gösterilir. Bir gün bekledikten sonra yeni değerlendirme oluşturun veya değerlendirme raporu 'Yeniden Hesapla' seçeneğini kullanarak mevcut değerlendirme güncelleştirin.

### <a name="i-specified-an-azure-geography-while-creating-a-migration-project-how-do-i-find-out-the-exact-azure-region-where-the-discovered-metadata-would-be-stored"></a>Ben Azure coğrafyası, bir geçiş projesi olduğunu nasıl bulabilirim burada bulunan meta verileri depolanan tam Azure bölgesinde oluşturulurken belirtilen?

Gidebilirsiniz **Essentials** konusundaki **genel bakış** meta verilerin depolandığı konumun tam tanımlamak için proje sayfası. Konumun Azure geçişi tarafından coğrafyada rastgele seçilir ve üzerinde değişiklik yapamazsınız. Yalnızca belirli bir bölgede bir proje oluşturmak istiyorsanız, geçiş projesi oluşturabilir ve istediğiniz bölgeyi geçirmek için REST API'lerini kullanabilirsiniz.

   ![Proje konumu](./media/troubleshooting-general/geography-location.png)

## <a name="collector-issues"></a>Toplayıcı sorunları

### <a name="deployment-of-azure-migrate-collector-failed-with-the-error-the-provided-manifest-file-is-invalid-invalid-ovf-manifest-entry"></a>Azure geçişi toplayıcısı dağıtımı işlemi şu hatayla başarısız oldu: Sağlanan bildirim dosyası geçersiz: Geçersiz OVF bildirim girişi.

1. Azure geçişi toplayıcısı OVA dosyasını doğru karma değerini denetleyerek indirilir olmadığını doğrulayın. Karma değerini doğrulamak için [bu makaleye](https://docs.microsoft.com/azure/migrate/tutorial-assessment-vmware#verify-the-collector-appliance) bakın. Karma değeriyle eşleşmiyor, tekrar OVA dosyasını indirmek ve dağıtımı yeniden deneyin.
2. Yine başarısız olursa ve OVF dağıtımı için VMware vSphere Client uygulamasını kullanıyorsanız vSphere Web Client üzerinden dağıtmayı deneyin. Yine başarısız olursa farklı web tarayıcısı kullanarak deneyin.
3. VSphere web istemcisi kullanarak ve vCenter Server 6.5 veya 6.7 dağıtılmaya çalışılırken, izleyerek doğrudan ESXi ana bilgisayarındaki OVA dağıtmayı deneyin aşağıdaki adımları:
   - ESXi Konağı (yerine doğrudan vCenter Server) bağlanma web istemcisi kullanarak (https:// <*ana bilgisayar IP adresi*> /ui)
   - Giriş gidin > envanteri
   - Dosyaya tıklayın > dağıtma OVF şablonu > OVA için göz atın ve dağıtımı tamamlandı
4. Dağıtım yine başarısız olursa Azure geçişi desteğe başvurun.

### <a name="unable-to-select-the-azure-cloud-in-the-appliance-fails-with-error-azure-cloud-selection-failed"></a>Gereç, hata ile başarısız oluyor, bulut "Azure bulut seçimi başarısız oldu" Unable to Azure'ı seçin

Bu bilinen bir sorundur ve sorun için bir düzeltme kullanılabilir. Lütfen indirin [BITS'en son yükseltme](https://docs.microsoft.com/azure/migrate/concepts-collector-upgrade#continuous-discovery-upgrade-versions) Gereci ve güncelleştirme düzeltmesi uygulamak için gereç için.

### <a name="collector-is-not-able-to-connect-to-the-internet"></a>Toplayıcı, internet'e bağlanmak mümkün değildir.

Kullanmakta olduğunuz makine bir proxy'nin arkasında olduğunda bu durum oluşabilir. Ara sunucu gerekiyorsa yetkilendirme kimlik bilgilerini sağladığınızdan emin olun.
Herhangi bir URL tabanlı güvenlik duvarı proxy'si kullanıyorsanız giden bağlantıyı denetlemek için bu URL'ler gerekli izin verilenler listesine emin olun:

**URL** | **Amacı**  
--- | ---
*.portal.azure.com | Azure hizmeti bağlantısını denetleyin ve zaman eşitleme doğrulamak için gereken sorunlar.
*.oneget.org | Gerekli powershell indirmek için vCenter Powerclı modülü temel.

**Toplayıcı, bir sertifika doğrulama hatası nedeniyle internet'e bağlanılamıyor**

Bu, Internet'e bağlanmak için bir araya giren bir proxy kullanıyorsanız ve Toplayıcı sanal makinesi proxy sertifikasını aktarılmamış olabilir. Ayrıntılı adımları kullanarak proxy sertifikasını içeri aktarabilirsiniz [burada](https://docs.microsoft.com/azure/migrate/concepts-collector).

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

### <a name="error-unhandledexception-internal-error-occurred-systemiofilenotfoundexception"></a>Hata UnhandledException Dahili hata oluştu: System.IO.FileNotFoundException

Bu sorun, VMware powerclı'yı yükleme ile ilgili bir sorun nedeniyle oluşabilir. İzleyin sorunu çözmek için aşağıdaki adımları:

1. Toplayıcı gerecini en son sürümüne bağımlı değilse [en son sürüme Toplayıcınızı yükseltin](https://aka.ms/migrate/col/checkforupdates) ve sorunun çözülüp çözülmediğini denetleyin.
2. En son Toplayıcı sürümü zaten varsa izleyin Powerclı temiz bir yüklemesi yapmak için aşağıdaki adımları:

   a. Gereç web tarayıcısında kapatın.

   b. Windows Hizmet Yöneticisi için (Windows Hizmet Yöneticisi'ni açmak için Aç 'Çalışma' ve türü services.msc) giderek 'Azure geçişi Toplayıcısı' hizmetini durdurun. Azure geçişi Toplayıcısı hizmeti üzerinde sağ tıklatın ve Durdur'u tıklatın.

   c. Aşağıdaki konumlardan 'VMware' ile başlayan tüm klasörleri Sil: C:\Program Files\WindowsPowerShell\Modules  
        C:\Program Files (x86)\WindowsPowerShell\Modules

   d. 'Azure geçişi Toplayıcısı' hizmeti, Windows Hizmet Yöneticisi'nde (Windows Hizmet Yöneticisi'ni açmak için Aç 'Çalışma' ve türü services.msc) yeniden başlatın. Azure geçişi Toplayıcısı hizmeti üzerinde sağ tıklatın ve Başlat'a tıklayın.

   e. Masaüstü kısayolu 'Toplayıcıyı Çalıştır' a çift tıklayın Toplayıcı uygulamasını başlatmak için. Toplayıcı uygulamasını otomatik olarak indirmek ve Powerclı'nın gerekli sürümü yüklemeniz gerekir.

3. Yukarıdaki sorunu çözmezse, adımları izleyin. bir c yukarıdaki ve Powerclı Gereci aşağıdaki adımları kullanarak el ile yükleyin:

   a. Temiz tüm eksik Powerclı adımları izleyerek yükleme dosyalarını #a yukarıdaki #2. adımda #c için.

   b. Başlat'a gidin > Çalıştır > açık Windows PowerShell(x86) yönetici modunda

   c. Komutu çalıştırın:  Install-Module "VMWare.VimAutomation.Core" - RequiredVersion "6.5.2.6234650" (tür 'onay sorduğunda A')

   d. 'Azure geçişi Toplayıcısı' hizmeti, Windows Hizmet Yöneticisi'nde (Windows Hizmet Yöneticisi'ni açmak için Aç 'Çalışma' ve türü services.msc) yeniden başlatın. Azure geçişi Toplayıcısı hizmeti üzerinde sağ tıklatın ve Başlat'a tıklayın.

   e. Masaüstü kısayolu 'Toplayıcıyı Çalıştır' a çift tıklayın Toplayıcı uygulamasını başlatmak için. Toplayıcı uygulamasını otomatik olarak indirmek ve Powerclı'nın gerekli sürümü yüklemeniz gerekir.

4. Güvenlik Duvarı sorunları nedeniyle gereç modülünde indirme bulamıyorsanız, indirin ve aşağıdaki adımları kullanarak internet erişimi olan bir makinede modülünü yükleyin:

    a. Temiz tüm eksik Powerclı adımları izleyerek yükleme dosyalarını #a yukarıdaki #2. adımda #c için.

    b. Başlat'a gidin > Çalıştır > açık Windows PowerShell(x86) yönetici modunda

    c. Komutu çalıştırın:  Install-Module "VMWare.VimAutomation.Core" - RequiredVersion "6.5.2.6234650" (tür 'onay sorduğunda A')

    d. "C:\Program Files (x86) \WindowsPowerShell\Modules" "VMware" ile başlayan tüm modüller Toplayıcı VM üzerinde aynı konuma kopyalayın.

    e. 'Azure geçişi Toplayıcısı' hizmeti, Windows Hizmet Yöneticisi'nde (Windows Hizmet Yöneticisi'ni açmak için Aç 'Çalışma' ve türü services.msc) yeniden başlatın. Azure geçişi Toplayıcısı hizmeti üzerinde sağ tıklatın ve Başlat'a tıklayın.

    f. Masaüstü kısayolu 'Toplayıcıyı Çalıştır' a çift tıklayın Toplayıcı uygulamasını başlatmak için. Toplayıcı uygulamasını otomatik olarak indirmek ve Powerclı'nın gerekli sürümü yüklemeniz gerekir.

### <a name="error-unabletoconnecttoserver"></a>Error UnableToConnectToServer

Şu hata nedeniyle vCenter Server'a bağlanamıyor "Servername.com:9443": İletiyi kabul edebilecek hiçbir uç nokta https://Servername.com:9443/sdk konumunu dinlemiyordu.

Toplayıcı gerecini'nın en son sürümünü, aksi halde, yükseltme gerecine denetleyin [en son sürümü](https://docs.microsoft.com/azure/migrate/concepts-collector).

Sorunu hala en son sürümde olursa, Toplayıcı makinesi belirtilen bağlantı noktası yanlış ya da belirtilen vCenter Server adını çözümleyemiyor nedeni olabilir. Bağlantı noktası belirtilmezse, varsayılan olarak, Toplayıcı bağlantı noktası numarası 443 bağlanmak çalışacaktır.

1. Toplayıcı makinesinden sunucuadı.com ping atmayı deneyin.
2. 1 adım başarısız olursa, IP adresi üzerinden vCenter Server’a bağlanmayı deneyin.
3. vCenter’a bağlanmak için doğru bağlantı noktasını belirleyin.
4. Son olarak vCenter Server’ın çalışır durumda olup olmadığını denetleyin.

### <a name="antivirus-exclusions"></a>Virüsten koruma dışlamaları

Azure geçişi Gereci sağlamlaştırmak için gereç bulunan aşağıdaki klasörler virüs koruma yazılımı taramasından hariç gerekir:

- Azure geçişi hizmeti için ikili dosyaları içeren klasör. Tüm alt klasörleri hariç tutun.
  %ProgramFiles%\ProfilerService  
- Azure geçişi Web uygulaması. Tüm alt klasörleri hariç tutun.
  %SystemDrive%\inetpub\wwwroot
- Veritabanı ve günlük dosyaları için yerel önbelleği. Azure geçişi hizmeti bu klasöre RW erişimi gerekir.
  %SystemDrive%\Profiler

## <a name="dependency-visualization-issues"></a>Bağımlılık görselleştirme sorunları

### <a name="i-am-unable-to-find-the-dependency-visualization-functionality-for-azure-government-projects"></a>Azure kamu projeleri için bağımlılık görselleştirme işlevini bulmaya oluşturamıyorum.

Azure geçişi, hizmet eşlemesinde için bağımlılık görselleştirme işlevini bağlıdır ve hizmet eşlemesi şu anda Azure Kamu'da kullanılabilir olduğundan, bu işlevselliği Azure Kamu'da kullanılabilir değil.

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
Azure geçişi en fazla bir saatlik süre için bağımlılıklar görmenize olanak tanır. Azure geçişi belirli bir tarihe kadar son bir ay için geçmişte dönün olanak tanısa da için bağımlılıkları görselleştirebilirsiniz en fazla süre 1 saate kadar ' dir. Örneğin, Dün için bağımlılıkları görüntülemek için bağımlılık Haritası saati süresi işlevleri kullanabilirsiniz ancak yalnızca bir için bir saat penceresinde görüntüleyebilirsiniz. Ancak, Azure İzleyici günlüklerine kullanabilirsiniz [bağımlılık verileri sorgulamak](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) üzerinden uzun bir süre.

### <a name="i-am-unable-to-visualize-dependencies-for-groups-with-more-than-10-vms"></a>10'dan fazla Vm'leri gruplar için bağımlılıkları görselleştirme oluşturamıyorum?
Yapabilecekleriniz [grupları için bağımlılıkları görselleştirme](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) sahip yukarı 10 VM için 10'dan fazla vm'lerle grubunuz varsa öneririz, grupta küçük kullanıcı gruplarına bölün ve bağımlılıklarını görselleştirin.

### <a name="i-installed-agents-and-used-the-dependency-visualization-to-create-groups-now-post-failover-the-machines-show-install-agent-action-instead-of-view-dependencies"></a>Yüklenen aracıları ve bağımlılık görselleştirmesi grupları oluşturmak için kullanılan bildirimi. Şimdi yük devretme, "aracı yükleme" eylem "Bağımlılıkları görüntüle" yerine makineleri Göster gönderin.
* POST planlanmış veya planlanmamış yük devretme, şirket içi makineleri kapatılır ve eşdeğer makineler çalışmaya Azure'da başlar. Bu makineler, farklı bir MAC adresi alın. Bunlar olup kullanıcı veya şirket içi IP adresi korumak seçiminize bağlı olarak farklı bir IP adresi al. MAC ve IP adresleri farklıysa, Azure geçişi ile herhangi bir hizmet eşlemesi bağımlılık verileri şirket içi makinelerin ilişkilendirmez ve bağımlılıklarını görüntüleme yerine aracıları yüklemek için kullanıcıya sorar.
* Test yük devretme sonrası beklendiği gibi şirket içi makineleri açık kalır. Azure'da küme çalışmaya başladıktan eşdeğer makineler farklı MAC adresini ve farklı IP adresini almanızdan. Azure geçişi kullanıcı blokları giden Azure İzleyici bu makinelerden trafik günlüklerini sürece şirket içi makineler hiçbir hizmet eşlemesi bağımlılık verilerle ilişkilendirmez ve bağımlılıklarını görüntüleme yerine aracıları yüklemek için kullanıcıya sorar.

## <a name="troubleshoot-azure-readiness-issues"></a>Azure için hazır olma sorunlarını giderme

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
   - Microsoft Edge/IE, kaydı otomatik olarak başlayacaktır. Bu gereksinimleri karşılamıyorsa yeşil YÜRÜT düğmesine tıklayın.
4. Hatayı yeniden oluşturmaya çalışın.
5. Kaydetme hatası karşılaştığınız sonra kaydı durdurmak ve kayıtlı etkinliği bir kopyasını kaydedin:
   - Chrome'da, sağ tıklatıp **içerikle HAR olarak Kaydet**. Bu, zıps ve günlükleri .har dosyası olarak dışarı aktarır.
   - Microsoft Edge/IE, tıklayın **dışarı aktarma yakalanan trafiği** simgesi. Bu, zıps ve günlük dışarı aktarır.
6. Gidin **konsol** için uyarıları veya hataları denetlemek için sekmesinde. Konsol günlüğüne kaydetmek için:
   - Chrome'da, konsol günlüğüne herhangi bir yere sağ tıklayın. Seçin **Kaydet**, dışarı aktarma ve günlük zip.
   - Microsoft Edge/IE, sağ tıklatın ve hataları **Tümünü Kopyala**.
7. Geliştirici Araçları'nı kapatın.

## <a name="collector-error-codes-and-recommended-actions"></a>Toplayıcı hata kodları ve Önerilen Eylemler

| Hata Kodu | Hata adı   | İleti   | Olası nedenler | Önerilen eylem  |
| --- | --- | --- | --- | --- |
| 601       | CollectorExpired               | Toplayıcının süresi doldu.                                                        | Toplayıcının Süresi Doldu.                                                                                    | Lütfen toplayıcının yeni bir sürümünü indirip yeniden deneyin.                                                                                      |
| 751       | UnableToConnectToServer        | '%Name;' adlı vCenter Server'a şu hata nedeniyle bağlanılamıyor: %ErrorMessage;     | Daha fazla ayrıntı için hata iletisini inceleyin.                                                             | Hatayı giderip yeniden deneyin.                                                                                                           |
| 752       | InvalidvCenterEndpoint         | '%Name;' adlı sunucu bir vCenter Server değil.                                  | vCenter Server ayrıntılarını sağlayın.                                                                       | Doğru vCenter Server ayrıntılarıyla işlemi yeniden deneyin.                                                                                   |
| 753       | InvalidLoginCredentials        | '%Name;' adlı vCenter Server'a şu hata nedeniyle bağlanılamıyor: %ErrorMessage; | Geçersiz oturum açma kimlik bilgileri nedeniyle vCenter Server bağlantısı başarısız oldu.                             | Sağlanan oturum açma kimlik bilgilerinin doğru olduğundan emin olun.                                                                                    |
| 754       | NoPerfDataAvailable           | Performans verileri kullanılamıyor.                                               | VCenter Server'da istatistik düzeyini denetleyin. Performans verilerinin kullanılabilmesi için 3 ayarlanması gerekir. | İstatistik Düzeyini 3 (5 dakika, 30 dakika ve 2 saatlik süre için) olarak değiştirin ve en az bir gün bekledikten sonra yeniden deneyin.                   |
| 756       | NullInstanceUUID               | InstanceUUID değeri null olan bir makine ile karşılaşıldı                                  | vCenter Server uygun olmayan bir nesneye sahip olabilir.                                                      | Hatayı giderip yeniden deneyin.                                                                                                           |
| 757       | VMNotFound                     | Sanal makine bulunamadı                                                  | Sanal makine silinmiş olabilir: %VMID;                                                                | vCenter envanterinin kapsamı belirlenirken seçilen sanal makinelerin keşif sırasında mevcut olduğundan emin olun                                      |
| 758       | GetPerfDataTimeout             | VCenter isteği zaman aşımına uğradı. İleti % Message;                                  | vCenter Server kimlik bilgileri yanlış                                                              | VCenter sunucusu kimlik bilgilerini denetleyin ve vCenter Server'ın erişilebilir olduğundan emin olun. İşlemi yeniden deneyin. Sorun devam ederse, destek ekibine başvurun. |
| 759       | VmwareDllNotFound              | VMWare.Vim DLL bulunamadı.                                                     | PowerCLI düzgün bir şekilde yüklenmedi.                                                                   | Lütfen Powerclı düzgün yüklü olup olmadığını denetleyin. İşlemi yeniden deneyin. Sorun devam ederse, destek ekibine başvurun.                               |
| 800       | ServiceError                   | Azure Geçişi Toplayıcısı hizmeti çalışmıyor.                               | Azure Geçişi Toplayıcısı hizmeti çalışmıyor.                                                       | Hizmeti services.msc kullanarak başlatın ve işlemi yeniden deneyin.                                                                             |
| 801       | PowerCLIError                  | VMware PowerCLI yüklenemedi.                                          | VMware PowerCLI yüklenemedi.                                                                  | İşlemi yeniden deneyin. Sorun devam ederse kendiniz yükleyin ve işlemi yeniden deneyin.                                                   |
| 802       | TimeSyncError                  | Saat, İnternet saat sunucusuyla eşitlenmemiş.                            | Saat, İnternet saat sunucusuyla eşitlenmemiş.                                                    | Makinedeki saatin saat dilimine göre doğru ayarlandığından emin olun ve işlemi yeniden deneyin.                                 |
| 702       | OMSInvalidProjectKey           | Geçersiz proje anahtarı belirtildi.                                                | Geçersiz proje anahtarı belirtildi.                                                                        | İşlemi doğru proje anahtarıyla yeniden deneyin.                                                                                              |
| 703       | OMSHttpRequestException        | İstek gönderilirken hata oluştu. İleti % Message;                                | Proje kimliği ile anahtarını denetleyerek uç noktanın erişilebilir olduğundan emin olun.                                       | İşlemi yeniden deneyin. Sorun devam ederse, Microsoft Destek olanağına başvurun.                                                                     |
| 704       | OMSHttpRequestTimeoutException | HTTP isteği zaman aşımına uğradı. İleti % Message;                                     | Proje kimliği ile anahtarını denetleyerek uç noktanın erişilebilir olduğundan emin olun.                                       | İşlemi yeniden deneyin. Sorun devam ederse, Microsoft Destek olanağına başvurun.                                                                     |
