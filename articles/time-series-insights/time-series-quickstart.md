---
title: "Hızlı Başlangıç - Azure Time Series Insights gezgini | Microsoft Docs"
description: "Bu hızlı başlangıçta, web tarayıcınızda Azure Time Series Insights gezgini ile büyük hacimlerdeki IoT verilerini görselleştirmeye nasıl başlayabileceğiniz açıklanır. Bir tanıtım ortamında temel özelliklere göz atın."
services: time-series-insights
ms.service: time-series-insights
author: MarkMcGeeAtAquent
ms.author: v-mamcge
manager: jhubbard
editor: MarkMcGeeAtAquent, jasonwhowell, kfile, MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc
ms.date: 11/15/2017
ms.openlocfilehash: b1f2881da21849c3ac09b008640fc9f72dc158dd
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="quickstart-explore-azure-time-series-insights"></a>Hızlı Başlangıç: Azure Time Series Insights’ı Keşfedin
Bu hızlı başlangıç, ücretsiz bir tanıtım ortamında Azure Time Series Insights gezgininin nasıl kullanılmaya başlanacağını gösterir. Web tarayıcınızı kullanarak büyük hacimlerdeki IoT verilerini görselleştirmeyi öğrenecek ve Time Series Insights gezgininin temel özelliklerinde bir tura çıkacaksınız. 

Azure Time Series Insights, milyarlarca IoT olayını aynı anda keşfedip analiz etmeyi kolaylaştıran ve tam olarak yönetilen bir analiz, depolama ve görselleştirme hizmetidir. Verilerinize ilişkin genel bir görünüm sunan bu hizmet, gizli eğilimleri keşfetmenize, anormallikleri belirlemenize ve neredeyse gerçek zamanlı olarak kök neden analizleri gerçekleştirmenize yardımcı olarak IoT çözümünüzü hızla doğrulamanıza ve görev açısından kritik cihazlarda, kapalı kalma süresinden kaynaklanan maliyetleri önlemenize olanak sağlar.  Zaman serisi verilerini depolaması veya saklaması gereken bir uygulama oluşturuyorsanız Time Series Insights REST API’lerini kullanarak geliştirebilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

## <a name="explore-time-series-insights-explorer-in-a-demo-environment"></a>Bir tanıtım ortamında Time Series Insights gezginini keşfedin

1. Tarayıcınızda [https://insights.timeseries.azure.com/demo](https://insights.timeseries.azure.com/demo) adresine gidin. 

2. Oturum açmanız istenirse Azure hesabınızın kimlik bilgilerini kullanarak Time Series Insights gezgininde oturum açın. 
 
3. Time Series Insights hızlı tur sayfası görüntülenir. Hızlı turu başlatmak için **İleri**’ye tıklayın.

   ![İleri’ye tıklayın](media/quickstart/quickstart1.png)

4. **Zaman seçim paneli** görüntülenir. Görselleştirilecek zaman aralığını seçmek için bu paneli kullanın.

   ![Zaman seçim paneli](media/quickstart/quickstart2.png)

5. Bölgeye tıklayıp sürükleyin, sonra **Ara** düğmesine tıklayın.
 
   ![Zaman aralığı seçin](media/quickstart/quickstart3.png) 

   Time Series Insights, belirttiğiniz zaman aralığı için bir grafik görselleştirmesi görüntüler. Çizgi grafikte filtreleme, sabitleme, sıralama ve yığma gibi çeşitli eylemler gerçekleştirebilirsiniz. 

   **Zaman seçim paneline** dönmek için gösterildiği gibi aşağı oka tıklayın:

   ![Grafik](media/quickstart/quickstart4.png)

6. Yeni bir arama terimi eklemek için **Terimler panelinden** **Ekle**’ye tıklayın.

   ![Öğe ekleme](media/quickstart/quickstart5.png)

7. Grafikte bir bölgeyi seçip bölgeye sağ tıklayabilir ve **Olayları Keşfet**’i seçebilirsiniz.
 
   ![Olayları Keşfet](media/quickstart/quickstart6.png)

   Keşfettiğiniz bölgeden ham verileriniz bir ızgara şeklinde görüntülenir:

   ![Izgara görünümü](media/quickstart/quickstart7.png)

8. Grafikteki değerleri değiştirmek için terimlerinizi düzenleyin ve farklı değer türlerini karşılıklı olarak ilişkilendirmek için başka bir terim ekleyin:

   ![Terim ekleme](media/quickstart/quickstart8.png)

9. Tek seferlik seri filtreleme için **Seriyi filtrele...** kutusuna bir filtreleme terimi girin. Bu hızlı başlangıçta, bir istasyona ait sıcaklığı ve basıncı karşılıklı olarak ilişkilendirmek için **Station5** terimini girin.
 
   ![Seriyi filtrele](media/quickstart/quickstart9.png)

Hızlı başlangıcı bitirdikten sonra örnek veri kümesiyle deneme yaparak farklı görselleştirmeler oluşturabilirsiniz. 

### <a name="next-steps"></a>Sonraki adımlar
Kendi Time Series Insights ortamınızı oluşturmaya hazırsınız:
> [!div class="nextstepaction"]
> [Time Series Insights ortamınızı planlayın](time-series-insights-environment-planning.md)
