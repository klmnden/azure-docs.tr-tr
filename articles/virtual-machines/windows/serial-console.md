---
title: Azure sanal makinesi seri konsol | Microsoft Docs
description: Azure sanal makineleri için çift yönlü seri Konsolu.
services: virtual-machines-windows
documentationcenter: ''
author: harijay
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/05/2018
ms.author: harijay
ms.openlocfilehash: e891e9c9fd87f370f0c98639ff0c6fc5b8cc81af
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32194412"
---
# <a name="virtual-machine-serial-console-preview"></a>Sanal makine seri konsol (Önizleme) 


Azure sanal makine seri konsolunuzdaki Linux ve Windows sanal makineler için metin tabanlı bir konsol erişim sağlar. Bu seri bağlantı COM1 seri bağlantı noktasına sanal makine ve sanal makine erişim sağlar ve sanal makinenin ağa ilgili olmayan / sistem durumu işletim. Seri konsol erişimi için bir sanal makine şu anda yalnızca Azure portal aracılığıyla yapılır ve VM katılımcı olan kullanıcılar için veya sanal makineye erişim yukarıda izin. 

> [!Note] 
> Kullanım koşullarını kabul ediyorum koşuluyla önizlemeleri için kullanılabilir hale getirilir. Daha fazla bilgi için bkz: [Microsoft Azure ek kullanım koşulları'nı Microsoft Azure önizlemeleri.] (https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Bu hizmet olarak şu anda **genel Önizleme** ve seri konsoluna erişimi sanal makineler için genel Azure bölgeler için kullanılabilir. Bu noktada seri konsol kullanılabilir Azure kamu, Azure Almanya ve Azure Çin bulut değil.

 

## <a name="prerequisites"></a>Önkoşullar 

* Sanal makine olmalıdır [önyükleme tanılama](boot-diagnostics.md) etkin 
* Seri konsol kullanarak hesabı olmalıdır [katkıda bulunan rolü](../../role-based-access-control/built-in-roles.md) VM için ve [önyükleme tanılama](boot-diagnostics.md) depolama hesabı. 

## <a name="open-the-serial-console"></a>Seri konsol açın
sanal makineler için seri Konsolu aracılığıyla erişilebilir durumda yalnızca [Azure portal](https://portal.azure.com). Portal üzerinden sanal makineler için seri konsoluna erişmek için adımları aşağıda verilmiştir 

  1. Azure portalını açın
  2. Soldaki menüde sanal makineleri seçin.
  3. VM listesinde tıklayın. VM için genel bakış sayfası açılır.
  4. Destek + sorun giderme bölümüne aşağı kaydırın ve seri konsol (Önizleme) seçeneğine tıklayın. Seri konsolu ile yeni bir bölmesini açın ve bağlantıyı başlatın.

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

### <a name="disable-feature"></a>Özelliğini devre dışı bırakma
Seri konsol işlevlerini belirli VM'ler için bu VM'in önyükleme tanılama ayarını devre dışı bırakarak devre dışı bırakılabilir.

## <a name="serial-console-security"></a>Seri konsol güvenlik 

### <a name="access-security"></a>Erişimi güvenliği 
Seri konsol erişimi olan kullanıcılar için sınırlı [VM katkıda bulunanlar](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) ya da sanal makineye erişim üstünde. AAD kiracınızın çok faktörlü kimlik doğrulaması gerektiren sonra uygulamaya erişim aracılığıyla olduğundan seri konsoluna erişimi MFA ayrıca gerekir [Azure portal](https://portal.azure.com).

### <a name="channel-security"></a>Kanalı güvenliği
Tüm verileri geri gönderilir ve kablo İleri şifrelenir.

### <a name="audit-logs"></a>Denetim günlükleri
Seri konsol tüm erişim şu anda oturum [önyükleme tanılama](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) sanal makinenin günlükleri. Bu günlükler erişim sahibi ve Azure sanal makine yöneticisi tarafından denetlenir.  

>[!CAUTION] 
Konsolda çalıştırma komutları içeriyorsa ya da parolaları, gizli, kullanıcı adlarını veya diğer, kişisel bilgilerin (PII) çıkış konsol için erişim parolası günlüğe kaydedilen olsa da, bu sanal makine önyükleme tanılaması için yazılır günlükleri, tüm diğer görünür metin, seri konsolun kaydırma uyarlamasını bir parçası olarak birlikte işlevselliği yedekleyin. Bu günlükler döngüsel ve parolaları ve/veya PII içeren herhangi bir şey için Uzak Masaüstü'nü kullanarak en iyi uygulama olarak önerilir ancak tanılama depolama hesabı için Okuma izinlerine sahip tek kişiler bunları erişimi. 

### <a name="concurrent-usage"></a>Eş zamanlı kullanım
Seri konsoluna bir kullanıcının bağlı olduğu ve başka bir kullanıcı başarıyla aynı sanal makinenin erişim isteğinde, ilk kullanıcının bağlantısı kesilecek ve ikinci kullanıcı duran ve fiziksel konsolunda ve yeni bir bırakarak ilk kullanıcı benzer bir şekilde bağlı durduğunu kullanıcı.

>[!CAUTION] 
Başka bir deyişle, bağlantısı kesilen kullanıcının oturumu değil! Bağlantıyı Kes (aracılığıyla SIGHUP veya benzer bir mekanizma) bağlı bir oturum kapatma zorlamak için hala yol haritası özelliğidir. Windows için yoktur SAC içinde etkin otomatik bir zaman aşımı ancak, Linux için terminal zaman aşımı ayarını yapılandırabilirsiniz. 


## <a name="accessing-serial-console-for-windows"></a>Seri konsol için Windows erişme 
Azure daha yeni Windows Server görüntülerinde olacaktır [Özel Yönetim Konsolu](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) (SAC) varsayılan olarak etkin. SAC Windows server sürümleri desteklenir, ancak istemci sürümleri (örneğin, Windows 10, Windows 8 veya Windows 7) mevcut değil. Seri konsol kullanarak oluşturulan Windows sanal makineler için etkinleştirmeyi Feb2018 veya alt görüntüler Lütfen aşağıdaki adımları kullanın: 

1. Windows sanal makinenizi Uzak Masaüstü aracılığıyla bağlanma
2. Bir yönetici komut isteminden aşağıdaki komutları çalıştırın 
* `bcdedit /ems {current} on`
* `bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200`
3. Etkinleştirilecek SAC Konsolu için sistemi yeniden başlatın

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

Gerekli SAC çevrimdışı de etkinleştirilebilir 

1. Var olan VM için bir veri diski olarak yapılandırılmış SAC istediğiniz windows disk ekleyin. 
2. Bir yönetici komut isteminden aşağıdaki komutları çalıştırın 
* `bcdedit /store <mountedvolume>\boot\bcd /ems {default} on`
* `bcdedit /store <mountedvolume>\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

### <a name="how-do-i-know-if-sac-is-enabled-or-not"></a>SAC veya etkin olup olmadığını nasıl anlayabilirim 

Varsa [SAC] (https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) etkin seri konsol SAC istemi gösterilmez. Bazı durumlarda bir VM sistem durumu bilgisi gösterebilir veya boş.  

### <a name="enabling-boot-menu-to-show-in-the-serial-console"></a>Seri konsoldan göstermek, önyükleme menüsü etkinleştirme 

Windows önyükleme yükleyicisi etkinleştirmeniz gerekirse göstermek için seri konsolunda Windows önyükleme yükleyicisi için aşağıdaki ek seçenekler ekleyebilirsiniz ister.

1. Windows sanal makinenizi Uzak Masaüstü aracılığıyla bağlanma
2. Bir yönetici komut isteminden aşağıdaki komutları çalıştırın 
* `bcdedit /set {bootmgr} displaybootmenu yes`
* `bcdedit /set {bootmgr} timeout 5`
* `bcdedit /set {bootmgr} bootems yes`
3. Önyükleme menüsünde etkinleştirilmesi için sistemi yeniden başlatma

> [!NOTE] 
> Bu işlev noktası desteği, anahtarları etkin değil, Gelişmiş Önyükleme Seçenekleri üzerinde bcdedit/set {geçerli} onetimeadvancedoptions kullanmanız gerekiyorsa, bkz [bcdedit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcdedit--set) daha fazla ayrıntı için

## <a name="windows-commands---cmd"></a>Windows komutları - CMD 

Bu bölümde örnek komutları burada SAC RDP bağlantı hatalarını giderme gerektiğinde gibi VM erişmek için kullanmanız gerekebilir senaryolarda ortak görevleri gerçekleştirmek için içerir.

SAC tüm Windows sürümlerinde bu yana Windows Server 2003 dahil ancak varsayılan olarak devre dışıdır. SAC kullanır `sacdrv.sys` çekirdek sürücüsü `Special Administration Console Helper` hizmet (`sacsvr`) ve `sacsess.exe` işlemi. Daha fazla bilgi için bkz: [Acil Durum Yönetimi Hizmetleri Araçları ve ayarları](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc787940(v%3dws.10)).

SAC seri bağlantı noktası üzerinden çalışan işletim sisteminizi bağlanmanıza olanak sağlar. SAC, CMD'den başlattığında `sacsess.exe` başlatır `cmd.exe` çalışan işletim sisteminizi içinde. Görev Yöneticisi'ni, aynı VM için RDP süresi bağlandığını SAC için seri Konsolu özelliği görebilirsiniz. SAC erişim CMD aynıdır `cmd.exe` RDP aracılığıyla bağlandığında kullanın. Hepsi aynı komutları ve araçları PowerShell Bu CMD örnekten başlatma olanağı dahil olmak üzere kullanılabilir. SAC arasındaki temel farklardan ve Windows Kurtarma Ortamı (WinRE) bu SAC'burada WinRE farklı, en düşük işletim sistemi önyükleniyor, çalışan işletim sistemini yönetmenize imkan. Azure VM'ler WinRE, seri konsol özelliğiyle erişme olanağını desteklemez ancak Azure VM'ler SAC yönetilebilir.

SAC 80 x 24 ekran arabelleğinde hiçbir kaydırma geri ile sınırlı olduğundan eklemek `| more` komutları aynı anda çıkış tek sayfa görüntüleme. Kullanım `<spacebar>` bir sonraki sayfada görmek için veya `<enter>` sonraki satıra görmek için.  

`SHIFT+INSERT` Seri konsol penceresi için Yapıştır kısayoludur.

SAC'ın sınırlı ekran arabelleğinde nedeniyle uzun komutlar çıkışı bir yerel metin düzenleyicide yazın daha kolay olabilir ve SAC yapıştırdığınız.

### <a name="view-and-edit-windows-registry-settings"></a>Windows kayıt defteri ayarlarını görüntüleyin ve düzenleyin
#### <a name="verify-rdp-is-enabled"></a>RDP etkin doğrulayın
`reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections`

`reg query "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections`

İlgili Grup İlkesi ayarı yapılandırılmışsa, ikinci anahtarı (altında \Policies) yalnızca mevcut olmaz.

#### <a name="enable-rdp"></a>RDP etkinleştir
`reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0`

`reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections /t REG_DWORD /d 0` 

İkinci anahtarı (altında \Policies) yalnızca ilgili Grup İlkesi ayarını yapılandırılmış gerekir. Grup İlkesi'nde yapılandırılmışsa, değer sonraki Grup İlkesi yenileme sırasında yazılacaktır.

### <a name="manage-windows-services"></a>Windows Hizmetleri yönetme

#### <a name="view-service-state"></a>Görünüm hizmet durumu
`sc query termservice`
####  <a name="view-service-logon-account"></a>Görünüm hizmeti oturum açma hesabı
`sc qc termservice`
#### <a name="set-service-logon-account"></a>Hizmeti oturum açma hesabını ayarlama 
`sc config termservice obj= "NT Authority\NetworkService"`

Bir alanı eşittir işaretinden sonra gereklidir.
#### <a name="set-service-start-type"></a>Hizmet başlangıç türünü ayarla
`sc config termservice start= demand` 

Bir alanı eşittir işaretinden sonra gereklidir. Olası başlangıç değerleri dahil `boot`, `system`, `auto`, `demand`, `disabled`, `delayed-auto`.
#### <a name="set-service-dependencies"></a>Hizmet bağımlıları ayarlayın
`sc config termservice depend= RPCSS`

Bir alanı eşittir işaretinden sonra gereklidir.
#### <a name="start-service"></a>Hizmeti Başlat
`net start termservice`

or

`sc start termservice`
#### <a name="stop-service"></a>Hizmetini durdurun
`net stop termservice`

or

`sc stop termservice`
### <a name="manage-networking-features"></a>Ağ özelliklerini yönetme
#### <a name="show-nic-properties"></a>NIC özelliklerini göster
`netsh interface show interface` 
#### <a name="show-ip-properties"></a>IP özelliklerini göster
`netsh interface ip show config`
#### <a name="show-ipsec-configuration"></a>IPSec yapılandırmasını göster
`netsh nap client show configuration`  
#### <a name="enable-nic"></a>NIC etkinleştir
`netsh interface set interface name="<interface name>" admin=enabled`
#### <a name="set-nic-to-use-dhcp"></a>DHCP kullanmak üzere NIC ayarlayın
`netsh interface ip set address name="<interface name>" source=dhcp`

Azure VM'ler her zaman konuk işletim sistemi bir IP adresi almak için DHCP kullanmak üzere yapılandırılmış olması gerekir. Azure statik IP ayarı hala statik IP VM'ye vermek için DHCP kullanır.
#### <a name="ping"></a>Ping
`ping 8.8.8.8` 
#### <a name="port-ping"></a>Bağlantı noktası ping  
Telnet istemcisi'ni yükleme

`dism /online /Enable-Feature /FeatureName:TelnetClient`

Bağlantıyı test etme

`telnet bing.com 80`

Telnet istemcisi'ni kaldırmak için

`dism /online /Disable-Feature /FeatureName:TelnetClient`

Windows yöntemleri için varsayılan olarak sınırlı olduğunda PowerShell bağlantı noktası bağlanabilirliği test etmek için daha iyi bir yaklaşım olabilir. Örnekler için aşağıdaki PowerShell bölümüne bakın.
#### <a name="test-dns-name-resolution"></a>DNS ad çözümlemesini test
`nslookup bing.com`
#### <a name="show-windows-firewall-rule"></a>Windows Güvenlik duvarı kuralı Göster
`netsh advfirewall firewall show rule name="Remote Desktop - User Mode (TCP-In)"`
#### <a name="disable-windows-firewall"></a>Windows Güvenlik Duvarı'nı devre dışı bırak
`netsh advfirewall set allprofiles state off`

Geçici olarak Windows Güvenlik duvarı kuralı için sorun giderme sırasında bu komutu kullanabilirsiniz. Sonraki yeniden başlatmada etkinleştir olması veya zaman içinde enaable aşağıdaki komutu kullanarak. Windows Güvenlik Duvarı hizmetini (MPSSVC) veya temel filtreleme altyapısı (BFE) hizmetini Windows Güvenlik duvarı kuralı şekilde durdurmaz. MPSSVC veya BFE durdurmak, tüm bağlantısı engellenme'nda neden olacak.
#### <a name="enable-windows-firewall"></a>Windows Güvenlik duvarını etkinleştir
`netsh advfirewall set allprofiles state on`
### <a name="manage-users-and-groups"></a>Kullanıcıları ve grupları yönetme
#### <a name="create-local-user-account"></a>Yerel kullanıcı hesabı oluşturma
`net user /add <username> <password>`
#### <a name="add-local-user-to-local-group"></a>Yerel kullanıcı yerel grubuna ekleme
`net localgroup Administrators <username> /add`
#### <a name="verify-user-account-is-enabled"></a>Kullanıcı hesabının etkin doğrulayın
`net user <username> | find /i "active"`

Genelleştirilmiş görüntüden oluşturulan azure VM'ler, VM sağlama işlemi sırasında belirtilen adına yeniden adlandırılması yerel yönetici hesabı sahip olur. Genellikle olmayacaktır şekilde `Administrator`.
#### <a name="enable-user-account"></a>Kullanıcı hesabını etkinleştirme
`net user <username> /active:yes`  
#### <a name="view-user-account-properties"></a>Kullanıcı hesabı özellikleri görüntüle
`net user <username>`

Bir yerel yönetici hesabından ilgi örnek satırlar:

`Account active Yes`

`Account expires Never`

`Password expires Never`

`Workstations allowed All`

`Logon hours allowed All`

`Local Group Memberships *Administrators`

#### <a name="view-local-groups"></a>Yerel grupları görüntüle
`net localgroup`
### <a name="manage-the-windows-event-log"></a>Windows olay günlüğünü yönetme
#### <a name="query-event-log-errors"></a>Olay günlüğü hatalarını sorgulama
`wevtutil qe system /c:10 /f:text /q:"Event[System[Level=2]]" | more`

Değişiklik `/c:10` dönün veya filtreyle eşleşen tüm olayları döndürülecek taşımak için olayları istenen sayıda.
#### <a name="query-event-log-by-event-id"></a>Sorgu olay günlüğüne olay kimliği
`wevtutil qe system /c:1 /f:text /q:"Event[System[EventID=11]]" | more`
#### <a name="query-event-log-by-event-id-and-provider"></a>Olay günlüğüne olay kimliği ve sağlayıcı tarafından sorgulama
`wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11]]" | more`
#### <a name="query-event-log-by-event-id-and-provider-for-the-last-24-hours"></a>Son 24 saat için sorgu olay günlüğüne olay kimliği ve sağlayıcı tarafından
`wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11 and TimeCreated[timediff(@SystemTime) <= 86400000]]]"`

Kullanım `604800000` geri 7 gün 24 saat yerine aramak için.
#### <a name="query-event-log-by-event-id-provider-and-eventdata-in-the-last-7-days"></a>Sorgu olay günlüğünde olay kimliği, sağlayıcı ve EventData son 7 gün
`wevtutil qe security /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-Security-Auditing'] and EventID=4624 and TimeCreated[timediff(@SystemTime) <= 604800000]] and EventData[Data[@Name='TargetUserName']='<username>']]" | more`
### <a name="view-or-remove-installed-applications"></a>Görüntüleme veya yüklü uygulamaları kaldırma
#### <a name="list-installed-applications"></a>Liste yüklü uygulamalar
`wmic product get Name,InstallDate | sort /r | more`

`sort /r` Göre azalan sırada sıralar ne yakın zamanda yüklenmiş görmeyi kolaylaştırmak için tarih yükleyin. Kullanım `<spacebar>` çıktı, sonraki sayfaya geçmek için veya `<enter>` tek bir çizgi ilerlemek için.
#### <a name="uninstall-an-application"></a>Bir uygulamayı kaldırın
`wmic path win32_product where name="<name>" call uninstall`

Değiştir `<name>` kaldırmak istediğiniz yukarıdaki komutta uygulama için verilen ada sahip.

### <a name="file-system-management"></a>Dosya sistemi yönetimi
#### <a name="get-file-version"></a>Dosya sürümü alma
`wmic datafile where "drive='C:' and path='\\windows\\system32\\drivers\\' and filename like 'netvsc%'" get version /format:list`

Bu örnek netvsc.sys, netvsc63.sys ya da Windows sürümüne bağlı olarak netvsc60.sys sanal NIC sürücüsünün dosya sürümünü döndürür.
#### <a name="scan-for-system-file-corruption"></a>Sistem dosyasının Bozulması tara
`sfc /scannow`

Ayrıca bkz. [Windows görüntüyü onarmak](https://docs.microsoft.com/windows-hardware/manufacture/desktop/repair-a-windows-image).
#### <a name="scan-for-system-file-corruption"></a>Sistem dosyasının Bozulması tara
`dism /online /cleanup-image /scanhealth`

Ayrıca bkz. [Windows görüntüyü onarmak](https://docs.microsoft.com/windows-hardware/manufacture/desktop/repair-a-windows-image).
#### <a name="export-file-permissions-to-text-file"></a>Dosya izinleri metin dosyasına dışarı aktarma
`icacls %programdata%\Microsoft\Crypto\RSA\MachineKeys /t /c > %temp%\MachineKeys_permissions_before.txt`
#### <a name="save-file-permissions-to-acl-file"></a>Dosya izinleri ACL dosyaya kaydet
`icacls %programdata%\Microsoft\Crypto\RSA\MachineKeys /save %temp%\MachineKeys_permissions_before.aclfile /t`  
#### <a name="restore-file-permissions-from-acl-file"></a>Dosya izinleri ACL dosyadan geri yükle
`icacls %programdata%\Microsoft\Crypto\RSA /save %temp%\MachineKeys_permissions_before.aclfile /t`

Yolun kullanırken `/restore` kullanırken, belirtilen klasörün üst klasörünü olması gereken `/save`. Bu örnekte, `\RSA` üst `\MachineKeys` belirtilen klasör `/save` Yukarıdaki örnek.
#### <a name="take-ntfs-ownership-of-a-folder"></a>Bir klasörü, NTFS sahipliğini
`takeown /f %programdata%\Microsoft\Crypto\RSA\MachineKeys /a /r`  
#### <a name="grant-ntfs-permissions-to-a-folder-recursively"></a>Bir klasör yinelemeli olarak NTFS izinleri
`icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "BUILTIN\Administrators:(F)"`  
### <a name="manage-devices"></a>Cihazları yönetme
#### <a name="remove-non-present-pnp-devices"></a>Mevcut olmayan PNP cihazları kaldırın
`%windir%\System32\RUNDLL32.exe %windir%\System32\pnpclean.dll,RunDLL_PnpClean /Devices /Maxclean`
### <a name="manage-group-policy"></a>Grup İlkesi'ni yönetme
#### <a name="force-group-policy-update"></a>Grup İlkesi güncelleştirmesini zorla
`gpupdate /force /wait:-1`
### <a name="miscellaneous-tasks"></a>Çeşitli görevleri
#### <a name="show-os-version"></a>İşletim sistemi sürümü göster
`ver`

or 

`wmic os get caption,version,buildnumber /format:list`

or 

`systeminfo  find /i "os name"`

`systeminfo | findstr /i /r "os.*version.*build"`
#### <a name="view-os-install-date"></a>Görünüm işletim sistemi yükleme tarihi
`systeminfo | find /i "original"`

or 

`wmic os get installdate`
#### <a name="view-last-boot-time"></a>Görünüm son önyükleme saati
`systeminfo | find /i "system boot time"`
#### <a name="view-time-zone"></a>Görünüm saat dilimi
`systeminfo | find /i "time zone"`

or

`wmic timezone get caption,standardname /format:list`
#### <a name="restart-windows"></a>Windows'u yeniden başlatma
`shutdown /r /t 0`

Ekleme `/f` olmayan uyarı kullanıcılar kapatmak için çalışan uygulamaları zorlar.
#### <a name="detect-safe-mode-boot"></a>Güvenli modda önyükleme Algıla
`bcdedit /enum | find /i "safeboot"` 

## <a name="windows-commands---powershell"></a>Windows komutları - PowerShell

Bir komut istemi eriştikten sonra PowerShell SAC içinde çalıştırmak için şunu yazın:

`powershell <enter>`

>[!CAUTION]
Diğer bir PowerShell komutları çalıştırmadan önce PowerShell oturumundan PSReadLine modülünü Kaldır. Burada fazladan karakterler PSReadLine SAC PowerShell oturumunda çalışıyorsa, panodan yapıştırılan metni sunulması bilinen bir sorun yoktur.

İlk PSReadLine yüklü olmadığını denetleyin. Windows Server 2016, Windows 10 ve sonraki Windows sürümlerinde varsayılan olarak yüklenir. El ile yüklemiş olduğu ise, yalnızca önceki Windows sürümlerinde mevcut olabilir. 

Bu komut istemi hiçbir çıkış döndürürse, ardından modülünün yüklenmedi ve PowerShell oturumu SAC içinde normal olarak kullanmaya devam edebilirsiniz.

`get-module psreadline`

Yukarıdaki komut PSReadLine Modül sürümü döndürürse, onu kaldırmak için aşağıdaki komutu çalıştırın. Bu komut silmeyin veya modülünü kaldırmak, yalnızca, geçerli PowerShell oturumundan kaldırır.

`remove-module psreadline`

### <a name="view-and-edit-windows-registry-settings"></a>Windows kayıt defteri ayarlarını görüntüleyin ve düzenleyin
#### <a name="verify-rdp-is-enabled"></a>RDP etkin doğrulayın
`get-itemproperty -path 'hklm:\system\curRentcontrolset\control\terminal server' -name 'fdenytsconNections'`

`get-itemproperty -path 'hklm:\software\policies\microsoft\windows nt\terminal services' -name 'fdenytsconNections'`

İlgili Grup İlkesi ayarı yapılandırılmışsa, ikinci anahtarı (altında \Policies) yalnızca mevcut olmaz.
#### <a name="enable-rdp"></a>RDP etkinleştir
`set-itemproperty -path 'hklm:\system\curRentcontrolset\control\terminal server' -name 'fdenytsconNections' 0 -type dword`

`set-itemproperty -path 'hklm:\software\policies\microsoft\windows nt\terminal services' -name 'fdenytsconNections' 0 -type dword`

İkinci anahtarı (altında \Policies) yalnızca ilgili Grup İlkesi ayarını yapılandırılmış gerekir. Grup İlkesi'nde yapılandırılmışsa, değer sonraki Grup İlkesi yenileme sırasında yazılacaktır.
### <a name="manage-windows-services"></a>Windows Hizmetleri yönetme
#### <a name="view-service-details"></a>Hizmet ayrıntılarını görüntüleme
`get-wmiobject win32_service -filter "name='termservice'" |  format-list Name,DisplayName,State,StartMode,StartName,PathName,ServiceType,Status,ExitCode,ServiceSpecificExitCode,ProcessId`

`Get-Service` kullanılabilir ancak hizmeti oturum açma hesabını içermez. `Get-WmiObject win32-service` yapar.
#### <a name="set-service-logon-account"></a>Hizmeti oturum açma hesabını ayarlama
`(get-wmiobject win32_service -filter "name='termservice'").Change($null,$null,$null,$null,$null,$false,'NT Authority\NetworkService')`

Bir hizmet hesabı dışındaki kullanırken `NT AUTHORITY\LocalService`, `NT AUTHORITY\NetworkService`, veya `LocalSystem`, hesap parolasını son (sekizinci) bağımsız değişken sonra hesap adı belirtin.
#### <a name="set-service-startup-type"></a>Hizmet başlatma türünü ayarlayın
`set-service termservice -startuptype Manual`

`Set-service` kabul `Automatic`, `Manual`, veya `Disabled` başlangıç türü.
#### <a name="set-service-dependencies"></a>Hizmet bağımlıları ayarlayın
`Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Services\TermService' -Name DependOnService -Value @('RPCSS','TermDD')`
#### <a name="start-service"></a>Hizmeti Başlat
`start-service termservice`
#### <a name="stop-service"></a>Hizmetini durdurun
`stop-service termservice`
### <a name="manage-networking-features"></a>Ağ özelliklerini yönetme
#### <a name="show-nic-properties"></a>NIC özelliklerini göster
`get-netadapter | where {$_.ifdesc.startswith('Microsoft Hyper-V Network Adapter')} |  format-list status,name,ifdesc,macadDresS,driverversion,MediaConNectState,MediaDuplexState`

or 

`get-wmiobject win32_networkadapter -filter "servicename='netvsc'" |  format-list netenabled,name,macaddress`

`Get-NetAdapter` 2012 +, 2008R2 kullanmak için kullanılabilir `Get-WmiObject`.
#### <a name="show-ip-properties"></a>IP özelliklerini göster
`get-wmiobject Win32_NetworkAdapterConfiguration -filter "ServiceName='netvsc'" |  format-list DNSHostName,IPAddress,DHCPEnabled,IPSubnet,DefaultIPGateway,MACAddress,DHCPServer,DNSServerSearchOrder`
#### <a name="enable-nic"></a>NIC etkinleştir
`get-netadapter | where {$_.ifdesc.startswith('Microsoft Hyper-V Network Adapter')} | enable-netadapter`

or

`(get-wmiobject win32_networkadapter -filter "servicename='netvsc'").enable()`

`Get-NetAdapter` 2012 +, 2008R2 kullanmak için kullanılabilir `Get-WmiObject`.
#### <a name="set-nic-to-use-dhcp"></a>DHCP kullanmak üzere NIC ayarlayın
`get-netadapter | where {$_.ifdesc.startswith('Microsoft Hyper-V Network Adapter')} | Set-NetIPInterface -DHCP Enabled`

`(get-wmiobject Win32_NetworkAdapterConfiguration -filter "ServiceName='netvsc'").EnableDHCP()`

`Get-NetAdapter` 2012 + üzerinde mevcut değil. Kullanmak için 2008R2 `Get-WmiObject`. Azure VM'ler her zaman konuk işletim sistemi bir IP adresi almak için DHCP kullanmak üzere yapılandırılmış olması gerekir. Azure statik IP ayarı hala VM IP vermek için DHCP kullanır.
#### <a name="ping"></a>Ping
`test-netconnection`

or

`get-wmiobject Win32_PingStatus -Filter 'Address="8.8.8.8"' | format-table -autosize IPV4Address,ReplySize,ResponseTime`

`Test-Netconnection` herhangi bir parametre ping dener olmadan `internetbeacon.msedge.net`. 2012 + üzerinde kullanılabilir. Kullanmak için 2008R2 `Get-WmiObject` ikinci örnekte olduğu gibi.
#### <a name="port-ping"></a>Ping bağlantı noktası
`test-netconnection -ComputerName bing.com -Port 80`

or

`(new-object Net.Sockets.TcpClient).BeginConnect('bing.com','80',$null,$null).AsyncWaitHandle.WaitOne(300)`

`Test-NetConnection` 2012 + üzerinde mevcut değil. 2008R2 kullanmak için `Net.Sockets.TcpClient`
#### <a name="test-dns-name-resolution"></a>DNS ad çözümlemesini test
`resolve-dnsname bing.com` 

or 

`[System.Net.Dns]::GetHostAddresses('bing.com')`

`Resolve-DnsName` 2012 + üzerinde mevcut değil. Kullanmak için 2008R2 `System.Net.DNS`.
#### <a name="show-windows-firewall-rule-by-name"></a>Windows Güvenlik duvarı kuralı tarafından adını göster
`get-netfirewallrule -name RemoteDesktop-UserMode-In-TCP` 
#### <a name="show-windows-firewall-rule-by-port"></a>Windows Güvenlik duvarı kuralı bağlantı noktası tarafından Göster
`get-netfirewallportfilter | where {$_.localport -eq 3389} | foreach {Get-NetFirewallRule -Name $_.InstanceId} | format-list Name,Enabled,Profile,Direction,Action`

or

`(new-object -ComObject hnetcfg.fwpolicy2).rules | where {$_.localports -eq 3389 -and $_.direction -eq 1} | format-table Name,Enabled`

`Get-NetFirewallPortFilter` 2012 + üzerinde mevcut değil. Kullanmak için 2008R2 `hnetcfg.fwpolicy2` COM nesnesi. 
#### <a name="disable-windows-firewall"></a>Windows Güvenlik Duvarı'nı devre dışı bırak
`Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False`

`Set-NetFirewallProfile` 2012 + üzerinde mevcut değil. Kullanmak için 2008R2 `netsh advfirewall` yukarıdaki CMD bölümünde başvurulan.
### <a name="manage-users-and-groups"></a>Kullanıcıları ve grupları yönetme
#### <a name="create-local-user-account"></a>Yerel kullanıcı hesabı oluşturma
`new-localuser <name>`
#### <a name="verify-user-account-is-enabled"></a>Kullanıcı hesabının etkin doğrulayın
`(get-localuser | where {$_.SID -like "S-1-5-21-*-500"}).Enabled`

or 

`(get-wmiobject Win32_UserAccount -Namespace "root\cimv2" -Filter "SID like 'S-1-5-%-500'").Disabled`

`Get-LocalUser` 2012 + üzerinde mevcut değil. Kullanmak için 2008R2 `Get-WmiObject`. Bu örnek, her zaman SID'sine sahip yerleşik yerel yönetici hesabı gösterir `S-1-5-21-*-500`. Genelleştirilmiş görüntüden oluşturulan azure VM'ler, VM sağlama işlemi sırasında belirtilen adına yeniden adlandırılması yerel yönetici hesabı sahip olur. Genellikle olmayacaktır şekilde `Administrator`.
#### <a name="add-local-user-to-local-group"></a>Yerel kullanıcı yerel grubuna ekleme
`add-localgroupmember -group Administrators -member <username>`
#### <a name="enable-local-user-account"></a>Yerel kullanıcı hesabı etkinleştir
`get-localuser | where {$_.SID -like "S-1-5-21-*-500"} | enable-localuser` 

Bu örnekte her zaman SID'sine sahip yerleşik yerel yönetici hesabını etkinleştirir `S-1-5-21-*-500`. Genelleştirilmiş görüntüden oluşturulan azure VM'ler, VM sağlama işlemi sırasında belirtilen adına yeniden adlandırılması yerel yönetici hesabı sahip olur. Genellikle olmayacaktır şekilde `Administrator`.
#### <a name="view-user-account-properties"></a>Kullanıcı hesabı özellikleri görüntüle
`get-localuser | where {$_.SID -like "S-1-5-21-*-500"} | format-list *`

or 

`get-wmiobject Win32_UserAccount -Namespace "root\cimv2" -Filter "SID like 'S-1-5-%-500'" |  format-list Name,Disabled,Status,Lockout,Description,SID`

`Get-LocalUser` 2012 + üzerinde mevcut değil. Kullanmak için 2008R2 `Get-WmiObject`. Bu örnek, her zaman SID'sine sahip yerleşik yerel yönetici hesabı gösterir `S-1-5-21-*-500`.
#### <a name="view-local-groups"></a>Yerel grupları görüntüle
`(get-localgroup).name | sort` `(get-wmiobject win32_group).Name | sort`

`Get-LocalUser` 2012 + üzerinde mevcut değil. Kullanmak için 2008R2 `Get-WmiObject`.
### <a name="manage-the-windows-event-log"></a>Windows olay günlüğünü yönetme
#### <a name="query-event-log-errors"></a>Olay günlüğü hatalarını sorgulama
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Level=2]]" | more`

Değişiklik `/c:10` dönün veya filtreyle eşleşen tüm olayları döndürülecek taşımak için olayları istenen sayıda.
#### <a name="query-event-log-by-event-id"></a>Sorgu olay günlüğüne olay kimliği
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[EventID=11]]" | more`
#### <a name="query-event-log-by-event-id-and-provider"></a>Olay günlüğüne olay kimliği ve sağlayıcı tarafından sorgulama
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11]]" | more`
#### <a name="query-event-log-by-event-id-and-provider-for-the-last-24-hours"></a>Son 24 saat için sorgu olay günlüğüne olay kimliği ve sağlayıcı tarafından
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11 and TimeCreated[timediff(@SystemTime) <= 86400000]]]"`

Kullanım `604800000` geri 7 gün 24 saat yerine aramak için. |
#### <a name="query-event-log-by-event-id-provider-and-eventdata-in-the-last-7-days"></a>Sorgu olay günlüğünde olay kimliği, sağlayıcı ve EventData son 7 gün
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Provider[@Name='Microsoft-Windows-Security-Auditing'] and EventID=4624 and TimeCreated[timediff(@SystemTime) <= 604800000]] and EventData[Data[@Name='TargetUserName']='<username>']]" | more`
### <a name="view-or-remove-installed-applications"></a>Görüntüleme veya yüklü uygulamaları kaldırma
#### <a name="list-installed-software"></a>Yüklü listesinde yazılım
`get-wmiobject win32_product | select installdate,name | sort installdate -descending | more`
#### <a name="uninstall-software"></a>Yazılım kaldırma
`(get-wmiobject win32_product -filter "Name='<name>'").Uninstall()`
### <a name="file-system-management"></a>Dosya sistemi yönetimi
#### <a name="get-file-version"></a>Dosya sürümü alma
`(get-childitem $env:windir\system32\drivers\netvsc*.sys).VersionInfo.FileVersion`

Bu örnek netvsc.sys, netvsc63.sys ya da Windows sürümüne bağlı olarak netvsc60.sys adlı sanal NIC sürücüsünün dosya sürümünü döndürür.
#### <a name="download-and-extract-file"></a>Karşıdan yükle ve dosyasını ayıklayın
`$path='c:\bin';md $path;cd $path;(new-object net.webclient).downloadfile( ('htTp:/'+'/download.sysinternals.com/files/SysinternalsSuite.zip'),"$path\SysinternalsSuite.zip");(new-object -com shelL.apPlication).namespace($path).CopyHere( (new-object -com shelL.apPlication).namespace("$path\SysinternalsSuite.zip").Items(),16)`

Bu örnekte bir `c:\bin` klasörü, ardından indirir ve araçlarınızla Sysinternals dizisi ayıklar `c:\bin`.
### <a name="miscellaneous-tasks"></a>Çeşitli görevleri
#### <a name="show-os-version"></a>İşletim sistemi sürümü göster
`get-wmiobject win32_operatingsystem | format-list caption,version,buildnumber` 
#### <a name="view-os-install-date"></a>Görünüm işletim sistemi yükleme tarihi
`(get-wmiobject win32_operatingsystem).converttodatetime((get-wmiobject win32_operatingsystem).installdate)`
#### <a name="view-last-boot-time"></a>Görünüm son önyükleme saati
`(get-wmiobject win32_operatingsystem).lastbootuptime`
#### <a name="view-windows-uptime"></a>Görünüm Windows açık kalma süresi
`"{0:dd}:{0:hh}:{0:mm}:{0:ss}.{0:ff}" -f ((get-date)-(get-wmiobject win32_operatingsystem).converttodatetime((get-wmiobject win32_operatingsystem).lastbootuptime))`

Açık kalma süresi olarak döndürür `<days>:<hours>:<minutes>:<seconds>:<milliseconds>`, örneğin `49:16:48:00.00`. 
#### <a name="restart-windows"></a>Windows'u yeniden başlatma
`restart-computer`

Ekleme `-force` olmayan uyarı kullanıcılar kapatmak için çalışan uygulamaları zorlar.
### <a name="instance-metadata"></a>Örnek meta verileri

OsType, konum, vmSize, bunun nedeni, ad, resourceGroupName, Subscriptionıd, privateIpAddress ve Publicıpaddress gibi ayrıntılarını görüntülemek için Azure VM dahilinde Azure örneği meta verileri sorgulayabilirsiniz.

Azure ana bilgisayar örneği meta veri hizmeti için bir REST çağrısı yaptığından örneği meta verileri Sorgulama sağlıklı Konuk ağ bağlantısı gerektirir. Örnek meta verileri sorgulayabilir varsa, bu nedenle, konuk bir Azure barındırılan hizmet için ağ üzerinden iletişim kuramıyor olduğunu bildirir.

Daha fazla bilgi için bkz: [Azure örneği meta veri hizmeti](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service).

#### <a name="instance-metadata"></a>Örnek meta verileri
`$im = invoke-restmethod -headers @{"metadata"="true"} -uri http://169.254.169.254/metadata/instance?api-version=2017-08-01 -method get`

`$im | convertto-json`
#### <a name="os-type-instance-metadata"></a>İşletim sistemi türü (örneği meta verileri)
`$im.Compute.osType`
#### <a name="location-instance-metadata"></a>Konum (örneği meta verileri)
`$im.Compute.Location`
#### <a name="size-instance-metadata"></a>Boyut (örneği meta verileri)
`$im.Compute.vmSize`
#### <a name="vm-id-instance-metadata"></a>VM kimliği (örneği meta verileri)
`$im.Compute.vmId`
#### <a name="vm-name-instance-metadata"></a>VM adı (örnek meta verileri)
`$im.Compute.name`
#### <a name="resource-group-name-instance-metadata"></a>Kaynak grubu adı (örnek meta verileri)
`$im.Compute.resourceGroupName`
#### <a name="subscription-id-instance-metadata"></a>Abonelik kimliği (örneği meta verileri)
`$im.Compute.subscriptionId`
#### <a name="tags-instance-metadata"></a>Etiketler (örneği meta verileri)
`$im.Compute.tags`
#### <a name="placement-group-id-instance-metadata"></a>Yerleştirme Grup Kimliği (örneği meta verileri)
`$im.Compute.placementGroupId`
#### <a name="platform-fault-domain-instance-metadata"></a>Platform hata etki alanı (örneği meta verileri)
`$im.Compute.platformFaultDomain`
#### <a name="platform-update-domain-instance-metadata"></a>Platform Güncelleştirmesi etki alanı (örneği meta verileri)
`$im.Compute.platformUpdateDomain`
#### <a name="ipv4-private-ip-address-instance-metadata"></a>IPv4 özel IP adresi (örnek meta verileri)
`$im.network.interface.ipv4.ipAddress.privateIpAddress`
#### <a name="ipv4-public-ip-address-instance-metadata"></a>IPv4 genel IP adresi (örnek meta verileri)
`$im.network.interface.ipv4.ipAddress.publicIpAddress`
#### <a name="ipv4-subnet-address--prefix-instance-metadata"></a>IPv4 alt ağ adresi / önek (meta verileri örneği)
`$im.network.interface.ipv4.subnet.address`

`$im.network.interface.ipv4.subnet.prefix`
#### <a name="ipv6-ip-address-instance-metadata"></a>IPv6 IP adresi (örnek meta verileri)
`$im.network.interface.ipv6.ipAddress`
#### <a name="mac-address-instance-metadata"></a>MAC adresi (örnek meta verileri)
`$im.network.interface.macAddress`

## <a name="errors"></a>Hatalar
Hataların çoğu doğasını ve bu bağlantı adresi denemeden geçicidir. Aşağıdaki tablo hataları ve azaltma listesini gösterir 

Hata                            |   Risk azaltma 
:---------------------------------|:--------------------------------------------|
İçin önyükleme tanılaması ayarları alınamadı '<VMNAME>'. Seri konsol kullanmak için bu önyükleme tanılaması bu VM için etkinleştirildiğinden emin olun. | VM sahip olduğundan emin olun [önyükleme tanılama](boot-diagnostics.md) etkin. 
VM durduruldu deallocated bir durumda değil. VM başlatmak ve seri konsol bağlantısı yeniden deneyin. | Sanal makinenin seri konsoluna erişmek için başlatılmış bir durumda olması gerekir
Bu VM seri konsol kullanmak için gerekli izinlere sahip değil. En az olduğundan emin olun VM katkıda bulunan rolü izinleri.| Seri konsol erişimi erişim izni belirli gerektirir. Bkz: [erişim gereksinimleri](#prerequisites) Ayrıntılar için
Önyükleme tanılama depolama hesabı için kaynak grubu belirlenemiyor '<STORAGEACCOUNTNAME>'. Bu VM için önyükleme tanılaması etkin ve bu depolama hesabına erişimi doğrulayın. | Seri konsol erişimi erişim izni belirli gerektirir. Bkz: [erişim gereksinimleri](#prerequisites) Ayrıntılar için

## <a name="known-issues"></a>Bilinen sorunlar 
Biz yine seri konsol erişimi için Önizleme aşamalarını olduğu gibi biz bilinen bazı sorunlar çalışıyorsanız, bunların olası geçici çözümler ile listesi aşağıdadır 

Sorun                           |   Risk azaltma 
:---------------------------------|:--------------------------------------------|
Sanal makine ölçek kümesi örnek seri konsolu ile bir seçenek yoktur | Önizleme zaman seri konsoluna erişimi sanal makine ölçek kümesi örneklerinin desteklenmiyor.
Devreyi enter bağlantı başlık satırında bir günlük göstermez | [Devreyi girin, hiçbir şey yapmıyor](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md)
Yalnızca sistem durumu bilgilerini bir Windows VM bağlanırken gösterilir| [Windows sistem durumu sinyalleri](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Windows_Health_Info.md)
Çekirdek hata ayıklama etkinleştirilirse SAC komut isteminde yazın kurulamıyor | RDP VM ve Çalıştır `bcdedit /debug {current} off` yükseltilmiş bir komut isteminden. RDP olamaz, bunun yerine başka bir Azure VM için işletim sistemi diski ekleyin ve verileri kullanarak bir disk olarak eklenmiş durumdayken değiştirmek `bcdedit /store <drive letter of data disk>:\boot\bcd /debug <identifier> off`, disk geri değiştirme.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular 
**Q. Geri bildirim nasıl gönderebilir miyim?**

A. Giderek bir sorun görüş https://aka.ms/serialconsolefeedback. Alternatif olarak daha az (tercih edilen) geribildirim gönderme aracılığıyla azserialhelp@microsoft.com veya sanal makine kategorisi http://feedback.azure.com 

**Q. Bir hata alıyorsunuz "Varolan konsol çakışan işletim sistemi türü istenen Linux işletim sistemi türü olan" Windows"var?**

A. Bu bu sorunu düzeltmek için bilinen bir sorundur yalnızca açık [Azure bulut Kabuk](https://docs.microsoft.com/azure/cloud-shell/overview) bash modunda ve yeniden deneyin.

**Q. I bir destek servis talebi burada dosya seri Konsolu erişebilir değilim?**

A. Bu önizleme özelliği Azure Önizleme koşulları ele alınmıştır. Bu destek en iyi yukarıda belirtilen kanallar aracılığıyla gerçekleştirilir. 

## <a name="next-steps"></a>Sonraki adımlar
* Seri konsol ayrıca kullanılabilir [Linux](../linux/serial-console.md) VM'ler
* Daha fazla bilgi edinmek [bootdiagnostics](boot-diagnostics.md)
