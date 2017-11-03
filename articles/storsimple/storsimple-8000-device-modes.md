---
title: "StorSimple cihaz modunu değiştirme | Microsoft Docs"
description: "StorSimple cihaz modlarını tanımlar ve StorSimple için Windows PowerShell aygıt modunu değiştirmek için nasıl kullanılacağını açıklar."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: dd160ede1189b0de544c8cf5db3b13228d212419
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
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


### <a name="recovery-mode"></a>Kurtarma moduna

Kurtarma moduna "Ağ desteği ile Windows güvenli modda" olarak tanımlanabilir. Kurtarma moduna Microsoft Support ekibi prosese ve bunları sistemde tanılama işlemleri gerçekleştirmek sağlar. Birincil kurtarma modunda sistem günlüklerini hedefidir.

Sisteminizi kurtarma moduna girer, sonraki adımlar için Microsoft Support başvurmanız gerekir. Daha fazla bilgi için Git [Microsoft Destek birimine başvurun](storsimple-8000-contact-microsoft-support.md).

> [!NOTE]
> **Cihaz kurtarma modunda yerleştirilemiyor. Aygıt hatalı bir durumda, cihaz Microsoft Support personeli inceleyebilirsiniz onu bir duruma getirmek kurtarma modunda çalışır.**

## <a name="determine-storsimple-device-mode"></a>StorSimple cihaz modunu belirler

#### <a name="to-determine-the-current-device-mode"></a>Geçerli cihaz modu belirlemek için

1. İçindeki adımları izleyerek cihaz seri konsoluna oturum [kullan cihaz seri konsoluna bağlanmak için PuTTY](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
2. Cihaz seri konsol menüsünde başlık iletisi bakın. Bu ileti, açıkça cihazın Bakım veya kurtarma modunda olup olmadığını gösterir. İleti sistemi moduna ilgili belirli bir bilgi içermiyorsa, cihaz normal modda çalışır.

## <a name="change-the-storsimple-device-mode"></a>StorSimple cihaz modunu değiştirme

StorSimple cihazı Bakımı yap veya Bakım modu güncelleştirmeleri yüklemek için (modundan normal) bakım moduna yerleştirebilirsiniz. Girin veya bakım modundan çıkmak için aşağıdaki yordamları gerçekleştirin.

> [!IMPORTANT]
> Bakım modu girmeden önce erişerek her iki cihaz denetleyicilerinin sağlıklı olduğunu doğrula **Aygıt Ayarları > donanım durumu** Azure portalında, cihazınız için. Bir veya iki denetleyicileri sağlıklı emin değilseniz, sonraki adımlar için Microsoft Support başvurun. Daha fazla bilgi için Git [Microsoft Destek birimine başvurun](storsimple-8000-contact-microsoft-support.md).
 

#### <a name="to-enter-maintenance-mode"></a>Bakım modu girmek için

1. İçindeki adımları izleyerek cihaz seri konsoluna oturum [kullan cihaz seri konsoluna bağlanmak için PuTTY](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
2. Seri konsol menüsünde seçeneği 1, **oturum oturum tam erişim**. İstendiğinde, sağlayın **cihaz Yöneticisi parolası**. Varsayılan parola: `Password1`.
3. Komut istemine yazın 
   
    `Enter-HcsMaintenanceMode`
4. Bakım modu tüm g/ç istekleri kesintiye ve Azure portalına bağlantı sever ve onaylamanız istenir bildiren bir uyarı iletisi görürsünüz. Tür **Y** bakım moduna girmek için.
5. Hem denetleyicileri yeniden başlatılır. Yeniden başlatma tamamlandıktan sonra seri konsol başlık cihazın bakım modunda olduğunu belirtir. Örnek çıktı aşağıda gösterilmiştir.

```
    ---------------------------------------------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected to Controller0 - Passive
    ---------------------------------------------------------------

    Controller0>Enter-HcsMaintenanceMode
    Checking device state...

    In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
    [Y] Yes [N] No (Default is "Y"): Y

    <BOTH CONTROLLERS RESTART>

    -----------------------MAINTENANCE MODE------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected to Controller0 - Passive
    ---------------------------------------------------------------

    Serial Console Menu
    [1] Log in with full access
    [2] Log into peer controller with full access
    [3] Connect with limited access
    [4] Change language
    Please enter your choice>

```

#### <a name="to-exit-maintenance-mode"></a>Bakım modundan çıkmak için

1. Cihaz seri konsoluna oturum açın. Cihazınızı bakım modunda olduğundan başlık iletisi doğrulayın.
2. Komut istemine yazın:
   
    `Exit-HcsMaintenanceMode`
3. Bir uyarı iletisi ve bir onay iletisi görüntülenir. Tür **Y** bakım modundan çıkmak için.
4. Hem denetleyicileri yeniden başlatılır. Yeniden başlatma tamamlandıktan sonra seri konsol başlık aygıtın normal modda olduğunu gösterir. Örnek çıktı aşağıda gösterilmiştir.

```
    -----------------------MAINTENANCE MODE------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected to Controller0
    ---------------------------------------------------------------

    Controller0>Exit-HcsMaintenanceMode
    Checking device state...

    Before exiting maintenance mode, ensure that any updates that are required on both controllers have been applied. Failure to install on each controller could result in data corruption. Exiting maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to exit maintenance mode?
    [Y] Yes [N] No (Default is "Y"): Y

    <BOTH CONTROLLERS RESTART>

    ---------------------------------------------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected to Controller0 - Active
    ---------------------------------------------------------------

    Serial Console Menu
    [1] Log in with full access
    [2] Log into peer controller with full access
    [3] Connect with limited access
    [4] Change language
    Please enter your choice>
```

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [normal ve Bakım modu güncelleştirmeleri uygulamak](storsimple-update-device.md) , StorSimple Cihazınızda.

