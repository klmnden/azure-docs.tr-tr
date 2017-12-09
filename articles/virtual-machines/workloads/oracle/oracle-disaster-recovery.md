---
title: "Azure ortamınızda bir Oracle olağanüstü durum kurtarma senaryosuna genel bakış | Microsoft Docs"
description: "Bir olağanüstü durum kurtarma senaryosunda bir Oracle veritabanına 12c veritabanı Azure ortamınızda için"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 6/2/2017
ms.author: rclaus
ms.openlocfilehash: f17ebb2b74cd7ad872f88483ed7cdb4f239ee069
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="disaster-recovery-for-an-oracle-database-12c-database-in-an-azure-environment"></a>Bir Oracle veritabanına 12c veritabanında bir Azure ortamı için olağanüstü durum kurtarma

## <a name="assumptions"></a>Varsayımlar

- Oracle Data Guard tasarım ve Azure ortamı bir anlama sahip.


## <a name="goals"></a>Hedefleri
- Olağanüstü Durum Kurtarma (DR) gereksinimlerinizi karşılayacak yapılandırma ve topoloji tasarlayın.

## <a name="scenario-1-primary-and-dr-sites-on-azure"></a>Senaryo 1: Birincil ve kurtarma sitelerinde Azure

Bir müşterinin bir Oracle veritabanı kümesi oluşturan birincil sitede. Bir DR, farklı bir bölgede sitedir. Müşteri, bu siteler arasında hızlı kurtarma için Oracle Data Guard kullanır. Birincil site ayrıca raporlama için ikincil bir veritabanı ve diğer kullanımlar sahiptir. 

### <a name="topology"></a>Topoloji

Azure kurulumu bir özeti aşağıda verilmiştir:

- İki site (bir birincil site ve bir DR sitesi)
- İki sanal ağ
- Veri koruma (birincil ve bekleme) ile iki Oracle veritabanları
- İki Oracle veritabanları Golden kapısı veya veri koruma (yalnızca birincil site)
- İki uygulama hizmetleri, bir birincil ve bir DR sitesinde
- Bir *kullanılabilirlik kümesi* birincil sitedeki veritabanı ve uygulama hizmeti için kullanılan
- Özel ağ erişimi kısıtlayan ve yalnızca bir yönetici oturum aç sağlayan her sitede bir jumpbox
- Jumpbox, uygulama hizmeti, veritabanı ve ayrı alt ağlar üzerinde VPN ağ geçidi
- Uygulama ve veritabanı alt ağlar üzerinde zorunlu NSG

![DR topoloji sayfasının ekran görüntüsü](./media/oracle-disaster-recovery/oracle_topology_01.png)

## <a name="scenario-2-primary-site-on-premises-and-dr-site-on-azure"></a>Senaryo 2: Birincil site şirket içi ve Azure üzerinde DR sitesi

Bir müşteri, bir şirket içi Oracle Veritabanı Kurulumu (birincil site) sahiptir. Azure üzerinde bir DR sitedir. Oracle Data Guard, bu siteler arasında hızlı kurtarma için kullanılır. Birincil site ayrıca raporlama için ikincil bir veritabanı ve diğer kullanımlar sahiptir. 

Bu ayar için iki yaklaşım vardır.

### <a name="approach-1-direct-connections-between-on-premises-and-azure-requiring-open-tcp-ports-on-the-firewall"></a>Yaklaşım 1: Şirket içi ve açık TCP bağlantı noktalarını Güvenlik Duvarı'nda gerektiren Azure arasında doğrudan bağlantıya 

Dış dünya için TCP bağlantı noktaları kullanıma biz doğrudan bağlantılar önerilmez.

#### <a name="topology"></a>Topoloji

Azure kurulumu bir özeti aşağıda verilmiştir:

- Bir DR sitesi 
- Bir sanal ağ
- Veri koruma (etkin) ile bir Oracle veritabanı
- DR sitesinde bir uygulama hizmeti
- Özel ağ erişimi kısıtlayan ve yalnızca bir yönetici oturum aç sağlayan bir jumpbox
- Jumpbox, uygulama hizmeti, veritabanı ve ayrı alt ağlar üzerinde VPN ağ geçidi
- Uygulama ve veritabanı alt ağlar üzerinde zorunlu NSG
- Bir NSG ilke/gelen TCP bağlantı noktası 1521 (veya kullanıcı tanımlı bir bağlantı noktası) izin verme kuralı
- Bir NSG ilke/yalnızca IP kısıtlamak için kural adresi/şirket içi (DB veya uygulama) sanal ağa erişim için adresleri

![DR topoloji sayfasının ekran görüntüsü](./media/oracle-disaster-recovery/oracle_topology_02.png)

### <a name="approach-2-site-to-site-vpn"></a>Yaklaşım 2: Siteden siteye VPN
Siteden siteye VPN daha iyi bir yaklaşımdır. VPN ayarlama hakkında daha fazla bilgi için bkz: [CLI kullanarak siteden siteye VPN bağlantısı olan bir sanal ağ oluşturma](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).

#### <a name="topology"></a>Topoloji

Azure kurulumu bir özeti aşağıda verilmiştir:

- Bir DR sitesi 
- Bir sanal ağ 
- Veri koruma (etkin) ile bir Oracle veritabanı
- DR sitesinde bir uygulama hizmeti
- Özel ağ erişimi kısıtlayan ve yalnızca bir yönetici oturum aç sağlayan bir jumpbox
- Jumpbox, uygulama hizmeti, veritabanı ve VPN ağ geçidi ayrı alt ağlarda bulunan
- Uygulama ve veritabanı alt ağlar üzerinde zorunlu NSG
- Şirket içi ve Azure arasında siteden siteye VPN bağlantısı

![DR topoloji sayfasının ekran görüntüsü](./media/oracle-disaster-recovery/oracle_topology_03.png)

## <a name="additional-reading"></a>Ek kaynaklar

- [Bir Oracle veritabanına Azure üzerinde tasarlayıp](oracle-design.md)
- [Oracle Data Guard yapılandırın](configure-oracle-dataguard.md)
- [Oracle Golden kapısı yapılandırın](configure-oracle-golden-gate.md)
- [Oracle yedekleme ve kurtarma](oracle-backup-recovery.md)


## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: yüksek oranda kullanılabilir sanal makineleri oluşturma](../../linux/create-cli-complete.md)
- [VM dağıtımı Azure CLI örnekleri keşfedin](../../linux/cli-samples.md)
