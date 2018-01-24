---
title: "Azure doku programlı ölçeklendirme hizmet | Microsoft Docs"
description: "Bir Azure Service Fabric kümesi veya ölçeklendirmek programlı olarak özel Tetikleyicileri göre"
services: service-fabric
documentationcenter: .net
author: mjrousos
manager: jonjung
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/17/2017
ms.author: mikerou
ms.openlocfilehash: 1744e3c49ac06abe9e1067d507fd56d694201ffc
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="scale-a-service-fabric-cluster-programmatically"></a>Service Fabric kümesi programlı olarak ölçeklendirin 

Azure Service Fabric kümesi ölçeklendirme temellerini ele alınmıştır belgelerinde üzerinde [küme ölçeklendirme](./service-fabric-cluster-scale-up-down.md). Bu makale nasıl Service Fabric kümeleri üzerinde sanal makine ölçek kümeleri oluşturulur ve el ile veya Otomatik ölçek kurallarla Genişletilebilir kapsar. Bu belgede daha Gelişmiş senaryolar için denetleyici Azure ölçeklendirme işlemlerinin programlama yöntemleri bakar. 

## <a name="reasons-for-programmatic-scaling"></a>Programsal ölçeklendirme nedenleri
Birçok senaryoda el ile veya Otomatik ölçek kuralı aracılığıyla ölçeklendirme iyi çözümdür. Diğer senaryolarda, ancak bunlar sağ uygun olmayabilir. Bu yaklaşım olası dezavantajları şunlardır:

- El ile ölçeklendirme oturum açın ve açıkça ölçeklendirme işlemleri istek gerektirir. Ölçeklendirme işlemlerinin sık veya beklenmeyen zamanlarda gerekirse, bu yaklaşım iyi bir çözüm olmayabilir.
- Otomatik ölçek kuralı sanal makine ölçek kümesindeki bir örneği kaldırdığınızda, düğüm türü Gümüş veya altın dayanıklılık düzeyini olmadıkça bunlar otomatik olarak bilgi o düğümün ilişkili Service Fabric kümesinden kaldırmayın. Otomatik ölçek kuralı düzeyi Ayarla ölçekte (yerine Service Fabric düzeyinde) çalışması nedeniyle otomatik ölçek kuralı düzgün biçimde kapatılıyor olmadan Service Fabric düğümleri kaldırabilirsiniz. Bu utangaç düğüm kaldırma 'hayalet' Service Fabric düğüm durumu ölçek bileşenini işlemlerinden sonra ardınızda. Tek bir (veya hizmet), Service Fabric kümesindeki kaldırılmış düğüm durumu düzenli aralıklarla temizlemeniz gerekir.
  - Hiçbir ek temizleme gerektiği şekilde altın veya gümüş dayanıklılık düzeyine sahip bir düğüm türü kaldırılan düğümlerini, otomatik olarak temizlenir.
- Vardır, ancak [birçok ölçümleri](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md) otomatik ölçek kuralları tarafından desteklenen, hala olduğu sınırlı bir dizi. Senaryonuz bu kümesinde kapsanmayan bazı ölçüm göre ölçekleme için çağırırsa, otomatik ölçeklendirme kurallarını iyi bir seçenek olmayabilir.

Bu sınırlamalar bağlı olarak, daha özelleştirilmiş otomatik ölçekleme modelleri uygulamak isteyebilirsiniz. 

## <a name="scaling-apis"></a>Ölçeklendirme API'leri
Azure API hangi uygulamaların program aracılığıyla sanal makine ile çalışması için ayarlar ölçeklendirme ve Service Fabric kümeleri izin yok. Varolan otomatik ölçeklendirme seçeneği senaryonuz için işe yaramazsa, bu API'leri, özel ölçeklendirme mantığını uygulamak mümkün kılar. 

Yeni bir durum bilgisiz hizmeti ölçeklendirme işlemlerini yönetmek için Service Fabric uygulaması eklemek için otomatik ölçeklendirme 'giriş yapılan' işlevselliği'Bu uygulama için bir yaklaşım ise. Hizmetin içinde `RunAsync` yöntemi, bir dizi Tetikleyicileri belirleyebilir ölçeklendirme (küme boyutu üst sınırı gibi parametreleri denetleniyor ve cooldowns ölçeklendirme dahil) gerekli olup olmadığını.   

Sanal makine ölçek kümesi için etkileşimler (her ikisi de geçerli sanal makine örneği sayısını denetlemek ve değiştirmek için) kullanılan API [fluent yönetim Azure işlem kitaplığının](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/). Fluent işlem kitaplığı, sanal makine ölçek kümeleri ile etkileşmek için kullanımı kolay API sağlar.

Service Fabric kümesi etkileşim kurmak için kullanmak [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).

Elbette, ölçekleme kod ölçeklendirilmesi için kümedeki bir hizmet olarak çalıştırmak gerekmez. Her ikisi de `IAzure` ve `FabricClient` ölçeklendirme hizmeti bir konsol uygulaması veya Windows hizmeti Service Fabric uygulaması dışında çalıştırma kolayca olabilir şekilde ilişkili Azure kaynaklarını uzaktan bağlanabilirsiniz. 

## <a name="credential-management"></a>Kimlik bilgisi yönetimi
Ölçeklendirme işlemek için bir hizmet yazma bir hizmetin etkileşimli oturum açma olmadan sanal makine ölçek kümesi kaynaklara erişebilmeniz iştir. Service Fabric kümesi erişme ölçeklendirme hizmeti kendi başına Service Fabric uygulaması değiştirme, ancak ölçek kümesine erişmek için kimlik bilgileri gerekli kolaydır. Oturum açmak için kullanabileceğiniz bir [hizmet sorumlusu](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli) ile oluşturulan [Azure CLI 2.0](https://github.com/azure/azure-cli).

Bir hizmet sorumlusu aşağıdaki adımlarla oluşturulabilir:

1. Azure CLI için oturum açma (`az login`) sanal makine ölçek erişimi olan bir kullanıcı olarak ayarlayın
2. Hizmet sorumlusu ile oluşturma`az ad sp create-for-rbac`
    1. AppID ('istemci kimliği' başka bir yerde olarak adlandırılır), adı, şifresi ve Kiracı daha sonra kullanmak için not edin.
    2. İle görüntülenebilir abonelik Kimliğinizi de gerekir`az account list`

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

Alternatif olarak, sanal makine ölçek kümesi boyutu PowerShell cmdlet'leri ile yönetilebilir. [`Get-AzureRmVmss`](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss)sanal makine ölçek kümesi nesnesi alabilir. Geçerli kapasitenin aracılığıyla kullanılabilir `.sku.capacity` özelliği. İstenen değere kapasite değiştirdikten sonra sanal makineyi ölçeği Azure'da Ayarla ile güncelleştirilebilir [ `Update-AzureRmVmss` ](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss) komutu.

Bir düğüm el ile eklerken, örnek bir ölçek kümesi ekleme olması gerektiği gibi şablon ölçek kümesi bu yana yeni bir Service Fabric düğümü başlatmak için gerekli olan tüm otomatik olarak yeni örnekleri için Service Fabric kümesi katılmak için uzantılar içerir. 

## <a name="scaling-in"></a>İçinde ölçeklendirme

İçinde ölçeklendirme ölçeğini için benzer. Gerçek sanal makine ölçek değişiklikleri hemen hemen aynı ayarlanır. Ancak, daha önce açıklandığı gibi Service Fabric kaldırılan düğümlerini altın veya Gümüş bir dayanıklılık yalnızca otomatik olarak temizlenir. Bu nedenle Bronz dayanıklılık ölçek bileşenini durumunda kaldırılacak düğümü kapatılacağını Service Fabric kümesi ile etkileşim kurmak ise gerekli ardından durumuna kaldırmak için.

Düğümü için kapatma hazırlanıyor (en son eklenen düğüm) düğümü kaldırılan bulunmasını ve devre dışı bırakmadan içerir. Çekirdek olmayan düğümleri için yeni düğümler karşılaştırarak bulunabilir `NodeInstanceId`. 

```csharp
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n => n.NodeInstanceId)
        .FirstOrDefault();
```

Çekirdek düğüm farklı ve büyük örneği kimlikleri ilk kaldırılır kuralı mutlaka izlemeyin.

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

## <a name="potential-drawbacks"></a>Olası dezavantajları

Önceki kod parçacıkları gösterildiği gibi kendi hizmet ölçeklendirme oluşturmak en yüksek derecede denetim ve uygulamanızı üzerinden özelleştirilebilirliğini davranışı ölçeklendirme kullanıcının sağlar. Bu, ne zaman veya nasıl bir uygulama veya ölçeklendirir üzerinde kesin denetim gerektiren senaryolar için faydalı olabilir. Ancak, bu denetim bir kod karmaşıklığı kolaylığını ile birlikte gelir. Bu yaklaşımı kullanarak, Önemsiz olmayan olan kendi ölçeklendirme kod, gerektiği anlamına gelir.

Service Fabric ölçeklendirme nasıl ulaşmalıdır, senaryoya bağlıdır. Ölçeklendirme seyrek ise, ekleme veya düğümleri el ile kaldırma yeteneği büyük olasılıkla yeterli olur. Daha karmaşık senaryolar için otomatik ölçeklendirme kurallarını ve SDK'ları ölçeklendirmenizi program aracılığıyla gösterme güçlü alternatifleri sunar.

## <a name="next-steps"></a>Sonraki adımlar

Kendi otomatik ölçeklendirme mantığı uygulamak başlamak için aşağıdaki kavramlar ve yararlı API'leri ile edinin:

- [El ile veya Otomatik ölçek kurallarla ölçeklendirme](./service-fabric-cluster-scale-up-down.md)
- [.NET için Azure yönetim kitaplıklarını Fluent](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (bir Service Fabric kümenin temel sanal makine ölçek kümeleri ile etkileşim için yararlıdır)
- [System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (Service Fabric kümesini ve düğümlerini ile etkileşim için yararlıdır)
