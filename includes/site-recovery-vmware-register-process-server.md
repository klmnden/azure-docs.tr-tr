---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 04/28/2019
ms.author: raynew
ms.openlocfilehash: 088cd5447b1f96dbf172b5918c29e4f3293289a6
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67534737"
---
1. İşlem Sunucusu çalıştıran makineye Uzak Masaüstü bağlantısı kurun. 
2. Azure Site kurtarma işlemi sunucu yapılandırma aracını başlatmak için cspsconfigtool.exe'yi çalıştırın.
    - Araç, ilk işlem sunucusuna oturum açtığınızda otomatik olarak başlatılır.
    - Otomatik olarak açılmazsa, yalnızca masaüstü kısayoluna tıklayın.

3. İçinde **yapılandırma sunucusu FQDN veya IP**, işlem sunucusunu kaydetmek kullanılacak yapılandırma sunucusunun IP adresi veya adı belirtin.
4. İçinde **yapılandırma sunucusu bağlantı noktası**, 443 belirtildiğinden emin olun. Bu yapılandırma sunucusunun istekleri dinlediği bağlantı noktasıdır.
5. İçinde **bağlantı parolası**, yapılandırma sunucusunu ayarladıktan ayarlandığında belirttiğiniz parolayı belirtin. Parolayı bulmak için:
    -  Yapılandırma sunucusunda Site Recovery yükleme klasörüne gidin * *\home\svssystems\bin\** :
    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    ```
    - Çalıştırma aşağıdaki komutu geçerli parolayı yazdırmak için:
    ```
    genpassphrase.exe -n
    ```

6. İçinde **veri aktarımı bağlantı noktası**, özel bir bağlantı noktası belirtmediyseniz, varsayılan değeri bırakın.

7. Tıklayın **Kaydet** ayarları kaydedin ve işlem sunucusunu kaydetme.

    
    ![İşlem sunucusunu kaydetme](./media/site-recovery-vmware-register-process-server/register-ps.png)
