---
title: Klasik Azure CLI'de eksiksiz bir Linux ortamı oluşturma | Microsoft Docs
description: Depolama, bir Linux VM, sanal ağ ve alt ağ, yük dengeleyici, bir NIC, genel bir IP ve tüm yönleriyle Azure Klasik CLI kullanarak bir ağ güvenlik grubu oluşturun.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 4ba4060b-ce95-4747-a735-1d7c68597a1a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/09/2017
ms.author: cynthn
ms.openlocfilehash: 04c1d69fc46b9a918038e93c4fc56681f225d365
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60328723"
---
# <a name="create-a-complete-linux-environment-with-the-azure-classic-cli"></a>Klasik Azure CLI'de eksiksiz bir Linux ortamı oluşturma
Bu makalede, bir yük dengeleyici ve bir çift geliştirme ve basit bilgi işlem için yararlı olan Vm'leri içeren basit bir ağ ekleriz. İki çalışma, güvenli, gelen herhangi bir Internet'te bağlanabileceği sanal makineleri bulunana kadar komutu komut sürecinde inceleyeceğiz. Ardından, daha karmaşık ağlar ve ortam geçebilirsiniz.

Bu doğrultuda, bağımlılık hiyerarşi, Resource Manager dağıtım modeli sağlar ve bu hakkında ne kadar güç sağlar öğrenin. Sistemin nasıl oluşturulduğunu gördükten sonra bu çok daha hızlı bir şekilde kullanarak yeniden oluşturabilirsiniz [Azure Resource Manager şablonları](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Ortamınızı bölümlerini nasıl bir araya getireceğinizi öğrendikten sonra Ayrıca bunları otomatik hale getirmek için şablonları oluşturma daha kolay olur.

Ortam içerir:

* İki sanal makine bir kullanılabilirlik kümesi içinde.
* Bir yük dengeleyici ile bağlantı noktası 80 üzerinde bir yük dengeleyici kuralı.
* İstenmeyen trafiği, sanal Makineyi korumak için ağ güvenlik grubu (NSG) kuralları.

Bu özel ortamımı oluşturmak için en gereken [Klasik Azure CLI'yı](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Resource Manager modunda (`azure config mode arm`). Ayrıca bir JSON ayrıştırma Aracı gerekir. Bu örnekte [jq](https://stedolan.github.io/jq/).


## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri
Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- [Azure Klasik CLI](#quick-commands) : Klasik ve kaynak yönetimi dağıtım modellerine (Bu makale) yönelik CLI'mız
- [Azure CLI](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -müşterilerimize yönelik yeni nesil CLI kaynak yönetimi dağıtım modeline


## <a name="quick-commands"></a>Hızlı komutlar
Hızlı bir şekilde, aşağıdaki bölümde ayrıntıları temel görevi gerekiyorsa, Azure'a bir VM'yi karşıya yükleme komutları. Bilgi ve içerik her adımda başlangıç belgenin geri kalanında bulunabilir ayrıntılı [burada](#detailed-walkthrough).

Sahip olduğunuzdan emin olun [Azure Klasik CLI](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oturum açmış ve Resource Manager moduna kullanarak:

```azurecli
azure config mode arm
```

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında `myResourceGroup`, `mystorageaccount`, ve `myVM`.

Kaynak grubunu oluşturun. Aşağıdaki örnek `westeurope` konumunda `myResourceGroup` adlı bir kaynak grubu oluşturur:

```azurecli
azure group create -n myResourceGroup -l westeurope
```

JSON ayrıştırıcı kullanarak kaynak grubunu doğrulayın:

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Depolama hesabı oluşturun. Aşağıdaki örnekte adlı bir depolama hesabı oluşturur `mystorageaccount`. (Depolama hesabı adı benzersiz olmalıdır; bu nedenle kendi benzersiz bir ad sağlayın.)

```azurecli
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

Depolama hesabı, JSON ayrıştırıcı kullanarak doğrulayın:

```azurecli
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

Sanal ağı oluşturun. Aşağıdaki örnekte adlı bir sanal ağ oluşturur `myVnet`:

```azurecli
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

Bir alt ağ oluşturun. Aşağıdaki örnekte adlı bir alt ağ oluşturulmaktadır `mySubnet`:

```azurecli
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

Sanal ağ ve alt JSON ayrıştırıcı kullanarak doğrulayın:

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Bir genel IP oluşturun. Aşağıdaki örnekte adlı bir genel IP oluşturur `myPublicIP` DNS adıyla `mypublicdns`. (DNS adı benzersiz olmalıdır; bu nedenle kendi benzersiz bir ad sağlayın.)

```azurecli
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

Yük Dengeleyiciyi oluşturun. Aşağıdaki örnekte adlı bir yük dengeleyici oluşturur `myLoadBalancer`:

```azurecli
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

Yük Dengeleyici için ön uç IP havuzu oluşturun ve genel IP ilişkilendirebilirsiniz. Aşağıdaki örnekte adlı bir ön uç IP havuzu oluşturur `mySubnetPool`:

```azurecli
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool
```

Yük Dengeleyici için arka uç IP havuzu oluşturun. Aşağıdaki örnekte adlı bir arka uç IP havuzu oluşturur `myBackEndPool`:

```azurecli
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

SSH gelen ağ adresi çevirisi (NAT) kuralları, yük dengeleyici oluşturun. Aşağıdaki örnek, iki yük dengeleyici kuralı oluşturur. `myLoadBalancerRuleSSH1` ve `myLoadBalancerRuleSSH2`:

```azurecli
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

Web oluşturma gelen NAT kuralları yük dengeleyici için. Aşağıdaki örnekte adlı bir yük dengeleyici kuralı oluşturur `myLoadBalancerRuleWeb`:

```azurecli
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

Yük Dengeleyici durum araştırması oluşturun. Aşağıdaki örnekte adlı bir TCP araştırması oluşturur `myHealthProbe`:

```azurecli
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

Yük Dengeleyici, IP havuzu ve NAT kuralları JSON ayrıştırıcı kullanarak doğrulayın:

```azurecli
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

İlk ağ arabirim kartı (NIC) oluşturun. Değiştirin `#####-###-###` kendi Azure abonelik kimliğinizi bölümleri Aboneliğinizi çıktısında kimliği not ettiğiniz **jq** oluşturmakta olduğunuz kaynakları incelediğinizde. Abonelik Kimliğiniz ile de görüntüleyebilirsiniz `azure account list`.

Aşağıdaki örnekte adlı bir NIC oluşturur `myNic1`:

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

İkinci NIC oluşturma Aşağıdaki örnekte adlı bir NIC oluşturur `myNic2`:

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

JSON ayrıştırıcı kullanarak iki NIC doğrulayın:

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

Ağ güvenlik grubu oluşturun. Aşağıdaki örnekte adlı bir ağ güvenlik grubu oluşturur `myNetworkSecurityGroup`:

```azurecli
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

Ağ güvenlik grubu için iki gelen kuralları ekleyin. Aşağıdaki örnek, iki kural oluşturur `myNetworkSecurityGroupRuleSSH` ve `myNetworkSecurityGroupRuleHTTP`:

```azurecli
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

Ağ güvenlik grubu ve gelen kuralları JSON ayrıştırıcı kullanarak doğrulayın:

```azurecli
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

Ağ güvenlik grubu için iki NIC bağlayın:

```azurecli
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

Kullanılabilirlik kümesi oluşturun. Aşağıdaki örnek, adında bir kullanılabilirlik kümesi oluşturur. `myAvailabilitySet`:

```azurecli
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

İlk Linux VM'nizi oluşturun. Aşağıdaki örnekte adlı bir VM oluşturur `myVM1`:

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM1 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic1 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username azureuser
```

İkinci bir Linux VM oluşturun. Aşağıdaki örnekte adlı bir VM oluşturur `myVM2`:

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM2 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic2 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username azureuser
```

JSON ayrıştırıcının doğrulamak için oluşturulan her şeyi kullanın:

```azurecli
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

Ortamınızı yeni örnekleri hızla yeniden oluşturmak için bir şablonu dışarı aktarın:

```azurecli
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a>Ayrıntılı kılavuz
Her komut ortamınızı oluşturdukça ne yaptığını ayrıntılı adımları açıklanmaktadır. Geliştirme veya üretim için kendi özel ortamlarda oluşturduğunuzda bu kavramlar yararlı olur.

Sahip olduğunuzdan emin olun [Azure Klasik CLI](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oturum açmış ve Resource Manager moduna kullanarak:

```azurecli
azure config mode arm
```

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında `myResourceGroup`, `mystorageaccount`, ve `myVM`.

## <a name="create-resource-groups-and-choose-deployment-locations"></a>Kaynak grupları oluşturun ve dağıtım konumları seçin
Azure kaynak gruplarını yapılandırma bilgilerini ve kaynak dağıtımlarını mantıksal yönetimini etkinleştirmek için meta verileri içeren mantıksal dağıtım varlıklardır. Aşağıdaki örnek `westeurope` konumunda `myResourceGroup` adlı bir kaynak grubu oluşturur:

```azurecli
azure group create --name myResourceGroup --location westeurope
```

Çıkış:

```azurecli                        
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westeurope
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
Eklemek istediğiniz herhangi bir ek veri diskleri ve sanal makine diskleriniz için depolama hesaplarının gerekir. Kaynak grupları oluşturma sonra hemen hemen depolama hesapları oluşturun.

Burada kullandığımız `azure storage account create` komutu, hesap, kaynak grubunun konumunu, geçirme ve istediğiniz depolama desteği türünü denetler. Aşağıdaki örnekte adlı bir depolama hesabı oluşturur `mystorageaccount`:

```azurecli
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

Çıkış:

```azurecli
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

Kaynak grubumuz kullanarak incelemek için `azure group show` komutu, kullanalım [jq](https://stedolan.github.io/jq/) ile birlikte aracı `--json` Azure CLI seçeneği. (Kullanabilirsiniz **jsawk** veya JSON ayrıştırmak için tercih ettiğiniz herhangi bir dil kitaplığı.)

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Çıkış:

```json
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

CLI kullanarak depolama hesabı araştırmak için öncelikle adları ve anahtarları ayarlamak gerekir. Aşağıdaki örnekte bir depolama hesabı adı, seçtiğiniz bir adı ile değiştirin:

```bash
export AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

Ardından, depolama bilgilerini kolayca görüntüleyebilirsiniz:

```azurecli
azure storage container list
```

Çıkış:

```azurecli
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a>Sanal ağ ve alt ağ oluşturma
Sonraki Vm'lerinizi oluşturabileceğiniz bir alt ağ ve Azure ile çalışan bir sanal ağ oluşturmak ihtiyacınız olacak. Aşağıdaki örnekte adlı bir sanal ağ oluşturur `myVnet` ile `192.168.0.0/16` adres ön eki:

```azurecli
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16
```

Çıkış:

```azurecli
info:    Executing command network vnet create
+ Looking up virtual network "myVnet"
+ Creating virtual network "myVnet"
+ Loading virtual network state
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet
data:    Name                            : myVnet
data:    Type                            : Microsoft.Network/virtualNetworks
data:    Location                        : westeurope
data:    ProvisioningState               : Succeeded
data:    Address prefixes:
data:      192.168.0.0/16
info:    network vnet create command OK
```

Yeniden--json seçeneğini kullanalım `azure group show` ve `jq` nasıl kaynaklarımızın oluşturmakta olduğumuz görmek için. Artık bir `storageAccounts` kaynak ve `virtualNetworks` kaynak.  

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Çıkış:

```json
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
      "name": "myVnet",
      "type": "virtualNetworks",
      "location": "westeurope",
      "tags": null
    },
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Artık bir alt ağda oluşturalım `myVnet` sanal ağ, VM dağıtılır. Kullandığımız `azure network vnet subnet create` zaten oluşturduk kaynakların yanı sıra, komut: `myResourceGroup` kaynak grubu ve `myVnet` sanal ağ. Aşağıdaki örnekte adlı alt ağ eklediğimiz `mySubnet` alt ağ adres ön eki ile `192.168.1.0/24`:

```azurecli
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

Çıkış:

```azurecli
info:    Executing command network vnet subnet create
+ Looking up the subnet "mySubnet"
+ Creating subnet "mySubnet"
+ Looking up the subnet "mySubnet"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : mySubnet
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

Alt ağ mantıksal olarak bir sanal ağ içinde olduğundan, biz biraz farklı bir komutla alt ağ bilgilerini arayın. Kullandığımız komut `azure network vnet show`, ancak kullanarak JSON çıkışında incelemek devam ediyoruz `jq`.

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Çıkış:

```json
{
  "subnets": [
    {
      "ipConfigurations": [],
      "addressPrefix": "192.168.1.0/24",
      "provisioningState": "Succeeded",
      "name": "mySubnet",
      "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
    }
  ],
  "tags": {},
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "provisioningState": "Succeeded",
  "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "name": "myVnet",
  "location": "westeurope"
}
```

## <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma
Şimdi biz atadığınız yük dengeleyicinizin genel IP adresi (PIP) oluşturalım. Vm'lerinizi kullanarak Internet'ten bağlanmanızı sağlayan `azure network public-ip create` komutu. Varsayılan Adres dinamik olduğundan, adlandırılmış bir DNS girişi oluştururuz **cloudapp.azure.com** kullanarak etki alanı `--domain-name-label` seçeneği. Aşağıdaki örnekte adlı bir genel IP oluşturur `myPublicIP` DNS adıyla `mypublicdns`. DNS adının benzersiz olması gerektiğinden, kendi benzersiz bir DNS adı sağlayın:

```azurecli
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

Çıkış:

```azurecli
info:    Executing command network public-ip create
+ Looking up the public ip "myPublicIP"
+ Creating public ip address "myPublicIP"
+ Looking up the public ip "myPublicIP"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
data:    Name                            : myPublicIP
data:    Type                            : Microsoft.Network/publicIPAddresses
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Allocation method               : Dynamic
data:    Idle timeout                    : 4
data:    Domain name label               : mypublicdns
data:    FQDN                            : mypublicdns.westeurope.cloudapp.azure.com
info:    network public-ip create command OK
```

İle görebilmeniz için genel IP adresini de düzey bir kaynakla olan `azure group show`.

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Çıkış:

```json
{
"tags": {},
"id": "/subscriptions/guid/resourceGroups/myResourceGroup",
"name": "myResourceGroup",
"provisioningState": "Succeeded",
"location": "westeurope",
"properties": {
    "provisioningState": "Succeeded"
},
"resources": [
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "name": "myPublicIP",
    "type": "publicIPAddresses",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
    "name": "myVnet",
    "type": "virtualNetworks",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "name": "mystorageaccount",
    "type": "storageAccounts",
    "location": "westeurope",
    "tags": null
    }
],
"permissions": [
    {
    "actions": [
        "*"
    ],
    "notActions": []
    }
]
}
```

Tam kullanarak ve alt etki alanının tam etki alanı adını (FQDN) da dahil olmak üzere daha fazla kaynak ayrıntıları araştırabilir `azure network public-ip show` komutu. Genel IP adresi kaynağı mantıksal olarak ayrıldı, ancak belirli bir adres değil henüz atanmamış. Bir IP adresi elde etmek için değil henüz oluşturduk bir yük dengeleyici ihtiyacınız olacak.

```azurecli
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

Çıkış:

```json
{
"tags": {},
"publicIpAllocationMethod": "Dynamic",
"dnsSettings": {
    "domainNameLabel": "mypublicdns",
    "fqdn": "mypublicdns.westeurope.cloudapp.azure.com"
},
"idleTimeoutInMinutes": 4,
"provisioningState": "Succeeded",
"etag": "W/\"c63154b3-1130-49b9-a887-877d74d5ebc5\"",
"id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
"name": "myPublicIP",
"location": "westeurope"
}
```

## <a name="create-a-load-balancer-and-ip-pools"></a>Bir yük dengeleyici ve IP havuzları oluşturma
Bir yük dengeleyici oluşturduğunuzda, trafiği birden çok VM'ye dağıtmasını sağlar. Ayrıca bakım ya da ağır yük olması durumunda kullanıcı isteklerine yanıt birden fazla VM çalıştırarak uygulamanıza yedeklilik sağlar. Aşağıdaki örnekte adlı bir yük dengeleyici oluşturur `myLoadBalancer`:

```azurecli
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

Çıkış:

```azurecli
info:    Executing command network lb create
+ Looking up the load balancer "myLoadBalancer"
+ Creating load balancer "myLoadBalancer"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer
data:    Name                            : myLoadBalancer
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```

Şimdi bazı IP havuzlarını oluşturmak oldukça boş bizim yük dengeleyicidir. Biri ön uç diğeri arka ucu için yük dengeleyici için iki IP havuzları oluşturmak istiyoruz. Ön uç IP havuzu herkese görünür. Bu da size daha önce oluşturduğumuz PIP atama konumdur. Ardından arka uç havuzu için sanal makinelerimiz konumu olarak bağlanmak için kullanırız. Bu şekilde, trafiğin vm'lere yük dengeleyici üzerinden akabilir.

İlk olarak, bizim ön uç IP havuzu oluşturalım. Aşağıdaki örnekte adlı bir ön uç havuzu oluşturur `myFrontEndPool`:

```azurecli
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool
```

Çıkış:

```azurecli
info:    Executing command network lb frontend-ip create
+ Looking up the load balancer "myLoadBalancer"
+ Looking up the public ip "myPublicIP"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
info:    network lb mySubnet-ip create command OK
```

Nasıl kullandığımız Not `--public-ip-name` geçirin geç `myPublicIP` daha önce oluşturduğumuz. Yük dengeleyiciye genel IP adresi atayarak sanal makinelerinize Internet üzerinden erişmek sağlar.

Ardından, bizim ikinci IP havuzu bizim arka uca trafik için bu kez oluşturalım. Aşağıdaki örnekte adlı bir arka uç havuzu oluşturur `myBackEndPool`:

```azurecli
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

Çıkış:

```azurecli
info:    Executing command network lb address-pool create
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

Bizim yük dengeleyici ile bakarak nasıl çalıştığını görebiliriz `azure network lb show` JSON çıkışını inceleyerek:

```azurecli
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

Çıkış:

```json
{
  "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [],
  "probes": []
}
```

## <a name="create-load-balancer-nat-rules"></a>NAT kuralları yük dengeleyici oluşturma
Bizim yük dengeleyici üzerinden akan trafiği almak için şu ağ adresi çevirisi (NAT) kuralları, gelen veya giden eylemleri belirtin oluşturmanız gerekir. Kullanılacak protokol belirtin, ardından istediğiniz gibi iç bağlantı noktaları için dış bağlantı noktalarını eşleme. Ortamımız için SSH aracılığıyla Vm'lerimiz, şimdiye kadarki bizim yük dengeleyiciye izin veren bazı kurallar oluşturalım. TCP bağlantı noktası 22 üzerinde (Bu, daha sonra oluştururuz) sanal makinelerimiz yönlendirmek için TCP bağlantı noktalarını 4222 ve 4223 ayarladık. Aşağıdaki örnekte adlı bir kural oluşturur `myLoadBalancerRuleSSH1` TCP bağlantı noktası 4222 22 numaralı bağlantı noktasına eşlemek için:

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

Çıkış:

```azurecli
info:    Executing command network lb inbound-nat-rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default enable floating ip: false
warn:    Using default idle timeout: 4
warn:    Using default mySubnet IP configuration "myFrontEndPool"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleSSH1
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 4222
data:    Backend port                    : 22
data:    Enable floating IP              : false
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
info:    network lb inbound-nat-rule create command OK
```

Yordamı, ikinci NAT kuralı için SSH için yineleyin. Aşağıdaki örnekte adlı bir kural oluşturur `myLoadBalancerRuleSSH2` TCP bağlantı noktası 4223 22 numaralı bağlantı noktasına eşlemek için:

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

Şimdi de devam edin ve kural bizim kadar IP havuzları takma web trafiği için TCP bağlantı noktası 80 için bir NAT kuralı oluşturun. Biz kural Vm'lerimiz, şimdiye kadarki kuralı için ayrı ayrı olaylara yerine bir IP havuzuna bağlama, biz ekleyebilir veya Vm'leri IP havuzundan kaldırabilirsiniz. Yük Dengeleyici trafik akışını otomatik olarak ayarlar. Aşağıdaki örnekte adlı bir kural oluşturur `myLoadBalancerRuleWeb` TCP bağlantı noktası 80 80 numaralı bağlantı noktasına eşlemek için:

```azurecli
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

Çıkış:

```azurecli
info:    Executing command network lb rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default idle timeout: 4
warn:    Using default enable floating ip: false
warn:    Using default load distribution: Default
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleWeb
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 80
data:    Backend port                    : 80
data:    Enable floating IP              : false
data:    Load distribution               : Default
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
data:    Backend address pool id         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
info:    network lb rule create command OK
```

## <a name="create-a-load-balancer-health-probe"></a>Yük dengeleyici durum yoklaması oluşturma
Bir sistem durumu araştırma düzenli aralıklarla vm'lerde çalışan ve tanımlanan isteklere yanıt emin olmak için yük dengeleyicinin arkasında olduklarından denetler. Aksi durumda, kullanıcılar bunları yönlendirilmesini değil emin olmak için işlemi kaldırıldı. Aralık ve zaman aşımı değerlerini yanı sıra sistem durumu araştırması için özel denetimler tanımlayabilirsiniz. Sistem durumu araştırmaları hakkında daha fazla bilgi için bkz: [Load Balancer araştırmaları](../../load-balancer/load-balancer-custom-probe-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Aşağıdaki örnek, bir TCP oluşturur. sistem durumu araştırıldığı adlandırılmış `myHealthProbe`:

```azurecli
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

Çıkış:

```azurecli
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myHealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

Burada, bizim sistem durumu denetimleri için 15 saniyelik bir aralıkta belirtilen. Biz, en fazla dört araştırmaları (yük dengeleyici konak artık çalışıp çalışmadığını göz önünde bulundurur. önce bir dakika) eksik.

## <a name="verify-the-load-balancer"></a>Yük Dengeleyici doğrulayın
Artık yük dengeleyici yapılandırmasında gerçekleştirilir. Gerçekleştirdiğiniz adımlar şunlardır:

1. Bir yük dengeleyici oluşturdunuz.
2. Ön uç IP havuzu oluşturduğunuz ve bir genel IP atanmış.
3. VM'ler bağlanabilir bir arka uç IP havuzu oluşturduğunuz.
4. Yönetim için web uygulamamız için TCP bağlantı noktası 80 izin verecek bir kural yanı sıra VM'ler için SSH izin veren NAT kuralları oluşturduğunuz.
5. Düzenli aralıklarla Vm'leri denetlemek için durum araştırması eklediğiniz. Bu durum araştırması, kullanıcılar artık çalışmıyor veya içerik sunan bir VM'ye erişmenin çalışmayın sağlar.

Load balancer'ınız şimdi gibi göründüğünü inceleyelim:

```azurecli
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

Çıkış:

```json
{
  "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH1",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4222,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    },
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH2",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4223,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    }
  ],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "inboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        },
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleWeb",
      "provisioningState": "Succeeded",
      "enableFloatingIP": false,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "backendAddressPool": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
      },
      "protocol": "Tcp",
      "loadDistribution": "Default",
      "mySubnetPort": 80,
      "backendPort": 80,
      "idleTimeoutInMinutes": 4
    }
  ],
  "probes": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myHealthProbe",
      "provisioningState": "Succeeded",
      "numberOfProbes": 4,
      "intervalInSeconds": 15,
      "port": 80,
      "protocol": "Tcp",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/probes/myHealthProbe"
    }
  ]
}
```

## <a name="create-an-nic-to-use-with-the-linux-vm"></a>Linux VM ile kullanmak için bir NIC oluşturup
NIC program aracılığıyla kullanılabilir olduklarından kullanımları kuralları uygulayabilirsiniz. Birden fazla olabilir. Aşağıdaki `azure network nic create` komutu, NIC yük arka uç IP havuzu için bağlama ve SSH trafiğine izin vermek için NAT kuralıyla ilişkilendirin.

Değiştirin `#####-###-###` kendi Azure abonelik kimliğinizi bölümleri Aboneliğinizi çıktısında kimliği not ettiğiniz `jq` oluşturmakta olduğunuz kaynakları incelediğinizde. Abonelik Kimliğiniz ile de görüntüleyebilirsiniz `azure account list`.

Aşağıdaki örnekte adlı bir NIC oluşturur `myNic1`:

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

Çıkış:

```azurecli
info:    Executing command network nic create
+ Looking up the subnet "mySubnet"
+ Looking up the network interface "myNic1"
+ Creating network interface "myNic1"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1
data:    Name                            : myNic1
data:    Type                            : Microsoft.Network/networkInterfaces
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Enable IP forwarding            : false
data:    IP configurations:
data:      Name                          : Nic-IP-config
data:      Provisioning state            : Succeeded
data:      Private IP address            : 192.168.1.4
data:      Private IP allocation method  : Dynamic
data:      Subnet                        : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:      Load balancer backend address pools:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
data:      Load balancer inbound NAT rules:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
data:
info:    network nic create command OK
```

Kaynak doğrudan inceleyerek ayrıntılarını görebilirsiniz. Kullanarak kaynak inceleyin `azure network nic show` komutu:

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

Çıkış:

```json
{
  "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
  "provisioningState": "Succeeded",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1",
  "name": "myNic1",
  "type": "Microsoft.Network/networkInterfaces",
  "location": "westeurope",
  "ipConfigurations": [
    {
      "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/Nic-IP-config",
      "loadBalancerBackendAddressPools": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
        }
      ],
      "loadBalancerInboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        }
      ],
      "privateIPAddress": "192.168.1.4",
      "privateIPAllocationMethod": "Dynamic",
      "subnet": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
      },
      "provisioningState": "Succeeded",
      "name": "Nic-IP-config"
    }
  ],
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": []
  },
  "enableIPForwarding": false,
  "resourceGuid": "a20258b8-6361-45f6-b1b4-27ffed28798c"
}
```

Şimdi ikinci NIC takma yeniden bizim için arka uç IP havuzu içinde oluşturun. Bu zaman ikinci NAT kuralı SSH trafiğine izin verir. Aşağıdaki örnekte adlı bir NIC oluşturur `myNic2`:

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a>Bir ağ güvenlik grubu ve kuralları oluşturma
Şimdi biz bir ağ güvenlik grubu ve NIC erişimi yöneten gelen kuralları oluşturma Bir ağ güvenlik grubu, bir NIC veya alt ağ için uygulanabilir. Sanal makinelerinizin içine ve dışına trafik akışını denetlemek için kurallar tanımlar. Aşağıdaki örnekte adlı bir ağ güvenlik grubu oluşturur `myNetworkSecurityGroup`:

```azurecli
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

Şimdi gelen (SSH desteklemek için) 22 numaralı bağlantı noktasında gelen bağlantılara izin verecek bir NSG kuralı ekleyin. Aşağıdaki örnekte adlı bir kural oluşturur `myNetworkSecurityGroupRuleSSH` bağlantı noktası 22 üzerinde TCP'ye izin vermek için:

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

Şimdi gelen (web trafiğini desteklemek için), 80 numaralı bağlantı noktasında gelen bağlantılara izin verecek bir NSG kuralı ekleyelim. Aşağıdaki örnekte adlı bir kural oluşturur `myNetworkSecurityGroupRuleHTTP` bağlantı noktası 80 üzerinde TCP'ye izin vermek için:

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [!NOTE]
> Gelen kuralı, gelen ağ bağlantıları için bir filtredir. Bu örnekte, bağlantı noktası 22 yapılan tüm istekleri aracılığıyla NIC'ye bizim VM'de geçirildiğini anlamına gelir Vm'leri sanal NIC için NSG bağlayın. Bu gelen kuralı bir ağ bağlantısı hakkında ve bir uç nokta hakkında hangi hakkında Klasik dağıtımlarda olması. Bir bağlantı noktasını açmak için bırakmalıdır `--source-port-range` kümesine '\*' (gelen gelen istekleri kabul etmek için varsayılan değer) **herhangi** bağlantı noktası isteniyor. Bağlantı noktaları genellikle dinamiktir.
>
>

## <a name="bind-to-the-nic"></a>NIC'ye bağlama
NIC'leri için NSG bağlayın. Bizim NIC bizim ağ güvenlik grubu ile bağlanmak ihtiyacımız var. Hem bizim NIC yeteneklerinizi bağlı olarak, her iki komutu çalıştırın:

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a>Kullanılabilirlik kümesi oluşturma
Sanal makinelerinizin hata etki alanları ve yükseltme etki alanları arasında Yardım yayılan kullanılabilirlik kümeleri. Bir kullanılabilirlik kümesindeki sanal makineleriniz için oluşturalım. Aşağıdaki örnek, adında bir kullanılabilirlik kümesi oluşturur. `myAvailabilitySet`:

```azurecli
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

Hata etki alanları ortak bir güç kaynağı ve ağ anahtarını paylaşan sanal makinelerin bir gruplandırma tanımlayın. Varsayılan olarak, kullanılabilirlik kümenizde yapılandırılmış olan sanal makineler en fazla üç hata etki alanları arasında ayrılır. Bu hata etki alanlarına birinde bir donanım sorunu, uygulamanızı çalıştıran her VM etkilemez olur. Azure otomatik olarak VM'ler hata etki alanlarında bunları bir kullanılabilirlik kümesine yerleştirdiğinizde dağıtır.

Yükseltme etki alanları, sanal makineler ve aynı anda yeniden başlatılabilecek temel alınan fiziksel donanımları grupları belirtin. Yükseltme etki alanları yeniden başlatıldığı sırada planlanan bakım sırasında sıralı olmayabilir, ancak yalnızca bir yükseltme aynı anda yeniden başlatılır. Yeniden Azure otomatik olarak sanal makinelerinizin yükseltme etki alanları arasında bunları bir kullanılabilirlik alanında yerleştirirken dağıtır.

Daha fazla bilgi edinin [VM kullanılabilirliğini yönetme](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="create-the-linux-vms"></a>Linux VM oluşturma
İnternet'ten erişilebilen Vm'leri desteklemek için depolama ve ağ kaynakları oluşturdunuz. Şimdi şimdi bunları VM'ler oluşturun ve bunları güvenli bir parola sahip olmayan bir SSH anahtarı. Bu durumda, bir Ubuntu VM üzerinde en son LTS tabanlı oluşturmak için ekleyeceğiz. Biz kullanarak bu görüntü bilgilerini bulma `azure vm image list`anlatılan şekilde [Azure VM görüntüleri bulma](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Komutunu kullanarak görüntüyü seçtik `azure vm image list westeurope canonical | grep LTS`. Bu durumda, kullandığımız `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`. Son alan için biz geçirmek `latest` böylece en son derlemenin her zaman gelecekteki aldığımız. (Kullandığımız dize `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).

Bu sonraki adım zaten oluşturulmuş herkese tanıdık bir ssh rsa ortak ve özel anahtar pair Linux veya Mac kullanarak **ssh-keygen - t rsa -b 2048**. Tüm sertifika anahtar çiftleri varsa değil, `~/.ssh` dizini oluşturup bunları:

* Kullanarak otomatik olarak `azure vm create --generate-ssh-keys` seçeneği.
* Kullanarak el ile [kendiniz oluşturmanız için yönergeleri](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Alternatif olarak, `--admin-password` VM oluşturulduktan sonra SSH bağlantıların kimliğini doğrulamak için yöntem. Bu yöntem genellikle daha az güvenlidir.

Tüm kaynaklar ve bilgi ile birlikte taşıyarak VM'yi oluştururuz `azure vm create` komutu:

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM1 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic1 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username azureuser
```

Çıkış:

```azurecli
info:    Executing command vm create
+ Looking up the VM "myVM1"
info:    Verifying the public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account mystorageaccount
+ Looking up the availability set "myAvailabilitySet"
info:    Found an Availability set "myAvailabilitySet"
+ Looking up the NIC "myNic1"
info:    Found an existing NIC "myNic1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet" in the NIC "myNic1"
info:    This is an NIC without publicIP configured
info:    The storage URI 'https://mystorageaccount.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by the parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

Varsayılan SSH anahtarlarınızı kullanarak sanal makinenizde hemen bağlanabilir. Biz, yük dengeleyici üzerinden geçirme beri uygun bağlantı noktasını belirttiğinizden emin olun. (Bizim ilk VM için NAT kuralı bağlantı noktası 4222 bizim VM iletecek şekilde ayarladık.)

```bash
ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

Çıkış:

```bash
The authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    https://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

Devam edin ve aynı şekilde, ikinci bir sanal makine oluşturun:

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM2 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic2 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username azureuser
```

Ve artık `azure vm show myResourceGroup myVM1` oluşturduğunuz özellikleri incelemek için komutu. Bu noktada, Ubuntu Vm'leri bir yük dengeleyicinin arkasına (parolaları devre dışı bırakıldığı için) yalnızca SSH anahtar çiftinizi ile oturum Azure'da çalıştırıyorsunuz. Ngınx veya httpd yükleyin, bir web uygulaması dağıtın ve trafiği de görmenize hem sanal makinelerin yük dengeleyici üzerinden akış.

```azurecli
azure vm show --resource-group myResourceGroup --name myVM1
```

Çıkış:

```azurecli
info:    Executing command vm show
+ Looking up the VM "TestVM1"
+ Looking up the NIC "myNic1"
data:    Id                              :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Succeeded
data:    Name                            :myVM1
data:    Location                        :westeurope
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :16.04.0-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clib45a8b650f4428a1-os-1471973896525
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/clib45a8b650f4428a1-os-1471973896525.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-24-D4-AA
data:          Provisioning State        :Succeeded
data:          Name                      :LmyNic1
data:          Location                  :westeurope
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://mystorageaccount.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm show command OK

```


## <a name="export-the-environment-as-a-template"></a>Ortamı şablon olarak dışarı aktarma
Ne bu ortamını geliştirdim, aynı parametreleri ya da eşleşecek bir üretim ortamında bir ek geliştirme ortamında oluşturmak istiyorsunuz? Resource Manager, ortamınız için tüm parametreleri tanımlayan JSON şablonlarını kullanır. Tüm ortamları, bu JSON şablonunu başvurarak oluşturun. Yapabilecekleriniz [JSON şablonları el ile derleme](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya sizin için JSON şablonunu oluşturmak için bir ortamı dışarı aktarın:

```azurecli
azure group export --name myResourceGroup
```

Bu komut oluşturur `myResourceGroup.json` geçerli çalışma dizininizde dosya. Bu şablonu kullanarak bir ortam oluşturduğunuzda, yük dengeleyici, ağ arabirimleri ve VM'lerin adlarını da dahil olmak üzere tüm kaynak adları için istenir. Ekleyerek bu adlar, şablon dosyanızda doldurabilirsiniz `-p` veya `--includeParameterDefaultValue` parametresi `azure group export` daha önce gösterilen komutu. Kaynak adlarını belirtmek için JSON şablonunu düzenlemeniz veya [parameters.json dosya oluşturma](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kaynak adları belirtir.

Şablonunuzdan bir ortam oluşturmak için:

```azurecli
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

Okumak isteyebilirsiniz [şablonlardan dağıtma hakkında daha fazla](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Artımlı olarak ortamları güncelleştirebilir, parametre dosyasını kullanın ve tek bir depolama konumundan şablonları erişim hakkında bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar
Artık birden çok ağ bileşenleri ve Vm'leri ile çalışmaya başlamak hazırsınız. Burada sunulan temel bileşenleri kullanarak uygulamanızı oluşturmak için bu örnek ortamı kullanabilirsiniz.
