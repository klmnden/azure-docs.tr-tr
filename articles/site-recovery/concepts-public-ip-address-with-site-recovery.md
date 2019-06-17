---
title: Azure Site Recovery ile yük devretme sonrasında genel IP adresleri kullanın. | Microsoft Docs
description: Olağanüstü durum kurtarma ve geçiş için Azure Site Recovery ve Azure Traffic Manager ile genel IP adreslerini ayarlama açıklanmaktadır
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: mayg
ms.openlocfilehash: 1f20818f0b899eede9fff05d71e98c8bffb94b0a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62101971"
---
# <a name="set-up-public-ip-addresses-after-failover"></a>Yük devretme sonrasında genel IP adreslerini ayarlama

Genel IP adresleri, İnternet kaynaklarının Azure kaynakları ile gelen iletişimi kurmasına izin verir. Genel IP adresleri ayrıca Azure kaynaklarının İnternet ve kaynağa atanmış bir IP adresi ile genel kullanıma yönelik Azure hizmetleri ile giden iletişimi kurmasını sağlar.
- Azure sanal makineler (VM), Azure uygulama ağ geçitleri, Azure Load balancer'ları, Azure VPN ağ geçitleri ve diğerleri gibi kaynak Internet'ten gelen iletişimi. Yine de Internet'ten sanal makineleri gibi bazı kaynaklar ile bir VM, sanal makine bir yük dengeleyici arka uç havuzu bir parçasıdır ve yük dengeleyicinin genel IP adresi atanır sürece atanmış bir genel IP adresi yoksa iletişim kurabilir.
- Tahmin edilebilir bir IP adresi kullanarak İnternet'e giden bağlantı. Örneğin, bir sanal makinenin genel IP adresi olmadan İnternet'e giden kendisine atanmış, ancak Azure tarafından varsayılan olarak bir öngörülemeyen genel adresine çevrilen ağ adresi kendi adresidir iletişim kurabilir. Bir kaynağa genel IP adresi atamayı hangi IP adresi giden bağlantı için kullanılan bilmenizi sağlar. Tahmin edilebilir olsa, seçilen atama yöntemine bağlı olarak adresi değişebilir. Daha fazla bilgi için [genel IP adresi oluşturma](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address). Azure kaynaklarını giden bağlantılar hakkında daha fazla bilgi için bkz: [giden bağlantıları anlama](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Azure Resource Manager'da bir genel IP adresini, kendi özelliklerine sahip bir kaynaktır. Genel bir IP adresini ilişkilendirebileceğiniz kaynakların bazıları şunlardır:

* Sanal makine ağ arabirimleri
* İnternet'e yönelik yük dengeleyiciler
* VPN ağ geçitleri
* Uygulama ağ geçitleri

Bu makalede, genel IP adresleri Site Recovery ile nasıl kullanabileceğiniz açıklanır.

## <a name="public-ip-address-assignment-using-recovery-plan"></a>Kurtarma planı kullanarak genel IP adresi ataması

Genel IP adresi bir üretim uygulamasının **yük devretmede tutulamıyor**. Yük devretme işleminin bir parçası bir hedef bölgede Azure genel IP kaynağı atanması gerektiğinden getirdiği iş yükleri. Bu adım ya da el ile yapılabilir veya kurtarma planları ile otomatik hale getirilmiştir. Kurtarma planında makineleri kurtarma gruplar halinde toplar. Bir sistematik bir kurtarma işlemi tanımlamak için yardımcı olur. Sipariş koymak için bir kurtarma planı kullanın ve Azure ya da komut dosyaları için yük devretme için Azure Otomasyonu runbook'ları kullanarak her adımda gerekli eylemleri otomatik hale getirin.

Kurulumu aşağıdaki gibidir:
- Oluşturma bir [kurtarma planı](../site-recovery/site-recovery-create-recovery-plans.md#create-a-recovery-plan) ve planı iş yükünüz gerektiği gibi gruplayın.
- Plan genel bir IP adresi kullanarak eklemek için bir adım ekleyerek özelleştirme [Azure Otomasyonu runbook'ları](../site-recovery/site-recovery-runbook-automation.md#customize-the-recovery-plan) devredilen VM'nin betikleri.

 
## <a name="public-endpoint-switching-with-dns-level-routing"></a>Ortak uç nokta DNS düzeyi yönlendirme ile değiştirme

Azure Traffic Manager etkinleştirir DNS uç noktaları arasında yönlendirme düzeyi ve yardımcı olabilecek [, Rto'lar sürüş](../site-recovery/concepts-traffic-manager-with-site-recovery.md#recovery-time-objective-rto-considerations) DR senaryosu için. 

Traffic Manager ile yük devretme senaryoları hakkında daha fazlasını okuyun:
1. [Şirket içi Azure yük devretme](../site-recovery/concepts-traffic-manager-with-site-recovery.md#on-premises-to-azure-failover) Traffic Manager ile 
2. [Azure'dan Azure'a yük devretme](../site-recovery/concepts-traffic-manager-with-site-recovery.md#azure-to-azure-failover) Traffic Manager ile 

Kurulumu aşağıdaki gibidir:
- Oluşturma bir [Traffic Manager profili](../traffic-manager/traffic-manager-create-profile.md).
- Yararlanarak **öncelik** yönlendirme yöntemi oluşturma iki uç nokta – **birincil** kaynağı için ve **yük devretme** Azure için. **Birincil** öncelik 1 atanır ve **yük devretme** öncelik 2 atanır.
- **Birincil** uç nokta olabilir [Azure](../traffic-manager/traffic-manager-endpoint-types.md#azure-endpoints) veya [dış](../traffic-manager/traffic-manager-endpoint-types.md#external-endpoints) kaynak ortamınız içinde veya Azure dışından olmasına bağlı olarak.
- **Yük devretme** uç nokta olarak oluşturulan bir **Azure** uç noktası. Kullanım bir **statik genel IP adresi** gibi bu dış e yönelik uç nokta Traffic Manager için olağanüstü durum olayı olacaktır.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [Traffic Manager ile Azure Site Recovery](../site-recovery/concepts-traffic-manager-with-site-recovery.md)
- Traffic Manager hakkında daha fazla bilgi edinin [yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md).
- Daha fazla bilgi edinin [kurtarma planları](site-recovery-create-recovery-plans.md) uygulama yük devretme otomatikleştirmek için.
