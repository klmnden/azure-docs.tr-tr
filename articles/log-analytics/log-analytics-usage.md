---
title: Log Analytics'te veri kullanımını çözümleme | Microsoft Belgeleri
description: Log Analytics’e ne kadar veri gönderileceğini değerlendirmek ve nelerin öngörülemez artışlara neden olabileceğini belirlemek için Log Analytics’te Kullanım ve tahmini maliyet panosunu kullanın.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 74d0adcb-4dc2-425e-8b62-c65537cef270
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/11/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: c72e1c92815f70838db20ab67c3f70fc5223ac03
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52964756"
---
# <a name="analyze-data-usage-in-log-analytics"></a>Log Analytics'te veri kullanımını çözümleme

> [!NOTE]
> Bu makale, Log analytics'te veri kullanımını çözümleme açıklar.  İlgili bilgiler için aşağıdaki makalelere göz atın.
> - [Veri hacmi ve saklama Log analytics'te kontrol ederek maliyet yönetme](log-analytics-manage-cost-storage.md) veri saklama döneminizin değiştirerek maliyetlerinizi denetlemek nasıl açıklar.
> - [Kullanım ve Tahmini maliyetler izleme](../monitoring-and-diagnostics/monitoring-usage-and-estimated-costs.md) çoklu Azure İzleme özelliklerini farklı fiyatlandırma modelleri için tahmini maliyetleri ve kullanım görüntülemeyi açıklar. Ayrıca, uygulamanızın fiyatlandırma modelinin değiştirilmesi nasıl açıklar.

## <a name="understand-usage"></a>Kullanımını anlama

Kullanım **Log Analytics kullanımı ve Tahmini maliyetler** gözden geçirin ve veri kullanımını çözümleme. Her çözüm tarafından toplanan veri miktarını gösterir, ne kadar veri tutulur ve maliyetlerinizi tahmini temel alınan veri miktarına ve herhangi ek bir saklama dahil edilen miktarın üzerinde.

![Kullanım ve tahmini maliyetler](media/log-analytics-usage/usage-estimated-cost-dashboard-01.png)<br>

Verilerinizi daha ayrıntılı incelemek için üstteki simgeye tıklayın ya da grafikler, sağ **kullanım ve Tahmini maliyetler** sayfası. Artık daha fazla kullanım ayrıntılarını incelemek için bu sorgu ile çalışabilirsiniz.  

![Günlükleri görüntüle](media/log-analytics-usage/logs.png)<br>

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>Kullanımın neden beklenenden daha yüksek olduğuyla ilgili sorunları giderme
Yüksek kullanımın nedeni aşağıdakilerden biri veya her ikisidir:
- Log Analytics'da beklenenden daha fazla veri gönderiliyordur
- Log analytics'e gönderme beklenen veri değerinden daha fazla düğüm veya bazı düğümler normalden daha fazla veri gönderme

Şimdi nasıl biz hem de bu nedenler hakkında daha fazla bilgi bir göz atalım. 

> [!NOTE]
> Bazı alanlar kullanım veri türünün hala şemada sırada, kullanımdan kaldırılan ve bunların değerlerini artık doldurulur. Bunlar **bilgisayar** alımıyla ilgili alanları yanı sıra (**TotalBatches**, **BatchesWithinSla**, **BatchesOutsideSla**,  **BatchesCapped** ve **AverageProcessingTimeMs**.
> Bilgisayar başına alınan veri miktarı sorgulamak yeni yolu için aşağıya bakın. 

### <a name="data-volume"></a>Veri hacmi 
Üzerinde **kullanım ve Tahmini maliyetler** sayfasında *çözüm başına veri alımı* grafik, toplam gönderilen veri hacmini ve ne kadar her çözüm tarafından gönderilen verilerin gösterir. Bu sayede olup genel veri kullanımı (veya belirli bir çözüm tarafından kullanım) artıyor mu gibi eğilimleri belirlemek sabit kaldığını veya azaldığını. Bu oluşturmak için kullanılan sorgu

`Usage| where TimeGenerated > startofday(ago(31d))| where IsBillable == true
| summarize TotalVolumeGB = sum(Quantity) / 1024 by bin(TimeGenerated, 1d), Solution| render barchart`

Unutmayın yan tümcesi "nerede IsBillable = true" veri türleri için ücretsizdir alımı belirli çözümlerinden filtreler. 

Bkz: veri eğilimlerini IIS günlükler nedeniyle verileri incelemek isterseniz, örneğin belirli veri türleri için daha fazla sınırlandıramazsınız gidebilir:

`Usage| where TimeGenerated > startofday(ago(31d))| where IsBillable == true
| where DataType == "W3CIISLog"
| summarize TotalVolumeGB = sum(Quantity) / 1024 by bin(TimeGenerated, 1d), Solution| render barchart`

### <a name="nodes-sending-data"></a>Veri gönderen düğüm

Geçen ay verilerini raporlamaya bilgisayarların (düğümlerin) sayısını anlamak için kullanın

`Heartbeat | where TimeGenerated > startofday(ago(31d))
| summarize dcount(ComputerIP) by bin(TimeGenerated, 1d)    
| render timechart`

Görmek için **boyutu** bilgisayar başına alınan, Faturalanabilir olayların kullanın

`union withsource = tt * 
| where _IsBillable == true 
| summarize Bytes=sum(_BilledSize) by  Computer | sort by Bytes nulls last `

Veri türleri arasında taramaları çalıştırmak pahalı olduğundan bu sorguları tedbirli şekilde kullanın. Bu sorgu, bu kullanım veri türüyle sorgulama eski biçimini değiştirir. 

Görmek için **sayısı** bilgisayar başına alınan olayların kullanın

`union withsource = tt *
| summarize count() by Computer | sort by count_ nulls last`

Bilgisayar başına alınan Faturalanabilir olayların sayısını görmek için kullanın 

`union withsource = tt * 
| where _IsBillable == true 
| summarize count() by Computer  | sort by count_ nulls last`

Sayıları Faturalandırılabilir veri türleri için belirli bir bilgisayar için veri gönderdiğini görmek istiyorsanız, bu seçeneği kullanın:

`union withsource = tt *
| where Computer == "*computer name*"
| where _IsBillable == true 
| summarize count() by tt | sort by count_ nulls last `

Belirli veri türü için veri kaynağına daha ayrıntılı incelemek için bazı yararlı örnek sorgular şunlardır:

+ **Güvenlik** çözümü
  - `SecurityEvent | summarize AggregatedValue = count() by EventID`
+ **Günlük Yönetimi** çözümü
  - `Usage | where Solution == "LogManagement" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | summarize AggregatedValue = count() by DataType`
+ **Perf** veri türü
  - `Perf | summarize AggregatedValue = count() by CounterPath`
  - `Perf | summarize AggregatedValue = count() by CounterName`
+ **Event** veri türü
  - `Event | summarize AggregatedValue = count() by EventID`
  - `Event | summarize AggregatedValue = count() by EventLog, EventLevelName`
+ **Syslog** veri türü
  - `Syslog | summarize AggregatedValue = count() by Facility, SeverityLevel`
  - `Syslog | summarize AggregatedValue = count() by ProcessName`
+ **AzureDiagnostics** veri türü
  - `AzureDiagnostics | summarize AggregatedValue = count() by ResourceProvider, ResourceId`

### <a name="tips-for-reducing-data-volume"></a>Veri hacmini azaltmak için ipuçları

Toplanan günlük hacmini azaltmak için bazı öneriler şunlardır:

| Yüksek veri hacminin kaynağı | Veri hacmi nasıl azaltılır |
| -------------------------- | ------------------------- |
| Güvenlik olayları            | [Yaygın veya en az güvenlik olaylarını](https://blogs.technet.microsoft.com/msoms/2016/11/08/filter-the-security-events-the-oms-security-collects/) seçin <br> Güvenlik denetimi ilkesini yalnızca gerekli olayları toplayacak şekilde değiştirin. Özellikle, şunlarla ilgili olayları toplamak gerekip gerekmediğini gözden geçirin: <br> - [filtre platformu denetimi](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [kayıt defteri denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941614(v%3dws.10))<br> - [dosya sistemi denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772661(v%3dws.10))<br> - [çekirdek nesnesi denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941615(v%3dws.10))<br> - [tanıtıcı değiştirme denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772626(v%3dws.10))<br> - çıkarılabilir depolama birimi denetimi |
| Performans sayaçları       | [Performans sayacı yapılandırmasını](log-analytics-data-sources-performance-counters.md) şöyle değiştirin: <br> - Koleksiyonun sıklığını azaltın <br> - Performans sayaçlarının sayısını azaltın |
| Olay günlükleri                 | [Olay günlüğü yapılandırmasını](log-analytics-data-sources-windows-events.md) şöyle değiştirin: <br> - Toplanan olay günlüklerinin sayısını azaltın <br> - Yalnızca gerekli olay düzeylerini toplayın. Örneğin, *Bilgi* düzeyindeki olayları toplamayın |
| Syslog                     | [Syslog yapılandırmasını](log-analytics-data-sources-syslog.md) şu şekilde değiştirin: <br> - Toplanan tesislerin sayısını azaltın <br> - Yalnızca gerekli olay düzeylerini toplayın. Örneği *Bilgi* ve *Hata Ayıklama* düzeyindeki olayları toplamayın |
| AzureDiagnostics           | Aşağıdaki amaçlarla kaynak günlüğü koleksiyonunu değiştirin: <br> - Log Analytics’e günlük gönderen kaynak sayısını azaltma <br> - Yalnızca gerekli günlükleri toplama |
| Çözüm ihtiyacı olmayan bilgisayarlardan toplanan çözüm verileri | Yalnızca gerekli bilgisayar gruplarından veri toplamak için [çözüm hedefleme](../azure-monitor/insights/solution-targeting.md) özelliğini kullanın. |

### <a name="getting-node-counts"></a>Başlangıç düğüm sayısı 

"Fiyatlandırma katmanı düğümde (OMS)" olduğunuz sonra düğüm ve çözüm sayısına göre kullanacağınız Insights sayısı ve Analytics düğümleri için faturalandırılır gösterilecek tabloda ücretlendirilir **kullanım ve tahmini maliyet**sayfası.  

Güvenlik düğümleri sayısını görmek için sorguyu kullanabilirsiniz:

`union
(
    Heartbeat
    | where (Solutions has 'security' or Solutions has 'antimalware' or Solutions has 'securitycenter')
    | project Computer
),
(
    ProtectionStatus
    | where Computer !in~
    (
        (
            Heartbeat
            | project Computer
        )
    )
    | project Computer
)
| distinct Computer
| project lowComputer = tolower(Computer)
| distinct lowComputer
| count`

Farklı bir Otomasyon düğüm sayısını görmek için sorguyu kullanın:

` ConfigurationData 
 | where (ConfigDataType == "WindowsServices" or ConfigDataType == "Software" or ConfigDataType =="Daemons") 
 | extend lowComputer = tolower(Computer) | summarize by lowComputer 
 | join (
     Heartbeat 
       | where SCAgentChannel == "Direct"
       | extend lowComputer = tolower(Computer) | summarize by lowComputer, ComputerEnvironment
 ) on lowComputer
 | summarize count() by ComputerEnvironment | sort by ComputerEnvironment asc`

## <a name="create-an-alert-when-data-collection-is-higher-than-expected"></a>Toplanan veriler beklenenden fazlaysa uyarı oluşturma
Bu bölümde, aşağıdaki durumlarda nasıl uyarı oluşturulacağı açıklanır:
- Veri hacmi belirtilen bir miktarı aştığında.
- Veri hacminin belirtilen bir miktarı aşacağı tahmin edildiğinde.

Azure Uyarıları, arama sorguları kullanan [günlük uyarılarını](../monitoring-and-diagnostics/monitor-alerts-unified-log.md) destekler. 

Aşağıdaki sorgu, son 24 saatte 100 GB'den fazla veri toplandığında bir sonuç verir:

`union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type | where DataGB > 100`

Aşağıdaki sorgu, ne zaman bir günde 100 GB'den fazla veri toplanacağını tahmin etmek için basit bir formül kullanır: 

`union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type | where EstimatedGB > 100`

Farklı bir veri hacminde uyarıda bulunmak için, sorgulardaki 100 değerini uyarılmak istediğiniz GB sayısıyla değiştirin.

Toplanan veri beklenen miktarı aştığında size bildirilmesini sağlamak için, [yeni günlük uyarısı oluşturma](../monitoring-and-diagnostics/alert-metric.md) başlığı altında açıklanan adımları kullanın.

İlk sorgu için, yani 24 saat içinde 100 GB'den fazla veri toplandığında uyarı oluştururken şu ayarları yapın:  

- **Uyarı koşulunu tanımlama** adımında Log Analytics çalışma alanınızı kaynak hedefi olarak belirtin.
- **Uyarı ölçütleri** alanında aşağıdakileri belirtin:
   - **Sinyal Adı** bölümünde **Özel günlük araması**'nı seçin
   - **Arama sorgusu**: `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type | where DataGB > 100`
   - **Uyarı mantığı**, **Temeli** *bir dizi sonuçtur* ve **Koşul**, *Büyüktür* bir **Eşik değeri**, *0*
   - Kullanım verileri saatte bir güncelleştirildiğinden **Süre** *1440* dakika, **Uyarı sıklığı** ise *60* dakikada bir olarak belirlenmiştir.
- **Uyarı ayrıntılarını tanımlama** adımında aşağıdakileri belirtin:
   - **Ad**: *24 saat içinde 100 GB'den büyük veri hacmi*
   - **Önem derecesi**: *Uyarı*

Günlük uyarısı ölçütlerle eşleştiğinde bilgilendirme yapılması için var olan bir [Eylem Grubunu](../monitoring-and-diagnostics/monitoring-action-groups.md) kullanın veya yeni bir tane oluşturun.

İkinci sorgu için, yani 24 saat içinde 100 GB'den fazla veri olacağı tahmin edildiğinde uyarı oluştururken şu ayarları yapın:

- **Uyarı koşulunu tanımlama** adımında Log Analytics çalışma alanınızı kaynak hedefi olarak belirtin.
- **Uyarı ölçütleri** alanında aşağıdakileri belirtin:
   - **Sinyal Adı** bölümünde **Özel günlük araması**'nı seçin
   - **Arama sorgusu**: `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type | where EstimatedGB > 100`
   - **Uyarı mantığı**, **Temeli** *bir dizi sonuçtur* ve **Koşul**, *Büyüktür* bir **Eşik değeri**, *0*
   - Kullanım verileri saatte bir güncelleştirildiğinden **Süre** *180* dakika, **Uyarı sıklığı** ise *60* dakikada bir olarak belirlenmiştir.
- **Uyarı ayrıntılarını tanımlama** adımında aşağıdakileri belirtin:
   - **Ad**: *24 saat içinde veri hacminin 100 GB'den büyük olacağı tahmin ediliyor*
   - **Önem derecesi**: *Uyarı*

Günlük uyarısı ölçütlerle eşleştiğinde bilgilendirme yapılması için var olan bir [Eylem Grubunu](../monitoring-and-diagnostics/monitoring-action-groups.md) kullanın veya yeni bir tane oluşturun.

Uyarı aldığınızda, kullanımın neden beklenenden fazla olduğu konusundaki sorunları gidermek için aşağıdaki bölümde yer alan adımları kullanın.

## <a name="next-steps"></a>Sonraki adımlar
* Arama dilini nasıl kullanacağınızı öğrenmek için bkz. [Log Analytics'te günlük aramaları](log-analytics-queries.md). Kullanım verilerinde başka analizler yapmak için arama sorgularını kullanabilirsiniz.
* Bir arama ölçütü karşılandığında size bildirilmesini sağlamak için, [yeni günlük uyarısı oluşturma](../monitoring-and-diagnostics/alert-metric.md) başlığı altında açıklanan adımları kullanın.
* Yalnızca gerekli bilgisayar gruplarından veri toplamak için [çözüm hedefleme](../azure-monitor/insights/solution-targeting.md) özelliğini kullanın.
* Etkili bir güvenlik olay koleksiyonu ilkesi yapılandırmak için, [Azure Güvenlik Merkezi filtreleme ilkesi](../security-center/security-center-enable-data-collection.md) konusunu gözden geçirin.
* [Performans sayacı yapılandırmasını](log-analytics-data-sources-performance-counters.md) değiştirin.
* Olay toplama ayarlarınızda değişiklik yapmak için, [olay günlüğü yapılandırması](log-analytics-data-sources-windows-events.md) konusunu gözden geçirin.
* Syslog koleksiyonu ayarlarınızda değişiklik yapmak için, [syslog yapılandırması](log-analytics-data-sources-syslog.md) konusunu gözden geçirin.
