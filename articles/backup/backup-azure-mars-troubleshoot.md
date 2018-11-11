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
ms.openlocfilehash: 9b31b250499bc49135a606bde8b26f6ff45cde31
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51278521"
---
# <a name="troubleshoot-microsoft-azure-recovery-services-mars-agent"></a>Microsoft Azure kurtarma Hizmetleri (MARS) aracısı sorunlarını giderme

Yapılandırma, kaydı, yedekleme sırasında görebileceği hataların nasıl çözüleceği işte ve geri yükleme.

## <a name="invalid-vault-credentials-provided"></a>Sağlanan kasa kimlik bilgileri geçersiz
| Hata Ayrıntıları | Olası nedenler | Önerilen eylemler |
| ---     | ---     | ---    |
| **Hata** </br> *Sağlanan kasa kimlik bilgileri geçersiz. Dosya bozuk veya mu değil sahip en son kimlik bilgilerini kurtarma hizmeti ile ilişkili. (KİMLİK: 34513)* | <ul><li> Kasa kimlik bilgileri geçersiz (diğer bir deyişle, bunlar 48 saatten fazla kayıt süreden önce yüklenen).<li>MARS aracısını indirip yükleyin. Windows Temp Dizini silemiyor. <li>Kasa kimlik bilgilerini bir ağ konumunda ' dir. <li>TLS 1.0 devre dışı bırakıldı.<li> Yapılandırılmış bir proxy sunucusundan bağlantıyı engelliyor. <br> |  <ul><li>Yeni kasa kimlik bilgilerini indirin.<li>Git **Internet Seçenekleri** > **güvenlik** > **Internet**. Ardından, **Özel düzey**ve bölüm karşıdan dosya görene kadar kaydırın. Ardından **etkinleştirme**.<li>Siteye eklemek gerekebilir [Güvenilen siteler](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Bir proxy sunucusu kullanmak için ayarları değiştirin. Ardından proxy sunucusu ayrıntıları sağlayın. <li> Tarih ve saat, makine ile aynı.<li>C:/Windows/Temp gidin ve .tmp uzantısına sahip birden fazla 60.000 veya 65.000 dosyaları olup olmadığını denetleyin. Varsa, bu dosyaları silin. Bu sunucuda SDP paket çalıştırarak doğrulayabilirsiniz. Dosya indirmeleri izin verilmeyen bildiren bir hata alırsanız, çok sayıda dosya C:/Windows/Temp dizininde olması olasıdır.<li>.NET Framework 4.6.2 yüklü olduğundan emin olun. <li>PCI uyumluluk nedeniyle TLS 1.0 devre dışı bıraktığınız bu değilse [sorun giderme sayfası](https://support.microsoft.com/help/4022913). <li>Sunucuda yüklü virüsten koruma yazılımınız varsa, aşağıdaki dosyaların virüsten koruma tarama dışında tut: <ul><li>CBengine.exe<li>.NET Framework ile ilgili CSC.exe. Sunucuda yüklü her .NET sürümü için bir CSC.exe yoktur. .NET Framework etkilenen sunucudaki tüm sürümleri bağlıdır CSC.exe dosyaları hariç tutun. <li>Karalama klasörünü veya önbellek konumu. <br>C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch karalama klasörünü veya önbellek konumu yolu için varsayılan konumdur.

## <a name="the-mars-agent-was-unable-to-connect-to-azure-backup"></a>MARS Aracısı Azure Backup'a bağlanamadı.

| Hata Ayrıntıları | Olası nedenler | Önerilen eylemler |
| ---     | ---     | ---    |
| **Hata** </br><ul><li>*Microsoft Azure Recovery hizmet Aracısı, Microsoft Azure Backup'a bağlanamadı. (KİMLİK: 100050) Ağ ayarlarınızı denetleyin ve internet'e bağlanabilir olduğundan emin olun*<li>*(407) Ara sunucu kimlik doğrulaması gerekli* |Proxy sunucusu bağlantıyı engelliyor. |  <ul><li>Git **Internet Seçenekleri** > **güvenlik** > **Internet**. Ardından **Özel düzey**ve bölüm karşıdan dosya görene kadar kaydırın. **Etkinleştir**’i seçin.<li>Siteye eklemek gerekebilir [Güvenilen siteler](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Bir proxy sunucusu kullanmak için ayarları değiştirin. Ardından proxy sunucusu ayrıntıları sağlayın. <li>Sunucuda yüklü virüsten koruma yazılımınız varsa, aşağıdaki dosyaların virüsten koruma tarama dışında tut. <ul><li>CBEngine.exe (yerine dpmra.exe).<li>CSC.exe (.NET Framework ile ilgili). Sunucuda yüklü her .NET sürümü için bir CSC.exe yoktur. .NET Framework etkilenen sunucudaki tüm sürümleri bağlıdır CSC.exe dosyaları hariç tutun. <li>Karalama klasörünü veya önbellek konumu. <br>C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch karalama klasörünü veya önbellek konumu yolu için varsayılan konumdur.

## <a name="failed-to-set-the-encryption-key-for-secure-backups"></a>Güvenli yedekleme işlemleri için şifreleme anahtarı ayarlanamadı

| Hata Ayrıntıları | Olası nedenler | Önerilen eylemler |
| ---     | ---     | ---    |      
| **Hata** </br>*Güvenli yedekleme işlemleri için şifreleme anahtarı ayarlanamadı. Şifreleme parolası aşağıdaki dosyasına kaydedildi ancak etkinleştirme başarısız oldu tamamen*. |<li>Sunucu zaten başka bir kasa ile kayıtlı.<li>Yapılandırma sırasında parola bozuktu.| Kasadan sunucunun kaydını silin ve yeni bir parola ile tekrar kaydedin.

## <a name="the-activation-did-not-complete-successfully"></a>Etkinleştirme başarıyla tamamlanmadı

| Hata Ayrıntıları | Olası nedenler | Önerilen eylemler |
|---------|---------|---------|
|**Hata** </br><ol>*Etkinleştirme başarıyla tamamlanmadı. Geçerli işlem bir [0x1FC07] iç hizmet hatası nedeniyle başarısız oldu. Bir süre sonra işlemi yeniden deneyin. Sorun devam ederse lütfen Microsoft desteğine başvurun*     | <li> Karalama klasörünü yeterli alana sahip bir birimde bulunur. <li> Karalama klasörünü yanlış başka bir konuma taşındı. <li> OnlineBackup.KEK dosyası eksik.         | <li>Yükseltme [en son sürümü](https://aka.ms/azurebackup_agent) MARS Agent'ın.<li>Karalama klasörünü veya önbellek konumunu yedekleme verilerinin toplam boyutunun 5-%10 boş disk alanı ile bir birime taşıyın. Önbellek konumunu doğru şekilde taşımak için adımlarda bakın [Azure Backup Aracısı hakkında sorular](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> OnlineBackup.KEK dosyanın mevcut olduğundan emin olun. <br>*Karalama klasörünü veya önbellek konumu yolu için varsayılan konum C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch olan*.        |

## <a name="encryption-passphrase-not-correctly-configured"></a>Şifreleme parolası düzgün yapılandırılmış

| Hata Ayrıntıları | Olası nedenler | Önerilen eylemler |
|---------|---------|---------|
|**Hata** </br><ol>*34506 hata oluştu. Bu bilgisayarda depolanan şifreleme parolası düzgün yapılandırılmamış*.    | <li> Karalama klasörünü yeterli alana sahip bir birimde bulunur. <li> Karalama klasörünü yanlış başka bir konuma taşındı. <li> OnlineBackup.KEK dosyası eksik.        | <li>Yükseltme [en son sürümü](https://aka.ms/azurebackup_agent) MARS Agent'ın.<li>Karalama klasörünü veya önbellek konumunu boş alan 5-%10 eşdeğer yedekleme verilerinin toplam boyutu olan bir birime taşıyın. Önbellek konumunu doğru şekilde taşımak için adımlarda bakın [Azure Backup Aracısı hakkında sorular](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> OnlineBackup.KEK dosyanın mevcut olduğundan emin olun. <br>*Karalama klasörünü veya önbellek konumu yolu için varsayılan konum C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch olan*.         |

## <a name="backups-dont-run-according-to-the-schedule"></a>Yedeklemeler zamanlamaya göre çalıştırma
El ile yedeklemeler sorunsuz çalışırken zamanlanmış yedeklemeleri otomatik olarak tetiklenir yoksa, aşağıdaki işlemleri deneyin: 
 
- PowerShell 3.0 veya üzeri sunucu üzerinde yüklü olup olmadığını. PowerShell sürümünü denetlemek için aşağıdaki komutu çalıştırın ve doğrulayın *ana* sürüm numarası 3'ten büyük ya da eşit.

  `$PSVersionTable.PSVersion`

- Aşağıdaki yolun bir parçası olup olmadığını *PSMODULEPATH* ortam değişkeni.

  `<MARS agent installation path>\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup`

- PowerShell yürütme ilkesini devredışı *LocalMachine* olduğu kadar kısıtlı ayarlayın, yedekleme görevi tetiklenir PowerShell cmdlet'i başarısız olabilir. Aşağıdaki komutları denetleyin ve ya da yürütme ilkesini ayarlamak için yükseltilmiş modda çalıştırın *Kısıtlanmamış* veya *RemoteSigned*.

  `PS C:\WINDOWS\system32> Get-ExecutionPolicy -List`

  `PS C:\WINDOWS\system32> Set-ExecutionPolicy Unrestricted`

- Git **Denetim Masası** > **Yönetimsel Araçlar** > **Görev Zamanlayıcı**. Genişletin **Microsoft**seçip **çevrimiçi yedekleme**. Çift **Microsoft OnlineBackup**ve Git **Tetikleyicileri** sekmesi. Durum ayarlandığından emin olun **etkin**. Aksi takdirde seçin **Düzenle**seçip **etkin** onay kutusu. Üzerinde **genel** sekmesine gidin **güvenlik seçenekleri**. Görevi çalıştırmak için seçili kullanıcı hesabı ya da olduğundan emin olun **sistem** veya **yerel Yöneticiler grubuna** sunucusunda.

> [!TIP]
> Değişiklikleri tutarlı bir şekilde uygulandığından emin olmak için yukarıdaki adımları gerçekleştirdikten sonra sunucuyu yeniden başlatın.


## <a name="troubleshoot-restore-issues"></a>Geri yükleme sorunlarını giderme

Azure yedekleme başarılı bir şekilde bağlama kurtarma birimi değil, birkaç dakika sonra bile olabilir. Ayrıca, işlem sırasında hata iletileri alabilirsiniz. Normal olarak kurtarmaya başlayabilmeleri için şu adımları izleyin: 

1.  Birkaç dakikadan uzun süredir çalışıyor durumda devam eden bağlama işlemini iptal edin.

2.  Yedekleme aracının en son sürümüne bağımlı olup olmadığınızı bakın. Sürümü, bulunacak **eylemleri** bölmesinde seçin MARS konsolunun **hakkında Microsoft Azure kurtarma Hizmetleri Aracısı**. Onaylayın **sürüm** sayıdır belirtilen sürümden daha yüksek veya ona eşit [bu makalede](https://go.microsoft.com/fwlink/?linkid=229525). En son sürümü karşıdan yükleyebileceğiniz [burada](https://go.microsoft.com/fwLink/?LinkID=288905).

3.  Git **cihaz Yöneticisi** > **depolama denetleyicileri**bulun **Microsoft iSCSI başlatıcısı**. Bulabiliyorsa, doğrudan 7. adıma gidin. 

4.  Microsoft iSCSI başlatıcısı hizmetini bulamazsanız, altında bir giriş bulunacak deneyin **cihaz Yöneticisi** > **depolama denetleyicileri** adlı **bilinmeyen cihaz**, Donanım kimliği ile **ROOT\ISCSIPRT**.

5.  Sağ **bilinmeyen cihaz**seçip **güncelleştirme yazılımı**.

6.  Sürücü seçeneği seçerek güncelleştirme **otomatik olarak güncelleştirilen sürücü yazılım Ara**. Güncelleştirmenin tamamlanması değiştirilmelidir **bilinmeyen cihaz** için **Microsoft iSCSI başlatıcısı**, aşağıda gösterildiği gibi. 

    ![Ekran görüntüsü, Azure yedekleme cihaz Yöneticisi, vurgulanan depolama denetleyicileri](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  Git **Görev Yöneticisi'ni** > **hizmetler (yerel)** > **Microsoft iSCSI başlatıcısı hizmetini**. 

    ![Ekran Azure yedekleme Görev Yöneticisi'nin, ile vurgulanmış hizmetler (yerel)](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)
    
8.  Microsoft iSCSI başlatıcısı hizmetini yeniden başlatın. Bunu yapmak için hizmette select sağ **Durdur**tekrar sağ tıklayın ve seçin **Başlat**.

9.  Kurtarmayı kullanarak yeniden deneyin. **anında geri yükleme**. 

Kurtarma yine başarısız olursa, sunucu veya istemci yeniden başlatın. Yeniden başlatılmasını istemediğiniz veya kurtarma, hala bile sunucu yeniden başlatıldıktan sonra başarısız olursa başka bir makineden kurtarma deneyin. Bağlantısındaki [bu makalede](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine).

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun
Hala yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi almak [Azure Backup aracısının yüklediği Windows Server Yedekleme](tutorial-backup-windows-server-to-azure.md).
* Bir yedeklemeyi geri yüklemeniz gerekirse [dosyaları bir Windows makinesine geri yüklemek](backup-azure-restore-windows-server.md) için bu makaleyi kullanın.
