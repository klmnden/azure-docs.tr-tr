---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 926fb3e9a2c09d30da549330842d8b7e185674ae
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66171760"
---
**vCenter Ekle** menüsünde vSphere konağı veya vCenter sunucusu için bir kolay ad belirtin ve ardından sunucunun IP adresini ya da FQDN’sini belirtin. VMware sunucularınız farklı bir bağlantı noktasındaki istekleri dinleyecek şekilde yapılandırılmadıkça bağlantı noktasını 443 olarak bırakın. VMware vCenter veya vSphere ESXi sunucusuna bağlanacak hesabı seçin. **Tamam**'ı tıklatın.

    ![VMware](./media/site-recovery-add-vcenter/vmware-server.png)

   > [!NOTE]
   > VMware vCenter sunucusunda veya değil, vCenter veya konak sunucusu üzerinde yönetici ayrıcalıklarına sahip olduğundan emin olun bir hesapla VMware vSphere konağını ekliyorsanız, hesabın şu ayrıcalıklarının etkin olduğundan: Veri Merkezi, veri deposu, klasör, konak, ağ, kaynak, sanal makine ve vSphere dağıtılmış anahtarı. Ayrıca, VMware vCenter sunucusunda Depolama görünümleri ayrıcalığının etkin olması gerekir.
