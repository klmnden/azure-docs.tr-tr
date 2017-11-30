---
title: "Azure Site Recovery Azure başka bir Azure sanal makineleri çoğaltmak için kılavuz ağ | Microsoft Docs"
description: "Azure sanal makineleri çoğaltmak için kılavuz ağ oluşturma"
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/31/2017
ms.author: sujayt
ms.openlocfilehash: a2ea66f2ed3815d0375001571e64dad338f16e3e
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2017
---
# <a name="networking-guidance-for-replicating-azure-virtual-machines"></a>Azure sanal makineleri çoğaltmak için kılavuz ağ oluşturma

>[!NOTE]
> Azure sanal makineler için Site Recovery çoğaltma şu anda önizlemede değil.

Çoğaltma ve Azure sanal makineleri başka bir bölgeye bir bölgesinden kurtarma Bu makalede Azure Site Recovery için Ağ Kılavuzu ayrıntıları verilmektedir. Azure Site Recovery gereksinimleri hakkında daha fazla bilgi için bkz: [Önkoşullar](site-recovery-support-matrix-azure-to-azure.md) makalesi.

## <a name="site-recovery-architecture"></a>Site Recovery mimarisi

Site Recovery, böylece birincil bölge kesilme varsa bunlar kurtarılabilir başka bir Azure bölgesine Azure sanal makinelerde çalışan uygulamaların çoğaltmak için basit ve kolay bir yol sağlar. Daha fazla bilgi edinmek [bu senaryo ve Site Recovery mimarisi](concepts-azure-to-azure-architecture.md).

## <a name="your-network-infrastructure"></a>Ağ altyapınızın

Aşağıdaki diyagram tipik Azure ortamı Azure sanal makinelerde çalışan bir uygulama için gösterir:

![Müşteri ortamı](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Azure ExpressRoute veya bir şirket içi ağ bir VPN bağlantısı Azure kullanıyorsanız, ortam şöyle görünür:

![Müşteri ortamı](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

Genellikle, müşteriler güvenlik duvarları ve/veya ağ güvenlik grupları (Nsg'ler) kullanarak kendi ağları koruyun. Güvenlik duvarları, ağ bağlantısını denetlemek için URL tabanlı ya da IP tabanlı uygulamaları güvenilir listeye almayı kullanabilirsiniz. Nsg'ler, IP aralıklarını kullanarak ağ bağlantısını denetlemek için kuralları izin verir.

>[!IMPORTANT]
> Ağ bağlantısını denetlemek için doğrulanmış bir proxy kullanıyorsanız, ancak desteklenmez ve Site Recovery çoğaltma etkinleştirilemez.

Aşağıdaki bölümlerde, Site Recovery çoğaltmanın çalışması Azure sanal makinelerden gerekli ağ giden bağlantı değişiklikleri açıklanmaktadır.

## <a name="outbound-connectivity-for-azure-site-recovery-urls"></a>Azure Site kurtarma URL'ler için giden bağlantı

Herhangi bir URL tabanlı bir güvenlik duvarı proxy kullanıyorsanız giden bağlantıyı denetlemek için bunlar Azure Site Recovery hizmeti URL'lerine gerekli beyaz liste ile emin olun:


**URL** | **Amacı**  
--- | ---
*.blob.core.windows.net | Veri kaynağı bölgede önbellek depolama hesabı sanal makineden yazılabilir böylece gereklidir.
Login.microsoftonline.com | Yetkilendirme ve kimlik doğrulaması için Site Recovery hizmeti URL'lerine için gereklidir.
*.hypervrecoverymanager.windowsazure.com | Site Recovery hizmeti iletişimi sanal makineden gerçekleştirilmesi gerekir.
*. servicebus.windows.net | Böylece Site Recovery izleme ve tanılama verilerini sanal makineden yazılabilir gereklidir.

## <a name="outbound-connectivity-for-azure-site-recovery-ip-ranges"></a>Azure Site kurtarma IP aralıkları için giden bağlantı

>[!NOTE]
> Ağ güvenlik grubu gerekli NSG kuralları otomatik olarak oluşturmak için [indirin ve bu komut dosyası](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).

>[!IMPORTANT]
> * Gerekli NSG kuralları bir test ağ güvenlik grubu oluşturun ve bir üretim ağ güvenlik grubu kuralları oluşturmadan önce herhangi bir sorun olduğunu doğrulayın öneririz.
> * NSG kuralları gereken sayısını oluşturmak için aboneliğinizi Güvenilenler listesine olmasını sağlayın. Aboneliğinizde NSG kural sınırını artırmak üzere desteğe başvurun.

Giden bağlantıyı denetlemek için herhangi bir IP tabanlı bir güvenlik duvarı proxy veya NSG kuralları kullanıyorsanız, aşağıdaki IP aralıklarını sanal makinelerin kaynak ve hedef konumların bağlı olarak güvenilir listesinde olması gerekir:

- Kaynak konumuna karşılık gelen tüm IP aralıkları. (İndirebilirsiniz [IP aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653).) Böylece veri önbelleği depolama hesabına sanal makineden yazılabilir uygulamaları güvenilir listeye almayı gereklidir.

- Office 365'e karşılık gelen tüm IP aralıklarını [kimlik doğrulama ve kimlik IP V4 uç noktaları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

    >[!NOTE]
    > Yeni IP'leri Office 365 IP aralıkları için gelecekte eklenir, yeni NSG kuralları oluşturmanız gerekir.

- Kurtarma Hizmeti uç noktası IP'leri site ([bir XML dosyasında kullanılabilir](https://aka.ms/site-recovery-public-ips)), hedef konumuna bağlıdır:

   **Hedef konumu** | **Site Recovery hizmeti IP'leri** |  **Site Recovery IP izleme**
   --- | --- | ---
   Doğu Asya | 52.175.17.132 | 13.94.47.61
   Güneydoğu Asya | 52.187.58.193 | 13.76.179.223
   Orta Hindistan | 52.172.187.37 | 104.211.98.185
   Güney Hindistan | 52.172.46.220 | 104.211.224.190
   Orta Kuzey ABD | 23.96.195.247 | 168.62.249.226
   Kuzey Avrupa | 40.69.212.238 | 52.169.18.8
   Batı Avrupa | 52.166.13.64 | 40.68.93.145
   Doğu ABD | 13.82.88.226 | 104.45.147.24
   Batı ABD | 40.83.179.48 | 104.40.26.199
   Orta Güney ABD | 13.84.148.14 | 104.210.146.250
   Orta ABD | 40.69.144.231 | 52.165.34.144
   Doğu ABD 2 | 52.184.158.163 | 40.79.44.59
   Japonya Doğu | 52.185.150.140 | 138.91.1.105
   Japonya Batı | 52.175.146.69 | 138.91.17.38
   Güney Brezilya | 191.234.185.172 | 23.97.97.36
   Avustralya Doğu | 104.210.113.114 | 191.239.64.144
   Avustralya Güneydoğu | 13.70.159.158 | 191.239.160.45
   Orta Kanada | 52.228.36.192 | 40.85.226.62
   Doğu Kanada | 52.229.125.98 | 40.86.225.142
   Batı Orta ABD | 52.161.20.168 | 13.78.149.209
   Batı ABD 2 | 52.183.45.166 | 13.66.228.204
   Birleşik Krallık Batı | 51.141.3.203 | 51.141.14.113
   Birleşik Krallık Güney | 51.140.43.158 | 51.140.189.52
   UK Güney 2 | 13.87.37.4| 13.87.34.139
   UK Kuzey | 51.142.209.167 | 13.87.102.68
   Kore Orta | 52.231.28.253 | 52.231.32.85
   Kore Güney | 52.231.298.185 | 52.231.200.144

## <a name="sample-nsg-configuration"></a>Örnek NSG yapılandırma
Bu bölüm, Site Recovery çoğaltma sanal bir makinede çalışabilmeniz için NSG kuralları yapılandırmak için gereken adımları açıklar. Giden bağlantıyı denetlemek için NSG kuralları kullanıyorsanız, "HTTPS giden izin ver" kuralları için tüm gerekli IP aralıklarını kullanın.

>[!Note]
> Ağ güvenlik grubu gerekli NSG kuralları otomatik olarak oluşturmak için [indirin ve bu komut dosyası](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).

Örneğin, VM kaynak konumu "Doğu ABD" ve çoğaltma hedef konumu "Orta ABD" ise, sonraki iki bölümde'ndaki adımları izleyin.

>[!IMPORTANT]
> * Gerekli NSG kuralları bir test ağ güvenlik grubu oluşturun ve bir üretim ağ güvenlik grubu kuralları oluşturmadan önce herhangi bir sorun olduğunu doğrulayın öneririz.
> * NSG kuralları gereken sayısını oluşturmak için aboneliğinizi Güvenilenler listesine olmasını sağlayın. Aboneliğinizde NSG kural sınırını artırmak üzere desteğe başvurun.

### <a name="nsg-rules-on-the-east-us-network-security-group"></a>Doğu ABD ağ güvenlik grubu NSG kuralları

* Karşılık gelen kuralları oluşturma [Doğu ABD IP aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653). Bu, böylece veri önbelleği depolama hesabına sanal makineden yazılabilir gereklidir.

* Office 365'e karşılık gelen tüm IP aralıkları için kurallar oluşturun [kimlik doğrulama ve kimlik IP V4 uç noktaları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

* Hedef konuma karşılık gelen kurallarını oluşturun:

   **Konum** | **Site Recovery hizmeti IP'leri** |  **Site Recovery IP izleme**
    --- | --- | ---
   Orta ABD | 40.69.144.231 | 52.165.34.144

### <a name="nsg-rules-on-the-central-us-network-security-group"></a>Orta ABD ağ güvenlik grubu NSG kuralları

Bu kurallar, böylece kaynak bölgesi yük devretme sonrasında için etkin hale getirilebilir çoğaltma hedef bölgesinden gereklidir:

* Karşılık gelen kuralları [merkezi ABD IP aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653). Veri önbelleği depolama hesabına sanal makineden yazılabilir şekilde bu gereklidir.

* Office 365'e karşılık gelen tüm IP aralıkları için kuralları [kimlik doğrulama ve kimlik IP V4 uç noktaları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

* Kaynak konumuna karşılık gelen kuralları:

   **Konum** | **Site Recovery hizmeti IP'leri** |  **Site Recovery IP izleme**
    --- | --- | ---
   Doğu ABD | 13.82.88.226 | 104.45.147.24


## <a name="guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration"></a>Var olan Azure--şirket içi için ExpressRoute/VPN yapılandırma yönergeleri

Şirket içi ve Azure kaynak konumu arasında bir ExpressRoute veya VPN bağlantısı varsa, bu bölümdeki yönergeleri uygulayın.

### <a name="forced-tunneling-configuration"></a>Zorlamalı tünel yapılandırma

Bir ortak müşteri aracılığıyla şirket içi konuma giden Internet akışına zorlar varsayılan yol (0.0.0.0/0) tanımlamak için bir yapılandırmadır. Bunu önermiyoruz. Site Recovery hizmeti iletişimi ve çoğaltma trafiğini Azure sınır bırakmamalısınız. Kullanıcı tanımlı yollar (Udr'ler) eklemek için çözümdür [bu IP aralıkları](#outbound-connectivity-for-azure-site-recovery-ip-ranges) böylece şirket içi çoğaltma trafiğini Git değil.

### <a name="connectivity-between-the-target-and-on-premises-location"></a>Hedef ve şirket içi konum arasında bağlantı

Hedef konumu ve şirket içi konumunuz bağlantılar için bu yönergeleri izleyin:
- Uygulamanız için şirket içi makineler bağlanın veya uygulamaya şirket içi VPN/ExpressRoute bağlanan istemciler varsa, en az bir sağlamak gerekiyorsa [siteden siteye bağlantı](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) arasında Hedef Azure bölgesi ve şirket içi veri merkezi.

- Hedef Azure bölgesi ve şirket içi veri merkezi arasında trafiğe çok bekliyorsanız, başka bir oluşturmalısınız [ExpressRoute bağlantısı](../../expressroute/expressroute-introduction.md) hedef Azure bölgesi ve şirket içi veri merkezi arasında.

- Bunlar üzerinde başarısız olduktan sonra sanal makineler için IP korumak istiyorsanız, hedef bölgenin site siteye/ExpressRoute bağlantısı bağlantısı kesik durumda tutun. Hiçbir aralık çakışmasından kaynak bölgesinin IP aralıklarını ve hedef bölgenin IP aralıkları arasında olduğundan emin olmak için budur.

### <a name="best-practices-for-expressroute-configuration"></a>ExpressRoute yapılandırması için en iyi yöntemler
ExpressRoute yapılandırması için bu en iyi uygulamaları izleyin:

- Bir expressroute bağlantı hattı hem kaynak hem de hedef bölgelerde oluşturmanız gerekir. Ardından arasında bir bağlantı oluşturmanız gerekir:
  - Kaynak sanal ağ ve expressroute bağlantı hattı.
  - Hedef sanal ağ ve expressroute bağlantı hattı.

- ExpressRoute standart bir parçası olarak, aynı coğrafi bölgede devreler oluşturabilirsiniz. ExpressRoute bağlantı hatları farklı coğrafi bölgelerde oluşturmak için Azure ExpressRoute Premium gereklidir artımlı bir maliyet içerir. (ExpressRoute Premium zaten kullanıyorsanız, var. ek bir maliyeti yoktur.) Daha fazla ayrıntı için bkz: [ExpressRoute konumları belge](../../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) ve [ExpressRoute fiyatlandırma](https://azure.microsoft.com/pricing/details/expressroute/).

- Kaynak ve hedef bölgeler farklı IP aralıkları kullanmanızı öneririz. Expressroute bağlantı hattı iki Azure sanal ağlar aynı IP aralıklarının aynı anda bağlanabiliyor olmayacaktır.

- Her iki bölgelerinde aynı IP aralıklarıyla sanal ağlar oluşturabilir ve sonra ExpressRoute bağlantı hatları her iki bölgelerde oluşturabilirsiniz. Bir yük devretme olayından söz konusu olduğunda bağlantı hattı kaynak sanal ağla olan bağlantınızı kesmek ve hedef sanal ağ bağlantı hattındaki bağlanın.

 >[!IMPORTANT]
 > Birincil bölge tamamen kapalı ise, bağlantıyı kesme işlemi başarısız olabilir. ExpressRoute bağlantı alma, hedef sanal ağ engeller.

## <a name="next-steps"></a>Sonraki adımlar
İş yükleri tarafından korumaya başlamak [Azure sanal makineleri çoğaltmak](azure-to-azure-quickstart.md).
