---
title: Service Fabric kümelerinde Chaos anlamına | Microsoft Docs
description: Kümede Chaos yönetmek için hata ekleme ve küme Analysis Service API'leri kullanarak.
services: service-fabric
documentationcenter: .net
author: motanv
manager: anmola
editor: motanv
ms.assetid: 2bd13443-3478-4382-9a5a-1f6c6b32bfc9
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/05/2018
ms.author: motanv
ms.openlocfilehash: 7a22b17d45218c2f78220f980605b3c211495606
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543733"
---
# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>Denetimli Chaos Service Fabric kümelerinde anlamına
Doğası gereği güvenilir bulut altyapıları gibi büyük ölçekli dağıtılmış sistemler. Azure Service Fabric, geliştiricilerin güvenilir olmayan bir altyapının en üstünde güvenilir dağıtılmış hizmetlerinizi yazmanızı sağlar. Sağlam dağıtılmış hizmetleri güvenilir bir altyapı üzerine yazmak için güvenilir olmayan altyapının hataları nedeniyle karmaşık durumu geçişleri üzerinden geçiyor sırada hizmetlerinin kararlılığını test edebilmek geliştiricilerin gerekir.

[Hata ekleme ve küme analiz hizmeti](https://docs.microsoft.com/azure/service-fabric/service-fabric-testability-overview) (hata analizi hizmeti olarak da bilinir) geliştiriciler hizmetlerini test etmek için hataları anlamına sağlar. Hedeflenen bu gibi hataları, benzetimli [bir bölümü yeniden](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps), en yaygın durum geçişlerini alıştırma yardımcı olabilir. Ancak hedeflenen sanal hataları tanımına göre ağırlıklı ve bu nedenle kaçırabilir gösteren yedekleme yalnızca sabit tahmin, uzun ve karmaşık dizisi durumu geçişleri hataları. Bir popülasyon test etmek için Chaos kullanabilirsiniz.

Kaos küme boyunca düzenli, aralıklı hatalar (normal ve yaşanmamasını) uzun süreler boyunca benzetimini yapar. Service Fabric API çağrılarının bir dizi normal bir hata oluşur, çünkü bu bir çoğaltma üzerinde açık tarafından izlenen bir kapatma, yeniden çoğaltma hatası normal hata. Çoğaltmayı kaldırmak için birincil çoğaltma ve ikincil çoğaltma taşıma Chaos tarafından gerçekleştirilen diğer normal hatalarıdır taşıyın. Düğümü yeniden başlatın ve yeniden kod paketi gibi işlem sonlandırılır yaşanmamasını hatalarıdır. 

Kaos türü arızaları oranı ile yapılandırdıktan sonra C#, Powershell veya REST API hatalarını kümesindeki ve hizmetlerinizdeki oluşturmaya başlamak için Chaos başlayabilirsiniz. Sonra hangi Chaos durdurur (örneğin, bir saat), belirtilen süre boyunca çalıştırmak için Chaos otomatik olarak yapılandırabilir veya StopChaos API (C#, Powershell veya REST) dilediğiniz zaman durdurmak için çağırabilirsiniz.

> [!NOTE]
> Mevcut haliyle, dış hataların olmaması durumunda çekirdek kayıp veya veri kaybı hiçbir zaman oluştuğunu anlamına gelir, yalnızca güvenli hataları Chaos sevk.
>

Kaos çalışırken, şu anda çalışma durumunu yakalayan farklı olayları üretir. Örneğin, bir ExecutingFaultsEvent Chaos bu yinelemede yürütmek için karar verdiği tüm hataları içerir. Bir ValidationFailedEvent küme doğrulama sırasında bulundu bir doğrulama hatası (sistem durumu veya sağlamlık sorunları) ayrıntılarını içerir. GetChaosReport Chaos çalıştırılan rapor almak için API (C#, Powershell veya REST) çağırabilirsiniz. Bu olaylar, kalıcı bir [güvenilir bir sözlük](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-reliable-collections), iki yapılandırmaları tarafından dikte bir kesme ilkesi vardır: **MaxStoredChaosEventCount** (varsayılan değer: 25000) ve **StoredActionCleanupIntervalInSeconds** (varsayılan değer: 3600). Her *StoredActionCleanupIntervalInSeconds* Chaos denetler ve tüm ancak en son *MaxStoredChaosEventCount* olayları, güvenilir sözlükten temizlendi.

## <a name="faults-induced-in-chaos"></a>İçinde Chaos kaynaklanan hatalar
Kaos hatalar arasında tüm Service Fabric kümesi oluşturur ve ay veya yıl birkaç saat içinde görülmeyen hataları sıkıştırır. Yüksek hata oranı aralıklı hatalar birleşimi, aksi takdirde atlanabilir köşe durumları bulur. Bu alıştırmada Chaos hizmet kod kalitesi önemli bir iyileştirme neden olur.

Aşağıdaki kategorilerden hataları Chaos sevk:

* Bir düğümü yeniden başlatın
* Dağıtılan bir kod paketi yeniden başlatın
* Bir çoğaltma kaldırma
* Bir çoğaltmayı yeniden başlatın
* Bir birincil çoğaltmaya (yapılandırılabilir) taşıma
* İkincil bir çoğaltmaya (yapılandırılabilir) taşıma

Kaos birden çok yinelemelerde çalıştırır. Her yineleme hataları ve belirli bir süre için küme doğrulama oluşur. Kümenin kararlı ve doğrulamanın başarılı olması için harcanan süre yapılandırabilirsiniz. Küme doğrulama bir hata bulunursa, Chaos oluşturur ve bir ValidationFailedEvent UTC zaman damgası ve hatası ayrıntıları ile devam ettirir. Örneğin, bir saat için en fazla üç eş zamanlı hataları ile çalışacak şekilde ayarlanmış bir Chaos örneği göz önünde bulundurun. Chaos üç hataları sevk ve ardından küme durumu doğrular. Önceki yinelenir, açıkça StopChaosAsync API aracılığıyla durmuş veya bir saatlik oluncaya kadar adım geçirir. Kümenin her yinelemede bozulursa (diğer bir deyişle, değil Sabitle veya içinde geçirilen MaxClusterStabilizationTimeout sağlıklı olmaz), Chaos bir ValidationFailedEvent oluşturur. Bu olay, bir sorun oluştu ve daha fazla araştırma gerekebileceğini gösterir.

Kaos başlattığı hangi hataların almak için GetChaosReport API (powershell, C# veya REST) kullanabilirsiniz. API sonraki segmentini Chaos raporun geçilen devamlılık belirteci veya geçirilen zaman aralığı temel alır. Kaos raporun sonraki segmentini almak için ContinuationToken ya da belirtebilirsiniz veya StartTimeUtc ve EndTimeUtc aracılığıyla zaman aralığını belirtebilirsiniz, ancak aynı çağrıda ContinuationToken hem zaman aralığı belirtemezsiniz. 100'den fazla Chaos olaylar olduğunda, Chaos rapor burada bir segment en fazla 100 Chaos olayları içeren segmentler döndürülür.

## <a name="important-configuration-options"></a>Önemli yapılandırma seçenekleri
* **Timetorun değeri**: Kaos çalışmadan başarılı bir şekilde biten toplam süresi. StopChaos API aracılığıyla timetorun değeri boyunca çalıştıktan önce Chaos durdurabilirsiniz.

* **MaxClusterStabilizationTimeout**: Maksimum kümenin sağlıklı duruma bir ValidationFailedEvent üretme önce beklenecek süre miktarı. Kurtarılmakta olduğu sırada küme üzerindeki yükü azaltmak için bu bekleyin. Gerçekleştirilen denetimler şunlardır:
  * Küme durumu Tamam ise
  * Hizmet durumu Tamam ise
  * Hizmet bölüm hedef çoğaltma kümesi boyutu varsa sağlanır
  * Hiçbir Inbuild çoğaltmaların bulunduğunu
* **MaxConcurrentFaults**: Her yinelemede başlattığı eşzamanlı hataların sayısı. Daha yüksek bir sayı, daha agresif kaos ve yük devretme ve küme geçtiği durum geçişi birleşimleri ayrıca daha karmaşık olmasıdır. 

> [!NOTE]
> Bakılmaksızın nasıl yüksek bir değer *MaxConcurrentFaults* varsa, Chaos garanti - dış hataları - olmaması durumunda çekirdek kayıp veya veri kaybı yoktur.
>

* **EnableMoveReplicaFaults**: Etkinleştirir veya taşımak birincil veya ikincil çoğaltmaları neden hataları devre dışı bırakır. Bu hatalar, varsayılan olarak etkindir.
* **WaitTimeBetweenIterations**: Yinelemeleri arasında beklenecek süre miktarı. Diğer bir deyişle, hataları ve küme durumunu karşılık gelen doğrulama tamamlandı bir gidiş yürütüldükten sonra süreyi Chaos duraklatılır. Yüksek değer, alt ortalama hata ekleme hızıdır.
* **WaitTimeBetweenFaults**: Tek bir yineleme iki ardışık hataları arasında beklenecek süre miktarı. Daha yüksek değeri, daha düşük eşzamanlılığı (veya arasında çakışma) hataları.
* **ClusterHealthPolicy**: Küme sistem durumu ilkesi Chaos yinelemeleri arasında küme durumunu doğrulamak için kullanılır. Küme durumu hatası varsa veya hatasına yürütme sırasında beklenmeyen bir özel durum meydana gelirse, Chaos önce sonraki durumu-küme biraz zaman recuperate sağlamak için onay - 30 dakika bekler.
* **Bağlam**: (String, string) koleksiyonunu anahtar-değer çiftleri girin. Harita Chaos çalıştırma hakkında bilgi kaydetmek için kullanılabilir. 100'den fazla çiftleri olamaz ve (anahtar veya değer) her bir dizenin en fazla 4095 karakter uzunluğunda olabilir. Bu harita, isteğe bağlı olarak belirli bir işlemle ilgili bağlam depolamak için çalıştırması Chaos başlatıcı tarafından ayarlanır.
* **Birden**: Bu filtre, hedef Chaos hataları yalnızca belirli düğüm türleri veya yalnızca belirli uygulama örnekleri için kullanılabilir. Birden kullanılmıyorsa, tüm küme varlıklar Chaos hataları. Birden kullanılıyorsa Chaos birden belirtimi karşılayan varlıklar hataları. Chaostargetfilter'daki Nodetypeınclusionlist ve Applicationınclusionlist'in yalnızca birleşim semantiği sağlar. Diğer bir deyişle, kesişim chaostargetfilter'daki Nodetypeınclusionlist ve Applicationınclusionlist'in belirtmek mümkün değildir. Örneğin, "Bu uygulama yalnızca söz konusu düğüm türünde olduğunda hata." belirlemek mümkün değil Bir varlık chaostargetfilter'daki Nodetypeınclusionlist veya Applicationınclusionlist'in dahil sonra bu varlık birden kullanarak tutulamaz. ApplicationX Applicationınclusionlist'in görünmez olsa bile, bunu chaostargetfilter'daki Nodetypeınclusionlist içinde bulunan nodeTypeY düğümünde olması gerektiğinden bazı Chaos yinelemede applicationX hatalı. ArgumentException hem chaostargetfilter'daki Nodetypeınclusionlist ve Applicationınclusionlist'in null veya boş ise oluşturulur.
    * **Chaostargetfilter'daki Nodetypeınclusionlist**: Kaos hataları dahil etmek için düğüm türleri listesi. Tüm tür hataları (düğümü yeniden başlatın, codepackage yeniden, çoğaltmayı kaldırmak, çoğaltmayı yeniden başlatın, birincil taşıma ve ikincil Taşı) bu düğüm türü için düğümleri etkinleştirilir. Bir nodetype (NodeTypeX diyelim) chaostargetfilter'daki Nodetypeınclusionlist görünmüyor sonra düğüm düzeyi hataları (gibi NodeRestart) hiçbir zaman NodeTypeX düğümleri için etkinleştirilecek ancak kod paketi ve çoğaltma hataları hala etkinleştirilebilir NodeTypeX için de bir uygulama bildirimi Applicationınclusionlist'in NodeTypeX düğümde olur. Bu sayıyı artırmak için bu listede, en fazla 100 düğüm tipi adları eklenebilir, yapılandırma yükseltme MaxNumberOfNodeTypesInChaosTargetFilter yapılandırma için gereklidir.
    * **Applicationınclusionlist'in**: Kaos hataları dahil etmek için URI uygulama listesi. Bu uygulamaların hizmetlerine ait tüm çoğaltmaları tfs'deki Chaos ile çoğaltma hataları (yeniden başlatma çoğaltma, çoğaltma Kaldır, taşıma birincil ve taşıma ikincil). Kod paketi çoğaltmaları bu uygulamaların yalnızca barındırıyorsa chaos bir kod paketi yeniden başlatılabilir. Bir uygulama bu listede görünmüyorsa, chaostargetfilter'daki Nodetypeınclusionlist içinde bulunan bir düğüm türü, bir düğüm üzerinde uygulama sona ererse, yine de bazı Chaos yinelemede hatalı. Yerleştirme kısıtlamaları ve applicationX aracılığıyla nodeTypeY applicationX bağlıdır, ancak eksik Applicationınclusionlist'in ve nodeTypeY eksik chaostargetfilter'daki Nodetypeınclusionlist sonra applicationX hiçbir zaman hatayla kapatılacak. En fazla 1000 uygulama adları bu sayıyı artırmak için bu listede, eklenebilir, yapılandırma yükseltme MaxNumberOfApplicationsInChaosTargetFilter yapılandırma için gereklidir.

## <a name="how-to-run-chaos"></a>Kaos çalıştırmayı öğrenin

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Fabric;

using System.Diagnostics;
using System.Fabric.Chaos.DataStructures;

class Program
{
    private class ChaosEventComparer : IEqualityComparer<ChaosEvent>
    {
        public bool Equals(ChaosEvent x, ChaosEvent y)
        {
            return x.TimeStampUtc.Equals(y.TimeStampUtc);
        }
        public int GetHashCode(ChaosEvent obj)
        {
            return obj.TimeStampUtc.GetHashCode();
        }
    }

    static void Main(string[] args)
    {
        var clusterConnectionString = "localhost:19000";
        using (var client = new FabricClient(clusterConnectionString))
        {
            var startTimeUtc = DateTime.UtcNow;

            // The maximum amount of time to wait for all cluster entities to become stable and healthy. 
            // Chaos executes in iterations and at the start of each iteration it validates the health of cluster
            // entities. 
            // During validation if a cluster entity is not stable and healthy within
            // MaxClusterStabilizationTimeoutInSeconds, Chaos generates a validation failed event.
            var maxClusterStabilizationTimeout = TimeSpan.FromSeconds(30.0);

            var timeToRun = TimeSpan.FromMinutes(60.0);

            // MaxConcurrentFaults is the maximum number of concurrent faults induced per iteration. 
            // Chaos executes in iterations and two consecutive iterations are separated by a validation phase.
            // The higher the concurrency, the more aggressive the injection of faults -- inducing more complex
            // series of states to uncover bugs.
            // The recommendation is to start with a value of 2 or 3 and to exercise caution while moving up.
            var maxConcurrentFaults = 3;

            // Describes a map, which is a collection of (string, string) type key-value pairs. The map can be
            // used to record information about the Chaos run. There cannot be more than 100 such pairs and
            // each string (key or value) can be at most 4095 characters long.
            // This map is set by the starter of the Chaos run to optionally store the context about the specific run.
            var startContext = new Dictionary<string, string>{{"ReasonForStart", "Testing"}};

            // Time-separation (in seconds) between two consecutive iterations of Chaos. The larger the value, the
            // lower the fault injection rate.
            var waitTimeBetweenIterations = TimeSpan.FromSeconds(10);

            // Wait time (in seconds) between consecutive faults within a single iteration.
            // The larger the value, the lower the overlapping between faults and the simpler the sequence of
            // state transitions that the cluster goes through. 
            // The recommendation is to start with a value between 1 and 5 and exercise caution while moving up.
            var waitTimeBetweenFaults = TimeSpan.Zero;

            // Passed-in cluster health policy is used to validate health of the cluster in between Chaos iterations. 
            var clusterHealthPolicy = new ClusterHealthPolicy
            {
                ConsiderWarningAsError = false,
                MaxPercentUnhealthyApplications = 100,
                MaxPercentUnhealthyNodes = 100
            };

            // All types of faults, restart node, restart code package, restart replica, move primary replica,
            // and move secondary replica will happen for nodes of type 'FrontEndType'
            var nodetypeInclusionList = new List<string> { "FrontEndType"};

            // In addition to the faults included by nodetypeInclusionList,
            // restart code package, restart replica, move primary replica, move secondary replica faults will
            // happen for 'fabric:/TestApp2' even if a replica or code package from 'fabric:/TestApp2' is residing
            // on a node which is not of type included in nodeypeInclusionList.
            var applicationInclusionList = new List<string> { "fabric:/TestApp2" };

            // List of cluster entities to target for Chaos faults.
            var chaosTargetFilter = new ChaosTargetFilter
            {
                NodeTypeInclusionList = nodetypeInclusionList,
                ApplicationInclusionList = applicationInclusionList
            };

            var parameters = new ChaosParameters(
                maxClusterStabilizationTimeout,
                maxConcurrentFaults,
                true, /* EnableMoveReplicaFault */
                timeToRun,
                startContext,
                waitTimeBetweenIterations,
                waitTimeBetweenFaults,
                clusterHealthPolicy) {ChaosTargetFilter = chaosTargetFilter};

            try
            {
                client.TestManager.StartChaosAsync(parameters).GetAwaiter().GetResult();
            }
            catch (FabricChaosAlreadyRunningException)
            {
                Console.WriteLine("An instance of Chaos is already running in the cluster.");
            }

            var filter = new ChaosReportFilter(startTimeUtc, DateTime.MaxValue);

            var eventSet = new HashSet<ChaosEvent>(new ChaosEventComparer());

            string continuationToken = null;

            while (true)
            {
                ChaosReport report;
                try
                {
                    report = string.IsNullOrEmpty(continuationToken)
                        ? client.TestManager.GetChaosReportAsync(filter).GetAwaiter().GetResult()
                        : client.TestManager.GetChaosReportAsync(continuationToken).GetAwaiter().GetResult();
                }
                catch (Exception e)
                {
                    if (e is FabricTransientException)
                    {
                        Console.WriteLine("A transient exception happened: '{0}'", e);
                    }
                    else if(e is TimeoutException)
                    {
                        Console.WriteLine("A timeout exception happened: '{0}'", e);
                    }
                    else
                    {
                        throw;
                    }

                    Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
                    continue;
                }

                continuationToken = report.ContinuationToken;

                foreach (var chaosEvent in report.History)
                {
                    if (eventSet.Add(chaosEvent))
                    {
                        Console.WriteLine(chaosEvent);
                    }
                }

                // When Chaos stops, a StoppedEvent is created.
                // If a StoppedEvent is found, exit the loop.
                var lastEvent = report.History.LastOrDefault();

                if (lastEvent is StoppedEvent)
                {
                    break;
                }

                Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
            }
        }
    }
}
```

```powershell
$clusterConnectionString = "localhost:19000"
$timeToRunMinute = 60

# The maximum amount of time to wait for all cluster entities to become stable and healthy.
# Chaos executes in iterations and at the start of each iteration it validates the health of cluster entities.
# During validation if a cluster entity is not stable and healthy within MaxClusterStabilizationTimeoutInSeconds,
# Chaos generates a validation failed event.
$maxClusterStabilizationTimeSecs = 30

# MaxConcurrentFaults is the maximum number of concurrent faults induced per iteration.
# Chaos executes in iterations and two consecutive iterations are separated by a validation phase.
# The higher the concurrency, the more aggressive the injection of faults -- inducing more complex series of
# states to uncover bugs.
# The recommendation is to start with a value of 2 or 3 and to exercise caution while moving up.
$maxConcurrentFaults = 3

# Time-separation (in seconds) between two consecutive iterations of Chaos. The larger the value, the lower the
# fault injection rate.
$waitTimeBetweenIterationsSec = 10

# Wait time (in seconds) between consecutive faults within a single iteration.
# The larger the value, the lower the overlapping between faults and the simpler the sequence of state
# transitions that the cluster goes through.
# The recommendation is to start with a value between 1 and 5 and exercise caution while moving up.
$waitTimeBetweenFaultsSec = 0

# Passed-in cluster health policy is used to validate health of the cluster in between Chaos iterations. 
$clusterHealthPolicy = new-object -TypeName System.Fabric.Health.ClusterHealthPolicy
$clusterHealthPolicy.MaxPercentUnhealthyNodes = 100
$clusterHealthPolicy.MaxPercentUnhealthyApplications = 100
$clusterHealthPolicy.ConsiderWarningAsError = $False

# Describes a map, which is a collection of (string, string) type key-value pairs. The map can be used to record
# information about the Chaos run.
# There cannot be more than 100 such pairs and each string (key or value) can be at most 4095 characters long.
# This map is set by the starter of the Chaos run to optionally store the context about the specific run.
$context = @{"ReasonForStart" = "Testing"}

#List of cluster entities to target for Chaos faults.
$chaosTargetFilter = new-object -TypeName System.Fabric.Chaos.DataStructures.ChaosTargetFilter
$chaosTargetFilter.NodeTypeInclusionList = new-object -TypeName "System.Collections.Generic.List[String]"

# All types of faults, restart node, restart code package, restart replica, move primary replica, and move
# secondary replica will happen for nodes of type 'FrontEndType'
$chaosTargetFilter.NodeTypeInclusionList.AddRange( [string[]]@("FrontEndType") )
$chaosTargetFilter.ApplicationInclusionList = new-object -TypeName "System.Collections.Generic.List[String]"

# In addition to the faults included by nodetypeInclusionList, 
# restart code package, restart replica, move primary replica, move secondary replica faults will happen for
# 'fabric:/TestApp2' even if a replica or code package from 'fabric:/TestApp2' is residing on a node which is
# not of type included in nodeypeInclusionList.
$chaosTargetFilter.ApplicationInclusionList.Add("fabric:/TestApp2")

Connect-ServiceFabricCluster $clusterConnectionString

$events = @{}
$now = [System.DateTime]::UtcNow

Start-ServiceFabricChaos -TimeToRunMinute $timeToRunMinute -MaxConcurrentFaults $maxConcurrentFaults -MaxClusterStabilizationTimeoutSec $maxClusterStabilizationTimeSecs -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ClusterHealthPolicy $clusterHealthPolicy -ChaosTargetFilter $chaosTargetFilter

while($true)
{
    $stopped = $false
    $report = Get-ServiceFabricChaosReport -StartTimeUtc $now -EndTimeUtc ([System.DateTime]::MaxValue)

    foreach ($e in $report.History) {

        if(-Not ($events.Contains($e.TimeStampUtc.Ticks)))
        {
            $events.Add($e.TimeStampUtc.Ticks, $e)
            if($e -is [System.Fabric.Chaos.DataStructures.ValidationFailedEvent])
            {
                Write-Host -BackgroundColor White -ForegroundColor Red $e
            }
            else
            {
                Write-Host $e
                # When Chaos stops, a StoppedEvent is created.
                # If a StoppedEvent is found, exit the loop.
                if($e -is [System.Fabric.Chaos.DataStructures.StoppedEvent])
                {
                    return
                }
            }
        }
    }

    Start-Sleep -Seconds 1
}
```
