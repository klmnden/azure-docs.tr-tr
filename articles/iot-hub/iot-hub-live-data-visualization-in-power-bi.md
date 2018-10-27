---
title: Azure IOT Hub-Power BI gelen algılayıcı verilerini gerçek zamanlı veri Görselleştirme | Microsoft Docs
description: Algılayıcıdan toplanan ve Azure IOT hub'ına gönderilen sıcaklık ve nem verileri görselleştirmek için Power BI'ı kullanın.
author: rangv
keywords: gerçek zamanlı veri görselleştirme, canlı veri görselleştirme, sensör verilerini Görselleştirme
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 4/11/2018
ms.author: rangv
ms.openlocfilehash: a533bd6ee447479f08add23833bf5acdde5c4d40
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50155108"
---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-power-bi"></a>Power BI'ı kullanarak Azure IOT Hub'ından gerçek zamanlı algılayıcı verilerini Görselleştirme

![Uçtan uca diyagramı](./media/iot-hub-live-data-visualization-in-power-bi/1_end-to-end-diagram.png)


[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Öğrenecekleriniz

Power BI'ı kullanarak Azure IOT hub'ınızın aldığı gerçek zamanlı algılayıcı verilerini görselleştirme öğrenin. Toe denemek istiyorsanız, Web Apps ile IOT hub'ınızdaki verileri görselleştirebilir, lütfen bakın [Azure IOT hub'ı gerçek zamanlı sensör verilerini görselleştirmek için kullanım Azure Web Apps](iot-hub-live-data-visualization-in-web-apps.md).

## <a name="what-you-do"></a>Neler

* IOT hub'ınıza bir tüketici grubu ekleyerek veri erişim için hazırlanın.

* Oluşturma, yapılandırma ve veri aktarımı için bir Stream Analytics işi, Power BI hesabınızda IOT hub'ından çalıştırın.

* Oluşturma ve verileri görselleştirmek için Power BI raporu yayımlayın.

## <a name="what-you-need"></a>Ne gerekiyor

 * Öğretici [Cihazınızı ayarlama](iot-hub-raspberry-pi-kit-node-get-started.md) tamamlandı, aşağıdaki gereksinimleri ele alınmaktadır:

   * Etkin bir Azure aboneliği.
   * Azure IOT hub, aboneliğiniz altında.
   * Azure IOT hub'ınıza ileti gönderen bir istemci uygulaması.

* Power BI hesabı. ([Power BI'ı ücretsiz deneyin](https://powerbi.microsoft.com/))

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Oluşturma, yapılandırma ve bir Stream Analytics işini çalıştır

Bir Stream Analytics işi oluşturarak başlayalım. İş oluşturduktan sonra girişler, çıkışlar ve verileri almak için kullanılan sorgu tanımlayın.

### <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

1. [Azure portalında](https://portal.azure.com), **Kaynak oluştur** > **Nesnelerin İnterneti** > **Stream Analytics işi**'ne tıklayın.

2. İş için aşağıdaki bilgileri girin.

   **İş adı**: İşin adı. Adın genel olarak benzersiz olması gerekir.

   **Kaynak grubu**: IOT hub'ınıza kullandığı aynı kaynak grubunu kullanın.

   **Konum**: kaynak grubunuzun aynı konumu kullanın.

   **Panoya sabitle**: Panodan IoT hub'ınıza kolay erişim için bu seçeneği işaretleyin.

   ![Azure'da bir Stream Analytics işi oluşturma](./media/iot-hub-live-data-visualization-in-power-bi/2_create-stream-analytics-job-azure.png)

3. **Oluştur**’a tıklayın.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Stream Analytics işine giriş ekleme

1. Stream Analytics işini açın.

2. **İş Topolojisi**'nin altında **Girişler**'e tıklayın.

3. İçinde **girişleri** bölmesinde tıklayın **Ekle**ve ardından aşağıdaki bilgileri girin:

   **Giriş diğer adı**: Giriş benzersiz diğer adı.

   **Kaynak**: seçin **IOT hub'ı**.

   **Tüketici grubu**: oluşturduğunuz tüketici grubu seçin.

4. **Oluştur**’a tıklayın.

   ![Azure'da bir giriş için bir Stream Analytics işi ekleme](./media/iot-hub-live-data-visualization-in-power-bi/3_add-input-to-stream-analytics-job-azure.png)

### <a name="add-an-output-to-the-stream-analytics-job"></a>Stream Analytics işine çıkış ekleme

1. **İş Topolojisi**'nin altında **Çıkışlar**'a tıklayın.

2. İçinde **çıkışları** bölmesinde tıklayın **Ekle**ve ardından aşağıdaki bilgileri girin:

   **Çıkış diğer adı**: Çıkışın benzersiz diğer adı.

   **Havuz**: seçin **Power BI**.

3. Tıklayın **Authorize**ve ardından Power BI hesabınızda oturum açın.

4. Yetkilendirmenin ardından, aşağıdaki bilgileri girin:

   **Grup çalışma alanı**: hedef grup çalışma alanınızı seçin.

   **Veri kümesi adı**: bir veri kümesi adı girin.

   **Tablo adı**: Tablo adı girin.

5. **Oluştur**’a tıklayın.

   ![Azure Stream Analytics işinde bir çıktı ekleyin](./media/iot-hub-live-data-visualization-in-power-bi/4_add-output-to-stream-analytics-job-azure.png)

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Stream Analytics işinin sorgusunu yapılandırma

1. **İş Topolojisi**'nin altında **Sorgu**'ya tıklayın.

2. `[YourInputAlias]` değerini işin giriş diğer adıyla değiştirin.

3. `[YourOutputAlias]` değerini işin çıkış diğer adıyla değiştirin.

4. **Kaydet**’e tıklayın.

   ![Azure Stream Analytics işinde sorgu ekleme](./media/iot-hub-live-data-visualization-in-power-bi/5_add-query-stream-analytics-job-azure.png)

### <a name="run-the-stream-analytics-job"></a>Stream Analytics işini çalıştırma

Stream Analytics işinde **Başlat** > **Şimdi** > **Başlat**'a tıklayın. İş düzgün bir şekilde başlatıldıktan sonra, **Durduruldu** olan iş durumu **Çalışıyor** olarak değiştirilir.

![Azure'da bir Stream Analytics işini çalıştır](./media/iot-hub-live-data-visualization-in-power-bi/6_run-stream-analytics-job-azure.png)

## <a name="create-and-publish-a-power-bi-report-to-visualize-the-data"></a>Oluşturma ve verileri görselleştirmek için Power BI raporu yayımlama

1. Örnek uygulamanın, Cihazınızda çalıştığından emin olun. Öğreticiler altında başvurabilirsiniz değil, varsa [Cihazınızı ayarlama](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).

2. [Power BI](https://powerbi.microsoft.com/en-us/) hesabınızda oturum açın.

3. Stream Analytics işine ilişkin çıkış oluştururken ayarladığınız Grup çalışma alanına gidin.

4. Tıklayın **akış veri kümeleri**.

   Stream Analytics işi için çıkış oluştururken belirttiğiniz veri kümesinin listelendiğini görüyor olmalısınız.

5. **EYLEMLER**'in altında, ilk simgeye tıklayarak bir rapor oluşturun.

   ![Microsoft Power BI raporu oluşturma](./media/iot-hub-live-data-visualization-in-power-bi/7_create-power-bi-report-microsoft.png)

6. Zamanla değişen gerçek zamanlı sıcaklığı göstermek için bir çizgi grafik oluşturun.

   1. Rapor oluşturma sayfasındaki bir çizgi grafiğe ekleyin.

   2. **Alanlar** bölmesinde, Stream Analytics işi için çıkış oluştururken belirttiğiniz tabloyu genişletin.
   
   3. **Görselleştirmeler** bölmesinde **EventEnqueuedUtcTime** öğesini **Eksen** üzerine sürükleyin.
   
   4. **temperature** öğesini **Değerler** üzerine sürükleyin.

      Bir çizgi grafik oluşturulur. Grafiğin x ekseninde UTC saat dilimine göre tarih ve saat görüntülenir. Grafiğin y ekseninde algılayıcıdan alınan sıcaklık görüntülenir.

      ![Bir çizgi grafik sıcaklık için bir Microsoft Power BI raporuna ekleme](./media/iot-hub-live-data-visualization-in-power-bi/8_add-line-chart-for-temperature-to-power-bi-report-microsoft.png)

7. Zamanla değişen gerçek zamanlı nem oranını göstermek için bir çizgi grafik daha oluşturun. Bunu yapmak için aynı adımları izleyin ve yerleştirin **EventEnqueuedUtcTime** x eksenindeki ve **nem** y ekseninde.

   ![Bir çizgi grafik nem için bir Microsoft Power BI raporuna ekleme](./media/iot-hub-live-data-visualization-in-power-bi/9_add-line-chart-for-humidity-to-power-bi-report-microsoft.png)

8. Raporu kaydetmek için **Kaydet**’e tıklayın.

9. Tıklayın **dosya** > **Web'de Yayımla**.

10. Tıklayın **ekleme kodu oluştur**ve ardından **Yayımla**.

Rapor bağlantısı edinirsiniz rapor erişim ve raporu blog'unuza veya Web tümleştirmek için bir kod parçacığı herkesle paylaşabilirsiniz.

![Microsoft Power BI raporunu yayımlama](./media/iot-hub-live-data-visualization-in-power-bi/10_publish-power-bi-report-microsoft.png)

Microsoft sunduğu [Power BI mobil uygulamalarında](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) görüntüleme ve Power BI panolarınızı ve raporlarınızı ile etkileşim kurma için mobil Cihazınızda.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT hub'ından gerçek zamanlı sensör verilerini görselleştirmek için Power BI başarıyla kullandınız.

Azure IOT Hub'ından verileri görselleştirmek için alternatif bir yolu yoktur. Bkz: [Azure IOT hub'ı gerçek zamanlı sensör verilerini görselleştirmek için kullanım Azure Web Apps](iot-hub-live-data-visualization-in-web-apps.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]