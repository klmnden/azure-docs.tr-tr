---
title: Azure Time Series Insights (Önizleme) kullanım örnekleri | Microsoft Docs
description: Kullanım örnekleri anlama Azure Time Series Insights (Önizleme)
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/03/2018
ms.openlocfilehash: 67be21ae7f0cb997563f17130b9d5ecb7d359b31
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52873878"
---
# <a name="azure-time-series-insights-preview-use-cases"></a>Azure Time Series Insights (Önizleme) kullanım örnekleri

Bu makalede, Azure zaman serisi öngörüleri (TSI) için bazı ortak kullanım durumları için genel bir bakış sağlar. Uygulamalar ve çözümlerle TSI geliştirirken bu makaledeki önerileri bir başlangıç noktası olarak hizmet.

Bu makaleyi okuduktan sonra aşağıdaki soruları yanıtlamak mümkün olacaktır:

* Azure TSI için yaygın kullanım örnekleri nelerdir?
* Veri keşfi ve görsel anomali algılama için Azure TSI kullanmanın avantajları nelerdir?
* Azure TSI operasyonel analiz ve süreç verimliliği için kullanmanın avantajları nelerdir?
* Gelişmiş analiz için Azure TSI kullanmanın avantajları nelerdir?

Bu belge, Azure TSI özel önizlemesi için tasarlanmış kullanım örnekleri için genel bir bakış sağlar.

## <a name="introduction"></a>Giriş

Azure TSI bir uçtan uca hizmet olarak Platform-A-alma, işlem, depolama ve yüksek oranda contextualized, zaman serisi iyileştirilmiş IOT ölçekli veri sorgulamak için ' dir. Bu nedenle, Azure TSI operasyonel analiz yanı sıra geçici veri keşfi için idealdir. TSI karşıladığını endüstriyel IOT dağıtımları geniş gerektiğini sunan benzersiz şekilde genişletilebilir, özelleştirilmiş, hizmetidir.

## <a name="data-exploration-and-visual-anomaly-detection"></a>Veri inceleme ve görsel anormallik algılama

Verilerinizdeki anomalileri bulmak ve gizli eğilimleri keşfetmek için milyarlarca olayı anında inceleyip analiz edin. Neredeyse gerçek zamanlı performans, IOT ve DevOps analiz iş yükleri için Azure TSI sunar.

![Veri Gezgini][1]

Tüm Azure TSI'ın gücü müşterilerin çoğu zaman görüşleri arasında güçlü olduğunu kabul etmiş olursunuz. TSI, veriler için ön hazırlık gerektirir ve dakikalar içinde milyarlarca olaya Azure IOT hub'ı veya olay hub'ına bağlanma hızlı çalışır.  Bağlandıktan sonra görselleştirin ve milyarlarca olayı anormallikleri için analiz edin ve verilerinizi gizli eğilimleri keşfedin.  TSI, sezgisel ve kullanması kolay olduğundan, tek satırlık bir kod yazmadan verilerinizle etkileşim kurma başlayacağız. Ayrıca, yeni bir dil öğrenmeniz de gerekmez. Zaman Serisi Öngörüleri, SQL’e alışık olan ileri düzey kullanıcılar için ayrıntılı, metin tabanlı sorgulama olanağı sağlamanın yanı sıra, yeni başlayanlar için seçip tıklayarak gezinme seçeneği de sunar.

Bir IOT çözümündeki ve alanlar tanımlamak bir hatanın kök nedenini araştırmak için veri bilimi girişimlerini almak için DevOps gerçekleştirmede varlık ilgili sorunları hızlı bir şekilde tanılamak için müşteriler bu hızı avantajından yararlanmaya görüyoruz.  

TSI içinde depolanan verilerle etkileşim kurmak için başlıca üç yolu vardır:

1. Bizim görselleştirme, tüm verilerinizi tek bir yerde hızlı bir şekilde görselleştirmenizi sağlar Gezgini ilk ve kullanmaya başlamak kolay adıdır. Verilerinizi görsel olarak sayede anomalileri basit oluşturan ısı haritası gibi araçlar sağlar. Ayrıca, tek bir pano, bir veya daha fazla TSI ortamlarda dört görünümleri kadar tüm konumlar arasında zaman serisi verilerinin bir görünümünü verir karşılaştırmak hangi etkinleştirir perspektif görünüm sağlar. Daha fazla bilgi edinin [TSI Gezgini](./time-series-insights-update-explorer.md). TSI güncelleştirme ortamınızı planlamak için okuma [TSI güncelleştirme planlama](./time-series-insights-update-plan.md).

1. İkinci yol hızla güçlü grafikler ve graflar kendi web uygulamasına eklemek için JavaScript SDK'mız kullanmaktır. Yalnızca birkaç kod satırıyla, çizgi grafikler, pasta, çubuk grafikler, ısı Haritaları, veri kılavuzları ve daha fazlasını doldurmak için güçlü sorgular yazabilirsiniz. Tüm bu öğeleri,-SDK'sını kullanarak hazır mevcut. SDK ayrıca TSI sorgu API'leri, böylece bir Panoda göstermek istediğiniz verileri sorgulamak için SQL benzeri koşullar yazmak soyutlar. Karma sunu katmanı çözümler için sorunsuz bağlantı noktaları ile Gezgini'nde derin incelemeler için veri sağlayan parametreli URL'lerin TSI sunar. JavaScript SDK'sı hakkında daha fazla bilgi edinmek için [TSI JS istemci Kitaplığı](https://docs.microsoft.com/azure/time-series-insights/tutorial-explore-js-client-lib) ve [TSI istemci](https://github.com/Microsoft/tsiclient) belgeleri. Makalemizi parametreli URL'lerin hakkında daha fazla bilgi için okumaya [URL'leri parametreli](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-parameterized-urls).  

1. Son olarak, TSI TSI içinde depolanan verileri sorgulamak için güçlü bir API sağlar. TSI geçici işleçler gibi ilk ve son olarak, toplama ve dönüştürmeler ortalama, min, max, bölme ölçütü, sıralama ve DateHistogram gibi ve filtreleme gibi işleçler varsa, buna sahip ve, veya, büyüktür, REGEX, vs. Bu işleçler ilginç eğilimleri ve desenleri verilerinizi hızla bulmak aşağı akış uygulamaları etkinleştirmek ve anomalileri görselleştirmeleri ev yapımı doldurmak için kullanılabilir.  

## <a name="operational-analysis-and-driving-process-efficiency"></a>İşlem analizi ve işlem verimliliğini sağlama

Sistem durumu, kullanım ve performans ekipmanının ın uygun ölçekte, böylece işlem verimliliğini ölçmeye yönelik kolay bir yol sağlayarak izlemeyi etkinleştirin. Time Series Insights, öngörülemeyen çeşitli IoT iş yüklerini, alma ve sorgu performansından ödün vermeden yönetmenize yardımcı olur.

![genel bakış][2]

Başarıyla akış ve gelen işletimsel işlemlerden veri sürekli işleme tüm işletmeler sağa teknoloji/çözüm ile birlikte durumunda dönüştürebilirsiniz. Çoğunlukla bu çözümler, araştırma birden çok sistemleri birleşimi ve özel IOT bölge içinde sürekli olarak değişir ve yaygın bir düzen paylaşan veri analizini olur.

Bu düzenleri, genellikle milyarlarca olayı cihazlardan ve sensörlerden çeşitli yerel ayarlara yayılan alma etkin IOT platformları ile başlatın. Bu sistemler işleyebilir ve gerçek zamanlı anlayışlar ve Eylemler türetmek için akış verilerini, verileri neredeyse gerçek zamanlı ve toplu analiz sıcak ve soğuk depolama için genellikle arşivlenir. Toplanan verileri temizlemek ve aşağı akış sorgulama ve analiz senaryoları için bağlama göre ele alınmasına işleme dizi geçer. Microsoft Azure, bu IOT senaryoları (varlık bakım, üretim, vb.) uygulanabilir zengin hizmetleri sunar. Bunlar, Azure TSI, Azure IOT Hub, Azure Event Hubs, Azure Stream Analytics, Azure işlevleri, Azure Logic Apps, Azure Databricks, Azure Machine Learning ve Microsoft Power BI içerir.

Bu çözüm mimarisi ulaşılabilecek gibi – sınıfının en iyisi güvenlik, aktarım hızı ve gecikme süresi için Azure IOT Hub veya Azure olay hub'ı aracılığıyla veri alma. Belirli veri işleme gereksinimlerine bağlı olarak Azure Stream Analytics, Azure Logic Apps, Azure işlevleri gibi hizmetler aracılığıyla alınan verileri funneling tarafından veri işleme ve hesaplamalar gerçekleştirin. Hesaplanan sinyal işleme ardışık düzendeki, depolamak ve analiz için Azure TSI itilir. Azure Time Series Insights, geçmiş veriler üzerinde gerçek zamanlı bir veri keşfi ve varlık temelli öngörüleri sunar. İş gereksinimlerine bağlı olarak, zaman serisi öngörüleri için HDInsight Time Series Insights'ı bağlayarak depolanan veriler MapReduce ve Hive işlerini yeniden çalıştırabilirsiniz. Time Series Insights içinde depolanan verileri Power BI ve diğer Time Series Insights genel yüzey sorgu API'leri ayrıntılı iş ve operasyonel zeka senaryoları için aracılığıyla müşteri uygulamaları için kullanılabilir hale getirilebilir.

## <a name="advanced-analytics"></a>Gelişmiş Analiz

Azure Machine Learning ve Azure Databricks gibi gelişmiş Analiz Hizmetleri ile tümleştirin. Time Series Insights, milyonlarca cihazdan ham veri girişi yapar ve bir Azure analiz hizmetleri paketi ile sorunsuzca tüketilebilecek bağlam verileri ekler.

![analizler][3]

Gelişmiş analiz ve makine öğrenimi kullanan ve büyük hacimli veri odaklı kararlar ve Tahmine dayalı analiz gerçekleştirmek için verileri işleyebilirsiniz. IOT kullanım durumlarında, Gelişmiş analiz algoritmaları milyonlarca, verileri birden çok kez saniyede iletebildiği cihazından toplanan verilerden bilgi edinin. Ancak, IOT cihazlarından toplanan verileri ham ve cihaz, sensör okuma vb., bu nedenle verilerin Gelişmiş analiz için doğrudan kullanılması zorlaştıran birimi konumu gibi bağlamsal bilgi içermemektedir.

Azure Time Series Insights, iki basit ve ekonomik şekilde IOT veri ve Gelişmiş analiz arasındaki boşluk arasında köprü. İlk olarak, zaman serisi görüşleri güncelleştirme milyonlarca IOT hub'ı kullanarak CİHAZDAN ham telemetri verileri toplar, bağlamsal bilgiler verilerle zenginleştirir ve ', Gelişmiş Analiz Hizmetleri bir dizi gibi kolayca tümleştirebilirsiniz parquet biçiminde' verileri dönüştürür Azure Machine Learning, Azure Databricks ve diğer üçüncü taraf uygulamaları.  Time Series Insights kaynak-ın-gerçekte tüm veriler için bir kuruluş genelindeki, böylece aşağı akış analizi için merkezi bir depo kullanmak için iş yükleri oluşturma görebilir.  Time Series Insights itibaren olan neredeyse gerçek zamanlı bir depolama hizmeti, Gelişmiş analiz modelleri daha doğru tahminler elde etmeye gelen IOT telemetri verilerini sürekli olarak bilgi edinin.

İkinci olarak, zaman serisi görüşleri çıktıyı görselleştirmek ve bunların sonuçlarını depolamak için makine öğrenme ve tahmin modellerinin bu nedenle kuruluşların en iyi duruma getirmek ve modellerini ince yardımcı veri.  Bunun da ötesinde, zaman serisi görüşleri anomalileri saptayın ve düzenlerini belirleyen veri bilimi ekipleri yardımcı olmak için eğitilen modeli çıkarır gibi aynı düzlemde ilişkin telemetri verilerini akış görselleştirmek kolaylaştırır.  

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [TSI Gezgini](./time-series-insights-update-explorer.md).

Ortamınızın planlamak için okuma [TSI (Önizleme) planlama](./time-series-insights-update-plan.md).

Okuma [TSI istemci](https://github.com/Microsoft/tsiclient) belgeleri.

<!-- Images -->
[1]: media/v2-update-use-cases/data-explorer.svg
[2]: media/v2-update-use-cases/overview.svg
[3]: media/v2-update-use-cases/advanced-analytics.svg
