---
title: Kaos ve yük devretme testleri oluşturmak için Azure Service Fabric | Microsoft Docs
description: Service Fabric kullanarak test kaos ve yük devretme hatalarını anlamına ve hizmetlerinizi güvenilirliğini doğrulamak için test senaryoları.
services: service-fabric
documentationcenter: .net
author: motanv
manager: rsinha
editor: toddabel
ms.assetid: 8eee7e89-404a-4605-8f00-7e4d4fb17553
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: motanv
ms.openlocfilehash: d12c5097d4ba5e0ccfe0e2b2cbc8ccd758c32d98
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60865042"
---
# <a name="testability-scenarios"></a>Test Edilebilirlik senaryoları
Doğası gereği güvenilir bulut altyapıları gibi büyük çaplı dağıtılmış sistemlerde. Azure Service Fabric, geliştiricilerin Hizmetleri güvenilir altyapıları üzerinde çalıştırmak için yazma olanağı sunar. Yüksek kaliteli hizmetler yazmak üzere anlamına böyle güvenilir bir altyapı hizmetlerinin kararlılığını test etmek geliştiricilerin gerekir.

Hata analizi hizmeti geliştiricilerin Hizmetleri oluşturucunun hata olması durumunda test etmek için hata eylemleri anlamına olanağı sağlar. Bununla birlikte, hedeflenen sanal hataları yalnızca kadarki alırsınız. Test daha fazla yararlanmak için Service Fabric'te test senaryoları kullanabilirsiniz: bir chaos testi ve yük devretme sınaması. Bu senaryolar hem normal hem de küme üzerinden uzun süreler boyunca yaşanmamasını olmak üzere sürekli araya eklemeli hatalarının benzetimini yapın. Bir test türü arızaları ve oranı ile yapılandırıldıktan sonra C# API veya PowerShell, küme ve hizmetinizi hataları oluşturmak için aracılığıyla başlatılabilir.

> [!WARNING]
> Daha esnek, hizmet tabanlı bir şeye kaos ChaosTestScenario değiştirilmektedir. Lütfen yeni makaleye başvurun [denetimli Chaos](service-fabric-controlled-chaos.md) daha fazla ayrıntı için.
> 
> 

## <a name="chaos-test"></a>Kaos test
Kaos senaryo hataları tüm Service Fabric kümesi oluşturur. Bu senaryo genellikle ay veya yıl birkaç saat içinde görülen hatalar sıkıştırır. Yüksek hata oranı aralıklı hatalar birleşimi, aksi takdirde eksik köşe durumları bulur. Bu hizmet kod kalitesi önemli bir iyileştirme neden olur.

### <a name="faults-simulated-in-the-chaos-test"></a>Kaos testinde benzetimli hataları
* Bir düğümü yeniden başlatın
* Dağıtılan bir kod paketi yeniden başlatın
* Bir çoğaltma kaldırma
* Bir çoğaltmayı yeniden başlatın
* (İsteğe bağlı) birincil kopya Taşı
* (İsteğe bağlı) bir ikincil çoğaltma Taşı

Kaos test hataları ve küme doğrulama çoklu yinelemelerini belirtilen süre için çalışır. Kümenin kararlı ve doğrulamanın başarılı olması için harcanan süreyi de yapılandırılabilir. Tek bir küme doğrulama hatası ulaştığınızda senaryo başarısız olur.

Örneğin, bir saat boyunca en fazla üç eş zamanlı hataları ile çalışacak şekilde ayarlanmış bir test düşünün. Test üç hataları anlamına ve ardından küme sistem durumunu doğrulayın. Test önceki adımda küme kötüleşir ya da bir saat geçirir kadar yineleme. Kümenin her yinelemede bozulursa, başka bir deyişle, yapılandırılmış bir süre içinde Sabitle değil, test bir özel durum ile başarısız olur. Bu özel durum bir sorun oluştu ve daha fazla araştırma gereken gösterir.

Mevcut haliyle chaos testinde hata üretme altyapısı yalnızca güvenli hataları sevk. Bu, dış hataların olmaması durumunda, bir çekirdek veya veri kaybı asla meydana gelmez anlamına gelir.

### <a name="important-configuration-options"></a>Önemli yapılandırma seçenekleri
* **Timetorun değeri**: Test başarılı bir şekilde bitmeden önce çalışır, toplam süre. Test, daha önce bir doğrulama hatası yerine tamamlayabilir.
* **MaxClusterStabilizationTimeout**: En fazla kümenin sağlıklı duruma test başarısız olmadan önce beklenecek süre miktarı. Küme durumu Tamam olup gerçekleştirilen denetimler olan, hizmet durumu sağlam, hizmet bölüm hedef çoğaltma kümesi boyutu elde edilir ve hiçbir Inbuild çoğaltmaların mevcut.
* **MaxConcurrentFaults**: En fazla eş zamanlı hataların sayısı, her yinelemede başlattı. Sayı, daha agresif bu nedenle daha karmaşık yük devretmeler ve geçiş kombinasyonları kaynaklanan test. Test dış hataların olmaması durumunda olmaz, bu yapılandırmanın nasıl yüksek olduğu fark etmeksizin çekirdek veya veri kaybı garanti eder.
* **EnableMoveReplicaFaults**: Etkinleştirir veya birincil veya ikincil çoğaltmaları taşınmasına neden olan hataları devre dışı bırakır. Bu hatalar, varsayılan olarak devre dışıdır.
* **WaitTimeBetweenIterations**: Hataları ve karşılık gelen doğrulama bir tur sonra başka bir deyişle, yinelemeleri arasında beklenecek süre miktarı.

### <a name="how-to-run-the-chaos-test"></a>Kaos test çalıştırma
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
Yük devretme testi senaryosu, bir hizmete bölüm hedefleyen chaos test senaryosu sürümüdür. Yük devretme belirli hizmet bölüm üzerindeki etkisini, diğer hizmetler etkilenmeyen bırakarak sınar. Hedef bölüm bilgileri ve diğer parametreler ile yapılandırıldıktan sonra hizmet bölümü hataları oluşturmak için C# API veya PowerShell kullanan bir istemci-tarafı aracı olarak çalışır. İş mantığınızı bir iş yükü sağlamak tarafında çalışırken senaryo sanal hataları ve hizmet doğrulama bir dizi yinelenir. Hizmet doğrulama bir hata, daha fazla araştırma gerektiren bir sorun olduğunu gösterir.

### <a name="faults-simulated-in-the-failover-test"></a>Yük devretme testi sanal hataları
* Bölümün barındırıldığı dağıtılan kod paketi yeniden başlatın
* Bir birincil/ikincil çoğaltma veya durum bilgisi olmayan örnek Kaldır
* Birincil ve ikincil bir çoğaltmaya (kalıcı hizmet varsa) yeniden başlatın.
* Birincil çoğaltma Taşı
* İkincil bir çoğaltmaya Taşı
* Bölümü yeniden başlatın

Yük devretme testi seçilen hata sevk ve kendi kararlılık sağlamak hizmette doğrulama çalıştırır. Yük devretme testi tek bir hata chaos test birden çok hatalarının olası aksine, bir zaman sevk. Hizmet bölüm sonra her hata yapılandırılmış zaman aşımı süresi içinde Sabitle değil, test başarısız olur. Test yalnızca güvenli hataları sevk. Başka bir deyişle, dış hataları olmaması durumunda, bir çekirdek veya veri kaybı olmayan ortaya çıkar.

### <a name="important-configuration-options"></a>Önemli yapılandırma seçenekleri
* **PartitionSelector**: Hedeflenecek gereken bölümü belirtir Seçici nesnesi.
* **Timetorun değeri**: Test bitmeden önce çalışır, toplam süre.
* **MaxServiceStabilizationTimeout**: En fazla kümenin sağlıklı duruma test başarısız olmadan önce beklenecek süre miktarı. Hizmet durumu Tamam olup gerçekleştirilen denetimler olan, hedef çoğaltma kümesi boyutu tüm bölümler için elde edilir ve hiçbir Inbuild çoğaltmaların mevcut.
* **WaitTimeBetweenFaults**: Her hata ve doğrulama döngüsü arasında beklenecek süre miktarı.

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
