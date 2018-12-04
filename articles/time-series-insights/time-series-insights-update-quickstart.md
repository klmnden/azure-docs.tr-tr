---
title: Azure Time Series Insights (Önizleme) tanıtım ortamı keşfedin | Microsoft Docs
description: Azure Time Series Insights (Önizleme) tanıtım ortamı anlama
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: anshan
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc
ms.date: 11/28/2018
ms.openlocfilehash: 872969b0e72d32913e4528c5c6446827ae42691b
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52853160"
---
# <a name="explore-the-azure-time-series-insights-preview-demo-environment"></a>Azure Time Series Insights (Önizleme) tanıtım ortamı keşfedin

Bu hızlı başlangıçta, Azure Time Series Insights (Önizleme) Gezgini ücretsiz bir tanıtım ortamında kullanmaya başlama işlemini göstermektedir. Web tarayıcınız, büyük hacimli geçmiş endüstriyel IOT verileri görselleştirmek ve Time Series Insights Gezgini (Önizleme) temel özelliklerini anlatan bir tura kullanmayı öğrenin.

Azure Time Series Insights bir uçtan uca hizmet olarak Platform-A-alma, işlem, depolama ve operasyonel analiz yanı sıra geçici veri keşfi için yüksek oranda contextualized, zaman serisi iyileştirilmiş IOT ölçekli veriler sorgu sunmasını sağlar. Time Series Insights bir fark yaratan benzersiz ihtiyaçlarını endüstriyel IOT dağıtımlar için özel olarak uyarlanmış bir tekliftir.

Tanıtım ortamında Time Series Insights verilerini eyleme dönüştürülebilir içgörüler keşfedin ve kısa kök neden analizi gerçekleştirmek için kullanarak bir elektrik nesil Contoso şirketi gösterir. Contoso iki Rüzgar türbinin grupları, her on turbines ile çalışır ve her türbinin dakikada Azure IOT Hub'ına raporlama verilerini 20 algılayıcılara sahiptir. Hava koşulları, dikey aralığı & yaw konumu, oluşturucu performans, gearbox davranışı ve güvenilirlik, izleyiciler, sensör bilgilerini toplayın.

Time Series Insights güncelleştirme, son iki yıl-40 daha iyi anlamanıza ve kritik hatalar hem de yavaş hareket eden bakım sorunları tahmin GB – şu anda büyüyen veri kümesinden analiz etmek için kullanılır.

Azure aboneliğiniz yoksa, oluşturun bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) başlamadan önce.

## <a name="explore-time-series-insights-explorer-in-a-demo-environment"></a>Bir tanıtım ortamında Time Series Insights gezginini keşfedin

1. Tarayıcınızda gidin [insights.timeseries.azure.com/preview/samples](https://insights.timeseries.azure.com/preview/samples).  

1. İstenirse, Time Series Insights Azure hesabı kimlik bilgilerinizi kullanarak Gezgini için oturum açın.

### <a name="demo-step-one"></a>Tanıtım adımları bir

1. Bir göz atalım **Rüzgar türbinin #7 #1 gruptaki**. Bu iyi.  

    * Eylemi: görüntüleme aralığı için güncelleştirme `1/1/17 20:00 – 3/10/17 20:00 (UTC)` ve ekleme `Farm 1 > W7 > Generator > GeneratorSpeed` algılayıcı.

       ![Hızlı bir başlangıç][1]

1. Yakın zamanda **Contoso yangın türbinin #7 ' bulunan**. Şimdi burada ayrıntılara girin. Ateş döneminde etkinleştirilmiş yangın uyarı algılayıcı görebiliriz.

    * Eylemi: görüntüleme aralığı için güncelleştirme `3/9/17 20:00 – 3/10/17 20:00 (UTC)` ve ekleme `Safety > FireAlert` algılayıcı.

      ![İki Hızlı Başlangıç][2]

1. Başka ne oluştuğu yangın sırada olduğunu görelim. Her ikisi de baskısı ve etkin uyarıları yangın hemen önce ancak sorunu engellemeye için çok geç olduğu bu nokta civarında Petrol.

    * Eylem: Ekleme `Pitch > HydraulicOilPressure` sensör ve `Pitch > ActiveWarning` algılayıcı.

      ![Üç Hızlı Başlangıç][3]

1. Biz uzaklaştırabilir, Ateş için öncesinde işaretleri vardı görebiliriz. Her iki algılayıcılar fluctuated. Bu nedenle bu önce oluştu?

    * Eylemi: görüntüleme aralığı için güncelleştirme `2/24/17 20:00 – 3/10/17 20:00 (UTC)`.

      ![Dört hızlı başlangıç][4]

1. Tüm iki yıla ilişkin verileri inceleyeceğiz, önceki bir güvenlik olayı aynı işaretiyle görebiliriz. Bu verileri, sistemleri, bu gibi sorunları erken yakalamak için ekleriz.

    * Eylemi: görüntüleme aralığı için güncelleştirme `1/1/16 – 12/31/17` (tüm veriler).

       ![Hızlı Başlangıç 5][5]

### <a name="demo-step-two"></a>İkinci adımda Tanıtımı

1. Diğer sorunları, daha hafif ve tanılamak daha zor. Time Series Insights bir zor sorunları izlemenize yardımcı olacak çeşitli özellikler sağlar. Üzerinde bir uyarı algılayıcı kesinti burada görebiliriz `turbine #6` üzerinde `6/25`. Ancak, gerçekte neler olduğunu?

    * Eylem: geçerli sensörlerden kaldırın. Ardından görüntüleme aralığı için güncelleştirme `6/1/17 20:00 – 7/1/17 20:00 (UTC)` ve ekleme `Farm 1 > W6 > Safety > VoltageActuatorSwitchWarning`.

       ![Hızlı Başlangıç altı][6]

1. Uyarı Oluşturucu tarafından çıkış olan voltaj ile bir sorun olduğunu gösterir. Ancak, bunun nedeni nedir? Genel power Oluşturucu görünüyor ince ayrıntılı bir aralıkta çıktı. Ancak, verileri toplayarak kesin bir bırakma görebiliriz devre dışı.

    * Eylem: Uzaktan `VoltageActuatorSwitchWarning` ve ekleme `Generator > ActivePower` ve güncelleştirme aralığı `3d`.

       ![Hızlı Başlangıç yedi][7]

1. Veri kümesinde biz forward giderseniz, bu yalnızca geçici bir sorun değildir görebiliriz. Sorun devam ediyor.

    * Eylem: görünüm ilerletmek için sağ ok tuşuna basın.

       ![Sekiz hızlı başlangıç][8]

1. Şimdi daha fazla ayrıntıya. Voltaj aşaması görüntülemek için diğer algılayıcı veri noktası gerçekleştirebiliriz. Ancak bunların tümü karşılaştırılabilir bakın. Şimdi, gerçek değerleri görmek için bir işaretçi bırakın. Üç aşama 3 çıkış ile ilgili bir sorun olduğu görülüyor.

    * Eylem: `Add Generator > GridVoltagePhase1, 2, & 3`. Bir işaret görülebilir alanında son datapoint bırakın.

       ![Sekiz hızlı başlangıç][8]

1. Şu üç aynı ölçekte görüntülerseniz, aşama 3 atılacak daha belirgin yapar. Bu noktada bu sorunu bakım ekibimiz uyarı neden iyi bir müşteri adayı ile başvurmak hazırız.  

    * Eylem: aynı grafik ölçeğini tüm algılayıcılar kaplama görüntülenecek güncelleştirin.

       ![Dokuz hızlı başlangıç][9]

## <a name="next-steps"></a>Sonraki adımlar

Azure Time Series Insights (Önizleme) ortamınızı oluşturmaya hazırsınız:

> [!div class="nextstepaction"]
> [Azure Time Series Insights (Önizleme) ortamınızı planlama](time-series-insights-update-plan.md)

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