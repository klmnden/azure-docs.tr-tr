---
title: Azure Service Fabric durum bilgisi olan hizmetler için birim testleri geliştirirsiniz | Microsoft Docs
description: Service Fabric durum bilgisi olan hizmetler için birim testleri geliştirmeyi öğrenin.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: vturecek
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/04/2018
ms.author: atsenthi
ms.openlocfilehash: b066296ca52d3067f8985245161eb4fa7b484a07
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58669121"
---
# <a name="create-unit-tests-for-stateful-services"></a>Durum bilgisi olan hizmetler için birim testleri oluşturma
Birim testi Service Fabric durum bilgisi olan hizmetler mutlaka geleneksel uygulama veya etki alanına özgü birim testi tarafından yakalanabilen değil yaygın hataları açıklığa kavuşturur. Durum bilgisi olan hizmetler için birim testleri geliştirirken akılda tutulması gereken bazı noktalar vardır.

1. Her yineleme uygulama kodu yürütür, ancak farklı bir bağlam altında. Hizmet üç kopyaya kullanıyorsa, hizmet kodunu paralel farklı bağlam/rolü altında üç düğümlerinde yürütüyor.
2. Durum bilgisi olan hizmet içinde depolanan durumu arasında tüm çoğaltmaları tutarlı olmalıdır. Bu tutarlılık,-hazır durum Yöneticisi ve güvenilir koleksiyonlar sağlar. Ancak, uygulama kodu tarafından yönetilmek üzere bellek içi durumu gerekir.
3. Her çoğaltma sırasında küme üzerinde çalışan roller, belirli bir noktada değiştirir. Birincil barındıran düğümü, kullanılamıyor veya aşırı yüklenmiş olur olay, bir ikincil çoğaltma, birincil hale gelir. Service Fabric Hizmetleri altında farklı bir rol sonunda yürütmek için bu nedenle planlamanız gerekir için doğal bir davranış budur.

Bu makalede varsayar [birim Service Fabric durum bilgisi olan hizmetler testi](service-fabric-concepts-unit-testing.md) okunmuş.

## <a name="the-servicefabricmocks-library"></a>ServiceFabric.Mocks kitaplığı
3.3.0, Sürüm'den itibaren [ServiceFabric.Mocks](https://www.nuget.org/packages/ServiceFabric.Mocks/) orchestration çoğaltmaları durum yönetimi ve sahte işlem için bir API sağlar. Bu örneklerde kullanılır.

[Nuget](https://www.nuget.org/packages/ServiceFabric.Mocks/)
[GitHub](https://github.com/loekd/ServiceFabric.Mocks)

*ServiceFabric.Mocks değil ait veya Microsoft tarafından korunur. Ancak, şu anda Microsoft kitaplığı birim testi durum bilgisi olan hizmetler için önerilen budur.*

## <a name="set-up-the-mock-orchestration-and-state"></a>Sahte düzenleme ve durumunu ayarlama
Bir test Yerleştir kısmı bir parçası olarak sahte bir çoğaltma kümesi ve durum Yöneticisi oluşturulur. Çoğaltma kümesi, ardından her yineleme için test edilmiş hizmet örneği oluşturma hakimi olursunuz. Yürütülen yaşam döngüsü olayları gibi de ait `OnChangeRole` ve `RunAsync`. Sahte bir durum Yöneticisi durum Yöneticisi karşı gerçekleştirilen tüm işlemler çalıştırın ve gerçek durum Yöneticisi olduğu gibi kalmasını sağlayacaktır.

1. Test edilen hizmet örneği bir hizmet Fabrika temsilcisi oluşturun. Hizmet üreteci geri genellikle içinde bulunan bu benzer veya aynı olmalıdır `Program.cs` bir Service Fabric hizmet ya da aktör için. Bu, aşağıdaki imzası izlemelidir:
   ```csharp
   MyStatefulService CreateMyStatefulService(StatefulServiceContext context, IReliableStateManagerReplica2 stateManager)
   ```
2. Bir örneğini oluşturmak `MockReliableStateManager` sınıfı. Bu durum Yöneticisi ile tüm etkileşimler sahte.
3. Bir örneğini oluşturmak `MockStatefulServiceReplicaSet<TStatefulService>` burada `TStatefulService` test edilen hizmet türüdür. Bu temsilci #1. adımda oluşturduğunuz ve #2'de örneği durum Yöneticisi'ni gerektirir
4. Çoğaltmalar, çoğaltma kümesine ekleyin. (Örneğin, birincil, ActiveSecondary, IdleSecondary) rolü ve çoğaltma kimliği belirtin
   > Çoğaltma kimlikleri tutun! Bu, büyük olasılıkla işlemi sırasında kullanılacak ve birim testi bölümlerini onay.

```csharp
//service factory to instruct how to create the service instance
var serviceFactory = (StatefulServiceContext context, IReliableStateManagerReplica2 stateManager) => new MyStatefulService(context, stateManager);
//instantiate a new mock state manager
var stateManager = new MockReliableStateManager();
//instantiate a new replica set with the service factory and state manager
var replicaSet = new MockStatefulServiceReplicaSet<MyStatefulService>(CreateStatefulService, stateManager);
//add a new Primary replica with id 1
await replicaSet.AddReplicaAsync(ReplicaRole.Primary, 1);
//add a new ActiveSecondary replica with id 2
await replicaSet.AddReplicaAsync(ReplicaRole.ActiveSecondary, 2);
//add a second ActiveSecondary replica with id 3
await replicaSet.AddReplicaAsync(ReplicaRole.ActiveSecondary, 3);
```

## <a name="execute-service-requests"></a>Hizmet istekleri
Hizmet isteklerini aramalar ve kolaylık özellikleri kullanarak belirli bir çoğaltma üzerinde çalıştırılabilir.
```csharp
const string stateName = "test";
var payload = new Payload(StatePayload);

//execute a request on the primary replica using
await replicaSet.Primary.ServiceInstance.InsertAsync(stateName, payload);

//execute a request against replica with id 2
await replicaSet[2].ServiceInstance.InsertAsync(stateName, payload);

//execute a request against one of the active secondary replicas
await replicaSet.FirstActiveSecondary.InsertAsync(stateName, payload);
```

## <a name="execute-a-service-move"></a>Hizmet taşıma yürütün
Sahte çoğaltma kümesine hizmet taşır farklı türde tetiklemek için çeşitli kullanışlı yöntemler sunar.
```csharp
//promote the first active secondary to primary
replicaSet.PromoteNewReplicaToPrimaryAsync();
//promote the secondary with replica id 4 to primary
replicaSet.PromoteNewReplicaToPrimaryAsync(4);

//promote the first idle secondary to an active secondary
PromoteIdleSecondaryToActiveSecondaryAsync();
//promote idle secondary with replica id 4 to active secondary
PromoteIdleSecondaryToActiveSecondaryAsync(4);

//add a new replica with randomly assigned replica id and promote it to primary
PromoteNewReplicaToPrimaryAsync()
//add a new replica with replica id 4 and promote it to primary
PromoteNewReplicaToPrimaryAsync(4)
```

## <a name="putting-it-all-together"></a>Hepsini bir araya getirme
Şu test, bir üç düğümlü çoğaltma ayarlama ayarlama ve rol değişiklikten sonra verileri ikincil bölgeden kullanılabilir olduğu doğrulanıyor gösterir. Veri sırasında eklediyseniz bu catch tipik bir sorun olduğunu `InsertAsync` bir şey bellek veya bir güvenilir koleksiyon çalıştırılmadan kaydedilmiş `CommitAsync`. Her iki durumda da ikincil birincil ile eşitlenmemiş olabilir. Hizmet taşındıktan sonra bu tutarsız yanıtlarını sunulmasını sağlar.

```csharp
[TestMethod]
public async Task TestServiceState_InMemoryState_PromoteActiveSecondary()
{
    var stateManager = new MockReliableStateManager();
    var replicaSet = new MockStatefulServiceReplicaSet<MyStatefulService>(CreateStatefulService, stateManager);
    await replicaSet.AddReplicaAsync(ReplicaRole.Primary, 1);
    await replicaSet.AddReplicaAsync(ReplicaRole.ActiveSecondary, 2);
    await replicaSet.AddReplicaAsync(ReplicaRole.ActiveSecondary, 3);

    const string stateName = "test";
    var payload = new Payload(StatePayload);

    //insert data
    await replicaSet.Primary.ServiceInstance.InsertAsync(stateName, payload);
    //promote one of the secondaries to primary
    await replicaSet.PromoteActiveSecondaryToPrimaryAsync(2);
    //get data
    var payloads = (await replicaSet.Primary.ServiceInstance.GetPayloadsAsync()).ToList();

    //data should match what was inserted against the primary
    Assert.IsTrue(payloads.Count == 1);
    Assert.IsTrue(payloads[0].Content == payload.Content);

    //verify the data was saved against the reliable dictionary
    var dictionary = await StateManager.GetOrAddAsync<IReliableDictionary<string, Payload>>(MyStatefulService.StateManagerDictionaryKey);
    using(var tx = StateManager.CreateTransaction())
    {
        var payload = await dictionary.TryGetValue(stateName);
        Assert.IsTrue(payload.HasValue);
        Assert.IsTrue(payload.Value.Content == payload.Content);
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Nasıl test edeceğinizi öğrenmek [hizmetten hizmete iletişimi](service-fabric-testability-scenarios-service-communication.md) ve [denetimli chaos ile hataların benzetimini](service-fabric-controlled-chaos.md).
