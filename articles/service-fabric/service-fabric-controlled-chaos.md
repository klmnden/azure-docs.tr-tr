---
title: "Service Fabric kümeleri Chaos anlamına | Microsoft Docs"
description: "Hata ekleme ve küme analiz hizmeti API'leri Chaos kümede yönetmek için kullanma."
services: service-fabric
documentationcenter: .net
author: motanv
manager: anmola
editor: motanv
ms.assetid: 2bd13443-3478-4382-9a5a-1f6c6b32bfc9
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/05/2018
ms.author: motanv
ms.openlocfilehash: 81206257cb2c7157bbb1ffcf3a79ced7c896ef80
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>Service Fabric kümelerinde denetimli Chaos anlamına
Bulut altyapılarının doğası gereği güvenilir gibi büyük ölçekli dağıtılmış sistemler. Azure Service Fabric, güvenilir olmayan bir altyapının en üstünde güvenilir dağıtılmış hizmet yazmak geliştiricilerin sağlar. Güvenilir olmayan bir altyapının en üstünde güçlü dağıtılmış hizmet yazmak için geliştiriciler güvenilmez altyapının hataları nedeniyle karmaşık durumu geçişleri üzerinden giderek sırada hizmetlerini kararlılığını test etmek gerekir.

[Hata ekleme ve küme analiz hizmeti](https://docs.microsoft.com/azure/service-fabric/service-fabric-testability-overview) (hata analiz hizmeti olarak da bilinir) geliştiricilerin kendi hizmetlerini test etmek için hataları anlamına olanağı sağlar. Bunlar hedeflenen gibi hataları, benzetimli [bir bölümü yeniden](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps), en yaygın durumu geçişleri çalışma yardımcı olabilir. Ancak hedeflenen benzetimli hataları tanımına göre ağırlıklı ve böylece kaçırabilir gösteren yedekleme yalnızca sabit tahmin, uzun ve karmaşık sırası durumu geçişleri içinde hataları. Bir taraflı olmayan test etmek için Chaos kullanabilirsiniz.

Chaos uzun süreler boyunca küme boyunca düzenli, araya eklemeli hataları (normal ve durunda) benzetimini yapar. Service Fabric API çağrıları birtakım normal bir hata oluşur, çünkü bu bir çoğaltma üzerinde açık tarafından izlenen bir kapatma Örneğin, yeniden başlatma çoğaltma hatası normal hata. Çoğaltma kaldırmak için birincil çoğaltma ve taşıma ikincil çoğaltma Chaos tarafından kullanılabilecek diğer normal hatalarıdır taşıyın. Düğüm ve restrat kod paketi yeniden gibi işlem çıkar durunda hatalarıdır. 

Hızı ve hataları türü ile Chaos yapılandırdıktan sonra C#, Powershell veya REST API hatalarını küme ve hizmetlerinizi oluşturma başlatmak için aracılığıyla Chaos başlatabilirsiniz. Sonra hangi Chaos durdurur (örneğin, bir saat), belirtilen bir süre boyunca çalıştırmak için Chaos otomatik olarak yapılandırabilir veya (C#, Powershell veya REST) herhangi bir zamanda durdurmak için StopChaos API'si çağırabilirsiniz.

> [!NOTE]
> Mevcut haliyle, dış hataları olmaması durumunda bir çekirdek kayıp veya veri kaybı hiçbir zaman oluştuğunu anlamına gelir, yalnızca güvenli hataları Chaos uygulanmasını.
>

Chaos çalışırken, bu anda çalışma durumunu yakalama farklı olayları üretir. Örneğin, bir ExecutingFaultsEvent Chaos bu yinelemede yürütmek için karar verdiği tüm hataları içerir. Bir ValidationFailedEvent küme doğrulama sırasında bulundu bir doğrulama hatası (sistem durumu veya kararlılık sorunları) ayrıntılarını içerir. GetChaosReport Chaos çalıştırır rapor almak için API (C#, Powershell veya REST) çağırabilirsiniz. Bu olaylar kalıcı bir [güvenilir sözlük](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-reliable-services-reliable-collections), iki yapılandırmaları tarafından dikte kesilmesi ilkesi vardır: **MaxStoredChaosEventCount** (varsayılan değer olan 25000) ve **StoredActionCleanupIntervalInSeconds** (varsayılan değer olan 3600). Every *StoredActionCleanupIntervalInSeconds* Chaos checks and all but the most recent *MaxStoredChaosEventCount* events, are purged from the reliable dictionary.

## <a name="faults-induced-in-chaos"></a>İçinde Chaos kopyaladığınızda hataları
Chaos hataları tüm Service Fabric kümesi oluşturur ve ay veya yıl birkaç saat görülür hataları sıkıştırır. Yüksek hata oranı araya eklemeli hataları birleşimi, aksi takdirde atlanabilir köşe durumlarda bulur. Bu alıştırmada karmaşık dünyada hizmet kod kalitesi, önemli bir iyileştirme neden olmaktadır.

Aşağıdaki kategorilerden hataları Chaos uygulanmasını:

* Bir düğümü yeniden başlatın
* Dağıtılmış kod paketi yeniden başlatın
* Bir yineleme kaldırma
* Bir çoğaltmayı yeniden başlatın
* Bir birincil çoğaltmayı (yapılandırılabilir) taşıma
* Bir ikincil çoğaltma (yapılandırılabilir) taşıma

Chaos içinde birden çok kez çalışır. Her yineleme hataları ve belirtilen süre için küme doğrulama oluşur. Başarılı olması doğrulama ve kararlı küme için harcanan süre yapılandırabilirsiniz. Küme doğrulama hata bulunursa, Chaos oluşturur ve ValidationFailedEvent UTC zaman damgası ve hata ayrıntıları ile devam ettirir. Örneğin, bir saat için en fazla üç eşzamanlı hataları ile çalışacak şekilde ayarlanmış karmaşık dünyada örneği göz önünde bulundurun. Chaos üç hataları uygulanmasını ve küme sistem doğrular. Önceki tekrarlanan açıkça StopChaosAsync API'si aracılığıyla durdurulmuş veya bir saatlik olana kadar adım geçirir. Kümenin her yinelemede bozulursa (diğer bir deyişle, değil Sabitle veya geçilen MaxClusterStabilizationTimeout içinde sağlıklı olmaz), Chaos bir ValidationFailedEvent oluşturur. Bu olay, bir şeyler yanlış geçti ve daha fazla araştırma gerekebilir gösterir.

Chaos kopyaladığınızda hangi hataları almak için GetChaosReport API (powershell, C# veya REST) kullanabilirsiniz. API geçilen devamlılık belirteci veya geçen zaman aralığı göre Chaos raporun sonraki kesimini alır. Chaos rapor sonraki kesimin almak için ContinuationToken ya da belirtebilirsiniz veya StartTimeUtc ve EndTimeUtc aracılığıyla zaman aralığını belirtebilirsiniz, ancak aynı çağrısında ContinuationToken ve zaman aralığı belirtemezsiniz. 100'den fazla Chaos olayları olduğunda Chaos rapor, en fazla 100 Chaos olay kesimi içerdiği segmentlerinde döndürülür.

## <a name="important-configuration-options"></a>Önemli yapılandırma seçenekleri
* **TimeToRun**: toplam saat Chaos çalıştıran ile başarılı tamamlanmadan önce. StopChaos API aracılığıyla TimeToRun boyunca çalıştıktan önce Chaos durdurabilirsiniz.

* **MaxClusterStabilizationTimeout**: bir ValidationFailedEvent oluşturan önce sağlıklı duruma kümeye için beklenecek en fazla süreyi. Bu bekleme, Kurtarma sırasında küme üzerinde yük azaltmaktır. Gerçekleştirilen denetimleri şunlardır:
  * Küme durumu Tamam ise
  * Hizmet durumu Tamam ise
  * Hedef çoğaltma kümesi boyutu, hizmet bölümü elde edilir
  * Hiçbir Inbuild çoğaltmaların var
* **MaxConcurrentFaults**: her yinelemede kopyaladığınızda eşzamanlı hatalarının sayısı. Daha yüksek sayı, daha agresif karmaşası ve yük devretme ve küme geçtiği durumu geçişi birleşimleri ayrıca daha karmaşık. 

> [!NOTE]
> Ne olursa olsun nasıl yüksek bir değer *MaxConcurrentFaults* varsa, Chaos - dış hataları - olmaması durumunda çekirdek kayıp veya veri kaybı yoktur güvence altına alır.
>

* **EnableMoveReplicaFaults**: etkinleştirir veya taşımak birincil veya ikincil çoğaltmaları neden hataları devre dışı bırakır. Bu hataları varsayılan olarak etkinleştirilir.
* **WaitTimeBetweenIterations**: yineleme arasında beklenecek süre miktarı. Diğer bir deyişle, süre Chaos gidiş hataları ve küme durumunu karşılık gelen doğrulanması tamamlandı yürütüldükten sonra duraklatılır. Daha yüksek alt ortalama hata ekleme oranı değerdir.
* **WaitTimeBetweenFaults**: tek bir yineleme iki ardışık hataları arasında beklenecek süre. Daha yüksek değeri, alt eşzamanlılığı (veya arasındaki çakışmayı) hataları.
* **ClusterHealthPolicy**: küme sistem durumu ilkesi Chaos yineleme Between küme durumunu doğrulamak için kullanılır. Küme durumu hata veya hataya yürütme sırasında beklenmeyen bir özel durum oluşursa, Chaos önce sonraki sistem durumu kümeyi biraz zaman recuperate sağlamak için denetimi - 30 dakika bekler.
* **Bağlam**: (dize, dize) koleksiyonunu anahtar-değer çiftlerini yazın. Harita Chaos işlemle ilgili bilgileri kaydetmek için kullanılabilir. Bu tür çiftleri 100'den fazla olamaz ve her bir dize (anahtar veya değer) en fazla 4095 karakterden uzun olamaz. Bu haritada bağlam belirli çalışma hakkında isteğe bağlı olarak depolamak için Çalıştır karmaşası başlatıcı tarafından ayarlanır.
* **ChaosTargetFilter**: Bu filtre hedef Chaos hataları yalnızca belirli düğüm türleri veya yalnızca belirli uygulama örnekleri için kullanılabilir. ChaosTargetFilter kullanılmazsa, tüm küme varlıklar Chaos hataları. ChaosTargetFilter kullanılırsa, Chaos ChaosTargetFilter belirtimi karşılayan varlıklar hataları. NodeTypeInclusionList ve ApplicationInclusionList yalnızca birleşim anlamsallarını izin verir. Diğer bir deyişle, NodeTypeInclusionList ve ApplicationInclusionList kesişimini belirlemek mümkün değil. Örneğin, "Bu uygulama yalnızca bu düğüm türünde olduğunda hata." belirlemek mümkün değil Bir varlık NodeTypeInclusionList veya ApplicationInclusionList dahil sonra o varlık ChaosTargetFilter kullanarak tutulamaz. ApplicationX içinde ApplicationInclusionList bile görünmüyorsa, NodeTypeInclusionList dahil nodeTypeY bir düğümü üzerinde olmasını olur çünkü bazı Chaos yinelemede applicationX hatalı. Hem NodeTypeInclusionList hem de ApplicationInclusionList null veya boş ise, ArgumentException atılır.
    * **NodeTypeInclusionList**: Chaos hataları eklenecek düğüm türleri listesi. Hataları tüm türleri (düğümü yeniden başlatın, codepackage yeniden başlatın, çoğaltmayı kaldırmak, çoğaltmayı yeniden başlatın, birincil taşımak ve ikincil taşıma) bu düğüm türleri düğümleri için etkinleştirilir. Bir nodetype (NodeTypeX söyleyin) NodeTypeInclusionList içinde görünmez sonra düğüm düzeyi hataları (gibi NodeRestart) NodeTypeX düğümleri için hiçbir zaman etkinleştirilir, ancak kod paketi ve çoğaltma hataları hala etkinleştirilebilir NodeTypeX için bir uygulamada varsa ApplicationInclusionList NodeTypeX düğümde olur. En fazla 100 düğüm türü adı bu sayıyı artırmak için bu listede, eklenebilir, MaxNumberOfNodeTypesInChaosTargetFilter yapılandırması için gerekli yapılandırma yükseltmedir.
    * **ApplicationInclusionList**: Chaos hataları içerecek şekilde URI'ler uygulama listesi. Bu uygulamalar Hizmetleri'ne ait tüm çoğaltmaları uygun Chaos tarafından Çoğaltma hataları (yeniden başlatma çoğaltma, Kaldır çoğaltma, taşıma birincil ve ikincil taşıma). Kod paketi yalnızca bu uygulamaları çoğaltmalarının barındırıyorsa chaos kod paketi yeniden başlatılabilir. Bir uygulama bu listede görünmüyorsa, uygulama NodeTypeInclusionList incuded bir düğüm türü, bir düğümde ererse, hala bazı Chaos yinelemede hatalı. Yerleştirme kısıtlamaları ve applicationX aracılığıyla nodeTypeY applicationX bağlıdır, ancak eksik ApplicationInclusionList ve nodeTypeY yok NodeTypeInclusionList sonra applicationX hiçbir zaman hatayla kapatılacak. En fazla 1000 uygulama adları bu sayıyı artırmak için bu listede, eklenebilir, MaxNumberOfApplicationsInChaosTargetFilter yapılandırması için gerekli yapılandırma yükseltmedir.

## <a name="how-to-run-chaos"></a>Chaos çalıştırma

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
            // Chaos executes in iterations and at the start of each iteration it validates the health of cluster entities. 
            // During validation if a cluster entity is not stable and healthy within MaxClusterStabilizationTimeoutInSeconds, Chaos generates a validation failed event.
            var maxClusterStabilizationTimeout = TimeSpan.FromSeconds(30.0);

            var timeToRun = TimeSpan.FromMinutes(60.0);

            // MaxConcurrentFaults is the maximum number of concurrent faults induced per iteration. 
            // Chaos executes in iterations and two consecutive iterations are separated by a validation phase. 
            // The higher the concurrency, the more aggressive the injection of faults -- inducing more complex series of states to uncover bugs. 
            // The recommendation is to start with a value of 2 or 3 and to exercise caution while moving up.
            var maxConcurrentFaults = 3;

            // Describes a map, which is a collection of (string, string) type key-value pairs. The map can be used to record information about
            // the Chaos run. There cannot be more than 100 such pairs and each string (key or value) can be at most 4095 characters long.
            // This map is set by the starter of the Chaos run to optionally store the context about the specific run.
            var startContext = new Dictionary<string, string>{{"ReasonForStart", "Testing"}};

            // Time-separation (in seconds) between two consecutive iterations of Chaos. The larger the value, the lower the fault injection rate.
            var waitTimeBetweenIterations = TimeSpan.FromSeconds(10);

            // Wait time (in seconds) between consecutive faults within a single iteration. 
            // The larger the value, the lower the overlapping between faults and the simpler the sequence of state transitions that the cluster goes through. 
            // The recommendation is to start with a value between 1 and 5 and exercise caution while moving up.
            var waitTimeBetweenFaults = TimeSpan.Zero;

            // Passed-in cluster health policy is used to validate health of the cluster in between Chaos iterations. 
            var clusterHealthPolicy = new ClusterHealthPolicy
            {
                ConsiderWarningAsError = false,
                MaxPercentUnhealthyApplications = 100,
                MaxPercentUnhealthyNodes = 100
            };

            // All types of faults, restart node, restart code package, restart replica, move primary replica, and move secondary replica will happen
            // for nodes of type 'FrontEndType'
            var nodetypeInclusionList = new List<string> { "FrontEndType"};

            // In addition to the faults included by nodetypeInclusionList, 
            // restart code package, restart replica, move primary replica, move secondary replica faults will happen for 'fabric:/TestApp2'
            // even if a replica or code package from 'fabric:/TestApp2' is residing on a node which is not of type included in nodeypeInclusionList.
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
# During validation if a cluster entity is not stable and healthy within MaxClusterStabilizationTimeoutInSeconds, Chaos generates a validation failed event.
$maxClusterStabilizationTimeSecs = 30

# MaxConcurrentFaults is the maximum number of concurrent faults induced per iteration. 
# Chaos executes in iterations and two consecutive iterations are separated by a validation phase. 
# The higher the concurrency, the more aggressive the injection of faults -- inducing more complex series of states to uncover bugs. 
# The recommendation is to start with a value of 2 or 3 and to exercise caution while moving up.
$maxConcurrentFaults = 3

# Time-separation (in seconds) between two consecutive iterations of Chaos. The larger the value, the lower the fault injection rate.
$waitTimeBetweenIterationsSec = 10

# Wait time (in seconds) between consecutive faults within a single iteration. 
# The larger the value, the lower the overlapping between faults and the simpler the sequence of state transitions that the cluster goes through. 
# The recommendation is to start with a value between 1 and 5 and exercise caution while moving up.
$waitTimeBetweenFaultsSec = 0

# Passed-in cluster health policy is used to validate health of the cluster in between Chaos iterations. 
$clusterHealthPolicy = new-object -TypeName System.Fabric.Health.ClusterHealthPolicy
$clusterHealthPolicy.MaxPercentUnhealthyNodes = 100
$clusterHealthPolicy.MaxPercentUnhealthyApplications = 100
$clusterHealthPolicy.ConsiderWarningAsError = $False

# Describes a map, which is a collection of (string, string) type key-value pairs. The map can be used to record information about
# the Chaos run. There cannot be more than 100 such pairs and each string (key or value) can be at most 4095 characters long.
# This map is set by the starter of the Chaos run to optionally store the context about the specific run.
$context = @{"ReasonForStart" = "Testing"}

#List of cluster entities to target for Chaos faults.
$chaosTargetFilter = new-object -TypeName System.Fabric.Chaos.DataStructures.ChaosTargetFilter
$chaosTargetFilter.NodeTypeInclusionList = new-object -TypeName "System.Collections.Generic.List[String]"

# All types of faults, restart node, restart code package, restart replica, move primary replica, and move secondary replica will happen
# for nodes of type 'FrontEndType'
$chaosTargetFilter.NodeTypeInclusionList.AddRange( [string[]]@("FrontEndType") )
$chaosTargetFilter.ApplicationInclusionList = new-object -TypeName "System.Collections.Generic.List[String]"

# In addition to the faults included by nodetypeInclusionList, 
# restart code package, restart replica, move primary replica, move secondary replica faults will happen for 'fabric:/TestApp2'
# even if a replica or code package from 'fabric:/TestApp2' is residing on a node which is not of type included in nodeypeInclusionList.
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
