---
title: "StorSimple cihaz modunu değiştirme | Microsoft Docs"
description: "StorSimple cihaz modlarını tanımlar ve StorSimple için Windows PowerShell aygıt modunu değiştirmek için nasıl kullanılacağını açıklar."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: e9d7d277-8a2f-45eb-9fef-355486e14cbc
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2016
ms.author: alkohli
ms.openlocfilehash: 33c65bf2eecff3914f3227c76f7d638a4507e1f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-device-mode-on-your-storsimple-device"></a>StorSimple Cihazınızda aygıt modunu değiştirme
Bu makale, StorSimple Cihazınızı çalışabilir çeşitli modlar kısa bir açıklamasını sağlar. StorSimple Cihazınızı üç modda çalışabilir: normal, Bakım ve kurtarma. 

Bu makaleyi okuduktan sonra şunları öğrenmiş olacaksınız:

* StorSimple cihaz modu
* StorSimple cihazı hangi modu anlamak nasıl kullanılıyor
* Bakım modu normal arasında değiştirme ve *tersi*

Yukarıdaki yönetim görevlerini yalnızca StorSimple Cihazınızı Windows PowerShell arabirimi gerçekleştirilebilir.

## <a name="about-storsimple-device-modes"></a>StorSimple cihaz modları hakkında
StorSimple Cihazınızı normal, Bakım veya kurtarma modunda çalışır. Bu modların her birinde kısaca aşağıda açıklanmıştır.

### <a name="normal-mode"></a>Normal modda
Bu, normal işlem modu için tam olarak yapılandırılmış bir StorSimple cihazı olarak tanımlanır. Varsayılan olarak, Cihazınızı normal modda olmalıdır.

### <a name="maintenance-mode"></a>Bakım modu
Bazen StorSimple cihazı bakım moduna alındığında gerekebilir. Bu mod cihaz üzerinde bakım gerçekleştirmek ve disk yazılımla ilgili olanlar gibi kesintiye uğratan güncelleştirmeleri yüklemenize olanak sağlar.

StorSimple için Windows PowerShell aracılığıyla yalnızca bakım moduna sistem koyabilirsiniz. Tüm g/ç istekleri bu modda duraklatıldı. Geçici olmayan rasgele erişim belleği (NVRAM) gibi hizmetleri veya Küme hizmeti de durdurulur. İki denetleyiciye girin ya da bu modundan çıkmak yeniden başlatılır. Bakım modu çıktığınızda, tüm hizmetleri devam eder ve iyi durumda olmalıdır. Bu birkaç dakika sürebilir.

> [!NOTE]
> **Bakım modu yalnızca düzgün çalışan bir aygıtta desteklenir. İçinde bir veya iki denetleyicilerinin çalışmıyor bir aygıtta desteklenmiyor.**
> </br>
> 
> 

### <a name="recovery-mode"></a>Kurtarma moduna
Kurtarma moduna "Ağ desteği ile Windows güvenli modda" olarak tanımlanabilir. Kurtarma moduna Microsoft Support ekibi prosese ve bunları sistemde tanılama işlemleri gerçekleştirmek sağlar. Birincil kurtarma modunda sistem günlüklerini hedefidir.

Sisteminizi kurtarma moduna girer, sonraki adımlar için Microsoft Support başvurmanız gerekir. Daha fazla bilgi için Git [Microsoft Destek birimine başvurun](storsimple-contact-microsoft-support.md).

> [!NOTE]
> **Cihaz kurtarma modunda yerleştirilemiyor. Aygıt hatalı bir durumda, cihaz Microsoft Support personeli inceleyebilirsiniz onu bir duruma getirmek kurtarma modunda çalışır.**
> 
> 

## <a name="determine-storsimple-device-mode"></a>StorSimple cihaz modunu belirler
#### <a name="to-determine-the-current-device-mode"></a>Geçerli cihaz modu belirlemek için
1. İçindeki adımları izleyerek cihaz seri konsoluna oturum [kullan cihaz seri konsoluna bağlanmak için PuTTY](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. Cihaz seri konsol menüsünde başlık iletisi bakın. Bu ileti, açıkça cihazın Bakım veya kurtarma modunda olup olmadığını gösterir. İleti sistemi moduna ilgili belirli bir bilgi içermiyorsa, cihaz normal modda çalışır.

## <a name="change-the-storsimple-device-mode"></a>StorSimple cihaz modunu değiştirme
StorSimple cihazı Bakımı yap veya Bakım modu güncelleştirmeleri yüklemek için (modundan normal) bakım moduna yerleştirebilirsiniz. Girin veya bakım modundan çıkmak için aşağıdaki yordamları gerçekleştirin.

> [!IMPORTANT]
> Bakım modu girmeden önce erişerek her iki cihaz denetleyicilerinin sağlıklı olduğunu doğrula **donanım durum** üzerinde **Bakım** Klasik Azure portalındaki sayfası. Bir veya iki denetleyicileri sağlıklı emin değilseniz, sonraki adımlar için Microsoft Support başvurun. Daha fazla bilgi için Git [Microsoft Destek birimine başvurun](storsimple-contact-microsoft-support.md).
> 
> 

#### <a name="to-enter-maintenance-mode"></a>Bakım modu girmek için
1. İçindeki adımları izleyerek cihaz seri konsoluna oturum [kullan cihaz seri konsoluna bağlanmak için PuTTY](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. Seri konsol menüsünde seçeneği 1, **oturum oturum tam erişim**. İstendiğinde, sağlayın **cihaz Yöneticisi parolası**. Varsayılan parola: `Password1`.
3. Komut istemine yazın 
   
    `Enter-HcsMaintenanceMode`
4. Bakım modu tüm g/ç istekleri kesintiye ve klasik Azure portalı bağlantısı sever ve onaylamanız istenir bildiren bir uyarı iletisi görürsünüz. Tür **Y** bakım moduna girmek için.
5. Hem denetleyicileri yeniden başlatılır. Yeniden başlatma tamamlandıktan sonra aygıtın bakım modunda olduğunu gösteren başka bir ileti görüntülenir.

#### <a name="to-exit-maintenance-mode"></a>Bakım modundan çıkmak için
1. Cihaz seri konsoluna oturum açın. Cihazınızı bakım modunda olduğundan başlık iletisi doğrulayın.
2. Komut istemine yazın:
   
    `Exit-HcsMaintenanceMode`
3. Bir uyarı iletisi ve bir onay iletisi görüntülenir. Tür **Y** bakım modundan çıkmak için.
4. Hem denetleyicileri yeniden başlatılır. Yeniden başlatma tamamlandıktan sonra aygıtın normal modda olduğunu gösteren başka bir ileti görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [normal ve Bakım modu güncelleştirmeleri uygulamak](storsimple-update-device.md) , StorSimple Cihazınızda.

