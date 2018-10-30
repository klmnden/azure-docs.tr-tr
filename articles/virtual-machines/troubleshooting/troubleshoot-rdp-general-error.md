---
title: Azure'da bir Windows sanal makinesi için RDP genel hata sorunlarını giderme | Microsoft Docs
description: Azure'da bir Windows sanal makinesi için RDP genel hata sorunlarını gidermeyi öğrenin | Microsoft Docs
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
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: 701373efa3c3c22eb5969705927e1c0cc6e3f36b
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50215818"
---
# <a name="troubleshoot-an-rdp-general-error-in-azure-vm"></a>Azure VM'de bir RDP genel hata sorunlarını giderme

Bu makalede, azure'da Windows sanal makine (VM) için Uzak Masaüstü Protokolü (RDP) bağlantısı sırasında karşılaşabileceğiniz genel bir hata açıklanır.

## <a name="symptoms"></a>Belirtiler

Azure'da penceresi VM ile RDP bağlantısı değişiklik yaptığınızda, aşağıdaki genel hata iletisini alabilirsiniz:

**Uzak Masaüstü aşağıdaki nedenlerden biri için uzak bilgisayara bağlanamıyor:**

1. **Uzaktan erişim sunucusu için etkin değil**

2. **Uzak bilgisayar kapalı**

3. **Uzak bilgisayarın ağda kullanılamaz**

**Uzak bilgisayarın açık ve ağa bağlı ve Uzaktan erişim etkinleştirildiğinden emin olun.**

## <a name="cause"></a>Nedeni

Bu sorun, aşağıdaki nedenlerden dolayı ortaya çıkabilir:

### <a name="cause-1"></a>Neden 1

RDP bileşeni gibi devre dışı bırakılır:

- Bileşen düzeyinde
- Dinleyici düzeyinde
- Terminal sunucusu üzerinde
- Uzak Masaüstü oturumu ana bilgisayarı rolü

### <a name="cause-2"></a>Neden 2

Uzak Masaüstü Hizmetleri (TermService) çalışmıyor.

### <a name="cause-3"></a>Neden 3

RDP dinleyicisi yanlış yapılandırılmış.

## <a name="solution"></a>Çözüm

Bu sorunu çözmek için [işletim sistemi diskini yedekleme](../windows/snapshot-copy-managed-disk.md), ve [kurtarma VM için işletim sistemi diski](troubleshoot-recovery-disks-portal-windows.md)ve çözüm seçenekleri uygun şekilde izleyin veya tek tek çözümleri deneyin.

### <a name="serial-console"></a>Seri konsol

#### <a name="step-1-turn-on-remote-desk"></a>1. adım: Uzak Masaüstü Aç

1. Erişim [seri konsol](serial-console-windows.md) seçerek **destek ve sorun giderme** > **seri konsol (Önizleme)**. VM'de özelliği etkinleştirilmişse, sanal makine başarıyla bağlanabilirsiniz.

2. CMD örneği için yeni bir kanal oluşturun. Tür **CMD** kanal adını almak için kanal başlatmak için.

3. CMD örneğini çalıştıran, bu durumda, kanal 1 olması gerektiğini kanala geçin.

   ```
   ch -si 1
   ```

4. Uzak Masaüstü, şu ilkeyi değiştirerek GPO İlkesi aracılığıyla etkinleştirin:

   ```
   Computer Configuration\Policies\Administrative Templates: Policy definitions\Windows Components\Remote Desktop Services\Remote Desktop Session Host\Connections\Allow users to connect remotely by using Remote Desktop Services
   ```

5. Bu soruna neden olabilecek bazı diğer kayıt defteri anahtarları kayıt defteri anahtarlarının değerleri aşağıdaki gibi denetleyin:

   1. RDP bileşeni etkin olduğundan emin olun.

      ```
      REM Get the local policy
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server " /v fDenyTSConnections

      REM Get the domain policy if any
      reg query "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections
      ```

      Etki alanı ilkesi varsa, kurulumu yerel ilkesinde üzerine yazılır.

         - Etki alanı ilkesi RDP devre dışı (1) ve ardından güncelleştirme AD İlkesi olduğunu bildiren durumunda.
         - Etki alanı ilkesi RDP etkin (0) olduğunu belirtiyorsa, ardından hiçbir güncelleştirme gerekir.

      Etki alanı ilkesi mevcut değil ve yerel ilkesini RDP devre dışı bırakıldığını belirtir (1), aşağıdaki komutu kullanarak RDP'yi etkinleştirin:

         ```
         reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
         ```

   2. Terminal sunucusu geçerli yapılandırmasını denetleyin.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSEnabled
      ```

   3. Komut 0 döndürürse, terminal sunucusu devre dışı bırakıldı. Ardından, terminal sunucusu aşağıdaki gibi etkinleştirin:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f
      ```

   4. Terminal Sunucu modülü, bir terminal sunucu grubuna (RDS veya Citrix) sunucusuysa modu boşaltma için ayarlanır. Terminal sunucusu modülü geçerli modunu kontrol edin.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSServerDrainMode
      ```

   5. Komut 1 değerini döndürürse, Terminal Sunucu modülü boşaltma moduna ayarlanır. Ardından, çalışma moduna modülü şu şekilde ayarlayın:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f
      ```

   6. Terminal sunucusuna bağlanabilir olup olmadığını denetleyin.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSUserEnabled
      ```

   7. Komut 1 değerini döndürürse, terminal sunucusuna bağlanamıyor. Ardından, bağlantıyı aşağıdaki gibi etkinleştirin:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f
      ```

   8. RDP dinleyicisi geçerli yapılandırmasını denetleyin.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation
      ```

   9. Komut 0 döndürürse, RDP dinleyicisi devre dışı bırakıldı. Ardından, dinleyici aşağıdaki gibi etkinleştirin:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f
      ```

   10. RDP dinleyiciye bağlanıp bağlanamadığınızı denetleyin.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled
      ```

   11. Komut 1 değerini döndürürse, RDP dinleyicisi ile bağlantı kurulamıyor. Ardından, bağlantıyı aşağıdaki gibi etkinleştirin:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f
      ```

6. VM’yi yeniden başlatın.

7. Yazarak çıkış CMD örneğinden `exit`ve tuşuna **Enter** iki kez.

8. VM'yi yeniden başlatın yazarak `restart`.

Hala sorun olursa, adım 2 taşıyın.

#### <a name="step-2-enable-remote-desktop-services"></a>2. adım: Uzak Masaüstü Hizmetleri etkinleştirme

Daha fazla bilgi için [Uzak Masaüstü Hizmetleri, bir Azure sanal makinesinde başlatılıyor değil](troubleshoot-remote-desktop-services-issues.md).

#### <a name="step-3-reset-rdp-listener"></a>3. adım: Sıfırlama RDP dinleyicisi

Daha fazla bilgi için [Uzak Masaüstü bağlantısını keser sık Azure VM'de](troubleshoot-rdp-intermittent-connectivity.md).

### <a name="offline-repair"></a>Çevrimdışı onarım

#### <a name="step-1-turn-on-remote-desk"></a>1. adım: Uzak Masaüstü Aç

> [!NOTE]  
> Ekli işletim sistemi diski için atanan sürücü harfini f Değiştir olduğunu varsayıyoruz, sanal makinenizin uygun değere sahip. Sistem ve yazılım yığınlarını takılamadı ve ardından bağlı gerekir.

1. Yükseltilmiş bir komut örneği açın ve aşağıdaki betikler kurtarma VM üzerinde çalıştırın:

   ```
   reg load HKLM\BROKENSYSTEM f:\windows\system32\config\SYSTEM.hiv
   reg load HKLM\BROKENSOFTWARE f:\windows\system32\config\SOFTWARE.hiv

   REM Ensure that Terminal Server is enabled
   reg add "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f

   REM Ensure Terminal Service is not set to Drain mode
   reg add "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f

   REM Ensure Terminal Service has logon enabled
   reg add "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f

   REM Ensure the RDP Listener is not disabled
   reg add "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f

   REM Ensure the RDP Listener accepts logons
   reg add "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f

   REM RDP component is enabled
   reg add "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
   reg add "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections /t REG_DWORD /d 0 /f

   reg unload HKLM\BROKENSYSTEM
   reg unload HKLM\BROKENSOFTWARE
   ```

2. Sistem ve yazılım yığınlarını bağlayın.

   ```
   reg load HKLM\BROKENSYSTEM f:\windows\system32\config\SYSTEM
   reg load HKLM\BROKENSOFTWARE f:\windows\system32\config\SOFTWARE
   ```

3. Sanal Makine etki alanına katılmış ise, RDP İlkesi düzeyinde devre dışı bırakılamadı. Bu durumda doğrulamak için aşağıdaki kayıt defteri anahtarını kontrol edin:

   ```
   HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services\fDenyTSConnectionS
   ```

4. Bu anahtar değerini 1 olarak ayarlayın, RDP ilke tarafından devre dışı bırakılır.

5. Uzak Masaüstü, şu ilkeyi değiştirerek GPO İlkesi aracılığıyla etkinleştirin:

   ```
   Computer Configuration\Policies\Administrative Templates: Policy definitions\Windows Components\Remote Desktop Services\Remote Desktop Session Host\Connections\Allow users to connect remotely by using Remote Desktop Services
   ```

6. Bu kayıt defteri anahtarı mevcut değilse, aşağıdaki kayıt defteri anahtarını kontrol edin:

   ```
   HKLM\System\CurrentControlSet\Control\Terminal Server\fDenyTSConnections
   ```

7. Bu anahtarın 1 olarak ayarlarsanız, RDP Aç kapalıdır. Anahtar değerini 0 olarak değiştirin.
8. VM kurtarma diskten çıkarın.
9. [Diskten yeni bir VM oluşturma](../windows/create-vm-specialized.md).

Hala sorun olursa, adım 2 taşıyın.

#### <a name="step-2-enable-remote-desktop-services"></a>2. adım: Uzak Masaüstü Hizmetleri etkinleştirme

Daha fazla bilgi için [Uzak Masaüstü Hizmetleri, bir Azure sanal makinesinde başlatılıyor değil](troubleshoot-remote-desktop-services-issues.md).

#### <a name="step-3-reset-rdp-listener"></a>3. adım: Sıfırlama RDP dinleyicisi

Daha fazla bilgi için [Uzak Masaüstü bağlantısını keser sık Azure VM'de](troubleshoot-rdp-intermittent-connectivity.md).

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
