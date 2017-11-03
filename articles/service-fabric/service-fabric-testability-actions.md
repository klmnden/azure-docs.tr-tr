---
title: "Azure mikro hatalarına benzetimini | Microsoft Docs"
description: "Bu makalede, Microsoft Azure Service Fabric içinde bulunan Test Edilebilirlik eylemler hakkında alınmaktadır."
services: service-fabric
documentationcenter: .net
author: motanv
manager: timlt
editor: toddabel
ms.assetid: ed53ca5c-4d5e-4b48-93c9-e386f32d8b7a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: motanv;heeldin
ms.openlocfilehash: c8ddc7732999ae555323bebaef60aa34c8f2ec17
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="testability-actions"></a>Test Edilebilirlik Eylemler
Güvenilir olmayan bir altyapı benzetimini yapmak için Azure Service Fabric çeşitli gerçek hataları ve durumu geçişleri gerçekleştirecek birçok yöntem ile geliştirici olarak size sağlar. Bu Test Edilebilirlik eylemler olarak sunulur. Belirli bir arıza ekleme, durum geçişi veya doğrulama neden alt düzey API'leri eylemlerdir. Bu eylemler ile birleştirerek kapsamlı test senaryoları için hizmetlerinizi yazabilirsiniz.

Service Fabric, bazı ortak test senaryoları bu eylemleri oluşan sağlar. Ortak durumu geçişleri ve hata durumları test etmek için dikkatle seçilen bu yerleşik senaryolar kullanan öneririz. Ancak, Eylemler, kapsam, yerleşik senaryolarla henüz kapsamında yer almayan veya uygulamanız için özel olarak hazırlanmış özel olan senaryolar için eklemek istediğiniz zaman özel test senaryoları oluşturmak için kullanılabilir.

C# uygulamalarının eylemlerin System.Fabric.dll bütünleştirilmiş kodunda bulunamadı. Sistem Fabric PowerShell modülü Microsoft.ServiceFabric.Powershell.dll bütünleştirilmiş kodunda bulunamadı. Çalışma zamanı yüklemesinin bir parçası olarak, kullanım kolaylığı için izin vermek için ServiceFabric PowerShell Modülü yüklü.

## <a name="graceful-vs-ungraceful-fault-actions"></a>Normal durunda hataya Eylemler karşılaştırması
Test Edilebilirlik Eylemler iki ana demet sınıflandırılır:

* Durunda hatası: Bu hataları hataları makine yeniden başlatmaları gibi benzetimini gerçekleştirmek ve işlem kilitleniyor. Hatalar bu gibi durumlarda, işlem yürütme bağlamı aniden durdurur. Bu uygulama yeniden başlatılmadan önce durumu temizleme çalıştırabileceğiniz anlamına gelir.
* Normal hatası: Bu hataları çoğaltma taşır gibi normal eylemlerin ve Yük Dengeleme tarafından tetiklenen düşme benzetimini yapma. Böyle durumlarda, hizmet kapatma, bir bildirim alır ve durumunu çıkmadan önce temizleyebilirsiniz.

Daha iyi kalite doğrulama için çeşitli normal ve durunda hataları inducing sırasında hizmet ve iş yükünü çalıştırın. Durunda hataları nerede hizmet işlemi bazı iş akışı ortasında aniden çıkar senaryoları uygulamaktadır. Hizmet çoğaltma Service Fabric tarafından geri yüklendikten sonra bu kurtarma yolu sınar. Bu, veri tutarlılığı ve hizmet durumu hataları sonra doğru olup olmadığını yönetilmesini test yardımcı olur. Diğer kümesi Service Fabric tarafından taşınan çoğaltmaları için hizmet doğru şekilde tepki verdiğini hataları (normal hata sayısı) test. Bu işleme iptal RunAsync yönteminde sınar. Olan ayarlayın, doğru durumunu kaydetmek için iptal belirteci denetleyin ve RunAsync yöntemi çıkmak hizmet gerekiyor.

## <a name="testability-actions-list"></a>Test Edilebilirlik eylemler listesi
| Eylem | Açıklama | Yönetilen API | PowerShell cmdlet'i | Normal/durunda hataları |
| --- | --- | --- | --- | --- |
| CleanTestState |Tüm test durumu kümeyi test sürücüsünün hatalı bir kapanma durumunda kaldırır. |CleanTestStateAsync |Remove-ServiceFabricTestState |Uygulanamaz |
| InvokeDataLoss |Veri kaybı hizmet bölüme uygulanmasını. |InvokeDataLossAsync |Invoke-ServiceFabricPartitionDataLoss |Normal |
| InvokeQuorumLoss |Belirtilen durum bilgisi olan hizmet bölüm çekirdek kayıp yerleştirir. |InvokeQuorumLossAsync |Çağırma ServiceFabricQuorumLoss |Normal |
| Birincil taşıma |Durum bilgisi olan hizmet belirtilen birincil çoğaltmasını belirtilen küme düğümü taşır. |MovePrimaryAsync |Taşıma ServiceFabricPrimaryReplica |Normal |
| İkincil taşıma |Bir durum bilgisi olan hizmetin geçerli ikincil çoğaltma için farklı küme düğümü taşır. |MoveSecondaryAsync |Taşıma ServiceFabricSecondaryReplica |Normal |
| RemoveReplica |Bir çoğaltma hatası bir kümeden bir çoğaltma kaldırarak benzetimini yapar. Bu çoğaltma kapatılacak ve rolüne geçirecektir 'None', durumunun tamamı kümeden kaldırma. |RemoveReplicaAsync |Remove-ServiceFabricReplica |Normal |
| RestartDeployedCodePackage |Kod paketi işlemi başarısız bir kümedeki bir düğümün dağıtılmış kod paketi yeniden başlatarak benzetimini yapar. Bu işlemde barındırılan tüm kullanıcı hizmet çoğaltmalar yeniden kod paket işlemi durdurur. |RestartDeployedCodePackageAsync |Yeniden başlatma ServiceFabricDeployedCodePackage |Durunda |
| RestartNode |Bir Service Fabric kümesi düğüm hatasından bir düğümü yeniden başlatarak benzetimini yapar. |RestartNodeAsync |Yeniden başlatma ServiceFabricNode |Durunda |
| RestartPartition |Bir veri merkezi Kararma veya küme Kararma senaryosu bir bölüm, bazı veya tüm çoğaltmaları yeniden başlatarak benzetimini yapar. |RestartPartitionAsync |Restart-ServiceFabricPartition |Normal |
| RestartReplica |Bir çoğaltma hatası kalıcı çoğaltma bir kümede yeniden başlatma, çoğaltma kapatma ve yeniden açmayı benzetimini yapar. |RestartReplicaAsync |Yeniden başlatma ServiceFabricReplica |Normal |
| BaşlangıçDüğümü |Bir düğüm zaten durdurulmuş bir kümede başlatır. |StartNodeAsync |Start-ServiceFabricNode |Uygulanamaz |
| StopNode |Bir düğüm hatasından bir küme düğümünde durdurarak benzetimini yapar. BaşlangıçDüğümü çağrılıncaya kadar düğümü kapalı kalır. |StopNodeAsync |Stop-ServiceFabricNode |Durunda |
| ValidateApplication |Kullanılabilirlik ve bazı hata sisteme inducing sonra genellikle bir uygulamadaki tüm Service Fabric Hizmetleri durumunu doğrular. |ValidateApplicationAsync |Test-ServiceFabricApplication |Uygulanamaz |
| ValidateService |Kullanılabilirlik ve Service Fabric hizmeti bazı hata sisteme genellikle inducing sonra doğrular. |ValidateServiceAsync |Test-ServiceFabricService |Uygulanamaz |

## <a name="running-a-testability-action-using-powershell"></a>PowerShell kullanarak bir Test Edilebilirlik eylem çalıştırma
Bu öğreticide PowerShell kullanarak bir Test Edilebilirlik eylemi çalıştırmak nasıl gösterir. Yerel (bir çalıştırma) küme veya bir Azure küme karşı Test Edilebilirlik eylemini çalıştırmak öğreneceksiniz. Microsoft.Fabric.Powershell.dll--Service Fabric PowerShell modülü--Microsoft Service Fabric MSI yüklediğinizde otomatik olarak yüklenir. Modül bir PowerShell istemi açtığınızda otomatik olarak yüklenir.

Eğitmen kesimleri:

* [Bir eylem karşı bir kutusunu küme çalıştırın](#run-an-action-against-a-one-box-cluster)
* [Bir eylem Azure bir küme karşı çalıştırma](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a>Bir eylem karşı bir kutusunu küme çalıştırın
Yerel küme karşı bir Test Edilebilirlik eylemi çalıştırmak için ilk kümeye bağlanın ve PowerShell komut istemini yönetici modunda açın. Bize bakmak **yeniden ServiceFabricNode** eylem.

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

Burada eylemi **yeniden ServiceFabricNode** "Düğüm1" adlı bir düğümde çalıştırın. Bu düğümü yeniden başlatma eylemi gerçekten başarılı olup olmadığını doğrulamanız değil tamamlama modunu belirtir. "Doğrula" yeniden başlatma eylemi gerçekten başarılı olup olmadığını doğrulamak neden olacak şekilde tamamlama modunu belirtme. Düğümün adını kullanarak doğrudan belirtmek yerine, bu bölüm anahtarı ve çoğaltma, tür gibi belirtebilirsiniz:

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

**Yeniden başlatma ServiceFabricNode** bir kümede bir Service Fabric düğümü yeniden başlatmak için kullanılmalıdır. Bu, tüm bu düğümde barındırılan sistem hizmeti ve kullanıcı hizmet çoğaltmalar yeniden Fabric.exe işlemi durdurur. Hizmetinizi test etmek için bu API'yi kullanarak yük devretme kurtarma yolları boyunca hatalar ortaya çıkarmaya yardımcı olur. Kümedeki düğüm hatalarını benzetimini yardımcı olur.

Aşağıdaki ekran görüntüsü gösterildiği **yeniden ServiceFabricNode** eylem Test Edilebilirlik komutu.

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

İlk çıkış **Get-ServiceFabricNode** (Service Fabric PowerShell modülden bir cmdlet) yerel küme beş düğümü olan gösterir: Node.5 Node.1. Test Edilebilirlik eylem (cmdlet'ini) sonra **yeniden ServiceFabricNode** düğümde yürütülen Node.4 adlı, biz düğümün çalışır durumda kalma süresi sıfırlama bakın.

### <a name="run-an-action-against-an-azure-cluster"></a>Bir eylem Azure bir küme karşı çalıştırma
Bir Test Edilebilirlik eylemi çalıştıran bir Azure küme karşı (PowerShell kullanarak), yerel bir küme karşı eylemi çalıştırmak için benzer. Tek fark yerel kümeye bağlanma yerine eylem çalıştırmadan önce ilk Azure kümeye bağlanmak yeterli olmasıdır.

## <a name="running-a-testability-action-using-c35"></a>C &#35;kullanarak bir Test Edilebilirlik eylemi çalıştıran;
C# kullanarak bir Test Edilebilirlik eylemi çalıştırmak için öncelikle FabricClient kullanarak kümeye bağlanmak gerekir. Ardından eylemi çalıştırmak için gerekli parametreleri edinin. Farklı Parametreler aynı eylemi çalıştırmak için kullanılabilir.
RestartServiceFabricNode eylem baktığınızda, çalıştırmak için bir kümede bulunan düğüm bilgileri (düğüm adı ve düğüm örnek kimliği) kullanarak yoludur.

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

Parametre açıklaması:

* **CompleteMode** modu yeniden başlatma eylemi gerçekten başarılı olup olmadığını doğrulamalıdır değil olduğunu belirtir. "Doğrula" yeniden başlatma eylemi gerçekten başarılı olup olmadığını doğrulamak neden olacak şekilde tamamlama modunu belirtme.  
* **OperationTimeout** işlemi TimeoutException özel durum önce tamamlanması için geçen süreyi ayarlar.
* **CancellationToken** iptal edilmesi bekleyen bir çağrı sağlar.

Düğümün adını kullanarak doğrudan belirtmek yerine, onu bir bölüm anahtarı ve çoğaltma türünü aracılığıyla belirtebilirsiniz.

Daha fazla bilgi için bkz: [PartitionSelector ve ReplicaSelector](#partition_replica_selector).

```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Fabric.Testability;
using System.Fabric;
using System.Threading;
using System.Numerics;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");
        string nodeName = "N0040";
        BigInteger nodeInstanceId = 130743013389060139;

        Console.WriteLine("Starting RestartNode test");
        try
        {
            //Restart the node by using ReplicaSelector
            RestartNodeAsync(clusterConnection, serviceName).Wait();

            //Another way to restart node is by using nodeName and nodeInstanceId
            RestartNodeAsync(clusterConnection, nodeName, nodeInstanceId).Wait();
        }
        catch (AggregateException exAgg)
        {
            Console.WriteLine("RestartNode did not complete: ");
            foreach (Exception ex in exAgg.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("RestartNode completed.");
        return 0;
    }

    static async Task RestartNodeAsync(string clusterConnection, Uri serviceName)
    {
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);
        ReplicaSelector primaryofReplicaSelector = ReplicaSelector.PrimaryOf(randomPartitionSelector);

        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(primaryofReplicaSelector, CompletionMode.Verify);
    }

    static async Task RestartNodeAsync(string clusterConnection, string nodeName, BigInteger nodeInstanceId)
    {
        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(nodeName, nodeInstanceId, CompletionMode.Verify);
    }
}
```

## <a name="partitionselector-and-replicaselector"></a>PartitionSelector ve ReplicaSelector
### <a name="partitionselector"></a>PartitionSelector
PartitionSelector Test Edilebilirlik kullanıma sunulan bir yardımcı olan ve belirli bir bölüm Test Edilebilirlik eylemlerden herhangi birini gerçekleştirileceği seçmek için kullanılır. Bölüm kimliği önceden biliniyorsa belirli bir bölüm seçmek için kullanılabilir. Ya da bölüm anahtarı sağlayın ve işlemi bölüm kimliği dahili olarak çözer. Ayrıca, rastgele bir bölüm seçme seçeneğiniz de vardır.

Bu yardımcı kullanmak için PartitionSelector nesnesi oluşturun ve Select * yöntemlerden birini kullanarak bölümü seçin. Daha sonra PartitionSelector nesnesinde gerektiriyorsa API'sine geçirin. Hiçbir seçenek belirlenirse, rastgele bir bölüm için varsayılan olarak ayarlanır.

```csharp
Uri serviceName = new Uri("fabric:/samples/InMemoryToDoListApp/InMemoryToDoListService");
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
string partitionName = "Partition1";
Int64 partitionKeyUniformInt64 = 1;

// Select a random partition
PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

// Select a partition based on ID
PartitionSelector partitionSelectorById = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);

// Select a partition based on name
PartitionSelector namedPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionName);

// Select a partition based on partition key
PartitionSelector uniformIntPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionKeyUniformInt64);
```

### <a name="replicaselector"></a>ReplicaSelector
ReplicaSelector Test Edilebilirlik kullanıma sunulan bir yardımcı olan ve bir yineleme Test Edilebilirlik eylemlerden herhangi birini gerçekleştirileceği seçmenize yardımcı olmak üzere kullanılır. Çoğaltma Kimliği önceden biliniyorsa belirli bir çoğaltma seçmek için kullanılabilir. Ayrıca, bir birincil çoğaltmayı veya rastgele ikincil seçme seçeneğiniz vardır. Çoğaltma ve Test Edilebilirlik işlemi gerçekleştirmek istediğiniz bölümü seçmek gereken şekilde ReplicaSelector PartitionSelector türer.

Bu yardımcı kullanmak için bir ReplicaSelector nesnesi oluşturun ve çoğaltma ve bölüm seçmek istediğiniz şekilde ayarlayın. Ardından bunu gerektiren API geçirebilirsiniz. Hiçbir seçenek belirlenirse, bir rastgele çoğaltma ve rastgele bölüm için varsayılan olarak.

```csharp
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);
long replicaId = 130559876481875498;

// Select a random replica
ReplicaSelector randomReplicaSelector = ReplicaSelector.RandomOf(partitionSelector);

// Select the primary replica
ReplicaSelector primaryReplicaSelector = ReplicaSelector.PrimaryOf(partitionSelector);

// Select the replica by ID
ReplicaSelector replicaByIdSelector = ReplicaSelector.ReplicaIdOf(partitionSelector, replicaId);

// Select a random secondary replica
ReplicaSelector secondaryReplicaSelector = ReplicaSelector.RandomSecondaryOf(partitionSelector);
```

## <a name="next-steps"></a>Sonraki adımlar
* [Test Edilebilirlik senaryoları](service-fabric-testability-scenarios.md)
* Hizmetinizi test etme
  * [Hataları sırasında hizmet iş yüklerinin benzetimi](service-fabric-testability-workload-tests.md)
  * [Hizmetten hizmete iletişim hatası](service-fabric-testability-scenarios-service-communication.md)

