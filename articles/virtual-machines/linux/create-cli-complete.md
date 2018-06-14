---
title: Azure CLI 2.0 ile Linux ortamı oluşturun | Microsoft Docs
description: Depolama, bir Linux VM, bir sanal ağ ve alt ağ, bir yük dengeleyici, bir NIC, ortak bir IP ve tüm sıfırdan Azure CLI 2.0 kullanarak bir ağ güvenlik grubu oluşturun.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
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
ms.author: iainfou
ms.openlocfilehash: f41bfec3c9f950893b69c90a86c2e4a254b72a8b
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
ms.locfileid: "29852141"
---
# <a name="create-a-complete-linux-virtual-machine-with-the-azure-cli"></a>Azure CLI ile tam Linux sanal makine oluşturma
Hızla bir sanal makine (VM) oluşturmak için gerekli tüm destekleyici kaynakları oluşturmak için varsayılan değerleri kullanan tek bir Azure CLI komutu kullanabilirsiniz. Bir sanal ağ, genel IP adresi ve ağ güvenlik grubu kuralları gibi kaynakları otomatik olarak oluşturulur. Üretim ortamınızda, daha fazla denetim için kullanmak, önceden bu kaynakları oluşturmak ve bunları Vm'leriniz Ekle. Bu makalede, VM ve destekleyici kaynakları tek tek her nasıl oluşturulacağını aracılığıyla size yol gösterir.

En son yüklediğinizden emin olun [Azure CLI 2.0](/cli/azure/install-az-cli2) ve bir Azure hesabı ile oturum [az oturum açma](/cli/azure/reference-index#az_login).

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında *myResourceGroup*, *myVnet*, ve *myVM*.

## <a name="create-resource-group"></a>Kaynak grubu oluşturma
Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bir kaynak grubu, bir sanal makine ve destekleyici sanal ağ kaynakları önce oluşturulmalıdır. Kaynak grubu ile oluşturma [az grubu oluşturma](/cli/azure/group#az_group_create). Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

Varsayılan olarak, Azure CLI komutların çıktısı JSON (JavaScript nesne gösterimi) ' dir. Örneğin, bir liste veya tablo çıktısı varsayılan değeri değiştirmek için kullanmak [az yapılandırmak--çıkış](/cli/azure/reference-index#az_configure). Ayrıca ekleyebileceğiniz `--output` herhangi bir komutunu bir kez için çıktı biçiminde değiştirin. Aşağıdaki örnek, JSON çıktısını gösterir `az group create` komutu:

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
Azure ve bir alt ağda bir sanal ağ oluşturma sonraki Vm'leriniz oluşturabilirsiniz. Kullanım [az ağ vnet oluşturma](/cli/azure/network/vnet#az_network_vnet_create) adlı bir sanal ağ oluşturmak için *myVnet* ile *192.168.0.0/16* adres öneki. Ayrıca adlı bir alt ağ Ekle *mySubnet* adres öneki ile *192.168.1.0/24*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

Alt ağ içindeki sanal ağı mantıksal olarak oluşturulan çıkış gösterir:

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
Şimdi bir genel IP adresiyle oluşturalım [az ağ genel IP oluşturun](/cli/azure/network/public-ip#az_network_public_ip_create). Bu genel IP adresi Vm'leriniz için Internet'ten bağlanmanıza olanak sağlar. Varsayılan Adres dinamik olduğundan içeren adlandırılmış bir DNS girişi oluşturmak `--domain-name-label` parametresi. Aşağıdaki örnek adlı ortak IP oluşturur *myPublicIP* DNS adı ile *mypublicdns*. DNS adının benzersiz olması gerektiğinden, kendi benzersiz DNS adını belirtin:

```azurecli
az network public-ip create \
    --resource-group myResourceGroup \
    --name myPublicIP \
    --dns-name mypublicdns
```

Çıktı:

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
Vm'leriniz ve trafik akışını denetlemek için bir ağ güvenlik grubu bir sanal NIC veya alt ağ için geçerlidir. Aşağıdaki örnek kullanır [az ağ nsg oluşturma](/cli/azure/network/nsg#az_network_nsg_create) adlı bir ağ güvenlik grubu oluşturmak için *myNetworkSecurityGroup*:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

İzin veren veya belirli bir trafik reddeden kuralları tanımlar. Bağlantı noktası 22 (SSH erişimini etkinleştirmek için) gelen bağlantılara izin vermek için bir gelen kuralı ile oluşturma [az ağ nsg kuralını](/cli/azure/network/nsg/rule#az_network_nsg_rule_create). Aşağıdaki örnek, adında bir kural oluşturur *myNetworkSecurityGroupRuleSSH*:

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

(Web trafiği için) 80 numaralı bağlantı noktasında gelen bağlantılara izin vermek için başka bir ağ güvenlik grubu kural ekleyin. Aşağıdaki örnek, adında bir kural oluşturur *myNetworkSecurityGroupRuleHTTP*:

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

Ağ güvenlik grubu ve kurallarla inceleyin [az ağ nsg Göster](/cli/azure/network/nsg#az_network_nsg_show):

```azurecli
az network nsg show --resource-group myResourceGroup --name myNetworkSecurityGroup
```

Çıktı:

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
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowAzureLoadBalancerInBou
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

## <a name="create-a-virtual-nic"></a>Sanal bir NIC'ye oluşturma
Sanal ağ arabirimi kartlarıyla (NIC) program aracılığıyla kullanılabilir olduklarından kullanımları kuralları uygulayabilirsiniz. Bağlı olarak [VM boyutu](sizes.md), bir VM'ye birden çok sanal NIC ekleyebilirsiniz. Aşağıdaki [az ağ NIC oluşturmak](/cli/azure/network/nic#az_network_nic_create) komutunu adlı bir NIC oluşturduğunuz *myNic* ve ağ güvenlik grubuyla ilişkilendirin. Genel IP adresi *myPublicIP* de bir sanal NIC ile ilişkilidir

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --public-ip-address myPublicIP \
    --network-security-group myNetworkSecurityGroup
```

Çıktı:

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
Kullanılabilirlik Yardım yayılan hata etki alanları ve güncelleştirme etki alanları arasında Vm'leriniz ayarlar. Yalnızca bir VM şimdi oluşturduğunuz olsa bile, gelecekte genişletin kolaylaştırmak için kullanılabilirlik kümeleri kullanmak en iyi bir uygulamadır. 

Sanal makineler, ortak bir güç kaynağı ve ağ anahtarı paylaşmak gruplandırması hata etki alanlarını tanımlayın. Varsayılan olarak, en çok üç hata etki alanlarında kullanılabilirlik kümesi içinde yapılandırılmış olan sanal makinelere ayrılır. Bu hata etki alanlarını birinde bir donanım sorunundan uygulamanızı çalıştıran her VM etkilemez.

Güncelleme etki alanına sanal makineler ve aynı anda yeniden temel alınan fiziksel donanım grupları belirtin. Planlı bakım sırasında hangi güncelleştirme etki alanlarını yeniden sıra sıralı olmayabilir, ancak aynı anda yalnızca tek bir güncelleştirme etki alanı yeniden başlatıldı.

Azure otomatik olarak VM'ler arıza ve güncelleştirme etki alanları arasında bunları bir kullanılabilirlik kümesine yerleştirdiğinizde dağıtır. Daha fazla bilgi için bkz: [VM'ler kullanılabilirliğini yönetme](manage-availability.md).

Kullanılabilirlik için VM ile kümesi oluştur [az vm kullanılabilirlik kümesi oluşturma](/cli/azure/vm/availability-set#az_vm_availability_set_create). Aşağıdaki örnek *myAvailabilitySet* adında bir kullanılabilirlik kümesi oluşturur:

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet
```

Çıktı notları hata etki alanları ve güncelleme etki alanına:

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
Internet'ten erişilebilen sanal makineleri desteklemek için ağ kaynaklarına oluşturduğunuzu düşünün. Şimdi bir VM oluşturun ve bir SSH anahtarı ile güvenliğini sağlayın. Bu örnekte, bir Ubuntu VM en son LTS dayalı oluşturalım. Ek görüntülerle bulabilirsiniz [az vm görüntü listesi](/cli/azure/vm/image#az_vm_image_list)açıklandığı gibi [Azure VM görüntülerini bulma](cli-ps-findimage.md).

Kimlik doğrulaması için kullanılacak bir SSH anahtarı belirtin. SSH ortak anahtar çifti yoksa, şunları yapabilirsiniz [bunları oluşturmanız](mac-create-ssh-keys.md) veya `--generate-ssh-keys` bunları sizin için oluşturmak için parametre. Bir anahtar çifti zaten varsa, bu parametre varolan anahtarların kullanan `~/.ssh`.

Tüm kaynaklara ve bilgi ile birlikte getirerek VM oluşturma [az vm oluşturma](/cli/azure/vm#az_vm_create) komutu. Aşağıdaki örnek *myVM* adlı bir VM oluşturur:

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

SSH ortak IP adresi oluştururken sağladığınız DNS girişi ile VM. Bu `fqdn` , VM oluşturma çıktısında gösterilir:

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

Çıktı:

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
    http://www.ubuntu.com/business/services/cloud

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

NGINX yükleyin ve trafiği de görmenize VM akış. NGINX gibi yükleyin:

```bash
sudo apt-get install -y nginx
```

Eylem varsayılan NGINX sitede görmek için web tarayıcınızı açın ve FQDN değerinizi girin:

![Varsayılan NGINX sitesinde VM](media/create-cli-complete/nginx.png)

## <a name="export-as-a-template"></a>Bir şablon olarak dışarı aktarma
Ne artık aynı parametreleri ya da bir üretim ortamında ek geliştirme ortamı oluşturmak isterseniz, bu eşleştiğinde? Resource Manager, ortamınız için tüm parametreleri tanımlayan JSON şablonlarını kullanır. Bu JSON şablonunu başvurarak tüm ortamlar oluşturun. Yapabilecekleriniz [JSON şablonları el ile oluşturabilir](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya sizin için JSON şablonunu oluşturmak için varolan bir ortama dışa aktarın. Kullanım [az grup verme](/cli/azure/group#az_group_export) kaynak grubunuz gibi dışarı aktarmak için:

```azurecli
az group export --name myResourceGroup > myResourceGroup.json
```

Bu komut oluşturur `myResourceGroup.json` geçerli çalışma dizini dosyasında. Bu şablonu kullanarak bir ortam oluşturmak için tüm kaynak adları sorulur. Ekleyerek bu adları şablon dosyanızın doldurabilirsiniz `--include-parameter-default-value` parametresi `az group export` komutu. Kaynak adları belirtmek için JSON şablonunuzu düzenleyin veya [parameters.json dosyası oluşturma](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kaynak adları belirtir.

Bir ortamda, şablonu oluşturmak için kullanmak [az grup dağıtımı oluşturmak](/cli/azure/group/deployment#az_group_deployment_create) gibi:

```azurecli
az group deployment create \
    --resource-group myNewResourceGroup \
    --template-file myResourceGroup.json
```

Okumak isteyebilirsiniz [şablonlardan dağıtma hakkında daha fazla](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Artımlı olarak update ortamlar, parametre dosyasını kullanın ve şablonları bir tek bir depolama konumdan erişim hakkında bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar
Artık birden çok ağ bileşenleri ve VM'ler ile çalışmaya başlamak hazırsınız. Burada sunulan çekirdek bileşenlerini kullanarak uygulamanızı oluşturmak için bu örnek ortamı kullanabilirsiniz.
