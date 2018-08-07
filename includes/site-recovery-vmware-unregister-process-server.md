---
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: include
ms.date: 08/06/2018
ms.author: ramamill
ms.openlocfilehash: 81390d38b4c0c38b7ac6883ae2bf18c64542fa00
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39582786"
---
Bir işlem sunucusunun kaydını kaldırma adımları, Yapılandırma Sunucusu’ndaki bağlantı durumuna bağlı olarak farklılık gösterir.

### <a name="unregister-a-process-server-that-is-in-a-connected-state"></a>Bağlı durumdaki işlem sunucusunun kaydını kaldırma

1. İşlem sunucusunda Yönetici olarak uzaktan oturum açın.
2. **Denetim Masası**’nı başlatıp **Programlar > Program kaldır**’ı açın
3. **Microsoft Azure Site Recovery Yapılandırması/İşlem Sunucusu** adlı programı kaldırın
4. Adım 3 tamamlandıktan sonra **Microsoft Azure Site Recovery Yapılandırması/İşlem Sunucusu Bağımlılıkları**’nı kaldırabilirsiniz

### <a name="unregister-a-process-server-that-is-in-a-disconnected-state"></a>Bağlantısı kesik durumdaki işlem sunucusunun kaydını kaldırma

> [!WARNING]
> İşlem Sunucusunun yüklü olduğu sanal makinenin canlandırılması mümkün değilse aşağıdaki adımlar kullanılmalıdır.

1. Yapılandırma sunucunuzda yönetici olarak oturum açın.
2. Bir Yönetici komut istemi açın ve `%ProgramData%\ASR\home\svsystems\bin` dizinine gidin.
3. Şimdi komutu çalıştırın.

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
4. Yukarıdaki komutu (olabilir birden çok yinelenen girişler durumunda) işlem sunucuları listesi sağlar Seri number(S.No), IP adresi (IP), VM adı ile hangi işlem sunucusu dağıtılır (ad), aşağıda gösterildiği gibi Kalp beat VM'nin (sinyal aralığı).
    ![Kaydı-cmd](media/site-recovery-vmware-unregister-process-server/Unregister-cmd.PNG)
5. Şimdi, seri numarası, istediğiniz işlem sunucusunun kaydını silmek için girin.
6. Bu işlem sunucusunun ayrıntılarını sistemden temizler ve iletisini görüntüler: **sunucu-adı kaydı başarıyla silindi > (sunucu IP adresi)**

