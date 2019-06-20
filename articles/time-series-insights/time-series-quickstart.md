---
title: 'Hızlı Başlangıç: Azure Time Series Insights Gezgini | Microsoft Docs'
description: Bu hızlı başlangıçta, büyük hacimlerdeki IOT verilerini görselleştirmek için Azure Time Series Insights Gezgini ile web tarayıcınızda başlama işlemini göstermektedir. Bir tanıtım ortamında temel özelliklere göz atın.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc seodec18
ms.date: 04/22/2019
ms.openlocfilehash: 415ce28a7cab77c538a7dfb8f387900ff515dd0e
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164558"
---
# <a name="quickstart-explore-azure-time-series-insights"></a>Hızlı Başlangıç: Azure Time Series Insights’ı keşfedin

Bu hızlı başlangıçta Azure Time Series Insights Gezgini ücretsiz bir tanıtım ortamında Time Series Insights ile çalışmaya başlamanıza yardımcı olur. Artık genel kullanıma sunulmuş olan IOT verilerini ve turu anahtar özelliklerini büyük hacimli görselleştirmek için web tarayıcınızı kullanmayı öğrenin.

Azure Time Series Insights keşfedin ve milyarlarca IOT olayını aynı anda keşfedip Analiz işlemini kolaylaştıran tam olarak yönetilen bir analiz, depolama ve görselleştirme hizmeti olan. Böylece hızla IOT çözümünüzü doğrulamanıza ve görev açısından kritik cihazlarda, kapalı kalma kaçının, verilerinize ilişkin genel bir görünüm sağlar. Azure Time Series Insights, gizli eğilimleri anormallikleri keşfetmenize ve neredeyse gerçek zamanlı olarak kök neden analizleri gerçekleştirebilir yardımcı olur.

Daha fazla esneklik için kendi güçlü üzerinden önceden var olan bir uygulama için Azure Time Series Insights ekleyebilirsiniz [REST API'leri](./time-series-insights-update-tsq.md) ve [istemci SDK'sı](./tutorial-create-tsi-sample-spa.md). API'leri, sorgu, depolayın ve tercih ettiğiniz bir istemci uygulamasında zaman serisi verilerini kullanmak için kullanabilirsiniz. İstemci SDK'sı, kullanıcı Arabirimi bileşenleri, varolan bir uygulamaya eklemek için de kullanabilirsiniz.

Bu hızlı başlangıçta Time Series Insights Gezgini, artık genel kullanıma sunulmuş olan özellikler için Kılavuzlu bir turu sunar.

## <a name="prepare-the-demo-environment"></a>Tanıtım ortamı hazırlama

1. Oluşturma bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) , bir henüz oluşturmadıysanız.

1. Tarayıcınızda, Git [genel kullanılabilirlik tanıtım](https://insights.timeseries.azure.com/demo).

1. İstenirse, Time Series Insights Gezgini Azure hesabı kimlik bilgilerinizi kullanarak oturum açın.

1. Time Series Insights Hızlı Tur sayfası görüntülenir. Seçin **sonraki** hızlı turu başlatmak için.

   [![İleri'yi seçin](media/quickstart/quickstart1.png)](media/quickstart/quickstart1.png#lightbox)

## <a name="explore-the-demo-environment"></a>Tanıtım ortamı keşfedin

1. **Zaman seçimi paneline** görüntüler. Görselleştirilecek zaman aralığını seçmek için bu paneli kullanın.

   [![Zaman seçimi paneli](media/quickstart/quickstart2.png)](media/quickstart/quickstart2.png#lightbox)

1. Bir zaman aralığı seçin ve bölgede sürükleyin. Ardından **arama**.

   [![Bir zaman aralığı seçin](media/quickstart/quickstart3.png)](media/quickstart/quickstart3.png#lightbox)

   Time Series Insights, belirttiğiniz zaman aralığı için bir grafik görselleştirmesi görüntüler. Bunu, çizgi grafik çeşitli eylemler gerçekleştirebilirsiniz. Örneğin, filtreleme, sabitleme, sıralama yığın ve.

   Geri dönmek için **zaman seçimi paneline**, gösterildiği gibi aşağı oku seçin:

   [![Grafik](media/quickstart/quickstart4.png)](media/quickstart/quickstart4.png#lightbox)

1. Seçin **Ekle** içinde **terimler paneli** yeni bir arama terimi eklemek için.

   [![Öğe Ekle](media/quickstart/quickstart5.png)](media/quickstart/quickstart5.png#lightbox)

1. Grafikte bir bölgeyi seçip bölgeye sağ tıklayabilir ve **Olayları Keşfet**’i seçebilirsiniz.

   [![Olayları keşfet](media/quickstart/quickstart6.png)](media/quickstart/quickstart6.png#lightbox)

   Ham verileriniz bir ızgara, araştırırken, bölgeden görüntüler.

   [![Izgara Görünümü](media/quickstart/quickstart7.png)](media/quickstart/quickstart7.png#lightbox)

## <a name="select-and-filter-data"></a>Seçin ve verileri filtreleme

1. Grafikteki değerleri değiştirmek için terimlerinizi düzenleyin. Karşılıklı çapraz-farklı türde değerler olarak ilişkilendirmek için başka bir terim ekleyin.

   [![Terim ekleme](media/quickstart/quickstart8.png)](media/quickstart/quickstart8.png#lightbox)

1. Bir filtreleme terimi girin **filtre serisi** improvised seri filtreleme kutusu. Bu hızlı başlangıçta, bir istasyona ait sıcaklığı ve basıncı karşılıklı olarak ilişkilendirmek için **Station5** terimini girin.

   [![Seriyi Filtrele](media/quickstart/quickstart9.png)](media/quickstart/quickstart9.png#lightbox)

Hızlı başlangıcı bitirdikten sonra örnek veri kümesiyle deneme yaparak farklı görselleştirmeler oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Kendi Time Series Insights ortamınızı oluşturmaya hazırsınız:
> [!div class="nextstepaction"]
> [Time Series Insights ortamınızı planlayın](time-series-insights-environment-planning.md)
