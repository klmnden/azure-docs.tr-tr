---
title: Azure İzleyici'de günlük uyarıları
description: Azure Uyarıları'için belirttiğiniz analitik sorgu koşullar karşılandığında tetikleyici e-posta, bildirimleri, Web siteleri URL'leri (Web kancaları) ya da Otomasyon çağırın.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: b257226a8bee0258a6bcc57715f959454577524c
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52962394"
---
# <a name="log-alerts-in-azure-monitor"></a>Azure İzleyici'de günlük uyarıları
Bu makalede, günlük uyarı ayrıntıları, bir içinde desteklenen uyarı türleri sağlanır [Azure uyarıları](monitoring-overview-alerts.md) ve kullanıcıların uyarmak için temel olarak Azure'nın analiz platformu kullanmasına izin verir.

Günlük araması kuralları için oluşturulan günlük uyarı oluşur [Azure Log Analytics](../azure-monitor/learn/tutorial-viewdata.md) veya [Application Insights](../application-insights/app-insights-cloudservices.md#view-azure-diagnostic-events). Kullanımı hakkında daha fazla bilgi için bkz. [Azure'da günlüğü uyarıları oluşturma](alert-log.md)

> [!NOTE]
> Popüler günlük verilerini [Azure Log Analytics](../azure-monitor/learn/tutorial-viewdata.md) şimdi de Azure İzleyici ölçüm platformda kullanılabilir. Ayrıntılar görünümü için [günlükleri için ölçüm Uyarısı](monitoring-metric-alerts-logs.md)


## <a name="log-search-alert-rule---definition-and-types"></a>Günlük araması uyarı kuralı - tanım ve türleri

Belirtilen günlük sorgularını düzenli aralıklarla otomatik olarak çalıştırmak için, Azure Uyarıları tarafından günlük arama kuralları oluşturulur.  Günlük sorgusunun sonuçları belirli ölçütlerle eşleşiyorsa bir uyarı kaydı oluşturulur. Ardından kural, [Eylem Grupları](monitoring-action-groups.md)'nı kullanıp bir veya birden çok eylemi otomatik olarak çalıştırabilir. 

Günlük arama kuralları aşağıdaki ayrıntıları tarafından tanımlanır:
- **Oturum sorgu**.  Uyarı kural her çalıştığında sorgu tetikler.  Bu sorgu tarafından döndürülen kayıtları, bir uyarı oluşturulup oluşturulmayacağını belirlemek için kullanılır. Analiz sorgusu de bulunabilir [çapraz uygulama çağrıları](https://dev.applicationinsights.io/ai/documentation/2-Using-the-API/CrossResourceQuery), [çalışma alanı çağrıları, platformlar arası ve [kaynaklar arası aramalar](../azure-monitor/log-query/cross-workspace-query.md) sağlanan kullanıcının dış uygulama için erişim haklarına sahip. 

    > [!IMPORTANT]
    > Kullanıcı olmalıdır [Azure izleme katılımcı](monitoring-roles-permissions-security.md) rol oluşturma, değiştirme ve güncelleştirme birlikte erişim; Azure İzleyici'de günlük uyarıları & Sorgu analytics hedefleri uyarı kuralı ya da uyarı sorgusu yürütme hakları. Kullanıcı oluşturma uyarı kuralı ya da uyarı sorgusu - tüm analytics hedeflere erişiminiz yoksa, kural oluşturma işlemi başarısız olabilir veya günlük uyarı kuralı ile kısmi sonuçlar yürütülür.

- **Dönem**.  Sorgu için zaman aralığını belirtir. Sorgu yalnızca bu geçerli zaman aralığı içinde oluşturulmuş olan kayıtları döndürür. Süre kötüye kullanımı önlemek günlük sorgusu için alınan verileri sınırlar ve hiçbir zaman komut bozar (önce ister) günlük sorguda kullanılan. <br>*Örneğin, zaman aralığı 60 dakika olarak ayarlanmıştır ve sorguyu, 13: 15'te çalıştırmak, 12:15 PM arasında 13: 15'te oluşturulan kayıtları döndürülür günlük sorgusu yürütülemedi. Günlük sorgusu komutu gibi önce zaman kullanıyorsa, şimdi (7 g), günlük sorgusu çalıştırılması yalnızca 1:15 PM - 12:15 PM arasında verileri için veri yalnızca son 60 dakika için varmış gibi. Yedi günün verilerini günlük sorgusu belirtildiği için değil.*
- **Sıklık**.  Sorgunun ne sıklıkta çalıştırılması gerektiğini belirtir. Herhangi bir değer 5 dakika ile 24 saat arasında olabilir. Eşittir veya süre değerinden küçük olmalıdır.  Değer süre büyük ise, eksik kayıtları riski oluşur.<br>*Örneğin, 30 dakikalık bir süre ve 60 dakikalık bir sıklık göz önünde bulundurun.  Sorgu 1: 00'da çalışıyorsa, 12:30 ve 1:00 arasında kayıtları döndürür.  Sorgu çalıştıracağınız sonraki 2:00 kayıtlar 1:30 ve 2:00 arasında zaman döndürecekti zamandır.  1:30 ile 1:00 arasında oluşturulan kayıtları hiç hesaplanmadı.*
- **Eşik**.  Günlük aramasının sonuçları, bir uyarının oluşturulması gerekip gerekmediğini belirlemek için değerlendirilir.  Farklı türde günlük araması uyarı kuralları için eşik farklıdır.

Günlük arama kuralları olması için [Azure Log Analytics](../azure-monitor/learn/tutorial-viewdata.md) veya [Application Insights](../application-insights/app-insights-cloudservices.md#view-azure-diagnostic-events), iki türde olabilir. Bu türlerinin her birini ayrıntılı olarak bölümlerde açıklanmıştır.

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

#### <a name="example-of-number-of-records-type-log-alert"></a>Sayı, kayıt türü günlük uyarı örneği
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
    > Depo işlev sorgusunda aralığını belirtmek için kullanılmalıdır. Eşit zaman aralıkları - bin() oluşturacağından uyarı otomatik olarak depo komut çalışma zamanı, uygun zamanda bin_at komutuyla sabit bir nokta sonuçlar sağlamak için dönüştürülecektir. Ölçüm ölçüsü günlük uyarı türünü tekil bin() komutu sorgularla çalışmak için tasarlanmıştır
    
- **Eşik**: ölçüm ölçüsü uyarı kuralları için eşik ihlallerinin sayısı ve toplam değer ile tanımlanır.  Günlük araması'nda herhangi bir veri noktasının bu değeri aşarsa, bir ihlal dikkate almıştır.  Ardından ihlalleriyle sonuçları herhangi bir nesne sayısı belirtilen değeri aşarsa, bu nesne için bir uyarı oluşturulur.

#### <a name="example-of-metric-measurement-type-log-alert"></a>Ölçüm ölçüsü türü günlük uyarı örneği
Burada herhangi bir bilgisayarda, % 90'ın işlemci kullanımı üç kez tekrar 30 dakika aşılırsa bir uyarı istiyordu bir senaryo düşünün.  Bir uyarı kuralı aşağıdaki ayrıntılarla oluşturacak:  

- **Sorgu:** Perf | burada ObjectName "İşlemci" ve CounterName == "% işlemci zamanı" == | Summarize aggregatedvalue = avg(CounterValue) bin (TimeGenerated, 5 milyon), bilgisayar tarafından<br>
- **Zaman aralığı:** 30 dakika<br>
- **Uyarı sıklığı:** beş dakika<br>
- **Toplam değer:** 90'dan büyük<br>
- **Bağlı olarak uyarıyı Tetikle:** toplam ihlal sayısı 2'den büyük<br>

Sorguyu 5 dakikalık aralıklarla her bilgisayar için ortalama bir değer oluşturur.  Bu sorgu, önceki 30 dakika boyunca 5 dakikada bir toplanan veriler için çalıştırılmaz.  Örnek verileri varsayılan olarak, üç bilgisayar için aşağıda gösterilmiştir.

![Örnek sorgu sonuçları](media/monitor-alerts-unified-log/metrics-measurement-sample-graph.png)

Bunlar üç kez zaman aralığında toplanan % 90 eşiğini ihlal olduğundan bu örnekte, ayrı uyarılar srv02 srv03 için oluşturulmuş olması.  Varsa **bağlı olarak uyarıyı Tetikle:** üzere değiştirilmiştir **art arda** sonra üç ardışık örnekler eşiği ihlal olduğundan bir uyarı yalnızca srv03 için oluşturulacak.

## <a name="log-search-alert-rule---firing-and-state"></a>Günlük araması uyarı kuralı - Açmadığınızda ve durumu
Günlük araması uyarı kuralı yapılandırması ve kullanılan özel bir analytics sorgusunu göre kullanıcı tarafından predicated mantığı üzerinde çalışır. Tam bir koşul veya neden neden uyarı kuralı gereken mantığını itibaren tetikleyici her günlük uyarı kuralı değişebilir bir Analytics sorgusunun içinde-kapsüllenir. Azure uyarıları günlük araması uyarı kuralı eşiğini koşulu karşılanmadığından veya aşıldığından olduğunda belirli temel alınan kök neden günlük sonuçları içinde önemli bilgileri içeriyor. Bu nedenle, günlük uyarıları için olarak durumu olmadan verilir ve günlük arama sonucunu günlük uyarıları içinde belirtilen eşiği aşması yeterli her kullanıldığında harekete *sonuç sayısı* veya *ölçüm ölçüsü* türü koşulu. Ve tarafından sağlanan özel analytics sorgusu sonucunu Uyarı koşulu karşılandı sürece uyarı kuralları sürekli tetikleme tutmak, oturum; uyarı olmadan her çözülmüş. Kullanıcı tarafından sağlanan analytics sorgusunun içindeki nedeninin tam kök-hata izleme mantığını maskelenmiş; yaratacağı eşiği karşılamayan günlük arama sonucunu sorunun çözüm gösterip göstermeyeceğini anlaşılmasını Azure uyarıları hiçbir anlamı yoktur.

Artık olarak adlandırılan bir günlük uyarı kuralı sahibiz varsayar *Contoso günlüğü Uyarısı*, başına yapılandırma gibi [sonuçlarını numarası türü günlüğü uyarısı için sağlanan örneği](#example-of-number-of-records-type-log-alert). 
- Saat 13:05 Contoso günlüğü uyarısı göre Azure uyarıları ne zaman yürütülmesi'te, 0 kaydı günlük arama sonucunu veriyor; eşiğini ve uyarı tetikleme değil dolayısıyla aşağıda. 
- 1:10 PM Contoso günlüğü uyarısı göre Azure uyarıları ne zaman yürütülmesi sırasında bir sonraki yinelemede, günlük arama sonucunu 5 kaydı sağlanan; Eşiği aşan ve tetikleyerek hemen sonra uyarı tetikleme [eylem grubu](monitoring-action-groups.md) ilişkili. 
- 1: saat 15 Contoso günlüğü uyarısı göre Azure uyarıları ne zaman yürütülmesi'te, günlük arama sonucunu 2 kayıtları sağlanan; Eşiği aşan ve tetikleyerek hemen sonra uyarı tetikleme [eylem grubu](monitoring-action-groups.md) ilişkili.
- Artık 1:20 PM Contoso günlüğü uyarısı Azure uyarı tarafından ne zaman yürütülmesi sırasında bir sonraki yinelemede, günlük arama sonucunu tekrar 0 kaydı sağlanan; eşiğini ve uyarı tetikleme değil dolayısıyla aşağıda.

Ancak, yukarıda listelenen durumda da, 13: 15'te - Azure uyarıları 1: 10'da görüldüğü temel sorunların kalıcı olduğunu ve net yeni hatalar olup olmadığını belirleyemiyor. kullanıcı tarafından sağlanan sorgu hesaba önceki kayıt - katılarak olarak Azure uyarılarına emin olabilirsiniz. Bu nedenle dikkatli olun, Contoso günlük uyarı verildiğinde tarafında hata için 13: 15'te, yapılandırılmış yürütülen [eylem grubu](monitoring-action-groups.md) yeniden harekete geçirilir. Artık saat 13:20, kayıt görülen - Azure uyarıları emin olamaz'te kayıtları nedenini çözümlendikten; Bu nedenle Contoso günlük uyarı çözümlendi olarak Azure uyarı Pano ve/veya göz uyarının çözümleme belirten gönderilen bildirimleri değişmemiş olur.


## <a name="pricing-and-billing-of-log-alerts"></a>Fiyatlandırma ve faturalama günlük uyarıları
Günlük uyarıları fiyatlandırması geçerli belirtildikten adresindeki [Azure İzleyici fiyatlandırma](https://azure.microsoft.com/pricing/details/monitor/) sayfası. Azure faturaları günlük uyarıları tür olarak temsil edilir `microsoft.insights/scheduledqueryrules` ile:
- Kaynak grubu ve uyarı özellikleri ile birlikte tam uyarı adı ile gösterilen Application ınsights günlük uyarıları
- Günlük uyarıları Log Analytics ile uyarı adı gösterilen üzerinde `<WorkspaceName>|<savedSearchId>|<scheduleId>|<ActionId>` yanı sıra kaynak grubu ve uyarı özellikleri

    > [!NOTE]
    > Tüm kayıtlı aramalar, çizelgeler ve günlük analizi API'si ile oluşturulan eylem adını, küçük olmalıdır. Geçersiz gibi karakterler varsa `<, >, %, &, \, ?, /` olan kullanılan - bunlar ile değiştirilecek `_` , faturada.

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [azure'da günlüğü uyarıları oluşturma](alert-log.md).
* Anlamak [günlük uyarıları azure'da Web kancalarıyla](monitor-alerts-unified-log-webhook.md).
* Hakkında bilgi edinin [Azure uyarıları](monitoring-overview-alerts.md).
* Daha fazla bilgi edinin [Application Insights](../application-insights/app-insights-analytics.md).
* Daha fazla bilgi edinin [Log Analytics](../azure-monitor/log-query/log-query-overview.md).    
