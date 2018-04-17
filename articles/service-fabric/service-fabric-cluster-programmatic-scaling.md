---
title: Azure doku programlı ölçeklendirme hizmet | Microsoft Docs
description: Bir Azure Service Fabric kümesi veya ölçeklendirmek programlı olarak özel Tetikleyicileri göre
services: service-fabric
documentationcenter: .net
author: mjrousos
manager: jonjung
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: mikerou
ms.openlocfilehash: b875351ef80050687fcf85e35da132cf37bab83b
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="scale-a-service-fabric-cluster-programmatically"></a>Service Fabric kümesi programlı olarak ölçeklendirin 

Service Fabric kümeleri Azure'da çalışan sanal makine ölçek kümeleri üzerinde oluşturulmuştur.  [Küme ölçeklendirme](./service-fabric-cluster-scale-up-down.md) Service Fabric kümeleri el ile veya Otomatik ölçek kurallarla nasıl Genişletilebilir açıklar. Bu makalede daha gelişmiş bir senaryodur SDK fluent Azure kullanarak işlem veya kimlik bilgilerini yöneten ve bir kümede ölçeklendirmek nasıl açıklanır. Genel bir bakış için okuma [operations ölçeklendirme Azure Eşgüdümleme programlı yöntemleri](service-fabric-cluster-scaling.md#programmatic-scaling). 

## <a name="manage-credentials"></a>Kimlik bilgilerini yönetme
Ölçeklendirme işlemek için bir hizmet yazma bir hizmetin etkileşimli oturum açma olmadan sanal makine ölçek kümesi kaynaklara erişebilmeniz iştir. Service Fabric kümesi erişme ölçeklendirme hizmeti kendi başına Service Fabric uygulaması değiştirme, ancak ölçek kümesine erişmek için kimlik bilgileri gerekli kolaydır. Oturum açmak için kullanabileceğiniz bir [hizmet sorumlusu](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli) ile oluşturulan [Azure CLI 2.0](https://github.com/azure/azure-cli).

Bir hizmet sorumlusu aşağıdaki adımlarla oluşturulabilir:

1. Azure CLI için oturum açma (`az login`) sanal makine ölçek erişimi olan bir kullanıcı olarak ayarlayın
2. Hizmet sorumlusu ile oluşturma `az ad sp create-for-rbac`
    1. AppID ('istemci kimliği' başka bir yerde olarak adlandırılır), adı, şifresi ve Kiracı daha sonra kullanmak için not edin.
    2. İle görüntülenebilir abonelik Kimliğinizi de gerekir `az account list`

Fluent işlem kitaplığı gibi bu kimlik bilgilerini kullanarak oturum açabilir (çekirdek fluent Azure türleri ister Not `IAzure` bulunan [Microsoft.Azure.Management.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Fluent/) paket):

```csharp
var credentials = new AzureCredentials(new ServicePrincipalLoginInformation {
                ClientId = AzureClientId,
                ClientSecret = 
                AzureClientKey }, AzureTenantId, AzureEnvironment.AzureGlobalCloud);
IAzure AzureClient = Azure.Authenticate(credentials).WithSubscription(AzureSubscriptionId);

if (AzureClient?.SubscriptionId == AzureSubscriptionId)
{
    ServiceEventSource.Current.ServiceMessage(Context, "Successfully logged into Azure");
}
else
{
    ServiceEventSource.Current.ServiceMessage(Context, "ERROR: Failed to login to Azure");
}
```

Aracılığıyla oturum açtıktan sonra ölçek kümesi örnek sayısı sorgulanabilir `AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId).Capacity`.

## <a name="scaling-out"></a>Ölçeği genişletme
İşlem SDK fluent Azure kullanarak, yalnızca birkaç aramaları ayarlamak sanal makine ölçek örnekleri eklenebilir-

```csharp
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
``` 

Alternatif olarak, sanal makine ölçek kümesi boyutu PowerShell cmdlet'leri ile yönetilebilir. [`Get-AzureRmVmss`](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss) sanal makine ölçek kümesi nesnesi alabilir. Geçerli kapasitenin aracılığıyla kullanılabilir `.sku.capacity` özelliği. İstenen değere kapasite değiştirdikten sonra sanal makineyi ölçeği Azure'da Ayarla ile güncelleştirilebilir [ `Update-AzureRmVmss` ](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss) komutu.

Bir düğüm el ile eklerken, örnek bir ölçek kümesi ekleme olması gerektiği gibi şablon ölçek kümesi bu yana yeni bir Service Fabric düğümü başlatmak için gerekli olan tüm otomatik olarak yeni örnekleri için Service Fabric kümesi katılmak için uzantılar içerir. 

## <a name="scaling-in"></a>İçinde ölçeklendirme

İçinde ölçeklendirme ölçeğini için benzer. Gerçek sanal makine ölçek değişiklikleri hemen hemen aynı ayarlanır. Ancak, daha önce açıklandığı gibi Service Fabric kaldırılan düğümlerini altın veya Gümüş bir dayanıklılık yalnızca otomatik olarak temizlenir. Bu nedenle Bronz dayanıklılık ölçek bileşenini durumunda kaldırılacak düğümü kapatılacağını Service Fabric kümesi ile etkileşim kurmak ise gerekli ardından durumuna kaldırmak için.

Kaldırıldı (en son eklenen sanal makine ölçek kümesi örneği) düğümü bulma kapatma içerir düğüm hazırlama ve onu devre dışı bırakma. Yeni düğümler (hangi eşleşme örnek adları temel sanal makine ölçek kümesi) düğümleri adları sayı soneki karşılaştırarak bulunabilir böylece sanal makine ölçek kümesi örneklerinin eklendikleri, sırayla numaralandırılır. 

```csharp
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n =>
        {
            var instanceIdIndex = n.NodeName.LastIndexOf("_");
            var instanceIdString = n.NodeName.Substring(instanceIdIndex + 1);
            return int.Parse(instanceIdString);
        })
        .FirstOrDefault();
```

Kaldırılacak düğüm bulunduktan sonra devre dışı bırakılabilir ve aynı kullanarak kaldırılan `FabricClient` örneği ve `IAzure` örneğinden daha önce.

```csharp
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove the node from the Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up to a timeout) for the node to gracefully shutdown
var timeout = TimeSpan.FromMinutes(5);
var waitStart = DateTime.Now;
while ((mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Up || mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Disabling) &&
        DateTime.Now - waitStart < timeout)
{
    mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync()).FirstOrDefault(n => n.NodeName == mostRecentLiveNode.NodeName);
    await Task.Delay(10 * 1000);
}

// Decrement VMSS capacity
var newCapacity = (int)Math.Max(MinimumNodeCount, scaleSet.Capacity - 1); // Check min count 

scaleSet.Update().WithCapacity(newCapacity).Apply(); 
```

Komut dosyası bir yaklaşım tercih ise olarak, sanal makine ölçek değiştirmek için PowerShell cmdlet'leri ölçeğini genişletme ile kümesi kapasitesi de kullanılabilir. Sanal makine örneğini kaldırıldığında Service Fabric düğüm durumu kaldırılabilir.

```csharp
await client.ClusterManager.RemoveNodeStateAsync(mostRecentLiveNode.NodeName);
```

## <a name="next-steps"></a>Sonraki adımlar

Kendi otomatik ölçeklendirme mantığı uygulamak başlamak için aşağıdaki kavramlar ve yararlı API'leri ile edinin:

- [El ile veya Otomatik ölçek kurallarla ölçeklendirme](./service-fabric-cluster-scale-up-down.md)
- [.NET için Azure yönetim kitaplıklarını Fluent](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (bir Service Fabric kümenin temel sanal makine ölçek kümeleri ile etkileşim için yararlıdır)
- [System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (Service Fabric kümesini ve düğümlerini ile etkileşim için yararlıdır)
