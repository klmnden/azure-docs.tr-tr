---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: dc50f94ae9b207961a71480c2fc172e88db79cf4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61409979"
---
#### <a name="to-install-regular-updates-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell aracılığıyla düzenli güncelleştirmeler yüklemek için
1. Cihaz seri konsoluna açıp seçenek 1, **tam erişimle oturum açmak**. Parolayı yazın. Varsayılan parola *Password1*. 
2. Komut istemine şunları yazın:
   
     `Get-HcsUpdateAvailability`
   
    Güncelleştirmeler varsa ve yıkıcı veya kesintiye uğratmayan güncelleştirmelerin olup olmadığını size bildirilir.
3. Komut istemine şunları yazın:
   
     `Start-HcsUpdate`
   
    Güncelleştirme işlemini başlatacak.

> [!IMPORTANT]
> * Bu komut yalnızca düzenli güncelleştirmeler için geçerlidir. Bu komut yalnızca bir denetleyicisinde çalıştırın, ancak her iki denetleyicilerinin güncelleştirilir. 
> * Denetleyici yük devretmesi güncelleştirme işlemi sırasında fark edebilirsiniz; Ancak, yük devretme işlemi ya da sistem kullanılabilirliği etkilemez.
> 
> 

