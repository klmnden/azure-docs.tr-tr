---
title: "Azure CLI komut dosyası örneği - yük trafiği dengelemek için sanal makineleri yüksek oranda kullanılabilirlik için | Microsoft Docs"
description: "Azure CLI komut dosyası örneği - yük trafiği dengelemek için sanal makineleri yüksek oranda kullanılabilirlik için"
services: load-balancer
documentationcenter: load-balancer
author: KumudD
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: kumud
ms.openlocfilehash: 810899c671ea141f45f7ce140c3f862f568a2f87
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="load-balance-traffic-to-vms-for-high-availability"></a>Yük trafiği dengelemek için sanal makineleri yüksek oranda kullanılabilirlik için

Bu komut dosyası örneği yüksek oranda kullanılabilir yapılandırılmış birkaç Ubuntu sanal makineleri çalıştırmak ve dengeli yapılandırma yüklemek için gereken her şeyi oluşturur. Betiği çalıştırdıktan sonra üç sanal makineler, birleştirilmiş bir Azure kullanılabilirlik kümesi için ve bir Azure yük dengeleyici üzerinden erişilebilir olacaktır. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu komut, bir kaynak grubu, sanal makine, kullanılabilirlik kümesi, yük dengeleyici ve tüm ilişkili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az ağ vnet oluşturma](https://docs.microsoft.com/cli/azure/network/vnet#az_network_vnet_create) | Bir Azure sanal ağ ve alt ağ oluşturur. |
| [az ağ genel IP oluşturun](https://docs.microsoft.com/cli/azure/network/public-ip#az_network_public_ip_create) | Bir ortak IP adresi statik bir IP adresi ve ilişkili bir DNS adı ile oluşturur. |
| [az ağ lb oluşturma](https://docs.microsoft.com/cli/azure/network/lb#az_network_lb_create) | Bir Azure yük dengeleyici oluşturur. |
| [az ağ lb araştırması oluştur](https://docs.microsoft.com/cli/azure/network/lb/probe#az_network_lb_probe_create) | Bir yük dengeleyici araştırması oluşturur. Yük Dengeleyici araştırmasını yük dengeleyici kümesindeki her bir VM izlemek için kullanılır. Tüm VM erişilemez hale gelirse VM trafik yönlendirilmez. |
| [az ağ lb kuralı oluşturma](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | Yük Dengeleyici kuralı oluşturur. Bu örnekte, bağlantı noktası 80 için bir kural oluşturulur. HTTP trafiği yük dengeleyicide ulaşan gibi 80 numaralı bağlantı noktasına yönlendirilir LB kümesindeki sanal makineleri biri. |
| [az ağ lb gelen nat-kuralı oluşturma](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule#az_network_lb_inbound_nat_rule_create) | Yük Dengeleyici ağ adresi çevirisi (NAT) kuralı oluşturur.  NAT kuralları, bir VM üzerinde bir bağlantı noktasına bir bağlantı noktası yük dengeleyicinin eşleyin. Bu örnekte, SSH trafiği yük dengeleyici kümesindeki her bir VM için NAT kuralı oluşturulur.  |
| [az ağ nsg oluşturma](https://docs.microsoft.com/cli/azure/network/nsg#az_network_nsg_create) | Internet ve sanal makine arasında bir güvenlik sınırı olan bir ağ güvenlik grubu (NSG) oluşturur. |
| [az ağ nsg kuralı oluşturma](https://docs.microsoft.com/cli/azure/network/nsg/rule#az_network_nsg_rule_create) | Gelen trafiğe izin veren bir NSG kuralı oluşturur. Bu örnekte, bağlantı noktası 22 SSH trafiği için açıldı. |
| [az ağ NIC oluşturun](https://docs.microsoft.com/cli/azure/network/nic#az_network_nic_create) | Bir sanal ağ kartı oluşturur ve sanal ağ, alt ağ ve NSG ekler. |
| [az vm kullanılabilirlik kümesi oluştur](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | Bir kullanılabilirlik kümesi oluşturur. Kullanılabilirlik kümeleri uygulama çalışma süresi hatası oluşursa, kümesinin tamamını değil parametreden şekilde sanal makineler arasında fiziksel kaynakları yayarak emin olun. |
| [az vm oluşturma](/cli/azure/vm#az_vm_create) | Sanal makine oluşturur ve ağ kartı, sanal ağ, alt ağ ve NSG bağlanır. Bu komut ayrıca kullanılan ve yönetici kimlik bilgileri olması için sanal makine görüntüsü belirtir.  |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek ağ Azure CLI kod örnekleri bulunabilir [Azure ağ belgeleri](../cli-samples.md).