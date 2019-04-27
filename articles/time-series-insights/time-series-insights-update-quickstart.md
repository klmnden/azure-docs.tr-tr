---
title: 'Hızlı Başlangıç: Azure zaman serisi öngörüleri önizlemesi tanıtım ortamı keşfedin | Microsoft Docs'
description: Azure zaman serisi öngörüleri önizlemesi tanıtım ortamı anlayın.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: anshan
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc seodec18
ms.date: 12/03/2018
ms.openlocfilehash: de5e853db6c6a0e98dea9251cc07b526288574e1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60805259"
---
# <a name="quickstart-explore-the-azure-time-series-insights-preview-demo-environment"></a>Hızlı Başlangıç: Azure zaman serisi öngörüleri önizlemesi tanıtım ortamı keşfedin

Bu hızlı başlangıçta ücretsiz bir tanıtım ortamında Azure zaman serisi Insight önizlemesi Gezgini'ni kullanma gösterilmektedir. Zaman serisi öngörüleri Önizleme gezgininin temel özelliklerini'de büyük hacimli geçmiş endüstriyel IOT verilerini ve, turu görselleştirmek için web tarayıcınızı kullanmayı öğrenin.

Time Series Insights, hizmet (PaaS) teklifi olarak uçtan uca bir platform sağlar. Alma, işlem, depolama ve improvised veri keşfi için yüksek oranda contextualized, zaman serisi iyileştirilmiş IOT ölçekli veriler sorgu. Ayrıca, operasyonel analiz sağlar. Time Series Insights benzersiz ihtiyaçlarını endüstriyel IOT dağıtımları için uyarlanmış fark yaratan bir tekliftir.

Tanıtım ortamı bir elektrik nesil şirket, Contoso gösterir. Ortamında Time Series Insights Contoso verileri eyleme dönüştürülebilir içgörüler keşfedin ve kısa kök neden analizi gerçekleştirmek için kullanın. Contoso iki Rüzgar türbinin grupları, her 10 turbines ile çalışır. Her türbinin, Azure IOT Hub'ına rapor verilerini dakikada 20 algılayıcılara sahiptir. Sensör hava koşulları, dikey aralığı hakkında bilgi toplamak ve konumu, oluşturucu performans, gearbox davranışı ve güvenliği izleyiciler yaw.

Zaman serisi öngörüleri önizlemesi, şu anda 40 GB olan Contoso büyüyen veri kümesinden son iki yıl çözümlemek için kullanın. Daha iyi anlamanıza ve kritik hatalar hem de yavaş hareket eden bakım sorunları tahmin etmenize yardımcı olabilir.

Azure aboneliğiniz yoksa, oluşturun bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) başlamadan önce.

## <a name="explore-the-time-series-insights-explorer-in-a-demo-environment"></a>Bir tanıtım ortamında Time Series Insights gezginini keşfedin

1. Tarayıcınızda, Git [Contoso Rüzgar grup ortamında](https://insights.timeseries.azure.com/preview/samples).  

1. İstenirse, Time Series Insights gezgininin Azure hesabı kimlik bilgilerinizle oturum açın.

### <a name="demo-step-1"></a>Adım 1'in Tanıtımı

1. Rüzgar türbinin bir göz atalım **W7** içinde **Contoso tesis 1**.  

    * **Eylem**: Görüntüleme aralığı için güncelleştirme **1/1/17 20:00 3/10/17 için 20:00 (UTC)**, ekleme **Contoso tesis 1** > **W7** > **Oluşturucu sistem**   >  **GeneratorSpeed** sensör ve sonra görünen elde edilen değerleri.

       ![Hızlı bir başlangıç][1]

1. Kısa bir süre önce Contoso yangın Rüzgar türbinin içinde bulunan **W7**. Şimdi burada ayrıntılara girin. Ateş uyarı algılayıcı sırasında yangın etkinleştirildi görebiliriz.

    * **Eylem**: Görüntüleme aralığı için güncelleştirme **3/9/17 20:00 3/10/17 için 20:00 (UTC)** ve ekleme **güvenlik sistemi** > **FireAlert** algılayıcı.

      ![İki Hızlı Başlangıç][2]

1. Başka ne oluştuğu yangın sırada olduğunu görelim. Petrol baskısı ve sorunu önlemek için çok geç yangın hemen önce ancak o zaman tarafından civarında etkin uyarılar.

    * **Eylem**: Ekleme **aralık sistem** > **HydraulicOilPressure** sensör ve **aralık sistem** > **ActiveWarning**algılayıcı.

      ![Üç Hızlı Başlangıç][3]

1. Biz uzaklaştırabilir, Ateş için öncesinde işaretleri vardı görebiliriz. Her iki algılayıcılar fluctuated. Bu nedenle, önce bu sorunu oluştu?

    * **Eylem**: Görüntüleme aralığı için güncelleştirme **2/24/17 20:00 3/10/17 için 20:00 (UTC)**.

      ![Dört hızlı başlangıç][4]

1. Tüm iki yıla ilişkin verileri inceleyeceğiz, önceki bir güvenlik olayı aynı işaretiyle görebiliriz. Bu verilerle Biz bu gibi sorunları yakalamak için sistemleri bir erken oluşturabilirsiniz.

    * **Eylem**: Görüntüleme aralığı için güncelleştirme **1/1/16 31/12/17 '** (tüm veriler).

       ![Hızlı Başlangıç 5][5]

### <a name="demo-step-2"></a>Tanıtım 2. adım

1. Diğer sorunları, daha hafif ve tanılamak daha zor. Time Series Insights bize zor sorunları izlemenize yardımcı olmak için yol çeşitli sağlar. Üzerinde bir uyarı algılayıcı kesinti burada görebiliriz **W6** üzerinde **6/25**. Ancak, gerçekte neler olduğunu?

    * **Eylem**: Geçerli sensörlerden kaldırmak için görüntüleme aralığı için güncelleştirme **6/1/17 20:00 için 7/1/17 20:00 (UTC)** ve ardından eklemek **Contoso tesis 1** > **W6**  >  **Güvenlik sistemi** > **VoltageActuatorSwitchWarning** algılayıcı.

       ![Hızlı Başlangıç altı][6]

1. Uyarı Oluşturucu tarafından çıkış olan voltaj ile bir sorun olduğunu gösterir. Ancak, bunun nedeni nedir? Genel power Oluşturucu görünüyor ince ayrıntılı bir aralıkta çıktı. Ancak, verileri toplayarak kesin bir bırakma görebiliriz.

    * **Eylem**: Kaldırma **VoltageActuatorSwitchWarning** algılayıcısı ekleme **Oluşturucu sistem** > **ActivePower** sensör ve güncelleştirme aralığının**3B**.

       ![Hızlı Başlangıç yedi][7]

1. Veri kümesinde biz forward giderseniz, bu sorun geçici değilse görebiliriz. Bunu devam ediyor.

    * **Eylem**: Sağa zaman aralığını genişletin.

       ![Sekiz hızlı başlangıç][8]

1. Şimdi daha fazla ayrıntıya. Voltaj aşaması görüntülemek için diğer algılayıcı veri noktası ekleyebiliriz. Ancak tüm veri noktaları karşılaştırılabilir bakın. Şimdi, gerçek değerleri görmek için bir işaretçi bırakın. 3. Aşama çıktıyla bir sorun var gibi görünüyor.

    * **Eylem**: Ekleme **Oluşturucu sistem** > **GridVoltagePhase1**, **GridVoltagePhase2**, ve **GridVoltagePhase3** algılayıcılar. Son görünen alanın veri noktasına bir işaret bırakın.

       ![Sekiz hızlı başlangıç][8]

1. 3. Aşama teslim tüm üç veri noktaları aynı ölçekte görüntülerseniz daha belirgin olarak görünür. Bu noktada, uyarı neden iyi bir müşteri adayı ile bakım ekibimiz sorunu başvurmak hazırız.  

    * **Eylem**: Ekranı kaplama aynı grafik ölçeğini tüm algılayıcılar için güncelleştirin.

       ![Dokuz hızlı başlangıç][9]

## <a name="next-steps"></a>Sonraki adımlar

Kendi zaman serisi öngörüleri Önizleme ortamı oluşturmak hazır duruma gelirsiniz:

> [!div class="nextstepaction"]
> [Zaman serisi öngörüleri Önizleme ortamınızı planlama](time-series-insights-update-plan.md)

<!-- Images -->
[1]: media/v2-update-quickstart/quickstart-one.png
[2]: media/v2-update-quickstart/quickstart-two.png
[3]: media/v2-update-quickstart/quickstart-three.png
[4]: media/v2-update-quickstart/quickstart-four.png
[5]: media/v2-update-quickstart/quickstart-five.png
[6]: media/v2-update-quickstart/quickstart-six.png
[7]: media/v2-update-quickstart/quickstart-seven.png
[8]: media/v2-update-quickstart/quickstart-eight.png
[9]: media/v2-update-quickstart/quickstart-nine.png
