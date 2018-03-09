---
title: "Sanal Ağ eşlemesi ile - sanal ağlara bağlanabilir Azure CLI | Microsoft Docs"
description: "Sanal Ağ eşlemesi ile sanal ağları bağlamayı öğrenin."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/06/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: df56f2e3e13f80e7ce2c2b6c9cffeac3d03776e5
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-the-azure-cli"></a>Azure CLI kullanarak sanal ağ eşlemesi ile sanal ağlara bağlanabilir

Sanal ağlar birbirlerine sanal ağ eşlemesi ile bağlayabilirsiniz. Sanal ağlar eşlendikten sonra iki sanal ağlarda bulunan kaynaklar kaynaklar aynı sanal ağda değilmiş gibi aynı gecikme süresi ve bant genişliği ile birbirleri ile iletişim kuramıyor. Bu makalede, oluşturma ve iki sanal ağlar arasındaki eşleme kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * İki sanal ağ oluşturma
> * Sanal ağlar arasında eşleme oluşturma
> * Test eşleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 

## <a name="create-virtual-networks"></a>Sanal ağlar oluşturma

Bir sanal ağ oluşturmadan önce sanal ağ ve bu makalede oluşturulan tüm kaynaklar için bir kaynak grubu oluşturmanız gerekir. [az group create](/cli/azure/group#az_group_create) ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

[az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) komutu ile bir sanal ağ oluşturun. Aşağıdaki örnek adlı bir sanal ağ oluşturur *myVirtualNetwork1* adres ön ekine sahip *10.0.0.0/16*.

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork1 \
  --resource-group myResourceGroup \
  --address-prefixes 10.0.0.0/16 \
  --subnet-name Subnet1 \
  --subnet-prefix 10.0.0.0/24
```

Adlı bir sanal ağ oluşturma *myVirtualNetwork2* adres ön ekine sahip *10.1.0.0/16*. Adres ön eki adres ön eki ile çakışmaması *myVirtualNetwork1* sanal ağ. Adres öneklerini örtüşen sanal ağlar eş olamaz.

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --address-prefixes 10.1.0.0/16 \
  --subnet-name Subnet1 \
  --subnet-prefix 10.1.0.0/24
```

## <a name="peer-virtual-networks"></a>Eş sanal ağlar

Eşlemeler, her sanal ağ ile Kimliğini ilk edinmeniz gerekir böylece sanal ağ kimlikleri arasında kurulan [az ağ vnet show](/cli/azure/network/vnet#az_network_vnet_show) ve kimliği bir değişkende saklayın.

```azurecli-interactive
# Get the id for myVirtualNetwork1.
vNet1Id=$(az network vnet show \
  --resource-group myResourceGroup \
  --name myVirtualNetwork1 \
  --query id --out tsv)

# Get the id for myVirtualNetwork2.
vNet2Id=$(az network vnet show \
  --resource-group myResourceGroup \
  --name myVirtualNetwork2 \
  --query id \
  --out tsv)
```

Gelen eşlemesi oluşturmak *myVirtualNetwork1* için *myVirtualNetwork2* ile [az ağ vnet eşlemesi oluşturma](/cli/azure/network/vnet/peering#az_network_vnet_peering_create). Varsa `--allow-vnet-access` parametresi belirtilmezse, bir eşleme kuruldu, ancak hiçbir iletişimi üzerinden akabilir.

```azurecli-interactive
az network vnet peering create \
  --name myVirtualNetwork1-myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork1 \
  --remote-vnet-id $vNet2Id \
  --allow-vnet-access
```

Önceki komutu yürütüldükten sonra döndürülen çıktısında, gördüğünüz **peeringState** olan *başlatılan*. Eşleme kalırken *başlatılan* gelen eşlemesi oluşturmak kadar durum *myVirtualNetwork2* için *myVirtualNetwork1*. Gelen eşlemesi oluşturmak *myVirtualNetwork2* için *myVirtualNetwork1*. 

```azurecli-interactive
az network vnet peering create \
  --name myVirtualNetwork2-myVirtualNetwork1 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork2 \
  --remote-vnet-id $vNet1Id \
  --allow-vnet-access
```

Önceki komutu yürütüldükten sonra döndürülen çıktısında, gördüğünüz **peeringState** olan *bağlı*. Azure eşleme durumunu da değişir *myVirtualNetwork1 myVirtualNetwork2* için eşleme *bağlı*. Onaylamak için eşleme durumu *myVirtualNetwork1 myVirtualNetwork2* eşliği değiştirildi *bağlı* ile [az ağ vnet eşleme Göster](/cli/azure/network/vnet/peering#az_network_vnet_peering_show).

```azurecli-interactive
az network vnet peering show \
  --name myVirtualNetwork1-myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork1 \
  --query peeringState
```

Kadar diğer sanal ağ kaynaklarında bir sanal ağ olamaz iletişim kaynaklarla **peeringState** hem de sanal ağlarda eşlemeleri için *bağlı*. 

Eşlemeler iki sanal ağ arasında ancak geçişli değil. Böylece, örneğin, ayrıca eş istemeniz durumunda *myVirtualNetwork2* için *myVirtualNetwork3*, ek sanal ağlar arasında eşlemeyi oluşturmanıza gerek *myVirtualNetwork2* ve *myVirtualNetwork3*. Olsa bile *myVirtualNetwork1* ile eşlenen *myVirtualNetwork2*, kaynakları içinde *myVirtualNetwork1* yalnızca kaynaklara erişim  *myVirtualNetwork3* varsa *myVirtualNetwork1* de ile eşlenen *myVirtualNetwork3*. 

Eşleme önce üretim sanal ağlar, baştan sona ile öğrenmeniz olduğunu önerilir [eşleme genel bakış](virtual-network-peering-overview.md), [eşliği yönetme](virtual-network-manage-peering.md), ve [sanal ağ sınırları ](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Bu makalede bir iki sanal ağ aynı abonelik ve konum arasında eşleme gösterir ancak sanal ağlarda ayrıca eş [farklı bölgelerde](#register) ve [farklı Azure abonelikleri](create-peering-different-subscriptions.md#cli). Ayrıca oluşturabilirsiniz [hub ve bağlı bileşen ağ tasarımları](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) eşliği ile.

## <a name="test-peering"></a>Test eşleme

Bir eşleme aracılığıyla farklı sanal ağlardaki sanal makineler arasındaki ağ iletişimi sınamak için her alt ağda bir sanal makine dağıtın ve sanal makineler iletişim. 

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bir sanal makine oluşturma [az vm oluşturma](/cli/azure/vm#az_vm_create). Aşağıdaki örnek, bir sanal makine adlı oluşturur *myVm1* içinde *myVirtualNetwork1* sanal ağ. SSH anahtarları varsayılan anahtar konumunda zaten mevcut değilse komutu bunları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın. `--no-wait` Seçeneği bir sonraki adıma devam etmek için bu sanal makine arka planda oluşturur.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork1 \
  --subnet Subnet1 \
  --generate-ssh-keys \
  --no-wait
```

Azure otomatik olarak atar 10.0.0.4 sanal makinenin özel IP adresi 10.0.0.4 ilk kullanılabilir IP adresi olduğundan *Subnet1* , *myVirtualNetwork1*. 

Bir sanal makine oluşturmak *myVirtualNetwork2* sanal ağ.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork2 \
  --subnet Subnet1 \
  --generate-ssh-keys
```

Sanal makine oluşturmak için birkaç dakika sürer. Sanal makine oluşturulduktan sonra Azure CLI bilgileri aşağıdaki örneğe benzer şekilde gösterir: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVm2",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.1.0.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```

Örnek çıktıda, gördüğünüz **privateIpAddress** olan *10.1.0.4*. Azure DHCP otomatik olarak atanmış 10.1.0.4 sanal makineye, listedeki ilk kullanılabilir adres olduğundan *Subnet1* , *myVirtualNetwork2*. Not edin **Publicıpaddress**. Bu adres sanal makine bir sonraki adımda Internet'ten erişmek için kullanılır.

### <a name="test-virtual-machine-communication"></a>Test sanal makinesi iletişimi

İle bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın *myVm2* sanal makine. Değiştir `<publicIpAddress>` sanal makinenizin ortak IP adresine sahip. Önceki örnekte, ortak IP adresidir *13.90.242.231*.

```bash 
ssh <publicIpAddress>
```

Sanal makinede ping *myVirtualNetwork1*.

```bash 
ping 10.0.0.4 -c 4
```

Dört yanıt alırsınız. Sanal makinenin adına göre ping (*myVm1*), IP adresini yerine ping, çünkü başarısız *myVm1* bir bilinmeyen ana bilgisayar adıdır. Azure'nın varsayılan ad çözümlemesi, aynı sanal ağdaki sanal makineler arasında ancak farklı sanal ağlardaki sanal makineler arasında değil çalışır. Sanal ağlar arasında adları çözümlemek için şunları yapmanız gerekir [kendi DNS sunucusu dağıtma](virtual-networks-name-resolution-for-vms-and-role-instances.md) veya [Azure DNS özel etki alanları](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

SSH oturumu kapatmanız *myVm2* sanal makine. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerektiğinde kullanmak [az grubu Sil](/cli/azure/group#az_group_delete) kaynak grubu ve içerdiği kaynakların tümünü kaldırmak için.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

**<a name="register"></a>Genel sanal ağ eşleme Önizleme için kaydolun**

Aynı bölgedeki sanal ağları eşleme özelliği genel kullanıma açıktır. Sanal ağlar farklı bölgelerde şu anda önizlemede eşleme. Bkz: [sanal ağı güncelleştirmelerini](https://azure.microsoft.com/updates/?product=virtual-network) bölgeleri için kullanılabilir. Sanal ağlar bölgeler arasında eş için önce (içinde eş istediğiniz her sanal ağ kullanılıyor abonelik) aşağıdaki adımları tamamlayarak Önizleme için kaydetmeniz gerekir:

1. Önizleme için aşağıdaki komutları girerek kaydedin:

  ```azurecli-interactive
  az feature register --name AllowGlobalVnetPeering --namespace Microsoft.Network
  az provider register --name Microsoft.Network
  ```

2. Aşağıdaki komutu girerek Önizleme için kayıtlı olduklarını doğrulayın:

  ```azurecli-interactive
  az feature show --name AllowGlobalVnetPeering --namespace Microsoft.Network
  ```

  Sanal ağlar farklı bölgelerde önce eş çalışırsanız **RegistrationState** önceki komutunu girdikten sonra aldığınız çıktı **kayıtlı** hem de abonelikleri için başarısız eşleme .

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, sanal ağ eşlemesi ile iki ağlara bağlanmak nasıl öğrendiniz. Yapabilecekleriniz [kendi bilgisayarınızda bir sanal ağa bağlanmak](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bir VPN üzerinden ve sanal ağ veya eşlenmiş sanal ağlar kaynakları ile etkileşim.

Birçok sanal ağ makalelerinde ele görevi tamamlamak yeniden kullanılabilir komut dosyaları için kod örnekleri devam edin.

> [!div class="nextstepaction"]
> [Sanal ağ kod örnekleri](../networking/cli-samples.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
