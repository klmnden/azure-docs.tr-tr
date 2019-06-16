---
title: Windows Azure sanal makinesinde döngü yeniden | Microsoft Docs
description: Windows başlatma döngüsünde sorunlarını gidermeyi öğrenin | Microsoft Docs
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
ms.date: 10/15/2018
ms.author: genli
ms.openlocfilehash: 1c97b1da094b759ccf85f310ceec4c7abfd91b9b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65472286"
---
# <a name="windows-reboot-loop-on-an-azure-vm"></a>Azure VM'de Windows başlatma döngüsünde
Bu makalede, Microsoft azure'da Windows sanal makine (VM) üzerinde karşılaşabileceğiniz başlatma döngüsünde açıklanır.

## <a name="symptom"></a>Belirti

Kullanırken [önyükleme tanılaması](./boot-diagnostics.md) bir VM'nin bir ekran görüntüleri almak için sanal makine önyükleme ancak önyükleme işlemi kesintiye ve işlem başlıyorum bulun.

![Başlangıç ekranı 1](./media/troubleshoot-reboot-loop/start-screen-1.png)

## <a name="cause"></a>Nedeni

Başlatma döngüsünde aşağıdakilerden biri nedeniyle oluşur:

### <a name="cause-1"></a>Neden 1

Önemli olarak işaretlenmiş bir üçüncü taraf hizmeti mevcuttur ve başlatılamaz. Bu, işletim sisteminin yeniden neden olur.

### <a name="cause-2"></a>Neden 2

İşletim sistemi için bazı değişiklikler yapıldı. Genellikle, bu güncelleştirme yüklemesi, uygulama yükleme veya yeni bir ilke ilgilidir. Ek bilgi için aşağıdaki günlüklere bakın gerekebilir:

- Olay Günlükleri
- CBS.logWindows
- Update.log

### <a name="cause-3"></a>Neden 3

Dosya Sistemi Bozulması bu neden olabilir. Ancak, tanılama ve işletim sistemi Bozulması neden olan değişikliği belirlemek zor olabilir.

## <a name="solution"></a>Çözüm

Bu sorunu çözmek için [işletim sistemi diskini yedekleme](../windows/snapshot-copy-managed-disk.md), ve [kurtarma VM için işletim sistemi diski](../windows/troubleshoot-recovery-disks-portal.md)ve çözüm seçenekleri uygun şekilde izleyin veya tek tek çözümleri deneyin.

### <a name="solution-for-cause-1"></a>Çözüm nedeni 1 için

1. İşletim sistemi diski, çalışan VM bağlı sonra disk olarak işaretlenmiş emin olun **çevrimiçi** içeren bölümün sürücü harfini unutmayın ve Disk Yönetimi'nde konsol **\Windows** klasör.

2. Disk ayarlanırsa **çevrimdışı**, ayarlayabilirsiniz **çevrimiçi**.

3. Bir kopyasını oluşturmak **\Windows\System32\config** klasör durumda değişiklikleri geri alma gereklidir.

4. VM kurtarma üzerinde Windows Kayıt Defteri Düzenleyicisi'ni (regedit) açın.

5. Seçin **HKEY_LOCAL_MACHINE** anahtar ve ardından **dosya** > **yığını** menüsünde.

6. Sistem dosyasına göz atın **\Windows\System32\config** klasör.

7. Seçin **açık**, türü **BROKENSYSTEM** genişletin adı için **HKEY_LOCAL_MACHINE** anahtarı ve ardından adlı ek bir anahtar göreceksiniz **BROKENSYSTEM** .

8. Bilgisayar önyükleme hangi ControlSet denetleyin. Aşağıdaki kayıt defteri anahtarı numarası göreceksiniz.

    `HKEY_LOCAL_MACHINE\BROKENSYSTEM\Select\Current`

9. VM aracısı aşağıdaki kayıt defteri anahtarı aracılığıyla kritikliği olduğunu denetleyin.

    `HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Services\RDAgent\ErrorControl`

10. Kayıt defteri anahtarının değeri olarak ayarlanmazsa, **2**sonraki azaltma gidin.

11. Kayıt defteri anahtarının değerini ayarlanırsa **2**, ardından değerini değiştirmek **2** için **1**.

12. Aşağıdaki anahtarlardan herhangi birine var ve bunlar değeri varsa **2** veya **3**ve ardından bu değerleri değiştirmek **1** buna göre:

    - `HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Services\AzureWLBackupCoordinatorSvc\ErrorControl`
    - `HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Services\AzureWLBackupInquirySvc\ErrorControl`
    - `HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Services\AzureWLBackupPluginSvc\ErrorControl`

13. Seçin **BROKENSYSTEM** anahtar ve ardından **dosya** > **yığını** menüsünde.

14. İşletim sistemi diski sorun giderme sanal makineden çıkarın.

15. Diskin sorun giderme sanal makineden kaldırın ve Azure bu diski yayın yaklaşık 2 dakika bekleyin.

16. [İşletim sistemi diskinden yeni bir VM oluşturmak](../windows/create-vm-specialized.md).

17. Sorun çözüldükten sonra yeniden yüklemeniz gerekebilir [RDAgent](https://blogs.msdn.microsoft.com/mast/2014/04/07/install-the-vm-agent-on-an-existing-azure-vm/) (WaAppAgent.exe).

### <a name="solution-for-cause-2"></a>Neden 2 çözümü

Sanal Makinenin bilinen son iyi yapılandırmaya geri yükleme, adımları [bilinen son iyi yapılandırma ile Azure Windows VM başlatma](https://support.microsoft.com/help/4016731/).

### <a name="solution-for-cause-3"></a>Çözüm nedeni 3
>[!NOTE]
>Aşağıdaki yordam, yalnızca son kaynak olarak kullanılmalıdır. Regback geri erişim makineye geri, ancak veri kayıt defteri kovanı zaman damgası ve geçerli gün arasında kayıp olduğundan işletim sistemi kararlı olarak kabul edilmez. Yeni bir VM oluşturun ve veri taşımak için ilave planlar yapmanıza gerek.

1. Bir sorun giderme sanal makinesine disk bağlandıktan sonra disk olarak işaretlenmiş emin olun **çevrimiçi** Disk Yönetimi Konsolu'nda.

2. Bir kopyasını oluşturmak **\Windows\System32\config** klasör durumda değişiklikleri geri alma gereklidir.

3. Dosyaları kopyalama **\Windows\System32\config\regback** klasör ve dosyaları değiştirin **\Windows\System32\config** klasör.

4. Diskin sorun giderme sanal makineden kaldırın ve Azure bu diski yayın yaklaşık 2 dakika bekleyin.

5. [İşletim sistemi diskinden yeni bir VM oluşturmak](../windows/create-vm-specialized.md).


