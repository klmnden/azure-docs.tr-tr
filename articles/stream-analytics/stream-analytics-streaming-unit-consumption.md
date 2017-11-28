---
title: "Azure Stream Analytics: Anlamak ve akış birimleri ayarlama | Microsoft Docs"
description: "Hangi faktörlerin Azure akış analizi performans etkisi anlayın."
keywords: "Birim, sorgu performansı akış"
services: stream-analytics
documentationcenter: 
author: JSeb225
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeanb
ms.openlocfilehash: e1fb9ee3147f94b173b0fd324943b8801b984d2b
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="understand-and-adjust-streaming-units"></a>Anlama ve akış birimi Ayarla

Azure Stream Analytics akış birimleri (SUs) bir işi çalıştırma "weight" performans toplar. SUs bir işlemi yürütmek için kullanılan bilgi işlem kaynaklarını temsil eder. Bu birimler CPU, bellek, okuma ve yazma hızlarının harmanlanmış bir ölçümünü temel alarak göreceli olay işleme kapasitesini açıklamak için bir yol sağlar. Başarım düşünceleri katmanı, işiniz için el ile bellek ve CPU core-sayısı yaklaşık depolama bilmenize gerek sizden zamanında işinizi çalıştırmak için gereken kaldırır ve sorgu mantığı odaklanmak bu kapasite sağlar.

İşleme akış düşük gecikme süresine ulaşmanız için Azure akış analizi işi tüm işleme bellekte uygulayın. Bellek yetersiz çalıştırırken, iş akışında başarısız olur. Sonuç olarak, bir üretim işi için bir akış projenin kaynak kullanımını izlemek ve 7/24 çalışan işleri tutmak için ayrılmış yeterli kaynak bulunduğundan emin olun önemlidir.

% %0 100 arasında bir yüzde sayı ölçümüdür. Bir iş akışında için en az ayak izini ile SU % kullanımı genellikle %10 20 arasında ölçümüdür. % 80'için zaman zaman ani hesap için aşağıdaki ölçüm tutmak en iyisidir.  Ölçüm üzerinde bir uyarı ayarlayabilirsiniz (bkz [ölçüm uyarıları ayarlamak için burayı](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/insights-alerts-portal)).



## <a name="configure-stream-analytics-streaming-units-sus"></a>Akış birimleri (SUs) akış analizi yapılandırın
1. Oturum [Azure portalı](http://portal.azure.com/)
2. Kaynak listesinde, ölçeklendirme ve daha sonra açmak istediğiniz akış analizi işi bulun. 
3. İş sayfasında, yapılandırma, tıklayın **ölçek**. 

    ![Azure portal Stream Analytics işi yapılandırma][img.stream.analytics.preview.portal.settings.scale]
    
4. İş için SUs ayarlamak için kaydırıcıyı kullanın. Belirli SU ayarlar sınırlı olduğuna dikkat edin. 


## <a name="monitor-job-performance"></a>İş performansını izleme
Azure Portalı'nı kullanarak, bir iş üretimini izleyebilirsiniz:

![Azure Stream Analytics işleri izleme][img.stream.analytics.monitor.job]

İş yükünün beklenen verimliliği hesaplayın. Üretilen iş beklenenden daha az ise, giriş bölümü ayarlamak, sorgu ayarlamak ve işinize SUs ekleyin.


## <a name="how-many-sus-are-required-for-a-job"></a>Kaç tane SUs bir iş için gerekli?

Belirli bir proje için gerekli SUs sayısını seçmeye girişleri ve proje içinde tanımlanmış sorgu için bölüm yapılandırmasını bağlıdır. **Ölçek** sayfası SUs doğru sayıda ayarlamanıza olanak sağlar. Gerekli olandan daha fazla SUs ayırmak için en iyi bir uygulamadır. Stream Analytics işleme altyapısı gecikme süresi ve ek bellek ayırma, üretilen iş için en iyi duruma getirir.

Genel olarak, en iyi uygulama olarak kullanmayın sorgular için 6 SUs ile başlatmaktır **bölüm tarafından**. Ardından Lezzetli nokta temsili miktarda veriyi geçirmek ve SU % kullanım ölçüm inceleyin sonra SUs sayısı değişiklik bir deneme yanılma yöntemi kullanarak belirler.

SUs doğru sayıda seçme hakkında daha fazla bilgi için bu sayfaya bakın: [verimliliğini artırmak için ölçek Azure akış analizi işleri](stream-analytics-scale-jobs.md)

> [!Note]
> Kaç tane SUs seçme girişleri için bölüm yapılandırmasını ve iş için tanımlanan sorgusu belirli bir işin bağlıdır için gereklidir. SUs kota bir iş için en fazla seçebilirsiniz. Varsayılan olarak, belirli bir bölgede en fazla 200 SUs tüm analytics işleri kotası her Azure aboneliği sahiptir. SUs ötesinde Bu kota, abonelikler için artırmak için başvurun [Microsoft Support](http://support.microsoft.com). İş başına SUs için geçerli değerler: 1, 3, 6 ve 6'ın artışlarla yukarı.



## <a name="factors-increasing-su-utilization"></a>SU % kullanımı artırmayı Etkenler 
### <a name="stateful-query-logic"></a>Durum bilgisi sorgusu mantığı 
Azure Stream Analytics işi benzersiz yeteneğini pencerelenmiş toplamlar, zamana bağlı birleştirmeler ve zamana bağlı analitik işlevler gibi durum bilgisi olan işlem gerçekleştirmek için biridir. Bu işleçlere her bazı durumlar tutar. 
#### <a name="windowed-aggregates"></a>Pencerelenmiş toplamlar
Bir pencereli toplama durumu boyutunu grubu (kardinalite) işleci grubundaki orantılıdır. Örneğin, aşağıdaki sorguda clusterid ile ilişkili sorgu önemi sayıdır. 

    SELECT count(*)
    FROM input 
    GROUP BY  clusterid, tumblingwindow (minutes, 5)


Önceki sorgu yüksek kardinalite nedeniyle oluşan sorunları ameliorate için olayları olay sistem vererek '' clusterid'' ve genişletme sorgu tarafından bölümlenmiş ayrı olarak kullanarak her bir giriş bölümü işlemeye Hub'ına gönderebilirsiniz **bölüm tarafından**  aşağıdaki örnekte gösterildiği gibi:


    SELECT count(*) 
    FROM PARTITION BY PartitionId
    GROUP BY PartitionId, clusterid, tumblingwindow (minutes, 5)

Sorgu bölümlere ayrıldıktan sonra birden çok düğüme dağıtılır. Sonuç olarak, her bir düğümüne gelen clusterid sayısı saklayarak grubunun kardinalite azaltması işleciyle azalır. 

Olay hub'ı bölümleri azalan adımla gereksinimini ortadan kaldırmak için gruplandırma anahtarı tarafından bölümlenmiş. Ek ayrıntıları ele alınmıştır [burada](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-overview). 
#### <a name="temporal-join"></a>Zamana bağlı birleştirme
Zamana bağlı bir birleşim durumu boyutunu Çarp hareket alanına yer boyutuna göre olay giriş oranıdır katılma zamana bağlı hareket alanına odada olayların sayısının orantılıdır. 

Birleştirme eşleşmeyen olayların sayısının sorgu için bellek kullanımını etkiler. Aşağıdaki sorgu, tıklama oluşturan reklam görüntüleme sayısını bulmayı amaçlamaktadır:

    SELECT id
    FROM clicks 
    INNER JOIN, impressions ON impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10.

Bu örnekte, reklam çok sayıda gösterilir ve birkaç kişi tıklayın ve zaman penceresi için tüm olayları tutmak için gerekli olan mümkündür. Tüketilen bellek miktarı pencere boyutu ve olay hızıyla doğru orantılıdır. 

Bunu düzeltmek için olay sistem vererek birleştirme anahtarları (Bu durumda kimliği) ve genişletme sorgu tarafından bölümlenmiş ayrı olarak kullanarak her bir giriş bölümü işlemeye Hub'ına olayları göndermek **bölüm tarafından** gösterildiği gibi:

    SELECT id
    FROM clicks PARTITION BY PartitionId
    INNER JOIN impressions PARTITION BY PartitionId 
    ON impression.PartitionId = clocks.PartitionId AND impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10 
</code>

Sorgu bölümlere ayrıldıktan sonra birden çok düğüme dağıtılır. Sonuç olarak böylece birleştirme penceresinde tutulan durumu boyutunu azaltma her düğümüne gelen olayların sayısı azalır. 
#### <a name="temporal-analytic-function"></a>Zamana bağlı analitik işlevi
Zamana bağlı bir analitik işlev durumu boyutunu Çarp süreye göre olay hızı orantılıdır. Düzeltme zamana bağlı birleşime benzer. Sorgu kullanarak ölçeklendirebilirsiniz **bölüm tarafından**. 

### <a name="out-of-order-buffer"></a>Bozuk arabellek 
Kullanıcı, yapılandırma bölmesi sıralama durumunda bozuk arabellek boyutu yapılandırabilirsiniz. Arabellek Girişleri penceresinin süresi için basılı tutun ve onları yeniden sıralamak için kullanılır. Arabellek boyutu bozuk pencere boyutunu tarafından Çarp olay giriş hızı orantılıdır. Varsayılan pencere boyutu 0'dır. 

Bunu düzeltmek için ölçeklendirme sorgu kullanarak **bölüm tarafından**. Sorgu bölümlere ayrıldıktan sonra birden çok düğüme dağıtılır. Sonuç, böylece her sipariş arabellek olayların sayısını azaltmayı her düğümüne gelen olayların sayısı azalır. 

### <a name="input-partition-count"></a>Giriş bölüm sayısı 
Giriş bir işin giriş her bölüm bir arabellek sahiptir. Giriş bölümleri fazla sayıda, daha fazla kaynak iş tüketir. Olay hub'ınızın ASA SU numarası bölüm sayısı ile eşleşecek şekilde isteyebilirsiniz her SU için yaklaşık 1 MB/sn giriş, Azure akış analizi işleyebilir. Genellikle, 1SU işi (olay hub'ına yönelik en düşük olan) bir Event Hub ile 2 bölümler için yeterli olur. Olay hub'ı daha fazla bölüm varsa, ASA işinizi daha fazla kaynağı kullanır, ancak mutlaka olay Hub tarafından sağlanan ek işleme kullanır. 6SU işi için olay hub'ı 4 veya 8 bölümlerden gerekebilir. 16 bölümleri olan veya daha büyük bir 1SU içinde bir olay hub'ı kullanarak iş genellikle aşırı kaynak kullanımına katkıda bulunur ve kaçınılmalıdır. 

### <a name="reference-data"></a>Başvuru verileri 
ASA başvuru verileri hızlı arama için belleğe yüklenir. Birden çok kez aynı başvurusu verilerle katılma olsa bile geçerli bir uygulama ile her birleştirme işlemi başvuru verileri ile bellekte başvuru verilerin bir kopyasını tutar. Sorguları için **bölüm tarafından**, bölümlerin tam olarak ayrılmış şekilde her bölüm başvuru verileri bir kopyası bulunur. Çarpan etkisi ile bellek kullanımı hızlı başvuru verileri birden çok kez ile birden çok bölüm ile katılırsanız çok yüksek alabilirsiniz.  

#### <a name="use-of-udf-functions"></a>UDF işlevleri kullanımı
UDF işlev eklediğinizde, Azure akış analizi belleğe JavaScript çalışma zamanı yükler. Bu, SU % etkiler.


## <a name="get-help"></a>Yardım alın
Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics paralelleştirilebilir sorgular oluşturma](stream-analytics-parallelization.md)
* [Verimliliğini artırmak için Azure Stream Analytics işlerini ölçeklendirme](stream-analytics-scale-jobs.md)

<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor-NewPortal.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings-NewPortal.png   
