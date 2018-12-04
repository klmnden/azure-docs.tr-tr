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
ms.date: 11/27/2018
ms.openlocfilehash: 9d9fab9f0a515cacdf2a1425c4da06c9e3d4c364
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52857000"
---
# <a name="azure-time-series-insights-preview-use-cases"></a>Azure Time Series Insights (Önizleme) kullanım örnekleri

Bu makalede, Azure zaman serisi öngörüleri (TSI) için bazı ortak kullanım durumları için genel bir bakış sağlar. Uygulamalar ve çözümlerle TSI geliştirirken bu makaledeki önerileri bir başlangıç noktası olarak hizmet.

Bu makaleyi okuduktan sonra aşağıdaki soruları yanıtlamak mümkün olacaktır:

* Azure TSI için yaygın kullanım örnekleri nelerdir?
* Veri keşfi ve görsel anomali algılama için Azure TSI kullanmanın avantajları nelerdir?
* Azure TSI operasyonel analiz ve süreç verimliliği için kullanmanın avantajları nelerdir?
* Gelişmiş analiz için Azure TSI kullanmanın avantajları nelerdir?

Bu belge, Azure zaman serisi öngörüleri özel önizlemesi için tasarlanmış kullanım örnekleri için genel bir bakış sağlar.

## <a name="data-exploration-and-visual-anomaly-detection"></a>Veri keşfi ve görsel anomali algılama

Anında keşfedin ve milyarlarca olayı anormallikleri için analiz ve veri gizli eğilimleri keşfedin. Neredeyse gerçek zamanlı performans, IOT ve DevOps analiz iş yükleri için Azure TSI sunar.

![Veri Gezgini][1]

Tüm Azure TSI'ın gücü müşterilerin çoğu zaman görüşleri arasında güçlü olduğunu kabul etmiş olursunuz. TSI, veriler için ön hazırlık gerektirir ve dakikalar içinde milyarlarca olaya Azure IOT hub'ı veya olay hub'ına bağlanma hızlı çalışır.  Bağlandıktan sonra görselleştirin ve milyarlarca olayı anormallikleri için analiz edin ve verilerinizi gizli eğilimleri keşfedin.  TSI, sezgisel ve kullanması kolay olduğundan, tek satırlık bir kod yazmadan verilerinizle etkileşim kurma başlayacağız. Ayrıca, yeni bir dil öğrenmeniz de gerekmez. Zaman Serisi Öngörüleri, SQL’e alışık olan ileri düzey kullanıcılar için ayrıntılı, metin tabanlı sorgulama olanağı sağlamanın yanı sıra, yeni başlayanlar için seçip tıklayarak gezinme seçeneği de sunar.

Bir IOT çözümündeki ve alanlar tanımlamak bir hatanın kök nedenini araştırmak için veri bilimi girişimlerini almak için DevOps gerçekleştirmede varlık ilgili sorunları hızlı bir şekilde tanılamak için müşteriler bu hızı avantajından yararlanmaya görüyoruz.  

TSI içinde depolanan verilerle etkileşim kurmak için başlıca üç yolu vardır:

1. Bizim görselleştirme, tüm verilerinizi tek bir yerde hızlı bir şekilde görselleştirmenizi sağlar Gezgini ilk ve kullanmaya başlamak kolay adıdır. Bu sayede perspektif görünüm yanı sıra verilerinizi görsel olarak sayede anomalileri basit hale getirmek ısı haritası gibi araçları arasında zaman serisi verilerinin bir görünümünü verir, tek bir pano, bir veya daha fazla TSI ortamlarda en fazla dört görünümleri karşılaştırın sağlar Tüm konumlarınıza. Daha fazla bilgi edinin [TSI Gezgini](./time-series-insights-update-explorer.md). TSI güncelleştirme ortamınızı planlamak için okuma [TSI güncelleştirme planlama](./time-series-insights-update-plan.md).

1. İkinci yol hızla güçlü grafikler ve graflar kendi web uygulamasına eklemek için JavaScript SDK'mız kullanmaktır. Yalnızca birkaç kod satırıyla, çizgi grafikler, pasta, çubuk grafikler, ısı Haritaları, veri kılavuzları ve daha fazlasını doldurmak için güçlü sorgular yazabilirsiniz. Tüm bu öğeleri,-SDK'sını kullanarak hazır mevcut. SDK ayrıca TSI sorgu API'leri, böylece bir Panoda göstermek istediğiniz verileri sorgulamak için SQL benzeri koşullar yazmak soyutlar. Karma sunu katmanı çözümler için sorunsuz bağlantı noktaları ile Gezgini'nde derin incelemeler için veri sağlayan parametreli URL'lerin TSI sunar. JavaScript SDK'sı hakkında daha fazla bilgi edinmek için [TSI JS istemci Kitaplığı](https://docs.microsoft.com/azure/time-series-insights/tutorial-explore-js-client-lib) ve [TSI istemci](https://github.com/Microsoft/tsiclient) belgeleri. Makalemizi parametreli URL'lerin hakkında daha fazla bilgi için okumaya [URL'leri parametreli](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-parameterized-urls).  

1. Son olarak, TSI TSI içinde depolanan verileri sorgulamak için güçlü bir API sağlar. TSI geçici işleçler gibi ilk ve son olarak, toplama ve dönüştürmeler ortalama, min, max, bölme ölçütü, sıralama ve DateHistogram gibi ve filtreleme gibi işleçler varsa, buna sahip ve, veya, büyüktür, REGEX, vs. Bu işleçler ilginç eğilimleri ve desenleri verilerinizi hızla bulmak aşağı akış uygulamaları etkinleştirmek ve anomalileri görselleştirmeleri ev yapımı doldurmak için kullanılabilir.  

IOT için Azure teklifleri hakkında daha fazla bilgi için bkz. [kendi nesnelerin interneti oluşturma](https://www.microsoft.com/internet-of-things).

## <a name="operational-analysis-and-driving-process-efficiency"></a>Operasyonel analiz ve sürüş süreç verimliliği

Sistem durumu, kullanım ve performans ekipmanının ın uygun ölçekte, böylece işlem verimliliğini ölçmeye yönelik kolay bir yol sağlayarak izlemeyi etkinleştirin. Time Series Insights, alma ve sorgu performansından ödün vermeden çeşitli ve öngörülemez IOT iş yüklerine yönetmenize yardımcı olur.

![genel bakış][2]

Başarıyla akış ve gelen işletimsel işlemlerden veri sürekli işleme tüm işletmeler sağa teknoloji/çözüm ile birlikte durumunda dönüştürebilirsiniz. Çoğunlukla bu çözümleri, özel IOT bölge içinde sürekli olarak değişen verileri analiz ve araştırma birden çok sistemleri bir birleşimini olur. Bu sistemler birlikte ışık, içe alma işlemi, söz konusu olduğunda yaygın bir düzen paylaşma senaryolarını'kurmak depolamak, çözümlemek ve IOT verilerini görselleştirin.

Sistemleri milyarlarca olayı cihazlardan ve sensörlerden çeşitli yerel ayarlara yayılan alma gerek çözümün bir parçası. Ardından, bu sistemler işleyebilir ve gerçek zamanlı Öngörüler akış verilerini analiz edin. Verileri neredeyse gerçek zamanlı ve toplu analiz sıcak ve soğuk depolama için ardından arşivlenir.

Ardından, işlemi etkinleştirmek için toplanan verilerini temizlemek ve aşağı akış sorgulama ve analiz senaryoları etkinleştirmek için veri depolama sırasında contextualization, bu sistemler gerekir. Microsoft Azure, Azure TSI, Azure IOT Hub, Azure Event Hubs, Azure Stream Analytics, Azure işlevleri, Azure Logic Apps, Azure Databricks, Azure Machine Learning ve Microsoft Power BI dahil olmak üzere bu IOT senaryoları için uygulanabilir zengin hizmetleri sunar.

İle düşük gecikme süresi yüksek aktarım hızı veri alma olanağı sağlar olarak yukarıdaki çözümü kurulumu ile Azure IOT veya olay hub'ları veri aktarılabilir. Azure Stream Analytics, Azure Logic Apps ve Azure işlevleri için gerçek zamanlı öngörülere işlenmesi gereken alınan verileri funneled. Sonucu sonra Power BI için gerçek zamanlı yönelik Kompozit beslenir, hem de geçmiş dengeli dağıtım için karşılaştırma izleme ve uyarı Azure Time Series Insights yüklenebilir. Veri keşfi, gereken alınan verileri neredeyse gerçek zamanlı veya doğrudan Azure Time Series Insights için geçmiş eğilimleri belirleme için sorgulama geçici yüklenebilir. Yüklenen verileri operasyonel analiz ve işlemleri en yüksek verimlilik için en iyi duruma getirme analytics için sınırsız geçmiş verileriyle birlikte Sorgulanacak hazırdır. Tüm veriler veya yalnızca yüklenen verilerde yapılan en son başvuru verileri gerçek zamanlı analiz parçası olarak kullanılabilir. Ayrıca, veriler daha fazla iyileştirilmektedir ve HDInsight için Azure Time Series Insights verilere bağlanma Map/Reduce, Hive, vb. işleri tarafından işlenen. Son olarak bu veriler Power bı'da ve ortak müşterilerimizin yüzey sorgu API'leri aracılığıyla herhangi bir müşteri uygulama kullanımına.

## <a name="advanced-analytics"></a>Gelişmiş Analiz

Azure Machine Learning ve Azure Databricks gibi gelişmiş Analiz Hizmetleri ile tümleştirin. TSI ingresses ham verileri, milyonlarca cihaz ve sorunsuz bir şekilde Azure Analiz Hizmetleri paketi tarafından tüketilebilecek bağlamsal veriler ekler.

![analizler][3]

Gelişmiş analiz ve makine öğrenimi kullanan ve büyük hacimli veri odaklı kararlar ve Tahmine dayalı analiz gerçekleştirmek için verileri işleyebilirsiniz. IOT kullanım durumlarında, Gelişmiş analiz algoritmaları milyonlarca, verileri birden çok kez saniyede iletebildiği cihazından toplanan verilerden bilgi edinin. Ancak, IOT cihazlarından toplanan verileri ham ve cihaz, sensör okumaya vb. birimi konumu gibi bağlamsal bilgi içermemektedir. Bu veriler, Gelişmiş analiz için doğrudan tüketilemiyor.

Azure TSI, basit ve uygun maliyetli bir şekilde IOT veri ve Gelişmiş analiz arasındaki boşluk arasında köprü. TSI milyonlarca CİHAZDAN ham telemetri verileri toplar, bağlamsal bilgiler verilerle zenginleştirir ve veri 'birkaç Azure Gelişmiş Analiz Hizmetleri gibi Azure Machine Learning, Azure ile kolayca tümleştirilebilir parquet biçimi' dönüştürür DataBricks ve kendi üçüncü taraf uygulamaları. Gelişmiş analiz modelleri daha doğru tahminler elde etmeye gelen IOT telemetri verilerini sürekli olarak öğrenebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [TSI Gezgini](./time-series-insights-update-explorer.md).

* Ortamınızın planlamak için okuma [TSI (Önizleme) planlama](./time-series-insights-update-plan.md).

* Okuma [TSI istemci](https://github.com/Microsoft/tsiclient) belgeleri.

<!-- Images -->
[1]: media/v2-update-use-cases/data-explorer.png
[2]: media/v2-update-use-cases/overview.png
[3]: media/v2-update-use-cases/advanced-analytics.png
