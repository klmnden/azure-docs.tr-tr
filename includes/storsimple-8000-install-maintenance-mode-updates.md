---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 560c9c177bfa693580979101e5b9343fcff7fe40
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188567"
---
### <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell aracılığıyla Bakım modu güncelleştirmeleri yükle

StorSimple cihazını Bakım modu güncelleştirmeleri uygulayabilir, tüm g/ç istekleri duraklatıldı. Geçici olmayan rastgele erişim belleği (NVRAM) gibi hizmetler veya Küme hizmeti durdurulur. Her iki denetleyicilerinin girin ya da bu modundan çıkmak yeniden başlatın. Bu modundan çıkmak, tüm hizmetleri sürdürme ve iyi durumda. (Bu işlem birkaç dakika sürebilir.)

> [!IMPORTANT]
> * Bakım modu girmeden önce her iki cihaz denetleyicisinin Azure portalında sağlıklı olduğunu doğrulayın. Denetleyicinin sağlam, değilse [Microsoft Destek'e başvur](../articles/storsimple/storsimple-8000-contact-microsoft-support.md) sonraki adımlar için.
> * Bakım modunda olduğunda, öncelikle bir denetleyici ve diğer denetleyiciye güncelleştirmeniz gerekiyor.

1. Seri konsoluna bağlanmak için PuTTY kullanın. [Seri konsola bağlanmak için PuTTy kullanma](../articles/storsimple/storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console) bölümündeki ayrıntılı yönergeleri izleyin. Komut isteminde **Enter** tuşuna basın. Seçenek 1 **tam erişimle oturum açmak**.

2. Denetleyici bakım moduna almak için şunu yazın:
    
    `Enter-HcsMaintenanceMode`

    Her iki denetleyici de bakım moduna yeniden başlatın.

3. Bakım modu güncelleştirmeleri yükleyin. Şunu yazın:

    `Start-HcsUpdate`

    Onaylamanız istenir. Güncelleştirmeleri onayladıktan sonra bunlar şu anda erişen denetleyicisine yüklenir. Güncelleştirmeler yüklendikten sonra denetleyici yeniden başlatılır.

4. Güncelleştirmelerin durumunu izleyin. Geçerli denetleyici güncelleştiriyor ve diğer tüm komutların işleyebilir değil eş denetleyicisine oturum açın. Şunu yazın:

    `Get-HcsUpdateStatus`

    Varsa `RunInProgress` olduğu `True`, güncelleştirme devam ediyor. Varsa `RunInProgress` olduğu `False`, güncelleştirme tamamlandığını gösterir.

5. Disk üretici yazılımı güncelleştirmeleri başarıyla uygulanıp ve güncelleştirilmiş denetleyicisi yeniden başlatıldıktan sonra disk üretici yazılımı sürümünü doğrulayın. Güncelleştirilmiş denetleyicide yazın:

    `Get-HcsFirmwareVersion`
   
    Beklenen disk üretici yazılımı sürümleri şunlardır:  `XMGJ, XGEG, KZ50, F6C2, VR08, N003, 0107`

6. Bakım modundan çıkın. Her bir cihaz denetleyicisi için aşağıdaki komutu yazın:

    `Exit-HcsMaintenanceMode`

    Bakım modundan çıktığınızda denetleyiciler yeniden başlatılır.

7. Azure portalına dönün. Yüklediğiniz Bakım modu güncelleştirmeleri 24 saat boyunca portalda gösterilmeyebilir.