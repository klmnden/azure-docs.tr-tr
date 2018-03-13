---
title: "Birden çok alt - Azure CLI ile bir Azure sanal ağ oluşturma | Microsoft Docs"
description: "Azure CLI kullanarak birden çok alt ağı ile bir sanal ağ oluşturmayı öğrenin."
services: virtual-network
documentationcenter: 
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 0b0bfae02147910d98231d7c93f5ab260f26364f
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2018
---
# <a name="create-a-virtual-network-with-multiple-subnets-using-the-azure-cli"></a>Azure CLI kullanarak birden çok alt ağı ile bir sanal ağ oluşturma

Bir sanal ağ Internet ile ve özel olarak birbirleriyle iletişim kurmak için Azure kaynaklarını çeşitli türleri sağlar. Bir sanal ağ içinde birden fazla alt ağ oluşturmak, böylece filtre uygulayabilirsiniz ağınız segmentlere ayırmak veya denetim alt ağlar arasındaki trafik akışını sağlar. Bu makalede, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Sanal ağ oluşturma
> * Alt ağ oluşturma
> * Sanal makineler arasındaki ağ iletişimi test

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli.md). 

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[az group create](/cli/azure/group#az_group_create) ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

[az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) komutu ile bir sanal ağ oluşturun. Aşağıdaki örnek adlı bir sanal ağ oluşturur *myVirtualNetwork* adres ön ekine sahip *10.0.0.0/16*. Komut adlı bir alt ağı oluşturur *ortak*, adres ön ekine sahip *10.0.0.0/24*.

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefixes 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24
```

Bir konum önceki komutta belirtilmediğinden, Azure sanal ağı aynı konumda oluşturur *myResourceGroup* kaynak grubu bulunmaktadır. **Adres öneklerini** ve **alt ağ öneki** CIDR gösteriminde belirtilir. Belirtilen adres ön eki IP adreslerini 10.0.0.0-10.0.255.254 içerir. Alt ağ için belirtilen önek, sanal ağ için tanımlanan adres ön eki içinde olmalıdır. Azure DHCP, IP adresleri bir alt ağda dağıtılan kaynak ait bir alt ağ adres öneklerini atar. Azure yalnızca atar adresleri 10.0.0.4-10.0.0.254 içinde dağıtılan kaynaklara **ortak** alt ağ, Azure ilk dört adresleri ayırdığından (Bu örnekte alt ağ için 10.0.0.0-10.0.0.3) ve son adres () Bu örnekte alt ağ için 10.0.0.255) her alt ağda.

## <a name="create-a-subnet"></a>Alt ağ oluşturma

Bir alt ağ ile oluşturma [az ağ sanal alt oluşturma](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Aşağıdaki örnek adlı bir alt ağı oluşturur *özel* içinde *myVirtualNetwork* sanal ağ adres ön eki ile *10.0.1.0/24*. Adres ön eki sanal ağ için tanımlanan adres ön eki içinde olmalı ve sanal ağda başka bir alt ağ adresi önek çakışamaz.

```azurecli-interactive 
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name Private \
  --address-prefix 10.0.1.0/24
```

Azure sanal ağlar ve alt ağları üretim kullanımı için dağıtmadan önce kapsamlı bir adres alanıyla öğrenmeniz olduğunu önerilir [konuları](manage-virtual-network.md#create-a-virtual-network) ve [sanal ağ sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Kaynakların alt dağıtıldığında, bazı sanal ağ ve adres aralıkları değiştirme gibi alt ağ değişiklikleri içindeki alt ağlara dağıtılan var olan Azure kaynak çözümünüzün yeniden dağıtımını gerektirebilir.

## <a name="test-network-communication"></a>Test ağ iletişimi

Bir sanal ağ Internet ile ve özel olarak birbirleriyle iletişim kurmak için Azure kaynaklarını çeşitli türleri sağlar. Tek bir sanal ağa dağıttığınız kaynak türü, bir sanal makinedir. İki sanal makine sanal ağ oluşturun, bir sonraki adımda bunları ve Internet arasındaki ağ iletişimi test edebilirsiniz.

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bir sanal makine oluşturma [az vm oluşturma](/cli/azure/vm#az_vm_create). Aşağıdaki örnek, bir sanal makine adlı oluşturur *myVmWeb* içinde *ortak* alt ağ. `--no-wait` Parametresi bir sonraki komuta devam edebilmek için arka planda komutu çalıştırmak Azure sağlar. Bu öğretici kolaylaştırmak için bir parola kullanılır. Anahtarlar genellikle üretim dağıtımında kullanılır. Anahtarları kullanıyorsanız, SSH aracı iletmeyi kalan adımları tamamlamak için yapılandırmanız gerekir. SSH aracı iletmeyi hakkında daha fazla bilgi için SSH istemcisi için belgelere bakın. Değiştir `<replace-with-your-password>` seçtiğiniz bir parola ile aşağıdaki komutu.

```azurecli-interactive
adminPassword="<replace-with-your-password>"

az vm create \
  --resource-group myResourceGroup \
  --name myVmWeb \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Public \
  --admin-username azureuser \
  --admin-password $adminPassword \
  --no-wait
```

Azure otomatik olarak atanan 10.0.0.4 sanal makinenin özel IP adresi 10.0.0.4 ilk kullanılabilir IP adresi olduğundan *ortak* alt ağ. 

Bir sanal makine oluşturmak *özel* alt ağ.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVmMgmt \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Private \
  --admin-username azureuser \
  --admin-password $adminPassword
```
Sanal makine oluşturmak için birkaç dakika sürer. Sanal makine oluşturulduktan sonra Azure CLI çıktı aşağıdaki örneğe benzer şekilde döndürür: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVmMgmt",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.1.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```

Örnek çıktıda, gördüğünüz **privateIpAddress** olan *10.0.1.4*. Oluşturulan azure bir [ağ arabirimi](virtual-network-network-interface.md), sanal makineye bağlı, ağ arabiriminin özel bir IP adresi atanır ve **macAddress**. Azure DHCP otomatik olarak atanmış 10.0.1.4 ağ arabirimine, ilk kullanılabilir IP adresi olduğundan *özel* alt ağ. Ağ arabirimi silinene kadar özel IP ve MAC adreslerinin ağ arabirimine atanmış olarak kalır. 

Not edin **Publicıpaddress**. Bu adres sanal makine bir sonraki adımda Internet'ten erişmek için kullanılır. Bir sanal makine kendisine atanmış bir ortak IP adresi olması gerekli değildir ancak Azure varsayılan oluşturmak her bir sanal makine bir ortak IP adresi atar. Bir sanal makineye Internet üzerinden iletişim kurmak için bir ortak IP adresi sanal makineyi atanması gerekir. Bir ortak IP adresi sanal makineye atanmış olup olmadığına bakılmaksızın tüm sanal makinelerin giden Internet ile iletişim kurabilir. Azure'a giden Internet bağlantıları hakkında daha fazla bilgi için bkz: [azure'da giden bağlantılar](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Bu makalede oluşturulan sanal makineler bir tane [ağ arabirimi](virtual-network-network-interface.md) dinamik olarak ağ arabirimine atanmış bir IP adresine sahip. VM dağıtıldıktan sonra şunları yapabilirsiniz [birden çok ortak ve özel IP adreslerini ekleyin ya da statik IP adresi ataması yöntemi Değiştir](virtual-network-network-interface-addresses.md#add-ip-addresses). Yapabilecekleriniz [ağ arabirimlerini ekleyin](virtual-network-network-interface-vm.md#vm-add-nic), desteklediği sınırına kadar [VM boyutu](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bir sanal makine oluşturduğunuzda seçin. Ayrıca [tek köklü g/ç Sanallaştırması (SR-IOV) etkinleştir](create-vm-accelerated-networking-cli.md) bir VM, ancak yalnızca özelliğini destekleyen bir VM boyutu ile bir VM oluşturun.

### <a name="communicate-between-virtual-machines-and-with-the-internet"></a>Sanal makineler arasında ve internet ile iletişim

İle bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın *myVmMgmt* sanal makine. Değiştir `<publicIpAddress>` sanal makinenizin ortak IP adresine sahip. Önceki örnekte, ortak IP adresidir *13.90.242.231*. Girdiğiniz parola için bir parola istendiğinde, girin [sanal makineler oluşturma](#create-virtual-machines).

```bash 
ssh azureuser@<publicIpAddress>
```

Güvenlik nedenleriyle, sanal makinelere uzaktan sanal bir ağa bağlanabilir sayısını sınırlamak için yaygın bir sorundur. Bu öğreticide *myVmMgmt* yönetmek için kullanılan sanal makine *myVmWeb* sanal ağdaki sanal makine. SSH için aşağıdaki komutu kullanın *myVmWeb* sanal makineden *myVmMgmt* sanal makine:

```bash 
ssh azureuser@myVmWeb
```

İçin iletişim kurmak için *myVmMgmt* sanal makineden *myVmWeb* sanal makine, bir komut isteminden aşağıdaki komutu girin:

```
ping -c 4 myvmmgmt
```

Aşağıdaki örnek çıkış benzer bir çıktı alırsınız:
    
```
PING myvmmgmt.hxehizax3z1udjnrx1r4gr30pg.bx.internal.cloudapp.net (10.0.1.4) 56(84) bytes of data.
64 bytes from 10.0.1.4: icmp_seq=1 ttl=64 time=1.45 ms
64 bytes from 10.0.1.4: icmp_seq=2 ttl=64 time=0.628 ms
64 bytes from 10.0.1.4: icmp_seq=3 ttl=64 time=0.529 ms
64 bytes from 10.0.1.4: icmp_seq=4 ttl=64 time=0.674 ms

--- myvmmgmt.hxehizax3z1udjnrx1r4gr30pg.bx.internal.cloudapp.net ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3029ms
rtt min/avg/max/mdev = 0.529/0.821/1.453/0.368 ms
```
      
Görebilirsiniz adresini *myVmMgmt* sanal makinedir 10.0.1.4. 10.0.1.4 adres aralığı içinde ilk kullanılabilir IP adresi olan *özel* dağıttığınız alt *myVmMgmt* sanal makineye bir önceki adımda.  Sanal makinenin tam etki alanı adı olup olmadığını *myvmmgmt.hxehizax3z1udjnrx1r4gr30pg.bx.internal.cloudapp.net*. Ancak *hxehizax3z1udjnrx1r4gr30pg* etki alanı adı bölümü, sanal makine için farklı olduğundan, etki alanı adını Kalan bölümleri aynıdır. Varsayılan olarak, tüm Azure sanal makineleri varsayılan Azure DNS hizmeti kullanın. Bir sanal ağ içindeki tüm sanal makineleri Azure'nın varsayılan DNS hizmeti ile aynı sanal ağdaki diğer tüm sanal makinelerin adlarını çözümleyebilir. Azure'nın varsayılan DNS hizmeti kullanmak yerine, kendi DNS sunucusu veya Azure DNS hizmeti, özel etki alanı özelliği kullanabilirsiniz. Ayrıntılar için bkz [kendi DNS sunucu kullanılarak ad çözümleme](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) veya [kullanarak Azure DNS özel etki alanları için](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Nginx web sunucusu yüklemek için aşağıdaki komutları kullanın *myVmWeb* sanal makine:

```bash 
# Update package source
sudo apt-get -y update

# Install NGINX
sudo apt-get -y install nginx
```

Nginx yüklendikten sonra kapatmak *myVmWeb* isteminde bırakır SSH oturumu *myVmMgmt* sanal makine. Nginx Karşılama ekranında almak için aşağıdaki komutu girin *myVmWeb* sanal makine.

```bash
curl myVmWeb
```

Nginx Hoş Geldiniz ekranında döndürülür.

İle SSH oturumu kapatmak *myVmMgmt* sanal makine.

Azure oluşturduğunuzda *myVmWeb* sanal makine, adlandırılmış bir ortak IP adresi *myVmWebPublicIP* de oluşturulur ve sanal makineye atanmış. Azure ile atanan Genel Adres Al [az ağ ortak IP Göster](/cli/azure/network/public-ip#az_network_public_ip_show).

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroup \
  --name myVmWebPublicIP \
  --query ipAddress
```

Aşağıdaki kendi bilgisayarınızdan girin komutu, değiştirme `<publicIpAddress>` önceki komuttan döndürülen adresine sahip:

```bash
curl <publicIpAddress>
```

Kendi bilgisayarınızı nginx Karşılama ekranında curl denemesi başarısız olur. Sanal makineler dağıtıldığında, bu Azure varsayılan olarak her bir sanal makine için bir ağ güvenlik grubu oluşturduğundan denemesi başarısız olur. 

Ağ güvenlik grubu izin veren veya reddeden IP adresi ve bağlantı noktasına gelen ve giden ağ trafiğinin güvenlik kuralları içerir. Oluşturulan Azure varsayılan ağ güvenlik grubu, aynı sanal ağdaki kaynakları arasındaki tüm bağlantı noktaları üzerinden iletişim sağlar. Linux sanal makineleri için varsayılan ağ güvenlik grubu tüm gelen trafiği tüm bağlantı noktaları üzerinden Internet'ten engellediği, TCP bağlantı noktası 22 (SSH) kabul edin. Sonuç olarak, varsayılan olarak, aynı zamanda bir SSH oturumu doğrudan oluşturabileceğiniz *myVmWeb* sanal makine Internet'ten gelen değil isteyebilirsiniz olsa bile 22 açık bir web sunucusuna bağlantı noktası. Bu yana `curl` 80 numaralı bağlantı noktası üzerinden trafiğe izin varsayılan ağ güvenlik grubu kural olduğundan komut bağlantı noktası 80 iletişim kurar iletişim Internet'ten başarısız olur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerektiğinde kullanmak [az grubu Sil](/cli/azure/group#az_group_delete) kaynak grubu ve içerdiği kaynakların tümünü kaldırmak için.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir sanal ağ birden fazla alt ağı dağıtmayı öğrendiniz. Ayrıca, Linux sanal makine oluşturduğunuzda, Azure'nın, sanal makineye ekler ve yalnızca trafiğe 22, bağlantı noktası üzerinden Internet'ten izin veren bir ağ güvenlik grubu oluşturur bir ağ arabirimi oluşturur öğrendiniz. Ağ trafiği alt ağlara yerine tek tek sanal makineleri için filtre öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Alt ağ trafiği filtreleme](./virtual-networks-create-nsg-arm-cli.md)
