---
title: Azure ortamınızda bir Oracle olağanüstü durum kurtarma senaryosuna genel bakış | Microsoft Docs
description: Azure ortamınızda bir Oracle Database 12c veritabanı için bir olağanüstü durum kurtarma senaryosu
services: virtual-machines-linux
documentationcenter: virtual-machines
author: romitgirdhar
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/02/2018
ms.author: rogirdh
ms.openlocfilehash: db0b9887b80f13938045a5d11fb09ed0a43efc19
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706974"
---
# <a name="disaster-recovery-for-an-oracle-database-12c-database-in-an-azure-environment"></a>Azure ortamınızda bir Oracle Database 12c veritabanı için olağanüstü durum kurtarma

## <a name="assumptions"></a>Varsayımlar

- Oracle Data Guard tasarım ve Azure ortamları bir anlayışa sahip.


## <a name="goals"></a>Hedefleri
- Olağanüstü Durum Kurtarma (DR) gereksinimlerini karşılayan yapılandırma ve topoloji tasarlayın.

## <a name="scenario-1-primary-and-dr-sites-on-azure"></a>Senaryo 1: Birincil ve kurtarma siteleri azure'da

Müşterinin bir Oracle veritabanı kümesi oluşturan birincil sitede. Farklı bir bölgede bir DR sitedir. Müşteri, bu siteler arasında hızlı kurtarma için Oracle Data Guard kullanır. Birincil site, raporlama için ikincil bir veritabanı ve diğer kullanımlar da vardır. 

### <a name="topology"></a>Topoloji

Azure kurulumu bir özeti aşağıda verilmiştir:

- İki site (bir birincil site ve DR sitesi)
- İki sanal ağ
- İki Oracle veritabanları ile Data Guard'ın (birincil ve bekleme)
- İki Oracle veritabanları ile Golden ağ geçidi veya veri koruma (yalnızca birincil site)
- İki uygulama hizmetleri, birincil ve kurtarma sitesinde bir tane
- Bir *kullanılabilirlik kümesi* birincil sitedeki veritabanı ve uygulama hizmeti için kullanılan
- Özel ağa erişimi kısıtlayan ve yalnızca bir yönetici tarafından oturum sağlayan her sitede bir Sıçrama kutusu
- Sıçrama kutusu, uygulama hizmeti, veritabanı ve ayrı alt ağlara VPN ağ geçidi
- Uygulama ve veritabanı alt ağlar üzerinde uygulanan NSG

![DR topolojisi sayfasının ekran görüntüsü](./media/oracle-disaster-recovery/oracle_topology_01.png)

## <a name="scenario-2-primary-site-on-premises-and-dr-site-on-azure"></a>Senaryo 2: Birincil site şirket içi ve Azure üzerinde DR sitesi

Bir müşterinin bir şirket içi Oracle Veritabanı Kurulumu (birincil site). Azure'da bir DR sitedir. Oracle Data Guard, bu siteler arasında hızlı kurtarma için kullanılır. Birincil site, raporlama için ikincil bir veritabanı ve diğer kullanımlar da vardır. 

Bu ayar için iki yaklaşım vardır.

### <a name="approach-1-direct-connections-between-on-premises-and-azure-requiring-open-tcp-ports-on-the-firewall"></a>Yaklaşım 1: Şirket içi ve Azure, güvenlik duvarında TCP bağlantı noktalarını açma gerektiren arasında doğrudan bağlantı 

Dış dünya için TCP bağlantı noktalarını ortaya olduğundan doğrudan bağlantılar önerilmemektedir.

#### <a name="topology"></a>Topoloji

Azure Kurulum özeti aşağıda verilmiştir:

- Bir DR sitesi 
- Bir sanal ağ
- Bir Oracle veritabanına veri koruma (etkin)
- DR sitesinde bir uygulama hizmeti
- Yalnızca bir yönetici tarafından oturum sağlar ve özel ağa erişimi kısıtlayan bir Sıçrama kutusu
- Sıçrama kutusu, uygulama hizmeti, veritabanı ve ayrı alt ağlara VPN ağ geçidi
- Uygulama ve veritabanı alt ağlar üzerinde uygulanan NSG
- Bir NSG ilke/gelen TCP bağlantı noktası 1521 (veya kullanıcı tanımlı bir bağlantı noktası) izin vermek için kural
- Bir NSG ilke/yalnızca IP kısıtlamak için kuralı adresi/şirket içi (DB veya uygulama) sanal ağa erişmek için adresleri

![DR topolojisi sayfasının ekran görüntüsü](./media/oracle-disaster-recovery/oracle_topology_02.png)

### <a name="approach-2-site-to-site-vpn"></a>Yaklaşım 2: Siteden siteye VPN
Siteden siteye VPN daha iyi bir yaklaşımdır. VPN ayarlama hakkında daha fazla bilgi için bkz. [CLI kullanarak siteden siteye VPN bağlantısı ile sanal ağ oluşturma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).

#### <a name="topology"></a>Topoloji

Azure Kurulum özeti aşağıda verilmiştir:

- Bir DR sitesi 
- Bir sanal ağ 
- Bir Oracle veritabanına veri koruma (etkin)
- DR sitesinde bir uygulama hizmeti
- Yalnızca bir yönetici tarafından oturum sağlar ve özel ağa erişimi kısıtlayan bir Sıçrama kutusu
- Sıçrama kutusu, uygulama hizmeti, veritabanı ve VPN ağ geçidi üzerinde ayrı alt ağlardır
- Uygulama ve veritabanı alt ağlar üzerinde uygulanan NSG
- Şirket içi ve Azure arasında siteden siteye VPN bağlantısı

![DR topolojisi sayfasının ekran görüntüsü](./media/oracle-disaster-recovery/oracle_topology_03.png)

## <a name="additional-reading"></a>Ek okuma

- [Bir Oracle veritabanına Azure'da tasarlayıp](oracle-design.md)
- [Oracle Data Guard'ı yapılandırma](configure-oracle-dataguard.md)
- [Oracle Golden ağ geçidi yapılandırma](configure-oracle-golden-gate.md)
- [Oracle yedekleme ve kurtarma](oracle-backup-recovery.md)


## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Yüksek oranda kullanılabilir VM'ler oluşturma](../../linux/create-cli-complete.md)
- [VM dağıtımı Azure CLI örneklerini keşfedin](../../linux/cli-samples.md)
