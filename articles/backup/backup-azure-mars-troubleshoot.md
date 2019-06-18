---
title: Azure Backup Aracısı sorunlarını giderme
description: Yükleme ve Azure yedekleme Aracısı'nın kayıt sorunlarını giderme
services: backup
author: saurabhsensharma
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: saurse
ms.openlocfilehash: 2c2ed46ed6e4a5d6663387777d3425d18b50500e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67060211"
---
# <a name="troubleshoot-microsoft-azure-recovery-services-mars-agent"></a>Microsoft Azure kurtarma Hizmetleri (MARS) aracısı sorunlarını giderme

Yapılandırma, kaydı, yedekleme sırasında görebileceği hataların nasıl çözüleceği işte ve geri yükleme.

## <a name="basic-troubleshooting"></a>Temel sorun giderme

Gerçekleştirmenizi öneririz doğrulama başlamadan önce Microsoft Azure kurtarma Hizmetleri (MARS) aracısı sorunlarını giderme:

- [Microsoft Azure kurtarma Hizmetleri (MARS) Aracısı'nın güncel olduğundan emin olun.](https://go.microsoft.com/fwlink/?linkid=229525&clcid=0x409)
- [MARS aracısı ile Azure arasında ağ bağlantısı sağlandığından emin olun](https://aka.ms/AB-A4dp50)
- Microsoft Azure Kurtarma Hizmetleri'nin çalıştığından emin olun (Hizmet konsolunda). Gerekirse yeniden başlatın ve işlemi yeniden deneyin
- [Boş klasör konumunda %5-10 oranında kullanılabilir alan olduğundan emin olun](https://aka.ms/AB-AA4dwtt)
- [Azure Backup ile çakışan başka bir işlem veya virüsten koruma yazılımı olup olmadığını kontrol edin](https://aka.ms/AB-AA4dwtk)
- [Zamanlanmış yedekleme başarısız oluyor ancak el ile yedekleme çalışıyor](https://aka.ms/ScheduledBackupFailManualWorks)
- İşletim sisteminizde en son güncelleştirmelerin yüklü olduğundan emin olun
- [Desteklenmeyen sürücüleri ve desteklenmeyen öznitelikleri olan dosyaları yedeklemeden hariç tutulur emin olun.](backup-support-matrix-mars-agent.md#supported-drives-or-volumes-for-backup)
- Korumalı sistemdeki **Sistem Saatinin** doğru saat dilimine göre yapılandırıldığından emin olun <br>
- [Sunucuda en az .Net Framework 4.5.2 ve üzeri bir sürümün yüklü olduğundan emin olun](https://www.microsoft.com/download/details.aspx?id=30653)<br>
- **Sunucunuzu bir kasaya yeniden kaydetmeye** çalışıyorsanız: <br>
  - Aracının sunucudan kaldırıldığından ve portaldan silindiğinden emin olun <br>
  - Sunucu kaydedilirken kullanılan parolayı kullanın <br>
- Çevrimdışı Yedekleme durumunda çevrimdışı yedekleme işlemi başlamadan önce Azure PowerShell sürüm 3.7.0 hem kaynak hem de kopya bilgisayarda yüklü olduğundan emin olun.
- [Backup aracısını bir Azure sanal makinesi üzerinde çalışırken önemli noktalar](https://aka.ms/AB-AA4dwtr)

## <a name="invalid-vault-credentials-provided"></a>Sağlanan kasa kimlik bilgileri geçersiz

**Hata iletisi**: Sağlanan kasa kimlik bilgileri geçersiz. Dosya bozuk veya mu değil sahip en son kimlik bilgilerini kurtarma hizmeti ile ilişkili. (Kimlik: 34513)

| Nedeni | Önerilen Eylem |
| ---     | ---    |
| **Kasa kimlik bilgileri geçersiz** <br/> <br/> Kasa kimlik bilgileri dosyalarını (yani 48 saatten fazla kayıt süreden önce yüklenen) süresi dolmuş olabilir veya bozuk olabilir| Yeni Azure portalında kurtarma Hizmetleri kasasından indirin (bkz *6. adım* altında [ **MARS Aracısı'nı indirme** ](https://docs.microsoft.com/azure/backup/backup-configure-vault#download-the-mars-agent) bölümü) ve aşağıda: <ul><li> Zaten yüklü ve kayıtlı Microsoft Azure Yedekleme aracısı, Microsoft Azure Backup Aracısı MMC konsolunu açın ve seçin **kaydını sunucusunun** Eylem bölmesinden yeni indirilen kaydı tamamlamak için kimlik bilgileri <br/> <li> Yeni yükleme başarısız olursa sonra yeni kimlik bilgilerini kullanarak yeniden yüklemeyi deneyin</ul> **Not**: Yalnızca en son indirilen dosyanın birden çok kasa kimlik bilgileri dosyalarını daha önce indirdiyseniz, 48 saat içinde geçerlidir. Bu nedenle, yeni yeni kasa kimlik bilgileri dosyası indirmek için önerilir.
| **Proxy sunucusu/güvenlik duvarı engelleme <br/>veya <br/>Hayır Internet bağlantısı** <br/><br/> Makine ya da Proxy sunucusu sınırlı Internet erişimi daha sonra gerekli URL'ler listeleme olmadan kayıt başarısız olur.| Bu sorunu çözmek için gerçekleştirmek aşağıda:<br/> <ul><li> Sistemin Internet bağlantısı olduğundan emin olmak için BT Ekibi ile çalışın<li> Ardından aracı kaydı sırasında bir proxy seçeneği seçili olduğundan emin olun, Proxy sunucusu yoksa, listelenen proxy ayarları adımları doğrulayın [burada](#verifying-proxy-settings-for-windows)<li> Bir güvenlik duvarı/proxy sunucunuz varsa, ardından URL'leri ve IP adresi sağlamak için ağ ekibinizle iş erişimi<br/> <br> **URL'leri**<br> - *www.msftncsi.com* <br>-  *. Microsoft.com* <br> -  *.WindowsAzure.com* <br>-  *.microsoftonline.com* <br>-  *. windows.net* <br>**IP adresi**<br> - *20.190.128.0/18* <br> - *40.126.0.0/18* <br/></ul></ul>Yukarıdaki sorun giderme adımlarını tamamladıktan sonra yeniden kaydolmayı deneyin
| **Virüsten koruma yazılımının engelliyor** | Sunucuda yüklü virüsten koruma yazılımınız varsa, aşağıdaki dosyaları için gerekli hariç tutma kuralları virüsten koruma yazılımı taramasından ekleyin: <br/><ui> <li> *CBengine.exe* <li> *CSC.exe*<li> Karalama klasörünü, varsayılan konumdur *C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch* <li> Bin klasöründeki *C:\Program Files\Microsoft Azure Recovery Services Agent\Bin*

### <a name="additional-recommendations"></a>Ek öneriler
- Git *C:/Windows/Temp* ve .tmp uzantısına sahip birden fazla 60.000 veya 65.000 dosyaları olup olmadığını denetleyin. Varsa, bu dosyaları sil
- Makinenin tarihi ve saati yerel saat dilimiyle eşleşen emin olun.
- Olun [aşağıdaki](backup-configure-vault.md#verify-internet-access) siteleri güvenilen siteler IE eklendi

### <a name="verifying-proxy-settings-for-windows"></a>Windows için proxy ayarları doğrulanıyor

- İndirme **psexec** gelen [burada](https://docs.microsoft.com/sysinternals/downloads/psexec)
- Bu çalıştırma `psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"` komut isteminde yükseltilmiş:
- Bu başlatacak *Internet Explorer* penceresi
- Git *Araçları* -> *Internet Seçenekleri* -> *bağlantıları* -> *LAN Ayarları*
- Proxy ayarlarını doğrulayın *sistem* hesabı
- Yapılandırılmış hiçbir proxy ve proxy ayrıntıları sağlanır, ardından ayrıntılarını kaldırın
-   Proxy yapılandırılır ve proxy ayrıntıları yanlış, ardından olun *Proxy IP* ve *bağlantı noktası* doğru ayrıntıları
- Kapat *Internet Explorer*

## <a name="unable-to-download-vault-credential-file"></a>Kasa kimlik bilgileri dosyası indirilemedi

| Hata Ayrıntıları | Önerilen Eylemler |
| ---     | ---    |
|Kasa kimlik bilgileri dosyası indirilemedi. (Kimlik: 403) | <ul><li> Farklı bir tarayıcı kullanarak kasa kimlik bilgilerini indirerek deneyin ya da aşağıdaki adımları: <ul><li> IE, F12 tuşuna başlatın. </li><li> Git **ağ** IE önbelleği ve tanımlama bilgilerini temizleme için sekmesinde </li> <li> Sayfayı yenileyin<br>(VEYA)</li></ul> <li> Abonelik devre dışı bırakılmış/süresi dolmuş olup olmadığını denetleyin<br>(VEYA)</li> <li> Herhangi bir güvenlik duvarı kural kasa kimlik bilgileri dosyası indirme engelleyip engellemediğini denetleyin <br>(VEYA)</li> <li> Kasa (kasa başına 50 makine) sınırı tüketmiş değil emin olun.<br>(VEYA)</li>  <li> Kullanıcı, Azure Backup izni kasa kimlik bilgileri indirin ve sunucuyu kasaya kaydetmek için bkz: gerekli olduğundan emin olun [makale](backup-rbac-rs-vault.md)</li></ul> |

## <a name="the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>Microsoft Azure Kurtarma Hizmeti Aracısı, Microsoft Azure Backup'a bağlanamadı

| Hata Ayrıntıları | Olası nedenler | Önerilen Eylemler |
| ---     | ---     | ---    |
| **Hata:** <br /><ol><li>*Microsoft Azure Recovery hizmet Aracısı, Microsoft Azure Backup'a bağlanamadı. (Kimlik: 100050) ağ ayarlarınızı denetleyin ve internet'e bağlanabilir olduğundan emin olun*<li>*(407) Proxy Kimlik Doğrulaması Gerekli* |Proxy bağlantıyı engelliyor. |  <ul><li>Başlatma **IE** > **ayarı** > **Internet Seçenekleri** > **güvenlik**  >  **Internet**. Ardından **Özel düzey** ve bölüm karşıdan dosya görene kadar kaydırın. Seçin **etkinleştirme**.<li>Bu siteleri IE'de eklemeniz gerekebilir [Güvenilen siteler](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins).<li>Bir proxy sunucusu kullanmak için ayarları değiştirin. Ardından proxy sunucusu ayrıntıları sağlayın.<li> Makinenizde sınırlı internet erişimi, güvenlik duvarı ayarlarını makinede veya proxy bu izin verdiğinden emin olun [URL'leri](backup-configure-vault.md#verify-internet-access) ve [IP adresi](backup-configure-vault.md#verify-internet-access). <li>Aşağıdaki dosyaları, sunucu üzerinde yüklü virüsten koruma yazılımınız varsa, virüsten koruma tarama dışında tut. <ul><li>CBEngine.exe (yerine dpmra.exe).<li>CSC.exe (.NET Framework ile ilgili). Sunucuda yüklü her .NET sürümü için bir CSC.exe yoktur. .NET framework etkilenen sunucudaki tüm sürümleri bağlıdır CSC.exe dosyaları hariç tutun. <li>Karalama klasörünü veya önbellek konumu. <br>*Karalama klasörünü veya önbellek konumu yolu için varsayılan konum C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch olan*.<li>Bin klasörü C:\Program Files\Microsoft Azure Recovery Services Agent\Bin



## <a name="failed-to-set-the-encryption-key-for-secure-backups"></a>Güvenli yedekleme işlemleri için şifreleme anahtarı ayarlanamadı

| Hata Ayrıntıları | Olası nedenler | Önerilen Eylemler |
| ---     | ---     | ---    |
| **Hata:** <br />*Güvenli yedekleme işlemleri için şifreleme anahtarı ayarlanamadı. Şifreleme parolası aşağıdaki dosyasına kaydedildi ancak etkinleştirme başarısız oldu tamamen*. |<li>Sunucu zaten başka bir kasa ile kayıtlı.<li>Yapılandırma sırasında parola bozuktu.| Kasadan sunucunun kaydını silin ve yeni bir parola ile tekrar kaydedin.

## <a name="the-activation-did-not-complete-successfully"></a>Etkinleştirme başarıyla tamamlanmadı

| Hata Ayrıntıları | Olası nedenler | Önerilen Eylemler |
|---------|---------|---------|
|**Hata:** <br />*Etkinleştirme başarıyla tamamlanmadı. Geçerli işlem bir [0x1FC07] iç hizmet hatası nedeniyle başarısız oldu. İşlemi bir süre sonra yeniden deneyin. Sorun devam ederse lütfen Microsoft desteğine başvurun*     | <li> Karalama klasörünü yeterli alana sahip bir birimde bulunur. <li> Karalama klasörünü yanlış başka bir konuma taşındı. <li> OnlineBackup.KEK dosyası eksik.         | <li>Yükseltme [en son sürümü](https://aka.ms/azurebackup_agent) MARS Agent'ın.<li>Karalama klasörünü veya önbellek konumunu yedekleme verilerinin toplam boyutunun 5-%10 boş disk alanı ile bir birime taşıyın. Önbellek konumunu doğru şekilde taşımak için adımlarda bakın [Azure Backup Aracısı hakkında sorular](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> OnlineBackup.KEK dosyanın mevcut olduğundan emin olun. <br>*Karalama klasörünü veya önbellek konumu yolu için varsayılan konum C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch olan*.        |

## <a name="encryption-passphrase-not-correctly-configured"></a>Şifreleme parolası düzgün yapılandırılmış

| Hata Ayrıntıları | Olası nedenler | Önerilen Eylemler |
|---------|---------|---------|
|**Hata:** <br />*34506 hata oluştu. Bu bilgisayarda depolanan şifreleme parolası düzgün yapılandırılmamış*.    | <li> Karalama klasörünü yeterli alana sahip bir birimde bulunur. <li> Karalama klasörünü yanlış başka bir konuma taşındı. <li> OnlineBackup.KEK dosyası eksik.        | <li>Yükseltme [en son sürümü](https://aka.ms/azurebackup_agent) MARS Agent'ın.<li>Karalama klasörünü veya önbellek konumunu boş alan 5-%10 eşdeğer yedekleme verilerinin toplam boyutu olan bir birime taşıyın. Önbellek konumunu doğru şekilde taşımak için adımlarda bakın [Azure Backup Aracısı hakkında sorular](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> OnlineBackup.KEK dosyanın mevcut olduğundan emin olun. <br>*Karalama klasörünü veya önbellek konumu yolu için varsayılan konum C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch olan*.         |


## <a name="backups-dont-run-according-to-the-schedule"></a>Yedeklemeler zamanlamaya göre çalıştırma
El ile yedeklemeler sorunsuz çalışırken zamanlanmış yedeklemeleri otomatik olarak tetiklenir yoksa, aşağıdaki işlemleri deneyin:

- Windows Server Yedekleme zamanlaması Azure dosyaları ve klasörleri yedekleme zamanlaması ile çakışmadığından emin olun.

- Çevrimiçi Yedekleme durumu ayarlandığından emin olun **etkinleştirme**. Durum gerçekleştirmek doğrulamak için aşağıdaki:

  - Açık **Görev Zamanlayıcı** genişletin **Microsoft**seçip **çevrimiçi yedekleme**.
  - Çift **Microsoft OnlineBackup**ve Git **Tetikleyicileri** sekmesi.
  - Durum ayarlanırsa doğrulayın **etkin**. Aksi takdirde, ardından **Düzenle** > **etkin** onay kutusunu ve tıklatın **Tamam**.

- Görevi çalıştırmak için seçili kullanıcı hesabı ya da olduğundan emin olun **sistem** veya **yerel Yöneticiler grubuna** sunucusunda. Kullanıcı hesabı doğrulamak için Git **genel** denetleyin ve sekme **güvenlik** seçenekleri.

- PowerShell 3.0 veya üstü sunucuda yüklü emin olun. PowerShell sürümünü denetlemek için aşağıdaki komutu çalıştırın ve doğrulayın *ana* sürüm numarası 3'ten büyük ya da eşit.

  `$PSVersionTable.PSVersion`

- Aşağıdaki yolun bir parçası olduğundan emin olun *PSMODULEPATH* ortam değişkeni

  `<MARS agent installation path>\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup`

- PowerShell yürütme ilkesini devredışı *LocalMachine* olduğu kadar kısıtlı ayarlayın, yedekleme görevi tetiklenir PowerShell cmdlet'i başarısız olabilir. Aşağıdaki komutları denetleyin ve ya da yürütme ilkesini ayarlamak için yükseltilmiş modda çalıştırın *Kısıtlanmamış* veya *RemoteSigned*

  `PS C:\WINDOWS\system32> Get-ExecutionPolicy -List`

  `PS C:\WINDOWS\system32> Set-ExecutionPolicy Unrestricted`

- Eksik olmadığından emin olun veya bozuk **PowerShell** Modülü **MSonlineBackup**. Çözümlenecek durumunda tüm eksik veya bozuk dosyaları, bu sorunu gerçekleştirmek aşağıda:

  - Herhangi bir makineden MSOnlineBackup klasörden kopyalayın düzgün, MARS Aracısı sahip *(C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules)* yolu.
  - Bu yolla sorunlu makine yapıştırın *(C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules)* .
  - Varsa **MSOnlineBackup** klasörü makinede zaten var, yapıştırın veya içindeki içerik dosyalarını değiştirin.


> [!TIP]
> Değişiklikleri tutarlı bir şekilde uygulandığından emin olmak için yukarıdaki adımları gerçekleştirdikten sonra sunucuyu yeniden başlatın.


## <a name="troubleshoot-restore-issues"></a>Geri yükleme sorunlarını giderme

Azure Yedekleme başarıyla kurtarma birimi, birkaç dakika sonra bile bağlamak değil. Ayrıca, işlem sırasında hata iletileri alabilirsiniz. Normal olarak kurtarmaya başlayabilmeleri için şu adımları izleyin:

1.  Birkaç dakikadan uzun süredir çalışıyor durumda devam eden bağlama işlemini iptal edin.

2.  Yedekleme aracının en son sürümüne bağımlı olup olmadığınızı bakın. Sürümü, bulunacak **eylemleri** bölmesinde seçin MARS konsolunun **hakkında Microsoft Azure kurtarma Hizmetleri Aracısı**. Onaylayın **sürüm** sayıdır belirtilen sürümden daha yüksek veya ona eşit [bu makalede](https://go.microsoft.com/fwlink/?linkid=229525). En son sürümü [buradan](https://go.microsoft.com/fwLink/?LinkID=288905) indirebilirsiniz.

3.  Git **cihaz Yöneticisi** > **depolama denetleyicileri**bulun **Microsoft iSCSI başlatıcısı**. Bulabiliyorsa, doğrudan 7. adıma gidin.

4.  Microsoft iSCSI başlatıcısı hizmetini bulamazsanız, altında bir giriş bulunacak deneyin **cihaz Yöneticisi** > **depolama denetleyicileri** adlı **bilinmeyen cihaz**, Donanım kimliği ile **ROOT\ISCSIPRT**.

5.  Sağ **bilinmeyen cihaz**seçip **güncelleştirme yazılımı**.

6.  Sürücü seçeneği seçerek güncelleştirme **otomatik olarak güncelleştirilen sürücü yazılım Ara**. Güncelleştirmenin tamamlanması değiştirilmelidir **bilinmeyen cihaz** için **Microsoft iSCSI başlatıcısı**, aşağıda gösterildiği gibi.

    ![Ekran görüntüsü, Azure yedekleme cihaz Yöneticisi, vurgulanan depolama denetleyicileri](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  Git **Görev Yöneticisi'ni** > **hizmetler (yerel)**  > **Microsoft iSCSI başlatıcısı hizmetini**.

    ![Ekran Azure yedekleme Görev Yöneticisi'nin, ile vurgulanmış hizmetler (yerel)](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)

8.  Microsoft iSCSI başlatıcısı hizmetini yeniden başlatın. Bunu yapmak için hizmette select sağ **Durdur**tekrar sağ tıklayın ve seçin **Başlat**.

9.  Kurtarmayı kullanarak yeniden deneyin. [ **anında geri yükleme**](backup-instant-restore-capability.md).

Kurtarma yine başarısız olursa, sunucu veya istemci yeniden başlatın. Yeniden başlatılmasını istemediğiniz veya kurtarma, hala bile sunucu yeniden başlatıldıktan sonra başarısız olursa başka bir makineden kurtarma deneyin. Bağlantısındaki [bu makalede](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine).

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun
Hala yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi almak [Azure Backup aracısının yüklediği Windows Server Yedekleme](tutorial-backup-windows-server-to-azure.md).
* Bir yedeklemeyi geri yüklemeniz gerekirse [dosyaları bir Windows makinesine geri yüklemek](backup-azure-restore-windows-server.md) için bu makaleyi kullanın.
