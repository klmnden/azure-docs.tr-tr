---
title: Azure İzleyici'de günlük uyarıları
description: Azure Uyarıları'için belirttiğiniz analitik sorgu koşullar karşılandığında tetikleyici e-posta, bildirimleri, Web siteleri URL'leri (Web kancaları) ya da Otomasyon çağırın.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: fd278ad6865c871ed0a5ed9272c9fadfca0f38db
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39440438"
---
# <a name="log-alerts-in-azure-monitor---alerts"></a>Azure İzleyici - uyarılar günlük uyarıları 
Bu makalede, günlük uyarı ayrıntıları, bir yeni içinde desteklenen uyarı türleri sağlanır [Azure uyarıları](monitoring-overview-unified-alerts.md) ve kullanıcıların uyarmak için temel olarak Azure'nın analiz platformu kullanmasına izin verir.


Günlük araması kuralları için oluşturulan günlük uyarı oluşur [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) veya [Application Insights](../application-insights/app-insights-cloudservices.md#view-azure-diagnostic-events). Günlük uyarıları kullanılabilir fiyatlandırma ayrıntıları [Azure İzleyici fiyatlandırma](https://azure.microsoft.com/en-us/pricing/details/monitor/) sayfası. Azure faturaları günlük uyarıları tür olarak temsil edilir `microsoft.insights/scheduledqueryrules` ile:
- Kaynak grubu ve uyarı özellikleri ile birlikte tam uyarı adı ile gösterilen Application ınsights günlük uyarıları
- Günlük uyarıları Log Analytics ile uyarı adı gösterilen üzerinde `<WorkspaceName>|<savedSearchId>|<scheduleId>|<ActionId>` yanı sıra kaynak grubu ve uyarı özellikleri

    > [!NOTE]
    > Tüm kayıtlı aramalar, çizelgeler ve günlük analizi API'si ile oluşturulan eylem adını, küçük olmalıdır. Geçersiz gibi karakterler varsa `<, >, %, &, \, ?, /` olan kullanılan - bunlar ile değiştirilecek `_` , faturada.

## <a name="log-search-alert-rule---definition-and-types"></a>Günlük araması uyarı kuralı - tanım ve türleri

Belirtilen günlük sorgularını düzenli aralıklarla otomatik olarak çalıştırmak için, Azure Uyarıları tarafından günlük arama kuralları oluşturulur.  Günlük sorgusunun sonuçları belirli ölçütlerle eşleşiyorsa bir uyarı kaydı oluşturulur. Ardından kural, [Eylem Grupları](monitoring-action-groups.md)'nı kullanıp bir veya birden çok eylemi otomatik olarak çalıştırabilir. 

Günlük arama kuralları aşağıdaki ayrıntıları tarafından tanımlanır:
- **Oturum sorgu**.  Uyarı kural her çalıştığında sorgu tetikler.  Bu sorgu tarafından döndürülen kayıtları, bir uyarı oluşturulup oluşturulmayacağını belirlemek için kullanılır. *Azure Application Insights* sorgu de bulunabilir [çapraz uygulama çağrıları](https://dev.applicationinsights.io/ai/documentation/2-Using-the-API/CrossResourceQuery), sağlanan kullanıcının dış uygulama için erişim haklarına sahip. 

    > [!IMPORTANT]
    > Destek, [uygulama sorgu Application Insights için platformlar arası](https://dev.applicationinsights.io/ai/documentation/2-Using-the-API/CrossResourceQuery) önizlemede - işlevselliği sınırlı 2 veya daha fazla uygulamaları ile kullanma ve kullanıcı deneyimi değiştirilebilir ise. Kullanımını [çalışma sorgu çapraz](https://dev.loganalytics.io/oms/documentation/3-Using-the-API/CrossResourceQuery) ve [Log Analytics için kaynaklar arası sorgu](../log-analytics/log-analytics-cross-workspace-search.md) şu anda **desteklenmiyor** Azure Uyarıları'nda.

- **Dönem**.  Sorgu için zaman aralığını belirtir. Sorgu yalnızca bu geçerli zaman aralığı içinde oluşturulmuş olan kayıtları döndürür. Süre kötüye kullanımı önlemek günlük sorgusu için alınan verileri sınırlar ve hiçbir zaman komut bozar (önce ister) günlük sorguda kullanılan. <br>*Örneğin, zaman aralığı 60 dakika olarak ayarlanmıştır ve sorguyu, 13: 15'te çalıştırmak, 12:15 PM arasında 13: 15'te oluşturulan kayıtları döndürülür günlük sorgusu yürütülemedi. Günlük sorgusu komutu gibi önce zaman kullanıyorsa, şimdi (7 g), günlük sorgusu çalıştırılması yalnızca 1:15 PM - 12:15 PM arasında verileri için veri yalnızca son 60 dakika için varmış gibi. Yedi günün verilerini günlük sorgusu belirtildiği için değil.*
- **Sıklık**.  Sorgunun ne sıklıkta çalıştırılması gerektiğini belirtir. Herhangi bir değer 5 dakika ile 24 saat arasında olabilir. Eşittir veya süre değerinden küçük olmalıdır.  Değer süre büyük ise, eksik kayıtları riski oluşur.<br>*Örneğin, 30 dakikalık bir süre ve 60 dakikalık bir sıklık göz önünde bulundurun.  Sorgu 1: 00'da çalışıyorsa, 12:30 ve 1:00 arasında kayıtları döndürür.  Sorgu çalıştıracağınız sonraki 2:00 kayıtlar 1:30 ve 2:00 arasında zaman döndürecekti zamandır.  1:30 ile 1:00 arasında oluşturulan kayıtları hiç hesaplanmadı.*
- **Eşik**.  Günlük aramasının sonuçları, bir uyarının oluşturulması gerekip gerekmediğini belirlemek için değerlendirilir.  Farklı türde günlük araması uyarı kuralları için eşik farklıdır.

Günlük arama kuralları olması için [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) veya [Application Insights](../application-insights/app-insights-cloudservices.md#view-azure-diagnostic-events), iki türde olabilir. Bu türlerinin her birini ayrıntılı olarak bölümlerde açıklanmıştır.

- **[Sonuç sayısı](#number-of-results-alert-rules)**. Günlük araması tarafından döndürülen kayıt sayısını belirtilen bir sayıyı aştığında oluşturulan tek bir uyarı.
- **[Ölçüm ölçüsü](#metric-measurement-alert-rules)**.  Uyarı belirtilen Eşiği aşan değerler ile günlük arama sonuçlarındaki her nesne için oluşturuldu.

Uyarı kuralı türleri arasındaki farklar aşağıdaki gibidir.

- *Sonuç sayısı* uyarı kuralları her zaman tek bir uyarı, süre oluşturur *ölçüm ölçüsü* uyarı kuralı eşiğini aştığında her nesne için bir uyarı oluşturur.
- *Sonuç sayısı* tek bir kez eşiği aştığında bir uyarı uyarı kuralları oluşturma. *Ölçüm ölçüsü* uyarı kuralları, bir uyarı oluşturabilir, eşiği aşıldığında belirli bir süre boyunca belirli bir sayı.

### <a name="number-of-results-alert-rules"></a>Sonuçları uyarı kuralları sayısı
**Sonuç sayısı** arama sorgu tarafından döndürülen kayıt sayısını belirtilen eşiği aştığında uyarı kuralları tek bir uyarı oluşturun. Bu uyarı kuralının türünü, Windows olay günlükleri, Syslog, WebApp yanıt ve özel günlükleri gibi olayları ile çalışmak için idealdir.  Belirli hata olayı oluştururken ya da belirli bir süre içinde birden çok hata olayı oluşturulduğunda bir uyarı oluşturmak isteyebilirsiniz.

**Eşik**: sonuçları uyarı kuralları sayısı için eşik değerinden büyük veya belirli bir değerden daha az.  Ardından günlük arama tarafından döndürülen kayıt sayısını bu ölçütlerle eşleşiyorsa bir uyarı oluşturulur.

Tek bir olay üzerine uyarmak için sonuç sayısı 0'dan daha büyük bir değere ayarlayın ve sorgu çalıştırıldığı son daraltılmasından oluşturulan tek bir olay oluşumunu denetleyin. Bazı uygulamalar, mutlaka bir uyarı Oluştur olmamalıdır zaman hatayla oturum açabilir.  Örneğin, uygulama hata olayı oluşturan işlemi yeniden deneyin ve ardından sonraki başarılı.  Bu durumda, belirli bir süre içinde birden çok olayı oluşturulduğu sürece uyarı oluşturma istemeyebilirsiniz.  

Bazı durumlarda, bir olayın olmaması durumunda bir uyarı oluşturmak isteyebilirsiniz.  Örneğin, bir işlemin düzgün çalıştığını göstermek için normal olaylarla oturum açabilir.  Bu olaylardan biri belirli bir süre içinde oturumu değil, bir uyarı oluşturulmalıdır.  Bu durumda, eşiği ayarlamalısınız **1'den az**.

#### <a name="example"></a>Örnek
İstediğiniz zaman web tabanlı uygulama kullanıcılarına kod 500 yanıt verir bilmek bir senaryo düşünün (yani) iç sunucu hatası. Bir uyarı kuralı aşağıdaki ayrıntılarla oluşturacak:  
- **Sorgu:** istekleri | burada resultCode "500" ==<br>
- **Zaman aralığı:** 30 dakika<br>
- **Uyarı sıklığı:** beş dakika<br>
- **Eşik değeri:** 0'dan büyük<br>

Uyarı her 5 dakikada bir, sonuç kodu 500 olduğu kayıtlar için aramak için verilerin - 30 dakika sonra sorguyu çalıştırabilir. Böyle bir kayıt bulursa uyarı tetikler ve yapılandırılan eylem tetikler.

### <a name="metric-measurement-alert-rules"></a>Ölçüm ölçüsü uyarı kuralları

- **Ölçüm ölçüsü** uyarı kuralları belirtilen eşiği aşarsa bir değere sahip sorgudaki her nesne için bir uyarı oluştur.  Aşağıdaki farklı fark sahip oldukları **sonuç sayısı** uyarı kuralları.
- **Toplama işlevi**: gerçekleştirilen hesaplama ve büyük olasılıkla bir sayısal belirler toplanacak alan.  Örneğin, **Count() işlevi** sorguda kayıt sayısını döndürür **avg(CounterValue)** aralığında Ort alanın ortalamasını döndürür. Sorgu toplama işlevinde adlı/adlı olmalıdır: AggregatedValue ve sayısal bir değer sağlayın. 
- **Alan grubu**: Bu alan her bir örneği için bir toplu değer içeren bir kayıt oluşturulur ve her biri için bir uyarı oluşturulabilir.  Örneğin, her bilgisayar için bir uyarı oluşturmak istediyseniz, kullanacağınız **bilgisayar tarafından** 

    > [!NOTE]
    > Application Insights temel ölçüm ölçüsü uyarı kuralları için verileri gruplandırmak için alanı belirtebilirsiniz. Bunu yapmak için **bulunan** kural tanımı seçeneği.   
    
- **Aralığı**: üzerinde verileri toplanır zaman aralığını tanımlar.  Örneğin, belirttiğiniz **beş dakika**, bir kayıt için uyarı belirtilen süre boyunca 5 dakikalık aralıklarla toplanan grup alanının her örneği için oluşturulması.

    > [!NOTE]
    > Depo işlev sorgusunda aralığını belirtmek için kullanılmalıdır. Eşit zaman aralıkları - bin() oluşturacağından uyarı otomatik olarak depo komut bin_at komutuyla uygun zamanda çalışma zamanı, sabit bir nokta sonuçlar sağlamak için dönüştürülecektir
    
- **Eşik**: ölçüm ölçüsü uyarı kuralları için eşik ihlallerinin sayısı ve toplam değer ile tanımlanır.  Günlük araması'nda herhangi bir veri noktasının bu değeri aşarsa, bir ihlal dikkate almıştır.  Ardından ihlalleriyle sonuçları herhangi bir nesne sayısı belirtilen değeri aşarsa, bu nesne için bir uyarı oluşturulur.

#### <a name="example"></a>Örnek
Burada herhangi bir bilgisayarda, % 90'ın işlemci kullanımı üç kez tekrar 30 dakika aşılırsa bir uyarı istiyordu bir senaryo düşünün.  Bir uyarı kuralı aşağıdaki ayrıntılarla oluşturacak:  

- **Sorgu:** Perf | burada ObjectName "İşlemci" ve CounterName == "% işlemci zamanı" == | Summarize aggregatedvalue = avg(CounterValue) bin (TimeGenerated, 5 milyon), bilgisayar tarafından<br>
- **Zaman aralığı:** 30 dakika<br>
- **Uyarı sıklığı:** beş dakika<br>
- **Toplam değer:** 90'dan büyük<br>
- **Bağlı olarak uyarıyı Tetikle:** toplam ihlal sayısı 2'den büyük<br>

Sorguyu 5 dakikalık aralıklarla her bilgisayar için ortalama bir değer oluşturur.  Bu sorgu, önceki 30 dakika boyunca 5 dakikada bir toplanan veriler için çalıştırılmaz.  Örnek verileri varsayılan olarak, üç bilgisayar için aşağıda gösterilmiştir.

![Örnek sorgu sonuçları](./media/monitor-alerts-unified/metrics-measurement-sample-graph.png)

Bunlar zaman diliminde 3 kez % 90 eşiğini ihlal olduğundan bu örnekte, ayrı uyarılar srv02 srv03 için oluşturulmuş olması.  Varsa **bağlı olarak uyarıyı Tetikle:** üzere değiştirilmiştir **art arda** sonra üç ardışık örnekler eşiği ihlal olduğundan bir uyarı yalnızca srv03 için oluşturulacak.


## <a name="log-search-alert-rule---creation-and-modification"></a>Günlük araması uyarı kuralı - oluşturma ve değiştirme

Günlüğü uyarısı ve bunun yanı sıra kendi consisting günlük araması uyarı kuralı görüntülenebilir, oluşturulan veya gelen değiştiren:
- Azure portal
- REST API (PowerShell dahil)
- Azure Resource Manager şablonları

### <a name="azure-portal"></a>Azure portal
Sunulmasından beri [yeni Azure uyarıları](monitoring-overview-unified-alerts.md)artık kullanıcılar tek bir konumdan ile kullanım için benzer adımları her tür uyarılar Azure portalında yönetebilir. Daha fazla bilgi edinin [yeni Azure Uyarıları'nı kullanarak](monitor-alerts-unified-usage.md).

Kullanıcılar ayrıca, azure'da tercih ettiğiniz bir analiz platformuyla sorgularında mükemmel ve ardından *kullanın Uyarılardaki için sorgu kaydederek aktarın*. İzlemeniz gereken adımlar:
- *Application ınsights*: Go için Analytics portalı, sorgu ve sonuçları doğrulayın. Benzersiz bir ad ile kaydedin *paylaşılan sorgular*.
- *Log Analytics için*: Go için günlük araması'nı tıklatın, sorgu ve sonuçları doğrulayın. Ardından, herhangi bir kategori benzersiz bir adla kaydedin.

Sonra ne zaman [uyarıları günlük uyarısı oluşturma ](monitor-alerts-unified-usage.md), sinyal türü olarak listelenen kaydedilmiş sorgu gördüğünüz **günlük (kayıtlı sorgu)**; aşağıdaki örnekte gösterildiği: ![uyarıları içe sorgu kaydedildi](./media/monitor-alerts-unified/AlertsPreviewResourceSelectionLog-new.png)

> [!NOTE]
> Kullanarak **günlük (kayıtlı sorgu)** uyarılarını alma işleminde sonuçlanır. Bu nedenle Analytics'te sonra yapılan değişiklikler günlük araması uyarı kuralları ve tersi yansıtıcı olmayacaktır.

### <a name="rest-apis"></a>REST API'leri
Günlük uyarıları RESTful olan ve Azure Resource Manager REST API aracılığıyla erişilebilen olması için API'ler sağlanmıştır. Bu nedenle de diğer seçenekler API'lerden yararlanarak PowerShell erişilebilir.

Lütfen REST API'sini kullanarak örnekler yanı sıra ayrıntılar başvurmak için:
- [Analytics uyarı REST API oturum](../log-analytics/log-analytics-api-alerts.md) - oluşturma ve Azure Log Analytics için günlük araması uyarı kuralları yönetme
- [Azure İzleyici zamanlanmış sorgu kuralları REST API'si](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) - oluşturma ve günlük araması uyarı kuralları için Azure Application ınsights'ı yönetme

### <a name="azure-resource-manager-template"></a>Azure Resource Manager Şablonu
Kullanıcı tarafından sağlanan esneklik de kullanabilir [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) oluşturma veya güncelleştirme günlük uyarıları için kaynaklar - oluşturmak için.

Resource Manager şablonları, lütfen kullanma örnekleri yanı sıra ayrıntılar başvurmak için:
- [Kaydedilen arama ve uyarılar Yönetim](monitor-alerts-unified-log-template.md#managing-log-alert-on-log-analytics) Azure Log Analytics temelinde göre günlük uyarıları için
- [Zamanlanmış sorgu kuralını](monitor-alerts-unified-log-template.md#managing-log-alert-on-application-insights) Azure Application Insights tabanlı günlük uyarıları için
 

## <a name="next-steps"></a>Sonraki adımlar
* Anlamak [günlük uyarıları azure'da Web kancalarıyla](monitor-alerts-unified-log-webhook.md).
* Yeni hakkında bilgi edinin [Azure uyarıları](monitoring-overview-unified-alerts.md).
* Daha fazla bilgi edinin [Application Insights](../application-insights/app-insights-analytics.md).
* Daha fazla bilgi edinin [Log Analytics](../log-analytics/log-analytics-overview.md).    
