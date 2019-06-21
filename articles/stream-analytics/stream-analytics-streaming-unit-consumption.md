---
title: Anlayabilmemiz ve Azure Stream Analytics, akış birimleri
description: Bu makalede, akış birimleri ayarı ve Azure Stream Analytics performansını etkileyen diğer faktörlere açıklanır.
services: stream-analytics
author: JSeb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/17/2019
ms.openlocfilehash: acafd6d8f37edd3e16561a4e588556bb771619f8
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206702"
---
# <a name="understand-and-adjust-streaming-units"></a>Anlayabilmemiz ve akış birimleri

Akış birimleri (su), bir işin yürütülmesi için ayrılan işlem kaynakları temsil eder. SU sayısı ne kadar büyük olursa işe ayrılan CPU ve bellek kaynakları o kadar fazla olur. Bu sorgu mantığına odaklanabilir kapasite sağlar ve, Stream Analytics'i çalıştırmak için donanım yönetmenize gerek zamanında iş özetleri.

Düşük gecikme süreli akış işlemeyi başarabilmek için, Azure Stream Analytics işleri tüm işlemi bellekte gerçekleştirir. Bellek yetersiz çalıştırırken, iş akışında başarısız olur. Sonuç olarak, bir üretim iş için bir akış işin kaynak kullanımını izlemek ve 7/24 çalışan işleri tutmak için ayrılmış yeterli kaynak bulunduğundan emin olun önemlidir.

% 0, % 100 aralıkları, SU % utilization ölçümünü iş yükünüz bellek kullanımını açıklar. Bu ölçüm, en küçük ayak izine sahip bir akış işi için %10 20 arasında genellikle olur. SU kullanım yüzdesi düşüktür ve giriş olaylarını biriktirme listesine alınan, yükünüz büyük olasılıkla SUs sayısını artırmanız gerektiren daha fazla işlem kaynakları gerektirir. % Zaman iniş hesap için 80 aşağıda SU ölçüm tutmak en iyisidir. Microsoft, Kaynak Tükenmesi önlemek için % 80 SU kullanım ölçüm uyarı ayarını önerir. Daha fazla bilgi için [Öğreticisi: Azure Stream Analytics işleri için uyarıları ayarlama](stream-analytics-set-up-alerts.md).

## <a name="configure-stream-analytics-streaming-units-sus"></a>Stream Analytics, akış birimleri (su) yapılandırma
1. [Azure portalda](https://portal.azure.com/) oturum açın.

2. Kaynak listesinde, ölçeklendirme ve sonra açmak istediğiniz Stream Analytics işini bulun. 

3. İş sayfasının altında **yapılandırma** başlığı seçin **ölçek**. 

    ![Azure portal Stream Analytics işi yapılandırma][img.stream.analytics.preview.portal.settings.scale]
    
4. İş için SUs ayarlamak için kaydırıcıyı kullanın. SU ayarlarınıza sınırlı olduğuna dikkat edin. 

## <a name="monitor-job-performance"></a>İş performansını izleme
Azure portalını kullanarak bir işin aktarım hızı izleyebilirsiniz:

![Azure Stream Analytics işlerini izleme][img.stream.analytics.monitor.job]

İş yükü, beklenen aktarım hızıyla hesaplayın. Aktarım hızı, beklenenden daha az ise, giriş bölümünü ayarlamak, sorguyu ayarlama ve işiniz için SUs ekleyin.

## <a name="how-many-sus-are-required-for-a-job"></a>İş için kaç su gereklidir?

Belirli bir proje için gerekli SUs sayısını seçme girişleri ve iş içinde tanımlanan bir sorgu için bölümlendirme yapılandırmasına bağlıdır. **Ölçek** sayfası SUs doğru sayıda ayarlamanıza olanak sağlar. Gerekli olandan daha fazla SUs ayırmak için iyi bir uygulamadır. Stream Analytics işleme altyapısı, gecikme ve ek bellek ayırma, performans için iyileştirir.

Genel olarak, kullanmayan sorgular için 6 SUs başlatmak için en iyi yöntem olacaktır **PARTITION BY**. Ardından tatlı nokta temsili miktarda veri aktarmak ve SU % Utilization ölçümünü inceleyin sonra SUs sayısını değiştirerek bir deneme yanılma yöntemini kullanarak belirler. Stream Analytics işi tarafından kullanılan akış birimi sayısı, iş ve her bir adımın bölüm sayısı için tanımlanan sorgu, adım sayısı bağlıdır. Sınırları hakkında daha fazla bilgi [burada](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#calculate-the-maximum-streaming-units-of-a-job).

SUs doğru sayıda seçme hakkında daha fazla bilgi için bu sayfaya bakın: [Verimliliği artırmak için Azure Stream Analytics işlerini ölçeklendirme](stream-analytics-scale-jobs.md)

> [!Note]
> Kaç su seçerek belirli bir işin girişler için bölümlendirme yapılandırmasına ve iş için tanımlanan sorguya bağlıdır için gereklidir. SUs kota bir iş için en fazla seçebilirsiniz. Varsayılan olarak, her Azure aboneliği, belirli bir bölgede bir kota analytics işleri için en fazla 500 sus'tan sahiptir. Aboneliğiniz bu kotayı aşan SUs artırmak için bağlantı [Microsoft Support](https://support.microsoft.com). İş başına SUs için geçerli değerler şunlardır: 1, 3, 6 ve 6'lık artışlarla.

## <a name="factors-that-increase-su-utilization"></a>SU kullanım yüzdesi artırmak faktörleri 

Zamana bağlı sorgu (zaman tabanlı), durum bilgisi olan işleçleri Stream Analytics tarafından sağlanan dizi temel öğeleridir. Stream Analytics, kullanıcı adına bu işlemleri dahili olarak durumunu bellek tüketimi, dayanıklılık ve durumu kurtarma için denetim noktası hizmeti yükseltmeleri sırasında yöneterek yönetir. Stream Analytics, tamamen durumları yöneten olsa da, bir dizi kullanıcılar göz önünde bulundurmanız gereken en iyi yöntem önerileri vardır.

Not bile, sürekli olarak giriş olayları alamayan, yüksek SU kullanım yüzdesi karmaşık sorgu mantığı ile bir iş olabilir. Bu, sonra giriş ve çıkış olayları ani bir artış oluşabilir. İş sorgusu karmaşıksa, bellekte durumunu korumak üzere devam edebilir.

SU kullanım yüzdesi aniden beklenen düzeylerine geri dönmeyi önce kısa bir süre için 0 olarak düşebilir. Bu, geçici hatalar veya sistem tarafından başlatılan yükseltme işlemleri nedeniyle gerçekleşir. Bir işi SU kullanım yüzdesi azaltabilir, bu sorgunuzu değilse değil için akış birimi sayısını artırabilir [tam olarak paralel](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization).

## <a name="stateful-query-logicin-temporal-elements"></a>Durum bilgisi olan sorgu mantığının zamana bağlı öğeleri
Azure Stream Analytics işi benzersiz yeteneğinin, pencereli toplamlar, zamana bağlı birleştirmeler ve geçici analiz işlevleri gibi durum bilgisi olan işlem gerçekleştirmektir. Bu işleçlerden her biri, durum bilgilerini tutar. Bu sorgu öğeleri için maksimum pencere boyutunu yedi gündür. 

Geçici pencere kavramı çeşitli Stream Analytics sorgu öğelerin görünür:
1. Pencereli toplamlar: Grup tarafından atlaması ve windows kayan atlayan,

2. Zamana bağlı birleşimler: DATEDIFF işlevi ile birleştirme

3. Zamana bağlı analitik İşlevler: ISFİRST, LAST ve GECİKME süresi sınırı

Kullanılan bellek aşağıdaki faktörleri etkilemek (akış birimi ölçüm parçası) Stream Analytics işleri tarafından:

## <a name="windowed-aggregates"></a>Pencereli toplamlar
Kullanılan bellek (durum boyutu) bir pencereli toplama için her zaman penceresi boyutuna orantılı değil. Bunun yerine, kullanılan bellek veri ya da grubu her zaman penceresinde kardinalite orantılıdır.


Örneğin, sayı ile ilişkili aşağıdaki sorguda `clusterid` nicelik sorgu. 

   ```sql
   SELECT count(*)
   FROM input 
   GROUP BY  clusterid, tumblingwindow (minutes, 5)
   ```

Önceki sorguda yüksek kardinalite kaynaklanan sorunları düzeltmek için olayları olay hub'ına tarafından bölümlenmiş gönderebilirsiniz `clusterid`ve her giriş bölümünü üzerinden ayrı olarak işlemek sistem sağlayarak sorgu genişletme **bölüm TARAFINDAN** aşağıdaki örnekte gösterildiği gibi:

   ```sql
   SELECT count(*) 
   FROM input PARTITION BY PartitionId
   GROUP BY PartitionId, clusterid, tumblingwindow (minutes, 5)
   ```

Sorgu bölümlere ayrıldıktan sonra birden çok düğüme dağıtılır. Sonuç olarak, sayısını `clusterid` her düğüme gelen değerleri azaltıldı böylece grubunun kardinalite azaltma işleciyle. 

Olay hub'ı bölümleri, bir azaltma adımı ihtiyacını önlemek için gruplandırma anahtara göre bölümlendirilmelidir. Daha fazla bilgi için [Event Hubs'a genel bakış](../event-hubs/event-hubs-what-is-event-hubs.md). 

## <a name="temporal-joins"></a>Zamana bağlı birleştirmeler
Kullanılan bellek (durum boyutu) zamana bağlı bir birleşimin giriş olayı oranı hareket alanına odası boyutu tarafından birden çok kez birleştirme işleminin zamana bağlı hareket alanına odada bir olay sayısı orantılıdır. Diğer bir deyişle, DATEDIFF zaman aralığına göre ortalama olay oranı çarpılan katılımlar tarafından kullanılan bellek orantılıdır.

Birleştirme için eşleşmeyen olay sayısı, sorgu için bellek kullanımını etkiler. Aşağıdaki sorgu, tıklama oluşturan reklam görüntüleme sayısını bulmayı amaçlamaktadır:

   ```sql
   SELECT clicks.id
   FROM clicks 
   INNER JOIN impressions ON impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10.
   ```

Bu örnekte, birçok gösterilir ve birkaç insanın bunlara tıklaması ve tüm olayların zaman penceresinde tutmak için gerekli olan mümkündür. Tüketilen bellek miktarı pencere boyutu ve olay hızıyla doğru orantılıdır. 

Bu sorunu düzeltmek için olayları olay sistem vererek join anahtarlar (Bu durumda kimliği) ve sorgu genişletme bölümlenmiş kullanılarak ayrı giriş her bölümün Hub'ına gönderme **PARTITION BY** gösterildiği gibi:

   ```sql
   SELECT clicks.id
   FROM clicks PARTITION BY PartitionId
   INNER JOIN impressions PARTITION BY PartitionId 
   ON impression.PartitionId = clicks.PartitionId AND impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10 
   ```

Sorgu bölümlere ayrıldıktan sonra birden çok düğüme dağıtılır. Sonuç olarak, her düğüme gelen olay sayısı azalmasına yol açar birleştirme penceresinde durum boyutu. 

## <a name="temporal-analytic-functions"></a>Zamana bağlı bir analiz işlevleri
Kullanılan bellek (durum boyutu) bir zamana bağlı analitik işlevinin birden çok kez süresine olay hızıyla doğru orantılıdır. Analitik işlevler tarafından tüketilen bellek miktarı pencere boyutu için doğru orantılı değil, ancak bunun yerine her zaman penceresi sayısına bölümü.

Zamana bağlı katılmak için düzeltme benzerdir. Sorgu kullanarak ölçeği genişletin **PARTITION BY**. 

## <a name="out-of-order-buffer"></a>Sıralama dışına çıkan arabelleği 
Kullanıcı yapılandırma bölmesi sıralama olayda sıralamaya arabellek boyutu yapılandırabilirsiniz. Arabellek Girişleri penceresinin süresi boyunca tutun ve yeniden sıralamak için kullanılır. Arabellek boyutu birden çok kez sıralamaya pencere boyutuna göre olay giriş oranı orantılıdır. Varsayılan pencere boyutu 0'dır. 

Sıralama dışına çıkan arabellek taşması sorunu düzeltmek için ölçeklendirme sorgu kullanarak **PARTITION BY**. Sorgu bölümlere ayrıldıktan sonra birden çok düğüme dağıtılır. Sonuç olarak, her düğüme gelen olay sayısını azalmasına yol açar, her yeniden sıralama arabelleği içinde olay sayısı. 

## <a name="input-partition-count"></a>Giriş bölüm sayısı 
Bir işin giriş giriş her bölüm, bir arabellek sahiptir. İş giriş bölüm sayısı daha büyük, daha fazla kaynak tüketir. Her akış birimi yaklaşık 1 MB/sn giriş, Azure Stream Analytics işleyebilir. Bu nedenle, Stream Analytics, akış birimleri, olay Hub'ındaki bölüm sayısı ile sayısını eşleştirerek iyileştirebilirsiniz. 

Genellikle, bir akış birimi ile yapılandırılmış bir işi (olay hub'ı için en düşük olan) bir Event Hub ile iki bölüm için yeterli olur. Olay hub'ı daha fazla bölüm varsa, Stream Analytics işinizi daha fazla kaynak tüketir, ancak mutlaka olay hub'ı tarafından sağlanan ek aktarım hızı kullanır. 

Akış birimleri 6 ile bir proje için olay hub'ı 4 veya 8 bölümlerden gerekebilir. Ancak, aşırı kaynak kullanımının neden olduğundan çok fazla gereksiz bölümler kaçının. Örneğin, bir olay hub'ı 16 bölümleri veya 1 akış birimi olan bir Stream Analytics işinde daha büyük. 

## <a name="reference-data"></a>Başvuru verileri 
Başvuru verilerinde ASA için hızlı arama belleğe yüklenir. Birden çok kez aynı başvuru verileriyle birleştirme bile bellekte geçerli bir uygulama ile her başvuru verileriyle birleştirme işlemi başvuru verilerinin bir kopyasını tutar. Sorgular için **PARTITION BY**, her bölüm, bölümlerin tam olarak ayrılmış şekilde, başvuru verilerini bir kopyasına sahip olur. Çarpan etkisi ile bellek kullanımı hızla birden çok kez ile birden çok bölüme başvuru verileriyle birleştirme, çok yüksek alabilirsiniz.  

### <a name="use-of-udf-functions"></a>UDF işlevleri kullanımı
UDF işlevi eklediğinizde, Azure Stream Analytics JavaScript çalışma zamanı belleğine yükler. Bu, SU % etkiler.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'te paralelleştirilebilir sorguları oluşturma](stream-analytics-parallelization.md)
* [Verimliliği artırmak için Azure Stream Analytics işlerini ölçeklendirme](stream-analytics-scale-jobs.md)

<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor-NewPortal.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings-NewPortal.png   
