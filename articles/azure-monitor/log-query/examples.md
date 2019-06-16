---
title: Azure İzleyici günlük sorgu örnekleri | Microsoft Docs
description: Azure Kusto sorgu dilini kullanarak izleme günlüğünü sorgularda örnekleri.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/03/2018
ms.author: bwren
ms.openlocfilehash: 2c35bc4026c81cbc8b95225e688a3922bc320554
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60759920"
---
# <a name="azure-monitor-log-query-examples"></a>Azure İzleyici günlük sorgu örnekleri
Bu makalede, çeşitli örneklerini içerir [sorguları](log-query-overview.md) kullanarak [Kusto sorgu dili](/azure/kusto/query/) Azure İzleyici'den farklı türde günlük verileri alınamadı. Farklı yöntemleri, birleştirmek ve bu örnekleri kendi gereksinimleriniz için kullanabileceğiniz farklı stratejiler tanımlamak için kullanabileceğiniz şekilde, verileri analiz etmek için kullanılır.  

Bkz: [Kusto dil başvurusu](https://docs.microsoft.com/azure/kusto/query/) Bu örneklerde kullanılan farklı anahtar sözcükler hakkında ayrıntılı bilgi için. Git aracılığıyla bir [sorguları oluşturma Ders](get-started-queries.md) Azure İzleyici yeniyseniz.

## <a name="events"></a>Events

### <a name="search-application-level-events-described-as-cryptographic"></a>Uygulama düzeyinde olaylar "Şifreli" açıklanan arama
Bu örnekte arama **olayları** olduğu kayıtlar için tablo **EventLog** olduğu _uygulama_ ve **RenderedDescription** içerir _şifreleme_. Son 24 saat kayıtları içerir.

```Kusto
Event
| where EventLog == "Application" 
| where TimeGenerated > ago(24h) 
| where RenderedDescription == "cryptographic"
```

### <a name="search-events-related-to-unmarshaling"></a>Unmarshaling için ilgili arama olayları
Arama tabloları **olay** ve **SecurityEvents** kayıtlarını bu Bahsetme _unmarshaling_.

```Kusto
search in (Event, SecurityEvent) "unmarshaling"
```

## <a name="heartbeat"></a>Sinyal

### <a name="chart-a-week-over-week-view-of-the-number-of-computers-sending-data"></a>Grafik bir hafta üzerinden Hafta görünümünü veri gönderen bilgisayarların sayısı

Aşağıdaki örnek, her hafta sinyal gönderilen farklı bilgisayarların sayısını grafikleri.

```Kusto
Heartbeat
| where TimeGenerated >= startofweek(ago(21d))
| summarize dcount(Computer) by endofweek(TimeGenerated) | render barchart kind=default
```

### <a name="find-stale-computers"></a>Eski bilgisayarları Bul

Aşağıdaki örnekte, son gün içinde etkin ancak son bir saat içinde sinyal göndermediği bilgisayarları bulur.

```Kusto
Heartbeat
| where TimeGenerated > ago(1d)
| summarize LastHeartbeat = max(TimeGenerated) by Computer
| where isnotempty(Computer)
| where LastHeartbeat < ago(1h)
```

### <a name="get-the-latest-heartbeat-record-per-computer-ip"></a>Bilgisayar IP başına en son sinyal kayıt Al

Bu örnekte, her bilgisayar IP adresi için son sinyal kaydı döndürür.
```Kusto
Heartbeat
| summarize arg_max(TimeGenerated, *) by ComputerIP
```

### <a name="match-protected-status-records-with-heartbeat-records"></a>Sinyal kayıtları ile korunan durum kayıt eşleşmesi

Bu örnek, ilgili koruma durumu ve sinyal kayıtlarını, hem bilgisayar hem de saat eşleşen bulur.
Not saat alanı için en yakın dakikaya yuvarlanır. Bunu yapmak için çalışma zamanı bin hesaplama kullandık: `round_time=bin(TimeGenerated, 1m)`.

```Kusto
let protection_data = ProtectionStatus
    | project Computer, DetectionId, round_time=bin(TimeGenerated, 1m);
let heartbeat_data = Heartbeat
    | project Computer, Category, round_time=bin(TimeGenerated, 1m);
protection_data | join (heartbeat_data) on Computer, round_time
```

### <a name="server-availability-rate"></a>Sunucu kullanılabilirlik oranı

Sinyal kayıtlarını temel alarak sunucusu kullanılabilirlik oranını hesaplayın. Kullanılabilirlik, "saat başına en az 1 sinyal" olarak tanımlanır.
Bir sunucu kullanılabilir 98 100 saat ise, bu nedenle kullanılabilirlik %98 hızıdır.

```Kusto
let start_time=startofday(datetime("2018-03-01"));
let end_time=now();
Heartbeat
| where TimeGenerated > start_time and TimeGenerated < end_time
| summarize heartbeat_per_hour=count() by bin_at(TimeGenerated, 1h, start_time), Computer
| extend available_per_hour=iff(heartbeat_per_hour>0, true, false)
| summarize total_available_hours=countif(available_per_hour==true) by Computer 
| extend total_number_of_buckets=round((end_time-start_time)/1h)+1
| extend availability_rate=total_available_hours*100/total_number_of_buckets
```


## <a name="multiple-data-types"></a>Birden çok veri türü

### <a name="chart-the-record-count-per-table"></a>Tablo başına kayıt sayısı grafik
Aşağıdaki örnekte, son beş saatten tüm tabloların tüm kayıtları toplar ve kaç tane kaydın her tabloda olan sayar. Sonuçlar içinde zaman grafiğini gösterilir.

```Kusto
union withsource=sourceTable *
| where TimeGenerated > ago(5h) 
| summarize count() by bin(TimeGenerated,10m), sourceTable
| render timechart
```

### <a name="count-all-logs-collected-over-the-last-hour-by-type"></a>Son saat üzerinden türü tarafından toplanan tüm günlük sayısı
Aşağıdaki örnekte, son bir saat içinde bildirilen her şeyi arar ve her tablo kayıtlarını sayar **türü**. Sonuçlar bir çubuk grafik olarak görüntülenir.

```Kusto
search *
| where TimeGenerated > ago(1h) 
| summarize CountOfRecords = count() by Type
| render barchart
```

## <a name="azurediagnostics"></a>AzureDiagnostics

### <a name="count-azure-diagnostics-records-per-category"></a>Kategori başına Azure tanılama kayıt sayısı
Bu örnekte benzersiz her kategori için tüm Azure tanılama kayıtlarını sayar.

```Kusto
AzureDiagnostics 
| where TimeGenerated > ago(1d)
| summarize count() by Category
```

### <a name="get-a-random-record-for-each-unique-category"></a>Benzersiz her kategori için rastgele bir kayıt Al
Bu örnekte benzersiz her kategori için bir tek rastgele Azure tanılama kaydı alır.

```Kusto
AzureDiagnostics
| where TimeGenerated > ago(1d) 
| summarize any(*) by Category
```

### <a name="get-the-latest-record-per-category"></a>Kategori başına en son kayıt Al
Bu örnekte, en son Azure tanılama kayıt her benzersiz bir kategoride yer alır.

```Kusto
AzureDiagnostics
| where TimeGenerated > ago(1d) 
| summarize arg_max(TimeGenerated, *) by Category
```

## <a name="network-monitoring"></a>Ağ izleme

### <a name="computers-with-unhealthy-latency"></a>Sağlıksız gecikme süresi olan bilgisayarlar
Bu örnek, sağlıksız gecikme süresiyle, farklı bilgisayarların bir listesini oluşturur.

```Kusto
NetworkMonitoring 
| where LatencyHealthState <> "Healthy" 
| where Computer != "" 
| distinct Computer
```

## <a name="performance"></a>Performans

### <a name="join-computer-perf-records-to-correlate-memory-and-cpu"></a>Bellek ve CPU ilişkilendirmek için bilgisayar performans kayıtlarını katılın
Bu örnek, belirli bir bilgisayarın karşılık gelen **perf** kaydeder ve iki zaman grafiklerini, ortalama CPU ve en fazla bellek oluşturur.

```Kusto
let StartTime = now()-5d;
let EndTime = now()-4d;
Perf
| where CounterName == "% Processor Time"  
| where TimeGenerated > StartTime and TimeGenerated < EndTime
and TimeGenerated < EndTime
| project TimeGenerated, Computer, cpu=CounterValue 
| join kind= inner (
   Perf
    | where CounterName == "% Used Memory"  
    | where TimeGenerated > StartTime and TimeGenerated < EndTime
    | project TimeGenerated , Computer, mem=CounterValue 
) on TimeGenerated, Computer
| summarize avgCpu=avg(cpu), maxMem=max(mem) by TimeGenerated bin=30m  
| render timechart
```

### <a name="perf-cpu-utilization-graph-per-computer"></a>Bilgisayar başına performans CPU kullanım grafiği
Bu örnekte, hesaplar ve grafikler ile başlayan bilgisayar CPU kullanımını _Contoso_.

```Kusto
Perf
| where TimeGenerated > ago(4h)
| where Computer startswith "Contoso" 
| where CounterName == @"% Processor Time"
| summarize avg(CounterValue) by Computer, bin(TimeGenerated, 15m) 
| render timechart
```

## <a name="protection-status"></a>Koruma durumu

### <a name="computers-with-non-reporting-protection-status-duration"></a>Koruma durumu süresi raporlama yapmayan bilgisayarlar
Bu örnekte koruma durumu olan bilgisayarları listeleyen _raporlama_ ve bu durumda oldukları süresi.

```Kusto
ProtectionStatus
| where ProtectionStatus == "Not Reporting"
| summarize count(), startNotReporting = min(TimeGenerated), endNotReporting = max(TimeGenerated) by Computer, ProtectionStatusDetails
| join ProtectionStatus on Computer
| summarize lastReporting = max(TimeGenerated), startNotReporting = any(startNotReporting), endNotReporting = any(endNotReporting) by Computer
| extend durationNotReporting = endNotReporting - startNotReporting
```

### <a name="match-protected-status-records-with-heartbeat-records"></a>Sinyal kayıtları ile korunan durum kayıt eşleşmesi
Bu örnek, ilgili koruma durumu ve hem bilgisayar hem de saat eşleşen sinyal kayıtlarını bulur.
Zaman alan, en yakın dakika kullanarak yuvarlanır **bin**.

```Kusto
let protection_data = ProtectionStatus
    | project Computer, DetectionId, round_time=bin(TimeGenerated, 1m);
let heartbeat_data = Heartbeat
    | project Computer, Category, round_time=bin(TimeGenerated, 1m);
protection_data | join (heartbeat_data) on Computer, round_time
```


## <a name="security-records"></a>Güvenlik kayıtları

### <a name="count-security-events-by-activity-id"></a>Etkinlik Kimliğine göre güvenlik olay sayısı


Bu örnekte sabit yapısına bağlıdır **etkinlik** sütun: \<Kimliği\>-\<adı\>.
Bunu ayrıştırır **etkinlik** iki yeni sütunlara, değer ve her oluşumu sayılarını **ActivityID**.

```Kusto
SecurityEvent
| where TimeGenerated > ago(30m) 
| project Activity 
| parse Activity with activityID " - " activityDesc
| summarize count() by activityID
```

### <a name="count-security-events-related-to-permissions"></a>İzinlerle ilgili güvenlik olay sayısı
Bu örnek sayısını gösterir **securityEvent** olan kayıtları **etkinlik** sütun içeren tüm terimi _izinleri_. Son 30 dakika boyunca oluşturulan kayıtları uygular.

```Kusto
SecurityEvent
| where TimeGenerated > ago(30m)
| summarize EventCount = countif(Activity has "Permissions")
```

### <a name="find-accounts-that-failed-to-log-in-from-computers-with-a-security-detection"></a>Güvenlik algılama ile bilgisayarlardan oturum açılamadı Hesapla
Bu örnekte bulur ve biz güvenlik algılama tanımlamak bilgisayarlardan oturum açılamadı hesapları sayar.

```Kusto
let detections = toscalar(SecurityDetection
| summarize makeset(Computer));
SecurityEvent
| where Computer in (detections) and EventID == 4624
| summarize count() by Account
```

### <a name="is-my-security-data-available"></a>Güvenlik verilerimi kullanılabilir mi?
Başlangıç veri araştırma genellikle veri kullanılabilirlik denetimi ile başlatır. Bu örnek sayısını gösterir **SecurityEvent** son 30 dakika içinde kaydeder.

```Kusto
SecurityEvent 
| where TimeGenerated  > ago(30m) 
| count
```

### <a name="parse-activity-name-and-id"></a>Etkinlik adı ve kimliği ayrıştırılamıyor
Aşağıdaki iki örnek sabit yapısına kullanan **etkinlik** sütun: \<Kimliği\>-\<adı\>. İlk örnekte **ayrıştırma** iki yeni sütun için değer atamak için işleç: **ActivityID** ve **activityDesc**.

```Kusto
SecurityEvent
| take 100
| project Activity 
| parse Activity with activityID " - " activityDesc
```

Bu örnekte **bölme** ayrı değerler dizisi oluşturmak için işleç
```Kusto
SecurityEvent
| take 100
| project Activity 
| extend activityArr=split(Activity, " - ") 
| project Activity , activityArr, activityId=activityArr[0]
```

### <a name="explicit-credentials-processes"></a>Açık kimlik bilgileri işlemleri
Aşağıdaki örnek, bir pasta grafiğinin geçen hafta açık kimlik bilgileri kullanılan işlemleri gösterir.

```Kusto
SecurityEvent
| where TimeGenerated > ago(7d)
// filter by id 4648 ("A logon was attempted using explicit credentials")
| where EventID == 4648
| summarize count() by Process
| render piechart 
```

### <a name="top-running-processes"></a>Çalışan işlemlerin üst

Aşağıdaki örnek, son üç günde beş en yaygın işlemler için etkinlik zaman çizelgesini gösterir.

```Kusto
// Find all processes that started in the last three days. ID 4688: A new process has been created.
let RunProcesses = 
    SecurityEvent
    | where TimeGenerated > ago(3d)
    | where EventID == "4688";
// Find the 5 processes that were run the most
let Top5Processes =
RunProcesses
| summarize count() by Process
| top 5 by count_;
// Create a time chart of these 5 processes - hour by hour
RunProcesses 
| where Process in (Top5Processes) 
| summarize count() by bin (TimeGenerated, 1h), Process
| render timechart
```


### <a name="find-repeating-failed-login-attempts-by-the-same-account-from-different-ips"></a>Aynı hesabı ve farklı ıp'lerden başarısız oturum açma denemesi yinelenen Bul

Aşağıdaki örnekte son altı saat içinde beşten fazla farklı Ip'lerden aynı hesabı tarafından başarısız oturum açma denemesi bulur.

```Kusto
SecurityEvent 
| where AccountType == "User" and EventID == 4625 and TimeGenerated > ago(6h) 
| summarize IPCount = dcount(IpAddress), makeset(IpAddress)  by Account
| where IPCount > 5
| sort by IPCount desc
```

### <a name="find-user-accounts-that-failed-to-log-in"></a>Oturum açmak için başarısız olan kullanıcı hesaplarını bulmak 
Aşağıdaki örnek, birden fazla beş kez son günü ve bunların en son oturum açmayı denediğinde oturum için başarısız olan kullanıcı hesaplarını tanımlar.

```Kusto
let timeframe = 1d;
SecurityEvent
| where TimeGenerated > ago(1d)
| where AccountType == 'User' and EventID == 4625 // 4625 - failed log in
| summarize failed_login_attempts=count(), latest_failed_login=arg_max(TimeGenerated, Account) by Account 
| where failed_login_attempts > 5
| project-away Account1
```

Kullanarak **birleştirme**, ve **izin** deyimleri biz aynı şüpheli hesapları daha sonra başarıyla oturum açamaz olup olmadığını kontrol edebilirsiniz.

```Kusto
let timeframe = 1d;
let suspicious_users = 
    SecurityEvent
    | where TimeGenerated > ago(timeframe)
    | where AccountType == 'User' and EventID == 4625 // 4625 - failed login
    | summarize failed_login_attempts=count(), latest_failed_login=arg_max(TimeGenerated, Account) by Account 
    | where failed_login_attempts > 5
    | project-away Account1;
let suspicious_users_that_later_logged_in = 
    suspicious_users 
    | join kind=innerunique (
        SecurityEvent
        | where TimeGenerated > ago(timeframe)
        | where AccountType == 'User' and EventID == 4624 // 4624 - successful login,
        | summarize latest_successful_login=arg_max(TimeGenerated, Account) by Account
    ) on Account
    | extend was_login_after_failures = iif(latest_successful_login>latest_failed_login, 1, 0)
    | where was_login_after_failures == 1
;
suspicious_users_that_later_logged_in
```

## <a name="usage"></a>Kullanım

### <a name="calculate-the-average-size-of-perf-usage-reports-per-computer"></a>İyileştirilmiş kullanım raporları, bilgisayar başına ortalama boyutunu Hesapla

Bu örnekte, son 3 saat boyunca iyileştirilmiş kullanım raporları, bilgisayar başına ortalama boyutu hesaplar.
Sonuçlar bir çubuk grafik olarak görüntülenir.
```Kusto
Usage 
| where TimeGenerated > ago(3h)
| where DataType == "Perf" 
| where QuantityUnit == "MBytes" 
| summarize avg(Quantity) by Computer
| sort by avg_Quantity desc nulls last
| render barchart
```

### <a name="timechart-latency-percentiles-50-and-95"></a>Zaman grafiğini gecikme yüzdebirliklerini 50 ila 95

Bu örnek hesaplar ve grafikler 50. ve 95. yüzdebirlik değerleri bildirilen **avgLatency** tarafından son 24 saat içinde saat.

```Kusto
Usage
| where TimeGenerated > ago(24h)
| summarize percentiles(AvgLatencyInSeconds, 50, 95) by bin(TimeGenerated, 1h) 
| render timechart
```

### <a name="usage-of-specific-computers-today"></a>Bugün belirli bilgisayarlara kullanımı
Bu örnek alır **kullanım** dizeyi içeren son gününden bilgisayar adları için veri _ContosoFile_. Sonuçlar göre sıralanır **TimeGenerated**.

```Kusto
Usage
| where TimeGenerated > ago(1d)
| where  Computer contains "ContosoFile" 
| sort by TimeGenerated desc nulls last
```

## <a name="updates"></a>Güncelleştirmeler

### <a name="computers-still-missing-updates"></a>Bilgisayarlar hala eksik güncelleştirmeler
Bu örnek, bir veya daha fazla kritik güncelleştirmeler birkaç gün önce eksikti ve hala güncelleştirmeleri eksik olan bilgisayarların listesini gösterir.

```Kusto
let ComputersMissingUpdates3DaysAgo = Update
| where TimeGenerated between (ago(3d)..ago(2d))
| where  Classification == "Critical Updates" and UpdateState != "Not needed" and UpdateState != "NotNeeded"
| summarize makeset(Computer);

Update
| where TimeGenerated > ago(1d)
| where  Classification == "Critical Updates" and UpdateState != "Not needed" and UpdateState != "NotNeeded"
| where Computer in (ComputersMissingUpdates3DaysAgo)
| summarize UniqueUpdatesCount = dcount(Product) by Computer, OSType
```


## <a name="next-steps"></a>Sonraki adımlar

- Başvurmak [Kusto dil başvurusu](/azure/kusto/query) dili hakkında ayrıntılı bilgi için.
- İzlenecek yol bir [Azure İzleyici'de günlük sorguları yazma Ders](get-started-queries.md).
