---
title: "Azure Automation Değişiklikleri İzle"
description: "Değişiklik izleme çözümü, yazılım ve ortamınızda ortaya Windows hizmet değişiklikleri belirlemenize yardımcı olur."
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: article
manager: carmonm
ms.devlang: na
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 06034a87d6015a057c01c2bc87ae4db9fba1269a
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="track-changes-in-your-environment-with-the-change-tracking-solution"></a>Değişiklik izleme çözümü ile ortamınızdaki Değişiklikleri İzle

Bu makalede, ortamınızdaki değişiklikleri kolayca belirlemek için değişiklik izleme çözümü kullanmanıza yardımcı olur. Çözüm, Windows ve Linux yazılım, Windows ve Linux dosyaları, Windows kayıt defteri anahtarları, Windows Hizmetleri ve Linux Daemon değişiklikleri izler. Yapılandırma değişiklikleri tanımlayan işletim sorunları belirlemenize yardımcı olabilir.

Yüklü yazılım, Windows Hizmetleri, Windows kayıt defteri ve dosya ve izlenen sunuculara Linux Daemon değişiklikleri işleme bulutta günlük analizi hizmetine gönderilir. Mantığı alınan verilere uygulanır ve bulut hizmeti verilerini kaydeder. Değişiklik izleme panosunda bilgileri kullanarak, sunucu altyapınızda yapılan değişiklikler kolayca görebilirsiniz.

## <a name="enable-change-tracking-and-inventory"></a>Değişiklik İzlemeyi ve Sayımı Etkinleştirme


Değişiklikleri izlemeye başlamak için değişiklik izleme ve stok çözüm Automation hesabınız için etkinleştirmeniz gerekir.

1. Azure Portal'da, Automation hesabınızı gidin
1. Seçin **değişiklik izleme** altında **yapılandırma**.
2. Varolan bir günlük analizi çalışma alanı seçin veya **yeni çalışma alanı oluştur** tıklatıp **etkinleştirmek**.

Bu, automation hesabınız için çözüm sağlar. Çözüm etkinleştirmek için 15 dakika sürebilir. Çözüm etkinleştirildiğinde mavi başlık size bildirir. Geri gidin **değişiklik izleme** çözümü yönetmek için sayfa.

## <a name="configuring-change-tracking-and-inventory"></a>Değişiklik izleme ve stok yapılandırma

Bilgi edinmek için nasıl çözüme yerleşik bilgisayarlara ziyaret edin: [ekleme Otomasyon çözümleri](automation-onboard-solutions-from-automation-account.md). Yeni bir dosya veya izlemek için kayıt defteri anahtarı etkinleştirdiğinizde, değişiklik izleme ve stok için etkinleştirilir.

### <a name="configure-linux-files-to-track"></a>Linux dosyaları izlemek için yapılandırma

Dosya izleme Linux bilgisayarlarda yapılandırmak için aşağıdaki adımları kullanın:

1. Otomasyon hesabınızı seçin **değişiklik izleme** altında **yapılandırma yönetimi**. Tıklatın **ayarlarını Düzenle** (dişli symbol).
2. Üzerinde **değişiklik izleme** sayfasında, **Linux dosyaları**, ardından **+ Ekle** izlemek için yeni bir dosya eklemek için.
3. Üzerinde **değişiklik izleme için Linux Dosya Ekle**, dosya veya dizin tıklatıp izlemek için bilgileri girin **kaydetmek**.

|Özellik  |Açıklama  |
|---------|---------|
|Etkin     | Ayar uygulanmış olup olmadığını belirler.        |
|Öğe Adı     | İzlenmesi gereken dosyasının kolay adı.        |
|Grup     | Dosyaları mantıksal gruplandırma için bir grup adı.        |
|Yolu Gir     | Dosya için denetlenecek yol. Örneğin: "/etc/*.conf"       |
|Yol Türü     | İzlenen, olası değerler öğesi türü: dosya ve dizin.        |
|Özyineleme     | İzlenecek öğe aranırken özyinelemenin kullanılıp kullanılmadığını belirler.        |
|Sudo Kullan     | Bu ayar, öğe denetlenirken sudonun kullanılıp kullanılmadığını belirler.         |
|Bağlantılar     | Bu ayar, dizinleri dolaşırken sembolik bağlantıların nasıl ele alındığını belirler.<br> **Yoksay** - sembolik bağlantılar yoksayar ve başvurulan dosya veya dizinlerin içermez.<br>**İzleyin** - sembolik bağlantılar sırasında özyineleme izler ve ayrıca başvurulan dosya veya dizinlerin içerir.<br>**Yönetme** - sembolik bağlantılar izler ve döndürülen içeriğinin değiştirilmesine izin verir.     |

> [!NOTE]
> “Yönet” bağlantıları seçeneği önerilmez. Dosya içeriğini alma desteklenmiyor.

### <a name="configure-windows-files-to-track"></a>Windows dosyaları izlemek için yapılandırma

Windows bilgisayarlarda izleme dosyaları yapılandırmak için aşağıdaki adımları kullanın:

1. Otomasyon hesabınızı seçin **değişiklik izleme** altında **yapılandırma yönetimi**. Tıklatın **ayarlarını Düzenle** (dişli symbol).
2. Üzerinde **değişiklik izleme** sayfasında, **Windows dosyalarını**, ardından **+ Ekle** izlemek için yeni bir dosya eklemek için.
3. Üzerinde **değişiklik izleme için Windows Dosya Ekle**, izlemek ve tıklatın dosyaya bilgilerini girin **kaydetmek**.

|Özellik  |Açıklama  |
|---------|---------|
|Etkin     | Ayar uygulanmış olup olmadığını belirler.        |
|Öğe Adı     | İzlenmesi gereken dosyasının kolay adı.        |
|Grup     | Dosyaları mantıksal gruplandırma için bir grup adı.        |
|Yolu Gir     | Dosyanın denetleneceği yol. Örneğin: “c:\temp\myfile.txt”       |

### <a name="configure-windows-registry-keys-to-track"></a>Windows kayıt defteri anahtarlarını izlemek için

Windows bilgisayarlarda kayıt defteri anahtarı izleme yapılandırmak için aşağıdaki adımları kullanın:

1. Otomasyon hesabınızı seçin **değişiklik izleme** altında **yapılandırma yönetimi**. Tıklatın **ayarlarını Düzenle** (dişli symbol).
2. Üzerinde **değişiklik izleme** sayfasında, **Windows kayıt defteri**, ardından **+ Ekle** izlemek için yeni bir kayıt defteri anahtarı eklemek için.
3. Üzerinde **değişiklik izleme için Windows kayıt defteri eklemek**, izlemek ve'ı tıklatın anahtarı için bilgileri girin **kaydetmek**.

|Özellik  |Açıklama  |
|---------|---------|
|Etkin     | Ayar uygulanmış olup olmadığını belirler.        |
|Öğe Adı     | İzlenmesi gereken dosyasının kolay adı.        |
|Grup     | Dosyaları mantıksal gruplandırma için bir grup adı.        |
|Windows Kayıt Defteri Anahtarı   | Dosya için denetlenecek yol. Örneğin: "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Kabuk Folders\Common Başlangıç"      |

## <a name="limitations"></a>Sınırlamalar

Değişiklik izleme çözümü aşağıdaki öğeler şu anda desteklemiyor:

* Klasörleri (dizinleri) Windows için dosya izleme
* Windows dosya izleme için özyineleme
* Joker karakterler Windows için dosya izleme
* Yol değişkenleri
* Ağ dosya sistemleri
* Dosya içeriği

Diğer sınırlamaları:

* **En büyük dosya boyutu** sütun ve değerlerini geçerli uygulamasında kullanılmayan.
* 30 dakikalık toplama döngüsü içinde birden fazla 2500 dosya toplarsanız çözüm performansın düşük.
* Ağ trafiği yüksek olduğunda, değişiklik kayıtları görüntülemek altı saate kadar sürebilir.
* Bilgisayar, bir bilgisayar kapalıyken yapılandırmasını değiştirirseniz, önceki yapılandırmaya ait değişiklikleri sonrasında.

## <a name="known-issues"></a>Bilinen Sorunlar

Değişiklik izleme çözümü şu anda aşağıdaki sorunları yaşıyor:
* Windows 10 oluşturucuları Update ve Windows Server 2016 çekirdek RS3 makineleri için düzeltme güncelleştirmelerini toplanmadı.

## <a name="change-tracking-data-collection-details"></a>Veri toplama ayrıntılı izleme değiştirme

Aşağıdaki tabloda değişiklik türleri için veri toplama sıklığını gösterir. Her türü için geçerli durumunun veri anlık görüntüsü de en az her 24 saatte bir yenilenir:

| **Değişiklik türü** | **Sıklık** |
| --- | --- |
| Windows kayıt defteri | 50 dakika | 
| Windows dosya | 30 dakika | 
| Linux file | 15 dakika | 
| Windows hizmetleri | 30 dakika | 
| Linux Daemon | 5 dakika |
| Windows yazılım | 30 dakika | 
| Linux yazılım | 5 dakika | 

### <a name="registry-key-change-tracking"></a>Kayıt defteri anahtarı değişiklik izleme

Kayıt defteri anahtarlarını yapılan değişiklikleri izleme amacı, burada etkinleştirebilir üçüncü taraf kodu ve kötü amaçlı yazılım genişletilebilirlik noktaları sabitleme olmaktır. Aşağıdaki listede, önceden yapılandırılmış kayıt defteri anahtarları listesini gösterir. Bu anahtarları yapılandırılmış ancak etkin değil. Bu kayıt defteri anahtarları izlemek için her biri etkinleştirmeniz gerekir.

> [!div class="mx-tdBreakAll"]
> |  |
> |---------|
> |**HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Windows Gezgini ve genellikle çalışma işlemdeki Explorer.exe ile doğrudan kanca izleyiciler ortak autostart girişleri.    |
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Başlangıçta çalıştırılmasını izleyiciler betikler.     |
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Shutdown**    |
|&nbsp;&nbsp;&nbsp;&nbsp;İzleyiciler kapatma sırasında çalışan komutlar.     |
> |**HKEY\_yerel\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Kendi Windows hesabı kullanıcı işaretlerine önce yüklenen anahtarları izler. Anahtarı, 64-bit bilgisayarlar üzerinde çalışan 32-bit programlar için kullanılır.    |
> |**HKEY\_yerel\_MACHINE\SOFTWARE\Microsoft\Active Setup\Installed bileşenleri**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Uygulama ayarlarına değişiklikleri izler.     |
> |**HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Windows Gezgini ve genellikle çalışma işlemdeki Explorer.exe ile doğrudan kanca izleyiciler ortak autostart girişleri.|
> |**HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Windows Gezgini ve genellikle çalışma işlemdeki Explorer.exe ile doğrudan kanca izleyiciler ortak autostart girişleri.|
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers**|
|&nbsp;&nbsp;&nbsp;&nbsp;İzleyici simgesi için işleyici kaydı kaplama.|
|**HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers**|
|&nbsp;&nbsp;&nbsp;&nbsp;İzleyici simgesi için 64-bit bilgisayarlar üzerinde çalışan 32-bit programlar için işleyici kaydı kaplama.|
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects**|
|&nbsp;&nbsp;&nbsp;&nbsp;Internet Explorer için yeni tarayıcı Yardımcısı nesnesi eklentilerin izleyiciler. Geçerli sayfanın belge nesne modeli (DOM) erişmek için ve gezinti denetlemek için kullanılır.|
> |**HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects**|
|&nbsp;&nbsp;&nbsp;&nbsp;Internet Explorer için yeni tarayıcı Yardımcısı nesnesi eklentilerin izleyiciler. Geçerli sayfanın belge nesne modeli (DOM) erişmek için ve 64-bit bilgisayarlar üzerinde çalışan 32-bit programlar için Gezinti denetlemek için kullanılır.|
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Internet Explorer\Extensions**|
|&nbsp;&nbsp;&nbsp;&nbsp;Özel araç menüleri ve özel araç çubuğu düğmeleri gibi yeni Internet Explorer uzantıları için İzleyici.|
> |**HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions**|
|&nbsp;&nbsp;&nbsp;&nbsp;Özel araç menüleri ve 64-bit bilgisayarlar üzerinde çalışan 32-bit programlar için özel araç çubuğu düğmeleri gibi yeni Internet Explorer uzantıları için İzleyici.|
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32**|
|&nbsp;&nbsp;&nbsp;&nbsp;Wavemapper, wave1 ve wave2, msacm.imaadpcm, .msadpcm, .msgsm610 ve vidc ile ilişkili 32-bit sürücüleri izler. SİSTEM [drivers] bölümünde benzer. INI dosyası.|
> |**HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32**|
|&nbsp;&nbsp;&nbsp;&nbsp;İzleyiciler 32-bit sürücüleri wavemapper, wave1 ve wave2, msacm.imaadpcm, .msadpcm, .msgsm610 ve 64-bit bilgisayarlar üzerinde çalışan 32-bit programlar için vidc ile ilişkili. SİSTEM [drivers] bölümünde benzer. INI dosyası.|
> |**HKEY\_yerel\_MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls**|
|&nbsp;&nbsp;&nbsp;&nbsp;Bilinen veya sık kullanılan sistem DLL'leri listesi izler; Bu sistem, sistem DLL'leri Truva atı sürümlerinde bırakarak zayıf uygulama dizin izinlerini yararlanmasını kişilerin engeller.|
> |**HKEY\_yerel\_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify**|
|&nbsp;&nbsp;&nbsp;&nbsp;Winlogon, Windows işletim sistemi için etkileşimli oturum açma desteği modeli olay bildirimleri almak mümkün olan paketlerin listesini izler.|

## <a name="use-change-tracking"></a>Değişiklik izlemeyi kullanma

Çözüm etkinleştirildikten sonra değişikliklerin özetini izlenen bilgisayarlarınız için seçerek görüntüleyebilirsiniz **değişiklik izleme** altında **yapılandırma yönetimi** Otomasyon hesabınızda.

Değişiklikleri bilgisayarlarınız ve ardından-ayrıntıya her olayla ilgili ayrıntıları görüntüleyebilirsiniz. Açılan listelerini, grafik ve değişiklik türü ve zaman aralıklarını temel alarak ayrıntılı bilgileri sınırlamak için grafiğin üstünde. ' I tıklatın ve özel zaman aralığı seçmek için grafikte sürükleyin.

![Değişiklik izleme Panosu görüntüsü](./media/automation-change-tracking/change-tracking-dash01.png)

Bir değişiklik veya olay'ı tıklatarak bu değişiklik hakkında ayrıntılı bilgileri getirir. Örnekte görebildiğiniz gibi hizmet başlangıç türünü otomatik olarak el ile değiştirildi.

![değişiklik ayrıntıları izleme görüntüsü](./media/automation-change-tracking/change-tracking-details.png)

## <a name="search-logs"></a>Arama günlükleri

Portalı'nda sağlanan Ayrıntılar ek olarak, arama günlüklerini karşı yapılabilir. İle **değişiklik izleme** sayfa açık tıklatın **günlük analizi**, bu açılır **günlük arama** sayfası.

### <a name="sample-queries"></a>Örnek sorgular

Aşağıdaki tabloda bu çözüm tarafından toplanan kayıtları örnek günlük arar değiştirmek sağlar:

|Sorgu  |Açıklama  |
|---------|---------|
|ConfigurationData<br>&#124;Burada ConfigDataType "WindowsServices" ve SvcStartupType == "Auto" ==<br>&#124;Burada SvcState "Durduruldu" ==<br>&#124;özetlemek arg_max(TimeGenerated, *) SoftwareName, bilgisayar tarafından         | Otomatik olarak ayarlanmış, ancak durduruldu olarak raporlandı Windows Hizmetleri için en son Envanter kayıtlarının gösterir<br>Bu SoftwareName ve bilgisayar için en son kayda sonuçları sınırlıdır      |
|ConfigurationChange<br>&#124;Burada ConfigChangeType "Yazılım" ve ChangeCategory == "Kaldırıldı" ==<br>&#124;TimeGenerated desc sıralama|Kaldırılan yazılım değişikliği kayıtları gösterir|

## <a name="next-steps"></a>Sonraki adımlar

Öğretici, çözüm kullanma hakkında daha fazla bilgi edinmek için değişiklik izleme ziyaret edin:

> [!div class="nextstepaction"]
> [Ortamınızdaki değişiklikler sorun giderme](automation-tutorial-troubleshoot-changes.md)

* Kullanım [günlük analizi aramaları oturum](../log-analytics/log-analytics-log-searches.md) ayrıntılı değişiklik izleme verilerini görüntülemek için.
