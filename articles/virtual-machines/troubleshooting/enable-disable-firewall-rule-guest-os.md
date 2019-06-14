---
title: Etkinleştirmek veya devre dışı bir güvenlik duvarı kuralı Azure VM'de bir konuk işletim sistemi | Microsoft Docs
description: ''
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: willchen
editor: ''
tags: ''
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.date: 11/22/2018
ms.author: delhan
ms.openlocfilehash: ed3d89bc15f960947a48ac4364bd14f3fdf50cc2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60505582"
---
# <a name="enable-or-disable-a-firewall-rule-on-an-azure-vm-guest-os"></a>Etkinleştirmek veya bir Azure VM konuk işletim sisteminde bir güvenlik duvarı kuralı devre dışı bırak

Bu makalede, konuk işletim sistemi Güvenlik Duvarı'nı bir sanal makine'de (VM) kısmi trafik filtreleme olduğunu düşündüğünüz bir durum sorun giderme için bir başvuru sağlar. Bu aşağıdaki nedenlerden dolayı yararlı olabilir:

*   RDP bağlantıları başarısız olmasına neden olan bir güvenlik duvarı için kasıtlı olarak bir değişiklik yapıldıysa özel betik uzantısı özelliğini kullanarak sorunu çözebilir.

*   Tüm güvenlik duvarı profilleri devre dışı bırakma, RDP özel güvenlik duvarı kuralının ayarlanması daha sorun giderme daha iyi bir yoludur.

## <a name="solution"></a>Çözüm

Güvenlik duvarı kurallarını nasıl yapılandırabileceğinizi gereken sanal makine erişimine izin düzeyine bağlıdır. Aşağıdaki örnekler, RDP kurallarını kullanın. Ancak, doğru kayıt defteri anahtarına işaret ederek aynı yöntemleri, trafiği herhangi başka bir tür için'e uygulanabilir.

### <a name="online-troubleshooting"></a>Çevrimiçi sorun giderme 

#### <a name="mitigation-1-custom-script-extension"></a>1\. azaltma: Özel Betik Uzantısı

1.  Aşağıdaki şablonu kullanarak, komut dosyası oluşturun.

    *   Bir kuralı etkinleştirmek için:
        ```cmd
        netsh advfirewall firewall set rule dir=in name="Remote Desktop - User Mode (TCP-In)" new enable=yes
        ```

    *   Bir kural devre dışı bırakmak için:
        ```cmd
        netsh advfirewall firewall set rule dir=in name="Remote Desktop - User Mode (TCP-In)" new enable=no
        ```

2.  Bu betik, Azure portal kullanarak karşıya [özel betik uzantısı](../extensions/custom-script-windows.md) özelliği. 

#### <a name="mitigation-2-remote-powershell"></a>Azaltma 2: Uzak PowerShell

Sanal makine çevrimiçi olduğundan ve aynı sanal ağdaki başka bir sanal Makineye erişilebildiğinden ve diğer sanal makine kullanarak izleyin risk azaltma işlemleri yapabilirsiniz.

1.  Sorun giderme sanal makinede bir PowerShell konsol penceresi açın.

2.  Aşağıdaki komutları çalıştırın.

    *   Bir kuralı etkinleştirmek için:
        ```powershell
        Enter-PSSession (New-PSSession -ComputerName "<HOSTNAME>" -Credential (Get-Credential) -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck)) 
        Enable-NetFirewallRule -DisplayName  "RemoteDesktop-UserMode-In-TCP"
        exit
        ```

    *   Bir kural devre dışı bırakmak için:
        ```powershell
        Enter-PSSession (New-PSSession -ComputerName "<HOSTNAME>" -Credential (Get-Credential) -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck)) 
        Disable-NetFirewallRule -DisplayName  "RemoteDesktop-UserMode-In-TCP"
        exit
        ```

#### <a name="mitigation-3-pstools-commands"></a>3\. azaltma: PSTools komutları

Sanal makine çevrimiçi olduğundan ve aynı sanal ağdaki başka bir sanal Makineye erişilebildiğinden ve diğer sanal makine kullanarak izleyin risk azaltma işlemleri yapabilirsiniz.

1.  Sorun giderme sanal makinede indirme [PSTools](https://docs.microsoft.com/sysinternals/downloads/pstools).

2.  CMD örneği açın ve VM'nin, iç IP (DIP aracılığıyla) erişim. 

    * Bir kuralı etkinleştirmek için:
        ```cmd
        psexec \\<DIP> -u <username> cmd
        netsh advfirewall firewall set rule dir=in name="Remote Desktop - User Mode (TCP-In)" new enable=yes
        ```

    *   Bir kural devre dışı bırakmak için:
        ```cmd
        psexec \\<DIP> -u <username> cmd
        netsh advfirewall firewall set rule dir=in name="Remote Desktop - User Mode (TCP-In)" new enable=no
        ```

#### <a name="mitigation-4-remote-registry"></a>Azaltma 4: Uzak Kayıt defteri

Sanal makine çevrimiçi olduğundan ve aynı sanal ağdaki başka bir VM üzerinde erişilebilir, kullanabileceğiniz [uzak kayıt defteri](https://support.microsoft.com/help/314837/how-to-manage-remote-access-to-the-registry) diğer VM üzerinde.

1.  Sorun giderme sanal Kayıt Defteri Düzenleyicisi'ni (regedit.exe) başlatın ve ardından **dosya** > **bağlanmak ağ kayıt defteri**.

2.  Açık *hedef makine*\SYSTEM dallandırma ve ardından aşağıdaki değerleri belirtin:

    * Bir kuralı etkinleştirmek için aşağıdaki kayıt defteri değerini açın:
    
        *TARGET MACHINE*\SYSTEM\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules\RemoteDesktop-UserMode-In-TCP
    
        Ardından **etkin = FALSE** için **etkin = TRUE** dizesinde:

        **v2.22 | Eylem = izin | Etkin = TRUE | Dizini içinde = | Protokol 6 = | Profile = Domain | Profili özel = | Profil ortak = | LPort 3389 = | App=%SystemRoot%\System32\svchost.exe| SVC termservice = | Adı =\@FirewallAPI.dll,-28775 | Desc =\@FirewallAPI.dll,-28756 | EmbedCtxt =\@FirewallAPI.dll,-28752 |**
    
    * Bir kural devre dışı bırakmak için aşağıdaki kayıt defteri değerini açın:
    
        *TARGET MACHINE*\SYSTEM\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules\RemoteDesktop-UserMode-In-TCP

        Ardından **etkin = TRUE** için **etkin = FALSE**:
        
        **v2.22 | Eylem = izin | Etkin = FALSE | Dizini içinde = | Protokol 6 = | Profile = Domain | Profili özel = | Profil ortak = | LPort 3389 = | App=%SystemRoot%\System32\svchost.exe| SVC termservice = | Adı =\@FirewallAPI.dll,-28775 | Desc =\@FirewallAPI.dll,-28756 | EmbedCtxt =\@FirewallAPI.dll,-28752 |**

3.  Değişiklikleri uygulamak için VM'yi yeniden başlatın.

### <a name="offline-troubleshooting"></a>Çevrimdışı sorunlarını giderme 

Herhangi bir yöntemle VM erişemiyorsanız, özel betik uzantısı kullanarak başarısız olur ve sistem diski aracılığıyla doğrudan çevrimdışı modda çalışmak gerekir.

Bu adımları gerçekleştirmeden önce sistem diski etkilenen sanal makinenin anlık görüntüsünü yedekleyin. Daha fazla bilgi için [bir diskin anlık görüntüsünü alma](../windows/snapshot-copy-managed-disk.md).

1.  [Bir kurtarma VM'si sistem diski](troubleshoot-recovery-disks-portal-windows.md).

2.  Kurtarma VM'sini bir Uzak Masaüstü Bağlantısı'nı başlatın.

3.  Disk olarak işaretlenmiş olduğundan emin olun **çevrimiçi** Disk Yönetimi Konsolu'nda. Sürücü harfi not eklenmiş sistem diskine atanır.

4.  Herhangi bir değişiklik yapmadan önce tüm değişiklikleri geri alma gerekli olması durumunda \windows\system32\config klasörüne bir kopyasını oluşturun.

5.  Kayıt Defteri Düzenleyicisi'ni (regedit.exe) sorun giderme sanal makinede başlatın.

6.  Vurgulama **HKEY_LOCAL_MACHINE** anahtar ve ardından **dosya** > **yığını** menüsünde.

    ![Regedit](./media/enable-or-disable-firewall-rule-guest-os/load-registry-hive.png)

7.  Bulun ve ardından \windows\system32\config\SYSTEM dosyasını açın. 

    > [!Note]
    > İçin bir ad istenir. Girin **BROKENSYSTEM**ve ardından **HKEY_LOCAL_MACHINE**. Şimdi adlı ek bir anahtar göreceksiniz **BROKENSYSTEM**. Bu sorun giderme için Biz bu sorun yığınlarını olarak bağlama **BROKENSYSTEM**.

8.  BROKENSYSTEM dalda aşağıdaki değişiklikleri yapın:

    1.  Hangi denetleyin **ControlSet** VM başlatılmasını kayıt defteri anahtarı. HKLM\BROKENSYSTEM\Select\Current anahtar numarasını görürsünüz.

    2.  Bir kuralı etkinleştirmek için aşağıdaki kayıt defteri değerini açın:
    
        HKLM\BROKENSYSTEM\ControlSet00X\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules\RemoteDesktop-UserMode-In-TCP
        
        Ardından **etkin = FALSE** için **etkin = True**.
        
        **v2.22 | Eylem = izin | Etkin = TRUE | Dizini içinde = | Protokol 6 = | Profile = Domain | Profili özel = | Profil ortak = | LPort 3389 = | App=%SystemRoot%\System32\svchost.exe| SVC termservice = | Adı =\@FirewallAPI.dll,-28775 | Desc =\@FirewallAPI.dll,-28756 | EmbedCtxt =\@FirewallAPI.dll,-28752 |**

    3.  Bir kural devre dışı bırakmak için aşağıdaki kayıt defteri anahtarı açın:

        HKLM\BROKENSYSTEM\ControlSet00X\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules\RemoteDesktop-UserMode-In-TCP

        Ardından **etkin = True** için **etkin = FALSE**.
        
        **v2.22 | Eylem = izin | Etkin = FALSE | Dizini içinde = | Protokol 6 = | Profile = Domain | Profili özel = | Profil ortak = | LPort 3389 = | App=%SystemRoot%\System32\svchost.exe| SVC termservice = | Adı =\@FirewallAPI.dll,-28775 | Desc =\@FirewallAPI.dll,-28756 | EmbedCtxt =\@FirewallAPI.dll,-28752 |**

9.  Vurgulama **BROKENSYSTEM**ve ardından **dosya** > **yığın** menüsünde.

10. [VM yeniden oluşturma ve sistem diskini](troubleshoot-recovery-disks-portal-windows.md).

11. Sorunun çözülüp çözülmediğini denetleyin.
