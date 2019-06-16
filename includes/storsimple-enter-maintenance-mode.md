---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: d35f0ef783a2c48f8211657bc8829635c19495aa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66171969"
---
#### <a name="to-enter-maintenance-mode"></a>Bakım moduna girmek için
1. Seri konsol menüsünde seçeneği 1 **tam erişimle oturum açmak**.
2. Parolayı yazın. Varsayılan parola **Password1**.
3. Komut istemine yazın
   
     `Enter-HcsMaintenanceMode`
4. Bakım modu tüm g/ç istekleri dengesini bozarsınız hem Klasik Azure portalında bağlantı sever ve onaylamanız istenir bildiren bir uyarı iletisi görürsünüz. Tür **Y** bakım moduna girmek için.
   
    Her iki denetleyicilerinin yeniden başlatılır. Yeniden başlatma tamamlandıktan sonra cihaz bakım modunda olduğunu gösteren başka bir ileti görüntülenir.

