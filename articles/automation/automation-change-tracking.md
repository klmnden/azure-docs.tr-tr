---
title: Azure Otomasyonu ile değişiklikleri izleme
description: Değişiklik izleme çözümü, yazılım ve ortamınızda gerçekleşen Windows hizmeti değişiklikleri belirlemenize yardımcı olur.
services: automation
ms.service: automation
ms.component: change-inventory-management
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: conceptual
manager: carmonm
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 79d64a5a7eb339c6904fe026209292202632f640
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37342020"
---
# <a name="track-changes-in-your-environment-with-the-change-tracking-solution"></a>Değişiklik izleme çözümüyle ortamınızdaki Değişiklikleri İzle

Bu makalede değişiklik izleme çözümü ortamınızdaki değişikliklerini kolayca belirlemenize yardımcı olur. Çözüm, Windows ve Linux yazılım, Windows ve Linux dosyaları, Windows kayıt defteri anahtarlarını, Windows Hizmetleri ve Linux Daemon'ları için değişiklikleri izler. Yapılandırma değişikliklerini belirlemek işletimsel sorunları belirlemenize yardımcı olabilir.

Yüklü yazılım, Windows Hizmetleri, Windows kayıt defteri ve dosya ve izlenen sunucularda Linux Daemon'ları için değişiklikler, işleme için buluttaki Log Analytics hizmetine gönderilir. Mantıksal alınan verilere uygulanır ve bulut hizmeti olan verileri kaydeder. Değişiklik izleme Panoda bilgileri kullanarak, sunucu altyapınızda yapılan değişiklikleri kolayca görebilirsiniz.

## <a name="enable-change-tracking-and-inventory"></a>Değişiklik İzlemeyi ve Sayımı Etkinleştirme

Değişiklikleri izlemeye başlamak için Automation hesabınız için değişiklik izleme ve stok çözümü etkinleştirmek gerekir.

1. Azure portalında, Otomasyon hesabınıza gidin
1. Seçin **değişiklik izleme** altında **yapılandırma**.
1. Mevcut bir Log analytics çalışma alanı seçin veya **yeni çalışma alanı oluştur** tıklatıp **etkinleştirme**.

Bu, automation hesabınız için bir çözüm sağlar. Çözümü etkinleştirmek 15 dakika kadar sürebilir. Mavi renkli bir başlık, çözüm etkinleştirildiğinde size bildirir. Geri gidin **değişiklik izleme** çözümü yönetmek için sayfa.

## <a name="configuring-change-tracking-and-inventory"></a>Değişiklik izleme ve stok yapılandırma

Bilgi edinmek için nasıl çözüme bilgisayarları ekleme ziyaret edin: [ekleme Otomasyon çözümleri](automation-onboard-solutions-from-automation-account.md). Değişiklik izleme ve stok çözümü ile bir makine ekleme oluşturduktan sonra öğelerini izlemek için yapılandırabilirsiniz. Yeni bir dosya veya kayıt defteri anahtarı izlemek için etkinleştirdiğinizde, değişiklik izleme ve stok için etkinleştirilir.

Hem Windows hem de Linux dosyalarda değişiklik izleme için MD5 karmaları dosyaları kullanılır. Bu karmalar, daha sonra son Envanter beri bir değişiklik yapılıp yapılmadığını belirlemek için kullanılır.

### <a name="configure-linux-files-to-track"></a>Linux dosyaları izlemek için yapılandırma

Linux Bilgisayarları'nda dosyaları izlemeyi yapılandırmak için aşağıdaki adımları kullanın:

1. Otomasyon hesabınızı seçin **değişiklik izleme** altında **yapılandırma yönetimi**. Tıklayın **ayarlarını Düzenle** (dişli simgesi).
2. Üzerinde **değişiklik izleme** sayfasında **Linux dosyaları**, ardından **+ Ekle** izlemek için yeni bir dosya eklemek için.
3. Üzerinde **değişiklik izleme için Linux dosyası ekleme**, dosya veya dizin ve izlemek için bilgi girin **Kaydet**.

|Özellik  |Açıklama  |
|---------|---------|
|Etkin     | Ayarın uygulanmış olup olmadığını belirler.        |
|Öğe Adı     | İzlenecek dosyanın kolay adı.        |
|Grup     | Dosyaları mantıksal olarak gruplamak için bir grup adı.        |
|Yolu Gir     | Dosyanın denetleneceği yol. Örneğin: "/etc/*.conf"       |
|Yol Türü     | İzlenen olası değerler için öğe türü: dosya ve dizin.        |
|Özyineleme     | İzlenecek öğe aranırken özyinelemenin kullanılıp kullanılmadığını belirler.        |
|Sudo Kullan     | Bu ayar, öğe denetlenirken sudonun kullanılıp kullanılmadığını belirler.         |
|Bağlantılar     | Bu ayar, dizinleri dolaşırken sembolik bağlantıların nasıl ele alındığını belirler.<br> **Yoksay** - sembolik bağlantıları yoksayar ve başvurulan dosyaları veya dizinleri içermez.<br>**İzleyin** - özyineleme sırasında sembolik bağlantıları izler ve başvurulan dosyaları veya dizinleri de içerir.<br>**Yönetme** - sembolik bağlantıları izler ve döndürülen içeriğin değiştirilmesine izin verir.     |

> [!NOTE]
> “Yönet” bağlantıları seçeneği önerilmez. Dosya içeriğini alma desteklenmiyor.

### <a name="configure-windows-files-to-track"></a>Windows dosyaları izlemek için yapılandırma

Windows bilgisayarlarda izlemeye dosyaları yapılandırmak için aşağıdaki adımları kullanın:

1. Otomasyon hesabınızı seçin **değişiklik izleme** altında **yapılandırma yönetimi**. Tıklayın **ayarlarını Düzenle** (dişli simgesi).
2. Üzerinde **değişiklik izleme** sayfasında **Windows dosyaları**, ardından **+ Ekle** izlemek için yeni bir dosya eklemek için.
3. Üzerinde **değişiklik izleme için Windows dosyası ekleme**, izlemek ve dosya bilgilerini girin **Kaydet**.

|Özellik  |Açıklama  |
|---------|---------|
|Etkin     | Ayarın uygulanmış olup olmadığını belirler.        |
|Öğe Adı     | İzlenecek dosyanın kolay adı.        |
|Grup     | Dosyaları mantıksal olarak gruplamak için bir grup adı.        |
|Yolu Gir     | Dosyanın denetleneceği yol. Örneğin: “c:\temp\myfile.txt”       |

### <a name="configure-windows-registry-keys-to-track"></a>Windows kayıt defteri anahtarlarını izlemek için

Windows bilgisayarlarda kayıt defteri anahtarı izlemeyi yapılandırmak için aşağıdaki adımları kullanın:

1. Otomasyon hesabınızı seçin **değişiklik izleme** altında **yapılandırma yönetimi**. Tıklayın **ayarlarını Düzenle** (dişli simgesi).
2. Üzerinde **değişiklik izleme** sayfasında **Windows kayıt defteri**, ardından **+ Ekle** izlemek için yeni bir kayıt defteri anahtarı eklemek için.
3. Üzerinde **için değişiklik izleme Windows kayıt defteri Ekle**, izlemek ve anahtar bilgilerini girin **Kaydet**.

|Özellik  |Açıklama  |
|---------|---------|
|Etkin     | Ayarın uygulanmış olup olmadığını belirler.        |
|Öğe Adı     | İzlenecek dosyanın kolay adı.        |
|Grup     | Dosyaları mantıksal olarak gruplamak için bir grup adı.        |
|Windows Kayıt Defteri Anahtarı   | Dosyanın denetleneceği yol. Örneğin: "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup"      |

## <a name="limitations"></a>Sınırlamalar

Değişiklik izleme çözümü, aşağıdaki öğeler şu anda desteklemez:

* İzleme Windows dosyası için klasörleri (dizin)
* İzleme Windows dosyası için özyineleme
* Joker karakterler için Windows dosya izleme
* İzleme Windows kayıt defteri için özyineleme
* Yol değişkenleri
* Ağ dosya sistemleri
* Dosya İçeriği

Diğer sınırlamalar:

* **En büyük dosya boyutu** sütun ve değerlerini geçerli uygulamada kullanılmayan.
* 30 dakikalık toplama döngüsü içinde 2500'den fazla dosya toplarsanız çözüm performans düzeyi düşürülmüş.
* Ağ trafiği yüksek olduğunda, değişiklik kayıtları görüntülemek için altı saat sürebilir.
* Bilgisayar, bir bilgisayar kapatıldı yapılandırmasını değiştirirseniz, önceki yapılandırmaya ait değiştiğinde post.

## <a name="known-issues"></a>Bilinen sorunlar

Değişiklik izleme çözümü şu anda aşağıdaki sorunlarla karşılaşıyor:

* İçin Windows 10 Creators Update ve Windows Server 2016 Core RS3 makineleri düzeltme güncelleştirmelerini toplanmadı.

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

### <a name="windows-service-tracking"></a>Windows hizmeti izleme

Windows Hizmetleri için varsayılan toplama sıklığı 30 dakikadır. Sıklığı yapılandırma Git **değişiklik izleme**. Altında **ayarlarını Düzenle** üzerinde **Windows Hizmetleri** sekmesinde, Windows Hizmetleri'nden için toplama sıklığı için en çok 30 dakika olarak 10 saniye olan en kısa sürede değiştirmenize izin veren bir kaydırıcı yoktur. Kaydırıcı çubuğunu taşımak istediğiniz sıklığı ve onu otomatik olarak kaydeder.

![Windows Hizmetleri kaydırıcı](./media/automation-change-tracking/windowservices.png)

Aracı yalnızca değişiklikleri izler, bu aracı performansını iyileştirir. Hizmet ilk durumuna geri döndürüldü., eşiğin üstüne ayarlayarak değişiklikleri eksik olabilir. Sıklığı ayarını daha küçük bir değere, aksi takdirde atlanabilir değişiklikleri yakalamak sağlar.

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
> |**HKEY\_yerel\_MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Windows Gezgini ve genellikle çalıştırma işlem içi Explorer.exe ile doğrudan bağlama izleyiciler ortak autostart girdileri.|
> |**HKEY\_yerel\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers**|
|&nbsp;&nbsp;&nbsp;&nbsp;İzleyici simge için işleyici kaydı yer.|
|**HKEY\_yerel\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers**|
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

## <a name="use-change-tracking"></a>Değişiklik izlemeyi kullanma

Çözüm etkinleştirildikten sonra değişikliklerin özeti için izlenen bilgisayarlar seçerek görüntüleyebilirsiniz **değişiklik izleme** altında **yapılandırma yönetimi** Otomasyon hesabınızdaki.

Değişiklikleri bilgisayarlarınız ve ardından-ayrıntıya her olayla ilgili ayrıntıları görüntüleyebilirsiniz. Değişiklik türü ve zaman aralıklarına göre ayrıntılı bilgiler ve grafik sınırlamak için grafiğin üstünde açılan listeler kullanılabilir. ' A tıklayın ve özel bir zaman aralığı seçmek için grafiğe sürükleyin.

![Değişiklik izleme panosunun görüntüsü](./media/automation-change-tracking/change-tracking-dash01.png)

Bir değişiklik veya olay'ı tıklatarak bu değişiklik hakkında ayrıntılı bilgi getirir. Örnekte görebileceğiniz gibi hizmet başlatma türünü otomatik olarak el ile değiştirildi.

![değişiklik ayrıntıları izleme görüntüsü](./media/automation-change-tracking/change-tracking-details.png)

## <a name="search-logs"></a>Günlüklerinde arama yapma

Portalda sağlanan Ayrıntılar ek olarak, arama günlüklerine karşı yapılabilir. İle **değişiklik izleme** sayfası açıldığında, tıklayın **Log Analytics**, bu açılır **günlük araması** sayfası.

### <a name="sample-queries"></a>Örnek sorgular

Aşağıdaki tabloda bu çözüm tarafından toplanan kayıtlarına ilişkin örnek günlük aramaları değiştirme sağlar:

|Sorgu  |Açıklama  |
|---------|---------|
|ConfigurationData<br>&#124;Burada ConfigDataType "WindowsServices" ve SvcStartupType == "Auto" ==<br>&#124;Burada SvcState "Durduruldu" ==<br>&#124;Özetleme arg_max(TimeGenerated, *) SoftwareName, bilgisayar tarafından         | Otomatik olarak ayarlanmış, ancak durduruldu olarak bildirilen bir Windows Hizmetleri için en son Envanter kayıtlarının gösterir<br>Sonuçları en son kayıt için söz konusu ve bilgisayar ölçütlerine sınırlıdır      |
|ConfigurationChange<br>&#124;Burada ConfigChangeType "Yazılım" ve ChangeCategory == "Kaldırıldı" ==<br>&#124;TimeGenerated desc sıralama|Kaldırılacak yazılım için değişiklik kayıtları gösterir|

## <a name="next-steps"></a>Sonraki adımlar

Öğretici çözümünü kullanma hakkında daha fazla bilgi edinmek için değişiklik izleme'ı ziyaret edin:

> [!div class="nextstepaction"]
> [Ortamınızdaki değişikliklerle ilgili sorunları giderme](automation-tutorial-troubleshoot-changes.md)

* Kullanım [Log Analytics'te günlük aramaları](../log-analytics/log-analytics-log-searches.md) ayrıntılı değişiklik izleme verileri görüntülemek için.
