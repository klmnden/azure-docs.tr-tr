---
title: Azure VMware CloudSimple çözümüyle yönetmek için kullanılan temel kavramları
description: Azure VMware CloudSimple çözümüyle yönetmek için kullanılan temel kavramları açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 4/24/19
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 3eff61408cb190396987ace6dee21182cff4f25c
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165181"
---
# <a name="key-concepts-for-administration-of-azure-vmware-solution-by-cloudsimple"></a>Azure VMware CloudSimple çözümüyle yönetimi için temel kavramları

Azure VMware CloudSimple çözümüyle yönetmek, aşağıdaki kavramları bilinmesini gerektirir:

* Azure VMware çözümü CloudSimple - hizmet tarafından görüntülenen CloudSimple hizmeti
* Azure VMware çözümü CloudSimple - düğümünde görüntülenen CloudSimple düğümü
* CloudSimple özel bulut
* Ağ hizmeti
* Azure VMware çözümü CloudSimple - sanal makine tarafından görüntülenen CloudSimple sanal makine

## <a name="cloudsimple-service"></a>CloudSimple hizmeti

CloudSimple hizmetiyle oluşturabilir ve VMware çözümleriyle CloudSimple tarafından Azure portalından ilişkili tüm kaynakları yönetin. Bir hizmet kaynağı nerede hizmetini kullanmayı düşündüğünüz her bölgede oluşturun.

Daha fazla bilgi edinin [CloudSimple hizmet](cloudsimple-service.md).

## <a name="cloudsimple-node"></a>CloudSimple düğümü

VMware ESXi hiper yönetici dağıtıldığı bir ayrılmış, çıplak, Hiper yakınsanmış işlem ve depolama konak CloudSimple düğümüdür. Bu düğüm, ardından VMware vSphere, vCenter, Vsan'a ve NSX platformları eklenmiştir. CloudSimple ağ hizmetleri ve uç ağ hizmetlerinin de etkinleştirilir. Her düğüm oluşturmak için sağlayabileceğiniz işlem ve depolama kapasitesi, bir birim olarak hizmet veren [CloudSimple özel Bulutlar](cloudsimple-private-cloud.md). Sağlar veya CloudSimple hizmetin kullanılabildiği bir bölge içinde düğümler ayırmak.


Daha fazla bilgi edinin [CloudSimple düğümleri](cloudsimple-node.md).

## <a name="cloudsimple-private-cloud"></a>CloudSimple özel bulut

CloudSimple özel bulut kendi Yönetim etki alanındaki bir vCenter sunucusu tarafından yönetilen bir yalıtılmış VMware yığın ortamıdır. VMware yığınınızın ESXi konakları, vSphere, vCenter, Vsan'a ve NSX içerir. Yığın çalışır, adanmış düğümleri (ayrılmış ve yalıtılmış çıplak bilgisayar donanım) ve vCenter ve NSX Yöneticisi'ni içeren yerel VMware araçları aracılığıyla kullanıcılar tarafından kullanılan. Adanmış düğümler Azure konumlarında dağıtılır ve Azure tarafından yönetilir. Her özel bulut, bölümlenmiş ve VLAN'ların ve alt ağları ve güvenlik duvarı tablolar gibi ağ hizmetlerini kullanarak güvenli hale getirilmiş. Şirket içi ortamınız ile Azure ağ bağlantılarını bağlantıları güvenli kullanarak özel VPN ve Azure ExpressRoute tarafından oluşturulur.

Daha fazla bilgi edinin [CloudSimple özel bulut](cloudsimple-private-cloud.md).

## <a name="service-networking"></a>Ağ hizmeti

CloudSimple CloudSimple hizmetinizin dağıtıldığı bölge başına bir ağ hizmetidir. Varsayılan olarak etkin yönlendirme ile tek bir TCP Katman 3 adres alanı ağdır. Tüm özel Bulutlar ve alt ağlar bu bölgede oluşturulan herhangi bir ek yapılandırma birbirleri ile iletişim kurar. VCenter VLAN'ları kullanarak dağıtılmış bağlantı noktası grupları oluşturun. Aşağıdaki ağ özelliklerini yapılandırmak ve özel bulut iş yükü kaynaklarınızın güvenliğini sağlamak için kullanabilirsiniz:

* [VLAN ve alt ağları](cloudsimple-vlans-subnets.md)
* [Güvenlik Duvarı tabloları](cloudsimple-firewall-tables.md)
* [VPN ağ geçitleri](cloudsimple-vpn-gateways.md)
* [Genel IP](cloudsimple-public-ip-address.md)
* [Azure ağ bağlantısı](cloudsimple-azure-network-connection.md)

## <a name="cloudsimple-virtual-machine"></a>CloudSimple sanal makine

CloudSimple hizmet ile birlikte VMware sanal makinelerini, Azure portalından yönetebilirsiniz. Hizmet oluşturulduğu abonelik için bir veya daha fazla küme veya vSphere ortamınızdan kaynak havuzları eşlenebilir.

Daha fazla bilgi:

* [CloudSimple sanal makineler](cloudsimple-virtual-machines.md)
* [Azure aboneliği eşleme](https://docs.azure.cloudsimple.com/azure-subscription-mapping/)
