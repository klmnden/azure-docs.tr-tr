---
title: "Azure Service Fabric kümesi ölçeklendirin | Microsoft Docs"
description: "Service Fabric kümesi kolayca ölçek öğrenin."
services: service-fabric
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/24/2017
ms.author: adegeo
ms.openlocfilehash: baed3e290ed29d1459455441e7cac7c5b1b1ebd9
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="scale-a-service-fabric-cluster"></a>Ölçek Service Fabric kümesi

Bu öğretici iki serinin bir parçasıdır ve varolan kümenizi ve ölçeklendirmek nasıl gösterir. Seçtiğiniz zaman bitirdiğinizde, kümenizi ölçeklendirme ve tüm sol üzerinden kaynakları temizlemek nasıl anlarsınız.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Küme düğüm sayısı okuma
> * Küme düğümleri (ölçeklendirme) Ekle
> * Küme düğümleri (Ölçek) Kaldır

Bu öğretici serisinde öğrenin nasıl yapılır:
> [!div class="checklist"]
> * Güvenli oluşturma [Windows Küme](service-fabric-tutorial-create-vnet-and-windows-cluster.md) veya [Linux kümesi](service-fabric-tutorial-create-vnet-and-linux-cluster.md) bir şablonu kullanarak azure'da
> * Bir küme veya ölçeklendirme
> * [Yükseltme küme çalışma zamanı](service-fabric-tutorial-upgrade-cluster.md)
> * [API Management Service Fabric ile dağıtma](service-fabric-tutorial-deploy-api-management.md)

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce:
- Bir Azure aboneliğiniz yoksa, oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Yükleme [Azure Powershell modülü sürümü 4.1 veya üzerini](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) veya [Azure CLI 2.0](/cli/azure/install-azure-cli).
- Güvenli oluşturma [Windows Küme](service-fabric-tutorial-create-vnet-and-windows-cluster.md) veya [Linux kümesi](service-fabric-tutorial-create-vnet-and-linux-cluster.md) Azure ile ilgili
- Windows Küme dağıtırsanız, bir Windows geliştirme ortamını ayarlama. Yükleme [Visual Studio 2017](http://www.visualstudio.com) ve **Azure geliştirme**, **ASP.NET ve web geliştirme**, ve **.NET Core platformlar arası geliştirme**iş yükleri.  Daha sonra ayarlanmış bir [.NET geliştirme ortamı](service-fabric-get-started.md).
- Linux kümesi dağıtırsanız, üzerinde bir Java geliştirme ortamını ayarlama [Linux](service-fabric-get-started-linux.md) veya [MacOS](service-fabric-get-started-mac.md).  Yükleme [doku CLI hizmet](service-fabric-cli.md). 

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
Azure komutları çalıştırmadan önce Azure hesabınızda oturum açın, aboneliğinizi seçin.

```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

```azurecli
az login
az account set --subscription <guid>
```

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

Öğreticinin bu bölümü başarıyla tamamlamak için Service Fabric kümesi ve (küme barındıran) sanal makine ölçek kümesi için bağlanmanız gerekir. Sanal makine ölçek kümesi Azure Service Fabric barındıran Azure kaynak değil.

Bir kümeye bağladığınızda, bilgi için sorgulayabilirsiniz. Hangi düğümleri hakkında küme bilmektedir öğrenmek için küme kullanabilirsiniz. Aşağıdaki kodda kümeye bağlanma, bu makalenin ilk bölümünde oluşturulan aynı sertifikayı kullanır. Belirlediğinizden emin olun `$endpoint` ve `$thumbprint` değerlerinizi değişkenleri.

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

Bağlı değilseniz, kümedeki her düğümün durumunu almak için bir komutu kullanabilirsiniz. İçin PowerShell kullanın `Get-ServiceFabricClusterHealth` komutu ve **sfctl** kullanmak '' komutu.

## <a name="scale-out"></a>Ölçeği genişletme

Ölçeği genişletme ölçek kümesine daha fazla sanal makine örnekleri ekleyin. Bu örnekler Service Fabric kullanan düğümleri haline gelir. Service Fabric ölçek kümesini (ölçeğini tarafından) eklenen fazla örneğe sahip olduğunda bilir ve otomatik olarak tepki verir. Aşağıdaki kod bir ölçeği artırır ve ad ile Ayarla alır **kapasite** ölçeğin 1 ayarlayın.

```powershell
$scaleset = Get-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity += 1

Update-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm -VirtualMachineScaleSet $scaleset
```

Bu kod, kapasitesi 6 ayarlar.

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 6
```

## <a name="scale-in"></a>İçinde ölçeklendirme

İçinde ölçeklendirme aynıdır ölçeğini, bir alt kullanması dışında **kapasite** değeri. Ölçek kümesindeki ölçeklendirdiğinizde ölçek kümesinde sanal makine örneklerini kaldırın. Normalde, ne olduğunu Service Fabric farkında değildir ve bir düğüm kaybolduğunda düşündüğü. Service Fabric kümesi için kötü bir durum ardından raporlar. Hatalı bu durumu önlemek için kayboluyor düğüme beklediğiniz service fabric bildirin gerekir.

### <a name="remove-the-service-fabric-node"></a>Service fabric düğümü Kaldır

> [!NOTE]
> Bu bölümü yalnızca uygular *Bronz* dayanıklılık katmanı. Dayanıklılık hakkında daha fazla bilgi için bkz: [Service Fabric kümesi kapasite planlaması][durability].

Bir sanal makine ölçek kümesindeki ölçeklendirdiğinizde ölçeği (çoğu durumda) ayarla en son oluşturulan sanal makine örneğini kaldırır. Bu nedenle, oluşturulan en son service fabric düğümü eşleşen bulmanız gerekir. Bu son düğüm biggest denetleyerek bulabileceğiniz `NodeInstanceId` service fabric düğümlerinde özellik değeri. Bir düğüm tarafından sıralama aşağıda kod örnekleri örneği ve en büyük kimliği değerine sahip örnek ayrıntılarını döndürür. 

```powershell
Get-ServiceFabricNode | Sort-Object NodeInstanceId -Descending | Select-Object -First 1
```

```azurecli
`sfctl node list --query "sort_by(items[*], &instanceId)[-1]"`
```

Service fabric kümesi, bu düğüm kaldırılacak gittiği bilmek ister. Atmanız gereken üç adım vardır:

1. Böylece artık replicate verileri için olan düğüm devre dışı bırakın.  
PowerShell:`Disable-ServiceFabricNode`  
sfcli:`sfctl node disable`

2. Düğüm, böylece service fabric çalışma zamanı düzgün bir şekilde kapanır ve uygulamanızı bir sonlandırma isteği alır durdurun.  
PowerShell:`Start-ServiceFabricNodeTransition -Stop`  
sfcli:`sfctl node transition --node-transition-type Stop`

2. Düğümü kümeden kaldırın.  
PowerShell:`Remove-ServiceFabricNodeState`  
sfcli:`sfctl node remove-state`

Bu üç adımı düğüme uygulandıktan sonra ölçek kümesinden kaldırılabilir. Herhangi bir dayanıklılık katmanı yanı sıra kullanıyorsanız [Bronz][durability], Ölçek kümesi örneği olduğunda kaldırılır için şu adımları yapılır.

Aşağıdaki kod bloğu son oluşturulan düğümünü alır, devre dışı bırakır, durdurur ve düğümü kümeden kaldırır.

```powershell
# Get the node that was created last
$node = Get-ServiceFabricNode | Sort-Object NodeInstanceId -Descending | Select-Object -First 1

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

İçinde **sfctl** aşağıdaki kod, aşağıdaki komut almak için kullanılan **düğüm adı** ve **düğümü örnek kimliği** son oluşturulan düğümünün değerler:`sfctl node list --query "sort_by(items[*], &instanceId)[-1].[instanceId,name]"`

```azurecli
# Inform the node that it is going to be removed
sfctl node disable --node-name _nt1vm_5 --deactivation-intent 4 -t 300

# Stop the node using a random guid as our operation id
sfctl node transition --node-instance-id 131541348482680775 --node-name _nt1vm_5 --node-transition-type Stop --operation-id c17bb4c5-9f6c-4eef-950f-3d03e1fef6fc --stop-duration-in-seconds 14400 -t 300

# Remove the node from the cluster
sfctl node remove-state --node-name _nt1vm_5
```

> [!TIP]
> Aşağıdaki **sfctl** her adımının durumunu denetlemek için sorgular
>
> **Devre dışı bırakma durumunu denetleme**  
> `sfctl node list --query "sort_by(items[*], &instanceId)[-1].nodeDeactivationInfo"`
>
> **Stop durumunu denetleme**  
> `sfctl node list --query "sort_by(items[*], &instanceId)[-1].isStopped"`
>


### <a name="scale-in-the-scale-set"></a>Ölçek kümesindeki ölçeklendirme

Service fabric düğümü kümeden kaldırılmış, sanal makine ölçek kümesi içinde genişletilebilir. Aşağıdaki örnekte, 1 ile ölçek kümesi kapasitesi azalır.

```powershell
$scaleset = Get-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity -= 1

Update-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm -VirtualMachineScaleSet $scaleset
```

Bu kod, kapasitesi 5'e ayarlar.

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 5
```


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Küme düğüm sayısı okuma
> * Küme düğümleri (ölçeklendirme) Ekle
> * Küme düğümleri (Ölçek) Kaldır


Ardından, bir kümenin çalışma zamanı yükseltme hakkında bilgi edinmek için aşağıdaki öğretici ilerleyin.
> [!div class="nextstepaction"]
> [Yükseltme küme çalışma zamanı](service-fabric-tutorial-upgrade-cluster.md)

[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
