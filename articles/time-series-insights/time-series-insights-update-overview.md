---
title: 'Genel Bakış: Azure Time Series Insights Önizleme | Microsoft Docs'
description: Azure zaman serisi İçgörüler Önizleme genel bakış.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: anshan
ms.workload: big-data
ms.topic: overview
ms.date: 12/05/2018
ms.custom: seodec18
ms.openlocfilehash: fbf347ceb3ccae4802c984a2737c2298a4e43e72
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63760099"
---
# <a name="azure-time-series-insights-preview-overview"></a>Azure zaman serisi İçgörüler Önizleme genel bakış

Azure zaman serisi öngörüleri Önizleme uçtan uca hizmet olarak platform teklifidir. İçe alma, işlem, depolama ve yüksek oranda contextualized, zaman serisi iyileştirilmiş IOT ölçekli veri sorgulamak için kullanılır. Time Series Insights, geçici veri keşfi ve operasyonel analiz için idealdir. Time Series Insights karşıladığını endüstriyel IOT dağıtımları geniş gerektiğini sunan bir benzersiz şekilde genişletilebilir ve özelleştirilmiş hizmetidir.

## <a name="video"></a>Video

Bu videoda, Azure zaman serisi öngörüleri önizlemesi, bulut tabanlı IOT analiz platformu için genel bir bakış sunuyoruz.

> [!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Azure-Time-Series-Insights-e2e-solution-for-industrial-IoT-analytics/player]

## <a name="define-iot-data"></a>IOT verilerini tanımlayın

IOT verilerini varlık yoğun kuruluşlarda kullanılabilir olan tüm "endüstriyel" verilerdir. Oldukça gürültülü ölçümleri kayıt varlıklarından gönderildiği IOT verilerini genellikle yüksek oranda yapılandırılmamış. Bu ölçümler, sıcaklık, hareket ve nem içerir. Bu veri akışlarını Sık önemli boşlukları, bozuk bir ileti ve false okumalar tarafından belirlenir. Herhangi bir çözümlemesi gerçekleşebilmesi bu akıştaki verilerin temizlenmesi gerekir. IOT verilerini genellikle yalnızca CRM veya ERP gibi birinci taraf kaynaklardan gelen ek verisi girişleri bağlamında anlamlı olur. Giriş, ayrıca hava durumu veya konumu gibi üçüncü taraf veri kaynaklarından gelir.

Sonuç olarak, veriler yalnızca bir bölümünü işlem ve işletme amacıyla kullanılır. Bu tür veriler iş raporlama ve analiz için tutarlı, kapsamlı, geçerli ve doğru bilgileri sağlar. Durumun dönüş verileri eyleme dönüştürülebilir içgörüler haline gerektirir IOT toplanır:

* Veri işleme, temizlemek için filtre enterpolasyon dönüştürme ve verileri analiz için hazırlar.
* Gidin ve diğer bir deyişle verileri anlamak için Normalleştir ve veri bağlama göre ele alınmasına yapısı.
* Yıllık işlenen ya da türetilmiş değerinde, veri ve ham verileri uzun veya sınırsız bekletme için uygun maliyetli depolama sağlar.

Tipik bir IOT veri akışı aşağıdaki resimde gösterilmektedir.

  ![IOT veri akışı][1]

## <a name="azure-time-series-insights-for-industrial-iot"></a>Endüstriyel IOT için Azure Time Series Insights

Geçerli IOT yatay farklı. Müşteriler, üretim, otomotiv, enerji, yardımcı programlar, akıllı Binalar ve danışmanlık sektörler yayılır. Geçici veri keşfi verinin şeklini bilinmeyen olduğu senaryolar içerir. Senaryoları operasyonel analiz Operasyonel Verimliliği için şema veya açıkça Modellenen veriler üzerinde de içerir. Bu senaryolar, genellikle yan yana var ve farklı kullanım durumlarına destekler. Endüstriyel IOT kuruluşlara ve kendi dijital revolution başarısı için önemli olduğu platform özellikleri şunlardır:

- Çok katmanlı depolama, hem sıcak ve soğuk. 
- Zaman serisi verilerini tutarında yıllık Depolama olanağı. 
- Açıkça model ve varlık tabanlı operasyonel zeka sorgularını en iyi duruma getirme olanağı.

Time Series Insights bir kapsamlı, uçtan uca olarak-a-IOT veri keşfi ve operasyonel içgörüler için hizmet olarak platform teklifidir ' dir. Zaman serisi görüşleri, IOT ölçekli zaman serisi verilerini analiz etmek için tam olarak yönetilen bulut hizmeti sunar.

Ham veri bir şemasız, bellek içi depolama alanında depolayabilirsiniz. Ardından, bir dağıtılmış sorgu altyapısı ve API etkileşimli geçici sorgular gerçekleştirebilirsiniz. Olun milyarlarca olayı saniyeler içinde görselleştirin için zengin kullanıcı deneyimini kullanın. Daha fazla bilgi edinin [veri keşfi özellikleri](./time-series-insights-overview.md).

Time Series Insights şu anda önizlemede operasyonel içgörüler özellikleri de sunar. Etkileşimli veri keşfi ve operasyonel zeka ile birlikte IOT varlıklarından toplanan verilerinizden daha fazla değer türetmek için Time Series Insights'ı kullanabilirsiniz. Önizleme Teklifi destekler:

* Bir ölçeklenebilir, performans ve maliyet açısından iyileştirilmiş zaman serisi verilerini depolar. Bu bulut tabanlı bir IOT çözümü, saniyeler içinde zaman serisi verilerini tutarında yıllık eğilim.
* Etki alanı ve varlıkları ve cihazlardan türetilmiş ve türetilmeyen sinyal ile ilişkili meta verileri tanımlayan anlam modeli desteği.
* Varlık tabanlı veri öngörüleri birleştiren zengin ve geçici data analytics ile geliştirilmiş bir kullanıcı deneyimi. Bu birleşim, iş ve operasyonel zeka sürücüleri.
* Gelişmiş makine öğrenme ve analiz araçları ile tümleştirme. Azure Databricks, Apache Spark, Azure Machine Learning, Jupyter not defterleri ve Power BI araçları içerir. Bu araçlar zaman serisi verilerini sorunlarını gidermek ve Operasyonel Verimliliği artırmaya yardımcı olur.

Birlikte, operasyonel içgörüler ve veri keşfi, basit bir Kullandıkça Öde fiyatlandırma modeline veri işleme, depolama ve sorgu ile sunulmaktadır. Bu faturalama modeli, değişen iş gereksinimlerinize uygun.

Bu üst düzey veri akış diyagramı güncelleştirmeleri gösterir.

  ![Temel işlevler][2]

Bu anahtar endüstriyel IOT özelliklerini sunulmasıyla birlikte, zaman serisi görüşleri aşağıdaki faydaları sağlar.

| | |
| ---| ---|
| **IOT ölçekli zaman serisi verilerinin çok katmanlı depolama** | Veri alma için ortak veri işleme sahip bir işlem hattı, etkileşimli sorgular için Orta Gecikmeli depolama birimindeki verileri depolayabilirsiniz. Veri soğuk depolama için büyük miktarda veriyi depolayabilirsiniz. Yüksek performanslı varlık tabanlı avantajlarından yararlanmak [sorguları](./time-series-insights-update-tsq.md). |
| **Zaman serisi modeli, ham telemetri bağlama göre ele alınmasına ve varlık tabanlı içgörülere sahip olun** | Ham telemetri verileri tanımlayıcı ile bağlama göre ele alınmasına [zaman serisi modeli](./time-series-insights-update-tsm.md). Zengin operasyonel zeka ile yüksek oranda cihaz tabanlı sorgular performans ve maliyet iyileştirme türetilir. |
| **Diğer veri çözümleriyle sorunsuz ve sürekli tümleştirme** |  Time Series Insights veri [depolanan](./time-series-insights-update-storage-ingress.md) açık kaynaklı Apache Parquet dosyalarını içinde. Bu tümleştirme sayesinde diğer veri çözümleri ilk ya da üçüncü taraf uçtan uca senaryolar için kolay olup olmadığını. Bu senaryolar, iş zekası, Gelişmiş makine öğrenimi ve Tahmine dayalı analiz içerir. |
| **Gerçek zamanlı veri araştırması** | [Azure zaman serisi öngörüleri önizlemesi Gezgini](./time-series-insights-update-explorer.md) görselleştirme tüm veri alımı komut zinciri ile akış için kullanıcı deneyimi sunar. Kısa bir süre içinde bir olay kaynağı bağlandıktan sonra görüntülemek keşfedin ve olay verileri sorgulayın. Bu şekilde, bir cihaz beklendiği gibi veri yayan olup olmadığını doğrulayabilirsiniz. Bir IOT varlık sistem durumu, üretkenlik ve genel verimliliğini de izleyebilirsiniz. |
| **Kök neden analizini ve anormallik algılama** | [Azure zaman serisi öngörüleri önizlemesi Gezgini](./time-series-insights-update-explorer.md) gerçekleştirin ve çok adımlı kök analiz neden deseni hem perspektif görünüm destekler. Azure Stream Analytics ile birlikte, neredeyse gerçek zamanlı uyarılar ve anomalileri algılamak için Time Series Insights'ı kullanabilirsiniz. |
| **Time Series Insights platformunu temel alan özel uygulamalar** | Zaman serisi görüşleri destekler [JavaScript SDK'sı](./tutorial-explore-js-client-lib.md). SDK, zengin denetimleri ve sorguları yönelik Basitleştirilmiş erişim sağlar. İş gereksinimlerinize uyacak şekilde özel IOT uygulamaları Time Series Insights üzerine oluşturmak için SDK'yi kullanın. Time Series Insights ayrıca kullanabileceğiniz [sorgu API'leri](./time-series-insights-update-tsq.md) doğrudan sürücü verilerini özel IOT uygulamaları için. |

## <a name="next-steps"></a>Sonraki adımlar

Azure zaman serisi öngörüleri önizlemesi ile çalışmaya başlayın:

> [!div class="nextstepaction"]
> [Hızlı Başlangıç kılavuzunu okuyun](./time-series-insights-update-quickstart.md)

Kullanım örnekleri hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [Azure zaman serisi öngörüleri Önizleme kullanım örnekleri](./time-series-insights-update-use-cases.md)

<!-- Images -->
[1]: media/v2-update-overview/overview_one.png
[2]: media/v2-update-overview/overview_two.png