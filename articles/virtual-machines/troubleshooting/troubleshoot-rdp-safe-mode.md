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
ms.openlocfilehash: 8e108d88282894a7b1bf014146083008bedd483d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60319501"
---
#  <a name="cannot-rdp-to-a-vm-because-the-vm-boots-into-safe-mode"></a>VM Güvenli Modu'nda önyüklenir olmadığından bir VM'ye RDP yapılamıyor

Bu makale, size bağlanamıyor Azure Windows sanal makinelerine (VM'ler) sanal makine yapılandırıldığından bir sorunun nasıl çözüleceği güvenli moduna önyükleme.

> [!NOTE]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../azure-resource-manager/resource-manager-deployment-model.md). Bu makale, Klasik dağıtım modeli yerine yeni dağıtımlar için kullanmanızı öneririz Resource Manager dağıtım modelini kullanarak kapsar.

## <a name="symptoms"></a>Belirtiler

VM yapılandırıldığından, RDP bağlantısı veya diğer bağlantılar (örneğin, HTTP) azure'da VM yapamazsınız güvenli moduna önyükleme. Ne zaman iade ekran [önyükleme tanılaması](../troubleshooting/boot-diagnostics.md) Azure Portal'da, VM normal önyükleme yapmaz, ancak ağ arabirimi kullanılamıyor görebilirsiniz:

![Güvenli modda ağ inferce hakkında görüntü](./media/troubleshoot-rdp-safe-mode/network-safe-mode.png)

## <a name="cause"></a>Nedeni

Güvenli modda RDP hizmeti kullanılamıyor. Gerekli sistem programlar ve hizmetler yalnızca VM Güvenli Mod'da önyüklendiğinde yüklenir. Bu, "En az güvenli önyükleme" olan güvenli mod ve "Güvenli Önyükleme ile bağlantı" iki farklı sürümleri için geçerlidir.


## <a name="solution"></a>Çözüm

Bu adımları gerçekleştirmeden önce etkilenen makinenin işletim sistemi diskinin anlık yedekleyin. Daha fazla bilgi için [bir diskin anlık görüntüsünü alma](../windows/snapshot-copy-managed-disk.md).

Bu sorunu gidermek için sanal Makinenin normal moduna önyüklemesini yapılandırmak için seri denetimi kullanın veya [çevrimdışı VM'yi onarın](#repair-the-vm-offline) kullanarak bir kurtarma VM'si.

### <a name="use-serial-control"></a>Seri denetimini kullanma

1. Bağlanma [seri konsol ve örnek CMD Aç](./serial-console-windows.md#use-cmd-or-powershell-in-serial-console
   ). Seri konsol sanal makinenizde etkinleştirilmemiş olmadığını [çevrimdışı VM'yi onarın](#repair-the-vm-offline).
2. Önyükleme yapılandırma verileri kontrol edin:

        bcdedit /enum

    VM yapılandırıldıysa Güvenli Mod'da önyüklemek için ek bir bayrak altında görürsünüz **Windows önyükleme yükleyicisi** adlı bölüm **başlatılmayı**. Görmüyorsanız, **başlatılmayı** bayrağı, VM değil güvenli modda. Bu makaleyi senaryonuz için geçerli değildir.

    **Başlatılmayı** bayrağı aşağıdaki değerlerle görünür:
   - En az
   - Ağ

     Ya da bu iki mod, RDP başlatılmaz. Bu nedenle, düzeltme aynı kalır.

     ![Güvenli modu bayrağını hakkında görüntü](./media/troubleshoot-rdp-safe-mode/safe-mode-tag.png)

3. Silme **safemoade** bayrak VM normal moduna önyüklemesini şekilde:

        bcdedit /deletevalue {current} safeboot

4. Önyükleme yapılandırma verileri emin olmak için kontrol **başlatılmayı** bayrağı kaldırıldı:

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
    ```

#### <a name="configure-the-windows-to-boot-into-normal-mode"></a>Normal moduna önyüklemesini için Windows yapılandırma

1. Yükseltilmiş bir komut istemi oturumu açın (**yönetici olarak çalıştır**).
2. Önyükleme yapılandırma verileri kontrol edin. Aşağıdaki komutlar, ekli işletim sistemi diski için atanan sürücü harfini f Değiştir VM'niz için uygun değeri bu sürücü harfiyle olduğunu varsayıyoruz.

        bcdedit /store F:\boot\bcd /enum
    Tanımlayıcı adı olan bölümünün Not **\windows** klasör. Varsayılan olarak, "Varsayılan" tanımlayıcı adıdır.

    VM yapılandırıldıysa Güvenli Mod'da önyüklemek için ek bir bayrak altında görürsünüz **Windows önyükleme yükleyicisi** adlı bölüm **başlatılmayı**. Görmüyorsanız, **başlatılmayı** bayrağı, bu makaleyi senaryonuz için uygulanmaz.

    ![Önyükleme tanımlayıcısı hakkında görüntü](./media/troubleshoot-rdp-safe-mode/boot-id.png)

3. Kaldırma **başlatılmayı** bayrak VM normal moduna önyüklemesini şekilde:

        bcdedit /store F:\boot\bcd /deletevalue {Default} safeboot
4. Önyükleme yapılandırma verileri emin olmak için kontrol **başlatılmayı** bayrağı kaldırıldı:

        bcdedit /store F:\boot\bcd /enum
5. [İşletim sistemi diskini ve VM yeniden](../windows/troubleshoot-recovery-disks-portal.md). Daha sonra sorun çözülmüş olup olmadığını denetleyin.
