---
title: "Ağ güvenlik grupları - PowerShell sorunlarını giderme | Microsoft Docs"
description: "Ağ güvenlik grupları Azure PowerShell kullanarak Azure Resource Manager dağıtım modelinde sorun giderme öğrenin."
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 4c732bb7-5cb1-40af-9e6d-a2a307c2a9c4
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: d556f2d6d37956c3b3bca2a2905b2c947e6be0df
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="troubleshoot-network-security-groups-using-azure-powershell"></a>Ağ güvenlik grupları Azure PowerShell kullanarak sorun giderme
> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-nsg-troubleshoot-portal.md)
> * [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
> 
> 

Ağ güvenlik grupları (Nsg'ler), sanal makine (VM) üzerinde yapılandırılmış ve VM bağlantı sorunları yaşıyorsanız, bu makalede daha fazla gidermek Nsg'ler tanılama özelliklerine genel bakış sağlar.

Nsg'ler sanal makineleri (VM'ler) ve akan trafik türlerini denetlemenize sağlar. Nsg'ler alt ağlara bir Azure sanal ağı (VNet), ağ arabirimleri (NIC) ya da her ikisini de uygulanabilir. Bir NIC uygulanan etkili bir NIC'ye uygulanan Nsg'ler mevcut kurallar ve bağlı olduğu alt ağ bir toplama kurallardır. Bu Nsg'ler arasında kuralları bazen birbiriyle çelişen ve bir sanal makinenin ağ bağlantısını etkileyebilir.  

VM Nıc'lerde uygulanan olarak, tüm etkin güvenlik kuralları Nsg'lerinizi görüntüleyebilirsiniz. Bu makalede, bu kurallar Azure Resource Manager dağıtım modelinde kullanarak VM bağlantı sorunlarını gidermek gösterilmiştir. VNet ve NSG kavramlarına alışık değilseniz, okuma [sanal ağ](virtual-networks-overview.md) ve [ağ güvenlik grupları](virtual-networks-nsg.md) genel bakış makaleleri.

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a>VM trafik akışı sorun giderme için etkili güvenlik kurallarını kullanma
Aşağıdaki senaryoda, ortak bir bağlantı sorunu örneğidir:

Adlı bir VM'den *VM1* adlı bir alt ağın parçası olan *Subnet1* adlı bir sanal ağ içindeki *WestUS VNet1*. 3389 numaralı TCP bağlantı noktası üzerinden RDP kullanarak VM bağlanma denemesi başarısız olur. Nsg'ler, her iki NIC üzerinde uygulanır *VM1 nıc1* ve alt ağ *Subnet1*. TCP bağlantı noktası 3389 trafiği ağ arabirimiyle ilişkilendirilmiş NSG izin verilen *VM1 nıc1*, kullanıcının VM1 ping TCP bağlantı noktası ancak 3389 başarısız olur.

Bu örnek 3389 numaralı TCP bağlantı noktasını kullanır, ancak herhangi bir bağlantı noktası gelen ve giden bağlantı hataları belirlemek için aşağıdaki adımları kullanılabilir.

## <a name="detailed-troubleshooting-steps"></a>Ayrıntılı sorun giderme adımları
Nsg'leri bir VM için sorun giderme için aşağıdaki adımları tamamlayın:

1. Bir Azure PowerShell oturumu ve Azure oturum açma başlatın. Azure PowerShell ile bilmiyorsanız okuma [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview) makale. Hesabınızı atanmalıdır *Microsoft.Network/networkInterfaces/effectiveNetworkSecurityGroups/action* ağ arabirimi için işlemi. Operations hesaplara atamak üzere öğrenmek için bkz: [Azure rol tabanlı erişim denetimi için özel roller oluşturmanızı](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#actions).
2. Adlı bir NIC uygulanan tüm NSG kuralları döndürmek için aşağıdaki komutu girin *VM1 nıc1* kaynak grubunda *RG1*:
   
        Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [!TIP]
   > Bir NIC adını bilmiyorsanız, bir kaynak grubundaki tüm NIC'ler adları almak için aşağıdaki komutu girin: 
   > 
   > `Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name`
   > 
   > 
   
    Aşağıdaki metni için döndürülen etkili kuralları çıkış örneğidir *VM1 nıc1* NIC:
   
        NetworkSecurityGroup   : {
                                   "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/VM1-NIC1-NSG"
                                 }
        Association            : {
                                   "NetworkInterface": {
                                     "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkInterfaces/VM1-NIC1"
                                   }
                                 }
        EffectiveSecurityRules : [
                                 {
                                 "Name": "securityRules/allowRDP",
                                 "Protocol": "Tcp",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "3389-3389",
                                 "SourceAddressPrefix": "Internet",
                                 "DestinationAddressPrefix": "0.0.0.0/0",
                                 "ExpandedSourceAddressPrefix": [… ],
                                 "ExpandedDestinationAddressPrefix": [],
                                 "Access": "Allow",
                                 "Priority": 1000,
                                 "Direction": "Inbound"
                                 },
                                 {
                                 "Name": "defaultSecurityRules/AllowVnetInBound",
                                 "Protocol": "All",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "0-65535",
                                 "SourceAddressPrefix": "VirtualNetwork",
                                 "DestinationAddressPrefix": "VirtualNetwork",
                                 "ExpandedSourceAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                 "ExpandedDestinationAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                  "Access": "Allow",
                                  "Priority": 65000,
                                  "Direction": "Inbound"
                                  },…
                         ]
   
        NetworkSecurityGroup   : {
                                   "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/Subnet1-NSG"
                                 }
        Association            : {
                                   "Subnet": {
                                     "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/virtualNetworks/WestUS-VNet1/subnets/Subnet1"
                                 }
                                 }
        EffectiveSecurityRules : [
                                 {
                                "Name": "securityRules/denyRDP",
                                "Protocol": "Tcp",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "3389-3389",
                                "SourceAddressPrefix": "Internet",
                                "DestinationAddressPrefix": "0.0.0.0/0",
                                "ExpandedSourceAddressPrefix": [
                                   ... ],
                                "ExpandedDestinationAddressPrefix": [],
                                "Access": "Deny",
                                "Priority": 1000,
                                "Direction": "Inbound"
                                },
                                {
                                "Name": "defaultSecurityRules/AllowVnetInBound",
                                "Protocol": "All",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "0-65535",
                                "SourceAddressPrefix": "VirtualNetwork",
                                "DestinationAddressPrefix": "VirtualNetwork",
                                "ExpandedSourceAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "ExpandedDestinationAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "Access": "Allow",
                                "Priority": 65000,
                                "Direction": "Inbound"
                                },...
                                ]
   
    Çıktı aşağıdaki bilgileri unutmayın:
   
   * Var olan iki **NetworkSecurityGroup** bölümler: bir alt ağ ile ilişkili biridir (*Subnet1*) ve bir NIC ile ilişkili olduğu (*VM1 nıc1*). Bu örnekte, her bir NSG uygulanmıştır.
   * **İlişkilendirme** kaynak (alt ağ veya NIC) belirli bir NSG ile ilişkili olduğunu gösterir. NSG kaynağı hemen bu komutu çalıştırmadan önce taşınmış ve ilişkilendirmesi ise, komut çıktısında yansıtacak şekilde değiştirmek için birkaç saniye beklemeniz gerekebilir. 
   * İle başlayan kuralı adları *defaultSecurityRules*: olduğunda bir NSG oluşturulur, birkaç varsayılan güvenlik kuralları içinde oluşturulur. Varsayılan kurallar kaldırılamaz, ancak daha yüksek öncelik kuralları ile geçersiz kılınabilir.
     Okuma [NSG genel bakış](virtual-networks-nsg.md#default-rules) makale güvenlik kuralları varsayılan NSG hakkında daha fazla bilgi edinin.
   * **ExpandedAddressPrefix** NSG varsayılan etiketleri için adres önekleri genişletir. Etiketler, birden çok adres öneklerini temsil eder. Etiketlerin genişletme denetleyicisinden belirli adres öneklerini VM bağlantı sorunlarını giderirken faydalı olabilir. Örneğin, VNET eşlemesi ile VIRTUAL_NETWORK etiketine eşlenmiş VNet ön önceki çıktısında göstermek için genişletir.
     
     > [!NOTE]
     > Bir NSG'yi bir alt ağ, bir NIC veya her ikisi ile ilişkili ise, komut yalnızca gösterir etkili kuralları. Bir VM uygulanan farklı Nsg'ler ile birden çok NIC olabilir. Sorunlarını giderirken, her bir NIC komutunu çalıştırın
     > 
     > 
3. Daha fazla sayıda NSG kuralları filtreleme kolaylaştırmak için daha fazla sorun giderme için aşağıdaki komutları girin: 
   
        $NSGs = Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
        $NSGs.EffectiveSecurityRules | Sort-Object Direction, Access, Priority | Out-GridView
   
    RDP trafiğine (TCP bağlantı noktası 3389) için bir filtre aşağıdaki resimde gösterildiği gibi kılavuz görünümüne uygulanır:
   
    ![Kuralları listesi](./media/virtual-network-nsg-troubleshoot-powershell/rules.png)
4. Izgara görünümünde görebileceğiniz gibi vardır her ikisi de izin ver ve Reddet kurallarının için RDP. 2. adım çıktısını gösterir *DenyRDP* alt ağa uygulanan nsg'deki kuralıdır. Gelen kuralları için alt ağa uygulanan Nsg'ler önce işlenir. Bir eşleşme bulunursa, bir ağ arabirimine uygulanan NSG işlenmez. Bu durumda, *DenyRDP* alt ağdan kural, VM için RDP engeller (**VM1**).
   
   > [!NOTE]
   > Bir VM ekli birden çok NIC olabilir. Her farklı bir alt ağa bağlı olabilir. Önceki adımları komutlar bir NIC karşı çalışır olduğundan, bağlantı hatası yaşıyoruz NIC belirttiğinizden emin olun önemlidir. Emin değilseniz VM'ye bağlı her NIC karşı komutları her zaman çalıştırabilirsiniz.
   > 
   > 
5. VM1 halinde RDP için değiştirme *Reddet RDP (3389)* kuralı *izin RDP(3389)* içinde **Subnet1 NSG** NSG. TCP bağlantı noktası 3389 VM için RDP bağlantısı açarak veya PsPing aracını kullanarak açık olduğundan emin olun. Okuyarak PsPing hakkında daha fazla bilgiyi [PsPing indirme sayfası](https://technet.microsoft.com/sysinternals/psping.aspx)
   
    Aşağıdaki komut çıktısı bilgileri kullanarak bir NSG kuralları kaldırın ve yapabilirsiniz:
   
        Get-Help *-AzureRmNetworkSecurityRuleConfig

## <a name="considerations"></a>Dikkat edilmesi gerekenler
Bağlantı sorunlarını giderme yaparken aşağıdaki noktaları dikkate alın:

* Varsayılan NSG kuralları internet'ten gelen erişimi engellemek ve yalnızca VNet izin gelen trafiği. Internet'ten gelen erişim gerektiğinde izin vermek için açıkça kuralları eklenmesi gerekir.
* Bir sanal makinenin ağ bağlantısı başarısız olmasına neden olan hiçbir NSG güvenlik kuralları varsa, sorunu nedeniyle olabilir:
  * Sanal makinenin işletim sistemi içinde çalışan güvenlik duvarı yazılımı
  * Sanal gereçler veya şirket içi trafiği için yapılandırılmış yollar. Internet trafiği, şirket içi zorlamalı tünel aracılığıyla yönlendirilebilir. VM Internet'ten bir RDP/SSH bağlantısı nasıl şirket içi ağ donanımı bu trafiği işler bağlı olarak bu ayar ile çalışmayabilir. Okuma [sorun giderme yolları](virtual-network-routes-troubleshoot-powershell.md) makale içeri ve dışarı VM trafik akışını engelleyen rota sorunları tanılamak öğrenin. 
* Varsayılan olarak sanal ağlar, eşlenen, VIRTUAL_NETWORK etiketine önekler için eşlenmiş Vnet'lerde dahil etmek için otomatik olarak genişletilir. Bu önekleri görüntüleyebilirsiniz **ExpandedAddressPrefix** VNet eşleme bağlantısı ilgili sorunları gidermek için liste. 
* Etkin güvenlik kuralları yalnızca VM NIC ve veya alt ağ ilişkilendirilmiş bir NSG olup olmadığını gösterilir. 
* Alt ağ ve NIC ile ilişkili hiçbir Nsg'ler vardır veya VM'nize atanan genel bir IP adresi varsa, tüm bağlantı noktalarına gelen ve giden erişimi açık olacaktır. VM bir ortak IP adresi varsa, NIC veya alt ağ Nsg'ler uygulanması önerilir.  

