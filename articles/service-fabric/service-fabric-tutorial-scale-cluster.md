---
title: Azure'daki bir Service Fabric kümesini ölçeklendirme | Microsoft Docs
description: Bu öğreticide, Azure'da bir Service Fabric kümesini ölçekleme konusunda bilgi edinin.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/19/2019
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: 8b5051787855413e16de984cb5d26a24e305b00c
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58498409"
---
# <a name="tutorial-scale-a-service-fabric-cluster-in-azure"></a>Öğretici: Azure'da bir Service Fabric kümesini ölçekleme

Bu öğretici bir serinin üçüncü bölümüdür ve mevcut kümenizin ve gösterilmektedir. Tamamladığınızda, kümenizin nasıl ölçekleneceğini ve kalan kaynakların nasıl temizleneceğini öğrenmiş olacaksınız.  Azure'da çalışan bir küme ölçeklendirme ile ilgili daha fazla bilgi için okuma [ölçeklendirme Service Fabric kümeleri](service-fabric-cluster-scaling.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Ekleme ve kaldırma (ölçeği genişletme ve ölçeği daraltma) düğümler
> * Ekleme ve kaldırma (ölçeği genişletme ve ölçeği daraltma) düğüm türleri
> * Artırma düğümünde kaynaklarını (büyütme)

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Güvenli oluşturma [Windows Küme](service-fabric-tutorial-create-vnet-and-windows-cluster.md) bir şablonu kullanarak azure'da
> * [Bir kümesini izleme](service-fabric-tutorial-monitor-cluster.md)
> * Bir kümenin ölçeğini daraltma veya genişletme
> * [Bir kümenin çalışma zamanını yükseltme](service-fabric-tutorial-upgrade-cluster.md)
> * [Küme silme](service-fabric-tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun
* [Azure Powershell modülü sürüm 4.1 veya üzerini](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) ya da [Azure CLI](/cli/azure/install-azure-cli)'yı yükleyin.
* Güvenli oluşturma [Windows Küme](service-fabric-tutorial-create-vnet-and-windows-cluster.md) azure'da

## <a name="important-considerations-and-guidelines"></a>Önemli noktalar ve yönergeleri

Uygulama iş yüklerini zaman içinde değiştirebilir, var olan hizmetlerinizin daha fazla (veya daha az) kaynakları gerekiyor mu?  [Ekleme veya düğümleri kaldırma](#add-nodes-to-or-remove-nodes-from-a-node-type) artırmak veya küme kaynaklarını azaltmak için bir düğüm türünden.

Kümeniz için 100'den fazla düğüm eklemek gerekiyor mu?  Tek bir Service Fabric düğüm türü/ölçek kümesi, 100'den fazla düğümler/VM'ler içeremez.  Bir kümeyi 100 düğüm ötesinde ölçeklendirme için [ek düğüm türleri ekleme](#add-nodes-to-or-remove-nodes-from-a-node-type).

Uygulamanızı birden çok hizmetlere sahip olmadığı ve herhangi biri genel veya internet'e yönelik olmam gerekir mi?  Normal uygulama, istemciden gelen giriş alan bir ön uç ağ geçidi hizmeti ve iletişim kuran bir veya daha fazla arka uç Hizmetleri ile ön uç hizmetleri içerir. Bu durumda, öneririz [en az iki düğüm türleri ekleme](#add-nodes-to-or-remove-nodes-from-a-node-type) kümeye.  

Hizmetlerinizi daha büyük RAM veya daha yüksek CPU çevrimleri gibi farklı altyapı gereksinimleri var mı? Örneğin, uygulamanızın ön uç hizmeti ve arka uç hizmeti içerir. Ön uç hizmeti (örneğin, D2 VM boyutları) bağlantı noktalarının açık internet'e sahip daha küçük sanal makinelerinde çalıştırabilirsiniz. Arka uç hizmeti, hesaplama yoğun ve internet olmayan daha büyük Vm'leri (D4, D6, D15 gibi VM boyutlarıyla) çalıştırma gerekiyor ancak yaşıyor. Bu durumda, önerilir, [iki veya daha fazla düğüm türleri ekleme](#add-nodes-to-or-remove-nodes-from-a-node-type) kümenize. Bu, her düğüm türü, internet bağlantısı veya VM boyutu gibi farklı özelliklere sahip olmasını sağlar. VM sayısı ölçeklendirilebilir bağımsız olarak da.

Azure kümesine ölçeklendirme, aşağıdaki yönergeleri göz önünde bulundurun:

* Tek bir Service Fabric düğüm türü/ölçek kümesi, 100'den fazla düğümler/VM'ler içeremez.  100 düğüm ötesinde bir kümenin ölçeğini ek düğüm türleri ekleyin.
* Üretim iş yükleri çalıştıran birincil düğüm türleri olmalıdır bir [dayanıklılık düzeyi] [ durability] Silver veya Gold ve her zaman beş veya daha fazla düğüm varsa.
* durum bilgisi olan üretim iş yükleri çalıştıran birincil olmayan düğüm türleri, her zaman beş veya daha fazla düğüme sahip olmalıdır.
* durum bilgisi olmayan üretim iş yükleri çalıştıran birincil olmayan düğüm türleri, her zaman iki veya daha fazla düğüme sahip olmalıdır.
* Herhangi bir düğüm türü [dayanıklılık düzeyi] [ durability] Silver veya Gold beş veya daha fazla düğüm her zaman olmalıdır.
* Ölçeklendirme (düğümlerden kaldırma), bir birincil düğüm türü, hiçbir zaman daha düşük örneği sayısını düşürmelisiniz [güvenilirlik düzeyi] [ reliability] gerektirir.

Daha fazla bilgi için okuma [küme kapasitesi Kılavuzu](service-fabric-cluster-capacity.md).

## <a name="export-the-template-for-the-resource-group"></a>Kaynak grubu için şablonu dışarı aktarma

Güvenli oluşturduktan sonra [Windows Küme](service-fabric-tutorial-create-vnet-and-windows-cluster.md) ve kaynak grubunuz başarıyla ayarlama kaynak grubu için Resource Manager şablonunu dışarı aktarma. Şablonu dışarı aktarma, şablonun tüm eksiksiz altyapı içerdiğinden kümenin ve onun kaynaklarını gelecekteki dağıtımlar otomatikleştirmenizi sağlar.  Şablonları dışarı aktarma ile ilgili daha fazla bilgi için okuma [Azure portalını kullanarak kaynak gruplarını yönetme Azure Resource Manager](/azure/azure-resource-manager/manage-resource-groups-portal).

1. İçinde [Azure portalında](https://portal.azure.com)kümeyi içeren kaynak grubuna gidin (**sfclustertutorialgroup**, bu öğreticiyi takip ediyorsanız). 

2. Sol bölmede seçin **dağıtımları**, veya altındaki bağlantıyı seçin **dağıtımları**. 

3. En son başarılı dağıtımı listeden seçin.

4. Sol bölmede seçin **şablon** seçip **indirme** şablonu ZIP dosyası olarak dışarı aktarmak için.  Şablon ve parametreleri yerel bilgisayarınıza kaydedin.

## <a name="add-nodes-to-or-remove-nodes-from-a-node-type"></a>Düğüm ekleme veya düğümleri bir düğüm türünden kaldırın

Giriş ve çıkış ölçeklendirme ve yatay ölçeklendirme, kümedeki düğümlerin sayısını değiştirir. Ölçeğini daraltma veya genişletme, daha fazla sanal makine örnekleri için ölçek kümesi ekleyin. Bu örnekler, Service Fabric tarafından kullanılan düğümler haline gelir. Service Fabric, ölçek kümesine ne zaman daha fazla örnek eklendiğini bilir (ölçek genişletilerek) ve otomatik olarak tepki verir. Kümedeki herhangi bir zamanda iş yükleri küme üzerinde çalışırken bile ölçeklendirebilirsiniz.

### <a name="update-the-template"></a>Güncelleştirme şablonu

[Bir şablon ve parametreleri dosyasını dışarı](#export-the-template-for-the-resource-group) en son dağıtım için bir kaynak grubundan.  Açık *parameters.json* dosya.  Küme kullanarak dağıttıysanız [örnek şablonu] [ template] Bu öğreticide, üç düğüm türleri kümesi ve her düğüm türü için düğüm sayısını ayarlayan üç parametre vardır:  *nt0InstanceCount*, *nt1InstanceCount*, ve *nt2InstanceCount*.  *Nt1InstanceCount* parametresi, örneğin, İkinci düğüm türü için örnek sayısını ayarlar ve ilişkili sanal makine ölçek kümesindeki VM sayısını ayarlar.

Değerini güncelleştirerek bunu *nt1InstanceCount* İkinci düğüm türü düğüm sayısını değiştirin.  Düğüm türü çıkış 100'den fazla düğümlere ölçeklendirilemez unutmayın.  durum bilgisi olan üretim iş yükleri çalıştıran birincil olmayan düğüm türleri, her zaman beş veya daha fazla düğüme sahip olmalıdır. durum bilgisi olmayan üretim iş yükleri çalıştıran birincil olmayan düğüm türleri, her zaman iki veya daha fazla düğüme sahip olmalıdır.

İçinde ölçeklendirme, bir düğüm türü, Bronz düğümleri kaldırma [dayanıklılık düzeyi] [ durability] gerekir [düğümleri durumunu el ile kaldırma](service-fabric-cluster-scale-up-down.md#manually-remove-vms-from-a-node-typevirtual-machine-scale-set).  Silver ve Gold dayanıklılık katmanı için şu adımları, platform tarafından otomatik olarak gerçekleştirilir.

### <a name="deploy-the-updated-template"></a>Güncelleştirilmiş şablonu dağıtma
Değişiklikleri Kaydet *template.json* ve *parameters.json* dosyaları.  Güncelleştirilmiş şablonu dağıtmak için aşağıdaki komutu çalıştırın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName sfclustertutorialgroup -TemplateFile c:\temp\template.json -TemplateParameterFile c:\temp\parameters.json -Name "ChangingInstanceCount"
```
Veya aşağıdaki Azure CLI komutunu kullanın:
```azure-cli
az group deployment create --resource-group sfclustertutorialgroup --template-file c:\temp\template.json --parameters c:\temp\parameters.json
```

## <a name="add-a-node-type-to-the-cluster"></a>Kümeye bir düğüm türü ekleme

Azure'da çalışan bir Service Fabric kümesinde tanımlanan her düğüm türü olarak ayarlanan bir [ayrı sanal makine ölçek kümesi](service-fabric-cluster-nodetypes.md). Ardından her düğüm türü ayrı olarak yönetilebilir. Bağımsız olarak, her düğüm türünün ölçeği artırın veya azaltın, farklı bağlantı noktası kümeleri açık olan ve farklı kapasite ölçümleri kullanın. Bağımsız olarak da, her küme düğümünde çalışan işletim sistemi SKU'yu değiştirebilirsiniz ancak Windows ve Linux örnek kümede çalışan bir karışımını sahip olamayacağını unutmayın. Tek düğüm türü/ölçek kümesini 100'den fazla düğümler içeremez.  Ek düğüm türleri/ölçek kümelerinize ekleyerek, 100'den fazla düğümleri kümeye yatay yönde ölçeklendirebilirsiniz. Kümedeki herhangi bir zamanda iş yükleri küme üzerinde çalışırken bile ölçeklendirebilirsiniz.

### <a name="update-the-template"></a>Güncelleştirme şablonu

[Bir şablon ve parametreleri dosyasını dışarı](#export-the-template-for-the-resource-group) en son dağıtım için bir kaynak grubundan.  Açık *parameters.json* dosya.  Küme kullanarak dağıttıysanız [örnek şablonu] [ template] Bu öğreticide, kümedeki üç düğüm türleri vardır.  Bu bölümde, güncelleştirme ve Resource Manager şablonu dağıtma dördüncü bir düğüm türü ekleyin. 

Yeni düğüm türü, ek olarak, aynı zamanda (ayrı bir sanal ağ alt ağda çalışır) ilişkili sanal makine ölçek kümesi ekleyebilir ve ağ güvenlik grubu.  Yeni veya var olan genel IP adresini ve yeni ölçek kümesi için Azure yük dengeleyici kaynakları eklemek seçebilirsiniz.  Yeni düğüm türüne sahip bir [dayanıklılık düzeyi] [ durability] Silver ve "İşler için standart_d2_v2" boyutu.

İçinde *template.json* dosyasında, aşağıdaki yeni parametreleri ekleyin:
```json
"nt3InstanceCount": {
    "defaultValue": 5,
    "type": "Int",
    "metadata": {
        "description": "Instance count for node type"
    }
},
"vmNodeType3Size": {
    "defaultValue": "Standard_D2_V2",
    "type": "String"
},
```

İçinde *template.json* dosyasında, aşağıdaki yeni değişkenleri ekleyin:
```json
"lbID3": "[resourceId('Microsoft.Network/loadBalancers',concat('LB','-', parameters('clusterName'),'-',variables('vmNodeType3Name')))]",
"lbIPConfig3": "[concat(variables('lbID3'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
"lbPoolID3": "[concat(variables('lbID3'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
"lbProbeID3": "[concat(variables('lbID3'),'/probes/FabricGatewayProbe')]",
"lbHttpProbeID3": "[concat(variables('lbID3'),'/probes/FabricHttpGatewayProbe')]",
"lbNatPoolID3": "[concat(variables('lbID3'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]",
"vmNodeType3Name": "[toLower(concat('NT4', variables('vmName')))]",
"vmStorageAccountName3": "[toLower(concat(uniqueString(resourceGroup().id), '1', '3' ))]",
"nt3applicationStartPort": "20000",
"nt3applicationEndPort": "30000",
"nt3ephemeralStartPort": "49152",
"nt3ephemeralEndPort": "65534",
"nt3fabricTcpGatewayPort": "19000",
"nt3fabricHttpGatewayPort": "19080",
"nt3reverseProxyEndpointPort": "19081",
"subnet3Name": "Subnet-3",
"subnet3Prefix": "10.0.3.0/24",
"subnet3Ref": "[concat(variables('vnetID'),'/subnets/',variables('subnet3Name'))]",
```

İçinde *template.json* dosyasında, yeni bir alt ağ için sanal ağ kaynağı ekleyin:
```json
{
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[variables('virtualNetworkName')]",
    "apiVersion": "2018-08-01",
    "location": "[variables('computeLocation')]",
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    },
    "properties": {
        "addressSpace": {
            "addressPrefixes": [
                "[variables('addressPrefix')]"
            ]
        },
        "subnets": [
            ...
            {
                "name": "[variables('subnet3Name')]",
                "properties": {
                    "addressPrefix": "[variables('subnet3Prefix')]",
                    "networkSecurityGroup": {
                        "id": "[resourceId('Microsoft.Network/networkSecurityGroups', concat('nsg', variables('subnet3Name')))]"
                    }
                }
            }
        ]
    },
    "dependsOn": [
        ...
        "[concat('Microsoft.Network/networkSecurityGroups/', concat('nsg', variables('subnet3Name')))]"
    ]
},
```

İçinde *template.json* yeni ortak IP adresi ve yük dengeleyici kaynakları ekleyin:
```json
{
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[concat(variables('lbIPName'),'-',variables('vmNodeType3Name'))]",
    "apiVersion": "2018-08-01",
    "location": "[variables('computeLocation')]",
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    },
    "properties": {
        "dnsSettings": {
            "domainNameLabel": "[concat(variables('dnsName'),'-','nt4')]"
        },
        "publicIPAllocationMethod": "Dynamic"
    }
},
        {
    "type": "Microsoft.Network/loadBalancers",
    "name": "[concat('LB','-', parameters('clusterName'),'-',variables('vmNodeType3Name'))]",
    "apiVersion": "2018-08-01",
    "location": "[variables('computeLocation')]",
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    },
    "properties": {
        "frontendIPConfigurations": [
            {
                "name": "LoadBalancerIPConfig",
                "properties": {
                    "publicIPAddress": {
                        "id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('lbIPName'),'-',variables('vmNodeType3Name')))]"
                    }
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
            {
                "name": "LBRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID3')]"
                    },
                    "backendPort": "[variables('nt3fabricTcpGatewayPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig3')]"
                    },
                    "frontendPort": "[variables('nt3fabricTcpGatewayPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[variables('lbProbeID3')]"
                    },
                    "protocol": "tcp"
                }
            },
            {
                "name": "LBHttpRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID3')]"
                    },
                    "backendPort": "[variables('nt3fabricHttpGatewayPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig3')]"
                    },
                    "frontendPort": "[variables('nt3fabricHttpGatewayPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[variables('lbHttpProbeID3')]"
                    },
                    "protocol": "tcp"
                }
            },
            {
                "name": "AppPortLBRule1",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID3')]"
                    },
                    "backendPort": "[parameters('loadBalancedAppPort1')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig3')]"
                    },
                    "frontendPort": "[parameters('loadBalancedAppPort1')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID3'),'/probes/AppPortProbe1')]"
                    },
                    "protocol": "tcp"
                }
            },
            {
                "name": "AppPortLBRule2",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID3')]"
                    },
                    "backendPort": "[parameters('loadBalancedAppPort2')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig3')]"
                    },
                    "frontendPort": "[parameters('loadBalancedAppPort2')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID3'),'/probes/AppPortProbe2')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            {
                "name": "FabricGatewayProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port": "[variables('nt3fabricTcpGatewayPort')]",
                    "protocol": "tcp"
                }
            },
            {
                "name": "FabricHttpGatewayProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port": "[variables('nt3fabricHttpGatewayPort')]",
                    "protocol": "tcp"
                }
            },
            {
                "name": "AppPortProbe1",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port": "[parameters('loadBalancedAppPort1')]",
                    "protocol": "tcp"
                }
            },
            {
                "name": "AppPortProbe2",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port": "[parameters('loadBalancedAppPort2')]",
                    "protocol": "tcp"
                }
            }
        ],
        "inboundNatPools": [
            {
                "name": "LoadBalancerBEAddressNatPool",
                "properties": {
                    "backendPort": "3389",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig3')]"
                    },
                    "frontendPortRangeEnd": "4500",
                    "frontendPortRangeStart": "3389",
                    "protocol": "tcp"
                }
            }
        ]
    },
    "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/',concat(variables('lbIPName'),'-',variables('vmNodeType3Name')))]"
    ]
},
```

İçinde *template.json* dosyasında, yeni ağ güvenlik grubunu ve sanal makine ölçek kümesi kaynaklarının ekleyin.  Service Fabric uzantısı özelliklerini sanal makine ölçek kümesi içinde NodeTypeRef özelliği belirtilen düğüm türü ölçek kümesine eşler.

```json
{
    "type": "Microsoft.Network/networkSecurityGroups",
    "name": "[concat('nsg', variables('subnet3Name'))]",
    "apiVersion": "2018-08-01",
    "location": "[resourceGroup().location]",
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    },
    "properties": {
        "securityRules": [
            {
                "name": "allowSvcFabSMB",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "445",
                    "direction": "Inbound",
                    "priority": 3950,
                    "protocol": "*",
                    "sourceAddressPrefix": "VirtualNetwork",
                    "sourcePortRange": "*",
                    "description": "allow SMB traffic within the net, used by fabric to move packages around"
                }
            },
            {
                "name": "allowSvcFabCluser",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "1025-1027",
                    "direction": "Inbound",
                    "priority": 3920,
                    "protocol": "*",
                    "sourceAddressPrefix": "VirtualNetwork",
                    "sourcePortRange": "*",
                    "description": "allow ports within vnet that are used by the fabric to talk between nodes"
                }
            },
            {
                "name": "allowSvcFabEphemeral",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "[concat(variables('nt3ephemeralStartPort'), '-', variables('nt3ephemeralEndPort'))]",
                    "direction": "Inbound",
                    "priority": 3930,
                    "protocol": "*",
                    "sourceAddressPrefix": "VirtualNetwork",
                    "sourcePortRange": "*",
                    "description": "allow fabric ephemeral ports within the vnet"
                }
            },
            {
                "name": "allowSvcFabPortal",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "[variables('nt3fabricHttpGatewayPort')]",
                    "direction": "Inbound",
                    "priority": 3900,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "allow port used to access the fabric cluster web portal"
                }
            },
            {
                "name": "allowSvcFabClient",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "[variables('nt3fabricTcpGatewayPort')]",
                    "direction": "Inbound",
                    "priority": 3910,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "allow port used by the fabric client (includes powershell)"
                }
            },
            {
                "name": "allowSvcFabApplication",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "[concat(variables('nt3applicationStartPort'), '-', variables('nt3applicationEndPort'))]",
                    "direction": "Inbound",
                    "priority": 3940,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "allow fabric application ports within the vnet"
                }
            },
            {
                "name": "blockAll",
                "properties": {
                    "access": "Deny",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "*",
                    "direction": "Inbound",
                    "priority": 4095,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "block all traffic except what we've explicitly allowed"
                }
            },
            {
                "name": "allowVNetRDP",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "3389",
                    "direction": "Inbound",
                    "priority": 3960,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "allow RDP within the net"
                }
            },
            {
                "name": "allowSvcFabReverseProxy",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "[variables('nt3reverseProxyEndpointPort')]",
                    "direction": "Inbound",
                    "priority": 3980,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "allow port used to access the fabric cluster using reverse proxy"
                }
            },
            {
                "name": "allowAppPort1",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "[parameters('loadBalancedAppPort1')]",
                    "direction": "Inbound",
                    "priority": 2001,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "allow public application port 1"
                }
            },
            {
                "name": "allowAppPort2",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "[parameters('loadBalancedAppPort2')]",
                    "direction": "Inbound",
                    "priority": 2002,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "allow public application port 2"
                }
            }
        ]
    }
},
{
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    "sku": {
        "name": "[parameters('vmNodeType3Size')]",
        "capacity": "[parameters('nt3InstanceCount')]",
        "tier": "Standard"
    },
    "name": "[variables('vmNodeType3Name')]",
    "apiVersion": "2018-10-01",
    "location": "[variables('computeLocation')]",
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    },
    "properties": {
        "overprovision": "[variables('overProvision')]",
        "upgradePolicy": {
            "mode": "Automatic"
        },
        "virtualMachineProfile": {
            "extensionProfile": {
                "extensions": [
                    {
                        "name": "[concat(variables('vmNodeType3Name'),'OMS')]",
                        "properties": {
                            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
                            "type": "MicrosoftMonitoringAgent",
                            "typeHandlerVersion": "1.0",
                            "autoUpgradeMinorVersion": true,
                            "settings": {
                                "workspaceId": "[reference(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')), '2015-11-01-preview').customerId]"
                            },
                            "protectedSettings": {
                                "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')),'2015-11-01-preview').primarySharedKey]"
                            }
                        }
                    },
                    {
                        "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType3Name')]",
                        "properties": {
                            "type": "ServiceFabricNode",
                            "autoUpgradeMinorVersion": true,
                            "protectedSettings": {
                                "StorageAccountKey1": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key1]",
                                "StorageAccountKey2": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key2]"
                            },
                            "publisher": "Microsoft.Azure.ServiceFabric",
                            "settings": {
                                "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                                "nodeTypeRef": "[variables('vmNodeType3Name')]",
                                "dataPath": "D:\\SvcFab",
                                "durabilityLevel": "Silver",
                                "enableParallelJobs": true,
                                "nicPrefixOverride": "[variables('subnet3Prefix')]",
                                "certificate": {
                                    "thumbprint": "[parameters('certificateThumbprint')]",
                                    "x509StoreName": "[parameters('certificateStoreValue')]"
                                }
                            },
                            "typeHandlerVersion": "1.0"
                        }
                    },
                    {
                        "name": "[concat('VMDiagnosticsVmExt','_vmNodeType3Name')]",
                        "properties": {
                            "type": "IaaSDiagnostics",
                            "autoUpgradeMinorVersion": true,
                            "protectedSettings": {
                                "storageAccountName": "[variables('applicationDiagnosticsStorageAccountName')]",
                                "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
                                "storageAccountEndPoint": "https://core.windows.net/"
                            },
                            "publisher": "Microsoft.Azure.Diagnostics",
                            "settings": {
                                "WadCfg": {
                                    "DiagnosticMonitorConfiguration": {
                                        "overallQuotaInMB": "50000",
                                        "EtwProviders": {
                                            "EtwEventSourceProviderConfiguration": [
                                                {
                                                    "provider": "Microsoft-ServiceFabric-Actors",
                                                    "scheduledTransferKeywordFilter": "1",
                                                    "scheduledTransferPeriod": "PT5M",
                                                    "DefaultEvents": {
                                                        "eventDestination": "ServiceFabricReliableActorEventTable"
                                                    }
                                                },
                                                {
                                                    "provider": "Microsoft-ServiceFabric-Services",
                                                    "scheduledTransferPeriod": "PT5M",
                                                    "DefaultEvents": {
                                                        "eventDestination": "ServiceFabricReliableServiceEventTable"
                                                    }
                                                }
                                            ],
                                            "EtwManifestProviderConfiguration": [
                                                {
                                                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                                                    "scheduledTransferLogLevelFilter": "Information",
                                                    "scheduledTransferKeywordFilter": "4611686018427387904",
                                                    "scheduledTransferPeriod": "PT5M",
                                                    "DefaultEvents": {
                                                        "eventDestination": "ServiceFabricSystemEventTable"
                                                    }
                                                }
                                            ]
                                        }
                                    }
                                },
                                "StorageAccount": "[variables('applicationDiagnosticsStorageAccountName')]"
                            },
                            "typeHandlerVersion": "1.5"
                        }
                    },
                    {
                        "name": "[concat('VMIaaSAntimalware','_vmNodeType3Name')]",
                        "properties": {
                            "publisher": "Microsoft.Azure.Security",
                            "type": "IaaSAntimalware",
                            "typeHandlerVersion": "1.5",
                            "settings": {
                                "AntimalwareEnabled": "true",
                                "Exclusions": {
                                    "Paths": "D:\\SvcFab;D:\\SvcFab\\Log;C:\\Program Files\\Microsoft Service Fabric",
                                    "Processes": "Fabric.exe;FabricHost.exe;FabricInstallerService.exe;FabricSetup.exe;FabricDeployer.exe;ImageBuilder.exe;FabricGateway.exe;FabricDCA.exe;FabricFAS.exe;FabricUOS.exe;FabricRM.exe;FileStoreService.exe"
                                },
                                "RealtimeProtectionEnabled": "true",
                                "ScheduledScanSettings": {
                                    "isEnabled": "true",
                                    "scanType": "Quick",
                                    "day": "7",
                                    "time": "120"
                                }
                            },
                            "protectedSettings": null
                        }
                    }
                ]
            },
            "networkProfile": {
                "networkInterfaceConfigurations": [
                    {
                        "name": "[concat(variables('nicName'), '-2')]",
                        "properties": {
                            "ipConfigurations": [
                                {
                                    "name": "[concat(variables('nicName'),'-',2)]",
                                    "properties": {
                                        "loadBalancerBackendAddressPools": [
                                            {
                                                "id": "[variables('lbPoolID3')]"
                                            }
                                        ],
                                        "loadBalancerInboundNatPools": [
                                            {
                                                "id": "[variables('lbNatPoolID3')]"
                                            }
                                        ],
                                        "subnet": {
                                            "id": "[variables('subnet3Ref')]"
                                        }
                                    }
                                }
                            ],
                            "primary": true
                        }
                    }
                ]
            },
            "osProfile": {
                "adminPassword": "[parameters('adminPassword')]",
                "adminUsername": "[parameters('adminUsername')]",
                "computernamePrefix": "[variables('vmNodeType3Name')]",
                "secrets": [
                    {
                        "sourceVault": {
                            "id": "[parameters('sourceVaultValue')]"
                        },
                        "vaultCertificates": [
                            {
                                "certificateStore": "[parameters('certificateStoreValue')]",
                                "certificateUrl": "[parameters('certificateUrlValue')]"
                            }
                        ]
                    }
                ]
            },
            "storageProfile": {
                "imageReference": {
                    "publisher": "[parameters('vmImagePublisher')]",
                    "offer": "[parameters('vmImageOffer')]",
                    "sku": "[parameters('vmImageSku')]",
                    "version": "[parameters('vmImageVersion')]"
                },
                "osDisk": {
                    "caching": "ReadOnly",
                    "createOption": "FromImage",
                    "managedDisk": {
                        "storageAccountType": "[parameters('storageAccountType')]"
                    }
                }
            }
        }
    },
    "dependsOn": [
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
        "[concat('Microsoft.Network/loadBalancers/', concat('LB','-', parameters('clusterName'),'-',variables('vmNodeType3Name')))]",
        "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]",
        "[concat('Microsoft.Storage/storageAccounts/', variables('applicationDiagnosticsStorageAccountName'))]"
    ]
},
```

İçinde *template.json* dosya ve küme kaynağı güncelleştirme yeni düğüm türü ekleyin:
```json
{
    "type": "Microsoft.ServiceFabric/clusters",
    "name": "[parameters('clusterName')]",
    "apiVersion": "2018-02-01",
    "location": "[parameters('clusterLocation')]",
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    },
    "properties": {
        "nodeTypes": [
            ...
            {
                "name": "[variables('vmNodeType3Name')]",
                "applicationPorts": {
                    "endPort": "[variables('nt3applicationEndPort')]",
                    "startPort": "[variables('nt3applicationStartPort')]"
                },
                "clientConnectionEndpointPort": "[variables('nt3fabricTcpGatewayPort')]",
                "durabilityLevel": "Silver",
                "ephemeralPorts": {
                    "endPort": "[variables('nt3ephemeralEndPort')]",
                    "startPort": "[variables('nt3ephemeralStartPort')]"
                },
                "httpGatewayEndpointPort": "[variables('nt3fabricHttpGatewayPort')]",
                "isPrimary": false,
                "reverseProxyEndpointPort": "[variables('nt3reverseProxyEndpointPort')]",
                "vmInstanceCount": "[parameters('nt3InstanceCount')]"
            }
        ],    
    }
}                
```

İçinde *parameters.json* dosyasında, aşağıdaki yeni parametreler ve değerler ekleyin:
```json
"nt3InstanceCount": {
    "Value": 5    
},
"vmNodeType3Size": {
    "Value": "Standard_D2_V2"
},
```

### <a name="deploy-the-updated-template"></a>Güncelleştirilmiş şablonu dağıtma
Değişiklikleri Kaydet *template.json* ve *parameters.json* dosyaları.  Güncelleştirilmiş şablonu dağıtmak için aşağıdaki komutu çalıştırın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName sfclustertutorialgroup -TemplateFile c:\temp\template.json -TemplateParameterFile c:\temp\parameters.json -Name "AddingNodeType"
```
Veya aşağıdaki Azure CLI komutunu kullanın:
```azure-cli
az group deployment create --resource-group sfclustertutorialgroup --template-file c:\temp\template.json --parameters c:\temp\parameters.json
```

## <a name="remove-a-node-type-from-the-cluster"></a>Kümeden bir düğüm türü Kaldır
Service Fabric kümesi oluşturduktan sonra küme yatay bir düğüm türü (sanal makine ölçek kümesi) ve tüm üst düğümleri kaldırarak ölçeklendirebilirsiniz. Kümedeki herhangi bir zamanda iş yükleri küme üzerinde çalışırken bile ölçeklendirebilirsiniz. Küme ölçekler gibi uygulamalarınızı otomatik olarak da ölçeklendirin.

> [!WARNING]
> Düğüm türü, bir üretim kümesinden kaldırmak için remove-AzureRmServiceFabricNodeType kullanarak sık kullanılan olarak kullanılması önerilmez. Sanal makine ölçek kümesi kaynak düğüm türü arkasında sildiği tehlikeli bir komuttur. 

Düğüm türü kaldırmak için çalıştırın [Remove-AzureRmServiceFabricNodeType](/powershell/module/azurerm.servicefabric/remove-azurermservicefabricnodetype) cmdlet'i.  Düğüm türü, Silver veya Gold olmalıdır [dayanıklılık düzeyi] [ durability] cmdlet düğüm türü ile ilişkili bir ölçek kümesini siler ve tamamlanması biraz zaman alabilir.  Ardından çalıştırın [Remove-ServiceFabricNodeState](/powershell/module/servicefabric/remove-servicefabricnodestate?view=azureservicefabricps) cmdlet'i her düğüm kaldırmak için düğüm durumu siler ve düğümleri kümeden kaldırır. Düğümler üzerinde Hizmetleri varsa, ardından Hizmetleri önce başka bir düğüme taşınır. Küme Yöneticisi çoğaltma/hizmet için bir düğüm bulunamıyor, işlemi Gecikmeli/engellenmiş olur.

```powershell
$groupname = "sfclustertutorialgroup"
$nodetype = "nt4vm"
$clustername = "mysfcluster123"

Remove-AzureRmServiceFabricNodeType -Name $clustername  -NodeType $nodetype -ResourceGroupName $groupname

Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster123.eastus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <thumbprint> `
          -FindType FindByThumbprint -FindValue <thumbprint> `
          -StoreLocation CurrentUser -StoreName My

$nodes = Get-ServiceFabricNode | Where-Object {$_.NodeType -eq $nodetype} | Sort-Object { $_.NodeName.Substring($_.NodeName.LastIndexOf('_') + 1) } -Descending

Foreach($node in $nodes)
{
    Remove-ServiceFabricNodeState -NodeName $node.NodeName -TimeoutSec 300 -Force 
}
```

## <a name="increase-node-resources"></a>Artırma düğümünde kaynaklarını 
Service Fabric kümesi oluşturduktan sonra bir küme düğümü türü dikey olarak ölçeklendirebilirsiniz (düğümlerin kaynakları değişikliği) veya Vm'lere düğüm türündeki işletim sistemini yükseltin.  

> [!WARNING]
> Gümüş dayanıklılık konumunda çalışan veya büyük olmadığı sürece bir ölçek kümesi/düğüm türündeki sanal makine SKU'su değiştirmemenizi öneririz. VM SKU boyutunu değiştirerek verileri yıkıcı yerinde altyapı bir işlemdir. Gecikme veya bu değişikliği izlemek için bazı kabiliyeti olmadan işlem durum bilgisi olan hizmetler için veri kaybına neden veya durum bilgisiz iş yükleri için bile diğer öngörülemeyen operasyonel sorunlara neden olabilir.

> [!WARNING]
> Tehlikeli bir işlemdir ve desteklenmeyen birincil düğüm türünde VM SKU değiştirmemenizi öneririz.  Daha fazla küme kapasitesine ihtiyacınız varsa, daha fazla sanal makine örneği veya ek düğüm türleri ekleyebilirsiniz.  Bu mümkün değilse, yeni bir küme oluşturabilir ve [uygulama durumunu geri yükle](service-fabric-reliable-services-backup-restore.md) (varsa) eski kümenizden.  Bu mümkün değilse, yapabilecekleriniz [birincil düğüm türünde VM SKU değişimi](service-fabric-scale-up-node-type.md).

### <a name="update-the-template"></a>Güncelleştirme şablonu

[Bir şablon ve parametreleri dosyasını dışarı](#export-the-template-for-the-resource-group) en son dağıtım için bir kaynak grubundan.  Açık *parameters.json* dosya.  Küme kullanarak dağıttıysanız [örnek şablonu] [ template] Bu öğreticide, kümedeki üç düğüm türleri vardır.  

İkinci düğüm türünde VM boyutlarını kümesinde *vmNodeType1Size* parametresi.  Değişiklik *vmNodeType1Size* işler için standart_d2_v2 için parametre değerinin [işler için standart_d3_v2](/azure/virtual-machines/windows/sizes-general#dv2-series), her sanal makine örneği kaynakları Katlar.

Tüm üç düğüm türleri VM SKU kümesinde *vmImageSku* parametresi.  Yine, bir düğüm türündeki sanal makine SKU'su değiştirme dikkatli yaklaşıldığında ve birincil düğüm türü için önerilmez.

### <a name="deploy-the-updated-template"></a>Güncelleştirilmiş şablonu dağıtma
Değişiklikleri Kaydet *template.json* ve *parameters.json* dosyaları.  Güncelleştirilmiş şablonu dağıtmak için aşağıdaki komutu çalıştırın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName sfclustertutorialgroup -TemplateFile c:\temp\template.json -TemplateParameterFile c:\temp\parameters.json -Name "ScaleUpNodeType"
```
Veya aşağıdaki Azure CLI komutunu kullanın:
```azure-cli
az group deployment create --resource-group sfclustertutorialgroup --template-file c:\temp\template.json --parameters c:\temp\parameters.json
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Ekleme ve kaldırma (ölçeği genişletme ve ölçeği daraltma) düğümler
> * Ekleme ve kaldırma (ölçeği genişletme ve ölçeği daraltma) düğüm türleri
> * Artırma düğümünde kaynaklarını (büyütme)

Ardından, kümenin çalışma zamanının nasıl yükseltileceğini öğrenmek için aşağıdaki öğreticiye geçin.
> [!div class="nextstepaction"]
> [Bir kümenin çalışma zamanını yükseltme](service-fabric-tutorial-upgrade-cluster.md)

[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
[reliability]: service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster
[template]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.json
[parameters]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.Parameters.json
