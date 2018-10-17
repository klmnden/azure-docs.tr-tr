---
title: Windows Azure dosyaları sorunlarını giderme | Microsoft Docs
description: Windows Azure dosyaları sorunlarını giderme
services: storage
author: jeffpatt24
tags: storage
ms.service: storage
ms.topic: article
ms.date: 05/11/2018
ms.author: jeffpatt
ms.component: files
ms.openlocfilehash: a0a330d3ea7362ffabb20a5d390cee87cbf7d8ff
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49365414"
---
# <a name="troubleshoot-azure-files-problems-in-windows"></a>Windows Azure dosyaları sorunlarını giderme

Bu makalede Windows istemcilerinden bağlandığınızda, Microsoft Azure dosyaları'na ilgili genel sorunları listeler. Ayrıca olası nedenleri ve çözümlemeleri için bu sorunları sağlar. Bu makalede sorun giderme adımlarını ek olarak da kullanabilirsiniz [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) Windows istemci ortam önkoşulları doğru olduğundan emin olmak için. En iyi performansı elde etmek için ortamınızı ayarlama yardımcı olur ve bu makalede değinilen belirtileri çoğunu algılanması AzFileDiagnostics otomatikleştirir. Bu bilgiler de bulabilirsiniz [Azure dosyaları paylaşımlarını sorun giderici](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) bağlama/eşleme/bağlama Azure dosyaları paylaşımlarını sorunlara yardımcı olmak için adımları sağlar.


<a id="error53-67-87"></a>
## <a name="error-53-error-67-or-error-87-when-you-mount-or-unmount-an-azure-file-share"></a>Hata 53, hata 67 veya hata bağlama ya da bir Azure dosya paylaşımını ayırma 87

Şirket içinde veya farklı bir veri merkezinde bir dosya paylaşımını bağlayabilmeniz çalıştığınızda aşağıdaki hataları alabilirsiniz:

- 53 sistem hatası oluştu. Ağ yolu bulunamadı.
- 67 sistem hatası oluştu. Ağ adı bulunamıyor.
- 87 sistem hatası oluştu. Parametresi geçersiz.

### <a name="cause-1-unencrypted-communication-channel"></a>1. neden: Şifrelenmemiş iletişim kanalı

Güvenlik nedenleriyle, Azure dosya paylaşımlarını bağlantı iletişim kanalını şifreli değildir ve Azure dosya paylaşımlarını bulunduğu aynı veri merkezlerinden bağlantı girişimi yapılmadan değil engellenir. Aynı veri merkezindeki şifrelenmemiş bağlantıları da ise engellenir [güvenli aktarım gerekli](https://docs.microsoft.com/en-us/azure/storage/common/storage-require-secure-transfer) depolama hesabı seçeneği etkinleştirilmiştir. Kullanıcının istemci işletim sistemi SMB şifrelemesi destekliyorsa iletişim kanalı şifreleme sağlanır.

Windows 8, Windows Server 2012 ve sonraki sürümleri her sistem şifrelemeyi destekleyen SMB 3.0 içeren istekleri anlaşır.

### <a name="solution-for-cause-1"></a>Çözüm nedeni 1 için

1. Doğrulama [güvenli aktarım gerekli](https://docs.microsoft.com/en-us/azure/storage/common/storage-require-secure-transfer) ayarı depolama hesabı devre dışı.
2. Aşağıdakilerden birini gerçekleştiren bir istemciden bağlanma:

    - Windows 8 ve Windows Server 2012 veya sonraki sürümler gereksinimlerini karşılar
    - Azure dosya paylaşımı için kullanılan Azure depolama hesabıyla aynı veri merkezinde bir sanal makineden bağlanır

### <a name="cause-2-port-445-is-blocked"></a>2. neden: Bağlantı noktası 445 engellendi

Bağlantı noktası 445 giden iletişimi, Azure dosyaları bir veri merkezine engellenirse, sistem hatası 53 veya sistem hatası 67 ortaya çıkabilir. İzin vermek veya vermemek bağlantı noktası 445 erişimden ISS'ler özetini görmek için Git [TechNet](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx).

Bu nedenle "Sistem hata 53" iletisi arkasında olup olmadığını anlamak için TCP:445 uç nokta sorgulamak için Portqry kullanabilirsiniz. TCP:445 uç noktası filtrelenmiş şekilde görüntülenirse TCP bağlantı noktası engellenir. Örnek bir sorgu aşağıda verilmiştir:

  `g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

445 numaralı TCP bağlantı noktası, ağ yolunda bir kural tarafından engellenirse aşağıdaki çıktıyı görürsünüz:

  `TCP port 445 (microsoft-ds service): FILTERED`

Portqry kullanımı hakkında daha fazla bilgi için bkz. [Portqry.exe komut satırı yardımcı programının açıklaması](https://support.microsoft.com/help/310099).

### <a name="solution-for-cause-2"></a>Neden 2 çözümü

Bağlantı noktası 445'in giden açmak için BT Departmanınızla birlikte çalışma [Azure IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="cause-3-ntlmv1-is-enabled"></a>3. neden: NTLMv1 etkin

NTLMv1 iletişim istemcide etkinse, sistem hatası 53 veya sistem hatası 87 ortaya çıkabilir. Azure dosyaları yalnızca NTLMv2 kimlik doğrulamasını destekler. NTLMv1 etkin olması daha az güvenli bir istemci oluşturur. Bu nedenle, iletişim, Azure dosyaları için engellenir. 

Bu hatanın nedenini olup olmadığını belirlemek için aşağıdaki kayıt defteri alt anahtarı 3 değerine ayarlandığını doğrulayın:

**Imcompatibilitylevel > LmCompatibilityLevel**

Daha fazla bilgi için [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) TechNet'teki konu.

### <a name="solution-for-cause-3"></a>Çözüm nedeni 3

Geri döndürme **LmCompatibilityLevel** değeri 3 Aşağıdaki kayıt defteri alt anahtarında varsayılan değeri:

  **HKLM\SYSTEM\CurrentControlSet\Control\Lsa**

<a id="error1816"></a>
## <a name="error-1816-not-enough-quota-is-available-to-process-this-command-when-you-copy-to-an-azure-file-share"></a>Hata 1816 "yeterli kotası ise bu komutu işlemek için" bir Azure dosya paylaşımına kopyaladığınızda

### <a name="cause"></a>Nedeni

Dosya paylaşımının nerede bağlı bilgisayar için bir dosya izin verilen eşzamanlı açık tanıtıcıları üst sınırına ulaşmadığınız hata 1816 olur.

### <a name="solution"></a>Çözüm

Bazı işler kapatarak eşzamanlı açık tanıtıcı sayısını azaltın ve yeniden deneyin. Daha fazla bilgi için [Microsoft Azure depolama performansı ve ölçeklenebilirlik denetim listesi](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-files-in-windows"></a>Dosya ve Windows Azure dosyalarından kopyalamak yavaş

Dosyaları Azure dosya Hizmeti'ne aktarmaya çalıştığınızda, yavaş performansla görebilirsiniz.

- Belirli bir en düşük g/ç boyutu gereksinimi yoksa, en iyi performans için 1 MiB g/ç boyutu kullanmanızı öneririz.
-   Ardından yazma ile uzatıyoruz. bir dosyanın son boyutu bildiğiniz ve dosya çubuğunda yazılı kuyruğunu sıfır içerdiğinde yazılımınızı uyumluluk sorunlarına sahip değil, her bir genişletme yazma yapmak yerine önceden dosya boyutunu ayarlayın.
-   Doğru kopyalama yöntemi kullanın:
    -   Kullanım [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) iki dosya paylaşımları arasındaki tüm aktarımları için.
    -   Kullanım [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) bir şirket içi bilgisayarda dosya paylaşımları arasında.

### <a name="considerations-for-windows-81-or-windows-server-2012-r2"></a>Windows 8.1 veya Windows Server 2012 R2 için dikkat edilmesi gerekenler

Windows 8.1 veya Windows Server 2012 R2 çalıştıran istemciler olması için emin [KB3114025](https://support.microsoft.com/help/3114025) düzeltme yüklenir. Bu düzeltme, oluşturma performansını artırır ve tanıtıcıları kapatın.

Düzeltmenin yüklenip yüklenmediğini denetlemek için aşağıdaki betiği çalıştırabilirsiniz:

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Düzeltme yüklü değilse, aşağıdaki çıktıyı görüntülenir:

`HKEY_Local_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies {96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1`

> [!Note]
> Windows Server 2012 R2 görüntüleri Azure Market'te düzeltme aralık 2015'ten başlayarak varsayılan olarak yüklenen KB3114025 vardır.

<a id="shareismissing"></a>
## <a name="no-folder-with-a-drive-letter-in-my-computer"></a>Bir sürücü harfiyle klasör **Bilgisayarım**

Net kullanım kullanarak yönetici olarak bir Azure dosya paylaşımı eşlerseniz, paylaşım eksik gibi görünüyor.

### <a name="cause"></a>Nedeni

Varsayılan olarak, Windows dosya Gezgini'nde, bir yönetici olarak çalıştırmaz. Bir yönetici komut isteminden net kullanım çalıştırırsanız, yönetici olarak bir ağ sürücüsüne eşleyin. Eşlenen sürücüleri kullanıcı merkezli olduğundan, farklı bir kullanıcı hesabı altında bağlarsanız açan kullanıcı hesabı sürücüleri görüntülemez.

### <a name="solution"></a>Çözüm
Bir yönetici olmayan komut satırından paylaşımını. Alternatif olarak, izleyebilirsiniz [bu başlıklı TechNet konusuna](https://technet.microsoft.com/library/ee844140.aspx) yapılandırmak için **EnableLinkedConnections** kayıt defteri değeri.

<a id="netuse"></a>
## <a name="net-use-command-fails-if-the-storage-account-contains-a-forward-slash"></a>Depolama hesabı eğik çizgi varsa net use komutu başarısız oluyor

### <a name="cause"></a>Nedeni

Net use komutunu eğik çizgi (/) bir komut satırı seçeneği olarak yorumlar. Sürücü eşleme, kullanıcı hesabınızın adını bir eğik çizgiyle başlıyorsa başarısız olur.

### <a name="solution"></a>Çözüm

Sorunu çözmek için aşağıdaki adımlardan birini kullanabilirsiniz:

- Aşağıdaki PowerShell komutunu çalıştırın:

  `New-SmbMapping -LocalPath y: -RemotePath \\server\share -UserName accountName -Password "password can contain / and \ etc" `

  Bir toplu iş dosyasından bu şekilde komutu çalıştırabilirsiniz:

  `Echo new-smbMapping ... | powershell -command –`

- İlk karakter eğik olmadığı sürece bu soruna geçici bir çözüm--anahtarını çift tırnak işaretleri yerleştirin. İse, etkileşimli mod kullanabilir ve ayrı olarak parolanızı girin veya eğik çizgiyle başlamaması bir anahtar almak için anahtarlarınızı yeniden.

<a id="cannotaccess"></a>
## <a name="application-or-service-cannot-access-a-mounted-azure-files-drive"></a>Uygulama veya hizmet bağlanan bir Azure dosyaları sürücüsüne erişemiyor

### <a name="cause"></a>Nedeni

Sürücüleri, kullanıcı başına bağlanır. Uygulama, uygulamanızın veya hizmetinizin bağlı sürücüye olandan farklı bir kullanıcı hesabı altında çalışıyorsa, sürücü görmezsiniz.

### <a name="solution"></a>Çözüm

Şu çözümlerden birini kullanın:

-   Sürücü, uygulamayı içeren kullanıcı hesabından bağlayın. PsExec gibi bir araç kullanabilirsiniz.
- Kullanıcı adını depolama hesabı adı ve anahtarı geçirin ve password parametrelerini, net komutunu kullanın.
- Cmdkey komutunun içine kimlik bilgisi Yöneticisi kimlik bilgilerini eklemek için kullanın. Bu hizmet hesabı bağlamında, etkileşimli oturum açma aracılığıyla ya da farklı Çalıştır'ı kullanarak bir komut satırından gerçekleştirin.
  
  `cmdkey /add:<storage-account-name>.file.core.windows.net /user:AZURE\<storage-account-name> /pass:<storage-account-key>`
- Paylaşımı doğrudan eşlenen sürücü harfini kullanmadan eşleyin. Tam UNC yolunu kullanarak daha güvenilir olabilir bazı uygulamalar için sürücü harfini düzgün bir şekilde yeniden değil. 

  `net use * \\storage-account-name.file.core.windows.net\share`

Bu yönergeleri uyguladıktan sonra sistem/ağ hizmeti hesabı için ağ kullanım çalıştırdığınızda aşağıdaki hata iletisini alabilirsiniz: "1312 sistem hatası oluştu. Belirtilen oturum yok. Bunu zaten kapatılmış olabilir." Bu meydana gelirse, net use yöntemine geçirilen kullanıcı adı etki alanı bilgilerini içerdiğinden emin olun (örneğin: "[depolama hesabı adı]. file.core.windows .net").

<a id="doesnotsupportencryption"></a>
## <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>"Şifrelemeyi desteklemeyen bir hedefe bir dosya kopyalarsınız" hatası

Ağ üzerinden bir dosya kopyaladığınızda, dosya kaynak bilgisayarda şifresi düz metin olarak aktarılan ve hedefte yeniden şifrelenir. Ancak, şifrelenmiş bir dosyaya kopyalamaya çalışırken şu hatayı görebilirsiniz: "Şifrelemeyi desteklemeyen bir hedefe dosya kopyalarsınız."

### <a name="cause"></a>Nedeni
Şifreleme Dosya Sistemi (EFS) kullanıyorsanız, bu sorun oluşabilir. Azure dosyaları için BitLocker ile şifrelenmiş dosya kopyalanabilir. Ancak, Azure dosyaları NTFS EFS desteklemez.

### <a name="workaround"></a>Geçici çözüm
Ağ üzerinden dosya kopyalamak için önce şifresini çözmelisiniz. Aşağıdaki yöntemlerden birini kullanın:

- Kullanım **/d kopyalama** komutu. Hedef konumda şifresi çözülen dosyalar olarak kaydedilecek şifrelenmiş dosyaları sağlar.
- Aşağıdaki kayıt defteri anahtarını ayarlayın:
  - Yol HKLM\Software\Policies\Microsoft\Windows\System =
  - Değer türü = DWORD
  - Ad CopyFileAllowDecryptedRemoteDestination =
  - Değer = 1

Kayıt defteri anahtarı ayarı ağ paylaşımlara yapılan tüm kopyalama işlemleri etkilediğini unutmayın.

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.
Hala yardıma ihtiyacınız varsa [Destek ekibiyle iletişime geçin](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi alınamıyor.
