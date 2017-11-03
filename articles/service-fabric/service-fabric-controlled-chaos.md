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
ms.date: 08/09/2017
ms.author: motanv
ms.openlocfilehash: 3b3b93bc9ec5ecdcfc289e5b62e84de6aa4172ed
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>Service Fabric kümelerinde denetimli Chaos anlamına
Bulut altyapılarının doğası gereği güvenilir gibi büyük ölçekli dağıtılmış sistemler. Azure Service Fabric, güvenilir olmayan bir altyapının en üstünde güvenilir dağıtılmış hizmet yazmak geliştiricilerin sağlar. Güvenilir olmayan bir altyapının en üstünde güçlü dağıtılmış hizmet yazmak için geliştiriciler güvenilmez altyapının hataları nedeniyle karmaşık durumu geçişleri üzerinden giderek sırada hizmetlerini kararlılığını test etmek gerekir.

[Hata ekleme ve küme analiz hizmeti](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-testability-overview) (hata analiz hizmeti olarak da bilinir) geliştiricilerin kendi hizmetlerini test etmek için hataları anlamına olanağı sağlar. Bunlar hedeflenen gibi hataları, benzetimli [bir bölümü yeniden](https://docs.microsoft.com/en-us/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps), en yaygın durumu geçişleri çalışma yardımcı olabilir. Ancak hedeflenen benzetimli hataları tanımına göre ağırlıklı ve böylece kaçırabilir gösteren yedekleme yalnızca sabit tahmin, uzun ve karmaşık sırası durumu geçişleri içinde hataları. Bir taraflı olmayan test etmek için Chaos kullanabilirsiniz.

Chaos uzun süreler boyunca küme boyunca düzenli, araya eklemeli hataları (normal ve durunda) benzetimini yapar. Hızı ve hataları türü ile Chaos yapılandırdıktan sonra C# veya Powershell API hatalarını küme ve hizmetlerinizi oluşturma başlatmak için aracılığıyla Chaos başlatabilirsiniz. Sonra hangi Chaos durdurur (örneğin, bir saat), belirtilen bir süre boyunca çalıştırmak için Chaos otomatik olarak yapılandırabilir veya (C# veya Powershell) herhangi bir zamanda durdurmak için StopChaos API'si çağırabilirsiniz.

> [!NOTE]
> Mevcut haliyle, dış hataları olmaması durumunda bir çekirdek kayıp veya veri kaybı hiçbir zaman oluştuğunu anlamına gelir, yalnızca güvenli hataları Chaos uygulanmasını.
>

Chaos çalışırken, bu anda çalışma durumunu yakalama farklı olayları üretir. Örneğin, bir ExecutingFaultsEvent Chaos bu yinelemede yürütmek için karar verdiği tüm hataları içerir. Bir ValidationFailedEvent küme doğrulama sırasında bulundu bir doğrulama hatası (sistem durumu veya kararlılık sorunları) ayrıntılarını içerir. GetChaosReport Chaos çalıştırır rapor almak için API (C# veya Powershell) çağırabilirsiniz. Bu olaylar kalıcı bir [güvenilir sözlük](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-reliable-services-reliable-collections), iki yapılandırmaları tarafından dikte kesilmesi ilkesi vardır: **MaxStoredChaosEventCount** (varsayılan değer olan 25000) ve **StoredActionCleanupIntervalInSeconds** (varsayılan değer olan 3600). Her *StoredActionCleanupIntervalInSeconds* Chaos denetler ve tüm ancak en son *MaxStoredChaosEventCount* olayları, güvenilir sözlükten temizlendi.

## <a name="faults-induced-in-chaos"></a>İçinde Chaos kopyaladığınızda hataları
Chaos hataları tüm Service Fabric kümesi oluşturur ve ay veya yıl birkaç saat görülür hataları sıkıştırır. Yüksek hata oranı araya eklemeli hataları birleşimi, aksi takdirde atlanabilir köşe durumlarda bulur. Bu alıştırmada karmaşık dünyada hizmet kod kalitesi, önemli bir iyileştirme neden olmaktadır.

Aşağıdaki kategorilerden hataları Chaos uygulanmasını:

* Bir düğümü yeniden başlatın
* Dağıtılmış kod paketi yeniden başlatın
* Bir yineleme kaldırma
* Bir çoğaltmayı yeniden başlatın
* Bir birincil çoğaltmayı (yapılandırılabilir) taşıma
* Bir ikincil çoğaltma (yapılandırılabilir) taşıma

Chaos içinde birden çok kez çalışır. Her yineleme hataları ve belirtilen süre için küme doğrulama oluşur. Başarılı olması doğrulama ve kararlı küme için harcanan süre yapılandırabilirsiniz. Küme doğrulama hata bulunursa, Chaos oluşturur ve ValidationFailedEvent UTC zaman damgası ve hata ayrıntıları ile devam ettirir. Örneğin, bir saat için en fazla üç eşzamanlı hataları ile çalışacak şekilde ayarlanmış karmaşık dünyada örneği göz önünde bulundurun. Chaos üç hataları uygulanmasını ve küme sistem doğrular. Önceki tekrarlanan açıkça StopChaosAsync API'si aracılığıyla durdurulmuş veya bir saatlik olana kadar adım geçirir. Kümenin her yinelemede bozulursa (diğer bir deyişle, onu içinde geçirilen MaxClusterStabilizationTimeout Sabitle değil), Chaos bir ValidationFailedEvent oluşturur. Bu olay, bir şeyler yanlış geçti ve daha fazla araştırma gerekebilir gösterir.

Chaos kopyaladığınızda hangi hataları almak için GetChaosReport API (powershell veya C#) kullanabilirsiniz. API geçilen devamlılık belirteci veya geçen zaman aralığı göre Chaos raporun sonraki kesimini alır. Chaos rapor sonraki kesimin almak için ContinuationToken ya da belirtebilirsiniz veya StartTimeUtc ve EndTimeUtc aracılığıyla zaman aralığını belirtebilirsiniz, ancak aynı çağrısında ContinuationToken ve zaman aralığı belirtemezsiniz. 100'den fazla Chaos olayları olduğunda Chaos rapor, en fazla 100 Chaos olay kesimi içerdiği segmentlerinde döndürülür.

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

* **EnableMoveReplicaFaults**: etkinleştirir veya taşımak birincil veya ikincil çoğaltmaları neden hataları devre dışı bırakır. Bu hataları varsayılan olarak devre dışıdır.
* **WaitTimeBetweenIterations**: yineleme arasında beklenecek süre miktarı. Diğer bir deyişle, süre Chaos gidiş hataları ve küme durumunu karşılık gelen doğrulanması tamamlandı yürütüldükten sonra duraklatılır. Daha yüksek alt ortalama hata ekleme oranı değerdir.
* **WaitTimeBetweenFaults**: tek bir yineleme iki ardışık hataları arasında beklenecek süre. Daha yüksek değeri, alt eşzamanlılığı (veya arasındaki çakışmayı) hataları.
* **ClusterHealthPolicy**: küme sistem durumu ilkesi Chaos yineleme Between küme durumunu doğrulamak için kullanılır. Küme durumu hata veya hataya yürütme sırasında beklenmeyen bir özel durum oluşursa, Chaos önce sonraki sistem durumu kümeyi biraz zaman recuperate sağlamak için denetimi - 30 dakika bekler.
* **Bağlam**: (dize, dize) koleksiyonunu anahtar-değer çiftlerini yazın. Harita Chaos işlemle ilgili bilgileri kaydetmek için kullanılabilir. Bu tür çiftleri 100'den fazla olamaz ve her bir dize (anahtar veya değer) en fazla 4095 karakterden uzun olamaz. Bu haritada bağlam belirli çalışma hakkında isteğe bağlı olarak depolamak için Çalıştır karmaşası başlatıcı tarafından ayarlanır.

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
            var stabilizationTimeout = TimeSpan.FromSeconds(30.0);
            var timeToRun = TimeSpan.FromMinutes(60.0);
            var maxConcurrentFaults = 3;

            var parameters = new ChaosParameters(
                stabilizationTimeout,
                maxConcurrentFaults,
                true, /* EnableMoveReplicaFault */
                timeToRun);

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

            while (true)
            {
                var report = client.TestManager.GetChaosReportAsync(filter).GetAwaiter().GetResult();

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
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

$events = @{}
$now = [System.DateTime]::UtcNow

Start-ServiceFabricChaos -TimeToRunMinute $timeToRun -MaxConcurrentFaults $concurrentFaults -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec

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
                if($e -is [System.Fabric.Chaos.DataStructures.StoppedEvent])
                {
                    $stopped = $true
                }

                Write-Host $e
            }
        }
    }

    if($stopped -eq $true)
    {
        break
    }

    Start-Sleep -Seconds 1
}

Stop-ServiceFabricChaos
```
