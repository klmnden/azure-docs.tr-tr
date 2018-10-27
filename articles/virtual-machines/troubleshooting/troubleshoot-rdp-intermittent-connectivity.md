---
title: Uzak Masaüstü bağlantısını keser sık Azure VM'de | Microsoft Docs
description: Uzak Masaüstü Azure VM'de sık keser sorun giderme hakkında bilgi edinin.
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
ms.date: 10/24/2018
ms.author: genli
ms.openlocfilehash: b65cd4d7e6f68f9444ca0264892bcca31cfbe674
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142514"
---
# <a name="remote-desktop-disconnects-frequently-in-azure-vm"></a>Azure VM ile Uzak Masaüstü sık bağlantısını keser

Bu makale, Uzak Masaüstü Azure VM'de sık keser sorunu nasıl giderilir.

> [!NOTE] 
> Azure, kaynak oluşturmak ve bu kaynaklarla çalışmak için iki dağıtım modeli kullanır: [Resource Manager ve klasik](../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli yerine yeni dağıtımlar için kullanmanızı öneririz Resource Manager dağıtım modeli kullanılarak açıklanır.

## <a name="symptom"></a>Belirti 

RDP iniş, oturumları sırasında yönetmektir. VM'ye bağlanabilir; ama bağlantıyı keser.

## <a name="cause"></a>Nedeni

Bu sorunun nedeni olabilir RDP dinleyicisi yanlış yapılandırılmış. Genellikle, bu sorunu özel sanal makinesinde gerçekleştirilir.

## <a name="solution"></a>Çözüm  

Adımları izlemeden önce [işletim sistemi diskinin anlık](../windows/snapshot-copy-managed-disk.md) etkilenen VM yedek olarak. 

Bu sorunu gidermek için seri denetimi kullanın veya [çevrimdışı VM'yi onarın](#repair-the-vm-offline) sanal Makinenin işletim sistemi diskini bir kurtarma sanal Makinesine ekleyerek.

### <a name="reset-rdp-configurations"></a>RDP yapılandırmaları Sıfırla

1. Bağlanma [seri konsol ve örnek CMD Aç](./serial-console-windows.md), ardından sıfırlama RDP yapılandırmaları bölümünde aşağıdaki komutları çalıştırın. Seri konsol sanal makinenizde etkin değilse, sonraki adıma taşıyın. 
2. 0, RDP güvenlik katmanı, sunucu ve istemci arasındaki iletişimin yerel RDP şifreleme kullanacak şekilde düşürün.

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'SecurityLayer' /t REG_DWORD /d 0 /f
3. En eski RDP istemcilerin bağlanmasına izin vermek için şifreleme düzeyini düşürün.

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MinEncryptionLevel' /t REG_DWORD /d 1 /f
4. İstemci makineye RDP kullanıcı yapılandırmasını ayarlayın.

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fQueryUserConfigFromLocalMachine' /t REG_DWORD /d 1 /f
5. RDP Keep-Alive denetimini etkinleştir:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'KeepAliveTimeout' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveEnable' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveInterval' /t REG_DWORD /d 1 /f
6. RDP yeniden denetimini ayarlama

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritReconnectSame' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fReconnectSame' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'fDisableAutoReconnect' /t REG_DWORD /d 0 /f
7. RDP oturumu zaman denetimini ayarlama 

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxSessionTime' /t REG_DWORD /d 1 /f
8. RDP bağlantı kesme zaman denetimini ayarlama 

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxDisconnectionTime' /t REG_DWORD /d 1 /f
        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxDisconnectionTime' /t REG_DWORD /d 0 /f
9. RDP bağlantı zamanı denetimi ayarlayın:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxConnectionTime' /t REG_DWORD /d 0 /f
10. RDP oturumu boşta zamanı denetimi ayarlayın:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxIdleTime' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v ' MaxIdleTime' /t REG_DWORD /d 0 /f
11. Kümesi en fazla eşzamanlı bağlantı sınırı:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxInstanceCount' /t REG_DWORD /d ffffffff /f

12. VM'yi yeniden başlatın ve size, RDP kullanarak bağlanıp bağlanamadığınızı görün.

### <a name="repair-the-vm-offline"></a>VM'yi çevrimdışı onarın

1. [İşletim sistemi diskini bir kurtarma VM'si ekleme](../windows/troubleshoot-recovery-disks-portal.md).
2. İşletim sistemi diskini bir kurtarma VM'si bağlandıktan sonra disk olarak işaretlenmiş emin olun **çevrimiçi** Disk Yönetimi Konsolu'nda. Ekli işletim sistemi diski için atanan sürücü harfini unutmayın.
3. Gözat **\windows\system32\config** klasör. Bir geri alma gerekli olması durumunda tüm dosyaların yedek olarak, bu klasöre kopyalayın.
4. Kayıt Defteri Düzenleyicisi'ni (regedit.exe) başlatın.
5. Seçin **HKEY_LOCAL_MACHINE** anahtarı. Menüsünde **dosya** > **yığını**:
6. Gözat **\windows\system32\config\SYSTEM** bağlı işletim sistemi diski klasörü. Hive için adı girin **BROKENSYSTEM**. Yeni kayıt defteri kovanını altında görüntülenen **HKEY_LOCAL_MACHINE** anahtarı.
7. Gözat **\windows\system32\config\SOFTWARE** bağlı işletim sistemi diski klasörü. Hive yazılım adını girin **BROKENSOFTWARE**.
8. Ardından RDP yapılandırmaları sıfırlamak için geri kalan adımlarda komutları çalıştırın bir yükseltilmiş komut istemi komut penceresi (yönetici olarak çalıştır) açın. 
9. Burada sunucu ve istemci arasındaki iletişimin yerel RDP şifreleme kullanacağı 0 RDP güvenlik katmanı daha düşük:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'SecurityLayer' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'SecurityLayer' /t REG_DWORD /d 0 /f
10. En eski RDP istemcilerin bağlanmasına izin vermek için şifreleme düzeyini düşürün:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MinEncryptionLevel' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MinEncryptionLevel' /t REG_DWORD /d 1 /f
11. İstemci makineye RDP kullanıcı yapılandırmasını ayarlayın.

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fQueryUserConfigFromLocalMachine' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fQueryUserConfigFromLocalMachine' /t REG_DWORD /d 1 /f
12. RDP Keep-Alive denetimini etkinleştir:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'KeepAliveTimeout' /t REG_DWORD /d 1 /f 
        
        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'KeepAliveTimeout' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveEnable' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveInterval' /t REG_DWORD /d 1 /f
13. RDP yeniden denetimini ayarlayın:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritReconnectSame' /t REG_DWORD /d 0 /f 
        
        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fReconnectSame' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritReconnectSame' /t REG_DWORD /d 0 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fReconnectSame' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'fDisableAutoReconnect' /t REG_DWORD /d 0 /f

14. RDP oturumu zaman denetimini ayarlayın:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxSessionTime' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxSessionTime' /t REG_DWORD /d 1 /f
15. RDP bağlantı kesme zamanı denetimi ayarlayın:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxDisconnectionTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxDisconnectionTime' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxDisconnectionTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxDisconnectionTime' /t REG_DWORD /d 0 /f
16. RDP bağlantı zamanı denetimi ayarlayın:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxConnectionTime' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxConnectionTime' /t REG_DWORD /d 0 /f
17. RDP oturumu boşta kalma süresi denetimi ayarlama: ' HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP Tcp "/v 'fInheritMaxIdleTime' /t REG_DWORD /d 1 /f REG ADD 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v ' MaxIdleTime' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxIdleTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v ' MaxIdleTime' /t REG_DWORD /d 0 /f
18. Kümesi en fazla eşzamanlı bağlantı sınırı:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxInstanceCount' /t REG_DWORD /d ffffffff /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxInstanceCount' /t REG_DWORD /d ffffffff /f
19. VM'yi yeniden başlatın ve size, RDP kullanarak bağlanıp bağlanamadığınızı görün.

## <a name="need-help"></a>Yardım mı gerekiyor? 
Desteğe başvurun. Hala yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.





