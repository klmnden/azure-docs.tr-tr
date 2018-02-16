---
title: "Azure Site Kurtarma'yı kullanarak Azure için Azure olağanüstü durum kurtarma de ağ oluşturmayla ilgili | Microsoft Docs"
description: "Azure Site Recovery kullanarak Azure VM'ler, çoğaltma için ağ genel bir bakış sağlar."
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/08/2018
ms.author: sujayt
ms.openlocfilehash: 5ce85761df4e0ad62c22a829f67464a3145fd827
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="about-networking-in-azure-to-azure-replication"></a>Azure için Azure çoğaltma ağ oluşturma hakkında

>[!NOTE]
> Azure sanal makineler için Site Recovery çoğaltma şu anda önizlemede değil.

Çoğaltma ve kullanarak Azure Vm'leri bir bölgesinden diğerine kurtarma Ağ Kılavuzu bu makalede sunar [Azure Site Recovery](site-recovery-overview.md). 

## <a name="before-you-start"></a>Başlamadan önce

Site Recovery olağanüstü durum kurtarma için nasıl sağladığını öğrenin [bu senaryo](azure-to-azure-architecture.md).

## <a name="typical-network-infrastructure"></a>Tipik ağ altyapısı

Aşağıdaki diyagram, Azure Vm'lerinde çalışan uygulamalar için tipik bir Azure ortamı gösterir:

![Müşteri ortamı](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Azure ExpressRoute veya bir VPN bağlantısı şirket içi ağınızdan Azure'a kullanıyorsanız, ortam şöyle görünür:

![Müşteri ortamı](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

Genellikle, ağları güvenlik duvarları ve ağ güvenlik gruplarını (Nsg'ler) kullanarak korunur. Güvenlik duvarları, ağ bağlantısını denetlemek için URL veya IP tabanlı uygulamaları güvenilir listeye almayı kullanın. Nsg'leri ağ bağlantısını denetlemek için IP adres aralıklarını kullanan kurallar sağlar.

>[!IMPORTANT]
> Doğrulanmış bir proxy denetim ağ bağlantısı için kullanarak Site Recovery tarafından desteklenmiyor ve çoğaltma etkinleştirilemez.


## <a name="outbound-connectivity-for-urls"></a>URL'ler için giden bağlantı

Giden bağlantıyı denetlemek için bir URL tabanlı bir güvenlik duvarı proxy kullanıyorsanız, bu Site kurtarma URL'leri izin ver:


**URL** | **Ayrıntılar**  
--- | ---
*.blob.core.windows.net | Veri kaynağı bölgede önbellek depolama hesabı sanal makineden yazılabilir böylece gereklidir.
login.microsoftonline.com | Yetkilendirme ve kimlik doğrulaması için Site Recovery hizmeti URL'lerine için gereklidir.
*.hypervrecoverymanager.windowsazure.com | Site Recovery hizmeti iletişimi sanal makineden gerçekleştirilmesi gerekir.
*.servicebus.windows.net | Böylece Site Recovery izleme ve tanılama verilerini sanal makineden yazılabilir gereklidir.

## <a name="outbound-connectivity-for-ip-address-ranges"></a>IP adres aralıkları için giden bağlantı

Giden bağlantıyı denetlemek için bir IP tabanlı bir güvenlik duvarı proxy ya da NSG kuralları kullanıyorsanız, bu IP aralıkları izin verilmesi gerekir.

- Kaynak konumuna karşılık gelen tüm IP adresi aralığı.
    - İndirebilirsiniz [IP adres aralıklarını](https://www.microsoft.com/download/confirmation.aspx?id=41653).
    - Böylece veri önbelleği depolama hesabına sanal makineden yazılabilir bu adresleri izin vermeniz gerekir.
- Office 365'e karşılık gelen tüm IP adres aralıklarını [kimlik doğrulama ve kimlik IP V4 uç noktaları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).
    - Yeni bir adres için Office 365 aralıkları gelecekte eklenirse, yeni NSG kuralları oluşturmanız gerekir.
- Site Recovery Hizmeti uç noktası IP adresleri. Bunlar kullanılabilir olan bir [XML dosyası](https://aka.ms/site-recovery-public-ips)ve hedef konumuna bağlıdır.
-  Yapabilecekleriniz [indirin ve bu komut dosyası](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702)üzerinde NSG gerekli kuralları otomatik olarak oluşturmak için. 
- Bir test NSG gerekli NSG kuralları oluşturmak ve bir üretim NSG kuralları oluşturmadan önce herhangi bir sorun olduğunu doğrulayın öneririz.
- NSG kuralları gereken sayısını oluşturmak için aboneliğinizi Güvenilenler listesine olmasını sağlayın. Kişi Azure destek aboneliğinizde NSG kural sınırını artırın.

IP adres aralıklarını aşağıdaki gibidir:

>
   **Hedef** | **Site Recovery IP** |  **Site Recovery IP izleme**
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
   
   
  

## <a name="example-nsg-configuration"></a>Örnek NSG yapılandırma

Bu örnek çoğaltmak bir VM NSG kurallarını yapılandırmak nasıl gösterir. 

- Giden bağlantıyı denetlemek için NSG kuralları kullanıyorsanız, tüm gerekli IP adresi aralıkları için "HTTPS giden izin ver" kurallarını kullanın.
- Örnek VM kaynak konumu "Doğu ABD" olduğunu ve hedef konumu "Orta ABD. olduğunu varsayar.

### <a name="nsg-rules---east-us"></a>NSG kuralları - Doğu ABD

1. Karşılık gelen kuralları oluşturma [BİZE Doğu IP adres aralıklarını](https://www.microsoft.com/download/confirmation.aspx?id=41653). Bu, böylece veri önbelleği depolama hesabına sanal makineden yazılabilir gereklidir.
2. Office 365'e karşılık gelen tüm IP adres aralıkları için kurallar oluşturun [kimlik doğrulama ve kimlik IP V4 uç noktaları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).
3. Hedef konuma karşılık gelen kurallarını oluşturun:

   **Konum** | **Site kurtarma IP adresi** |  **Site kurtarma izleme IP adresi**
    --- | --- | ---
   Orta ABD | 40.69.144.231 | 52.165.34.144

### <a name="nsg-rules---central-us"></a>NSG kuralları - Orta ABD 

Bu kurallar, böylece kaynak bölgesi yük devretme sonrasında için etkin hale getirilebilir çoğaltma hedef bölgesinden gereklidir:

* Karşılık gelen kuralları [merkezi ABD IP aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653). Veri önbelleği depolama hesabına sanal makineden yazılabilir şekilde bu gereklidir.

* Office 365'e karşılık gelen tüm IP aralıkları için kuralları [kimlik doğrulama ve kimlik IP V4 uç noktaları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

* Kaynak konumuna karşılık gelen kuralları:
    - Doğu ABD
    - Site kurtarma IP adresi: 13.82.88.226
    - Site kurtarma IP adresi izleme: 104.45.147.24


## <a name="expressroutevpn"></a>ExpressRoute/VPN 

Şirket içi ve Azure konum arasında bir ExpressRoute veya VPN bağlantısı varsa, bu bölümdeki yönergeleri uygulayın.

### <a name="forced-tunneling"></a>Zorlamalı tünel oluşturma

Genellikle, giden Internet trafiği aracılığıyla şirket içi konuma akış zorlar varsayılan yol (0.0.0.0/0) tanımlayın. Bunu önermiyoruz. Site Recovery hizmeti iletişimi ve çoğaltma trafiğini Azure sınır bırakmamalısınız. Kullanıcı tanımlı yollar (Udr'ler) eklemek için çözümdür [bu IP aralıkları](#outbound-connectivity-for-azure-site-recovery-ip-ranges) böylece şirket içi çoğaltma trafiğini Git değil.

### <a name="connectivity"></a>Bağlantı 

Hedef konumu ve şirket içi konumunuz bağlantılar için bu yönergeleri izleyin:
- Uygulamanız için şirket içi makineler bağlanın veya uygulamaya şirket içi VPN/ExpressRoute bağlanan istemciler varsa, en az bir sağlamak gerekiyorsa [siteden siteye bağlantı](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) arasında Hedef Azure bölgesi ve şirket içi veri merkezi.

- Hedef Azure bölgesi ve şirket içi veri merkezi arasında trafiğe çok bekliyorsanız, başka bir oluşturmalısınız [ExpressRoute bağlantısı](../expressroute/expressroute-introduction.md) hedef Azure bölgesi ve şirket içi veri merkezi arasında.

- Bunlar üzerinde başarısız olduktan sonra sanal makineler için IP korumak istiyorsanız, hedef bölgenin site siteye/ExpressRoute bağlantısı bağlantısı kesik durumda tutun. Hiçbir aralık çakışmasından kaynak bölgesinin IP aralıklarını ve hedef bölgenin IP aralıkları arasında olduğundan emin olmak için budur.

### <a name="expressroute-configuration"></a>ExpressRoute yapılandırma
ExpressRoute yapılandırması için bu en iyi uygulamaları izleyin:

- Bir expressroute bağlantı hattı hem kaynak hem de hedef bölgelerde oluşturmanız gerekir. Ardından arasında bir bağlantı oluşturmanız gerekir:
  - Kaynak sanal ağ ve expressroute bağlantı hattı.
  - Hedef sanal ağ ve expressroute bağlantı hattı.

- ExpressRoute standart bir parçası olarak, aynı coğrafi bölgede devreler oluşturabilirsiniz. ExpressRoute bağlantı hatları farklı coğrafi bölgelerde oluşturmak için Azure ExpressRoute Premium gereklidir artımlı bir maliyet içerir. (ExpressRoute Premium zaten kullanıyorsanız, var. ek bir maliyeti yoktur.) Daha fazla ayrıntı için bkz: [ExpressRoute konumları belge](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) ve [ExpressRoute fiyatlandırma](https://azure.microsoft.com/pricing/details/expressroute/).

- Kaynak ve hedef bölgeler farklı IP aralıkları kullanmanızı öneririz. Expressroute bağlantı hattı iki Azure sanal ağlar aynı IP aralıklarının aynı anda bağlanabiliyor olmayacaktır.

- Her iki bölgelerinde aynı IP aralıklarıyla sanal ağlar oluşturabilir ve sonra ExpressRoute bağlantı hatları her iki bölgelerde oluşturabilirsiniz. Bir yük devretme olayından söz konusu olduğunda bağlantı hattı kaynak sanal ağla olan bağlantınızı kesmek ve hedef sanal ağ bağlantı hattındaki bağlanın.

 >[!IMPORTANT]
 > Birincil bölge tamamen kapalı ise, bağlantıyı kesme işlemi başarısız olabilir. ExpressRoute bağlantı alma, hedef sanal ağ engeller.

## <a name="next-steps"></a>Sonraki adımlar
İş yükleri tarafından korumaya başlamak [Azure sanal makineleri çoğaltmak](site-recovery-azure-to-azure.md).
