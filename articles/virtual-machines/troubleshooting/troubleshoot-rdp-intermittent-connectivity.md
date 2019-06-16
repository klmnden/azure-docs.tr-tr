---
title: Uzak Masaüstü bağlantısını keser sık Azure VM'de | Microsoft Docs
description: Azure sanal makinede Uzak Masaüstü'nün sık bağlantı kesilmesi sorunlarını gidermeyi öğrenin.
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
ms.openlocfilehash: 7fecf8c5fdafb64f7922054dd2bb9755b0dec031
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60386185"
---
# <a name="remote-desktop-disconnects-frequently-in-azure-vm"></a>Azure VM ile Uzak Masaüstü sık bağlantısını keser

Bu makalede Uzak Masaüstü Protokolü RDP aracılığıyla Azure sanal makinesi (VM) için sık sık bağlantı kesilmesi sorunlarını giderme açıklanmaktadır).

> [!NOTE] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Resource Manager dağıtım modelini incelemektedir. Bu model Klasik dağıtım modeli kullanılarak yerine yeni dağıtımlar için kullanmanızı öneririz.

## <a name="symptom"></a>Belirti

Aralıklı RDP bağlantı sorunlarını, oturumları sırasında yönetmektir. Başlangıçta VM'ye bağlanabilirsiniz, ancak bağlantıyı keser.

## <a name="cause"></a>Nedeni

Bu sorun, RDP dinleyicisi yanlış yapılandırılmış ortaya çıkabilir. Genellikle, özel bir görüntü kullanan bir VM üzerinde bu sorun oluşur.

## <a name="solution"></a>Çözüm

Bu adımları izlemeden önce [işletim sistemi diskinin anlık](../windows/snapshot-copy-managed-disk.md) etkilenen VM yedek olarak. 

Bu sorunu gidermek için seri denetimi kullanın veya [çevrimdışı VM'yi onarın](#repair-the-vm-offline) sanal Makinenin işletim sistemi diskini bir kurtarma sanal Makinesine ekleyerek.

### <a name="serial-control"></a>Seri denetimi

1. Bağlanma [seri konsol ve örnek CMD Aç](./serial-console-windows.md). Ardından, RDP yapılandırmaları sıfırlamak için aşağıdaki komutları çalıştırın. Seri konsol sanal makinenizde etkin değilse, sonraki adıma gidin.
2. 0 RDP güvenlik katmanı daha düşük. Bu ayarı, istemci ve sunucu arasındaki iletişimi yerel RDP şifreleme kullanın.

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'SecurityLayer' /t REG_DWORD /d 0 /f
3. Eski RDP istemcilerin bağlanmasına izin vermek için en düşük ayarı için şifreleme düzeyini düşürün.

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MinEncryptionLevel' /t REG_DWORD /d 1 /f
4. RDP, istemci bilgisayarın kullanıcı yapılandırmasını ayarlayın.

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fQueryUserConfigFromLocalMachine' /t REG_DWORD /d 1 /f
5. RDP Keep-Alive denetimi etkinleştir:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'KeepAliveTimeout' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveEnable' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveInterval' /t REG_DWORD /d 1 /f
6. RDP yeniden denetimini ayarlayın:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritReconnectSame' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fReconnectSame' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'fDisableAutoReconnect' /t REG_DWORD /d 0 /f
7. RDP oturumu zaman denetim ayarlayın:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxSessionTime' /t REG_DWORD /d 1 /f
8. RDP bağlantı kesme zamanı denetimi ayarlayın: 

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxDisconnectionTime' /t REG_DWORD /d 1 /f
        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxDisconnectionTime' /t REG_DWORD /d 0 /f
9. RDP bağlantı zamanı denetimi ayarlayın:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxConnectionTime' /t REG_DWORD /d 0 /f
10. RDP oturumu boşta kalma süresi denetimi ayarlayın:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxIdleTime' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxIdleTime' /t REG_DWORD /d 0 /f
11. "En fazla eş zamanlı bağlantı sayısı sınırı" Denetim ayarlayın:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxInstanceCount' /t REG_DWORD /d 4294967295 /f

12. VM'yi yeniden başlatın ve buna RDP kullanarak bağlanmak yeniden deneyin.

### <a name="repair-the-vm-offline"></a>VM'yi çevrimdışı onarın

1. [İşletim sistemi diskini bir kurtarma VM'si ekleme](../windows/troubleshoot-recovery-disks-portal.md).
2. İşletim sistemi diskini bir kurtarma VM'si bağlandıktan sonra disk olarak işaretlenmiş emin olun **çevrimiçi** Disk Yönetimi Konsolu'nda. Ekli işletim sistemi diski için atanan sürücü harfini unutmayın.
3. Bağlı işletim sistemi diskinde gidin **\windows\system32\config** klasör. Bir geri alma gerekli olması durumunda tüm dosyaları yedek olarak, bu klasöre kopyalayın.
4. Kayıt Defteri Düzenleyicisi'ni (regedit.exe) başlatın.
5. Seçin **HKEY_LOCAL_MACHINE** anahtarı. Menüsünde **dosya** > **yığını**:
6. Gözat **\windows\system32\config\SYSTEM** bağlı işletim sistemi diski klasörü. Hive için adı girin **BROKENSYSTEM**. Yeni kayıt defteri kovanını altında görüntülenen **HKEY_LOCAL_MACHINE** anahtarı. Yazılım hive'ı yükleme **\windows\system32\config\SOFTWARE** altında **HKEY_LOCAL_MACHINE** anahtarı. Hive yazılım adını girin **BROKENSOFTWARE**. 
7. Yükseltilmiş bir komut istemi penceresi açın (**yönetici olarak çalıştır**), ve RDP yapılandırmaları sıfırlamak için kalan adımlarda komutları çalıştırın. 
8. Böylece yerel RDP şifreleme istemci ve sunucu arasındaki iletişimleri 0 RDP güvenlik katmanı daha düşük:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'SecurityLayer' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'SecurityLayer' /t REG_DWORD /d 0 /f
9. Eski RDP istemcilerin bağlanmasına izin vermek için en düşük ayarı için şifreleme düzeyini düşürün:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MinEncryptionLevel' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MinEncryptionLevel' /t REG_DWORD /d 1 /f
10. İstemci makine kullanıcı yapılandırmasını yüklemek için RDP ayarlayın.

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fQueryUserConfigFromLocalMachine' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fQueryUserConfigFromLocalMachine' /t REG_DWORD /d 1 /f
11. RDP Keep-Alive denetimi etkinleştir:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'KeepAliveTimeout' /t REG_DWORD /d 1 /f 
        
        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'KeepAliveTimeout' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveEnable' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveInterval' /t REG_DWORD /d 1 /f
12. RDP yeniden denetimini ayarlayın:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritReconnectSame' /t REG_DWORD /d 0 /f 
        
        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fReconnectSame' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritReconnectSame' /t REG_DWORD /d 0 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fReconnectSame' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'fDisableAutoReconnect' /t REG_DWORD /d 0 /f

13. RDP oturumu zaman denetim ayarlayın:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxSessionTime' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxSessionTime' /t REG_DWORD /d 1 /f
14. RDP bağlantı kesme zamanı denetimi ayarlayın:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxDisconnectionTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxDisconnectionTime' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxDisconnectionTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxDisconnectionTime' /t REG_DWORD /d 0 /f
15. RDP bağlantı zamanı denetimi ayarlayın:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxConnectionTime' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxConnectionTime' /t REG_DWORD /d 0 /f
16. RDP oturumu boşta kalma süresi denetimi ayarlayın:     REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP Tcp" /v 'fInheritMaxIdleTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v ' MaxIdleTime' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxIdleTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v ' MaxIdleTime' /t REG_DWORD /d 0 /f
17. "En fazla eş zamanlı bağlantı sayısı sınırı" Denetim ayarlayın:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxInstanceCount' /t REG_DWORD /d ffffffff /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxInstanceCount' /t REG_DWORD /d ffffffff /f
18. VM'yi yeniden başlatın ve buna RDP kullanarak bağlanmak yeniden deneyin.

## <a name="need-help"></a>Yardım mı gerekiyor? 
Desteğe başvurun. Hala yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.





