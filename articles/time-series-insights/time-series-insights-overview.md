---
title: 'Genel Bakış: Azure Time Series Insights nedir? | Microsoft Docs'
description: Zaman serisi verilerinin analizi ve IoT çözümleri için yeni bir hizmet olan Azure Time Series Insights'a giriş.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: overview
ms.date: 04/26/2019
ms.custom: seodec18
ms.openlocfilehash: 6ef965b9c22524df5893d5826548a0b07f077062
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237626"
---
# <a name="what-is-azure-time-series-insights"></a>Azure Time Series Insights nedir?

Azure zaman serisi görüşleri depolama, görselleştirin ve zaman serisi verileri gibi IOT cihazlar tarafından oluşturulan büyük miktarda sorgu yerleşik olarak bulunur. Bulutta zaman serisi verilerini depolamak, yönetmek, sorgulamak veya görselleştirmek istiyorsanız Time Series Insights aradığınız çözüm olabilir. 

![Time Series Insights akış çizelgesi](media/overview/time-series-insights-flowchart.png)

Time Series Insights dört temel işe sahiptir:

- Azure IOT Hub ve Azure Event Hubs gibi bulut ağ geçidi ile tamamen tümleşiktir. Bu olay kaynaklarına kolayca bağlanır ve JSON kodu içindeki veri içeren iletilerle yapıları ayrıştırarak temiz satırlara ve sütunlara uygular. Meta verileri telemetri verileriyle birleştirerek verilerinizi sütunlu bir depoda dizinler.
- Zaman serisi görüşleri depolama, verilerinizin yönetir. Verileri her zaman kolayca erişilebilir olduğundan emin olmak için verilerinizi bellek ve SSD 400 gün boyunca saklar. Milyarlarca olaya saniye – üzerine etkileşimli bir şekilde sorgulayabilirsiniz.
- Zaman serisi öngörüleri Giden kutusu görselleştirme Time Series Insights Gezgini aracılığıyla sağlar. 
- Time Series Insights hem de Time Series Insights gezgininin zaman serisi verilerinizle özel uygulamalarda katıştırma tümleştirmek kolay API'leri kullanarak bir sorgu hizmeti sağlar.

Kullanılacak dış müşteriler dahili tüketim için veya bir uygulama oluşturuyorsanız Time Series Insights bir arka uç kullanabilirsiniz. Dizin, depolama ve toplama zaman serisi verilerini kullanabilirsiniz. En üstte bir özel Görselleştirme ve kullanıcı deneyimi oluşturmak için kullanın [istemci SDK'sı](tutorial-explore-js-client-lib.md). Time Series Insights da donatılmış birkaç ile [sorgu API'leri](how-to-shape-query-json.md) senaryoları bu etkinleştirmek için özelleştirilebilir.

Zaman serisi verileri bir varlığın veya işlemin zaman içindeki değişimini gösterir. Zaman serisi verilerinde zaman damgası tarafından dizine eklenir ve en anlamlı eksen hangi tür veriler düzenlenir zamandır. Genellikle bir güncelleştirme veritabanınıza yerine INSERT olarak kabul edilir şekilde zaman serisi verilerini genellikle sıralı bir düzende ulaşır.

Bu, depolama, dizin sorgulama, analiz ve büyük birimleri, zaman serisi verilerini görselleştirme zor olabilir.
Azure Time Series Insights yakalar ve her yeni olay bir satır olarak depolar ve değişiklik zamanla verimli bir şekilde ölçülür. Sonuç olarak, ileride değişiklik tahmin etmeye yardımcı olması için son ınsights çizmek için geriye doğru bakabilirsiniz.

## <a name="video"></a>Video

### <a name="learn-more-about-azure-time-series-insights-the-cloud-based-iot-analytics-platformbr"></a>Azure zaman serisi görüşleri, IOT bulut tabanlı analiz platformu hakkında daha fazla bilgi edinin.</br>

[![VIDEO](https://img.youtube.com/vi/GaARrFfjoss/0.jpg)](https://www.youtube.com/watch?v=GaARrFfjoss)

## <a name="primary-scenarios"></a>Birincil senaryolar

- Zaman serisi verileri, ölçeklenebilir bir şekilde Store. 

   Time Series Insights'ın temelinde zaman serisi verilerine göre tasarlanmış bir veritabanı bulunur. Ölçeklenebilir ve tam olarak yönetilen olduğundan, zaman serisi görüşleri depolama ve olayları yönetme işini gerçekleştirir.

- Neredeyse gerçek zamanlı verileri araştırın. 

   Time Series Insights, ortama bu akışları tüm verileri görselleştiren bir Gezgin sağlar. Kısa bir süre içinde bir olay kaynağına bağlandıktan sonra görüntülemek, keşfedin ve zaman serisi görüşleri olay verileri sorgulama. Verilerin beklendiği gibi bir cihaza veri yayan olup olmadığını doğrulamak için ve bir IOT varlık sistem durumu, üretkenlik ve genel etkinliğini izlemek için yardımcı olur. 

- Kök neden analizi gerçekleştirin ve anomalileri algılayın.

   Time Series Insights, desenleri ve çok adımlı kök neden analizi gerçekleştirin ve perspektif görünümleri gibi araçlara sahiptir. Time Series Insights ayrıca uyarılar görüntüleyebilmesi için Azure Stream Analytics gibi hizmetler uyarı ile çalışır ve neredeyse gerçek zamanlı Time Series Insights Gezgininde algılanan anomalileri. 

- Birden çok varlık veya site karşılaştırma için birbirinden farklı konumlardan akışları zaman serisi verilerini genel bir görünümünü elde edin.

   Bir Time Series Insights ortamına birden fazla olay kaynağı bağlayabilirsiniz. Bu şekilde, birden fazla konumdan, farklı bir araya gerçek zamanlı akış sırasında verileri görüntüleyebilirsiniz. Kullanıcılar iş liderlerine ile veri paylaşımı için bu görünürlük yararlanabilirsiniz. Daha iyi uzmanlıklarını dersleri paylaşmak sorunlarını çözmek ve en iyi yöntemleri uygulamanıza yardımcı olması için uygulayabileceğiniz etki alanı uzmanlarla işbirliği yapabilir.

- Time Series Insights üzerine bir müşteri uygulama oluşturun. 

   Zaman serisi öngörüleri REST sorgu API'leri, zaman serisi verilerini kullanan uygulamalar oluşturmak için kullanabileceğiniz kullanıma sunar.

## <a name="capabilities"></a>Özellikler

- **Hızla işe koyulun**: Azure Time Series Insights, milyonlarca IOT hub'ı veya olay hub'ı hızlı bir şekilde bağlanabilir, böylece veriler için ön hazırlık gerektirmez. Bağlandıktan sonra görselleştirin ve IOT çözümlerinizi hızla doğrulamak için sensör verilerle etkileşimde bulunmak. Kod yazmadan verilerinizle etkileşim kurabilir ve yeni dil öğrenmek gerekmez. Zaman serisi görüşleri İleri düzey kullanıcılar için ayrıntılı, serbest metin kullanılan bir sorgu yüzeyi sağlar ve üzerine gelin ve araştırma tıklayın.

- **Neredeyse gerçek zamanlı görüşler**: Time Series Insights milyonlarca sensör olayını her gün, bir dakikalık bir gecikmeyle alabilen. Time Series Insights sensör verileriniz Öngörüler elde etmeye yardımcı olur. Eğilimleri ve anormallikleri, kök neden analizleri gerçekleştirebilir ve yüksek maliyetli kapalı kalma süresini önlemek için bunu kullanın. Gerçek zamanlı verilerle geçmiş verileri arasında çapraz bağıntıya verilerinde gizli eğilimleri bulmanıza yardımcı olur.

- **Özel çözümler derleme**: Azure zaman serisi öngörüleri verilerini mevcut uygulamalarınıza ekleyin. Ayrıca, zaman serisi öngörüleri REST API'leriyle yeni özel çözümler oluşturabilirsiniz. Diğer kişilerin de içgörülerinizi incelemesini sağlamak için paylaşabileceğiniz kişiselleştirilmiş görünümler oluşturun.

- **Ölçeklenebilirlik**: Zaman serisi görüşleri, ölçekli olarak Iot'yi destekleyecek şekilde tasarlanmıştır. Günde 1 milyon ile 100 milyon arası olay alabilir ve varsayılan saklama süresi 31 gündür. Görselleştirin ve canlı veri akışlarını yanında geçmiş verileri neredeyse gerçek zamanlı analiz edin.

## <a name="get-started"></a>başlarken

Başlamak için aşağıdaki adımları izleyin.

1. Azure portalında bir zaman serisi görüşleri ortamı sağlayın.
1. IOT hub'ı veya bir olay hub'ı gibi bir olay kaynağı bağlayın. 
1. Başvuru verilerini karşıya yükleyin. Bu, ek bir hizmet değildir.
1. Time Series Insights gezgininde verilerinizi dakikalar halinde görün.

## <a name="time-series-insights-explorer"></a>Time Series Insights gezgini

Örnek zaman serisinin Bu diyagramda gösterilmektedir ınsights verilerini Time Series Insights Gezgini üzerinden görüntülenebilir.

![Time Series Insights gezgini](media/time-series-insights-explorer/explorer4.png)

## <a name="next-steps"></a>Sonraki adımlar

- Azure Time Series Insights genel kullanılabilirliğini keşfedin [ücretsiz tanıtım ortamı](./time-series-quickstart.md).
- Kullanma hakkında daha fazla bilgi edinin [, zaman serisi görüşleri planlama](time-series-insights-environment-planning.md) ortam.
