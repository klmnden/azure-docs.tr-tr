---
title: Azure Service Fabric için ağ desenleri | Microsoft Docs
description: Service Fabric ve Azure ağ özellikleri kullanılarak bir küme oluşturma için ortak ağ desenleri açıklar.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/19/2018
ms.author: ryanwi
ms.openlocfilehash: b180e62804b875ca4547a9d09f19efff32ae0cd9
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="service-fabric-networking-patterns"></a>Service Fabric ağ desenleri
Azure Service Fabric kümesi Azure diğer ağ özelliklerini ile tümleştirebilirsiniz. Bu makalede, sizi, aşağıdaki özellikleri kullanan bir küme nasıl oluşturulacağını gösterir:

- [Var olan sanal ağ veya alt ağ](#existingvnet)
- [Statik genel IP adresi](#staticpublicip)
- [Yalnızca dahili yük dengeleyici](#internallb)
- [İç ve dış yük dengeleyici](#internalexternallb)

Service Fabric standart sanal makine ölçek kümesindeki çalışır. Bir sanal makine ölçek kümesindeki kullanabileceğiniz herhangi bir işlevsellik, Service Fabric kümesi ile kullanabilirsiniz. Sanal makine ölçek kümeleri ve Service Fabric için Azure Resource Manager şablonları ağ bölümlerini aynıdır. Mevcut bir sanal ağa dağıttıktan sonra Azure ExpressRoute, Azure VPN ağ geçidi, bir ağ güvenlik grubu ve sanal ağ eşlemesi gibi diğer ağ özelliklerini içerecek şekilde kolaydır.

Service Fabric diğer ağ özelliklerini tek bir yönüne içinde benzersizdir. [Azure portal](https://portal.azure.com) dahili olarak bir küme düğümlerini ve uygulamalar hakkında bilgi almak için aranacak Service Fabric kaynak sağlayıcısı kullanır. Service Fabric kaynak sağlayıcısı yönetim uç HTTP ağ geçidi bağlantı noktası (varsayılan olarak 19080, bağlantı noktası) için genel olarak erişilebilir gelen erişim gerektirir. [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) kümenizi yönetmek için yönetim uç noktası kullanır. Service Fabric kaynak sağlayıcısı bu bağlantı noktası için küme hakkında bilgileri sorgulama Azure portalında görüntülemek için de kullanır. 

Bağlantı noktası 19080 Service Fabric kaynak sağlayıcısından erişilebilir durumda değilse, bir ileti ister *düğümleri bulunamadı* Portalı'nda görüntülenir ve düğüm ve uygulama listenizi boş görünür. Kümenizi Azure portalında görmek istiyorsanız, bir ortak IP adresi, yük dengeleyici kullanıma gerekir ve ağ güvenlik grubu gelen bağlantı noktası 19080 trafiğe izin vermelidir. Azure portalı kurulumunuzu bu gereksinimlerini karşılamıyorsa, küme durumunu görüntülemez.

## <a name="templates"></a>Şablonlar

Tüm Service Fabric şablonları bulunan [GitHub](https://github.com/Azure/service-fabric-scripts-and-templates/tree/master/templates/networking). Şablon olarak dağıtamaz olmalıdır-aşağıdaki PowerShell komutlarını kullanmaktır. Var olan Azure Virtual Network şablonu veya statik genel IP şablonu dağıtıyorsanız, önce okuma [ilk kurulum](#initialsetup) bu makalenin.

<a id="initialsetup"></a>
## <a name="initial-setup"></a>İlk kurulumu

### <a name="existing-virtual-network"></a>Var olan sanal ağ

Aşağıdaki örnekte, biz ExistingRG-vnet adlı varolan bir sanal ağı Başlat **ExistingRG** kaynak grubu. Alt ağ varsayılan olarak adlandırılır. Standart bir sanal makine (VM) oluşturmak için Azure Portalı'nı kullandığınızda bu varsayılan kaynakları oluşturulur. VM oluşturmak zorunda kalmadan sanal ağ ve alt oluşturabilirsiniz, ancak mevcut bir sanal ağa bir kümeyi eklemeyi ana amacı diğer VM'ler için ağ bağlantısı sağlamaktır. VM oluşturma, varolan bir sanal ağı genellikle nasıl kullanıldığını, iyi bir örnek verir. Service Fabric kümesi yalnızca bir iç yük dengeleyici, genel bir IP adresi olmadan kullanıyorsa, VM ve genel IP güvenli kullanabileceğiniz *kutusunu atlama*.

### <a name="static-public-ip-address"></a>Statik genel IP adresi

Bir statik genel IP adresi, genellikle için atanan VM ya da sanal makineleri ayrı olarak yönetilen ayrılmış bir kaynak değil. (Kendisini Service Fabric küme kaynağı grubuna aygıtlardır), ayrılmış bir ağ kaynak grubunda sağlanır. Aynı ExistingRG kaynak grubunda, Azure portalında veya PowerShell kullanarak staticIP1 adlı bir statik genel IP adresi oluşturun:

```powershell
PS C:\Users\user> New-AzureRmPublicIpAddress -Name staticIP1 -ResourceGroupName ExistingRG -Location westus -AllocationMethod Static -DomainNameLabel sfnetworking

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

Bu makaledeki örneklerde, Service Fabric template.json kullanırız. Küme oluşturmadan önce şablonu portalından karşıdan yüklemek için standart portal Sihirbazı'nı kullanabilirsiniz. Aşağıdakilerden birini de kullanabilirsiniz [örnek şablonlarından](https://github.com/Azure-Samples/service-fabric-cluster-templates)gibi [güvenli beş düğümlü Service Fabric kümesi](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure).

<a id="existingvnet"></a>
## <a name="existing-virtual-network-or-subnet"></a>Var olan sanal ağ veya alt ağ

1. Alt parametre mevcut alt adını değiştirin ve ardından mevcut bir sanal ağ başvurmak için iki yeni parametreler ekleyin:

    ```
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


2. Değişiklik `vnetID` varolan bir sanal ağa işaret edecek şekilde değişkeni:

    ```
            /*old "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",*/
            "vnetID": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingVNetRGName'), '/providers/Microsoft.Network/virtualNetworks/', parameters('existingVNetName'))]",
    ```

3. Kaldırma `Microsoft.Network/virtualNetworks` kaynaklarınızdan, bu nedenle Azure oluşturmaz yeni bir sanal ağ:

    ```
    /*{
    "apiVersion": "[variables('vNetApiVersion')]",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[parameters('virtualNetworkName')]",
    "location": "[parameters('computeLocation')]",
    "properities": {
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

4. Sanal ağdan çıkışı açıklama `dependsOn` özniteliği `Microsoft.Compute/virtualMachineScaleSets`, yeni bir sanal ağ oluşturma ile ilgili bağımlı yok:

    ```
    "apiVersion": "[variables('vmssApiVersion')]",
    "type": "Microsoft.Computer/virtualMachineScaleSets",
    "name": "[parameters('vmNodeType0Name')]",
    "location": "[parameters('computeLocation')]",
    "dependsOn": [
        /*"[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]",
        */
        "[Concat('Microsoft.Storage/storageAccounts/', variables('uniqueStringArray0')[0])]",

    ```

5. Şablon dağıtma:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkingexistingvnet -Location westus
    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingexistingvnet -TemplateFile C:\SFSamples\Final\template\_existingvnet.json
    ```

    Dağıtımdan sonra sanal ağınızı yeni içermelidir ölçek kümesi VM. Sanal makine ölçek kümesi düğüm türü, varolan sanal ağ ve alt göstermesi gerekir. Sanal ağ zaten olan VM erişmek için Uzak Masaüstü Protokolü (RDP) de kullanabilirsiniz ve yeni ölçek ping işlemi yapmak için sanal makineleri ayarlayın:

    ```
    C:>\Users\users>ping 10.0.0.5 -n 1
    C:>\Users\users>ping NOde1000000 -n 1
    ```

Başka bir örnek için bkz: [Service Fabric belirli olmayan bir](https://github.com/gbowerman/azure-myriad/tree/master/existing-vnet).


<a id="staticpublicip"></a>
## <a name="static-public-ip-address"></a>Statik genel IP adresi

1. Mevcut bir statik IP kaynak grubunun adı, adı ve tam etki alanı adı (FQDN) parametreleri ekleyin:

    ```
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

    ```
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

3. Var olan bir statik IP adresi başvurusu için bir değişken ekleyin:

    ```
    "existingStaticIP": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingStaticIPResourceGroup'), '/providers/Microsoft.Network/publicIPAddresses/', parameters('existingStaticIPName'))]",
    ```

4. Kaldırma `Microsoft.Network/publicIPAddresses` kaynaklarınızdan, bu nedenle Azure oluşturmaz yeni bir IP adresi:

    ```
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

5. IP adresinden çıkışı açıklama `dependsOn` özniteliği `Microsoft.Network/loadBalancers`, yeni bir IP adresi oluşturma bağımlı yok:

    ```
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

6. İçinde `Microsoft.Network/loadBalancers` kaynak, değişiklik `publicIPAddress` öğesinin `frontendIPConfigurations` varolan statik IP adresi yerine yeni oluşturulan bir başvurmak için:

    ```
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

7. İçinde `Microsoft.ServiceFabric/clusters` kaynak, değişiklik `managementEndpoint` statik IP adresinin DNS FQDN için. Güvenli bir küme kullanıyorsanız, değiştirdiğinizden emin olun *http://* için *https://*. (Bu adım yalnızca Service Fabric kümeleri için geçerli olduğunu unutmayın. Bir sanal makine ölçek kümesini kullanıyorsanız, bu adımı atlayın.)

    ```
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',parameters('existingStaticIPDnsFQDN'),':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

8. Şablon dağıtma:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkingstaticip -Location westus

    $staticip = Get-AzureRmPublicIpAddress -Name staticIP1 -ResourceGroupName ExistingRG

    $staticip

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingstaticip -TemplateFile C:\SFSamples\Final\template\_staticip.json -existingStaticIPResourceGroup $staticip.ResourceGroupName -existingStaticIPName $staticip.Name -existingStaticIPDnsFQDN $staticip.DnsSettings.Fqdn
    ```

Dağıtımdan sonra Yük Dengeleyici diğer bir kaynak grubundan ortak statik IP adresine bağlı olduğunu görebilirsiniz. Service Fabric istemci bağlantı uç noktasının ve [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) DNS FQDN uç noktasına statik IP adresi.

<a id="internallb"></a>
## <a name="internal-only-load-balancer"></a>Yalnızca dahili yük dengeleyici

Bu senaryo varsayılan Service Fabric şablonunda dış yük dengeleyici yalnızca iç yük dengeleyici ile değiştirir. Azure portal ve Service Fabric kaynak sağlayıcısı için uygulamaları için önceki bölümüne bakın.

1. Kaldırma `dnsName` parametresi. (Bu gerekli değildir.)

    ```
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

2. İsteğe bağlı olarak, statik ayırma yöntemi kullanırsanız, bir statik IP adresi parametre ekleyebilirsiniz. Dinamik ayırma yöntemini kullanırsanız, bu adımı gerekmez.

    ```
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

3. Kaldırma `Microsoft.Network/publicIPAddresses` kaynaklarınızdan, bu nedenle Azure oluşturmaz yeni bir IP adresi:

    ```
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

4. IP adresini kaldırın `dependsOn` özniteliği `Microsoft.Network/loadBalancers`, yeni bir IP adresi oluşturma bağımlı yok. Sanal ağ ekleme `dependsOn` yük dengeleyici şimdi alt ağdan sanal ağa bağımlı olduğundan dolayı özniteliği:

    ```
                "apiVersion": "[variables('lbApiVersion')]",
                "type": "Microsoft.Network/loadBalancers",
                "name": "[concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'))]",
                "location": "[parameters('computeLocation')]",
                "dependsOn": [
                    /*"[concat('Microsoft.Network/publicIPAddresses/',concat(parameters('lbIPName'),'-','0'))]"*/
                    "[concat('Microsoft.Network/virtualNetworks/',parameters('virtualNetworkName'))]"
                ],
    ```

5. Yük dengeleyicinin değiştirme `frontendIPConfigurations` kullanımından ayarını bir `publicIPAddress`, bir alt ağ kullanarak ve `privateIPAddress`. `privateIPAddress` önceden tanımlanmış statik iç IP adresi kullanır. Dinamik IP adresi kullanmak için kaldırmak `privateIPAddress` öğesini ve ardından değişiklik `privateIPAllocationMethod` için **dinamik**.

    ```
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

6. İçinde `Microsoft.ServiceFabric/clusters` kaynak, değişiklik `managementEndpoint` iç yük dengeleyici adresine yönlendirin. Güvenli bir küme kullanıyorsanız, değiştirdiğiniz emin olun *http://* için *https://*. (Bu adım yalnızca Service Fabric kümeleri için geçerli olduğunu unutmayın. Bir sanal makine ölçek kümesini kullanıyorsanız, bu adımı atlayın.)

    ```
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',reference(variables('lbID0')).frontEndIPConfigurations[0].properties.privateIPAddress,':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

7. Şablon dağıtma:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkinginternallb -Location westus

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternallb -TemplateFile C:\SFSamples\Final\template\_internalonlyLB.json
    ```

Dağıtımdan sonra yük dengeleyicisi statik 10.0.0.250 özel IP adresi kullanır. Bu aynı sanal ağdaki başka bir makine varsa, iç ağa gidebilirsiniz [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) uç noktası. Yük dengeleyicinin arkasındaki düğümlerinden biri bağlanır unutmayın.

<a id="internalexternallb"></a>
## <a name="internal-and-external-load-balancer"></a>İç ve dış yük dengeleyici

Bu senaryoda, var olan tek bir düğüm türü dış yük dengeleyici ile başlatın ve aynı düğüm türü için iç yük dengeleyiciye ekleyin. Yalnızca bir tek yük dengeleyici arka uç adres havuzuna bağlı bir arka uç bağlantı noktası atanabilir. Hangi yük dengeleyici, uygulama bağlantı noktaları sahip ve hangi yük dengeleyici Yönetimi noktalarınızı (bağlantı noktaları 19000 ve 19080) sahip seçin. İç yük dengeleyici ile ilgili yönetim uç noktalarının yerleştirirseniz, Service Fabric kaynak sağlayıcısı kısıtlamaları makalenin önceki bölümlerinde açıklanan unutmayın. Örnekte kullanıyoruz, yönetim uç noktaları dış yük dengeleyici üzerinde kalır. Ayrıca bir bağlantı noktası 80 uygulama bağlantı noktası eklemek ve iç yük dengeleyicide yerleştirin.

İki düğüm türü kümedeki bir düğüm üzerinde dış yük dengeleyici türüdür. Bir düğüm türü için iç yük dengeleyicide ' dir. İki düğüm türü küme (sahip iki yük dengeleyici desteklemektedir) portal tarafından oluşturulan iki düğüm türü şablonunda kullanmak için ikinci yük dengeleyici için bir iç yük dengeleyici geçin. Daha fazla bilgi için bkz: [yalnızca dahili yük dengeleyici](#internallb) bölümü.

1. Statik iç yük dengeleyici IP adresi parametresini ekleyin. (Dinamik bir IP adresi kullanmayla ilgili notlar için bu makalenin önceki bölümlerinde bkz.)

    ```
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

2. Bir uygulama bağlantı noktası 80 parametresini ekleyin.

3. Var olan iç sürümleri kopyalamak ve yapıştırmak değişkenleri, ağ ekleyin ve eklemek için "-Int" adı:

    ```
    /* Add internal load balancer networking variables */
            "lbID0-Int": "[resourceId('Microsoft.Network/loadBalancers', concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'), '-Internal'))]",
            "lbIPConfig0-Int": "[concat(variables('lbID0-Int'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
            "lbPoolID0-Int": "[concat(variables('lbID0-Int'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
            "lbProbeID0-Int": "[concat(variables('lbID0-Int'),'/probes/FabricGatewayProbe')]",
            "lbHttpProbeID0-Int": "[concat(variables('lbID0-Int'),'/probes/FabricHttpGatewayProbe')]",
            "lbNatPoolID0-Int": "[concat(variables('lbID0-Int'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]",
            /* Internal load balancer networking variables end */
    ```

4. Uygulama bağlantı noktası 80 portal tarafından oluşturulan şablonla başlatırsanız, varsayılan portal şablonu AppPort1 ekler (bağlantı noktası 80) dış yük dengeleyici üzerinde. Bu durumda, AppPort1 dış yük dengeleyiciden kaldırın `loadBalancingRules` ve iç yük dengeleyiciye ekleyebilmek araştırmalar:

    ```
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

5. İkinci bir ekleme `Microsoft.Network/loadBalancers` kaynak. Oluşturulan iç yük dengeleyiciye benzer [yalnızca dahili yük dengeleyici](#internallb) bölüm, ancak kullanır "-Int" Yük Dengeleyici değişkenleri ve yalnızca uygulama bağlantı noktası 80 uygular. Bu da kaldırır `inboundNatPools`, genel yük dengeleyiciye üzerinde RDP uç noktaları korumak için. İç yük dengeleyici üzerinde RDP istiyorsanız taşıyın `inboundNatPools` dışarıdan yük dengeleyici bu dahili yük dengeleyici için:

    ```
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

6. İçinde `networkProfile` için `Microsoft.Compute/virtualMachineScaleSets` kaynak, iç arka uç adres havuzu ekleyin:

    ```
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

7. Şablon dağıtma:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkinginternalexternallb -Location westus

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternalexternallb -TemplateFile C:\SFSamples\Final\template\_internalexternalLB.json
    ```

Dağıtımdan sonra kaynak grubunda iki yük dengeleyici görebilirsiniz. Yük Dengeleyici göz atarsanız, genel IP adresi atanmış ortak IP adresi ve yönetim uç noktalar (bağlantı noktaları 19000 ve 19080) görebilirsiniz. İç yük dengeleyiciye atanan statik iç IP adresi ve uygulama uç noktası (bağlantı noktası 80) de görebilirsiniz. Her iki yük dengeleyicisi, aynı sanal makine ölçek kümesi arka uç havuzu kullanın.

## <a name="next-steps"></a>Sonraki adımlar
[Küme oluşturma](service-fabric-cluster-creation-via-arm.md)
