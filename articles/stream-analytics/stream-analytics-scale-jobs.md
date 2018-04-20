---
title: Azure Stream Analytics işlerine yukarı ve aşağı doğru ölçeklendirme
description: Bu makalede, akış analizi işi giriş veri bölümlendirme, sorgu ayarlama ve iş akış birimleri ayarlama ölçeklendirme açıklar.
services: stream-analytics
author: JSeb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/22/2017
ms.openlocfilehash: 1438ffa34652268572fe89dc63583cc25607d722
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="scale-an-azure-stream-analytics-job-to-increase-throughput"></a>Bir Azure akış analizi işi verimliliğini artırmak için ölçeklendirme
Bu makalede akış analizi işleri verimliliğini artırmak için bir akış analizi sorgu ince ayar gösterilmektedir. İşinizi daha yüksek yükü işlemek ve daha fazla sistem kaynakları (örneğin, daha fazla bant genişliği, daha fazla CPU kaynaklarını, daha fazla bellek) yararlanmak için ölçeklendirmek için aşağıdaki kılavuzu kullanın.
Önkoşul olarak, aşağıdaki makaleler okuma gerekebilir:
-   [Akış Birimlerini anlama ve ayarlama](stream-analytics-streaming-unit-consumption.md)
-   [Paralelleştirilebilir işleri oluşturma](stream-analytics-parallelization.md)

## <a name="case-1--your-query-is-inherently-fully-parallelizable-across-input-partitions"></a>1 – sorgunuzu giriş bölümler kendiliğinden tam paralelleştirilebilir durumdur
Sorgunuz giriş bölümler kendiliğinden tam paralelleştirilebilir ise, aşağıdaki adımları izleyin:
1.  Kullanarak utandırıcı derecede paralel olacak şekilde sorgunuzu Yazar **bölüm tarafından** anahtar sözcüğü. Utandırıcı derecede paralel işi bölümünde daha fazla ayrıntı görmek [bu sayfadaki](stream-analytics-parallelization.md).
2.  Sorgunuzda kullanılan çıkış türlerine bağlı olarak bazı ya da olmayabilir paralelleştirilebilir, çıktı veya daha fazla utandırıcı derecede paralel olarak yapılandırması gerekir. Örneğin, SQL, SQL DW ve Powerbı çıkışları paralelleştirilebilir değildir. Çıkış için çıkış havuzunun göndermeden önce her zaman birleştirilir. BLOB'lar, tablolar, ADLS, hizmet veri yolu, paralel birkaç ve Azure işlevi otomatik olarak ölçeklendirin. CosmosDB ve olay hub'ı PartitionKey yapılandırması ile eşleşecek şekilde ayarlanmış olması gerekiyor **bölüm tarafından** alan (genellikle PartitionID). Olay hub'ı için aynı zamanda tüm girişleri ve bölümler arasında çapraz üzerinden önlemek için tüm çıkışları için bölümlerin sayısı eşleştirmek için fazladan dikkat edin. 
3.  Sorgunuzu ile çalıştırmak **6 SU** (tek bir bilgi işlem düğümü tam kapasitesini olmayan) maksimum ulaşılabilir verim ölçmek için ve kullanıyorsanız **GROUP BY**, kaç tane grupları (kardinalite) iş yapabilirsiniz ölçmek işler. Sistem kaynak sınırlarını basarsa iş genel belirtileri aşağıda verilmiştir.
    - SU % kullanımı üzerinde % 80 ölçümüdür. Bu bellek kullanımı yüksek olduğunu gösterir. Bu ölçüm artırmak için katkıda bulunan Etkenler açıklanan [burada](stream-analytics-streaming-unit-consumption.md). 
    -   Çıkış zaman damgası duvar saati süresi göre dönmeden. Sorgu mantığınızı bağlı olarak, çıktı zaman damgası duvar saati süresi mantığı uzaklığı olabilir. Ancak, kabaca aynı hızında ilerleme. Çıkış zaman damgası başka ve bunun arkasında dönmeden, sistem overworking bir gösterge olur. Aşağı Akış çıkışı havuz azaltma veya yüksek CPU kullanımı sonucu olabilir. İki ayırt zor olabilir CPU kullanımı ölçümü şu anda sağlamaz.
        - Sorun havuz azaltma nedeniyle ise, çıktı bölümleri sayısını artırmak (ve ayrıca iş tam olarak paralelleştirilebilir tutmak için bölüm girişi), gerekebilir veya (örneğin CosmosDB için birim istek sayısı) havuz kaynaklarının miktarını artırın.
    - İş şemada yoktur bir bölüm biriktirme listesi olay ölçüm her giriş için başına. Biriktirme listesi olay ölçüm artmaya devam ederse, bu da sistem kaynağı (ya da havuz çıkış azaltma veya yüksek CPU nedeniyle) kısıtlı bir göstergesidir.
4.  6 SU iş ulaşıp ulaşamadığını sınırları belirledikten sonra belirli bölüm "etkin" yapar eğme herhangi bir veri yok varsayılarak daha fazla SUs ekledikçe, doğrusal olarak iş işlem kapasitesini tahmin
>[!Note]
> Akış birimleri sağ sayısını seçin: her 6 SU eklenen için Stream Analytics işleme düğümü oluşturduğundan, bölümleri düğümler arasında eşit olarak dağıtılabilir giriş bölüm sayısı bölenini düğüm sayısı olmak en iyisidir.
> Örneğin, 6 ölçülen SU işini 4 MB elde edebileceğiniz/s işleme hızı ve giriş bölümü sayınız 4. İşinizi yaklaşık 8 MB/sn işleme hızını elde etmek için 12 SU veya 16 MB/sn elde etmek için 24 SU ile çalıştırmayı seçebilirsiniz. Ardından zaman hangi değere iş işlevi, giriş oranının olarak SU sayıyı artırmaya karar verebilirsiniz.


## <a name="case-2---if-your-query-is-not-embarrassingly-parallel"></a>Durum sorgunuzu utandırıcı derecede paralel değilse 2 -.
Sorgunuz utandırıcı derecede paralel değilse, aşağıdaki adımları izleyebilirsiniz.
1.  Başlangıç içermeyen bir sorgu ile **bölüm tarafından** ilk karmaşıklık bölümlenmesini önlemeye ve sorgunuzu olarak en fazla yük ölçmek için 6 SU ile çalıştırmak için [durum 1](#case-1--your-query-is-inherently-fully-parallelizable-across-input-partitions).
2.  Üretilen iş vadede beklenen yük elde edebilirsiniz, yapılır. Alternatif olarak, SU ve 1 SU senaryonuz için çalışır SU en az sayıda öğrenmek için 3 çalıştıran aynı iş ölçmek seçebilirsiniz.
3.  İstenen verimlilik elde etmek, onu olmayan birden çok adımı zaten varsa ve sorgudaki her adımı için 6 SU tahsis sorgunuzu Mümkünse, içine birden çok adımı bölüneceği deneyin. Örneğin "Ölçeklendirme" seçeneğinde 18 SU tahsis 3 adımları varsa.
4.  Böyle bir iş çalışırken, Stream Analytics her adım ayrılmış 6 SU kaynaklarla kendi düğümde koyar. 
5.  Yük hedef hala elde yapmadıysanız, kullanmayı deneyebilir **bölüm tarafından** yakın adımları girdisi başlatılıyor. İçin **GROUP BY** doğal olarak bölümlenebilir olmayabilir işleci, yerel/global toplama düzeni bir bölümlenmiş gerçekleştirmek için kullanabileceğiniz **GROUP BY** bir bölümlenmemiş tarafından izlenen **GROUP BY** . Saymak istiyorsanız, örneğin, her Ücretli Stand 3 dakikada bir ve veri hacmi giderek kaç araba ötesindedir olan 6 SU tarafından işlenebilir.

Sorgu:

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Yukarıdaki sorguda, bölüm başına Ücretli Stand başına araba sayım ve sayı tüm bölümler birlikte ekleme.

Bölümlenmiş, adım, her bir bölüm için bir kez 6 SU, her bölüm 6 sahip tahsis SU her bölüm kendi işlem düğümünde yerleştirilebilecek maksimum olduğundan.

> [!Note]
> Sorgunuz bölümlenmiş, ek SU çok adımları sorguda ekleme her zaman üretilen işi artırabilir değil. Performans elde etmek için bir yerel/global toplama düzeni kullanan ilk adımları birimde yukarıda 5. adımda açıklandığı gibi azaltmak için yoludur.

## <a name="case-3---you-are-running-lots-of-independent-queries-in-a-job"></a>Durum 3 - bir işi bağımsız sorgular çok sayıda çalışan.
Belirli ISV kullanmak için olduğu daha tek bir işlemde birden çok kiracıdan verileri işlemek için ekonomik, durumlarda, her bir kiracı için ayrı girişleri ve çıkışları kullanarak, oldukça birkaç (örneğin 20) çalışan bitebilir tek bir İşte bağımsız sorgular. Her tür alt sorgu ait yük görece küçük varsayılır. Bu durumda, aşağıdaki adımları izleyebilirsiniz.
1.  Bu durumda, kullanmayın **bölüm tarafından** sorgu
2.  Olay hub'ı kullanıyorsanız, giriş bölüm sayısı 2 olası en düşük değerini azaltın.
3.  Sorgu ile 6 SU çalıştırın. İş sistem kaynak sınırlarını basarsa kadar her alt sorgu için beklenen yükü ile bu tür sayıda alt sorgulara mümkün olduğunca ekleyin. Başvurmak [durum 1](#case-1--your-query-is-inherently-fully-parallelizable-across-input-partitions) böyle bir durumda belirtiler için.
4.  Yukarıdaki ölçülen alt sorgu sınırı devreyi sonra yeni bir iş alt sorgu eklemeye başlayın. Bir bağımsız olan sorgu sayısını işlevi olarak çalıştırmak için iş sayısı eğme yük yok varsayılarak oldukça doğrusal olmalıdır. Ardından sunmak istediğiniz Kiracı sayısı bir işlevi olarak çalıştırmanız gereken kaç tane 6 SU işleri tahmin.
5.  Başvuru veri JOIN sorgularını ile kullanırken, UNION girişleri birlikte, aynı başvurusu verilerle katılmadan önce olaylarını gerekirse bölünmesi gerekir. Aksi takdirde, her başvuru veri birleştirme büyük olasılıkla bellek kullanımını gereksiz yere blowing bellekte başvuru verilerin bir kopyasını tutar.

> [!Note] 
> Her bir iş koymak için kaç tane kiracılar?
> Bu sorgu deseni genellikle çok sayıda alt sorgulara sahiptir ve çok büyük ve karmaşık topolojisinde sonuçlanır. İşin denetleyicisi büyük bir topoloji işleyebilen olmayabilir. Altın kural, 3 SU ve 6 SU işleri için 1 SU işinin altında 40 kiracılar ve 60 kiracılar kalır. Denetleyici kapasitesini aşıldığında işi başarıyla başlatılmaz.


## <a name="an-example-of-stream-analytics-throughput-at-scale"></a>Stream Analytics verimlilik ölçekte örneği
Nasıl Stream Analytics işlerini ölçeklendirme anlamanıza yardımcı olmak için biz Raspberry Pi'yi aygıt girişten dayalı bir denemeyi gerçekleştirilir. Bu deneme birden çok akış birimleri ile bölümleri verimini etkisini görmek bize.

Bu senaryoda, cihaz algılayıcı verilerini (istemciler) olay hub'ına gönderir. Analytics akış verileri işler ve bir uyarı ya da istatistikleri çıkış olarak başka bir olay hub'ına gönderir. 

İstemcinin JSON biçiminde algılayıcı verileri gönderir. Veri çıkışı da JSON biçimindedir. Veri şöyle görünür:

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

Aşağıdaki sorgu ışık kapalı olduğunda bir uyarı göndermek için kullanılır:

    SELECT AVG(lght), "LightOff" as AlertText
    FROM input TIMESTAMP BY devicetime 
    PARTITION BY PartitionID
    WHERE lght< 0.05 GROUP BY TumblingWindow(second, 1)

### <a name="measure-throughput"></a>Ölçü işleme

Bu bağlamda verimlilik Sabit sürede akış analizi tarafından işlenen giriş veri miktarıdır. (Biz 10 dakika cinsinden ölçülen.) Giriş verileri için en iyi işleme verimliliği elde etmek için veri akış girişine ve sorgu bölümlenmiş. Biz dahil **COUNT()** kaç tane giriş olaylarını işlenen ölçmek için sorgu. İşi yalnızca giriş olaylarını gelen beklediği değil emin olmak için her bir bölümü giriş olay hub'ın yaklaşık 300 MB ile giriş verilerini önceden.

Aşağıdaki tabloda, biz akış birim sayısını artar ve karşılık gelen bölüm olay hub ' sayar gördüğümüz sonuçlarını gösterir.  

<table border="1">
<tr><th>Giriş bölümleri</th><th>Çıktı bölümleri</th><th>Akış Birimleri</th><th>Aralıksız üretilen
</th></td>

<tr><td>12</td>
<td>12</td>
<td>6</td>
<td>4.06 MB/sn</td>
</tr>

<tr><td>12</td>
<td>12</td>
<td>12</td>
<td>8.06 MB/sn</td>
</tr>

<tr><td>48</td>
<td>48</td>
<td>48</td>
<td>38.32 MB/sn</td>
</tr>

<tr><td>192</td>
<td>192</td>
<td>192</td>
<td>172.67 MB/sn</td>
</tr>

<tr><td>480</td>
<td>480</td>
<td>480</td>
<td>454.27 MB/sn</td>
</tr>

<tr><td>720</td>
<td>720</td>
<td>720</td>
<td>609.69 MB/sn</td>
</tr>
</table>

Ve aşağıdaki grafikte bir görsel olarak SUs ve üretilen iş arasındaki ilişkiyi gösterir.

![img.stream.analytics.perfgraph][img.stream.analytics.perfgraph]

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor-NewPortal.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings-NewPortal.png   

<!--Link references-->

[microsoft.support]: http://support.microsoft.com
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

