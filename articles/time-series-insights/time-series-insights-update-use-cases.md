---
title: Azure zaman serisi öngörüleri Önizleme kullanım örnekleri | Microsoft Docs
description: Azure zaman serisi öngörüleri Önizleme kullanım örnekleri anlayın.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: 787445d5186a173b2cba674b36cd95879cc863e5
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66389988"
---
# <a name="azure-time-series-insights-preview-use-cases"></a>Azure zaman serisi öngörüleri Önizleme kullanım örnekleri

Bu makalede, Azure zaman serisi öngörüleri önizlemesi için birçok yaygın kullanım örnekleri özetlenmektedir. Bu makalede öneriler, uygulamalarınızı ve Time Series Insights ile çözümler geliştirmek için bir başlangıç noktası işlevi görür.

Özellikle, bu makalede, aşağıdaki soruları yanıtlamaktadır:

* Time Series Insights için yaygın kullanım örnekleri nelerdir?
* Time Series Insights için kullanmanın avantajları nelerdir [veri keşfi ve görsel anomali algılama](#data-exploration-and-visual-anomaly-detection)?
* Time Series Insights için kullanmanın avantajları nelerdir [operasyonel analiz ve işlem verimliliğini](#operational-analysis-and-driving-process-efficiency)?
* Time Series Insights için kullanmanın avantajları nelerdir [Gelişmiş analiz](#advanced-analytics)?

Bu genel kullanım senaryoları aşağıdaki bölümlerde açıklanmıştır.

## <a name="introduction"></a>Giriş

Azure Time Series Insights uçtan uca hizmet olarak platform teklifidir. Toplamak, işlem, depolamak, çözümlemek ve yüksek oranda contextualized, zaman serisi iyileştirilmiş IOT ölçekli veri sorgulamak için kullanılır. Time Series Insights, geçici veri keşfi ve operasyonel analiz için idealdir. Time Series Insights karşıladığını endüstriyel IOT dağıtımları geniş gerektiğini sunan bir benzersiz şekilde genişletilebilir, özelleştirilmiş hizmetidir.

## <a name="data-exploration-and-visual-anomaly-detection"></a>Veri inceleme ve görsel anormali algılama

Verilerinizdeki anomalileri bulmak ve gizli eğilimleri keşfetmek için milyarlarca olayı anında inceleyip analiz edin. Time Series Insights, IoT ve DevOps analiz iş yüklerinize neredeyse gerçek zamanlı performans katar.

[![Veri Gezgini](media/v2-update-use-cases/data-explorer.svg)](media/v2-update-use-cases/data-explorer.svg#lightbox)

Çoğu müşteri görüşleri zaman Time Series Insights güçlü varlıkları arasında olduğunu kabul etmiş olursunuz. Time Series Insights, veriler için ön hazırlık gerektirir. Hızlı milyarlarca olaya Azure IOT Hub veya Azure Event hubs'ı dakikalar içinde bağlanmaya çalışır. Bağlandıktan sonra görselleştirin ve milyarlarca olayı anormallikleri için analiz edin ve verilerinizi gizli eğilimleri keşfedin.

Time Series Insights, sezgisel ve kullanımı kolaydır. Tek satırlık bir kod yazmadan verilerinizle etkileşim kurabilirsiniz. Yeni dil öğrenmek gerekmez yoktur. Time Series Insights ayrıntılı metin tabanlı SQL ile ilgili bilgi sahibi olan gelişmiş kullanıcılar için sorgulama sağlar. Ayrıca, seçin ve tıklama başlayanlara da sağlar.

Müşteriler, varlık ilgili sorunları hızla tanılayın hızını avantajlarından yararlanın. Bunlar, bir IOT çözümündeki bir hatanın nedenini almak için DevOps gerçekleştirebilirsiniz. Bunlar ayrıca alanlar için veri bilimi girişimlerini araştırmaya belirleyebilir.  

Time Series Insights içinde depolanan verilerle etkileşim kurmak için başlıca üç yolu vardır:

- Başlamak için ilk ve en kolay yolu zaman serisi öngörüleri Önizleme Gezgini'yle birlikte ' dir. Hızlı bir şekilde tüm verilerinizi tek bir yerden görselleştirmek için kullanabilirsiniz. Verilerinizdeki anormallikleri yardımcı olması için ısı haritası gibi araçlar sağlar. Ayrıca, bir perspektif görünüm sağlar. Bir veya daha fazla zaman serisi görüşleri ortamları tek bir Panoda en fazla dört görünümleri karşılaştırmak için kullanın. Pano, konumlar arasında zaman serisi verilerinin bir görünümünü sağlar. Daha fazla bilgi edinin [zaman serisi öngörüleri Önizleme Gezgini](./time-series-insights-update-explorer.md). Time Series Insights ortamınızı planlama okuyun [Time Series Insights'ı planlama](./time-series-insights-update-plan.md).

- İkinci yol başlatmak için hızla güçlü grafikler ve graflar web uygulamanızda eklemek için JavaScript SDK'sı kullanmaktır. Yalnızca birkaç kod satırıyla, güçlü sorgular yazabilirsiniz. Çizgi grafikler, pasta, çubuk grafikler, ısı Haritaları, veri kılavuzları ve daha fazla doldurmak için bunları kullanın. Tüm bu öğeleri kullanıma hazır, SDK'sını kullanarak mevcut. SDK ayrıca Time Series Insights sorgu API'leri soyutlar. Bir Panoda göstermek istediğiniz verileri sorgulamak için SQL benzeri koşullar yazmak için kullanabilirsiniz. Karma sunu katmanı çözümler için parametreli URL'lerin Time Series Insights'ı sunar. Sorunsuz bağlantı noktaları zaman serisi öngörüleri Önizleme Gezgini'yle birlikte verilerin ayrıntılı incelemeler için sağlarlar.

    * Okuma [zaman serisi öngörüleri JS istemci Kitaplığı](tutorial-explore-js-client-lib.md) ve [Time Series Insights istemci](https://github.com/Microsoft/tsiclient) JavaScript SDK'sı hakkında daha fazla bilgi edinmek için belgeleri.

    * URL'ler ve yeni kullanıcı Arabirimi inceleyerek paylaşma hakkında daha fazla bilgi edinin [Azure zaman serisi öngörüleri önizlemesi gezginde verileri görselleştirme](time-series-insights-update-explorer.md).

- Başlatmak için üçüncü Time Series Insights içinde depolanan verileri sorgulamak için güçlü API'lerini kullanmaktır. Time Series Insights sahip geçici işleçler gibi `from`, `to`, `first`, ve `last`. Toplamalar ve dönüştürmeler gibi sahip `average`, `min`, `max`, `split by`, `order by`, ve `DateHistogram`. İşleçler gibi filtreleme de vardır `has`, `in`, `and`, `or`, `greater than`, ve `REGEX`. Bu işleçler, ilginç eğilimleri ve desenleri verilerinizi hızla bulmak aşağı akış uygulamaları etkinleştirin. Anomalileri görselleştirmeleri ev yapımı doldurmak için bunları kullanın.

## <a name="operational-analysis-and-driving-process-efficiency"></a>İşlem analizi ve işlem verimliliğini sağlama

Time Series Insights, sistem durumu, kullanım ve uygun ölçekte donanım performansını izlemek için kullanın. Time Series Insights, işlem verimliliğini ölçmeye yönelik kolay bir yol sağlar. Time Series Insights, öngörülemeyen çeşitli IoT iş yüklerini, alma ve sorgu performansından ödün vermeden yönetmenize yardımcı olur.

[![Genel bakış](media/v2-update-use-cases/overview.svg)](media/v2-update-use-cases/overview.svg#lightbox)

Sağa teknoloji veya çözüm ile birlikte kullanıldığında akış ve gelen işletimsel işlemlerden veri sürekli işleme başarıyla tüm işletmeler dönüştürebilirsiniz. Genellikle bu çözümleri birden çok sistem birleşimidir. Bunlar, araştırması ve analizi, IOT bölge özellikle, devamlı olarak değiştirir ve yaygın bir düzen paylaşımları veri sağlar.

Bu düzenleri, genellikle milyarlarca olayı cihazlardan ve sensörlerden çeşitli yerel ayarlara yayılan içe alma, IOT özellikli platformlar çalışmaya başlayın. Bu sistemler işleyebilir ve gerçek zamanlı anlayışlar ve Eylemler türetmek için akış verilerini analiz edin. Veriler genellikle normal arşivlenir ve soğuk depolama için neredeyse gerçek zamanlı ve toplu.

Toplanan verileri bir dizi temizlemeyi ve aşağı akış sorgulama ve analiz senaryoları için bağlama göre ele alınmasına işleme geçer. Azure IOT senaryoları varlık Bakım ve üretim gibi uygulanabilir zengin hizmetleri sunar. Bu hizmetler, zaman serisi görüşleri, IOT Hub, Event Hubs, Azure Stream Analytics, Azure işlevleri, Azure Logic Apps, Azure Databricks, Azure Machine Learning ve Power BI içerir.

Çözüm mimarisi, aşağıdaki şekilde sağlanabilir:

- Sınıfının en iyisi güvenlik, aktarım hızı ve gecikme süresi için IOT hub'ı veya olay hub'ları aracılığıyla veri alın.
- Veri işleme ve hesaplamalar gerçekleştirin. Stream Analytics, Logic Apps ve Azure işlevleri gibi hizmetler aracılığıyla alınan verileri Huni. Kullandığınız hizmetin belirli veri işleme gereksinimlerine bağlıdır.
- Hesaplanan sinyal işleme ardışık düzendeki zaman serisi öngörüleri için depolamak ve analiz için gönderilir.

Time Series Insights, geçmiş veriler üzerinde gerçek zamanlı bir veri keşfi ve varlık temelli öngörüleri sunar. İş gereksinimlerinize bağlı olarak, Azure HDInsight için Time Series Insights'ı bağlayarak Time Series Insights içinde depolanan veriler MapReduce ve Hive işleri çalıştırabilirsiniz. Time Series Insights içinde depolanan veriler, Power BI ve diğer Time Series Insights genel yüzey sorgu API'leri aracılığıyla müşteri uygulamaları için kullanılabilir. Bu verileri kapsamlı iş ve operasyonel zeka senaryoları için kullanılabilir.

## <a name="advanced-analytics"></a>Gelişmiş analiz

Machine Learning ve Azure Databricks gibi gelişmiş Analiz Hizmetleri ile tümleştirin. Zaman serisi görüşleri ingresses ham verilerden milyonlarca cihazı. Sorunsuz bir şekilde Azure Analiz Hizmetleri paketi tarafından tüketilebilecek bağlamsal veriler ekler.

[![Analytics](media/v2-update-use-cases/advanced-analytics.svg)](media/v2-update-use-cases/advanced-analytics.svg#lightbox)

Gelişmiş analiz ve makine öğrenimi kullanan ve büyük hacimli verileri işleyebilirsiniz. Bu veriler, veri odaklı kararlar ve Tahmine dayalı analiz gerçekleştirmek için kullanılır. IOT kullanım durumlarında, Gelişmiş analiz algoritmaları milyonlarca CİHAZDAN toplanan verilerden bilgi edinin. Bu cihazlar, birden çok kez saniyede veri aktarır. IOT cihazlarından toplanan verileri ham. Bağlamsal bilgiler konumu gibi cihaz ve sensör okumaya birimi eksik. Sonuç olarak, ham verileri doğrudan Gelişmiş analiz için kullanmak üzere zordur.

Time Series Insights, iki basit ve ekonomik şekilde IOT veri ve Gelişmiş analiz arasındaki boşluk arasında köprü:

- İlk olarak, zaman serisi görüşleri IOT Hub'ı kullanarak ham telemetri verileri milyonlarca CİHAZDAN toplar. Bağlamsal bilgilerle birlikte veri zenginleştirir ve verileri parquet biçime dönüştürür. Bu biçim, hizmetleri, Machine Learning, Azure Databricks ve üçüncü taraf uygulamalar gibi diğer gelişmiş analiz ile kolayca tümleştirilebilir.

    Time Series Insights, kuruluş genelinde tüm verileri doğru kaynağı olarak görebilir. Bunu kullanmak için iş yüklerini aşağı akış analizi için merkezi bir depo oluşturur. Time Series Insights'ı olduğu için neredeyse gerçek zamanlı bir depolama hizmeti, Gelişmiş analiz modelleri gelen IOT telemetri verilerini sürekli olarak bilgi edinin. Sonuç olarak, modelleri daha doğru tahminler yapabilir.

- İkinci olarak, zaman serisi görüşleri görselleştirin ve sonuçlarını depolamak için makine öğrenme ve tahmin modellerinin çıkış beslenir. Bu yordamı iyileştirmek ve modellerini ince ayar için kuruluşlara yardımcı olur. Time Series Insights eğitilen model çıktıları gibi aynı düzlemde ilişkin telemetri verilerini akış görselleştirmek basit hale getirir. Bu şekilde, anormallikleri belirlemek ve düzenlerini belirleyen veri bilimi ekipleri yardımcı olur.  

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [zaman serisi öngörüleri Önizleme Gezgini](./time-series-insights-update-explorer.md).
- Okuma [zaman serisi öngörüleri Önizleme planlama](./time-series-insights-update-plan.md) ortamınızı planlamak için.
- Okuma [Time Series Insights istemci](https://github.com/Microsoft/tsiclient) belgeleri.
