---
title: Azure Data Lake Store ile Uzaktan izleme çözümü tümleşik | Microsoft Docs
description: Uzaktan izleme çözümü kullanarak Azure Data Lake bir Azure akış analizi işi Store ile tümleşik öğrenin.
author: philmea
manager: timlt
ms.author: philmea
ms.date: 04/29/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 3bd29e348fd067c12def8ca36fbdc1d7e35b2874
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34627595"
---
# <a name="integrate-the-remote-monitoring-solution-with-azure-data-lake-store"></a>Uzaktan izleme çözümü Azure Data Lake Store ile tümleştirme

Uzaktan izleme çözümünde sunulan ötesinde analytics gereksinimleri Gelişmiş. Bunu yoğun ve farklı veri kümelerinden veri depolamak yanı sıra için isteğe bağlı analizler sağlamak üzere Azure Data Lake Analytics ile tümleştirmek azure Data Lake Store bu uygulama için idealdir.

Bu yöntem, size bir Azure akış analizi işi veri akışı IOT hub'ından Uzaktan izleme çözümünüz bir Azure Data Lake Store için kullanır.

## <a name="prerequisites"></a>Önkoşullar

Bu yöntem tamamlamak için aşağıdakiler gerekir:

* [Uzaktan izleme Çözüm Hızlandırıcısı dağıtmak](iot-accelerators-remote-monitoring-deploy.md).
  * Uzaktan izleme çözümü, IOT hub'ı ve bu makalede Azure aboneliğinize kullanılan Azure Stream Analytics işi dağıtır.
* [Bir Azure Data Lake Store dağıtma](../data-lake-store/data-lake-store-get-started-portal.md)
  * Data Lake Store, Uzaktan izleme çözümünüz aynı bölgeye dağıtılmalıdır.
  * [Bir klasör oluşturun](../data-lake-store/data-lake-store-get-started-portal.md#createfolder) "hesabınızı akış" adlı.

## <a name="create-a-consumer-group"></a>Bir tüketici grubu oluştur

Uzaktan izleme çözümünüz IOT hub, ayrılmış bir tüketici grubu oluşturun. Bu akış analizi işi tarafından Data Lake Store için veri akışı için kullanılır.

> [!NOTE]
> Tüketici grupları, Azure IOT Hub'ından veri çekmek için uygulamalar tarafından kullanılır. Her beş çıkış Tüketiciler için yeni bir tüketici grubu oluşturmanız gerekir. En fazla 32 tüketici grupları oluşturabilirsiniz.

1. Azure Portal’da oturum açın.

1. Azure portalında tıklatın **bulut Kabuk** düğmesi.

    ![Portal başlatma simgesi](./media/iot-accelerators-integrate-data-lake/portal-launch-icon.png)

1. Yeni bir tüketici grubu oluşturmak için bu komutu çalıştırın:

```azurecli-interactive
az iot hub consumer-group create --hub-name contoso-rm30263 --name streamanalyticsjob --resource-group contoso-rm
```

> [!NOTE]
> Kaynak grubu ve IOT hub'ı Uzaktan izleme çözümünüz adlarından kullanın.

## <a name="create-stream-analytics-job"></a>Akış analizi işi oluşturma

IOT hub'ınızı verileri Azure Data Lake deponuza akışı için bir Azure akış analizi işi oluşturun.

1. Tıklatın **kaynak oluşturma**, nesnelerin interneti marketten seçin ve tıklatın **Stream Analytics işi**.

    ![Yeni akış analizi işi](./media/iot-accelerators-integrate-data-lake/new-stream-analytics-job.png)

1. Bir iş adı girin ve uygun abonelik ve kaynak grubunu seçin.

1. Bir konum seçin yakın veya Data Lake Store ile aynı bölgede. Doğu ABD burada kullanıyoruz.

1. Barındırma ortamı varsayılan olarak bırakmak için olun **bulut**.

1. **Oluştur**’a tıklayın.

    ![Akış analizi işi oluşturma](./media/iot-accelerators-integrate-data-lake/create-stream-analytics-job.png)

## <a name="configure-the-stream-analytics-job"></a>Stream Analytics işi yapılandırın

1. Git **Stream Analytics işi** Uzaktan izleme çözümünün kaynak grubunuzdaki.

1. Genel bakış sayfasında, tıklatın **girişleri**.

    ![Genel Bakış sayfası](./media/iot-accelerators-integrate-data-lake/stream-analytics-overview.png)

1. Tıklatın **akış giriş Ekle** seçip **IOT hub'ı** açılan gelen.

    ![Giriş Ekle](./media/iot-accelerators-integrate-data-lake/stream-analytics-add-input.png)

1. Yeni Giriş sekmesinde, bir giriş diğer adı girin **Iothub**.

1. Tüketici grubu açılan listeden, daha önce oluşturduğunuz tüketici grubu seçin. Burada kullanıyoruz **streamanalyticsjob**.

    ![Giriş seçin](./media/iot-accelerators-integrate-data-lake/stream-analytics-new-input.png)

1. **Kaydet**’e tıklayın.

1. Genel bakış sayfasında, tıklatın **çıkışları**.

    ![Data Lake Store ekleme](./media/iot-accelerators-integrate-data-lake/stream-analytics-overview-2.png)

1. ' I tıklatın **Ekle** seçip **Data Lake Store** gelen açılır.

    ![Çıktı Ekle](./media/iot-accelerators-integrate-data-lake/stream-analytics-output.png)

1. Yeni Çıkış sekmesinde Çıkış diğer adını girin **DataLakeStore**.

1. Önceki adımlarda oluşturduğunuz Data Lake Store hesabını seçin ve veri deposuna akışı için klasör yapısı sağlayın.

1. Tarih biçimi alanı girin **/streaming/ {date} / {time}**. Varsayılan tarih biçimi YYYY/AA/GG ve SS saat biçimi bırakın.

    ![Klasör yapısı sağlayın](./media/iot-accelerators-integrate-data-lake/stream-analytics-new-output.png)

1. Tıklatın **yetkilendirmek**.

    Dosya sistemine Stream analytics işi yazma erişimi vermek için Data Lake Store ile yetkilendirilmesi gerekir.

    ![Akış analizi Data Lake Store'a yetkilendirmek](./media/iot-accelerators-integrate-data-lake/stream-analytics-out-authorize.png)

    Açılan pencere görür ve açılan pencere kapatıldıktan sonra yetkilendirme tamamlandıktan sonra Authorize düğmesi gri görünür.

    > [!NOTE]
    > Açılan pencerede hata görürseniz, Incognito modunda yeni bir tarayıcı penceresi açın ve yeniden deneyin.

1. **Kaydet**’e tıklayın.

## <a name="edit-the-stream-analytics-query"></a>Stream Analytics Sorgu Düzenle

Azure Stream Analytics SQL benzeri bir sorgu dildir veri akışlarını bir giriş kaynağı belirtin, istenen ve depolama veya işleme hedeflerine çeşitli çıkış olarak bu verileri dönüştürmek için kullanır.

1. Genel sekmesinde, **düzenleme sorgu**.

    ![Sorguyu Düzenle](./media/iot-accelerators-integrate-data-lake/stream-analytics-edit-query.png)

1. Sorgu Düzenleyicisi'nde [YourOutputAlias] değiştirin ve önceden tanımlanmış değerler [YourInputAlias] yer tutucularını.

    ```sql
    SELECT
        *, System.Timestamp as time
    INTO
        DataLakeStore
    FROM
        IoTHub
    ```

    ![Stream Analytics sorgusu](./media/iot-accelerators-integrate-data-lake/stream-analytics-query.png)

1. **Kaydet**’e tıklayın.
1. Tıklatın **Evet** değişiklikleri kabul etmek için.

## <a name="start-the-stream-analytics-job"></a>Stream Analytics işi Başlat

1. Genel sekmesinde, **Başlat**.

    ![Akış analizi işi Başlat](./media/iot-accelerators-integrate-data-lake/stream-analytics-start.png)

1. Başlangıç iş sekmesinde, **özel**.

1. Akış Cihazınızı ne zaman başlatıldığını verileri almak için birkaç saat geri dönmek için özel saatini ayarlayın.

1. Tıklatın **Başlat**.

    ![Özel tarih seçin](./media/iot-accelerators-integrate-data-lake/stream-analytics-start-custom.png)

    Sorgudan, olabilir hatalar görürseniz, işi çalışır durumda, içine gider kadar bekleyin sözdiziminin doğru olup doğruladığınızdan emin olun.

    ![İşi çalıştırma](./media/iot-accelerators-integrate-data-lake/stream-analytics-running.png)

    İş akışında, IOT Hub'ından veri okumak ve Data Lake Store'da verileri depolamak başlar. Verilerin Data Lake Store'da görünmesi başlamak birkaç dakika sürebilir.

## <a name="explore-the-streaming-data"></a>Veri akışı keşfedin

1. Data Lake Store için gidin.

1. Genel sekmesinde, **Veri Gezgini**.

1. Veri Gezgini'ni detayına gitmek **/ akış** klasör. YYYY/AA/GG/HH biçimiyle oluşturulan klasörler görürsünüz.

    ![Veri akışı keşfedin](./media/iot-accelerators-integrate-data-lake/data-lake-store-data-explorer.png)

    Saat başına bir dosyayla json dosyaları görürsünüz.

    ![Veri akışı keşfedin](./media/iot-accelerators-integrate-data-lake/data-lake-store-file-preview.png)

## <a name="next-steps"></a>Sonraki Adımlar

Azure Data Lake Analytics, Data Lake Store veri kümeleri hakkında büyük veri analizi yapmak için kullanılabilir. Daha fazla bilgi almak [Data Lake analizi belgeleri](https://docs.microsoft.com/en-us/azure/data-lake-analytics).
