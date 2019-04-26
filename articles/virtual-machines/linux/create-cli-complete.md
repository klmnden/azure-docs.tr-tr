---
title: Azure CLI ile bir Linux ortamı oluşturma | Microsoft Docs
description: Depolama, bir Linux VM, sanal ağ ve alt ağ, yük dengeleyici, bir NIC, genel bir IP ve tüm yönleriyle Azure CLI kullanarak bir ağ güvenlik grubu oluşturun.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 4ba4060b-ce95-4747-a735-1d7c68597a1a
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/14/2017
ms.author: cynthn
ms.openlocfilehash: eb4c5897cdadecd074c2764faceeed13f4c724c3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60328650"
---
# <a name="create-a-complete-linux-virtual-machine-with-the-azure-cli"></a>Azure CLI'de eksiksiz bir Linux sanal makinesi oluşturma
Bir sanal makine (VM) Azure'da hızlıca oluşturmak için gerekli tüm destekleyici kaynakları oluşturmak için varsayılan değerleri kullanan tek bir Azure CLI komutunu kullanabilirsiniz. Bir sanal ağ, genel IP adresi ve ağ güvenlik grubu kuralları gibi kaynakları otomatik olarak oluşturulur. Daha fazla denetim üretim ortamınızda kullanmak, önceden bu kaynakları oluşturmak ve Vm'lerinizi bunlara ekleyin. Bu makalede, VM ve destekleyici kaynakların tek tek her biri oluşturma hakkında size yol gösterir.

En son yüklediğinizden emin olun [Azure CLI](/cli/azure/install-az-cli2) ve bir Azure hesabı ile oturum [az login](/cli/azure/reference-index).

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında *myResourceGroup*, *myVnet*, ve *myVM*.

## <a name="create-resource-group"></a>Kaynak grubu oluşturma
Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bir kaynak grubu, bir sanal makine ve destekleyici ağ kaynakları önce oluşturulmalıdır. Kaynak grubu oluşturun [az grubu oluşturma](/cli/azure/group). Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

Varsayılan olarak, Azure CLI komutlarının çıkışı JSON (JavaScript nesne gösterimi) ' dir. Örneğin, bir liste veya tablo çıktısı varsayılan değiştirmek için kullanın [az yapılandırma--çıktı](/cli/azure/reference-index). Ayrıca ekleyebilirsiniz `--output` herhangi bir komutu bir kez çıkış biçimde değiştirin. Aşağıdaki örnek JSON çıktısında `az group create` komutu:

```json                       
{
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "location": "eastus",
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-a-virtual-network-and-subnet"></a>Sanal ağ ve alt ağ oluşturma
Azure'da ve bir alt ağ içinde bir sanal ağ oluşturma sonraki Vm'lerinizi oluşturabilirsiniz. Kullanım [az ağ sanal ağ oluşturma](/cli/azure/network/vnet) adlı bir sanal ağ oluşturmak için *myVnet* ile *192.168.0.0/16* adres ön eki. Ayrıca adlı bir alt ağ Ekle *mySubnet* adres ön eki ile *192.168.1.0/24*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

Sanal ağ içinde alt mantıksal olarak oluşturulan çıktı gösterir:

```json
{
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "etag": "W/\"e95496fc-f417-426e-a4d8-c9e4d27fc2ee\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "location": "eastus",
  "name": "myVnet",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "resourceGuid": "ed62fd03-e9de-430b-84df-8a3b87cacdbb",
  "subnets": [
    {
      "addressPrefix": "192.168.1.0/24",
      "etag": "W/\"e95496fc-f417-426e-a4d8-c9e4d27fc2ee\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet",
      "ipConfigurations": null,
      "name": "mySubnet",
      "networkSecurityGroup": null,
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "resourceNavigationLinks": null,
      "routeTable": null
    }
  ],
  "tags": {},
  "type": "Microsoft.Network/virtualNetworks",
  "virtualNetworkPeerings": null
}
```


## <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma
Şimdi bir genel IP adresiyle oluşturalım [az network public-IP oluşturma](/cli/azure/network/public-ip). Bu genel IP adresi, Internet'ten Vm'lerinizi bağlanmanızı sağlar. Varsayılan Adres dinamik olduğundan, adlandırılmış bir DNS girişi ile oluşturma `--domain-name-label` parametresi. Aşağıdaki örnekte adlı bir genel IP oluşturur *Mypublicıp* DNS adıyla *mypublicdns*. DNS adının benzersiz olması gerektiğinden, kendi benzersiz bir DNS adı sağlayın:

```azurecli
az network public-ip create \
    --resource-group myResourceGroup \
    --name myPublicIP \
    --dns-name mypublicdns
```

Çıkış:

```json
{
  "publicIp": {
    "dnsSettings": {
      "domainNameLabel": "mypublicdns",
      "fqdn": "mypublicdns.eastus.cloudapp.azure.com",
      "reverseFqdn": null
    },
    "etag": "W/\"2632aa72-3d2d-4529-b38e-b622b4202925\"",
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "idleTimeoutInMinutes": 4,
    "ipAddress": null,
    "ipConfiguration": null,
    "location": "eastus",
    "name": "myPublicIP",
    "provisioningState": "Succeeded",
    "publicIpAddressVersion": "IPv4",
    "publicIpAllocationMethod": "Dynamic",
    "resourceGroup": "myResourceGroup",
    "resourceGuid": "4c65de38-71f5-4684-be10-75e605b3e41f",
    "tags": null,
    "type": "Microsoft.Network/publicIPAddresses"
  }
}
```


## <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma
Vm'lerinizi ve giden trafik akışını denetlemek için sanal bir NIC veya alt ağ bir ağ güvenlik grubu uygulayın. Aşağıdaki örnekte [az ağ nsg oluşturma](/cli/azure/network/nsg) adlı bir ağ güvenlik grubu oluşturmak için *Vm2*:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

İzin veren veya belirli bir trafiği reddeden kuralları tanımlayın. (SSH erişimini etkinleştirmek için) 22 numaralı bağlantı noktasında gelen bağlantılara izin vermek için bir gelen kuralı ile oluşturma [az ağ nsg kuralı oluşturmak](/cli/azure/network/nsg/rule). Aşağıdaki örnekte adlı bir kural oluşturur *myNetworkSecurityGroupRuleSSH*:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleSSH \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 22 \
    --access allow
```

(Web trafiği için) 80 numaralı bağlantı noktasında gelen bağlantılara izin vermek için başka bir ağ güvenlik grubu kuralı ekleyin. Aşağıdaki örnekte adlı bir kural oluşturur *myNetworkSecurityGroupRuleHTTP*:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleWeb \
    --protocol tcp \
    --priority 1001 \
    --destination-port-range 80 \
    --access allow
```

Ağ güvenlik grubu ve kurallarıyla inceleyin [az ağ nsg show](/cli/azure/network/nsg):

```azurecli
az network nsg show --resource-group myResourceGroup --name myNetworkSecurityGroup
```

Çıkış:

```json
{
  "defaultSecurityRules": [
    {
      "access": "Allow",
      "description": "Allow inbound traffic from all VMs in VNET",
      "destinationAddressPrefix": "VirtualNetwork",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowVnetInBound",
      "name": "AllowVnetInBound",
      "priority": 65000,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "VirtualNetwork",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow inbound traffic from azure load balancer",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowAzureLoadBalancerInBou",
      "name": "AllowAzureLoadBalancerInBound",
      "priority": 65001,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "AzureLoadBalancer",
      "sourcePortRange": "*"
    },
    {
      "access": "Deny",
      "description": "Deny all inbound traffic",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/DenyAllInBound",
      "name": "DenyAllInBound",
      "priority": 65500,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow outbound traffic from all VMs to all VMs in VNET",
      "destinationAddressPrefix": "VirtualNetwork",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowVnetOutBound",
      "name": "AllowVnetOutBound",
      "priority": 65000,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "VirtualNetwork",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow outbound traffic from all VMs to Internet",
      "destinationAddressPrefix": "Internet",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowInternetOutBound",
      "name": "AllowInternetOutBound",
      "priority": 65001,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Deny",
      "description": "Deny all outbound traffic",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/DenyAllOutBound",
      "name": "DenyAllOutBound",
      "priority": 65500,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    }
  ],
  "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup",
  "location": "eastus",
  "name": "myNetworkSecurityGroup",
  "networkInterfaces": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "resourceGuid": "47a9964e-23a3-438a-a726-8d60ebbb1c3c",
  "securityRules": [
    {
      "access": "Allow",
      "description": null,
      "destinationAddressPrefix": "*",
      "destinationPortRange": "22",
      "direction": "Inbound",
      "etag": "W/\"9e344b60-0daa-40a6-84f9-0ebbe4a4b640\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/myNetworkSecurityGroupRuleSSH",
      "name": "myNetworkSecurityGroupRuleSSH",
      "priority": 1000,
      "protocol": "Tcp",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": null,
      "destinationAddressPrefix": "*",
      "destinationPortRange": "80",
      "direction": "Inbound",
      "etag": "W/\"9e344b60-0daa-40a6-84f9-0ebbe4a4b640\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/myNetworkSecurityGroupRuleWeb",
      "name": "myNetworkSecurityGroupRuleWeb",
      "priority": 1001,
      "protocol": "Tcp",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    }
  ],
  "subnets": null,
  "tags": null,
  "type": "Microsoft.Network/networkSecurityGroups"
}
```

## <a name="create-a-virtual-nic"></a>Sanal bir NIC oluşturup
Sanal ağ arabirim kartları (NIC) program aracılığıyla kullanılabilir olduklarından kullanımları kuralları uygulayabilirsiniz. Yapılandırmanıza bağlı olarak [VM boyutu](sizes.md), birden çok sanal NIC bir VM'ye ekleyebilirsiniz. Aşağıdaki [az ağ NIC oluşturup](/cli/azure/network/nic) komutunu adlı bir NIC oluşturduğunuz *Mynıc* , ağ güvenlik grubu ile ilişkilendirin. Genel IP adresini *Mypublicıp* sanal NIC ile ilişkilendirilir

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --public-ip-address myPublicIP \
    --network-security-group myNetworkSecurityGroup
```

Çıkış:

```json
{
  "NewNIC": {
    "dnsSettings": {
      "appliedDnsServers": [],
      "dnsServers": [],
      "internalDnsNameLabel": null,
      "internalDomainNameSuffix": "brqlt10lvoxedgkeuomc4pm5tb.bx.internal.cloudapp.net",
      "internalFqdn": null
    },
    "enableAcceleratedNetworking": false,
    "enableIpForwarding": false,
    "etag": "W/\"04b5ab44-d8f4-422a-9541-e5ae7de8466d\"",
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic",
    "ipConfigurations": [
      {
        "applicationGatewayBackendAddressPools": null,
        "etag": "W/\"04b5ab44-d8f4-422a-9541-e5ae7de8466d\"",
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic/ipConfigurations/ipconfig1",
        "loadBalancerBackendAddressPools": null,
        "loadBalancerInboundNatRules": null,
        "name": "ipconfig1",
        "primary": true,
        "privateIpAddress": "192.168.1.4",
        "privateIpAddressVersion": "IPv4",
        "privateIpAllocationMethod": "Dynamic",
        "provisioningState": "Succeeded",
        "publicIpAddress": {
          "dnsSettings": null,
          "etag": null,
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
          "idleTimeoutInMinutes": null,
          "ipAddress": null,
          "ipConfiguration": null,
          "location": null,
          "name": null,
          "provisioningState": null,
          "publicIpAddressVersion": null,
          "publicIpAllocationMethod": null,
          "resourceGroup": "myResourceGroup",
          "resourceGuid": null,
          "tags": null,
          "type": null
        },
        "resourceGroup": "myResourceGroup",
        "subnet": {
          "addressPrefix": null,
          "etag": null,
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet",
          "ipConfigurations": null,
          "name": null,
          "networkSecurityGroup": null,
          "provisioningState": null,
          "resourceGroup": "myResourceGroup",
          "resourceNavigationLinks": null,
          "routeTable": null
        }
      }
    ],
    "location": "eastus",
    "macAddress": null,
    "name": "myNic",
    "networkSecurityGroup": {
      "defaultSecurityRules": null,
      "etag": null,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup",
      "location": null,
      "name": null,
      "networkInterfaces": null,
      "provisioningState": null,
      "resourceGroup": "myResourceGroup",
      "resourceGuid": null,
      "securityRules": null,
      "subnets": null,
      "tags": null,
      "type": null
    },
    "primary": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "myResourceGroup",
    "resourceGuid": "b3dbaa0e-2cf2-43be-a814-5cc49fea3304",
    "tags": null,
    "type": "Microsoft.Network/networkInterfaces",
    "virtualMachine": null
  }
}
```


## <a name="create-an-availability-set"></a>Kullanılabilirlik kümesi oluşturma
Sanal makinelerinizin hata etki alanları ve güncelleme etki alanları arasında Yardım yayılan kullanılabilirlik kümeleri. Yalnızca bir sanal makine şu anda oluşturduğunuz olsa da, gelecekte genişletin daha kolay hale getirmek için kullanılabilirlik kümelerini kullanın. en iyi bir uygulamadır. 

Hata etki alanları ortak bir güç kaynağı ve ağ anahtarını paylaşan sanal makinelerin bir gruplandırma tanımlayın. Varsayılan olarak, kullanılabilirlik kümenizde yapılandırılmış olan sanal makineler en fazla üç hata etki alanları arasında ayrılır. Bu hata etki alanlarına birinde bir donanım sorunu, uygulamanızı çalıştıran her VM etkilemez.

Güncelleştirme etki alanları, sanal makineler ve aynı anda yeniden başlatılabilecek temel alınan fiziksel donanımları grupları belirtin. Planlı bakım sırasında hangi güncelleştirme etki alanlarını yeniden başlatıldığı sırada sıralı olmayabilir, ancak aynı anda yalnızca bir güncelleme etki alanı yeniden başlatılır.

Azure otomatik olarak Vm'leri hatası ve güncelleme etki alanları arasında bunları bir kullanılabilirlik kümesine yerleştirdiğinizde dağıtır. Daha fazla bilgi için [VM kullanılabilirliğini yönetme](manage-availability.md).

Bir kullanılabilirlik kümesi ile sanal makinenizde [az vm kullanılabilirlik kümesi oluşturma](/cli/azure/vm/availability-set). Aşağıdaki örnek *myAvailabilitySet* adında bir kullanılabilirlik kümesi oluşturur:

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet
```

Çıkış notları hata etki alanları ve güncelleme etki alanları:

```json
{
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet",
  "location": "eastus",
  "managed": null,
  "name": "myAvailabilitySet",
  "platformFaultDomainCount": 2,
  "platformUpdateDomainCount": 5,
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": null,
    "managed": true,
    "tier": null
  },
  "statuses": null,
  "tags": {},
  "type": "Microsoft.Compute/availabilitySets",
  "virtualMachines": []
}
```


## <a name="create-a-vm"></a>VM oluşturma
İnternet'ten erişilebilen Vm'leri desteklemek için ağ kaynaklarına oluşturdunuz. Artık bir VM oluşturun ve bir SSH anahtarı ile güvenli hale getirme. Bu örnekte, bir Ubuntu VM üzerinde en son LTS tabanlı oluşturalım. Ek görüntülerle bulabilirsiniz [az vm görüntüsü listesi](/cli/azure/vm/image)anlatılan şekilde [Azure VM görüntüleri bulma](cli-ps-findimage.md).

Kimlik doğrulaması için kullanılacak bir SSH anahtarı belirtin. SSH ortak anahtar çifti yoksa, şunları yapabilirsiniz [oluşturduktan](mac-create-ssh-keys.md) veya `--generate-ssh-keys` parametresini kullanarak bunları sizin için oluşturur. Bir anahtar çifti zaten varsa, bu parametre, mevcut anahtarları kullanır. `~/.ssh`.

Tüm kaynaklara ve bilgi ile birlikte taşıyarak VM oluşturma [az vm oluşturma](/cli/azure/vm) komutu. Aşağıdaki örnek *myVM* adlı bir VM oluşturur:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location eastus \
    --availability-set myAvailabilitySet \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Genel IP adresini oluştururken sağladığınız DNS girişi ile sanal makinenize yönelik SSH. Bu `fqdn` VM oluşturma çıktısında gösterilir:

```json
{
  "fqdns": "mypublicdns.eastus.cloudapp.azure.com",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-13-71-C8",
  "powerState": "VM running",
  "privateIpAddress": "192.168.1.5",
  "publicIpAddress": "13.90.94.252",
  "resourceGroup": "myResourceGroup"
}
```

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

Çıkış:

```bash
The authenticity of host 'mypublicdns.eastus.cloudapp.azure.com (13.90.94.252)' can't be established.
ECDSA key fingerprint is SHA256:SylINP80Um6XRTvWiFaNz+H+1jcrKB1IiNgCDDJRj6A.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'mypublicdns.eastus.cloudapp.azure.com,13.90.94.252' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.11.0-1016-azure x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    https://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

azureuser@myVM:~$
```

NGINX yüklemek ve trafiği de görmenize VM akış. NGINX aşağıdaki gibi yükleyin:

```bash
sudo apt-get install -y nginx
```

Varsayılan NGINX sitesi iş başında görmek için web tarayıcınızı açın ve FQDN değerinizi girin:

![Vm'nizde varsayılan NGINX sitesi](media/create-cli-complete/nginx.png)

## <a name="export-as-a-template"></a>Şablon olarak dışarı aktarma
Ne artık bir ek geliştirme ortamı aynı parametreleri ya da üretim ortamı oluşturmak istediğiniz, eşleşen? Resource Manager, ortamınız için tüm parametreleri tanımlayan JSON şablonlarını kullanır. Tüm ortamları, bu JSON şablonunu başvurarak oluşturun. Yapabilecekleriniz [JSON şablonları el ile derleme](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya sizin için JSON şablonunu oluşturmak için bir ortamı dışarı aktarın. Kullanım [az grubunu dışarı aktarma](/cli/azure/group) kaynak grubunuzun şu şekilde dışarı aktarmak için:

```azurecli
az group export --name myResourceGroup > myResourceGroup.json
```

Bu komut oluşturur `myResourceGroup.json` geçerli çalışma dizininizde dosya. Bu şablonu kullanarak bir ortam oluşturduğunuzda, tüm kaynak adları için istenir. Ekleyerek bu adlar, şablon dosyanızda doldurabilirsiniz `--include-parameter-default-value` parametresi `az group export` komutu. Kaynak adlarını belirtmek için JSON şablonunu düzenlemeniz veya [parameters.json dosya oluşturma](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kaynak adları belirtir.

Şablonunuzdan bir ortam oluşturmak için kullanın [az grubu dağıtım oluşturma](/cli/azure/group/deployment) gibi:

```azurecli
az group deployment create \
    --resource-group myNewResourceGroup \
    --template-file myResourceGroup.json
```

Okumak isteyebilirsiniz [şablonlardan dağıtma hakkında daha fazla](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Artımlı olarak ortamları güncelleştirebilir, parametre dosyasını kullanın ve tek bir depolama konumundan şablonları erişim hakkında bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar
Artık birden çok ağ bileşenleri ve Vm'leri ile çalışmaya başlamak hazırsınız. Burada sunulan temel bileşenleri kullanarak uygulamanızı oluşturmak için bu örnek ortamı kullanabilirsiniz.
