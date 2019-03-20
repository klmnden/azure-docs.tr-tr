---
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: include
ms.date: 03/11/2019
ms.author: ramamill
ms.openlocfilehash: 0d090f43b69b42a07f1c8949d1662e8e720f3cf4
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "57908544"
---
Bir işlem sunucusunun kaydını kaldırma adımları, Yapılandırma Sunucusu’ndaki bağlantı durumuna bağlı olarak farklılık gösterir.

### <a name="unregister-a-process-server-that-is-in-a-connected-state"></a>Bağlı durumdaki işlem sunucusunun kaydını kaldırma

1. İşlem sunucusunda Yönetici olarak uzaktan oturum açın.
2. Başlatma **Denetim Masası** açın **Programlar > program Kaldır**.
3. Adlı bir program Kaldır **Microsoft Azure Site Recovery Mobility hizmeti/ana hedef sunucusu**.
4. Adlı bir program Kaldır **Microsoft Azure Site Recovery yapılandırması/işlem sunucusu**.
5. Adım 3 ve 4 programlarında kaldırıldığında kaldırabilirsiniz **Microsoft Azure Site Recovery yapılandırması/işlem sunucusu bağımlılıkları**.

### <a name="unregister-a-process-server-that-is-in-a-disconnected-state"></a>Bağlantısı kesik durumdaki işlem sunucusunun kaydını kaldırma

> [!WARNING]
> Kullanım aşağıdaki işlem sunucusunun yüklü olduğu sanal makinenin canlandırılması mümkün değilse adımları.

1. Yapılandırma sunucunuzda yönetici olarak oturum açın.
2. Bir Yönetici komut istemi açın ve `%ProgramData%\ASR\home\svsystems\bin` dizinine gidin.
3. Şimdi komutu çalıştırın.

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
4. Yukarıdaki komutu (olabilir birden çok yinelenen girişler durumunda) işlem sunucuları listesi sağlar Seri number(S.No), IP adresi (IP), VM adı ile hangi işlem sunucusu dağıtılır (ad), aşağıda gösterildiği gibi Kalp beat VM'nin (sinyal aralığı).
    ![Kaydı-cmd](media/site-recovery-vmware-unregister-process-server/Unregister-cmd.PNG)
5. Şimdi, seri numarası, istediğiniz işlem sunucusunun kaydını silmek için girin.
6. Bu işlem sunucusunun ayrıntılarını sistemden temizler ve iletisini görüntüler: **Sunucu adı kaydı başarıyla silindi > (sunucu IP adresi)**

