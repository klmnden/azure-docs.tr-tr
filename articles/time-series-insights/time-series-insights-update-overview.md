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
ms.date: 11/28/2018
ms.openlocfilehash: 5400d8eeba1983d3137b5145c2f885faee036417
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52853152"
---
# <a name="azure-time-series-insights-overview"></a>Azure Time Series Insights genel bakış

Azure zaman serisi öngörüleri (TSI) bir uçtan uca hizmet olarak Platform-A-alma, işlem, depolama ve yüksek oranda contextualized, zaman serisi iyileştirilmiş IOT ölçekli veri sorgulamak için ' dir. Bu nedenle, Azure TSI operasyonel analiz yanı sıra geçici veri keşfi için idealdir. TSI karşıladığını endüstriyel IOT dağıtımları geniş gerektiğini sunan benzersiz şekilde genişletilebilir, özelleştirilmiş, hizmetidir.

## <a name="what-is-iot-data"></a>IOT verilerini nedir?

IOT, herhangi bir 'endüstriyel' veri varlık açısından yoğun kaynak gerektiren kuruluşlarda kullanılabilir verilerdir.
Sıcaklık, hareket, nem gibi oldukça gürültülü ölçümleri kayıt varlıklar aralığındaki geniş göndermesinden itibaren geçen IOT verilerini genellikle yüksek oranda yapılandırılmamış.

Ayrıca, bu veri akışlarını Sık önemli boşlukları, bozuk bir ileti ve false okumalar tarafından belirlenir. Herhangi bir çözümlemesi gerçekleşebilmesi bu akıştaki verilerin temizlenmesi gerekir. IOT verilerini anlamlı yalnızca ilk-(örneğin, CRM veya ERP) veya üçüncü taraf gelen ek verisi girişleri bağlamında genellikle veri kaynakları (örneğin, hava durumu veya konum).

Sonuç olarak, bu verileri yalnızca göz ardı edilebilir avantajlarının işlem ve işletme amacıyla kullanılır. Bu tür veriler iş raporlama ve analiz için tutarlı, kapsamlı, geçerli ve doğru bilgileri sağlar. Durumun dönüş IOT toplanan verileri eyleme dönüştürülebilir içgörüler haline, bu nedenle birkaç önemli özellikleri gerektirir:

* Veri işleme, temizlemek için filtre, enterpolasyon, dönüştürme ve verileri analiz için hazırlar
* Gidin ve verileri (normalleştirmek için veri bağlama göre ele alınmasına) anlamak için yapı
* İşlenen (türetilmiş) de gibi ham verileri yıllardır için uzun / sınırsız bekletme için uygun maliyetli depolama

Tipik bir IOT veri akışı aşağıdaki gibi konabilecek:

  ![IOT veri akışı][1]

## <a name="azure-time-series-insights-for-industrial-iot"></a>Endüstriyel IOT için Azure Time Series Insights

Geçerli IOT yatay farklı. Bu, müşterilerin kapsayıcı üretim, otomobil, Petrol ve gaz, güç & yardımcı programı, akıllı Binalar ve danışmanlık içerir. Geçici veri keşfi verinin şeklini bilinmeyen yanı sıra operasyonel analiz Operasyonel Verimliliği için şema (açıkça Modellenen) veriler üzerinde olduğu senaryolar içerir. Bu senaryolar, genellikle yan yana var ve farklı kullanım durumlarına destekler. Çok katmanlı depolama (sıcak ve soğuk) teknolojilerinden değerinde zaman serisi verileri depolama olanağı ve açıkça model ve varlık tabanlı operasyonel zeka sorgularını en iyi duruma getirme olanağı olma gibi platform özelliklerinden başarısı için anahtar Endüstriyel IOT kuruluşlara ve kendi dijital revolution.

Azure TSI, hem IOT veri keşfi, hem de operasyonel içgörüler için kapsamlı bir uçtan uca hizmet olarak Platform-A teklifidir. Zaman serisi görüşleri, IOT ölçekli zaman serisi verilerini analiz etmek için tam olarak yönetilen bulut hizmeti sunar.

Müşteriler ham veri bir şemasız, bellek içi, deposuna ve bir dağıtılmış sorgu altyapısı ve API üzerinden etkileşimli geçici sorguları gerçekleştirme yapabilir milyarlarca olayı saniyeler içinde görselleştirmek için zengin kullanıcı deneyimimizi yararlanın. Daha fazla bilgi edinin bizim [veri keşfi özellikleri](./time-series-insights-overview.md).

TSI, ayrıca şu anda genel Önizlemedeki özellikler operasyonel içgörüler sunar. Time Series Insights, etkileşimli veri keşfi ve operasyonel zeka ile birlikte IOT varlıklarından toplanan verilerinizden daha fazla değer türetmek müşterilerin sağlar. Özellikle, genel Önizleme Teklifi aşağıdaki anahtar özelliklerini destekler:

* Bir ölçeklenebilir, performans ve saniyeler içinde zaman serisi verilerini değerinde eğilim yıl için bulut tabanlı bir IOT çözümü sağlayan maliyet açısından iyileştirilmiş zaman serisi veri deposu.
* Varlıklar ve cihazlardan türetilmiş ve türetilmeyen sinyaller ile ilişkili meta verileri ve etki alanını tanımlamak için anlam modeli desteği.
* Varlık tabanlı veri öngörüleri, iş ve operasyonel zeka kullanımını zengin analizlerle, geçici verileri birleştiren bir gelişmiş kullanıcı deneyimi
* Gelişmiş makine öğrenimi ve Azure Databricks, Apache Spark, Azure Machine Learning, Jupyter Notebooks'u, Power BI gibi analiz araçları ile tümleştirme vb. müşterilerin zaman serisi verilerini sorunlarını gidermek ve Operasyonel Verimliliği artırmaya yardımcı olmak için.

Fiyatlandırma modeli için veri işleme, depolama ve sorgu, böylece müşteriler, değişen iş gereksinimlerine uygun olarak çok daha ölçeklenebilir bir model sağlayan bir basit Kullandıkça Öde ile birlikte operasyonel içgörüler ve veri araştırması sunulur.

Güncelleştirilmiş özelliklerini gösteren bir üst düzey veri akış diyagramı aşağıdadır:

  ![Temel işlevler][2]

Bu anahtar endüstriyel IOT özelliklerini sunulmasıyla birlikte, Azure TSI aşağıdaki faydaları sağlar.

* IOT ölçekli zaman serisi verilerinin çok katmanlı depolama

  * Veri almak için bir genel veri işleme işlem hattı ile müşteriler sıcak depolama için etkileşimli sorgular ve/veya soğuk depolama, büyük hacimli verileri depolamak için verileri depolamak ve yüksek performanslı, varlık tabanlı sorgular faydalanmak özelliğine sahiptir.

  * Depolama katmanları arasında dinamik yönlendirme yakında kullanıma sunulacaktır.

* Zaman serisi modeli ham telemetri contextualizing ve varlık dayalı Öngörüler türetme

  * Müşteriler ham telemetri verileri açıklayıcı zaman serisi modeli ile bağlama göre ele alınmasına ve yüksek oranda cihaz tabanlı sorgular performans ve maliyet iyileştirme ile zengin operasyonel zeka türetilir.

* Diğer veri çözümleriyle sorunsuz tümleştirme
  
  * Azure Time Series Insights veri açık kaynaklı Apache Parquet dosyalarda depolandığından, müşterilerin diğer veri çözümleriyle kolayca tümleştirebilirsiniz (ilk ya da üçüncü taraf) makine öğrenimi, Gelişmiş iş zekası gibi uçtan uca senaryolar Tahmine dayalı analiz, vb.

* Gerçek zamanlı veri araştırması

  * Tüm veri alımı komut zinciri ile akış görselliğini Azure Time Series Insights Gezgini kullanıcı deneyimi sağlar. Kısa bir süre içinde bir olay kaynağı bağladıktan sonra müşteriler görüntüleyebilir, keşfedin ve olay verilerini bir cihaz verileri beklenen şekilde yayma ve bir IOT varlık sistem durumu, üretkenlik ve genel verimliliğini izlemeye olup olmadığını doğrulamak için kullanılan sorgu.

* Kök neden analizini ve anormallik algılama

  * Azure Time Series Insights Gezgini, desenleri ve çok adımlı kök neden analizi gerçekleştirin ve perspektif görünüm destekler. Azure Stream Analytics ile birlikte, müşteriler, uyarılar ve neredeyse gerçek zamanlı olarak anomalileri algılamak için zaman serisi görüşleri kullanabilir.

* Time Series Insights platforma özel uygulamalar oluşturun

  * Azure Time Series Insights JavaScript SDK'sı ile zengin denetimleri ve müşterilerin bireysel işletme gereksinimlerine uyacak şekilde özel IOT uygulamaları Time Series Insights platform üzerinde derleme için sorguları yönelik Basitleştirilmiş erişim destekler.
  * Müşteriler, özel IOT uygulamalarınızı doğrudan sürücü verilere Time Series Insights sorgu API'lerini de kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar

Azure TSI özel Önizleme ile kullanmaya başlamak hazır olursunuz:

> [!div class="nextstepaction"]
> [Hızlı Başlangıç kılavuzunu okuyun](./time-series-insights-update-quickstart.md)

<!-- Images -->
[1]: media/v2-update-overview/overview_one.png
[2]: media/v2-update-overview/overview_two.png