---
title: Azure CLI 1.0 ile eksiksiz bir Linux ortamı oluşturma | Microsoft Docs
description: Depolama, bir Linux VM, bir sanal ağ ve alt ağ, bir yük dengeleyici, bir NIC, ortak bir IP ve tüm sıfırdan Azure CLI 1.0 kullanarak bir ağ güvenlik grubu oluşturun.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
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
ms.author: iainfou
ms.openlocfilehash: 4a43e138d3497e01fe9e0e5c55a4a66adac767c6
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="create-a-complete-linux-environment-with-the-azure-cli-10"></a>Azure CLI 1.0 ile eksiksiz bir Linux ortamı oluşturma
Bu makalede, bir yük dengeleyici ve geliştirme ve basit bilgi işlem için yararlı olan VM'ler çifti ile basit bir ağ oluşturun. İki çalışma, güvenli Linux, gelen herhangi bir yere Internet'te bağlanabileceği VM'ler kadar biz komut komut işleminde size kılavuzluk. Ardından, daha karmaşık ağlar ve ortamlar geçebilirsiniz.

Yol boyunca bağımlılık hiyerarşi, Resource Manager dağıtım modeli sağlar ve bunu hakkında ne kadar güç sağlar öğrenin. Sistem nasıl yapılandırıldığını gördükten sonra bu çok daha hızlı bir şekilde kullanarak yeniden oluşturmak için kullanabileceğiniz [Azure Resource Manager şablonları](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Ortamınızı bölümlerini nasıl bir araya getireceğinizi öğrenin sonra da, bunları otomatikleştirmek için şablonlar oluşturma daha kolay olur.

Ortam içerir:

* İki VM bir kullanılabilirlik kümesi içinde.
* Yük Dengeleyici Yük Dengeleme kuralı bağlantı noktası 80 ile.
* VM gelen istenmeyen trafiği korumak için ağ güvenlik grubu (NSG) kuralları.

Bu özel ortam oluşturmak için en son gerekir [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Kaynak Yöneticisi modunda (`azure config mode arm`). Ayrıca aracı ayrıştırma JSON gerekir. Bu örnekte [jq](https://stedolan.github.io/jq/).


## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri
Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- [Azure CLI 1.0](#quick-commands) – bizim CLI Klasik ve kaynak yönetimi dağıtım modeline (Bu makalede)
- [Azure CLI 2.0](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız


## <a name="quick-commands"></a>Hızlı komutlar
Bir VM için Azure karşıya yüklemek için hızlı bir şekilde, aşağıdaki bölümde ayrıntıları temel görevi gerekiyorsa komutları. Her adım başlangıç belgenin geri kalanı bulunabilir bilgi ve içerik daha ayrıntılı [burada](#detailed-walkthrough).

Bilgisayarınızda yüklü olduğundan emin olun [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oturum açmış ve Resource Manager modunu kullanma:

```azurecli
azure config mode arm
```

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında `myResourceGroup`, `mystorageaccount`, ve `myVM`.

Kaynak grubunu oluşturun. Aşağıdaki örnek, bir kaynak grubu oluşturur `myResourceGroup` içinde `westeurope` konumu:

```azurecli
azure group create -n myResourceGroup -l westeurope
```

Kaynak Grup, JSON ayrıştırıcısı kullanarak doğrulayın:

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Depolama hesabı oluşturun. Aşağıdaki örnek adlı bir depolama hesabı oluşturur `mystorageaccount`. (Depolama hesabı adı gerekir benzersiz olmalıdır, böylece kendi benzersiz bir ad sağlayın.)

```azurecli
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

Depolama hesabı JSON ayrıştırıcısı kullanarak doğrulayın:

```azurecli
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

Sanal ağı oluşturun. Aşağıdaki örnek adlı bir sanal ağ oluşturur `myVnet`:

```azurecli
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

Bir alt ağ oluşturun. Aşağıdaki örnek adlı bir alt ağı oluşturur `mySubnet`:

```azurecli
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

Sanal ağ ve alt JSON ayrıştırıcısı kullanarak doğrulayın:

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Genel IP oluşturun. Aşağıdaki örnek adlı ortak IP oluşturur `myPublicIP` DNS adı ile `mypublicdns`. (DNS adı gerekir benzersiz olmalıdır, böylece kendi benzersiz bir ad sağlayın.)

```azurecli
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

Yük Dengeleyici oluşturun. Aşağıdaki örnek, adlandırılmış bir yük dengeleyici oluşturur `myLoadBalancer`:

```azurecli
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

Yük Dengeleyici için bir ön uç IP havuzu oluşturun ve genel IP ilişkilendirebilirsiniz. Aşağıdaki örnek adlı bir ön uç IP havuzu oluşturur `mySubnetPool`:

```azurecli
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool
```

Yük Dengeleyici için arka uç IP havuzu oluşturun. Aşağıdaki örnek adlı bir arka uç IP havuzu oluşturur `myBackEndPool`:

```azurecli
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

SSH gelen ağ adresi çevirisi (NAT) kuralları yük dengeleyici için oluşturun. Aşağıdaki örnek, iki yük dengeleyici kuralı oluşturur `myLoadBalancerRuleSSH1` ve `myLoadBalancerRuleSSH2`:

```azurecli
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

Web oluşturulamıyor gelen NAT kuralları yük dengeleyici için. Aşağıdaki örnek adlı yük dengeleyici kuralı oluşturur `myLoadBalancerRuleWeb`:

```azurecli
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

Yük Dengeleyici durum araştırması oluşturun. Aşağıdaki örnek adlı bir TCP araştırması oluşturur `myHealthProbe`:

```azurecli
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

Yük Dengeleyici, IP havuzları ve NAT kuralları JSON ayrıştırıcısı kullanarak doğrulayın:

```azurecli
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

İlk ağ arabirim kartı (NIC) oluşturun. Değiştir `#####-###-###` bölümleri, kendi Azure aboneliği ID'ye sahip Aboneliğinizi çıktısında kimliği not ettiğiniz **jq** oluşturmakta kaynakları incelediğinizde. Abonelik Kimliğinizle de görüntüleyebilirsiniz `azure account list`.

Aşağıdaki örnek, adlandırılmış bir NIC oluşturur `myNic1`:

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

İkinci NIC oluşturun Aşağıdaki örnek, adlandırılmış bir NIC oluşturur `myNic2`:

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

İki NIC JSON ayrıştırıcısı kullanarak doğrulayın:

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

Ağ güvenlik grubu oluşturun. Aşağıdaki örnek adlı bir ağ güvenlik grubu oluşturur `myNetworkSecurityGroup`:

```azurecli
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

Ağ güvenlik grubu için iki gelen kuralı ekleyin. Aşağıdaki örnek, iki kural oluşturur `myNetworkSecurityGroupRuleSSH` ve `myNetworkSecurityGroupRuleHTTP`:

```azurecli
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

Ağ güvenlik grubu ve gelen kuralları JSON ayrıştırıcısı kullanarak doğrulayın:

```azurecli
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

Ağ güvenlik grubu için iki NIC bağlayın:

```azurecli
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

Kullanılabilirlik kümesi oluşturun. Aşağıdaki örnek, kullanılabilirlik adlandırılmış kümesi oluşturur `myAvailabilitySet`:

```azurecli
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

İlk Linux VM oluşturun. Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur `myVM1`:

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

İkinci Linux VM oluşturun. Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur `myVM2`:

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

JSON ayrıştırıcı doğrulamak için oluşturulmuş her şeyi kullanın:

```azurecli
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

Ortamınızı hızla yeni örnekleri yeniden oluşturmak için bir şablonu dışarı aktarın:

```azurecli
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a>Ayrıntılı kılavuz
Ortamınızı yapı gibi her komutun ne yaptığını ayrıntılı adımları açıklanmaktadır. Bu kavramlar, kendi özel ortamınız için geliştirme veya üretim derlerken faydalıdır.

Bilgisayarınızda yüklü olduğundan emin olun [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oturum açmış ve Resource Manager modunu kullanma:

```azurecli
azure config mode arm
```

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında `myResourceGroup`, `mystorageaccount`, ve `myVM`.

## <a name="create-resource-groups-and-choose-deployment-locations"></a>Kaynak grupları oluşturun ve dağıtım konumları seçin
Azure kaynak gruplarını yapılandırma bilgilerini ve kaynak dağıtımları mantıksal yönetimini etkinleştirmek için meta veriler içeren mantıksal dağıtım varlıklardır. Aşağıdaki örnek, bir kaynak grubu oluşturur `myResourceGroup` içinde `westeurope` konumu:

```azurecli
azure group create --name myResourceGroup --location westeurope
```

Çıktı:

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
Eklemek istediğiniz herhangi bir ek veri disklerinin ve VM diskleriniz için depolama hesapları gerekir. Kaynak grupları oluşturun sonra hemen hemen depolama hesapları oluşturun.

Burada kullandığımız `azure storage account create` komutu, hesap, kaynak grubu konumu geçirme denetler ve istediğiniz depolama destek türü. Aşağıdaki örnek adlı bir depolama hesabı oluşturur `mystorageaccount`:

```azurecli
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

Çıktı:

```azurecli
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

Bizim kaynak grubu kullanarak incelemek için `azure group show` komutu, kullanalım [jq](https://stedolan.github.io/jq/) ile birlikte aracı `--json` Azure CLI seçeneği. (Kullanabileceğiniz **jsawk** veya JSON ayrıştırmak için tercih ettiğiniz herhangi bir dil kitaplığı.)

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Çıktı:

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

CLI kullanarak depolama hesabına araştırmak için ilk hesap adlarını ve anahtarları ayarlamanız gerekir. Aşağıdaki örnekte depolama hesabı adı seçtiğiniz bir adla değiştirin:

```bash
export AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

Ardından, depolama bilgilerinizi kolayca görüntüleyebilirsiniz:

```azurecli
azure storage container list
```

Çıktı:

```azurecli
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a>Sanal ağ ve alt ağ oluşturma
Sonraki Azure ve Vm'lerinizi oluşturabileceğiniz bir alt ağ içinde çalışan bir sanal ağ oluşturmak ihtiyacınız olacak. Aşağıdaki örnek adlı bir sanal ağ oluşturur `myVnet` ile `192.168.0.0/16` adres öneki:

```azurecli
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16
```

Çıktı:

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

Yeniden--json seçeneği kullanalım `azure group show` ve `jq` nasıl KAYNAKLARIMIZI oluşturmakta olduğumuz görmek için. Şimdi sahip olduğumuz bir `storageAccounts` kaynak ve `virtualNetworks` kaynak.  

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Çıktı:

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

Şimdi bir alt ağda oluşturalım `myVnet` sanal ağ, sanal makineleri dağıtılır. Kullanırız `azure network vnet subnet create` zaten oluşturduğumuz kaynakları birlikte komutu: `myResourceGroup` kaynak grubu ve `myVnet` sanal ağ. Aşağıdaki örnekte, eklediğimiz adlı alt ağın `mySubnet` alt ağ adresi öneki ile `192.168.1.0/24`:

```azurecli
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

Çıktı:

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

Alt ağ içindeki sanal ağı mantıksal olarak olduğundan, biz biraz farklı bir komutla alt ağ bilgilerini arayın. Kullanırız komut `azure network vnet show`, ancak kullanarak JSON çıktısını incelemek devam `jq`.

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Çıktı:

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
Şimdi biz yük dengeleyicinizin Ata genel IP adresi (PIP) oluşturalım. Vm'leriniz için Internet'ten kullanarak bağlanmak sağlar `azure network public-ip create` komutu. Varsayılan Adres dinamik olduğundan, adlandırılmış bir DNS girişi oluşturuyoruz **cloudapp.azure.com** kullanarak etki alanı `--domain-name-label` seçeneği. Aşağıdaki örnek adlı ortak IP oluşturur `myPublicIP` DNS adı ile `mypublicdns`. DNS adının benzersiz olması gerektiğinden, kendi benzersiz DNS adını belirtin:

```azurecli
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

Çıktı:

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

Kendisiyle görebilmeniz için genel IP adresini de üst düzey bir kaynak değildir `azure group show`.

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Çıktı:

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

Tam kullanarak alt etki alanının tam etki alanı adı (FQDN) de dahil olmak üzere daha fazla kaynak ayrıntılarını araştırabilirsiniz `azure network public-ip show` komutu. Ortak IP adresi kaynağı mantıksal olarak ayrıldı, ancak belirli bir adresi yok henüz atanmamış. Bir IP adresi elde etmek için değil henüz oluşturduk bir yük dengeleyici ihtiyacınız olacak.

```azurecli
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

Çıktı:

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
Bir yük dengeleyici oluşturduğunuzda, trafiği birden çok VM dağıtmanızı sağlar. Ayrıca, Bakım veya ağır yük durumunda kullanıcı isteklerine yanıt birden çok VM çalıştırarak uygulamanız için artıklık da sağlar. Aşağıdaki örnek, adlandırılmış bir yük dengeleyici oluşturur `myLoadBalancer`:

```azurecli
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

Çıktı:

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

Bizim yük dengeleyici sağlandığından bazı IP havuzları oluşturmak oldukça boş olur. Bizim yük dengeleyicisi, ön uç için diğeri için arka uç için iki IP havuzları oluşturmak üzere istiyoruz. Ön uç IP havuzu herkese görünür. Ayrıca, daha önce oluşturduğumuz PIP atamak biz konum değil. Ardından arka uç havuzu bizim VM'ler için bir konum olarak bağlanmak için kullanırız. Bu şekilde, sanal makineleri yük dengeleyiciye üzerinden trafik akabilir.

İlk olarak, bizim ön uç IP havuzu oluşturalım. Aşağıdaki örnek adlı bir ön uç havuzu oluşturur `myFrontEndPool`:

```azurecli
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool
```

Çıktı:

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

Nasıl kullandık Not `--public-ip-name` içinde geçirmek için geçiş `myPublicIP` daha önce oluşturduğumuz. Genel IP adresini yük dengeleyici ile atama Vm'leriniz Internet üzerinden erişmek sağlar.

Ardından, bizim ikinci IP havuzu bizim arka uç trafiği için bu süre oluşturalım. Aşağıdaki örnek adlı bir arka uç havuzu oluşturur `myBackEndPool`:

```azurecli
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

Çıktı:

```azurecli
info:    Executing command network lb address-pool create
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

Bizim yük dengeleyici ile bakarak nasıl çalıştığını görebiliriz `azure network lb show` ve JSON çıktısını inceleyerek:

```azurecli
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

Çıktı:

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

## <a name="create-load-balancer-nat-rules"></a>Yük dengeleyicisi NAT kuralları oluşturma
Bizim yük dengeleyici üzerinden giden trafiği almak için biz ağ adresi gelen veya giden eylemleri belirtin çevirisi (NAT) kuralları oluşturmanız gerekir. Kullanılacak protokolü belirtin, sonra istediğiniz gibi dahili bağlantı noktaları için dış bağlantı noktası eşleme. Bizim ortamı için SSH bizim VM'ler bizim yük dengeleyiciye üzerinden izin veren bazı kurallar oluşturalım. TCP bağlantı noktası 22 (hangi daha sonra oluşturuyoruz) bizim Vm'lerinde yönlendirmek için TCP bağlantı noktalarını 4222 ve 4223 ayarlarız. Aşağıdaki örnek, adında bir kural oluşturur `myLoadBalancerRuleSSH1` bağlantı noktası 22 TCP bağlantı noktası 4222 eşlemek için:

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

Çıktı:

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

Yordamı, ikinci NAT kuralı için SSH için yineleyin. Aşağıdaki örnek, adında bir kural oluşturur `myLoadBalancerRuleSSH2` TCP bağlantı noktası 4223 bağlantı noktası 22 eşlemek için:

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

Şimdi de tane bizim IP havuzları kadar kural takma web trafiği için TCP bağlantı noktası 80 için NAT kuralı oluşturun. Biz kural bizim VM'ler için ayrı ayrı takma yerine bir IP havuzuna kural bağlanacağını varsa biz ekleyebilir veya VM'ler IP havuzundan kaldırabilirsiniz. Yük Dengeleyici trafik akışını otomatik olarak ayarlar. Aşağıdaki örnek, adında bir kural oluşturur `myLoadBalancerRuleWeb` TCP bağlantı noktası 80 80 numaralı bağlantı noktasına eşlemek için:

```azurecli
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

Çıktı:

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

## <a name="create-a-load-balancer-health-probe"></a>Yük dengeleyici durum araştırması oluşturma
Bir sistem durumu araştırması düzenli aralıklarla işletim ve tanımlanan isteklerine yanıt emin olmak için yük dengeleyicinin ardındaysa VM'ler denetler. Aksi durumda, kullanıcıların kendilerine yönlendirilmeye olmayan emin olmak için işlem kaldırılmış. Aralıklarla ve zaman aşımı değerlerinin yanı sıra durumu araştırması için özel denetimleri tanımlayabilirsiniz. Sistem durumu araştırmalarının hakkında daha fazla bilgi için bkz: [yük dengeleyici yoklamaları](../../load-balancer/load-balancer-custom-probe-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Aşağıdaki örnek, bir TCP oluşturur sistem durumu araştırılan adlandırılmış `myHealthProbe`:

```azurecli
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

Çıktı:

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

Burada, 15 saniye bizim durumu denetimleri için bir aralığı belirtildi. Biz, en fazla dört araştırmalar (bir yük dengeleyici konak artık çalışmıyor varsaymadan önce dakika) eksik.

## <a name="verify-the-load-balancer"></a>Yük Dengeleyici doğrulayın
Yük Dengeleyici yapılandırması şimdi yapılır. Yaptığınız adımlar şunlardır:

1. Bir yük dengeleyici oluşturuldu.
2. Bir ön uç IP havuzu oluşturduğunuz ve bir genel IP atanmış.
3. Sanal makineleri bağlanabileceği bir arka uç IP havuzu oluşturuldu.
4. Yönetim için web uygulamamız için TCP bağlantı noktası 80 sağlayan bir kural birlikte VM'ler için SSH izin veren NAT kuralları oluşturuldu.
5. Düzenli aralıklarla VM'ler denetlemek için bir sistem durumu araştırması eklendi. Bu sistem durumu araştırma, kullanıcılar artık çalışmıyor veya içerik hizmet veren bir VM erişmek denemeyin sağlar.

Şimdi, yük dengeleyici şimdi gibi göründüğünü gözden geçirin:

```azurecli
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

Çıktı:

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

## <a name="create-an-nic-to-use-with-the-linux-vm"></a>Linux VM ile kullanmak için bir NIC oluşturun
NIC program aracılığıyla kullanılabilir olduklarından kullanımları kuralları uygulayabilirsiniz. Birden fazla olabilir. Aşağıdaki `azure network nic create` komutu, yük arka uç IP havuzu NIC'ye bağlanacağını ve SSH trafiğe izin vermek için NAT kuralı ile ilişkilendirin.

Değiştir `#####-###-###` bölümleri, kendi Azure aboneliği ID'ye sahip Aboneliğinizi çıktısında kimliği not ettiğiniz `jq` oluşturmakta kaynakları incelediğinizde. Abonelik Kimliğinizle de görüntüleyebilirsiniz `azure account list`.

Aşağıdaki örnek, adlandırılmış bir NIC oluşturur `myNic1`:

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

Çıktı:

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

Kaynağı doğrudan inceleyerek ayrıntılarını görebilirsiniz. Kullanarak kaynağın incelemeniz `azure network nic show` komutu:

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

Çıktı:

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

Şimdi takma yeniden bizim arka uç IP havuzuna, ikinci bir NIC oluşturun. Bu zaman ikinci NAT kuralı SSH trafiğe izin verir. Aşağıdaki örnek, adlandırılmış bir NIC oluşturur `myNic2`:

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a>Bir ağ güvenlik grubu ve kuralları oluşturma
Şimdi biz bir ağ güvenlik grubu ve NIC erişimi yöneten gelen kuralları oluşturma Bir ağ güvenlik grubu, bir NIC veya alt ağ için uygulanabilir. Vm'leriniz ve trafik akışını denetlemek için kurallar tanımlar. Aşağıdaki örnek adlı bir ağ güvenlik grubu oluşturur `myNetworkSecurityGroup`:

```azurecli
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

NSG bağlantı noktası 22 (SSH desteklemek için) gelen bağlantılara izin vermek için gelen kuralı ekleyelim. Aşağıdaki örnek, adında bir kural oluşturur `myNetworkSecurityGroupRuleSSH` bağlantı noktası 22 üzerinde TCP'ye izin vermek için:

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

Şimdi NSG (web trafiği desteklemek için) 80 numaralı bağlantı noktasında gelen bağlantılara izin vermek için gelen kuralı ekleyelim. Aşağıdaki örnek, adında bir kural oluşturur `myNetworkSecurityGroupRuleHTTP` bağlantı noktası 80 üzerinde TCP'ye izin vermek için:

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [!NOTE]
> Gelen kuralı, gelen ağ bağlantıları için bir filtredir. Bu örnekte, bağlantı noktası 22 için herhangi bir istek NIC'ye bizim VM geçirilir, anlamına gelir VM'ler sanal NIC için NSG bağlayın. Bu gelen kuralı bir ağ bağlantısı hakkında ve bir uç nokta hakkında hangi hakkında Klasik dağıtımlarda olması. Bir bağlantı noktasını açmak için bırakmalıdır `--source-port-range` kümesine '\*' (öğesinden gelen istekleri kabul etmek için varsayılan değer) **herhangi** bağlantı noktası isteme. Bağlantı noktaları genellikle dinamik.
>
>

## <a name="bind-to-the-nic"></a>NIC'ye bağlama
NSG NIC'ler bağlayın. Bizim NIC'ler bizim ağ güvenlik grubuyla bağlanmak gerekir. Hem bizim NIC'ler kanca için her iki komutları çalıştırın:

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a>Kullanılabilirlik kümesi oluşturma
Kullanılabilirlik Yardım yayılan hata etki alanları ve yükseltme etki alanları arasında Vm'leriniz ayarlar. Kullanılabilirlik kümesi Vm'leriniz için oluşturalım. Aşağıdaki örnek, kullanılabilirlik adlandırılmış kümesi oluşturur `myAvailabilitySet`:

```azurecli
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

Sanal makineler, ortak bir güç kaynağı ve ağ anahtarı paylaşmak gruplandırması hata etki alanlarını tanımlayın. Varsayılan olarak, en çok üç hata etki alanlarında kullanılabilirlik kümesi içinde yapılandırılmış olan sanal makinelere ayrılır. Bu hata etki alanlarını birinde bir donanım sorunundan uygulamanızı çalıştıran her VM etkilemez olur. Azure otomatik olarak VM'ler hata etki alanlarında bunları bir kullanılabilirlik kümesine yerleştirdiğinizde dağıtır.

Yükseltme etki alanları, sanal makineler ve aynı anda yeniden temel alınan fiziksel donanım grupları belirtin. Yükseltme etki alanları yeniden başlatılır sıra planlı bakım sırasında sıralı olmayabilir, ancak aynı anda yalnızca bir yükseltme yeniden başlatıldı. Yeniden Azure otomatik olarak Vm'leriniz yükseltme etki alanları arasında bir kullanılabilirlik sitede yerleştirme sırasında dağıtır.

Daha fazla bilgi edinin [VM'ler kullanılabilirliğini yönetme](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="create-the-linux-vms"></a>Linux VM oluşturma
Internet'ten erişilebilen sanal makineleri desteklemek için depolama ve ağ kaynaklarının oluşturduğunuzu düşünün. Şimdi şimdi bunları VM'ler oluşturun ve bir parola sahip olmayan bir SSH anahtarı ile güvenli. Bu durumda, bir Ubuntu VM en son LTS göre oluşturmak için yapacağız. Biz kullanarak bu görüntü bilgilerini bulun `azure vm image list`açıklandığı gibi [Azure VM görüntülerini bulma](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Komutunu kullanarak bir görüntü seçtik `azure vm image list westeurope canonical | grep LTS`. Bu durumda, kullandığımız `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`. Son alanı için biz geçirmek `latest` böylece gelecekte en son yapı'biz her zaman alın. (Kullanırız dizesi `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).

Bu sonraki adım zaten oluşturulmuş herkes alışkın olduğu bir ssh rsa ortak ve özel anahtarı eşleştirin Linux veya Mac kullanarak **ssh-keygen - t rsa -b 2048**. Tüm sertifika anahtar çiftleri varsa değil, `~/.ssh` dizin oluşturabilirsiniz bunları:

* Kullanarak otomatik olarak `azure vm create --generate-ssh-keys` seçeneği.
* Kullanarak el ile [kendiniz oluşturmanız için yönergeler](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Alternatif olarak, kullanabileceğiniz `--admin-password` VM oluşturulduktan sonra SSH bağlantılarının kimliğini doğrulamak için yöntem. Bu yöntem genellikle daha az güvenlidir.

Bizim tüm kaynakları ve bilgileri ile birlikte getirerek VM oluşturuyoruz `azure vm create` komutu:

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

Çıktı:

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

Varsayılan SSH anahtarları kullanılarak VM'nize hemen bağlanabilir. Yük Dengeleyici üzerinden geçirme bu yana uygun bağlantı noktasını belirttiğinizden emin olun. (Bizim ilk VM için NAT kuralı bağlantı noktası 4222 bizim VM iletmek için ayarlarız.)

```bash
ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

Çıktı:

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
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

Tane ikinci VM'nizi aynı şekilde oluşturun:

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

Şimdi kullanabileceğiniz `azure vm show myResourceGroup myVM1` oluşturmuş olduğunuz incelemek için komutu. Bu noktada, Ubuntu Vm'leriniz yük dengeleyici arkasında (parolalar devre dışı bırakıldığı için), yalnızca, SSH anahtar çifti ile içine imzalayabilirsiniz Azure'da kullanıyorsunuz. Nginx veya httpd yükleyin, bir web uygulaması dağıtma ve trafiği de görmenize yük dengeleyici sanal makineleri her ikisine de aktığı.

```azurecli
azure vm show --resource-group myResourceGroup --name myVM1
```

Çıktı:

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


## <a name="export-the-environment-as-a-template"></a>Ortam şablon olarak dışarı aktarma
Ne bu ortamını yerleşik, aynı parametreleri ya da eşleşecek bir üretim ortamında ek geliştirme ortamı oluşturmak istediğiniz? Resource Manager, ortamınız için tüm parametreleri tanımlayan JSON şablonlarını kullanır. Bu JSON şablonunu başvurarak tüm ortamlar oluşturun. Yapabilecekleriniz [JSON şablonları el ile oluşturabilir](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya sizin için JSON şablonunu oluşturmak için varolan bir ortama verme:

```azurecli
azure group export --name myResourceGroup
```

Bu komut oluşturur `myResourceGroup.json` geçerli çalışma dizini dosyasında. Bu şablonu kullanarak bir ortam oluşturduğunuzda, yük dengeleyici, ağ arabirimlerine veya VM'ler için adlar dahil olmak üzere tüm kaynak adları, istenir. Ekleyerek bu adları şablon dosyanızın doldurabilirsiniz `-p` veya `--includeParameterDefaultValue` parametresi `azure group export` daha önce gösterilen komutu. Kaynak adları belirtmek için JSON şablonunuzu düzenleyin veya [parameters.json dosyası oluşturma](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kaynak adları belirtir.

Bir ortamda, şablonu oluşturmak için:

```azurecli
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

Okumak isteyebilirsiniz [şablonlardan dağıtma hakkında daha fazla](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Artımlı olarak update ortamlar, parametre dosyasını kullanın ve şablonları bir tek bir depolama konumdan erişim hakkında bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar
Artık birden çok ağ bileşenleri ve VM'ler ile çalışmaya başlamak hazırsınız. Burada sunulan çekirdek bileşenlerini kullanarak uygulamanızı oluşturmak için bu örnek ortamı kullanabilirsiniz.
