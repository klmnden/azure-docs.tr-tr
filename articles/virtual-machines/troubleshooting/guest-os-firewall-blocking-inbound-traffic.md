---
title: Azure VM konuk işletim sistemi güvenlik duvarı gelen trafiği engelleme | Microsoft Docs
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
ms.openlocfilehash: 0a0da446385c592bfeda2e01e209ef1fb75b7de3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60711586"
---
# <a name="azure-vm-guest-os-firewall-is-blocking-inbound-traffic"></a>Azure VM konuk işletim sistemi güvenlik duvarı gelen trafiği engelliyor

Bu makalede, konuk işletim sistemi güvenlik duvarı blokları gelen trafik oluşan Uzak Masaüstü Portal (RDP) sorunun nasıl çözüleceğini açıklar.

## <a name="symptoms"></a>Belirtiler

RDP bağlantısı, bir Azure sanal makinesine (VM) bağlamak için kullanamazsınız. Önyüklemesinden Tanılama -> ekran görüntüsü, işletim sisteminin tam Hoş Geldiniz ekranında (Ctrl + Alt + Del) yüklü olduğunu gösterir.

## <a name="cause"></a>Nedeni

### <a name="cause-1"></a>Neden 1

RDP kuralının RDP trafiğine izin verecek şekilde ayarlanır değil.

### <a name="cause-2"></a>Neden 2

Konuk sistemi Güvenlik Duvarı profilleri, RDP trafiği de dahil olmak üzere tüm gelen bağlantıları engelleyecek şekilde ayarlanmıştır.

![Güvenlik Duvarı ayarı](./media/guest-os-firewall-blocking-inbound-traffic/firewall-advanced-setting.png)

## <a name="solution"></a>Çözüm

Bu adımları gerçekleştirmeden önce sistem diski etkilenen sanal makinenin anlık görüntüsünü yedekleyin. Daha fazla bilgi için [bir diskin anlık görüntüsünü alma](../windows/snapshot-copy-managed-disk.md).

Bu sorunu düzeltmek için metotlarından birini kullanın: [Azure VM sorunlarını gidermek için Uzak Araçlar'ı kullanma](remote-tools-troubleshoot-azure-vm-issues.md) VM'ye uzaktan bağlanın ve ardından konuk işletim sistemi güvenlik duvarı kurallarına **izin** RDP trafiği .

### <a name="online-troubleshooting"></a>Çevrimiçi sorun giderme

Bağlanma [seri konsolu ve bir PowerShell örneği açın](serial-console-windows.md#use-cmd-or-powershell-in-serial-console). Seri konsol VM üzerinde etkin değilse, Git "[VM çevrimdışı onarım](troubleshoot-rdp-internal-error.md#repair-the-vm-offline).

#### <a name="mitigation-1"></a>1 risk azaltma

1.  Azure aracısı yüklü olduğundan ve doğru VM üzerinde çalışan, altında "Yalnızca sıfırlama yapılandırması" seçeneğini kullanabilirsiniz, **destek + sorun giderme** > **parolayı Sıfırla** VM menüsünde.

2.  Bu kurtarma seçeneğini çalıştıran şunları yapar:

    *   Devre dışı bir RDP bileşeni sağlar.

    *   Tüm Windows Güvenlik duvarı profillerini sağlar.

    *   RDP kuralının Windows Güvenlik duvarının açık olduğundan emin olun.

    *   Önceki adımlar işe yaramazsa, güvenlik duvarı kuralını el ile sıfırlayın. Bunu yapmak için aşağıdaki komutu çalıştırarak ad "Uzak Masaüstü" içeren tüm kurallar sorgu:

        ```cmd
        netsh advfirewall firewall show rule dir=in name=all | select-string -pattern "(Name.*Remote Desktop)" -context 9,4 | more
        ```

        RDP bağlantı noktası 3389 dışında'diğer tüm bağlantı noktası ayarlandıysa, oluşturulur ve bu bağlantı noktasına ayarlandığı herhangi bir özel kural bulmanız gerekir. Özel bir bağlantı noktası olan tüm gelen kuralları için sorgulamak için aşağıdaki komutu çalıştırın:

        ```cmd
        netsh advfirewall firewall show rule dir=in name=all | select-string -pattern "(LocalPort.*<CUSTOM PORT>)" -context 9,4 | more
        ```

3.  Kuralı devre dışı olduğunu görürseniz, bunu etkinleştirin. Yerleşik Uzak Masaüstü grubu gibi tüm bir grubu açmak için aşağıdaki komutu çalıştırın:

    ```cmd
    netsh advfirewall firewall set rule group="Remote Desktop" new enable=yes
    ```

    Aksi takdirde, Uzak Masaüstü (TCP-Gelen) kuralı açmak için aşağıdaki komutu çalıştırın:

    ```cmd
    netsh advfirewall firewall set rule name="<CUSTOM RULE NAME>" new enable=yes
    ```

4.  Sorun giderme için Güvenlik Duvarı profilleri off kapatabilirsiniz:

    ```cmd
    netsh advfirewall set allprofiles state off
    ```

    Sorun giderme ve güvenlik duvarı doğru şekilde ayarlanması tamamladıktan sonra Güvenlik Duvarı'nı yeniden etkinleştirin.

    > [!Note]
    > Bu değişiklikleri uygulamak için VM'yi yeniden başlatma gerekmez.

5.  Sanal Makineye erişmek için bir RDP bağlantı kurmayı dener.

#### <a name="mitigation-2"></a>Risk azaltma 2

1.  Gelen güvenlik duvarı ilkesi olarak ayarlandığını belirlemek için Güvenlik Duvarı profilleri sorgu *BlockInboundAlways*:

    ```cmd
    netsh advfirewall show allprofiles | more
    ```

    ![Allprofiles](./media/guest-os-firewall-blocking-inbound-traffic/firewall-profiles.png)

    > [!Note]
    > Nasıl ayarlandığına bağlı olarak güvenlik duvarı ilkesi, aşağıdaki kurallar uygulanır:
    >    * *BlockInbound*: Aslında o trafiğe izin verecek bir kural yoksa, tüm gelen trafik engellenir.
    >    * *BlockInboundAlways*: Tüm güvenlik duvarı kurallarını göz ardı edilir ve tüm trafik engellenir.

2.  Düzen *DefaultInboundAction* bu profiller ayarlanacak **izin** trafiği. Bunu yapmak için aşağıdaki komutu çalıştırın:

    ```cmd
    netsh advfirewall set allprofiles firewallpolicy allowinbound,allowoutbound
    ```

3.  Yeniden değişikliğiniz başarıyla yapıldı emin olmak için profiller sorgulayın. Bunu yapmak için aşağıdaki komutu çalıştırın:

    ```cmd
    netsh advfirewall show allprofiles | more
    ```

    > [!Note]
    > Değişiklikleri uygulamak için VM'yi yeniden başlatma gerekmez.

4.  Sanal makinenize RDP erişmek bir kez daha deneyin.

### <a name="offline-mitigations"></a>Çevrimdışı bir risk azaltma işlemleri

1.  [Bir kurtarma VM'si sistem diski](troubleshoot-recovery-disks-portal-windows.md).

2.  Kurtarma VM'sini bir Uzak Masaüstü Bağlantısı'nı başlatın.

3.  Disk olarak işaretlenmiş olduğundan emin olun **çevrimiçi** Disk Yönetimi Konsolu'nda. Bağlı sistem diskine atanmış sürücü harfini unutmayın.

#### <a name="mitigation-1"></a>1 risk azaltma

Bkz: [nasıl bir konuk işletim sisteminde etkinleştirme-devre dışı bırakma için bir güvenlik duvarı kuralı](enable-disable-firewall-rule-guest-os.md).

#### <a name="mitigation-2"></a>Risk azaltma 2

1.  [Bir kurtarma VM'si sistem diski](troubleshoot-recovery-disks-portal-windows.md).

2.  Kurtarma VM'sini bir Uzak Masaüstü Bağlantısı'nı başlatın.

3.  Sistem diskini bir kurtarma VM'si bağlandıktan sonra disk olarak işaretlenmiş emin olun **çevrimiçi** Disk Yönetimi Konsolu'nda. Ekli işletim sistemi diski için atanan sürücü harfini unutmayın.

4.  Yükseltilmiş bir komut örneği açın ve ardından aşağıdaki betiği çalıştırın:

    ```cmd
    REM Backup the registry prior doing any change
    robocopy f:\windows\system32\config f:\windows\system32\config.BACK /MT

    REM Mount the hive
    reg load HKLM\BROKENSYSTEM f:\windows\system32\config\SYSTEM

    REM Delete the keys to block all inbound connection scenario
    REG DELETE "HKLM\BROKENSYSTEM\ControlSet001\services\SharedAccess\Parameters\FirewallPolicy\DomainProfile" /v DoNotAllowExceptions
    REG DELETE "HKLM\BROKENSYSTEM\ControlSet001\services\SharedAccess\Parameters\FirewallPolicy\PublicProfile" /v DoNotAllowExceptions
    REG DELETE "HKLM\BROKENSYSTEM\ControlSet001\services\SharedAccess\Parameters\FirewallPolicy\StandardProfile" /v DoNotAllowExceptions
    REG DELETE "HKLM\BROKENSYSTEM\ControlSet002\services\SharedAccess\Parameters\FirewallPolicy\DomainProfile" /v DoNotAllowExceptions
    REG DELETE "HKLM\BROKENSYSTEM\ControlSet002\services\SharedAccess\Parameters\FirewallPolicy\PublicProfile" /v DoNotAllowExceptions
    REG DELETE "HKLM\BROKENSYSTEM\ControlSet002\services\SharedAccess\Parameters\FirewallPolicy\StandardProfile" /v DoNotAllowExceptions

    REM Unmount the hive
    reg unload HKLM\BROKENSYSTEM
    ```

5.  [VM yeniden oluşturma ve sistem diskini](troubleshoot-recovery-disks-portal-windows.md).

6.  Sorunun çözülüp çözülmediğini denetleyin.
