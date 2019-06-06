---
title: Azure İzleyici'de günlük uyarıları
description: Azure Uyarıları'için belirttiğiniz analitik sorgu koşullar karşılandığında tetikleyici e-posta, bildirimleri, Web siteleri URL'leri (Web kancaları) ya da Otomasyon çağırın.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 5/31/2019
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: ae35c735cffeb8cd85af1f32bb2d14ede6dc6b69
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427421"
---
# <a name="log-alerts-in-azure-monitor"></a>Azure İzleyici'de günlük uyarıları

Bu makalede, günlük uyarı ayrıntıları, bir içinde desteklenen uyarı türleri sağlanır [Azure uyarıları](../../azure-monitor/platform/alerts-overview.md) ve kullanıcıların uyarmak için temel olarak Azure'nın analiz platformu kullanmasına izin verir.

Günlük araması kuralları için oluşturulan günlük uyarı oluşur [Azure İzleyici günlüklerine](../../azure-monitor/learn/tutorial-viewdata.md) veya [Application Insights](../../azure-monitor/app/cloudservices.md#view-azure-diagnostics-events). Kullanımı hakkında daha fazla bilgi için bkz. [Azure'da günlüğü uyarıları oluşturma](../../azure-monitor/platform/alerts-log.md)

> [!NOTE]
> Popüler günlük verilerini [Azure İzleyici günlüklerine](../../azure-monitor/learn/tutorial-viewdata.md) şimdi de Azure İzleyici ölçüm platformda kullanılabilir. Ayrıntılar görünümü için [günlükleri için ölçüm Uyarısı](../../azure-monitor/platform/alerts-metric-logs.md)


## <a name="log-search-alert-rule---definition-and-types"></a>Günlük araması uyarı kuralı - tanım ve türleri

Belirtilen günlük sorgularını düzenli aralıklarla otomatik olarak çalıştırmak için, Azure Uyarıları tarafından günlük arama kuralları oluşturulur.  Günlük sorgusunun sonuçları belirli ölçütlerle eşleşiyorsa bir uyarı kaydı oluşturulur. Ardından kural, [Eylem Grupları](../../azure-monitor/platform/action-groups.md)'nı kullanıp bir veya birden çok eylemi otomatik olarak çalıştırabilir. [Azure izleme katılımcı](../../azure-monitor/platform/roles-permissions-security.md) rolü, oluşturma, değiştirme ve günlük uyarıları güncelleştirme gerekebilir; uyarı kuralı ya da uyarı sorgusu analytics hedefleri için erişim ve sorgu yürütme hakları yanı sıra. Kullanıcı oluşturma, tüm analytics hedeflere uyarı kuralı ya da uyarı sorgusu - erişimi yok durumunda kuralı oluşturma işlemi başarısız olabilir veya günlük uyarı kuralı ile kısmi sonuçlar yürütülür.

Günlük arama kuralları aşağıdaki ayrıntıları tarafından tanımlanır:

- **Oturum sorgu**.  Uyarı kural her çalıştığında sorgu tetikler.  Bu sorgu tarafından döndürülen kayıtları, bir uyarı tetiklenmesi için uygun olup olmadığını belirlemek için kullanılır. Analiz sorgusu belirli Log Analytics çalışma alanı veya Application Insights uygulama için kullanılabilir ve arasında bile span [birden fazla Log Analytics ve Application Insights kaynağı](../../azure-monitor/log-query/cross-workspace-query.md#querying-across-log-analytics-workspaces-and-from-application-insights) olanağının erişebildiği yanı sıra sağlanan sorgu tüm haklar kaynaklar. 
    > [!IMPORTANT]
    > [kaynaklar arası sorgu](../../azure-monitor/log-query/cross-workspace-query.md#querying-across-log-analytics-workspaces-and-from-application-insights) günlük uyarıları için Application ınsights'ı ve günlük uyarıları için destek [Log Analytics scheduledQueryRules API kullanılarak yapılandırılan](../../azure-monitor/platform/alerts-log-api-switch.md) yalnızca.

    Bazı analitik komutları ve birleşimleri günlük uyarıları kullanımda ile uyumsuz; Daha fazla ayrıntı görüntülemek için [uyarı sorguları Azure İzleyici'de oturum](../../azure-monitor/platform/alerts-log-query.md).

- **Dönem**.  Sorgu için zaman aralığını belirtir. Sorgu yalnızca bu geçerli zaman aralığı içinde oluşturulmuş olan kayıtları döndürür. Süre kötüye kullanımı önlemek günlük sorgusu için alınan verileri sınırlar ve hiçbir zaman komut bozar (önce ister) günlük sorguda kullanılan. <br>*Örneğin, zaman aralığı 60 dakika olarak ayarlanmıştır ve sorguyu, 13: 15'te çalıştırmak, 12:15 PM arasında 13: 15'te oluşturulan kayıtları döndürülür günlük sorgusu yürütülemedi. Günlük sorgusu komutu gibi önce zaman kullanıyorsa, şimdi (7 g), günlük sorgusu çalıştırılması yalnızca 1:15 PM - 12:15 PM arasında verileri için veri yalnızca son 60 dakika için varmış gibi. Yedi günün verilerini günlük sorgusu belirtildiği için değil.*

- **Sıklık**.  Sorgunun ne sıklıkta çalıştırılması gerektiğini belirtir. Herhangi bir değer 5 dakika ile 24 saat arasında olabilir. Eşittir veya süre değerinden küçük olmalıdır.  Değer süre büyük ise, eksik kayıtları riski oluşur.<br>*Örneğin, 30 dakikalık bir süre ve 60 dakikalık bir sıklık göz önünde bulundurun.  Sorgu 1: 00'da çalışıyorsa, 12:30 ve 1:00 arasında kayıtları döndürür.  Sorgu çalıştıracağınız sonraki 2:00 kayıtlar 1:30 ve 2:00 arasında zaman döndürecekti zamandır.  1:30 ile 1:00 arasında oluşturulan kayıtları hiç hesaplanmadı.*

- **Eşik**.  Günlük aramasının sonuçları, bir uyarının oluşturulması gerekip gerekmediğini belirlemek için değerlendirilir.  Farklı türde günlük araması uyarı kuralları için eşik farklıdır.

Günlük arama kuralları olması için [Azure İzleyici günlüklerine](../../azure-monitor/learn/tutorial-viewdata.md) veya [Application Insights](../../azure-monitor/app/cloudservices.md#view-azure-diagnostics-events), iki türde olabilir. Bu türlerinin her birini ayrıntılı olarak bölümlerde açıklanmıştır.

- **[Sonuç sayısı](#number-of-results-alert-rules)** . Günlük araması tarafından döndürülen kayıt sayısını belirtilen bir sayıyı aştığında oluşturulan tek bir uyarı.
- **[Ölçüm ölçüsü](#metric-measurement-alert-rules)** .  Uyarı belirtilen Eşiği aşan değerler ile günlük arama sonuçlarındaki her nesne için oluşturuldu.

Uyarı kuralı türleri arasındaki farklar aşağıdaki gibidir.

- *Sonuç sayısı* uyarı kuralları her zaman tek bir uyarı, süre oluşturur *ölçüm ölçüsü* uyarı kuralı eşiğini aştığında her nesne için bir uyarı oluşturur.
- *Sonuç sayısı* tek bir kez eşiği aştığında bir uyarı uyarı kuralları oluşturma. *Ölçüm ölçüsü* uyarı kuralları, bir uyarı oluşturabilir, eşiği aşıldığında belirli bir süre boyunca belirli bir sayı.

### <a name="number-of-results-alert-rules"></a>Sonuçları uyarı kuralları sayısı

**Sonuç sayısı** arama sorgu tarafından döndürülen kayıt sayısını belirtilen eşiği aştığında uyarı kuralları tek bir uyarı oluşturun. Bu uyarı kuralının türünü, Windows olay günlükleri, Syslog, WebApp yanıt ve özel günlükleri gibi olayları ile çalışmak için idealdir.  Belirli hata olayı oluştururken ya da belirli bir süre içinde birden çok hata olayı oluşturulduğunda bir uyarı oluşturmak isteyebilirsiniz.

**Eşik**: Sonuçları uyarı kuralları sayısı için eşik değerinden büyük veya belirli bir değerden daha az.  Ardından günlük arama tarafından döndürülen kayıt sayısını bu ölçütlerle eşleşiyorsa bir uyarı oluşturulur.

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

**Ölçüm ölçüsü** uyarı kuralları belirtilen eşiği aşması ve tetikleyici koşulu belirtilen değerle bir sorgu her nesne için bir uyarı oluştur. Farklı **sonuç sayısı** uyarı kuralları, **ölçüm ölçüsü** analiz sonucu bir zaman serisi sağladığında uyarı kuralları çalışır. Aşağıdaki farklı fark sahip oldukları **sonuç sayısı** uyarı kuralları.

- **Toplama işlevi**: Gerçekleştirilen hesaplama ve büyük olasılıkla bir sayısal belirler toplanacak alan.  Örneğin, **Count() işlevi** sorguda kayıt sayısını döndürür **avg(CounterValue)** aralığında Ort alanın ortalamasını döndürür. Adlı/adlı sorgu toplama işlevi olması gerekir: AggregatedValue ve sayısal bir değer sağlayın. 

- **Alan grubu**: Bu alan her bir örneği için bir toplu değer içeren bir kayıt oluşturulur ve her biri için bir uyarı oluşturulabilir.  Örneğin, her bilgisayar için bir uyarı oluşturmak istediyseniz, kullanacağınız **bilgisayar tarafından**. Var olması durumunda, uyarı sorgusu üzerinde belirtilmiş birden çok grup alanları, kullanıcı kullanarak sıralamak için kullanılacak alanı sonuçlarını belirtebilirsiniz **üzerinde toplama** (metricColumn) parametresi

    > [!NOTE]
    > *Üzerinde toplama* (metricColumn) seçeneği, ölçüm ölçüsü türü günlük uyarıları için Application ınsights'ı ve günlük uyarıları için kullanılabilir [Log Analytics scheduledQueryRules API kullanılarak yapılandırılan](../../azure-monitor/platform/alerts-log-api-switch.md) yalnızca.

- **Aralığı**:  Hangi veriler toplanır zaman aralığını tanımlar.  Örneğin, belirttiğiniz **beş dakika**, bir kayıt için uyarı belirtilen süre boyunca 5 dakikalık aralıklarla toplanan grup alanının her örneği için oluşturulması.

    > [!NOTE]
    > Depo işlev sorgusunda aralığını belirtmek için kullanılmalıdır. Eşit zaman aralıkları - bin() oluşturacağından uyarı otomatik olarak depo komut çalışma zamanı, uygun zamanda bin_at komutuyla sabit bir nokta sonuçlar sağlamak için dönüştürülecektir. Ölçüm ölçüsü günlük uyarı türünü bin() komutunun sağlayan üç adede kadar örnek içeren sorgularla çalışmak için tasarlanmıştır
    
- **Eşik**: Ölçüm ölçüsü uyarı kuralları için eşik ihlallerinin sayısı ve toplam değer ile tanımlanır.  Günlük araması'nda herhangi bir veri noktasının bu değeri aşarsa, bir ihlal dikkate almıştır.  Ardından ihlalleriyle sonuçları herhangi bir nesne sayısı belirtilen değeri aşarsa, bu nesne için bir uyarı oluşturulur.

Yanlış yapılandırılması *üzerinde toplama* veya *metricColumn* seçeneği misfire için uyarı kuralları neden olabilir. Daha fazla bilgi için [ölçüm ölçüsü uyarı kuralı yanlış olduğunda sorun giderme](alert-log-troubleshoot.md#metric-measurement-alert-rule-is-incorrect).

#### <a name="example-of-metric-measurement-type-log-alert"></a>Ölçüm ölçüsü türü günlük uyarı örneği

Burada herhangi bir bilgisayarda, % 90'ın işlemci kullanımı üç kez tekrar 30 dakika aşılırsa bir uyarı istiyordu bir senaryo düşünün.  Bir uyarı kuralı aşağıdaki ayrıntılarla oluşturacak:  

- **Sorgu:** Perf | Burada ObjectName "İşlemci" ve CounterName == "% işlemci zamanı" == | Summarize aggregatedvalue = avg(CounterValue) bin (TimeGenerated, 5 milyon), bilgisayar tarafından<br>
- **Zaman aralığı:** 30 dakika<br>
- **Uyarı sıklığı:** beş dakika<br>
- **Alert Logic - koşul & eşiği:** 90'dan büyük<br>
- **Alan (toplama üzerinde):** Computer
- **Bağlı olarak uyarıyı Tetikle:** 2'den büyük toplam ihlal sayısı<br>

Sorguyu 5 dakikalık aralıklarla her bilgisayar için ortalama bir değer oluşturur.  Bu sorgu, önceki 30 dakika boyunca 5 dakikada bir toplanan veriler için çalıştırılmaz. Seçilen alan (toplam açma) sütunlu 'bilgisayara' - olduğundan AggregatedValue 'Bilgisayara' çeşitli değerleri için ayrılır ve ortalama işlemci kullanımını her bilgisayar için 5 dakikalık bir zaman depo belirlenir.  Örnek sorgu sonucu için (örneğin üç bilgisayar) olacak şekilde aşağıda.


|TimeGenerated [UTC] |Computer  |AggregatedValue  |
|---------|---------|---------|
|20xx-xx-xxT01:00:00Z     |   Srv01.contoso.com      |    72     |
|20xx-xx-xxT01:00:00Z     |   SRV02.contoso.com      |    91     |
|20xx-xx-xxT01:00:00Z     |   srv03.contoso.com      |    83     |
|...     |   ...      |    ...     |
|20xx-xx-xxT01:30:00Z     |   Srv01.contoso.com      |    88     |
|20xx-xx-xxT01:30:00Z     |   SRV02.contoso.com      |    84     |
|20xx-xx-xxT01:30:00Z     |   srv03.contoso.com      |    92     |

Sorgu sonucu çizilmesini olduysa, olarak görünür.

![Örnek sorgu sonuçları](media/alerts-unified-log/metrics-measurement-sample-graph.png)

Bu örnekte, 5 dakika depo her üç bilgisayar - 5 dakika için ortalama işlemci kullanımı olarak hesaplanan görüyoruz. Yalnızca bir kez srv01 tarafından 1:25 ihlal 90 eşiğini depo. Buna karşılık, srv02 1: 10'da 90 eşiğini aşan 1:15 ve 1:25 depo; srv03 1: 10'da, 1:15, 90 eşiğini aşıyor ancak 1:20 ve 1:30.
Uyarı şekilde yapılandırıldığından toplam ihlal sayısı üzerinde tetiklenecek ikiden, srv02 ve srv03 yalnızca ölçütlere uyan olduğunu görüyoruz. Bunlar birden çok zaman depo % 90 eşiğini ihlal olduğundan bu nedenle ayrı uyarılar srv02 srv03 için oluşturulmuş olması.  Varsa *bağlı olarak uyarıyı Tetikle:* parametresi için bunun yerine yapılandırılmışsa *sürekli ihlallerini* uyarı tetiklendi seçeneğini **yalnızca** bunu ihlal beri srv03 için üç ardışık zaman depo 1: 10'dan 1:20 eşiği. Ve **değil** srv02 için olarak ihlal 1: 10'dan 1:15 iki ardışık zaman depo eşiği.

## <a name="log-search-alert-rule---firing-and-state"></a>Günlük araması uyarı kuralı - Açmadığınızda ve durumu

Günlük araması uyarı kuralı yapılandırması ve kullanılan özel bir analytics sorgusunu göre kullanıcı tarafından predicated mantığı üzerinde çalışır. İzleme mantığını itibaren tam bir koşul veya uyarı kuralı neden tetiklemesi gereken neden dahil olmak üzere, her günlük uyarı kuralı farklı olabilir ve analytics sorgu - kapsüllenir. Azure uyarıları, önemli bilgileri belirli temel alınan kök neden (veya) günlük araması uyarı kuralı eşiğini koşulu karşılanmadığından veya aşıldığından zaman değerlendirilen senaryo vardır. Bu nedenle günlük uyarıları gibi durumu-küçük denir. Ve günlük uyarı kuralları tarafından sağlanan özel analytics sorgusu sonucunu Uyarı koşulu karşılandı sürece tetikleme tutun. Uyarı olmadan tam kök hata izleme neden mantığını kullanıcı tarafından sağlanan analytics sorgusunun içindeki maskelenir gibi her, çözülmüş. Yüklenmekte olan uyarıları yaratacağı kök nedeni çözüldüğünde çıkarmaya Azure izleme mekanizması vardır.

Bize pratik bir örnekle aynı bkz olanak tanır. Adlı bir günlük uyarı kuralı sahibiz varsayar *Contoso günlüğü Uyarısı*, başına yapılandırma gibi [sonuçlarını numarası türü günlüğü uyarısı için sağlanan örneği](#example-of-number-of-records-type-log-alert) - burada özel uyarı sorgusu için 500 aramak için tasarlanmıştır Sonuç kodu günlüğüne kaydeder.

- Saat 13:05 Contoso günlüğü uyarısı göre Azure uyarıları ne zaman yürütülmesi'te, sıfır kayıt 500 olan sonuç kodu ile günlük arama sonucunu veriyor. Sıfır eşiğin altındadır ve uyarı başlatılmamış olduğundan.
- 1:10 PM Contoso günlüğü uyarısı göre Azure uyarıları ne zaman yürütülmesi sırasında bir sonraki yinelemede, günlük arama sonucunu beş kayıtlarla Sonuç kodu 500 sağlanır. Beş eşiğini ve uyarı ile ilişkili eylemler tetiklenir tetiklenir.
- 1: saat 15 Contoso günlüğü uyarısı göre Azure uyarıları ne zaman yürütülmesi'te, iki kayıt 500 Sonuç kodu ile günlük arama sonucunu sağlanır. İki eşiğini ve uyarı ile ilişkili eylemler tetiklenir tetiklenir.
- Artık 1:20 PM Contoso günlüğü uyarısı Azure uyarı tarafından ne zaman yürütülmesi sırasında bir sonraki yinelemede, günlük arama sonucunu 500 Sonuç kodu yeniden sıfır kayıtlarla sağlanmaktadır. Sıfır eşiğin altındadır ve uyarı başlatılmamış olduğundan.

Ancak, yukarıda listelenen durumda da, 13: 15'te - Azure uyarıları 1: 10'da görüldüğü temel sorunların kalıcı olduğunu ve net yeni hatalar olup olmadığını belirleyemiyor. Kullanıcı tarafından sağlanan sorgu hesaba önceki kayıt - katılarak olarak Azure uyarılarına emin olabilirsiniz. Mantıksal uyarı için uyarı sorguda - kapsüllenir böylece iki 500 Sonuç kodu 13: 15'te görüldüğü kayıtlarla olabilir veya 1:10 PM zaten görüldü değil. Bu nedenle dikkatli kenarındaki Contoso günlüğü uyarısı 13: 15'te yürütülürken hata için yapılandırılan bir eylem yeniden tetiklenir. Artık saat 13:20 olduğunda sıfır kayıt görülür 500 sonuç koduyla - Azure uyarıları 500 Sonuç kodu 1:10 PM ve 13: 15'te görüldüğü nedenini artık çözülür ve Azure İzleyici uyarılar güvenle çıkarabilir belirli olamaz'te 500 hata sorunlarını aynı nedenden dolayı yapılmaz yeniden s. Bu nedenle Contoso günlük uyarı çözümlendi olarak Azure uyarı Pano ve/veya göz uyarının çözümleme belirten gönderilen bildirimleri değişmemiş olur. Bunun yerine tam bir koşul veya embedded analytics sorgunuzda mantıksal nedeni anlayan kullanıcı şunları yapabilir [uyarı kapalı olarak işaretleme](alerts-managing-alert-states.md) gerektiğinde.

## <a name="pricing-and-billing-of-log-alerts"></a>Fiyatlandırma ve faturalama günlük uyarıları

Günlük uyarıları fiyatlandırması geçerli belirtildikten adresindeki [Azure İzleyici fiyatlandırma](https://azure.microsoft.com/pricing/details/monitor/) sayfası. Azure faturaları günlük uyarıları tür olarak temsil edilir `microsoft.insights/scheduledqueryrules` ile:

- Kaynak grubu ve uyarı özellikleri ile birlikte tam uyarı adı ile gösterilen Application ınsights günlük uyarıları
- Kaynak grubu ve uyarı özellikleri ile birlikte tam uyarı adı ile gösterilen Log Analytics günlük uyarılarını; oluşturulan kullanırken [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules)

[Eski Log Analytics API](../../azure-monitor/platform/api-alerts.md) uyarı eylemleri ve zamanlamaları Log Analytics kayıtlı arama ve olmayan uygun kapsamında olan [Azure kaynaklarını](../../azure-resource-manager/resource-group-overview.md). Bu nedenle böyle eski günlük uyarıları için faturalama için oluşturulan Log Analytics Azure portalını kullanarak için **olmadan** [yeni API'ye yönelik geçiş](../../azure-monitor/platform/alerts-log-api-switch.md) veya aracılığıyla [eski Log Analytics API](../../azure-monitor/platform/api-alerts.md) - Gizli sahte uyarı kuralları oluşturulur `microsoft.insights/scheduledqueryrules` azure'da faturalandırma için. Faturalandırma üzerinde oluşturulan gizli sahte uyarı kuralları `microsoft.insights/scheduledqueryrules` olarak gösterildiği gibi `<WorkspaceName>|<savedSearchId>|<scheduleId>|<ActionId>` yanı sıra kaynak grubu ve uyarı özellikleri.

> [!NOTE]
> Geçersiz gibi karakterler varsa `<, >, %, &, \, ?, /` mevcut ile değiştirilecek `_` gizli sahte uyarı kuralı adını ve bu nedenle de Azure fatura.

Uyarı kuralları kullanarak faturalandırması oluşturulan gizli scheduleQueryRules kaynakları kaldırmak için [eski Log Analytics API](api-alerts.md), kullanıcı aşağıdakilerden herhangi birini gerçekleştirebilirsiniz:

- Her iki kullanıcı [API tercih uyarı kuralları için Log Analytics çalışma alanı için geçiş](../../azure-monitor/platform/alerts-log-api-switch.md) ve uyarı kuralları ya da izleme taşıma için Azure Resource Manager uyumlu kaybı olmadan [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules). Böylece gizli sahte uyarı kuralları faturalandırma ihtiyacını ortadan kaldırır.
- Veya kullanıcı API tercih geçiş yapmak istediğiniz değil, kullanıcının gerekecek **Sil** özgün zamanlamayı ve uyarı eylemi kullanarak [eski Log Analytics API](api-alerts.md) veya sildiğiniz [Azure portalı özgün günlük uyarı kuralı](../../azure-monitor/platform/alerts-log.md#view--manage-log-alerts-in-azure-portal)

Ayrıca kullanarak uyarı kuralları faturalandırması oluşturulan gizli scheduleQueryRules kaynakları için [eski Log Analytics API](api-alerts.md), bir değiştirme işlemi gibi PUT başarısız olur. Olarak `microsoft.insights/scheduledqueryrules` türü sözde kurallardır faturalandırma kullanılarak oluşturulan uyarı kuralları bir amaç için [eski Log Analytics API](api-alerts.md). Uyarı kuralı değişiklikler yapılması gerektiğini kullanarak [eski Log Analytics API](api-alerts.md) (veya) kullanıcı [uyarı kuralları için API tercih geçiş](../../azure-monitor/platform/alerts-log-api-switch.md) kullanılacak [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) Bunun yerine.

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [azure'da günlüğü uyarıları oluşturma](../../azure-monitor/platform/alerts-log.md).
* Anlamak [günlük uyarıları azure'da Web kancalarıyla](alerts-log-webhook.md).
* Hakkında bilgi edinin [Azure uyarıları](../../azure-monitor/platform/alerts-overview.md).
* Daha fazla bilgi edinin [Application Insights](../../azure-monitor/app/analytics.md).
* Daha fazla bilgi edinin [Log Analytics](../../azure-monitor/log-query/log-query-overview.md).