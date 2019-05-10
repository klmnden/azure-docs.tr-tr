---
title: VLAN ve alt ağları CloudSimple - Azure ile VMware çözümde
description: VLAN ve alt ağları CloudSimple özel bir bulutta hakkında bilgi edinin
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: e88977cc4d99df176116e6be7d8e06adb6297782
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65209572"
---
# <a name="vlans-and-subnets-overview"></a>VLAN ve alt ağları genel bakış

CloudSimple CloudSimple hizmetinizin dağıtıldığı bölge başına bir ağ sağlar.  Varsayılan olarak etkin yönlendirme ile tek bir TCP Katman 3 adres alanı ağdır.  Tüm özel Bulutlar ve alt ağlar bu bölgede oluşturulan herhangi bir ek yapılandırma birbiriyle iletişim kurabilir.  VLAN'ları kullanarak vCenter dağıtılmış bağlantı noktası grupları oluşturabilirsiniz.

## <a name="vlans"></a>VLAN'ları

VLAN'ları (Katman 2 ağ) özel bir bulut oluşturulur.  Katman 2 trafik, özel bulut yerel trafiği yalıtmak olanak tanıyan bir özel bulutun sınırları içinde kalır.  Özel bulutta oluşturulan bir VLAN, yalnızca o özel buluta dağıtılmış bağlantı noktası grupları oluşturmak için kullanılabilir.  Bir özel bulutta oluşturulan bir VLAN, bir özel bulutun Konaklara bağlı tüm anahtarlar üzerinde otomatik olarak yapılandırılır.

## <a name="subnets"></a>Alt ağlar

Bir VLAN alt ağın adres alanı tanımlayarak oluşturduğunuzda, bir alt ağ oluşturabilirsiniz. Adres alanından bir IP adresi alt ağı ağ geçidi olarak atanır. Müşteri ve bölge tek bir özel Katman 3 adres alanı atanır. Ağ bölgenizde herhangi RFC 1918 çakışmayan adres alanı, şirket içi ağınızla veya Azure sanal ağı ile yapılandırabilirsiniz.

Tüm alt ağlar, varsayılan olarak yapılandırma özel Bulutlar arasında yönlendirme için ek yükü azaltarak birbiriyle iletişim kurabilir. Doğu-Batı veriler aynı bölgede bulunan bilgisayarlar arasında aynı katman 3 ağında kalır ve bölge içindeki yerel ağ altyapı üzerinden aktarılır. Hiçbir çıkış bir bölgeye özel Bulutlar arasındaki iletişim için gereklidir. Bu yaklaşım, farklı iş yüklerini farklı özel bulutlarda dağıtma konusunda WAN/çıkış performans cezaları ortadan kaldırır.

## <a name="vspherevsan-subnets-cidr-range"></a>vSphere/vSAN alt ağ CIDR aralığı

Bir yalıtılmış VMware yığınınızın (ESXi konaklarını ve vCenter, Vsan'a ve NSX) oluşturulan bir özel bulut ortamı bir vCenter sunucusu tarafından yönetilen.  Yönetim bileşenleri için seçilen ağ dağıtıldığı **vSphere/vSAN alt ağ CIDR**.  Ağ CIDR aralığı, dağıtım sırasında farklı alt ağlara bölünmüştür.

En düşük vSphere/vSAN alt ağ CIDR aralığı ön eki: **/24** en fazla vSphere/vSAN alt ağ CIDR aralığı ön eki:   **/21**

### <a name="vspherevsan-subnets-cidr-range-limits"></a>vSphere/vSAN alt ağ CIDR aralığı sınırları

VSphere/vSAN alt ağ CIDR aralığı boyutunu seçme, özel bulut boyutuna göre bir etkisi yoktur.  Aşağıdaki tabloda, en fazla vSphere/vSAN alt ağ CIDR boyutuna göre düğüm sayısını gösterir.

| Belirtilen vSphere/vSAN alt ağ CIDR ön ek uzunluğu | En fazla düğüm sayısı |
|---------------------------------------------------|-------------------------|
| /24 | 26 |
| /23 | 58 |
| /22 | 118 |
| /21 | 220 |

### <a name="management-subnets-created-on-a-private-cloud"></a>Bir özel bulutta oluşturulan yönetim alt ağlar

Aşağıdaki yönetim alt ağlar, bir özel bulut oluşturduğunuzda oluşturulur. 

* **Sistem Yönetimi** -VLAN ve alt ağ ESXi ana bilgisayarlarının yönetimi için ağ, DNS sunucusu, vCenter sunucusu.
* **VMotion** -VLAN ve alt ağ için ESXi konakları vMotion ağ.
* **VSAN** -VLAN ve alt ağ için ESXi konakları vSAN ağ.
* **NsxtEdgeUplink1** -VLAN ve alt ağ VLAN dış ağ yukarı bağlantılar için.
* **NsxtEdgeUplink2** -VLAN ve alt ağ VLAN dış ağ yukarı bağlantılar için.
* **NsxtEdgeTransport** -VLAN ve alt ağ aktarımı bölgeleri için Denetim NSX-t Katman 2 ağların ulaşma
* **NsxtHostTransport** -VLAN ve alt ağ için ana bölge aktarım.

### <a name="management-network-cidr-range-breakdown"></a>Yönetim ağ CIDR aralığı dökümü

Belirtilen vSphere/vSAN alt ağ CIDR aralığı birden çok alt ağına bölünür.  Aşağıdaki tabloda, izin verilen ön ekler için dökümü örneği gösterilmektedir.  Örnekte **192.168.0.0** CIDR aralığı.

Örnek:

| Belirtilen vSphere/vSAN alt ağ CIDR/öneki | 192.168.0.0/21 | 192.168.0.0/22 | 192.168.0.0/23 | 192.168.0.0/24 |
|---------------------------------|----------------|----------------|----------------|----------------|
| Sistem Yönetimi | 192.168.0.0/24 | 192.168.0.0/24 | 192.168.0.0/25 | 192.168.0.0/26 |
| VMotion'ı | 192.168.1.0/24 | 192.168.1.0/25 | 192.168.0.128/26 | 192.168.0.64/27 |
| vSAN | 192.168.2.0/24 | 192.168.1.128/25 | 192.168.0.192/26 | 192.168.0.96/27 |
| NSX-T konak taşıma | 192.168.4.0/23 | 192.168.2.0/24 | 192.168.1.0/25 | 192.168.0.128/26 |
| NSX-T Edge taşıma | 192.168.7.208/28 | 192.168.3.208/28 | 192.168.1.208/28 | 192.168.0.208/28 |
| Edge Uplink1 NSX-T | 192.168.7.224/28 | 192.168.3.224/28 | 192.168.1.224/28 | 192.168.0.224/28 |
| Edge uplink2 NSX-T | 192.168.7.240/28 | 192.168.3.240/28 | 192.168.1.240/28 | 192.168.0.240/28 |

## <a name="next-steps"></a>Sonraki adımlar

* [VLAN ve alt ağlar oluşturun ve yönetin](https://docs.azure.cloudsimple.com/create-vlan-subnet/)