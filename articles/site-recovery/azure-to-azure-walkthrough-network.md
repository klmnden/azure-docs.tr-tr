---
title: "Site Recovery ile Azure çoğaltma için VMware ağ planı | Microsoft Docs"
description: "Bu makalede Azure Site Recovery ile Azure sanal makineleri çoğaltırken ağ gerekli planlama açıklanır"
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
ms.date: 08/01/2017
ms.author: sujayt
ms.openlocfilehash: 7b37e853f6b97ba313111f9201f877846a28fae9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="step-3-plan-networking-for-azure-vm-replication"></a>3. adım: Azure VM çoğaltması için ağ planlama

Doğrulandıktan sonra [dağıtımının önkoşulları](azure-to-azure-walkthrough-prerequisites.md), çoğaltma ve Azure sanal makineleri (VM'ler) bir Azure bölgesinden diğerine kurtarma ağ konuları anlamak için bu makaleyi okuyun Azure kullanma Site Recovery hizmeti. 

- Makaleyi tamamladığınızda, Azure sanal makineleri çoğaltmak istediğiniz için giden erişimi ayarlamak nasıl ve bu sanal makineleri şirket içi sitenizden bağlanma NET bir anlayış olmalıdır.
- Tüm yorumlarınızı bu makalenin alt kısmında paylaşabilir veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda soru sorabilirsiniz.

>[!NOTE]
> Site Recovery ile Azure VM çoğaltma şu anda önizlemede değil.



## <a name="network-overview"></a>Ağ genel bakış

Şirket içi sitenizi bir bağlantıdan Azure ExpressRoute kullanarak veya bir VPN bağlantısı yoktur ve genellikle bir Azure sanal ağ/alt ağında Azure Vm'leriniz bulunur.

Ağlar genellikle güvenlik duvarları kullanarak korunur ve/veya ağ güvenlik grubu (Nsg'ler). Güvenlik duvarları URL tabanlı denetim ağ bağlantısı için IP tabanlı listeleri kullanabilirsiniz. Nsg'ler IP aralığı tabanlı kurallarını kullanın. 


Site Recovery'nın beklenen çalışmaya çoğaltmak istediğiniz VM'lerin giden ağ bağlantısı'nda bazı değişiklikler yapmanız gerekir. Site kurtarma denetim ağ bağlantısı için bir kimlik doğrulama proxy'si kullanımını desteklemez. Bir kimlik doğrulama proxy'si varsa, çoğaltma etkinleştirilemez. 


Aşağıdaki diyagram tipik bir ortamı Azure Vm'lerinde çalışan bir uygulama için gösterir.

![Müşteri ortamı](./media/azure-to-azure-walkthrough-network/source-environment.png)

**Azure VM ortamı**

Ayrıca, şirket içi sitenizden ayarlanan Azure Azure ExpressRoute veya bir VPN bağlantısı kullanarak bağlantı olabilir. 

![Müşteri ortamı](./media/azure-to-azure-walkthrough-network/source-environment-expressroute.png)

**Azure için şirket içi bağlantısı**



## <a name="outbound-connectivity-for-urls"></a>URL'ler için giden bağlantı

Giden bağlantıyı denetlemek için bir URL tabanlı bir güvenlik duvarı proxy kullanıyorsanız, Site Recovery tarafından kullanılan bu URL'leri izin emin olun

**URL** | **Ayrıntılar**  
--- | ---
*.blob.core.windows.net | Sanal makineden kaynak bölge önbellek depolama hesabına yazılması veri sağlar.
Login.microsoftonline.com | Yetkilendirme ve kimlik doğrulaması için Site Recovery hizmeti URL'lerine sağlar.
*.hypervrecoverymanager.windowsazure.com | Site Recovery hizmeti ile iletişimi sanal makineden sağlar.
*. servicebus.windows.net | Böylece Site Recovery izleme ve tanılama verilerini sanal makineden yazılabilir gereklidir.

## <a name="outbound-connectivity-for-ip-address-ranges"></a>IP adres aralıkları için giden bağlantı

- Otomatik olarak gerekli kuralları üzerinde NSG indirme ve çalıştırma oluşturabilirsiniz [bu komut dosyası](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).
- Bir test NSG gerekli NSG kuralları oluşturmak ve bir üretim NSG kuralları oluşturmadan önce herhangi bir sorun olduğunu doğrulayın öneririz.
- NSG kuralları gereken sayısını oluşturmak için aboneliğinizi Güvenilenler listesine olmasını sağlayın. Aboneliğinizde NSG kural sınırını artırmak üzere desteğe başvurun.
Giden bağlantıyı denetlemek için herhangi bir IP tabanlı bir güvenlik duvarı proxy veya NSG kuralları kullanıyorsanız, aşağıdaki IP aralıklarını sanal makinelerin kaynak ve hedef konumların bağlı olarak güvenilir listesinde olması gerekir:

    - Kaynak konumuna karşılık gelen tüm IP adresi aralığı. Karşıdan [aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653).) Böylece veri önbelleği depolama hesabına sanal makineden yazılabilir uygulamaları güvenilir listeye almayı gereklidir.
    - Office 365'e karşılık gelen tüm IP aralıklarını [kimlik doğrulama ve kimlik IP V4 uç noktaları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity). Yeni IP Office 365 IP aralıklarına eklediyseniz, bu yeni NSG kuralları oluşturmanız gerekir.
-     Site Recovery Hizmeti uç noktası IP adresleri ([bir XML dosyasında kullanılabilir](https://aka.ms/site-recovery-public-ips)), hedef konumuna bağlıdır: 

   **Hedef konumu** | **Site Recovery hizmeti IP'leri** |  **Site Recovery IP izleme**
   --- | --- | ---
   Doğu Asya | 52.175.17.132</br>40.83.121.61 | 13.94.47.61
   Güneydoğu Asya | 52.187.58.193</br>52.187.169.104 | 13.76.179.223
   Orta Hindistan | 52.172.187.37</br>52.172.157.193 | 104.211.98.185
   Güney Hindistan | 52.172.46.220</br>52.172.13.124 | 104.211.224.190
   Orta Kuzey ABD | 23.96.195.247</br>23.96.217.22 | 168.62.249.226
   Kuzey Avrupa | 40.69.212.238</br>13.74.36.46 | 52.169.18.8
   Batı Avrupa | 52.166.13.64</br>52.166.6.245 | 40.68.93.145
   Doğu ABD | 13.82.88.226</br>40.71.38.173 | 104.45.147.24
   Batı ABD | 40.83.179.48</br>13.91.45.163 | 104.40.26.199
   Orta Güney ABD | 13.84.148.14</br>13.84.172.239 | 104.210.146.250
   Orta ABD | 40.69.144.231</br>40.69.167.116 | 52.165.34.144
   Doğu ABD 2 | 52.184.158.163</br>52.225.216.31 | 40.79.44.59
   Japonya Doğu | 52.185.150.140</br>13.78.87.185 | 138.91.1.105
   Japonya Batı | 52.175.146.69</br>52.175.145.200 | 138.91.17.38
   Güney Brezilya | 191.234.185.172</br>104.41.62.15 | 23.97.97.36
   Avustralya Doğu | 104.210.113.114</br>40.126.226.199 | 191.239.64.144
   Avustralya Güneydoğu | 13.70.159.158</br>13.73.114.68 | 191.239.160.45
   Orta Kanada | 52.228.36.192</br>52.228.39.52 | 40.85.226.62
   Doğu Kanada | 52.229.125.98</br>52.229.126.170 | 40.86.225.142
   Batı Orta ABD | 52.161.20.168</br>13.78.230.131 | 13.78.149.209
   Batı ABD 2 | 52.183.45.166</br>52.175.207.234 | 13.66.228.204
   Birleşik Krallık Batı | 51.141.3.203</br>51.140.226.176 | 51.141.14.113
   Birleşik Krallık Güney | 51.140.43.158</br>51.140.29.146 | 51.140.189.52

## <a name="example-nsg-configuration"></a>Örnek NSG yapılandırma

Bir VM için çoğaltmalar çalışır böylece bu bölümde NSG kurallarının nasıl yapılandırıldığı gösterir. Giden bağlantıyı denetlemek için NSG kuralları kullanıyorsanız, "HTTPS giden izin ver" kuralları için tüm gerekli IP aralıklarını kullanın.

Bu örnekte, "Doğu ABD" VM kaynak konum değil. Çoğaltma hedef Orta ABD konumdur.


### <a name="example"></a>Örnek

#### <a name="east-us"></a>Doğu ABD

1. Karşılık gelen kuralları oluşturma [Doğu ABD IP aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653). Bu, böylece veri önbelleği depolama hesabına sanal makineden yazılabilir gereklidir.
2. Office 365'e karşılık gelen tüm IP aralıkları için kurallar oluşturun [kimlik doğrulama ve kimlik IP V4 uç noktaları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).
3. Hedef konuma karşılık gelen kurallarını oluşturun:

   **Konum** | **Site Recovery hizmeti IP'leri** |  **Site Recovery IP izleme**
    --- | --- | ---
   Orta ABD | 40.69.144.231</br>40.69.167.116 | 52.165.34.144

#### <a name="central-us"></a>Orta ABD

Bu kurallar, böylece yük devretme sonrasında çoğaltma hedef bölgesinden kaynak bölgesine etkinleştirilebilir gereklidir.

1. Karşılık gelen kuralları oluşturma [merkezi ABD IP aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653).
2. Office 365'e karşılık gelen tüm IP aralıkları için kurallar oluşturun [kimlik doğrulama ve kimlik IP V4 uç noktaları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).
3. Kaynak konumuna karşılık gelen kurallarını oluşturun:

   **Konum** | **Site Recovery hizmeti IP'leri** |  **Site Recovery IP izleme**
    --- | --- | ---
   Doğu ABD | 13.82.88.226</br>40.71.38.173 | 104.45.147.24


## <a name="existing-on-premises-connection"></a>Var olan bağlantı şirket içi

Şirket içi siteniz ve azure'da kaynak konumu arasında bir ExpressRoute veya VPN bağlantınız varsa, aşağıdaki yönergeleri izleyin:

**Yapılandırma** | **Ayrıntılar**
--- | ---
**Zorlanan tünel** | Genellikle varsayılan yol (0.0.0.0/0) aracılığıyla şirket içi konuma giden internet akışına zorlar. Bu önerilmemektedir. Çoğaltma trafiği ve Site Recovery iletişimleri Azure içinde kalır.<br/><br/> Bir çözüm olarak, kullanıcı tanımlı yollar (Udr'ler) add [bu IP aralıkları](#outbound-connectivity-for-azure-site-recovery-ip-ranges), böylece trafiğin şirket içi Git değil.
**Bağlantı** | Uygulamaları gerek şirket içi makinelere bağlanmak ya da şirket içi istemcilerde VPN/ExpressRoute şirket içi uygulamaya bağlanmak, elinizde en az bir emin [siteden siteye bağlantı](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), arasında hedef Azure bölgesi ve Şirket içi site.<br/><br/> Hedef Azure bölgesi ve şirket içi site arasında trafiği birimleri yüksekse, başka birini oluşturmak [ExpressRoute bağlantısı](../expressroute/expressroute-introduction.md), hedef bölgesi ve şirket içi arasında.<br/><br/> IP'leri VM'ler için yük devretme sonrasında korumak istiyorsanız, hedef bölgenin site siteye/ExpressRoute bağlantısı bağlantısı kesik durumda tutun. Bu, kaynak ve hedef IP adresi aralıkları arasında hiçbir aralık çakışmalarından vardır sağlar.
**ExpressRoute** | Bir expressroute bağlantı hattı kaynak ve hedef bölgeler oluşturun.<br/><br/> Kaynak ağ ile ExpressRoute devresi arasında ve hedef ağ ile bağlantı hattı arasında bir bağlantı oluşturun.<br/><br/> Kaynak ve hedef bölgeler farklı IP aralıkları kullanmanızı öneririz. Bağlantı hattı aynı anda birden fazla Azure ağları ile aynı IP aralıkları bağlanmak mümkün olmayacaktır.<br/><br/> Her iki bölgelerinde aynı IP aralıklarıyla sanal ağlar oluşturabilir ve sonra ExpressRoute bağlantı hatları her iki bölgelerde oluşturun. Yük devretme, kaynak sanal ağdan bağlantı hattı kesin ve hedef sanal ağ'daki hattına bağlayın.<br/><br/> Birincil bölge tamamen kapalı ise, bağlantıyı kesme işlemi başarısız olabilir. Bu durumda, hedef sanal ağ ExpressRoute bağlantısı alamazsınız.
**ExpressRoute Premium** | Aynı coğrafi bölgede devreler oluşturabilirsiniz.<br/><br/> Farklı coğrafi bölgelerde devreler oluşturmak için Azure ExpressRoute Premium gerekir.<br/><br/> Premium ile artımlı maliyetidir. Zaten kullanıyorsanız, ek bir maliyet yoktur.<br/><br/> Daha fazla bilgi edinmek [konumları](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) ve [fiyatlandırma](https://azure.microsoft.com/pricing/details/expressroute/).



## <a name="next-steps"></a>Sonraki adımlar

Git [4. adım: bir kasa oluşturun](azure-to-azure-walkthrough-vault.md)

