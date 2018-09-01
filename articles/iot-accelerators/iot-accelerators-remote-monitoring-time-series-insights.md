---
title: Azure Time Series Insights ile Uzaktan izleme telemetri Görselleştirme | Microsoft Docs
description: Time Series Insights ortamınızı keşfedin ve analiz edin Uzaktan izleme çözüm Hızlandırıcısını tarafından oluşturulan telemetri için yapılandırmayı öğrenin.
author: philmea
manager: timlt
ms.author: philmea
ms.date: 08/20/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 02b3afbc62be7a7a9c396de888378d762405248a
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2018
ms.locfileid: "43383288"
---
# <a name="use-time-series-insights-to-visualize-telemetry-sent-from-the-remote-monitoring-solution-accelerator"></a>Uzaktan izleme çözüm hızlandırıcısının gönderilen telemetri görselleştirmek için Time Series Insights'ı kullanın

Bir operatör olarak, Uzaktan izleme çözüm Hızlandırıcısını kullanıma hazır veri görselleştirme yeteneklerini genişletmek isteyebilirsiniz. Bu nasıl yapılır kılavuzunda, zaman serisi görüşleri ortamına görselleştirmesine ve analiz için Uzaktan izleme çözüm Hızlandırıcısını gönderilen telemetri için nasıl kullanılacağını gösterir.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır Kılavuzu'ndaki adımları takip etmek için bir etkin Azure aboneliği gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu nasıl yapılır Kılavuzu'ndaki adımları, Azure aboneliğinize Uzaktan izleme çözüm Hızlandırıcısını dağıttınız varsayılır. Çözüm Hızlandırıcısını dağıtmadıysanız adımları [dağıtma ve çalıştırma bir bulut tabanlı uzaktan izleme çözümü](quickstart-remote-monitoring-deploy.md) hızlı başlangıç.

Bu makalede, Çözüm Hızlandırıcısı adı olduğunu varsayar **contoso-simülasyon**. Değiştirin **contoso-simülasyon** çözüm hızlandırıcınız aynı ada sahip aşağıdaki adımları tamamlayın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [iot-accelerators-create-tsi.md](../../includes/iot-accelerators-create-tsi.md)]

## <a name="time-series-insights-explorer"></a>Time Series Insights gezgini

Time Series Insights Gezgini, telemetrinizi görselleştirmek için kullanabileceğiniz bir web uygulamasıdır.

1. Azure Time Series Insights portalında **genel bakış** sekmesi.

1. Time Series Insights Gezgini web uygulaması'nı açmak için **ortama Git**:

    ![Time Series Insights gezgini](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-environment.png)

1. Zaman seçimi paneli içinde seçin **son 30 dakika** hızlı menüsüne ve ardından süreleri'nden **arama**:

    ![Time Series Insights Gezgini araması](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-search-time.png)

1. Sol taraftaki terimler panelinden seçin **sıcaklık** olarak **ölçü** ve **iothub-bağlantı-cihaz-ID** olarak **bölünmüş tarafından** değeri:

    ![Zaman serisi görüşleri Gezgini sorgu](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-query1.png)

1. Sağ tıklatın ve grafik **olayları keşfet**:

    ![Zaman serisi görüşleri Gezgini olaylar](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-explore-events.png)

1. Olay verilerini bir kılavuzda gösterir:

    ![Zaman serisi görüşleri Gezgini tablosu](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-table.png)

1. Perspektif görüntüle düğmesine tıklayın:

    ![Zaman serisi görüşleri Gezgini perspektifi](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-explorer-perspective.png)

1. Tıklayın **+** perspektifi için yeni bir sorgu eklemek için:

    ![Time Series Insights Gezgini sorgu Ekle](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-new-query.png)

1. Seçin **son 30 dakika** zaman aralığı olarak **nem** olarak **ölçü**, ve **iothub-bağlantı-cihaz-ID** olarak**Bölünmüş tarafından** değeri:

    ![Zaman serisi görüşleri Gezgini sorgu](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-query2.png)

1. Cihaz telemetrisi panoyu görüntülemek için perspektif görüntüle düğmesine tıklayın:

    ![Zaman serisi öngörüleri Gezgini Panosu](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-dashboard.png)

[!INCLUDE [iot-accelerators-cleanup-tsi.md](../../includes/iot-accelerators-cleanup-tsi.md)]

## <a name="next-steps"></a>Sonraki Adımlar

Keşfedin ve Time Series Insights gezgininin verilerde sorgulama hakkında bilgi edinmek için [Azure Time Series Insights gezgininin](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer).
