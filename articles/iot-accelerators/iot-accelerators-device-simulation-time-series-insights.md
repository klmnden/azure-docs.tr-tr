---
title: Time Series Insights - Azure ile cihaz benzetimi telemetri Görselleştirme | Microsoft Docs
description: Time Series Insights ortamınızı keşfedin ve cihaz benzetimi çözüm Hızlandırıcı tarafından oluşturulan telemetri analiz etmek için yapılandırmayı öğrenin.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.date: 08/20/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 5d20adc11e0d679e12fd060e719593a50180db8e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65834764"
---
# <a name="use-time-series-insights-to-visualize-telemetry-sent-from-the-device-simulation-solution-accelerator"></a>Cihaz benzetimi çözüm hızlandırıcıdan gönderilen telemetri görselleştirmek için Time Series Insights'ı kullanın

Cihaz benzetimi çözüm Hızlandırıcısını IOT çözümlerinizi test etmek için sanal cihazlardan telemetri oluşturmanıza olanak sağlar. Bu nasıl yapılır kılavuzunda görselleştirmesine ve analiz kullanarak zaman serisi görüşleri ortamına sanal telemetri gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır Kılavuzu'ndaki adımları takip etmek için bir etkin Azure aboneliği gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu nasıl yapılır Kılavuzu'ndaki adımları, Azure aboneliğinize cihaz benzetimi çözüm Hızlandırıcısını dağıttınız varsayılır. Çözüm Hızlandırıcısını dağıtmadıysanız adımları [dağıtma ve çalıştırma bir bulut tabanlı cihaz benzetimi çözümü](quickstart-device-simulation-deploy.md) hızlı başlangıç.

Bu makalede, Çözüm Hızlandırıcısı adı olduğunu varsayar **contoso-simülasyon**. Değiştirin **contoso-simülasyon** çözüm hızlandırıcınız aynı ada sahip aşağıdaki adımları tamamlayın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-consumer-group"></a>Bir tüketici grubu oluşturun

IOT hub'ınıza zaman serisi öngörüleri için akış telemetri ayrılmış bir tüketici grubu oluşturmanız gerekir. Zaman serisi görüşleri'nde bir olay kaynağı, bir IOT Hub tüketici grubu özel kullanımı olması gerekir.

Azure CLI, tüketici grubu oluşturmak için aşağıdaki adımları Azure Cloud Shell'de kullanın:

1. IOT hub cihaz benzetimi çözüm Hızlandırıcısını dağıttığınızda oluşturulan çeşitli kaynaklar biridir. Aşağıdaki komut Bul IOT hub'ınızın adını execute - çözüm hızlandırıcınız adını kullanmanız gerektiğini unutmayın:

    ```azurecli-interactive
    az resource list --resource-group contoso-simulation -o table
    ```

    IOT hub'ı türünde bir kaynaktır **Microsoft.Devices/ıothubs**.

1. Adlı bir tüketici grubu Ekle **devicesimulationtsi** hub. Aşağıdaki komutta, hub ve çözüm Hızlandırıcı adını kullanın:

    ```azurecli-interactive
    az iot hub consumer-group create --hub-name contoso-simulation7d894 --name devicesimulationtsi --resource-group contoso-simulation
    ```

    Artık Azure Cloud Shell'i kapatabilirsiniz.

## <a name="create-a-new-time-series-insights-environment"></a>Yeni bir zaman serisi görüşleri ortamı oluşturma

[Azure Time Series Insights](../../articles/time-series-insights/time-series-insights-overview.md) bulutta IOT ölçekli zaman serisi verilerini yönetmek için tam olarak yönetilen bir analiz, depolama ve görselleştirme hizmeti. Yeni bir zaman serisi görüşleri ortamı oluşturmak için:

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Seçin **kaynak Oluştur** > **nesnelerin interneti** > **Time Series Insights**:

    ![Yeni zaman serisi görüşleri](./media/iot-accelerators-device-simulation-time-series-insights/new-time-series-insights.png)

1. Çözüm hızlandırıcınız aynı kaynak grubunda Time Series Insights ortamınızı oluşturmak için aşağıdaki tablodaki değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Ortam adı | Aşağıdaki ekran adı kullanan **Contoso TSI**. Bu adımı tamamladığınızda, kendi benzersiz bir ad seçin. |
    | Abonelik | Açılan listeden Azure aboneliğinizi seçin. |
    | Kaynak grubu | **contoso-simülasyon**. Çözüm hızlandırıcınız adını kullanın. |
    | Location | Bu örnekte **Doğu ABD**. Ortamınızı, cihaz benzetimi Hızlandırıcı ile aynı bölgede oluşturun. |
    | Sku |**S1** |
    | Kapasite | **1** |

    ![Zaman serisi görüşleri oluşturma](./media/iot-accelerators-device-simulation-time-series-insights/new-time-series-insights-create.png)

    > [!NOTE]
    > Çözüm Hızlandırıcısını aynı kaynak grubunda ortama, zaman serisi görüşleri eklemeyi çözüm Hızlandırıcısını sildiğinizde silinmeden anlamına gelir.

1. **Oluştur**’a tıklayın. Bu ortamın oluşturulması birkaç dakika sürebilir.

## <a name="create-event-source"></a>Olay kaynağı oluşturma

IOT hub'ınıza bağlanmak için yeni bir olay kaynağı oluşturun. Önceki adımlarda oluşturduğunuz tüketici grubu kullanın. Zaman serisi görüşleri olay kaynağı başka bir hizmet tarafından kullanılmayan bir ayrılmış bir tüketici grubu gerektirir.

1. Azure Portal'da yeni zaman serisi ortamınıza gidin.

1. Sol tarafta, tıklayın **olay kaynakları**:

    ![Olay kaynakları görüntüle](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-event-sources.png)

1. Tıklayın **ekleme**:

    ![Olay kaynağı ekleme](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-event-sources-add.png)

1. IOT hub'ınıza yeni bir olay kaynağı yapılandırmak için aşağıdaki tablodaki değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Olay kaynağı adı | Aşağıdaki ekran adı kullanan **contoso IOT hub**. Bu adımı tamamladığınızda, kendi benzersiz bir ad kullanın. |
    | source | **IoT Hub’ı** |
    | İçeri aktarma seçeneği | **Mevcut aboneliklerden IOT hub'ı kullanın** |
    | Abonelik Kimliği | Açılan listeden Azure aboneliğinizi seçin. |
    | IOT hub'ı adı | **contoso simulation7d894**. Cihaz benzetimi çözüm hızlandırıcınız gelen IOT hub'ınızın adını kullanın. |
    | IOT hub'ı ilke adı | **iothubowner** |
    | IOT hub'ı ilke anahtarı | Bu alan otomatik olarak doldurulur. |
    | IOT hub tüketici grubu | **devicesimulationtsi** |
    | Olay serileştirme biçimi | **JSON** |
    | Zaman damgası özellik adı | Boş bırakın |

    ![Olay kaynağı oluşturma](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-event-source-create.png)

1. **Oluştur**’a tıklayın.

> [!NOTE]
> Yapabilecekleriniz [ek kullanıcılara erişim verme](../../articles/time-series-insights/time-series-insights-data-access.md#grant-data-access) Time Series Insights Gezgini.

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

## <a name="clean-up-resources"></a>Kaynakları temizleme

Daha fazla keşfetmeye devam etmeyi planlıyorsanız, dağıtılan çözüm Hızlandırıcısını bırakın.

Çözüm Hızlandırıcısını artık ihtiyacınız kalmadığında, silmek [sağlanan çözümleri](https://www.azureiotsolutions.com/Accelerators#dashboard) sayfasında, seçerek ve ardından **Sil çözüm**.

Çözüm Hızlandırıcısını ait kaynak grubu için zaman serisi görüşleri ortamına eklediyseniz, çözüm Hızlandırıcısını sildiğinizde otomatik olarak silinir. Aksi takdirde Azure portalından el ile zaman serisi görüşleri ortamına kaldırmanız gerekir.

## <a name="next-steps"></a>Sonraki Adımlar

Keşfedin ve Time Series Insights gezgininin verilerde sorgulama hakkında daha fazla bilgi için bkz. [Azure Time Series Insights gezgininin](../time-series-insights/time-series-insights-explorer.md).
