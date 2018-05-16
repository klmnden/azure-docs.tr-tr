---
title: Yollar - PowerShell sorunlarını giderme | Microsoft Docs
description: Azure PowerShell kullanarak Azure Resource Manager dağıtım modelinde yollar giderileceğini öğrenin.
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: ''
tags: azure-resource-manager
ms.assetid: bf7dc5e7-9399-460e-8e0d-8992dbed98a6
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: 4ef1387e3c8573a2bfa64c166f08bf47723eca62
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="troubleshoot-routes-using-azure-powershell"></a>Azure PowerShell kullanarak yolları sorun giderme
> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-routes-troubleshoot-portal.md)
> * [PowerShell](virtual-network-routes-troubleshoot-powershell.md)
> 
> 

Ağ bağlantısı sorunları için veya, Azure sanal makine'nden (VM) karşılaşıyorsanız, yolları VM trafik akışına etkiliyor olabilir. Bu makalede daha fazla gidermek rotalar için tanılama özelliklerine genel bakış sağlar.

Yönlendirme tabloları alt ağ ile ilişkili ve tüm ağ arabirimlerindeki (NIC) bu alt ağdaki etkili olur. Aşağıdaki türde yollar her bir ağ arabirimine uygulanabilir:

* **Sistem yolları:** varsayılan olarak, yerel VNet trafiği, VPN ağ geçitleri üzerinden şirket içi trafik ve Internet trafiğini izin sistem yönlendirme tabloları bir Azure sanal ağı (VNet) içinde oluşturulan her alt ağ vardır. Sistem yolları da eşlenmiş Vnet'ler için mevcut.
* **BGP yolları:** ExpressRoute veya siteden siteye VPN bağlantıları üzerinden ağ arabirimlerine yayılır. Okuyarak BGP yönlendirme hakkında daha fazla bilgi [VPN gateways ile BGP'ye](../vpn-gateway/vpn-gateway-bgp-overview.md) ve [ExpressRoute genel bakış](../expressroute/expressroute-introduction.md) makaleleri.
* **Kullanıcı tanımlı yolları (UDR):** ağ sanal Gereçleri veya olan kullanıyorsanız zorlamalı tünel trafiği için bir şirket içi ağ bir siteden siteye VPN aracılığıyla alt yol tablosu ile ilişkili kullanıcı tanımlı yollar (Udr'ler) olabilir. İle Udr'ler bilmiyorsanız okuma [kullanıcı tanımlı yollar](virtual-networks-udr-overview.md#user-defined) makalesi.

Bir ağ arabirimi için uygulanabilecek çeşitli yollar, hangi toplama yolların etkili belirlemek zor olabilir. VM ağ bağlantısı gidermenize yardımcı olması için Azure Resource Manager dağıtım modelinde bir ağ arabirimi için tüm etkin yollar görüntüleyebilirsiniz.

## <a name="using-effective-routes-to-troubleshoot-vm-traffic-flow"></a>VM trafik akışı gidermek için etkili yolları kullanma
Bu makalede, bir ağ arabirimi için etkili rotaları ile ilgili sorunları giderme göstermek için örnek olarak aşağıdaki senaryoyu kullanır:

Bir VM'yi (*VM1*) Vnet'e bağlı (*VNet1*, öneki: 10.9.0.0/16) VM(VM3) yeni vnet'teki bağlanamıyor (*VNet3*, 10.10.0.0/16 öneki). VM'ye bağlı VM1 nıc1 ağ arabirimine uygulanan Udr'ler ya da BGP yolları, yalnızca sistem yolları uygulanır vardır.

Bu makalede, Azure kaynak yönetimi dağıtım modelinde etkili yollar özelliği kullanarak bağlantı hatanın nedenini belirlemek açıklanmaktadır.
Bu örnek yalnızca sistem yolları kullanır, ancak aynı adımlar herhangi bir rota türü gelen ve giden bağlantı hataları belirlemek için kullanılabilir.

> [!NOTE]
> VM bağlı birden çok NIC varsa, her bir VM gelen ve giden ağ bağlantısı sorunları tanılamak için NIC etkili yolları denetleyin.
> 
> 

### <a name="view-effective-routes-for-a-virtual-machine"></a>Bir sanal makine için Görünüm etkili yolları
Bir VM'ye uygulanan toplama rotaları görmek için aşağıdaki adımları tamamlayın:

### <a name="view-effective-routes-for-a-network-interface"></a>Bir ağ arabirimi için Görünüm etkili yolları
Bir ağ arabirimine uygulanan toplama rotaları görmek için aşağıdaki adımları tamamlayın:

1. Bir Azure PowerShell oturumu ve Azure oturum açma başlatın. Azure PowerShell ile bilmiyorsanız okuma [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview) makale. Hesabınızı atanmalıdır *Microsoft.Network/networkInterfaces/effectiveRouteTable/action* ağ arabirimi için işlemi. Operations hesaplara atamak üzere öğrenmek için bkz: [Azure rol tabanlı erişim denetimi için özel roller oluşturmanızı](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Aşağıdaki komut, tüm yolları adlı ağ arabirimi için uygulanan döndürür *VM1 nıc1* kaynak grubunda *RG1*.
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [!TIP]
   > Bir ağ arabirimi adını bilmiyorsanız, bir kaynak group.* tüm ağ arabirimlerini adları almak için aşağıdaki komutu yazın
   > 
   > 
   
       Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name
   
   Aşağıdaki çıkış NIC bağlı olduğu alt ağa uygulanan her yolunun çıktıya benzer:
   
       Name :
       State : Active
       AddressPrefix : {10.9.0.0/16}
       NextHopType : VNetLocal
       NextHopIpAddress : {}
   
       Name :
       State : Active
       AddressPrefix : {0.0.0.0/16}
       NextHopType : Internet
       NextHopIpAddress : {}
   
   Çıktı aşağıdakilere dikkat edin:
   
   * **Ad**: açıkça belirtilen, kullanıcı tanımlı yollar için sürece etkili rotanın adı boş olabilir. 
   * **Durum**: etkin yönlendirme durumunu gösterir. "Active" veya "Geçersiz" olası değerler şunlardır:
   * **AddressPrefixes**: CIDR gösteriminde etkin yönlendirme adresi öneki belirtir. 
   * **nextHopType**: belirli bir rota için sonraki atlama gösterir. Olası değerler şunlardır: *değerinin VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*, veya *Null*. Değerini *Null* için **nextHopType** bir UDR geçersiz bir yol gösteriyor olabilir. Örneğin, varsa **nextHopType** olan *değerinin VirtualAppliance* ve ağ sanal gereç VM sağlanan çalışıyor durumda değil. Varsa **nextHopType** olan *VPNGateway* ve sağlanan/çalışan verilen VNet içinde yol geçersiz hale ağ geçidi yok.
   * **Nexthopıpaddress**: etkin yolunun sonraki atlama IP adresini belirtir.
   
   Aşağıdaki komut yolları tablosunu görüntülemek bir kolay içinde döndürür:
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1 | Format-Table
   
   Aşağıdaki çıkış daha önce açıklanan senaryo için alınan çıkış bazıları verilmiştir:
   
       Name State AddressPrefix NextHopType NextHopIpAddress
       ---- ----- ------------- ----------- ----------------
       Active {10.9.0.0/16} VnetLocal {}
       Active {0.0.0.0/0} Internet {}
3. Listelenen yol yok *WestUS VNet3* VNet (10.10.0.0/16)** gelen önek *WestUS VNet1* (10.9.0.0/16 önek) önceki adımdaki çıktı. Aşağıdaki resimde gösterildiği gibi VNet eşlemesi ile bağlantı *WestUS VNet3* VNet konusu *bağlantı kesildi* durumu.
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)
   
    Çift yönlü bağlantı eşlemeyi bozuk için açıklayan neden VM1 içinde VM3 bağlanılamadı *WestUS VNet3* VNet. Çift yönlü bir VNet eşleme bağlantısı yeniden için Kurulum *WestUS VNet1* ve *WestUS VNet3* sanal ağlar. VNet eşleme bağlantısı doğru kurulduktan sonra döndürülen çıktının aşağıdaki gibidir:
   
        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {10.10.0.0/16} VNetPeering {}
        Active {0.0.0.0/0} Internet {}
   
    Sorunu belirledikten sonra ekleyebilir, Kaldır, veya yolları değiştirmek ve tabloları rota. Bunu yapmak için kullanılan komutların listesini görmek için aşağıdaki komutu yazın:
   
        Get-Help *-AzureRmRouteConfig

## <a name="considerations"></a>Dikkat edilmesi gerekenler
Yolların listesini gözden geçirirken göz önünde bulundurmanız gereken birkaç şey döndürdü:

* Yönlendirme üzerinde en uzun ön ek eşleşmesi (LPM) Udr'ler arasında temel BGP ve sistem yolları. Aynı LPM eşleşmesine sahip birden fazla yol varsa, bir yol aşağıdaki sırayla ve kaynağına göre seçilir:
  
  * Kullanıcı tanımlı yol
  * BGP yolu
  * Sistem (varsayılan) yolu
    
    Etkin yollar ile tüm kullanılabilir yollara dayalı LPM eşleşmesine olan etkili yollar yalnızca görebilirsiniz. Nasıl yollar için belirli bir NIC gerçekte değerlendirildiği göstererek Bu, çok bağlantı denetleyicisinden VM etkileyen belirli yollar gidermek kolaylaştırır.
* Udr'ler varsa ve trafiği bir ağ sanal gereç (NVA) ile göndermeye *değerinin VirtualAppliance* olarak **nextHopType**, IP iletimini trafiğini alma NVA üzerinde etkinleştirildiğinden emin olun veya paket bırakılır. 
* Zorlamalı tünel etkinse, tüm giden Internet trafiği şirket içi yönlendirilir. Şirket içi bu trafiği nasıl işler bağlı olarak bu ayar, VM için RDP/SSH Internet'ten çalışmayabilir. 
  Zorlamalı tünel etkinleştirilebilir:
  * Siteden siteye VPN, VPN ağ geçidi olarak nextHopType ile bir kullanıcı tanımlı yönlendirme (UDR) ayarlayarak kullanıyorsanız
  * Varsayılan rota BGP Tanıtımı
* Bir sistem rotası doğru çalışması VNet eşleme trafiği için **nextHopType** *VNetPeering* eşlenmiş sanal ağınızın önek aralığının mevcut olması gerekir. Böyle bir yol yok ve VNet eşleme bağlantısı sorunsuz görünüyor varsa:
  * Birkaç saniye bekleyin ve yeni oluşturulmuş bir eşleme bağlantısı ise, yeniden deneyin. Bazen, bir alt ağdaki tüm ağ arabirimleri yolları yaymak için daha uzun sürer.
  * Ağ güvenlik grubu (NSG) kuralları, trafik akışlarının etkiliyor olabilir. Daha fazla bilgi için bkz: [sorun giderme ağ güvenlik grupları](virtual-network-nsg-troubleshoot-powershell.md) makalesi.

