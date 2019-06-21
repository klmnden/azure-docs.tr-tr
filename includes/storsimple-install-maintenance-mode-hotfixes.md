---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 2d0fd904dd704c704662192e1e92fe403f0971c5
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188401"
---
#### <a name="to-install-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell aracılığıyla Bakım modu düzeltmelerini yüklemek için
> [!IMPORTANT]
> Bakım modunda bir denetleyici ve ardından diğer denetleyiciye ilk düzeltmesini uygulamanız gerekir.
> 
> 

1. Cihaz, bakım moduna yerleştirir. Bkz: [2. adım: Bakım moduna](../articles/storsimple/storsimple-update-device.md#step2) bakım moduna girmek yönergeler.
2. Düzeltmeyi uygulamak için şunu yazın:
   
     `Start-HcsHotfix` 
3. İstendiğinde, düzeltme dosyalarını içeren ağ paylaşılan klasörün yolunu girin.
4. Onaylamanız istenir. Tür **Y** düzeltme yüklemeye devam etmek için.
5. Diğer denetleyiciye bir denetleyicisinde düzeltmeyi uyguladıktan sonra oturum açın. Önceki denetleyici için yaptığınız gibi düzeltmeyi uygulayın.
6. Düzeltmeleri uyguladıktan sonra bakım modundan çıkın. Bkz: [4. adım: Bakım modundan çık](../articles/storsimple/storsimple-update-device.md#step4) yönergeler için.

