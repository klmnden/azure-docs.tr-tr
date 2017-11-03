---
title: "Windows Azure dosyaları sorunlarını giderme | Microsoft Docs"
description: "Windows Azure dosyaları sorunlarını giderme"
services: storage
documentationcenter: 
author: genlin
manager: willchen
editor: na
tags: storage
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: genli
ms.openlocfilehash: 5aacc8a920c9343c5efa89128aabb1505fc2d9aa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-azure-files-problems-in-windows"></a>Windows Azure dosyaları sorunlarını giderme

Bu makalede Windows istemcilerinden gelen bağlandığınızda, Microsoft Azure dosyalarıyla ilgili genel sorunları listeler. Ayrıca olası nedenleri ve çözümlemeleri için bu sorunları sağlar. Bu makaledeki sorun giderme adımlarını ek olarak da kullanabilirsiniz [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) Windows istemci ortamı doğru Önkoşullar olduğundan emin olmak için. AzFileDiagnostics algılama en iyi performansı elde etmek ortamınızı ayarlayın yardımcı olur ve bu makalede açıklanan Belirtiler çoğunu otomatikleştirir. Bu bilgiler de bulabilirsiniz [Azure dosya paylaşımları sorun gidericisini](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) bağlanma/eşleme/bağlama Azure dosya paylaşımları sorunlara yardımcı olmak için adımları sağlar.


<a id="error53-67-87"></a>
## <a name="error-53-error-67-or-error-87-when-you-mount-or-unmount-an-azure-file-share"></a>Hata 53, hata 67 veya hata bağlama ya da Azure dosya paylaşımının çıkarın 87

Şirket içi veya farklı bir veri merkezinde bir dosya paylaşımını bağlama çalıştığınızda aşağıdaki hataları alabilirsiniz:

- 53 sistem hatası oluştu. Ağ yolu bulunamadı.
- 67 sistem hatası oluştu. Ağ adı bulunamıyor.
- 87 sistem hatası oluştu. Parametresi geçersiz.

### <a name="cause-1-unencrypted-communication-channel"></a>1. neden: Şifrelenmemiş bir iletişim kanalı

Güvenlik nedenleriyle, Azure dosya paylaşımları için bağlantılar iletişim kanalını şifrelenmiş değil ve bağlantı denemesi aynı veri merkezinden Azure dosya paylaşımları bulunduğu değil yaptıysanız engellenir. Yalnızca kullanıcının istemci işletim sistemi SMB şifrelemesi destekliyorsa iletişim kanalı şifreleme sağlanır.

Windows 8, Windows Server 2012 ve sonraki sürümlerinde her sistem şifrelemeyi destekleyen SMB 3.0 içeren isteklerin anlaşmaları.

### <a name="solution-for-cause-1"></a>Neden 1 için çözüm

Aşağıdakilerden birini yapan bir istemciden Bağlan:

- Windows 8 ve Windows Server 2012 veya sonraki sürümler gereksinimlerini karşılayan
- Azure dosya paylaşımı için kullanılan Azure depolama hesabı ile aynı veri merkezinde bir sanal makineden bağlanır

### <a name="cause-2-port-445-is-blocked"></a>Neden 2: Bağlantı noktası 445 engellendi

Bağlantı noktası 445 giden iletişim Azure dosyaları veri merkezine engellenirse, sistem hatası 53 veya sistem hatası 67 ortaya çıkabilir. İzin veren veya bağlantı noktası 445 erişimden izin vermeyecek ISS'ler özetini görmek için Git [TechNet](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx).

Bu nedenle "Sistem hata 53" iletisi arkasında olup olmadığını anlamak için Portqry TCP:445 endpoint sorgulamak için kullanabilirsiniz. TCP:445 endpoint filtre olarak görüntüleniyorsa, TCP bağlantı noktasını engellendi. Örnek bir sorgu şöyledir:

  `g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

TCP bağlantı noktası 445 ağ yol kuralı tarafından engellenirse aşağıdaki çıktı görürsünüz:

  `TCP port 445 (microsoft-ds service): FILTERED`

Portqry kullanma hakkında daha fazla bilgi için bkz: [Portqry.exe komut satırı yardımcı programının açıklaması](https://support.microsoft.com/help/310099).

### <a name="solution-for-cause-2"></a>Neden 2 için çözüm

445 bağlantı noktası için giden açmak için BT departmanınızın ile çalışırsınız [Azure IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="cause-3-ntlmv1-is-enabled"></a>3. neden: NTLMv1 etkin

İstemcide NTLMv1 iletişim etkinse, sistem hatası 53 veya sistem hatası 87 ortaya çıkabilir. Azure dosyaları yalnızca NTLMv2 kimlik doğrulamasını destekler. Etkin NTLMv1 sahip daha az güvenli bir istemci oluşturur. Bu nedenle, iletişim için Azure dosyaları engellendi. 

Bu hatanın nedenini olup olmadığını belirlemek için aşağıdaki kayıt defteri alt anahtarı 3 değerine ayarlandığını doğrulayın:

**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**

Daha fazla bilgi için bkz: [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) TechNet'te konu.

### <a name="solution-for-cause-3"></a>Neden 3 için çözüm

Geri **LmCompatibilityLevel** varsayılan değeri aşağıdaki kayıt defteri alt anahtarında 3 değerine:

  **HKLM\SYSTEM\CurrentControlSet\Control\Lsa**

<a id="error1816"></a>
## <a name="error-1816-not-enough-quota-is-available-to-process-this-command-when-you-copy-to-an-azure-file-share"></a>Hata 1816 "yeterli kotası, bu komutu işlemek için kullanılabilir" bir Azure dosya paylaşımına kopyaladığınızda

### <a name="cause"></a>Nedeni

Dosya paylaşımının nerede bağlı bilgisayar için bir dosya izin verilen eşzamanlı açık tanıtıcıların sayısı üst sınırı ulaştığında hata 1816 olur.

### <a name="solution"></a>Çözüm

Bazı tanıtıcıları kapatarak eşzamanlı açık tanıtıcı sayısını azaltın ve yeniden deneyin. Daha fazla bilgi için bkz: [Microsoft Azure Storage performans ve ölçeklenebilirlik Yapılacaklar listesi](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-files-in-windows"></a>Yavaş dosya için ve Windows Azure dosyaları kopyalama

Azure dosya hizmeti dosyaları aktarmaya çalıştığınızda yavaş performans görebilirsiniz.

- Belirli bir en düşük g/ç boyutu gereksinim yoksa, en iyi performans için g/ç boyutu 1 MB kullanmanızı öneririz.
-   Son yazma ile genişletme dosya boyutunu bildiğiniz ve dosyada unwritten tail sıfır içerdiğinde yazılımınızı uyumluluk sorunlarına sahip değil, önceden bir genişletme yazma her yazma yapmak yerine dosya boyutunu ayarlayın.
-   Sağ copy yöntemini kullanın:
    -   Kullanım [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) iki dosya paylaşımları arasında herhangi bir aktarım için.
    -   Kullanım [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) bir şirket içi bilgisayar dosya paylaşımlarına arasında.

### <a name="considerations-for-windows-81-or-windows-server-2012-r2"></a>Windows 8.1 veya Windows Server 2012 R2 için ilgili önemli noktalar

Windows 8.1 veya Windows Server 2012 R2 çalıştıran istemciler için olduğundan emin olun, [KB3114025](https://support.microsoft.com/help/3114025) düzeltme yüklenir. Bu düzeltme oluşturma performansını artırır ve tanıtıcıları kapatın.

Düzeltme yüklü olup olmadığını denetlemek için aşağıdaki betiği çalıştırabilirsiniz:

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Düzeltme yüklediyseniz, aşağıdaki çıkış görüntülenir:

`HKEY_Local_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies {96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1`

> [!Note]
> Windows Server 2012 R2 Azure Marketi görüntülerinde düzeltme aralık 2015'ten başlayarak varsayılan olarak yüklenen KB3114025 sahip.

<a id="shareismissing"></a>
## <a name="no-folder-with-a-drive-letter-in-my-computer"></a>Bir sürücü harfine sahip bir klasör bulunmadığından **Bilgisayarım**

Yönetici olarak net Kullan'ı kullanarak Azure dosya paylaşımının eşlerseniz paylaşım eksik gibi görünüyor.

### <a name="cause"></a>Nedeni

Varsayılan olarak, bir yönetici olarak Windows dosya Gezgini çalışmaz. Bir yönetici komut isteminden net kullanım çalıştırırsanız, yönetici olarak ağ sürücüsü eşlemeli. Eşlenen sürücüler kullanıcı merkezli olduğundan, farklı bir kullanıcı hesabı altında bağlarsanız oturum kullanıcı hesabı sürücüleri görüntülemez.

### <a name="solution"></a>Çözüm
Paylaşımı, yönetici olmayan komut satırından bağlayın. Alternatif olarak, izleyebilirsiniz [bu TechNet konusuna](https://technet.microsoft.com/library/ee844140.aspx) yapılandırmak için **EnableLinkedConnections** kayıt defteri değeri.

<a id="netuse"></a>
## <a name="net-use-command-fails-if-the-storage-account-contains-a-forward-slash"></a>Depolama hesabı eğik içeriyorsa net use komutu başarısız olur.

### <a name="cause"></a>Nedeni

Net use komutunu eğik çizgi (/) komut satırı seçeneği olarak yorumlar. Kullanıcı hesap adınızı eğik çizgiyle başlıyorsa, sürücü eşleme başarısız olur.

### <a name="solution"></a>Çözüm

Sorunu çözmek için aşağıdaki adımlardan birini kullanabilirsiniz:

- Aşağıdaki PowerShell komutunu çalıştırın:

  `New-SmbMapping -LocalPath y: -RemotePath \\server\share -UserName accountName -Password "password can contain / and \ etc" `

  Bir toplu iş dosyasından bu şekilde komutu çalıştırabilirsiniz:

  `Echo new-smbMapping ... | powershell -command –`

- İlk karakter eğik olmadığı sürece bu soruna geçici bir çözüm--anahtar etrafında çift tırnak işareti koyun. İse, etkileşimli mod kullanabilir ve ayrıca, parolanızı girin veya eğik çizgiyle başlamaması anahtarını almak için anahtarları yeniden.

<a id="cannotaccess"></a>
## <a name="application-or-service-cannot-access-a-mounted-azure-files-drive"></a>Uygulama veya hizmet bağlı Azure dosyaları sürücüye erişemiyor

### <a name="cause"></a>Nedeni

Kullanıcı başına bağlı sürücüler. Uygulama veya hizmet takılı sürücü olandan farklı bir kullanıcı hesabı altında çalışıyorsa, uygulama sürücü görmezsiniz.

### <a name="solution"></a>Çözüm

Şu çözümlerden birini kullanın:

-   Sürücü uygulamayı içeren kullanıcı hesabından bağlayın. PsExec gibi bir araç kullanabilirsiniz.
- Kullanıcı adı depolama hesabı adı ve anahtarı geçirin ve net parola parametrelerini komutunu kullanın.

Bu yönergeleri izledikten sonra sistem/ağ hizmeti hesabı için net kullanım çalıştırdığınızda aşağıdaki hata iletisini alabilirsiniz: "1312 sistem hatası oluştu. Belirtilen oturum yok. Bunu zaten kapatılmış olabilir." Bu gerçekleşirse, net use yöntemine geçirilen kullanıcı adı etki alanı bilgileri içerdiğinden emin olun (örneğin: "[depolama hesabı adı]. file.core.windows .net").

<a id="doesnotsupportencryption"></a>
## <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>"Dosya şifreleme desteklemeyen bir hedefe Kopyalamakta olduğunuz" hatası

Bir dosya ağ üzerinden kopyalandığında, dosya kaynak bilgisayarda şifresi, düz metin olarak iletilen ve hedefte yeniden şifrelenir. Ancak, şifrelenmiş bir dosya kopyalamaya çalışırken aşağıdaki hatayı görebilirsiniz: "Şifrelemeyi desteklemiyor bir hedef dosya kopyalıyorsunuz."

### <a name="cause"></a>Nedeni
Şifreleme Dosya Sistemi (EFS) kullanıyorsanız, bu sorun oluşabilir. BitLocker ile şifrelenmiş dosyaları Azure dosyaları kopyalanabilir. Ancak, Azure dosyaları NTFS EFS desteklemez.

### <a name="workaround"></a>Geçici çözüm
Ağ üzerinden dosya kopyalamak için önce şifresini gerekir. Aşağıdaki yöntemlerden birini kullanın:

- Kullanım **/d kopyalama** komutu. Hedef konumda şifresi çözülen dosyaları olarak kaydedilecek şifrelenmiş dosyaları sağlar.
- Aşağıdaki kayıt defteri anahtarını ayarlayın:
  - Yol HKLM\Software\Policies\Microsoft\Windows\System =
  - Değer türü = DWORD
  - Adı CopyFileAllowDecryptedRemoteDestination =
  - Değer = 1

Kayıt defteri anahtarı ayarı ağ paylaşımlarına yapılan tüm kopyalama işlemleri etkilediğini unutmayın.

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.
Hala yardıma gereksiniminiz varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözülmüş sorununuzu almak için.
