---
title: Azure'daki bir Service Fabric kümesini ölçeklendirme | Microsoft Docs
description: Bu öğreticide, Azure'daki bir Service Fabric kümesini hızlı bir şekilde ölçeklendirmeyi öğreneceksiniz.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 010/01/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 54433aa9e618bdde7badec3be8a786f943f88198
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51236922"
---
# <a name="tutorial-scale-a-service-fabric-cluster-in-azure"></a>Öğretici: Azure'daki bir Service Fabric kümesini ölçeklendirme

Bu öğretici, bir serinin ikinci kısmı olup mevcut kümenizin ölçeğini nasıl genişleteceğinizi ve daraltacağınızı gösterir. Tamamladığınızda, kümenizin nasıl ölçekleneceğini ve kalan kaynakların nasıl temizleneceğini öğrenmiş olacaksınız.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Küme düğümü sayısını okuma
> * Küme düğümleri ekleme (ölçeği genişletme)
> * Küme düğümlerini kaldırma (ölçeği daraltma)

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Şablon kullanarak Azure'da güvenli bir [Windows kümesi](service-fabric-tutorial-create-vnet-and-windows-cluster.md) veya [Linux kümesi](service-fabric-tutorial-create-vnet-and-linux-cluster.md) oluşturma
> * Bir kümenin ölçeğini daraltma veya genişletme
> * [Bir kümenin çalışma zamanını yükseltme](service-fabric-tutorial-upgrade-cluster.md)
> * [Küme silme](service-fabric-tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun
* [Azure Powershell modülü sürüm 4.1 veya üzerini](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) ya da [Azure CLI](/cli/azure/install-azure-cli)'yı yükleyin.
* Azure'da güvenli bir [Windows kümesi](service-fabric-tutorial-create-vnet-and-windows-cluster.md) veya [Linux kümesi](service-fabric-tutorial-create-vnet-and-linux-cluster.md) oluşturma
* Windows kümesi dağıtıyorsanız, bir Windows dağıtım ortamı ayarlayın. [Visual Studio 2017](https://www.visualstudio.com)'yi ve **Azure geliştirme**, **ASP.NET ve web geliştirme**, ayrıca **.NET Core çoklu platform geliştirme** iş yüklerini yükleyin.  Ardından bir [.NET dağıtım ortamı](service-fabric-get-started.md) ayarlayın.
* Linux kümesi dağıtıyorsanız, [Linux](service-fabric-get-started-linux.md) veya [MacOS](service-fabric-get-started-mac.md) üzerinde bir Java dağıtım ortamı ayarlayın.  [Service Fabric CLI](service-fabric-cli.md)'yı yükleyin.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure komutlarını yürütmeden önce Azure hesabınızda oturum açıp aboneliğinizi seçin.

```powershell
Connect-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

```azurecli
az login
az account set --subscription <guid>
```

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

Öğreticinin bu kısmını başarıyla tamamlamak için hem Service Fabric kümesine hem de sanal makine ölçek kümesine (kümeyi barındıran) bağlanmanız gerekir. Sanal makine ölçek kümesi, Azure’da Service Fabric’i barındıran Azure kaynağıdır.

Bir kümeye bağlandığınızda, bilgi için kümeyi sorgulayabilirsiniz. Kümenin hangi düğümleri bildiğini öğrenmek için kümeyi kullanabilirsiniz. Aşağıdaki kodda yer alan kümeye bağlanmak için, bu serinin ilk kısmında oluşturulan aynı sertifika kullanılır. `$endpoint` ve `$thumbprint` değişkenlerini kendi değerlerinize ayarladığınızdan emin olun.

```powershell
$endpoint = "<mycluster>.southcentralus.cloudapp.azure.com:19000"
$thumbprint = "63EB5BA4BC2A3BADC42CA6F93D6F45E5AD98A1E4"

Connect-ServiceFabricCluster -ConnectionEndpoint $endpoint `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint $thumbprint `
          -FindType FindByThumbprint -FindValue $thumbprint `
          -StoreLocation CurrentUser -StoreName My

Get-ServiceFabricClusterHealth
```

```azurecli
sfctl cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 \
--pem ./aztestcluster201709151446.pem --no-verify
```

Şimdi bağlandığınıza göre, kümedeki her bir düğümün durumunu almak için bir komut kullanabilirsiniz. **PowerShell** için `Get-ServiceFabricClusterHealth` komutunu ve **sfctl** için `sfctl cluster select` komutunu kullanın.

## <a name="scale-out"></a>Ölçeği genişletme

Ölçeği genişlettiğinizde, ölçek kümesine daha fazla sanal makine örneği eklersiniz. Bu örnekler, Service Fabric tarafından kullanılan düğümler haline gelir. Service Fabric, ölçek kümesine ne zaman daha fazla örnek eklendiğini bilir (ölçek genişletilerek) ve otomatik olarak tepki verir. Aşağıdaki kod, ada göre bir ölçek kümesi alır ve ölçek kümesinin **kapasitesini** 1 artırır.

```powershell
$scaleset = Get-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity += 1

Update-AzureRmVmss -ResourceGroupName $scaleset.ResourceGroupName -VMScaleSetName $scaleset.Name -VirtualMachineScaleSet $scaleset
```

Bu kod, kapasiteyi 6 olarak ayarlar.

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 6
```

## <a name="scale-in"></a>Ölçeği daraltma

Ölçeği daraltma, ölçeği genişletme ile aynıdır; tek fark daha düşük bir **kapasite** değeri kullanılmasıdır. Ölçek kümesinin ölçeğini daralttığınızda, ölçek kümesinden sanal makine örneklerini kaldırırsınız. Normalde Service Fabric ne olduğunu fark etmez ve bir düğümün kaybolduğunu düşünür. Daha sonra Service Fabric, küme için kötü bir durum olduğunu bildirir. Bu kötü durumu önlemek için, Service Fabric’e düğümün kaybolmasını beklediğinizi bildirmeniz gerekir.

### <a name="remove-the-service-fabric-node"></a>Service Fabric düğümünü kaldırma

> [!NOTE]
> Bu kısım yalnızca *Bronz* dayanıklılık katmanı için geçerlidir. Dayanıklılık hakkında daha fazla bilgi için bkz. [Service Fabric küme kapasitesi planlaması][durability].

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
$scaleset = Get-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity -= 1

Update-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm -VirtualMachineScaleSet $scaleset
```

Bu kod, kapasiteyi 5 olarak ayarlar.

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 5
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Küme düğümü sayısını okuma
> * Küme düğümleri ekleme (ölçeği genişletme)
> * Küme düğümlerini kaldırma (ölçeği daraltma)

Ardından, kümenin çalışma zamanının nasıl yükseltileceğini öğrenmek için aşağıdaki öğreticiye geçin.
> [!div class="nextstepaction"]
> [Bir kümenin çalışma zamanını yükseltme](service-fabric-tutorial-upgrade-cluster.md)

[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
