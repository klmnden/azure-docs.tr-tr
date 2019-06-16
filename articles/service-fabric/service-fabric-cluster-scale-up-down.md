---
title: İç veya dış bir Service Fabric kümesini ölçekleme | Microsoft Docs
description: Her düğüm türü/sanal makine ölçek kümesi için otomatik ölçeklendirme kurallarını ayarlayarak talebini eşleştirmek için Service Fabric kümesini içe veya dışa ölçeklendirme. Bir Service Fabric küme düğümleri ekleyebilir veya kaldırabilirsiniz
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: aeb76f63-7303-4753-9c64-46146340b83d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/12/2019
ms.author: aljo
ms.openlocfilehash: 400e4653800d445506d4854e70034a707dcc4629
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66161798"
---
# <a name="scale-a-cluster-in-or-out"></a>Bir kümenin ölçeğini daraltma veya genişletme

> [!WARNING]
> Ölçeği önce bu bölümü okuyun

Uygulama iş yükünüz kasıtlı planlama gerektirir, neredeyse her zaman bir üretim ortamında tamamlamak için bir saatten daha uzun sürer ve iş yükü ve iş bağlamını anlamak ihtiyacınız kaynağına işlem kaynaklarını ölçeklendirme; Bu etkinlik önce hiçbir zaman yaptıysanız, aslında, okuma ve anlama başlattığınız önerilir [Service Fabric kümesi kapasite planlaması konuları](service-fabric-cluster-capacity.md), bu belgenin geri kalanında devam etmeden önce. Bu istenmeyen LiveSite sorunlarını önlemek için önerilir ve ayrıca bir üretim dışı ortamda karşı gerçekleştirmeye karar işlemleri başarıyla test önerilir. Herhangi bir zamanda yapabilecekleriniz [üretim sorunlarını bildirmek veya Azure için Ücretli destek isteği](service-fabric-support.md#report-production-issues-or-request-paid-support-for-azure). Mühendislerin yeterli bağlama sahip bu işlemleri gerçekleştirmek için ayrılan, bu makalede ölçeklendirme işlemleri açıklanmaktadır, ancak karar verin ve işlemleri, kullanım örneği için uygun olduğunu anlamak; hangi kaynakları ölçeklendirme (CPU, depolama, bellek) gibi hangi yönü (yatay veya dikey olarak) ölçeklendirmek için ve hangi işlemleri (kaynak şablonu dağıtımı, Portal, PowerShell/CLI) gerçekleştirin.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules-or-manually"></a>Bir Service Fabric kümesini otomatik ölçeklendirme kurallarını kullanarak içe veya dışa ölçeklendirme veya el ile
Sanal makine ölçek kümeleri, dağıtmak ve sanal makine koleksiyonunu bir küme olarak yönetmek için kullanabileceğiniz bir Azure işlem kaynağıdır. Bir Service Fabric kümesinde tanımlanan her düğüm türü ayrı bir sanal makine ölçek kümesi ayarlanır. Her düğüm türü, ölçeklendirilebilir veya out bağımsız olarak, farklı bağlantı noktası kümeleri açık olan ve farklı kapasite ölçümleri yapılabilir. İçinde hakkında daha fazla bilgiyi [Service Fabric düğüm türleri](service-fabric-cluster-nodetypes.md) belge. Kümenizde Service Fabric düğüm türleri arka uçtaki sanal makine ölçek kümelerinin yapılan olduğundan, her düğüm türü/sanal makine ölçek kümesi için otomatik ölçeklendirme kurallarını ayarlamanız gerekir.

> [!NOTE]
> Bu kümeyi oluşturan yeni Vm'leri eklemek için yeterli çekirdek aboneliğinizin olması gerekir. Model doğrulama yoktur şu anda, herhangi bir kota sınırları ulaşırsanız dağıtım zamanı hatası alabilmeniz. Ayrıca tek bir düğüm türü VMSS başına 100 düğüm yalnızca aşamaz. VMSS kullanıcının hedeflenen ölçeğe ulaşmak için ve otomatik ölçeklendirme başlatamaz eklemeniz gerekebilir automagically VMSS'ın ekleyin. Yerinde VMSS'ın dinamik bir kümeye ekleme, zorlu bir görevdir ve yaygın olarak kullanıcıların yeni küme oluşturma sırasında sağlanan uygun düğümü türleri ile sağlama sonuçlanır; [küme kapasitesini planlama](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) uygun şekilde. 
> 
> 

## <a name="choose-the-node-typevirtual-machine-scale-set-to-scale"></a>Düğüm türü/sanal makine ölçek kümesi ölçeği seçin
Şu anda, portalı kullanarak bir Service Fabric kümesi oluşturma, bize bu nedenle düğüm türlerini listelemek ve bunlara otomatik ölçeklendirme kurallarını eklemek için Azure PowerShell (1.0 +) kullanmak için sanal makine ölçek kümeleri için otomatik ölçeklendirme kurallarını belirtmek mümkün değildir.

Kümeyi oluşturan sanal makine ölçek kümesi listesini almak için aşağıdaki cmdlet'leri çalıştırın:

```powershell
Get-AzResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzVmss -ResourceGroupName <RGname> -VMScaleSetName <virtual machine scale set name>
```

## <a name="set-auto-scale-rules-for-the-node-typevirtual-machine-scale-set"></a>Düğüm türü/sanal makine ölçek kümesi için otomatik ölçeklendirme kurallarını ayarlama
Birden çok düğüm türleri kümenizi varsa, bu her düğüm türü/sanal makine Ölçek (iç veya dış) ölçeklendirmek istediğiniz ayarlar yineleyin. Otomatik ölçeklendirmeyi ayarlamadan önce sahip olmanız gereken düğüm sayısını dikkate alın. Birincil düğüm türü için gereken düğüm sayısı alt sınırı seçmiş olduğunuz güvenilirlik düzeyi tarafından yönetilir. Daha fazla bilgi edinin [güvenilirlik düzeylerini](service-fabric-cluster-capacity.md).

> [!NOTE]
> Birincil düğüm ölçeklendirme değerinden daha düşük sayı yap kararsız küme yazın veya taşıyın. Bu, sistem hizmetleri ve uygulamalarınız için veri kaybına neden olabilir.
> 
> 

Şu anda otomatik ölçeklendirme özelliği için Service Fabric uygulamalarınızı raporlama yükleri tarafından yönlendirilen değil. Bu nedenle şu anda size otomatik ölçeklendirme tamamen tarafından yayılan her sanal makinenin performans sayaçlarını kullanan ölçek kümesi örnekleri.  

Bu yönergeleri izleyin [otomatik ölçeklendirme, her sanal makine ölçek kümesi için ayarlamak için](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

> [!NOTE]
> Bir ölçek senaryosu, aşağı, düğüm türü sahip olmadığı sürece bir [dayanıklılık düzeyi] [ durability] Silver veya Gold çağırmanız gerekir [Remove-ServiceFabricNodeState cmdlet'i](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate) ile uygun düğümü adı. Bronz dayanıklılık sağlamak için aynı anda birden fazla düğümlerini ölçeklendirmek için önerilmez.
> 
> 

## <a name="manually-add-vms-to-a-node-typevirtual-machine-scale-set"></a>Vm'leri bir düğüm türü/sanal makine ölçek kümesi için el ile Ekle

Ölçeği genişlettiğinizde, ölçek kümesine daha fazla sanal makine örneği eklersiniz. Bu örnekler, Service Fabric tarafından kullanılan düğümler haline gelir. Service Fabric, ölçek kümesine ne zaman daha fazla örnek eklendiğini bilir (ölçek genişletilerek) ve otomatik olarak tepki verir. 

> [!NOTE]
> Vm'leri ekleme anında eklemeleri beklemiyoruz şekilde zaman alır. Bu nedenle VM kapasitesi çoğaltmaları/hizmet örneklerinin yerleştirilen kullanılabilir olmadan önce tekrar 10 dakika için izin vermek için önceden, kapasite eklemeyi planlayın.
> 

### <a name="add-vms-using-a-template"></a>Şablon kullanarak VM ekleme
Örnek/yönergeleri izleyin [hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) her düğüm türünde VM sayısını değiştirmek için. 

### <a name="add-vms-using-powershell-or-cli-commands"></a>PowerShell veya CLI komutlarını kullanarak VM ekleme
Aşağıdaki kod, ada göre bir ölçek kümesi alır ve ölçek kümesinin **kapasitesini** 1 artırır.

```powershell
$scaleset = Get-AzVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity += 1

Update-AzVmss -ResourceGroupName $scaleset.ResourceGroupName -VMScaleSetName $scaleset.Name -VirtualMachineScaleSet $scaleset
```

Bu kod, kapasiteyi 6 olarak ayarlar.

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 6
```

## <a name="manually-remove-vms-from-a-node-typevirtual-machine-scale-set"></a>Vm'leri bir düğüm türü/sanal makine ölçek kümesinden el ile kaldırma
Bir düğüm türündeki ölçeğini daralttığınızda ölçek kümesi VM örneklerini kaldırın. Düğüm türü Bronz dayanıklılık düzeyi ise, Service Fabric ne olduğunu ve bir düğümün kaybolduğunu raporları farkında değil. Daha sonra Service Fabric, küme için kötü bir durum olduğunu bildirir. Bu kötü durumu önlemek için açıkça düğümü kümeden kaldırma ve düğüm durumu kaldırmanız gerekir.

Birincil düğüm türü kümenizde service fabric sistem hizmetlerinin çalıştırın. Hiçbir zaman birincil düğüm türü ölçeklendirme, daha düşük örneği sayısını ölçeği [güvenilirlik katmanını](service-fabric-cluster-capacity.md) gerektirir. 
 
Bir durum bilgisi olan hizmet için belirli bir sayıda olması her zaman en çok kullanılabilirliği sürdürmek ve hizmetinizi durumunu korumak için düğümleri gerekir. Çok az düğüm sayısını bölüm/hizmet hedef çoğaltma kümesi sayısı eşittir ihtiyacınız vardır.

### <a name="remove-the-service-fabric-node"></a>Service Fabric düğümünü kaldırma

Düğüm durumu el ile kaldırma adımlarını yalnızca düğüm türü ile geçerli bir *Bronz* dayanıklılık katmanı.  İçin *Silver* ve *Gold* dayanıklılık katmanı, bu adımları yapılır otomatik olarak platform tarafından. Dayanıklılık hakkında daha fazla bilgi için bkz. [Service Fabric küme kapasitesi planlaması][durability].

Küme düğümlerini yükseltme ve hata etki alanlarına eşit olarak dağıtarak eşit bir şekilde kullanılmalarını sağlamak için önce en son oluşturulan düğümün kaldırılması gerekir. Başka bir deyişle düğümler, oluşturma sırasının tersine kaldırılmalıdır. En son oluşturulan düğüm, `virtual machine scale set InstanceId` özelliğinin değeri en yüksek olandır. Aşağıdaki kod örnekleri en son oluşturulan düğümü döndürür.

```powershell
Get-ServiceFabricNode | Sort-Object { $_.NodeName.Substring($_.NodeName.LastIndexOf('_') + 1) } -Descending | Select-Object -First 1
```

```azurecli
sfctl node list --query "sort_by(items[*], &name)[-1]"
```

Service Fabric kümesinin, bu düğümün kaldırılacağını bilmesi gerekir. Uygulamanız gereken üç adım vardır:

1. Artık veriler için çoğaltma olmaması için düğümü devre dışı bırakın.  
PowerShell: `Disable-ServiceFabricNode`  
sfctl: `sfctl node disable`

2. Service Fabric çalışma zamanının düzgün şekilde kapanması ve uygulamanızın bir sonlandırma isteği alması için düğümü durdurun.  
PowerShell: `Start-ServiceFabricNodeTransition -Stop`  
sfctl: `sfctl node transition --node-transition-type Stop`

2. Kümeden düğümü kaldırın.  
PowerShell: `Remove-ServiceFabricNodeState`  
sfctl: `sfctl node remove-state`

Bu üç adım düğüme uygulandıktan sonra ölçek kümesinden kaldırılabilir. [Bronz][durability]’un yanı sıra dayanıklılık katmanları kullanıyorsanız ölçek kümesi örneği kaldırıldığında bu adımlar sizin için uygulanır.

Aşağıdaki kod bloğu, en son oluşturulan düğümü alır, düğümü devre dışı bırakır, durdurur ve kümeden kaldırır.

```powershell
#### After you've connected.....
# Get the node that was created last
$node = Get-ServiceFabricNode | Sort-Object { $_.NodeName.Substring($_.NodeName.LastIndexOf('_') + 1) } -Descending | Select-Object -First 1

# Node details for the disable/stop process
$nodename = $node.NodeName
$nodeid = $node.NodeInstanceId

$loopTimeout = 10

# Run disable logic
Disable-ServiceFabricNode -NodeName $nodename -Intent RemoveNode -TimeoutSec 300 -Force

$state = Get-ServiceFabricNode | Where-Object NodeName -eq $nodename | Select-Object -ExpandProperty NodeStatus

while (($state -ne [System.Fabric.Query.NodeStatus]::Disabled) -and ($loopTimeout -ne 0))
{
    Start-Sleep 5
    $loopTimeout -= 1
    $state = Get-ServiceFabricNode | Where-Object NodeName -eq $nodename | Select-Object -ExpandProperty NodeStatus
    Write-Host "Checking state... $state found"
}

# Exit if the node was unable to be disabled
if ($state -ne [System.Fabric.Query.NodeStatus]::Disabled)
{
    Write-Error "Disable failed with state $state"
}
else
{
    # Stop node
    $stopid = New-Guid
    Start-ServiceFabricNodeTransition -Stop -OperationId $stopid -NodeName $nodename -NodeInstanceId $nodeid -StopDurationInSeconds 300

    $state = (Get-ServiceFabricNodeTransitionProgress -OperationId $stopid).State
    $loopTimeout = 10

    # Watch the transaction
    while (($state -eq [System.Fabric.TestCommandProgressState]::Running) -and ($loopTimeout -ne 0))
    {
        Start-Sleep 5
        $state = (Get-ServiceFabricNodeTransitionProgress -OperationId $stopid).State
        Write-Host "Checking state... $state found"
    }

    if ($state -ne [System.Fabric.TestCommandProgressState]::Completed)
    {
        Write-Error "Stop transaction failed with $state"
    }
    else
    {
        # Remove the node from the cluster
        Remove-ServiceFabricNodeState -NodeName $nodename -TimeoutSec 300 -Force
    }
}
```

Aşağıdaki **sfctl** kodunda, en son oluşturulan düğümün **node-name** değerini almak için şu komut kullanılır: `sfctl node list --query "sort_by(items[*], &name)[-1].name"`

```azurecli
# Inform the node that it is going to be removed
sfctl node disable --node-name _nt1vm_5 --deactivation-intent 4 -t 300

# Stop the node using a random guid as our operation id
sfctl node transition --node-instance-id 131541348482680775 --node-name _nt1vm_5 --node-transition-type Stop --operation-id c17bb4c5-9f6c-4eef-950f-3d03e1fef6fc --stop-duration-in-seconds 14400 -t 300

# Remove the node from the cluster
sfctl node remove-state --node-name _nt1vm_5
```

> [!TIP]
> Her bir adımın durumunu denetlemek için şu **sfctl** sorgularını kullanın
>
> **Devre dışı bırakma durumunu denetleme**
> `sfctl node list --query "sort_by(items[*], &name)[-1].nodeDeactivationInfo"`
>
> **Durdurma durumunu denetleme**
> `sfctl node list --query "sort_by(items[*], &name)[-1].isStopped"`
>

### <a name="scale-in-the-scale-set"></a>Ölçek kümesinin ölçeğini daraltma

Şimdi Service Fabric düğümü kümeden kaldırıldığına göre sanal makine ölçek kümesinin ölçeği daraltılabilir. Aşağıdaki örnekte, ölçek kümesi kapasitesi 1 azaltılır.

```powershell
$scaleset = Get-AzVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity -= 1

Update-AzVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm -VirtualMachineScaleSet $scaleset
```

Bu kod, kapasiteyi 5 olarak ayarlar.

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 5
```

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>Service Fabric Explorer'ın davranışlarla karşılaşabilirsiniz
Bir kümenin ölçeğini artırdığınızda, Service Fabric Explorer (sanal makine ölçek kümesi örnekleri) ve kümenin parçası olan düğüm sayısını yansıtır.  Ancak, ölçeğini daralttığınızda, kümeyi kötü durumda, çağırmadığınız sürece görüntülenen kaldırılan düğüm/sanal makine örneği görürsünüz [Remove-ServiceFabricNodeState cmd](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate) uygun düğümü ada sahip.   

Bu davranış için açıklaması aşağıda verilmiştir.

Service Fabric Explorer'da listelenen bir yansıma Service Fabric sistem hizmetlerinin düğümlerdir (FM özellikle) kümesi vardı/sahip düğüm sayısını hakkında bilir. Sanal makine ölçek kümesini ölçeklendirme, sanal makine silinmiş, ancak FM sistem hizmeti hala gördüğü (silindikten sonra VM eşlendi) düğüme geri dönün. Bu nedenle Service Fabric Explorer (sistem durumu hata veya bilinmeyen olabilir, ancak) düğüm görüntülemeye devam eder.

Bir düğüm, bir VM kaldırıldığında kaldırıldığını emin olmak için iki seçeneğiniz vardır:

1. Altyapı tümleştirmesinde size, kümedeki düğüm türleri için Silver veya Gold bir dayanıklılık düzeyi seçin. Ölçeği aşağı olduğunda, ardından otomatik olarak düğümleri bizim Sistem Hizmetleri (FM) durumundan kaldırır.
Başvurmak [dayanıklılık düzeyleri ayrıntıları](service-fabric-cluster-capacity.md)

2. Sanal makine örneği ölçeği bir kez çağırmanız gerekir [Remove-ServiceFabricNodeState cmdlet'i](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate).

> [!NOTE]
> Service Fabric kümeleri yedekleme kullanılabilirliği sürdürmek ve durumu "çekirdek koruma olarak." başvurulan - korumak için her zaman olması için düğümleri belirli sayıda gerektirir Bu nedenle, kümedeki tüm makinelerin ilk yapmadığınız sürece kapatmak için genellikle güvenli olmayan bir [durumunuzu tam yedekleme](service-fabric-reliable-services-backup-restore.md).
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Küme kapasitesini planlama, Küme yükseltme ve hizmetlerini bölümleme hakkında da bilgi için aşağıdakileri okuyun:

* [Küme kapasitenizi planlama](service-fabric-cluster-capacity.md)
* [Küme yükseltme](service-fabric-cluster-upgrade.md)
* [Bölüm durum bilgisi olan hizmetler için en yüksek ölçek](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png

[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
