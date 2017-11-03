---
title: "Azure CLI komut dosyası örneği - yük dengelemesi Azure CLI ile birden çok Web siteleri | Microsoft Docs"
description: "Azure CLI komut dosyası örneği - yükünü dengelemek için aynı sanal makine birden çok Web sitesi"
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
ms.openlocfilehash: 98b07bfabf2d01c7ae3db7365cfbab3639c6f026
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="load-balance-multiple-websites"></a>Yük Dengelemesi birden çok Web sitesi

Bu komut dosyası örneği iki sanal bir kullanılabilirlik kümesi üyesi olan makinelerle (VM) bir sanal ağ oluşturur. Bir yük dengeleyici iki VM için iki ayrı IP adresleri için trafiğini yönlendirir. Komut dosyasını çalıştırdıktan sonra web sunucusu yazılımı VM'ler ve ana bilgisayar, her biri kendi IP adresiyle birden çok web sitelerini dağıtabilirsiniz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası


[!code-azurecli-interactive[main](../../../cli_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.sh  "Load balance multiple web sites")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, bir kaynak grubu, sanal ağ, yük dengeleyici ve tüm ilişkili kaynakları oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az ağ vnet oluşturma](https://docs.microsoft.com/cli/azure/network/vnet#az_network_vnet_create) | Bir Azure sanal ağ ve alt ağ oluşturur. |
| [az ağ genel IP oluşturun](https://docs.microsoft.com/cli/azure/network/public-ip#az_network_public_ip_create) | Bir ortak IP adresi statik bir IP adresi ve ilişkili bir DNS adı ile oluşturur. |
| [az ağ lb oluşturma](https://docs.microsoft.com/cli/azure/network/lb#az_network_lb_create) | Bir Azure yük dengeleyici oluşturur. |
| [az ağ lb araştırması oluştur](https://docs.microsoft.com/cli/azure/network/lb/probe#az_network_lb_probe_create) | Bir yük dengeleyici araştırması oluşturur. Yük Dengeleyici araştırmasını yük dengeleyici kümesindeki her bir VM izlemek için kullanılır. Tüm VM erişilemez hale gelirse VM trafik yönlendirilmez. |
| [az ağ lb kuralı oluşturma](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | Yük Dengeleyici kuralı oluşturur. Bu örnekte, bağlantı noktası 80 için bir kural oluşturulur. HTTP trafiği yük dengeleyicide ulaşan gibi 80 numaralı bağlantı noktasına yönlendirilir yük dengeleyici kümesindeki sanal makineleri biri. |
| [az ağ lb ön uç-IP oluşturun](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip#az_network_lb_frontend_ip_create) | Bir ön uç IP adresi yük dengeleyici için oluşturun. |
| [az ağ lb adres havuzu oluşturma](https://docs.microsoft.com/cli/azure/network/lb/address-pool#az_network_lb_address_pool_create) | Arka uç adres havuzu oluşturur. |
| [az ağ NIC oluşturun](https://docs.microsoft.com/cli/azure/network/nic#az_network_nic_create) | Bir sanal ağ kartı oluşturur ve sanal ağ ve alt ekler. |
| [az vm kullanılabilirlik kümesi oluştur](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | Bir kullanılabilirlik kümesi oluşturur. Kullanılabilirlik kümeleri uygulama çalışma süresi hatası oluşursa, kümesinin tamamını değil parametreden şekilde sanal makineler arasında fiziksel kaynakları yayarak emin olun. |
| [az ağ NIC IP-config oluşturma](https://docs.microsoft.com/cli/azure/network/nic/ip-config#az_network_nic_ip_config_create) | Bir IP confiuration oluşturur. Aboneliğiniz için etkin Microsoft.Network/AllowMultipleIpConfigurationsPerNic özelliği yüklü olmalıdır. Yalnızca bir yapılandırma belirlenmiş NIC, her birincil IP yapılandırması kullanan yapma birincil bayrağı. |
| [az vm oluşturma](https://docs.microsoft.com/cli/azure/vm/availability-set#az_vm_availability_set_create) | Sanal makine oluşturur ve ağ kartı, sanal ağ, alt ağ ve NSG bağlanır. Bu komut ayrıca kullanılan ve yönetici kimlik bilgileri olması için sanal makine görüntüsü belirtir.  |
| [az grubu Sil](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek ağ CLI kod örnekleri bulunabilir [Azure ağ genel görünümü belgelerine](../cli-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
