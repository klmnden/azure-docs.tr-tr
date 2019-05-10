---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 04/28/2019
ms.author: raynew
ms.openlocfilehash: cf39baf34096691144181332566cf567ebc02310
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925593"
---
1. İşlem Sunucusu çalıştıran makineye Uzak Masaüstü bağlantısı kurun. 
2. Azure Site kurtarma işlemi sunucu yapılandırma aracını başlatmak için cspsconfigtool.exe'yi çalıştırın.
    - Araç, ilk işlem sunucusuna oturum açtığınızda otomatik olarak başlatılır.
    - Otomatik olarak açılmazsa, yalnızca masaüstü kısayoluna tıklayın.

3. İçinde **yapılandırma sunucusu FQDN veya IP**, işlem sunucusunu kaydetmek kullanılacak yapılandırma sunucusunun IP adresi veya adı belirtin.
4. İçinde **yapılandırma sunucusu bağlantı noktası**, 443 belirtildiğinden emin olun. Bu yapılandırma sunucusunun istekleri dinlediği bağlantı noktasıdır.
5. İçinde **bağlantı parolası**, yapılandırma sunucusunu ayarladıktan ayarlandığında belirttiğiniz parolayı belirtin. Parolayı bulmak için:
    -  Yapılandırma sunucusunda Site Recovery yükleme klasörüne gidin **\home\svssystems\bin\**. 
    - Bu komutu çalıştırın: **genpassphrase.exe.n**. Bu, daha sonra Not Parola konumunu gösterir.

6. İçinde **veri aktarımı bağlantı noktası**, özel bir bağlantı noktası belirtmediyseniz, varsayılan değeri bırakın.

7. Tıklayın **Kaydet** ayarları kaydedin ve işlem sunucusunu kaydetme.

    
    ![İşlem sunucusunu kaydetme](./media/site-recovery-vmware-register-process-server/register-ps.png)
