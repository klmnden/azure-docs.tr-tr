---
title: Azure Service Fabric uygulamalarında hata benzetimleri gerçekleştirme | Microsoft Docs
description: Hizmetlerinizi zarif ve yaşanmamasını arızalarına karşı zorlaştırmak nasıl.
services: service-fabric
documentationcenter: .net
author: anmolah
manager: chackdan
editor: ''
ms.assetid: 44af01f0-ed73-4c31-8ac0-d9d65b4ad2d6
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/15/2017
ms.author: anmola
ms.openlocfilehash: ceb6ad1a6a1182d78c473b8b0387c365eb660065
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60865281"
---
# <a name="simulate-failures-during-service-workloads"></a>Hizmet iş yükleri sırasında hata benzetimleri yapma
Azure Service fabric'te Test Edilebilirlik senaryoları, geliştiricilerin tek hataları ne yapılacağı hakkında endişe duymamanızı olanak sağlar. Burada bir açık istemci iş yükü ve hataları Interleaving gerekebilecek senaryo vardır. İstemci iş yükü ve hataları Interleaving hata gerçekleştiğinde hizmet aslında bir eylem gerçekleştiriyor sağlar. Test Edilebilirlik sağlar denetim düzeyini göz önünde bulundurulduğunda, bu iş yükü yürütmeye kesin noktalarda olabilir. Bu endüksiyon farklı durumlarda uygulama hatalarını, hataları bulabilir ve kalitesini geliştirin.

## <a name="sample-custom-scenario"></a>Özel örnek senaryosu
Bu test, iş yüküyle karışır bir senaryo gösterilmektedir [zarif ve yaşanmamasını hataları](service-fabric-testability-actions.md#graceful-vs-ungraceful-fault-actions). Hataları, ortada hizmet işlemleri ya da en iyi sonuçlar için işlem başlattığı.

Dört iş yükleri ortaya koyan bir hizmet örneği atalım: A, B, C ve d Her iş akışları için karşılık gelen ve işlem, depolama veya bir karışımı olabilir. Basitleştirmek amacıyla, biz Bu örnekte iş yüklerinin ölçeğini soyut. Bu örnekte yürütülen farklı hatalar şunlardır:

* RestartNode: Makinenin yeniden başlatılması benzetimini yapmak için yaşanmamasını hatası.
* RestartDeployedCodePackage: Hizmet ana bilgisayarı işlemi benzetimini yapmak için yaşanmamasını hata kilitleniyor.
* RemoveReplica: Yineleme kaldırma benzetimini yapmak için normal hata.
* MovePrimary: Service Fabric yük dengeleyici tarafından tetiklenen bir yineleme taşır benzetimini yapmak için normal hata.

```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll.

using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        // Replace these strings with the actual version for your cluster and application.
        string clusterConnection = "localhost:19000";
        Uri applicationName = new Uri("fabric:/samples/PersistentToDoListApp");
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");

        Console.WriteLine("Starting Workload Test...");
        try
        {
            RunTestAsync(clusterConnection, applicationName, serviceName).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Workload Test failed: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Workload Test completed successfully.");
        return 0;
    }

    public enum ServiceWorkloads
    {
        A,
        B,
        C,
        D
    }

    public enum ServiceFabricFaults
    {
        RestartNode,
        RestartCodePackage,
        RemoveReplica,
        MovePrimary,
    }

    public static async Task RunTestAsync(string clusterConnection, Uri applicationName, Uri serviceName)
    {
        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);
        // Maximum time to wait for a service to stabilize.
        TimeSpan maxServiceStabilizationTime = TimeSpan.FromSeconds(120);

        // How many loops of faults you want to execute.
        uint testLoopCount = 20;
        Random random = new Random();

        for (var i = 0; i < testLoopCount; ++i)
        {
            var workload = SelectRandomValue<ServiceWorkloads>(random);
            // Start the workload.
            var workloadTask = RunWorkloadAsync(workload);

            // While the task is running, induce faults into the service. They can be ungraceful faults like
            // RestartNode and RestartDeployedCodePackage or graceful faults like RemoveReplica or MovePrimary.
            var fault = SelectRandomValue<ServiceFabricFaults>(random);

            // Create a replica selector, which will select a primary replica from the given service to test.
            var replicaSelector = ReplicaSelector.PrimaryOf(PartitionSelector.RandomOf(serviceName));
            // Run the selected random fault.
            await RunFaultAsync(applicationName, fault, replicaSelector, fabricClient);
            // Validate the health and stability of the service.
            await fabricClient.ServiceManager.ValidateServiceAsync(serviceName, maxServiceStabilizationTime);

            // Wait for the workload to finish successfully.
            await workloadTask;
        }
    }

    private static async Task RunFaultAsync(Uri applicationName, ServiceFabricFaults fault, ReplicaSelector selector, FabricClient client)
    {
        switch (fault)
        {
            case ServiceFabricFaults.RestartNode:
                await client.ClusterManager.RestartNodeAsync(selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RestartCodePackage:
                await client.ApplicationManager.RestartDeployedCodePackageAsync(applicationName, selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RemoveReplica:
                await client.ServiceManager.RemoveReplicaAsync(selector, CompletionMode.Verify, false);
                break;
            case ServiceFabricFaults.MovePrimary:
                await client.ServiceManager.MovePrimaryAsync(selector.PartitionSelector);
                break;
        }
    }

    private static Task RunWorkloadAsync(ServiceWorkloads workload)
    {
        throw new NotImplementedException();
        // This is where you trigger and complete your service workload.
        // Note that the faults induced while your service workload is running will
        // fault the primary service. Hence, you will need to reconnect to complete or check
        // the status of the workload.
    }

    private static T SelectRandomValue<T>(Random random)
    {
        Array values = Enum.GetValues(typeof(T));
        T workload = (T)values.GetValue(random.Next(values.Length));
        return workload;
    }
}
```
