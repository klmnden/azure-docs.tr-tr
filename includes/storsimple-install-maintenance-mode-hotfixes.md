---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 5e4921be3116754f146ed0845513010f2642c97b
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165287"
---
<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell aracılığıyla Bakım modu düzeltmelerini yüklemek için
> [!IMPORTANT]
> Bakım modunda bir denetleyici ve ardından diğer denetleyiciye ilk düzeltmesini uygulamanız gerekir.
> 
> 

1. Cihaz, bakım moduna yerleştirir. Bkz: [2. adım: girin Bakım modu](../articles/storsimple/storsimple-update-device.md#step2) bakım moduna girmek yönergeler.
2. Düzeltmeyi uygulamak için şunu yazın:
   
     `Start-HcsHotfix` 
3. İstendiğinde, düzeltme dosyalarını içeren ağ paylaşılan klasörün yolunu girin.
4. Onaylamanız istenir. Tür **Y** düzeltme yüklemeye devam etmek için.
5. Diğer denetleyiciye bir denetleyicisinde düzeltmeyi uyguladıktan sonra oturum açın. Önceki denetleyici için yaptığınız gibi düzeltmeyi uygulayın.
6. Düzeltmeleri uyguladıktan sonra bakım modundan çıkın. Bkz: [4. adım: çıkış Bakım modu](../articles/storsimple/storsimple-update-device.md#step4) yönergeler için.

