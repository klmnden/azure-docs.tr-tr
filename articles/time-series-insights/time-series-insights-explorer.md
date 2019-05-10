---
title: Azure Time Series Insights Gezginini kullanarak verileri keşfedin | Microsoft Docs
description: Bu makalede hızla büyük verilerinize ilişkin genel bir görünüm görün ve IOT ortamınızı doğrulamak için web tarayıcınızda Azure Time Series Insights gezgininin kullanmayı açıklar.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/07/2019
ms.custom: seodec18
ms.openlocfilehash: 0f22a0245d002b94d9fc0004214c37944350e262
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65413065"
---
# <a name="azure-time-series-insights-explorer"></a>Azure Time Series Insights Gezgini

Bu makalede özelliklere ve kullanılabilirlik için Azure Time Series Insights genel seçenekleri [explorer web uygulaması](https://insights.timeseries.azure.com/). Time Series Insights Gezgini hizmet tarafından sağlanan güçlü veri görselleştirme özellikleri gösterir ve kendi ortamında erişilebilir.

Azure Time Series Insights, milyarlarca IoT olayını aynı anda keşfedip analiz etmeyi kolaylaştıran ve tam olarak yönetilen bir analiz, depolama ve görselleştirme hizmetidir. IOT çözümünüzü hızlıca doğrulamanıza ve görev açısından kritik cihazlarda, kapalı kalma önlemenize olanak tanır, verilerinizin genel bir görünüm sağlar. Anomalileri, gizli eğilimleri keşfetmenize ve neredeyse gerçek zamanlı olarak kök neden analizleri gerçekleştirebilir. Time Series Insights Gezgini şu anda genel Önizleme aşamasındadır.

> [!TIP]
> Tanıtım ortamı üzerinden Kılavuzlu Tur için okuma [Azure zaman serisi öngörüleri hızlı](time-series-quickstart.md).

## <a name="video"></a>Video

### <a name="learn-about-querying-data-using-the-time-series-insights-explorer-br"></a>Time Series Insights Gezginini kullanarak verileri sorgulama hakkında bilgi edinin. </br>

> [!VIDEO https://www.youtube.com/embed/SHFPZvrR71s]

>[!NOTE]
>Önceki video bkz <a href="https://www.youtube.com/watch?v=6ehNf6AJkFo">"Kullanarak bir Azure IOT Çözüm Hızlandırıcısı TSI ile Başlarken"</a>.

## <a name="prerequisites"></a>Önkoşullar

Time Series Insights gezgininin kullanabilmeniz için önce şunları yapmalısınız:

- Zaman serisi görüşleri ortamı oluşturun. Daha fazla bilgi için [Time Series Insights ile çalışmaya başlama konusunda](./time-series-insights-get-started.md).
- [Erişim sağlamak](time-series-insights-data-access.md) hesabınıza ortamında.
- Ekleme bir [IOT hub'ı](time-series-insights-how-to-add-an-event-source-iothub.md) veya [olay hub'ı](time-series-insights-how-to-add-an-event-source-eventhub.md) olay kaynağına.

## <a name="explore-and-query-data"></a>Keşfedin ve veri sorgulama

Zaman serisi görüşleri ortamınıza olay kaynağınızı bağlanan dakika içinde keşfedebilir ve zaman serisi verilerinizi sorgulayın.

1. Başlamak için açık [Time Series Insights gezgininin](https://insights.timeseries.azure.com/) web tarayıcısı ve pencerenin sol tarafındaki bir ortam seçin. Alfabetik olarak erişiminiz olan tüm ortamlar listelenir.

1. Bir ortam seçin sonra kullanın ya da **FROM** ve **Kime** yapılandırmaları, üst veya sürükleyip bırakın, istenen zaman aralığı.  Sağ, üst köşedeki büyütece tıklayın veya seçilen zaman aralığı üzerinde sağ tıklayıp **arama**.  

1. Ayrıca kullanılabilirlik otomatik olarak her dakika seçerek yenileyebileceğiniz **otomatik üzerinde** düğmesi. **Otomatik açma** düğmesine yalnızca ana görselleştirme içeriğini değil, kullanılabilirlik grafiği uygular.

1. Ortamınızı Azure portalında için gereken Azure bulut simgesi göreceksiniz.

   [![Zaman serisi görüşleri ortamı](media/time-series-insights-explorer/explorer1.png)](media/time-series-insights-explorer/explorer1.png#lightbox)

1. Ardından, seçili zaman aralığı tüm olayların sayısını gösteren bir grafik görürsünüz.  Buraya çeşitli denetimler vardır:

    **Koşulları Düzenleyicisi paneli**:  Ortamınızı sorgu burada terimi alandır.  Ekranın sol tarafındaki bulunursa:
      - **Ölçü**:  Tüm sayısal sütunları bu açılan gösterir (**double**)
      - **Bölme ölçütü**: Bu açılan kategorik sütunlar gösterir (**dizeleri**)
      - Basamaklı interpolasyon etkinleştirme, minimum ve maksimum Göster ve sonraki ölçmek için Denetim Masası'ndan y ekseni ayarlayın.  Ayrıca, count, average ya da veri toplamını gösterilen verileri olup olmadığını ayarlayabilir.
      - Aynı x eksenine görüntülemek için en fazla beş koşullarını ekleyebilirsiniz.  Kullanım **kopyalama aşağı** düğmesine tıklayın ya da ek bir terim ekleyin **Ekle** düğmesini yeni bir terim ekleyin.

        [![Koşulları Düzenleyicisi paneli](media/time-series-insights-explorer/explorer2.png)](media/time-series-insights-explorer/explorer2.png#lightbox)

      - **Koşul**:  Koşul olaylarınızı aşağıda listelenen işlenen kümesini kullanarak hızlı bir şekilde filtrelemenize olanak sağlar. Seçme/tıklayarak arama yapma, koşul güncelleştirme otomatik olarak bu arama temel. Desteklenen işlenen türleri şunlardır:

         |İşlem  |Desteklenen türler  |Notlar  |
         |---------|---------|---------|
         |`<`, `>`, `<=`, `>=`     |  Çift, DateTime, zaman aralığı       |         |
         |`=`, `!=`, `<>`     | Dize, Bool, Double, DateTime, zaman aralığı, NULL        |         |
         |IN     | Dize, Bool, Double, DateTime, zaman aralığı, NULL        |  Tüm işlenenler aynı türde veya NULL sabiti olması.        |
         |SAHİP     | String        |  Yalnızca sabit dize değişmez değerleri, sağ tarafında izin verilir. Boş dize ve NULL yapılamaz.       |

      - **Sorgu örnekleri**

         [![Örnek sorgular](media/time-series-insights-explorer/explorer9.png)](media/time-series-insights-explorer/explorer9.png#lightbox)

1. **Aralık boyutu** kaydırıcı aracını içine ve dışına aralıklar aynı timespan yakınlaştırma olanak sağlar.  Bu, zaman dilimleri aşağı kesintisiz eğilimlerini ayrıntılı, yüksek çözünürlüklü keser verilerinizi görmenize olanak sağlayan milisaniyelik kadar küçük Göster büyük dilimleri arasında taşıma daha kesin bir denetim sağlar. Kaydırıcının varsayılan başlangıç noktası seçiminizden verilerinin en iyi görünümü ayarlanır; Dengeleme çözümü, sorgu hızı ve ayrıntı düzeyi.

1. **Zaman fırça** aracı sezgisel bir UX ön ve zaman aralıkları arasında sorunsuz taşıma için merkezi bir başka bir zaman aralığını gitmek kolaylaştırır.

1. **Kaydet** komutu, geçerli sorguyu kaydedin ve ortam diğer kullanıcılarla paylaşmak için etkinleştirme sağlar. Kullanarak **açık**, tüm kayıtlı sorgularınızı ve diğer kullanıcıların erişim iznine sahip olduğunuz ortamlarda paylaşılan sorgular görebilirsiniz.

   [![Sorguları](media/time-series-insights-explorer/explorer3.png)](media/time-series-insights-explorer/explorer3.png#lightbox)

## <a name="visualize-data"></a>Verileri görselleştirme

1. **Perspektif Görünüm** aracı, en fazla dört benzersiz sorguları eş zamanlı bir görünümünü sağlar. Perspektif görüntüle düğmesi grafiğin sağ üst köşesinde bulabilirsiniz.  

   [![Perspektif Görünüm](media/time-series-insights-explorer/explorer4.png)](media/time-series-insights-explorer/explorer4.png#lightbox)

1. **Grafik** verilerinizi görsel olarak keşfetmenize olanak sağlar. Grafik araçları içerir:

    - **Seç/click**, belirli bir zaman aralığı veya tek bir veri dizisi seçimi sağlar.  
    - Seçimi bir süre içinde span, yakınlaştırma veya olayları keşfedin.  
    - Bir veri serisi içinde serisi başka bir sütunu örneğe göre Böl, seri yeni bir terim olarak ekleyebilir, yalnızca seçilen seriyi Göster, seçili seriyi dışta bırak, serisi ping veya seçili serisinden olayları keşfedin.
    - Grafiğin sol filtre alanına tüm görüntülenen veri serisini görmek ve değer veya adına göre yeniden sıralama, tüm veri serisi veya herhangi bir sabitlenmiş veya sabitlenmemiş seri görüntüleyin.  Ayrıca tek bir veri dizisi seçin ve seri başka bir sütunu örneğe göre Böl, seri yeni bir terim olarak Ekle, yalnızca seçilen seriyi Göster, seçili seriyi dışta bırak, serisi sabitleme veya seçili serisinden olayları keşfedin.
    - Aynı anda birden çok kullanım koşulları görüntülerken, yığın, biriktir, veri serisi hakkında ek verileri görmek ve grafiğin sağ üst köşesindeki düğme ile tüm terimler arasında aynı y ekseni kullan.

    [![Grafik araç](media/time-series-insights-explorer/explorer5.png)](media/time-series-insights-explorer/explorer5.png#lightbox)

1. **Isı Haritası** hızla benzersiz veya anormal veri serisi içinde belirli bir sorgu nokta için kullanılabilir. Yalnızca bir arama terimi bir ısı Haritası görselleştirilebilir.

    [![Isı Haritası](media/time-series-insights-explorer/explorer6.png)](media/time-series-insights-explorer/explorer6.png#lightbox)

1. **Olayları**:  Seçeneğini belirlediğinizde seçerek ya da sağ paneli kullanılabilir hale getirileceğini olayları yukarıdaki olayları keşfedin.  Tüm ham olaylarınızı burada görebilirsiniz ve olaylarınızı JSON veya CSV dosyaları olarak dışarı aktarın. Time Series Insights, tüm ham verileri depolar.

    [![Olayları](media/time-series-insights-explorer/explorer7.png)](media/time-series-insights-explorer/explorer7.png#lightbox)

1. Tıklayın **İSTATİSTİKLERİ** olayları desenleri ve sütun istatistikleri kullanıma sunmak için keşfetmek sonra sekmesi.  

    - **Desenler**: Bu özellik, seçili veri bölgesindeki en istatistiksel açıdan anlamlı desenleri proaktif olarak ortaya çıkarır. Bu, hangi desenleri en zaman ve enerji garanti anlamak için olayları binlerce aramak zorunda kalmamasını. Ayrıca, Time Series Insights analiz yapmadan devam etmek için doğrudan bu istatistiksel desenleri ile bağlantı sağlar. Bu özellik ayrıca, geçmiş verilerin son İnceleme araştırmalar için yararlıdır.

    - **Sütun istatistikleri**:  Sütun istatistikleri, grafik ve seçili zaman aralığı seçili veri serisinin her bir sütundan gelen verilerin ayırmanız tablolar sağlar.  

      [![İSTATİSTİKLERİ](media/time-series-insights-explorer/explorer8.png)](media/time-series-insights-explorer/explorer8.png#lightbox)

Artık çeşitli özellikler ve Time Series Insights Gezgini web app içinde seçeneklerini gördünüz.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [tanılama ve sorun giderme](time-series-insights-diagnose-and-solve-problems.md) Time Series Insights ortamınızdaki.

- Kılavuzlu ele [Azure zaman serisi öngörüleri hızlı](time-series-quickstart.md) turu.
