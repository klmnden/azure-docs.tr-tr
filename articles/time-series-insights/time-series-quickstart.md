---
title: 'Hızlı Başlangıç: Azure Time Series Insights Gezgini | Microsoft Docs'
description: Bu hızlı başlangıçta, web tarayıcınızda Azure Time Series Insights gezgini ile büyük hacimlerdeki IoT verilerini görselleştirmeye nasıl başlayabileceğiniz açıklanır. Bir tanıtım ortamında temel özelliklere göz atın.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc seodec18
ms.date: 04/22/2019
ms.openlocfilehash: be663520c9c1afd11ace57cd9dcb8ffb8219b86b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64696950"
---
# <a name="quickstart-explore-azure-time-series-insights"></a>Hızlı Başlangıç: Azure Time Series Insights’ı keşfedin

Hızlı Başlangıç Gezgini, ücretsiz bir tanıtım ortamında Azure Time Series Insights ile başlamak için sağlanır. Büyük hacimlerdeki IOT verilerini ve turu temel özellikleri ayrılırsınız genel kullanılabilirlik görselleştirmek için web tarayıcınızı kullanmayı öğreneceksiniz.

Azure Time Series Insights keşfedin ve milyarlarca IOT olayını aynı anda keşfedip Analiz işlemini kolaylaştıran tam olarak yönetilen bir analiz, depolama ve görselleştirme hizmeti olan. IOT çözümünüzü hızlıca doğrulamanıza ve görev açısından kritik cihazlarda, kapalı kalma önlemenize olanak verilerinize ilişkin genel bir görünüm sağlar. Azure zaman serisi öngörüleri aracılığıyla anormallikleri gizli eğilimleri keşfetmenize ve neredeyse gerçek zamanlı olarak kök neden analizleri gerçekleştirebilir.

Daha fazla esneklik için Azure zaman serisi görüşleri önceden var olan bir uygulama, güçlü aracılığıyla eklenebilir [REST API'leri](./time-series-insights-update-tsq.md) ve [istemci SDK'sı](./tutorial-create-tsi-sample-spa.md). API'ler, zaman serisi verileri sorgulamak, depolamanızı sağlayan ve tercih ettiğiniz bir istemci uygulamasında zaman serisi verilerini kullanma. Mevcut uygulamanızı kullanıcı Arabirimi bileşenleri eklemek için İstemci SDK'sı kullanmayı tercih edebilirsiniz.

Time Series Insights Gezgini Kılavuzlu bir turu, şu anda genel kullanılabilirlik sunar.

## <a name="prepare-the-demo-environment"></a>Tanıtım ortamı hazırlama

1. Oluşturma bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) bir oluşturulmadıysa durumunda.

1. Tarayıcınızda gidin [genel kullanılabilirlik tanıtım](https://insights.timeseries.azure.com/demo).

1. İstenirse, Azure hesabı kimlik bilgilerinizi kullanarak Time Series Insights Gezgininde oturum açın.

1. Time Series Insights hızlı tur sayfası görüntülenir. Hızlı turu başlatmak için **İleri**’ye tıklayın.

   [![İleri'ye tıklayın](media/quickstart/quickstart1.png)](media/quickstart/quickstart1.png#lightbox)

## <a name="explore-the-demo-environment"></a>Tanıtım ortamı keşfedin

1. **Zaman seçim paneli** görüntülenir. Görselleştirilecek zaman aralığını seçmek için bu paneli kullanın.

   [![Zaman seçimi paneli](media/quickstart/quickstart2.png)](media/quickstart/quickstart2.png#lightbox)

1. Bölgeye tıklayıp sürükleyin, sonra **Ara** düğmesine tıklayın.

   [![Bir zaman aralığı seçin](media/quickstart/quickstart3.png)](media/quickstart/quickstart3.png#lightbox)

   Time Series Insights, belirttiğiniz zaman aralığı için bir grafik görselleştirmesi görüntüler. Çizgi grafikte filtreleme, sabitleme, sıralama ve yığma gibi çeşitli eylemler gerçekleştirebilirsiniz.

   **Zaman seçim paneline** dönmek için gösterildiği gibi aşağı oka tıklayın:

   [![Grafik](media/quickstart/quickstart4.png)](media/quickstart/quickstart4.png#lightbox)

1. Yeni bir arama terimi eklemek için **Terimler panelinden** **Ekle**’ye tıklayın.

   [![Öğe Ekle](media/quickstart/quickstart5.png)](media/quickstart/quickstart5.png#lightbox)

1. Grafikte bir bölgeyi seçip bölgeye sağ tıklayabilir ve **Olayları Keşfet**’i seçebilirsiniz.

   [![Olayları keşfet](media/quickstart/quickstart6.png)](media/quickstart/quickstart6.png#lightbox)

   Keşfettiğiniz bölgeden ham verileriniz bir ızgara şeklinde görüntülenir:

   [![Izgara Görünümü](media/quickstart/quickstart7.png)](media/quickstart/quickstart7.png#lightbox)

## <a name="select-and-filter-data"></a>Seçin ve verileri filtreleme

1. Grafikteki değerleri değiştirmek için terimlerinizi düzenleyin ve farklı değer türlerini karşılıklı olarak ilişkilendirmek için başka bir terim ekleyin:

   [![Terim ekleme](media/quickstart/quickstart8.png)](media/quickstart/quickstart8.png#lightbox)

1. Bir filtreleme terimi girin **serisi Filtrele...**  improvised seri filtreleme kutusu. Bu hızlı başlangıçta, bir istasyona ait sıcaklığı ve basıncı karşılıklı olarak ilişkilendirmek için **Station5** terimini girin.

   [![Seriyi Filtrele](media/quickstart/quickstart9.png)](media/quickstart/quickstart9.png#lightbox)

Hızlı başlangıcı bitirdikten sonra örnek veri kümesiyle deneme yaparak farklı görselleştirmeler oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Kendi Time Series Insights ortamınızı oluşturmaya hazırsınız:
> [!div class="nextstepaction"]
> [Time Series Insights ortamınızı planlayın](time-series-insights-environment-planning.md)
