---
title: Azure sanal makinesine Uzak Masaüstü Hizmetleri Başlatılıyor yok | Microsoft Docs
description: Bir sanal makineye bağlanırken, Uzak Masaüstü Hizmetleri ile ilgili sorunları gidermeyi öğrenin | Microsoft Docs
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
ms.date: 10/23/2018
ms.author: genli
ms.openlocfilehash: 39b793e2722766f3f28829b4dc48534054abd97e
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49989248"
---
# <a name="remote-desktop-services-is-not-starting-on-an-azure-vm"></a>Azure sanal makinesine Uzak Masaüstü Hizmetleri başlamıyor

Bu makale, Uzak Masaüstü Hizmetleri (TermService) başlatırken veya başlatılamamasına değil, bir Azure sanal makine (VM) için bağlanma sorunlarını açıklar.

>[!NOTE]
>Azure, kaynak oluşturmak ve bu kaynaklarla çalışmak için iki dağıtım modeli kullanır: [Resource Manager ve klasik](../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede Resource Manager dağıtım modeli kullanılarak açıklanır. Bu model Klasik dağıtım modeli kullanılarak yerine yeni dağıtımlar için kullanmanızı öneririz.

## <a name="symptoms"></a>Belirtiler

Bir VM'ye bağlanmaya çalıştığınızda, aşağıdaki senaryolarda karşılaşırsınız:

- VM ekran görüntüsü, işletim sistemini tam olarak yüklenir ve kimlik bilgileri bekleniyor olduğunu gösterir.
- VM üzerindeki tüm uygulamalara beklendiği şekilde çalışıyor ve erişilebilir durumdadır.
- VM, Microsoft Uzak Masaüstü Protokolü (RDP) bağlantı noktasını (varsayılan olarak 3389) TCP bağlantı vermiyor.
- RDP bağlantısı kurmaya çalışırken kimlik bilgileri istenmez.

## <a name="cause"></a>Nedeni

Uzak Masaüstü Hizmetleri sanal makinede çalışmadığından bu sorun oluşur. Nedeni aşağıdaki senaryolara bağlı olabilir:

- TermService'nin ayarlandı **devre dışı bırakılmış**.
- TermService'nin kilitlenme veya askıda.

## <a name="solution"></a>Çözüm

Bu sorunu çözmek için aşağıdaki çözümlerden birini kullanın veya tek tek çözümleri deneyin:

### <a name="solution-1-using-the-serial-console"></a>Çözüm 1: seri konsol kullanma

1. Erişim [seri konsol](serial-console-windows.md) seçerek **destek ve sorun giderme** > **seri konsol (Önizleme)**. VM'de özelliği etkinleştirilmişse, sanal makine başarıyla bağlanabilirsiniz.

2. CMD örneği için yeni bir kanal oluşturun. Tür **CMD** kanal adını almak için kanal başlatmak için.

3. CMD örneğini çalıştıran, bu durumda, kanal 1 olması gerektiğini kanala geçin.

   ```
   ch -si 1
   ```

4. Tuşuna **Enter** yeniden ve sanal makine için geçerli bir kullanıcı adı ve parola (yerel veya etki alanı kimliği) yazın.

5. TermService hizmetin durumunu sorgulayın.

   ```
   sc query TermService
   ```

6. Hizmet durumunu gösteriyorsa **durduruldu**, hizmeti başlatmayı deneyin.

   ```
   sc start TermService
   ```

7. Hizmet yeniden hizmeti başarıyla başlatıldığını emin olmak için sorgu.

   ```
   sc query TermService
   ```

### <a name="solution-2-using-a-recovery-vm-to-enable-the-service"></a>Çözüm 2: hizmet için bir kurtarma VM'si kullanma

[İşletim sistemi diskini yedekleme](../windows/snapshot-copy-managed-disk.md) ve [kurtarma VM için işletim sistemi diski](../windows/troubleshoot-recovery-disks-portal.md). Ardından, yükseltilmiş bir komut örneği açın ve aşağıdaki betikler kurtarma VM üzerinde çalıştırın:

>[!NOTE]
>Ekli işletim sistemi diski için atanan sürücü harfini f Değiştir olduğunu varsayıyoruz, sanal makinenizin uygun değere sahip. Bunu yaptıktan sonra diski kurtarma VM'sini kullanımdan çıkarın ve [VM yeniden oluşturma](../windows/create-vm-specialized.md). Kullanabileceğiniz daha fazla sorun giderme için **Çözüm 1** seri konsol devre dışı bırakıldığından.

```
reg load HKLM\BROKENSYSTEM f:\windows\system32\config\SYSTEM

REM Enable Serial Console
bcdedit /store <Volume Letter Where The BCD Folder Is>:\boot\bcd /set {bootmgr} displaybootmenu yes
bcdedit /store <Volume Letter Where The BCD Folder Is>:\boot\bcd /set {bootmgr} timeout 10
bcdedit /store <Volume Letter Where The BCD Folder Is>:\boot\bcd /set {bootmgr} bootems yes
bcdedit /store <Volume Letter Where The BCD Folder Is>:\boot\bcd /ems {<Boot Loader Identifier>} ON
bcdedit /store <Volume Letter Where The BCD Folder Is>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200

REM Get the current ControlSet from where the OS is booting
for /f "tokens=3" %x in ('REG QUERY HKLM\BROKENSYSTEM\Select /v Current') do set ControlSet=%x
set ControlSet=%ControlSet:~2,1%

REM Suggested configuration to enable OS Dump
set key=HKLM\BROKENSYSTEM\ControlSet00%ControlSet%\Control\CrashControl
REG ADD %key% /v CrashDumpEnabled /t REG_DWORD /d 2 /f
REG ADD %key% /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
REG ADD %key% /v NMICrashDump /t REG_DWORD /d 1 /f

REM Set default values back on the broken service
reg add "HKLM\BROKENSYSTEM\ControlSet001\services\<Process Name>" /v start /t REG_DWORD /d <Startup Type> /f
reg add "HKLM\BROKENSYSTEM\ControlSet001\services\<Process Name>" /v ImagePath /t REG_EXPAND_SZ /d "<Image Path>" /f
reg add "HKLM\BROKENSYSTEM\ControlSet001\services\<Process Name>" /v ObjectName /t REG_SZ /d "<Startup Account>" /f
reg add "HKLM\BROKENSYSTEM\ControlSet001\services\<Process Name>" /v type /t REG_DWORD /d 16 /f
reg add "HKLM\BROKENSYSTEM\ControlSet002\services\<Process Name>" /v start /t REG_DWORD /d <Startup Type> /f
reg add "HKLM\BROKENSYSTEM\ControlSet002\services\<Process Name>" /v ImagePath /t REG_EXPAND_SZ /d "<Startup Account>" /f
reg add "HKLM\BROKENSYSTEM\ControlSet002\services\<Process Name>" /v ObjectName /t REG_SZ /d "<Startup Account>" /f
reg add "HKLM\BROKENSYSTEM\ControlSet002\services\<Process Name>" /v type /t REG_DWORD /d 16 /f

REM Enable default dependencies from the broken service
reg add "HKLM\BROKENSYSTEM\ControlSet001\services\<Driver/Service Name>" /v start /t REG_DWORD /d <Startup Type> /f
reg add "HKLM\BROKENSYSTEM\ControlSet002\services\<Driver/Service Name>" /v start /t REG_DWORD /d <Startup Type> /f
reg unload HKLM\BROKENSYSTEM
```

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
