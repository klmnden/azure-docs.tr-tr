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
ms.openlocfilehash: 0bbab2acc9bf9e22e1d3c36336aa9dad04b0b73a
ms.sourcegitcommit: 6932af4f4222786476fdf62e1e0bf09295d723a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66688496"
---
# <a name="quickstart-explore-the-azure-time-series-insights-preview-demo-environment"></a>Hızlı Başlangıç: Azure zaman serisi öngörüleri önizlemesi tanıtım ortamı keşfedin

Bu hızlı başlangıçta, Azure zaman serisi öngörüleri önizlemesi ortamıyla hemen çalışmaya başlamanızı sağlar. Ücretsiz tanıtımında, zaman serisi öngörüleri Önizleme için eklenen anahtar özellikler turuna katılın.

Zaman serisi öngörüleri Önizleme tanıtım ortamı bir senaryo iki Rüzgar türbinin grupları işleyen Contoso şirketi içerir. Her bir grup 10 turbines sahiptir. Her türbinin, Azure IOT Hub'ına rapor verilerini dakikada 20 algılayıcılara sahiptir. Sensör hava koşulları, dikey aralığı hakkında bilgi toplamak ve yaw konumu. Oluşturucu performans gearbox davranışı ve güvenliği izleyicileri hakkında bilgileri de kaydedilir.

Bu hızlı başlangıçta, Contoso verileri eyleme dönüştürülebilir Öngörüler elde etmek zaman serisi görüşleri kullanmayı öğrenin. Ayrıca kısa kök neden analizi kritik hataları daha iyi tahmin etmek ve bakım yapma

## <a name="explore-the-time-series-insights-explorer-in-a-demo-environment"></a>Bir tanıtım ortamında Time Series Insights gezginini keşfedin

Zaman serisi öngörüleri Önizleme Gezgini geçmiş verileri gösteren ve kök neden analizi. Kullanmaya başlamak için:

1. Oluşturma bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) tane yoksa.

1. Git [Contoso Rüzgar grubu tanıtım](https://insights.timeseries.azure.com/preview/samples) ortam.  

1. İstenirse, Time Series Insights Gezgini Azure hesabı kimlik bilgilerinizi kullanarak oturum açın.

## <a name="work-with-historical-data"></a>Geçmiş verileri ile çalışma

1. İçinde **Contoso tesis 1**, Rüzgar türbinin sırasında Ara **W7**.  

   1. Görüntüleme aralığı için değişiklik **1/1/17 20:00 3/10/17 için 20:00 (UTC)** .
   1. Algılayıcıyı seçmek için **Contoso tesis 1** > **W7** > **Oluşturucu sistem** > **GeneratorSpeed** . Ardından, gösterilen değerleri gözden geçirin.

      [![W7 bulunan Contoso 1](media/v2-update-quickstart/quickstart-one.png)](media/v2-update-quickstart/quickstart-one.png#lightbox)

1. Kısa bir süre önce Contoso yangın Rüzgar türbinin içinde bulunan **W7**. Ateş neyin hakkındaki düşünceleri son derece değişir. Zaman serisi Öngörülerinde yangın uyarı algılayıcı sırasında yangın etkinleştirildi görebiliriz.

   1. Görüntüleme aralığı için değişiklik **3/9/17 20:00 3/10/17 için 20:00 (UTC)** .
   1. Seçin **güvenlik sistemi** > **FireAlert**.

      [![Contoso yangın Rüzgar türbinin W7 bulunamadı.](media/v2-update-quickstart/quickstart-two.png)](media/v2-update-quickstart/quickstart-two.png#lightbox)

1. Ateş neler olduğunu anlamak için zamana yakın diğer olayları gözden geçirin. Petrol baskısı ve hemen önce yangın civarında etkin uyarılar.

   1. Seçin **aralık sistem** > **HydraulicOilPressure**.
   1. Seçin **aralık sistem** > **ActiveWarning**.

      [![Yaklaşık aynı zamanda diğer olayları gözden geçirin](media/v2-update-quickstart/quickstart-three.png)](media/v2-update-quickstart/quickstart-three.png#lightbox)

1. Etkin uyarı sensörlerden ve Petrol baskısı hemen önce yangın civarında. Görüntülenen zaman serisi için yangın sonlanan değiştirmeye karşı korumalı diğer işaretleri görmek için genişletin. Her iki algılayıcılar, zaman içinde tutarlı bir şekilde fluctuated. Dalgalanmaları kalıcı ve içimiz rahat bir desen belirtin.

    * Görüntüleme aralığı için değişiklik **2/24/17 20:00 3/10/17 için 20:00 (UTC)** .

      [![Petrol baskısı ve ayrıca civarında etkin uyarı algılayıcılar](media/v2-update-quickstart/quickstart-four.png)](media/v2-update-quickstart/quickstart-four.png#lightbox)

1. İki yıla ilişkin geçmiş verileri inceleme, aynı algılayıcı dalgalanmaları olan başka bir Ateş olay ortaya çıkarır.

    * Görüntüleme aralığı için değişiklik **1/1/16 31/12/17 '** (tüm veriler).

      [![Geçmiş desenleri arayın](media/v2-update-quickstart/quickstart-five.png)](media/v2-update-quickstart/quickstart-five.png#lightbox)

Time Series Insights'ı ve bizim algılayıcı telemetri kullanarak geçmiş verileri gizli uzun vadeli ve sorunlu bir eğilim keşfettiniz. Bu yeni Öngörüler ile yapabiliriz:

> [!div class="checklist"]
> * Aslında ne olduğunu açıklar.
> * Sorunu düzeltin.
> * Üstün uyarı bildirim sistemlerini yere yerleştirin.

## <a name="root-cause-analysis"></a>Kök neden analizi

1. Bazı senaryolarda veri Zarif ipuçları ortaya çıkarmak için Gelişmiş bir analiz gerektirir. Yeldeğirmeni seçin **W6** tarihinde **6/25**.

    1. Görüntüleme aralığı için değişiklik **6/1/17 20:00 için 7/1/17 20:00 (UTC)** .
    1. Seçin **Contoso tesis 1** > **W6** > **güvenlik sistemi** > **VoltageActuatorSwitchWarning**.

       [![Görüntüleme aralığı değiştirmek ve W6 seçin](media/v2-update-quickstart/quickstart-six.png)](media/v2-update-quickstart/quickstart-six.png#lightbox)

1. Uyarı Oluşturucu tarafından çıkış olan voltaj ile bir sorun olduğunu gösterir. Toplam güç çıkışı oluşturucunun geçerli aralığında içinde normal parametreleri işletim. Bizim aralığı arttırılarak, başka bir desen ortaya çıkar. Kesin bir bırakma açıkça görülmektedir.

    1. Kaldırma **VoltageActuatorSwitchWarning** algılayıcı.
    1. Seçin **Oluşturucu sistem** > **ActivePower**.
    1. İçin aralığı değiştirmeniz **3B**.

       [![3B'ye aralığını değiştirme](media/v2-update-quickstart/quickstart-seven.png)](media/v2-update-quickstart/quickstart-seven.png#lightbox)

1. Zaman aralığının genişletilmesi tarafından sorun olup olmadığını durdurulmuş veya yanıt olup devam belirleyebiliriz.

    * Zaman aralığı 60 gün için genişletin.

      [![Zaman aralığı 60 gün için genişletin](media/v2-update-quickstart/quickstart-eight.png)](media/v2-update-quickstart/quickstart-eight.png#lightbox)

1. Daha fazla bağlam sağlamak için diğer algılayıcı veri noktası eklenebilir. Sorun ilişkin daha kapsamlı olduğundan, daha fazla algılayıcılar biz görüntüleyin. Şimdi, gerçek değerleri görmek için bir işaretçi bırakın. 

    1. Seçin **Oluşturucu sistem**ve ardından üç sensörlerden seçin: **GridVoltagePhase1**, **GridVoltagePhase2**, ve **GridVoltagePhase3**.
    1. Son görünen alanın veri noktasına bir işaret bırakın.

       [![Bir işaretçi bırak](media/v2-update-quickstart/quickstart-nine.png)](media/v2-update-quickstart/quickstart-nine.png#lightbox)

    İki voltaj algılayıcıları karşılaştırılabilir ve normal parametreleri içinde çalışıyor. Gibi görünüyor **GridVoltagePhase3** algılayıcı sorunlu olduğunu.

1. Eklenen son derece bağlamsal veriler ile 3. Aşama teslim sorun daha da fazla görünür. Artık, uyarı neden iyi bir müşteri adayı sahibiz. Bakım ekibimiz sorunu başvurmak hazırız.  

    * Tüm yer değiştirme **Oluşturucu sistem** algılayıcılar aynı grafik ölçek.

      [![Her şey dahil etmek için değiştirme](media/v2-update-quickstart/quickstart-ten.png)](media/v2-update-quickstart/quickstart-ten.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

Zaman serisi öngörüleri Önizleme ortamınızı oluşturmaya hazırsınız. Başlamak için:

> [!div class="nextstepaction"]
> [Zaman serisi öngörüleri Önizleme ortamınızı planlama](time-series-insights-update-plan.md)

Tanıtım ve özelliklerini öğrenin:

> [!div class="nextstepaction"]
> [Zaman serisi öngörüleri Önizleme Gezgini](time-series-insights-update-explorer.md)
