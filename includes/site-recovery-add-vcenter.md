---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 8f7eae06947a40117f3952a0a5624c22df750b34
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165802"
---
* **vCenter Ekle** menüsünde vSphere konağı veya vCenter sunucusu için bir kolay ad belirtin ve ardından sunucunun IP adresini ya da FQDN’sini belirtin. VMware sunucularınız farklı bir bağlantı noktasındaki istekleri dinleyecek şekilde yapılandırılmadıkça bağlantı noktasını 443 olarak bırakın. VMware vCenter veya vSphere ESXi sunucusuna bağlanacak hesabı seçin. **Tamam** düğmesine tıklayın.

    ![VMware](./media/site-recovery-add-vcenter/vmware-server.png)

   > [!NOTE]
   > vCenter veya konak sunucusu üzerinde yönetici ayrıcalıklarına sahip olmayan bir hesapla VMware vCenter sunucusunu veya VMware vSphere konağını ekliyorsanız, hesabın şu ayrıcalıklarının etkin olduğundan emin olun: Veri Merkezi, Veri Deposu, Klasör, Konak, Ağ, Kaynak, Sanal makine ve vSphere Dağıtılmış Anahtarı. Ayrıca, VMware vCenter sunucusunda Depolama görünümleri ayrıcalığının etkin olması gerekir.
