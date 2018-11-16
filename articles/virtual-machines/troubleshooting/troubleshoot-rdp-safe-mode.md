---
title: VM Güvenli Modu'nda önyüklenir için Azure sanal makinelere uzaktan bağlanamıyor | Microsoft Docs
description: İçinde olamaz VM'ye RDP VM Güvenli Modu'nda önyüklenir çünkü bir sorun gidermeyi öğrenin. | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/13/2018
ms.author: genli
ms.openlocfilehash: 8dfe61430423298eea81510d3e92d49066217a05
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708807"
---
#  <a name="cannot-rdp-to-a-vm-because-the-vm-boots-into-safe-mode"></a>VM Güvenli Modu'nda önyüklenir olmadığından bir VM'ye RDP yapılamıyor

Bu makale, bunu yapamazsınız Uzak Masaüstü Azure Windows sanal makinelerin (VM'ler) sanal makine yapılandırıldığından bir sorunun nasıl çözüleceği güvenli moduna önyükleme.

> [!NOTE] 
> Azure, kaynak oluşturmak ve bu kaynaklarla çalışmak için iki dağıtım modeli kullanır: [Resource Manager ve klasik](../../azure-resource-manager/resource-manager-deployment-model.md). Bu makale, Klasik dağıtım modeli yerine yeni dağıtımlar için kullanmanızı öneririz Resource Manager dağıtım modelini kullanarak kapsar. 

## <a name="symptoms"></a>Belirtiler 

VM yapılandırıldığından, RDP bağlantısı ve diğer bağlantılar (örneğin, HTTP) azure'da VM yapamazsınız güvenli moduna önyükleme. Ne zaman iade ekran [önyükleme tanılaması](../troubleshooting/boot-diagnostics.md) Azure Portal'da, VM normal önyüklenir, ancak ağ arabirimi yok görebilirsiniz:

![Güvenli modda ağ inferce hakkında görüntü](./media/troubleshoot-rdp-safe-mode/network-safe-mode.png)

## <a name="cause"></a>Nedeni

Güvenli modda RDP hizmeti kullanılamıyor. Gerekli sistem programlar ve hizmetler yalnızca VM Güvenli Mod'da önyüklendiğinde yüklenir. Bu, "En az güvenli önyükleme" olan güvenli mod ve "Güvenli Önyükleme ile bağlantı" iki farklı sürümleri için geçerlidir.


## <a name="solution"></a>Çözüm 

Bu adımları gerçekleştirmeden önce etkilenen makinenin işletim sistemi diskinin anlık yedekleyin. Daha fazla bilgi için [bir diskin anlık görüntüsünü alma](../windows/snapshot-copy-managed-disk.md).

Bu sorunu gidermek için sanal Makinenin normal moduna önyüklemesini yapılandırmak için seri denetimi kullanın veya [çevrimdışı VM'yi onarın](#repair-the-vm-offline) kullanarak bir kurtarma VM'si.

### <a name="use-serial-control"></a>Seri denetimini kullanma

1. Bağlanma [seri konsol ve örnek CMD Aç](./serial-console-windows.md#open-cmd-or-powershell-in-serial-console
). Seri konsol sanal makinenizde etkinleştirilmemiş olmadığını [çevrimdışı VM'yi onarın](#repair-the-vm-offline).
2. Önyükleme yapılandırma verileri kontrol edin: 

        bcdedit /enum

    VM yapılandırıldıysa Güvenli Mod'da önyüklemek için ek bir bayrak altında görürsünüz **Windows önyükleme yükleyicisi** adlı bölüm **başlatılmayı**. Görmüyorsanız, **başlatılmayı** bayrağı, VM değil güvenli modda. Bu makaleyi senaryonuz için geçerli değildir.

    Başlatılmayı bayrağı aşağıdaki değerlerle görünebilir:
    - En az
    - Ağ

    Bu iki mod hiçbirinde RDP başlatılmaz. Bu nedenle düzeltme aynı kalır.

    ![Güvenli modu bayrağını hakkında görüntü](./media/troubleshoot-rdp-safe-mode/safe-mode-tag.png)

3. Silme **safemoade** bayrak VM normal moduna önyüklemesini şekilde:

        bcdedit /deletevalue {current} safeboot
        
4. Önyükleme yapılandırma verileri başlatılmayı bayrağı kaldırıldığından emin olmak için kontrol edin:

        bcdedit /enum

5. VM'yi yeniden başlatın ve sorunun çözülüp çözülmediğini denetleyin.

### <a name="repair-the-vm-offline"></a>VM'yi çevrimdışı onarın

#### <a name="attach-the-os-disk-to-a-recovery-vm"></a>İşletim sistemi diskini bir kurtarma VM'si ekleme

1. [İşletim sistemi diskini bir kurtarma VM'si ekleme](../windows/troubleshoot-recovery-disks-portal.md).
2. Kurtarma VM'sini bir Uzak Masaüstü Bağlantısı'nı başlatın. 
3. Disk olarak işaretlenmiş olduğundan emin olun **çevrimiçi** Disk Yönetimi Konsolu'nda. Ekli işletim sistemi diski için atanan sürücü harfini unutmayın.

#### <a name="enable-dump-log-and-serial-console-optional"></a>Döküm günlük ve seri konsol (isteğe bağlı) etkinleştirme

Seri konsol ve döküm günlük yapmak için bize yardımcı olacak sorun bu makalede bir çözüm tarafından çözümlenemezse ek sorun giderme.

Döküm günlük ve seri konsol etkinleştirmek için aşağıdaki betiği çalıştırın.

1. Yükseltilmiş bir komut istemi oturumu açın (**yönetici olarak çalıştır**).
2. Şu betiği çalıştırın:

    Bu betikte ekli işletim sistemi diski için atanan sürücü harfini f Değiştir VM'niz için uygun değeri bu sürücü harfiyle olduğunu varsayıyoruz.

    ```powershell
    reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM

    REM Enable Serial Console
    bcdedit /store F:\boot\bcd /set {bootmgr} displaybootmenu yes
    bcdedit /store F:\boot\bcd /set {bootmgr} timeout 5
    bcdedit /store F:\boot\bcd /set {bootmgr} bootems yes
    bcdedit /store F:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    bcdedit /store F:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200

    REM Suggested configuration to enable OS Dump
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    reg unload HKLM\BROKENSYSTEM

#### Configure the Windows to boot into normal mode

1. Open an elevated command prompt session (**Run as administrator**).
2. Check the boot configuration data. In the following commands, we assume that the drive letter that is assigned to the attached OS disk is F. Replace this drive letter with the appropriate value for your VM. 

        bcdedit /store F:\boot\bcd /enum
    Take note of the Identifier name of the partition that has the **\windows** folder. By default, the  Identifier name is "Default".  

    If the VM is configured to boot into Safe Mode, you will see an extra flag under the **Windows Boot Loader** section called **safeboot**. If you do not see the “safeboot” flag, this article does not apply to your scenario.

    ![The image about boot Identifier](./media/troubleshoot-rdp-safe-mode/boot-id.png)

3. Remove the **safeboot** flag, so the VM will boot into normal mode:

        bcdedit /store F:\boot\bcd /deletevalue {Default} safeboot
4. Check the boot configuration data to make sure that the safeboot flag is removed:

        bcdedit /store F:\boot\bcd /enum
5. [Detach the OS disk and recreate the VM](../windows/troubleshoot-recovery-disks-portal.md). Then check whether the issue is resolved.
