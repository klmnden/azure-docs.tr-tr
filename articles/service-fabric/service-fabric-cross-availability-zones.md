---
title: Kullanılabilirlik alanları genelinde bir Azure Service Fabric kümesine dağıtma | Microsoft Docs
description: Kullanılabilirlik alanları genelinde bir Azure Service Fabric kümesi oluşturmayı öğrenin.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2019
ms.author: pepogors
ms.openlocfilehash: b664c3d655ab45c89a65a0aea31622f57ddc8d9e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65077463"
---
# <a name="deploy-an-azure-service-fabric-cluster-across-availability-zones"></a>Kullanılabilirlik alanları genelinde bir Azure Service Fabric kümesine dağıtma
Azure kullanılabilirlik alanları, veri merkezi arızasına karşı uygulamalarınızı ve verilerinizi koruyan sunan bir yüksek kullanılabilirlik olur. Bir kullanılabilirlik bölgesine benzersiz bir fiziksel konum bağımsız güç ile donatılmış soğutma ve ağ bir Azure bölgesi içinde var.

Service Fabric düğüm türleri, Sabitlenen belirli bölgelere dağıtarak kullanılabilirlik alanları genelinde span kümeleri destekler. Bu, uygulamalarınızın yüksek kullanılabilirlik garanti eder. Azure kullanılabilirlik alanları, yalnızca seçilmiş bölgelerde kullanılabilir. Daha fazla bilgi için [Azure kullanılabilirlik alanlarına genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview).

Örnek şablonları mevcuttur: [Service Fabric arası kullanılabilirlik bölgesi şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates)

## <a name="recommended-topology-for-primary-node-type-of-azure-service-fabric-clusters-spanning-across-availability-zones"></a>Azure Service Fabric küme kullanılabilirlik alanları genelinde kapsayan birincil düğüm türü için topolojiyi önerilir
Kullanılabilirlik alanları genelinde dağıtılmış bir Service Fabric kümesine, küme durumunu yüksek kullanılabilirliğini sağlar. Service Fabric kümesi bölgeler arasında yaymasına izin, bölge tarafından desteklenen her kullanılabilirlik alanı birincil düğüm türü oluşturmanız gerekir. Bu çekirdek düğümleri eşit olarak her birincil düğüm türleri arasında dağıtır.

Birincil düğüm türü için önerilen topoloji aşağıda ana hatlarıyla belirtilen kaynaklar gereklidir:

* Kümenin güvenilirlik düzeyi için Platinum ayarlayın.
* Üç düğüm türleri, birincil olarak işaretlendi.
    * Her düğüm türü kendi sanal makine ölçek kümesi farklı bölgelerde bulunan eşlenmesi gerekir.
    * Her sanal makine ölçek kümesi en az beş düğüm (Gümüş dayanıklılık) sahip olmalıdır.
* Tek bir genel IP standart SKU kullanarak kaynak.
* Tek bir yük dengeleyici standart SKU'su kullanarak kaynak.
* Alt ağ, sanal makine ölçek kümeleri dağıttığınız tarafından başvurulan bir NSG.

>[!NOTE]
> Sanal makine ölçek kümesi tek bir yerleştirme grubu özelliği, Service Fabric bölgelere yayılan bir tek sanal makine ölçek kümesi desteklemediğinden, true olarak ayarlamanız gerekir.

 ![Azure Service Fabric kullanılabilirlik alanı mimarisi][sf-architecture]

## <a name="networking-requirements"></a>Ağ gereksinimleri
### <a name="public-ip-and-load-balancer-resource"></a>Genel IP ve yük dengeleyici kaynağı
Özelliği bir sanal makine ölçek kümesi kaynak, yük dengeleyici ve bu sanal makine ölçek kümesi tarafından başvurulan IP kaynağı bölgeleri etkinleştirmek için her ikisini de kullanarak gerekir bir *standart* SKU. Bir yük dengeleyici veya SKU özelliği olmadan IP kaynağı oluşturma, bir temel, kullanılabilirlik bölgelerini desteklemiyor SKU, oluşturacaksınız. Standart SKU yük dengeleyici, varsayılan olarak dış giden tüm trafiği engeller; dışında trafiğine izin veren bir NSG alt ağa dağıtılması gerekir.

```json
{
    "apiVersion": "2018-11-01",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[concat('LB','-', parameters('clusterName')]",
    "location": "[parameters('computeLocation')]",
    "sku": {
        "name": "Standard"
    }
}
```

```json
{
    "apiVersion": "2018-11-01",
    "type": "Microsoft.Network/loadBalancers",
    "name": "[concat('LB','-', parameters('clusterName')]", 
    "location": "[parameters('computeLocation')]",
    "dependsOn": [
        "[concat('Microsoft.Network/networkSecurityGroups/', concat('nsg', parameters('subnet0Name')))]"
    ],
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
                "addressPrefix": "[parameters('subnet0Prefix')]",
                "networkSecurityGroup": {
                "id": "[resourceId('Microsoft.Network/networkSecurityGroups', concat('nsg', parameters('subnet0Name')))]"
              }
            }
          }
        ]
    },
    "sku": {
        "name": "Standard"
    }
}
```

>[!NOTE]
> SKU genel IP ve yük dengeleyici kaynaklar üzerinde bir yerinde değişiklik yapmak mümkün değildir. Temel bir SKU'ya sahip mevcut kaynaklardan geçiriyorsanız, bu makalenin geçiş bölümüne bakın.

### <a name="virtual-machine-scale-set-nat-rules"></a>Sanal makine ölçek kümesi NAT kuralları
Yük Dengeleyici gelen NAT kuralları, sanal makine ölçek kümesi gelen NAT havuzları eşleşmelidir. Her sanal makine ölçek kümesi, benzersiz bir gelen NAT havuzu olması gerekir.

```json
{
"inboundNatPools": [
    {
        "name": "LoadBalancerBEAddressNatPool0",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "50999",
            "frontendPortRangeStart": "50000",
            "protocol": "tcp"
        }
    },
    {
        "name": "LoadBalancerBEAddressNatPool1",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "51999",
            "frontendPortRangeStart": "51000",
            "protocol": "tcp"
        }
    },
    {
        "name": "LoadBalancerBEAddressNatPool2",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "52999",
            "frontendPortRangeStart": "52000",
            "protocol": "tcp"
        }
    }
    ]
}
```

### <a name="standard-sku-load-balancer-outbound-rules"></a>Standart SKU yük dengeleyici giden kuralları
Standart Load Balancer ve standart genel IP temel SKU'ları kullanarak karşılaştırıldığında giden bağlantıyı yeni yetenekler ve farklı davranışları dağıtır. Standart SKU'lar ile çalışırken giden bağlantı istiyorsanız, bunu standart genel IP adresleri veya genel bir Standard Load Balancer ile açıkça tanımlamalısınız. Daha fazla bilgi için [giden bağlantılar](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#snatexhaust) ve [Azure Standard Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview).

>[!NOTE]
> Standart şablon varsayılan olarak tüm giden trafiğe izin veren bir NSG başvuruyor. Service Fabric yönetim işlemleri için gereken bağlantı noktalarına gelen trafiği sınırlıdır. NSG kuralları, gereksinimlerinizi karşılayacak şekilde değiştirilebilir.

### <a name="enabling-zones-on-a-virtual-machine-scale-set"></a>Etkinleştirme bölgelerinde sanal makine ölçek kümesi
Aşağıdaki üç değerden bir bölge etkinleştirmek için üzerinde sanal makine ölçek kümesi sanal makine ölçek kümesi kaynak içermesi gerekir.

* İlk değer **bölgeleri** özelliği hangi kullanılabilirlik sanal makine ölçek kümesi dağıtılacağı bölgeyi belirtir.
* İkinci değerdir ayarlanması gerekir "singlePlacementGroup" özelliği true.
* Service Fabric sanal makine ölçek kümesi uzantısını "faultDomainOverride" özelliğinde üçüncü değerdir. Bu özellik için değer, bu sanal makine ölçek kümesi yerleştirileceği bölge ve bölge içermelidir. Örnek: "faultDomainOverride": "eastus/az1 tüm sanal makine ölçek kümesi kaynakları" Azure Service Fabric kümeleri bölge desteği olmadığı için aynı bölgede yerleştirilmelidir.

```json
{
    "apiVersion": "2018-10-01",
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    "name": "[parameters('vmNodeType1Name')]",
    "location": "[parameters('computeLocation')]",
    "zones": ["1"],
    "properties": {
        "singlePlacementGroup": "true",
    },
    "virtualMachineProfile": {
    "extensionProfile": {
    "extensions": [
    {
    "name": "[concat(parameters('vmNodeType1Name'),'_ServiceFabricNode')]",
    "properties": {
        "type": "ServiceFabricNode",
        "autoUpgradeMinorVersion": false,
        "publisher": "Microsoft.Azure.ServiceFabric.Test",
        "settings": {
            "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
            "nodeTypeRef": "[parameters('vmNodeType1Name')]",
            "dataPath": "D:\\\\SvcFab",
            "durabilityLevel": "Silver",
            "certificate": {
                "thumbprint": "[parameters('certificateThumbprint')]",
                "x509StoreName": "[parameters('certificateStoreValue')]"
            },
            "systemLogUploadSettings": {
                "Enabled": true
            },
            "faultDomainOverride": "eastus/az1"
        },
        "typeHandlerVersion": "1.0"
    }
}
```

### <a name="enabling-multiple-primary-node-types-in-the-service-fabric-cluster-resource"></a>Service Fabric kümesi kaynağında birden çok birincil düğüm türleri etkinleştirme
Bir veya daha fazla düğüm türleri, bir küme kaynağı içindeki birincil olarak ayarlamak için "true" "Isprimary" özelliğini ayarlayın. Service Fabric kümesi kullanılabilirlik alanları genelinde dağıtım yaparken, üç düğüm türleri farklı bölgelerde olmalıdır.

```json
{
    "reliabilityLevel": "Platinum",
    "nodeTypes": [
    {
        "name": "[parameters('vmNodeType0Name')]",
        "applicationPorts": {
            "endPort": "[parameters('nt0applicationEndPort')]",
            "startPort": "[parameters('nt0applicationStartPort')]"
        },
        "clientConnectionEndpointPort": "[parameters('nt0fabricTcpGatewayPort')]",
        "durabilityLevel": "Silver",
        "ephemeralPorts": {
            "endPort": "[parameters('nt0ephemeralEndPort')]",
            "startPort": "[parameters('nt0ephemeralStartPort')]"
        },
        "httpGatewayEndpointPort": "[parameters('nt0fabricHttpGatewayPort')]",
        "isPrimary": true,
        "vmInstanceCount": "[parameters('nt0InstanceCount')]"
    },
    {
        "name": "[parameters('vmNodeType1Name')]",
        "applicationPorts": {
            "endPort": "[parameters('nt1applicationEndPort')]",
            "startPort": "[parameters('nt1applicationStartPort')]"
        },
        "clientConnectionEndpointPort": "[parameters('nt1fabricTcpGatewayPort')]",
        "durabilityLevel": "Silver",
        "ephemeralPorts": {
            "endPort": "[parameters('nt1ephemeralEndPort')]",
            "startPort": "[parameters('nt1ephemeralStartPort')]"
        },
        "httpGatewayEndpointPort": "[parameters('nt1fabricHttpGatewayPort')]",
        "isPrimary": true,
        "vmInstanceCount": "[parameters('nt1InstanceCount')]"
    },
    {
        "name": "[parameters('vmNodeType2Name')]",
        "applicationPorts": {
            "endPort": "[parameters('nt2applicationEndPort')]",
            "startPort": "[parameters('nt2applicationStartPort')]"
        },
        "clientConnectionEndpointPort": "[parameters('nt2fabricTcpGatewayPort')]",
        "durabilityLevel": "Silver",
        "ephemeralPorts": {
            "endPort": "[parameters('nt2ephemeralEndPort')]",
            "startPort": "[parameters('nt2ephemeralStartPort')]"
        },
        "httpGatewayEndpointPort": "[parameters('nt2fabricHttpGatewayPort')]",
        "isPrimary": true,
        "vmInstanceCount": "[parameters('nt2InstanceCount')]"
    }
    ],
}
```

## <a name="migrate-to-using-availability-zones-from-a-cluster-using-a-basic-sku-load-balancer-and-a-basic-sku-ip"></a>Kullanılabilirlik alanları temel SKU yük dengeleyici ve bir temel SKU IP kullanarak bir kümeden kullanarak geçirme
Bir yük dengeleyici ve IP ile temel bir SKU'ya kullanıyordu, bir kümeye geçirmek için standart SKU'nun kullanarak tamamen yeni bir yük dengeleyici ve IP kaynağı oluşturun. Bu kaynakları yerinde güncelleştirmek mümkün değildir.

Yeni LB ve IP kullanmak istediğiniz yeni çapraz kullanılabilirlik alanı düğüm türü başvurulan. Yukarıdaki örnekte, üç yeni sanal makine ölçek kümesi kaynaklarının 1,2 ve 3 bölgelerinde eklenmiştir. Bu sanal makine ölçek kümeleri yeni oluşturulan LB ve IP başvurusu ve Service Fabric küme kaynağı birincil düğüm türleri olarak işaretlenir.

Başlamak için mevcut Resource Manager şablonunuzu yeni kaynakları eklemeniz gerekir. Bu kaynaklar içerir:
* Standart SKU kullanan bir genel IP kaynağı.
* Standart SKU kullanarak yük dengeleyici kaynak.
* Alt ağ, sanal makine ölçek kümeleri dağıttığınız tarafından başvurulan bir NSG.
* Üç düğüm türleri, birincil olarak işaretlendi.
    * Her düğüm türü kendi sanal makine ölçek kümesi farklı bölgelerde bulunan eşlenmesi gerekir.
    * Her sanal makine ölçek kümesi en az beş düğüm (Gümüş dayanıklılık) sahip olmalıdır.

Bu kaynaklar örneği bulunabilir [örnek şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/10-VM-Ubuntu-2-NodeType-Secure).

```powershell
New-AzureRmResourceGroupDeployment `
    -ResourceGroupName $ResourceGroupName `
    -TemplateFile $Template `
    -TemplateParameterFile $Parameters
```

Kaynakları, dağıtımı tamamladıktan sonra birincil düğüm türü özgün kümeden düğüm devre dışı bırakmak başlayabilirsiniz. Düğümleri devre dışı bırakılmış şekilde, Hizmetler'i Yukarıdaki adımda dağıtılan yeni birincil düğüm türü için geçirilecektir.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint $ClusterName `
    -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $thumb  `
    -FindType FindByThumbprint `
    -FindValue $thumb `
    -StoreLocation CurrentUser `
    -StoreName My 

Write-Host "Connected to cluster"

$nodeNames = @("_nt0_0", "_nt0_1", "_nt0_2", "_nt0_3", "_nt0_4")

Write-Host "Disabling nodes..."
foreach($name in $nodeNames) {
    Disable-ServiceFabricNode -NodeName $name -Intent RemoveNode -Force
}
```

Düğümlerin tümü devre dışı bırakılmıştır sonra Sistem Hizmetleri dilimlerinde dağıldığında birincil düğüm türünde çalıştırırsınız. Bu gibi durumlarda, devre dışı düğümler ardından kümesinden kaldırabilirsiniz. Düğümleri kaldırdıktan sonra özgün IP, Load Balancer'ı kaldırmanız ve kaynaklar sanal makine ölçek kümesi.

```powershell
foreach($name in $nodeNames){
    # Remove the node from the cluster
    Remove-ServiceFabricNodeState -NodeName $name -TimeoutSec 300 -Force
    Write-Host "Removed node state for node $name"
}

$scaleSetName="nt0"
Remove-AzureRmVmss -ResourceGroupName $groupname -VMScaleSetName $scaleSetName -Force

$lbname="LB-cluster-nt0"
$oldPublicIpName="LBIP-cluster-0"
$newPublicIpName="LBIP-cluster-1"

Remove-AzureRmLoadBalancer -Name $lbname -ResourceGroupName $groupname -Force
Remove-AzureRmPublicIpAddress -Name $oldPublicIpName -ResourceGroupName $groupname -Force
```

Ardından, bu kaynaklara başvurular dağıtılmış Resource Manager şablonundan kaldırmanız gerekir.

Son adım, genel IP ve DNS adını güncelleştirme içerecektir.

```powershell
$oldprimaryPublicIP = Get-AzureRmPublicIpAddress -Name $oldPublicIpName  -ResourceGroupName $groupname
$primaryDNSName = $oldprimaryPublicIP.DnsSettings.DomainNameLabel
$primaryDNSFqdn = $oldprimaryPublicIP.DnsSettings.Fqdn

Remove-AzureRmLoadBalancer -Name $lbname -ResourceGroupName $groupname -Force
Remove-AzureRmPublicIpAddress -Name $oldPublicIpName -ResourceGroupName $groupname -Force

$PublicIP = Get-AzureRmPublicIpAddress -Name $newPublicIpName  -ResourceGroupName $groupname
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzureRmPublicIpAddress -PublicIpAddress $PublicIP

```

[sf-architecture]: ./media/service-fabric-cross-availability-zones/sf-cross-az-topology.png
