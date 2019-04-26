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
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: f290a7e16938c66d45fab9b78086f77bfdfe4839
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60319522"
---
# <a name="troubleshoot-an-rdp-general-error-in-azure-vm"></a>Azure VM'de bir RDP genel hata sorunlarını giderme

Bu makalede, azure'da Windows sanal makine (VM) için Uzak Masaüstü Protokolü (RDP) bağlantısı sırasında karşılaşabileceğiniz genel bir hata açıklanır.

## <a name="symptom"></a>Belirti

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

Bu sorunu çözmek için [işletim sistemi diskini yedekleme](../windows/snapshot-copy-managed-disk.md), ve [kurtarma VM için işletim sistemi diski](troubleshoot-recovery-disks-portal-windows.md)ve ardından adımları izleyin.

### <a name="serial-console"></a>Seri konsol

#### <a name="step-1-open-cmd-instance-in-serial-console"></a>1. Adım: CMD örnek seri konsolu aç

1. Erişim [seri konsol](serial-console-windows.md) seçerek **destek ve sorun giderme** > **seri konsol (Önizleme)**. VM'de özelliği etkinleştirilmişse, sanal makine başarıyla bağlanabilirsiniz.

2. CMD örneği için yeni bir kanal oluşturun. Tür **CMD** kanal adını almak için kanal başlatmak için.

3. CMD örneğini çalıştıran, bu durumda, kanal 1 olması gerektiğini kanala geçin.

   ```
   ch -si 1
   ```

#### <a name="step-2-check-the-values-of-rdp-registry-keys"></a>2. Adım: RDP kayıt defteri anahtarlarını değerlerini kontrol edin:

1. RDP tarafından devre dışı bırakılırsa onay ilkeleri.

      ```
      REM Get the local policy 
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server " /v fDenyTSConnections

      REM Get the domain policy if any
      reg query "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections
      ```

      - Etki alanı ilkesi varsa, kurulumu yerel ilkesinde üzerine yazılır.
      - Etki alanı ilkesi RDP devre dışı (1) ve ardından etki alanı denetleyicisinden AD güncelleştirme ilkesi olduğunu bildiren durumunda.
      - Etki alanı ilkesi RDP etkin (0) olduğunu belirtiyorsa, ardından hiçbir güncelleştirme gerekir.
      - Etki alanı ilkesi mevcut değil ve yerel ilkesini RDP devre dışı bırakıldığını belirtir (1), aşağıdaki komutu kullanarak RDP'yi etkinleştirin: 
      
            reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
                  

2. Terminal sunucusu geçerli yapılandırmasını denetleyin.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSEnabled
      ```

      Komut 0 döndürürse, terminal sunucusu devre dışı bırakıldı. Ardından, terminal sunucusu aşağıdaki gibi etkinleştirin:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f
      ```

3. Terminal Sunucu modülü, bir terminal sunucu grubuna (RDS veya Citrix) sunucusuysa modu boşaltma için ayarlanır. Terminal sunucusu modülü geçerli modunu kontrol edin.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSServerDrainMode
      ```

      Komut 1 değerini döndürürse, Terminal Sunucu modülü boşaltma moduna ayarlanır. Ardından, çalışma moduna modülü şu şekilde ayarlayın:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f
      ```

4. Terminal sunucusuna bağlanabilir olup olmadığını denetleyin.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSUserEnabled
      ```

      Komut 1 değerini döndürürse, terminal sunucusuna bağlanamıyor. Ardından, bağlantıyı aşağıdaki gibi etkinleştirin:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f
      ```
5. RDP dinleyicisi geçerli yapılandırmasını denetleyin.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation
      ```

      Komut 0 döndürürse, RDP dinleyicisi devre dışı bırakıldı. Ardından, dinleyici aşağıdaki gibi etkinleştirin:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f
      ```

6. RDP dinleyiciye bağlanıp bağlanamadığınızı denetleyin.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled
      ```

   Komut 1 değerini döndürürse, RDP dinleyicisi ile bağlantı kurulamıyor. Ardından, bağlantıyı aşağıdaki gibi etkinleştirin:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f
      ```

7. VM’yi yeniden başlatın.

8. Yazarak çıkış CMD örneğinden `exit`ve tuşuna **Enter** iki kez.

9. VM'yi yeniden başlatın yazarak `restart`ve sonra VM'ye bağlanın.

Hala sorun olursa, adım 2 taşıyın.

#### <a name="step-2-enable-remote-desktop-services"></a>2. Adım: Uzak Masaüstü Hizmetleri etkinleştirme

Daha fazla bilgi için [Uzak Masaüstü Hizmetleri, bir Azure sanal makinesinde başlatılıyor değil](troubleshoot-remote-desktop-services-issues.md).

#### <a name="step-3-reset-rdp-listener"></a>3. Adım: RDP dinleyici Sıfırla

Daha fazla bilgi için [Uzak Masaüstü bağlantısını keser sık Azure VM'de](troubleshoot-rdp-intermittent-connectivity.md).

### <a name="offline-repair"></a>Çevrimdışı onarım

#### <a name="step-1-turn-on-remote-desktop"></a>1. Adım: Uzak Masaüstü Aç

1. [İşletim sistemi diskini bir kurtarma VM'si ekleme](../windows/troubleshoot-recovery-disks-portal.md).
2. Kurtarma VM'sini bir Uzak Masaüstü Bağlantısı'nı başlatın.
3. Disk olarak işaretlenmiş olduğundan emin olun **çevrimiçi** Disk Yönetimi Konsolu'nda. Ekli işletim sistemi diski için atanan sürücü harfini unutmayın.
4. Kurtarma VM'sini bir Uzak Masaüstü Bağlantısı'nı başlatın.
5. Yükseltilmiş bir komut istemi oturumu açın (**yönetici olarak çalıştır**). Aşağıdaki komut dosyasını çalıştırın. Bu betikte ekli işletim sistemi diski için atanan sürücü harfini f Değiştir VM'niz için uygun değeri bu sürücü harfiyle olduğunu varsayıyoruz.

      ```
      reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv 
      reg load HKLM\BROKENSOFTWARE F:\windows\system32\config\SOFTWARE.hiv 
 
      REM Ensure that Terminal Server is enabled 

      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f 

      REM Ensure Terminal Service is not set to Drain mode 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f 

      REM Ensure Terminal Service has logon enabled 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f 

      REM Ensure the RDP Listener is not disabled 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f 

      REM Ensure the RDP Listener accepts logons 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f 

      REM RDP component is enabled 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections /t REG_DWORD /d 0 /f 

      reg unload HKLM\BROKENSYSTEM 
      reg unload HKLM\BROKENSOFTWARE 
      ```

6. VM'nin etki alanına katılmış ise, RDP devre dışı bırakan bir Grup İlkesi olup olmadığını görmek için aşağıdaki kayıt defteri anahtarını kontrol edin. 

      ```
      HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services\fDenyTSConnectionS
      ```

      Bu anahtar değeri ayarlamak RDP anlamına gelen 1 ilke tarafından devre dışı bırakılır. Uzak Masaüstü GPO İlkesi aracılığıyla etkinleştirmek için etki alanı denetleyicisinden şu ilkeyi değiştirin:

   
      **Bilgisayar Yapılandırması\İlkeler\Yönetim şablonları:**

      İlke Deﬁ Bileşenleri\Uzak Desktop Hizmetleri\Uzak Masaüstü oturumu Host\Connections\Allow kullanıcıların Uzak Masaüstü Hizmetleri kullanarak uzaktan bağlanma
  
1. VM kurtarma diskten çıkarın.
1. [Diskten yeni bir VM oluşturma](../windows/create-vm-specialized.md).

Hala sorun olursa, adım 2 taşıyın.

#### <a name="step-2-enable-remote-desktop-services"></a>2. Adım: Uzak Masaüstü Hizmetleri etkinleştirme

Daha fazla bilgi için [Uzak Masaüstü Hizmetleri, bir Azure sanal makinesinde başlatılıyor değil](troubleshoot-remote-desktop-services-issues.md).

#### <a name="step-3-reset-rdp-listener"></a>3. Adım: RDP dinleyici Sıfırla

Daha fazla bilgi için [Uzak Masaüstü bağlantısını keser sık Azure VM'de](troubleshoot-rdp-intermittent-connectivity.md).

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun

Hala yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
