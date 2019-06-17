---
title: Kritik hizmet bir Azure sanal makinesi önyükleme başarısız oldu | Microsoft Docs
description: Önyükleme yaparken oluşan "0x0000005A kritik hizmet başarısız" hatası sorunlarını gidermeyi öğrenin | Microsoft Docs
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
ms.date: 10/08/2018
ms.author: genli
ms.openlocfilehash: e828a8fc4211a0f0c4b53a9e18fa1c2fb6f6916b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60593232"
---
# <a name="windows-shows-critical-service-failed-on-blue-screen-when-booting-an-azure-vm"></a>"Windows Kritik hizmet başarısız" mavi ekranda bir Azure sanal makinesi önyükleme yaparken gösterir
Bu makalede, Microsoft Azure'da Windows sanal makinesi (VM) önyüklediğinizde karşılaşabileceğiniz "Kritik hizmet başarısız" hatası. Bu sorunları gidermek için sorun giderme adımlarını sağlar. 

> [!NOTE] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli yerine yeni dağıtımlar için kullanmanızı öneririz Resource Manager dağıtım modeli kullanılarak açıklanır.

## <a name="symptom"></a>Belirti 

Bir Windows VM başlamaz. Ne zaman iade önyükleme ekran görüntüleri [önyükleme tanılaması](./boot-diagnostics.md), mavi bir ekranda gördüğünüz aşağıdaki hata iletilerinden biri:

- "Bir sorun ve yeniden başlatmanız gerekiyor bilgisayarınıza çalıştı. Yeniden başlatabilirsiniz. Bu sorun ve olası düzeltmeler hakkında daha fazla bilgi için ziyaret http://windows.com/stopcode. Destek ekibiyle çağırırsanız, kullanıcıların bu bilgileri sağlayın: Kod durdurun: KRİTİK HİZMETİ BAŞARISIZ OLDU" 
- "Bir sorun ve yeniden başlatmanız gerekiyor bilgisayarınıza çalıştı. Biz yalnızca bazı hata bilgisi toplayacağınızı ve ardından biz sizin için yeniden başlatmanız gerekecektir. Daha fazla bilgi edinmek istiyorsanız, arayabilirsiniz daha sonra bu hata için çevrimiçi: CRITICAL_SERVICE_FAILED"

## <a name="cause"></a>Nedeni

Durdurma hataları çeşitli nedenleri vardır. En yaygın nedenleri şunlardır:
- Bir sürücü ile ilgili sorun
- Bozuk bir sistem dosyası veya bellek
- Uygulamanın bellek yasaklı bir kesim erişir

## <a name="solution"></a>Çözüm 

Bu sorunu çözmek için [Destek ekibiyle iletişime geçin ve bir döküm dosyası göndermek](./troubleshoot-common-blue-screen-error.md#collect-memory-dump-file), hangi yardımcı oluyor sorunu daha hızlı bir şekilde tanılamak veya aşağıdaki kendi kendine yardım çözümü deneyin.

### <a name="attach-the-os-disk-to-a-recovery-vm"></a>İşletim sistemi diskini bir kurtarma VM'si ekleme

1. Etkilenen sanal makinenin işletim sistemi diskinin anlık yedekleyin. Daha fazla bilgi için [bir diskin anlık görüntüsünü alma](../windows/snapshot-copy-managed-disk.md).
2. [İşletim sistemi diskini bir kurtarma VM'si ekleme](./troubleshoot-recovery-disks-portal-windows.md). 
3. Kurtarma VM'sini Uzak Masaüstü bağlantı kurun.

### <a name="enable-dump-logs-and-serial-console"></a>Döküm günlükleri ve seri konsol etkinleştir

Döküm günlük ve [seri konsol](./serial-console-windows.md) yapmak için bize yardımcı olacak daha fazla sorun giderme.

Döküm günlükleri ve seri konsol etkinleştirmek için aşağıdaki betiği çalıştırın.

1. (Yönetici olarak çalıştır) bir yükseltilmiş komut istemi oturumu açın.
2. Şu betiği çalıştırın:

    Bu betikte, biz ekli işletim sistemi diski için atanan sürücü harfini f olduğunu varsayın. Bu sanal Makineniz için uygun değeri ile değiştirmelisiniz.

    ```powershell
    reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

    REM Enable Serial Console
    bcdedit /store F:\boot\bcd /set {bootmgr} displaybootmenu yes
    bcdedit /store F:\boot\bcd /set {bootmgr} timeout 10
    bcdedit /store F:\boot\bcd /set {bootmgr} bootems yes
    bcdedit /store F:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    bcdedit /store F:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200

    REM Suggested configuration to enable OS Dump
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 2 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 2 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    reg unload HKLM\BROKENSYSTEM
    ```

### <a name="replace-the-unsigned-drivers"></a>İmzasız sürücülerin değiştirin

1. Kurtarma sanal makinesinde aşağıdaki komutu yükseltilmiş bir komut isteminden çalıştırın. Bu komut, bir sonraki önyüklemede güvenli moduna başlatmak için etkilenen işletim sistemi diskini ayarlar:

        bcdedit /store <OS DISK you attached>:\boot\bcd /set {default} safeboot minimal

    Örneğin, F sürücü bağlı işletim sistemi diski ise aşağıdaki komutu çalıştırın:

        bcdedit /store F: boot\bcd /set {default} safeboot minimal

2. [İşletim sistemi diski çıkarın ve ardından etkilenen VM için işletim sistemi diskini yeniden ekleme](troubleshoot-recovery-disks-portal-windows.md). VM Modu'nda önyüklenir. Hata oluşmaya devam ederse isteğe bağlı bir adıma gidin.
3. Açık **çalıştırma** kutusuna ve çalıştırma **Doğrulayıcı** sürücü doğrulama Yöneticisi aracını başlatmak için.
4. Seçin **otomatik olarak imzalanmamış sürücüleri seçin**ve ardından **sonraki**.
5. İmzasız sürücü dosyaları listesini alırsınız. Dosya adlarını unutmayın.
6. Bu dosyaların aynı sürümde çalışan bir VM kopyalayın ve ardından bu imzalanmamış dosyaların değiştirin. 

7. Güvenli Önyükleme ayarları kaldırın:

        bcdedit /store <OS DISK LETTER>:\boot\bcd /deletevalue {default} safeboot
8.  VM’yi yeniden başlatın. 

### <a name="optional-analyze-the-dump-logs-in-dump-crash-mode"></a>İsteğe bağlı: Kilitlenme bilgi dökümü modunda döküm günlüklerini çözümleme

Kendiniz döküm günlükleri analiz etmek için şu adımları izleyin:

1. İşletim sistemi diskini bir kurtarma sanal makinesine ekleyin.
2. Bağlı işletim sistemi diskinde göz atın **\windows\system32\config**. Bir geri alma gerekli olması durumunda yedek olarak tüm dosyaları kopyalayın.
3. Başlangıç **Kayıt Defteri Düzenleyicisi'ni** (regedit.exe).
4. Seçin **HKEY_LOCAL_MACHINE** anahtarı. Menüsünde **dosya** > **yığını**.
5. Gözat **\windows\system32\config\SYSTEM** bağlı işletim sistemi diski klasörü. Hive için adı girin **BROKENSYSTEM**. Yeni kayıt defteri kovanını altında görüntülenen **HKEY_LOCAL_MACHINE** anahtarı.
6. Gözat **HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Control\CrashControl** ve aşağıdaki değişiklikleri yapın:

    AutoReboot = 0

    CrashDumpEnabled = 2
7.  Seçin **BROKENSYSTEM**. Menüden **dosya** > **yığın**.
8.  Hata ayıklama moduna önyükleme BCD Kurulum değiştirin. Yükseltilmiş bir komut isteminden aşağıdaki komutları çalıştırın:

    ```cmd
    REM Setup some debugging flags on the boot manager
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {bootmgr} default {<IDENTIFIER OF THE WINDOWS BOOT LOADER>}
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {bootmgr} nointegritychecks on
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {bootmgr} integrityservices disable

    REM Setup some debugging flags on the boot loader
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} bootlog yes
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} bootstatuspolicy displayallfailures
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} nointegritychecks on
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} testsigning off
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} recoveryenabled no
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} integrityservices disable
    ```
9. [İşletim sistemi diski çıkarın ve ardından etkilenen VM için işletim sistemi diskini yeniden ekleme](troubleshoot-recovery-disks-portal-windows.md).
10. Dökümü analizi gösterir görmek için VM'yi önyüklemek. Yüklenemiyordur dosyasını bulun. Bu dosya bir dosyadan VM çalışma ile değiştirmeniz gerekir. 

    Dökümü analizi örneği verilmiştir. Gördüğünüz gibi **hatası** filecrypt.sys üzerinde olan: "FAILURE_BUCKET_ID: 0x5A_c0000428_IMAGE_filecrypt.sys".

    ```
    kd> !analyze -v 

    ******************************************************************************* 
    * * * Bugcheck Analysis * * * 

    ******************************************************************************* 
    CRITICAL_SERVICE_FAILED (5a) Arguments: Arg1: 0000000000000001 Arg2: ffffd80f4bfe7070 Arg3: ffffb00b0513d320 Arg4: ffffffffc0000428 Debugging 
    Details: ------------------ 
    DUMP_CLASS: 1 DUMP_QUALIFIER: 401 BUILD_VERSION_STRING: 10.0.14393.1770 (rs1_release.170917-1700) 
    DUMP_TYPE: 1 BUGCHECK_P1: 1 BUGCHECK_P2: ffffd80f4bfe7070 BUGCHECK_P3: ffffb00b0513d320 BUGCHECK_P4: ffffffffc0000428 BUGCHECK_STR: 0x5A_c0000428 
    CPU_COUNT: 1 CPU_MHZ: a22 CPU_VENDOR: GenuineIntel CPU_FAMILY: 6 CPU_MODEL: 3d CPU_STEPPING: 4 DEFAULT_BUCKET_ID: WIN8_DRIVER_FAULT 
    PROCESS_NAME: System CURRENT_IRQL: 0 ANALYSIS_SESSION_HOST: MININT-6RMM091 ANALYSIS_SESSION_TIME: 11-15-2017 19:32:42.0841 
    ANALYSIS_VERSION: 10.0.16361.1001 amd64fre STACK_TEXT: ffffc701`1dc74948 fffff803`b2ff4b4a : 00000000`0000005a 00000000`00000001 ffffd80f`4bfe7070 ffffb00b`0513d320 : nt!KeBugCheckEx [d:\rs1\minkernel\ntos\ke\amd64\procstat.asm @ 127] ffffc701`1dc74950 fffff803`b3205df3 : ffffd80f`4bba9f58 ffffd80f`4bba9f58 ffffc701`1dc74b80 ffffd80f`00000006 : nt!IopLoadDriver+0x19f8e6 [d:\rs1\minkernel\ntos\ke\amd64\threadbg.asm @ 81] 
    RETRACER_ANALYSIS_TAG_STATUS: DEBUG_FLR_FAULTING_IP is not found THREAD_SHA1_HASH_MOD_FUNC: eb79608c07faa1af62c0e61f25ff6bc1d6dfdb25 THREAD_SHA1_HASH_MOD_FUNC_OFFSET: 96a3a314834bb4e8443a8b7201525fc5dfc1878b THREAD_SHA1_HASH_MOD: 30a3e915496deaace47137d5b90c3ecc03746bf6 FOLLOWUP_NAME: wintriag
    MODULE_NAME: filecrypt IMAGE_NAME: filecrypt.sys DEBUG_FLR_IMAGE_TIMESTAMP: 0 IMAGE_VERSION: STACK_COMMAND: .thread ; .cxr ; kb FAILURE_BUCKET_ID: 0x5A_c0000428_IMAGE_filecrypt.sys BUCKET_ID: 0x5A_c0000428_IMAGE_filecrypt.sys PRIMARY_PROBLEM_CLASS: 0x5A_c0000428_IMAGE_filecrypt.sys TARGET_TIME: 2017-11-13T20:51:04.000Z OSBUILD: 14393 OSSERVICEPACK: 1770 SERVICEPACK_NUMBER: 0 OS_REVISION: 0 SUITE_MASK: 144 PRODUCT_TYPE: 3 OSPLATFORM_TYPE: x64 OSNAME: Windows 10 OSEDITION: Windows 10 Server TerminalServer DataCenter OS_LOCALE: USER_LCID: 0 OSBUILD_TIMESTAMP: 2017-09-17 19:16:08 BUILDDATESTAMP_STR: 170917-1700 BUILDLAB_STR: rs1_release BUILDOSVER_STR: 10.0.14393.1770 ANALYSIS_SESSION_ELAPSED_TIME: bfc ANALYSIS_SOURCE: KM FAILURE_ID_HASH_STRING: km:0x5a_c0000428_image_filecrypt.sys FAILURE_ID_HASH: {35f25777-b01e-70a1-c502-f690dab6cb3a} FAILURE_ID_REPORT_LINK: https://go.microsoft.com/fwlink/?LinkID=397724&FailureHash=35f25777-b01e-70a1-c502-f690dab6cb3a
    ```

11. VM çalışma ve normal şekilde önyüklenmesini sonra dökümü kilitlenme ayarları kaldırın:

    ```cmd
    REM Restore the boot manager to default values
    bcdedit /store <OS DISK LETTER>:\boot\bcd /deletevalue {bootmgr} nointegritychecks
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {bootmgr} integrityservices enable

    REM Restore the boot loader to default values for an azure VM
    bcdedit /store <OS DISK LETTER>:\boot\bcd /deletevalue {default} bootlog
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} bootstatuspolicy ignoreallfailures
    bcdedit /store <OS DISK LETTER>:\boot\bcd /deletevalue {default} nointegritychecks
    bcdedit /store <OS DISK LETTER>:\boot\bcd /deletevalue {default} testsigning
    bcddit /store <OS DISK LETTER>:\boot\bcd /set {default} recoveryenabled no
    bcdedit /store <OS DISK LETTER>:\boot\bcd /set {default} integrityservicesenable
    ```
