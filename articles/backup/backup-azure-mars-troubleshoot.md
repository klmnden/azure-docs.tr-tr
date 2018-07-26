---
title: Azure Backup Aracısı sorunlarını giderme
description: Yükleme ve Azure yedekleme Aracısı'nın kayıt sorunlarını giderme
services: backup
author: saurabhsensharma
manager: shreeshd
ms.service: backup
ms.topic: conceptual
ms.date: 7/25/2018
ms.author: saurse
ms.openlocfilehash: e7a63167285c06fdfe632e7d45d9fddd3cca7842
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39248531"
---
# <a name="troubleshoot-microsoft-azure-recovery-services-mars-agent-issues"></a>Microsoft Azure kurtarma Hizmetleri (MARS) aracısı sorunlarını giderme
## <a name="recommended-steps"></a>Önerilen adımlar
Yapılandırma, kaydı, yedekleme sırasında hataları gidermek için bu makaleye bakın ve MARS Aracısı'nı kullanarak geri yükleme.

## <a name="invalid-vault-credentials-provided-the-file-is-either-corrupted-or-does-not-have-the-latest-credentials-associated-with-recovery-service"></a>Sağlanan kasa kimlik bilgileri geçersiz. Dosya bozuk veya mu değil sahip en son kimlik bilgilerini kurtarma hizmeti ile ilişkili.
| Hata ayrıntıları | Olası nedenler | Önerilen eylemler |
| ---     | ---     | ---    |
| **Hata** </br> *Sağlanan kasa kimlik bilgileri geçersiz. Dosya bozuk veya mu değil sahip en son kimlik bilgilerini kurtarma hizmeti ile ilişkili. (KİMLİK: 34513)* | <ul><li> Kasa kimlik bilgileri geçersiz (diğer bir deyişle, bunlar 48 saatten fazla kayıt süreden önce yüklenen).<li>MARS aracısını indirip yükleyin. Windows Temp Dizini silemiyor. <li>Kasa kimlik bilgilerini bir ağ konumunda ' dir. <li>TLS 1.0 devre dışı bırakıldı<li> Yapılandırılmış bir proxy sunucusundan bağlantıyı engelliyor. <br> |  <ul><li>Yeni kasa kimlik bilgilerini indirin.<li>Git **Internet Seçenekleri** > **güvenlik** > **Internet**. Ardından, **Özel düzey**ve bölüm karşıdan dosya görene kadar kaydırın. Ardından **etkinleştirme**.<li>Siteye eklemek gerekebilir [Güvenilen siteler](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Bir proxy sunucusu kullanmak için ayarları değiştirin. Ardından proxy sunucusu ayrıntıları sağlayın. <li> Tarih ve saat, makine ile aynı.<li>C:/Windows/Temp gidin ve .tmp uzantısına sahip birden fazla 60.000 veya 65.000 dosyaları olup olmadığını denetleyin. Varsa, bu dosyaları silin.<li>Bu SDP paket sunucu üzerinde çalıştığını doğrulayabilirsiniz. Dosya indirmeleri izin verilmeyen bildiren bir hata alırsanız, çok sayıda dosya C:/Windows/Temp dizininde olması olasıdır.<li>.NET framework 4.6.2 yüklü olduğundan emin olun. <li>PCI uyumluluk nedeniyle TLS 1.0 devre dışı bıraktığınız, şuna başvurun [sorun giderme sayfası](https://support.microsoft.com/help/4022913). <li>Aşağıdaki dosyaları, sunucu üzerinde yüklü virüsten koruma yazılımınız varsa, virüsten koruma tarama dışında tut: <ul><li>CBengine.exe<li>.NET Framework ile ilgili CSC.exe. Sunucuda yüklü her .NET sürümü için bir CSC.exe yoktur. .NET framework etkilenen sunucudaki tüm sürümleri bağlıdır CSC.exe dosyaları hariç tutun. <li>Karalama klasörünü veya önbellek konumu. <br>*Karalama klasörünü veya önbellek konumu yolu için varsayılan konum C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch olan*.

## <a name="the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>Microsoft Azure Recovery hizmet Aracısı Microsoft Azure Backup'a bağlanamadı.

| Hata ayrıntıları | Olası nedenler | Önerilen eylemler |
| ---     | ---     | ---    |
| **Hata** </br><ol><li>*Microsoft Azure Recovery hizmet Aracısı, Microsoft Azure Backup'a bağlanamadı. (KİMLİK: 100050) Ağ ayarlarınızı denetleyin ve internet'e bağlanabilir olduğundan emin olun*<li>*(407) Ara sunucu kimlik doğrulaması gerekli* |Proxy bağlantıyı engelliyor. |  <ul><li>Na **Internet Seçenekleri** > **güvenlik** > **Internet**. Ardından **Özel düzey** ve bölüm karşıdan dosya görene kadar kaydırın. **Etkinleştir**’i seçin.<li>Siteye eklemek gerekebilir [Güvenilen siteler](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Bir proxy sunucusu kullanmak için ayarları değiştirin. Ardından proxy sunucusu ayrıntıları sağlayın. <li>Aşağıdaki dosyaları, sunucu üzerinde yüklü virüsten koruma yazılımınız varsa, virüsten koruma tarama dışında tut. <ul><li>CBEngine.exe (yerine dpmra.exe).<li>CSC.exe (.NET Framework ile ilgili). Sunucuda yüklü her .NET sürümü için bir CSC.exe yoktur. .NET framework etkilenen sunucudaki tüm sürümleri bağlıdır CSC.exe dosyaları hariç tutun. <li>Karalama klasörünü veya önbellek konumu. <br>*Karalama klasörünü veya önbellek konumu yolu için varsayılan konum C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch olan*.

## <a name="failed-to-set-the-encryption-key-for-secure-backups"></a>Güvenli yedekleme işlemleri için şifreleme anahtarı ayarlanamadı

| Hata ayrıntıları | Olası nedenler | Önerilen eylemler |
| ---     | ---     | ---    |      
| **Hata** </br>*Şifreleme parolası aşağıdaki dosyasına kaydedildi ancak güvenli yedekleme etkinleştirme başarısız oldu tamamen şifreleme anahtarı ayarlanamadı*. |<li>Sunucu zaten başka bir kasa ile kayıtlı.<li>Yapılandırma sırasında parola bozuk| Kasadan sunucunun kaydını silin ve yeni bir parola ile tekrar kaydedin.

## <a name="the-activation-did-not-complete-successfully-the-current-operation-failed-due-to-an-internal-service-error-0x1fc07"></a>Etkinleştirme başarıyla tamamlanmadı. Geçerli işlem bir [0x1FC07] iç hizmet hatası nedeniyle başarısız oldu

| Hata ayrıntıları | Olası nedenler | Önerilen eylemler |
|---------|---------|---------|
|**Hata** </br><ol>*Etkinleştirme başarıyla tamamlanmadı. Geçerli işlem bir [0x1FC07] iç hizmet hatası nedeniyle başarısız oldu. Bir süre sonra işlemi yeniden deneyin. Sorun devam ederse lütfen Microsoft desteğine başvurun*     | <li> Karalama klasörünü yeterli alana sahip bir birimde bulunur. <li> Karalama klasörünü yanlış başka bir konuma taşındı. <li> OnlineBackup.KEK dosyası eksik.         | <li>Yükseltme [en son sürümü](http://aka.ms/azurebackup_agent) MARS Agent'ın.<li>Karalama klasörünü veya önbellek konumunu yedekleme verilerinin toplam boyutunun 5-%10 boş disk alanı ile bir birime taşıyın. Önbellek konumunu doğru şekilde taşımak için adımlarda bakın [Azure Backup Aracısı hakkında sorular](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> OnlineBackup.KEK dosyanın mevcut olduğundan emin olun. <br>*Karalama klasörünü veya önbellek konumu yolu için varsayılan konum C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch olan*.        |

## <a name="error-34506-the-encryption-passphrase-stored-on-this-computer-is-not-correctly-configured"></a>34506 hata oluştu. Bu bilgisayarda depolanan şifreleme parolası düzgün yapılandırılmamış

| Hata ayrıntıları | Olası nedenler | Önerilen eylemler |
|---------|---------|---------|
|**Hata** </br><ol>*34506 hata oluştu. Bu bilgisayarda depolanan şifreleme parolası düzgün yapılandırılmamış*.    | <li> Karalama klasörünü yeterli alana sahip bir birimde bulunur. <li> Karalama klasörünü yanlış başka bir konuma taşındı. <li> OnlineBackup.KEK dosyası eksik.        | <li>Yükseltme [en son sürümü](http://aka.ms/azurebackup_agent) MARS Agent'ın.<li>Karalama klasörünü veya önbellek konumunu boş alan 5-%10 eşdeğer yedekleme verilerinin toplam boyutu olan bir birime taşıyın. Önbellek konumunu doğru şekilde taşımak için adımlarda bakın [Azure Backup Aracısı hakkında sorular](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> OnlineBackup.KEK dosyanın mevcut olduğundan emin olun. <br>*Karalama klasörünü veya önbellek konumu yolu için varsayılan konum C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch olan*.         |

## <a name="backups-do-not-run-as-per-schedule"></a>Yedeklemeleri zamanlamaya göre çalıştırma
El ile yedeklemeler sorunsuz çalışırken zamanlanmış yedeklemeleri otomatik olarak değil tetiklendiğinde aşağıdaki adımları gerçekleştirin. 
 
<li>Olun, PowerShell 3.0 veya üzeri sunucusuna yüklenir. PowerShell sürümünü denetlemek için aşağıdaki komutu çalıştırın ve doğrulayın *ana* sürüm numarası 3'ten büyük ya da eşit.

`$PSVersionTable.PSVersion`
<li>Aşağıdaki yolun bir parçası olduğundan emin olun *PSMODULEPATH* ortam değişkeni.

`<MARS agent installation path>\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup`
<li>Powershell yürütme ilkesini devredışı *LocalMachine* olduğu kadar kısıtlı ayarlayın, yedekleme görevi tetiklenir powershell cmdlet'i başarısız olabilir. Yükseltilmiş modda denetleyin ve ya da yürütme ilkesini ayarlamak için aşağıdaki komutları yürütün *Kısıtlanmamış* veya *RemoteSigned*

`PS C:\WINDOWS\system32> Get-ExecutionPolicy -List`

`PS C:\WINDOWS\system32> Set-ExecutionPolicy Unrestricted`
<li>Denetim Masası'na gidin, Yönetimsel Araçlar -> Görev Zamanlayıcısı -> -> Microsoft genişletin select çevrimiçi yedekleme ->
<li>'Microsoft OnlineBackup' görevi çift tıklatın ve 'Tetikleyiciler' sekmesine gidin.
<li>'Status' görevinin 'Etkin' olarak ayarlandığından emin olun. Aksi halde 'Düzenle' ' yi tıklatın ve 'Enabled' onay kutusunu işaretleyin
<li>Gidin *güvenlik seçenekleri* bölümünü *genel* sekmesi
<li>Görevi çalıştırmak için seçili kullanıcı hesabı ya da olduğundan emin olun *sistem* veya sunucuda yerel Yöneticiler grubunun > [!TIP] değişiklikleri emin olmak için yukarıdaki adımları gerçekleştirdikten sonra sunucunun yeniden başlatılması önerilir yapılan tutarlı bir şekilde uygulanır


## <a name="troubleshooting-restore-issues"></a>Geri yükleme sorunlarını giderme

### <a name="failure-to-mount-the-recovery-volume-during-recovery-of-files-and-folders"></a>Dosya ve klasörleri kurtarma sırasında kurtarma birimi bağlama hatası
Azure Backup kurtarma birimi başarıyla ya da birkaç dakika sonra bağlamaz varsa **bağlama** veya başarısız hatalarla bir veya daha fazla kurtarma birimi bağlamak için normal olarak kurtarmaya başlayabilmeleri için aşağıdaki adımları izleyin.

1.  Birkaç dakikadan uzun süredir çalışıyor durumda devam eden bağlama işlemini iptal edin.

2.  Azure yedekleme aracısının en son sürümüne bağımlı olduğundan emin olun. Azure Backup Aracısı'nın sürüm bilgileri bulmak için tıklayın **hakkında Microsoft Azure kurtarma Hizmetleri Aracısı** üzerinde **eylemleri** Microsoft Azure Backup'ın bölmesinde eminolunvekonsol**Sürüm** sayıdır belirtilen sürümden daha yüksek veya ona eşit [bu makalede](https://go.microsoft.com/fwlink/?linkid=229525). En son sürümü karşıdan yükleyebileceğiniz [burada](https://go.microsoft.com/fwLink/?LinkID=288905)

3.  Git **cihaz Yöneticisi** -> **depolama denetleyicileri** ve sizi bulabilmesini sağlama **Microsoft iSCSI başlatıcısı**. Bulabiliyorsa, doğrudan adım 7'ye gidin. 

4.  Bir girişin altında bulup bulamayacağınızı öğrenmek için Microsoft iSCSI başlatıcısı hizmetini 3. adımda bahsedilen bulamazsanız denetleyin **cihaz Yöneticisi** -> **depolama denetleyicileri** adlı  **Bilinmeyen cihaz** donanım Kimliğine sahip **ROOT\ISCSIPRT**.

5.  Sağ tıklayın **bilinmeyen cihaz** seçip **güncelleştirme yazılımı**.

6.  Sürücü seçeneği seçerek güncelleştirme **otomatik olarak güncelleştirilen sürücü yazılım Ara**. Güncelleştirmenin tamamlanması değiştirilmelidir **bilinmeyen cihaz** için **Microsoft iSCSI başlatıcısı** aşağıda gösterildiği gibi. 

    ![Şifreleme](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  Git **Görev Yöneticisi'ni** -> **hizmetler (yerel)** -> **Microsoft iSCSI başlatıcısı hizmetini**. 

    ![Şifreleme](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)
    
8.  Hizmette sağ tıklayarak, Durdur ve daha fazla tekrar sağ tıklayıp tıklayarak Microsoft iSCSI başlatıcısı hizmetini yeniden **Başlat**.

9.  Anında geri yükleme kullanarak kurtarma yeniden deneyin. 

Sunucu/istemci kurtarma yine başarısız olursa yeniden başlatın. Yeniden başlatma arzu değil veya kurtarma, hala bile sunucu yeniden başlatıldıktan sonra başarısız olursa başka bir makineden Kurtarma aşağıdaki adımları deneyin listelenen [bu makalede](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun
Hala yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi almak [Azure Backup aracısının yüklediği Windows Server Yedekleme](tutorial-backup-windows-server-to-azure.md).
* Bir yedeklemeyi geri yüklemeniz gerekirse [dosyaları bir Windows makinesine geri yüklemek](backup-azure-restore-windows-server.md) için bu makaleyi kullanın.
