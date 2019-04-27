---
title: Bir statik genel IP adresiyle - Azure CLI VM oluşturma | Microsoft Docs
description: Azure komut satırı arabirimi (CLI) kullanarak statik genel IP adresiyle VM oluşturma konusunda bilgi edinin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: 55bc21b0-2a45-4943-a5e7-8d785d0d015c
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/08/2018
ms.author: kumud
ms.openlocfilehash: eafdbf731ce6ae37c321712d7574ce578e704cc0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60743063"
---
# <a name="create-a-virtual-machine-with-a-static-public-ip-address-using-the-azure-cli"></a>Azure CLI kullanarak bir statik genel IP adresiyle bir sanal makine oluşturun

Bir statik genel IP adresiyle bir sanal makine oluşturabilirsiniz. Genel bir IP adresi bir sanal makineye internet üzerinden iletişim kurmasına olanak tanır. Adresi hiçbir zaman değiştirdiğinden emin olmak için bir dinamik adres yerine bir statik genel IP adresi atayın. Daha fazla bilgi edinin [statik genel IP adresleri](virtual-network-ip-addresses-overview-arm.md#allocation-method). Varolan bir sanal makineye gelen dinamik statik olarak atanmış bir genel IP adresini değiştirmek için veya özel IP adresleri ile çalışmak için bkz: [ekleme, değiştirme veya kaldırma IP adresleri](virtual-network-network-interface-addresses.md). Genel IP adreslerine sahip bir [nominal bir ücret](https://azure.microsoft.com/pricing/details/ip-addresses)İşte bir [sınırı](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) için abonelik başına kullanabileceğiniz ortak IP adresi sayısı.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Yerel bilgisayarınızdan veya Azure Cloud Shell'i kullanarak aşağıdaki adımları tamamlayabilirsiniz. Yerel bilgisayarınıza kullanılacak olduğundan emin olun [Azure CLI'yı](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Azure Cloud Shell'i kullanmak için **deneyin** takip eden herhangi bir komut kutusunu sağ üst köşesindeki içinde. Cloud Shell oturumunuzu Azure'da oturum açar.

1. Cloud Shell kullanıyorsanız, 2. adıma atlayın. Azure'a komut oturumuna ve oturum açma `az login`.
2. [az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek, Doğu ABD Azure bölgesinde bir kaynak grubu oluşturur:

   ```azurecli-interactive
   az group create --name myResourceGroup --location eastus
   ```

3. [az vm create](/cli/azure/vm#az-vm-create) komutuyla bir sanal makine oluşturun. `--public-ip-address-allocation=static` Seçeneği, sanal makine için statik genel IP adresi atar. Aşağıdaki örnek, adlandırılmış bir statik, temel SKU genel IP adresiyle bir Ubuntu sanal makinesi oluşturur *Mypublicıpaddress*:

   ```azurecli-interactive
   az vm create \
     --resource-group myResourceGroup \
     --name myVM \
     --image UbuntuLTS \
     --admin-username azureuser \
     --generate-ssh-keys \
     --public-ip-address myPublicIpAddress \
     --public-ip-address-allocation static
   ```

   Standart SKU genel IP adresi olması gerekiyorsa, ekleme `--public-ip-sku Standard` önceki komutu. Daha fazla bilgi edinin [SKU genel IP adresi](virtual-network-ip-addresses-overview-arm.md#sku). Sanal makine genel bir Azure Load Balancer arka uç havuzuna eklenecek, sanal makinenin genel IP adresinin SKU yük dengeleyicinin genel IP adresinin SKU eşleşmesi gerekir. Ayrıntılar için bkz [Azure Load Balancer](../load-balancer/load-balancer-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#skus).

4. Atanan genel IP adresini görüntüleyin ve ile bir statik, temel SKU adresi oluşturulduğunu onaylayın [az ağ public-ip show](/cli/azure/network/public-ip#az-network-public-ip-show):

   ```azurecli-interactive
   az network public-ip show \
     --resource-group myResourceGroup \
     --name myPublicIpAddress \
     --query [ipAddress,publicIpAllocationMethod,sku] \
     --output table
   ```

   Azure, sanal makineyi oluşturduğunuz bölge içinde kullanılan adreslerinden genel bir IP adresi atanır. Azure [Genel](https://www.microsoft.com/download/details.aspx?id=56519), [US government](https://www.microsoft.com/download/details.aspx?id=57063), [Çin](https://www.microsoft.com/download/details.aspx?id=57062) ve [Almanya](https://www.microsoft.com/download/details.aspx?id=57064) bulutları için bu aralıkların (ön ekler) listesini indirebilirsiniz.

> [!WARNING]
> Sanal makinenin işletim sistemi içinde IP adresi ayarlarını değiştirmeyin. İşletim sistemi Azure genel IP adreslerini farkında değil. Özel IP adresi ayarları işletim sistemine ekleyebilirsiniz ancak sürece bunu öneririz gerektiği kadar edindikten sonra değil [bir işletim sistemine özel bir IP adresi Ekle](virtual-network-network-interface-addresses.md#private).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az group delete](/cli/azure/group#az-group-delete) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [genel IP adresleri](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) azure'da
- Tüm hakkında daha fazla bilgi [genel IP adresi ayarları](virtual-network-public-ip-address.md#create-a-public-ip-address)
- Daha fazla bilgi edinin [özel IP adresleri](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) ve atama bir [statik özel IP adresi](virtual-network-network-interface-addresses.md#add-ip-addresses) bir Azure sanal makinesi için
- Oluşturma hakkında daha fazla bilgi edinin [Linux](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Windows](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makineler
