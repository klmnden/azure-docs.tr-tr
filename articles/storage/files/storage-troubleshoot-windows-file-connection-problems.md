---
title: Windows Azure dosyaları sorunlarını giderme | Microsoft Docs
description: Windows Azure dosyaları sorunlarını giderme
services: storage
author: jeffpatt24
tags: storage
ms.service: storage
ms.topic: article
ms.date: 01/02/2019
ms.author: jeffpatt
ms.subservice: files
ms.openlocfilehash: 9658ed46e1a46aa3fc2c7fe251fd73b2ef0a13dd
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65991360"
---
# <a name="troubleshoot-azure-files-problems-in-windows"></a>Windows Azure dosyaları sorunlarını giderme

Bu makalede Windows istemcilerinden bağlandığınızda, Microsoft Azure dosyaları'na ilgili genel sorunları listeler. Ayrıca olası nedenleri ve çözümlemeleri için bu sorunları sağlar. Bu makalede sorun giderme adımlarını ek olarak da kullanabilirsiniz [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) Windows istemci ortam önkoşulları doğru olduğundan emin olmak için. En iyi performansı elde etmek için ortamınızı ayarlama yardımcı olur ve bu makalede değinilen belirtileri çoğunu algılanması AzFileDiagnostics otomatikleştirir. Bu bilgiler de bulabilirsiniz [Azure dosyaları paylaşımlarını sorun giderici](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) bağlama/eşleme/bağlama Azure dosyaları paylaşımlarını sorunlara yardımcı olmak için adımları sağlar.

<a id="error5"></a>

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="error-5-when-you-mount-an-azure-file-share"></a>Bir Azure dosya paylaşımını bağladığınızda 5 hatası

Bir dosya paylaşımını bağlayabilmeniz çalıştığınızda şu hatayı alabilirsiniz:

- 5. Sistem hatası oluştu. Erişim reddedildi.

### <a name="cause-1-unencrypted-communication-channel"></a>1. neden: Şifrelenmemiş iletişim kanalı

Güvenlik nedenleriyle, Azure dosya paylaşımlarını bağlantı iletişim kanalını şifreli değildir ve Azure dosya paylaşımlarını bulunduğu aynı veri merkezlerinden bağlantı girişimi yapılmadan değil engellenir. Aynı veri merkezindeki şifrelenmemiş bağlantıları da ise engellenir [güvenli aktarım gerekli](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) depolama hesabı seçeneği etkinleştirilmiştir. Şifreli iletişim kanalı, yalnızca kullanıcının istemci işletim sistemi SMB şifrelemesi destekliyorsa sağlanır.

Windows 8, Windows Server 2012 ve sonraki sürümleri her sistem şifrelemeyi destekleyen SMB 3.0 içeren istekleri anlaşır.

### <a name="solution-for-cause-1"></a>Çözüm nedeni 1 için

1. SMB şifrelemesi (Windows 8, Windows Server 2012 veya üstü) destekleyen bir istemciyi bağlanmak veya aynı veri merkezinde Azure dosya paylaşımı için kullanılan Azure depolama hesabı olarak bir sanal makinesinden bağlanabilirsiniz.
2. Doğrulama [güvenli aktarım gerekli](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) ayarı devre dışı depolama hesabında istemci SMB şifrelemesi desteklemiyorsa.

### <a name="cause-2-virtual-network-or-firewall-rules-are-enabled-on-the-storage-account"></a>2. neden: Sanal ağ veya güvenlik duvarı kuralları depolama hesabı etkinleştirilir 

Sanal ağ (VNET) ve güvenlik duvarı kuralları depolama hesabında yapılandırılmışsa, istemci IP adresi veya sanal ağ erişimine izin verilmesini sürece ağ trafiğini erişimi reddedilir.

### <a name="solution-for-cause-2"></a>Neden 2 çözümü

Sanal ağ ve güvenlik duvarı kuralları depolama hesabı düzgün şekilde yapılandırıldığından doğrulayın. Sanal ağ veya güvenlik duvarı kuralları neden sorun varsa test etmek için geçici olarak depolama hesabı için ayarı değiştirmeniz **tüm ağlardan erişime izin ver**. Daha fazla bilgi için bkz. [yapılandırma Azure depolama güvenlik duvarlarını ve sanal ağlar](https://docs.microsoft.com/azure/storage/common/storage-network-security).

<a id="error53-67-87"></a>
## <a name="error-53-error-67-or-error-87-when-you-mount-or-unmount-an-azure-file-share"></a>Hata 53, hata 67 veya hata bağlama ya da bir Azure dosya paylaşımını ayırma 87

Şirket içinde veya farklı bir veri merkezinde bir dosya paylaşımını bağlayabilmeniz çalıştığınızda aşağıdaki hataları alabilirsiniz:

- 53 sistem hatası oluştu. Ağ yolu bulunamadı.
- 67 sistem hatası oluştu. Ağ adı bulunamıyor.
- 87 sistem hatası oluştu. Parametresi geçersiz.

### <a name="cause-1-port-445-is-blocked"></a>1. neden: Bağlantı noktası 445 engellendi

Bağlantı noktası 445 giden iletişimi, Azure dosyaları bir veri merkezine engellenirse, sistem hatası 53 veya sistem hatası 67 ortaya çıkabilir. İzin vermek veya vermemek bağlantı noktası 445 erişimden ISS'ler özetini görmek için Git [TechNet](https://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx).

445 bağlantı noktası güvenlik duvarı veya ISS engelleyip engellemediğini denetlemek için kullanmak [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) aracı veya `Test-NetConnection` cmdlet'i. 

Kullanılacak `Test-NetConnection` cmdlet, Azure PowerShell Modülü yüklü olması gerekir, bkz: [Azure PowerShell modülü yükleme](/powershell/azure/install-Az-ps) daha fazla bilgi için. `<your-storage-account-name>` ile `<your-resource-group-name>` yerine depolama hesabınızla ilgili bilgileri yazmayı unutmayın.

   
    $resourceGroupName = "<your-resource-group-name>"
    $storageAccountName = "<your-storage-account-name>"

    # This command requires you to be logged into your Azure account, run Login-AzAccount if you haven't
    # already logged in.
    $storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName

    # The ComputerName, or host, is <storage-account>.file.core.windows.net for Azure Public Regions.
    # $storageAccount.Context.FileEndpoint is used because non-Public Azure regions, such as sovereign clouds
    # or Azure Stack deployments, will have different hosts for Azure file shares (and other storage resources).
    Test-NetConnection -ComputerName ([System.Uri]::new($storageAccount.Context.FileEndPoint).Host) -Port 445
    
Bağlantı başarılı olursa şu çıktıyı görmeniz gerekir:
    
  
    ComputerName     : <your-storage-account-name>
    RemoteAddress    : <storage-account-ip-address>
    RemotePort       : 445
    InterfaceAlias   : <your-network-interface>
    SourceAddress    : <your-ip-address>
    TcpTestSucceeded : True
 

> [!Note]  
> Yukarıdaki komut, depolama hesabının geçerli IP adresini döndürür. Bu IP adresinin aynı kalacağı garanti edilmez ve bu adres herhangi bir zamanda değişebilir. Bu IP adresini betiklere veya güvenlik duvarına sabit şekilde kodlamayın.

### <a name="solution-for-cause-1"></a>Çözüm nedeni 1 için

#### <a name="solution-1---use-azure-file-sync"></a>Çözüm 1 - kullanımı Azure dosya eşitleme
Azure dosya eşitleme dönüştürür şirket içi Windows Server'ınızın Azure dosya paylaşımınızın hızlı bir önbelleğine. SMB, NFS ve FTPS gibi verilerinizi yerel olarak erişmek için Windows Server üzerinde kullanılabilir olan herhangi bir protokolünü kullanabilirsiniz. Azure dosya eşitleme, bağlantı noktası 443 üzerinden çalışır ve böylece geçici bir çözüm olarak Azure dosyaları bağlantı noktası 445'in engellenen istemcilerden erişmek için kullanılabilir. [Azure dosya eşitleme ayarlamayı öğrenin](https://docs.microsoft.com/azure/storage/files/storage-sync-files-extend-servers).

#### <a name="solution-2---use-vpn"></a>2 - kullanım VPN çözümü
Bir VPN belirli depolama hesabınıza ayarlayarak, trafik olarak güvenli bir tünel aracılığıyla internet üzerinden geçer. İzleyin [VPN ayarlamaya ilişkin yönergeler](https://github.com/Azure-Samples/azure-files-samples/tree/master/point-to-site-vpn-azure-files
) Windows Azure dosyalarına erişmek için.

#### <a name="solution-3---unblock-port-445-with-help-of-your-ispit-admin"></a>3 - çözüm ISS yardımıyla 445 numaralı bağlantı noktasının engelini kaldırmak / BT yöneticisi
İş ile BT departmanına ya da bağlantı noktası 445'in giden açmak için ISS [Azure IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653).

#### <a name="solution-4---use-rest-api-based-tools-like-storage-explorerpowershell"></a>4 - çözüm tabanlı REST API kullanma araçları gibi Depolama Gezgini/Powershell
Azure dosyaları SMB yanı sıra REST da destekler. REST erişim bağlantı noktası 443 (standart tcp) üzerinde çalışır. Zengin UI deneyimi sağlayan REST API kullanılarak yazılan çeşitli araçları vardır. [Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) , bunlardan biridir. [İndirme ve yükleme, Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) ve Azure dosyaları tarafından desteklenen dosya paylaşımına bağlanın. Ayrıca [PowerShell](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-powershell) , ayrıca kullanıcı REST API.


### <a name="cause-2-ntlmv1-is-enabled"></a>2. neden: NTLMv1 etkin

NTLMv1 iletişim istemcide etkinse, sistem hatası 53 veya sistem hatası 87 ortaya çıkabilir. Azure dosyaları yalnızca NTLMv2 kimlik doğrulamasını destekler. NTLMv1 etkin olması daha az güvenli bir istemci oluşturur. Bu nedenle, iletişim, Azure dosyaları için engellenir. 

Bu hatanın nedenini olup olmadığını belirlemek için aşağıdaki kayıt defteri alt anahtarı 3 değerine ayarlandığını doğrulayın:

**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**

Daha fazla bilgi için [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) TechNet'teki konu.

### <a name="solution-for-cause-2"></a>Neden 2 çözümü

Geri döndürme **LmCompatibilityLevel** değeri 3 Aşağıdaki kayıt defteri alt anahtarında varsayılan değeri:

  **HKLM\SYSTEM\CurrentControlSet\Control\Lsa**

<a id="error1816"></a>
## <a name="error-1816-not-enough-quota-is-available-to-process-this-command-when-you-copy-to-an-azure-file-share"></a>Hata 1816 "yeterli kotası ise bu komutu işlemek için" bir Azure dosya paylaşımına kopyaladığınızda

### <a name="cause"></a>Nedeni

Dosya paylaşımının nerede bağlı bilgisayar için bir dosya izin verilen eşzamanlı açık tanıtıcıları üst sınırına ulaşmadığınız hata 1816 olur.

### <a name="solution"></a>Çözüm

Bazı işler kapatarak eşzamanlı açık tanıtıcı sayısını azaltın ve yeniden deneyin. Daha fazla bilgi için [Microsoft Azure depolama performansı ve ölçeklenebilirlik denetim listesi](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

<a id="accessdeniedportal"></a>
## <a name="error-access-denied-when-browsing-to-an-azure-file-share-in-the-portal"></a>"Erişim reddedildi" hatası portalında bir Azure dosya paylaşımına göz atarken

Portalda bir Azure dosya paylaşımına göz attığınızda aşağıdaki hata iletisini alabilirsiniz:

Erişim reddedildi  
Erişim izniniz yok  
Bu içeriğe erişime izniniz yok gibi görünüyor. Erişim almak için lütfen sahibiyle iletişime geçin.  

### <a name="cause-1-your-user-account-does-not-have-access-to-the-storage-account"></a>1. neden: Kullanıcı hesabınızın, depolama hesabına erişimi yok

### <a name="solution-for-cause-1"></a>Çözüm nedeni 1 için

Azure dosya paylaşımının bulunduğu depolama hesabına Gözat'a tıklayın **erişim denetimi (IAM)** ve kullanıcı hesabınızın, depolama hesabına erişimi olduğunu doğrulayın. Daha fazla bilgi için bkz. [rol tabanlı erişim denetimi (RBAC) ile depolama hesabınızın güvenliğini sağlamak nasıl](https://docs.microsoft.com/azure/storage/common/storage-security-guide#how-to-secure-your-storage-account-with-role-based-access-control-rbac).

### <a name="cause-2-virtual-network-or-firewall-rules-are-enabled-on-the-storage-account"></a>2. neden: Sanal ağ veya güvenlik duvarı kuralları depolama hesabı etkinleştirilir

### <a name="solution-for-cause-2"></a>Neden 2 çözümü

Sanal ağ ve güvenlik duvarı kuralları depolama hesabı düzgün şekilde yapılandırıldığından doğrulayın. Sanal ağ veya güvenlik duvarı kuralları neden sorun varsa test etmek için geçici olarak depolama hesabı için ayarı değiştirmeniz **tüm ağlardan erişime izin ver**. Daha fazla bilgi için bkz. [yapılandırma Azure depolama güvenlik duvarlarını ve sanal ağlar](https://docs.microsoft.com/azure/storage/common/storage-network-security).

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

  `New-SmbMapping -LocalPath y: -RemotePath \\server\share -UserName accountName -Password "password can contain / and \ etc"`

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

Bu yönergeleri uyguladıktan sonra sistem/ağ hizmeti hesabı için ağ kullanım çalıştırdığınızda, aşağıdaki hata iletisini alabilirsiniz: "1312 sistem hatası oluştu. Belirtilen oturum yok. Bunu zaten kapatılmış olabilir." Bu meydana gelirse, net use yöntemine geçirilen kullanıcı adı etki alanı bilgilerini içerdiğinden emin olun (örneğin: "[depolama hesabı adı]. file.core.windows .net").

<a id="doesnotsupportencryption"></a>
## <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>"Şifrelemeyi desteklemeyen bir hedefe bir dosya kopyalarsınız" hatası

Ağ üzerinden bir dosya kopyaladığınızda, dosya kaynak bilgisayarda şifresi düz metin olarak aktarılan ve hedefte yeniden şifrelenir. Ancak, şifrelenmiş bir dosyaya kopyalamaya çalışırken şu hatayı görebilirsiniz: "Şifrelemeyi desteklemeyen bir hedefe dosya kopyalarsınız."

### <a name="cause"></a>Nedeni
Şifreleme Dosya Sistemi (EFS) kullanıyorsanız, bu sorun oluşabilir. Azure dosyaları için BitLocker ile şifrelenmiş dosya kopyalanabilir. Ancak, Azure dosyaları NTFS EFS desteklemez.

### <a name="workaround"></a>Geçici Çözüm
Ağ üzerinden dosya kopyalamak için önce şifresini çözmelisiniz. Aşağıdaki yöntemlerden birini kullanın:

- Kullanım **/d kopyalama** komutu. Hedef konumda şifresi çözülen dosyalar olarak kaydedilecek şifrelenmiş dosyaları sağlar.
- Aşağıdaki kayıt defteri anahtarını ayarlayın:
  - Path = HKLM\Software\Policies\Microsoft\Windows\System
  - Değer türü = DWORD
  - Ad CopyFileAllowDecryptedRemoteDestination =
  - Değer = 1

Kayıt defteri anahtarı ayarı ağ paylaşımlara yapılan tüm kopyalama işlemleri etkilediğini unutmayın.

## <a name="slow-enumeration-of-files-and-folders"></a>Dosya ve klasörlerin yavaş numaralandırması

### <a name="cause"></a>Nedeni

Büyük dizinler için istemci makinede yeterli bir önbellek ise bu sorun oluşabilir.

### <a name="solution"></a>Çözüm

Bu sorunu çözmek için ayarlama **DirectoryCacheEntrySizeMax** istemci makine büyük dizin listelerinde önbelleğe alınmasını izin vermek için kayıt defteri değeri:

- Konum: HKLM\System\CCS\Services\Lanmanworkstation\Parameters
- Değer mane: DirectoryCacheEntrySizeMax 
- Değer türü: DWORD
 
 
Örneğin, 0x100000 için ayarlayabilir ve performansını daha iyi hale gelirse bakın.

## <a name="error-aaddstenantnotfound-in-enabling-azure-active-directory-authentication-for-azure-files-unable-to-locate-active-tenants-with-tenant-id-aad-tenant-id"></a>Azure dosyaları "Kiracı kimliği aad Kiracı kimliği etkin bir kiracıyla bulmak için yapılandırılamıyor" Azure Active Directory kimlik doğrulamasının etkinleştirilmesinde hata AadDsTenantNotFound

### <a name="cause"></a>Nedeni

Hata AadDsTenantNotFound olur, çalıştığınızda [Azure dosyaları için Azure Active Directory (AAD) kimlik doğrulamasını etkinleştirme](https://docs.microsoft.com/azure/storage/files/storage-files-active-directory-enable) bir depolama hesabı üzerinde nereye [AAD etki alanı Service(AAD DS)](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) AAD'de oluşturulmaz. ilişkili abonelik kiracısıdır.  

### <a name="solution"></a>Çözüm

AAD DS AAD kiracısı için Dağıtılmış depolama hesabınızın abonelik etkinleştirin. Yönetilen bir etki alanı oluşturmak için AAD Kiracı yönetici ayrıcalıkları gerekir. Azure AD Kiracı yöneticisi değilseniz, yöneticisine başvurun ve adım adım yönergeleri [etkinleştirme Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started).

[!INCLUDE [storage-files-condition-headers](../../../includes/storage-files-condition-headers.md)]

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun.
Hala yardıma ihtiyacınız varsa [Destek ekibiyle iletişime geçin](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi alınamıyor.
