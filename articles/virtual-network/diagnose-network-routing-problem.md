---
title: Bir Azure sanal makine yönlendirme sorunu tanılama | Microsoft Docs
description: Bir sanal makine için geçerli rotalar görüntüleyerek bir sanal makine yönlendirme sorunu Tanılama hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/30/2018
ms.author: kumud
ms.openlocfilehash: 465d44ea823c99afbb4f25541d64770c114ba7e2
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64730498"
---
# <a name="diagnose-a-virtual-machine-routing-problem"></a>Bir sanal makine yönlendirme sorunu tanılama

Bu makalede, bir sanal makinede (VM) bir ağ arabirimi için geçerli rotaları görüntüleyerek bir yönlendirme sorunu tanılama öğrenin. Azure, her sanal ağ alt ağı için birkaç varsayılan yollar oluşturur. İçinde bir yol tablosu yolları tanımlayıp sonra yönlendirme tablosunu bir alt ağ ilişkilendirme Azure'nın varsayılan yolları geçersiz kılabilirsiniz. Yollar, oluşturma, Azure'nın varsayılan yollarını ve bir Azure VPN gateway aracılığıyla şirket içi ağınızdan (sanal ağınızı şirket içi ağınıza bağlı değilse) sınır ağ geçidi Protokolü (BGP) yayılan tüm rotalar birleşimidir bir alt ağdaki tüm ağ arabirimleri için geçerli rotalar. Sanal ağ, ağ arabirimi veya kavramları yönlendirme ile ilgili bilgi sahibi değilseniz bkz [sanal ağa genel bakış](virtual-networks-overview.md), [ağ arabirimi](virtual-network-network-interface.md), ve [yönlendirmeye genel bakış](virtual-networks-udr-overview.md).

## <a name="scenario"></a>Senaryo

Bir VM'ye bağlanmayı dener, ancak bağlantı başarısız olur. Neden sanal Makineye bağlanamıyorsunuz belirlemek için Azure'ı kullanarak bir ağ arabirimi için geçerli rotalar görüntüleyebilirsiniz [portalı](#diagnose-using-azure-portal), [PowerShell](#diagnose-using-powershell), veya [Azure CLI](#diagnose-using-azure-cli).

Aşağıdaki adımları için geçerli yolları görüntülemek için mevcut bir VM'ye sahip olduğunuz varsayılır. Varolan bir VM'yi yoksa, öncelikle dağıtma bir [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ile bu makaledeki görevleri tamamlamak için VM. Adlı bir VM için bu makaledeki örnekleri olan *myVM* adlı bir ağ arabirimi ile *myVMVMNic*. VM ve ağ arabirimi olan adlı bir kaynak grubunda *myResourceGroup*ve *Doğu ABD* bölge. Adımları, VM için tanılamak için uygun değerleri değiştirin.

## <a name="diagnose-using-azure-portal"></a>Azure portalını kullanarak tanılama

1. Azure'da oturum [portalı](https://portal.azure.com) olan bir Azure hesabıyla [gerekli izinleri](virtual-network-network-interface.md#permissions).
2. Azure portalının üst kısmında, arama kutusuna çalışır durumda olan bir sanal makinenin adını girin. VM adı arama sonuçlarında görüntülendiğinde seçin.
3. Seçin **Tanıla ve problemleri çözmenize**ve ardından altındaki **önerilen adımlar**seçin **geçerli rotalar** 7'de, aşağıdaki resimde gösterildiği gibi öğesi:

    ![Geçerli yollar bölümünü inceleyin](./media/diagnose-network-routing-problem/view-effective-routes.png)

4. Geçerli yollar adlı bir ağ arabirimi için **myVMVMNic** , aşağıdaki resimde gösterilmektedir:

     ![Geçerli yollar bölümünü inceleyin](./media/diagnose-network-routing-problem/effective-routes.png)

    VM'ye birden çok ağ arabirimi varsa, herhangi bir ağ arabirimi için geçerli rotalar seçerek görüntüleyebilirsiniz. Her bir ağ arabirimine farklı bir alt ağ içinde olamayacağından, her bir ağ arabirimine farklı geçerli rotalar olabilir.

    Önceki resimde gösterilen örnekte listelenen Azure her alt ağ için oluşturduğu varsayılan rotaları yollardır. Listenize en az Bu yolları vardır, ancak özellikleri gibi başka bir sanal ağla eşlendikten veya bir Azure VPN gateway aracılığıyla şirket içi ağınıza bağlı sanal ağınız için etkin bağlı olarak ek yolları olabilir. Her biri yollar ve ağ Arabiriminizin karşılaşabileceğiniz diğer yollar hakkında daha fazla bilgi için bkz: [sanal ağ trafiği yönlendirme](virtual-networks-udr-overview.md). Çok sayıda yollar listeniz varsa, seçmek daha kolay olabilir **indirme**, yolların listesini içeren bir .csv dosyası indirilemedi.

Geçerli rotalar sanal makine, önceki adımlarda aracılığıyla görüntülenebilir ancak geçerli rotalar aracılığıyla da görüntüleyebilirsiniz bir:
- **Bir ağ arabirimine**: Bilgi edinmek için nasıl [bir ağ arabirimi görüntülemek](virtual-network-network-interface.md#view-network-interface-settings).
- **Tek bir yol tablosu**: Bilgi edinmek için nasıl [yol Tablosu'nu görüntüleyin](manage-route-table.md#view-details-of-a-route-table).

## <a name="diagnose-using-powershell"></a>PowerShell kullanarak tanılama

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

İçinde izleyen komutları çalıştırabilirsiniz [Azure Cloud Shell](https://shell.azure.com/powershell), veya PowerShell bilgisayarınızdan çalıştırarak. Azure Cloud Shell ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. PowerShell kullanarak bilgisayarınızdan çalıştırırsanız, Azure PowerShell modülü, sürüm 1.0.0 gerekir veya üzeri. Çalıştırma `Get-Module -ListAvailable Az` yüklü sürümü bulmak için bilgisayarınızda. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell'i yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `Connect-AzAccount` Azure'a olan bir hesapla oturum [gerekli izinleri](virtual-network-network-interface.md#permissions).

Bir ağ arabirimi için geçerli rotalar alma [Get-AzEffectiveRouteTable](/powershell/module/az.network/get-azeffectiveroutetable). Aşağıdaki örnekte adlı bir ağ arabirimi için geçerli rotalar alır *myVMVMNic*, yani bir kaynak grubunda *myResourceGroup*:

```azurepowershell-interactive
Get-AzEffectiveRouteTable `
  -NetworkInterfaceName myVMVMNic `
  -ResourceGroupName myResourceGroup `
  | Format-Table
```

Çıkışında döndürülen bilgileri anlamak için bkz: [yönlendirmeye genel bakış](virtual-networks-udr-overview.md). Çıktıyı yalnızca VM'nin çalışır durumda döndürülür. VM'ye birden çok ağ arabirimi varsa, her ağ arabirimi için geçerli rotalar gözden geçirebilirsiniz. Her bir ağ arabirimine farklı bir alt ağ içinde olamayacağından, her bir ağ arabirimine farklı geçerli rotalar olabilir. Bir iletişim sorununu yaşamaya devam ediyorsanız bkz [ek tanılama](#additional-diagnosis) ve [konuları](#considerations).

Bir ağ arabirimi adını bilmiyorsanız, ancak ağ arabiriminin bağlı olduğu VM adını bilmeniz, aşağıdaki komutları bir VM'ye bağlı tüm ağ arabirimleri kimliklerini döndürür:

```azurepowershell-interactive
$VM = Get-AzVM -Name myVM `
  -ResourceGroupName myResourceGroup
$VM.NetworkProfile
```

Aşağıdaki örneğe benzer bir çıktı alırsınız:

```powershell
NetworkInterfaces
-----------------
{/subscriptions/<ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic
```

Ağ arabirimi adı önceki çıktısında olan *myVMVMNic*.

## <a name="diagnose-using-azure-cli"></a>Azure CLI'yı kullanarak tanılama

İçinde izleyen komutları çalıştırabilirsiniz [Azure Cloud Shell](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu makale Azure CLI Sürüm 2.0.32 gerekir veya üzeri. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `az login` ve Azure'da oturum olan bir hesapla [gerekli izinleri](virtual-network-network-interface.md#permissions).

Bir ağ arabirimi için geçerli rotalar alma [az network nic show-etkin-yönlendirme-tablosunu](/cli/azure/network/nic#az-network-nic-show-effective-route-table). Aşağıdaki örnekte adlı bir ağ arabirimi için geçerli rotalar alır *myVMVMNic* adlı bir kaynak grubunda olan *myResourceGroup*:

```azurecli-interactive
az network nic show-effective-route-table \
  --name myVMVMNic \
  --resource-group myResourceGroup
```

Çıkışında döndürülen bilgileri anlamak için bkz: [yönlendirmeye genel bakış](virtual-networks-udr-overview.md). Çıktıyı yalnızca VM'nin çalışır durumda döndürülür. VM'ye birden çok ağ arabirimi varsa, her ağ arabirimi için geçerli rotalar gözden geçirebilirsiniz. Her bir ağ arabirimine farklı bir alt ağ içinde olamayacağından, her bir ağ arabirimine farklı geçerli rotalar olabilir. Bir iletişim sorununu yaşamaya devam ediyorsanız bkz [ek tanılama](#additional-diagnosis) ve [konuları](#considerations).

Bir ağ arabirimi adını bilmiyorsanız, ancak ağ arabiriminin bağlı olduğu VM adını bilmeniz, aşağıdaki komutları bir VM'ye bağlı tüm ağ arabirimleri kimliklerini döndürür:

```azurecli-interactive
az vm show \
  --name myVM \
  --resource-group myResourceGroup
```

## <a name="resolve-a-problem"></a>Bir sorunu giderme

Genellikle yönlendirme sorunlarını çözme şunlardan oluşur:

- Bir Azure'nın varsayılan yollarını geçersiz kılmak için özel bir rota ekleniyor. Bilgi edinmek için nasıl [özel bir yol eklemek](manage-route-table.md#create-a-route).
- Değiştirin veya istenmeyen bir konuma yeniden yönlendirme neden olabilecek bir özel yol kaldırın. Bilgi nasıl [değiştirme](manage-route-table.md#change-a-route) veya [Sil](manage-route-table.md#delete-a-route) özel bir yol.
- Herhangi bir özel yolları içeren bir yol tablosu, tanımladınız sağlama, ağ arabiriminin içinde bulunduğu alt ağ için ilişkilidir. Bilgi edinmek için nasıl [bir alt ağ yönlendirme tablosunu ilişkilendirme](manage-route-table.md#associate-a-route-table-to-a-subnet).
- Cihazlar, dağıtılan Azure VPN ağ geçidi veya ağ sanal Gereçleri gibi çalıştırılabilir olduğundan emin olmak. Kullanım [VPN tanılamalarını](../network-watcher/diagnose-communication-problem-between-networks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) herhangi bir Azure VPN gateway sorunları belirlemek için Ağ İzleyicisi özelliğidir.

İletişim sorunları yaşamaya devam ediyorsanız bkz [konuları](#considerations) ve ek tanılama.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

İletişim sorunlarını giderirken aşağıdaki noktaları göz önünde bulundurun:

- Yönlendirme yolları arasında en uzun ön ek eşleştirmesi (LPM) tanımladığınız olduğunu, sınır ağ geçidi Protokolü (BGP) ve sistem yollarını temel alır. Aynı LPM sahip birden fazla yol yok sonra yol listelenen sırayla ve kaynağına göre seçilir [yönlendirmeye genel bakış](virtual-networks-udr-overview.md#how-azure-selects-a-route). Geçerli rotalarla kullanılabilir tüm yollara göre bir LPM eşleşme olan geçerli rotalar yalnızca görebilirsiniz. Yollar bir ağ arabirimi için nasıl değerlendirilir görmeye çok sanal makinenize gelen iletişimi etkileyen belirli yol sorunlarını giderme kolaylaştırır.
- İle bir ağ sanal Gereci (NVA) özel yollar tanımladıysanız *sanal gereç* sonraki atlama türü IP iletme trafiğini gönderip almak NVA etkin veya paketler bırakılır emin olun. Daha fazla bilgi edinin [bir ağ arabirimi için IP iletimini etkinleştirme](virtual-network-network-interface.md#enable-or-disable-ip-forwarding). Ayrıca, işletim sistemi veya uygulama NVA içinde de ağ trafiğini iletebilmelidir ve bunu yapmak için yapılandırılmış olması gerekir.
- Bir yolu 0.0.0.0/0 oluşturduysanız, tüm giden internet trafiğini belirlediğiniz sonraki atlama gibi bir NVA veya VPN ağ geçidi yönlendirilir. Böyle bir yol oluşturmak genellikle zorlamalı tünel olarak adlandırılır. Sonraki atlama trafiğini nasıl işleme bağlı olarak bu rota, İnternet'ten gelen RDP veya SSH protokolleri, VM ile uzak bağlantılar çalışmayabilir. Zorlamalı tünel etkinleştirilebilir:
    - Siteden siteye VPN, bir sonraki atlama türü ile bir yol oluşturarak kullanırken *VPN ağ geçidi*. Daha fazla bilgi edinin [yapılandırma zorlamalı tünel](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - Bir 0.0.0.0/0 değilse (varsayılan yönlendirme) tanıtılan BGP bir sanal ağ geçidinden bir siteden siteye VPN kullanarak veya ExpressRoute bağlantı hattı. İle BGP'yi kullanma hakkında daha fazla bilgi bir [siteden siteye VPN](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [ExpressRoute](../expressroute/expressroute-routing.md?toc=%2fazure%2fvirtual-network%2ftoc.json#ip-addresses-used-for-azure-private-peering).
- Bir sonraki atlama türü ile bir sistem yolu doğru şekilde çalışması sanal ağ eşleme trafiği *VNet eşlemesi* eşlenmiş sanal ağın ön ek aralığını için mevcut olması gerekir. Böyle bir yol yok ve sanal ağ eşleme bağlantısı varsa **bağlı**:
    - Birkaç saniye bekleyin ve yeniden deneyin. Yeni kurulan bir eşleme bağlantısı varsa, bazen bir alt ağdaki tüm ağ arabirimleri için rotalar yayılması uzun sürer. Sanal Ağ eşlemesi hakkında daha fazla bilgi için bkz: [sanal ağ eşleme genel bakış](virtual-network-peering-overview.md) ve [sanal ağ eşlemesi yönetme](virtual-network-manage-peering.md).
    - Ağ güvenlik grubu kuralları iletişim etkiliyor olabilir. Daha fazla bilgi için [bir sanal makine ağ trafik filtresi sorununu tanılama](diagnose-network-traffic-filter-problem.md).
- Azure VM'ye birden çok ağ arabirimi varsa, her Azure ağ arabirimi için varsayılan yollar atar ancak yalnızca birincil ağ arabirimi bir varsayılan yolun (0.0.0.0/0) ya da ağ geçidi, sanal makinenin işletim sistemi içinde atanır. Bağlı olan ikincil ağ arabirimleri için bir varsayılan rota oluşturmayı öğrenmenin bir [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) veya [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) VM. Daha fazla bilgi edinin [birincil ve ikincil ağ arabirimleri](virtual-network-network-interface-vm.md#constraints).

## <a name="additional-diagnosis"></a>Ek tanılama

* Bir konuma giden trafik için sonraki atlama türü belirlemek için hızlı bir testi çalıştırmak için kullandığınız [sonraki atlama](../network-watcher/diagnose-vm-network-routing-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Azure Ağ İzleyicisi özelliğidir. Sonraki atlama, belirtilen bir konuma giden trafik için sonraki atlama türü nedir bildirir.
* Bir sanal makinenin Ağ iletişimi başarısız olmasına neden olan hiçbir yol varsa, sorun nedeniyle güvenlik duvarı yazılımı çalıştıran sanal makinenin işletim sistemi içinde olabilir
* Eğer [zorlamalı tünel](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) trafiği bir şirket içi cihaza bir VPN ağ geçidi veya NVA aracılığıyla, bir VM'ye yönlendirme cihazlar için nasıl yapılandırdığınıza bağlı olarak internet'ten bağlanmak mümkün olmayabilir. Cihaz için yapılandırdığınız üretim trafiği ya da genel veya özel bir IP adresi sanal makine için yönlendirir olduğunu onaylayın.
* Kullanım [bağlantı sorunlarını giderme](../network-watcher/network-watcher-connectivity-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) giden iletişim sorunlarını yönlendirme, filtreleme ve işletim sistemi içinde nedenlerini belirlemek için Ağ İzleyicisi özelliğidir.

## <a name="next-steps"></a>Sonraki adımlar

- Tüm görevler, özellikleri ve ayarları hakkında bir [rota tablosu ve yol](manage-route-table.md).
- Hakkında bilgi edinin [sonraki atlama türleri, sistem yolları ve Azure'nın bir yolu nasıl seçer](virtual-networks-udr-overview.md).
