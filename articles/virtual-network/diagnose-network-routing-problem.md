---
title: Bir Azure sanal makinesi yönlendirme sorunu tanılamak | Microsoft Docs
description: Bir sanal makine için etkili rotaları görüntüleyerek bir sanal makine yönlendirme sorunu tanılamak öğrenin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/30/2018
ms.author: jdial
ms.openlocfilehash: 07352a5d7c8b465440efab17c654979662a95f8e
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34702395"
---
# <a name="diagnose-a-virtual-machine-routing-problem"></a>Bir sanal makine yönlendirme sorunu tanılamak

Bu makalede, bir sanal makine (VM) bir ağ arabiriminde etkili rotaları görüntüleyerek bir yönlendirme sorunu tanılamak öğrenin. Azure her sanal ağ alt ağı için birden fazla varsayılan yolların oluşturur. Bir rota tablosunda yolları tanımlayarak ve bir alt ağ için yol tablosu ilişkilendirme Azure'nın varsayılan yolların geçersiz kılabilirsiniz. Oluşturduğunuz yollar, Azure'nın varsayılan yolların ve bir Azure VPN ağ geçidi üzerinden şirket içi ağınızdan (sanal ağınız, şirket içi ağınıza bağlı değilse) sınır ağ geçidi Protokolü (BGP) yayılan yollar birleşimi bir alt ağdaki tüm ağ arabirimleri için etkili yolları. Sanal ağ, ağ arabirimi veya kavramları yönlendirme ile bilmiyorsanız, bkz: [sanal ağa genel bakış](virtual-networks-overview.md), [ağ arabirimi](virtual-network-network-interface.md), ve [yönlendirmeye genel bakış](virtual-networks-udr-overview.md).

## <a name="scenario"></a>Senaryo

Bir VM öğesine bağlanmaya çalışır, ancak bağlantı başarısız olur. Neden VM'ime bağlanamıyorum belirlemek için Azure kullanarak bir ağ arabirimi için etkili rotaları görüntüleyebilirsiniz [portal](#diagnose-using-azure-portal), [PowerShell](#diagnose-using-powershell), veya [Azure CLI](#diagnose-using-azure-cli).

Adımları için etkili rotaları görüntülemek için mevcut bir VM'yi olduğunu varsayın. Mevcut bir VM'yi yoksa, öncelikle dağıtmak bir [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bu makalede görevleri tamamlamak için VM. Bu makaledeki örneklerde adlı bir VM için olan *myVM* sahip bir ağ arabirimi adlı *myVMVMNic*. VM ve ağ arabirimi olan adlı bir kaynak grubunda *myResourceGroup*ve *Doğu ABD* bölge. Sorun için tanılama VM için uygun şekilde adımlarda değerlerini değiştirin.

## <a name="diagnose-using-azure-portal"></a>Azure portalını kullanarak tanılama

1. Azure günlüğüne [portal](https://portal.azure.com) sahip bir Azure hesabı ile [gerekli izinleri](virtual-network-network-interface.md#permissions).
2. Azure portalı üst kısmında, arama kutusuna çalışır durumda olan bir VM adını girin. VM adını arama sonuçlarında görüntülendiğinde, onu seçin.
3. Seçin **Tanıla ve sorunları**ve ardından **adımları önerilen**seçin **etkili yolları** 7'de, aşağıdaki resimde gösterildiği gibi öğesi:

    ![Görünüm etkili yolları](./media/diagnose-network-routing-problem/view-effective-routes.png)

4. Etkili yönlendirir adlı bir ağ arabirimi için **myVMVMNic** aşağıdaki resimde gösterilir:

     ![Görünüm etkili yolları](./media/diagnose-network-routing-problem/effective-routes.png)

    VM'ye bağlı birden çok ağ arabirimi varsa, herhangi bir ağ arabirimi için etkili rotaları seçerek görüntüleyebilirsiniz. Her bir ağ arabirimine farklı bir alt ağda olabileceği için her bir ağ arabirimine farklı etkili yollar olabilir.

    Önceki resimde gösterilen örnekte, her alt ağ için Azure oluşturduğu varsayılan yolların listelenen yolların edilir. Listenizin en az Bu yolları vardır, ancak gibi başka bir sanal ağ ile eşlenen veya bir Azure VPN ağ geçidi üzerinden şirket ağınıza bağlı sanal ağınız için etkin özelliklerine bağlı olarak ek yolları. Her yollar ve ağ arabirimi için karşılaşabileceğiniz diğer yollar hakkında daha fazla bilgi için bkz: [sanal ağ trafiği yönlendirmesini](virtual-networks-udr-overview.md). Çok sayıda yollar listeniz varsa, seçmek daha kolay **karşıdan**, yolların listesini içeren bir .csv dosyası indirilemedi.

Etkin yollar, önceki adımlarda VM üzerinden görüntülenebilir ancak yoluyla etkili yollar da görüntüleyebilirsiniz bir:
- **Bir ağ arabirimine**: öğrenin nasıl [bir ağ arabirimi görüntülemek](virtual-network-network-interface.md#view-network-interface-settings).
- **Tek tek yol tablosu**: öğrenin nasıl [bir yol tablosu görüntülemek](manage-route-table.md#view-details-of-a-route-table).

## <a name="diagnose-using-powershell"></a>PowerShell kullanarak tanılama

Takip edin komutları çalıştırmadan [Azure bulut Kabuk](https://shell.azure.com/powershell), veya bilgisayarınızdan PowerShell çalıştırarak. Azure bulut Kabuk ücretsiz etkileşimli kabuk ' dir. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. PowerShell bilgisayarınızdan çalıştırırsanız, gereksinim duyduğunuz *AzureRM* PowerShell modülü, sürüm 6.0.1 veya sonraki bir sürümü. Çalıştırma `Get-Module -ListAvailable AzureRM` bilgisayarınızda yüklü olan sürümü bulunamıyor. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell yerel olarak çalıştırıyorsanız, ayrıca çalıştırmanız gereken `Login-AzureRmAccount` Azure'da olan bir hesap ile oturum [gerekli izinleri](virtual-network-network-interface.md#permissions).

Bir ağ arabirimi için etkili rotaları alabilir [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/get-azurermeffectiveroutetable). Aşağıdaki örnek adlı ağ arabirimi için etkili rotaları alır *myVMVMNic*, yani bir kaynak grubunda adlı *myResourceGroup*:

```azurepowershell-interactive
Get-AzureRmEffectiveRouteTable `
  -NetworkInterfaceName myVMVMNic `
  -ResourceGroupName myResourceGroup `
  | Format-Table
```

Çıktıda dönen bilgiyi anlamak için bkz: [yönlendirmeye genel bakış](virtual-networks-udr-overview.md). VM çalışır durumda ise çıktı yalnızca döndürülür. VM'ye bağlı birden çok ağ arabirimi varsa, her ağ arabirimi için etkili rotaları gözden geçirebilirsiniz. Her bir ağ arabirimine farklı bir alt ağda olabileceği için her bir ağ arabirimine farklı etkili yollar olabilir. Bir iletişim sorunu yaşamaya devam ediyorsanız, bkz: [ek tanılama](#additional-diagnosis) ve [konuları](#considerations).

Bir ağ arabirimi adını bilmiyorsanız, ancak ağ arabiriminin bağlı olduğu VM adını biliyorsanız, aşağıdaki komutlar bir VM'ye bağlı tüm ağ arabirimleri kimliklerini döndürün:

```azurepowershell-interactive
$VM = Get-AzureRmVM -Name myVM `
  -ResourceGroupName myResourceGroup
$VM.NetworkProfile
```

Aşağıdaki örneğe benzer bir çıktı alırsınız:

```powershell
NetworkInterfaces
-----------------
{/subscriptions/<ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic
```

Önceki çıktısında, ağ arabirimi adıdır *myVMVMNic*.

## <a name="diagnose-using-azure-cli"></a>Azure CLI kullanarak tanılama

Takip edin komutları çalıştırmadan [Azure bulut Kabuk](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu makale Azure CLI Sürüm 2.0.32 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Azure CLI yerel olarak çalıştırıyorsanız, ayrıca çalıştırmanız gereken `az login` ve Azure günlüğüne sahip bir hesap ile [gerekli izinleri](virtual-network-network-interface.md#permissions).

Bir ağ arabirimi için etkili rotaları alabilir [az ağ NIC Göster-etkin-yol-tablosu](/cli/azure/network/nic#az-network-nic-show-effective-route-table). Aşağıdaki örnek adlı ağ arabirimi için etkili rotaları alır *myVMVMNic* adlı bir kaynak grubunda olan *myResourceGroup*:

```azurecli-interactive
az network nic show-effective-route-table \
  --name myVMVMNic \
  --resource-group myResourceGroup
```

Çıktıda dönen bilgiyi anlamak için bkz: [yönlendirmeye genel bakış](virtual-networks-udr-overview.md). VM çalışır durumda ise çıktı yalnızca döndürülür. VM'ye bağlı birden çok ağ arabirimi varsa, her ağ arabirimi için etkili rotaları gözden geçirebilirsiniz. Her bir ağ arabirimine farklı bir alt ağda olabileceği için her bir ağ arabirimine farklı etkili yollar olabilir. Bir iletişim sorunu yaşamaya devam ediyorsanız, bkz: [ek tanılama](#additional-diagnosis) ve [konuları](#considerations).

Bir ağ arabirimi adını bilmiyorsanız, ancak ağ arabiriminin bağlı olduğu VM adını biliyorsanız, aşağıdaki komutlar bir VM'ye bağlı tüm ağ arabirimleri kimliklerini döndürün:

```azurecli-interactive
az vm show \
  --name myVM \
  --resource-group myResourceGroup
```

## <a name="resolve-a-problem"></a>Bir sorunu giderme

Genellikle yönlendirme sorunlarını çözme oluşur:

- Azure'nın varsayılan yolların biri geçersiz kılmak için özel bir rota ekleniyor. Bilgi edinmek için nasıl [özel yol eklemek](manage-route-table.md#create-a-route).
- Değiştirme veya istenmeyen bir konuma yönlendirme neden olabilecek özel bir rota kaldırın. Bilgi nasıl [değiştirme](manage-route-table.md#change-a-route) veya [silmek](manage-route-table.md#delete-a-route) özel bir rota.
- Tüm özel rotaları içeren yol tablosu, tanımladığınız sağlayarak, ağ arabirimi bulunduğu alt ağ için ilişkilidir. Bilgi edinmek için nasıl [bir yol tablosu bir alt ağa ilişkilendirmek](manage-route-table.md#associate-a-route-table-to-a-subnet).
- Aygıtları dağıttıktan sonra Azure VPN ağ geçidi ya da ağ sanal Gereçleri gibi çalıştırılabilir sağlama. Kullanım [VPN tanılama](../network-watcher/diagnose-communication-problem-between-networks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bir Azure VPN ağ geçidi herhangi bir sorunu belirlemek için Ağ İzleyicisi yeteneğini.

İletişim sorunları yaşamaya devam ediyorsanız, bkz: [konuları](#considerations) ve [ek tanılama](#additional-dignosis).

## <a name="considerations"></a>Dikkat edilmesi gerekenler

İletişim sorunlarını giderirken aşağıdaki noktaları göz önünde bulundurun:

- Yönlendirme, en uzun ön ek eşleşmesi (LPM) yolları arasında tanımladığınız olduğunu, sınır ağ geçidi Protokolü (BGP) ve sistem yolları dayanır. Aynı LPM eşleşmesine sahip birden fazla yol yok sonra bir yol içinde listelenen sırayla ve kaynağına göre seçilir [yönlendirmeye genel bakış](virtual-networks-udr-overview.md#how-azure-selects-a-route). Etkin yollar ile kullanılabilir tüm yollara dayalı bir LPM eşleşmesine olan etkili yollar yalnızca görebilirsiniz. Yollar bir ağ arabirimi için nasıl değerlendirildiği görmesini çok VM gelen iletişimi etkileyen belirli yollar giderilir kolaylaştırır.
- İle bir ağ sanal gereç (NVA) özel yollar tanımladıysanız *sanal gereç* sonraki atlama türü IP iletimini trafiğini alma NVA üzerinde etkin veya paket bırakılır emin olun. Daha fazla bilgi edinmek [bir ağ arabirimi için IP iletimini etkinleştirme](virtual-network-network-interface.md#enable-or-disable-ip-forwarding). Ayrıca, işletim sistemi veya uygulama NVA içinde de ağ trafiğini iletmek ve bunu yapacak biçimde yapılandırılmış olması gerekir.
- 0.0.0.0/0 rotaya oluşturduysanız, tüm giden Internet trafiği, belirttiğiniz sonraki atlama bir NVA veya VPN ağ geçidi gibi yönlendirilir. Böyle bir yol oluşturma genellikle zorlanan tünel olarak adlandırılır. Sonraki atlama trafiğini nasıl işleme bağlı olarak bu yol, Internet'ten gelen RDP veya SSH protokolleri, VM ile uzak bağlantılar çalışmayabilir. Zorlamalı tünel etkinleştirilebilir:
    - Siteden siteye VPN, bir sonraki atlama türü ile bir yol oluşturarak kullanırken *VPN ağ geçidi*. Daha fazla bilgi edinmek [yapılandırma zorlamalı tünel](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - Bir 0.0.0.0/0 değilse (varsayılan yönlendirme) tanıtılan BGP bir sanal ağ geçidinden bir siteden siteye VPN kullanırken veya expressroute bağlantı hattı. BGP ile kullanma hakkında daha fazla bilgi edinin bir [siteden siteye VPN](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [ExpressRoute](../expressroute/expressroute-routing.md?toc=%2fazure%2fvirtual-network%2ftoc.json#ip-addresses-used-for-azure-private-peering).
- Sonraki atlama türü olan bir sistem yolu doğru şekilde çalışması sanal ağ eşleme trafiği *VNet eşlemesi* eşlenmiş sanal ağın önek aralığının mevcut olması gerekir. Böyle bir yol yok ve sanal ağ eşleme bağlantısı ise **bağlı**:
    - Birkaç saniye bekleyin ve yeniden deneyin. Yeni oluşturulan bir eşleme bağlantısı ise, bazen bir alt ağdaki tüm ağ arabirimleri yolları yaymak için daha uzun sürer. Sanal Ağ eşlemesi hakkında daha fazla bilgi için bkz: [sanal ağ eşleme genel bakış](virtual-network-peering-overview.md) ve [sanal ağ eşlemesi yönetmek](virtual-network-manage-peering.md).
    - Ağ güvenlik grubu kuralları iletişim etkiliyor olabilir. Daha fazla bilgi için bkz: [bir sanal makine ağ trafiği filtre sorunu tanılamak](diagnose-network-traffic-filter-problem.md).
- Azure VM'ye bağlı birden çok ağ arabirimi varsa, varsayılan yolların her Azure ağ arabirimine atar. ancak yalnızca birincil ağ arabirimi bir varsayılan yol (0.0.0.0/0) ya da ağ geçidi, sanal makinenin işletim sistemi içinde atanır. Bağlı ikincil ağ arabirimleri için bir varsayılan rota oluşturmayı öğrenin bir [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) veya [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) VM. Daha fazla bilgi edinmek [birincil ve ikincil ağ arabirimleri](virtual-network-network-interface-vm.md#constraints).

## <a name="additional-diagnosis"></a>Ek tanılama

* Bir konuma hedefleyen trafik için sonraki atlama türü belirlemek için hızlı bir testi çalıştırmak için kullandığınız [sonraki atlama](../network-watcher/diagnose-vm-network-routing-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Azure Ağ İzleyicisi yeteneği. Sonraki atlama ne belirtilen bir konuma hedefleyen trafik için sonraki atlama türü olduğunu söyler.
* Bir sanal makinenin Ağ iletişimi başarısız olmasına neden olan hiçbir yol varsa, sorunu VM'ın işletim sistemi içinde çalışan güvenlik duvarı yazılımı nedeniyle olabilir
* Kullanıyorsanız [zorlamalı tünel](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) trafiği bir VPN ağ geçidi ya da NVA yoluyla şirket içi cihaza, nasıl cihazlar için yönlendirme yapılandırdıktan bağlı olarak Internet'ten gelen bir VM'e bağlanmak mümkün olmayabilir. Aygıt için yapılandırdığınız yönlendirme trafiğini ya da bir genel veya özel IP adresi VM için yönlendirir olduğunu onaylayın.
* Kullanım [bağlantı sorunlarını giderme](../network-watcher/network-watcher-connectivity-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) giden iletişim sorunları yönlendirme, filtreleme ve işletim sistemi içinde nedenlerini belirlemek için Ağ İzleyicisi yeteneğini.

## <a name="next-steps"></a>Sonraki adımlar

- Tüm görevler, özellikleri ve ayarları hakkında bilgi edinin bir [rota tablosu ve yollar](manage-route-table.md).
- Tüm hakkında bilgi edinin [sonraki atlama türleri, sistem yolları ve Azure yol nasıl seçtiği](virtual-networks-udr-overview.md).
