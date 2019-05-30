---
title: 'Hızlı Başlangıç: Azure zaman serisi öngörüleri önizlemesi tanıtım ortamı keşfedin | Microsoft Docs'
description: Azure zaman serisi öngörüleri önizlemesi tanıtım ortamı anlayın.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: anshan
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc seodec18
ms.date: 04/22/2019
ms.openlocfilehash: e35d46607e0a186c8a3a38669c68a6ea52711b51
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66242084"
---
# <a name="quickstart-explore-the-azure-time-series-insights-preview-demo-environment"></a>Hızlı Başlangıç: Azure zaman serisi öngörüleri önizlemesi tanıtım ortamı keşfedin

Bu hızlı başlangıçta, Azure zaman serisi öngörüleri önizlemesi ile çalışmaya başlamanızı sağlar. Ücretsiz tanıtım, zaman serisi öngörüleri Önizleme sürümüne eklenmiştir temel özelliklerini anlatan bir tura.

Önizleme tanıtım ortamı bir senaryo iki Rüzgar türbinin grupları işleyen, Contoso şirketi, her 10 turbines içerir. Her türbinin, Azure IOT Hub'ına rapor verilerini dakikada 20 algılayıcılara sahiptir. Sensör hava koşulları, dikey aralığı hakkında bilgi toplamak ve yaw konumu. Ayrıca, oluşturucu performans gearbox davranışı ve güvenliği izler.

 Contoso verileri eyleme dönüştürülebilir Öngörüler elde etmek zaman serisi görüşleri kullanmayı öğreneceksiniz. Ayrıca, daha iyi tahmin kritik hataları ve bakım gerçekleştirmek için bir kısa kök neden analizi gerçekleştirin.

## <a name="explore-the-time-series-insights-explorer-in-a-demo-environment"></a>Bir tanıtım ortamında Time Series Insights gezginini keşfedin

Zaman serisi öngörüleri Önizleme Gezgini geçmiş verileri gösteren ve kök neden analizi. Kullanmaya başlamak için:

1. Oluşturma bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) bir oluşturulmadıysa durumunda.

1. Gidin [Contoso Rüzgar grubu tanıtım](https://insights.timeseries.azure.com/preview/samples) ortam.  

1. İstenirse, Azure hesabı kimlik bilgilerinizi kullanarak Time Series Insights Gezgininde oturum açın.

## <a name="work-with-historical-data"></a>Geçmiş verileri ile çalışma

1. Konum Rüzgar türbinin **W7** içinde **Contoso tesis 1**.  

    * Görüntüleme aralığı için güncelleştirme **1/1/17 20:00 3/10/17 için 20:00 (UTC)** .
    * Seçin **Contoso tesis 1** > **W7** > **Oluşturucu sistem** > **GeneratorSpeed** algılayıcı. Sonra sonuç değerlerini gözden geçirin.

      [![W7 bulunan Contoso 1](media/v2-update-quickstart/quickstart-one.png)](media/v2-update-quickstart/quickstart-one.png#lightbox)

1. Kısa bir süre önce Contoso yangın Rüzgar türbinin içinde bulunan **W7**. Ateş yakındaki nedeni neydi hakkındaki düşünceleri son derece değişir. Daha yakından incelemesi sırasında yangın uyarı algılayıcı sırasında yangın etkinleştirildi bakın.

    * Görüntüleme aralığı için güncelleştirme **3/9/17 20:00 3/10/17 için 20:00 (UTC)** .
    * Seçin **güvenlik sistemi** > **FireAlert** algılayıcı.

      [![Contoso yangın Rüzgar türbinin W7 bulunamadı.](media/v2-update-quickstart/quickstart-two.png)](media/v2-update-quickstart/quickstart-two.png#lightbox)

1. Ateş neler olduğunu anlamak için zamana yakın diğer olayları gözden geçirin. Petrol baskısı ve hemen önce yangın civarında etkin uyarılar.

    * Seçin **aralık sistem** > **HydraulicOilPressure** algılayıcı.
    * Seçin **aralık sistem** > **ActiveWarning** algılayıcı.

      [![Yaklaşık aynı zamanda diğer olayları gözden geçirin](media/v2-update-quickstart/quickstart-three.png)](media/v2-update-quickstart/quickstart-three.png#lightbox)

1. Etkin uyarı sensörlerden ve Petrol baskısı hemen önce yangın civarında. Diğer işaretleri mevcut yangın için öncesinde görmek için görüntülenen zaman serisi genişletin. Her iki sensörlerden kalıcı ve içimiz rahat bir desen gösteren zaman içinde tutarlı bir şekilde fluctuated.

    * Görüntüleme aralığı için güncelleştirme **2/24/17 20:00 3/10/17 için 20:00 (UTC)** .

      [![Petrol baskısı ve ayrıca civarında etkin uyarı algılayıcılar](media/v2-update-quickstart/quickstart-four.png)](media/v2-update-quickstart/quickstart-four.png#lightbox)

1. İki yıla ilişkin geçmiş verileri inceleyerek, başka bir Ateş olayla aynı algılayıcı dalgalanmaları ortaya çıkarır.

    * Görüntüleme aralığı için güncelleştirme **1/1/16 31/12/17 '** (tüm veriler).

      [![Geçmiş desenleri arayın](media/v2-update-quickstart/quickstart-five.png)](media/v2-update-quickstart/quickstart-five.png#lightbox)

Azure Time Series Insights'ı ve bizim algılayıcı telemetri kullanarak geçmiş verilerimizi gizli uzun vadeli ve sorunlu bir eğilim keşfettiniz. Bu yeni Öngörüler ile yapabiliriz:

> [!div class="checklist"]
> * Aslında ne olduğunu açıklar.
> * Sorunu düzeltin.
> * Üstün uyarı bildirim sistemlerini yere yerleştirin.

## <a name="root-cause-analysis"></a>Kök neden analizi

1. Bazı senaryolarda veri Zarif ipuçları ortaya çıkarmak için Gelişmiş bir analiz gerektirir. Yeldeğirmeni seçin **W6** tarihinde **6/25**

    * Görüntüleme aralığı için güncelleştirme **6/1/17 20:00 için 7/1/17 20:00 (UTC)** .
    * Ardından **Contoso tesis 1** > **W6** > **güvenlik sistemi** > **VoltageActuatorSwitchWarning**  algılayıcı.

      [![Görüntüleme aralığı güncelleştirin ve W6 seçin](media/v2-update-quickstart/quickstart-six.png)](media/v2-update-quickstart/quickstart-six.png#lightbox)

1. Uyarı Oluşturucu tarafından çıkış olan voltaj ile bir sorun olduğunu gösterir. Toplam güç çıkışı oluşturucunun içinde normal parametreler bizim geçerli aralık verilen işletim. Başka bir desen bizim aralığı arttırılarak, dolayısıyla: kesin bir bırakma yoktur.

    * Kaldırma **VoltageActuatorSwitchWarning** algılayıcı.
    * Seçin **Oluşturucu sistem** > **ActivePower** algılayıcı.
    * Güncelleştirme aralığı için **3B**.

      [![3B'ye güncelleştirme aralığı](media/v2-update-quickstart/quickstart-seven.png)](media/v2-update-quickstart/quickstart-seven.png#lightbox)

1. Zaman aralığının genişletilmesi tarafından sorun olup olmadığını durdurulmuş veya yanıt olup devam belirleyebiliriz.

    * Zaman aralığı 60 gün için genişletin.

      [![Zaman aralığı 60 gün için genişletin](media/v2-update-quickstart/quickstart-eight.png)](media/v2-update-quickstart/quickstart-eight.png#lightbox)

1. Diğer algılayıcı veri noktası üst bağlam sağlamak için eklenebilir. Daha fazla algılayıcılar biz görüntüleyebilirsiniz, soruna ilişkin bileşen olur. Şimdi, gerçek değerleri görmek için bir işaretçi bırakın. 

    * Seçin **Oluşturucu sistem** > **GridVoltagePhase1**, **GridVoltagePhase2**, ve **GridVoltagePhase3** algılayıcılar .
    * Son görünen alanın veri noktasına bir işaret bırakın.

      [![Bir işaretçi bırak](media/v2-update-quickstart/quickstart-nine.png)](media/v2-update-quickstart/quickstart-nine.png#lightbox)

    Üç voltaj algılayıcıları karşılaştırılabilir ve normal parametreleri içinde çalışıyor. Gibi görünüyor **GridVoltagePhase3** algılayıcı sorunlu olduğunu.

1. Eklenen son derece bağlamsal veriler ile 3. Aşama teslim sorunlu daha görünür. Artık uyarı neden iyi bir müşteri adayı ile bakım ekibimiz sorunu başvurmak hazırız.  

    * Tüm yardımcı görüntülenecek güncelleştirme **Oluşturucu sistem** algılayıcılar aynı grafik ölçek.

       [![Her şey dahil etmek için görüntü güncelleştirme](media/v2-update-quickstart/quickstart-ten.png)](media/v2-update-quickstart/quickstart-ten.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

Kendi zaman serisi öngörüleri Önizleme ortamı oluşturmak hazır duruma gelirsiniz:

> [!div class="nextstepaction"]
> [Zaman serisi öngörüleri Önizleme ortamınızı planlama](time-series-insights-update-plan.md)

Tanıtım ve özelliklerini öğrenin:

> [!div class="nextstepaction"]
> [Zaman serisi öngörüleri Önizleme Gezgini](time-series-insights-update-explorer.md)
