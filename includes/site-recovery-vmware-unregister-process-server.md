---
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: include
ms.date: 04/28/2019
ms.author: ramamill
ms.openlocfilehash: 00b0c1b1a40ad16db177916c57dba6e9d5a187a7
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66170013"
---
Belirli şartlarınıza adımları izleyin.

### <a name="unregister-a-connected-process-server"></a>Bir bağlı işlem sunucusunun kaydını kaldırma

1. İşlem sunucusunda yönetici olarak bir uzak bağlantı kurun.
2. İçinde **Denetim Masası**açın **Programlar > program Kaldır**.
3. Program Kaldır **Microsoft Azure Site Recovery Mobility hizmeti/ana hedef sunucusu**.
4. Program Kaldır **Microsoft Azure Site Recovery yapılandırması/işlem sunucusu**.
5. Adım 3 ve 4 programlarında kaldırılır sonra kaldırmak **Microsoft Azure Site Recovery yapılandırması/işlem sunucusu bağımlılıkları**.

### <a name="unregister-a-disconnected-process-server"></a>Bağlantısı kesilmiş işlem sunucusunun kaydını kaldırma

Yalnızca işlem sunucusunun yüklü olduğu makinenin canlandırılması mümkün ise aşağıdaki adımları kullanın.

1. Yapılandırma sunucusunda yönetici olarak oturum açın.
2. Bir yönetici komut istemi açın ve gidin `%ProgramData%\ASR\home\svsystems\bin`.
3. Bir veya daha fazla işlem sunucularının bir listesini almak için şu komutu çalıştırın.

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
    - S. Hayır: işlem sunucu seri numarası.
    - IP/adı: İşlem Sunucusu çalıştıran makinenin adını ve IP adresi.
    - Sinyal: İşlem sunucusu makinesi gelen son sinyal.
    ![Kaydı-cmd](media/site-recovery-vmware-unregister-process-server/Unregister-cmd.PNG)

4. Seri numarası kaydını kaldırmak istediğiniz işlem sunucusu belirtin.
5. İşlem Sunucusu'nun kaydını kaldırma tüm ayrıntılarını sistemden kaldırmanız ve iletisini görüntüler: **Sunucu adı kaydı başarıyla silindi > (sunucu IP adresi)**

