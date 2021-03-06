---
title: Azure İzleyici günlüklerine için kullanımı ve maliyetleri yönetme | Microsoft Docs
description: Fiyatlandırma planı değiştirmek ve Azure İzleyici'de Log Analytics çalışma alanınız için veri hacmi ve saklama ilkesini yönetme hakkında bilgi edinin.
services: azure-monitor
documentationcenter: azure-monitor
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/06/2019
ms.author: magoedte
ms.subservice: ''
ms.openlocfilehash: bcfefc9698f7f251e99531750e19e7c06395e064
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67655697"
---
# <a name="manage-usage-and-costs-with-azure-monitor-logs"></a>Kullanımı ve Azure İzleyici günlüklerine ile maliyetleri yönetme

> [!NOTE]
> Bu makalede, maliyetlerinizi Azure İzleyici'de Log Analytics çalışma alanınız için veri saklama süresi ayarlayarak denetlemek nasıl açıklar.  İlgili bilgi için şu makaleye bakın.
> - [Kullanım ve Tahmini maliyetler izleme](usage-estimated-costs.md) çoklu Azure İzleme özelliklerini farklı fiyatlandırma modelleri için tahmini maliyetleri ve kullanım görüntülemeyi açıklar. Ayrıca, uygulamanızın fiyatlandırma modelinin değiştirilmesi nasıl açıklar.

Azure İzleyici günlüklerine ölçek ve Destek toplama, dizin oluşturma ve kuruluşunuzda oldukça büyük miktardaki verileri günde herhangi bir kaynaktan depolamak üzere tasarlanmış veya Azure'da dağıtılır.  Bu, kuruluşunuz için birincil bir sürücü olabilir, ancak hesaplıdır sonuçta temel alınan sürücüsüdür. Bu amaçla bir Log Analytics çalışma alanı maliyeti, ayrıca bir plan üzerinde bağımlı olduğu ve ne kadar süreyle, bağlı kaynaklardan oluşturulan verileri depolamak seçtiğiniz toplanan verilerin hacmi üzerinden yalnızca temel almayan anlamak önemlidir.  

Bu makalede nasıl proaktif bir şekilde veri hacmi ve depolama büyüme izleyebilir ve bu ilişkili maliyetleri denetleyebilirsiniz sınırlarını tanımlamak inceleyin. 

Veri maliyetine aşağıdaki faktörlere bağlı olarak önemli ölçüde olabilir: 

- Oluşturulan ve çalışma alanı için alınan veri hacmi 
    - Etkin yönetim çözümlerinin sayısını
    - İzlenen sistemleri sayısı
    - İzlenen her kaynaktan toplanan veri türü 
- Verilerinizi korumak için karar süre 

## <a name="understand-your-workspaces-usage-and-estimated-cost"></a>Çalışma alanınızın kullanım ve tahmini maliyet anlama

Son kullanım modellerini temel maliyetleri ne olduğunu anlamak kolay, büyük olasılıkla azure İzleyici günlüklerine yapar. Bunu yapmak için **Log Analytics kullanımı ve Tahmini maliyetler** gözden geçirin ve veri kullanımını çözümleme. Her çözüm tarafından toplanan veri miktarını gösterir, ne kadar veri tutulur ve maliyetlerinizi tahmini temel alınan veri miktarına ve herhangi ek bir saklama dahil edilen miktarın üzerinde.

![Kullanım ve tahmini maliyetler](media/manage-cost-storage/usage-estimated-cost-dashboard-01.png)

Verilerinizi daha ayrıntılı incelemek için üstteki simgeye tıklayın ya da grafikler, sağ **kullanım ve Tahmini maliyetler** sayfası. Artık daha fazla kullanım ayrıntılarını incelemek için bu sorgu ile çalışabilirsiniz.  

![Günlükleri görüntüle](media/manage-cost-storage/logs.png)

Gelen **kullanım ve Tahmini maliyetler** sayfası ay için veri hacminiz gözden geçirebilirsiniz. Bu, Log Analytics çalışma alanınızda saklanır ve alınan tüm verileri içerir.  Tıklayın **kullanım ayrıntılarını** veri kaynağı, bilgisayarlar ve teklifi volume Trend bilgilerle kullanım panosunu görüntülemek için sayfanın üst. Görüntüleme ve günlük üst limit ayarlayabilir veya bekletme süresini değiştirmek için tıklayın **veri hacmi Yönetimi**.
 
Log Analytics ücretleri Azure faturanızı eklenir. Azure faturalandırma bölümünde Azure portal'ın veya fatura ayrıntılarını görebilirsiniz [Azure fatura portalı](https://account.windowsazure.com/Subscriptions).  

## <a name="daily-cap"></a>Günlük sınır

Günlük üst sınır yapılandırın ve çalışma alanınız için günlük alımı sınırlayabilirsiniz, ancak hedef günlük limite ulaşılmadan olmamalıdır dikkatli kullanın.  Aksi takdirde, diğer Azure Hizmetleri ve çözümleri olan işlevselliği güncel verileri çalışma alanında kullanılabilir olan bağımlı etkileyebilir günün geri kalanında verileri kaybedersiniz.  Sonuç olarak, BT Hizmetleri destekleyen kaynakların sistem durumu koşullarını etkilendiğinde yeteneğinizi inceleyin ve almak için sizi uyarır.  Günlük üst sınırınızı veya çalışma alanınız için planlanmayan ücretleri sınırlandırmak istediğinizde, yönetilen kaynaklardaki veri hacmindeki beklenmeyen artış yönetmek ve için bir yol olarak kullanılmak üzere tasarlanmıştır.  

Günlük sınıra ulaşıldığında, Faturalanabilir veri türlerinin günlük geri kalanı için durdurur. Seçili Log Analytics çalışma alanı için sayfanın üstündeki bir uyarı başlık görünür ve bir işlemi olay gönderilir *işlemi* altında tablo **LogManagement** kategorisi. Veri toplama sürdürür altında sıfırlama zaman tanımlandıktan sonra *günlük sınır ayarlanacak*. Günlük veri sınırına ulaşıldığında bildirmek için yapılandırılmış. Bu işlem olayı temel alan bir uyarı kuralı tanımlayan öneririz. 

> [!NOTE]
> Günlük üst sınır, Azure Güvenlik Merkezi'nde veri koleksiyonunu durdurmaz.

### <a name="identify-what-daily-data-limit-to-define"></a>Tanımlamak için hangi günlük veri sınırınızın tanımlayın

Gözden geçirme [Log Analytics kullanımı ve Tahmini maliyetler](usage-estimated-costs.md) veri alma eğilimi ve tanımlamak için günlük hacim üst sınırını ne olduğunu anlamak için. Sınıra ulaşıldıktan sonra kaynaklarınızı izleyin mümkün olmayacaktır beri dikkatlice değerlendirilmelidir. 

### <a name="manage-the-maximum-daily-data-volume"></a>Maksimum günlük veri hacmini yönetme

Log Analytics çalışma alanı günde içe alma veri hacmi yönetmek için bir sınır yapılandırma aşağıdaki adımları açıklanmaktadır.  

1. Çalışma alanınızın sayfasında, soldaki bölmeden **Kullanım ve tahmini maliyetler**’i seçin.
2. Üzerinde **kullanım ve Tahmini maliyetler** sayfasında seçilen çalışma alanı için **veri hacmi Yönetimi** sayfanın üst. 
3. Günlük üst sınır olan **OFF** varsayılan olarak – tıklayın **ON** etkinleştirin ve ardından veri birimi sınırı GB/gün.

    ![Log Analytics'e veri sınırını yapılandırın](media/manage-cost-storage/set-daily-volume-cap-01.png)

### <a name="alert-when-daily-cap-reached"></a>Uyarıyı günlük sınırına ulaşıldı

Veri sınırı eşiğine karşılandığında size görsel bir ipucu Azure portalında mevcut olsa da bu davranış mutlaka Acil dikkat gerektiren işletimsel sorunları nasıl yönettiğiniz için hizalayın değil.  Bir uyarı bildirimine almak, Azure İzleyici'de yeni bir uyarı kuralı oluşturabilirsiniz.  Daha fazla bilgi için bkz. [oluşturun, görüntüleyin ve Uyarıları yönetmek nasıl](alerts-metric.md).

Başlamanıza yardımcı olmak için uyarı için önerilen ayarları şunlardır:

- Hedef: Log Analytics kaynağınızı seçin
- Ölçütleri: 
   - Sinyal adı: Özel günlük araması
   - Arama sorgusu: İşlemi | Ayrıntı 'Altındaysa' sahip olduğu
   - Temel: Sonuç sayısı
   - Koşul: Büyüktür
   - Eşiği: 0
   - Dönem: 5 (dakika)
   - Sıklığı: 5 (dakika)
- Uyarı kuralı adı: Günlük veri sınırına ulaşıldı
- Önem derecesi: Uyarı (önem derecesi 1)

Uyarı tanımlanır ve sınıra ulaşıldıktan sonra bir uyarı tetiklenir ve eylem grubunda tanımlanan yanıt gerçekleştirir. Aracılığıyla e-posta veya metin iletileriyle ekibinizi bilgilendirin veya Web kancaları, Otomasyon runbook'ları kullanarak işlemleri otomatik hale getirmek veya [dış bir ITSM çözümüyle tümleştirme](itsmc-overview.md#create-itsm-work-items-from-azure-alerts). 

## <a name="change-the-data-retention-period"></a>Veri saklama süresini değiştirme

Aşağıdaki adımları ne kadar günlük verileri çalışma alanınızda tarafından tutulur yapılandırma açıklanmaktadır.
 
1. Çalışma alanınızın sayfasında, soldaki bölmeden **Kullanım ve tahmini maliyetler**’i seçin.
2. **Kullanım ve tahmini maliyetler** sayfasının üst kısmındaki **Veri hacmi yönetimi**'ni seçin.
3. Bölmede artırın veya gün sayısını azaltın, ardından kaydırıcıyı **Tamam**.  Kullanıyorsanız *ücretsiz* katmanı, veri bekletme süresini değiştirmek mümkün olmayacaktır ve bu ayarı denetlemek için ücretli katmana yükseltmeniz gerekir.

    ![Çalışma alanı veri saklama ayarını değiştirme](media/manage-cost-storage/manage-cost-change-retention-01.png)
    
Bekletme de olabilir [ARM ayarlamak](https://docs.microsoft.com/azure/azure-monitor/platform/template-workspace-configuration#configure-a-log-analytics-workspace) kullanarak `dataRetention` parametresi. Ayrıca, veri saklama için 30 gün ayarlarsanız, eski verileri kullanarak, hemen bir temizleme tetikleyebilirsiniz `immediatePurgeDataOn30Days` parametresini uyumluluk ilgili senaryolarda yararlı olabilir. Bu işlev, yalnızca ARM sunulur. 

## <a name="legacy-pricing-tiers"></a>Eski fiyatlandırma katmanları

Abonelikleri olan ve Log Analytics çalışma alanı veya Application Insights kaynağı 2 Nisan 2018'den önce olduğunu veya 1 Şubat 2019'dan önce kullanmaya bir Kurumsal Anlaşma bağlantılı fiyatlandırma katmanları eski erişiminiz devam eder: **Ücretsiz**, **tek başına (GB başına)** ve **düğüm başına (OMS)** .  Ücretsiz fiyatlandırma katmanı çalışma alanları, günlük veri alımı (hariç, Azure Güvenlik Merkezi tarafından toplanan güvenlik veri türleri) 500 MB ile sınırlı olacaktır ve veri saklama için 7 gün sınırlıdır. Ücretsiz fiyatlandırma katmanı, yalnızca değerlendirme amacıyla tasarlanmıştır. Çalışma alanları tek başına veya fiyatlandırma katmanları düğüm başına en fazla 2 yıllık kullanıcı tarafından yapılandırılabilir saklama sahiptir. Nisan 2016'dan önce oluşturulan çalışma alanları da özgün erişim sahibi **standart** ve **Premium** fiyatlandırma katmanları. Fiyatlandırma katmanı sınırlamaları hakkında daha fazla ayrıntı kullanılabilir [burada](https://docs.microsoft.com/azure/azure-subscription-service-limits#log-analytics-workspaces).

> [!NOTE]
> OMS E1 paketi, OMS E2 Suite veya System Center için OMS eklentisi satın alındıktan sonra sunulan destek haklarını kullanmak için Log Analytics seçin *düğüm başına* fiyatlandırma katmanı.

Log analytics'in erken Benimseyenler Ayrıca özgün fiyatlandırma katmanları erişiminiz **standart** ve **Premium**, sabit veri saklama, 30 ila 365 gün sırasıyla. 

## <a name="changing-pricing-tier"></a>Fiyatlandırma katmanını değiştirme

Log Analytics çalışma alanınızın eski fiyatlandırma katmanları arasında değiştirmek için eski fiyatlandırma katmanları erişimi varsa:

1. Azure portalında Log Analytics abonelikleri bölmesinde, bir çalışma alanı seçin.

2. Çalışma Alanı bölmesinden altında **genel**seçin **fiyatlandırma katmanı**.  

3. Altında **fiyatlandırma katmanı**, bir fiyatlandırma katmanı seçin ve ardından **seçin**.  
    ![Seçili fiyatlandırma planı](media/manage-cost-storage/workspace-pricing-tier-info.png)

Ayrıca [ARM aracılığıyla fiyatlandırma katmanını ayarlamak](https://docs.microsoft.com/azure/azure-monitor/platform/template-workspace-configuration#configure-a-log-analytics-workspace) kullanarak `ServiceTier` parametresi. 

## <a name="troubleshooting-why-log-analytics-is-no-longer-collecting-data"></a>Log Analytics, artık veri topluyor neden sorunlarını giderme

Eski ücretsiz fiyatlandırma katmanı olan ve bir günde 500 MB veri göndermiş, günün geri kalanı için veri toplamayı durdurur. Günlük sınıra ulaşılması Log Analytics Veri toplamayı durdurur ya da veri eksik gibi görünüyor yaygın bir nedenidir.  Log Analytics'e veri toplamayı başlatır ve durdurur ' % s'olay türü işlemi oluşturur. Aramada, günlük sınırınıza ulaşmanız ve verileri eksik olursa denetlemek için aşağıdaki sorguyu çalıştırın: 

```kusto
Operation | where OperationCategory == 'Data Collection Status'
```

Veri toplama sona erdiğinde, OperationStatus olduğu **uyarı**. Veri toplama başladığında OperationStatus olduğu **başarılı**. Aşağıdaki tabloda veri toplamayı durdurur nedenleri açıklanır ve veri koleksiyonu devam bir önerilen eylem:  

|Neden koleksiyonu durdurur| Çözüm| 
|-----------------------|---------|
|Eski ücretsiz fiyatlandırma katmanı günlük sınırına ulaşıldı |Koleksiyon otomatik olarak yeniden başlatmak için sonraki güne kadar bekleyin veya Ücretli fiyatlandırma katmanı olarak değiştirme.|
|Günlük üst sınır çalışma alanınızın ulaşıldı|Koleksiyon için otomatik olarak yeniden başlatılmasını bekleyin veya günlük veri birimi sınırı açıklanan artış maksimum günlük veri hacmi yönetin. Günlük sınır sıfırlama saati gösterir açıktır **veri hacmi Yönetimi** sayfası. |
|Azure aboneliği askıya alınma durumuna nedeniyle oluşturulur.<br> Ücretsiz deneme sürümü sona erdi<br> Azure pass süresi doldu<br> Aylık harcama sınırına (örneğin bir MSDN veya Visual Studio abonelik üzerinde)|Ücretli aboneliğe dönüştürme<br> Sınırı kaldırın veya sınır sıfırlar kadar bekleyin|

Veri toplamayı durdurur bozulma olduğunda bilgilendirilmeniz için açıklanan adımları kullanın. *oluşturma günlük veri üst sınırında* veri toplamayı durdurur olduğunda bilgilendirilmeniz için uyarı. İçinde açıklanan adımları kullanın [bir eylem grubu oluşturma](action-groups.md) uyarı kuralı için bir e-posta, Web kancasını veya runbook eylemini yapılandırmak için. 

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>Kullanımın neden beklenenden daha yüksek olduğuyla ilgili sorunları giderme

Yüksek kullanımın nedeni aşağıdakilerden biri veya her ikisidir:
- Log Analytics çalışma alanına veri gönderme beklenenden çok düğüm
- Log Analytics çalışma alanına gönderilen beklenenden daha fazla veri

## <a name="understanding-nodes-sending-data"></a>Veri gönderen düğüm anlama

Geçen ayın her günü sinyal bildiren bilgisayarların sayısını anlamak için kullanın

```kusto
Heartbeat | where TimeGenerated > startofday(ago(31d))
| summarize dcount(Computer) by bin(TimeGenerated, 1d)    
| render timechart
```

Çalışma alanını eski başına fiyatlandırma katmanında düğüm varsa, düğüm olarak faturalandırılır bilgisayarların listesini almak için gönderen düğümleri için konum **veri türleri faturalandırılır** (bazı veri türleri ücretsizdir). Bunu yapmak için `_IsBillable` [özelliği](log-standard-properties.md#_isbillable) ve tam etki alanı adının en soldaki alan kullanın. Bu, faturalandırılan verilerle bilgisayarların listesini döndürür:

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize TotalVolumeBytes=sum(_BilledSize) by computerName
```

Görülen Faturalanabilir düğüm sayısı olarak tahmin edilebilir: 

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| billableNodes=dcount(computerName)
```

> [!NOTE]
> Bu `union withsource = tt *` veri türlerinde taramaları çalıştırmak pahalı olduğundan tutumlu sorgular. Bu sorgu bilgisayar başına bilgi kullanım veri türü ile sorgulama eski biçimini değiştirir.  

Daha doğru bir hesaplama, aslında neler faturalandırılır, saatlik faturalandırılan veri türleri gönderme bilgisayar sayısını almaktır. (Eski düğüm başına fiyatlandırma katmanında çalışma alanları için Log Analytics, saatlik olarak faturalandırılırsınız gereken düğüm sayısını hesaplar.) 

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize billableNodes=dcount(computerName) by bin(TimeGenerated, 1h) | sort by TimeGenerated asc
```

## <a name="understanding-ingested-data-volume"></a>Anlama alınan veri hacmi

Üzerinde **kullanım ve Tahmini maliyetler** sayfasında *çözüm başına veri alımı* grafik, toplam gönderilen veri hacmini ve ne kadar her çözüm tarafından gönderilen verilerin gösterir. Bu sayede olup genel veri kullanımı (veya belirli bir çözüm tarafından kullanım) artıyor mu gibi eğilimleri belirlemek sabit kaldığını veya azaldığını. Bu oluşturmak için kullanılan sorgu

```kusto
Usage | where TimeGenerated > startofday(ago(31d))| where IsBillable == true
| summarize TotalVolumeGB = sum(Quantity) / 1024 by bin(TimeGenerated, 1d), Solution| render barchart
```

Unutmayın yan tümcesi "nerede IsBillable = true" veri türleri için ücretsizdir alımı belirli çözümlerinden filtreler. 

Bkz: veri eğilimlerini IIS günlükler nedeniyle verileri incelemek isterseniz, örneğin belirli veri türleri için daha fazla sınırlandıramazsınız gidebilir:

```kusto
Usage | where TimeGenerated > startofday(ago(31d))| where IsBillable == true
| where DataType == "W3CIISLog"
| summarize TotalVolumeGB = sum(Quantity) / 1024 by bin(TimeGenerated, 1d), Solution| render barchart
```

### <a name="data-volume-by-computer"></a>Bilgisayara göre veri hacmi

Görmek için **boyutu** Faturalanabilir olayların, bilgisayar başına alınan `_BilledSize` [özelliği](log-standard-properties.md#_billedsize), bayt cinsinden boyut sağlar:

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize Bytes=sum(_BilledSize) by  computerName | sort by Bytes nulls last
```

`_IsBillable` [Özelliği](log-standard-properties.md#_isbillable) alınan veri ücret uygulanabilir olup olmadığını belirtir.

Sayısını görmek için **Faturalanabilir** her bilgisayar, içe alınan olayları kullanma 

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize eventCount=count() by computerName  | sort by count_ nulls last
```

Sayıları Faturalandırılabilir veri türleri için belirli bir bilgisayar için veri gönderdiğini görmek istiyorsanız, bu seçeneği kullanın:

```kusto
union withsource = tt *
| where Computer == "computer name"
| where _IsBillable == true 
| summarize count() by tt | sort by count_ nulls last
```

### <a name="data-volume-by-azure-resource-resource-group-or-subscription"></a>Azure kaynak, kaynak grubuna veya aboneliğe göre veri hacmi

Verileri Azure üzerinde barındırılan düğümlerden alabileceğiniz **boyutu** alınan Faturalanabilir olayların __bilgisayar başına__, _ResourceId kullanın [özelliği](log-standard-properties.md#_resourceid), tam yolu sağlar Kaynak:

```kusto
union withsource = tt * 
| where _IsBillable == true 
| summarize Bytes=sum(_BilledSize) by _ResourceId | sort by Bytes nulls last
```

Azure'da barındırılan düğümlerden veri alabileceğiniz **boyutu** alınan Faturalanabilir olayların __Azure aboneliği başına__, ayrıştırma `_ResourceId` özelliği olarak:

```kusto
union withsource = tt * 
| where _IsBillable == true 
| parse tolower(_ResourceId) with "/subscriptions/" subscriptionId "/resourcegroups/" 
    resourceGroup "/providers/" provider "/" resourceType "/" resourceName   
| summarize Bytes=sum(_BilledSize) by subscriptionId | sort by Bytes nulls last
```

Değiştirme `subscriptionId` için `resourceGroup` Azure kaynak grubu bazında Faturalanabilir içe alınan veri hacmi gösterilir. 


> [!NOTE]
> Bazı kullanım veri türünde alanlar yine de şema sırada, kullanım dışı bırakıldı ve değerlerine artık doldurulur olur. Bunlar **bilgisayar** alımıyla ilgili alanları yanı sıra (**TotalBatches**, **BatchesWithinSla**, **BatchesOutsideSla**,  **BatchesCapped** ve **AverageProcessingTimeMs**.

### <a name="querying-for-common-data-types"></a>Ortak veri türleri için sorgulama

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
| Güvenlik olayları            | [Yaygın veya en az güvenlik olaylarını](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection#data-collection-tier) seçin <br> Güvenlik denetimi ilkesini yalnızca gerekli olayları toplayacak şekilde değiştirin. Özellikle, şunlarla ilgili olayları toplamak gerekip gerekmediğini gözden geçirin: <br> - [filtre platformu denetimi](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [kayıt defteri denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941614(v%3dws.10))<br> - [dosya sistemi denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772661(v%3dws.10))<br> - [çekirdek nesnesi denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941615(v%3dws.10))<br> - [tanıtıcı değiştirme denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772626(v%3dws.10))<br> - çıkarılabilir depolama birimi denetimi |
| Performans sayaçları       | [Performans sayacı yapılandırmasını](data-sources-performance-counters.md) şöyle değiştirin: <br> - Koleksiyonun sıklığını azaltın <br> - Performans sayaçlarının sayısını azaltın |
| Olay günlükleri                 | [Olay günlüğü yapılandırmasını](data-sources-windows-events.md) şöyle değiştirin: <br> - Toplanan olay günlüklerinin sayısını azaltın <br> - Yalnızca gerekli olay düzeylerini toplayın. Örneğin, *Bilgi* düzeyindeki olayları toplamayın |
| Syslog                     | [Syslog yapılandırmasını](data-sources-syslog.md) şu şekilde değiştirin: <br> - Toplanan tesislerin sayısını azaltın <br> - Yalnızca gerekli olay düzeylerini toplayın. Örneği *Bilgi* ve *Hata Ayıklama* düzeyindeki olayları toplamayın |
| AzureDiagnostics           | Aşağıdaki amaçlarla kaynak günlüğü koleksiyonunu değiştirin: <br> - Log Analytics’e günlük gönderen kaynak sayısını azaltma <br> - Yalnızca gerekli günlükleri toplama |
| Çözüm ihtiyacı olmayan bilgisayarlardan toplanan çözüm verileri | Yalnızca gerekli bilgisayar gruplarından veri toplamak için [çözüm hedefleme](../insights/solution-targeting.md) özelliğini kullanın. |

### <a name="getting-security-and-automation-node-counts"></a>Başlangıç güvenlik ve Otomasyon düğüm sayısı

"Fiyatlandırma katmanı düğümde (OMS)" olduğunuz sonra düğüm ve çözüm sayısına göre kullanacağınız Insights sayısı ve Analytics düğümleri için faturalandırılır gösterilecek tabloda ücretlendirilir **kullanım ve tahmini maliyet**sayfası.  

Güvenlik düğümleri sayısını görmek için sorguyu kullanabilirsiniz:

```kusto
union
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
| count
```

Farklı bir Otomasyon düğüm sayısını görmek için sorguyu kullanın:

```kusto
 ConfigurationData 
 | where (ConfigDataType == "WindowsServices" or ConfigDataType == "Software" or ConfigDataType =="Daemons") 
 | extend lowComputer = tolower(Computer) | summarize by lowComputer 
 | join (
     Heartbeat 
       | where SCAgentChannel == "Direct"
       | extend lowComputer = tolower(Computer) | summarize by lowComputer, ComputerEnvironment
 ) on lowComputer
 | summarize count() by ComputerEnvironment | sort by ComputerEnvironment asc
```

## <a name="create-an-alert-when-data-collection-is-high"></a>Veri toplama yüksek olduğunda bir uyarı oluştur

Bu bölümde, aşağıdaki durumlarda nasıl uyarı oluşturulacağı açıklanır:
- Veri hacmi belirtilen bir miktarı aştığında.
- Veri hacminin belirtilen bir miktarı aşacağı tahmin edildiğinde.

Azure Uyarıları, arama sorguları kullanan [günlük uyarılarını](alerts-unified-log.md) destekler. 

Aşağıdaki sorgu, son 24 saatte 100 GB'den fazla veri toplandığında bir sonuç verir:

```kusto
union withsource = $table Usage 
| where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true 
| extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type 
| where DataGB > 100
```

Aşağıdaki sorgu, ne zaman bir günde 100 GB'den fazla veri toplanacağını tahmin etmek için basit bir formül kullanır: 

```kusto
union withsource = $table Usage 
| where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true 
| extend Type = $table 
| summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type 
| where EstimatedGB > 100
```

Farklı bir veri hacminde uyarıda bulunmak için, sorgulardaki 100 değerini uyarılmak istediğiniz GB sayısıyla değiştirin.

Toplanan veri beklenen miktarı aştığında size bildirilmesini sağlamak için, [yeni günlük uyarısı oluşturma](alerts-metric.md) başlığı altında açıklanan adımları kullanın.

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

Günlük uyarısı ölçütlerle eşleştiğinde bilgilendirme yapılması için var olan bir [Eylem Grubunu](action-groups.md) kullanın veya yeni bir tane oluşturun.

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

Günlük uyarısı ölçütlerle eşleştiğinde bilgilendirme yapılması için var olan bir [Eylem Grubunu](action-groups.md) kullanın veya yeni bir tane oluşturun.

Uyarı aldığınızda, kullanımın neden beklenenden fazla olduğu konusundaki sorunları gidermek için aşağıdaki bölümde yer alan adımları kullanın.

## <a name="limits-summary"></a>Sınırları özeti

Bazıları fiyatlandırma katmanında Log Analytics temelinde bağımlı bazı ek Log Analytics sınırı yoktur. Bunlar belgelenen [burada](https://docs.microsoft.com/azure/azure-subscription-service-limits#log-analytics-workspaces).


## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [günlük aramaları Azure İzleyici günlüklerine](../log-query/log-query-overview.md) arama dilini nasıl kullanacağınızı öğrenin. Kullanım verilerinde başka analizler yapmak için arama sorgularını kullanabilirsiniz.
- Bir arama ölçütü karşılandığında size bildirilmesini sağlamak için, [yeni günlük uyarısı oluşturma](alerts-metric.md) başlığı altında açıklanan adımları kullanın.
- Yalnızca gerekli bilgisayar gruplarından veri toplamak için [çözüm hedefleme](../insights/solution-targeting.md) özelliğini kullanın.
- Bir etkin olay Koleksiyonu İlkesi yapılandırmak için gözden [Azure Güvenlik Merkezi filtreleme İlkesi](../../security-center/security-center-enable-data-collection.md).
- [Performans sayacı yapılandırmasını](data-sources-performance-counters.md) değiştirin.
- Olay toplama ayarlarınızda değişiklik yapmak için, [olay günlüğü yapılandırması](data-sources-windows-events.md) konusunu gözden geçirin.
- Syslog koleksiyonu ayarlarınızda değişiklik yapmak için, [syslog yapılandırması](data-sources-syslog.md) konusunu gözden geçirin.