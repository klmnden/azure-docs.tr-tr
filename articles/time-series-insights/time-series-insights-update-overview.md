---
title: Azure Time Series Insights genel bakış | Microsoft Docs
description: Azure Time Series Insights genel bakış
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: anshan
ms.workload: big-data
ms.topic: overview
ms.date: 12/05/2018
ms.openlocfilehash: 9843a01ed3c96b362e17718e9035c378da6c3cf2
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53083402"
---
# <a name="azure-time-series-insights-overview"></a>Azure Time Series Insights genel bakış

Azure zaman serisi öngörüleri (TSI) bir uçtan uca hizmet olarak Platform-A-veri alabilen, işlem, depolama ve geçici veri keşfi, yanı sıra operasyonel analiz için ideal olan, yüksek oranda contextualized, zaman serisi iyileştirilmiş IOT ölçekli veriler sorgu için olan. Azure TSI karşıladığını endüstriyel IOT dağıtımları geniş gerektiğini sunan bir benzersiz şekilde genişletilebilir ve özelleştirilmiş hizmetidir.

## <a name="video"></a>Video

Bu videoda, size Azure Time Series Insights (Önizleme), bulut tabanlı IOT analiz platformu için genel bir bakış sağlar.

> [!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Azure-Time-Series-Insights-e2e-solution-for-industrial-IoT-analytics/player]

## <a name="defining-iot-data"></a>IOT verilerini tanımlama

IOT, herhangi bir "endüstriyel" veri varlığı yoğun kuruluşlarda kullanılabilir verilerdir. Sıcaklık, hareket, nem gibi oldukça gürültülü ölçümleri kayıt varlıklar aralığındaki geniş göndermesinden itibaren geçen IOT verilerini genellikle yüksek oranda yapılandırılmamış. Ayrıca, bu veri akışlarını Sık önemli boşlukları, bozuk bir ileti ve false okumalar tarafından belirlenir. Herhangi bir çözümlemesi gerçekleşebilmesi bu akıştaki verilerin temizlenmesi gerekir. IOT verilerini genellikle yalnızca CRM veya ERP gibi birinci taraf geldiğini ek verisi girişleri bağlamında anlamlı) veya üçüncü taraf veri kaynaklarından (örneğin, hava durumu veya konum).

Sonuç olarak, bu verileri yalnızca göz ardı edilebilir avantajlarının işlem ve işletme amacıyla kullanılır. Bu tür veriler iş raporlama ve analiz için tutarlı, kapsamlı, geçerli ve doğru bilgileri sağlar. Durumun dönüş IOT toplanan verileri eyleme dönüştürülebilir içgörüler haline, bu nedenle birkaç önemli özellikleri gerektirir:

* Veri işleme, temizlemek için filtre, enterpolasyon, dönüştürme ve verileri analiz için hazırlar
* Gidin ve verileri (normalleştirmek için veri bağlama göre ele alınmasına) anlamak için yapı
* İşlenen (türetilmiş) de gibi ham verileri yıllardır için uzun / sınırsız bekletme için uygun maliyetli depolama

Tipik bir IOT veri akışı aşağıdaki gibi konabilecek:

  ![IOT veri akışı][1]

## <a name="azure-time-series-insights-for-industrial-iot"></a>Endüstriyel IOT için Azure Time Series Insights

Geçerli IOT yatay farklı. Bu, üretim, otomotiv kapsayan müşteriler, enerji, yardımcı programlar, akıllı Binalar ve danışmanlık sektörler içerir. Geçici veri keşfi verinin şeklini bilinmeyen yanı sıra operasyonel analiz Operasyonel Verimliliği için şema (açıkça Modellenen) veriler üzerinde olduğu senaryolar içerir. Bu senaryolar, genellikle yan yana var ve farklı kullanım durumlarına destekler. Çok katmanlı depolama (sıcak ve soğuk) teknolojilerinden değerinde zaman serisi verileri depolama olanağı ve açıkça model ve varlık tabanlı operasyonel zeka sorgularını en iyi duruma getirme olanağı olma gibi platform özelliklerinden başarısı için anahtar Endüstriyel IOT kuruluşlara ve kendi dijital revolution.

Azure TSI, hem IOT veri keşfi, aynı zamanda operasyonel içgörüler için kapsamlı bir uçtan uca hizmet olarak Platform-A teklifidir. TSI IOT ölçekli zaman serisi verilerini analiz etmek için tam olarak yönetilen bulut hizmeti sunar.

Müşteriler ham verileri şemasız, bellek içi, bir depolama alanında depolayabilirsiniz. Müşteriler bir dağıtılmış sorgu altyapısı ile etkileşimli geçici sorgular daha sonra gerçekleştirebilirsiniz ve milyarlarca olayı saniyeler içinde görselleştirmek için API yararlanarak zengin kullanıcı deneyimi. Daha fazla bilgi edinin bizim [veri keşfi özellikleri](./time-series-insights-overview.md).

TSI, ayrıca şu anda önizlemede operasyonel içgörüler özellikleri sunar. TSI, etkileşimli veri keşfi ve operasyonel zeka ile birlikte IOT varlıklarından toplanan verilerinizden daha fazla değer türetmek müşterilerin sağlar. Özellikle, Önizleme Teklifi aşağıdaki anahtar özelliklerini destekler:

* Bir ölçeklenebilir, performans ve saniyeler içinde zaman serisi verilerini değerinde eğilim yıl için bulut tabanlı bir IOT çözümü sağlayan maliyet açısından iyileştirilmiş zaman serisi veri deposu.
* Varlıklardan ve cihazlardan alınan türetilmiş ve türetilmemiş sinyallerle ilişkili etki alanını ve meta verileri tanımlamak için semantik model desteği.
* Varlık tabanlı veri öngörüleri, iş ve operasyonel zeka kullanımını zengin analizlerle, geçici verileri birleştiren bir gelişmiş kullanıcı deneyimi
* Gelişmiş makine öğrenimi ve Azure Databricks, Apache Spark, Azure Machine Learning, Jupyter Notebooks'u, Power BI gibi analiz araçları ile tümleştirme vb. müşterilerin zaman serisi verilerini sorunlarını gidermek ve Operasyonel Verimliliği artırmaya yardımcı olmak için.

Fiyatlandırma modeli için veri işleme, depolama ve sorgu, böylece müşteriler, değişen iş gereksinimlerine uygun olarak çok daha ölçeklenebilir bir model sağlayan bir basit Kullandıkça Öde ile birlikte operasyonel içgörüler ve veri araştırması sunulur.

Güncelleştirilmiş özelliklerini gösteren bir üst düzey veri akış diyagramı aşağıdadır:

  ![Temel işlevler][2]

Bu anahtar endüstriyel IOT özelliklerini sunulmasıyla birlikte, Azure TSI aşağıdaki faydaları sağlar:

| | |
| ---| ---|
| **IOT ölçekli zaman serisi verilerinin çok katmanlı depolama** | Veri almak için bir genel veri işleme işlem hattı, etkileşimli sorgular için Orta Gecikmeli depolama ve/veya soğuk depolama, büyük hacimli verileri depolamak için özelliği deposu veri müşterileri sahiptir. Müşterilerin, yüksek performanslı, varlık tabanlı avantajlarından faydalanabilirsiniz [sorguları](./time-series-insights-update-tsq.md). |
| **Ham telemetri contextualizing ve varlık dayalı Öngörüler türetme için zaman serisi modeli** | Müşteriler bağlama göre ele alınmasına ham telemetri verileri tanımlayıcı ile [zaman serisi modeli](./time-series-insights-update-tsm.md) ve yüksek oranda cihaz tabanlı sorgular performans ve maliyet iyileştirme ile zengin operasyonel zeka türetilir. |
| **Diğer veri çözümleriyle sorunsuz tümleştirme** | Azure TSI içindeki veriler olduğundan [depolanan](./time-series-insights-update-storage-ingress.md) açık kaynaklı Apache Parquet dosyalarında müşterileri kolayca diğer veri çözümleriyle tümleştirilebilir (ilk ya da üçüncü taraf) iş zekası dahil olmak üzere, uçtan uca senaryolar için Gelişmiş makine Machine Learning, Tahmine dayalı analiz, vs. |
| **Gerçek zamanlı veri araştırması** | [Azure TSI Gezgini](./time-series-insights-update-explorer.md) görselleştirme tüm veri alımı komut zinciri ile akış için kullanıcı deneyimi sunar. Kısa bir süre içinde bir olay kaynağı bağladıktan sonra müşteriler görüntüleyebilir, keşfedin ve olay verilerini bir cihaz verileri beklenen şekilde yayma ve bir IOT varlık sistem durumu, üretkenlik ve genel verimliliğini izlemeye olup olmadığını doğrulamak için kullanılan sorgu. |
| **Kök neden analizini ve anormallik algılama** | [Azure TSI Gezgini](./time-series-insights-update-explorer.md) gerçekleştirin ve kaydetmek çok adımlı, kök neden analizi için desen hem perspektif görünüm destekler. Azure Stream Analytics ile birlikte, müşteriler Azure TSI uyarıları ve neredeyse gerçek zamanlı olarak anomalileri algılamak için kullanabilirsiniz. |
| **TSI platforma özel uygulamalar oluşturun** | Azure TSI destekler [JavaScript SDK'sı](./tutorial-explore-js-client-lib.md). SDK, zengin denetimleri ve sorguları yönelik Basitleştirilmiş erişim sağlar. SDK'sı, müşterilerin belirli iş ihtiyaçlarını karşılamak için özel IOT uygulamaları Azure TSI üzerine yapı sağlar. Müşteriler ayrıca Azure TSI kullanın [sorgu API'leri](./time-series-insights-update-tsq.md) doğrudan sürücü verilerini özel IOT uygulamaları için. |

## <a name="next-steps"></a>Sonraki adımlar

Azure TSI (Önizleme) kullanmaya başlamak hazır olursunuz:

> [!div class="nextstepaction"]
> [Hızlı Başlangıç kılavuzunu okuyun](./time-series-insights-update-quickstart.md)

Kullanım örnekleri hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [Azure TSI kullanım örnekleri](./time-series-insights-update-use-cases.md)

<!-- Images -->
[1]: media/v2-update-overview/overview_one.png
[2]: media/v2-update-overview/overview_two.png