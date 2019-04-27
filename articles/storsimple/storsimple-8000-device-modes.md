---
title: StorSimple cihaz modunu değiştirme | Microsoft Docs
description: StorSimple cihaz modları ve cihaz modunu değiştirmek için StorSimple için Windows PowerShell'i kullanmayı açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: e55964beff48df6ce24d99c01975d39b662f1612
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60576100"
---
# <a name="change-the-device-mode-on-your-storsimple-device"></a>StorSimple Cihazınızda cihaz modunu değiştirme

Bu makalede, StorSimple Cihazınızı çalışabilir çeşitli modlar kısa bir açıklamasını sağlar. StorSimple Cihazınızı üç modda çalışabilir: normal, Bakım ve kurtarma.

Bu makaleyi okuduktan sonra şunları öğrenmiş olacaksınız:

* StorSimple cihaz modu
* StorSimple cihazının hangi modu şekil nasıl bulunduğu
* Normal bakım moduna geçmek nasıl ve *tersi*

Yukarıdaki yönetim görevleri yalnızca StorSimple cihazınızın Windows PowerShell arabirimi üzerinden gerçekleştirilebilir.

## <a name="about-storsimple-device-modes"></a>StorSimple cihaz modları hakkında

StorSimple Cihazınızı normal, bakım ya da kurtarma modunda çalışır. Her iki mod kısaca aşağıda açıklanmıştır.

### <a name="normal-mode"></a>Normal mod

Bu, normal çalışma modu için tam olarak yapılandırılmış bir StorSimple cihazı olarak tanımlanır. Varsayılan olarak, Cihazınızı normal modda olmalıdır.

### <a name="maintenance-mode"></a>Bakım modu

Bazen StorSimple cihazını bakım moduna alındığında gerekebilir. Bu mod, cihaz üzerinde bakım gerçekleştirmek ve disk üretici yazılımı için ilgili olanlar gibi kesintiye uğratan güncelleştirmeleri yüklemenize olanak sağlar.

StorSimple için Windows PowerShell aracılığıyla yalnızca bakım moduna sistem koyabilirsiniz. Tüm g/ç istekleri bu modda duraklatıldı. Geçici olmayan rastgele erişim belleği (NVRAM) gibi hizmetler veya Küme hizmetini de durduruldu. Girin ya da bu modundan çıkmak her iki denetleyici yeniden başlatılır. Bakım modundan çıktığınızda, tüm hizmetleri devam eder ve iyi durumda olması gerekir. Bu birkaç dakika sürebilir.

> [!NOTE]
> **Bakım modu düzgün çalıştığını bir cihazda yalnızca desteklenir. İçinde bir veya iki denetleyicilerinin çalışmıyor bir cihazda desteklenmiyor.**


### <a name="recovery-mode"></a>Kurtarma modu

Kurtarma modu, "Ağ desteği ile Windows güvenli mod" olarak açıklanabilir. Kurtarma modunu Microsoft Support ekibine ilgilenir ve bunları sistemde tanılama işlemleri gerçekleştirmek izin verir. Kurtarma modunu birincil amacı, sistem günlüklerini sağlamaktır.

Sisteminizin kurtarma moduna girerse, sonraki adımlar için Microsoft Support başvurmanız gerekir. Daha fazla bilgi için Git [Microsoft Destek'e başvur](storsimple-8000-contact-microsoft-support.md).

> [!NOTE]
> **Cihaz kurtarma modunda yerleştirilemiyor. Aygıt hatalı bir durumda, cihazın Microsoft Support personeli inceleyebilirsiniz, bir duruma getirmek kurtarma modunda çalışır.**

## <a name="determine-storsimple-device-mode"></a>StorSimple cihaz modunu belirleme

#### <a name="to-determine-the-current-device-mode"></a>Geçerli cihaz modunu belirleme

1. Cihaz seri konsoluna adımları izleyerek oturum [kullan cihaz seri konsoluna bağlanmak için PuTTY](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
2. Cihazın seri konsol menüsünde bir başlık iletisi bakın. Bu ileti, cihaz bakım ya da kurtarma modunda olup olmadığını açıkça gösterir. İleti sistemi moduna ilgili belirli bilgilere içermiyorsa, cihazın normal modda çalışır.

## <a name="change-the-storsimple-device-mode"></a>StorSimple cihaz modunu değiştirme

StorSimple cihaz Bakımı yap veya Bakım modu güncelleştirmeleri yüklemek için bakım modundan (normal mod) içine yerleştirebilirsiniz. Girin veya bakım modundan çıkmak için aşağıdaki yordamları gerçekleştirin.

> [!IMPORTANT]
> Bakım modu girmeden önce erişerek her iki cihaz denetleyicilerinin sağlıklı olduğunu doğrulayın **cihaz Ayarları > donanım sistem durumu** cihazınızın Azure portalında. Bir veya iki denetleyicilerinin sağlıklı emin değilseniz, sonraki adımlar için Microsoft Support başvurun. Daha fazla bilgi için Git [Microsoft Destek'e başvur](storsimple-8000-contact-microsoft-support.md).
 

#### <a name="to-enter-maintenance-mode"></a>Bakım moduna girmek için

1. Cihaz seri konsoluna adımları izleyerek oturum [kullan cihaz seri konsoluna bağlanmak için PuTTY](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
2. Seri konsol menüsünde seçeneği 1 **tam erişimle oturum açmak**. İstendiğinde, sağlayın **cihaz Yöneticisi parolası**. Varsayılan parola: `Password1`.
3. Komut istemine yazın 
   
    `Enter-HcsMaintenanceMode`
4. Bakım modu tüm g/ç istekleri dengesini bozarsınız hem Azure portalında bağlantı sever ve onaylamanız istenir bildiren bir uyarı iletisi görürsünüz. Tür **Y** bakım moduna girmek için.
5. Her iki denetleyicilerinin yeniden başlatılır. Yeniden başlatma tamamlandıktan sonra seri konsol başlık cihaz bakım modunda olduğunu gösterir. Örnek çıktı aşağıda gösterilmiştir.

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

1. Cihaz seri konsoluna oturum açın. Cihazınızın bakım modunda olduğundan başlık iletisi doğrulayın.
2. Komut istemine şunları yazın:
   
    `Exit-HcsMaintenanceMode`
3. Bir uyarı iletisi ve bir onay iletisi görüntülenir. Tür **Y** bakım modundan çıkmak için.
4. Her iki denetleyicilerinin yeniden başlatılır. Yeniden başlatma tamamlandıktan sonra seri konsol başlık cihazın normal modda olduğunu gösterir. Örnek çıktı aşağıda gösterilmiştir.

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

Bilgi edinmek için nasıl [normal ve Bakım modu güncelleştirmeleri uygulayabilir](storsimple-update-device.md) StorSimple Cihazınızda.

