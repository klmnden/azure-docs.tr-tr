---
title: Azure Stream Analytics çözüm desenleri
description: Azure Stream Analytics için çözüm desenler hakkında bilgi.
author: zhongc
ms.author: zhongc
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: 5929ff439bc31e16643e5c57868cd6b68f9cd99c
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67329565"
---
# <a name="azure-stream-analytics-solution-patterns"></a>Azure Stream Analytics çözüm desenleri

Çok sayıda diğer hizmetleri gibi Azure Stream Analytics en iyi diğer hizmetlerle daha büyük bir uçtan uca çözüm oluşturmak için kullanılır. Bu makalede, Azure Stream Analytics çözümleri basit ve çeşitli mimari desenleri açıklar. Daha karmaşık çözümleri geliştirmek üzere bu eğilimlere oluşturabilirsiniz. Bu makalede açıklanan desenleri, çok çeşitli senaryolarda kullanılabilir. Senaryoya özgü düzeni örnekleri vardır [Azure çözüm mimarileri](https://azure.microsoft.com/solutions/architecture/?product=stream-analytics).

## <a name="create-a-stream-analytics-job-to-power-real-time-dashboarding-experience"></a>Güç gerçek zamanlı yönelik Kompozit deneyimi için bir Stream Analytics işi oluşturma

Azure Stream Analytics ile gerçek zamanlı panolar ve uyarılar hızlı bir şekilde bildirimde bulunabilir. Event Hubs'ı veya IOT Hub, olaylarından basit bir çözüm alır ve [akış veri kümesini içeren Power BI Panosu akışları](/power-bi/service-real-time-streaming). Daha fazla bilgi için ayrıntılı bkz [Stream Analytics ile telefon araması verileri analiz etmek ve Power BI panosunda sonuçlarını görselleştirme](stream-analytics-manage-job.md).

![ASA Power BI Panosu](media/stream-analytics-solution-patterns/pbidashboard.png)

Bu çözüm, Azure portalından yalnızca birkaç dakika içinde oluşturulabilir. Kapsamlı kodlama söz konusu yoktur ve SQL dili, iş mantığı ifade etmek için kullanılır.

Bu çözüm düzeni Power BI panosuna bir tarayıcıda olay kaynağından en düşük gecikme sunar. Azure Stream Analytics, bu yerleşik özelliği ile yalnızca Azure hizmetidir.

## <a name="use-sql-for-dashboard"></a>SQL için panoyu kullanın.

Power BI panosunu, düşük gecikme süresi sunar, ancak tam kapsamlı Power BI raporlarını üretmek için kullanılamaz. Bir ortak raporlama verilerinizi SQL veritabanı için ilk çıktı modelidir. Ardından Power BI'ın SQL Bağlayıcısı SQL sorgu için en son verileri için kullanın.

![ASA SQL Panosu](media/stream-analytics-solution-patterns/sqldashboard.png)

SQL kullanarak veritabanı, daha fazla esneklik sağlar ancak biraz daha yüksek gecikme çoğaltamaz. Bu çözüm, bir ikincisinden büyük gecikme süresi gereksinimlerine sahip işler için idealdir. Bu yöntemle Power BI özelliklerini daha fazla dilim için en üst düzeye çıkarmak ve raporları ve çok daha fazla görselleştirme seçenekleri için verilerin ayrıntılarına inin. Ayrıca, Tableau gibi diğer Pano çözümlerini kullanma konusunda esneklik kazanabilir.

SQL yüksek aktarım hızı veri deposu değil. Azure Stream Analytics'ten gelen en yüksek aktarım bir SQL veritabanı, şu anda yaklaşık 24 MB/sn aşamasındadır. Verileri daha yüksek bir hızda çözümünüzdeki olay kaynakları oluşturmak, çıkış oranı SQL azaltmak için Stream Analytics'te işleme mantığı kullanmanız gerekir. Teknikleri, pencereli toplamlar, filtreleme gibi zamana bağlı birleştirmelerle eşleşen desen ve analitik işlevler kullanılabilir. SQL için çıkış oranı açıklanan teknikleri kullanarak daha fazla iyileştirilebilir [Azure SQL veritabanı için Azure Stream Analytics çıkış](stream-analytics-sql-output-perf.md).

## <a name="incorporate-real-time-insights-into-your-application-with-event-messaging"></a>Uygulamanızın olay Mesajlaşma ile gerçek zamanlı Öngörüler ekleyebilirsiniz.

İkinci en popüler Stream Analytics gerçek zamanlı uyarılar oluşturmak için kullanılır. Bu çözüm modelinde iş mantığı Stream analytics'te algılamak için kullanılabilir [zamana bağlı ve uzamsal desenleri](stream-analytics-geospatial-functions.md) veya [anomalileri](stream-analytics-machine-learning-anomaly-detection.md), ardından uyarı sinyal üretebilir. Ancak, birkaç ara veri havuzları burada Stream Analytics tercih edilen bir uç noktası olarak Power BI kullanır. Pano çözüm, kullanılabilir. Bu havuzlar, Event Hubs, Service Bus ve Azure işlevleri içerir. Uygulama oluşturucusu, hangi veri havuzu senaryonuz için en iyi çalıştığı karar vermeniz gerekir.

Aşağı Akış olayı tüketici mantıksal mevcut iş akışınızda uyarılar verecek şekilde uygulanmalıdır. Azure işlevleri'nde özel mantığı uygulayabilir olduğundan, Azure işlevleri, bu tümleştirme gerçekleştirebilirsiniz en hızlı yoludur. Bir öğretici için Azure işlevini kullanarak bir Stream Analytics işi için çıkış dosyasında bulunan [çalıştırın, Azure işlevleri Azure Stream Analytics işlerine](stream-analytics-with-azure-functions.md). Azure işlevleri, çeşitli metin ve e-posta bildirimleri de destekler. Mantıksal uygulama, Stream Analytics ve mantıksal uygulama arasında Event Hubs ile böyle bir tümleştirme için de kullanılabilir.

![ASA olay Mesajlaşma uygulaması](media/stream-analytics-solution-patterns/eventmessagingapp.png)

Olay hub'ları, diğer taraftan, en esnek tümleştirme noktası sunar. Azure Veri Gezgini ve Time Series Insights gibi çok sayıda diğer hizmet, olayları Event hubs'dan kullanabilir. Hizmetleri çözümünü tamamlamak için Azure Stream Analytics'ten doğrudan Event Hubs havuzuna bağlanabilir. Event Hubs, aynı zamanda en yüksek aktarım hızı Mesajlaşma aracısı gibi tümleştirme senaryoları için Azure üzerinde kullanılabilir.

## <a name="dynamic-applications-and-websites"></a>Dinamik uygulama ve Web siteleri

Azure Stream Analytics ve Azure SignalR hizmeti kullanılarak harita görselleştirmesi, ya da Pano gibi özel gerçek zamanlı görselleştirmeler oluşturun. SignalR kullanarak web istemcileri, güncelleştirilebilir ve dinamik içerik gerçek zamanlı olarak göster.

![ASA dinamik uygulama](media/stream-analytics-solution-patterns/dynamicapp.png)

## <a name="incorporate-real-time-insights-into-your-application-through-data-stores"></a>Uygulamanız hakkında gerçek zamanlı Öngörüler veri depoları arasında dahil edilip derecelendirilir.

Bugün çoğu web hizmetleri ve web uygulamaları istek/yanıt deseni sunu katmanını hizmet vermek için kullanın. İstek/yanıt desendir kolay bir şekilde oluşturun ve durum bilgisi olmayan bir ön uç ve Cosmos DB gibi ölçeklenebilir depolarını kullanarak düşük yanıt süresi ile kolayca ölçeklendirilebilir.

Yüksek veri hacmi, performans sorunlarını CRUD tabanlı bir sistemde genellikle oluşturur. [Olay kaynağını belirleme modeli çözüm](/azure/architecture/patterns/event-sourcing) performans sorunlarını gidermek için kullanılır. Zamana bağlı desenleri ve öngörüleri da zor ve geleneksel veri deposundaki verileri ayıklamak için yetersiz. Uygulamalar genellikle temelli Modern yüksek hacimli verileri bir veri akışı tabanlı mimari benimseyin. Azure Stream Analytics, Hareket halindeki veriler için işlem altyapısı olarak mimarideki bir numaralı ' dir.

![ASA olay kaynağını uygulama](media/stream-analytics-solution-patterns/eventsourcingapp.png)

Bu çözüm modelinde olayları işlenir ve veri depolarına Azure Stream Analytics tarafından toplanır. Uygulama katmanı, geleneksel istek/yanıt desenini kullanarak, veri depoları ile etkileşime girer. Stream Analytics, çok sayıda olay işleme yeteneği nedeniyle gerçek zamanlı, yüksek düzeyde ölçeklenebilir bir veri deposu katmanını oluşturan toplu gerek kalmadan uygulamasıdır. Veri deposu temelde bir gerçekleştirilmiş görünümün sistemde katmanıdır. [Azure Cosmos DB için Azure Stream Analytics çıkışı](stream-analytics-documentdb-output.md) Cosmos DB bir Stream Analytics çıktı olarak nasıl kullanıldığını açıklar.

Bazı kısımlarını logic bağımsız olarak yükseltme için gereken işleme mantığı karmaşık ve orada olduğu gerçek uygulamalarda, birden çok Stream Analytics işleri, aracı olay aracısı olarak Event Hubs ile birlikte oluşabilir.

![ASA karmaşık olay kaynağını uygulama](media/stream-analytics-solution-patterns/eventsourcingapp2.png)

Bu düzen esnekliği ve yönetilebilirlik sisteminin artırır. Ancak, tam olarak işleme sonra Stream Analytics garanti eder olsa da, yinelenen olayları Ara olay hub'ları, kavuşmak küçük bir fırsat yoktur. Olayları lookback penceresinde mantıksal anahtarları kullanarak yinelenenleri kaldırma aşağı akış Stream Analytics işi önemlidir. Olay teslimi hakkında daha fazla bilgi için bkz. [olay teslimat Garantileriyle](/stream-analytics-query/event-delivery-guarantees-azure-stream-analytics) başvuru.

## <a name="use-reference-data-for-application-customization"></a>Başvuru verilerini kullanması için uygulamayı özelleştirme

Azure Stream Analytics başvuru verileri özelliği, uyarı verme eşiği, kuralları, işleme gibi özellikle son kullanıcı özelleştirme için tasarlanmıştır ve [bölge sınırlarının](geospatial-scenarios.md). Uygulama katmanı, parametre değişiklikleri kabul etmek ve bunları bir SQL veritabanında depolayın. Stream Analytics işi, düzenli aralıklarla değişiklikler veritabanından sorgular ve özelleştirme parametreleri bir başvuru veri birleştirme erişilebilir hale getirir. Başvuru verilerini kullanması için uygulamayı özelleştirme konusunda daha fazla bilgi için bkz. [SQL başvuru verilerini](sql-reference-data.md) ve [başvuru veri birleştirme](/stream-analytics-query/reference-data-join-azure-stream-analytics).

Bu düzen ayrıca kurallar altyapısı uygulamak için kuralların eşikleri başvuru verilerini tanımlandığı kullanılabilir. Kurallar hakkında daha fazla bilgi için bkz. [işlem Azure Stream analytics'te yapılandırılabilir eşik temelli kurallar](stream-analytics-threshold-based-rules.md).

![ASA başvuru veri uygulaması](media/stream-analytics-solution-patterns/refdataapp.png)

## <a name="add-machine-learning-to-your-real-time-insights"></a>Machine Learning, gerçek zamanlı Öngörüler için ekleyin

Azure Stream Analytics'in yerleşik [Anomali algılama modeli](stream-analytics-machine-learning-anomaly-detection.md) uygulamanıza gerçek zamanlı Machine Learning tanıtmak için kullanışlı bir yoldur. Bir geniş aralığı, Machine Learning ihtiyaçları için bkz. [Azure Stream Analytics, Azure Machine Learning'ın Puanlama hizmetiyle tümleştirilir](stream-analytics-machine-learning-integration-tutorial.md).

Çevrimiçi eğitim ve puanlama aynı Stream Analytics işlem hattı birleştirmek istiyorsanız Gelişmiş kullanıcılar için bu örnek nasıl bunun [doğrusal regresyon](stream-analytics-high-frequency-trading.md).

![ASA makine öğrenimi uygulaması](media/stream-analytics-solution-patterns/mlapp.png)

## <a name="near-real-time-data-warehousing"></a>Neredeyse gerçek zamanlı veri ambarlama

Başka bir yaygın depolama, veri ambarı'nı akış olarak da adlandırılan gerçek zamanlı veri modelidir. Olay hub'ları ve IOT hub'ı uygulamanızdan gelen olayları yanı sıra [çalışan IOT Edge üzerinde Azure Stream Analytics](stream-analytics-edge.md) veri temizleme, veri azaltma ve veri deposu ve iletme gereksinimlerini karşılamak için kullanılabilir. Stream Analytics IOT Edge'de çalışan düzgün bir şekilde sisteminde bant genişliği sınırlaması ve bağlantı sorunları işleyebilir. SQL çıkış bağdaştırıcısı kullanılabilir çıktı SQL veri ambarı; Ancak, en yüksek aktarım 10 MB/sn için sınırlıdır.

![ASA veri ambarı](media/stream-analytics-solution-patterns/datawarehousing.png)

Bazı gecikme süresini artırabilen ile aktarım hızını artırmanın bir yolu olan Azure Blob depolama alanına olayları arşivlenecek ardından [Polybase ile SQL veri ambarı'na aktarın](../sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase.md). El ile BLOB Depolama, Stream analytics'ten çıkış ve giriş blob depolamadan SQL veri ambarı tarafından birleştirmek gerekir [zaman damgası veri arşivleme](stream-analytics-custom-path-patterns-blob-storage-output.md) ve düzenli aralıklarla alma.

Bu kullanım modelinde, Azure Stream Analytics, neredeyse gerçek zamanlı ETL altyapısı olarak kullanılır. Yeni gelen olayları sürekli olarak dönüştürülür ve aşağı akış analizi hizmet tüketimi için depolanır.

![ASA yüksek verimlilik veri ambarlama](media/stream-analytics-solution-patterns/datawarehousing2.png)

## <a name="archiving-real-time-data-for-analytics"></a>Gerçek zamanlı veri analizi için arşivleme

Çoğu veri bilimi ve analiz etkinlikleri çevrimdışı gerçekleşir. Azure Data Lake Store Gen2 çıkış Parquet Çıkış biçimleri ile veri Azure Stream Analytics tarafından saklanabilir. Bu özellik, doğrudan Azure Data Lake Analytics, Azure Databricks ve Azure HDInsight veri akışı ayırmamıza kaldırır. Azure Stream Analytics, bu çözümde neredeyse gerçek zamanlı ETL altyapısı olarak kullanılır. Veri Gölü'nde arşivlenmiş veriler, çeşitli işlem altyapıları kullanarak keşfedebilirsiniz.

![ASA çevrimdışı analiz](media/stream-analytics-solution-patterns/offlineanalytics.png)

## <a name="use-reference-data-for-enrichment"></a>Olayların başvuru verilerini kullanma

Veri zenginleştirme ETL altyapıları için genellikle bir gereksinimdir. Azure Stream Analytics ile veri zenginleştirme destekler [başvuru verileri](stream-analytics-use-reference-data.md) hem SQL veritabanı hem de Azure Blob Depolama. Veri zenginleştirme, hem Azure Data Lake hem de SQL veri ambarı'na gelen veriler için yapılabilir.

![Veri zenginleştirme ile ASA çevrimdışı analiz](media/stream-analytics-solution-patterns/offlineanalytics.png)

## <a name="operationalize-insights-from-archived-data"></a>Arşivlenen verilerden kullanıma hazır hale getirme

Neredeyse gerçek zamanlı uygulama ile çevrimdışı analiz düzeni birleştirmek, deseni, bir geri bildirim döngüsü oluşturabilirsiniz. Geri bildirim döngüsü, veri desenlerinde değiştirmek için otomatik olarak ayarlamak uygulama sağlar. Bu geri bildirim döngüsü, uyarı için eşik değerini değiştirme kadar basit veya makine öğrenimi modelleri yeniden eğitme gibi karmaşık olabilir. Aynı çözüm mimarisi, bulutta ve IOT Edge çalıştıran her iki ASA işleri için uygulanabilir.

![ASA ınsights kullanıma hazır hale getirme](media/stream-analytics-solution-patterns/insightsoperationalization.png)

## <a name="how-to-monitor-asa-jobs"></a>ASA işleri izleme

Azure Stream Analytics işi, gerçek zamanlı, sürekli olarak gelen olayları işlemek için 24 x 7 çalıştırılabilir. Kendi çalışma zamanı garantisi için uygulama durumunu önemlidir. Stream Analytics, yalnızca akış analiz hizmeti sunan sektörün ise bir [% 99,9 kullanılabilirliği garantisi](https://azure.microsoft.com/support/legal/sla/stream-analytics/v1_0/), yine de bazı düzeyini kesinti gerektirebilir. Yıllar içinde Stream Analytics, ölçümler, günlükler ve işlerin durumunu yansıtacak şekilde iş durumları sundu. Bunların tümünde Azure İzleyici hizmeti aracılığıyla öne çıkarılır ve daha fazla OMS'ye aktarılabilir. Daha fazla bilgi için [anlamak Stream Analytics, izleme ve sorguları izleme iş](stream-analytics-monitoring.md).

![ASA izleme](media/stream-analytics-solution-patterns/monitoring.png)

İzlemek için iki önemli nokta vardır:

- [İş durumu ile başarısız oldu.](job-states.md)

    İlk ve, işi çalıştığından emin olmanız gerekir. Olmadan iş çalışır durumda yeni ölçümler ya da günlükleri üretilir. İşler, bir yüksek SU kullanım düzeyi sahip dahil olmak üzere çeşitli nedenlerle başarısız duruma değiştirebilirsiniz (yani, kaynaklar yetersiz çalıştıran).

- [Eşik gecikmesi ölçümleri](https://azure.microsoft.com/blog/new-metric-in-azure-stream-analytics-tracks-latency-of-your-streaming-pipeline/)

    Bu ölçüm, işleme ne kadar arkasında yansıtan işlem hattını duvar saati süresi (saniye) olan. Bazı gecikme öznitelikli devralınan işleme mantığı. Sonuç olarak, artan bir eğilim izleme mutlak değeri izleme daha çok daha önemlidir. Kararlı bir duruma gecikme uygulama tasarım gereği, olmayan izleme veya uyarılar tarafından izlenmelidir.

Başarısızlık durumunda, etkinlik günlükleri ve [tanılama günlükleri](stream-analytics-job-diagnostic-logs.md) hataları bakarak başlamak için en iyi yerlerdir.

## <a name="build-resilient-and-mission-critical-applications"></a>Dayanıklı ve görev açısından kritik uygulamalar oluşturun

Azure Stream Analytics SLA garantisi bağımsız olarak ve nasıl dikkatli kesintiler meydana uçtan uca uygulamanızı çalıştırın. Görev açısından kritik uygulama ise kesintiler için düzgün bir şekilde kurtarmak için hazırlanması gerekir.

Uygulamaları uyarmak için sonraki uyarı algılamak için en önemli şey olabilir. Geçerli işi yeniden başlatmak seçebilir, uyarılarını yoksayma kurtarırken zaman. İş başlangıç zamanı semantiği ilk çıkış saati ilk giriş saat arasındadır. Giriş saati belirtilen zamanda ilk çıkış tam ve doğru garanti etmek için uygun miktarda bir geriye doğru sarılır. Kısmi toplamlar Al yüklemeyecekseniz ve sonucunda uyarılar beklenmedik bir şekilde tetikler.

Çıkış süresi geçmiş miktar başlatmayı seçebilirsiniz. Event Hubs hem IOT Hub'ın bekletme ilkeleri makul bir son işleme izin verecek şekilde veri tutar. Artırabilen ne kadar hızlı geçerli saate Kaçırdığınız ve zamanında yeni uyarılar oluşturmak Başlat ' dir. Geçerli tarihe kadar çabuk yakalamak önemlidir veri değeri zaman içinde hızla kaybeder. Çabuk yakalamak için iki yolu vardır:

- Daha fazla kaynağı (SU) yakalama olduğunda sağlayın.
- Geçerli saatten yeniden başlatın.

Geçerli kullanıcıdan yeniden başlatma zamanı işleme sırasında bir boşluk bırakarak karşılıklı avantaj ve dezavantajlarını yapmak basit bir işlemdir. Bu şekilde yeniden senaryoları uyarmak için Tamam olabilir, ancak Pano senaryoları için sorunlara neden olabilir ve olmayan-Başlatıcı arşivleme ve veri depolama senaryolarında olduğu.

Daha fazla kaynak sağlama işlemi hızlandırmak, ancak işleme hızı aşırı yükleme talebiyle sahip olmanın etkisi karmaşıktır.

- Test iş çok sayıda SUs fazladır. Tüm sorguları ölçeklenebilir. Sorgunuzu olduğundan emin olmak gereken [paralelleştirildi](stream-analytics-parallelization.md).

- Yukarı Akış Event hubs'ı veya IOT Hub'ın daha fazla işleme birimi (giriş aktarım hızı ölçeklendirme işleme birimi) ekleyebileceğiniz yetecek kadar bölüm olmadığından emin olun. Unutmayın, her Event Hubs işleme birimi 2 MB/sn çıkış hızında kullanıma yükselir.

- Yeterli kaynaklar (yani, SQL veritabanı, Cosmos DB) çıktı havuzlarından sağladığınızdan, aşırı yükleme talebiyle bazen alabilen çıkış azaltma olmayan şekilde kilitleme başlatılmasına neden emin olun.

İşleme hızı değişiklik tahmin, üretime geçmeden önce bu senaryoları test etme ve hata kurtarma zamanı işlenirken doğru bir şekilde ölçeklendirmek hazır olması için en önemli şey var.

Gelen olayları tüm aşırı senaryoda geciktirilmiş [olası tüm olaylar gecikir bırakılır olan](stream-analytics-time-handling.md) işinize geç gelen pencere uyguladıysanız. Olaylar bırakılıyor başında, gizemli bir davranış olarak görünebilir; Ancak, Stream Analytics gerçek zamanlı işleme altyapısıdır considering gelen olayları duvar saati süresi yakın olmasını bekliyor. Bu kısıtlamaları ihlal olayları bırakmak sahiptir.

### <a name="backfilling-process"></a>İyileştirmenin işlemi

Neyse ki, önceki veri arşivleme deseni geç Bu olayları düzgün bir şekilde işlemek için kullanılabilir. Arşivleme işi geliş saati gelen olayları işler ve Azure Blob veya Azure Data Lake Store, olay saati ile doğru zaman aralığındaki oturum olayları arşivleri olur. Bir olayın nasıl geç geldiğinde önemli değildir, hiçbir zaman bırakılır. Ayrıca, doğru zaman aralığındaki her zaman ulaşırsınız. Kurtarma sırasında arşivlenen olayları ve doldurma deponun tercih ettiğiniz sonuçları yeniden işlemek mümkündür.

![ASA doldurma](media/stream-analytics-solution-patterns/backfill.png)

Doldurma işlemi, çevrimdışı bir toplu işlem, büyük olasılıkla Azure Stream Analytics daha farklı bir programlama modeli olan sistem işleme yapılması gerekir. Bu, tüm işleme mantığının yeniden uygulamak sahip olduğunuz anlamına gelir.

İyileştirmenin için yine de en azından geçici olarak işleme gereksinimlerini kararlı durumdakinden daha yüksek aktarım hızını işlemek için çıktı daha fazla kaynak havuzlarını sağlamak için önemlidir.

|Senaryolar  |Yalnızca şimdi yeniden Başlat  |Yeniden başlatma zamanı son durduruldu. |Yeniden gelen now + arşivlenmiş olayları ile doldurma|
|---------|---------|---------|---------|
|**Yönelik Kompozit**   |Aralık oluşturur    |Kısa bir kesinti için Tamam    |Uzun kesinti için kullanın |
|**Uyarı verme**   |Kabul edilebilir |Kısa bir kesinti için Tamam    |Gerekli değil |
|**Olay kaynağını uygulama** |Kabul edilebilir |Kısa bir kesinti için Tamam    |Uzun kesinti için kullanın |
|**Veri ambarı**   |Veri kaybı  |Kabul edilebilir |Gerekli değil |
|**Çevrimdışı analiz**  |Veri kaybı  |Kabul edilebilir |Gerekli değil|

## <a name="putting-it-all-together"></a>Hepsini bir araya getirme

Yukarıda belirtilen çözüm modellerinin birlikte karmaşık bir uçtan uca sistemde birleştirilebilir Imagine sabit değil. Birleşik sistem panolar, uyarılar, olay kaynağını uygulama, veri ambarı ve çevrimdışı analiz özellikleri içerebilir.

Her alt sistemin yerleşik şekilde birleştirilebilir desenleri, sisteminizdeki test, yükseltme, tasarım ve bağımsız şekilde kurtarmasına anahtardır.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi, Azure Stream Analytics'i kullanarak çözüm modellerinin çeşitli gördünüz. Bundan sonra derinlere inerek ilk Stream Analytics işinizi oluşturabilirsiniz:

* [Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md).
* [Azure PowerShell kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-powershell.md).
* [Visual Studio kullanarak bir Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md).
