---
title: "Azure zaman serisi Öngörüler Gezgini'ni kullanarak verileri araştırmak | Microsoft Docs"
description: "Bu makalede Azure zaman serisi Öngörüler explorer web tarayıcınızda hızlı bir şekilde büyük verilerinize genel bir görünümünü görebilir ve IOT ortamınızı doğrulamak için nasıl kullanılacağını açıklar."
services: time-series-insights
ms.service: time-series-insights
author: MarkMcGeeAtAquent
ms.author: kfile
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: article
ms.date: 11/30/2017
ms.openlocfilehash: d09292cce1414a1b89e4b75df27d0a689738b4d6
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="azure-time-series-insights-explorer"></a>Azure zaman serisi Öngörüler Gezgini
Bu makalede, çeşitli özellikleri ve seçenekleri zaman serisi Öngörüler explorer web app içinde kullanılabilir araştırır. Verilerinizi görsel oluşturmak için web tarayıcınızda zaman serisi Öngörüler Gezgini'ni kullanın.
 
Azure Time Series Insights, milyarlarca IoT olayını aynı anda keşfedip analiz etmeyi kolaylaştıran ve tam olarak yönetilen bir analiz, depolama ve görselleştirme hizmetidir. IOT çözümünüzü hızlı bir şekilde doğrulamak ve kritik cihazlara maliyetli kapalı kalma sürelerinden kaçının olanak tanır, verilerinizi genel bir görünümünü sağlar. Gizli eğilimleri, nokta anormallikleri bulmak ve kök neden çözümlemeler yakın gerçek zamanlı yürütün. Zaman serisi Öngörüler explorer şu anda genel önizlemede değil.

## <a name="prerequisites"></a>Önkoşullar

Zaman serisi Öngörüler explorer kullanabilmeniz için önce şunları yapmalısınız:
- Bir zaman serisi Öngörüler ortam oluşturma
- Ortamında, hesabınıza erişimi sağlar
- Veri alma ve depolamak için bir olay kaynağı Ekle

## <a name="explore-and-query-data"></a>Keşfetmek ve veri sorgulama
Zaman serisi Öngörüler ortamınız için olay kaynağı bağlayan dakika içinde keşfedin ve zaman serisi verilerinizi sorgulayabilirsiniz.

1. Başlatmak için açın [zaman serisi Öngörüler explorer](https://insights.timeseries.azure.com/) web tarayıcısı ve pencerenin sol tarafında bir ortam seçin. Erişiminiz tüm ortamlar alfabetik sırada listelenir.

2. Bir ortam seçtikten sonra ya da kullanmak **FROM** ve **Kime** en üstte yapılandırmaları veya sürükleyip bırakın, istenen zaman aralığı içinde.  Sağ üst köşedeki, Büyüteç veya seçili timespan sağ tıklatın ve seçin **arama**.  

3. Ayrıca kullanılabilirlik otomatik olarak her dakika seçerek yenileyebileceğiniz **otomatik üzerinde** düğmesi.  'Otomatik-Aç' düğmesini yalnızca kullanılabilirlik grafiğe değil ana görselleştirme içeriğini geçerlidir unutmayın.

4. Bildirim, Azure bulut simgesi ortamınıza Azure portalında alır.

   ![Time Series Insights ortamı](media/time-series-insights-explorer/explorer1.png)

5. Ardından, seçili timespan sırasında tüm olayların sayısını gösteren bir grafik bakın.  Burada, çeşitli denetimler vardır:

    **Düzenleyicisi Masası koşulları**: ortamınızı where sorgu Terime alanıdır.  Etkinleştirir ekranın sol tarafında bulundu 
      - **Ölçü**: tüm sayısal sütunlar (çiftleri) bu bırakma gösterir
      - **Bölünmüş tarafından**: kategorik sütunları (dize) Bu açılan gösterir
      - Adım ilişkilendirme etkinleştirmek, minimum ve maksimum Göster ve sonraki ölçmek için Denetim Masası'ndan y ekseni ayarlayın.  Ayrıca, gösterilen veriler sayısı, ortalama veya verilerin toplamı olup ayarlayabilirsiniz.
      - Aynı x ekseni üzerinde görüntülemek için en fazla beş koşulları ekleyebilirsiniz.  Kullanım **kopyalama aşağı** tıklayın veya ek bir terim Ekle düğmesi **Ekle** yeni bir terim eklemek için düğmeyi.
     
        ![Koşulları Düzenleyicisi Masası](media/time-series-insights-explorer/explorer2.png)

      - **Koşulu**: koşulu olaylarınızı aşağıda listelenen işlenenler kümesini kullanarak hızlı bir şekilde filtrelemenize olanak sağlar. Seçme/tıklayarak bir arama yapın, koşul güncelleştirme arama üzerinde otomatik olarak temel.      Desteklenen işlenen türleri şunlardır:

         |İşlem  |Desteklenen türler  |Notlar  |
         |---------|---------|---------|
         |<, >, <=, >=     |  Double, DateTime, TimeSpan       |         |
         |=, !=, <>     | String, Bool, Double, DateTime, TimeSpan, NULL        |         |
         |IN     | String, Bool, Double, DateTime, TimeSpan, NULL        |  Tüm işlenenleri aynı tür veya NULL sabiti olması.        |
         |HAS     | Dize        |  Yalnızca sabit dize değişmez değerleri sağ tarafında izin verilir. Boş dize ve NULL izin verilmiyor.       |

      - **Sorgu örnekleri**
      
         ![Örnek sorgular](media/time-series-insights-explorer/explorer9.png)

6. **Aralık boyutu** kaydırıcı aracı aynı timespan aralıkları ve yakınlaştırma olanak sağlar.  Bu, zaman dilimleri aşağıya doğru kesintisiz eğilimleri ayrıntılı, yüksek çözünürlüklü keser verilerinizin görmenize olanak sağlayan mili saniye kadar küçük Göster büyük dilimleri arasında hareket daha kesin bir denetim sağlar. Başlangıç noktası Kaydırıcının varsayılan seçiminizden verileri en iyi görünüm olarak ayarlanır; Çözüm, sorgu hızı ve ayrıntı düzeyi Dengeleme.

7. **Zaman fırça** aracı sezgisel UX ön ve merkezi zaman aralıkları arasında sorunsuz taşıma için başka bir zaman aralık gitmek üzere kolaylaştırır.

8. **Kaydetmek** komutu, geçerli sorguyu kaydedin ve ortam diğer kullanıcılarla paylaşmak için etkinleştirme sağlar. Kullanarak **açık**, tüm kaydedilmiş sorgularınızı ve diğer kullanıcıların erişiminiz ortamlarda paylaşılan sorgular görebilirsiniz. 

   ![Sorgular](media/time-series-insights-explorer/explorer3.png)

9. **Perspektif görünümü** aracı en fazla dört benzersiz sorguları eşzamanlı bir görünümünü sağlar. Grafiğin sağ üst köşesindeki perspektif görüntüle düğmesi bulabilirsiniz.  

   ![Perspektif görünümü](media/time-series-insights-explorer/explorer4.png)

10. **Grafik** verilerinizi görsel olarak inceleyin olanak tanır. Grafik araçları içerir:

   - Seçim /, belirli bir timespan veya tek bir veri serisi seçimi sağlayan tıklatın.  
   - Bir süre içinde seçimi span, yakınlaştırma veya olayları keşfedin.  
   - Veri serisi seri başka bir sütuna göre bölebilir, seri yeni bir terim eklemek, yalnızca seçili serisi Göster, seçili seriler hariç, serisi ping veya seçili serisinden olayları keşfedin.
   - Grafik solundaki filtre alanına tüm görüntülenen veri serisi bakın ve değeri veya adıyla yeniden sıralamak, tüm veri serisi veya özellikle sabitlenmiş veya sabitlenmemiş serisi görüntüleyin.  Ayrıca tek bir veri serisi seçin ve seri başka bir sütuna göre bölebilir, seri yeni bir terim eklemek, yalnızca seçili serisi Göster, seçili seriler hariç, serisi PIN veya seçili serisinden olayları keşfedin.
   - Aynı anda birden çok koşulları görüntülerken, yığın, yığını kaldırma, bir veri serisi hakkında ek verileri görmek ve aynı y ekseni grafiğin sağ üst köşesinde düğmeleri ile tüm koşulları kullanın.
 
   ![Grafik araç](media/time-series-insights-explorer/explorer5.png) 

11. **Heatmap** hızlı bir şekilde belirli bir sorgu benzersiz veya anormal veri serisi nokta için kullanılabilir. Yalnızca bir arama terimi heatmap canlandırılabilir.    

   ![Isı Haritası](media/time-series-insights-explorer/explorer6.png)

12. **Olayları**: seçtiğinizde veya üstüne sağ tıklayarak, olayları paneli sunulacağını seçerken olayları keşfedin.  Burada, tüm ham olaylar görebilirsiniz ve olaylarınızı JSON ya da CSV dosyası olarak dışarı aktarma. Zaman serisi Öngörüler tüm ham verileri içerdiğini unutmayın.

   ![Olaylar](media/time-series-insights-explorer/explorer7.png)

13. Tıklatın **İSTATİSTİĞİ** desenleri ve sütun istatistikleri kullanıma sunmak için olayları keşfetme sonra sekmesi.  

   - **Desenler**: Bu özellik, seçili veri bölgesindeki istatistiksel olarak en önemli desenleri proaktif olarak ortaya çıkarır. Bu, saat ve enerji hangi desenleri en garanti anlamak için olayları binlerce arayın zorunda kalmaktan kurtarır. Ayrıca, zaman serisinin Öngörüler analizini yapmadan devam etmek için doğrudan bu istatistiksel olarak önemli desenleri atlamak sağlar. Bu özellik, geçmiş verileri içine Proje Sonrası Değerlendirme araştırmalar için de yararlıdır. 

   - **Sütun istatistikleri**: sütun istatistikleri, grafik ve seçilen zaman aralığı içinde seçili veri serisinin her bir sütunun verileri ayırmanız tablolar sağlar.  
 
      ![İSTATİSTİKLER](media/time-series-insights-explorer/explorer8.png) 

Şimdi çeşitli özellikleri ve seçenekleri zaman serisi Öngörüler explorer web app içinde kullanılabilir gördünüz. 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
>[Zaman serisi Öngörüler ortamınızdaki sorunları tanılamada ve](time-series-insights-diagnose-and-solve-problems.md)
