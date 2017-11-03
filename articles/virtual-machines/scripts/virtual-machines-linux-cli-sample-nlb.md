---
title: "Azure CLI örnek komut dosyası - NLB ile bir Linux VM oluşturma | Microsoft Docs"
description: "Azure CLI örnek komut dosyası - NLB ile bir Linux VM oluşturma"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: fdddf54dceab23394dd26a5c10fa57b921f8cd34
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-highly-available-vm"></a>Yüksek oranda kullanılabilir bir VM oluşturma

Bu komut dosyası örneği yüksek oranda kullanılabilir yapılandırılmış birkaç Ubuntu sanal makineleri çalıştırmak ve dengeli yapılandırma yüklemek için gereken her şeyi oluşturur. Betiği çalıştırdıktan sonra üç sanal makineler, birleştirilmiş bir Azure kullanılabilirlik kümesi için ve bir Azure yük dengeleyici üzerinden erişilebilir olacaktır. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, bir kaynak grubu, sanal makine, kullanılabilirlik kümesi, yük dengeleyici ve tüm ilişkili kaynakları oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az ağ vnet oluşturma](https://docs.microsoft.com/cli/azure/network/vnet#az_network_vnet_create) | Bir Azure sanal ağ ve alt ağ oluşturur. |
| [az ağ genel IP oluşturun](https://docs.microsoft.com/cli/azure/network/public-ip#az_network_public_ip_create) | Bir ortak IP adresi statik bir IP adresi ve ilişkili bir DNS adı ile oluşturur. |
| [az ağ lb oluşturma](https://docs.microsoft.com/cli/azure/network/lb#az_network_lb_create) | Bir Azure ağı yük dengeleyici (NLB) oluşturur. |
| [az ağ lb araştırması oluştur](https://docs.microsoft.com/cli/azure/network/lb/probe#az_network_lb_probe_create) | Bir NLB araştırması oluşturur. Bir NLB araştırması NLB kümesindeki her bir VM izlemek için kullanılır. Tüm VM erişilemez hale gelirse VM trafik yönlendirilmez. |
| [az ağ lb kuralı oluşturma](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | Bir NLB kuralı oluşturur. Bu örnekte, bağlantı noktası 80 için bir kural oluşturulur. NLB HTTP trafiği ulaşan gibi 80 numaralı bağlantı noktasına yönlendirilir NLB kümesindeki sanal makineleri biri. |
| [az ağ lb gelen nat-kuralı oluşturma](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule#az_network_lb_inbound_nat_rule_create) | Bir NLB ağ adresi çevirisi (NAT) kuralı oluşturur.  NAT kuralları bir VM üzerinde bir bağlantı noktasına bir bağlantı noktası, NLB eşleyin. Bu örnekte, SSH trafiği NLB kümesindeki her bir VM için NAT kuralı oluşturulur.  |
| [az ağ nsg oluşturma](https://docs.microsoft.com/cli/azure/network/nsg#az_network_nsg_create) | Internet ve sanal makine arasında bir güvenlik sınırı olan bir ağ güvenlik grubu (NSG) oluşturur. |
| [az ağ nsg kuralı oluşturma](https://docs.microsoft.com/cli/azure/network/nsg/rule#az_network_nsg_rule_create) | Gelen trafiğe izin veren bir NSG kuralı oluşturur. Bu örnekte, bağlantı noktası 22 SSH trafiği için açıldı. |
| [az ağ NIC oluşturun](https://docs.microsoft.com/cli/azure/network/nic#az_network_nic_create) | Bir sanal ağ kartı oluşturur ve sanal ağ, alt ağ ve NSG ekler. |
| [az vm kullanılabilirlik kümesi oluştur](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | Bir kullanılabilirlik kümesi oluşturur. Kullanılabilirlik kümeleri uygulama çalışma süresi hatası oluşursa, kümesinin tamamını değil parametreden şekilde sanal makineler arasında fiziksel kaynakları yayarak emin olun. |
| [az vm oluşturma](https://docs.microsoft.com/cli/azure/vm/availability-set#az_vm_availability_set_create) | Sanal makine oluşturur ve ağ kartı, sanal ağ, alt ağ ve NSG bağlanır. Bu komut ayrıca kullanılan ve yönetici kimlik bilgileri olması için sanal makine görüntüsü belirtir.  |
| [az grubu Sil](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek sanal makine CLI kod örnekleri bulunabilir [Azure Linux VM'de belgelerine](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
