---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 9bd2ee708f7d27cff5d07ca7f86d925ca6d2741d
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165291"
---
<!--author=SharS last changed: 12/01/15-->

#### <a name="to-enter-maintenance-mode"></a>Bakım moduna girmek için
1. Seri konsol menüsünde seçeneği 1 **tam erişimle oturum açmak**.
2. Parolayı yazın. Varsayılan parola **Password1**.
3. Komut istemine yazın
   
     `Enter-HcsMaintenanceMode`
4. Bakım modu tüm g/ç istekleri dengesini bozarsınız hem Klasik Azure portalında bağlantı sever ve onaylamanız istenir bildiren bir uyarı iletisi görürsünüz. Tür **Y** bakım moduna girmek için.
   
    Her iki denetleyicilerinin yeniden başlatılır. Yeniden başlatma tamamlandıktan sonra cihaz bakım modunda olduğunu gösteren başka bir ileti görüntülenir.

