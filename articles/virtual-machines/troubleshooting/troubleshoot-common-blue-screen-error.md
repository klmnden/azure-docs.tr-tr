---
title: Bir Azure sanal makinesi önyükleme yaparken ekran hataları mavi | Microsoft Docs
description: Sorun gidermeyi öğrenin, mavi ekran hata önyükleme yaparken alınan | Microsoft Docs
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
ms.date: 09/28/2018
ms.author: genli
ms.openlocfilehash: 26306489b11e24ab50f0ae893f11137d279c6127
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64719815"
---
# <a name="windows-shows-blue-screen-error-when-booting-an-azure-vm"></a>Windows mavi ekran hata, bir Azure sanal makinesi önyükleme yaparken gösterir.
Bu makalede, Microsoft Azure'da Windows sanal makinesi (VM) önyüklediğinizde karşılaşabileceğiniz mavi ekran hataları açıklanır. Bu, bir destek bileti için veri toplamanıza yardımcı olması için adımları sağlar. 

> [!NOTE] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli yerine yeni dağıtımlar için kullanmanızı öneririz Resource Manager dağıtım modeli kullanılarak açıklanır.

## <a name="symptom"></a>Belirti 

Bir Windows VM başlamaz. Ne zaman iade önyükleme ekran görüntüleri [önyükleme tanılaması](./boot-diagnostics.md), aşağıdaki hata iletilerinden mavi ekranda birine bakın:

- bir sorun ve yeniden başlatmanız gerekiyor bizim PC çalıştı. Biz yalnızca bazı hata bilgisi toplayacağınızı ve daha sonra yeniden başlatabilirsiniz.
- Bilgisayarınıza bir sorun ve yeniden başlatmanız gerekiyor çalıştı.

Bu bölümde, VM'ler yönetirken karşılaşabileceğiniz genel hata iletileri listelenir:

## <a name="cause"></a>Nedeni

Neden durdurma hatası alıyorum olarak birden çok nedeni olabilir. En yaygın nedenleri şunlardır:

- Bir sürücü ile ilgili sorun
- Bozuk bir sistem dosyası veya bellek
- Bir uygulama bellek yasaklı bir kesime erişir.

## <a name="collect-memory-dump-file"></a>Bellek dökümü toplama

Bu sorunu çözmek için kilitlenme döküm dosyası toplayın ve bilgi döküm dosyası ile desteğe ilk gerekir. Döküm dosyasını toplamak için şu adımları izleyin:

### <a name="attach-the-os-disk-to-a-recovery-vm"></a>İşletim sistemi diskini bir kurtarma VM'si ekleme

1. Etkilenen sanal makinenin işletim sistemi diskinin anlık yedekleyin. Daha fazla bilgi için [bir diskin anlık görüntüsünü alma](../windows/snapshot-copy-managed-disk.md).
2. [İşletim sistemi diskini bir kurtarma VM'si ekleme](../windows/troubleshoot-recovery-disks-portal.md). 
3. Kurtarma VM'sini Uzak Masaüstü.

### <a name="locate-dump-file-and-submit-a-support-ticket"></a>Döküm dosyasını bulun ve bir destek bileti gönderin

1. Kurtarma sanal makinesinde windows klasörü ekli işletim sistemi diski gidin. Ekli işletim sistemi diski için atanan sürücü harfini F ise F:\Windows için gitmeniz gerekiyor.
2. Memory.dmp dosyasını bulun ve ardından [bir destek bileti gönderin](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) döküm dosyası. 

Döküm dosyasını bulamazsanız, döküm günlük ve seri konsol etkinleştirmek için sonraki adımına geçmek.

### <a name="enable-dump-log-and-serial-console"></a>Döküm günlük ve seri konsol etkinleştir

Döküm günlük ve seri konsol etkinleştirmek için aşağıdaki betiği çalıştırın.

1. (Yönetici olarak çalıştır) yükseltilmiş bir komut istemi oturumu açın.
2. Şu betiği çalıştırın:

    Bu betikte, biz ekli işletim sistemi diski için atanan sürücü harfini f olduğunu varsayın.  Vm'nizde uygun değerle değiştirin.

    ```powershell
    reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

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

    1. Bu VM için seçiyorsunuz boyutuna bağlıdır RAM kadar bellek ayırmak için diskte yeterli alan emin olun.
    2. Yeterli alan yok veya bu büyük bir boyuta VM (G veya GS E serisi), ardından burada bu dosya oluşturulur ve bu VM'ye bağlı olduğu tüm diğer veri diski bakın konumu değiştirebilir. Bunu yapmak için aşağıdaki anahtarını değiştirmeniz gerekir:

            reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

            REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "<DRIVE LETTER OF YOUR DATA DISK>:\MEMORY.DMP" /f
            REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "<DRIVE LETTER OF YOUR DATA DISK>:\MEMORY.DMP" /f

            reg unload HKLM\BROKENSYSTEM

3. [İşletim sistemi diski çıkarın ve ardından etkilenen VM için işletim sistemi diskini yeniden ekleme](../windows/troubleshoot-recovery-disks-portal.md).
4. Sorunu yeniden oluşturmak için VM'i başlatın ve ardından bir döküm dosyası oluşturulur.
5. İşletim sistemi diskini bir kurtarma VM'si, toplama döküm dosyası ekleyin ve ardından [bir destek bileti gönderin](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) döküm dosyası.



