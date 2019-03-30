---
title: Bir Azure Service Fabric düğüm türü ölçeklendirme | Microsoft Docs
description: Bir sanal makine ölçek kümesi ekleyerek bir Service Fabric kümesini ölçekleme hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/13/2019
ms.author: aljo
ms.openlocfilehash: fcf10152be645eb92596894a3e89258908d747c4
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58668373"
---
# <a name="scale-up-a-service-fabric-cluster-primary-node-type"></a>Bir Service Fabric küme birincil düğüm türü ölçeklendirin
Bu makalede, bir Service Fabric küme birincil düğüm türü, sanal makine kaynakları artırarak ölçeklendirme açıklar. Service Fabric kümesi bir ağa bağlı, mikro hizmetlerin dağıtıldığı ve yönetildiği sanal veya fiziksel makine kümesidir. Bir makine ya da bir kümenin parçası olan sanal makine bir düğüm denir. Sanal makine ölçek kümeleri dağıtmak ve sanal makine koleksiyonunu bir küme olarak yönetmek için kullandığınız bir Azure işlem kaynağıdır. Bir Azure kümesinde tanımlanan her düğüm türü [ayrı ölçek kümesi olarak ayarlanan](service-fabric-cluster-nodetypes.md). Ardından her düğüm türü ayrı olarak yönetilebilir. Service Fabric kümesi oluşturduktan sonra bir küme düğümü türü dikey olarak ölçeklendirebilirsiniz (düğümlerin kaynakları değişikliği) veya Vm'lere düğüm türündeki işletim sistemini yükseltin.  Kümedeki herhangi bir zamanda iş yükleri küme üzerinde çalışırken bile ölçeklendirebilirsiniz.  Küme ölçekler gibi uygulamalarınızı otomatik olarak da ölçeklendirin.

> [!WARNING]
> Birincil nodetype VM SKU değiştirmek küme sistem durumu iyi olmayan sistem ise başlatmayın. Küme sistem durumu kötü durumda, sanal makine SKU'su değiştirmeye çalışırsanız küme Ayrıca, yalnızca kararlılığını.
>
> Adresindeki çalışmadığı sürece bir ölçek kümesi/düğüm türündeki sanal makine SKU'su değiştirmeyin öneririz [dayanıklılık Gümüş veya daha büyük](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster). VM SKU boyutunu değiştirerek verileri yıkıcı yerinde altyapı bir işlemdir. Gecikme veya bu değişikliği izlemek için bazı kabiliyeti olmadan işlem durum bilgisi olan hizmetler için veri kaybına neden veya durum bilgisiz iş yükleri için bile diğer öngörülemeyen operasyonel sorunlara neden olabilir. Bu durum bilgisi olan service fabric sistem hizmetlerinin çalıştığından, birincil düğüm türünüz anlamına gelir veya durum bilgisi olan uygulama iş çalışmakta olan herhangi bir düğüm türü yükler.
>

## <a name="upgrade-the-size-and-operating-system-of-the-primary-node-type-vms"></a>Boyutu ve işletim sistemi birincil düğüm türündeki sanal makineleri yükseltme
VM boyutu ve işletim sistemi birincil düğüm türündeki sanal makineleri güncelleştirmek için süreç şöyledir.  Yükseltmeden sonra birincil düğüm türü VM boyutu standart D4_V2 ve kapsayıcılar ile çalışan Windows Server 2016 Datacenter verilmiştir.

> [!WARNING]
> Bir üretim kümesindeki bu yordama başlamadan önce örnek şablonları incelemek ve bir test kümesine göre işlemini doğrulama öneririz. Kümeyi bir süre için de kullanılamaz. Çoklu VMSS aynı NodeType paralel olarak bildirilen değişiklikleri yapabilirsiniz değil; ayrılmış dağıtım işlemlerini değişiklikleri her NodeType VMSS için ayrı ayrı uygulamak için gerçekleştirmeniz gerekir.

1. İki düğüm türleri ve iki ölçek kümeleri (bir ölçek kümesi düğüm türü) ile ilk küme dağıtma kullanarak bu örnek [şablon](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-2ScaleSets.json) ve [parametreleri](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-2ScaleSets.parameters.json) dosyaları.  Hem ölçek kümeleri, standart D2_V2 boyutu ve çalışan Windows Server 2012 R2 Datacenter ' dir.  Taban çizgisi yükseltmeyi tamamlamak küme için bekleyin.   
2. İsteğe bağlı - durum bilgisi olan bir örnek kümesine dağıtın.
3. Birincil düğüm türü Vm'leri yükseltmeye karar verdikten sonra bu örnek kullanarak birincil düğüm türü için yeni bir ölçek eklemek [şablon](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.json) ve [parametreleri](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.parameters.json) birincil düğüm türü artık iki ölçek kümesi bu dosyaları.  Sistem hizmetlerini ve kullanıcı uygulamaları iki farklı ölçek kümelerinizdeki VM'ler arasında geçirme olanağına sahip olursunuz.  Sanal makinelerin yeni bir ölçek kümesi, standart D4_V2 boyutunu ve kapsayıcılar ile Windows Server 2016 Datacenter'ı çalıştırın.  Yeni Yük Dengeleyici ve genel IP adresi ile yeni ölçek kümesi de eklenir.  
    Şablonda yeni ölçek bulmak için arama tarafından adlandırılan "Microsoft.Compute/virtualMachineScaleSets" kaynağı için *vmNodeType2Name* parametresi.  Birincil düğüm türü kullanarak yeni bir ölçek kümesi eklenen Özellikler -> virtualMachineProfile - > extensionprofile öğesine -> uzantılar -> Özellikler -> Ayarlar -> nodeTypeRef ayarı.
4. Küme sistem durumunu denetleyin ve tüm düğümleri sağlıklı olduğunu doğrulayın.
5. Eski bir ölçek kümesi düğümü kaldırmak amacıyla birincil düğüm türünde düğümler devre dışı bırakın. Tek seferde devre dışı bırakabilir ve işlemler kuyruğa alınır. Tüm düğümleri, biraz zaman alabilir, devre dışı bırakılana kadar bekleyin.  Eski düğümleri düğüm türü devre dışı olarak birincil düğüm türünde yeni ölçek vm'lere sistem hizmetleri ve çekirdek düğümleri geçirin.
6. Birincil düğüm türü eski ölçek kaldırın.
7. Eski bir ölçek kümesi ile ilişkili yük dengeleyici kaldırın. Yeni ortak IP adresi ve Yük Dengeleyiciyi yapılandırılmış olsa da yeni bir ölçek kümesi için küme kullanılamıyor.  
8. Store DNS ayarları eski birincil düğümle ilişkili genel IP adresinin, Ölçek kümesi bir değişkende yazın ve bu genel IP adresini kaldırın.
9. Silinen genel IP adresini DNS ayarlarıyla yeni birincil düğüm türü ölçek ile ilişkili genel IP adresini DNS ayarlarını değiştirin.  Küme artık yeniden ulaşılabilir olduğunda.
10. Düğüm durumu düğümleri kümeden kaldırırsınız.  Eski bir ölçek kümesi dayanıklılık düzeyini silver veya gold varsa, bu adım sistem tarafından otomatik olarak yapılır.
11. Durum bilgisi olan uygulamayı önceki bir adımda dağıttıysanız, uygulamanın çalıştığını doğrulayın.

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
* Bilgi edinmek için nasıl [bir kümeye bir düğüm türü ekleme](virtual-machine-scale-set-scale-node-type-scale-out.md)
* Hakkında bilgi edinin [uygulama ölçeklenebilirlik](service-fabric-concepts-scalability.md).
* [Azure kümesine veya dışa ölçeklendirme](service-fabric-tutorial-scale-cluster.md).
* [Azure bir kümeyi programlama yoluyla ölçeklendirme](service-fabric-cluster-programmatic-scaling.md) fluent Azure kullanarak işlem SDK.
* [Tek başına küme içe veya dışa ölçeklendirme](service-fabric-cluster-windows-server-add-remove-nodes.md).

