---
title: "Azure İzleyicisi'nde - günlük uyarıları uyarılar (Önizleme) | Microsoft Docs"
description: "Azure uyarıları (Önizleme) belirttiğiniz karmaşık bir sorgu koşullar karşılandığında tetikleyici e-postalar, bildirimler, Web siteleri URL'leri (Web kancaları) ya da Otomasyon çağırın."
author: msvijayn
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: vinagara
ms.openlocfilehash: 0cee8bf77e0facc12159b823152b8859ce5cedd8
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="log-alerts-in-azure-monitor---alerts-preview"></a>Azure İzleyicisi'nde - günlük uyarıları uyarılar (Önizleme)
Bu makalede Azure Uyarıları'ni (Önizleme) analiz sorguları işlerinde nasıl uyarı kurallarında ayrıntılarını sağlar ve günlük uyarı kuralları farklı türleri arasındaki farklar açıklanmaktadır. Ölçüm günlüklerini kullanarak uyarı ayrıntılarını başvurmak [yakın gerçek zamanlı ölçüm uyarıları](monitoring-near-real-time-metric-alerts.md)

Azure uyarıları (Önizleme), destekler sorgularından uyarılar şu anda oturum [Azure günlük analizi](../log-analytics/log-analytics-tutorial-viewdata.md) ve [Application Insights](../application-insights/app-insights-cloudservices.md#view-azure-diagnostic-events).

> [!WARNING]

> Şu anda Azure Uyarıları'ni (Önizleme) günlük uyarıda arası çalışma alanında ya da uygulama içi sorguları desteklemiyor.

Ayrıca, kullanıcıların sorgularını analiz platformu Azure tercih mükemmel ve ardından *uyarıları (Önizleme) kullanmak için sorgu kaydederek aldıktan*. İzlemeniz gereken adımlar:
- Application Insights için: gidilecek Analytics portalı, sorgu ve sonuçlarını doğrulayın. Benzersiz bir ad ile Kaydet *paylaşılan sorgular*.
- Günlük analizi için: gidilecek günlük arama, sorgu ve sonuçlarını doğrulayın. Ardından herhangi bir kategoriye benzersiz bir ad ile kaydedin.

Sonra ne zaman [uyarıları (Önizleme) bir günlük uyarı oluşturma](monitor-alerts-unified-usage.md), sinyal türü olarak listelenen kaydedilmiş sorgu gördüğünüz **günlük (kayıtlı sorgunun)**; aşağıdaki örnekte gösterilen şekilde: ![kaydedilmiş uyarıları içe sorgu](./media/monitor-alerts-unified/AlertsPreviewResourceSelectionLog-new.png)

> [!NOTE]
> Kullanarak **günlük (kayıtlı sorgunun)** uyarıları alma işleminde sonuçlanır. Bu nedenle sonra analizleri yapılan tüm değişiklikler kaydedilmiş uyarı kuralları ve tam tersini yansıtıcı olmaz.

## <a name="log-alert-rules"></a>Günlük uyarı kuralları

Azure uyarıları otomatik olarak düzenli aralıklarla günlük sorguları çalıştırmak için (Önizleme) tarafından uyarılar oluşturulur.  Günlük sorgunun sonuçlarını belirli ölçütlere uyan varsa bir uyarı kaydı oluşturulur. Kural sonra otomatik olarak proaktif olarak uyarı bildiren veya harici Web uygulaması kullanarak verileri gönderme gibi başka bir işlem çağırmak için bir veya daha fazla Eylemler çalıştırabilirsiniz [json tabanlı Web kancası](monitor-alerts-unified-log-webhook.md)kullanarak [Eylem grupları](monitoring-action-groups.md). Uyarı kuralları farklı türlerde farklı mantık bu analizi yapmak için kullanın.

Uyarı kuralları tarafından aşağıdaki ayrıntıları tanımlanmıştır:

- **Oturum sorgu**.  Uyarı kural her çalıştığında sorgusu gönderir.  Bu sorgu tarafından döndürülen kayıtları, bir uyarı oluşturulup oluşturulmayacağını belirlemek için kullanılır.
- **Zaman penceresi**.  Sorgu için zaman aralığını belirtir.  Sorgu yalnızca bu geçerli zaman aralığı içinde oluşturulmuş olan kayıtları döndürür.  Zaman penceresi, 5 dakika ile 1440 dakika veya 24 saat arasında herhangi bir değer olabilir. Örneğin, zaman penceresi 60 dakika olarak ayarlanmıştır ve sorgu 13: 15'te çalıştırırsanız, yalnızca saat 12: 15'e ve 13: 15'te arasında oluşturulan kayıtları döndürülür.
- **Sıklık**.  Sorgunun ne sıklıkta çalıştırılması gerektiğini belirtir. 5 dakika ile 24 saat arasında herhangi bir değer olabilir. Eşit veya bu zaman penceresi'den daha az olmalıdır.  Değeri zaman penceresi'den büyükse, eksik kayıtları riski oluşur.<br>Örneğin, 30 dakikalık bir zaman penceresi ve 60 dakika sıklığını göz önünde bulundurun.  Sorgu 1: 00'dan çalıştırırsanız, 12:30 ve 1:00 arasında kayıt döndürür.  Sorguyu çalıştırabilir sonraki 2:00 kayıtlar 1:30 ve 2:00 arasında ne zaman döndürecektir süresidir.  1:00-1:30 arasında oluşturulan kayıtları hiçbir zaman değerlendirilmesi.
- **Eşik**.  Günlük arama sonuçlarını, bir uyarının oluşturulması gerekip gerekmediğini belirlemek için değerlendirilir.  Eşik uyarı kuralları farklı türleri için farklıdır.

Günlük analizi her uyarı kuralı iki türlerinden biridir.  Bu türleri izleyen bölümlerde ayrıntılı olarak açıklanmıştır.

- **[Sonuç sayısı](#number-of-results-alert-rules)**. Günlük araması tarafından döndürülen sayı kayıtları belirli sayıda aştıklarında oluşturulan tek bir uyarı.
- **[Ölçüm ölçüm](#metric-measurement-alert-rules)**.  Belirtilen eşiğini aşan değerlerle günlük arama sonuçlarında her nesne için oluşturulan bir uyarı.

Uyarı kuralı türleri arasındaki farkları aşağıdaki gibidir.

- ** Uyarı kuralları sonuçları sayısını her zaman tek bir uyarı süre oluşturur **ölçüm ölçüm** uyarı kuralı eşiği aşıyor her nesne için bir uyarı oluşturur.
- **Sonuç sayısı** uyarı kuralları tek bir kez eşiği aştığında bir uyarı oluşturmak. **Ölçüm ölçüm** uyarı kuralları, bir uyarı oluşturabilir, eşik aşıldığında, belirli bir süre boyunca belirli sayıda.

## <a name="number-of-results-alert-rules"></a>Sonuçları uyarı kuralları sayısı
**Sonuç sayısı** uyarı kuralları arama sorgusu tarafından döndürülen kayıt sayısını belirtilen eşiği aştığında tek bir uyarı oluştur. Bu tür bir uyarı kuralı, Windows olay günlükleri, Syslog, WebApp yanıt ve özel günlükleri gibi olaylar ile çalışmak için idealdir.  Belirli hata olayı oluştururken ya da birden çok hata olayları belirli zaman penceresi içinde oluşturulan bir uyarı oluşturmak isteyebilirsiniz.

**Eşik**: eşiği bir ** sonuçları uyarı kuralları sayısı sıfırdan büyük veya belirli bir değerden daha az.  Günlük araması tarafından döndürülen kayıt sayısını bu ölçütlere uyan varsa bir uyarı oluşturulur.

Tek bir olayda uyarmak için 0'dan büyük sonuç sayısını ayarlayın ve son kez sorgu çalıştırıldığında bu yana oluşturulan tek bir olay geçtiği denetleyin. Bazı uygulamalar, mutlaka bir uyarı oluşturmadan döndürmemelidir hatayla zaman oturum açabilir.  Örneğin, uygulama hata olayı oluşturan işlemi yeniden deneyin ve bir sonraki sefer başarılı.  Bu durumda, birden çok olay belirli zaman penceresi içinde oluşturulan sürece uyarı oluşturma istemeyebilirsiniz.  

Bazı durumlarda, bir olay olmaması durumunda bir uyarı oluşturmak isteyebilirsiniz.  Örneğin, bir işlem düzgün çalıştığını göstermek için normal olaylarla oturum açabilir.  Belirli bir zaman penceresi içinde aşağıdaki olaylardan biri oturum değil, bir uyarı oluşturulmalıdır.  Bu durumda, eşik ayarlamalısınız **değerinden 1**.

### <a name="example"></a>Örnek
İstediğiniz zaman, web tabanlı uygulama kullanıcılarına kod 500 yanıt verir bilmek bir senaryo düşünün (yani) iç sunucu hatası. Bir uyarı kuralı aşağıdaki ayrıntılarla oluşturacak:  
**Sorgu:** istekleri | nerede resultCode "500" ==<br>
**Zaman penceresi:** 30 dakika<br>
**Uyarı sıklığı:** beş dakika<br>
**Eşik değeri:** 0'dan büyük<br>

Uyarı her 5 dakikada bir, sonuç kodu 500 olduğu kayıtlar için Ara verilerin - 30 dakika sonra sorguyu çalıştırabilir. Böyle bile bir kayıt bulunursa, uyarı başlatılır ve yapılandırılmış eylemi tetikler.

## <a name="metric-measurement-alert-rules"></a>Ölçüm ölçüm uyarı kuralları

**Ölçüm ölçüm** uyarı kuralları bir sorguda belirtilen eşiği aşarsa bir değerle her nesne için bir uyarı oluştur.  Aşağıdaki ayrı farkları sahip oldukları **sonuç sayısı** uyarı kuralları.

**Toplama işlevi**: belirler gerçekleştirilir hesaplama ve büyük olasılıkla bir sayısal toplama alanı.  Örneğin, **count()** sorguda, kayıt sayısını döndürür **avg(CounterValue)** aralığı içinde CounterValue alanının ortalamasını döndürür.

> [!NOTE]

> Sorgu toplama işlevinde adlı/adlı olmalıdır: AggregatedValue ve sayısal bir değer sağlayın. 


**Alan grup**: Bu alan her örneği için bir toplu değeri olan bir kayıt oluşturulur ve her biri için bir uyarı oluşturulabilir.  Örneğin, her bilgisayar için bir uyarı oluşturmak istiyorsanız, kullanacağınız **bilgisayar tarafından**   

> [!NOTE]

> Application Insights dayalı ölçüm ölçüm uyarı kuralları için verileri gruplandırmak için alan belirtebilirsiniz. Bunu yapmak için kullanın **üzerinde toplama** kural tanımı seçeneği.   

**Aralığı**: üzerinden verileri toplanır zaman aralığını tanımlar.  Örneğin, belirttiğiniz **beş dakika**, bir kayıt her örneği için uyarı belirtilen zaman penceresi üzerinden 5 dakikalık aralıklarla toplanan grup alanının oluşturulması.
> [!NOTE]
> Depo işlevi sorguda kullanılan gerekir. Bin() eşit olmayan zaman aralıklarında - neden olabileceğinden uyarı yerine bin_at işlevi birlikte çalışma zamanında uygun zaman sabit bir nokta sonuçlar sağlamak için kullanır.

**Eşik**: ölçüm ölçüm uyarı kuralları için eşik bir toplam değerini ve bir dizi tarafından tanımlanır.  Herhangi bir veri noktasını günlük arama bu değeri aştığında bir ihlal dikkate almıştır.  Dizi içinde sonuçlarındaki herhangi bir nesne için belirtilen değeri aşarsa bir uyarı bu nesne için oluşturulur.

#### <a name="example"></a>Örnek
Burada herhangi bir bilgisayar işlemci kullanımı % 90'ın üç kez üzerinde 30 dakika aşılırsa bir uyarı isteyen bir senaryo düşünün.  Bir uyarı kuralı aşağıdaki ayrıntılarla oluşturacak:  

**Sorgu:** Perf | burada ObjectName "İşlemci" ve CounterName == "% işlemci zamanı" == | AggregatedValue özetlemek bin (TimeGenerated, 5 m), bilgisayar tarafından avg(CounterValue) =<br>
**Zaman penceresi:** 30 dakika<br>
**Uyarı sıklığı:** beş dakika<br>
**Toplam değer:** 90'dan büyük<br>
**Tetikleyici uyarı temel alarak:** toplam ihlal 5'ten büyük<br>

Sorgu her bilgisayar için bir ortalama değer 5 dakikalık aralıklarla oluşturursunuz.  Bu sorguyu her 5 dakikada bir toplanan veriler için önceki 30 dakika boyunca çalışır.  Örnek verileri varsayılan olarak, üç bilgisayarlar için aşağıda gösterilmiştir.

![Örnek sorgu sonuçları](../log-analytics/media/log-analytics-alerts/metrics-measurement-sample-graph.png)

Bunlar % 90 eşiği 3 kez zaman penceresi ihlal beri bu örnekte, ayrı uyarılar srv02 ve srv03 oluşturulması.  Varsa **tetikleyici uyarı temel alarak:** üzere değiştirilmiştir **art arda** üç ardışık örnekler eşiği ihlal bu yana bir uyarı yalnızca srv03 için oluşturulacak sonra.


## <a name="next-steps"></a>Sonraki adımlar
* Anlamak [günlük uyarılar için Web kancası eylemleri](monitor-alerts-unified-log-webhook.md)
* [Azure Uyarıları'ni (Önizleme) göz atın](monitoring-overview-unified-alerts.md)
* Hakkında bilgi edinin [kullanarak Azure uyarıları (Önizleme)](monitor-alerts-unified-usage.md)
* Daha fazla bilgi edinmek [Application Insights](../application-insights/app-insights-analytics.md)
* Daha fazla bilgi edinmek [günlük analizi](../log-analytics/log-analytics-overview.md).    
