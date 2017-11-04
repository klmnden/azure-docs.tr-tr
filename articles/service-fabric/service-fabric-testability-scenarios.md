---
title: "Azure mikro Chaos ve yük devretme testleri oluşturma | Microsoft Docs"
description: "Service Fabric kullanarak chaos test ve yük devretme hatalarını anlamına ve hizmetlerinizi güvenilirliğini doğrulamak için test senaryoları."
services: service-fabric
documentationcenter: .net
author: motanv
manager: rsinha
editor: toddabel
ms.assetid: 8eee7e89-404a-4605-8f00-7e4d4fb17553
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: motanv
ms.openlocfilehash: d06026c750e01ad5825338a78d9af331265f434a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="testability-scenarios"></a>Test Edilebilirlik senaryoları
Bulut altyapılarının doğası gereği güvenilir gibi büyük dağıtılan sistemlerde. Azure Service Fabric geliştiriciler güvenilmez altyapısının en üstünde services yazma olanağı sağlar. Yüksek kaliteli Hizmetleri yazma için geliştiriciler böyle güvenilir olmayan bir altyapı hizmetlerini kararlılığını test etmek için anlamına olması gerekir.

Hata analizi hizmeti geliştiriciler hataları varlığında Hizmetleri test etmek için hata eylemleri anlamına olanağı sağlar. Ancak, hedeflenen benzetimli hataları yalnızca o ana kadarki alırsınız. Test daha fazla yararlanmak için Service Fabric test senaryoları kullanabilirsiniz: chaos test ve bir yük devretme testi. Bu senaryolar sürekli araya eklemeli hataları, normal ve küme üzerinden uzun süre boyunca durunda benzetimi. Bir test oranı ve hataları tür ile yapılandırıldıktan sonra C# API'leri veya PowerShell, küme ve hizmetinizi hataları oluşturmak için üzerinden başlatılabilir.

> [!WARNING]
> ChaosTestScenario daha esnektir, hizmet tabanlı Chaos tarafından değiştirilmektedir. Lütfen yeni makalesine başvurun [denetlenen Chaos](service-fabric-controlled-chaos.md) daha fazla ayrıntı için.
> 
> 

## <a name="chaos-test"></a>Chaos test
Chaos senaryo hataları tüm Service Fabric kümesi oluşturur. Senaryo, ay veya yıl birkaç saat için genellikle görülen hataları sıkıştırır. Yüksek hata oranı araya eklemeli hataları birleşimi, aksi takdirde eksik köşe durumlarda bulur. Bu hizmet kod kalitesi, önemli bir iyileştirme neden olmaktadır.

### <a name="faults-simulated-in-the-chaos-test"></a>Chaos testinde benzetimli hataları
* Bir düğümü yeniden başlatın
* Dağıtılmış kod paketi yeniden başlatın
* Bir yineleme kaldırma
* Bir çoğaltmayı yeniden başlatın
* (İsteğe bağlı) birincil kopya taşıma
* (İsteğe bağlı) bir ikincil çoğaltma taşıma

Chaos test hataları ve küme doğrulama birden çok kez belirtilen süre için çalışır. Başarılı olması doğrulama ve kararlı küme için harcanan süre de yapılandırılabilir. Tek bir küme doğrulama hatasına isabet senaryo başarısız oluyor.

Örneğin, bir saat için en fazla üç eşzamanlı hataları ile çalışacak şekilde ayarlanmış bir test göz önünde bulundurun. Test üç hataları anlamına ve sonra küme sistem durumunu doğrulayın. Test ve önceki adımla küme bozulur veya bir saat geçirir kadar yineleme. Kümenin her yinelemede bozulursa, yani, yapılandırılan süre içinde Sabitle değil, test bir özel durum ile başarısız olur. Bu durum, bir şeyler yanlış geçti ve daha fazla araştırma gerekiyor gösterir.

Mevcut haliyle chaos test hata nesil altyapısında yalnızca güvenli hataları uygulanmasını. Başka bir deyişle, dış hataları olmaması durumunda, bir çekirdek veya veri kaybı asla meydana gelmez.

### <a name="important-configuration-options"></a>Önemli yapılandırma seçenekleri
* **TimeToRun**: toplam süre test ile başarılı tamamlamadan önce çalışır. Test daha önce bir doğrulama hatası yerine bitirebilirsiniz.
* **MaxClusterStabilizationTimeout**: en fazla test başarısız olmadan önce sağlıklı duruma kümeye için beklenecek süre miktarı. Küme durumu Tamam olup gerçekleştirilen denetimlerinin olduğundan, hizmet durumu Tamam olduğundan, hizmet bölüm hedef çoğaltma kümesi boyutu elde edilir ve hiçbir Inbuild çoğaltmaların mevcut.
* **MaxConcurrentFaults**: en fazla eş zamanlı hatalarının sayısı her yinelemede kopyaladığınızda. Sayı, daha katı bu nedenle daha karmaşık yük devretme ve geçiş birleşimleri kaynaklanan test. Test dış hataları olmaması durumunda olmayacaktır, bu yapılandırmanın nasıl yüksek olan yedeklemiş çekirdek veya veri kaybı güvence altına alır.
* **EnableMoveReplicaFaults**: etkinleştirir ya da birincil veya ikincil çoğaltmaları taşınmasına neden olan hataları devre dışı bırakır. Bu hataları varsayılan olarak devre dışıdır.
* **WaitTimeBetweenIterations**: tekrar, yani gidiş hataları ve karşılık gelen doğrulama arasında beklenecek süre.

### <a name="how-to-run-the-chaos-test"></a>Chaos testi çalıştırma
C# örneği

```csharp
using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";

        Console.WriteLine("Starting Chaos Test Scenario...");
        try
        {
            RunChaosTestScenarioAsync(clusterConnection).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Chaos Test Scenario did not complete: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Chaos Test Scenario completed.");
        return 0;
    }

    static async Task RunChaosTestScenarioAsync(string clusterConnection)
    {
        TimeSpan maxClusterStabilizationTimeout = TimeSpan.FromSeconds(180);
        uint maxConcurrentFaults = 3;
        bool enableMoveReplicaFaults = true;

        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);

        // The chaos test scenario should run at least 60 minutes or until it fails.
        TimeSpan timeToRun = TimeSpan.FromMinutes(60);
        ChaosTestScenarioParameters scenarioParameters = new ChaosTestScenarioParameters(
          maxClusterStabilizationTimeout,
          maxConcurrentFaults,
          enableMoveReplicaFaults,
          timeToRun);

        // Other related parameters:
        // Pause between two iterations for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenIterations = TimeSpan.FromSeconds(30);
        // Pause between concurrent actions for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenFaults = TimeSpan.FromSeconds(10);

        // Create the scenario class and execute it asynchronously.
        ChaosTestScenario chaosScenario = new ChaosTestScenario(fabricClient, scenarioParameters);

        try
        {
            await chaosScenario.ExecuteAsync(CancellationToken.None);
        }
        catch (AggregateException ae)
        {
            throw ae.InnerException;
        }
    }
}
```

PowerShell

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```


## <a name="failover-test"></a>Yük devretme testi
Yük devretme testi senaryosu, belirli hizmet bölüm hedefler chaos test senaryosu sürümüdür. Belirli hizmet bölüm yük devretme etkisi diğer hizmetlerin etkilenmemesini bırakarak sınar. Hedef bölüm bilgileri ve diğer parametreler ile yapılandırıldıktan sonra bir hizmet bölüm için hataları oluşturmak için C# API'leri veya PowerShell kullanan bir istemci-tarafı araç olarak çalışır. İş mantığınızın bir iş yükü sağlamak için tarafında çalışırken senaryo bir dizi benzetimli hataları ve hizmet doğrulama yineler. Bir hizmet doğrulama hatası daha fazla araştırma gereken bir sorun olduğunu gösterir.

### <a name="faults-simulated-in-the-failover-test"></a>Yük devretme testi benzetimli hataları
* Bölüm barındırıldığı dağıtılmış kod paketi yeniden başlatın
* Bir birincil/ikincil çoğaltma veya durum bilgisiz örneğini Kaldır
* Birincil ve ikincil bir çoğaltma (bir kalıcı hizmeti) yeniden Başlat
* Birincil çoğaltma taşıma
* Bir ikincil çoğaltma taşıma
* Bölüm yeniden başlatın

Yük devretme testi seçilen hataya uygulanmasını ve sonra doğrulama kendi kararlılık sağlamak için hizmeti üzerinde çalışır. Yük devretme testi yalnızca bir arıza chaos testinde birden çok hatalarının olası aksine, her defasında uygulanmasını. Hizmet bölüm sonra her hata yapılandırılmış zaman aşımı süresi içinde Sabitle değil sınama başarısız olur. Test yalnızca güvenli hataları uygulanmasını. Başka bir deyişle, dış hataları olmaması durumunda, bir çekirdek veya veri kaybı değil ortaya çıkar.

### <a name="important-configuration-options"></a>Önemli yapılandırma seçenekleri
* **PartitionSelector**: hedeflenecek gereken bölümü belirtir Seçici nesnesi.
* **TimeToRun**: toplam süre test tamamlamadan önce çalışır.
* **MaxServiceStabilizationTimeout**: en fazla test başarısız olmadan önce sağlıklı duruma kümeye için beklenecek süre miktarı. Hizmet durumu Tamam olup gerçekleştirilen denetimlerinin olduğundan, hedef çoğaltma kümesi boyutu tüm bölümler için elde edilir ve hiçbir Inbuild çoğaltmaların mevcut.
* **WaitTimeBetweenFaults**: her arıza ve doğrulama döngüsü süre miktarı.

### <a name="how-to-run-the-failover-test"></a>Yük devretme testi çalıştırma
**C#**

```csharp
using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");

        Console.WriteLine("Starting Chaos Test Scenario...");
        try
        {
            RunFailoverTestScenarioAsync(clusterConnection, serviceName).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Chaos Test Scenario did not complete: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Chaos Test Scenario completed.");
        return 0;
    }

    static async Task RunFailoverTestScenarioAsync(string clusterConnection, Uri serviceName)
    {
        TimeSpan maxServiceStabilizationTimeout = TimeSpan.FromSeconds(180);
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);

        // The chaos test scenario should run at least 60 minutes or until it fails.
        TimeSpan timeToRun = TimeSpan.FromMinutes(60);
        FailoverTestScenarioParameters scenarioParameters = new FailoverTestScenarioParameters(
          randomPartitionSelector,
          timeToRun,
          maxServiceStabilizationTimeout);

        // Other related parameters:
        // Pause between two iterations for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenIterations = TimeSpan.FromSeconds(30);
        // Pause between concurrent actions for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenFaults = TimeSpan.FromSeconds(10);

        // Create the scenario class and execute it asynchronously.
        FailoverTestScenario failoverScenario = new FailoverTestScenario(fabricClient, scenarioParameters);

        try
        {
            await failoverScenario.ExecuteAsync(CancellationToken.None);
        }
        catch (AggregateException ae)
        {
            throw ae.InnerException;
        }
    }
}
```


**PowerShell**

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/SampleApp/SampleService"

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindSingleton
```
