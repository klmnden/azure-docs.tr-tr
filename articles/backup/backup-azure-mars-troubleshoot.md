---
title: Azure Backup Aracısı sorunlarını giderme
description: Yükleme ve Azure Backup Aracısı'nın kayıt sorunlarını giderme
services: backup
author: saurabhsensharma
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: saurse
ms.openlocfilehash: 1c4c2ed6265bdb3c29986fb0b90c3d85d32aadca
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67434003"
---
# <a name="troubleshoot-the-microsoft-azure-recovery-services-mars-agent"></a>Microsoft Azure kurtarma Hizmetleri (MARS) aracısı sorunlarını giderme

Bu makalede yapılandırmasını, kaydı, yedekleme sırasında görebileceği hataların nasıl çözümleneceği açıklanır ve geri yükleme.

## <a name="basic-troubleshooting"></a>Temel sorun giderme

Sorun giderme Microsoft Azure kurtarma Hizmetleri (MARS) aracısı başlamadan önce aşağıdakileri kontrol etmenizi öneririz:

- [MARS aracısının güncel olduğundan emin olun](https://go.microsoft.com/fwlink/?linkid=229525&clcid=0x409).
- [MARS aracısı ve Azure arasında ağ bağlantısı olduğundan emin olun](https://aka.ms/AB-A4dp50).
- MARS (hizmet konsolunda) çalıştığından emin olun. Gerekirse, yeniden başlatın ve işlemi yeniden deneyin.
- [%5 ile % 10 ücretsiz birim alanı karalama klasörünü konumda olduğundan olun](https://aka.ms/AB-AA4dwtt).
- [Onay başka bir işlem veya virüsten koruma yazılımı Azure Backup ile engellemesini](https://aka.ms/AB-AA4dwtk).
- Yedekleme başarısız, el ile yedekleme çalışır ancak zamanlanmış değilse [yedekleme zamanlamaya göre çalıştırma yok](https://aka.ms/ScheduledBackupFailManualWorks).
- En son güncelleştirmeleri, işletim sistemi olduğundan emin olun.
- [Desteklenmeyen sürücüleri ve desteklenmeyen öznitelikleri olan dosyaları yedeklemeden hariç tutulur olun](backup-support-matrix-mars-agent.md#supported-drives-or-volumes-for-backup).
- Korumalı sistem saati doğru saat dilimi için yapılandırıldığından emin olun.
- [.NET Framework 4.5.2 emin olun veya üzeri sunucu üzerinde yüklü](https://www.microsoft.com/download/details.aspx?id=30653).
- Sunucunuz bir kasaya kaydetmeden çalışıyorsanız:
  - Sunucuda aracı kaldırılır ve bu portaldan silinir emin olun.
  - Başlangıçta sunucuyu kaydetmek için kullanılan parolayı kullanın.
- Çevrimdışı yedeklemeler için yedekleme işlemine başlamadan önce Azure PowerShell 3.7.0 hem kaynak hem de kopya bilgisayar yüklendiğinden emin olun.
- Backup aracısını bir Azure sanal makinesinde çalışıyorsa görürsünüz [bu makalede](https://aka.ms/AB-AA4dwtr).

## <a name="invalid-vault-credentials-provided"></a>Sağlanan kasa kimlik bilgileri geçersiz

**Hata iletisi**: Sağlanan kasa kimlik bilgileri geçersiz. Dosya bozuk veya mu değil sahip en son kimlik bilgilerini kurtarma hizmeti ile ilişkili. (Kimlik: 34513)

| Nedeni | Önerilen Eylemler |
| ---     | ---    |
| **Kasa kimlik bilgileri geçerli değil** <br/> <br/> Kasa kimlik bilgileri dosyalarını bozuk olabilir veya süresi dolmuş olabilir. (Örneğin, bunlar 48 saatten fazla kayıt süreden önce yüklenen.)| Yeni kimlik bilgilerini Azure portalında kurtarma Hizmetleri kasasından indirin. (Bkz. adım 6 [MARS Aracısı'nı indirme](https://docs.microsoft.com/azure/backup/backup-configure-vault#download-the-mars-agent) bölümüne.) Ardından uygun olarak aşağıdaki adımları uygulayın: <ul><li> Önceden yüklü ve kayıtlı MARS, Microsoft Azure Backup Aracısı MMC konsolunu açın ve ardından **kaydını sunucusunun** içinde **eylemleri** yeni kaydı tamamlamak için bölmesi kimlik bilgileri. <br/> <li> Yeni yükleme başarısız olursa, yeni kimlik bilgileriyle yeniden yüklemeyi deneyin.</ul> **Not**: Yalnızca en son dosya birden çok kasa kimlik bilgileri dosyalarını yüklenmişse, sonraki 48 saat için geçerlidir. Yeni bir kasa kimlik bilgileri dosyası indirmenizi öneririz.
| **Kayıt proxy sunucusu/güvenlik duvarı engelliyor** <br/>or <br/>**İnternet bağlantısı yok** <br/><br/> İnternet bağlantısı sınırlı bir makine ya da proxy sunucunuzun içerir ve erişim için gerekli olan URL'lerin garanti etmez, kayıt başarısız olur.| Aşağıdaki adımları gerçekleştirin:<br/> <ul><li> Sistemin internet bağlantısı olduğundan emin olmak için BT Ekibi ile çalışın.<li> Bir proxy sunucusu yoksa, aracı kaydedilirken proxy seçeneği seçili değilse emin olun. [Proxy ayarlarınızı kontrol edin](#verifying-proxy-settings-for-windows).<li> Bir güvenlik duvarı/proxy sunucunuz varsa, bu URL'ler emin olmak için ağ ekibinizle çalışmanızı ve IP adreslerinin erişimi vardır:<br/> <br> **URL'leri**<br> www.msftncsi.com <br> .Microsoft.com <br> .WindowsAzure.com <br> .microsoftonline.com <br> .windows.net <br>**IP adresleri**<br>  20.190.128.0/18 <br>  40.126.0.0/18 <br/></ul></ul>Önceki sorun giderme adımlarını tamamladıktan sonra tekrar kaydetmeyi deneyin.
| **Virüsten koruma yazılımı kaydı engelliyor** | Sunucuda yüklü virüsten koruma yazılımınız varsa, bu dosyaları ve klasörleri virüsten koruma taraması için gerekli dışlama kuralı ekleyin: <br/><ui> <li> CBengine.exe <li> CSC.exe<li> Karalama klasörünü. Varsayılan konumu C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch ' dir. <li> Bin klasörü C:\Program Files\Microsoft Azure Recovery Services Agent\Bin konumunda.

### <a name="additional-recommendations"></a>Ek öneriler
- C:/Windows/Temp gidin ve .tmp uzantısına sahip birden fazla 60.000 veya 65.000 dosyaları olup olmadığını denetleyin. Varsa, bu dosyaları silin.
- Yerel saat dilimi, makinenin tarih ve saat eşleşme emin olun.
- Olun [bu siteleri](backup-configure-vault.md#verify-internet-access) Internet Explorer'da Güvenilen siteler eklenir.

### <a name="verifying-proxy-settings-for-windows"></a>Windows için proxy ayarları doğrulanıyor

1. PsExec'nden indirin [Sysinternals](https://docs.microsoft.com/sysinternals/downloads/psexec) sayfası.
1. Çalıştırma `psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"` yükseltilmiş bir komut isteminden.

   Bu komut, Internet Explorer açılır.
1. Git **Araçları** > **Internet Seçenekleri** > **bağlantıları** > **LAN Ayarları**.
1. Sistem hesabı için proxy ayarlarını kontrol edin.
1. Yapılandırılmış hiçbir proxy ve proxy ayrıntıları sağlanır, ayrıntıları kaldırın.
1. Yapılandırılmış bir proxy ve proxy ayrıntıları yanlış, olun **Proxy IP** ve **bağlantı noktası** doğru ayrıntı.
1. Internet Explorer'ı kapatın.

## <a name="unable-to-download-vault-credential-file"></a>Kasa kimlik bilgileri dosyası indirilemedi

| Hata   | Önerilen Eylemler |
| ---     | ---    |
|Kasa kimlik bilgileri dosyası indirilemedi. (Kimlik: 403) | <ul><li> Farklı bir tarayıcı kullanarak kasa kimlik bilgilerini indirerek deneyin veya bu adımları uygulayın: <ul><li> Start Internet Explorer. F12'ı seçin. </li><li> Git **ağ** sekmesini ve önbelleği ve tanımlama bilgilerini temizleyin. </li> <li> Sayfayı yenileyin.<br></li></ul> <li> Abonelik devre dışı bırakılmış/süresi dolmuş olup olmadığını denetleyin.<br></li> <li> Herhangi bir güvenlik duvarı kural yüklemeyi engelleyip engellemediğini denetleyin. <br></li> <li> Kasa (kasa başına 50 makine) sınırı bitti henüz emin olun.<br></li>  <li> Kullanıcının kasa kimlik bilgileri indirin ve bir sunucuyu kasaya kaydetmek için gerekli olan Azure yedekleme izinleri olduğundan emin olun. Bkz: [Use Role-Based erişim denetimi, Azure Backup kurtarma noktaları yönetmek için](backup-rbac-rs-vault.md).</li></ul> |

## <a name="the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>Microsoft Azure Kurtarma Hizmeti Aracısı, Microsoft Azure Backup'a bağlanamadı

| Hata  | Olası nedeni | Önerilen Eylemler |
| ---     | ---     | ---    |
| <br /><ul><li>Microsoft Azure Recovery hizmet Aracısı, Microsoft Azure Backup'a bağlanamadı. (Kimlik: 100050) ağ ayarlarınızı denetleyin ve internet'e bağlanabilir olduğundan emin olun.<li>(407) Ara sunucu kimlik doğrulaması gerekli. |Bir ara sunucu bağlantıyı engelliyor. |  <ul><li>Internet Explorer'da Git **Araçları** > **Internet Seçenekleri** > **güvenlik** > **Internet**. Seçin **Özel düzey** ekranı aşağı kaydırarak **dosya indirme** bölümü. Seçin **etkinleştirme**.<p>Eklemeniz gerekebilir [URL'leri ve IP adresleri](backup-configure-vault.md#verify-internet-access) Internet Explorer'da Güvenilen siteler.<li>Bir proxy sunucusu kullanmak için ayarları değiştirin. Ardından proxy sunucusu ayrıntıları sağlayın.<li> Makinenizde sınırlı internet erişimi, güvenlik duvarı ayarlarını makinede veya proxy bu izin verdiğinden emin olun [URL'leri ve IP adresleri](backup-configure-vault.md#verify-internet-access). <li>Sunucuda yüklü virüsten koruma yazılımınız varsa, bu dosyaların virüsten koruma tarama dışında tut: <ul><li>CBEngine.exe (yerine dpmra.exe).<li>CSC.exe (.NET Framework ile ilgili). Sunucuda yüklü her .NET Framework sürümü için bir CSC.exe yoktur. Etkilenen sunucudaki tüm .NET Framework sürümleri için CSC.exe dosyaları hariç tutun. <li>Karalama klasörünü veya önbellek konumu. <br>C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch karalama klasörünü veya önbellek yolu için varsayılan konumdur.<li>Bin klasörü C:\Program Files\Microsoft Azure Recovery Services Agent\Bin konumunda.


## <a name="failed-to-set-the-encryption-key-for-secure-backups"></a>Güvenli yedekleme işlemleri için şifreleme anahtarı ayarlanamadı

| Hata | Olası nedenler | Önerilen Eylemler |
| ---     | ---     | ---    |
| <br />Güvenli yedekleme işlemleri için şifreleme anahtarı ayarlanamadı. Etkinleştirme tamamen başarılı olmadı, ancak şifreleme parolası aşağıdaki dosyasına kaydedildi. |<li>Sunucu zaten başka bir kasa ile kayıtlı.<li>Yapılandırma sırasında parola bozuktu.| Kasadan sunucunun kaydını silin ve yeni bir parola ile tekrar kaydedin.

## <a name="the-activation-did-not-complete-successfully"></a>Etkinleştirme başarıyla tamamlanmadı

| Hata  | Olası nedenler | Önerilen Eylemler |
|---------|---------|---------|
|<br />Etkinleştirme başarıyla tamamlanmadı. Geçerli işlem bir [0x1FC07] iç hizmet hatası nedeniyle başarısız oldu. İşlemi bir süre sonra yeniden deneyin. Sorun devam ederse lütfen Microsoft desteğine başvurun.     | <li> Karalama klasörünü yeterli alana sahip bir birimde bulunur. <li> Karalama klasörünü yanlış taşındı. <li> OnlineBackup.KEK dosyası eksik.         | <li>Yükseltme [en son sürümü](https://aka.ms/azurebackup_agent) MARS agent'ın.<li>Karalama klasörünü veya önbellek konumunu %5 ile % yedekleme verilerinin toplam boyutu 10 arasında boş alana sahip bir birime taşıyın. Önbellek konumunu doğru şekilde taşımak için adımlarda bakın [dosyaları ve klasörleri yedekleme hakkında sık sorulan sorular](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> OnlineBackup.KEK dosyanın mevcut olduğundan emin olun. <br>*Karalama klasörünü veya önbellek yolu için varsayılan konum C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch olan*.        |

## <a name="encryption-passphrase-not-correctly-configured"></a>Şifreleme parolası düzgün yapılandırılmış

| Hata  | Olası nedenler | Önerilen Eylemler |
|---------|---------|---------|
| <br />34506 hata oluştu. Bu bilgisayarda depolanan şifreleme parolası düzgün yapılandırılmamış.    | <li> Karalama klasörünü yeterli alana sahip bir birimde bulunur. <li> Karalama klasörünü yanlış taşındı. <li> OnlineBackup.KEK dosyası eksik.        | <li>Yükseltme [en son sürümü](https://aka.ms/azurebackup_agent) MARS Agent'ın.<li>Karalama klasörünü veya önbellek konumunu %5 ile % yedekleme verilerinin toplam boyutu 10 arasında boş alana sahip bir birime taşıyın. Önbellek konumunu doğru şekilde taşımak için adımlarda bakın [dosyaları ve klasörleri yedekleme hakkında sık sorulan sorular](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> OnlineBackup.KEK dosyanın mevcut olduğundan emin olun. <br>*Karalama klasörünü veya önbellek yolu için varsayılan konum C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch olan*.         |


## <a name="backups-dont-run-according-to-schedule"></a>Yedeklemeleri zamanlamaya göre çalıştırma
Zamanlanmış yedeklemeleri otomatik olarak tetiklenen yoktur ancak el ile yedeklemeler düzgün çalışması aşağıdaki işlemleri deneyin:

- Windows Server Yedekleme zamanlaması ile Azure dosyaları ve klasörleri yedekleme zamanlaması çakışmadığından emin olun.

- Çevrimiçi Yedekleme durumu ayarlandığından emin olun **etkinleştirme**. Durumu doğrulamak için şu adımları uygulayın:

  1. Görev Zamanlayıcısı'nda genişletin **Microsoft** seçip **çevrimiçi yedekleme**.
  1. Çift **Microsoft OnlineBackup** gidin **Tetikleyicileri** sekmesi.
  1. Durum ayarlanırsa denetleyin **etkin**. Aksi takdirde seçin **Düzenle**seçin **etkin**ve ardından **Tamam**.

- Görevi çalıştırmak için seçili kullanıcı hesabı ya da olduğundan emin olun **sistem** veya **yerel Yöneticiler grubuna** sunucusunda. Kullanıcı hesabı doğrulamak için Git **genel** denetleyin ve sekme **güvenlik** seçenekleri.

- PowerShell 3.0 veya üstü sunucuda yüklü emin olun. PowerShell sürümünü denetlemek için şu komutu çalıştırın ve doğrulayın `Major` 3 veya sonraki sürüm numarası:

  `$PSVersionTable.PSVersion`

- Bu yolun bir parçası olduğundan emin olmak `PSMODULEPATH` ortam değişkeni:

  `<MARS agent installation path>\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup`

- PowerShell yürütme ilkesini devredışı `LocalMachine` olduğu kadar kısıtlı ayarlayın, yedekleme görevi tetiklenir PowerShell cmdlet'i başarısız olabilir. Bu komutları denetleyin ve ya da yürütme ilkesini ayarlamak için yükseltilmiş modda çalıştırın `Unrestricted` veya `RemoteSigned`:

  `PS C:\WINDOWS\system32> Get-ExecutionPolicy -List`

  `PS C:\WINDOWS\system32> Set-ExecutionPolicy Unrestricted`

- Eksik veya bozuk PowerShell modülü MSOnlineBackup dosya yoksa emin olun. Tüm eksik veya bozuk dosyalar varsa, aşağıdaki adımları gerçekleştirin:

  1. Düzgün şekilde çalıştığını MARS aracısı olan herhangi bir makineden, C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules MSOnlineBackup klasörüne kopyalayın.
  1. Sorunlu makinede kopyalanan dosyalar aynı konumda klasör (C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules) yapıştırın.

     Zaten varsa MSOnlineBackup klasör makinede, dosyaları yapıştırın veya var olan dosyalar değiştirin.


> [!TIP]
> Değişiklikleri tutarlı bir şekilde uygulandığından emin olmak için önceki adımları gerçekleştirdikten sonra sunucuyu yeniden başlatın.


## <a name="troubleshoot-restore-problems"></a>Geri yükleme sorunlarını giderme

Azure Yedekleme başarıyla kurtarma birimi, birkaç dakika sonra bile bağlamak değil. Ve işlem sırasında hata iletileri alabilirsiniz. Normal olarak kurtarmaya başlayabilmeleri için şu adımları uygulayın:

1.  Birkaç dakikadır çalışıyorsa bağlama işlemini iptal edin.

2.  Yedekleme aracının en son sürümünü olup olmadığını denetleyin. Sürümü denetlemek için **eylemleri** bölmesinde seçin MARS konsolunun **hakkında Microsoft Azure kurtarma Hizmetleri Aracısı**. Onaylayın **sürüm** sayıdır belirtilen sürümden daha yüksek veya ona eşit [bu makalede](https://go.microsoft.com/fwlink/?linkid=229525). Bu bağlantıyı seçin [son sürümünü indirin](https://go.microsoft.com/fwLink/?LinkID=288905).

3.  Git **cihaz Yöneticisi** > **depolama denetleyicileri** bulun **Microsoft iSCSI başlatıcısı**. Bulursanız, doğrudan 7. adıma gidin.

4.  Microsoft iSCSI başlatıcısı hizmetinin bulamazsanız, altında bir giriş bulunacak deneyin **cihaz Yöneticisi** > **depolama denetleyicileri** adlı **bilinmeyen cihaz** Donanım kimliği ile **ROOT\ISCSIPRT**.

5.  Sağ **bilinmeyen cihaz** seçip **güncelleştirme yazılımı**.

6.  Sürücü seçeneği seçerek güncelleştirme **otomatik olarak güncelleştirilen sürücü yazılım Ara**. Bu güncelleştirme değiştirmelisiniz **bilinmeyen cihaz** için **Microsoft iSCSI başlatıcısı**:

    ![Ekran görüntüsü, Azure yedekleme cihaz Yöneticisi, vurgulanan depolama denetleyicileri](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  Git **Görev Yöneticisi'ni** > **hizmetler (yerel)**  > **Microsoft iSCSI başlatıcısı hizmetini**:

    ![Ekran Azure yedekleme Görev Yöneticisi'nin, ile vurgulanmış hizmetler (yerel)](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)

8.  Microsoft iSCSI başlatıcısı hizmetini yeniden başlatın. Bunu yapmak için hizmete sağ tıklayın ve seçin **Durdur**. Sonra tekrar sağ tıklayıp **Başlat**.

9.  Kurtarmayı kullanarak yeniden deneyin. [anında geri yükleme](backup-instant-restore-capability.md).

Kurtarma yine başarısız olursa, sunucu veya istemci yeniden başlatın. Yeniden başlatın ya da devre dışı bile bu, sunucu yeniden başlatıldıktan sonra kurtarma yine başarısız olursa denemek istemiyorsanız [başka bir makineden kurtarma](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine).

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun
Hala yardıma ihtiyacınız varsa [Destek ekibiyle iletişime geçin](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi alınamıyor.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi almak [nasıl Windows Server'ı Azure Backup Aracısı ile Yedekleme](tutorial-backup-windows-server-to-azure.md).
* Bir geri yüklemeniz gerekiyorsa bkz [bir Windows makinesine dosyaları geri](backup-azure-restore-windows-server.md).
