---
title: Azure günlük analizi, uyarıları anlama | Microsoft Docs
description: Günlük analizi uyarılarını OMS deponuzun önemli bilgileri tanımlamak ve önceden sorunları size bildiren veya düzeltmenize girişiminde Eylemler çağırma.  Bu makalede uyarı kuralları ve nasıl tanımlanır farklı türleri açıklanmaktadır.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/13/2018
ms.author: bwren
ms.openlocfilehash: cf1842c6abbbfd767184d8f480a5f3a5fd654ed0
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32178679"
---
# <a name="understanding-alerts-in-log-analytics"></a>Günlük analizi, uyarıları anlama

> [!NOTE]
> Günlük analizi uyarıların [Azure'da genişletilen](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Bu makaledeki bilgiler hala günlük analizi aramalar Azure portalında uyarılar ayrıntılarını tanımlamak için kullanılabilir.

Log Analytics’teki uyarılar, Log Analytics deponuzdaki önemli bilgileri belirler. Bu makalede bazı büyük olasılıkla ağ gecikmesi veya kapasite işleme ve veri günlüğüne yürüten nedeniyle veri alımı ile sorgulanan, rastgele gecikme olan veri toplama sıklığını göre yapılması gereken tasarım kararlarını açıklanır Analizi çalışma alanı. Ayrıca, günlük analizi çalışma nasıl uyarı kuralları ayrıntılarını sağlar ve uyarı kuralları farklı türleri arasındaki farklar açıklanmaktadır.

Uyarı kuralları oluşturma işlemi için aşağıdaki makalelere bakın:

- Kullanarak uyarı kuralları oluşturmak [Azure portalı](log-analytics-alerts-creating.md)
- Kullanarak uyarı kuralları oluşturmak [Resource Manager şablonu](../operations-management-suite/operations-management-suite-solutions-resources-searches-alerts.md)
- Kullanarak uyarı kuralları oluşturmak [REST API'si](log-analytics-api-alerts.md)

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Çeşitli çözümler ve veri türü mevcuttur için veri toplama sıklığı hakkında ayrıntıları [veri koleksiyonu ayrıntıları](log-analytics-add-solutions.md#data-collection-details) çözümleri genel bakış makalesi. Bu makalede belirtildiği gibi koleksiyon sıklığı olarak yedi günde bir kez kadar sık olabilir *bildirim*. Veri toplama sıklığı bir alarm ayarlamadan önce göz önünde bulundurun ve anlamak önemlidir. 

- Toplama sıklığı ne sıklıkta OMS aracısı için günlük analizi veri gönderir belirler. Örneği için toplama sıklığını 10 dakikadır ve sistemdeki diğer bir gecikmeler varsa ardından zaman damgaları iletilen verilerin herhangi bir yere sıfır arasında ve 10 dakika havuza eklenmeden önce eski olabilir ve günlük analizi aranabilir.

- Bir uyarıyı tetikleyen önce böylece sorgulandığında kullanılabilir veri deposu için yazılmış olmalıdır. Yukarıda açıklanan gecikme nedeniyle toplama sıklığı ile veri sorguları için kullanılabilir olduğu saat ile aynı değil. Verileri tam olarak her 10 min toplanan, ancak örneği için verileri düzensiz aralıklarla veri deposunda mevcut değil. Hypothetically, sıfır, 10 ve 20 dakika aralıklarla toplanan verileri sırasıyla veya başka bir düzensiz aralıkta alım gecikmesine Etkileyenler 25, 28 ve 35 dakika arama için kullanılabilir. Bu gecikme en kötü durum belgelenen [günlük analizi için SLA](https://azure.microsoft.com/support/legal/sla/log-analytics/v1_1), toplama sıklığı veya ağ arasındaki gecikme süresi bilgisayar ve günlük analizi hizmeti tarafından sunulan bir gecikme içermez.


## <a name="alert-rules"></a>Uyarı kuralları

Düzenli aralıklarla otomatik olarak günlük aramaları çalıştıran uyarı kuralları tarafından uyarılar oluşturulur. Günlük arama sonuçlarını belirli ölçütlere uyan varsa bir uyarı kaydı oluşturulur. Daha sonra kural, uyarı hakkında proaktif olarak size bildirim göndermek veya başka bir işlem çağırmak için otomatik olarak bir ya da daha fazla eylem çalıştırabilir. Uyarı kuralları farklı türlerde farklı mantık bu analizi yapmak için kullanın.

![Log Analytics uyarıları](media/log-analytics-alerts/overview.png)

Beklenen bir gecikme süresi ile günlük veri alım olduğundan, veri ve aramak kullanılabilir olduğunda dizin oluşturma arasında mutlak süreye edilemeyebilir. Toplanan verileri neredeyse gerçek zamanlı kullanılabilirliğini uyarı kurallarını tanımlama dikkate alınması gereken.

Güvenilirlik uyarıları ve uyarı yanıtlama arasında bir denge yoktur. Yanlış uyarılar ve eksik uyarıları en aza indirmek için Uyarı parametreleri yapılandırmak seçebileceğiniz veya hızlı izlenmekte olan, ancak bazen yanlış veya eksik uyarılar oluşturur koşullarına yanıt vermek için Uyarı parametreleri seçebilirsiniz.

Uyarı kuralları tarafından aşağıdaki ayrıntıları tanımlanmıştır:

- **Günlük arama**. Uyarı kural her çalıştığında sorgusu gönderir. Bu sorgu tarafından döndürülen kayıtları, bir uyarı oluşturulup oluşturulmayacağını belirlemek için kullanılır.
- **Zaman penceresi**. Sorgu için zaman aralığını belirtir. Sorgu yalnızca bu geçerli zaman aralığı içinde oluşturulmuş olan kayıtları döndürür. Bu beş dakika ile 24 saat arasında herhangi bir değer olabilir. Aralığın alım makul gecikme uyum sağlayacak şekilde geniş olması gerekir. Zaman penceresi işleyebilen olmasını istediğiniz en uzun gecikme uzunluğu iki katı olması gerekir.<br> 30 dakikalık gecikme güvenilir olarak uyarıları isterseniz, örneğin, ardından aralık bir saat olması gerekir.

    Zaman aralığı kadar küçükse, karşılaşması iki Belirtiler vardır.

    - **Eksik uyarıları**. Alım gecikme bazen 60 dakika olmakla birlikte, çoğu zaman, beş dakikadır varsayalım. Zaman penceresi 30 dakikaya ayarlarsanız, gecikme süresini 60 dakika olduğunda uyarı sorgusu çalıştırıldığında veri arama için kullanılabilir olmadığından sonra bir uyarı isabetsiz okuma. 
   
        >[!NOTE]
        >Uyarı eksik neden Tanıla çalışılırken mümkün değildir. Örneğin, yukarıdaki durumda, veriler 60 dakika uyarı sorgu yürütüldükten sonra depoya yazılır. Bir uyarı kaçırılan ve sonraki gün sorgu doğru zaman aralığı içinde yürütülür sonraki gün fark, günlük arama ölçütlerini sonucu eşleşir. Uyarıyı tetiklemesi gereken olduğunu görünür. Uyarı sorgusu çalıştırıldığında verileri henüz kullanılabilir olmadığından aslında, uyarının başlatıldığı değil. 
        >
 
    - **Yanlış uyarılar**. Bazen uyarı sorguları, olay eksikliklerini belirlemek için tasarlanmıştır. Bir sanal makine için eksik sinyal arayarak çevrimdışı olduğunda bunun bir örneği algılıyor. Olarak yukarıdaki sinyal uyarı zaman penceresi içinde arama için kullanılabilir değilse, sonra bir uyarı sinyal verileri henüz aranabilir olmadığından oluşturulur ve bu nedenle yok. Bu aynı sonucu VM yasal çevrimdışı ve tarafından oluşturulan hiçbir sinyal veriler olarak olur. Sonraki gün içinde doğru zaman penceresi sorgu yürütülürken sinyal vardı ve uyarı başarısız olduğunu gösterir. Uyarı zaman penceresini çok küçük olarak ayarlanmadığından aslında, sinyal henüz Ara kullanılamıyordu.

- **Sıklık**.  Sorgu ne sıklıkta çalıştırılması gerektiğini ve uyarılar normal örneği için daha iyi yanıt vermesi için kullanılan belirtir. Değer en fazla beş dakika ile 24 saat arasında olabilir ve eşit veya uyarı zaman penceresi'den daha az olmalıdır.  Değeri zaman penceresi'den büyükse, eksik kayıtları riski oluşur.<br>Hedef ise, için güvenilir olarak 30 dakika kadar geciktirir ve normal gecikmesi 10 dakikadır, zaman penceresi bir saat olmalıdır ve sıklığı değeri 10 dakika olmalıdır. Bu uyarı verileri oluşturulduğunda, 10, 20 dakika arasında bir 10 dakika alım gecikme sahip verileri olan bir uyarı tetikler.<br>Zaman penceresi çok geniş olduğundan aynı veriler için birden çok uyarı oluşturmamak için uyarıları bastırma seçeneği en az sürece zaman penceresi için uyarıları gizlemek için kullanılabilir.
  
- **Eşik**. Günlük arama sonuçlarını, bir uyarının oluşturulması gerekip gerekmediğini belirlemek için değerlendirilir. Eşik uyarı kuralları farklı türleri için farklıdır.

Günlük analizi her uyarı kuralı iki türlerinden biridir. Bu türleri izleyen bölümlerde ayrıntılı olarak açıklanmıştır.

- **[Sonuç sayısı](#number-of-results-alert-rules)**. Günlük araması tarafından döndürülen sayı kayıtları belirli sayıda aştıklarında oluşturulan tek bir uyarı.
- **[Ölçüm ölçüm](#metric-measurement-alert-rules)**. Belirtilen eşiğini aşan değerlerle günlük arama sonuçlarında her nesne için oluşturulan bir uyarı.

Uyarı kuralı türleri arasındaki farkları aşağıdaki gibidir.

- **Sonuç sayısı** uyarı kuralı her zaman tek bir uyarı while Oluştur **ölçüm ölçüm** uyarı kuralı eşiği aşıyor her nesne için bir uyarı oluşturur.
- **Sonuç sayısı** uyarı kuralları tek bir kez eşiği aştığında bir uyarı oluşturmak. **Ölçüm ölçüm** uyarı kuralları, bir uyarı oluşturabilir, eşik aşıldığında, belirli bir süre boyunca belirli sayıda.

## <a name="number-of-results-alert-rules"></a>Sonuçları uyarı kuralları sayısı
**Sonuç sayısı** uyarı kuralları arama sorgusu tarafından döndürülen kayıt sayısını belirtilen eşiği aştığında tek bir uyarı oluştur.

### <a name="threshold"></a>Eşik
Eşiği bir **sonuç sayısı** değerinden büyük veya belirli bir değerden daha az uyarı kuralı. Günlük araması tarafından döndürülen kayıt sayısını bu ölçütlere uyan varsa bir uyarı oluşturulur.

### <a name="scenarios"></a>Senaryolar

#### <a name="events"></a>Olaylar
Bu tür bir uyarı kuralı olaylar Windows olay günlükleri, Syslog, gibi ile çalışmak için idealdir ve özel günlüğe kaydeder. Belirli hata olayı oluştururken ya da birden çok hata olayları belirli zaman penceresi içinde oluşturulan bir uyarı oluşturmak isteyebilirsiniz.

Tek bir olayda uyarmak için 0 ile sıklığı ve 5 dakika zaman penceresi'den büyük sonuç sayısını ayarlayın. Her 5 dakikada bir ve onay son kez sorgu çalıştırıldığında bu yana oluşturulan tek bir olay geçtiği, sorguyu çalıştırır. Bir uzun sıklığı toplanmakta olan olay ile oluşturulan uyarı arasındaki süre geciktirebilir.

Bazı uygulamalar, mutlaka bir uyarı oluşturmadan döndürmemelidir hatayla zaman oturum açabilir. Örneğin, uygulama hata olayı oluşturan işlemi yeniden deneyin ve bir sonraki sefer başarılı. Bu durumda, birden çok olay belirli zaman penceresi içinde oluşturulan sürece uyarı oluşturma istemeyebilirsiniz.

Bazı durumlarda, bir olay olmaması durumunda bir uyarı oluşturmak isteyebilirsiniz. Örneğin, bir işlem düzgün çalıştığını göstermek için normal olaylarla oturum açabilir. Belirli bir zaman penceresi içinde aşağıdaki olaylardan biri oturum değil, bir uyarı oluşturulmalıdır. Bu durumda, eşik ayarlamalısınız **değerinden 1**.

#### <a name="performance-alerts"></a>Performans uyarıları
[Performans verileri](log-analytics-data-sources-performance-counters.md) olaylarına benzer OMS depo kayıt olarak depolanır. Bir performans sayacı belirli bir eşiği aştığında uyarı istiyorsanız, bu eşiği sorguda bulunması gerekir.

Örneğin, işlemci çalıştığında uyarı istiyorsanız %90 üzerindeki aşağıdaki gibi bir sorgu ile eşiği için uyarı kuralı kullanacağınız **0'dan büyük**.

    Perf | where ObjectName=="Processor" and CounterName=="% Processor Time" and CounterValue>90

İşlemci üzerinde belirli bir zaman penceresi için % 90 ortalaması olduğunda uyarır istiyorsanız, bir sorgu kullanarak kullanırsınız `measure` uyarı kuralı için eşik ile aşağıdaki gibi komutunu **0'dan büyük**.

    Perf | where ObjectName=="Processor" and CounterName=="% Processor Time" | summarize avg(CounterValue) by Computer | where CounterValue>90



## <a name="metric-measurement-alert-rules"></a>Ölçüm ölçüm uyarı kuralları

>[!NOTE]
> Ölçüm ölçüm uyarı kuralları şu anda genel önizlemede.

**Ölçüm ölçüm** uyarı kuralları bir sorguda belirtilen eşiği aşarsa bir değerle her nesne için bir uyarı oluştur. Aşağıdaki ayrı farkları sahip oldukları **sonuç sayısı** uyarı kuralları.

#### <a name="log-search"></a>Günlük araması
Herhangi bir sorgu için kullanabilirsiniz, ancak bir **sonuç sayısı** uyarı kuralı, bir ölçüm ölçüm uyarı kuralı için sorgu belirli koşullar yoktur.  İçermesi gerekir bir `measure` Grup sonuçları belirli bir alan için komutu. Bu komut, aşağıdaki öğeleri eklemeniz gerekir.


- **Toplama işlevi**.  Gerçekleştirilen hesaplama ve büyük olasılıkla bir sayısal belirler toplanacak alan. Örneğin, **count()** sorguda, kayıt sayısını döndürür **avg(CounterValue)** aralığı içinde CounterValue alanının ortalamasını döndürür.
- **Alan grup**.  Bu alan her örneği için bir toplu değeri içeren bir kayıt oluşturulur ve her biri için bir uyarı oluşturulabilir. Örneğin, her bilgisayar için bir uyarı oluşturmak istiyorsanız, kullanacağınız **bilgisayar tarafından**.   
- **Aralığı**.  Birleşik verileri zaman aralığını tanımlar.  Örneğin, belirttiğiniz **5 dakika**, bir kayıt her örneği için uyarı belirtilen zaman penceresi üzerinden 5 dakikalık aralıklarla toplanan grup alanının oluşturulması.

#### <a name="threshold"></a>Eşik
Ölçüm ölçüm uyarı kuralları için eşik bir toplam değerini ve bir dizi tarafından tanımlanır. Herhangi bir veri noktasını günlük arama bu değeri aştığında bir ihlal dikkate almıştır. Dizi içinde sonuçlarındaki herhangi bir nesne için belirtilen değeri aşarsa bir uyarı bu nesne için oluşturulur.

#### <a name="example"></a>Örnek
Burada herhangi bir bilgisayar işlemci kullanımı % 90'ın üç kez üzerinde 30 dakika aşılırsa bir uyarı isteyen bir senaryo düşünün. Bir uyarı kuralı aşağıdaki ayrıntılarla oluşturursunuz.

**Sorgu:** türü Perf ObjectName = işlemci CounterName = "% işlemci zamanı" = | avg(CounterValue) 5 dakika bilgisayar aralığına göre ölçün<br>
**Zaman penceresi:** 30 dakika<br>
**Uyarı sıklığı:** 5 dakika<br>
**Toplam değer:** 90 büyüktür<br>
**Tetikleyici uyarı temel alarak:** toplam ihlal 5'ten büyük<br>

Sorgu her bilgisayar için bir ortalama değer 5 dakika aralıklarla oluşturursunuz. Bu sorguyu her 5 dakikada bir toplanan veriler için önceki 30 dakika boyunca çalışır. Örnek verileri varsayılan olarak, üç bilgisayarlar için aşağıda gösterilmiştir.

![Örnek sorgu sonuçları](media/log-analytics-alerts/metrics-measurement-sample-graph.png)

Bunlar % 90 eşiği 3 kez zaman penceresi ihlal beri bu örnekte, ayrı uyarılar srv02 ve srv03 oluşturulması. Varsa **tetikleyici uyarı temel alarak:** üzere değiştirilmiştir **art arda** ardışık 3 örnek eşiği ihlal bu yana bir uyarı yalnızca srv03 için oluşturulacak sonra.

## <a name="alert-records"></a>Uyarı kaydeder
Günlük analizi uyarı kuralları tarafından oluşturulan uyarı kayıtlarına sahip bir **türü** , **uyarı** ve **SourceSystem** , **OMS**. Aşağıdaki tabloda özellikleri sahiptirler.

| Özellik | Açıklama |
|:--- |:--- |
| Tür |*Uyarı* |
| SourceSystem |*OMS* |
| *Nesne*  | [Ölçüm ölçüm uyarıları](#metric-measurement-alert-rules) Grup alan için bir özelliğe sahiptir. Örneğin, bilgisayarda günlük arama grupları uyarı kaydıyla varsa bilgisayar ada sahip bir bilgisayar alan değeri olarak.
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
* İzlenecek yollar için tamamlamak [bir webook yapılandırma](log-analytics-alerts-webhooks.md) bir uyarı kuralı ile.
* Nasıl yazılacağını öğrenmek [Azure automation'daki runbook'lar](https://azure.microsoft.com/documentation/services/automation) uyarılar tarafından tanımlanan sorunları düzeltmek için.
