---
title: -Azure CLI olan ağ trafiğini filtreleme | Microsoft Docs
description: Bu makalede, bir alt ağ Azure CLI kullanarak bir ağ güvenlik grubu ile ağ trafiğini filtreleme öğrenin.
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
ms.openlocfilehash: 2c24634a42fd420eae204437418b82479869bbe5
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59525553"
---
# <a name="filter-network-traffic-with-a-network-security-group-using-the-azure-cli"></a>Azure CLI kullanarak bir ağ güvenlik grubu ile ağ trafiğini filtreleme

Bir sanal ağ alt ağına gelen ve sanal ağ alt ağından giden ağ trafiğini, bir ağ güvenlik grubu ile filtreleyebilirsiniz. Ağ güvenlik grupları, ağ trafiğini IP adresi, bağlantı noktası ve protokole göre filtreleyen güvenlik kuralları içerir. Güvenlik kuralları bir alt ağda dağıtılmış kaynaklara uygulanır. Bu makalede şunları öğreneceksiniz:

* Ağ güvenlik grubu ve güvenlik kuralları oluşturma
* Bir sanal ağ oluşturma ve ağ güvenlik grubunu alt ağ ile ilişkilendirme
* Sanal makineleri (VM) bir alt ağa dağıtma
* Trafik filtrelerini test etme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure CLI 2.0.28 çalıştırdığınız gerekir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli). 


## <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

Bir ağ güvenlik grubu, güvenlik kuralları içerir. Güvenlik kuralları, bir kaynak ve hedefi belirtir. Kaynaklar ve hedefler, uygulama güvenlik grupları olabilir.

### <a name="create-application-security-groups"></a>Uygulama güvenlik grupları oluşturma

Önce bu makalede oluşturulan tüm kaynakları için bir kaynak grubu oluşturma [az grubu oluşturma](/cli/azure/group). Aşağıdaki örnekte *eastus* konumunda bir kaynak grubu oluşturulmaktadır: 

```azurecli-interactive
az group create \
  --name myResourceGroup \
  --location eastus
```

Bir uygulama güvenlik grubu oluşturun [az ağ asg oluşturma](/cli/azure/network/asg). Uygulama güvenlik grubu, benzer bağlantı noktası filtreleme gereksinimlerine sahip sunucuları gruplandırmanızı sağlar. Aşağıdaki örnek iki uygulama güvenlik grubu oluşturur.

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

Bir ağ güvenlik grubu oluşturun [az ağ nsg oluşturma](/cli/azure/network/nsg). Aşağıdaki örnek *myNsg* adlı bir ağ güvenlik grubu oluşturur: 

```azurecli-interactive 
# Create a network security group
az network nsg create \
  --resource-group myResourceGroup \
  --name myNsg
```

### <a name="create-security-rules"></a>Güvenlik kuralları oluşturma

Bir güvenlik kuralı oluşturun [az ağ nsg kuralı oluşturmak](/cli/azure/network/nsg/rule). Aşağıdaki örnek, internetten gelen trafiğin 80 ve 443 numaralı bağlantı noktaları üzerinden *myWebServers* uygulama güvenlik grubuna gitmesine izin veren bir kural oluşturur:

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

Aşağıdaki örnek, trafiğe izin veren bir kural oluşturur için Internet'ten gelen *myMgmtServers* bağlantı noktası 22 üzerinden uygulama güvenlik grubu:

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

Bu makalede, SSH (bağlantı noktası 22) için İnternet'e kullanıma sunulan *myAsgMgmtServers* VM. Bağlantı noktası 22 Internet'e gösterme yerine üretim ortamları için kullanarak yönetmek istediğiniz Azure kaynaklarına bağlamanız önerilir bir [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [özel](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ağ bağlantısı.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[az network vnet create](/cli/azure/network/vnet) komutu ile bir sanal ağ oluşturun. Aşağıdaki örnek *myVirtualNetwork* adlı bir sanal ağ oluşturur:

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefixes 10.0.0.0/16
```

Bir sanal ağ ile bir alt ağ Ekle [az ağ sanal ağ alt ağı oluşturma](/cli/azure/network/vnet/subnet). Aşağıdaki örnek, sanal ağa *mySubnet* adlı bir alt ağ ekler ve *myNsg* ağ güvenlik grubunu onunla ilişkilendirir:

```azurecli-interactive
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name mySubnet \
  --address-prefix 10.0.0.0/24 \
  --network-security-group myNsg
```

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Daha sonraki bir adımda trafik filtrelemesini doğrulayabilmek için sanal ağda iki VM oluşturun. 

[az vm create](/cli/azure/vm) ile bir VM oluşturun. Aşağıdaki örnek, web sunucusu olarak görev yapacak bir VM oluşturur. `--asgs myAsgWebServers` Seçenek neden üyesi sanal makine için oluşturduğu ağ arabirimi için Azure'da *Myvmweb* uygulama güvenlik grubu.

`--nsg ""` Seçeneği, Azure, Azure VM oluştururken oluşturduğu ağ arabirimi için bir varsayılan ağ güvenlik grubu oluşturmasını önlemek için belirtilir. Bu makalede kolaylaştırmak için bir parola kullanılır. Anahtarlar genellikle üretim dağıtımında kullanılır. Anahtarları kullanıyorsanız, SSH aracı iletmeyi kalan adımları yapılandırmanız da gerekir. Daha fazla bilgi için SSH istemcinizin belgelerine bakın. Değiştirin `<replace-with-your-password>` seçtiğiniz parolayla aşağıdaki komutta.

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

Sanal makinenin oluşturulması birkaç dakika sürer. VM oluşturulduktan sonra aşağıdaki örneğe benzer bir çıktı döndürülür: 

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

**publicIpAddress** değerini not alın. Bu adres, bir sonraki adımda internet'ten sanal Makineye erişmek için kullanılır.  Yönetim sunucusu olarak görev yapacak bir VM oluşturun:

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

Sanal makinenin oluşturulması birkaç dakika sürer. VM oluşturulduktan sonra Not **Publicıpaddress** döndürülen çıktı. Bu adres, sonraki adımda sanal Makineye erişmek için kullanılır. Azure VM oluşturma işlemini tamamlayana kadar sonraki adıma geçmeyin.

## <a name="test-traffic-filters"></a>Trafik filtrelerini test etme

İle bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın *myVmMgmt* VM. Değiştirin  *\<Publicıpaddress >* sanal makinenizin genel IP adresiyle. Yukarıdaki örnekte, IP adresidir *13.90.242.231*.

```bash 
ssh azureuser@<publicIpAddress>
```

Parola istendiğinde, girdiğiniz parolayı girin [oluşturduğunuz Vm'lere](#create-virtual-machines).

22 numaralı bağlantı noktasını, Internet'ten gelen izin verildiği için bağlantı başarılı *myAsgMgmtServers* bağlı ağ arabiriminin uygulama güvenlik grubu *myVmMgmt* olur.

SSH oturumu açmak için aşağıdaki komutu kullanın *myVmWeb* VM'den *myVmMgmt* VM:

```bash 
ssh azureuser@myVmWeb
```

Her bir ağ güvenlik grubu içindeki varsayılan güvenlik kuralı bir sanal ağ içindeki tüm IP adresleri arasında tüm bağlantı noktaları üzerinden trafiğe izin verdiği için bağlantı başarılı olur. SSH olamaz *myVmWeb* VM'nin İnternet'ten gelen güvenlik kuralı olmadığından *Myvmweb* bağlantı noktasına izin 22 Internet'ten gelen.

Ngınx web sunucusunu yüklemek için aşağıdaki komutları kullanın *myVmWeb* VM:

```bash 
# Update package source
sudo apt-get -y update

# Install NGINX
sudo apt-get -y install nginx
```

*MyVmWeb* VM nginx varsayılan güvenlik kuralından giden tüm trafiği İnternet'e izin verdiğinden almak için İnternet'e giden izin verilir. Çıkış *myVmWeb* adresindeki bırakan SSH oturumu `username@myVmMgmt:~$` , komut istemi *myVmMgmt* VM. Ngınx Karşılama ekranında alınacak *myVmWeb* VM, aşağıdaki komutu girin:

```bash
curl myVmWeb
```

Oturumunu kapatıp *myVmMgmt* VM. Erişebildiğinizi onaylamak için *myVmWeb* girin, web sunucusuna Azure dışından `curl <publicIpAddress>` kendi bilgisayardan. 80 numaralı bağlantı noktasını, Internet'ten gelen izin verildiği için bağlantı başarılı *Myvmweb* bağlı ağ arabiriminin uygulama güvenlik grubu *myVmWeb* olur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse [az grubu Sil](/cli/azure/group) kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir ağ güvenlik grubu oluşturdunuz ve bir sanal ağ alt ağ ile ilişkilendirilmiş. Ağ güvenlik grupları hakkında daha fazla bilgi edinmek bkz. [Ağ güvenlik grubuna genel bakış](security-overview.md) ve [Ağ güvenlik grubunu yönetme](manage-network-security-group.md).

Azure, varsayılan olarak trafiği alt ağlar arasında yönlendirir. Bunun yerine, alt ağlar arasındaki trafiği, örneğin, güvenlik duvarı olarak görev yapan bir VM aracılığıyla yönlendirmeyi seçebilirsiniz. Bilgi edinmek için bkz. nasıl [yönlendirme tablosu oluşturma](tutorial-create-route-table-cli.md).
