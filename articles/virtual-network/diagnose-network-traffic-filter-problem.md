---
title: Bir sanal makine ağ trafiği filtre sorunu tanılamak | Microsoft Docs
description: Bir sanal makine için etkili güvenlik kuralları görüntüleyerek bir sanal makine ağ trafiği filtre sorunu tanılamak öğrenin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: a54feccf-0123-4e49-a743-eb8d0bdd1ebc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2018
ms.author: jdial
ms.openlocfilehash: 1c33a75363eec2b4e338ba64e3d1ad877d8b1610
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34757236"
---
# <a name="diagnose-a-virtual-machine-network-traffic-filter-problem"></a>Bir sanal makine ağ trafiği filtre sorunu tanılamak

Bu makalede, bir sanal makine (VM) için etkin olan ağ güvenlik grubu (NSG) güvenlik kuralları görüntüleyerek bir ağ trafiği filtre sorunu tanılamak öğrenin.

Nsg'ler, trafik türlerini denetlemek için bir VM ve akan etkinleştirin. Bir NSG'yi bir alt ağ içinde bir Azure sanal ağı, VM veya her ikisi için bağlı bir ağ arabirimi ilişkilendirebilirsiniz. Bir ağ arabirimine uygulanan etkin güvenlik kuralları bir toplama NSG'de bulunan kuralların bir ağ arabirimi ile ilişkili ve alt ağ ağ arabiriminin içinde. Farklı Nsg'leri kurallarında bazen birbiriyle çelişen ve bir sanal makinenin ağ bağlantısını etkileyebilir. VM ağ arabirimlerindeki uygulanan Nsg'ler gelen tüm etkin güvenlik kuralları görüntüleyebilirsiniz. Sanal ağ, ağ arabirimi veya NSG kavramları bilmiyorsanız, bkz: [sanal ağa genel bakış](virtual-networks-overview.md), [ağ arabirimi](virtual-network-network-interface.md), ve [ağ güvenlik gruplarını genel bakış](security-overview.md).

## <a name="scenario"></a>Senaryo

Bir VM için bağlantı noktası 80 üzerinden internet'ten bağlanmaya çalışır, ancak bağlantı başarısız olur. Internet'ten bağlantı noktası 80 neden erişemiyor belirlemek için Azure kullanarak bir ağ arabirimi için etkili güvenlik kuralları görüntüleyebilirsiniz [portal](#diagnose-using-azure-portal), [PowerShell](#diagnose-using-powershell), veya [Azure CLI](#diagnose-using-azure-cli).

Adımları için etkili güvenlik kuralları görüntülemek için mevcut bir VM'yi olduğunu varsayın. Mevcut bir VM'yi yoksa, öncelikle dağıtmak bir [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bu makalede görevleri tamamlamak için VM. Bu makaledeki örneklerde adlı bir VM için olan *myVM* sahip bir ağ arabirimi adlı *myVMVMNic*. VM ve ağ arabirimi olan adlı bir kaynak grubunda *myResourceGroup*ve *Doğu ABD* bölge. Sorun için tanılama VM için uygun şekilde adımlarda değerlerini değiştirin.

## <a name="diagnose-using-azure-portal"></a>Azure portalını kullanarak tanılama

1. Azure günlüğüne [portal](https://portal.azure.com) sahip bir Azure hesabı ile [gerekli izinleri](virtual-network-network-interface.md#permissions).
2. Azure portalı üst kısmında, arama kutusuna VM adını girin. VM adını arama sonuçlarında görüntülendiğinde, onu seçin.
3. Altında **ayarları**seçin **ağ**, aşağıdaki resimde gösterildiği gibi:

    ![Güvenlik kurallarını görüntüle](./media/diagnose-network-traffic-filter-problem/view-security-rules.png)

    Listelenen önceki resimde gördüğünüz adlı bir ağ arabirimi için kurallardır **myVMVMNic**. Olduğunu gördüğünüz **gelen bağlantı noktası kuralları** iki farklı bir ağ güvenlik gruplarından ağ arabirimi için:- **mySubnetNSG**: ağ arabirimi bulunduğu alt ağ ile ilişkili.
        - **myVMNSG**: ağ arabirimi adlı VM ile ilişkili **myVMVMNic**.

    Adlı kuralı **DenyAllInBound** ne gelen iletişimi VM için bağlantı noktası 80 internet'ten engelliyor açıklandığı gibidir [senaryo](#scenario). Kural listeleri *0.0.0.0/0* için **kaynak**, internet içerir. Bağlantı noktası 80 (alt numarası) daha yüksek önceliğe sahip başka bir kural yok sağlar gelen. Bağlantı noktası 80 izin vermek için VM internet'ten gelen, bkz: [ilgili bir sorunu gidermek](#resolve-a-problem). Güvenlik kuralları ve Azure bunları nasıl uygulandığı hakkında daha fazla bilgi için bkz: [ağ güvenlik grupları](security-overview.md).

    Resim alt kısmında, ayrıca bkz: **giden bağlantı noktası kuralları**. Ağ arabirimi için giden bağlantı noktası kuralları altında olan. Resim her NSG için yalnızca dört gelen kuralları gösterir ancak birçok dörtten fazla kuralları Nsg'lerinizi olabilir. Aşağıdaki resimde gördüğünüz **VirtualNetwork** altında **kaynak** ve **hedef** ve **AzureLoadBalancer** altında  **Kaynak**. **VirtualNetwork** ve **AzureLoadBalancer** olan [hizmet etiketleri](security-overview.md#service-tags). Hizmet etiketleri karmaşıklık güvenlik kuralı oluşturma en aza indirmek için IP adresi öneklerini grubunu temsil eder.

4. VM çalışır durum ve ardından olduğundan emin olun **etkin güvenlik kuralları**, aşağıdaki resimde gösterilen etkin güvenlik kurallarını görmek için önceki resimde gösterildiği gibi:

    ![Etkin güvenlik kurallarını görüntüle](./media/diagnose-network-traffic-filter-problem/view-effective-security-rules.png)

    Ağ arabirimi ve alt ağ ile ilişkilendirilen NSG yönelik farklı sekmeler olsa listelenen içinde gördüğünüz aynı adım 3, kurallardır. Aşağıdaki resimde gördüğünüz gibi yalnızca ilk 50 kuralları gösterilmektedir. Tüm kuralları içeren bir .csv dosyasını indirmek için seçin **karşıdan**.

    Hangi her hizmet etiketi önekleri görmek için temsil eder, adlı kuralı gibi bir kural seçin **AllowAzureLoadBalancerInbound**. Aşağıdaki resimde öneklerini gösterilmiştir **AzureLoadBalancer** hizmet etiketi:

    ![Etkin güvenlik kurallarını görüntüle](./media/diagnose-network-traffic-filter-problem/address-prefixes.png)

    Ancak **AzureLoadBalancer** hizmet etiketi yalnızca bir önek temsil eder, diğer hizmet etiketleri birkaç önekleri temsil eder.

4. Önceki adımları gösterdi adlı ağ arabirimi için güvenlik kuralları **myVMVMNic**, ancak adlı ağ arabirimi gördüğünüze göre **myVMVMNic2** bazı önceki resim. Bu örnekte VM bağlı iki ağ arabirimi bulunur. Etkin güvenlik kuralları her ağ arabirimi için farklı olabilir.

    Kurallarını görmek için **myVMVMNic2** ağ arabirimi, onu seçin. Aşağıdaki resimde gösterildiği gibi ağ arabirimi kendi alt ağı ile ilişkili aynı kurallarına sahiptir **myVMVMNic** her iki ağ arabirimleri aynı alt ağda olduğundan ağ arabirimi,. Bir NSG'yi bir alt ağ ile ilişkilendirdiğinizde, kuralları alt ağdaki tüm ağ arabirimleri için uygulanır.

    ![Güvenlik kurallarını görüntüle](./media/diagnose-network-traffic-filter-problem/view-security-rules2.png)

    Farklı **myVMVMNic** ağ arabirimi, **myVMVMNic2** ağ arabirimi için ilişkili bir ağ güvenlik grubu yok. Bir NSG'yi ilişkili veya her bir ağ arabirimi ve alt ağ sıfır olabilir. Her bir ağ arabirimine NSG'yi ilişkili veya alt ağ aynı olabilir ya da farklı. Seçtiğiniz gibi aynı ağ güvenlik grubuna sayıda ağ arabirimleri ve alt ağları ilişkilendirebilirsiniz.

Etkin güvenlik kuralları VM görüntülenebilir olsa da etkin güvenlik kuralları üzerinden görüntüleyebilirsiniz bir:
- **Bir ağ arabirimine**: öğrenin nasıl [bir ağ arabirimi görüntülemek](virtual-network-network-interface.md#view-network-interface-settings).
- **Tek NSG'yi**: öğrenin nasıl [bir NSG görüntülemek](manage-network-security-group.md#view-details-of-a-network-security-group).

## <a name="diagnose-using-powershell"></a>PowerShell kullanarak tanılama

Takip edin komutları çalıştırmadan [Azure bulut Kabuk](https://shell.azure.com/powershell), veya bilgisayarınızdan PowerShell çalıştırarak. Azure bulut Kabuk ücretsiz etkileşimli kabuk ' dir. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. PowerShell bilgisayarınızdan çalıştırırsanız, gereksinim duyduğunuz *AzureRM* PowerShell modülü, sürüm 6.0.1 veya sonraki bir sürümü. Çalıştırma `Get-Module -ListAvailable AzureRM` bilgisayarınızda yüklü olan sürümü bulunamıyor. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell yerel olarak çalıştırıyorsanız, ayrıca çalıştırmanız gereken `Login-AzureRmAccount` Azure'da olan bir hesap ile oturum [gerekli izinleri](virtual-network-network-interface.md#permissions)].

Bir ağ arabirimi için etkili güvenlik kuralları alma [Get-AzureRmEffectiveNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermeffectivenetworksecuritygroup). Aşağıdaki örnek adlı ağ arabirimi için etkili güvenlik kuralları alır *myVMVMNic*, yani bir kaynak grubunda adlı *myResourceGroup*:

```azurepowershell-interactive
Get-AzureRmEffectiveNetworkSecurityGroup `
  -NetworkInterfaceName myVMVMNic interface `
  -ResourceGroupName myResourceGroup
```

Çıkış json biçiminde döndürülür. Çıktı anlamak için bkz: [komut çıktısı yorumlama](#interpret-command-output).
Bir NSG'yi ağ arabirimi, ağ arabiriminin bulunduğu alt ağ veya her ikisi ile ilişkili ise çıktı yalnızca döndürülür. VM çalışır durumda olması gerekir. Bir VM birden çok ağ arabirimleri uygulanan farklı Nsg'leri olabilir. Sorunlarını giderirken, her ağ arabirimi için komutunu çalıştırın.

Bir bağlantı sorunu yaşamaya devam ediyorsanız, bkz: [ek tanılama](#additional-diagnosis) ve [konuları](#considerations).

Bir ağ arabirimi adını bilmiyorsanız, ancak ağ arabiriminin bağlı olduğu VM adını biliyorsanız, aşağıdaki komutlar bir VM'ye bağlı tüm ağ arabirimleri kimliklerini döndürün:

```azurepowershell-interactive
$VM = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroup
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

Bu makalede görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure bulut Kabuk](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu makale Azure CLI Sürüm 2.0.32 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Azure CLI yerel olarak çalıştırıyorsanız, ayrıca çalıştırmanız gereken `az login` ve Azure günlüğüne sahip bir hesap ile [gerekli izinleri](virtual-network-network-interface.md#permissions).

Bir ağ arabirimi için etkili güvenlik kuralları alma [az ağ NIC listesi etkili nsg](/cli/azure/network/nic#az-network-nic-list-effective-nsg). Aşağıdaki örnek adlı ağ arabirimi için etkili güvenlik kuralları alır *myVMVMNic* adlı bir kaynak grubunda olan *myResourceGroup*:

```azurecli-interactive
az network nic list-effective-nsg \
  --name myVMVMNic \
  --resource-group myResourceGroup
```

Çıkış json biçiminde döndürülür. Çıktı anlamak için bkz: [komut çıktısı yorumlama](#interpret-command-output).
Bir NSG'yi ağ arabirimi, ağ arabiriminin bulunduğu alt ağ veya her ikisi ile ilişkili ise çıktı yalnızca döndürülür. VM çalışır durumda olması gerekir. Bir VM birden çok ağ arabirimleri uygulanan farklı Nsg'leri olabilir. Sorunlarını giderirken, her ağ arabirimi için komutunu çalıştırın.

Bir bağlantı sorunu yaşamaya devam ediyorsanız, bkz: [ek tanılama](#additional-diagnosis) ve [konuları](#considerations).

Bir ağ arabirimi adını bilmiyorsanız, ancak ağ arabiriminin bağlı olduğu VM adını biliyorsanız, aşağıdaki komutlar bir VM'ye bağlı tüm ağ arabirimleri kimliklerini döndürün:

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

Önceki çıktısında, ağ arabirimi adıdır *myVMVMNic arabirimi*.

## <a name="interpret-command-output"></a>Komut çıktısı yorumlama

Kullanıp [PowerShell](#diangose-using-powershell), veya [Azure CLI](#diagnose-using-azure-cli) sorunu tanılamak için aşağıdaki bilgileri içeren bir çıktı alırsınız:

- **NetworkSecurityGroup**: ağ güvenlik grubu kimliği.
- **İlişkilendirme**: ağ güvenlik grubu için ilişkili olup bir *Networkınterface* veya *alt*. Bir NSG'yi hem de ilişkiliyse çıktı ile döndürülür **NetworkSecurityGroup**, **ilişkilendirme**, ve **EffectiveSecurityRules**, her NSG için. NSG ilişkilendirilemiyor veya ilişkisi hemen etkili güvenlik kuralları görüntülemek için komutu çalıştırmadan önce komut çıktısında yansıtacak şekilde değiştirmek için birkaç saniye beklemeniz gerekebilir.
- **EffectiveSecurityRules**: her bir özellik açıklaması içinde ayrıntılı [bir güvenlik kuralını](manage-network-security-group.md#create-a-security-rule). Kural ile başlayan adlar *defaultSecurityRules /* varsayılan her NSG'de mevcut güvenlik kurallardır. Kural ile başlayan adlar *securityRules /* oluşturduğunuz kurallardır. Belirten kuralları bir [hizmet etiketi](security-overview.md#service-tags), gibi **Internet**, **VirtualNetwork**, ve **AzureLoadBalancer** için  **destinationAddressPrefix** veya **sourceAddressPrefix** özellikleri için değerleri de **expandedDestinationAddressPrefix** özelliği. **ExpandedDestinationAddressPrefix** özelliği hizmet etiketi tarafından temsil edilen tüm adres öneklerini listeler.

Çıktıda listelenen yinelenen kuralları görürseniz, bir NSG ağ arabirimi ve alt ağ için ilişkili olduğundan değildir. Hem Nsg'ler aynı varsayılan kurallar varsa ve her iki Nsg'ler aynı olmasına kendi kurallarınızı oluşturduysanız ek yinelenen kurallar olabilir.

Adlı kuralı **defaultSecurityRules/DenyAllInBound** ne gelen iletişimi VM için bağlantı noktası 80 internet'ten engelliyor açıklandığı gibidir [senaryo](#scenario). Bağlantı noktası (alt numarası) daha yüksek önceliğe sahip başka bir kural yok sağlar 80 internet'ten gelen.

## <a name="resolve-a-problem"></a>Bir sorunu giderme

Azure kullanıp [portal](#diagnose-using-azure-portal), [PowerShell](#diagnose-using-powershell), veya [Azure CLI](#diagnose-using-azure-cli) içinde sunulan sorunu tanılamak için [senaryo](#scenario) bu makale, çözümü bir ağ güvenlik kuralı aşağıdaki özelliklere sahip oluşturmaktır:

| Özellik                | Değer                                                                              |
|---------                |---------                                                                           |
| Kaynak                  | Herhangi biri                                                                                |
| Kaynak bağlantı noktası aralıkları      | Herhangi biri                                                                                |
| Hedef             | VM IP adresini, bir IP adresi aralığı veya alt ağdaki tüm adresleri. |
| Hedef bağlantı noktası aralıkları | 80                                                                                 |
| Protokol                | TCP                                                                                |
| Eylem                  | İzin Ver                                                                              |
| Öncelik                | 100                                                                                |
| Ad                    | İzin ver-HTTP-tüm                                                                     |

Kural oluşturduktan sonra bağlantı noktası 80 izin kuralının önceliğini adlı varsayılan güvenlik kuralını yüksek olduğu için internet'ten gelen *DenyAllInBound*, trafiği engeller. Bilgi nasıl [bir güvenlik kuralını](manage-network-security-group.md#create-a-security-rule). Farklı Nsg'leri ağ arabirimi hem alt ağ için ilişkili varsa, her iki Nsg'ler aynı kural oluşturmanız gerekir.

Azure işlemleri gelen trafik, (varsa ilişkilendirilmiş bir NSG) alt ağına ilişkili NSG kuralları işler ve ağ arabirimi ile ilişkili NSG kuralları işler. Ağ arabirimi ve alt ağ ile ilişkilendirilen bir NSG varsa, bağlantı noktası VM ulaşmak trafiği için her iki Nsg'ler açık olmalıdır. Yönetim ve iletişim sorunları hafifletmek için tek tek ağ arabirimleri yerine bir alt ağa bir NSG ilişkilendirmek öneririz. Bir alt ağ içindeki VM'ler farklı güvenlik kuralları gerekiyorsa, ağ arabirimleri üyeleri uygulama güvenlik grubu (ASG) yapın ve bir ASG kaynak ve hedef bir güvenlik kuralı belirtin. Daha fazla bilgi edinmek [uygulama güvenlik grupları](security-overview.md#application-security-groups).

İletişim sorunları yaşamaya devam ediyorsanız, bkz: [konuları](#considerations) ve [ek tanılama](#additional-dignosis).

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bağlantı sorunlarını giderme yaparken aşağıdaki noktaları dikkate alın:

* Varsayılan güvenlik kuralları, internet'ten gelen erişimi engellemek ve yalnızca sanal ağ üzerinden gelen trafiğe izin verir. Internet'ten gelen trafiğe izin vermek için güvenlik kuralları varsayılan kuralları daha yüksek önceliğe sahip ekleyin. Daha fazla bilgi edinmek [güvenlik kuralları varsayılan](security-overview.md#default-security-rules), veya nasıl [bir güvenlik Kuralı Ekle](manage-network-security-group.md#create-a-security-rule).
* Varsayılan olarak, sanal ağlar, eşlenen varsa **vırtual_network** hizmet etiketi otomatik olarak genişleyen önekleri eşlenen sanal ağlar için eklenecek. Sanal Ağ eşlemesi için ilgili sorunları gidermek için önekleri görüntüleyebilirsiniz **ExpandedAddressPrefix** listesi. Daha fazla bilgi edinmek [sanal ağ eşlemesi](virtual-network-peering-overview.md) ve [hizmet etiketleri](security-overview.md#service-tags).
* Etkin güvenlik kuralları yalnızca bir ağ arabirimi bir NSG ise VM ağ arabirimiyle ilişkilendirilmiş için gösterilen ve veya, alt ağ ve VM çalışır durumda ise.
* Ağ arabirimi veya alt ağ ile ilişkili hiçbir Nsg'ler vardır ve, varsa bir [genel IP adresi](virtual-network-public-ip-address.md) bir VM'ye atanan, tüm bağlantı noktaları'ten gelen erişim ve giden erişim için herhangi bir yere açın. VM bir ortak IP adresi varsa, alt ağa bir NSG uygulama ağ arabirimi öneririz.

## <a name="additional-diagnosis"></a>Ek tanılama

* Bir VM'den/VM'ye trafik izin verilip verilmediğini belirlemek için hızlı bir testi çalıştırmak için kullandığınız [IP akış doğrulayın](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md) Azure Ağ İzleyicisi yeteneği. IP akış doğrulayın trafiğine izin verilen veya reddedilen bildirir. Engellendi, IP akış doğrulayın söyler, hangi güvenlik kuralı trafiği reddediyor.
* Bir sanal makinenin ağ bağlantısı başarısız olmasına neden olan hiçbir güvenlik kuralları varsa, sorunu nedeniyle olabilir:
  * Sanal makinenin işletim sistemi içinde çalışan güvenlik duvarı yazılımı
  * Sanal gereçler veya şirket içi trafiği için yapılandırılmış yollar. Internet trafiği yeniden yönlendirilen şirket içi ağınıza [zorlamalı tünel](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Bir sanal gereç veya şirket içi tünel Internet trafiğini zorlarsanız, internet'ten VM'ye bağlanabiliyor olmayabilir. VM dışında trafik akışını engelleyebilecek rota sorunlara tanı koymak öğrenmek için bkz: [bir sanal makine ağ trafiği yönlendirme sorunu tanılamak](diagnose-network-routing-problem.md).

## <a name="next-steps"></a>Sonraki adımlar

- Tüm görevler, özellikleri ve ayarları hakkında bilgi edinin bir [ağ güvenlik grubu](manage-network-security-group.md#work-with-network-security-groups) ve [güvenlik kuralları](manage-network-security-group.md#work-with-security-rules).
- Hakkında bilgi edinin [güvenlik kuralları varsayılan](security-overview.md#default-security-rules), [hizmet etiketleri](security-overview.md#service-tags), ve [Azure gelen ve giden trafik için güvenlik kuralları nasıl işlediği](security-overview.md#network-security-groups) bir VM için.
