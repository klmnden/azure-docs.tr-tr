---
title: Ağ trafiği - Azure CLI filtre | Microsoft Docs
description: Bu makalede, Azure CLI kullanarak bir ağ güvenlik grubu ile bir alt ağ trafiğini filtrelemek öğrenin.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to filter network traffic to virtual machines that perform similar functions, such as web servers.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/30/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 11dc0e5f6ee398b2a745ed60cbc166e2a1697c3e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31423186"
---
# <a name="filter-network-traffic-with-a-network-security-group-using-the-azure-cli"></a>Azure CLI kullanarak bir ağ güvenlik grubu ile ağ trafiği filtreleme

Ağ güvenlik grubu ile ağ trafiği için gelen ve giden sanal ağ alt ağından filtreleyebilirsiniz. Ağ güvenlik grupları, IP adresi, bağlantı noktası ve protokol ağ trafiğinin filtre güvenlik kuralları içerir. Güvenlik kuralları, bir alt ağda dağıtılan kaynaklara uygulanır. Bu makalede şunları öğreneceksiniz:

* Ağ güvenlik grubu ve güvenlik kuralları oluşturma
* Bir sanal ağ oluşturun ve bir alt ağ için ağ güvenlik grubu ilişkilendirin
* Sanal makineler (VM) bir alt ağ dağıtma
* Test trafik filtreleri

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, bu makalede, Azure CLI Sürüm 2.0.28 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 


## <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

Ağ güvenlik grubu güvenlik kuralları içerir. Güvenlik kuralları, bir kaynak ve hedef belirtin. Kaynakları ve hedeflere uygulama güvenlik grupları olabilir.

### <a name="create-application-security-groups"></a>Uygulama güvenlik grupları oluşturma

Önce bu makalede ile oluşturulan tüm kaynaklar için bir kaynak grubu oluşturmak [az grubu oluşturma](/cli/azure/group#az_group_create). Aşağıdaki örnek, bir kaynak grubunda oluşturur *eastus* konumu: 

```azurecli-interactive
az group create \
  --name myResourceGroup \
  --location eastus
```

Uygulama güvenlik grubu ile oluşturma [az ağ asg oluşturma](/cli/azure/network/asg#az_network_asg_create). Uygulama güvenlik grubu grubu sunucuları için gereksinimler filtreleme benzer bağlantı noktası ile sağlar. Aşağıdaki örnek, iki uygulama güvenlik grubu oluşturur.

```azurecli-interactive
az network asg create \
  --resource-group myResourceGroup \
  --name myAsgWebServers \
  --location eastus

az network asg create \
  --resource-group myResourceGroup \
  --name myAsgMgmtServers \
  --location eastus
```

### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

Bir ağ güvenlik grubu oluşturma [az ağ nsg oluşturma](/cli/azure/network/nsg#az_network_nsg_create). Aşağıdaki örnek adlı bir ağ güvenlik grubu oluşturur *myNsg*: 

```azurecli-interactive 
# Create a network security group
az network nsg create \
  --resource-group myResourceGroup \
  --name myNsg
```

### <a name="create-security-rules"></a>Güvenlik kuralları oluşturma

Bir güvenlik kuralıyla oluşturma [az ağ nsg kuralını](/cli/azure/network/nsg/rule#az_network_nsg_rule_create). Aşağıdaki örnek, trafiğe izin veren bir kural oluşturur Internet'ten gelen *myWebServers* 80 ve 443 numaralı bağlantı noktaları üzerinden uygulama güvenlik grubu:

```azurecli-interactive
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsg \
  --name Allow-Web-All \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix Internet \
  --source-port-range "*" \
  --destination-asgs "myAsgWebServers" \
  --destination-port-range 80 443
```

Aşağıdaki örnek, trafiğe izin veren bir kural oluşturur Internet'ten gelen *myMgmtServers* bağlantı noktası 22 üzerinden uygulama güvenlik grubu:

```azurecli-interactive
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsg \
  --name Allow-SSH-All \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 110 \
  --source-address-prefix Internet \
  --source-port-range "*" \
  --destination-asgs "myAsgMgmtServers" \
  --destination-port-range 22
```

Bu makalede, SSH (bağlantı noktası 22) için internet kullanıma sunulan *myAsgMgmtServers* VM. Bağlantı noktası 22 Internet gösterme yerine üretim ortamları için kullanarak yönetmek istediğiniz Azure kaynaklarına bağlanmak önerilir bir [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [özel](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ağ bağlantısı.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) komutu ile bir sanal ağ oluşturun. Aşağıdaki örnek, bir sanal adlı oluşturur *myVirtualNetwork*:

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefixes 10.0.0.0/16
```

Bir sanal ağ ile bir alt ağ Ekle [az ağ sanal alt oluşturma](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Aşağıdaki örnek adlı bir alt ağ, *mySubnet* ilişkilendirir ve sanal ağ için *myNsg* ona ağ güvenlik grubu:

```azurecli-interactive
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name mySubnet \
  --address-prefix 10.0.0.0/24 \
  --network-security-group myNsg
```

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Trafik bir sonraki adımda filtreleme doğrulamak için iki VM sanal ağ oluşturun. 

[az vm create](/cli/azure/vm#az_vm_create) ile bir VM oluşturun. Aşağıdaki örnek, bir web sunucusu olarak davranacak bir VM oluşturur. `--asgs myAsgWebServers` Seçeneği neden olan bir üyesi VM için oluşturduğu ağ arabirimi yapmak Azure *myAsgWebServers* uygulama güvenlik grubu.

`--nsg ""` Seçeneği Azure VM oluşturduğunda oluşturduğu ağ arabirimi için varsayılan bir ağ güvenlik grubu oluşturma Azure önlemek için belirtilir. Bu makalede kolaylaştırmak için bir parola kullanılır. Anahtarlar genellikle üretim dağıtımında kullanılır. Anahtarları kullanıyorsanız, SSH aracı iletmeyi kalan adımları için yapılandırmanız gerekir. Daha fazla bilgi için SSH istemcisi belgelerine bakın. Değiştir `<replace-with-your-password>` seçtiğiniz bir parola ile aşağıdaki komutu.

```azurecli-interactive
adminPassword="<replace-with-your-password>"

az vm create \
  --resource-group myResourceGroup \
  --name myVmWeb \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet mySubnet \
  --nsg "" \
  --asgs myAsgWebServers \
  --admin-username azureuser \
  --admin-password $adminPassword
```

Sanal makinenin oluşturulması birkaç dakika sürer. VM oluşturulduktan sonra aşağıdaki örneğe benzer bir çıktı verilir: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVmWeb",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```

Not edin **Publicıpaddress**. Bu adres, sonraki adımda Internet'ten VM erişmek için kullanılır.  Bir yönetim sunucusu hizmet için bir VM oluşturun:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmMgmt \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet mySubnet \
  --nsg "" \
  --asgs myAsgMgmtServers \
  --admin-username azureuser \
  --admin-password $adminPassword
```

Sanal makinenin oluşturulması birkaç dakika sürer. VM oluşturulduktan sonra Not **Publicıpaddress** döndürülen çıkışı. Bu adres, sonraki adımda VM erişmek için kullanılır. Azure VM oluşturma tamamlanana kadar sonraki adıma devam yok.

## <a name="test-traffic-filters"></a>Test trafik filtreleri

İle bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın *myVmMgmt* VM. Değiştir *<publicIpAddress>* vm'nizin ortak IP adresine sahip. Yukarıdaki örnekte, IP adresidir *13.90.242.231*.

```bash 
ssh azureuser@<publicIpAddress>
```

Girdiğiniz parola için bir parola istendiğinde, girin [oluşturma VM'ler](#create-virtual-machines).

Bağlantı başarılı olur, Internet'ten gelen bağlantı noktası 22 izin verilmediğinden *myAsgMgmtServers* ağ arabiriminin bağlı uygulama güvenlik grubu *myVmMgmt* VM konusu.

SSH için aşağıdaki komutu kullanın *myVmWeb* VM'den *myVmMgmt* VM:

```bash 
ssh azureuser@myVmWeb
```

Varsayılan güvenlik kuralı her ağ güvenlik grubu içinde bir sanal ağ içindeki tüm IP adresleri arasındaki tüm bağlantı noktaları üzerinden trafiğe izin verdiğinden bağlantı başarılı olur. SSH için olamaz *myVmWeb* Internet'ten VM Güvenlik kuralı için çünkü *myAsgWebServers* bağlantı noktası izin vermez 22 Internet'ten gelen.

Nginx web sunucusu yüklemek için aşağıdaki komutları kullanın *myVmWeb* VM:

```bash 
# Update package source
sudo apt-get -y update

# Install NGINX
sudo apt-get -y install nginx
```

*MyVmWeb* VM varsayılan güvenlik kuralını internet tüm giden trafiğe izin verdiğinden nginx almak için internet giden izin verilir. Çıkış *myVmWeb* adresindeki bırakan SSH oturumu `username@myVmMgmt:~$` , komut istemi *myVmMgmt* VM. Nginx Karşılama ekranında almak için *myVmWeb* VM, aşağıdaki komutu girin:

```bash
curl myVmWeb
```

Oturumu kapatın *myVmMgmt* VM. Erişebildiğinizi onaylamak için *myVmWeb* web sunucusundan Azure dışında girin `curl <publicIpAddress>` kendi bilgisayardan. Bağlantı başarılı olur, Internet'ten gelen bağlantı noktası 80 izin verilmediğinden *myAsgWebServers* ağ arabiriminin bağlı uygulama güvenlik grubu *myVmWeb* VM konusu.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerektiğinde kullanmak [az grubu Sil](/cli/azure/group#az_group_delete) kaynak grubu ve içerdiği kaynakların tümünü kaldırmak için.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir ağ güvenlik grubu oluşturulur ve sanal ağ alt ağına ilişkilendirilmiş. Ağ güvenlik grupları hakkında daha fazla bilgi için bkz: [ağ güvenlik grubu genel bakış](security-overview.md) ve [bir ağ güvenlik grubunu yönetme](manage-network-security-group.md).

Varsayılan alt ağlar arasında trafiği Azure yollar. Bunun yerine, bir güvenlik duvarı hizmet veren bir VM ile alt ağlar arasında trafiği yönlendirmek için seçebilirsiniz örneğin. Bilgi edinmek için bkz [bir yol tablosu oluşturmanız](tutorial-create-route-table-cli.md).