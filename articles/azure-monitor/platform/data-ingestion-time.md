---
title: Azure İzleyici'de veri alma süresini oturum | Microsoft Docs
description: Azure İzleyici'de günlük verilerini toplamak, gecikme süresini etkileyen faktörleri farklı açıklar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/24/2019
ms.author: bwren
ms.openlocfilehash: d508ce217e3a97b3399435cb63295eb28965359a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65605610"
---
# <a name="log-data-ingestion-time-in-azure-monitor"></a>Azure İzleyici'de günlük veri alım zamanı
Azure İzleyici binlerce müşteri terabaytlarca veriyi her ay büyüyen bir hızda gönderme yapan bir büyük ölçekli veri hizmetidir. Çoğunlukla için günlük verileri toplandıktan sonra kullanılabilir olana kadar geçen süreyi hakkında sorular vardır. Bu makalede, bu gecikme süresini etkileyen faktörleri farklı açıklanmaktadır.

## <a name="typical-latency"></a>Tipik bir gecikme süresi
Gecikme süresi verileri izlenen sistemde oluşturulduğu tarih ve analiz Azure İzleyici'de gelen süreyi ifade eder. Günlük verisi alımı için tipik bir gecikme süresi arasında 2 ve 5 dakikadır. Herhangi bir veri belirli gecikme sürelerini faktörler aşağıda açıklanan çeşitli bağlı olarak değişir.


## <a name="factors-affecting-latency"></a>Gecikme süresini etkileyen faktörler
Toplam alım zaman belirli bir veri kümesi aşağıdaki üst düzey alanlarına ayrılabilir. 

- Aracı saati - bir olay bulmak, toplamak ve Azure İzleyici alma noktası günlük kaydı olarak gönderin. Çoğu durumda, bu işlem bir aracı tarafından işlenir.
- İşlem hattı süresi - Alım işlem hattının günlük kaydını işleme süresi. Bu olay özelliklerini ayrıştırma ve potansiyel olarak hesaplanan bilgi eklemeyi içerir.
- Dizin oluşturma saati-günlük kaydı Azure İzleyici büyük veri deposuna veri alımı için harcanan süre.

Bu işlemde sunulan farklı gecikme süresi ile ilgili ayrıntılar aşağıda açıklanmıştır.

### <a name="agent-collection-latency"></a>Aracı koleksiyon gecikme süresi
Aracılar ve yönetim çözümleri farklı stratejiler gecikme süresini etkileyebilir bir sanal makine verileri toplamak için kullanın. Belirli bazı örnekler şunlardır:

- Windows olayları, syslog olayları ve performans ölçümlerini hemen toplanır. Linux performans sayaçlarıyla 30 saniyelik aralıklarla yoklama.
- Zaman değiştikten sonra IIS günlükler ve özel günlükleri toplanır. IIS günlükler için bunu tarafından etkilenir [IIS üzerinde yapılandırılmış rollover zamanlaması](data-sources-iis-logs.md). 
- Active Directory çoğaltma çözümü, Active Directory değerlendirmesi çözümü, Active Directory altyapınızın haftalık bir değerlendirme gerçekleştirirken, değerlendirme beş günde gerçekleştirir. Yalnızca değerlendirme tamamlandıktan sonra aracı bu günlükleri toplar.

### <a name="agent-upload-frequency"></a>Aracı karşıya yükleme frekansı
Log Analytics aracısını basit olmasını sağlamak için aracı günlüklerini arabelleğe alır ve bunları Azure İzleyici düzenli aralıklarla gönderir. Karşıya yükleme ve veri türüne bağlı olarak 2 dakika 30 saniye arasında sıklığı değişir. 1 dakika içinde en çok veriyi karşıya yüklendi. Ağ koşulları, Azure İzleyici alma noktası ulaşmak için bu verileri gecikme süresini olumsuz yönde etkileyebilir.

### <a name="azure-activity-logs-diagnostic-logs-and-metrics"></a>Azure etkinlik günlükleri, tanılama günlükleri ve ölçümler
Azure veri işleme için Log Analytics alma noktasında kullanılabilir olana kadar ek zaman ekler:

- Azure hizmete bağlı olarak 2-15 dakika tanılama günlükleri verilerden yararlanın. Bkz: [aşağıdaki sorguyu](#checking-ingestion-time) bu gecikme süresi, ortamınızda incelemek için
- Azure platformu ölçümleri Log Analytics alımı noktasına gönderilecek 3 dakika alın.
- Etkinlik günlüğü verileri Log Analytics'e alımı noktasına gönderilmesi için yaklaşık 10-15 dakika sürer.

Alma noktasında kullanılabilir olduktan sonra veri sorgulama için kullanılabilir olması için ek 2-5 dakika sürer.

### <a name="management-solutions-collection"></a>Yönetim çözümleri toplama
Bazı çözümler verilerine bir Aracıdan toplamaz ve ek gecikme sağlayan bir koleksiyon yöntemi kullanabilir. Bazı çözümler, neredeyse gerçek zamanlı koleksiyon denemeden, düzenli aralıklarla veri toplayın. Belirli örnekler aşağıdakileri içerir:

- Office 365 çözüm, Office 365 Yönetim etkinliği şu anda tüm neredeyse gerçek zamanlı gecikme süresi garanti eder sağlamaz API'sini kullanarak etkinlik günlüklerini yoklar.
- Windows Analytics çözümleri (örneğin güncelleştirme uyumluluğu) veri günlük sıklıkta çözüm tarafından toplanır.

Koleksiyon sıklığının belirlenmesi her bir çözüm için belgelere bakın.

### <a name="pipeline-process-time"></a>İşlem hattı işlem süresi
Azure İzleyici ardışık düzende içe alınan günlük kayıtları sonra geçici depolama birimine Kiracı yalıtımı sağlamak ve veri kaybı olmadığından emin olmak için yazılan. Bu işlem genellikle 5-15 saniye ekler. Bazı yönetim çözümleri, veri toplama daha ağır algoritmalarını uygulayan ve içindeki veri akış içgörülere sahip olun. Örneğin, ağ performansı izleme gelen veri 3 dakikalık aralıklarında etkili bir şekilde 3 dakikalık bir gecikme ekleme toplar. Gecikme süresi ekler, başka bir işlem özel günlükleri işleyen işlemidir. Bazı durumlarda, bu işlem birkaç dakika gecikme dosyalarından aracısı tarafından toplanan günlükleri ekleyebilir.

### <a name="new-custom-data-types-provisioning"></a>Sağlama, yeni özel veri türleri
Öğesinden yeni bir özel veri türü oluşturulduğunda bir [özel günlük](data-sources-custom-logs.md) veya [veri toplayıcı API'sini](data-collector-api.md), sistem bir adanmış depolama kapsayıcısı oluşturur. Bu, bu veri türü yalnızca ilk görünümünü oluşan tek seferlik bir ek yüktür.

### <a name="surge-protection"></a>Aşırı yükleme talebiyle koruma
Azure İzleyicisi'nin en önemli öncelik sistem veri dalgalanmalarına için yerleşik koruma bulunur şekilde müşteri veri kaybı, olduğundan emin olmaktır. Bu, büyük yükü altında bile, sistemin çalışıp tutmak emin emin olmak için arabellek içerir. Normal yük altında bu denetimlerin bir dakikadan az ekleyebilirsiniz, ancak aşırı koşullar ve bunlar veri sağlarken önemli zaman ekleyebilirsiniz hataları güvenlidir.

### <a name="indexing-time"></a>Dizin oluşturma zamanı
Analiz ve Gelişmiş arama özellikleri aksine verilere hemen erişim sağlamaya sağlamaya arasındaki her büyük veri platformu için yerleşik bir denge yoktur. Azure İzleyici, kayıt milyarlarca üzerinde güçlü sorgular çalıştırın ve birkaç saniye içinde sonuçları almak sağlar. Altyapı ve verileri kendi alımı sırasında önemli ölçüde dönüştüren ve benzersiz compact yapılarını depolar olduğundan bu mümkün olur. Sistem, yeterli miktarı kadar bu yapıları oluşturmak için kullanılabilir veri arabelleğe alır. Arama sonuçlarında günlük kaydı görünmeden önce bu tamamlanması gerekir.

Bu işlem şu anda yaklaşık 5 dakika sürer. daha yüksek veri fiyatları üzerinden ancak daha az zaman veri düşük hacim olduğunda. Bu, birlikte değişkenlik mantığa aykırı görünüyor, ancak bu işlem gecikme süresi yüksek hacimli üretim iş yükleri için en iyi duruma getirilmesi sağlar.



## <a name="checking-ingestion-time"></a>Alım zamanı
Alma süresi, farklı koşullarda farklı kaynaklar için değişiklik gösterebilir. Ortamınızı belirli davranışını tanımlamak için günlük sorguları kullanabilirsiniz.

### <a name="ingestion-latency-delays"></a>Alma Gecikme gecikmeleri
Belirli bir kaydı gecikme süresini sonucunu karşılaştırarak ölçebilirsiniz [ingestion_time()](/azure/kusto/query/ingestiontimefunction) işlevi _TimeGenerated_ alan. Bu veri alımı gecikme nasıl davranacağını bulmak için çeşitli toplamalar ile kullanılabilir. Bazı yüzdebirlik büyük miktarda veri öngörüleri almak için alma süresini inceleyin. 

Örneğin, aşağıdaki sorguyu hangi bilgisayarların en yüksek alma geçerli gün içinde yansımamış gösterir: 

``` Kusto
Heartbeat
| where TimeGenerated > ago(8h) 
| extend E2EIngestionLatency = ingestion_time() - TimeGenerated 
| summarize percentiles(E2EIngestionLatency,50,95) by Computer 
| top 20 by percentile_E2EIngestionLatency_95 desc  
```
 
Bir süre boyunca belirli bir bilgisayar için alma zamanında detaya gitmek grafikteki verileri görselleştiren aşağıdaki sorguyu kullanın: 

``` Kusto
Heartbeat 
| where TimeGenerated > ago(24h) and Computer == "ContosoWeb2-Linux"  
| extend E2EIngestionLatencyMin = todouble(datetime_diff("Second",ingestion_time(),TimeGenerated))/60 
| summarize percentiles(E2EIngestionLatencyMin,50,95) by bin(TimeGenerated,30m) 
| render timechart  
```
 
Bunlar, kendi IP adresine göre bulunur ülke/bölge tarafından bilgisayar alımı saatini göstermek için aşağıdaki sorguyu kullanın: 

``` Kusto
Heartbeat 
| where TimeGenerated > ago(8h) 
| extend E2EIngestionLatency = ingestion_time() - TimeGenerated 
| summarize percentiles(E2EIngestionLatency,50,95) by RemoteIPCountry 
```
 
Önceki sorguların diğer türleri ile kullanılabilecek şekilde Aracıdan gelen farklı veri türleri farklı alımı gecikme süresi olabilir. Çeşitli Azure Hizmetleri alma süresini incelemek için aşağıdaki sorguyu kullanın: 

``` Kusto
AzureDiagnostics 
| where TimeGenerated > ago(8h) 
| extend E2EIngestionLatency = ingestion_time() - TimeGenerated 
| summarize percentiles(E2EIngestionLatency,50,95) by ResourceProvider
```

### <a name="resources-that-stop-responding"></a>Yanıt vermemeye kaynakları 
Bazı durumlarda, veri gönderen bir kaynak durdurabilirsiniz. Bir kaynak veya veri gönderiyor, anlamak için standardı tarafından tanımlanan, en son kaydını bakın _TimeGenerated_ alan.  

Kullanım _sinyal_ aracı tarafından gönderilen bir sinyal bir dakikadan beri bir sanal makine kullanılabilirliğini kontrol etmek için tablo. Sinyal son raporlanmayan etkin bilgisayarları listelemek için aşağıdaki sorguyu kullanın: 

``` Kusto
Heartbeat  
| where TimeGenerated > ago(1d) //show only VMs that were active in the last day 
| summarize NoHeartbeatPeriod = now() - max(TimeGenerated) by Computer  
| top 20 by NoHeartbeatPeriod desc 
```

## <a name="next-steps"></a>Sonraki adımlar
* Okuma [hizmet düzeyi sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/log-analytics/v1_1/) Azure İzleyici için.

