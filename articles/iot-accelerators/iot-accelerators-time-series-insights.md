---
title: Uzaktan izleme verilerini Azure zaman serisi Insights ile Görselleştirme | Microsoft Docs
description: Keşfetmek ve Uzaktan izleme çözümünün zaman serisi verileri çözümlemek için zaman serisi Öngörüler ortamınızı yapılandırmayı öğrenin.
author: philmea
manager: timlt
ms.author: philmea
ms.date: 04/29/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 7a0a5d4f1fbba5d7bd2813e8b9c300a37853e06c
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37111485"
---
# <a name="visualize-remote-monitoring-data-with-time-series-insights"></a>Zaman serisi Insights ile Uzaktan izleme verileri görselleştirin

Daha fazla çıkış genişletmek bir işleç isteyebilirsiniz kutusu verilerini görselleştirme Uzaktan izleme tarafından sağlanan önceden çözüm. Bizim Çözüm Hızlandırıcısı TSI kutusunu tümleşme dışında sağlar. Bu yöntem, cihaz telemetrisi çözümlemek ve anormallikleri algılamak için zaman serisi Öngörüler yapılandırma öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu yöntem tamamlamak için aşağıdakiler gerekir:

* [Önceden yapılandırılmış Uzaktan izleme çözümü dağıtma](iot-accelerators-remote-monitoring-deploy.md)

## <a name="create-a-consumer-group"></a>Bir tüketici grubu oluştur

Zaman serisi Öngörüler veri akış için kullanılacak IOT hub'ayrılmış bir tüketici grubu oluşturmanız gerekir.

> [!NOTE]
> Tüketici grupları, Azure IOT Hub'ından veri çekmek için uygulamalar tarafından kullanılır. Her bir tüketici grubu en fazla beş çıkış tüketicileri sağlar. Her beş havuzlarını çıkış ve en fazla 32 tüketici grupları oluşturmak için yeni bir tüketici grubu oluşturmanız gerekir.

1. Azure portalında bulut Kabuk düğmesini tıklatın.

1. Yeni bir tüketici grubu oluşturmak için aşağıdaki komutu yürütün:

```azurecli-interactive
az iot hub consumer-group create --hub-name contosorm30526 --name timeseriesinsights --resource-group ContosoRM
```

## <a name="create-a-new-time-series-insights-environment"></a>Yeni bir zaman serisi Öngörüler ortamı oluşturma

Azure Time Series Insights, bulutta IoT ölçekli zaman serisi verilerinin yönetimi için tam olarak yönetilen bir analiz, depolama ve görselleştirme hizmetidir. Büyük oranda ölçeklenebilir zaman serisi verileri depolama olanağı sağlar ve dünyanın her yerinden akışı yapılan milyarlarca olayı saniyeler içinde keşfetmenize ve analiz etmenize imkan tanır. Zaman serisi Öngörüler depolamak ve terabayt zaman serisi veri yönetme, keşfetme ve olayları milyarlarca aynı anda görselleştirme, kök neden analizi yürütmek ve birden çok site ve varlıklar karşılaştırmak için kullanın.

1. [Azure Portal](http://portal.azure.com/)’da oturum açın.

1. Seçin **kaynak oluşturma** > **nesnelerin interneti** > **zaman serisi Öngörüler**.

    ![Yeni zaman serisi Öngörüler](./media/iot-accelerators-time-series-insights/new-time-series-insights.png)

1. Zaman serisi Öngörüler ortamınızı oluşturmak için aşağıdaki tabloda değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Ortam adı | Aşağıdaki ekran adını kullanır **contorosrmtsi**. Bu adımı tamamladığınızda, kendi benzersiz bir ad seçin. |
    | Abonelik | Açılan listeden Azure aboneliğinizi seçin. |
    | Kaynak grubu | **Yeni Oluştur**. Adı kullanıyoruz **ContosoRM**. |
    | Konum | Kullanıyoruz **Doğu ABD**. Uzaktan izleme çözümü ile aynı bölgede ortamınızı oluşturun. |
    | Sku |**S1** |
    | Kapasite | **1** |
    | Panoya sabitle | **Evet** |

    ![Zaman serisi Öngörüler oluşturun](./media/iot-accelerators-time-series-insights/new-time-series-insights-create.png)

1. **Oluştur**’a tıklayın. Oluşturulacak Ortam için bir dakika sürebilir.

## <a name="create-event-source"></a>Olay kaynağı oluşturma

IOT hub'ınıza bağlanmak için yeni bir olay kaynağı oluşturun. Önceki adımlarda oluşturduğunuz tüketici grubu kullandığınızdan emin olun. Zaman serisi Öngörüler ayrılmış bir tüketici grubu başka bir hizmet tarafından kullanımda olan her hizmet gerektirir.

1. Yeni zaman serisi ortamınıza gidin.

1. Sol tarafta seçin **olay kaynakları**.

    ![Görünüm olay kaynakları](./media/iot-accelerators-time-series-insights/time-series-insights-event-sources.png)

1. **Ekle**'ye tıklayın.

    ![Olay Kaynağı Ekle](./media/iot-accelerators-time-series-insights/time-series-insights-event-sources-add.png)

1. IOT hub'ınızı yeni olay kaynağı olarak yapılandırmak için aşağıdaki tabloda değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Olay kaynağı adı | Aşağıdaki ekran adını kullanır **contosorm IOT hub**. Bu adımı tamamladığınızda, kendi benzersiz bir ad kullanın. |
    | Kaynak | **IoT Hub’ı** |
    | İçeri aktarma seçeneği | **Kullanılabilir aboneliklerden IOT hub'ı kullanın** |
    | Abonelik Kimliği | Açılan listeden Azure aboneliğinizi seçin. |
    | IOT hub'ı adı | **contosorma57a6**. Uzaktan izleme çözümünüzden IOT hub'ınızın adını kullanın. |
    | IOT hub ilke adı | **iothubowner** kullanılan ilkeyi bir sahibi İlkesi olduğundan emin olun. |
    | IOT hub ilke anahtarı | Bu alan otomatik olarak doldurulur. |
    | IOT hub tüketici grubu | **timeseriesinsights** |
    | Olay serileştirme biçimi | **JSON**     | Zaman damgası özelliği adı | Boş bırakın |

    ![Olay kaynağı oluşturma](./media/iot-accelerators-time-series-insights/time-series-insights-event-source-create.png)

1. **Oluştur**’a tıklayın.

> [!NOTE]
> Zaman serisi Öngörüler Gezgini'ne ek kullanıcılar erişim gerekiyorsa, aşağıdaki adımları kullanabilirsiniz [veri erişim](https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-data-access#grant-data-access).

## <a name="time-series-insights-explorer"></a>Time Series Insights Gezgini

Zaman serisi Öngörüler explorer veri görselleştirmeleri oluşturmanıza yardımcı olan bir web uygulamasıdır.

1. Seçin **genel bakış** sekmesi.

1. Tıklatın **ortam Git**, zaman serisinin Öngörüler explorer web uygulaması açılır.

    ![Time Series Insights Gezgini](./media/iot-accelerators-time-series-insights/time-series-insights-environment.png)

1. Saat seçimi panelinde seçin **son 12 saat** menüsüne ve ardından kez hızlı gelen **arama**.

    ![Zaman serisi Öngörüler Explorer arama](./media/iot-accelerators-time-series-insights/time-series-insights-search-time.png)

1. Sol taraftaki koşulları panelinde, bir ölçü birimi değeri seçin **sıcaklık** ve bölünmüş tarafından değerini **bağlantı cihaz kimliği ıothub**.

    ![Zaman serisi Öngörüler Explorer sorgu](./media/iot-accelerators-time-series-insights/time-series-insights-query1.png)

1. Sağ tıklatın ve grafik **olayları keşfedin**.

    ![Zaman serisi Öngörüler Explorer olayları](./media/iot-accelerators-time-series-insights/time-series-insights-explore-events.png)

1. Olayları kılavuzda tablo biçiminde işlenir.

    ![Zaman serisi Öngörüler Explorer tablosu](./media/iot-accelerators-time-series-insights/time-series-insights-table.png)

1. Perspektif Görünüm düğmesini tıklatın.

    ![Zaman serisi Öngörüler Explorer perspektifi](./media/iot-accelerators-time-series-insights/time-series-insights-explorer-perspective.png)

1. Tıklatın **Ekle** perspektife yeni bir sorgu oluşturmak için.

    ![Zaman serisi Öngörüler Explorer sorgu ekleme](./media/iot-accelerators-time-series-insights/time-series-insights-new-query.png)

1. Bir hızlı saatini seçin **son 12 saat**, bir ölçü **nem** ve bir bölme tarafından **bağlantı cihaz kimliği ıothub**.

    ![Zaman serisi Öngörüler Explorer sorgu](./media/iot-accelerators-time-series-insights/time-series-insights-query2.png)

1. Panonuz aygıt ölçümleri görüntülemek üzere perspektif Görünüm düğmesini tıklatın.

    ![Zaman serisi öngörüleri Explorer Panosu](./media/iot-accelerators-time-series-insights/time-series-insights-dashboard.png)

## <a name="next-steps"></a>Sonraki Adımlar

Keşfetmek ve zaman serisi Öngörüler Explorer'da veri sorgulama hakkında bilgi edinmek için [Azure zaman serisi Öngörüler explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer).
