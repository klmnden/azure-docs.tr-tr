---
title: Azure Service Fabric programlama ölçeklendirme | Microsoft Docs
description: Bir Azure Service Fabric kümesini içe veya dışa ölçeklendirme programlı olarak göre özel Tetikleyiciler
services: service-fabric
documentationcenter: .net
author: mjrousos
manager: jonjung
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: mikerou
ms.openlocfilehash: 552c9820cca4380c00e1bf435fdb3d068c0690fb
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62111317"
---
# <a name="scale-a-service-fabric-cluster-programmatically"></a>Service Fabric kümesi programlama yoluyla ölçeklendirme 

Azure'da çalışan Service Fabric kümeleri, sanal makine ölçek kümeleri üzerinde oluşturulur.  [Küme ölçeklendirme](./service-fabric-cluster-scale-up-down.md) el ile veya otomatik ölçeklendirme kurallarını ile Service Fabric kümeleri nasıl ölçeklendirilebilir açıklar. Bu makalede, kimlik bilgilerini yöneten ve bir kümedeki ölçeklendirme açıklar veya fluent Azure kullanarak işlem SDK'sı, ama daha gelişmiş bir senaryodur. Genel bir bakış için okuma [ölçeklendirme işlemlerinin Azure koordine programlı yöntemleri](service-fabric-cluster-scaling.md#programmatic-scaling). 


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="manage-credentials"></a>Kimlik bilgilerini yönetme
Ölçeklendirme işlemek için bir hizmet yazmanın bir hizmet sanal makine ölçek kümesi kaynaklarının etkileşimli oturum açma olmadan erişebilir zorluktur. Ölçeklendirme hizmet kendi Service Fabric uygulaması olarak değiştirip ancak ölçek kümesine erişmek için gerekli kimlik bilgilerini Service Fabric kümesine erişmek kolaydır. Oturum açmak için kullanabileceğiniz bir [hizmet sorumlusu](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli) ile oluşturulan [Azure CLI](https://github.com/azure/azure-cli).

Bir hizmet sorumlusu, aşağıdaki adımlarla oluşturulabilir:

1. Azure CLI için oturum açma (`az login`) sanal makine ölçek erişimi olan bir kullanıcı olarak ayarla
2. Hizmet sorumlusu oluşturma `az ad sp create-for-rbac`
    1. Uygulama Kimliği ('istemci kimliği' başka bir yerde olarak adlandırılır), adı, şifresi ve Kiracı daha sonra kullanmak için not edin.
    2. Ayrıca ile görüntülenebilir abonelik Kimliğinizi gerekir `az account list`

Fluent işlem kitaplığı gibi bu kimlik bilgilerini kullanarak oturum açabilir (çekirdek fluent Azure türleri gibi sağladığı Not `IAzure` bulunan [Microsoft.Azure.Management.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Fluent/) paket):

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
Fluent Azure kullanarak işlem SDK'sı, yalnızca birkaç çağrıları ile sanal makine ölçek örneklerinin eklenebilen-

```csharp
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
``` 

Alternatif olarak, sanal makine ölçek kümesi boyutu PowerShell cmdlet'leri ile yönetilebilir. [`Get-AzVmss`](https://docs.microsoft.com/powershell/module/az.compute/get-azvmss) sanal makine ölçek kümesi nesnesi alabilirsiniz. Geçerli kapasite aracılığıyla kullanılabilir `.sku.capacity` özelliği. İstenen değere kapasite değiştirdikten sonra Azure'da sanal makine ölçek ile güncelleştirilebilir [ `Update-AzVmss` ](https://docs.microsoft.com/powershell/module/az.compute/update-azvmss) komutu.

Bir düğüm el ile eklerken, bir ölçek kümesi örneği ekleme olması gerektiği gibi ölçek kümesi şablonu bu yana yeni bir Service Fabric düğüm başlatmak için gerekli olan tüm yeni örnekleri otomatik olarak Service Fabric kümesine katılmak uzantılar içerir. 

## <a name="scaling-in"></a>Ölçek artırma

Ölçek artırma, Ölçek genişletme için benzerdir. Gerçek sanal makine ölçek neredeyse aynı olduğundan değişiklikler ayarlayın. Ancak, daha önce açıklandığı gibi Service Fabric kaldırılan düğümlerini altın veya Gümüş bir dayanıklılık yalnızca otomatik olarak temizlenir. Bu nedenle, Bronz dayanıklılık ölçeğini durumda kaldırılacak düğüm kapalı kapatmak için Service Fabric kümesi ile etkileşim kurmak gereklidir ve sonra durumunu kaldırmak için.

Kaldırıldı (en son eklenen sanal makine ölçek kümesi örneği) düğümü bulma kapatma içerir düğümü hazırlama ve bunu devre dışı bırakma. Yeni düğümler (hangi eşleşme örneği adları temel alınan sanal makine ölçek kümesi) düğümlerinin adlarını sayı soneki karşılaştırarak bulunabilir. Bu nedenle, sanal makine ölçek kümesi örneklerine eklenir, sırayla numaralandırılır. 

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

Kaldırılacak düğüm bulunduğunda, devre dışı bırakılabilir ve aynı kaldırılan `FabricClient` örneği ve `IAzure` önceki örneği.

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

Bir komut dosyası yaklaşımı tercih ise olarak, sanal makine ölçek değiştirmek için PowerShell cmdlet'lerini genişletme ile kümesi kapasitesini de burada kullanılabilir. Service Fabric düğüm durumu, sanal makine örneği kaldırıldıktan sonra kaldırılabilir.

```csharp
await client.ClusterManager.RemoveNodeStateAsync(mostRecentLiveNode.NodeName);
```

## <a name="next-steps"></a>Sonraki adımlar

Otomatik ölçeklendirme kendi mantıksal uygulamaya başlamak için aşağıdaki kavramları ve kullanışlı API'leri ile bilgilenmeli:

- [El ile veya otomatik ölçeklendirme kurallarını ile ölçeklendirme](./service-fabric-cluster-scale-up-down.md)
- [.NET için Fluent Azure yönetim kitaplıkları](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (bir Service Fabric küme temel alınan sanal makine ölçek kümeleri ile etkileşim kurmak için yararlıdır)
- [System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (bir Service Fabric kümesini ve düğümlerini ile etkileşim kurmak için yararlıdır)
