---
title: Konuk işletim sistemi güvenlik duvarı Azure VM'de devre dışı bırakma | Microsoft Docs
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
ms.openlocfilehash: a8856bd46f516aa3c64965648d4f23b9ba665b1b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60505470"
---
# <a name="disable-the-guest-os-firewall-in-azure-vm"></a>Azure VM'de konuk işletim sistemi Güvenlik Duvarını devre dışı bırakma

Bu makalede, konuk işletim sistemi güvenlik duvarı kısmi veya tam bir sanal makine (VM) trafiği filtreleme olduğunu düşündüğünüz durumlar için bir başvuru sağlar. RDP bağlantıları başarısız olmasına neden olan bir güvenlik duvarı için kasıtlı olarak değişiklik yapıldıysa bu durum oluşabilir.

## <a name="solution"></a>Çözüm

Bu makalede açıklanan işlem, doğru güvenlik duvarı kurallarını ayarlama, gerçek sorunu düzeltmeye odaklanabilmeniz için geçici bir çözüm olarak kullanılmak üzere tasarlanmıştır. It\rquote s etkin Windows Güvenlik Duvarı bileşen için Microsoft en iyi uygulama. Gerekli VM that\rquote s erişim düzeyi güvenlik duvarı kuralları \cf3 nasıl yapılandırdığınıza bağlıdır.

### <a name="online-solutions"></a>Çevrimiçi çözümler 

Sanal makine çevrimiçi olduğundan ve aynı sanal ağdaki başka bir sanal Makineye erişilebildiğinden ve diğer sanal makine kullanarak bu risk azaltma işlemleri yapabilirsiniz.

#### <a name="mitigation-1-custom-script-extension-or-run-command-feature"></a>1. azaltma: Özel betik uzantısı veya Çalıştır komutu özelliği

Çalışan bir Azure aracısı varsa, kullanabileceğiniz [özel betik uzantısı](../extensions/custom-script-windows.md) veya [komutlarını Çalıştır](../windows/run-command.md) uzaktan aşağıdaki betikler çalıştırmak için (yalnızca Resource Manager Vm'lerinde) özelliği.

> [!Note]
> * Güvenlik Duvarı'nı yerel olarak ayarlarsanız, aşağıdaki betiği çalıştırın:
>   ```
>   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\DomainProfile' -name "EnableFirewall" -Value 0
>   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\PublicProfile' -name "EnableFirewall" -Value 0
>   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\Standardprofile' -name "EnableFirewall" -Value 0 
>   Restart-Service -Name mpssvc
>   ```
> * Güvenlik Duvarı bir Active Directory ilkesi ayarlarsanız, kullanabileceğiniz geçici erişim için aşağıdaki betiği çalıştırın. 
>   ```
>   Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\WindowsFirewall\DomainProfile' -name "EnableFirewall" -Value 0
>   Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\WindowsFirewall\PublicProfile' -name "EnableFirewall" -Value 0
>   Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\WindowsFirewall\StandardProfile' name "EnableFirewall" -Value 0
>   Restart-Service -Name mpssvc
>   ```
>   İlkeyi yeniden uygulandığı hemen sonra ancak, uzak oturumu dışında devreye girdi. Bu sorun için kalıcı bir düzeltme bu bilgisayara uygulandığında ilkesinin değiştirilmesidir.

#### <a name="mitigation-2-remote-powershell"></a>Azaltma 2: Uzak PowerShell

1.  RDP bağlantısını kullanarak ulaşamıyor VM ile aynı sanal ağda bulunan bir VM'ye bağlanın.

2.  Bir PowerShell konsol penceresi açın.

3.  Aşağıdaki komutları çalıştırın:

    ```powershell
    Enter-PSSession (New-PSSession -ComputerName "<HOSTNAME>" -Credential (Get-Credential) -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck)) 
    netsh advfirewall set allprofiles state off
    Restart-Service -Name mpssvc 
    exit
    ```

> [!Note]
> Güvenlik Duvarı bir Grup İlkesi nesnesi olarak ayarlanırsa, bu komut yalnızca yerel kayıt defteri girişlerini değiştiğinden bu yöntemin çalışmayabilir. Bir ilke yerinde olduğundan bu değişikliği geçersiz kılar. 

#### <a name="mitigation-3-pstools-commands"></a>3. azaltma: PSTools komutları

1.  Sorun giderme sanal makinede indirme [PSTools](https://docs.microsoft.com/sysinternals/downloads/pstools).

2.  CMD örneği açın ve sanal Makinenin kendi DIP erişin.

3.  Aşağıdaki komutları çalıştırın:

    ```cmd
    psexec \\<DIP> -u <username> cmd
    netsh advfirewall set allprofiles state off
    psservice restart mpssvc
    ```

#### <a name="mitigation-4-remote-registry"></a>Azaltma 4: Uzak Kayıt defteri 

Bu adımları izleyin [uzak kayıt defteri](https://support.microsoft.com/help/314837/how-to-manage-remote-access-to-the-registry).

1.  Sorun giderme sanal kayıt defteri düzenleyicisini başlatın ve ardından Git **dosya** > **bağlanmak ağ kayıt defteri**.

2.  Açık yukarı *hedef makine*\SYSTEM dallandırma ve aşağıdaki değerleri belirtin:

    ```
    <TARGET MACHINE>\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\DomainProfile\EnableFirewall           -->        0 
    <TARGET MACHINE>\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\PublicProfile\EnableFirewall           -->        0 
    <TARGET MACHINE>\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\StandardProfile\EnableFirewall         -->        0
    ```

3.  Hizmeti yeniden başlatın. Uzak Kayıt defterini kullanarak bunu yapamazsınız çünkü kaldırma Hizmeti konsolunu kullanmanız gerekir.

4.  Bir örneği açın **Services.msc**.

5.  Tıklayın **hizmetler (yerel)**.

6.  Seçin **başka bir bilgisayara bağlan**.

7.  Girin **özel IP adresi (DIP)** sorunun VM.

8.  Yerel Güvenlik Duvarı ilkesini yeniden başlatın.

9.  Yeniden kullanarak yerel bilgisayarınızdan VM'ye RDP aracılığıyla bağlanmak bu seçeneği deneyin.

### <a name="offline-solutions"></a>Çevrimdışı çözümleri 

İçinde sanal makine herhangi bir yöntemle ulaşamıyor bir durum varsa, özel betik uzantısı başarısız olur ve sistem diski aracılığıyla doğrudan çevrimdışı modda çalışmak gerekir. Bunu yapmak için şu adımları uygulayın:

1.  [Bir kurtarma VM'si sistem diski](troubleshoot-recovery-disks-portal-windows.md).

2.  Kurtarma VM'sini bir Uzak Masaüstü Bağlantısı'nı başlatın.

3.  Disk Disk Yönetimi konsolunda çevrimiçi işaretlenmiş olduğundan emin olun. Bağlı sistem diskine atanmış sürücü harfini unutmayın.

4.  Herhangi bir değişiklik yapmadan önce tüm değişiklikleri geri alma gerekli olması durumunda \windows\system32\config klasörüne bir kopyasını oluşturun.

5.  Kayıt Defteri Düzenleyicisi (regedit.exe) sorun giderme sanal makinede başlatın. 

6.  Bu sorun giderme yordamını için biz BROKENSYSTEM ve BROKENSOFTWARE yığınlarını bağlama.

7.  HKEY_LOCAL_MACHINE anahtarı vurgulayın ve sonra dosyayı seçin > yığını menüsünde.

8.  Bağlı sistem diskinde \windows\system32\config\SYSTEM dosyasını bulun.

9.  Yükseltilmiş bir PowerShell örneği açın ve ardından aşağıdaki komutları çalıştırın:

    ```cmd
    # Load the hives - If your attached disk is not F, replace the letter assignment here
    reg load HKLM\BROKENSYSTEM f:\windows\system32\config\SYSTEM
    reg load HKLM\BROKENSOFTWARE f:\windows\system32\config\SOFTWARE 
    # Disable the firewall on the local policy
    $ControlSet = (get-ItemProperty -Path 'HKLM:\BROKENSYSTEM\Select' -name "Current").Current
    $key = 'BROKENSYSTEM\ControlSet00'+$ControlSet+'\services\SharedAccess\Parameters\FirewallPolicy\DomainProfile'
    Set-ItemProperty -Path $key -name 'EnableFirewall' -Value 0 -Type Dword -force
    $key = 'BROKENSYSTEM\ControlSet00'+$ControlSet+'\services\SharedAccess\Parameters\FirewallPolicy\PublicProfile'
    Set-ItemProperty -Path $key -name 'EnableFirewall' -Value 0 -Type Dword -force
    $key = 'BROKENSYSTEM\ControlSet00'+$ControlSet+'\services\SharedAccess\Parameters\FirewallPolicy\StandardProfile'
    Set-ItemProperty -Path $key -name 'EnableFirewall' -Value 0 -Type Dword -force
    # To ensure the firewall is not set thru AD policy, check if the following registry entries exist and if they do, then check if the following entries exist:
    $key = 'HKLM:\BROKENSOFTWARE\Policies\Microsoft\WindowsFirewall\DomainProfile'
    Set-ItemProperty -Path $key -name 'EnableFirewall' -Value 0 -Type Dword -force
    $key = 'HKLM:\BROKENSOFTWARE\Policies\Microsoft\WindowsFirewall\PublicProfile'
    Set-ItemProperty -Path $key -name 'EnableFirewall' -Value 0 -Type Dword -force
    $key = 'HKLM:\BROKENSOFTWARE\Policies\Microsoft\WindowsFirewall\StandardProfile'
    Set-ItemProperty -Path $key -name 'EnableFirewall' -Value 0 -Type Dword -force
    # Unload the hives
    reg unload HKLM\BROKENSYSTEM
    reg unload HKLM\BROKENSOFTWARE
    ```

10. [VM yeniden oluşturma ve sistem diskini](troubleshoot-recovery-disks-portal-windows.md).

11. Sorunun çözülüp çözülmediğini denetleyin.
