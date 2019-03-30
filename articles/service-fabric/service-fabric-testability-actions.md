---
title: Azure mikro hizmetlerde hata benzetimleri yapma | Microsoft Docs
description: Bu makalede, Microsoft Azure Service Fabric'te bulunan Test Edilebilirlik eylemleri hakkında konuşuyor.
services: service-fabric
documentationcenter: .net
author: motanv
manager: chackdan
editor: heeldin
ms.assetid: ed53ca5c-4d5e-4b48-93c9-e386f32d8b7a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: motanv
ms.openlocfilehash: 37a794387f3a2f02124805705d380ad9f1fc1270
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58662865"
---
# <a name="testability-actions"></a>Test Edilebilirlik eylemleri
Güvenilir olmayan bir altyapı benzetimini yapmak için Azure Service Fabric, çeşitli gerçek hataları ve durum geçişlerini benzetimini yapmak için yol ile geliştirici olarak size sağlar. Bu işlem Test Edilebilirlik eylemleri sunulur. Belirli hata ekleme, durum geçişi ve doğrulama neden alt düzey API'ler eylemlerdir. Bu eylemleri birleştirerek, hizmetlerinizi için kapsamlı test senaryolarını yazabilirsiniz.

Service Fabric, bu eylemler bazı ortak test senaryoları oluşan sağlar. Ortak durumu geçişleri ve hata koşulları test etmek için dikkatli bir şekilde seçilen bu yerleşik senaryolar yazılımınız önemle öneririz. Ancak, Eylemler, kapsam, yerleşik senaryolarla henüz kapsamında değildir veya uygulamanız için uyarlanmış özel olan senaryoları için eklemek istediğiniz oluşturduğunuzda özel test senaryoları için kullanılabilir.

C#uygulamaları eylemlerin System.Fabric.dll derlemesinde bulunamadı. Sistem Fabric PowerShell modülünü Microsoft.ServiceFabric.Powershell.dll derlemesinde bulunamadı. Kullanım kolaylığı için izin vermek için ServiceFabric PowerShell modülü çalışma zamanı yüklemesinin bir parçası olarak yüklenir.

## <a name="graceful-vs-ungraceful-fault-actions"></a>Normal yaşanmamasını hata eylemleri karşılaştırması
Test Edilebilirlik eylemleri iki ana demetlerin içine sınıflandırılır:

* Yaşanmamasını hataları: Bu hatalar, makine yeniden başlatmaları gibi hata benzetimleri yapma ve kilitlenmeleri işleyebilirsiniz. Hata bu gibi durumlarda, işlem yürütme bağlamı aniden durdurur. Bu, uygulama yeniden başlatılmadan önce durumu temizlik çalıştırabileceğiniz anlamına gelir.
* Normal hataları: Bu hatalar, yineleme taşır ve Yük Dengeleme tarafından tetiklenen düşme gibi normal eylemlerin benzetimini yapar. Böyle durumlarda, hizmet kapatma ilişkin bir bildirim alır ve durumunu çıkmadan önce temizleyebilirsiniz.

Daha iyi kalite doğrulama için çeşitli zarif ve yaşanmamasını hataları inducing çalışırken hizmet ve iş yükünü çalıştırın. Yaşanmamasını hataları nerede hizmet işlemi bazı iş akışı ortasında aniden çıkar senaryoları alıştırma yapın. Service Fabric tarafından hizmet çoğaltma geri yüklendikten sonra bu kurtarma yolu sınar. Bu, veri tutarlılığı ve hizmet durumunu hatasından sonra doğru olup olmadığını yönetilmesini test yardımcı olur. Diğer küme hataları (normal hata sayısı) test Service Fabric tarafından taşınan çoğaltmalarına hizmetin doğru şekilde tepki verir. Bu işleme iptal RunAsync yönteminde sınar. Olan ayarlıysa, doğru durumunu kaydetmek için İptal belirtecini denetleyip RunAsync yöntemi çıkmak hizmet gerekiyor.

## <a name="testability-actions-list"></a>Test Edilebilirlik eylemleri listesi
| Eylem | Açıklama | Yönetilen API | PowerShell cmdlet'i | Normal/yaşanmamasını hataları |
| --- | --- | --- | --- | --- |
| CleanTestState |Tüm test durumu, test sürücüsünün hatalı bir kapanma durumunda bir kümeden kaldırır. |CleanTestStateAsync |Remove-ServiceFabricTestState |Geçerli değil |
| InvokeDataLoss |Veri kaybı hizmet bölüme sevk. |InvokeDataLossAsync |Invoke-ServiceFabricPartitionDataLoss |Normal |
| InvokeQuorumLoss |Belirli bir durum bilgisi olan hizmet bölüm çekirdek kayıp yerleştirir. |InvokeQuorumLossAsync |Çağırma ServiceFabricQuorumLoss |Normal |
| MovePrimary |Belirtilen küme düğümü için bir durum bilgisi olan hizmet belirtilen birincil çoğaltmasını taşır. |MovePrimaryAsync |Move-ServiceFabricPrimaryReplica |Normal |
| MoveSecondary |Durum bilgisi olan hizmet geçerli ikincil bir çoğaltmasına farklı küme düğümü için taşır. |MoveSecondaryAsync |Taşıma ServiceFabricSecondaryReplica |Normal |
| RemoveReplica |Bir çoğaltma hatası, bir kümeden bir çoğaltma kaldırarak benzetimini yapar. Bu çoğaltma kapatılır ve role geçer 'None', tüm durumuna kümeden kaldırma. |RemoveReplicaAsync |Remove-ServiceFabricReplica |Normal |
| RestartDeployedCodePackage |Bir kod paketi işlemi başarısız bir kümede dağıtılan bir kod paketi yeniden başlatarak benzetimini yapar. Bu, bu işlemde barındırılan tüm kullanıcı hizmet çoğaltmalar yeniden başlatılacak kod paket işlemi durdurur. |RestartDeployedCodePackageAsync |Yeniden başlatma ServiceFabricDeployedCodePackage |Yaşanmamasını |
| RestartNode |Bir Service Fabric küme düğümü hatası bir düğümü yeniden başlatarak benzetimini yapar. |RestartNodeAsync |Yeniden başlatma-ServiceFabricNode |Yaşanmamasını |
| RestartPartition |Bir veri merkezi Kararma veya küme Kararma senaryo bazılarını veya tümünü bir bölüm çoğaltmalarını yeniden başlatarak benzetimini yapar. |RestartPartitionAsync |Restart-ServiceFabricPartition |Normal |
| RestartReplica |Bir çoğaltma hatası bir kümede kalıcı bir çoğaltması yeniden başlatılıyor, çoğaltma kapatma ve yeniden açmayı benzetimini yapar. |RestartReplicaAsync |Yeniden başlatma-servicefabricreplica komutunu |Normal |
| BaşlangıçDüğümü |Bir düğüm zaten durdurulmuş bir kümede başlatır. |StartNodeAsync |Start-ServiceFabricNode |Geçerli değil |
| StopNode |Bir düğümde hata oluştuktan bir kümedeki bir düğüm durdurarak benzetimini yapar. BaşlangıçDüğümü çağrılana kadar düğümü kapalı kalır. |StopNodeAsync |Stop-ServiceFabricNode |Yaşanmamasını |
| ValidateApplication |Kullanılabilirlik ve bazı hata sisteme inducing sonra genellikle bir uygulamadaki tüm Service Fabric hizmetlerinin durumunu doğrular. |ValidateApplicationAsync |Test-ServiceFabricApplication |Geçerli değil |
| ValidateService |Bir Service Fabric hizmetinin sistem durumunu ve kullanılabilirliğini genellikle sisteme bazı hata inducing sonra doğrular. |ValidateServiceAsync |Test-ServiceFabricService |Geçerli değil |

## <a name="running-a-testability-action-using-powershell"></a>PowerShell kullanarak bir Test Edilebilirlik eylemi çalıştıran
Bu öğreticide PowerShell kullanarak bir Test Edilebilirlik eylemi çalıştırmak gösterilmektedir. Yerel bir (bir kutu) kümesi veya Azure kümesine karşı Test Edilebilirlik eylemi çalıştırmayı öğreneceksiniz. Microsoft.Fabric.Powershell.dll--Service Fabric PowerShell modülünü--Microsoft Service Fabric MSI yüklediğinizde otomatik olarak yüklenir. Bir PowerShell istemi açtığınızda modülü otomatik olarak yüklenir.

Öğretici segmentleri:

* [Eylem bir çalıştırma kümesine göre çalıştırma](#run-an-action-against-a-one-box-cluster)
* [Eylem Azure kümesine karşı çalıştırma](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a>Eylem bir çalıştırma kümesine göre çalıştırma
Test Edilebilirlik eylemi yerel kümede çalıştırmak için ilk kümeye bağlanın ve Yönetici modunda bir PowerShell istemi açın. Bize bakmak **Restart-ServiceFabricNode** eylem.

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

Burada eylemi **Restart-ServiceFabricNode** "Düğüm1" adlı bir düğümde çalıştırın. Bunu düğümünü yeniden başlatma eylemi gerçekten başarılı olup olmadığını doğrulamalısınız değil, tamamlama modunu belirtir. "Doğrula" yeniden başlatma eylemini gerçekten başarılı olup olmadığını doğrulamak neden olacak şekilde tamamlama modunu belirtme. Düğüm adını kullanarak doğrudan belirtmek yerine, bu bölüm anahtarını ve çoğaltma, türü şu şekilde belirtebilirsiniz:

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

**Yeniden başlatma-ServiceFabricNode** bir kümedeki Service Fabric düğümü yeniden başlatmak için kullanılmalıdır. Bu, tüm bu düğümde barındırılan sistemi hizmeti ve kullanıcı hizmet çoğaltmalar yeniden Fabric.exe işlemi durdurur. Hizmetinizi test etmek için bu API'yi kullanarak yük devretme kurtarma yolları boyunca hatalar ortaya çıkarmaya yardımcı olur. Kümedeki düğüm hata benzetimleri yapma yardımcı olur.

Aşağıdaki ekran görüntüsü gösterildiği **Restart-ServiceFabricNode** işlem Test Edilebilirlik komutu.

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

İlk çıkış **Get-ServiceFabricNode** (Service Fabric PowerShell modülden bir cmdlet) yerel kümeye beş düğüme sahip olduğunu gösterir: Node.1 Node.5 için. Test Edilebilirlik eylemi (cmdlet'ini) sonra **Restart-ServiceFabricNode** düğümde yürütülen Node.4 adlandırılmış düğümün çalışır durumda kalma süresi sıfırlandı görüyoruz.

### <a name="run-an-action-against-an-azure-cluster"></a>Eylem Azure kümesine karşı çalıştırma
Test Edilebilirlik eylemi çalıştıran bir Azure kümesine karşı (PowerShell kullanarak), eylemi yerel kümede çalıştırmak için benzerdir. Tek fark, yerel kümeye bağlanma yerine eylem çalıştırmadan önce ilk Azure kümesine bağlanmak olmanızın gerekmesidir.

## <a name="running-a-testability-action-using-c35"></a>C kullanarak bir Test Edilebilirlik eylemi çalıştıran&#35;
Test Edilebilirlik eylemi çalıştırmak amacıyla kullanmak üzere C#, ilk FabricClient kullanarak kümeye bağlanmak gerekir. Ardından bir eylemi çalıştırmak için gereken parametreleri edinin. Farklı Parametreler aynı eylemi çalıştırmak için kullanılabilir.
RestartServiceFabricNode eylem bakarak, çalıştırmak için bir kümedeki düğüm bilgileri (düğüm adı ve düğüm örnek kimliği) kullanarak yoludur.

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

Parametre açıklaması:

* **CompleteMode** modu yeniden başlatma eylemini gerçekten başarılı olup olmadığını doğrulamalısınız değil olduğunu belirtir. "Doğrula" yeniden başlatma eylemini gerçekten başarılı olup olmadığını doğrulamak neden olacak şekilde tamamlama modunu belirtme.  
* **OperationTimeout** işlemi TimeoutException özel durum önce tamamlanması için geçen süreyi ayarlar.
* **CancellationToken** iptal için bekleyen bir çağrı sağlar.

Düğümün adını kullanarak doğrudan belirtmek yerine, bu bölüm anahtarını ve çoğaltma türü belirtebilirsiniz.

PartitionSelector ve ReplicaSelector daha fazla bilgi için bkz.

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
PartitionSelector Test Edilebilirlik içinde kullanıma sunulan bir yardımcı olan ve herhangi bir Test Edilebilirlik eylemleri gerçekleştirmek için belirli bir bölüme seçmek için kullanılır. Belirli bir bölüme, bölüm kimliği önceden biliniyorsa seçmek için kullanılabilir. Ya da bölüm anahtarı sağlayabilir ve işlem bölüm kimliği dahili olarak çözer. Ayrıca, rastgele bir bölüm seçme seçeneğiniz de vardır.

Bu yardımcı kullanmak için PartitionSelector nesnesi oluşturun ve Select * yöntemlerden birini kullanarak bölümü seçin. Ardından PartitionSelector nesnesinde gerektiriyorsa API'sine geçirin. Hiçbir seçenek belirlenirse, rastgele bir bölümü için varsayılan olarak.

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
ReplicaSelector Test Edilebilirlik içinde kullanıma sunulan bir yardımcı olan ve herhangi bir Test Edilebilirlik eylemleri gerçekleştirmek için bir çoğaltma seçmenize yardımcı olması için kullanılır. Belirli bir çoğaltma, çoğaltma kimliği önceden biliniyorsa seçmek için kullanılabilir. Ayrıca, bir birincil çoğaltmaya veya rastgele bir ikincil seçme seçeneğiniz vardır. Çoğaltma hem de Test Edilebilirlik işlemi gerçekleştirmek istediğiniz bölümü seçmek gereken şekilde ReplicaSelector PartitionSelector türer.

Bu yardımcı kullanmak için ReplicaSelector nesnesi oluşturma ve çoğaltma ve bölüm seçmek istediğiniz şekilde ayarlayın. Ardından bunu gerektiren API'ye geçirebilirsiniz. Seçeneği seçili ise, bir rastgele çoğaltma ve rastgele bir bölüm için varsayılan olarak.

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
  * [Hizmet iş yükleri sırasında hata benzetimleri yapma](service-fabric-testability-workload-tests.md)
  * [Hizmetten hizmete iletişimi hataları](service-fabric-testability-scenarios-service-communication.md)

