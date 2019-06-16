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
ms.openlocfilehash: 0d890553ee145ca6aafed5a34d158c6a34d9af36
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66358176"
---
# <a name="key-concepts-for-administration-of-azure-vmware-solution-by-cloudsimple"></a>Azure VMware CloudSimple çözümüyle yönetimi için temel kavramları

Azure VMware CloudSimple çözümüyle yönetmek için aşağıdaki kavramları anlama gerektirir.

* CloudSimple hizmeti (Azure VMware çözümü CloudSimple - hizmet gösterilir)
* CloudSimple düğümü (CloudSimple - düğüm Azure VMware çözümü gösterilir)
* CloudSimple özel bulut
* Ağ hizmeti
* CloudSimple sanal makine (CloudSimple - sanal makine Azure VMware çözümü gösterilir)

## <a name="cloudsimple-service"></a>CloudSimple hizmeti

CloudSimple service, VMware çözümleriyle CloudSimple tarafından Azure portalından ilişkili tüm kaynakları oluşturup yönetmek sağlar. Bir hizmet kaynağı nerede hizmetini kullanmayı düşündüğünüz her bölgede oluşturun. 

Daha fazla bilgi edinin [CloudSimple hizmeti](cloudsimple-service.md)

## <a name="cloudsimple-node"></a>CloudSimple düğümü

Ayrılmış, çıplak metal, VMware ESXi hiper yönetici dağıtıldığı hiper yakınsanmış işlem ve depolama konak CloudSimple düğüm. Bu düğüm, ardından VMware vSphere, vCenter, Vsan'a ve NSX platformları eklenmiştir. CloudSimple ağ hizmetleri ve uç ağ hizmetlerinin de etkinleştirilir. Her bir düğüm oluşturmak için satın alabileceğiniz işlem ve depolama kapasitesi, bir birim olarak hizmet veren [CloudSimple özel Bulutları](cloudsimple-private-cloud.md). Satın aldığınız ya da CloudSimple hizmetin kullanılabildiği bir bölge içinde düğümler ayırmak.


Daha fazla bilgi edinin [CloudSimple düğümleri](cloudsimple-node.md)

## <a name="cloudsimple-private-cloud"></a>CloudSimple özel bulut

CloudSimple özel bulut kendi Yönetim etki alanındaki bir vCenter sunucusu tarafından yönetilen bir yalıtılmış VMware yığın ortamıdır. VMware yığınınızın ESXi konakları, vSphere, vCenter, Vsan'a ve NSX içerir.  Yığın çalışır, adanmış düğümleri (ayrılmış ve yalıtılmış çıplak metal donanım) ve vCenter ve NSX Yöneticisi'ni içeren yerel VMware araçları aracılığıyla kullanıcılar tarafından kullanılır. Adanmış düğümler Azure konumlarında dağıtılır ve Azure tarafından yönetilir. Her özel bulut bölümlenmiş ve kullanarak güvenli hale getirilmiş Hizmetleri VLAN'lar/alt ağlar gibi ağ ve güvenlik duvarı tablolar.  Şirket içi ortamınız ile Azure ağ bağlantıları, güvenli, özel VPN kullanılarak oluşturulur ve Azure ExpressRoute bağlantıları.

Daha fazla bilgi edinin [CloudSimple özel bulut](cloudsimple-private-cloud.md)

## <a name="service-networking"></a>Ağ hizmeti

CloudSimple CloudSimple hizmetinizin dağıtıldığı bölge başına bir ağ hizmetidir. Varsayılan olarak etkin yönlendirme ile tek bir TCP Katman 3 adres alanı ağdır. Tüm özel Bulutlar ve alt ağlar bu bölgede oluşturulan herhangi bir ek yapılandırma birbirleri ile iletişim kurar. VLAN'ları kullanarak vCenter'de dağıtılmış bağlantı noktası grupları oluşturun.  Aşağıdaki ağ özelliklerini yapılandırmak ve özel bulut iş yükü kaynaklarınızın güvenliğini sağlamak için kullanabilirsiniz.

* [VLAN'lar/alt ağlar](cloudsimple-vlans-subnets.md)
* [Güvenlik Duvarı tabloları](cloudsimple-firewall-tables.md)
* [VPN ağ geçitleri](cloudsimple-vpn-gateways.md)
* [Genel IP](cloudsimple-public-ip-address.md)
* [Azure ağ bağlantısı](cloudsimple-azure-network-connection.md)

## <a name="cloudsimple-virtual-machine"></a>CloudSimple sanal makine

CloudSimple hizmeti, VMware sanal makinelerini Azure portalından yönetmenize olanak sağlar. Hizmet oluşturulduğu abonelik için bir veya daha fazla küme veya vSphere ortamınızdan kaynak havuzları eşlenebilir.

Daha fazla bilgi:

* [CloudSimple sanal makineler](cloudsimple-virtual-machines.md)
* [Azure aboneliği eşleme](https://docs.azure.cloudsimple.com/azure-subscription-mapping/)
