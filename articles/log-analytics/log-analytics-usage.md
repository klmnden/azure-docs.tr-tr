---
title: Log Analytics'te veri kullanımını çözümleme | Microsoft Belgeleri
description: Log Analytics’e ne kadar veri gönderileceğini değerlendirmek ve nelerin öngörülemez artışlara neden olabileceğini belirlemek için Log Analytics’te Kullanım ve tahmini maliyet panosunu kullanın.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: 74d0adcb-4dc2-425e-8b62-c65537cef270
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/19/2018
ms.author: magoedte
ms.openlocfilehash: 9a9c898cf0f2e0b1387bbc2ac18b5009838d138b
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36317312"
---
# <a name="analyze-data-usage-in-log-analytics"></a>Log Analytics'te veri kullanımını çözümleme
Log Analytics, toplanan veri miktarı, verileri hangi kaynakların gönderdiği ve gönderilen farklı veri türleri hakkındaki bilgileri içerir.  Veri kullanımını gözden geçirmek ve analiz etmek için **Log Analytics Kullanımı** panosunu kullanın. Panoda her çözüm tarafından ne kadar veri toplandığı ve bilgisayarlarınızın ne kadar veri gönderdiği gösterilir.

## <a name="understand-the-usage-dashboard"></a>Kullanım panosunu anlama
**Log Analytics kullanım** panosu aşağıdaki bilgileri gösterir:

- Veri hacmi
    - Zaman içindeki veri hacmi (geçerli zaman kapsamınıza bağlı olarak)
    - Çözüme göre veri hacmi
    - Bir bilgisayar ile ilişkilendirilmemiş olan veriler
- Bilgisayarlar
    - Veri gönderen bilgisayarlar
    - Son 24 içinde hiç veri göndermeyen bilgisayarlar
- Teklifler
    - İçgörü ve Analiz düğümleri
    - Otomasyon ve Kontrol düğümleri
    - Güvenlik düğümleri  
- Performans
    - Verileri toplamak ve dizinlemek için harcanan süre  
- Sorgu listesi

![Kullanım ve maliyet panosu](./media/log-analytics-manage-cost-storage/usage-estimated-cost-dashboard-01.png)<br>
)

### <a name="to-work-with-usage-data"></a>Kullanım verileriyle çalışma
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.<br><br> ![Azure Portal](./media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
3. Log Analytics çalışma alanlarınızın listesinde, bir çalışma alanı seçin.
4. Sol bölmedeki listeden **Kullanım ve tahmini maliyetler**’i seçin.
5. **Kullanım ve tahmini maliyetler** panosunda, **Zaman: Son 24 saat** değerini seçerek zaman aralığı üzerinde değişiklik yapabilir ve zaman aralığını değiştirebilirsiniz.<br><br> ![zaman aralığı](./media/log-analytics-usage/usage-time-filter-01.png)<br><br>
6. İlginizi çeken alanları gösteren kategori dikey pencerelerini görüntüleyin. Bir dikey pencere seçin [Günlük Arama](log-analytics-log-searches.md)’te ayrıntılarını görüntülemek istediğiniz öğeye tıklayın.<br><br> ![örnek veri kullanım kpi’si](media/log-analytics-usage/data-volume-kpi-01.png)<br><br>
7. Günlük Arama panosunda aramanın döndürdüğü sonuçları inceleyin.<br><br> ![örnek kullanım günlüğü araması](./media/log-analytics-usage/usage-log-search-01.png)

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

Toplanan veri beklenen miktarı aştığında size bildirilmesini sağlamak için, [yeni günlük uyarısı oluşturma](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) başlığı altında açıklanan adımları kullanın.

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

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>Kullanımın neden beklenenden daha yüksek olduğuyla ilgili sorunları giderme
Kullanım panosu, kullanımın (dolayısıyla da maliyetin) neden beklediğinizden yüksek olduğunu belirlemenize yardımcı olur.

Yüksek kullanımın nedeni aşağıdakilerden biri veya her ikisidir:
- Log Analytics'da beklenenden daha fazla veri gönderiliyordur
- Log Analytics'e beklenenden daha fazla düğüm veri gönderiyordur

### <a name="check-if-there-is-more-data-than-expected"></a>Beklenenden daha fazla veri olup olmadığını denetleme 
Kullanım sayfasında, çok veri toplanmasına neyin neden olduğunu belirlemenize yardımcı olacak iki önemli bölüm vardır.

*Zaman içindeki veri hacmi* grafiği, gönderilen toplam veri hacmini ve en çok veriyi gönderen bilgisayarları gösterir. Üstteki grafik, genel olarak veri kullanımınızın arttığını, sabit kaldığını veya azaldığını görmenizi sağlar. Bilgisayar listesi, en çok veri gönderen 10 bilgisayarı gösterir.

*Çözüme göre veri hacmi* grafiği, her çözüm tarafından gönderilen veri hacmini ve en çok veri gönderen çözümleri gösterir. Üstteki grafik, zaman içinde her çözüm tarafından gönderilen toplam veri hacmini gösterir. Bu bilgiler bir çözümün zaman içinde daha fazla veri gönderdiğini, yaklaşık aynı miktarda veri gönderdiğini veya daha az veri gönderdiğini belirlemenize olanak tanır. Çözüm listesinde, en çok veriyi gönderen 10 çözüm gösterilir. 

Bu iki grafik, tüm verileri görüntüler. Bazı veriler faturalanabilir, bazıları ise ücretsizdir. Yalnızca faturalanabilir verilere odaklanmak için arama sayfasındaki sorguyu `IsBillable=true` ifadesini içerecek şekilde değiştirin.  

![veri hacmi grafikleri](./media/log-analytics-usage/log-analytics-usage-data-volume.png)

*Zaman içinde veri hacmi* grafiğine bakın. Belirli bir bilgisayara en çok veriyi gönderen çözümleri ve veri türlerini görmek için, bilgisayarın adına tıklayın. Listedeki ilk bilgisayarın adına tıklayın.

Aşağıdaki ekran görüntüsünde, bilgisayar için en fazla gönderilen veri *Günlük Yönetimi / Perf* veri türündedir.<br><br> ![bilgisayar için veri hacmi](./media/log-analytics-usage/log-analytics-usage-data-volume-computer.png)<br><br>

Ardından, *Kullanım* panosuna dönün ve *Çözüme göre veri hacmi* grafiğine bakın. Bir çözümle ilgili en fazla veriyi gönderen bilgisayarları görmek için, listede çözümün adına tıklayın. Listedeki ilk çözümün adına tıklayın. 

Aşağıdaki ekran görüntüsünde, Günlük Yönetimi çözümü için en çok veriyi *mycon* bilgisayarının gönderdiği doğrulanır.<br><br> ![çözüm için veri hacmi](./media/log-analytics-usage/log-analytics-usage-data-volume-solution.png)<br><br>

Gerekirse, bir çözüm veya veri türü içindeki büyük hacimleri belirlemek için ek çözümlemeler yapın. Örnek sorgular şunları içerir:

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

Toplanan günlük hacmini azaltmak için aşağıdaki adımları kullanın:

| Yüksek veri hacminin kaynağı | Veri hacmi nasıl azaltılır |
| -------------------------- | ------------------------- |
| Güvenlik olayları            | [Yaygın veya en az güvenlik olaylarını](https://blogs.technet.microsoft.com/msoms/2016/11/08/filter-the-security-events-the-oms-security-collects/) seçin <br> Güvenlik denetimi ilkesini yalnızca gerekli olayları toplayacak şekilde değiştirin. Özellikle, şunlarla ilgili olayları toplamak gerekip gerekmediğini gözden geçirin: <br> - [filtre platformu denetimi](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [kayıt defteri denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941614(v%3dws.10))<br> - [dosya sistemi denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772661(v%3dws.10))<br> - [çekirdek nesnesi denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941615(v%3dws.10))<br> - [tanıtıcı değiştirme denetimi](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772626(v%3dws.10))<br> - çıkarılabilir depolama birimi denetimi |
| Performans sayaçları       | [Performans sayacı yapılandırmasını](log-analytics-data-sources-performance-counters.md) şöyle değiştirin: <br> - Koleksiyonun sıklığını azaltın <br> - Performans sayaçlarının sayısını azaltın |
| Olay günlükleri                 | [Olay günlüğü yapılandırmasını](log-analytics-data-sources-windows-events.md) şöyle değiştirin: <br> - Toplanan olay günlüklerinin sayısını azaltın <br> - Yalnızca gerekli olay düzeylerini toplayın. Örneğin, *Bilgi* düzeyindeki olayları toplamayın |
| Syslog                     | [Syslog yapılandırmasını](log-analytics-data-sources-syslog.md) şu şekilde değiştirin: <br> - Toplanan tesislerin sayısını azaltın <br> - Yalnızca gerekli olay düzeylerini toplayın. Örneği *Bilgi* ve *Hata Ayıklama* düzeyindeki olayları toplamayın |
| AzureDiagnostics           | Aşağıdaki amaçlarla kaynak günlüğü koleksiyonunu değiştirin: <br> - Log Analytics’e günlük gönderen kaynak sayısını azaltma <br> - Yalnızca gerekli günlükleri toplama |
| Çözüm ihtiyacı olmayan bilgisayarlardan toplanan çözüm verileri | Yalnızca gerekli bilgisayar gruplarından veri toplamak için [çözüm hedefleme](../operations-management-suite/operations-management-suite-solution-targeting.md) özelliğini kullanın. |

### <a name="check-if-there-are-more-nodes-than-expected"></a>Beklenenden çok düğüm olup olmadığını denetleme
*Düğüm başına (OMS)* fiyatlandırma katmanındaysanız kullandığınız düğüm ve çözüm sayısına göre ücretlendirilirsiniz. Kullanım panosunun *teklifler* bölümünde her tekliften kaç düğümün kullanıldığını görebilirsiniz.<br><br> ![kullanım panosu](./media/log-analytics-usage/log-analytics-usage-offerings.png)<br><br>

Belirli bir teklifle ilgili veri gönderen bilgisayarların tam listesini görüntülemek için **Tümünü göster...** öğesine tıklayın.

Yalnızca gerekli bilgisayar gruplarından veri toplamak için [çözüm hedefleme](../operations-management-suite/operations-management-suite-solution-targeting.md) özelliğini kullanın.

## <a name="next-steps"></a>Sonraki adımlar
* Arama dilini nasıl kullanacağınızı öğrenmek için bkz. [Log Analytics'te günlük aramaları](log-analytics-log-searches.md). Kullanım verilerinde başka analizler yapmak için arama sorgularını kullanabilirsiniz.
* Bir arama ölçütü karşılandığında size bildirilmesini sağlamak için, [yeni günlük uyarısı oluşturma](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) başlığı altında açıklanan adımları kullanın.
* Yalnızca gerekli bilgisayar gruplarından veri toplamak için [çözüm hedefleme](../operations-management-suite/operations-management-suite-solution-targeting.md) özelliğini kullanın.
* Etkili bir güvenlik olay koleksiyonu ilkesi yapılandırmak için, [Azure Güvenlik Merkezi filtreleme ilkesi](../security-center/security-center-enable-data-collection.md) konusunu gözden geçirin.
* [Performans sayacı yapılandırmasını](log-analytics-data-sources-performance-counters.md) değiştirin.
* Olay toplama ayarlarınızda değişiklik yapmak için, [olay günlüğü yapılandırması](log-analytics-data-sources-windows-events.md) konusunu gözden geçirin.
* Syslog koleksiyonu ayarlarınızda değişiklik yapmak için, [syslog yapılandırması](log-analytics-data-sources-syslog.md) konusunu gözden geçirin.
