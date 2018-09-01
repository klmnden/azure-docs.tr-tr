---
title: include dosyası
description: include dosyası
services: iot-accelerators
author: dominicbetts
ms.service: iot-accelerators
ms.topic: include
ms.date: 08/20/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 8c114ed137089e70899e601ebdc1d4d39f562601
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2018
ms.locfileid: "43383296"
---
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

[Azure Time Series Insights](../articles/time-series-insights/time-series-insights-overview.md) bulutta IOT ölçekli zaman serisi verilerini yönetmek için tam olarak yönetilen bir analiz, depolama ve görselleştirme hizmeti. Yeni bir zaman serisi görüşleri ortamı oluşturmak için:

1. [Azure Portal](http://portal.azure.com/)’da oturum açın.

1. Seçin **kaynak Oluştur** > **nesnelerin interneti** > **Time Series Insights**:

    ![Yeni zaman serisi görüşleri](./media/iot-accelerators-create-tsi/new-time-series-insights.png)

1. Çözüm hızlandırıcınız aynı kaynak grubunda Time Series Insights ortamınızı oluşturmak için aşağıdaki tablodaki değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Ortam adı | Aşağıdaki ekran adı kullanan **Contoso TSI**. Bu adımı tamamladığınızda, kendi benzersiz bir ad seçin. |
    | Abonelik | Açılan listeden Azure aboneliğinizi seçin. |
    | Kaynak grubu | **contoso-simülasyon**. Çözüm hızlandırıcınız adını kullanın. |
    | Konum | Bu örnekte **Doğu ABD**. Ortamınızı, cihaz benzetimi Hızlandırıcı ile aynı bölgede oluşturun. |
    | Sku |**S1** |
    | Kapasite | **1** |

    ![Zaman serisi görüşleri oluşturma](./media/iot-accelerators-create-tsi/new-time-series-insights-create.png)

    > [!NOTE]
    > Çözüm Hızlandırıcısını aynı kaynak grubunda ortama, zaman serisi görüşleri eklemeyi çözüm Hızlandırıcısını sildiğinizde silinmeden anlamına gelir.

1. **Oluştur**’a tıklayın. Bu ortamın oluşturulması birkaç dakika sürebilir.

## <a name="create-event-source"></a>Olay kaynağı oluşturma

IOT hub'ınıza bağlanmak için yeni bir olay kaynağı oluşturun. Önceki adımlarda oluşturduğunuz tüketici grubu kullanın. Zaman serisi görüşleri olay kaynağı başka bir hizmet tarafından kullanılmayan bir ayrılmış bir tüketici grubu gerektirir.

1. Azure Portal'da yeni zaman serisi ortamınıza gidin.

1. Sol tarafta, tıklayın **olay kaynakları**:

    ![Olay kaynakları görüntüle](./media/iot-accelerators-create-tsi/time-series-insights-event-sources.png)

1. Tıklayın **ekleme**:

    ![Olay kaynağı ekleme](./media/iot-accelerators-create-tsi/time-series-insights-event-sources-add.png)

1. IOT hub'ınıza yeni bir olay kaynağı yapılandırmak için aşağıdaki tablodaki değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Olay kaynağı adı | Aşağıdaki ekran adı kullanan **contoso IOT hub**. Bu adımı tamamladığınızda, kendi benzersiz bir ad kullanın. |
    | Kaynak | **IoT Hub’ı** |
    | İçeri aktarma seçeneği | **Mevcut aboneliklerden IOT hub'ı kullanın** |
    | Abonelik Kimliği | Açılan listeden Azure aboneliğinizi seçin. |
    | IOT hub'ı adı | **contoso simulation7d894**. Cihaz benzetimi çözüm hızlandırıcınız gelen IOT hub'ınızın adını kullanın. |
    | IOT hub'ı ilke adı | **iothubowner** |
    | IOT hub'ı ilke anahtarı | Bu alan otomatik olarak doldurulur. |
    | IOT hub tüketici grubu | **devicesimulationtsi** |
    | Olay serileştirme biçimi | **JSON** |
    | Zaman damgası özellik adı | Boş bırakın |

    ![Olay kaynağı oluşturma](./media/iot-accelerators-create-tsi/time-series-insights-event-source-create.png)

1. **Oluştur**’a tıklayın.

> [!NOTE]
> Yapabilecekleriniz [ek kullanıcılara erişim verme](../articles/time-series-insights/time-series-insights-data-access.md#grant-data-access) Time Series Insights Gezgini.