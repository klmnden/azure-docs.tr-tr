---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 01612b32e6c1b363df8a5c70405d0c709210328e
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165978"
---
<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell aracılığıyla Bakım modu güncelleştirmeleri yüklemek için
1. Henüz yapmadıysanız, seçenek 1 ve cihaz seri konsoluna erişim **tam erişimle oturum açmak**. 
2. Parolayı yazın. Varsayılan parola **Password1**.
3. Komut istemine şunları yazın:
   
     `Get-HcsUpdateAvailability` 
4. Güncelleştirmeler varsa ve yıkıcı veya kesintiye uğratmayan güncelleştirmelerin olup olmadığını size bildirilir. Kesintiye uğratan güncelleştirmeleri uygulamak için cihaz, bakım moduna almanız gerekir. Bkz: [2. adım: girin Bakım modu](../articles/storsimple/storsimple-update-device.md#step2) yönergeler için.
5. Cihazınızın bakım modunda olduğunda komut isteminde aşağıdakini yazın: `Start-HcsUpdate`
6. Onaylamanız istenir. Güncelleştirmeleri onayladıktan sonra şu anda erişen denetleyicisine yüklenir. Güncelleştirmeler yüklendikten sonra denetleyici yeniden başlatılır. 
7. Güncelleştirmelerin durumunu izleyin. Şunu yazın:
   
    `Get-HcsUpdateStatus`
   
    Varsa `RunInProgress` olduğu `True`, güncelleştirme devam ediyor. Varsa `RunInProgress` olduğu `False`, güncelleştirme tamamlandığını gösterir.  
8. Geçerli denetleyici üzerinde güncelleştirme yüklendikten ve yeniden başlatıldıktan diğer denetleyiciye bağlanmak ve 1 ile 6 arasındaki adımları uygulayın.
9. Her iki denetleyicilerinin güncelleştirildikten sonra bakım modundan çıkın. Bkz: [4. adım: çıkış Bakım modu](../articles/storsimple/storsimple-update-device.md#step4) yönergeler için.

