---
title: Bir Windows 10 veya Windows Server 2016 VM azure'da uzaktan bağlanırken netvsc.sys sorun giderme | Microsoft Docs
description: Netsvc.sys ile ilgili bir RDP sorunlarını gidermeyi öğrenin ne zaman sorun, Windows 10 veya Windows Server 2016 VM azure'da bağlanma.
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: v-jesits
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/19/2018
ms.author: genli
ms.openlocfilehash: e6685a5e77d92bb9e05ab9578e48c99e80a64b74
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60362263"
---
# <a name="cannot-connect-remotely-to-a-windows-10-or-windows-server-2016-vm-in-azure-because-of-netvscsys"></a>Uzaktan Windows 10 veya Windows Server 2016 VM azure'da netvsc.sys nedeniyle bağlanılamıyor

Bu makalede olduğu ağ bağlantısı bir Windows 10 veya Windows Server 2016 Datacenter sanal makinesi (VM) bir Hyper-V Server 2016 konağında bağlandığınızda bir sorunun nasıl giderileceği açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

Uzak Masaüstü Protokolü (RDP) kullanarak bir Azure Windows 10 veya Windows Server 2016 VM bağlanamıyor. İçinde [önyükleme tanılaması](boot-diagnostics.md), ekran, ağ arabirim kartı (NIC) üzerinde kırmızı çarpı işareti gösterir. Bu, işletim sistemini tam olarak yüklendikten sonra VM bağlantısı olduğunu gösterir.

Genellikle, Windows bu sorun oluşur [derleme 14393](https://support.microsoft.com/help/4093120/) ve [derleme 15063](https://support.microsoft.com/help/4015583/). Bu makalede, işletim sistemi sürümü bu sürümlerden daha sonraysa, senaryonuz için geçerli değildir. Sistem sürümü denetlemek için bir CMD oturumda açın [seri erişim Konsolu özelliği](serial-console-windows.md)ve ardından çalıştırın **Ver**.

## <a name="cause"></a>Nedeni

Yüklü netvsc.sys sistem dosyasının sürümü ise bu sorun ortaya çıkabilir **10.0.14393.594** veya **10.0.15063.0**. Bu netvsc.sys sürümleri, Azure platformuyla etkileşim sistem engelleyebilir.


## <a name="solution"></a>Çözüm

Bu adımları izlemeden önce [sistem diski anlık](../windows/snapshot-copy-managed-disk.md) etkilenen VM yedek olarak. Bu sorunu gidermek için seri konsolu veya [çevrimdışı VM'yi onarın](#repair-the-vm-offline) sanal Makinenin sistem diskini bir kurtarma sanal Makinesine ekleyerek.


### <a name="use-the-serial-console"></a>Seri Konsolu

Bağlanma [seri konsolu, PowerShell örneği açın](serial-console-windows.md)ve ardından aşağıdaki adımları izleyin.

> [!NOTE]
> Seri konsol sanal makinenizde etkin değilse, Git [çevrimdışı VM'yi onarın](#repair-the-vm-offline) bölümü.

1. Dosya sürümünü almak için bir PowerShell örneği aşağıdaki komutu çalıştırın (**c:\windows\system32\drivers\netvsc.sys**):

   ```
   (get-childitem "$env:systemroot\system32\drivers\netvsc.sys").VersionInfo.FileVersion
   ```

2. Aynı bölgeden çalışan VM bağlı yeni veya var olan veri diski için uygun güncelleştirmeyi indirin:

   - **10.0.14393.594**: [KB4073562](https://support.microsoft.com/help/4073562) ya da daha yeni bir güncelleştirme
   - **10.0.15063.0**: [KB4016240](https://support.microsoft.com/help/4016240) ya da daha yeni bir güncelleştirme

3. VM çalışmasını yardımcı diski çıkarın ve ardından bozuk VM'e ekleyin.

4. Sanal makinede güncelleştirmeyi yüklemek için aşağıdaki komutu çalıştırın:

   ```
   dism /ONLINE /add-package /packagepath:<Utility Disk Letter>:\<KB .msu or .cab>
   ```

5. VM’yi yeniden başlatın.

### <a name="repair-the-vm-offline"></a>VM'yi çevrimdışı onarın

1. [Bir kurtarma VM'si sistem diski](../windows/troubleshoot-recovery-disks-portal.md).

2. Kurtarma VM'sini bir Uzak Masaüstü Bağlantısı'nı başlatın.

3. Disk olarak işaretlenmiş olduğundan emin olun **çevrimiçi** Disk Yönetimi Konsolu'nda. Bağlı sistem diskine atanmış sürücü harfini unutmayın.

4. Bir kopyasını oluşturmak **\Windows\System32\config** klasör değişiklikleri geri alma bir durumda gereklidir.

5. Kayıt Defteri Düzenleyicisi'ni (regedit.exe) kurtarma VM üzerinde başlatın.

6. Seçin **HKEY_LOCAL_MACHINE** anahtar ve ardından **dosya** > **yığını** menüsünde.

7. Sistem dosyayı bulun **\Windows\System32\config** klasör.

8. Seçin **açık**, türü **BROKENSYSTEM** genişletin adı için **HKEY_LOCAL_MACHINE** anahtar ve adlı ek bir anahtar bulun **BROKENSYSTEM** .

9. Şu konuma gidin:

   ```
   HKLM\BROKENSYSTEM\ControlSet001\Control\Class\{4d36e972-e325-11ce-bfc1-08002be10318}
   ```

10. Her alt anahtarında (örneğin, 0000) incelemek **DriverDesc** olarak görüntülenen değer **Microsoft HYPER-V ağ bağdaştırıcısı**.

11. Alt anahtarında inceleyin **DriverVersion** sanal makinenin ağ bağdaştırıcısı sürücü sürümü olan değer.

12. Uygun güncelleştirmeyi indirin:

    - **10.0.14393.594**: [KB4073562](https://support.microsoft.com/help/4073562) ya da daha yeni bir güncelleştirme
    - **10.0.15063.0**: [KB4016240](https://support.microsoft.com/help/4016240) ya da daha yeni bir güncelleştirme

13. Sistem diski, güncelleştirmeyi indirin bir kurtarma sanal makinesinde veri diski olarak ekleyin.

14. Sanal makinede güncelleştirmeyi yüklemek için aşağıdaki komutu çalıştırın:

    ```
    dism /image:<OS Disk letter>:\ /add-package /packagepath:c:\temp\<KB .msu or .cab>
    ```

15. Yığınlar kaldırmak için aşağıdaki komutu çalıştırın:

    ```
    reg unload HKLM\BROKENSYSTEM
    ```

16. [VM yeniden oluşturma ve sistem diskini](../windows/troubleshoot-recovery-disks-portal.md).

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun

Hala yardıma ihtiyacınız varsa [Azure desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
