---
title: Azure Otomasyonu ile değişiklikleri izleme
description: Değişiklik izleme çözümü, yazılım ve ortamınızda gerçekleşen Windows hizmeti değişiklikleri belirlemenize yardımcı olur.
services: automation
ms.service: automation
ms.subservice: change-inventory-management
author: georgewallace
ms.author: gwallace
ms.date: 04/29/2019
ms.topic: conceptual
manager: carmonm
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4f917c45030ad70a2ab76fed877bd822d1902f82
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64927290"
---
# <a name="track-changes-in-your-environment-with-the-change-tracking-solution"></a>Değişiklik izleme çözümüyle ortamınızdaki Değişiklikleri İzle

Bu makalede değişiklik izleme çözümü ortamınızdaki değişikliklerini kolayca belirlemenize yardımcı olur. Çözüm, Windows ve Linux yazılım, Windows ve Linux dosyaları, Windows kayıt defteri anahtarlarını, Windows Hizmetleri ve Linux Daemon'ları için değişiklikleri izler. Yapılandırma değişikliklerini belirlemek işletimsel sorunları belirlemenize yardımcı olabilir.

Yüklü yazılım, Windows Hizmetleri, Windows kayıt defteri ve dosya ve izlenen sunucularda Linux Daemon'ları için değişiklikler, işleme için bulutta Azure İzleyici'hizmetine gönderilir. Mantıksal alınan verilere uygulanır ve bulut hizmeti olan verileri kaydeder. Değişiklik izleme Panoda bilgileri kullanarak, sunucu altyapınızda yapılan değişiklikleri kolayca görebilirsiniz.

> [!NOTE]
> Azure Otomasyonu değişiklik izleme, sanal makineler'de değişiklikleri izler. Azure Resource Manager özellik değişiklikleri izlemek için Azure Kaynak grafiğin bkz [değişiklik geçmişini](../governance/resource-graph/how-to/get-resource-changes.md).

## <a name="supported-windows-operating-systems"></a>Desteklenen Windows işletim sistemleri

Aşağıdaki Windows işletim sistemi sürümleri Windows aracısı için resmi olarak desteklenir:

* Windows Server 2008 R2 veya üzeri

## <a name="supported-linux-operating-systems"></a>Desteklenen Linux işletim sistemleri

Resmi olarak desteklenen aşağıdaki Linux dağıtımları. Ancak, Linux Aracısı listelenmeyen diğer dağıtımlarında da çalışabilir. Aksi belirtilmediği sürece, listelenen her ana sürümünün tüm ikincil sürümleri desteklenir.  

### <a name="64-bit"></a>64 bit

* CentOS 6 ve 7
* Amazon Linux 2017.09
* Oracle Linux 6 ve 7
* Red Hat Enterprise Linux Server 6 ve 7
* Debian GNU/Linux 8 ve 9
* Ubuntu Linux 14.04 LTS, 16.04 LTS ve 18.04 LTS
* SUSE Linux Enterprise Server 12

### <a name="32-bit"></a>32 bit

* CentOS 6
* Oracle Linux 6
* Red Hat Enterprise Linux Server 6
* Debian GNU/Linux 8 ve 9
* Ubuntu Linux 14.04 LTS ve 16.04 LTS

## <a name="onboard"></a>Değişiklik izleme ve stok özelliklerini etkinleştirme

Değişiklikleri izlemeyi başlatmak için değişiklik izleme ve stok çözümü etkinleştirmek gerekir. Değişiklik izleme ve stok makine birçok yolu vardır. Aşağıdaki önerilen olan ve çözüm yollarını desteklenir.

* [Bir sanal makineden](automation-onboard-solutions-from-vm.md)
* [Birden çok makine gözatmanızı](automation-onboard-solutions-from-browse.md)
* [Otomasyon hesabınızdan](automation-onboard-solutions-from-automation-account.md)
* [Azure Otomasyonu runbook'u ile](automation-onboard-solutions.md)

## <a name="configuring-change-tracking-and-inventory"></a>Değişiklik izleme ve stok yapılandırma

Bilgi edinmek için nasıl çözüme bilgisayarları ekleme ziyaret edin: [Onboarding Otomasyon çözümleri](automation-onboard-solutions-from-automation-account.md). Değişiklik izleme ve stok çözümü ile bir makine ekleme oluşturduktan sonra öğelerini izlemek için yapılandırabilirsiniz. Yeni bir dosya veya kayıt defteri anahtarı izlemek için etkinleştirdiğinizde, değişiklik izleme ve stok için etkinleştirilir.

Hem Windows hem de Linux dosyalarda değişiklik izleme için MD5 karmaları dosyaları kullanılır. Bu karmalar, daha sonra son Envanter beri bir değişiklik yapılıp yapılmadığını belirlemek için kullanılır.

### <a name="configure-linux-files-to-track"></a>Linux dosyaları izlemek için yapılandırma

Linux Bilgisayarları'nda dosyaları izlemeyi yapılandırmak için aşağıdaki adımları kullanın:

1. Otomasyon hesabınızı seçin **değişiklik izleme** altında **yapılandırma yönetimi**. Tıklayın **ayarlarını Düzenle** (dişli simgesi).
2. Üzerinde **değişiklik izleme** sayfasında **Linux dosyaları**, ardından **+ Ekle** izlemek için yeni bir dosya eklemek için.
3. Üzerinde **değişiklik izleme için Linux dosyası ekleme**, dosya veya dizin ve izlemek için bilgi girin **Kaydet**.

|Özellik  |Açıklama  |
|---------|---------|
|Enabled     | Ayarın uygulanmış olup olmadığını belirler.        |
|Öğe Adı     | İzlenecek dosyanın kolay adı.        |
|Grup     | Dosyaları mantıksal olarak gruplamak için bir grup adı.        |
|Yolu Gir     | Dosyanın denetleneceği yol. Örneğin: "/etc/*.conf"       |
|Yol Türü     | İzlenen olası değerler için öğe türü: dosya ve dizin.        |
|Özyineleme     | İzlenecek öğe aranırken özyinelemenin kullanılıp kullanılmadığını belirler.        |
|Sudo Kullan     | Bu ayar, öğe denetlenirken sudonun kullanılıp kullanılmadığını belirler.         |
|Bağlantılar     | Bu ayar, dizinleri dolaşırken sembolik bağlantıların nasıl ele alındığını belirler.<br> **Yoksay** - sembolik bağlantıları yoksayar ve başvurulan dosyaları veya dizinleri içermez.<br>**İzleyin** - özyineleme sırasında sembolik bağlantıları izler ve başvurulan dosyaları veya dizinleri de içerir.<br>**Yönetme** - sembolik bağlantıları izler ve döndürülen içeriğin değiştirilmesine izin verir.     |
|Dosya içeriğini tüm ayarlar için karşıya yükleme| İzlenen değişikliklerin dosya içeriği karşıya yükleme işlemini açar veya kapatır. Mevcut seçenekler: **Doğru** veya **False**.|

> [!NOTE]
> “Yönet” bağlantıları seçeneği önerilmez. Dosya içeriğini alma desteklenmiyor.

### <a name="configure-windows-files-to-track"></a>Windows dosyaları izlemek için yapılandırma

Windows bilgisayarlarda izlemeye dosyaları yapılandırmak için aşağıdaki adımları kullanın:

1. Otomasyon hesabınızı seçin **değişiklik izleme** altında **yapılandırma yönetimi**. Tıklayın **ayarlarını Düzenle** (dişli simgesi).
2. Üzerinde **değişiklik izleme** sayfasında **Windows dosyaları**, ardından **+ Ekle** izlemek için yeni bir dosya eklemek için.
3. Üzerinde **değişiklik izleme için Windows dosyası ekleme**, izlemek ve dosya bilgilerini girin **Kaydet**.

|Özellik  |Açıklama  |
|---------|---------|
|Enabled     | Ayarın uygulanmış olup olmadığını belirler.        |
|Öğe Adı     | İzlenecek dosyanın kolay adı.        |
|Grup     | Dosyaları mantıksal olarak gruplamak için bir grup adı.        |
|Yolu Gir     | Dosyayı denetlemek için kullanılacak yol (örneğin, "c:\temp\\\*.txt")<br>"%winDir%\System32\\\*.*" gibi ortam değişkenleri de kullanabilirsiniz       |
|Özyineleme     | İzlenecek öğe aranırken özyinelemenin kullanılıp kullanılmadığını belirler.        |
|Dosya içeriğini tüm ayarlar için karşıya yükleme| İzlenen değişikliklerin dosya içeriği karşıya yükleme işlemini açar veya kapatır. Mevcut seçenekler: **Doğru** veya **False**.|

## <a name="wildcard-recursion-and-environment-settings"></a>Joker karakter, özyineleme ve ortam ayarları

Özyineleme, dizinleri ve dosyaları ortamlarında birden çok izleme olanak tanımak için ortam değişkenlerini izleme veya dinamik basitleştirmek için joker karakterler belirtmenize olanak sağlar sürücü adları. Aşağıdaki liste, özyineleme yapılandırırken bilmeniz gereken genel bilgileri gösterir:

* Joker karakter, birden çok dosyayı izlemek için gereklidir
* Joker karakterler kullanılıyorsa, bunlar yalnızca yolun son segmentinde kullanılabilir. (gibi `c:\folder\*file*` veya `/etc/*.conf`)
* Bir ortam değişkeni geçersiz bir yol varsa, doğrulama başarılı olur ancak sayım yürüttüğünde yol başarısız olur.
* Genel yolları gibi önlemek `c:\*.*` yolu ayarlarken bu geçilen çok fazla klasörlerinde neden.

## <a name="configure-file-content-tracking"></a>Dosya içeriği izlemeyi yapılandırma

Önce ve sonra bir değişiklik dosya içeriği değişiklik izleme ile bir dosyanın içeriğini görüntüleyebilirsiniz. Her dosya, dosyanın içeriğini değişiklik bir depolama hesabında depolanır ve önceki ve sonraki değişiklik, satır içi veya yan yana dosyasını gösterir bu dosyalar, Windows ve Linux için kullanılabilir. Daha fazla bilgi için bkz. [izlenen bir dosyanın içeriğini görüntülemek](change-tracking-file-contents.md).

![bir dosyadaki değişiklikleri görüntüleme](./media/change-tracking-file-contents/view-file-changes.png)

### <a name="configure-windows-registry-keys-to-track"></a>Windows kayıt defteri anahtarlarını izlemek için

Windows bilgisayarlarda kayıt defteri anahtarı izlemeyi yapılandırmak için aşağıdaki adımları kullanın:

1. Otomasyon hesabınızı seçin **değişiklik izleme** altında **yapılandırma yönetimi**. Tıklayın **ayarlarını Düzenle** (dişli simgesi).
2. Üzerinde **değişiklik izleme** sayfasında **Windows kayıt defteri**, ardından **+ Ekle** izlemek için yeni bir kayıt defteri anahtarı eklemek için.
3. Üzerinde **için değişiklik izleme Windows kayıt defteri Ekle**, izlemek ve anahtar bilgilerini girin **Kaydet**.

|Özellik  |Açıklama  |
|---------|---------|
|Enabled     | Ayarın uygulanmış olup olmadığını belirler.        |
|Öğe Adı     | İzlenecek kayıt defteri anahtarı kolay adı.        |
|Grup     | Kayıt defteri anahtarlarını mantıksal olarak gruplamak için bir grup adı.        |
|Windows Kayıt Defteri Anahtarı   | Kayıt defteri anahtarı denetleneceği yol. Örneğin: "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup"      |

## <a name="limitations"></a>Sınırlamalar

Değişiklik izleme çözümü şu anda aşağıdakileri desteklemez:

* İzleme Windows kayıt defteri için özyineleme
* Ağ dosya sistemleri

Diğer sınırlamalar:

* **En büyük dosya boyutu** sütun ve değerlerini geçerli uygulamada kullanılmayan.
* 30 dakikalık toplama döngüsü içinde 2500'den fazla dosya toplarsanız çözüm performans düzeyi düşürülmüş.
* Ağ trafiği yüksek olduğunda, değişiklik kayıtları görüntülemek için altı saat sürebilir.
* Bilgisayar, bir bilgisayar kapatıldı yapılandırmasını değiştirirseniz, önceki yapılandırmaya ait değiştiğinde post.

## <a name="known-issues"></a>Bilinen Sorunlar

Değişiklik izleme çözümü şu anda aşağıdaki sorunlarla karşılaşıyor:

* Düzeltme güncelleştirmelerini, Windows Server 2016 Core RS3 makinelerde toplanmadı.
* Hiçbir değişiklik yoktu rağmen Linux Daemon'ları değiştirilen durum gösterebilir. Bu nedeniyle nasıl, `SvcRunLevels` alan yakalanır.

## <a name="change-tracking-data-collection-details"></a>Değişiklik izleme veri koleksiyonu ayrıntıları

Aşağıdaki tabloda değişiklik türleri için veri toplama sıklığı gösterilmektedir. Her türü için geçerli durumu verilerin anlık görüntüsünü de en az 24 saatte bir yenilenir:

| **Türü Değiştir** | **Sıklık** |
| --- | --- |
| Windows kayıt defteri | 50 dakika |
| Windows dosya | 30 dakika |
| Linux dosyası | 15 dakika |
| Windows hizmetleri | 10 saniye olarak 30 dakika</br> Varsayılan: 30 dakika |
| Linux Daemon'ları | 5 dakika |
| Windows yazılım | 30 dakika |
| Linux yazılım | 5 dakika |

Aşağıdaki tablo, değişiklik izleme için makine başına izlenen öğe sınırları gösterir.

| **Kaynak** | **Sınırı**| **Notlar** |
|---|---|---|
|Dosya|500||
|Kayıt defteri|250||
|Windows yazılım|250|Yazılım güncelleştirmeleri dahil değildir|
|Linux paketleri|1250||
|Hizmetler|250||
|Daemon|250||

Log Analytics veri kullanımı değişiklik izleme ve stok kullanarak bir makine için yaklaşık 40 MB / ay ortalamadır. Bu değer yalnızca yaklaşık ve ortamınıza bağlı değiştirilebilir. Ortamınızı sahip olduğunuz tam kullanımını görmek için izlemeniz önerilir.

### <a name="windows-service-tracking"></a>Windows hizmeti izleme

Windows Hizmetleri için varsayılan toplama sıklığı 30 dakikadır. Sıklığını yapılandırmak için şu adrese gidin **değişiklik izleme**. Altında **ayarlarını Düzenle** üzerinde **Windows Hizmetleri** sekmesinde, Windows Hizmetleri'nden için toplama sıklığı için en çok 30 dakika olarak 10 saniye olan en kısa sürede değiştirmenize izin veren bir kaydırıcı yoktur. Kaydırıcı çubuğunu taşımak istediğiniz sıklığı ve onu otomatik olarak kaydeder.

![Windows Hizmetleri kaydırıcı](./media/change-tracking/windowservices.png)

Aracı yalnızca değişiklikleri izler, bu aracı performansını iyileştirir. Hizmet ilk durumuna geri döndürüldü, yüksek bir eşik ayarı değişiklikleri kaçırabilir. Sıklığı ayarını daha küçük bir değere, aksi takdirde atlanabilir değişiklikleri yakalamak sağlar.

> [!NOTE]
> Aracı için sırada değişiklikleri izleme için 10 ikinci bir aralık, veriler yine de portalında görüntülenmesi birkaç dakika sürer. Portalda görüntülemek için süre sırasında değişiklikleri hala izlenen ve günlüğe kaydedilir.
  
### <a name="registry-key-change-tracking"></a>Kayıt defteri anahtarı değişiklik izleme

Üçüncü taraf kodu ve kötü amaçlı yazılım burada etkinleştirebilirsiniz genişletilebilirlik noktaları saptamak için kayıt defteri anahtarları için değişiklik izleme amacı sağlamaktır. Aşağıdaki listede, önceden yapılandırılmış bir kayıt defteri anahtarları listesini gösterir. Bu anahtarları yapılandırılmış ancak etkin değil. Bu kayıt defteri anahtarlarını izlemek için her biri etkinleştirmeniz gerekir.

> [!div class="mx-tdBreakAll"]
> |  |
> |---------|
> |**HKEY\_yerel\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Windows Gezgini ve genellikle çalıştırma işlem içi Explorer.exe ile doğrudan bağlama izleyiciler ortak autostart girdileri.    |
> |**HKEY\_yerel\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup**     |
|&nbsp;&nbsp;&nbsp;&nbsp;İzleyiciler başlangıçta çalışan komutlar.     |
> |**HKEY\_yerel\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Shutdown**    |
|&nbsp;&nbsp;&nbsp;&nbsp;İzleyiciler kapatma sırasında çalışan komutlar.     |
> |**HKEY\_yerel\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Kullanıcı oturum açtığında kendi Windows hesabı için önce yüklenen anahtarları izler. Anahtarı, 64-bit bilgisayarlarda çalışan 32-bit programları için kullanılır.    |
> |**HKEY\_yerel\_MACHINE\SOFTWARE\Microsoft\Active Setup\Installed bileşenleri**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Uygulama ayarlarına değişiklikleri izler.     |
> |**HKEY\_yerel\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Windows Gezgini ve genellikle çalıştırma işlem içi Explorer.exe ile doğrudan bağlama izleyiciler ortak autostart girdileri.|
> |**HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Windows Gezgini ve genellikle çalıştırma işlem içi Explorer.exe ile doğrudan bağlama izleyiciler ortak autostart girdileri.|
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers**|
|&nbsp;&nbsp;&nbsp;&nbsp;İzleyici simge için işleyici kaydı yer.|
|**HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers**|
|&nbsp;&nbsp;&nbsp;&nbsp;İzleyici simge için 64-bit bilgisayarlarda çalışan 32-bit programları için işleyici kaydı yer.|
> |**HKEY\_yerel\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\Browser Yardımcısı nesneleri**|
|&nbsp;&nbsp;&nbsp;&nbsp;Internet Explorer için yeni tarayıcı Yardımcısı nesnesi eklentileri için izleyiciler. Geçerli sayfanın belge nesne modeli (DOM) erişmeye ve gezdirmeyi denetlemek için kullanılır.|
> |**HKEY\_yerel\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser Yardımcısı nesneleri**|
|&nbsp;&nbsp;&nbsp;&nbsp;Internet Explorer için yeni tarayıcı Yardımcısı nesnesi eklentileri için izleyiciler. Geçerli sayfanın belge nesne modeli (DOM) erişmeye ve 64-bit bilgisayarlarda çalışan 32-bit programları için gezdirmeyi denetlemek için kullanılır.|
> |**HKEY\_yerel\_MACHINE\Software\Microsoft\Internet Explorer\Extensions**|
|&nbsp;&nbsp;&nbsp;&nbsp;Özel araç menüler ve özel araç çubuğu düğmeleri gibi yeni Internet Explorer uzantıları için İzleyici.|
> |**HKEY\_yerel\_MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions**|
|&nbsp;&nbsp;&nbsp;&nbsp;Özel araç menüler ve 64-bit bilgisayarlarda çalışan 32-bit programları için özel araç çubuğu düğmeleri gibi yeni Internet Explorer uzantıları için İzleyici.|
> |**HKEY\_yerel\_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32**|
|&nbsp;&nbsp;&nbsp;&nbsp;Wavemapper, wave1 ve wave2, msacm.imaadpcm, .msadpcm, .msgsm610 ve vidc ile ilişkili 32-bit sürücüleri izler. Benzer şekilde sistem [drivers] bölümünde. INI dosyası.|
> |**HKEY\_yerel\_MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32**|
|&nbsp;&nbsp;&nbsp;&nbsp;İzleyiciler 32-bit sürücüleriniz wavemapper, wave1 ve wave2, msacm.imaadpcm, .msadpcm, .msgsm610 ve 64-bit bilgisayarlarda çalışan 32-bit programları için vidc ile ilişkili. Benzer şekilde sistem [drivers] bölümünde. INI dosyası.|
> |**HKEY\_yerel\_MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls**|
|&nbsp;&nbsp;&nbsp;&nbsp;Yaygın olarak kullanılan veya bilinen sistem DLL'lerini listesini izler; Bu sistem, kişiler, sistem DLL'lerini Truva atı sürümlerinde bırakarak zayıf uygulama dizin izinlerini faydalanmasını engeller.|
> |**HKEY\_yerel\_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify**|
|&nbsp;&nbsp;&nbsp;&nbsp;Winlogon, Windows işletim sistemi için etkileşimli oturum açma destek modeli olay bildirimleri almak için paketler listesini izler.|

## <a name="network-requirements"></a>Ağ gereksinimleri

Aşağıdaki adresleri özellikle değişiklik izleme için gerekli değildir. Bu adresler için iletişim bağlantı noktası 443 üzerinden gerçekleştirilir.

|Azure kamu  |Azure Kamu  |
|---------|---------|
|*.ods.opinsights.azure.com     |*.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net|*.blob.core.usgovcloudapi.net|
|*.azure-automation.net|*.azure-automation.us|

## <a name="use-change-tracking"></a>Değişiklik izlemeyi kullanma

Çözüm etkinleştirildikten sonra değişikliklerin özeti için izlenen bilgisayarlar seçerek görüntüleyebilirsiniz **değişiklik izleme** altında **yapılandırma yönetimi** Otomasyon hesabınızdaki.

Değişiklikleri bilgisayarlarınız ve ardından-ayrıntıya her olayla ilgili ayrıntıları görüntüleyebilirsiniz. Değişiklik türü ve zaman aralıklarına göre ayrıntılı bilgiler ve grafik sınırlamak için grafiğin üstünde açılan listeler kullanılabilir. ' A tıklayın ve özel bir zaman aralığı seçmek için grafiğe sürükleyin. **Değişiklik türü** aşağıdaki değerlerden biri olacak **olayları**, **Daemon'ları**, **dosyaları**, **kayıt defteri**,  **Yazılım**, **Windows Hizmetleri**. Kategori değişiklik türünü gösterir ve olabilir **eklenen**, **değiştirilen**, veya **kaldırıldı**.

![Değişiklik izleme panosunun görüntüsü](./media/change-tracking/change-tracking-dash01.png)

Bir değişiklik veya olay'ı tıklatarak bu değişiklik hakkında ayrıntılı bilgi getirir. Örnekte görebileceğiniz gibi hizmet başlatma türünü otomatik olarak el ile değiştirildi.

![değişiklik ayrıntıları izleme görüntüsü](./media/change-tracking/change-tracking-details.png)

## <a name="search-logs"></a>Günlüklerinde arama yapma

Portalda sağlanan Ayrıntılar ek olarak, arama günlüklerine karşı yapılabilir. İle **değişiklik izleme** sayfası açıldığında, tıklayın **Log Analytics**, bu açılır **günlükleri** sayfası.

### <a name="sample-queries"></a>Örnek sorgular

Aşağıdaki tabloda bu çözüm tarafından toplanan kayıtlarına ilişkin örnek günlük aramaları değiştirme sağlar:

|Sorgu  |Açıklama  |
|---------|---------|
|ConfigurationData<br>&#124;Burada ConfigDataType "WindowsServices" ve SvcStartupType == "Auto" ==<br>&#124;Burada SvcState "Durduruldu" ==<br>&#124;Özetleme arg_max(TimeGenerated, *) SoftwareName, bilgisayar tarafından         | Otomatik olarak ayarlanmış, ancak durduruldu olarak bildirilen bir Windows Hizmetleri için en son Envanter kayıtlarının gösterir<br>Sonuçları en son kayıt için söz konusu ve bilgisayar ölçütlerine sınırlıdır      |
|ConfigurationChange<br>&#124;Burada ConfigChangeType "Yazılım" ve ChangeCategory == "Kaldırıldı" ==<br>&#124;TimeGenerated desc sıralama|Kaldırılacak yazılım için değişiklik kayıtları gösterir|

## <a name="alert-on-changes"></a>Değişikliklerle ilgili uyarı

Bir anahtar özellik değişiklik izleme ve stok yapılandırma durumunu ve karma ortamınızın yapılandırma durumu değişiklikleri uyarı yeteneğidir.  

Aşağıdaki örnekte, ekran görüntüsünde, dosya gösterilmektedir `C:\windows\system32\drivers\etc\hosts` bir makinede değişiklik yapılmadı. Hosts dosyasını ana bilgisayar adları için IP adresleri ve bağlantı sorunları veya yeniden yönlendirme trafiği kötü amaçlı veya tehlikeli Aksi halde Web sitelerine neden bile DNS önceliklidir gidermek için Windows tarafından kullanıldığı için bu önemli bir dosyadır.

![Ana dosya değişikliği gösteren bir grafiği](./media/change-tracking/changes.png)

Bu değişikliği daha fazla analiz yapmak için günlük araması tıklama ile Git **Log Analytics**. Günlük araması'nda, Hosts dosyasına bir sorgu ile içerik değişikliklerini kez arama `ConfigurationChange | where FieldsChanged contains "FileContentChecksum" and FileSystemPath contains "hosts"`. Bu sorgu, bir değişiklik, tam nitelenmiş bir yol "ana bilgisayarlar" sözcüğünü içeren dosyalar için dosya içeriğini dahil edilen değişiklikler arar. Tam hâli yol bölümü değiştirerek belirli bir dosya için isteyebilir (örneğin `FileSystemPath == "c:\windows\system32\drivers\etc\hosts"`).

Sorgu istediğiniz sonuçları döndürdükten sonra tıklayın **yeni uyarı kuralı** uyarı oluşturma sayfasını açmak için günlük arama deneyimini düğmesi. Bu deneyim için konuma yönlendirilemedi **Azure İzleyici** Azure portalında. Uyarı oluşturma deneyiminde sorgumuzu tekrar kontrol edin ve uyarı mantığının değiştirin. Bu durumda, ortamdaki tüm makinelerde algılanan bile bir değişiklik olursa tetiklenmesi için uyarıyı istersiniz.

![Hosts dosyasına izleme değişiklikleri için değişiklik sorgu gösteren görüntü](./media/change-tracking/change-query.png)

Koşul mantığı ayarlandıktan sonra tetiklenen uyarının yanıt eylemleri gerçekleştirmek için Eylem grupları atayın. Bu durumda, ı gönderilecek e-postalar ve oluşturulacak bir ITSM bileti yedekleme ayarladınız.  Bir Azure işlevi, Otomasyon runbook'u, Web kancası veya mantıksal uygulama tetiklemek gibi birçok yararlı eylemler de alınabilir.

![Görüntüyü değişikliğin uyarı için bir eylem grubu yapılandırma](./media/change-tracking/action-groups.png)

Tüm parametreler ve mantıksal ayarladıktan sonra uyarı ortama uygulayabilirsiniz.

### <a name="alert-suggestions"></a>Uyarı önerileri

Hosts dosyasına değişiklikler uyarı uyarı değişiklik izleme veya envanter verileri için iyi bir uygulama olsa da, uyarı verme, aşağıdaki bölümde, örnek sorgularını birlikte tanımlanan durumlar da dahil olmak üzere pek çok daha fazla senaryo vardır.

|Sorgu  |Açıklama  |
|---------|---------|
|ConfigurationChange <br>&#124;Burada ConfigChangeType "Files" ve FileSystemPath == içeren "c:\\windows\\system32\\sürücüleri\\"|Kritik sistem dosyaları için değişiklik izleme için yararlı|
|ConfigurationChange <br>&#124;Burada FieldsChanged içeren "FileContentChecksum" ve FileSystemPath == "c:\\windows\\system32\\sürücüleri\\vb.\\ana bilgisayarlar"|Anahtar yapılandırma dosyaları için değişiklik izleme için yararlı|
|ConfigurationChange <br>&#124;Burada ConfigChangeType == "WindowsServices" ve "w3svc" ve SvcState SvcName içeren "Stopped" ==|Sistem kritik Hizmetleri için değişiklik izleme için yararlı|
|ConfigurationChange <br>&#124;Burada ConfigChangeType == "Daemon'ları" ve "ssh" ve SvcState SvcName içerir! = "Çalışıyor"|Sistem kritik Hizmetleri için değişiklik izleme için yararlı|
|ConfigurationChange <br>&#124;Burada ConfigChangeType "Yazılım" ve ChangeCategory == "Eklenir" ==|Yararlı ortamlar için gereken yazılım yapılandırmaları kilitli|
|ConfigurationData <br>&#124;Burada SoftwareName içeren "Monitoring Agent" ve CurrentVersion! = "8.0.11081.0"|Bir güncel olmayan veya uyumlu olmayan yazılım sürümü yüklü olan makineleri görmek için faydalıdır. Son raporlanan yapılandırma durumu değişiklikleri rapor.|
|ConfigurationChange <br>&#124;Burada RegistryKey == "HKEY_LOCAL_MACHINE\\yazılım\\Microsoft\\Windows\\CurrentVersion\\QualityCompat"| Değişiklikleri izlemek önemli virüsten koruma anahtarları için yararlı|
|ConfigurationChange <br>&#124;Burada RegistryKey içeren "HKEY_LOCAL_MACHINE\\sistem\\CurrentControlSet\\Hizmetleri\\SharedAccess\\parametreleri\\FirewallPolicy"| Güvenlik Duvarı ayarları değişiklikleri izlemek için yararlı|

## <a name="next-steps"></a>Sonraki adımlar

Öğretici çözümünü kullanma hakkında daha fazla bilgi edinmek için değişiklik izleme'ı ziyaret edin:

> [!div class="nextstepaction"]
> [Ortamınızdaki değişikliklerle ilgili sorunları giderme](automation-tutorial-troubleshoot-changes.md)

* Kullanım [günlük aramaları Azure İzleyici günlüklerine](../log-analytics/log-analytics-log-searches.md) ayrıntılı değişiklik izleme verileri görüntülemek için.