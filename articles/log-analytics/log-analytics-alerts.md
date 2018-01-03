---
title: "Azure günlük analizi, uyarıları anlama | Microsoft Docs"
description: "Günlük analizi uyarılarını OMS deponuzun önemli bilgileri tanımlamak ve önceden sorunları size bildiren veya düzeltmenize girişiminde Eylemler çağırma.  Bu makalede uyarı kuralları ve nasıl tanımlanır farklı türleri açıklanmaktadır."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/13/2017
ms.author: bwren
ms.openlocfilehash: a0897113660f764cb23239b066bc93c479a9a553
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
---
# <a name="understanding-alerts-in-log-analytics"></a>Günlük analizi, uyarıları anlama

Günlük analizi uyarılarını günlük analizi deponuzun önemli bilgiler tanımlayın.  Bu makalede, günlük analizi çalışma nasıl uyarı kuralları ayrıntılarını sağlar ve uyarı kuralları farklı türleri arasındaki farklar açıklanmaktadır.

Uyarı kuralları oluşturma işlemi için aşağıdaki makalelere bakın:

- Kullanarak uyarı kuralları oluşturmak [Azure portalı](log-analytics-alerts-creating.md)
- Kullanarak uyarı kuralları oluşturmak [Resource Manager şablonu](../operations-management-suite/operations-management-suite-solutions-resources-searches-alerts.md)
- Kullanarak uyarı kuralları oluşturmak [REST API'si](log-analytics-api-alerts.md)


## <a name="alert-rules"></a>Uyarı kuralları

Uyarılar, günlük aramaları düzenli aralıklarla otomatik olarak çalışacak uyarı kuralları tarafından oluşturulur.  Günlük arama sonuçlarını belirli ölçütlere uyan varsa bir uyarı kaydı oluşturulur.  Kural, ileriye dönük olarak uyarı bildiren veya başka bir işlem çağırmak için bir veya daha fazla eylemleri otomatik olarak çalıştırabilirsiniz.  Uyarı kuralları farklı türlerde farklı mantık bu analizi yapmak için kullanın.

![Log Analytics uyarıları](media/log-analytics-alerts/overview.png)

Uyarı kuralları tarafından aşağıdaki ayrıntıları tanımlanmıştır:

- **Günlük arama**.  Uyarı kural her çalıştığında sorgusu gönderir.  Bu sorgu tarafından döndürülen kayıtları bir uyarı oluşturulup oluşturulmayacağını belirlemek için kullanılır.
- **Zaman penceresi**.  Sorgu için zaman aralığını belirtir.  Sorgu, geçerli zaman aralığında oluşturulan kayıtları döndürür.  Bu, 5 dakika ile 24 saat arasında herhangi bir değer olabilir. Örneğin, zaman penceresi 60 dakika olarak ayarlanmıştır ve sorgu 13: 15'te çalıştırırsanız, yalnızca saat 12: 15'e ve 13: 15'te arasında oluşturulan kayıtları döndürülür.
- **Sıklık**.  Sorgunun ne sıklıkta çalıştırılması gerektiğini belirtir. 5 dakika ile 24 saat arasında herhangi bir değer olabilir. Eşit veya bu zaman penceresi'den daha az olmalıdır.  Değeri zaman penceresi'den büyükse, eksik kayıtları riski oluşur.<br>Örneğin, 30 dakikalık bir zaman penceresi ve 60 dakika sıklığını göz önünde bulundurun.  Sorgu 1: 00'dan çalıştırırsanız, 12:30 ve 1:00 arasında kayıt döndürür.  Sorguyu çalıştırabilir sonraki 2:00 kayıtlar 1:30 ve 2:00 arasında ne zaman döndürecektir süresidir.  1:00-1:30 arasında oluşturulan kayıtları hiçbir zaman değerlendirilmesi.
- **Eşik**.  Günlük arama sonuçlarını, bir uyarının oluşturulması gerekip gerekmediğini belirlemek için değerlendirilir.  Eşik uyarı kuralları farklı türleri için farklıdır.

Günlük analizi her uyarı kuralı iki türlerinden biridir.  Bu türleri izleyen bölümlerde ayrıntılı olarak açıklanmıştır.

- **[Sonuç sayısı](#number-of-results-alert-rules)**. Günlük araması tarafından döndürülen sayı kayıtları belirli sayıda aştıklarında oluşturulan tek bir uyarı.
- **[Ölçüm ölçüm](#metric-measurement-alert-rules)**.  Belirtilen eşiğini aşan değerlerle günlük arama sonuçlarında her nesne için oluşturulan bir uyarı.

Uyarı kuralı türleri arasındaki farkları aşağıdaki gibidir.

- **Sonuç sayısı** uyarı kuralı her zaman tek bir uyarı while Oluştur **ölçüm ölçüm** uyarı kuralı eşiği aşıyor her nesne için bir uyarı oluşturur.
- **Sonuç sayısı** uyarı kuralları tek bir kez eşiği aştığında bir uyarı oluşturmak. **Ölçüm ölçüm** uyarı kuralları, bir uyarı oluşturabilir, eşik aşıldığında, belirli bir süre boyunca belirli sayıda.

## <a name="number-of-results-alert-rules"></a>Sonuçları uyarı kuralları sayısı
**Sonuç sayısı** uyarı kuralları arama sorgusu tarafından döndürülen kayıt sayısını belirtilen eşiği aştığında tek bir uyarı oluştur.

### <a name="threshold"></a>Eşik
Eşiği bir **sonuç sayısı** yalnızca büyük veya belirli bir değerden daha az uyarı kuralı.  Günlük araması tarafından döndürülen kayıt sayısını bu ölçütlere uyan varsa bir uyarı oluşturulur.

### <a name="scenarios"></a>Senaryolar

#### <a name="events"></a>Olaylar
Bu tür bir uyarı kuralı olaylar Windows olay günlükleri, Syslog, gibi ile çalışmak için idealdir ve özel günlüğe kaydeder.  Belirli hata olayı oluştururken ya da birden çok hata olayları belirli zaman penceresi içinde oluşturulan bir uyarı oluşturmak isteyebilirsiniz.

Tek bir olayda uyarmak için 0 ile sıklığı ve 5 dakika zaman penceresi'den büyük sonuç sayısını ayarlayın.  Her 5 dakikada bir ve onay son kez sorgu çalıştırıldığında bu yana oluşturulan tek bir olay geçtiği, sorguyu çalıştırır.  Bir uzun sıklığı toplanmakta olan olay ile oluşturulan uyarı arasındaki süre geciktirebilir.

Bazı uygulamalar, mutlaka bir uyarı oluşturmadan döndürmemelidir hatayla zaman oturum açabilir.  Örneğin, uygulama hata olayı oluşturan işlemi yeniden deneyin ve bir sonraki sefer başarılı.  Bu durumda, birden çok olay belirli zaman penceresi içinde oluşturulan sürece uyarı oluşturma istemeyebilirsiniz.  

Bazı durumlarda, bir olay olmaması durumunda bir uyarı oluşturmak isteyebilirsiniz.  Örneğin, bir işlem düzgün çalıştığını göstermek için normal olaylarla oturum açabilir.  Belirli bir zaman penceresi içinde aşağıdaki olaylardan biri oturum değil, bir uyarı oluşturulmalıdır.  Bu durumda, eşik ayarlamalısınız **değerinden 1**.

#### <a name="performance-alerts"></a>Performans uyarıları
[Performans verileri](log-analytics-data-sources-performance-counters.md) olaylarına benzer OMS depo kayıt olarak depolanır.  Bir performans sayacı belirli bir eşiği aştığında uyarı istiyorsanız, bu eşiği sorguda bulunması gerekir.

Örneğin, işlemci çalıştığında uyarı istiyorsanız %90 üzerindeki aşağıdaki gibi bir sorgu ile eşiği için uyarı kuralı kullanacağınız **0'dan büyük**.

    Perf | where ObjectName=="Processor" and CounterName=="% Processor Time" and CounterValue>90

    

İşlemci üzerinde belirli bir zaman penceresi için % 90 ortalaması olduğunda uyarır istiyorsanız, aşağıdaki gibi bir sorgu ile eşiği için uyarı kuralı kullanırsınız **0'dan büyük**.

    Perf | where ObjectName=="Processor" and CounterName=="% Processor Time" | where CounterValue>90 | summarize avg(CounterValue) by Computer

    
>[!NOTE]
> Çalışma alanınız Henüz için değil yükseltildiyse [yeni günlük analizi sorgu dili](log-analytics-log-search-upgrade.md), yukarıdaki sorguları ikinci kullanarak aşağıdaki değişeceğinden sonra [ölçmek komutu](log-analytics-search-reference.md#commands):`Type=Perf ObjectName=Processor CounterName="% Processor Time" CounterValue>90`
> `Type=Perf ObjectName=Processor CounterName="% Processor Time" | measure avg(CounterValue) by Computer | where AggregatedValue>90`


## <a name="metric-measurement-alert-rules"></a>Ölçüm ölçüm uyarı kuralları

>[!NOTE]
> Ölçüm ölçüm uyarı kuralları şu anda genel önizlemede.

**Ölçüm ölçüm** uyarı kuralları bir sorguda belirtilen eşiği aşarsa bir değerle her nesne için bir uyarı oluştur.  Aşağıdaki ayrı farkları sahip oldukları **sonuç sayısı** uyarı kuralları.

#### <a name="log-search"></a>Günlük araması
Herhangi bir sorgu için kullanabilirsiniz, ancak bir **sonuç sayısı** uyarı kuralı, bir ölçüm ölçüm uyarı kuralı için sorgu belirli koşullar yoktur.  İçermesi gerekir bir [ölçmek komut](log-analytics-search-reference.md#commands) belirli bir alan sonuçları gruplandırmak için. Bu komut, aşağıdaki öğeleri eklemeniz gerekir.

- **Toplama işlevi**.  Gerçekleştirilen hesaplama ve büyük olasılıkla bir sayısal belirler toplanacak alan.  Örneğin, **count()** kayıt sayısını sorgudan döndürülecek **avg(CounterValue)** aralığı içinde CounterValue alanının ortalamasını döndürür.
- **Alan grup**.  Bu alan her örneği için bir toplu değeri içeren bir kayıt oluşturulur ve her biri için bir uyarı oluşturulabilir.  Örneğin, her bilgisayar için bir uyarı oluşturmak istiyorsanız, kullanacağınız **bilgisayar tarafından**.   
- **Aralığı**.  Birleşik verileri zaman aralığını tanımlar.  Örneğin, belirttiğiniz **5minutes**, bir kayıt her örneği için uyarı belirtilen zaman penceresi üzerinden 5 dakikalık aralıklarla toplanan grup alanının oluşturulması.

#### <a name="threshold"></a>Eşik
Ölçüm ölçüm uyarı kuralları için eşik bir toplam değerini ve bir dizi tarafından tanımlanır.  Herhangi bir veri noktasını günlük arama bu değeri aştığında bir ihlal dikkate almıştır.  Dizi içinde sonuçlarındaki herhangi bir nesne için belirtilen değeri aşarsa bir uyarı bu nesne için oluşturulur.

#### <a name="example"></a>Örnek
Burada herhangi bir bilgisayar işlemci kullanımı % 90'ın üç kez üzerinde 30 dakika aşılırsa bir uyarı isteyen bir senaryo düşünün.  Bir uyarı kuralı aşağıdaki ayrıntılarla oluşturursunuz.  

**Sorgu:** Perf | burada ObjectName "İşlemci" ve CounterName == "% işlemci zamanı" == | AggregatedValue özetlemek bin (TimeGenerated, 5 m), bilgisayar tarafından avg(CounterValue) =<br>
**Zaman penceresi:** 30 dakika<br>
**Uyarı sıklığı:** 5 dakika<br>
**Toplam değer:** 90 büyüktür<br>
**Tetikleyici uyarı temel alarak:** toplam ihlal 2'den büyük<br>

Sorgu her bilgisayar için bir ortalama değer 5 dakika aralıklarla oluşturursunuz.  Bu sorguyu her 5 dakikada bir toplanan veriler için önceki 30 dakika boyunca çalışır.  Örnek verileri varsayılan olarak, üç bilgisayarlar için aşağıda gösterilmiştir.

![Örnek sorgu sonuçları](media/log-analytics-alerts/metrics-measurement-sample-graph.png)

Bunlar % 90 eşiği 3 kez zaman penceresi ihlal beri bu örnekte, ayrı uyarılar srv02 ve srv03 oluşturulması.  Varsa **tetikleyici uyarı temel alarak:** üzere değiştirilmiştir **art arda** ardışık 3 örnek eşiği ihlal bu yana bir uyarı yalnızca srv03 için oluşturulacak sonra.

## <a name="alert-records"></a>Uyarı kaydeder
Günlük analizi uyarı kuralları tarafından oluşturulan uyarı kayıtlarına sahip bir **türü** , **uyarı** ve **SourceSystem** , **OMS**.  Aşağıdaki tabloda özellikleri sahiptirler.

| Özellik | Açıklama |
|:--- |:--- |
| Tür |*Uyarı* |
| SourceSystem |*OMS* |
| *Nesne*  | [Ölçüm ölçüm uyarıları](#metric-measurement-alert-rules) Grup alan için bir özelliğe sahiptir.  Örneğin, bilgisayarda günlük arama grupları uyarı kaydıyla varsa bilgisayar ada sahip bir bilgisayar alan değeri olarak.
| AlertName |Uyarı adı. |
| AlertSeverity |Uyarı önem derecesi. |
| LinkToSearchResults |Günlük analizi günlük kayıtları uyarı oluşturulan sorgudan döndüren bir arama bağlayın. |
| Sorgu |Çalıştırıldığı sorgu metni. |
| QueryExecutionEndTime |Sorgu için zaman aralığı sonu. |
| QueryExecutionStartTime |Sorgu için zaman aralığını başlangıcı. |
| ThresholdOperator | Uyarı kuralı tarafından kullanılan işleci. |
| ThresholdValue | Uyarı kuralı tarafından kullanılan değeri. |
| TimeGenerated |Uyarının oluşturulduğu tarih ve saat. |

Tarafından oluşturulan uyarı kayıtları diğer tür vardır [uyarı yönetim çözümü](log-analytics-solution-alert-management.md) kullanarak ve [Power BI dışarı aktarır](log-analytics-powerbi.md).  Bunların tümüne sahip bir **türü** , **uyarı** göre ayırt edilen ancak kendi **SourceSystem**.


## <a name="next-steps"></a>Sonraki adımlar
* Yükleme [uyarı yönetimi çözümü](log-analytics-solution-alert-management.md) System Center Operations Manager'dan toplanan uyarılar ile birlikte günlük analizi oluşturulan uyarıların çözümlemek için.
* Daha fazla bilgi edinin [oturum aramaları](log-analytics-log-searches.md) uyarılar oluşturabilir.
* İzlenecek yollar için tamamlamak [bir Web kancası yapılandırma](log-analytics-alerts-webhooks.md) bir uyarı kuralı ile.  
* Nasıl yazılacağını öğrenmek [Azure automation'daki runbook'lar](https://azure.microsoft.com/documentation/services/automation) uyarılar tarafından tanımlanan sorunları düzeltmek için.
