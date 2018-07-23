---
title: Uzaktan izleme Azure Time Series Insights ile verileri Görselleştirme | Microsoft Docs
description: Time Series Insights ortamınızı keşfedin ve Uzaktan izleme çözümünüzü öğelerin zaman serisi verilerini analiz etmek için yapılandırmayı öğrenin.
author: philmea
manager: timlt
ms.author: philmea
ms.date: 04/29/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 10617c129212d8196897af750c02647f0086c8e5
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185899"
---
# <a name="visualize-remote-monitoring-data-with-time-series-insights"></a>Time Series Insights ile Uzaktan izleme verileri Görselleştirme

Operatör daha fazla çıkış genişletmek isteyebilirsiniz veri görselleştirme Uzaktan izleme tarafından sağlanan önceden çözüm. TSI ile tümleştirmesi dışında sayfamızda Çözüm Hızlandırıcısı sağlar. Bu nasıl yapılır makalesinde Time Series Insights'ı, cihaz telemetrisini çözümlemek ve anormallikleri için yapılandırma öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır tamamlamak için aşağıdakiler gerekir:

* [Önceden yapılandırılmış Uzaktan izleme çözümü dağıtma](quickstart-remote-monitoring-deploy.md)

## <a name="create-a-consumer-group"></a>Bir tüketici grubu oluşturun

Time Series Insights veri akışı için kullanılmak üzere IOT hub'ınızda ayrılmış bir tüketici grubu oluşturmanız gerekir.

> [!NOTE]
> Tüketici grupları, Azure IOT Hub'ından veri çekmek için uygulamalar tarafından kullanılır. Her bir tüketici grubu en fazla beş çıkış tüketiciler sağlar. 32 adede kadar tüketici grubu oluşturabilirsiniz ve her beş havuzlarını çıktısı, yeni bir tüketici grubu oluşturmanız gerekir.

1. Azure portalında Cloud Shell düğmesine tıklayın.

1. Yeni bir tüketici grubu oluşturmak için aşağıdaki komutu yürütün:

```azurecli-interactive
az iot hub consumer-group create --hub-name contosorm30526 --name timeseriesinsights --resource-group ContosoRM
```

## <a name="create-a-new-time-series-insights-environment"></a>Yeni bir zaman serisi görüşleri ortamı oluşturma

Azure Time Series Insights, bulutta IoT ölçekli zaman serisi verilerinin yönetimi için tam olarak yönetilen bir analiz, depolama ve görselleştirme hizmetidir. Büyük oranda ölçeklenebilir zaman serisi verileri depolama olanağı sağlar ve dünyanın her yerinden akışı yapılan milyarlarca olayı saniyeler içinde keşfetmenize ve analiz etmenize imkan tanır. Time Series Insights, depolamak ve terabaytlarca zaman serisi verileri yönetmek, keşfedin ve milyarlarca olayı eşzamanlı olarak görselleştirin, kök neden analizi gerçekleştirin ve birden çok siteyi ve varlığı karşılaştırın için kullanın.

1. [Azure Portal](http://portal.azure.com/)’da oturum açın.

1. Seçin **kaynak Oluştur** > **nesnelerin interneti** > **Time Series Insights**.

    ![Yeni zaman serisi görüşleri](./media/iot-accelerators-time-series-insights/new-time-series-insights.png)

1. Time Series Insights ortamınızı oluşturmak için aşağıdaki tablodaki değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Ortam adı | Aşağıdaki ekran adı kullanan **contorosrmtsi**. Bu adımı tamamladığınızda, kendi benzersiz bir ad seçin. |
    | Abonelik | Açılan listeden Azure aboneliğinizi seçin. |
    | Kaynak grubu | **Yeni Oluştur**. Adı kullanıyoruz **ContosoRM**. |
    | Konum | Kullanıyoruz **Doğu ABD**. Uzaktan izleme çözümünüzü aynı bölgede ortamınızı oluşturun. |
    | Sku |**S1** |
    | Kapasite | **1** |
    | Panoya sabitle | **Evet** |

    ![Zaman serisi görüşleri oluşturma](./media/iot-accelerators-time-series-insights/new-time-series-insights-create.png)

1. **Oluştur**’a tıklayın. Uygulamanın oluşturulması için ortamı için biraz sürebilir.

## <a name="create-event-source"></a>Olay kaynağı oluşturma

IOT hub'ınıza bağlanmak için yeni bir olay kaynağı oluşturun. Önceki adımlarda oluşturulan tüketici grubu kullandığınızdan emin olun. Zaman serisi görüşleri her hizmeti, ayrılmış bir tüketici grubu başka bir hizmet tarafından kullanımda olması gerekir.

1. Yeni zaman serisi ortamınıza gidin.

1. Sol tarafta, seçin **olay kaynakları**.

    ![Olay kaynakları görüntüle](./media/iot-accelerators-time-series-insights/time-series-insights-event-sources.png)

1. **Ekle**'ye tıklayın.

    ![Olay kaynağı ekleme](./media/iot-accelerators-time-series-insights/time-series-insights-event-sources-add.png)

1. IOT hub'ınıza yeni bir olay kaynağı yapılandırmak için aşağıdaki tablodaki değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Olay kaynağı adı | Aşağıdaki ekran adı kullanan **contosorm IOT hub**. Bu adımı tamamladığınızda, kendi benzersiz bir ad kullanın. |
    | Kaynak | **IoT Hub’ı** |
    | İçeri aktarma seçeneği | **Mevcut aboneliklerden IOT hub'ı kullanın** |
    | Abonelik Kimliği | Açılan listeden Azure aboneliğinizi seçin. |
    | IOT hub'ı adı | **contosorma57a6**. Uzaktan izleme çözümünüzden IOT hub'ınızın adını kullanın. |
    | IOT hub'ı ilke adı | **iothubowner** kullanılan ilkeyi sahibi ilke olduğundan emin olun. |
    | IOT hub'ı ilke anahtarı | Bu alan otomatik olarak doldurulur. |
    | IOT hub tüketici grubu | **timeseriesinsights** |
    | Olay serileştirme biçimi | **JSON**     | Zaman damgası özellik adı | Boş bırakın |

    ![Olay kaynağı oluşturma](./media/iot-accelerators-time-series-insights/time-series-insights-event-source-create.png)

1. **Oluştur**’a tıklayın.

> [!NOTE]
> Ek kullanıcılar için Time Series Insights gezgininin erişim gerekiyorsa, bu adımlara kullanabileceğiniz [veri erişim izni verme](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-data-access#grant-data-access).

## <a name="time-series-insights-explorer"></a>Time Series Insights Gezgini

Time Series Insights gezgininin verilerinizin görselleştirmeler oluşturmanıza yardımcı olan bir web uygulamasıdır.

1. Seçin **genel bakış** sekmesi.

1. Tıklayın **ortam Git**, Time Series Insights Gezgini web uygulaması açılır.

    ![Time Series Insights Gezgini](./media/iot-accelerators-time-series-insights/time-series-insights-environment.png)

1. Zaman seçimi paneli içinde seçin **son 12 saat** hızlı menüsüne ve ardından süreleri'nden **arama**.

    ![Time Series Insights Gezgini araması](./media/iot-accelerators-time-series-insights/time-series-insights-search-time.png)

1. Sol taraftaki terimler paneli, bir ölçü değerini seçin **sıcaklık** bölünmüş tarafından değerini **iothub-bağlantı-cihaz-ID**.

    ![Time Series Insights Gezgini sorgu](./media/iot-accelerators-time-series-insights/time-series-insights-query1.png)

1. Sağ tıklatın ve grafik **olayları keşfet**.

    ![Zaman serisi öngörüleri Gezgini olaylar](./media/iot-accelerators-time-series-insights/time-series-insights-explore-events.png)

1. Olaylar, tablo biçiminde bir kılavuzda işlenir.

    ![Zaman serisi öngörüleri Gezgini tablosu](./media/iot-accelerators-time-series-insights/time-series-insights-table.png)

1. Perspektif görüntüle düğmesine tıklayın.

    ![Zaman serisi öngörüleri Gezgini perspektifi](./media/iot-accelerators-time-series-insights/time-series-insights-explorer-perspective.png)

1. Tıklayın **Ekle** perspektife yeni bir sorgu oluşturmak için.

    ![Time Series Insights Gezgini, Sorgu Ekle](./media/iot-accelerators-time-series-insights/time-series-insights-new-query.png)

1. Bir hızlı saati **son 12 saat**, bir ölçü **nem** ile bir bölme, **iothub-bağlantı-cihaz-id**.

    ![Time Series Insights Gezgini sorgu](./media/iot-accelerators-time-series-insights/time-series-insights-query2.png)

1. Cihaz ölçümleri panoyu görüntülemek üzere perspektif görüntüle düğmesine tıklayın.

    ![Zaman serisi öngörüleri Gezgini Panosu](./media/iot-accelerators-time-series-insights/time-series-insights-dashboard.png)

## <a name="next-steps"></a>Sonraki Adımlar

Keşfedin ve Time Series Insights gezgininin verilerde sorgulama hakkında bilgi edinmek için [Azure Time Series Insights gezgininin](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer).
