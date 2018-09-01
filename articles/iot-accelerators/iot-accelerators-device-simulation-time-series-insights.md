---
title: Azure Time Series Insights ile cihaz benzetimi telemetri Görselleştirme | Microsoft Docs
description: Time Series Insights ortamınızı keşfedin ve cihaz benzetimi çözüm Hızlandırıcı tarafından oluşturulan telemetri analiz etmek için yapılandırmayı öğrenin.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.date: 08/20/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 49cecca4e5ebef9f43fdda16cfa1fdc2ad7b6f94
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2018
ms.locfileid: "43383203"
---
# <a name="use-time-series-insights-to-visualize-telemetry-sent-from-the-device-simulation-solution-accelerator"></a>Cihaz benzetimi çözüm hızlandırıcıdan gönderilen telemetri görselleştirmek için Time Series Insights'ı kullanın

Cihaz benzetimi çözüm Hızlandırıcısını IOT çözümlerinizi test etmek için sanal cihazlardan telemetri oluşturmanıza olanak sağlar. Bu nasıl yapılır kılavuzunda görselleştirmesine ve analiz kullanarak zaman serisi görüşleri ortamına sanal telemetri gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır Kılavuzu'ndaki adımları takip etmek için bir etkin Azure aboneliği gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu nasıl yapılır Kılavuzu'ndaki adımları, Azure aboneliğinize cihaz benzetimi çözüm Hızlandırıcısını dağıttınız varsayılır. Çözüm Hızlandırıcısını dağıtmadıysanız adımları [dağıtma ve çalıştırma bir bulut tabanlı cihaz benzetimi çözümü](quickstart-device-simulation-deploy.md) hızlı başlangıç.

Bu makalede, Çözüm Hızlandırıcısı adı olduğunu varsayar **contoso-simülasyon**. Değiştirin **contoso-simülasyon** çözüm hızlandırıcınız aynı ada sahip aşağıdaki adımları tamamlayın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [iot-accelerators-create-tsi.md](../../includes/iot-accelerators-create-tsi.md)]

## <a name="start-a-simulation"></a>Bir benzetimi Başlat

Time Series Insights gezgininin kullanmadan önce birkaç telemetri oluşturmak için cihaz benzetimi çözüm Hızlandırıcısını yapılandırın. Aşağıdaki ekran görüntüsünde, 10 Soğutucu cihazları ile çalışan bir simülasyon gösterir:

![Cihaz simülasyonu çalıştırma](./media/iot-accelerators-device-simulation-time-series-insights/running-simulation.png)

## <a name="time-series-insights-explorer"></a>Time Series Insights gezgini

Time Series Insights Gezgini, telemetrinizi görselleştirmek için kullanabileceğiniz bir web uygulamasıdır.

1. Azure Time Series Insights portalında **genel bakış** sekmesi.

1. Time Series Insights Gezgini web uygulaması'nı açmak için **ortama Git**:

    ![Time Series Insights gezgini](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-environment.png)

1. Zaman seçimi paneli içinde seçin **son 30 dakika** hızlı menüsüne ve ardından süreleri'nden **arama**:

    ![Time Series Insights Gezgini araması](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-search-time.png)

1. Sol taraftaki terimler panelinden seçin **sıcaklık** olarak **ölçü** ve **iothub-bağlantı-cihaz-ID** olarak **bölünmüş tarafından** değeri:

    ![Zaman serisi görüşleri Gezgini sorgu](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-query1.png)

1. Sağ tıklatın ve grafik **olayları keşfet**:

    ![Zaman serisi görüşleri Gezgini olaylar](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-explore-events.png)

1. Olay verilerini bir kılavuzda gösterir:

    ![Zaman serisi görüşleri Gezgini tablosu](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-table.png)

1. Perspektif görüntüle düğmesine tıklayın:

    ![Zaman serisi görüşleri Gezgini perspektifi](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-explorer-perspective.png)

1. Tıklayın **+** perspektifi için yeni bir sorgu eklemek için:

    ![Time Series Insights Gezgini sorgu Ekle](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-new-query.png)

1. Seçin **son 30 dakika** zaman aralığı olarak **nem** olarak **ölçü**, ve **iothub-bağlantı-cihaz-ID** olarak**Bölünmüş tarafından** değeri:

    ![Zaman serisi görüşleri Gezgini sorgu](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-query2.png)

1. Cihaz telemetrisi panoyu görüntülemek için perspektif görüntüle düğmesine tıklayın:

    ![Zaman serisi öngörüleri Gezgini Panosu](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-dashboard.png)

[!INCLUDE [iot-accelerators-cleanup-tsi.md](../../includes/iot-accelerators-cleanup-tsi.md)]

## <a name="next-steps"></a>Sonraki Adımlar

Keşfedin ve Time Series Insights gezgininin verilerde sorgulama hakkında daha fazla bilgi için bkz. [Azure Time Series Insights gezgininin](../time-series-insights/time-series-insights-explorer.md).
