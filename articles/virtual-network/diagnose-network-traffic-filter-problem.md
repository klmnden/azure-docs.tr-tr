---
title: Bir sanal makine ağ trafik filtresi sorununu tanılama | Microsoft Docs
description: Bir sanal makine için geçerli güvenlik kuralları görüntüleyerek bir sanal makine ağ trafik filtresi sorununu tanılama hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: a54feccf-0123-4e49-a743-eb8d0bdd1ebc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2018
ms.author: kumud
ms.openlocfilehash: f84e8a24e8f28cdccc987afbd1449cb17422ce0c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64712677"
---
# <a name="diagnose-a-virtual-machine-network-traffic-filter-problem"></a>Bir sanal makine ağ trafik filtresi sorununu tanılama

Bu makalede, bir sanal makine (VM) için geçerli olan ağ güvenlik grubu (NSG) güvenlik kuralları görüntüleyerek bir ağ trafik filtresi sorununu tanılama öğrenin.

Nsg'ler trafik türlerini denetlemek için bir VM içine ve dışına akan olanak sağlar. Bir NSG bir alt ağdan bir Azure sanal ağı, VM veya her ikisi için bağlı bir ağ arabirimi ile ilişkilendirebilirsiniz. Mevcut NSG'de kurallar bir toplama bir ağ arabirimi ile ilişkilendirilmiş bir ağ arabirimi için uygulanan geçerli güvenlik kuralları olan ve alt ağ arabiriminin bulunduğu. Farklı Nsg'leri kurallarında bazen birbiriyle çelişen ve bir sanal makinenin ağ bağlantısını etkiler. Sanal makinenin ağ arabirimleri üzerinde uygulanan Nsg tüm geçerli güvenlik kuralları görüntüleyebilirsiniz. Sanal ağ, ağ arabirimi veya NSG kavramlar ile ilgili bilgi sahibi değilseniz bkz [sanal ağa genel bakış](virtual-networks-overview.md), [ağ arabirimi](virtual-network-network-interface.md), ve [ağ güvenlik gruplarına genel bakış](security-overview.md).

## <a name="scenario"></a>Senaryo

Bir VM'ye bağlantı noktası 80 üzerinden internet'ten bağlanma girişimi, ancak bağlantı başarısız olur. Internet'ten 80 numaralı bağlantı noktasını neden erişemiyor belirlemek için Azure'ı kullanarak bir ağ arabirimi için geçerli güvenlik kurallarını görüntüleyebilirsiniz [portalı](#diagnose-using-azure-portal), [PowerShell](#diagnose-using-powershell), veya [AzureCLI](#diagnose-using-azure-cli).

Aşağıdaki adımları için geçerli güvenlik kurallarını görüntülemek için mevcut bir VM'ye sahip olduğunuz varsayılır. Varolan bir VM'yi yoksa, öncelikle dağıtma bir [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ile bu makaledeki görevleri tamamlamak için VM. Adlı bir VM için bu makaledeki örnekleri olan *myVM* adlı bir ağ arabirimi ile *myVMVMNic*. VM ve ağ arabirimi olan adlı bir kaynak grubunda *myResourceGroup*ve *Doğu ABD* bölge. Adımları, VM için tanılamak için uygun değerleri değiştirin.

## <a name="diagnose-using-azure-portal"></a>Azure portalını kullanarak tanılama

1. Azure'da oturum [portalı](https://portal.azure.com) olan bir Azure hesabıyla [gerekli izinleri](virtual-network-network-interface.md#permissions).
2. Azure portalının üst kısmında, VM'nin adı arama kutusuna girin. VM adı arama sonuçlarında görüntülendiğinde seçin.
3. Altında **ayarları**seçin **ağ**, aşağıdaki resimde gösterildiği gibi:

   ![Güvenlik kurallarını görüntüle](./media/diagnose-network-traffic-filter-problem/view-security-rules.png)

   Adlı bir ağ arabirimi için listelenen önceki resimde gördüğünüz kurallar şunlardır **myVMVMNic**. Olduğunu gördüğünüz **gelen bağlantı noktası kuralları** için iki farklı ağ güvenlik grupları ağ arabiriminden:
   
   - **mySubnetNSG**: İlişkili ağ arabiriminin bulunduğu alt ağ için.
   - **myVMNSG**: Ağ arabirimi adlı sanal makine ile ilişkili **myVMVMNic**.

   Adlı kural **DenyAllInBound** açıklandığı olduğunu, hangi gelen iletişim istekleri için VM bağlantı noktası 80 üzerinden internet'ten engelliyor [senaryo](#scenario). Kural listeleri *0.0.0.0/0* için **kaynak**, internet içerir. 80 numaralı bağlantı noktasını daha yüksek bir önceliğe (düşük sayı) sahip başka hiçbir kural sağlayan gelen. 80 numaralı bağlantı noktasını izin vermek için internet'ten sanal Makineye gelen bkz [bir sorunu](#resolve-a-problem). Güvenlik kuralları ve bunları nasıl Azure uygular hakkında daha fazla bilgi için bkz: [ağ güvenlik grupları](security-overview.md).

   Resmin en altında Ayrıca bkz: **giden bağlantı noktası kuralları**. Ağ arabirimi için giden bağlantı noktası kuralları altında olan. Nsg'lerinizi, resim her NSG için yalnızca dört gelen kuralları gösterir ancak birçok dörtten fazla kurala sahip olabilir. Resimde gördüğünüz **VirtualNetwork** altında **kaynak** ve **hedef** ve **AzureLoadBalancer** altında  **Kaynak**. **VirtualNetwork** ve **AzureLoadBalancer** olan [hizmet etiketleri](security-overview.md#service-tags). Hizmet etiketlerini güvenlik kuralı oluşturma sırasındaki karmaşıklığı en aza indirmek için IP adresi ön eki grubunu temsil eder.

4. VM, durum ve ardından çalışır olduğundan emin olun **geçerli güvenlik kuralları**, aşağıdaki resimde gösterilen geçerli güvenlik kuralları görmek için önceki resimde gösterildiği gibi:

   ![Geçerli güvenlik kurallarını görüntüle](./media/diagnose-network-traffic-filter-problem/view-effective-security-rules.png)

   İlişkili ağ arabirimi ve alt ağ için NSG için farklı sekmeler olsa listelenen gördüğünüz aynı adım 3, kurallardır. Resimde görebileceğiniz gibi yalnızca ilk 50 kuralları gösterilmektedir. Tüm kurallar içeren bir .csv dosyasını indirmek için seçin **indirme**.

   Her hizmet etiketi, ön ekleri görmek için temsil eder, adlı kural gibi bir kural seçin **AllowAzureLoadBalancerInbound**. Ön ekleri için aşağıdaki resimde gösterilmiştir **AzureLoadBalancer** hizmet etiketi:

   ![Geçerli güvenlik kurallarını görüntüle](./media/diagnose-network-traffic-filter-problem/address-prefixes.png)

   Ancak **AzureLoadBalancer** hizmet etiketi yalnızca bir önek temsil eder, diğer hizmet etiketleri birkaç önekleri temsil eder.

5. Önceki adımları gösterilen adlı bir ağ arabirimi için güvenlik kuralları **myVMVMNic**, ancak bu adlı bir ağ arabirimi de gördüğünüz **myVMVMNic2** bazı önceki resimleri. Bu örnekte sanal Makineye bağlı iki ağ arabirimi var. Geçerli güvenlik kuralları her ağ arabirimi için farklı olabilir.

   Kuralları görmek için **myVMVMNic2** ağ arabirimi, onu seçin. Aşağıdaki resimde gösterildiği gibi ağ arabirimi kendi alt ağ ile ilişkili aynı kurallara sahip **myVMVMNic** her iki ağ arabirimi aynı alt ağda olduğundan ağ arabirimi. Bir NSG'yi bir alt ağ ile ilişkilendirdiğinizde, kendi kuralları alt ağdaki tüm ağ arabirimlerine uygulanır.

   ![Güvenlik kurallarını görüntüle](./media/diagnose-network-traffic-filter-problem/view-security-rules2.png)

   Farklı **myVMVMNic** ağ arabirimi, **myVMVMNic2** ağ arabirimi ile ilişkili ağ güvenlik grubu yok. Bir NSG ile ilişkili veya her bir ağ arabirimi ve alt ağ sıfır olabilir. NSG her ağ arabirimi ile ilişkilendirilmiş veya alt ağ, aynı olabilir veya farklı. İstediğiniz sayıda ağ arabirimine ve alt için aynı ağ güvenlik grubunu ilişkilendirebilirsiniz.

Geçerli güvenlik kuralları bir VM aracılığıyla görüntülenebilir ancak geçerli güvenlik kuralları bir kişi aracılığıyla da görüntüleyebilirsiniz:
- **Ağ arabirimi**: Bilgi edinmek için nasıl [bir ağ arabirimi görüntülemek](virtual-network-network-interface.md#view-network-interface-settings).
- **NSG**: Bilgi edinmek için nasıl [bir NSG görüntülemek](manage-network-security-group.md#view-details-of-a-network-security-group).

## <a name="diagnose-using-powershell"></a>PowerShell kullanarak tanılama

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

İçinde izleyen komutları çalıştırabilirsiniz [Azure Cloud Shell](https://shell.azure.com/powershell), veya PowerShell bilgisayarınızdan çalıştırarak. Azure Cloud Shell ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. PowerShell kullanarak bilgisayarınızdan çalıştırırsanız, Azure PowerShell modülü, sürüm 1.0.0 gerekir veya üzeri. Çalıştırma `Get-Module -ListAvailable Az` yüklü sürümü bulmak için bilgisayarınızda. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `Connect-AzAccount` Azure'a olan bir hesapla oturum [gerekli izinleri](virtual-network-network-interface.md#permissions)].

Bir ağ arabirimi için geçerli güvenlik kuralları alma [Get-AzEffectiveNetworkSecurityGroup](/powershell/module/az.network/get-azeffectivenetworksecuritygroup). Aşağıdaki örnekte adlı bir ağ arabirimi için geçerli güvenlik kuralları alır *myVMVMNic*, yani bir kaynak grubunda *myResourceGroup*:

```azurepowershell-interactive
Get-AzEffectiveNetworkSecurityGroup `
  -NetworkInterfaceName myVMVMNic `
  -ResourceGroupName myResourceGroup
```

Çıkış json biçiminde döndürülür. Çıkış anlamak için bkz: [komut çıktısı yorumlama](#interpret-command-output).
Bir NSG ağ arabirimi, ağ arabiriminin içinde bulunduğu alt ağa veya her ikisi ile ilişkili ise çıktı yalnızca döndürülür. VM'nin çalışır durumda olması gerekir. Bir VM ile farklı Nsg'ler uygulanan birden çok ağ arabirimine sahip olabilir. Sorunlarını giderirken, her ağ arabirimi için komutu çalıştırın.

Hala bir bağlantı sorunu yaşıyorsanız bkz [ek tanılama](#additional-diagnosis) ve [konuları](#considerations).

Bir ağ arabirimi adını bilmiyorsanız, ancak ağ arabiriminin bağlı olduğu VM adını bilmeniz, aşağıdaki komutları bir VM'ye bağlı tüm ağ arabirimleri kimliklerini döndürür:

```azurepowershell-interactive
$VM = Get-AzVM -Name myVM -ResourceGroupName myResourceGroup
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

Bu makaledeki görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure Cloud Shell](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu makale Azure CLI Sürüm 2.0.32 gerekir veya üzeri. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `az login` ve Azure'da oturum olan bir hesapla [gerekli izinleri](virtual-network-network-interface.md#permissions).

Bir ağ arabirimi için geçerli güvenlik kuralları alma [az NIC liste-etkin-nsg'yi ağ](/cli/azure/network/nic#az-network-nic-list-effective-nsg). Aşağıdaki örnekte adlı bir ağ arabirimi için geçerli güvenlik kuralları alır *myVMVMNic* adlı bir kaynak grubunda olan *myResourceGroup*:

```azurecli-interactive
az network nic list-effective-nsg \
  --name myVMVMNic \
  --resource-group myResourceGroup
```

Çıkış json biçiminde döndürülür. Çıkış anlamak için bkz: [komut çıktısı yorumlama](#interpret-command-output).
Bir NSG ağ arabirimi, ağ arabiriminin içinde bulunduğu alt ağa veya her ikisi ile ilişkili ise çıktı yalnızca döndürülür. VM'nin çalışır durumda olması gerekir. Bir VM ile farklı Nsg'ler uygulanan birden çok ağ arabirimine sahip olabilir. Sorunlarını giderirken, her ağ arabirimi için komutu çalıştırın.

Hala bir bağlantı sorunu yaşıyorsanız bkz [ek tanılama](#additional-diagnosis) ve [konuları](#considerations).

Bir ağ arabirimi adını bilmiyorsanız, ancak ağ arabiriminin bağlı olduğu VM adını bilmeniz, aşağıdaki komutları bir VM'ye bağlı tüm ağ arabirimleri kimliklerini döndürür:

```azurecli-interactive
az vm show \
  --name myVM \
  --resource-group myResourceGroup
```

Döndürülen çıktısı, aşağıdaki örneğe benzer bilgiler görürsünüz:

```azurecli
"networkProfile": {
    "additionalProperties": {},
    "networkInterfaces": [
      {
        "additionalProperties": {},
        "id": "/subscriptions/<ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic",
        "primary": true,
        "resourceGroup": "myResourceGroup"
      },
```

Ağ arabirimi adı önceki çıktısında olan *myVMVMNic arabirimi*.

## <a name="interpret-command-output"></a>Komut çıktısı yorumlama

Kullanıp [PowerShell](#diagnose-using-powershell), veya [Azure CLI](#diagnose-using-azure-cli) sorunu tanılamak için aşağıdaki bilgileri içeren bir çıktı alırsınız:

- **NetworkSecurityGroup**: Ağ güvenlik grubu kimliği.
- **İlişkilendirme**: Ağ güvenlik grubu için ilişkili olup olmadığını bir *Networkınterface* veya *alt*. Bir NSG hem de ilişkiliyse ile çıktı döndürülür **NetworkSecurityGroup**, **ilişkilendirme**, ve **EffectiveSecurityRules**, her NSG için. NSG'yi ilişkili ya da hemen geçerli güvenlik kuralları görmek için komutu çalıştırmadan önce ilişkilendirmesi komut çıktısında yansıtacak şekilde değiştirmek için birkaç saniye bekleyin gerekebilir.
- **EffectiveSecurityRules**: Bir açıklamanın yanı sıra her bir özellik içinde ayrıntılı [bir güvenlik kuralı oluşturun](manage-network-security-group.md#create-a-security-rule). Kural ile başlayan adları *defaultSecurityRules /* her NSG'de mevcut güvenlik kuralları varsayılan olduğu. Kural ile başlayan adları *securityRules /* oluşturduğunuz kurallardır. Belirten kuralları bir [hizmet etiketi](security-overview.md#service-tags), gibi **Internet**, **VirtualNetwork**, ve **AzureLoadBalancer** için  **destinationAddressPrefix** veya **sourceAddressPrefix** özellikleri için değerleri de **expandedDestinationAddressPrefix** özelliği. **ExpandedDestinationAddressPrefix** özelliği tüm adres ön ekleri hizmet etiketi tarafından temsil edilen listeler.

Çıktıda listelenen yinelenen kurallara görürseniz, hem ağ arabirimi hem de alt ağa bir NSG ilişkilendirilen olmasıdır. Hem Nsg'ler aynı varsayılan kurallara sahiptir ve her iki Nsg'ler aynı olan kendi kuralları oluşturduysanız ek yinelenen kurallara sahip olabilir.

Adlı kural **defaultSecurityRules/DenyAllInBound** açıklandığı olduğunu, hangi gelen iletişim istekleri için VM bağlantı noktası 80 üzerinden internet'ten engelliyor [senaryo](#scenario). Daha yüksek bir önceliğe (düşük sayı) sahip başka hiçbir kural sağlar bağlantı noktası 80, internet'ten gelen.

## <a name="resolve-a-problem"></a>Bir sorunu giderme

Azure kullanıp [portalı](#diagnose-using-azure-portal), [PowerShell](#diagnose-using-powershell), veya [Azure CLI](#diagnose-using-azure-cli) içinde sunulan sorunu tanılamak için [senaryo](#scenario) bu makalede, aşağıdaki özelliklere sahip bir ağ güvenlik kuralı oluşturmak için bir çözümdür:

| Özellik                | Değer                                                                              |
|---------                |---------                                                                           |
| Kaynak                  | Herhangi biri                                                                                |
| Kaynak bağlantı noktası aralıkları      | Herhangi biri                                                                                |
| Hedef             | VM'nin IP adresi, bir IP adresi aralığı veya alt ağdaki tüm adresleri. |
| Hedef bağlantı noktası aralıkları | 80                                                                                 |
| Protokol                | TCP                                                                                |
| Eylem                  | İzin Ver                                                                              |
| Öncelik                | 100                                                                                |
| Ad                    | İzin ver-HTTP-All                                                                     |

80 numaralı bağlantı noktası kuralı oluşturduktan sonra izin verilen kuralının önceliğini adlı varsayılan güvenlik kuralından yüksek olduğu için internet'ten gelen *DenyAllInBound*, trafiği engeller. Bilgi edinmek için nasıl [bir güvenlik kuralı oluşturun](manage-network-security-group.md#create-a-security-rule). Farklı Nsg'leri hem ağ arabirimi ve alt ağ, hem Nsg içinde aynı kuralı oluşturmanız gerekir.

Azure işlemleri gelen trafik, (varsa ilişkili bir NSG'si) alt ağ ile ilişkilendirilmiş NSG kurallarında işler ve ardından ağ arabirimi ile ilişkilendirilmiş NSG kuralları işler. Ağ arabirimi ve alt ağ ile ilişkilendirilen bir NSG varsa, bağlantı noktasını VM ulaşmak trafiği için hem Nsg içinde açık olması gerekir. Yönetim ve iletişim sorunları kolaylaştırmak için bir NSG'yi ağ arabirimine yerine bir alt ağı ilişkilendir öneririz. Farklı güvenlik kuralları bir alt ağ içindeki VM'ler ihtiyacınız varsa, ağ arabirimlerini bir uygulama güvenlik grubu (ASG) üyesi olun ve bir ASG kaynak ve hedef bir güvenlik kuralının olarak belirtin. Daha fazla bilgi edinin [uygulama güvenlik grupları](security-overview.md#application-security-groups).

İletişim sorunları yaşamaya devam ediyorsanız bkz [konuları](#considerations) ve ek tanılama.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bağlantı sorunlarını giderirken aşağıdaki noktaları göz önünde bulundurun:

* Varsayılan güvenlik kuralları, internet'ten gelen erişimi engellemek ve yalnızca sanal ağdan gelen trafiğe izin verir. Internet'ten gelen trafiğe izin vermek için varsayılan kurallar daha yüksek bir önceliğe sahip güvenlik kuralları ekleyin. Daha fazla bilgi edinin [varsayılan güvenlik kuralları](security-overview.md#default-security-rules), veya nasıl [güvenlik kuralı eklemeye](manage-network-security-group.md#create-a-security-rule).
* Varsayılan olarak, sanal ağlar eşlenmiş, **vırtual_network** hizmet etiketi için eşlenmiş sanal ağlardaki ön ekleri eklemek için otomatik olarak genişletir. Sanal Ağ eşlemesi için ilgili sorunları gidermek için ön ekleri görüntüleyebilirsiniz **ExpandedAddressPrefix** listesi. Daha fazla bilgi edinin [sanal ağ eşlemesi](virtual-network-peering-overview.md) ve [hizmet etiketleri](security-overview.md#service-tags).
* Geçerli güvenlik kuralları yalnızca bir ağ arabirimi bir NSG varsa sanal makinenin ağ arabirimiyle ilişkilendirilmiş için gösterilen ve, veya, alt ağ ve VM çalışır durumda.
* Ağ arabirimiyle ya da alt ağ ile ilişkili hiçbir Nsg varsa ve sahip olduğunuz bir [genel IP adresi](virtual-network-public-ip-address.md) bir VM'ye atanmış, tüm bağlantı noktaları'ten gelen erişim ve her yerden giden erişim için açık. VM'nin genel IP adresi varsa, alt ağa bir NSG uygulayarak ağ arabirimini öneririz.

## <a name="additional-diagnosis"></a>Ek tanılama

* İçin veya bir sanal Makinede gelen trafiğe izin verilip verilmediğini belirlemek için hızlı bir testi çalıştırmak için kullandığınız [IP akışı doğrulama](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md) Azure Ağ İzleyicisi özelliğidir. IP akışı doğrulama trafiklere izin verildiğini veya olmadığını bildirir. Engellendi, IP akışı doğrulama söyler, hangi güvenlik kuralı trafiği reddediyor.
* Bir sanal makinenin ağ bağlantısı başarısız olmasına neden olan hiçbir güvenlik kuralları varsa, sorunu nedeniyle olabilir:
  * Güvenlik duvarı yazılımı sanal makinenin işletim sistemi içinde çalıştırma
  * Yollar, sanal Gereçleri veya şirket içi trafiği için yapılandırılmış. Internet trafiğini şirket içi ağınız yönlendirilebilir [zorlamalı tünel](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Tünel internet trafiğini bir sanal gereç veya şirket içi zorlarsanız, internet'ten sanal Makineye bağlanmak mümkün olmayabilir. VM trafik akışını engelleyebilecek rota sorunları tanılamak öğrenmek için bkz: [bir sanal makine ağ trafiğini yönlendirme sorunu tanılama](diagnose-network-routing-problem.md).

## <a name="next-steps"></a>Sonraki adımlar

- Tüm görevler, özellikleri ve ayarları hakkında bir [ağ güvenlik grubu](manage-network-security-group.md#work-with-network-security-groups) ve [güvenlik kuralları](manage-network-security-group.md#work-with-security-rules).
- Hakkında bilgi edinin [varsayılan güvenlik kuralları](security-overview.md#default-security-rules), [hizmet etiketleri](security-overview.md#service-tags), ve [Azure gelen ve giden trafik için güvenlik kuralları nasıl işlediği](security-overview.md#network-security-groups) bir VM için.
