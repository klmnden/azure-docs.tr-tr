---
title: Data Lake Store - Azure ile Uzaktan izleme çözümünü tümleştirmek | Microsoft Docs
description: Uzaktan izleme çözümü, Azure Data Lake kullanarak bir Azure Stream Analytics işi Store ile tümleştirmeyi öğrenin.
author: philmea
manager: timlt
ms.author: philmea
ms.date: 04/29/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 021f18f588613817110539d408f9260fb9247895
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61449497"
---
# <a name="integrate-the-remote-monitoring-solution-with-azure-data-lake-store"></a>Uzaktan izleme çözümü, Azure Data Lake Store ile tümleştirme

Uzaktan izleme çözümünde sunulan ötesinde analizi gereksinimleri Gelişmiş. Çok büyük ve farklı veri kümelerini deposundan yapabilir, çünkü isteğe bağlı analizler sağlamak için Azure Data Lake Analytics ile tümleştirmek, azure Data Lake Store bu uygulama için idealdir.

Bu nasıl yapılır makalesinde, Azure Stream Analytics işi veri akışı için IOT hub'ından Uzaktan izleme çözümünüze bir Azure Data Lake Store kullanır.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır tamamlamak için aşağıdakiler gerekir:

* [Uzaktan izleme çözüm Hızlandırıcısını dağıtma](quickstart-remote-monitoring-deploy.md).
  * Uzaktan izleme çözümü, IOT hub ve Azure Stream Analytics işi bu makalede Azure aboneliğinize kullanılan dağıtır.
* [Bir Azure Data Lake Store dağıtma](../data-lake-store/data-lake-store-get-started-portal.md)
  * Data Lake Store, Uzaktan izleme çözümünüzü aynı bölgede dağıtılması gerekir.
  * [Bir klasör oluşturun](../data-lake-store/data-lake-store-get-started-portal.md#createfolder) "hesabınızdaki akış" adlı.

## <a name="create-a-consumer-group"></a>Bir tüketici grubu oluşturun

Uzaktan izleme çözümünüzü IOT hub'da ayrılmış bir tüketici grubu oluşturun. Bu Stream Analytics işi tarafından Data Lake Store için veri akışı için kullanılır.

> [!NOTE]
> Tüketici grupları, Azure IOT Hub'ından veri çekmek için uygulamalar tarafından kullanılır. Her beş çıkış Tüketiciler için yeni bir tüketici grubu oluşturmanız gerekir. 32 adede kadar tüketici grubu oluşturabilirsiniz.

1. Azure Portal’da oturum açın.

1. Azure portalında **Cloud Shell** düğmesi.

    ![Portal başlatma simgesi](./media/iot-accelerators-integrate-data-lake/portal-launch-icon.png)

1. Yeni bir tüketici grubu oluşturmak için şu komutu yürütün:

```azurecli-interactive
az iot hub consumer-group create --hub-name contoso-rm30263 --name streamanalyticsjob --resource-group contoso-rm
```

> [!NOTE]
> Kaynak grubu ve IOT hub'ı Uzaktan izleme çözümünüzü adlarından kullanın.

## <a name="create-stream-analytics-job"></a>Stream Analytics işi oluşturma

IOT hub'ınızdaki verileri, Azure Data Lake Store'a akış için bir Azure Stream Analytics işi oluşturun.

1. Tıklayın **kaynak Oluştur**, nesnelerin interneti marketten seçin ve tıklayın **Stream Analytics işi**.

    ![Yeni Stream Analytics işi](./media/iot-accelerators-integrate-data-lake/new-stream-analytics-job.png)

1. Bir proje adı girin ve uygun abonelik ve kaynak grubunu seçin.

1. Bir konum seçin yakın veya, Data Lake Store ile aynı bölgede. Doğu ABD burada kullanıyoruz.

1. Barındırma ortamı varsayılan olarak bırakmak olun **bulut**.

1. **Oluştur**’a tıklayın.

    ![Stream Analytics işi oluşturma](./media/iot-accelerators-integrate-data-lake/create-stream-analytics-job.png)

## <a name="configure-the-stream-analytics-job"></a>Stream Analytics işini yapılandırma

1. Git **Stream Analytics işi** Uzaktan izleme çözümü kaynak grubunuzda.

1. Genel bakış sayfasında tıklayın **girişleri**.

    ![Genel Bakış sayfası](./media/iot-accelerators-integrate-data-lake/stream-analytics-overview.png)

1. Tıklayın **akış Girişi Ekle** seçip **IOT hub'ı** açılır listeden.

    ![Girişi Ekle](./media/iot-accelerators-integrate-data-lake/stream-analytics-add-input.png)

1. Yeni Giriş sekmesinde, bir giriş diğer adı girin **IoTHub**.

1. Tüketici grubu açılan listeden, daha önce oluşturduğunuz tüketici grubu seçin. Burada kullandığımız **streamanalyticsjob**.

    ![Giriş'i seçin](./media/iot-accelerators-integrate-data-lake/stream-analytics-new-input.png)

1. **Kaydet**’e tıklayın.

1. Genel bakış sayfasında tıklayın **çıkışları**.

    ![Data Lake Store Ekle](./media/iot-accelerators-integrate-data-lake/stream-analytics-overview-2.png)

1. Tıklayın **Ekle** seçip **Data Lake Store** açılır listeden.

    ![Çıkış Ekle](./media/iot-accelerators-integrate-data-lake/stream-analytics-output.png)

1. Yeni Çıkış sekmesinde bir çıkış diğer adını girin **DataLakeStore**.

1. Önceki adımlarda oluşturduğunuz Data Lake Store hesabı seçin ve klasör yapısını Store'a veri akışı sağlar.

1. Tarih biçimi alanına **/streaming/ {date} / {time}** . YYYY/AA/GG biçimi varsayılan tarih ve saat biçimi SS bırakın.

    ![Klasör yapısı sağlar](./media/iot-accelerators-integrate-data-lake/stream-analytics-new-output.png)

1. Tıklayın **yetkilendirmek**.

    Dosya sistemine Stream analytics işi yazma erişimi vermek için Data Lake Store ile yetkilendirme gerekecektir.

    ![Data Lake Store Stream Analytics'e Yetkilendir](./media/iot-accelerators-integrate-data-lake/stream-analytics-out-authorize.png)

    Açılır pencere görürsünüz ve açılan pencere kapandıktan sonra yetkilendirme tamamlandıktan sonra Authorize düğmesi gri görünür.

    > [!NOTE]
    > Açılan penceresinde bir hata görürseniz, gizli modunda yeni bir tarayıcı penceresi açın ve yeniden deneyin.

1. **Kaydet**’e tıklayın.

## <a name="edit-the-stream-analytics-query"></a>Stream Analytics Sorgu Düzenle

Azure Stream Analytics SQL benzeri bir sorgu dilini veri akışları bir giriş kaynağı belirtin, istenen ve depolama veya işleme hedefleri çeşitli çıkış olarak bu verileri dönüştürmek için kullanır.

1. Genel Bakış sekmesinde, **sorguyu Düzenle**.

    ![Sorguyu Düzenle](./media/iot-accelerators-integrate-data-lake/stream-analytics-edit-query.png)

1. Sorgu Düzenleyicisi'nde değiştirme [YourOutputAlias] ve [YourInputAlias] yer tutucuları daha önce tanımladığınız değerlere sahip.

    ```sql
    SELECT
        *, System.Timestamp as time
    INTO
        DataLakeStore
    FROM
        IoTHub
    ```

    ![Stream Analytics Query](./media/iot-accelerators-integrate-data-lake/stream-analytics-query.png)

1. **Kaydet**’e tıklayın.
1. Tıklayın **Evet** değişiklikleri kabul etmek için.

## <a name="start-the-stream-analytics-job"></a>Stream Analytics işini başlatın

1. Genel Bakış sekmesinde, **Başlat**.

    ![Stream Analytics işini başlatın](./media/iot-accelerators-integrate-data-lake/stream-analytics-start.png)

1. Başlangıç işi sekmesine tıklayın **özel**.

1. Birkaç saat akış Cihazınızı ne zaman başladı verileri yerden devam edebiliyorduk dönmek için özel saatini ayarlayın.

1. **Başlat**'a tıklayın.

    ![Özel bir tarih seçin](./media/iot-accelerators-integrate-data-lake/stream-analytics-start-custom.png)

    Sorgunuzdan, olabilir hatalar görürseniz, işi durumu, çalışma moduna gider bekleyin sözdiziminin doğru olduğunu doğrulamak emin olun.

    ![Çalışan iş](./media/iot-accelerators-integrate-data-lake/stream-analytics-running.png)

    Akış işi verileri IOT Hub'ından okumak ve verileri depolamak, Data Lake Store içinde başlayacak. Bu veriler, Data Lake Store içinde görüntülenecek başlamak için birkaç dakika sürebilir.

## <a name="explore-the-streaming-data"></a>Akış verilerini keşfedin

1. Data Lake Store için gidin.

1. Genel Bakış sekmesinde, **Veri Gezgini**.

1. Veri Gezgini'nde Detaya Git **/ akış** klasör. YYYY/MM/DD/HH biçimiyle oluşturulan klasörü göreceksiniz.

    ![Akış verilerini keşfedin](./media/iot-accelerators-integrate-data-lake/data-lake-store-data-explorer.png)

    Json dosyaları saat başına bir dosya görürsünüz.

    ![Akış verilerini keşfedin](./media/iot-accelerators-integrate-data-lake/data-lake-store-file-preview.png)

## <a name="next-steps"></a>Sonraki Adımlar

Azure Data Lake Analytics, Data Lake Store veri kümelerinde büyük veri analizi gerçekleştirmek için kullanılabilir. Daha fazla bilgi [Data Lake Analytics belgeleri](https://docs.microsoft.com/azure/data-lake-analytics).
