---
title: "Hızlı Başlangıç - Azure zaman serisi Öngörüler explorer | Microsoft Docs"
description: "Bu Hızlı Başlangıç, büyük miktarlarda IOT verileri görselleştirmek için Azure zaman serisi Öngörüler Gezgini ile web tarayıcınızda başlayacağınızı gösterir. Anahtar özellikler bir tanıtım ortamında turuna katılın."
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
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="quickstart-explore-azure-time-series-insights"></a>Hızlı Başlangıç: Azure zaman serisi Öngörüler keşfedin
Bu hızlı başlangıç ücretsiz tanıtım ortamında Azure zaman serisi Öngörüler Gezgini ile çalışmaya başlama gösterilmiştir. Kullandığınız öğrenin büyük IOT veri birimleri görselleştirmek ve zaman serisi Öngörüler explorer'ın temel özellikleri turu için web tarayıcısı. 

Azure Zaman Serisi Öngörüleri, milyarlarca IoT olayını aynı anda keşfedip analiz etmeyi kolaylaştıran ve tam olarak yönetilen bir analiz, depolama ve görselleştirme hizmetidir. Bir genel görünüm verir hızla IOT çözümünüzü doğrulamak ve gizli eğilimleri keşfetmeye yardımcı olarak maliyetli kapalı kalma süresi kritik cihazlara önlemenize izin vererek, verilerinizin nokta anormallikleri ve kök neden çözümlemeler yakın gerçek zamanlı yürütün.  Mağaza veya sorgu zaman serisi veri gerektiren bir uygulama geliştiriyorsanız, zaman serisinin Öngörüler REST API'lerini kullanarak geliştirebilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

## <a name="explore-time-series-insights-explorer-in-a-demo-environment"></a>Zaman serisi Öngörüler explorer demo ortamında keşfedin

1. Tarayıcınızda gidin [https://insights.timeseries.azure.com/demo](https://insights.timeseries.azure.com/demo). 

2. İstenirse, zaman serisi Öngörüler Gezgini Azure hesabı kimlik bilgilerinizi kullanarak oturum açın. 
 
3. Zaman serisi Öngörüler hızlı gezinti sayfası görüntülenir. Tıklatın **sonraki** Hızlı Tur başlamak için.

   ![İleri'yi tıklatın](media/quickstart/quickstart1.png)

4. **Zaman seçim bölmesi** görüntülenir. Bu panoyu görselleştirmek için bir zaman çerçevesi seçmek için kullanın.

   ![Zaman Seçim Bölmesi](media/quickstart/quickstart2.png)

5. Tıklayın ve bölgede sürükleyin ve ardından **arama** düğmesi.
 
   ![Bir zaman çerçevesi seçin](media/quickstart/quickstart3.png) 

   Zaman serisi Öngörüler grafik görselleştirme, belirtilen zaman çerçevesi için görüntüler. Sıralama ve yığın sabitleme içinde çeşitli eylemler filtreleme gibi çizgi grafiği ile yapabilirsiniz. 

   Geri dönmek için **zaman seçim bölmesi**, gösterildiği gibi aşağı oka tıklayın:

   ![Grafik](media/quickstart/quickstart4.png)

6. Tıklatın **Ekle** içinde **koşulları paneli** yeni bir arama terimi eklemek için.

   ![Öğe Ekle](media/quickstart/quickstart5.png)

7. Grafikte bir bölge seçebilir, bölgeye sağ tıklayın ve seçin **keşfedin olayları**.
 
   ![Olayları keşfedin](media/quickstart/quickstart6.png)

   Ham verilerinizi oluşan bir kılavuz, araştırırken bölgesinden görüntülenir:

   ![Izgara Görünümü](media/quickstart/quickstart7.png)

8. Grafikte değerlerini değiştirmek için koşullarınızın düzenleyin ve farklı türlerdeki değerleri arası ilişkilendirmek için başka bir terim ekleyin:

   ![Bir terim Ekle](media/quickstart/quickstart8.png)

9. Bir filtre koşulu girin **filtre serisi...**  geçici serisi filtreleme kutusu. Hızlı Başlangıç için girin **Station5** sıcaklık ve bu istasyon Basıncı arası ilişkilendirmek için.
 
   ![Filtre serisi](media/quickstart/quickstart9.png)

Hızlı Başlangıç tamamladıktan sonra farklı görselleştirme oluşturmak için örnek veri kümesi ile deneyebilirsiniz. 

### <a name="next-steps"></a>Sonraki adımlar
Kendi zaman serisi Öngörüler ortamı oluşturmak hazır olursunuz:
> [!div class="nextstepaction"]
> [Zaman serisi Öngörüler ortamınızı planlama](time-series-insights-environment-planning.md)
