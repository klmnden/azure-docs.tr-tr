---
title: "Azure günlük analizi ile değişiklikleri izle | Microsoft Docs"
description: "Günlük analizi değişiklik izleme çözümünde yazılım ve ortamınızda ortaya Windows hizmet değişiklikleri belirlemenize yardımcı olur."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: f8040d5d-3c89-4f0c-8520-751c00251cb7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ede3519b0b61ed20d85ea141dc6dee2505420448
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="track-software-changes-in-your-environment-with-the-change-tracking-solution"></a>Değişiklik izleme çözümü ile ortamınızdaki yazılım değişiklikleri izle

![Değişiklik izleme simgesi](./media/log-analytics-change-tracking/change-tracking-symbol.png)

Bu makalede değişiklik izleme çözümü kolayca ortamınızdaki değişiklikleri belirlemek için günlük analizi kullanmanıza yardımcı olur. Çözüm, Windows ve Linux yazılım, Windows dosya ve kayıt defteri anahtarları, Windows Hizmetleri ve Linux Daemon değişiklikleri izler. Yapılandırma değişiklikleri tanımlayan işletim sorunları belirlemenize yardımcı olabilir.

Güncelleştirme yüklediğiniz aracı türü için çözüm yükleyin. Yüklü yazılım, Windows Hizmetleri ve izlenen sunuculara Linux Daemon değişiklikleri salt okunurdur. Ardından, veri işleme için günlük analizi hizmet olarak bulutta gönderilir. Mantığı alınan verilere uygulanır ve bulut hizmeti verilerini kaydeder. Değişiklik izleme panosunda bilgileri kullanarak, sunucu altyapınızda yapılan değişiklikler kolayca görebilirsiniz.

## <a name="installing-and-configuring-the-solution"></a>Yükleme ve çözüm yapılandırılıyor
Yüklemek ve çözüm yapılandırmak için aşağıdaki bilgileri kullanın.

* Bilmeniz gereken bir [Windows](log-analytics-windows-agent.md), [Operations Manager](log-analytics-om-agents.md), veya [Linux](log-analytics-linux-agents.md) değişiklikleri izlemek istediğiniz her bilgisayarda aracı.
* Değişiklik izleme çözümü, OMS çalışma alanından ekleyin [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ChangeTrackingOMS?tab=Overview). Veya, bilgileri kullanarak çözüm ekleyebilirsiniz [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md). Ek yapılandırma gerekli değildir.

### <a name="configure-linux-files-to-track"></a>Linux dosyaları izlemek için yapılandırma
Linux bilgisayarları izlemek için dosyaları yapılandırmak için aşağıdaki adımları kullanın.

1. OMS Portalı'nda tıklatın **ayarları** (dişli symbol).
2. Üzerinde **ayarları** sayfasında, **veri**ve ardından **Linux dosya izleme**.
3. Linux dosya değişiklik izleme altında izlemek ve ardından istediğiniz dosyasının dosya adı dahil olmak üzere tüm yol yazın **Ekle** simgesi. Örneğin: "/etc/*.conf"
4. **Kaydet**’e tıklayın.  

> [!NOTE]
> Linux dosya izleme dizini izleme, dizinler ve izleme joker aracılığıyla özyineleme dahil olmak üzere ek özellikleri vardır.

### <a name="configure-windows-files-to-track"></a>Windows dosyaları izlemek için yapılandırma
Windows bilgisayarlarda izlemek için dosyaları yapılandırmak için aşağıdaki adımları kullanın.

1. OMS Portalı'nda tıklatın **ayarları** (dişli symbol).
2. Üzerinde **ayarları** sayfasında, **veri**ve ardından **Windows dosya izleme**.
3. Windows dosya değişiklik izleme altında izlemek ve ardından istediğiniz dosyasının dosya adı dahil olmak üzere tüm yol yazın **Ekle** simgesi. Örneğin: C:\Program Files (x86) \Internet Explorer\iexplore.exe veya C:\Windows\System32\drivers\etc\hosts.
4. **Kaydet**’e tıklayın.  
   ![Windows dosya değişiklik izleme](./media/log-analytics-change-tracking/windows-file-change-tracking.png)

### <a name="configure-windows-registry-keys-to-track"></a>Windows kayıt defteri anahtarlarını izlemek için
Windows bilgisayarlarda izlemek için kayıt defteri anahtarlarını yapılandırmak için aşağıdaki adımları kullanın.

1. OMS Portalı'nda tıklatın **ayarları** (dişli symbol).
2. Üzerinde **ayarları** sayfasında, **veri**ve ardından **Windows kayıt defteri izleme**.
3. Windows kayıt defteri değişikliği izleme altında izlemek ve ardından istediğiniz anahtarın tamamının yazın **Ekle** simgesi.
4. **Kaydet**’e tıklayın.  
   ![Windows kayıt defteri değişiklik izleme](./media/log-analytics-change-tracking/windows-registry-change-tracking.png)

### <a name="explanation-of-linux-file-collection-properties"></a>Linux dosya koleksiyonu özellikleri açıklaması
1. **Tür**
   * **Dosya** (rapor dosya meta verileri - boyutu, değiştirilme tarihi, karma, vs.)
   * **Dizin** (rapor dizini meta verilerinin - boyutu, değiştirilme tarihi vb..)
2. **Bağlantılar** (diğer dosyaları veya dizinleri simgesel başvurular işleme Linux)
   * **Yoksay** (simgesel bağlantı başvurulan dosya veya dizinlerin içermeyecek şekilde özyineleme sırasında yoksay)
   * **İzleyin** (simgesel bağlantı da başvurulan dosya veya dizinlerin içerecek şekilde özyineleme sırasında izleyin)
   * **Yönetme** (simgesel bağlantı izleyin ve döndürülen içeriği işlenmesi alter)

   > [!NOTE]   
   > "Manage" bağlantıları seçeneği önerilmez. Dosya içeriği alma desteklenmiyor.

3. **Recurse** (klasör düzeyleri arasında Recurse ve yol ifadesi toplantı tüm dosyaları izlemek)
4. **Sudo** (access dosyalarını veya sudo ayrıcalık gerektiren dizinleri etkinleştir)

### <a name="limitations"></a>Sınırlamalar
Değişiklik izleme çözümü aşağıdaki öğeler şu anda desteklemiyor:

* Windows dosya izleme klasörleri (dizinleri)
* Windows dosya izleme için özyineleme
* Joker karakterler Windows dosya izleme
* Yol değişkenleri
* Ağ dosya sistemleri
* Dosya içeriği

Diğer sınırlamaları:

* **En büyük dosya boyutu** sütun ve değerlerini geçerli uygulamasında kullanılmayan.
* 30 dakikalık toplama döngüsü içinde birden fazla 2500 dosya toplarsanız çözüm performansın düşük.
* Ağ trafiği yüksek olduğunda, değişikliği kayıtları en fazla altı saat için görüntülenecek kadar sürebilir.
* Bilgisayar, bir bilgisayar kapalıyken yapılandırmasını değiştirirseniz, önceki yapılandırmaya ait dosya değişiklikleri sonrasında.

### <a name="known-issues"></a>Bilinen Sorunlar
Değişiklik izleme çözümü şu anda aşağıdaki sorunları yaşıyor:
* Windows 10 oluşturucuları Update ve Windows Server 2016 çekirdek RS3 makineleri için düzeltme güncelleştirmelerini toplanmadı.

## <a name="change-tracking-data-collection-details"></a>Veri toplama ayrıntılı izleme değiştirme
Değişiklik izleme, yazılım envanteri ve etkinleştirdiğiniz aracıları kullanarak Windows hizmeti meta verileri toplar.

Aşağıdaki tabloda, veri toplama yöntemleri ve değişiklik izleme verileri nasıl toplanır ilgili diğer ayrıntıları gösterir.

| Platform | Doğrudan Aracısı | Operations Manager Aracısı | Linux Aracısı | Azure Storage | Operations Manager gerekli? | Operations Manager Aracısı verilerinin yönetim grubu gönderilen | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Windows ve Linux | &#8226; | &#8226; | &#8226; |  |  | &#8226; | değişiklik türüne bağlı olarak 50 dakika için 5 dakika. Daha fazla bilgi için aşağıdaki tabloda görüntüleyin. |


Aşağıdaki tabloda değişiklik türleri için veri toplama sıklığını gösterir.

| **Değişiklik türü** | **frequency** | **Mu****Aracısı****bulunduğunda farklar Gönder?**  |
| --- | --- | --- |
| Windows kayıt defteri | 50 dakika | Hayır |
| Windows dosya | 30 dakika | Evet. 24 saat içindeki herhangi bir değişiklik varsa, bir anlık görüntü gönderilir. |
| Linux file | 15 dakika | Evet. 24 saat içindeki herhangi bir değişiklik varsa, bir anlık görüntü gönderilir. |
| Windows hizmetleri | 30 dakika | Evet, değişiklikler bulunduğunda 30 dakikada. 24 saatte bir anlık görüntü, değişiklik bağımsız olarak gönderilir. Bu nedenle, anlık görüntü bile gönderilen hiçbir değişiklik olduğu. |
| Linux Daemon | 5 dakika | Evet. 24 saat içindeki herhangi bir değişiklik varsa, bir anlık görüntü gönderilir. |
| Windows yazılım | 30 dakika | Evet, değişiklikler bulunduğunda 30 dakikada. 24 saatte bir anlık görüntü, değişiklik bağımsız olarak gönderilir. Bu nedenle, anlık görüntü bile gönderilen hiçbir değişiklik olduğu. |
| Linux yazılım | 5 dakika | Evet. 24 saat içindeki herhangi bir değişiklik varsa, bir anlık görüntü gönderilir. |

### <a name="registry-key-change-tracking"></a>Kayıt defteri anahtarı değişiklik izleme

Günlük analizi izleme ve değişiklik izleme çözümü ile izleme Windows kayıt defteri gerçekleştirir. Kayıt defteri anahtarlarını yapılan değişiklikleri izleme amacı, burada etkinleştirebilir üçüncü taraf kodu ve kötü amaçlı yazılım genişletilebilirlik noktaları sabitleme olmaktır. Aşağıdaki liste, çözümü tarafından izlenen ve her neden izlenen kayıt defteri anahtarlarının varsayılan gösterir.

- HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup
    - Başlangıçta çalıştırılmasını izleyiciler betikler.
- HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Shutdown
    - İzleyiciler kapatma sırasında çalışan komutlar.
- HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run
    - Kendi Windows hesabı kullanıcı işaretlerine önce yüklenen anahtarları izler. Anahtarı, 64-bit bilgisayarlar üzerinde çalışan 32-bit programlar için kullanılır.
- HKEY\_LOCAL\_MACHINE\SOFTWARE\Microsoft\Active Setup\Installed Components
    - Uygulama ayarlarına değişiklikleri izler.
- HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers
    - Windows Gezgini ve genellikle çalışma işlemdeki Explorer.exe ile doğrudan kanca izleyiciler ortak autostart girişleri.
- HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers
    - Windows Gezgini ve genellikle çalışma işlemdeki Explorer.exe ile doğrudan kanca izleyiciler ortak autostart girişleri.
- HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\Background\ShellEx\ContextMenuHandlers
    - Windows Gezgini ve genellikle çalışma işlemdeki Explorer.exe ile doğrudan kanca izleyiciler ortak autostart girişleri.
- HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
    - İzleyici simgesi için işleyici kaydı kaplama.
- HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
    - İzleyici simgesi için 64-bit bilgisayarlar üzerinde çalışan 32-bit programlar için işleyici kaydı kaplama.
- HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects
    - Internet Explorer için yeni tarayıcı Yardımcısı nesnesi eklentilerin izleyiciler. Geçerli sayfanın belge nesne modeli (DOM) erişmek için ve gezinti denetlemek için kullanılır.
- HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects
    - Internet Explorer için yeni tarayıcı Yardımcısı nesnesi eklentilerin izleyiciler. Geçerli sayfanın belge nesne modeli (DOM) erişmek için ve 64-bit bilgisayarlar üzerinde çalışan 32-bit programlar için Gezinti denetlemek için kullanılır.
- HKEY\_LOCAL\_MACHINE\Software\Microsoft\Internet Explorer\Extensions
    - Özel araç menüleri ve özel araç çubuğu düğmeleri gibi yeni Internet Explorer uzantıları için İzleyici.
- HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions
    - Özel araç menüleri ve 64-bit bilgisayarlar üzerinde çalışan 32-bit programlar için özel araç çubuğu düğmeleri gibi yeni Internet Explorer uzantıları için İzleyici.
- HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32
    - Wavemapper, wave1 ve wave2, msacm.imaadpcm, .msadpcm, .msgsm610 ve vidc ile ilişkili 32-bit sürücüleri izler. SİSTEM [drivers] bölümünde benzer. INI dosyası.
- HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32
    - İzleyiciler 32-bit sürücüleri wavemapper, wave1 ve wave2, msacm.imaadpcm, .msadpcm, .msgsm610 ve 64-bit bilgisayarlar üzerinde çalışan 32-bit programlar için vidc ile ilişkili. SİSTEM [drivers] bölümünde benzer. INI dosyası.
- HKEY\_yerel\_MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls
    - Bilinen veya sık kullanılan sistem DLL'leri listesi izler; Bu sistem, sistem DLL'leri Truva atı sürümlerinde bırakarak zayıf uygulama dizin izinlerini yararlanmasını kişilerin engeller.
- HKEY\_LOCAL\_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify
    - Winlogon, Windows işletim sistemi için etkileşimli oturum açma desteği modeli olay bildirimleri almak mümkün olan paketlerin listesini izler.


## <a name="use-change-tracking"></a>Değişiklik izlemeyi kullanma
Çözüm yüklendikten sonra değişikliklerin özetini izlenen sunucularınız için kullanarak görüntüleyebileceğiniz **değişiklik izleme** döşemesinin **genel bakış** OMS sayfasında.

![Değişiklik izleme kutucuğu görüntüsü](./media/log-analytics-change-tracking/change-tracking-tile.png)

Altyapı ve ardından-ayrıntıya Ayrıntılar aşağıdaki kategorilerde değişiklik görüntüleyebilirsiniz:

* Yazılım ve Windows Hizmetleri için yapılandırma türüne göre değişiklikler
* Uygulamaları ve güncelleştirmeleri tek tek sunucular için yazılım değişiklikleri
* Yazılım değişiklikleri her uygulama için toplam sayısı
* Linux paketleri
* Windows hizmet değişiklikleri tek tek sunucular için
* Linux daemon değişiklikleri

![Değişiklik izleme Panosu görüntüsü](./media/log-analytics-change-tracking/change-tracking-dash01.png)

![Değişiklik izleme Panosu görüntüsü](./media/log-analytics-change-tracking/change-tracking-dash02.png)

### <a name="to-view-changes-for-any-change-type"></a>Görüntülemek için herhangi bir için tür değişiklik
1. Üzerinde **genel bakış** sayfasında, **değişiklik izleme** döşeme.
2. Üzerinde **değişiklik izleme** Pano, değişiklik türü Kanatlar birinde özet bilgileri gözden geçirin ve ardından içinde hakkında ayrıntılı bilgi görüntülemek için **günlük arama** sayfası.
3. Herhangi bir günlük arama sayfası üzerinde sonuçları zaman, ayrıntılı sonuçları ve günlük arama geçmişi görüntüleyebilirsiniz. Sonuçları daraltmak için modelleri göre de filtre uygulayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [günlük analizi aramaları oturum](log-analytics-log-searches.md) ayrıntılı değişiklik izleme verilerini görüntülemek için.
