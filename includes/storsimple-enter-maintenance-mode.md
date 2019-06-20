---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: d35f0ef783a2c48f8211657bc8829635c19495aa
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188398"
---
#### <a name="to-enter-maintenance-mode"></a>Bakım moduna girmek için
1. Seri konsol menüsünde seçeneği 1 **tam erişimle oturum açmak**.
2. Parolayı yazın. Varsayılan parola **Password1**.
3. Komut istemine yazın
   
     `Enter-HcsMaintenanceMode`
4. Bakım modu tüm g/ç istekleri dengesini bozarsınız hem Klasik Azure portalında bağlantı sever ve onaylamanız istenir bildiren bir uyarı iletisi görürsünüz. Tür **Y** bakım moduna girmek için.
   
    Her iki denetleyicilerinin yeniden başlatılır. Yeniden başlatma tamamlandıktan sonra cihaz bakım modunda olduğunu gösteren başka bir ileti görüntülenir.

