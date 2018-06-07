---
title: Bir Azure Service Fabric kümesi VM'ler SKU veya işletim sistemini yükseltme | Microsoft Docs
description: Service Fabric kümenin birincil nodetype'nde sanal makinelerin yükseltmeyi öğrenin.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/21/2018
ms.author: ryanwi
ms.openlocfilehash: bf707bf4b1c001b5467dd9954e9c6feb55e65654
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655822"
---
# <a name="upgrade-the-primary-node-type-vms-of-a-service-fabric-cluster"></a>Service Fabric kümesi birincil düğüm türü VM'ler yükseltme
Bu makalede, Azure'da çalışan Service Fabric kümesi birincil düğüm türü sanal makinelerin yükseltme açıklar.  Service Fabric kümesi bir ağa bağlı içine, mikro dağıtılır ve yönetilen sanal veya fiziksel makineler kümesidir. Bir makine ya da bir kümenin parçasıysa VM düğüm denir. Sanal makine ölçek kümeleri dağıtmak ve sanal makinelerin bir koleksiyon kümesi olarak yönetmek için kullandığınız bir Azure işlem kaynaktır. Bir Azure kümesinde tanımlanan her düğüm türü [ayrı ölçek kümesi olarak ayarlanan](service-fabric-cluster-nodetypes.md). Her düğüm türü sonra ayrı olarak yönetilebilir. Service Fabric kümesi oluşturduktan sonra bir küme düğümü türü dikey olarak ölçeklendirmek (düğümlerin kaynakları değiştirin) veya düğüm türü VM'ler işletim sistemini yükseltme.  Küme herhangi bir zamanda kümede iş yükleri çalıştırırken bile ölçeklendirebilirsiniz.  Küme ölçeklendirir gibi uygulamalarınızı otomatik olarak da ölçeklendirin.

> [!WARNING]
> Adresindeki çalışmıyorsa ölçek kümesi/düğüm türünde VM SKU değiştirmemenizi öneririz [Silver dayanıklılık veya daha büyük](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster). VM SKU boyutunu değiştirme veri bozucu yerinde altyapı bir işlemdir. Gecikme veya bu değişikliği izlemek için bazı özelliği olmadan işlemi durum bilgisi olan hizmetler için veri kaybına neden veya durum bilgisiz iş yükleri için bile öngörülemeyen diğer işlemsel sorunlara neden olabilir. 
>

## <a name="upgrade-the-size-and-operating-system-of-the-primary-node-type-vms"></a>Boyutu ve birincil düğüm türü VM'ler işletim sistemine yükseltme
VM boyutu ve işletim sistemi birincil düğüm türü VM'ler güncelleştirme için işlem şöyledir.  Yükseltmeden sonra birincil düğüm türü VM'ler boyutu standart D4_V2 ve kapsayıcılar ile çalışan Windows Server 2016 Datacenter verilmiştir.

> [!WARNING]
> Bir üretim kümede bu yordama başlamadan önce örnek şablonları araştırmak ve test kümesi karşı işlemini doğrulama öneririz. Küme, ayrıca bir süre için kullanılamaz.

1. İlk küme iki düğüm türleri ve iki ölçek kümeleri (bir ölçek kümesi düğüm türü) ile dağıtma bu örneğini kullanarak [şablonu](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-2ScaleSets.json) ve [parametreleri](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-2ScaleSets.parameters.json) dosyaları.  Her iki ölçek boyutu standart D2_V2 ve çalışan Windows Server 2012 R2 Datacenter kümeleridir.  Taban çizgisi yükseltmeyi tamamlamak küme için bekleyin.   
2. İsteğe bağlı - durum bilgisi olan bir örnek kümeye dağıtın.
3. Birincil düğüm türü VM'ler yükseltmeye karar verildikten sonra bu örnek kullanarak birincil düğüm türü ayarlamak yeni bir ölçek eklemek [şablonu](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.json) ve [parametreleri](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.parameters.json) nedenle birincil düğüm türü şimdi iki ölçek kümesi dosyaları.  Sistem hizmetlerini ve kullanıcı uygulamaları iki farklı ölçek kümesindeki VM'ler arasında geçirmek kullanabilirsiniz.  VM'ler yeni ölçek kümesini standart D4_V2 boyut ve Windows Server 2016 Datacenter kapsayıcılarını ile çalıştırın.  Yeni Yük Dengeleyici ve genel IP adresi ile yeni ölçek kümesi de eklenir.  
    Yeni ölçeği şablonda ayarla bulmak için arama tarafından adlı "Microsoft.Compute/virtualMachineScaleSets" kaynağı için *vmNodeType2Name* parametresi.  Birincil düğüm türü kullanılarak yeni ölçek kümesi eklenen Özellikler -> virtualMachineProfile - > extensionProfile -> uzantılar -> Özellikler -> ayarları nodeTypeRef ayarı ->.
4. Küme sistem durumunu denetleyin ve tüm düğümleri sağlıklı olduğunu doğrulayın.
5. Birincil düğüm türü düğümünü kaldırmak için amacıyla eski ölçek kümesi içindeki düğümlerin devre dışı bırakın. Aynı anda devre dışı bırakabilir ve işlemleri sıralanır. Tüm düğümler, hangi biraz zaman alabilir devre dışı bırakılana kadar bekleyin.  Düğüm türü eski düğümlerin devre dışı olarak, yeni ölçeği birincil düğüm türünde Ayarla VM'ler için sistem hizmetleri ve çekirdek düğümleri geçirin.
6. Eski ölçeği birincil düğüm türü Ayarla kaldırın.
7. Eski ölçek kümesiyle ilişkili yük dengeleyici kaldırın. Küme, yeni ortak IP adresi ve yük dengeleyici yapılandırılması sırasında yeni ölçek kümesi için kullanılamaz.  
8. Mağaza DNS ayarları eski birincil düğümle ilişkilendirilen ortak IP adresinin bir değişkende ölçek kümesini yazın ve bu ortak IP adresini kaldırın.
9. Ortak IP adresi silindi DNS ayarlarıyla yeni birincil düğüm türü ölçeği ile ilişkili ortak IP adresinin DNS ayarlarını değiştirin.  Küme artık yeniden ulaşılabilir olduğunda.
10. Düğüm durumu düğümü kümeden kaldırın.  Eski ölçek kümesini dayanıklılık düzeyi, Gümüş veya altın idiyse, bu adım sistem tarafından otomatik olarak yapılır.
11. Bir önceki adımda durum bilgisi olan uygulama dağıtıldıysa, uygulama işlevsel olduğunu doğrulayın.

```powershell
# Variables.
$groupname = "sfupgradetestgroup"
$clusterloc="southcentralus"  
$subscriptionID="<your subscription ID>"

# sign in to your Azure account and select your subscription
Login-AzureRmAccount -SubscriptionId $subscriptionID 

# Create a new resource group for your deployment and give it a name and a location.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

# Deploy the two node type cluster.
New-AzureRmResourceGroupDeployment -ResourceGroupName $groupname -TemplateParameterFile "C:\temp\cluster\Deploy-2NodeTypes-2ScaleSets.parameters.json" `
    -TemplateFile "C:\temp\cluster\Deploy-2NodeTypes-2ScaleSets.json" -Verbose

# Connect to the cluster and check the cluster health.
$ClusterName= "sfupgradetest.southcentralus.cloudapp.azure.com:19000"
$thumb="F361720F4BD5449F6F083DDE99DC51A86985B25B"

Connect-ServiceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $thumb  `
    -FindType FindByThumbprint `
    -FindValue $thumb `
    -StoreLocation CurrentUser `
    -StoreName My 

Get-ServiceFabricClusterHealth

# Deploy a new scale set into the primary node type.  Create a new load balancer and public IP address for the new scale set.
New-AzureRmResourceGroupDeployment -ResourceGroupName $groupname -TemplateParameterFile "C:\temp\cluster\Deploy-2NodeTypes-3ScaleSets.parameters.json" `
    -TemplateFile "C:\temp\cluster\Deploy-2NodeTypes-3ScaleSets.json" -Verbose

# Check the cluster health again. All 15 nodes should be healthy.
Get-ServiceFabricClusterHealth

# Disable the nodes in the original scale set.
$nodeNames = @("_NTvm1_0","_NTvm1_1","_NTvm1_2","_NTvm1_3","_NTvm1_4")

Write-Host "Disabling nodes..."
foreach($name in $nodeNames){
    Disable-ServiceFabricNode -NodeName $name -Intent RemoveNode -Force
}

Write-Host "Checking node status..."
foreach($name in $nodeNames){
 
    $state = Get-ServiceFabricNode -NodeName $name 

    $loopTimeout = 50

    do{
        Start-Sleep 5
        $loopTimeout -= 1
        $state = Get-ServiceFabricNode -NodeName $name
        Write-Host "$name state: " $state.NodeDeactivationInfo.Status
    }

    while (($state.NodeDeactivationInfo.Status -ne "Completed") -and ($loopTimeout -ne 0))
    

    if ($state.NodeStatus -ne [System.Fabric.Query.NodeStatus]::Disabled)
    {
        Write-Error "$name node deactivation failed with state" $state.NodeStatus
        exit
    }
}

# Remove the scale set
$scaleSetName="NTvm1"
Remove-AzureRmVmss -ResourceGroupName $groupname -VMScaleSetName $scaleSetName -Force
Write-Host "Removed scale set $scaleSetName"

$lbname="LB-sfupgradetest-NTvm1"
$oldPublicIpName="PublicIP-LB-FE-0"
$newPublicIpName="PublicIP-LB-FE-2"

# Store DNS settings of public IP address related to old Primary NodeType into variable 
$oldprimaryPublicIP = Get-AzureRmPublicIpAddress -Name $oldPublicIpName  -ResourceGroupName $groupname

$primaryDNSName = $oldprimaryPublicIP.DnsSettings.DomainNameLabel

$primaryDNSFqdn = $oldprimaryPublicIP.DnsSettings.Fqdn

# Remove Load Balancer related to old Primary NodeType. This will cause a brief period of downtime for the cluster
Remove-AzureRmLoadBalancer -Name $lbname -ResourceGroupName $groupname -Force

# Remove the old public IP
Remove-AzureRmPublicIpAddress -Name $oldPublicIpName -ResourceGroupName $groupname -Force

# Replace DNS settings of Public IP address related to new Primary Node Type with DNS settings of Public IP address related to old Primary Node Type
$PublicIP = Get-AzureRmPublicIpAddress -Name $newPublicIpName  -ResourceGroupName $groupname
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzureRmPublicIpAddress -PublicIpAddress $PublicIP

# Check the cluster health
Get-ServiceFabricClusterHealth

# Remove node state for the deleted nodes.
foreach($name in $nodeNames){
    # Remove the node from the cluster
    Remove-ServiceFabricNodeState -NodeName $name -TimeoutSec 300 -Force
    Write-Host "Removed node state for node $name"
}
```

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [uygulama ölçeklenebilirlik](service-fabric-concepts-scalability.md).
* [Bir Azure bir küme veya ölçeklendirme](service-fabric-tutorial-scale-cluster.md).
* [Bir Azure küme programlı olarak ölçekleme](service-fabric-cluster-programmatic-scaling.md) fluent Azure kullanarak işlem SDK.
* [Giriş veya çıkış standaone küme ölçeklendirme](service-fabric-cluster-windows-server-add-remove-nodes.md).

