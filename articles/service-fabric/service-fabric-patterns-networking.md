---
title: Azure Service Fabric için ağ desenleri | Microsoft Docs
description: Ortak ağ desenleri için Service Fabric ve Azure ağ özelliklerini kullanarak bir küme oluşturma işlemini açıklar.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/19/2018
ms.author: aljo
ms.openlocfilehash: d5aa09f3ff899766e6eb6d1784e4417f7b48eac0
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59049906"
---
# <a name="service-fabric-networking-patterns"></a>Service Fabric ağ desenleri
Azure Service Fabric kümenizi Azure diğer ağ özelliklerini tümleştirebilirsiniz. Bu makalede, biz, aşağıdaki özellikleri kullanırsınız kümeleri oluşturma işlemini gösterir:

- [Var olan sanal ağ veya alt ağ](#existingvnet)
- [Statik genel IP adresi](#staticpublicip)
- [Yalnızca iç yük dengeleyici](#internallb)
- [İç ve dış yük dengeleyici](#internalexternallb)

Service Fabric, standart sanal makine ölçek kümesinde çalışır. Bir sanal makine ölçek kümesinde kullanabileceğiniz işlevleri, bir Service Fabric kümesi ile kullanabilirsiniz. Sanal makine ölçek kümeleri ve Service Fabric için Azure Resource Manager şablonları ağ bölümleri birbirinin aynıdır. Mevcut bir sanal ağa dağıttıktan sonra Azure ExpressRoute, Azure VPN ağ geçidi, bir ağ güvenlik grubu ve sanal ağ eşlemesi gibi diğer ağ özelliklerini dahil etmek kolay bir işlemdir.

Service Fabric yönlerinden biri de diğer ağ özelliklerini benzersizdir. [Azure portalında](https://portal.azure.com) dahili olarak bir küme düğümleri ve uygulamalar hakkında bilgi almak için çağırmak için Service Fabric kaynak sağlayıcısını kullanır. Service Fabric kaynak sağlayıcısı yönetim uç noktasında HTTP ağ geçidi bağlantı noktası (varsayılan olarak, 19080 bağlantı noktası) ortak olarak erişilebilen gelen erişim gerektirir. [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) kümenizi yönetmek için yönetim uç noktasını kullanır. Service Fabric kaynak sağlayıcısı bu bağlantı noktası bilgileri kümeniz, sorgulama için Azure portalında görüntülemek için de kullanır. 

Bağlantı noktası 19080 Service Fabric kaynak Sağlayıcısı'ndan erişilebilir durumda değilse, bir ileti ister *düğümleri Nebyl Nalezen* portalda görünür ve düğüm ve uygulama listenize boş görünür. Azure portalında kümenizin görmek istiyorsanız, yük dengeleyicinizin genel IP adresi kullanıma sunması gerekir ve ağ güvenlik grubunuzu gelen bağlantı noktası 19080 trafiğe izin vermeniz gerekir. Kurulumunuzu bu gereksinimleri karşılamıyorsa, Azure portalında kümenizin durumunu görüntülemez.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="templates"></a>Şablonlar

Tüm Service Fabric şablonları bulunan [GitHub](https://github.com/Azure/service-fabric-scripts-and-templates/tree/master/templates/networking). Şablon olarak dağıtabilir olmalıdır-aşağıdaki PowerShell komutlarını kullanmaktır. Varolan bir Azure sanal ağı şablonu veya statik genel IP şablonu dağıtıyorsanız, önce okuma [ilk kurulum](#initialsetup) bu makalenin.

<a id="initialsetup"></a>
## <a name="initial-setup"></a>Başlangıç kurulumu

### <a name="existing-virtual-network"></a>Var olan sanal ağı

Aşağıdaki örnekte, biz ExistingRG-vnet adlı bir sanal ağınız ile başlayan **ExistingRG** kaynak grubu. Alt ağ, varsayılan olarak adlandırılır. Standart bir sanal makine (VM) oluşturmak için Azure portalı kullandığınızda, bu varsayılan kaynaklar oluşturulur. VM oluşturmadan bir sanal ağ ve alt ağ oluşturabilirsiniz, ancak mevcut bir sanal ağa bir kümeyi eklemeyi ana amacı diğer Vm'lere ağ bağlantısı sağlamaktır. VM oluşturma bir sanal ağınız genellikle nasıl kullanıldığını iyi bir örnek sağlar. Service Fabric kümenizi yalnızca iç yük dengeleyici, genel bir IP adresi olmayan kullanıyorsa VM ve kendi genel IP güvenli kullanabileceğiniz *atlama kutusunu*.

### <a name="static-public-ip-address"></a>Statik genel IP adresi

Genellikle bir statik genel IP adresi atandığı VM veya Vm'leri ayrı olarak yönetilen ayrılmış bir kaynak ' dir. (Kendisi Service Fabric küme kaynağı grubuna karşılık olarak), ayrılmış bir ağ kaynak grubuna sağlanır. Aynı gruptaki ExistingRG kaynak, Azure portalında veya PowerShell kullanarak staticIP1 adlı statik genel IP adresi oluşturun:

```powershell
PS C:\Users\user> New-AzPublicIpAddress -Name staticIP1 -ResourceGroupName ExistingRG -Location westus -AllocationMethod Static -DomainNameLabel sfnetworking

Name                     : staticIP1
ResourceGroupName        : ExistingRG
Location                 : westus
Id                       : /subscriptions/1237f4d2-3dce-1236-ad95-123f764e7123/resourceGroups/ExistingRG/providers/Microsoft.Network/publicIPAddresses/staticIP1
Etag                     : W/"fc8b0c77-1f84-455d-9930-0404ebba1b64"
ResourceGuid             : 77c26c06-c0ae-496c-9231-b1a114e08824
ProvisioningState        : Succeeded
Tags                     :
PublicIpAllocationMethod : Static
IpAddress                : 40.83.182.110
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : null
DnsSettings              : {
                             "DomainNameLabel": "sfnetworking",
                             "Fqdn": "sfnetworking.westus.cloudapp.azure.com"
                           }
```

### <a name="service-fabric-template"></a>Service Fabric şablonu

Bu makaledeki örneklerde, Service Fabric template.json kullanırız. Küme oluşturmadan önce portaldan şablonunu indirmek için standart portal Sihirbazı'nı kullanabilirsiniz. Aşağıdakilerden birini de kullanabilirsiniz [örnek şablonlarından](https://github.com/Azure-Samples/service-fabric-cluster-templates)gibi [güvenli beş düğümlü bir Service Fabric kümesi](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure).

<a id="existingvnet"></a>
## <a name="existing-virtual-network-or-subnet"></a>Var olan sanal ağ veya alt ağ

1. Alt ağ parametresi var olan alt ağ adını değiştirin ve ardından mevcut sanal ağa başvurmak için iki yeni parametreler eklendi:

    ```json
        "subnet0Name": {
                "type": "string",
                "defaultValue": "default"
            },
            "existingVNetRGName": {
                "type": "string",
                "defaultValue": "ExistingRG"
            },

            "existingVNetName": {
                "type": "string",
                "defaultValue": "ExistingRG-vnet"
            },
            /*
            "subnet0Name": {
                "type": "string",
                "defaultValue": "Subnet-0"
            },
            "subnet0Prefix": {
                "type": "string",
                "defaultValue": "10.0.0.0/24"
            },*/
    ```

2. Açıklama `nicPrefixOverride` özniteliği `Microsoft.Compute/virtualMachineScaleSets`mevcut alt ağı kullanıyorsanız ve bu değişkeni 1. adım, devre dışı olduğundan.

    ```json
            /*"nicPrefixOverride": "[parameters('subnet0Prefix')]",*/
    ```

3. Değişiklik `vnetID` varolan bir sanal ağa işaret edecek şekilde değişkeni:

    ```json
            /*old "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",*/
            "vnetID": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingVNetRGName'), '/providers/Microsoft.Network/virtualNetworks/', parameters('existingVNetName'))]",
    ```

4. Kaldırma `Microsoft.Network/virtualNetworks` kaynaklarınızdan, bu nedenle Azure oluşturmaz yeni bir sanal ağ:

    ```json
    /*{
    "apiVersion": "[variables('vNetApiVersion')]",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[parameters('virtualNetworkName')]",
    "location": "[parameters('computeLocation')]",
    "properties": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "subnets": [
            {
                "name": "[parameters('subnet0Name')]",
                "properties": {
                    "addressPrefix": "[parameters('subnet0Prefix')]"
                }
            }
        ]
    },
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    }
    },*/
    ```

5. Sanal ağdan yorum `dependsOn` özniteliği `Microsoft.Compute/virtualMachineScaleSets`, yeni bir sanal ağ oluşturma ile ilgili bağlı olmayan:

    ```json
    "apiVersion": "[variables('vmssApiVersion')]",
    "type": "Microsoft.Computer/virtualMachineScaleSets",
    "name": "[parameters('vmNodeType0Name')]",
    "location": "[parameters('computeLocation')]",
    "dependsOn": [
        /*"[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]",
        */
        "[Concat('Microsoft.Storage/storageAccounts/', variables('uniqueStringArray0')[0])]",

    ```

6. Şablonu dağıtın:

    ```powershell
    New-AzResourceGroup -Name sfnetworkingexistingvnet -Location westus
    New-AzResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingexistingvnet -TemplateFile C:\SFSamples\Final\template\_existingvnet.json
    ```

    Dağıtımdan sonra sanal ağınızı yeni içermelidir ölçek kümesinin Vm'leri. Sanal makine ölçek kümesi düğüm türü, var olan sanal ağ ve alt göstermelidir. Sanal ağda zaten olan bir sanal Makineye erişmek için Uzak Masaüstü Protokolü (RDP) kullanabilirsiniz ve yeni ölçek ping Vm'leri ayarlayın:

    ```
    C:>\Users\users>ping 10.0.0.5 -n 1
    C:>\Users\users>ping NOde1000000 -n 1
    ```

Başka bir örnek için bkz: [, Service Fabric'e özgü değildir](https://github.com/gbowerman/azure-myriad/tree/master/existing-vnet).


<a id="staticpublicip"></a>
## <a name="static-public-ip-address"></a>Statik genel IP adresi

1. Mevcut bir statik IP kaynak grubu adı, adı ve tam etki alanı adı (FQDN) parametrelerini ekleyin:

    ```json
    "existingStaticIPResourceGroup": {
                "type": "string"
            },
            "existingStaticIPName": {
                "type": "string"
            },
            "existingStaticIPDnsFQDN": {
                "type": "string"
    }
    ```

2. Kaldırma `dnsName` parametresi. (Statik IP adresi zaten varsa.)

    ```json
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

3. Mevcut bir statik IP adresi başvurmak için bir değişken ekleyin:

    ```json
    "existingStaticIP": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingStaticIPResourceGroup'), '/providers/Microsoft.Network/publicIPAddresses/', parameters('existingStaticIPName'))]",
    ```

4. Kaldırma `Microsoft.Network/publicIPAddresses` kaynaklarınızdan, bu nedenle Azure oluşturmaz yeni bir IP adresi:

    ```json
    /*
    {
        "apiVersion": "[variables('publicIPApiVersion')]",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[concat(parameters('lbIPName'),)'-', '0')]",
        "location": "[parameters('computeLocation')]",
        "properties": {
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsName')]"
            },
            "publicIPAllocationMethod": "Dynamic"        
        },
        "tags": {
            "resourceType": "Service Fabric",
            "clusterName": "[parameters('clusterName')]"
        }
    }, */
    ```

5. IP adresinden yorum `dependsOn` özniteliği `Microsoft.Network/loadBalancers`, yeni bir IP adresi oluşturma ile ilgili bağlı olmayan:

    ```json
    "apiVersion": "[variables('lbIPApiVersion')]",
    "type": "Microsoft.Network/loadBalancers",
    "name": "[concat('LB', '-', parameters('clusterName'), '-', parameters('vmNodeType0Name'))]",
    "location": "[parameters('computeLocation')]",
    /*
    "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', concat(parameters('lbIPName'), '-', '0'))]"
    ], */
    "properties": {
    ```

6. İçinde `Microsoft.Network/loadBalancers` kaynak, değişiklik `publicIPAddress` öğesinin `frontendIPConfigurations` yeni oluşturulan bir tane yerine var olan statik IP adresi başvurmak için:

    ```json
                "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                "publicIPAddress": {
                                    /*"id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"*/
                                    "id": "[variables('existingStaticIP')]"
                                }
                            }
                        }
                    ],
    ```

7. İçinde `Microsoft.ServiceFabric/clusters` kaynak, değişiklik `managementEndpoint` statik IP adresini DNS FQDN'sine. Güvenli bir küme kullanıyorsanız, değiştirdiğiniz emin *http://* için *https://*. (Bu adım yalnızca Service Fabric kümeleri için geçerli olduğunu unutmayın. Bir sanal makine ölçek kümesi kullanıyorsanız, bu adımı atlayın.)

    ```json
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',parameters('existingStaticIPDnsFQDN'),':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

8. Şablonu dağıtın:

    ```powershell
    New-AzResourceGroup -Name sfnetworkingstaticip -Location westus

    $staticip = Get-AzPublicIpAddress -Name staticIP1 -ResourceGroupName ExistingRG

    $staticip

    New-AzResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingstaticip -TemplateFile C:\SFSamples\Final\template\_staticip.json -existingStaticIPResourceGroup $staticip.ResourceGroupName -existingStaticIPName $staticip.Name -existingStaticIPDnsFQDN $staticip.DnsSettings.Fqdn
    ```

Dağıtımdan sonra Yük dengeleyicinizin genel statik IP adresi başka bir kaynak grubundan bağlı olduğunu görebilirsiniz. Service Fabric istemci bağlantısı uç noktası ve [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) DNS FQDN uç noktasına statik IP adresi.

<a id="internallb"></a>
## <a name="internal-only-load-balancer"></a>Yalnızca iç yük dengeleyici

Bu senaryo, bir yalnızca iç yük dengeleyiciyle varsayılan Service Fabric şablondaki dış yük dengeleyici yerini alır. Azure Portal ve Service Fabric kaynak sağlayıcısı için sonuçları için önceki bölüme bakın.

1. Kaldırma `dnsName` parametresi. (Bu gerekli değildir.)

    ```json
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

2. İsteğe bağlı olarak, bir statik ayırma yöntemini kullanırsanız, bir statik IP adresi parametre ekleyebilirsiniz. Dinamik ayırma yöntemini kullanırsanız, bu adımı gerekmez.

    ```json
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

3. Kaldırma `Microsoft.Network/publicIPAddresses` kaynaklarınızdan, bu nedenle Azure oluşturmaz yeni bir IP adresi:

    ```json
    /*
    {
        "apiVersion": "[variables('publicIPApiVersion')]",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[concat(parameters('lbIPName'),)'-', '0')]",
        "location": "[parameters('computeLocation')]",
        "properties": {
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsName')]"
            },
            "publicIPAllocationMethod": "Dynamic"        
        },
        "tags": {
            "resourceType": "Service Fabric",
            "clusterName": "[parameters('clusterName')]"
        }
    }, */
    ```

4. IP adresini kaldırın `dependsOn` özniteliği `Microsoft.Network/loadBalancers`, yeni bir IP adresi oluşturma ile ilgili bağlı değilsiniz. Sanal ağ ekleme `dependsOn` yük dengeleyici artık alt ağdan sanal ağa bağlı olduğundan özniteliği:

    ```json
                "apiVersion": "[variables('lbApiVersion')]",
                "type": "Microsoft.Network/loadBalancers",
                "name": "[concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'))]",
                "location": "[parameters('computeLocation')]",
                "dependsOn": [
                    /*"[concat('Microsoft.Network/publicIPAddresses/',concat(parameters('lbIPName'),'-','0'))]"*/
                    "[concat('Microsoft.Network/virtualNetworks/',parameters('virtualNetworkName'))]"
                ],
    ```

5. Load balancer'ın değiştirme `frontendIPConfigurations` ayarı kullanarak bir `publicIPAddress`, bir alt ağ kullanarak ve `privateIPAddress`. `privateIPAddress` önceden tanımlanmış statik iç IP adresi kullanır. Dinamik IP adresi kullanmak için kaldırmak `privateIPAddress` öğesini ve ardından değişiklik `privateIPAllocationMethod` için **dinamik**.

    ```json
                "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                /*
                                "publicIPAddress": {
                                    "id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"
                                } */
                                "subnet" :{
                                    "id": "[variables('subnet0Ref')]"
                                },
                                "privateIPAddress": "[parameters('internalLBAddress')]",
                                "privateIPAllocationMethod": "Static"
                            }
                        }
                    ],
    ```

6. İçinde `Microsoft.ServiceFabric/clusters` kaynak, değişiklik `managementEndpoint` iç yük dengeleyici adresine yönlendirin. Güvenli bir küme kullanıyorsanız, değiştirdiğiniz unutmayın *http://* için *https://*. (Bu adım yalnızca Service Fabric kümeleri için geçerli olduğunu unutmayın. Bir sanal makine ölçek kümesi kullanıyorsanız, bu adımı atlayın.)

    ```json
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',reference(variables('lbID0')).frontEndIPConfigurations[0].properties.privateIPAddress,':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

7. Şablonu dağıtın:

    ```powershell
    New-AzResourceGroup -Name sfnetworkinginternallb -Location westus

    New-AzResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternallb -TemplateFile C:\SFSamples\Final\template\_internalonlyLB.json
    ```

Dağıtımdan sonra Yük dengeleyicinizin 10.0.0.250 statik özel IP adresini kullanır. Bu aynı sanal ağdaki başka bir makine varsa, iç ağa gidebilirsiniz [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) uç noktası. Yük dengeleyicinin arkasındaki düğümlerinden biri için bağlandığını unutmayın.

<a id="internalexternallb"></a>
## <a name="internal-and-external-load-balancer"></a>İç ve dış yük dengeleyici

Bu senaryoda, var olan tek bir düğüm türü dış yük dengeleyici ile başlayın ve aynı düğüm türü için bir iç yük dengeleyici ekleyin. Bir arka uç adres havuzuna bağlı bir arka uç bağlantı noktası yalnızca bir tek bir yük dengeleyiciye atanabilir. Hangi yük dengeleyici, uygulama bağlantı noktaları gerekir seçin ve hangi yük dengeleyici yönetim uç (bağlantı noktaları 19000 ve 19080) sahip. İç yük dengeleyici ile ilgili yönetim uç noktalarını kullanırsanız, Service Fabric kaynak sağlayıcısı kısıtlamaları makalenin önceki bölümlerinde açıklanan unutmayın. Örnekte kullanıyoruz, yönetim uç noktaları için dış yük dengeleyicide kalır. Ayrıca bir bağlantı noktası 80 uygulama bağlantı noktasını ekleyin ve iç yük dengeleyicide yerleştirin.

İki düğüm türü bir kümede bulunan bir düğüm türü için dış yük dengeleyicide ' dir. İç yük dengeleyici ile ilgili diğer düğüm türü değil. İki düğüm türü küme (Bu, iki yük dengeleyici ile birlikte gelir) portal tarafından oluşturulan iki düğüm türü şablonunda kullanmak için ikinci yük dengeleyici için iç yük dengeleyici geçin. Daha fazla bilgi için [yalnızca iç yük dengeleyici](#internallb) bölümü.

1. Statik iç yük dengeleyici IP adresi parametre ekleyin. (Bu makalenin önceki bölümlerinde dinamik IP adresi kullanımıyla ilgili notları için bkz.)

    ```json
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

2. Bir uygulama bağlantı noktası 80'parametresini ekleyin.

3. Var olan iç sürümlerini kopyalayın ve yapıştırın değişkenleri, ağ ekleyin ve eklemek için "-Int" adı:

    ```json
    /* Add internal load balancer networking variables */
            "lbID0-Int": "[resourceId('Microsoft.Network/loadBalancers', concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'), '-Internal'))]",
            "lbIPConfig0-Int": "[concat(variables('lbID0-Int'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
            "lbPoolID0-Int": "[concat(variables('lbID0-Int'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
            "lbProbeID0-Int": "[concat(variables('lbID0-Int'),'/probes/FabricGatewayProbe')]",
            "lbHttpProbeID0-Int": "[concat(variables('lbID0-Int'),'/probes/FabricHttpGatewayProbe')]",
            "lbNatPoolID0-Int": "[concat(variables('lbID0-Int'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]",
            /* Internal load balancer networking variables end */
    ```

4. Uygulama bağlantı noktası 80'i kullanan portal tarafından oluşturulan şablonla başlatırsanız, varsayılan portal şablonu AppPort1 ekler (bağlantı noktası 80) dış yük dengeleyici üzerindeki. Bu durumda, dış yük dengeleyiciden AppPort1 Kaldır `loadBalancingRules` ve araştırmaları, iç yük dengeleyiciye ekleyebilirsiniz:

    ```json
    "loadBalancingRules": [
        {
            "name": "LBHttpRule",
            "properties":{
                "backendAddressPool": {
                    "id": "[variables('lbPoolID0')]"
                },
                "backendPort": "[parameters('nt0fabricHttpGatewayPort')]",
                "enableFloatingIP": "false",
                "frontendIPConfiguration": {
                    "id": "[variables('lbIPConfig0')]"            
                },
                "frontendPort": "[parameters('nt0fabricHttpGatewayPort')]",
                "idleTimeoutInMinutes": "5",
                "probe": {
                    "id": "[variables('lbHttpProbeID0')]"
                },
                "protocol": "tcp"
            }
        } /* Remove AppPort1 from the external load balancer.
        {
            "name": "AppPortLBRule1",
            "properties": {
                "backendAddressPool": {
                    "id": "[variables('lbPoolID0')]"
                },
                "backendPort": "[parameters('loadBalancedAppPort1')]",
                "enableFloatingIP": "false",
                "frontendIPConfiguration": {
                    "id": "[variables('lbIPConfig0')]"            
                },
                "frontendPort": "[parameters('loadBalancedAppPort1')]",
                "idleTimeoutInMinutes": "5",
                "probe": {
                    "id": "[concate(variables('lbID0'), '/probes/AppPortProbe1')]"
                },
                "protocol": "tcp"
            }
        }*/

    ],
    "probes": [
        {
            "name": "FabricGatewayProbe",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('nt0fabricTcpGatewayPort')]",
                "protocol": "tcp"
            }
        },
        {
            "name": "FabricHttpGatewayProbe",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('nt0fabricHttpGatewayPort')]",
                "protocol": "tcp"
            }
        } /* Remove AppPort1 from the external load balancer.
        {
            "name": "AppPortProbe1",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('loadBalancedAppPort1')]",
                "protocol": "tcp"
            }
        } */

    ],
    "inboundNatPools": [
    ```

5. İkinci bir ekleme `Microsoft.Network/loadBalancers` kaynak. Oluşturulan iç yük dengeleyiciye benzer [yalnızca iç yük dengeleyici](#internallb) bölümü, ancak "-Int" Yük Dengeleyici değişkenleri ve yalnızca uygulama bağlantı noktası 80 uygular. Bu da kaldırır `inboundNatPools`, herkese açık yük dengeleyici üzerinde RDP uç noktaları korumak için. İç yük dengeleyici üzerinde RDP isterseniz taşıma `inboundNatPools` dışarıdan yük dengeleyici bu iç yük dengeleyici için:

    ```json
            /* Add a second load balancer, configured with a static privateIPAddress and the "-Int" load balancer variables. */
            {
                "apiVersion": "[variables('lbApiVersion')]",
                "type": "Microsoft.Network/loadBalancers",
                /* Add "-Internal" to the name. */
                "name": "[concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'), '-Internal')]",
                "location": "[parameters('computeLocation')]",
                "dependsOn": [
                    /* Remove public IP dependsOn, add vnet dependsOn
                    "[concat('Microsoft.Network/publicIPAddresses/',concat(parameters('lbIPName'),'-','0'))]"
                    */
                    "[concat('Microsoft.Network/virtualNetworks/',parameters('virtualNetworkName'))]"
                ],
                "properties": {
                    "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                /* Switch from Public to Private IP address
                                */
                                "publicIPAddress": {
                                    "id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"
                                }
                                */
                                "subnet" :{
                                    "id": "[variables('subnet0Ref')]"
                                },
                                "privateIPAddress": "[parameters('internalLBAddress')]",
                                "privateIPAllocationMethod": "Static"
                            }
                        }
                    ],
                    "backendAddressPools": [
                        {
                            "name": "LoadBalancerBEAddressPool",
                            "properties": {}
                        }
                    ],
                    "loadBalancingRules": [
                        /* Add the AppPort rule. Be sure to reference the "-Int" versions of backendAddressPool, frontendIPConfiguration, and the probe variables. */
                        {
                            "name": "AppPortLBRule1",
                            "properties": {
                                "backendAddressPool": {
                                    "id": "[variables('lbPoolID0-Int')]"
                                },
                                "backendPort": "[parameters('loadBalancedAppPort1')]",
                                "enableFloatingIP": "false",
                                "frontendIPConfiguration": {
                                    "id": "[variables('lbIPConfig0-Int')]"
                                },
                                "frontendPort": "[parameters('loadBalancedAppPort1')]",
                                "idleTimeoutInMinutes": "5",
                                "probe": {
                                    "id": "[concat(variables('lbID0-Int'),'/probes/AppPortProbe1')]"
                                },
                                "protocol": "tcp"
                            }
                        }
                    ],
                    "probes": [
                    /* Add the probe for the app port. */
                    {
                            "name": "AppPortProbe1",
                            "properties": {
                                "intervalInSeconds": 5,
                                "numberOfProbes": 2,
                                "port": "[parameters('loadBalancedAppPort1')]",
                                "protocol": "tcp"
                            }
                        }
                    ],
                    "inboundNatPools": [
                    ]
                },
                "tags": {
                    "resourceType": "Service Fabric",
                    "clusterName": "[parameters('clusterName')]"
                }
            },
    ```

6. İçinde `networkProfile` için `Microsoft.Compute/virtualMachineScaleSets` kaynak, iç arka uç adres havuzu ekleme:

    ```json
    "loadBalancerBackendAddressPools": [
                                                        {
                                                            "id": "[variables('lbPoolID0')]"
                                                        },
                                                        {
                                                            /* Add internal BE pool */
                                                            "id": "[variables('lbPoolID0-Int')]"
                                                        }
    ],
    ```

7. Şablonu dağıtın:

    ```powershell
    New-AzResourceGroup -Name sfnetworkinginternalexternallb -Location westus

    New-AzResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternalexternallb -TemplateFile C:\SFSamples\Final\template\_internalexternalLB.json
    ```

Dağıtımdan sonra kaynak grubunda iki yük Dengeleyiciler görebilirsiniz. Yük Dengeleyiciler göz atarsanız, genel bir IP adresi atanmış genel IP adresi ve yönetim uç noktalarını (bağlantı noktaları 19000 ve 19080) görebilirsiniz. İç yük dengeleyiciye atanan statik iç IP adresi ve uygulama uç noktası (bağlantı noktası 80) de görebilirsiniz. Her iki yük Dengeleyiciler, aynı sanal makine ölçek kümesi arka uç havuzunu kullanın.

## <a name="next-steps"></a>Sonraki adımlar
[Küme oluşturma](service-fabric-cluster-creation-via-arm.md) ternalLB.json
    ```

Dağıtımdan sonra kaynak grubunda iki yük Dengeleyiciler görebilirsiniz. Yük Dengeleyiciler göz atarsanız, genel bir IP adresi atanmış genel IP adresi ve yönetim uç noktalarını (bağlantı noktaları 19000 ve 19080) görebilirsiniz. İç yük dengeleyiciye atanan statik iç IP adresi ve uygulama uç noktası (bağlantı noktası 80) de görebilirsiniz. Her iki yük Dengeleyiciler, aynı sanal makine ölçek kümesi arka uç havuzunu kullanın.

## <a name="next-steps"></a>Sonraki adımlar
[Küme oluşturma](service-fabric-cluster-creation-via-arm.md)
