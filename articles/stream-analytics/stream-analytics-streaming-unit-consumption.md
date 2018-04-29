---
title: Anlamak ve Azure Stream Analytics akış birimlerinde ayarlayın
description: Bu makalede, akış birimi ayarı ve Azure akış analizi performansını etkileyen diğer faktörler açıklanmaktadır.
services: stream-analytics
author: JSeb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/12/2018
ms.openlocfilehash: c96e9825cddd0b60e67cd4752fab8f440ceaae76
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="understand-and-adjust-streaming-units"></a>Anlama ve akış birimi Ayarla

Azure Stream Analytics akış birimleri (SUs) bir işi çalıştırma "weight" performans toplar. SUs bir işlemi yürütmek için kullanılan bilgi işlem kaynaklarını temsil eder. Bu birimler CPU, bellek, okuma ve yazma hızlarının harmanlanmış bir ölçümünü temel alarak göreceli olay işleme kapasitesini açıklamak için bir yol sağlar. Bu sorgu mantığına odaklanan kapasite sağlar ve akış analizi çalıştırmak için donanım yönetmek için gereken iş zamanında özetleri.

İşleme akış düşük gecikme süresine ulaşmanız için Azure akış analizi işi tüm işleme bellekte uygulayın. Bellek yetersiz çalıştırırken, iş akışında başarısız olur. Sonuç olarak, bir üretim işi için bir akış projenin kaynak kullanımını izlemek ve 7/24 çalışan işleri tutmak için ayrılmış yeterli kaynak bulunduğundan emin olun önemlidir.

% %0 100 arasında bir yüzde sayı ölçümüdür. Bir iş akışında için en az ayak izini ile SU % kullanımı genellikle %10 20 arasında ölçümüdür. % 80'için zaman zaman ani hesap için aşağıdaki ölçüm tutmak en iyisidir. Microsoft, bir uyarı Kaynak Tükenmesi önlemek için % 80 SU kullanım ölçüm ayarı önerir. Daha fazla bilgi için bkz: [Öğreticisi: Azure akış analizi işleri için uyarıları ayarlama](stream-analytics-set-up-alerts.md).

## <a name="configure-stream-analytics-streaming-units-sus"></a>Akış birimleri (SUs) akış analizi yapılandırın
1. Oturum [Azure portalı](http://portal.azure.com/)

2. Kaynak listesinde, ölçeklendirme ve daha sonra açmak istediğiniz akış analizi işi bulun. 

3. İş sayfasının altında **yapılandırma** başlığını seçin **ölçek**. 

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

## <a name="factors-that-increase-su-utilization"></a>SU % kullanımını artırabilir Etkenler 

Zamana bağlı (zaman yönelimli) sorgusu, akış analizi tarafından sağlanan durum bilgisi olan işleçleri çekirdek kümesini öğelerdir. Akış analizi, bellek tüketimi, denetim noktası oluşturma dayanıklılık ve durum kurtarma hizmeti yükseltmeleri sırasında yöneterek bu işlemleri dahili olarak kullanıcı adına durumunu yönetir. Stream Analytics tam olarak durumları yönetir olsa da, kullanıcıların dikkate almanız gereken en iyi yöntem önerileri dizi vardır.

## <a name="stateful-query-logic-in-temporal-elements"></a>Zamana bağlı öğelerindeki durum bilgisi sorgusu mantığı
Azure Stream Analytics işi benzersiz yeteneğini pencerelenmiş toplamlar, zamana bağlı birleştirmeler ve zamana bağlı analitik işlevler gibi durum bilgisi olan işlem gerçekleştirmek için biridir. Bu işleçlere her durum bilgilerini tutar. Bu sorgu öğeler için en fazla pencere boyutu yedi gündür. 

Geçici pencere kavramı birkaç Stream Analytics sorgu öğeleri görünür:
1. Pencerelenmiş toplamlar: grubu tarafından atlamalı ve windows kayan dönen,

2. Zamana bağlı birleştirmeler: DATEDIFF işlevi ile birleştirme

3. Zamana bağlı analitik İşlevler: ISFİRST, son ve sınırı süreli GECİKME

Kullanılan bellek aşağıdaki etmenlere etkilemek (parçası birimleri ölçüm akış) akış analizi işleri tarafından:

## <a name="windowed-aggregates"></a>Pencerelenmiş toplamlar
Kullanılan bellek (durum boyutu) bir pencereli toplama için her zaman pencere boyutunu orantılı değil. Bunun yerine, kullanılan bellek veri ya da her zaman penceresinde gruplarının sayısını önemi orantılıdır.


Örneğin, aşağıdaki sorguda numarası ilişkili `clusterid` sorgu nicelik. 

   ```sql
   SELECT count(*)
   FROM input 
   GROUP BY  clusterid, tumblingwindow (minutes, 5)
   ```

Önceki sorgu yüksek kardinalite nedeniyle oluşan sorunları ameliorate için olaylar tarafından bölümlenmiş olay Hub'ına gönderebilirsiniz `clusterid`ve ayrı olarak kullanarak her bir giriş bölümü işlemeye sistem sağlayarak sorgu genişletme **bölümü TARAFINDAN** aşağıdaki örnekte gösterildiği gibi:

   ```sql
   SELECT count(*) 
   FROM PARTITION BY PartitionId
   GROUP BY PartitionId, clusterid, tumblingwindow (minutes, 5)
   ```

Sorgu bölümlere ayrıldıktan sonra birden çok düğüme dağıtılır. Sonuç olarak, sayısı `clusterid` her düğümüne gelen değerleri azaltılır saklayarak grubunun kardinalite azaltması işleciyle. 

Olay hub'ı bölümleri azalan adımla gereksinimini ortadan kaldırmak için gruplandırma anahtarı tarafından bölümlenmiş. Daha fazla bilgi için bkz: [Event Hubs'a genel bakış](../event-hubs/event-hubs-what-is-event-hubs.md). 

## <a name="temporal-joins"></a>Zamana bağlı birleşimler
Kullanılan bellek (durum boyutu) zamana bağlı bir birleştirme Çarp hareket alanına yer boyutuna göre olay giriş oranıdır katılma zamana bağlı hareket alanına odada olayların sayısının orantılıdır. Diğer bir deyişle, ortalama olay hızı tarafından çarpılan DateDiff zaman aralığı için katılımlar tarafından tüketilen bellek orantılıdır.

Birleştirme eşleşmeyen olayların sayısının sorgu için bellek kullanımını etkiler. Aşağıdaki sorgu, tıklama oluşturan reklam görüntüleme sayısını bulmayı amaçlamaktadır:

   ```sql
   SELECT clicks.id
   FROM clicks 
   INNER JOIN impressions ON impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10.
   ```

Bu örnekte, reklam çok sayıda gösterilir ve birkaç kişi tıklayın ve zaman penceresi için tüm olayları tutmak için gerekli olan mümkündür. Tüketilen bellek miktarı pencere boyutu ve olay hızıyla doğru orantılıdır. 

Bunu düzeltmek için olay sistem vererek birleştirme anahtarları (Bu durumda kimliği) ve genişletme sorgu tarafından bölümlenmiş ayrı olarak kullanarak her bir giriş bölümü işlemeye Hub'ına olayları göndermek **bölüm tarafından** gösterildiği gibi:

   ```sql
   SELECT clicks.id
   FROM clicks PARTITION BY PartitionId
   INNER JOIN impressions PARTITION BY PartitionId 
   ON impression.PartitionId = clicks.PartitionId AND impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10 
   ```

Sorgu bölümlere ayrıldıktan sonra birden çok düğüme dağıtılır. Sonuç olarak böylece birleştirme penceresinde tutulan durumu boyutunu azaltma her düğümüne gelen olayların sayısı azalır. 

## <a name="temporal-analytic-functions"></a>Zamana bağlı analitik İşlevler
Kullanılan bellek (durum boyutu) bir zamana bağlı analitik işlev Çarp süreye göre olay hızı orantılıdır. Analitik işlevler tarafından tüketilen bellek pencere boyutunu orantılı değil, ancak bunun yerine her zaman penceresi sayıma bölüm.

Düzeltme zamana bağlı birleşime benzer. Sorgu kullanarak ölçeklendirebilirsiniz **bölüm tarafından**. 

## <a name="out-of-order-buffer"></a>Bozuk arabellek 
Kullanıcı, yapılandırma bölmesi sıralama durumunda bozuk arabellek boyutu yapılandırabilirsiniz. Arabellek Girişleri penceresinin süresi için basılı tutun ve onları yeniden sıralamak için kullanılır. Arabellek boyutu bozuk pencere boyutunu tarafından Çarp olay giriş hızı orantılıdır. Varsayılan pencere boyutu 0'dır. 

Bozuk arabellek taşması düzeltmek için ölçeklendirme sorgu kullanarak **bölüm tarafından**. Sorgu bölümlere ayrıldıktan sonra birden çok düğüme dağıtılır. Sonuç olarak, böylece her sipariş arabellek olayların sayısını azaltmayı her bir düğümüne gelen olayların sayısı azalır. 

## <a name="input-partition-count"></a>Giriş bölüm sayısı 
Giriş bir işin giriş her bölüm bir arabellek sahiptir. Giriş bölümleri fazla sayıda, daha fazla kaynak iş tüketir. Her akış birimi için Azure Stream Analytics yaklaşık 1 MB/sn giriş işleyebilir. Bu nedenle, Stream Analytics akış birimleri, Event Hub'ındaki bölüm sayısı ile sayısı eşleştirerek iyileştirebilirsiniz. 

Genellikle, bir akış birimi ile yapılandırılmış bir işi (olay hub'ına yönelik en düşük olan) bir Event Hub ile iki bölüm için yeterli olur. Olay hub'ı daha fazla bölüm varsa, Stream Analytics işiniz daha fazla kaynağı kullanır, ancak mutlaka olay Hub tarafından sağlanan ek işleme kullanır. 

Akış birimleri 6 ile bir iş için olay hub'ı 4 veya 8 bölümlerden gerekebilir. Ancak, aşırı kaynak kullanımının neden bu yana çok fazla gereksiz bölümleri kaçının. Örneğin, bir Event Hub 16 bölümleri olan ya da 1 akış birimi olan bir akış analizi işi daha büyük. 

## <a name="reference-data"></a>Başvuru verileri 
ASA başvuru verileri hızlı arama için belleğe yüklenir. Birden çok kez aynı başvurusu verilerle katılma olsa bile geçerli bir uygulama ile her birleştirme işlemi başvuru verileri ile bellekte başvuru verilerin bir kopyasını tutar. Sorguları için **bölüm tarafından**, bölümlerin tam olarak ayrılmış şekilde her bölüm başvuru verileri bir kopyası bulunur. Çarpan etkisi ile bellek kullanımı hızlı başvuru verileri birden çok kez ile birden çok bölüm ile katılırsanız çok yüksek alabilirsiniz.  

### <a name="use-of-udf-functions"></a>UDF işlevleri kullanımı
UDF işlev eklediğinizde, Azure akış analizi belleğe JavaScript çalışma zamanı yükler. Bu, SU % etkiler.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics paralelleştirilebilir sorgular oluşturma](stream-analytics-parallelization.md)
* [Verimliliğini artırmak için Azure Stream Analytics işlerini ölçeklendirme](stream-analytics-scale-jobs.md)

<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor-NewPortal.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings-NewPortal.png   
