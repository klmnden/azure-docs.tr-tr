---
title: Azure İzleyicisi - uyarıları uyarılar oturum | Microsoft Docs
description: Azure uyarıları için belirttiğiniz analitik sorgu koşullar karşılandığında tetikleyici e-postalar, bildirimler, Web siteleri URL'leri (Web kancaları) ya da Otomasyon çağırın.
author: msvijayn
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: vinagara
ms.openlocfilehash: 3a4277d2106078136cee09dfe6aefc87a73c4e08
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="log-alerts-in-azure-monitor---alerts"></a>Azure İzleyicisi - uyarıları günlük uyarıları 
Bu makalede, günlük uyarı ayrıntılarını yeni içinde desteklenen uyarı türleri biri olan sağlanmaktadır [Azure uyarıları](monitoring-overview-unified-alerts.md) ve kullanıcıların Azure'nın analiz platformu uyarmak için temel olarak kullanmasına izin... Ölçüm günlüklerini kullanarak uyarı ayrıntılarını başvurmak [yakın gerçek zamanlı ölçüm uyarıları](monitoring-near-real-time-metric-alerts.md)


Günlük uyarı oluşur için oluşturulan günlük arama kurallarının [Azure günlük analizi](../log-analytics/log-analytics-tutorial-viewdata.md) veya [Application Insights](../application-insights/app-insights-cloudservices.md#view-azure-diagnostic-events)


## <a name="log-search-alert-rule---definition-and-types"></a>Günlük arama uyarı kuralı - tanımı ve türleri

Günlük arama kuralı uyarıları otomatik olarak düzenli aralıklarla belirtilen günlük sorguları çalıştırmak için Azure tarafından oluşturulur.  Günlük sorgunun sonuçlarını belirli ölçütlere uyan varsa bir uyarı kaydı oluşturulur. Kural sonra otomatik olarak kullanarak bir veya daha fazla eylemleri çalıştırabilirsiniz [Eylem grupları](monitoring-action-groups.md). 

Günlük arama kuralları tarafından aşağıdaki ayrıntıları tanımlanır:
- **Oturum sorgu**.  Uyarı kural her çalıştığında sorgusu gönderir.  Bu sorgu tarafından döndürülen kayıtları, bir uyarı oluşturulup oluşturulmayacağını belirlemek için kullanılır. *Azure Application Insights* sorgu de bulunabilir [uygulamalar arası çağrılar](https://dev.applicationinsights.io/ai/documentation/2-Using-the-API/CrossResourceQuery), kullanıcının dış uygulamalara erişim haklarına sahip sağlanan. 

    > [!IMPORTANT]
    > Suppport, [uygulama sorgu Application Insights için çapraz](https://dev.applicationinsights.io/ai/documentation/2-Using-the-API/CrossResourceQuery) Önizleme - işlevselliği ve kullanıcı deneyimi değiştirilebilir. Kullanımını [çalışma sorgu çapraz](https://dev.loganalytics.io/oms/documentation/3-Using-the-API/CrossResourceQuery) ve [arası kaynak sorgu günlük analizi için](../log-analytics/log-analytics-cross-workspace-search.md) şu anda **desteklenmiyor** Azure uyarılar.

- **Zaman dilimi**.  Sorgu için zaman aralığını belirtir. Sorgu yalnızca bu geçerli zaman aralığı içinde oluşturulmuş olan kayıtları döndürür. Süre günlük sorgu kötüye önlemek alınan veri kısıtlar ve herhangi bir zaman komutu bozar (önce ister) günlük sorguda kullanılan. <br>*Örneğin, süre 60 dakika olarak ayarlanmıştır ve sorgu 13: 15'te çalıştırırsanız, yalnızca saat 12: 15'e ve 13: 15'te arasında oluşturulan kayıtları döndürülür günlük sorgusu yürütülemedi. Günlük sorgu komutu gibi önce zaman kullanıyorsa şimdi (7d), günlük sorgu çalıştırılması yalnızca saat 12: 15'e ve 1:15 PM - arasında veri için veri için yalnızca son 60 dakika olarak varsa. Verilerin günlük sorgu belirtildiği gibi yedi gün için değil.*
- **Sıklık**.  Sorgunun ne sıklıkta çalıştırılması gerektiğini belirtir. 5 dakika ile 24 saat arasında herhangi bir değer olabilir. Eşit veya saati süresinden daha az olmalıdır.  Değer süre büyükse, eksik kayıtları riski oluşur.<br>*Örneğin, 30 dakikalık bir zaman dilimi ve 60 dakika sıklığını göz önünde bulundurun.  Sorgu 1: 00'dan çalıştırırsanız, 12:30 ve 1:00 arasında kayıt döndürür.  Sorguyu çalıştırabilir sonraki 2:00 kayıtlar 1:30 ve 2:00 arasında ne zaman döndürecektir süresidir.  1:00-1:30 arasında oluşturulan kayıtları hiçbir zaman değerlendirilmesi.*
- **Eşik**.  Günlük arama sonuçlarını, bir uyarının oluşturulması gerekip gerekmediğini belirlemek için değerlendirilir.  Eşik günlük arama uyarı kuralları farklı türleri için farklıdır.

Günlük arama kuralları olması için [Azure günlük analizi](../log-analytics/log-analytics-tutorial-viewdata.md) veya [Application Insights](../application-insights/app-insights-cloudservices.md#view-azure-diagnostic-events), iki türde olabilir. Bu türleri izleyen bölümlerde ayrıntılı olarak açıklanmıştır.

- **[Sonuç sayısı](#number-of-results-alert-rules)**. Günlük araması tarafından döndürülen sayı kayıtları belirli sayıda aştıklarında oluşturulan tek bir uyarı.
- **[Ölçüm ölçüm](#metric-measurement-alert-rules)**.  Belirtilen eşiğini aşan değerlerle günlük arama sonuçlarında her nesne için oluşturulan bir uyarı.

Uyarı kuralı türleri arasındaki farkları aşağıdaki gibidir.

- *Sonuç sayısı* uyarı kuralları her zaman tek bir uyarı, süre oluşturur *ölçüm ölçüm* uyarı kuralı eşiği aşıyor her nesne için bir uyarı oluşturur.
- *Sonuç sayısı* uyarı kuralları tek bir kez eşiği aştığında bir uyarı oluşturmak. *Ölçüm ölçüm* uyarı kuralları, bir uyarı oluşturabilir, eşik aşıldığında, belirli bir süre boyunca belirli sayıda.

### <a name="number-of-results-alert-rules"></a>Sonuçları uyarı kuralları sayısı
**Sonuç sayısı** uyarı kuralları arama sorgusu tarafından döndürülen kayıt sayısını belirtilen eşiği aştığında tek bir uyarı oluştur. Bu tür bir uyarı kuralı, Windows olay günlükleri, Syslog, WebApp yanıt ve özel günlükleri gibi olaylar ile çalışmak için idealdir.  Belirli hata olayı oluştururken ya da belirli bir süre içinde birden çok hata olayları oluşturduğunuzda bir uyarı oluşturmak isteyebilirsiniz.

**Eşik**: sonuçları uyarı kuralları sayısı için eşik değerinden büyük veya belirli bir değerden daha az.  Günlük araması tarafından döndürülen kayıt sayısını bu ölçütlere uyan varsa bir uyarı oluşturulur.

Tek bir olayda uyarmak için 0'dan büyük sonuç sayısını ayarlayın ve son kez sorgu çalıştırıldığında bu yana oluşturulan tek bir olay geçtiği denetleyin. Bazı uygulamalar, mutlaka bir uyarı oluşturmadan döndürmemelidir hatayla zaman oturum açabilir.  Örneğin, uygulama hata olayı oluşturan işlemi yeniden deneyin ve bir sonraki sefer başarılı.  Bu durumda, belirli bir süre içinde birden çok olayı oluşturulur sürece uyarı oluşturma istemeyebilirsiniz.  

Bazı durumlarda, bir olay olmaması durumunda bir uyarı oluşturmak isteyebilirsiniz.  Örneğin, bir işlem düzgün çalıştığını göstermek için normal olaylarla oturum açabilir.  Aşağıdaki olaylardan biri belirli bir süre içinde oturum değil, bir uyarı oluşturulmalıdır.  Bu durumda, eşik ayarlamalısınız **değerinden 1**.

#### <a name="example"></a>Örnek
İstediğiniz zaman, web tabanlı uygulama kullanıcılarına kod 500 yanıt verir bilmek bir senaryo düşünün (yani) iç sunucu hatası. Bir uyarı kuralı aşağıdaki ayrıntılarla oluşturacak:  
- **Sorgu:** istekleri | nerede resultCode "500" ==<br>
- **Süre:** 30 dakika<br>
- **Uyarı sıklığı:** beş dakika<br>
- **Eşik değeri:** 0'dan büyük<br>

Uyarı her 5 dakikada bir, sonuç kodu 500 olduğu kayıtlar için Ara verilerin - 30 dakika sonra sorguyu çalıştırabilir. Böyle bile bir kayıt bulunursa, uyarı başlatılır ve yapılandırılmış eylemi tetikler.

### <a name="metric-measurement-alert-rules"></a>Ölçüm ölçüm uyarı kuralları

- **Ölçüm ölçüm** uyarı kuralları bir sorguda belirtilen eşiği aşarsa bir değerle her nesne için bir uyarı oluştur.  Aşağıdaki ayrı farkları sahip oldukları **sonuç sayısı** uyarı kuralları.
- **Toplama işlevi**: belirler gerçekleştirilir hesaplama ve büyük olasılıkla bir sayısal toplama alanı.  Örneğin, **count()** sorguda, kayıt sayısını döndürür **avg(CounterValue)** aralığı içinde CounterValue alanının ortalamasını döndürür. Sorgu toplama işlevinde adlı/adlı olmalıdır: AggregatedValue ve sayısal bir değer sağlayın. 
- **Alan grup**: Bu alan her örneği için bir toplu değeri olan bir kayıt oluşturulur ve her biri için bir uyarı oluşturulabilir.  Örneğin, her bilgisayar için bir uyarı oluşturmak istiyorsanız, kullanacağınız **bilgisayar tarafından** 

    > [!NOTE]
    > Application Insights dayalı ölçüm ölçüm uyarı kuralları için verileri gruplandırmak için alan belirtebilirsiniz. Bunu yapmak için kullanın **üzerinde toplama** kural tanımı seçeneği.   
    
- **Aralığı**: üzerinden verileri toplanır zaman aralığını tanımlar.  Örneğin, belirttiğiniz **beş dakika**, bir kayıt her örneği için uyarı belirtilen süre içinde 5 dakikalık aralıklarla toplanan grup alanının oluşturulması.

    > [!NOTE]
    > Depo işlevi sorgu aralığı belirtmek için kullanılması gerekir. Bin() eşit olmayan zaman aralıklarında - neden olabileceğinden uyarı otomatik olarak depo komutu çalışma zamanında uygun zaman bin_at komutuyla için sabit bir nokta sonuçlar sağlamak için dönüştürür
    
- **Eşik**: ölçüm ölçüm uyarı kuralları için eşik bir toplam değerini ve bir dizi tarafından tanımlanır.  Herhangi bir veri noktasını günlük arama bu değeri aştığında bir ihlal dikkate almıştır.  Dizi içinde sonuçlarındaki herhangi bir nesne için belirtilen değeri aşarsa bir uyarı bu nesne için oluşturulur.

#### <a name="example"></a>Örnek
Burada herhangi bir bilgisayar işlemci kullanımı % 90'ın üç kez üzerinde 30 dakika aşılırsa bir uyarı isteyen bir senaryo düşünün.  Bir uyarı kuralı aşağıdaki ayrıntılarla oluşturacak:  

- **Sorgu:** Perf | burada ObjectName "İşlemci" ve CounterName == "% işlemci zamanı" == | AggregatedValue özetlemek bin (TimeGenerated, 5 m), bilgisayar tarafından avg(CounterValue) =<br>
- **Süre:** 30 dakika<br>
- **Uyarı sıklığı:** beş dakika<br>
- **Toplam değer:** 90'dan büyük<br>
- **Tetikleyici uyarı temel alarak:** toplam ihlal 5'ten büyük<br>

Sorgu her bilgisayar için bir ortalama değer 5 dakikalık aralıklarla oluşturursunuz.  Bu sorguyu her 5 dakikada bir toplanan veriler için önceki 30 dakika boyunca çalışır.  Örnek verileri varsayılan olarak, üç bilgisayarlar için aşağıda gösterilmiştir.

![Örnek sorgu sonuçları](./media/monitor-alerts-unified/metrics-measurement-sample-graph.png)

Bunlar % 90 eşik süre boyunca 3 kez ihlal beri bu örnekte, ayrı uyarılar srv02 ve srv03 oluşturulması.  Varsa **tetikleyici uyarı temel alarak:** üzere değiştirilmiştir **art arda** üç ardışık örnekler eşiği ihlal bu yana bir uyarı yalnızca srv03 için oluşturulacak sonra.


## <a name="log-search-alert-rule---creation-and-modification"></a>Günlük arama uyarı kuralı - oluşturma ve değiştirme

Günlük uyarı yanı sıra kendi consisting günlük arama uyarı kuralı görüntülenebilir, oluşturulan veya gelen değiştiren:
- Azure portalına
- REST API (PowerShell dahil)
- Azure Resource Manager şablonları

### <a name="azure-portal"></a>Azure portalına
Giriş itibaren [yeni Azure uyarılar](monitoring-overview-unified-alerts.md), artık kullanıcıları tek bir konum ve benzer adımları tüm Azure portalında uyarı türlerini yönetebilirsiniz. Daha fazla bilgi edinmek [yeni Azure uyarıları kullanarak](monitor-alerts-unified-usage.md).

Ayrıca, kullanıcıların sorgularını analiz platformu Azure tercih mükemmel ve ardından *Uyarıları kullanmak için sorgu kaydederek aldıktan*. İzlemeniz gereken adımlar:
- *Application Insights için*: gidilecek Analytics portalı, sorgu ve sonuçlarını doğrulayın. Benzersiz bir ad ile Kaydet *paylaşılan sorgular*.
- *Günlük analizi için*: gidilecek günlük arama, sorgu ve sonuçlarını doğrulayın. Ardından herhangi bir kategoriye benzersiz bir ad ile kaydedin.

Sonra ne zaman [günlük Uyarı Uyarıları oluşturma ](monitor-alerts-unified-usage.md), sinyal türü olarak listelenen kaydedilmiş sorgu bkz **günlük (kayıtlı sorgunun)**; aşağıdaki örnekte gösterilen şekilde: ![kaydedilmiş uyarıları içe sorgu](./media/monitor-alerts-unified/AlertsPreviewResourceSelectionLog-new.png)

> [!NOTE]
> Kullanarak **günlük (kayıtlı sorgunun)** uyarıları alma işleminde sonuçlanır. Bu nedenle sonra analizleri yapılan herhangi bir değişiklik günlüğü arama uyarı kuralları ve tam tersini yansıtıcı olmaz.

### <a name="rest-apis"></a>REST API'leri
Uyarıları RESTful olduğunuz ve Azure Resource Manager REST API'si erişilebilir günlük için API sağlanan. Bu nedenle de diğer seçenekler API'leri yararlanmak için PowerShell erişilebilir.

REST API, lütfen kullanma örnekleri yanı sıra ayrıntıları başvurmak için:
- [Analytics uyarı REST API oturum](../log-analytics/log-analytics-api-alerts.md) - oluşturma ve Azure günlük analizi için günlük arama uyarı kurallarını yönet
- [Azure İzleyici zamanlanmış sorgu kuralları REST API](https://docs.microsoft.com/en-us/rest/api/monitorr/scheduledqueryrules/) - oluşturma ve Azure Application Insights için günlük arama uyarı kurallarını yönet

### <a name="azure-resource-manager-template"></a>Azure Resource Manager Şablonu
Kullanıcılar tarafından sağlanan esneklik de kullanabilir [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) oluşturmak ve oluşturmak veya günlük uyarıları güncelleştirmek için kaynaklar - güncelleştirmek için.

Resource Manager şablonları, lütfen kullanma örnekleri yanı sıra ayrıntıları başvurmak için:
- [Arama ve uyarılar yönetim kaydedilmiş](monitor-alerts-unified-log-template.md#managing-log-alert-on-log-analytics) Azure günlük analizi göre günlük uyarılar için
- [Sorgu kuralı zamanlanmış](monitor-alerts-unified-log-template.md#managing-log-alert-on-application-insights) Azure Application Insights üzerinde temel günlük uyarılar için
 

## <a name="next-steps"></a>Sonraki adımlar
* Anlamak [uyarıları Azure'da oturum](monitor-alerts-unified-log-webhook.md).
* Yeni hakkında bilgi edinin [Azure uyarıları](monitoring-overview-unified-alerts.md).
* Daha fazla bilgi edinmek [Application Insights](../application-insights/app-insights-analytics.md).
* Daha fazla bilgi edinmek [günlük analizi](../log-analytics/log-analytics-overview.md).    
