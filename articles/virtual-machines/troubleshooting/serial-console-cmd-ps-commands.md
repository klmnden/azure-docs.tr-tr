---
title: CMD ve PowerShell Azure Windows vm'lerinde | Microsoft Docs
description: Azure Windows Vm'lerinde SAC CMD ve PowerShell komutlarını kullanma
services: virtual-machines-windows
documentationcenter: ''
author: alsin
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/14/2018
ms.author: alsin
ms.openlocfilehash: 55b7e45bb9e600267e1dad0e36e9a97eca9a7d40
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60306892"
---
# <a name="windows-commands---cmd-and-powershell"></a>Windows komutları - CMD ve PowerShell

Bu bölümde, örnek komutlar burada SAC RDP bağlantı sorunlarını giderme gerektiğinde gibi Windows VM erişmek için kullanmanız gerekebilir senaryolarda ortak görevleri gerçekleştirmek için içerir.

SAC tüm Windows sürümlerinde, Windows Server 2003 beri eklenmiştir, ancak varsayılan olarak devre dışıdır. SAC dayanır `sacdrv.sys` çekirdek sürücüsü `Special Administration Console Helper` hizmeti (`sacsvr`) ve `sacsess.exe` işlem. Daha fazla bilgi için [Acil Durum Yönetim Hizmetleri Araçları ve ayarları](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc787940(v%3dws.10)).

SAC seri bağlantı noktası üzerinden çalışan işletim sisteminizi bağlanmanıza olanak sağlar. SAC, CMD başlattığınızda `sacsess.exe` başlatır `cmd.exe` çalışan işletim sisteminiz içinde. Görev Yöneticisi'ni, aynı sanal makinenize yönelik RDP süresi bağlandığını SAC için seri konsol özelliği aracılığıyla görebilirsiniz. SAC erişim CMD aynıdır `cmd.exe` RDP aracılığıyla bağlandığında kullanın. Aynı komutları ve araçları PowerShell'i başlatın, CMD örneğinden özelliği dahil olmak üzere kullanılabilir. SAC arasında önemli bir fark ve bu SAC içinde Windows Kurtarma Ortamı (WinRE) burada WinRE farklı, en düşük işletim sistemi içinde önyükleme, çalışan işletim sistemini yönetmenizi sağlar. Azure Vm'leri seri konsol özelliğiyle WinRE, erişim olanağı desteklemez ancak Azure Vm'leri SAC yönetilebilir.

SAC bir 80 x 24 ekran arabelleğinin kaydırmayı geri ile sınırlı olduğundan, ekleme `| more` komutlarının çıkışı bir sayfanın aynı anda görüntülenecek. Kullanım `<spacebar>` sonraki sayfada, görmek için veya `<enter>` sonraki satıra görmek için.  

`SHIFT+INSERT` Seri konsol penceresi için Yapıştır kısayoldur.

SAC'ın sınırlı ekran arabelleği nedeniyle uzun komutlar bir yerel Metin Düzenleyicisi'nde dışarı yazabileceğiniz daha kolay olabilir ve ardından SAC yapıştırıldığında.

## <a name="view-and-edit-windows-registry-settings"></a>Windows kayıt defteri ayarlarını görüntüleyin ve düzenleyin
### <a name="verify-rdp-is-enabled"></a>RDP etkinleştirildiğini doğrulama
`reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections`

`reg query "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections`

İkinci anahtarı (\Policies altında) ilgili Grup İlkesi ayarı yapılandırılmışsa, yalnızca sunulacaktır.

### <a name="enable-rdp"></a>RDP'yi etkinleştirin
`reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0`

`reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections /t REG_DWORD /d 0` 

İkinci anahtarı (altındaki \Policies) yalnızca ilgili Grup İlkesi ayarını yapılandırılmış gerekecek. Grup İlkesi'nde yapılandırılmışsa, değer sonraki Grup İlkesi yenileme sırasında yazılacaktır.

## <a name="manage-windows-services"></a>Windows hizmetlerini yönet

### <a name="view-service-state"></a>Hizmet durumunu görüntüle
`sc query termservice`
###  <a name="view-service-logon-account"></a>Hizmeti oturum açma hesabı görüntüle
`sc qc termservice`
### <a name="set-service-logon-account"></a>Küme hizmeti oturum açma hesabı 
`sc config termservice obj= "NT Authority\NetworkService"`

Eşittir işaretinden sonra boşluk gereklidir.
### <a name="set-service-start-type"></a>Hizmet başlangıç türünü ayarla
`sc config termservice start= demand` 

Eşittir işaretinden sonra boşluk gereklidir. Olası başlangıç değerleri dahil `boot`, `system`, `auto`, `demand`, `disabled`, `delayed-auto`.
### <a name="set-service-dependencies"></a>Hizmet bağımlıları ayarlayın
`sc config termservice depend= RPCSS`

Eşittir işaretinden sonra boşluk gereklidir.
### <a name="start-service"></a>Hizmeti Başlat
`net start termservice`

or

`sc start termservice`
### <a name="stop-service"></a>Hizmeti Durdur
`net stop termservice`

or

`sc stop termservice`
## <a name="manage-networking-features"></a>Ağ özelliklerini yönetme
### <a name="show-nic-properties"></a>NIC özellikleri göster
`netsh interface show interface` 
### <a name="show-ip-properties"></a>IP özellikleri göster
`netsh interface ip show config`
### <a name="show-ipsec-configuration"></a>IPSec yapılandırmasını göster
`netsh nap client show configuration`  
### <a name="enable-nic"></a>NIC etkinleştir
`netsh interface set interface name="<interface name>" admin=enabled`
### <a name="set-nic-to-use-dhcp"></a>NIC DHCP kullanacak şekilde ayarlama
`netsh interface ip set address name="<interface name>" source=dhcp`

Hakkında daha fazla bilgi için `netsh`, [Buraya](https://docs.microsoft.com/windows-server/networking/technologies/netsh/netsh-contexts).

Azure sanal makineler, her zaman konuk işletim sistemi, bir IP adresini almak için DHCP kullanmak üzere yapılandırılmalıdır. Azure statik IP ayarı yine de statik IP VM vermek için DHCP kullanır.
### <a name="ping"></a>Ping
`ping 8.8.8.8` 
### <a name="port-ping"></a>Bağlantı noktası ping  
Telnet istemcisi yükleme

`dism /online /Enable-Feature /FeatureName:TelnetClient`

Bağlantıyı test etme

`telnet bing.com 80`

Telnet istemcisi kaldırmak için

`dism /online /Disable-Feature /FeatureName:TelnetClient`

Varsayılan olarak Windows kullanılabilen yöntemler sınırlıdır, PowerShell bağlantı noktası bağlanabilirliği test etmek için daha iyi bir yaklaşım olabilir. Örnekler için aşağıdaki PowerShell bölümüne bakın.
### <a name="test-dns-name-resolution"></a>DNS ad çözümlemesini test
`nslookup bing.com`
### <a name="show-windows-firewall-rule"></a>Windows Güvenlik duvarı kuralı Göster
`netsh advfirewall firewall show rule name="Remote Desktop - User Mode (TCP-In)"`
### <a name="disable-windows-firewall"></a>Windows Güvenlik Duvarı'nı devre dışı bırak
`netsh advfirewall set allprofiles state off`

Geçici olarak Windows Güvenlik duvarı kuralı için sorun giderme sırasında bu komutu kullanabilirsiniz. Bu, sonraki yeniden başlatmada veya aşağıdaki komutu kullanarak etkinleştirdiğinizde etkinleştirme olacaktır. Windows Güvenlik Duvarı hizmeti (MPSSVC) veya temel filtre altyapısı (BFE) hizmeti Windows Güvenlik duvarı kuralı şekilde durdurmayın. MPSSVC veya BFE durdurmak, tüm bağlantı engellenme neden olur.
### <a name="enable-windows-firewall"></a>Windows Güvenlik duvarını etkinleştir
`netsh advfirewall set allprofiles state on`
## <a name="manage-users-and-groups"></a>Kullanıcıları ve grupları yönetme
### <a name="create-local-user-account"></a>Yerel kullanıcı hesabı oluşturma
`net user /add <username> <password>`
### <a name="add-local-user-to-local-group"></a>Yerel kullanıcı yerel gruba ekleyin.
`net localgroup Administrators <username> /add`
### <a name="verify-user-account-is-enabled"></a>Kullanıcı hesabı etkinleştirildiğini doğrulama
`net user <username> | find /i "active"`

Genelleştirilmiş bir görüntüden oluşturulan azure VM'ler VM sağlama sırasında belirtilen adı olarak yeniden adlandırıldı yerel yönetici hesabı olacaktır. Genellikle olmayacaktır şekilde `Administrator`.
### <a name="enable-user-account"></a>Kullanıcı hesabı etkinleştir
`net user <username> /active:yes`  
### <a name="view-user-account-properties"></a>Kullanıcı hesabı özellikleri görüntüle
`net user <username>`

İlgilendiğiniz bir yerel yönetici hesabı için örnek satırlar:

`Account active Yes`

`Account expires Never`

`Password expires Never`

`Workstations allowed All`

`Logon hours allowed All`

`Local Group Memberships *Administrators`

### <a name="view-local-groups"></a>Yerel grupları görüntüleyin
`net localgroup`
## <a name="manage-the-windows-event-log"></a>Windows olay günlüğünü yönetme
### <a name="query-event-log-errors"></a>Olay günlüğü hatalarını sorgulama
`wevtutil qe system /c:10 /f:text /q:"Event[System[Level=2]]" | more`

Değişiklik `/c:10` olayları döndürür veya filtreyle eşleşen tüm olayları döndürülecek taşımak için istenen sayıda.
### <a name="query-event-log-by-event-id"></a>Olay günlüğü olay Kimliğine göre sorgulama
`wevtutil qe system /c:1 /f:text /q:"Event[System[EventID=11]]" | more`
### <a name="query-event-log-by-event-id-and-provider"></a>Olay günlüğüne olay kimliği ve sağlayıcı göre sorgulama
`wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11]]" | more`
### <a name="query-event-log-by-event-id-and-provider-for-the-last-24-hours"></a>Son 24 saat için olay günlüğüne olay kimliği ve sağlayıcı göre sorgulama
`wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11 and TimeCreated[timediff(@SystemTime) <= 86400000]]]"`

Kullanım `604800000` geri 7 gün 24 saat yerine aramak için.
### <a name="query-event-log-by-event-id-provider-and-eventdata-in-the-last-7-days"></a>Sorgu olay günlüğünde olay kimliği, sağlayıcı ve EventData tarafından son 7 gün
`wevtutil qe security /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-Security-Auditing'] and EventID=4624 and TimeCreated[timediff(@SystemTime) <= 604800000]] and EventData[Data[@Name='TargetUserName']='<username>']]" | more`
## <a name="view-or-remove-installed-applications"></a>Görüntüleme veya yüklü uygulamaları kaldırma
### <a name="list-installed-applications"></a>Uygulamaları yüklü Listele
`wmic product get Name,InstallDate | sort /r | more`

`sort /r` Göre azalan sırada sıralar ne yakın zamanda yüklenmiş görmeyi kolaylaştırmak için tarih yükleyin. Kullanım `<spacebar>` çıktı, sonraki sayfaya ilerlemek için veya `<enter>` bir satır ilerlemek için.
### <a name="uninstall-an-application"></a>Bir uygulamayı Kaldır
`wmic path win32_product where name="<name>" call uninstall`

Değiştirin `<name>` kaldırmak istediğiniz yukarıdaki komutta uygulama için verilen ada sahip.

## <a name="file-system-management"></a>Dosya sistemi yönetimi
### <a name="get-file-version"></a>Dosya sürümü Al
`wmic datafile where "drive='C:' and path='\\windows\\system32\\drivers\\' and filename like 'netvsc%'" get version /format:list`

Bu örnekte netvsc.sys, netvsc63.sys ya da Windows sürümüne bağlı olarak netvsc60.sys sanal NIC sürücü dosyası sürümünü döndürür.
### <a name="scan-for-system-file-corruption"></a>Sistem dosyasının Bozulması tara
`sfc /scannow`

Ayrıca bkz: [bir Windows görüntüsünü onarım](https://docs.microsoft.com/windows-hardware/manufacture/desktop/repair-a-windows-image).
### <a name="scan-for-system-file-corruption"></a>Sistem dosyasının Bozulması tara
`dism /online /cleanup-image /scanhealth`

Ayrıca bkz: [bir Windows görüntüsünü onarım](https://docs.microsoft.com/windows-hardware/manufacture/desktop/repair-a-windows-image).
### <a name="export-file-permissions-to-text-file"></a>Dosya izinleri metin dosyasına dışarı aktarma
`icacls %programdata%\Microsoft\Crypto\RSA\MachineKeys /t /c > %temp%\MachineKeys_permissions_before.txt`
### <a name="save-file-permissions-to-acl-file"></a>Dosya izinleri ACL dosyaya kaydet
`icacls %programdata%\Microsoft\Crypto\RSA\MachineKeys /save %temp%\MachineKeys_permissions_before.aclfile /t`  
### <a name="restore-file-permissions-from-acl-file"></a>Dosya izinleri ACL dosyasından geri yükleme
`icacls %programdata%\Microsoft\Crypto\RSA /save %temp%\MachineKeys_permissions_before.aclfile /t`

Yolun kullanırken `/restore` kullanırken belirttiğiniz klasörün üst klasörü olması gereken `/save`. Bu örnekte, `\RSA` üst `\MachineKeys` belirtilen klasör `/save` Yukarıdaki örneği.
### <a name="take-ntfs-ownership-of-a-folder"></a>Bir klasörün NTFS sahipliği alın
`takeown /f %programdata%\Microsoft\Crypto\RSA\MachineKeys /a /r`  
### <a name="grant-ntfs-permissions-to-a-folder-recursively"></a>Yinelemeli olarak bir klasörden NTFS izinleri verme
`icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "BUILTIN\Administrators:(F)"`  
## <a name="manage-devices"></a>Cihazları yönetme
### <a name="remove-non-present-pnp-devices"></a>Mevcut olmayan PNP cihazları kaldırma
`%windir%\System32\RUNDLL32.exe %windir%\System32\pnpclean.dll,RunDLL_PnpClean /Devices /Maxclean`
## <a name="manage-group-policy"></a>Grup İlkesi yönetme
### <a name="force-group-policy-update"></a>Grup İlkesi güncelleştirmesini zorla
`gpupdate /force /wait:-1`
## <a name="miscellaneous-tasks"></a>Çeşitli görevleri
### <a name="show-os-version"></a>İşletim sistemi sürümü göster
`ver`

or 

`wmic os get caption,version,buildnumber /format:list`

or 

`systeminfo  find /i "os name"`

`systeminfo | findstr /i /r "os.*version.*build"`
### <a name="view-os-install-date"></a>Görünüm işletim sistemi yükleme tarihi
`systeminfo | find /i "original"`

or 

`wmic os get installdate`
### <a name="view-last-boot-time"></a>Görünümü son önyükleme zamanı
`systeminfo | find /i "system boot time"`
### <a name="view-time-zone"></a>Görünüm saat dilimi
`systeminfo | find /i "time zone"`

or

`wmic timezone get caption,standardname /format:list`
### <a name="restart-windows"></a>Windows yeniden başlatma
`shutdown /r /t 0`

Ekleme `/f` olmayan kullanıcılar uyarı kapatmak için çalışan uygulamaların zorlar.
### <a name="detect-safe-mode-boot"></a>Güvenli modda algılayın
`bcdedit /enum | find /i "safeboot"` 

# <a name="windows-commands---powershell"></a>Windows komutları - PowerShell

Bir komut istemi ulaştıktan sonra PowerShell SAC içinde çalıştırmak için şunu yazın:

`powershell <enter>`

> [!CAUTION]
> Diğer bir PowerShell komutları çalıştırmadan önce PowerShell oturumundan PSReadLine modülü kaldırın. Burada ek karakterler PSReadLine bir PowerShell oturumunda SAC içinde çalışıyorsa panodan yapıştırılan metin tanıtılmak bilinen bir sorun yoktur.

İlk PSReadLine yüklenip yüklenmediğini kontrol edin. Windows Server 2016, Windows 10 ve sonraki Windows sürümlerinde varsayılan olarak yüklenir. El ile yüklenmiş olan, yalnızca önceki Windows sürümlerinde mevcut olacaktır. 

Bu komut çıktı istemiyle döndürür, ardından modülü yüklenmedi ve PowerShell oturumunda SAC içinde normal olarak kullanmaya devam edebilirsiniz.

`get-module psreadline`

Yukarıdaki komutu PSReadLine Modül sürümü döndürürse, onu kaldırmak için aşağıdaki komutu çalıştırın. Bu komut silmeyin veya modülünü kaldırmak, yalnızca bu geçerli bir PowerShell oturumundan kaldırır.

`remove-module psreadline`

## <a name="view-and-edit-windows-registry-settings"></a>Windows kayıt defteri ayarlarını görüntüleyin ve düzenleyin
### <a name="verify-rdp-is-enabled"></a>RDP etkinleştirildiğini doğrulama
`get-itemproperty -path 'hklm:\system\curRentcontrolset\control\terminal server' -name 'fdenytsconNections'`

`get-itemproperty -path 'hklm:\software\policies\microsoft\windows nt\terminal services' -name 'fdenytsconNections'`

İkinci anahtarı (\Policies altında) ilgili Grup İlkesi ayarı yapılandırılmışsa, yalnızca sunulacaktır.
### <a name="enable-rdp"></a>RDP'yi etkinleştirin
`set-itemproperty -path 'hklm:\system\curRentcontrolset\control\terminal server' -name 'fdenytsconNections' 0 -type dword`

`set-itemproperty -path 'hklm:\software\policies\microsoft\windows nt\terminal services' -name 'fdenytsconNections' 0 -type dword`

İkinci anahtarı (altındaki \Policies) yalnızca ilgili Grup İlkesi ayarını yapılandırılmış gerekecek. Grup İlkesi'nde yapılandırılmışsa, değer sonraki Grup İlkesi yenileme sırasında yazılacaktır.
## <a name="manage-windows-services"></a>Windows hizmetlerini yönet
### <a name="view-service-details"></a>Hizmet ayrıntılarını görüntüle
`get-wmiobject win32_service -filter "name='termservice'" |  format-list Name,DisplayName,State,StartMode,StartName,PathName,ServiceType,Status,ExitCode,ServiceSpecificExitCode,ProcessId`

`Get-Service` kullanılabilir ancak hizmeti oturum açma hesabını içermez. `Get-WmiObject win32-service` yapar.
### <a name="set-service-logon-account"></a>Küme hizmeti oturum açma hesabı
`(get-wmiobject win32_service -filter "name='termservice'").Change($null,$null,$null,$null,$null,$false,'NT Authority\NetworkService')`

Bir hizmet hesabı dışındaki kullanırken `NT AUTHORITY\LocalService`, `NT AUTHORITY\NetworkService`, veya `LocalSystem`, hesap parolasını (sekizinci) son bağımsız değişken olarak sonra hesap adını belirtin.
### <a name="set-service-startup-type"></a>Hizmet başlatma türünü ayarlayın
`set-service termservice -startuptype Manual`

`Set-service` kabul `Automatic`, `Manual`, veya `Disabled` başlangıç türü.
### <a name="set-service-dependencies"></a>Hizmet bağımlıları ayarlayın
`Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Services\TermService' -Name DependOnService -Value @('RPCSS','TermDD')`
### <a name="start-service"></a>Hizmeti Başlat
`start-service termservice`
### <a name="stop-service"></a>Hizmeti Durdur
`stop-service termservice`
## <a name="manage-networking-features"></a>Ağ özelliklerini yönetme
### <a name="show-nic-properties"></a>NIC özellikleri göster
`get-netadapter | where {$_.ifdesc.startswith('Microsoft Hyper-V Network Adapter')} |  format-list status,name,ifdesc,macadDresS,driverversion,MediaConNectState,MediaDuplexState`

or 

`get-wmiobject win32_networkadapter -filter "servicename='netvsc'" |  format-list netenabled,name,macaddress`

`Get-NetAdapter` 2012 +, 2008R2 kullanmak için kullanılabilir `Get-WmiObject`.
### <a name="show-ip-properties"></a>IP özellikleri göster
`get-wmiobject Win32_NetworkAdapterConfiguration -filter "ServiceName='netvsc'" |  format-list DNSHostName,IPAddress,DHCPEnabled,IPSubnet,DefaultIPGateway,MACAddress,DHCPServer,DNSServerSearchOrder`
### <a name="enable-nic"></a>NIC etkinleştir
`get-netadapter | where {$_.ifdesc.startswith('Microsoft Hyper-V Network Adapter')} | enable-netadapter`

or

`(get-wmiobject win32_networkadapter -filter "servicename='netvsc'").enable()`

`Get-NetAdapter` 2012 +, 2008R2 kullanmak için kullanılabilir `Get-WmiObject`.
### <a name="set-nic-to-use-dhcp"></a>NIC DHCP kullanacak şekilde ayarlama
`get-netadapter | where {$_.ifdesc.startswith('Microsoft Hyper-V Network Adapter')} | Set-NetIPInterface -DHCP Enabled`

`(get-wmiobject Win32_NetworkAdapterConfiguration -filter "ServiceName='netvsc'").EnableDHCP()`

`Get-NetAdapter` 2012 + kullanıma sunulmuştur. Kullanmak için 2008R2 `Get-WmiObject`. Azure sanal makineler, her zaman konuk işletim sistemi, bir IP adresini almak için DHCP kullanmak üzere yapılandırılmalıdır. Azure statik IP ayarı yine de VM IP vermek için DHCP kullanır.
### <a name="ping"></a>Ping
`test-netconnection`

or

`get-wmiobject Win32_PingStatus -Filter 'Address="8.8.8.8"' | format-table -autosize IPV4Address,ReplySize,ResponseTime`

`Test-Netconnection` herhangi bir parametre ping dener olmadan `internetbeacon.msedge.net`. 2012 + üzerinde kullanılabilir. Kullanmak için 2008R2 `Get-WmiObject` ikinci örnekte olduğu gibi.
### <a name="port-ping"></a>Bağlantı noktası Ping
`test-netconnection -ComputerName bing.com -Port 80`

or

`(new-object Net.Sockets.TcpClient).BeginConnect('bing.com','80',$null,$null).AsyncWaitHandle.WaitOne(300)`

`Test-NetConnection` 2012 + kullanıma sunulmuştur. 2008R2 kullanmak için `Net.Sockets.TcpClient`
### <a name="test-dns-name-resolution"></a>DNS ad çözümlemesini test
`resolve-dnsname bing.com` 

or 

`[System.Net.Dns]::GetHostAddresses('bing.com')`

`Resolve-DnsName` 2012 + kullanıma sunulmuştur. Kullanmak için 2008R2 `System.Net.DNS`.
### <a name="show-windows-firewall-rule-by-name"></a>Windows Güvenlik duvarı kuralı adına göre göster
`get-netfirewallrule -name RemoteDesktop-UserMode-In-TCP` 
### <a name="show-windows-firewall-rule-by-port"></a>Windows Güvenlik duvarı kuralı tarafından bağlantı noktası Göster
`get-netfirewallportfilter | where {$_.localport -eq 3389} | foreach {Get-NetFirewallRule -Name $_.InstanceId} | format-list Name,Enabled,Profile,Direction,Action`

or

`(new-object -ComObject hnetcfg.fwpolicy2).rules | where {$_.localports -eq 3389 -and $_.direction -eq 1} | format-table Name,Enabled`

`Get-NetFirewallPortFilter` 2012 + kullanıma sunulmuştur. Kullanmak için 2008R2 `hnetcfg.fwpolicy2` COM nesnesi. 
### <a name="disable-windows-firewall"></a>Windows Güvenlik Duvarı'nı devre dışı bırak
`Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False`

`Set-NetFirewallProfile` 2012 + kullanıma sunulmuştur. Kullanmak için 2008R2 `netsh advfirewall` yukarıdaki CMD bölümünde başvurulan.
## <a name="manage-users-and-groups"></a>Kullanıcıları ve grupları yönetme
### <a name="create-local-user-account"></a>Yerel kullanıcı hesabı oluşturma
`new-localuser <name>`
### <a name="verify-user-account-is-enabled"></a>Kullanıcı hesabı etkinleştirildiğini doğrulama
`(get-localuser | where {$_.SID -like "S-1-5-21-*-500"}).Enabled`

or 

`(get-wmiobject Win32_UserAccount -Namespace "root\cimv2" -Filter "SID like 'S-1-5-%-500'").Disabled`

`Get-LocalUser` 2012 + kullanıma sunulmuştur. Kullanmak için 2008R2 `Get-WmiObject`. Bu örnek, her zaman SID'ye sahip olduğu yerleşik yerel yönetici hesabı gösterir `S-1-5-21-*-500`. Genelleştirilmiş bir görüntüden oluşturulan azure VM'ler VM sağlama sırasında belirtilen adı olarak yeniden adlandırıldı yerel yönetici hesabı olacaktır. Genellikle olmayacaktır şekilde `Administrator`.
### <a name="add-local-user-to-local-group"></a>Yerel kullanıcı yerel gruba ekleyin.
`add-localgroupmember -group Administrators -member <username>`
### <a name="enable-local-user-account"></a>Yerel kullanıcı hesabı etkinleştir
`get-localuser | where {$_.SID -like "S-1-5-21-*-500"} | enable-localuser` 

Bu örnekte her zaman SID'ye sahip olduğu yerleşik yerel yönetici hesabını etkinleştirir `S-1-5-21-*-500`. Genelleştirilmiş bir görüntüden oluşturulan azure VM'ler VM sağlama sırasında belirtilen adı olarak yeniden adlandırıldı yerel yönetici hesabı olacaktır. Genellikle olmayacaktır şekilde `Administrator`.
### <a name="view-user-account-properties"></a>Kullanıcı hesabı özellikleri görüntüle
`get-localuser | where {$_.SID -like "S-1-5-21-*-500"} | format-list *`

or 

`get-wmiobject Win32_UserAccount -Namespace "root\cimv2" -Filter "SID like 'S-1-5-%-500'" |  format-list Name,Disabled,Status,Lockout,Description,SID`

`Get-LocalUser` 2012 + kullanıma sunulmuştur. Kullanmak için 2008R2 `Get-WmiObject`. Bu örnek, her zaman SID'ye sahip olduğu yerleşik yerel yönetici hesabı gösterir `S-1-5-21-*-500`.
### <a name="view-local-groups"></a>Yerel grupları görüntüleyin
`(get-localgroup).name | sort``(get-wmiobject win32_group).Name | sort`

`Get-LocalUser` 2012 + kullanıma sunulmuştur. Kullanmak için 2008R2 `Get-WmiObject`.
## <a name="manage-the-windows-event-log"></a>Windows olay günlüğünü yönetme
### <a name="query-event-log-errors"></a>Olay günlüğü hatalarını sorgulama
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Level=2]]" | more`

Değişiklik `/c:10` olayları döndürür veya filtreyle eşleşen tüm olayları döndürülecek taşımak için istenen sayıda.
### <a name="query-event-log-by-event-id"></a>Olay günlüğü olay Kimliğine göre sorgulama
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[EventID=11]]" | more`
### <a name="query-event-log-by-event-id-and-provider"></a>Olay günlüğüne olay kimliği ve sağlayıcı göre sorgulama
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11]]" | more`
### <a name="query-event-log-by-event-id-and-provider-for-the-last-24-hours"></a>Son 24 saat için olay günlüğüne olay kimliği ve sağlayıcı göre sorgulama
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11 and TimeCreated[timediff(@SystemTime) <= 86400000]]]"`

Kullanım `604800000` geri 7 gün 24 saat yerine aramak için. |
### <a name="query-event-log-by-event-id-provider-and-eventdata-in-the-last-7-days"></a>Sorgu olay günlüğünde olay kimliği, sağlayıcı ve EventData tarafından son 7 gün
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Provider[@Name='Microsoft-Windows-Security-Auditing'] and EventID=4624 and TimeCreated[timediff(@SystemTime) <= 604800000]] and EventData[Data[@Name='TargetUserName']='<username>']]" | more`
## <a name="view-or-remove-installed-applications"></a>Görüntüleme veya yüklü uygulamaları kaldırma
### <a name="list-installed-software"></a>Liste yüklü yazılım
`get-wmiobject win32_product | select installdate,name | sort installdate -descending | more`
### <a name="uninstall-software"></a>Yazılım kaldırma
`(get-wmiobject win32_product -filter "Name='<name>'").Uninstall()`
## <a name="file-system-management"></a>Dosya sistemi yönetimi
### <a name="get-file-version"></a>Dosya sürümü Al
`(get-childitem $env:windir\system32\drivers\netvsc*.sys).VersionInfo.FileVersion`

Bu örnekte netvsc.sys, netvsc63.sys veya netvsc60.sys Windows sürümüne bağlı olarak adlandırılan sanal NIC sürücü dosyası sürümünü döndürür.
### <a name="download-and-extract-file"></a>İndirin ve dosyayı ayıklayın
`$path='c:\bin';md $path;cd $path;(new-object net.webclient).downloadfile( ('htTp:/'+'/download.sysinternals.com/files/SysinternalsSuite.zip'),"$path\SysinternalsSuite.zip");(new-object -com shelL.apPlication).namespace($path).CopyHere( (new-object -com shelL.apPlication).namespace("$path\SysinternalsSuite.zip").Items(),16)`

Bu örnekte bir `c:\bin` klasörü, ardından indirir ve araçlarla Sysinternals paketini ayıklar `c:\bin`.
## <a name="miscellaneous-tasks"></a>Çeşitli görevleri
### <a name="show-os-version"></a>İşletim sistemi sürümü göster
`get-wmiobject win32_operatingsystem | format-list caption,version,buildnumber` 
### <a name="view-os-install-date"></a>Görünüm işletim sistemi yükleme tarihi
`(get-wmiobject win32_operatingsystem).converttodatetime((get-wmiobject win32_operatingsystem).installdate)`
### <a name="view-last-boot-time"></a>Görünümü son önyükleme zamanı
`(get-wmiobject win32_operatingsystem).lastbootuptime`
### <a name="view-windows-uptime"></a>Windows çalışma zamanı görüntüleme
`"{0:dd}:{0:hh}:{0:mm}:{0:ss}.{0:ff}" -f ((get-date)-(get-wmiobject win32_operatingsystem).converttodatetime((get-wmiobject win32_operatingsystem).lastbootuptime))`

Çalışma süresi olarak döndürür `<days>:<hours>:<minutes>:<seconds>:<milliseconds>`, örneğin `49:16:48:00.00`. 
### <a name="restart-windows"></a>Windows yeniden başlatma
`restart-computer`

Ekleme `-force` olmayan kullanıcılar uyarı kapatmak için çalışan uygulamaların zorlar.
## <a name="instance-metadata"></a>Örnek meta veri

OsType, konum, vmSize, VM kimliği, adı, resourceGroupName, Subscriptionıd, Privateıpaddress ve Publicıpaddress gibi ayrıntılarını görüntülemek için Azure VM içinde Azure örnek meta verileri sorgulayabilirsiniz.

Örnek meta veri sorgulama, örnek meta veri hizmeti bir Azure konağına üzerinden REST çağrısı yapacağı için sağlıklı Konuk ağ bağlantısı gerektirir. Örnek meta verileri sorgulayabilir, bu nedenle, Konuk Azure'da barındırılan bir hizmet ağ üzerinden iletişim kurabilir olduğunu bildirir.

Daha fazla bilgi için [Azure örnek meta veri hizmetine](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service).

### <a name="instance-metadata"></a>Örnek meta veri
`$im = invoke-restmethod -headers @{"metadata"="true"} -uri http://169.254.169.254/metadata/instance?api-version=2017-08-01 -method get`

`$im | convertto-json`
### <a name="os-type-instance-metadata"></a>İşletim sistemi türünü (meta verileri örneği)
`$im.Compute.osType`
### <a name="location-instance-metadata"></a>Konum (meta verileri örneği)
`$im.Compute.Location`
### <a name="size-instance-metadata"></a>Boyut (meta verileri örneği)
`$im.Compute.vmSize`
### <a name="vm-id-instance-metadata"></a>VM kimliği (meta verileri örneği)
`$im.Compute.vmId`
### <a name="vm-name-instance-metadata"></a>VM adı (örnek meta veri)
`$im.Compute.name`
### <a name="resource-group-name-instance-metadata"></a>Kaynak grubu adı (örnek meta veri)
`$im.Compute.resourceGroupName`
### <a name="subscription-id-instance-metadata"></a>Abonelik kimliği (meta verileri örneği)
`$im.Compute.subscriptionId`
### <a name="tags-instance-metadata"></a>Etiketleri (meta verileri örneği)
`$im.Compute.tags`
### <a name="placement-group-id-instance-metadata"></a>Yerleştirme grubu kimliği (meta verileri örneği)
`$im.Compute.placementGroupId`
### <a name="platform-fault-domain-instance-metadata"></a>Platform hata etki alanı (meta verileri örneği)
`$im.Compute.platformFaultDomain`
### <a name="platform-update-domain-instance-metadata"></a>Platform güncelleştirme etki alanı (meta verileri örneği)
`$im.Compute.platformUpdateDomain`
### <a name="ipv4-private-ip-address-instance-metadata"></a>IPv4 özel IP adresi (örnek meta veri)
`$im.network.interface.ipv4.ipAddress.privateIpAddress`
### <a name="ipv4-public-ip-address-instance-metadata"></a>IPv4 genel IP adresi (örnek meta veri)
`$im.network.interface.ipv4.ipAddress.publicIpAddress`
### <a name="ipv4-subnet-address--prefix-instance-metadata"></a>IPv4 alt ağ adresi / önek (meta verileri örneği)
`$im.network.interface.ipv4.subnet.address`

`$im.network.interface.ipv4.subnet.prefix`
### <a name="ipv6-ip-address-instance-metadata"></a>IPv6 IP adresi (örnek meta veri)
`$im.network.interface.ipv6.ipAddress`
### <a name="mac-address-instance-metadata"></a>MAC adresi (örnek meta veri)
`$im.network.interface.macAddress`

## <a name="next-steps"></a>Sonraki adımlar
* Ana seri konsol Windows belgeleri sayfasının bulunduğu [burada](serial-console-windows.md).
* Seri konsol için de kullanılabilir olan [Linux](serial-console-linux.md) VM'ler.
* Daha fazla bilgi edinin [önyükleme tanılaması](boot-diagnostics.md).
