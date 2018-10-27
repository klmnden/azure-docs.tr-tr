---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 77fc95c53ba89c3905ebd1aec785e22f42339369
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164788"
---
<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-regular-hotfixes-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell aracılığıyla normal düzeltmeleri yüklemek için
1. Cihaz seri konsoluna bağlanın. Daha fazla bilgi için [1. adım: seri konsoluna bağlanmak](../articles/storsimple/storsimple-update-device.md#step1).
2. Seri konsol menüsünde seçeneğini 1 **tam erişimle oturum açmak**. Parolayı yazın. Varsayılan parola **Password1**.
3. Komut istemine şunları yazın:
   
    ```
    Start-HcsHotfix
    ```
   
    > [!IMPORTANT]
    >
    > Bu komut, yalnızca normal düzeltmeler için geçerlidir. Bu komut yalnızca bir denetleyicisinde çalıştırın, ancak her iki denetleyicilerinin güncelleştirilir.
    > Denetleyici yük devretmesi güncelleştirme işlemi sırasında fark edebilirsiniz; Ancak, yük devretme işlemi ya da sistem kullanılabilirliği etkilemez.

4. İstendiğinde, düzeltme dosyalarını içeren ağ paylaşılan klasörün yolunu girin.
5. Onaylamanız istenir. Tür **Y** düzeltme yüklemeye devam etmek için.

