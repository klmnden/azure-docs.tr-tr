---
title: "Azure sanal makine ölçek kümeleri için ağ hizmeti | Microsoft Docs"
description: "Azure sanal makine ölçek kümeleri için yapılandırma ağ hizmeti özellikleri."
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/06/2017
ms.author: guybo
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 1c9487be5415d05a8699f458259d872591280d3d
ms.contentlocale: tr-tr
ms.lasthandoff: 07/10/2017


---
# <a name="networking-for-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümeleri için ağ hizmeti

Portal aracılığıyla Azure sanal makine ölçek kümesinin dağıtımını yaptığınızda gelen NAT kurallarıyla Azure Load Balancer gibi bazı ağ özellikleri varsayılan olarak gelir. Bu makalede, ölçek kümeleriyle yapılandırabileceğiniz daha gelişmiş ağ özelliklerinden bazılarının nasıl kullanıldığı açıklanır.

Bu makalede ele alınan özelliklerin tümünü Azure Resource Manager şablonlarını kullanarak yapılandırabilirsiniz. Belirli özellikler için Azure CLI örnekleri de eklenmiştir. Temmuz 2017 veya üstü bir CLI sürümü kullanın. Başka CLI ve PowerShell örnekleri yakında eklenecektir.

## <a name="accelerated-networking"></a>Hızlandırılmış Ağ
Azure [Hızlandırılmış Ağ](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-create-vm-accelerated-networking), sanal makineye tek kökte G/Ç sanallaştırmasına (SR-IV) olanak tanıyarak ağ performansını geliştirir. Ölçek kümeleriyle hızlandırılmış ağ kullanmak için, ölçek kümenizin networkInterfaceConfigurations ayarlarında enableAcceleratedNetworking değerini _true_ olarak ayarlayın. Örneğin:
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
    {
      "name": "niconfig1",
      "properties": {
        "primary": true,
        "enableAcceleratedNetworking" : true,
        "ipConfigurations": [
          ...
        ]
      }
    }
   ]
}
```

## <a name="create-a-scale-set-that-references-an-existing-azure-load-balancer"></a>Mevcut Azure Load Balancer’a başvuran bir ölçek kümesi oluşturma
Azure portalı kullanılarak ölçek kümesi oluşturulduğunda, yapılandırma seçeneklerinin çoğunda yeni bir yük dengeleyici oluşturulur. Mevcut yük dengeleyiciye başvurması gereken bir ölçek kümesi oluşturuyorsanız, bunu CLI kullanarak yapabilirsiniz. Aşağıdaki örnek betik bir yük dengeleyici ve ardından ona başvuran bir ölçek kümesi oluşturur:
```bash
az network lb create -g lbtest -n mylb --vnet-name myvnet --subnet mysubnet --public-ip-address-allocation Static --backend-pool-name mybackendpool

az vmss create -g lbtest -n myvmss --image Canonical:UbuntuServer:16.04-LTS:latest --admin-username negat --ssh-key-value /home/myuser/.ssh/id_rsa.pub --upgrade-policy-mode Automatic --instance-count 3 --vnet-name myvnet --subnet mysubnet --lb mylb --backend-pool-name mybackendpool

```

## <a name="configurable-dns-settings"></a>Yapılandırılabilir DNS Ayarları
Varsayılan olarak, ölçek kümeleri içinde oluşturuldukları VNET ve alt ağın belirli DNS ayarlarını alır. Bununla birlikte, ölçek kümesi için DNS ayarlarını doğrudan oluşturmanız da mümkündür.

### <a name="creating-a-scale-set-with-configurable-dns-servers"></a>Yapılandırılabilir DNS sunucularıyla ölçek kümesi oluşturma
CLI 2.0 kullanarak özel DNS yapılandırmasıyla bir ölçek kümesi oluşturmak için, _vmss create_ komutuna sunucu ip adreslerini ayıran boşluktan sonra --dns-servers bağımsız değişkenini ekleyin. Örneğin:
```bash
--dns-servers 10.0.0.6 10.0.0.5
```
Azure şablonunda özel DNS sunucularını yapılandırmak için, ölçek kümesinin networkInterfaceConfigurations bölümüne bir dnsSettings özelliği ekleyin. Örneğin:
```json
"dnsSettings":{
    "dnsServers":["10.0.0.6", "10.0.0.5"]
}
```

### <a name="creating-a-scale-set-with-configurable-virtual-machine-domain-names"></a>Yapılandırılabilir sanal makine etki alanı adlarıyla ölçek kümesi oluşturma
CLI 2.0 kullanarak özel DNS adıyla bir ölçek kümesi oluşturmak için, _vmss create_ komutuna etki alanı adını temsil eden dizeden sonra _--vm-domain-name_ bağımsız değişkenini ekleyin.

Azure şablonunda etki alanı adını ayarlamak için, ölçek kümesinin networkInterfaceConfigurations bölümüne bir dnsSettings özelliği ekleyin. Örneğin:

```json
"networkProfile": {
  "networkInterfaceConfigurations": [
    {
    "name": "nic1",
    "properties": {
      "primary": "true",
      "ipConfigurations": [
      {
        "name": "ip1",
        "properties": {
          "subnet": {
            "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
          },
          "publicIPAddressconfiguration": {
            "name": "publicip",
            "properties": {
            "idleTimeoutInMinutes": 10,
              "dnsSettings": {
                "domainNameLabel": "[parameters('vmssDnsName')]"
              }
            }
          }
        }
      }
    ]
    }
}
```

Tek bir sanal makine dns adı için çıkış şu biçimde olabilir: 
```
<vmname><vmindex>.<specifiedVmssDomainNameLabel>
```

## <a name="ipv6-preview-for-public-ips-and-load-balancer-pools"></a>Genel IP’ler ve Load Balancer havuzları için IPv6 önizlemesi
Azure Load Balancer’da IPv6 genel IP adreslerini yapılandırabilir ve bağlantıları sanal makine ölçek kümesi arka uç havuzlarına yönlendirebilirsiniz. Şu anda önizleme aşamasında olan IPv6’yı kullanmak için, ilk olarak IPv6 genel adres kaynağını oluşturun. Örneğin:
```json
{
    "apiVersion": "2016-03-30",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[parameters('ipv6PublicIPAddressName')]",
    "location": "[parameters('location')]",
    "properties": {
        "publicIPAddressVersion": "IPv6",
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
            "domainNameLabel": "[parameters('dnsNameforIPv6LbIP')]"
        }
    }
}
```
Ardından, IPv4 ve IPv6 için yük dengeleyici ön uç IP Yapılandırmalarınızı gerektiği gibi ayarlayın:

```json
"frontendIPConfigurations": [
    {
        "name": "LoadBalancerFrontEndIPv6",
        "properties": {
            "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('ipv6PublicIPAddressName'))]"
            }
        }
    }
]
```
Gerekli arka uç havuzlarını tanımlayın:
```json
"backendAddressPools": [
    {
        "name": "BackendPoolIPv4"
    },
    {
        "name": "BackendPoolIPv6"
    }
]
```
Tüm yük dengeleyici kurallarını tanımlayın:
```json
{
    "name": "LBRuleIPv6-46000",
    "properties": {
        "frontendIPConfiguration": {
            "id": "[variables('ipv6FrontEndIPConfigID')]"
        },
        "backendAddressPool": {
            "id": "[variables('ipv6LbBackendPoolID')]"
        },
        "protocol": "tcp",
        "frontendPort": 46000,
        "backendPort": 60001,
        "probe": {
            "id": "[variables('ipv4ipv6lbProbeID')]"
        }
    }
}
```
Son olarak, ölçek kümesi ağ özelliklerinin IPConfigurations bölümünde IPv6 havuzuna başvuru ekleyin:
```json
{
    "name": "ipv6IPConfig",
    "properties": {
        "privateIPAddressVersion": "IPv6",
        "loadBalancerBackendAddressPools": [
            {
                "id": "[variables('ipv6LbBackendPoolID')]"
            }
        ]
    }
}
```

## <a name="public-ipv4-per-virtual-machine"></a>Sanal makine başına genel IPv4
Genel olarak, Azure ölçek kümesi sanal makinelerinin kendi genel IP adresleri olması gerekmez. Senaryoların çoğunda, bir genel IP adresini yük dengeleyiciyle veya tek bir sanal makineyle (başka bir deyişle, sıçrama kutusu) ilişkilendirmek daha ekonomik ve güvenlidir; daha sonra o da, gelen bağlantıları gerektiği gibi ölçek kümesi sanal makinelerine yönlendirir (örneğin, gelen NAT kuralları aracılığıyla).

Öte yandan, bazı senaryolarda ölçek kümesi sanal makinelerinin kendi genel IP adresleri olması gerekir. Buna örnek olarak, konsolun oyunun fizik işlemlerini yapan bir bulut sanal makinesine doğrudan bağlanmasını gerektiren oyunları verebiliriz. Bir diğer örnek de, dağıtılmış bir veritabanında sanal makinelerin birbiriyle dış bağlantılar kurması gerektiği durumlardır.

### <a name="creating-a-scale-set-with-public-ip-per-virtual-machine"></a>Sanal makine başına bir genel IP ile ölçek kümesi oluşturma
CLI 2.0 ile her sanal makineye bir genel IP adresi atayan bir ölçek kümesi oluşturmak için, _vmss create_ komutuna _--public-ip-per-vm_ parametresini ekleyin. 

Azure şablonu kullanarak ölçek kümesi oluşturmak için, Microsoft.Compute/virtualMachineScaleSets kaynağı API sürümünün en az 2017-03-30 olduğundan emin olun ve ölçek kümesinin ipConfigurations bölümüne _publicIpAddressConfiguration_ JSON özelliği ekleyin. Örneğin:

```json
"publicIpAddressConfiguration": {
    "name": "pub1",
    "properties": {
      "idleTimeoutInMinutes": 15
    }
}
```
Örnek şablon: [201-vmss-public-ip-linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-public-ip-linux)

### <a name="querying-the-public-ip-addresses-of-the-virtual-machines-in-a-scale-set"></a>Ölçek kümesinde sanal makinelerin genel IP adreslerini sorgulama
CLI 2.0 kullanarak ölçek kümesi sanal makinelerine atanmış genel IP adreslerini listelemek için, _az vmss list-instance-public-ips_ komutunu kullanın.

Ayrıca, ölçek kümesi sanal makinelerine atanan genel IP adreslerini, [Azure Kaynak Gezgini](https://resources.azure.com)’ni ve Azure REST API’nin _2017-03-30_ veya üstü sürümlerini kullanarak da sorgulayabilirsiniz.

Kaynak kümesinin genel IP adreslerini Kaynak Gezgini’ni kullanarak görüntülemek için, ölçek kümenizin altındaki _publicipaddresses_ bölümüne bakın. Örneğin: https://resources.azure.com/subscriptions/_sub_kimliğiniz_/resourceGroups/_rg_değeriniz_/providers/Microsoft.Compute/virtualMachineScaleSets/_vmss_değeriniz_/publicipaddresses

```
GET https://management.azure.com/subscriptions/{your sub ID}/resourceGroups/{RG name}/providers/Microsoft.Compute/virtualMachineScaleSets/{scale set name}/publicipaddresses?api-version=2017-03-30
```

Örnek çıktı:
```json
{
  "value": [
    {
      "name": "pub1",
      "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/pipvmss/virtualMachines/0/networkInterfaces/pipvmssnic/ipConfigurations/yourvmssipconfig/publicIPAddresses/pub1",
      "etag": "W/\"a64060d5-4dea-4379-a11d-b23cd49a3c8d\"",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "ee8cb20f-af8e-4cd6-892f-441ae2bf701f",
        "ipAddress": "13.84.190.11",
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 15,
        "ipConfiguration": {
          "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/0/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig"
        }
      }
    },
    {
      "name": "pub1",
      "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/3/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig/publicIPAddresses/pub1",
      "etag": "W/\"5f6ff30c-a24c-4818-883c-61ebd5f9eee8\"",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "036ce266-403f-41bd-8578-d446d7397c2f",
        "ipAddress": "13.84.159.176",
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 15,
        "ipConfiguration": {
          "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/3/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig"
        }
      }
    }
```

## <a name="multiple-ip-addresses-per-nic"></a>NIC başına birden çok IP adresi
Ölçek kümesinde bir sanal makineye bağlanan her NIC ile ilişkilendirilmiş bir veya birden çok IP yapılandırması olabilir. Her yapılandırma bir özel IP adresine atanır. Her yapılandırmayla ilişkilendirilmiş bir genel IP adresi kaynağı da olabilir. NIC’ye kaç IP adresi atanabileceğini ve Azure aboneliğinde kaç genel IP adresi kullanabileceğinizi anlamak için, [Azure sınırlarına](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) bakın.

## <a name="multiple-nics-per-virtual-machine"></a>Sanal makine başına birden çok NIC
Sanal makinenin boyutuna bağlı olarak, sanal makine başına en çok 8 NIC’niz olabilir. Makine başına NIC sayısı üst sınırı, [Sanal makine boyutu makalesinde](../virtual-machines/windows/sizes.md) verilmiştir. Aşağıdaki örnek, sanal makine başına birden çok NIC girdisi ve birden çok genel IP gösteren bir ölçek kümesi ağ profilidir:
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
        "name": "nic1",
        "properties": {
            "primary": "true",
            "ipConfigurations": [
            {
                "name": "ip1",
                "properties": {
                "subnet": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                },
                "publicipaddressconfiguration": {
                    "name": "pub1",
                    "properties": {
                    "idleTimeoutInMinutes": 15
                    }
                },
                "loadBalancerInboundNatPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                    }
                ],
                "loadBalancerBackendAddressPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                    }
                ]
                }
            }
            ]
        }
        },
        {
        "name": "nic2",
        "properties": {
            "primary": "false",
            "ipConfigurations": [
            {
                "name": "ip1",
                "properties": {
                "subnet": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                },
                "publicipaddressconfiguration": {
                    "name": "pub1",
                    "properties": {
                    "idleTimeoutInMinutes": 15
                    }
                },
                "loadBalancerInboundNatPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                    }
                ],
                "loadBalancerBackendAddressPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                    }
                ]
                }
            }
            ]
        }
        }
    ]
}
```

## <a name="nsg-per-scale-set"></a>Ölçek kümesi başına NSG
Ağ Güvenlik Grupları, ölçek kümesi sanal makine özelliklerinin ağ arabirimi yapılandırması bölümüne bir başvuru eklemek yoluyla doğrudan ölçek kümesine uygulanabilir.

Örneğin: 
```
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
            "name": "nic1",
            "properties": {
                "primary": "true",
                "ipConfigurations": [
                    {
                        "name": "ip1",
                        "properties": {
                            "subnet": {
                                "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                            }
                "loadBalancerInboundNatPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                                }
                            ],
                            "loadBalancerBackendAddressPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                                }
                            ]
                        }
                    }
                ],
                "networkSecurityGroup": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/networkSecurityGroups/', variables('nsgName'))]"
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Sonraki adımlar
Azure sanal ağları hakkında daha fazla bilgi için, [bu belgelere](../virtual-network/virtual-networks-overview.md) bakın.

