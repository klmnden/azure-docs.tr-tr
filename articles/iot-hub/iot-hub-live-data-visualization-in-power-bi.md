---
title: Azure IOT Hub-Power BI gelen algılayıcı verilerini gerçek zamanlı veri Görselleştirme | Microsoft Docs
description: Algılayıcıdan toplanan ve Azure IOT hub'ına gönderilen sıcaklık ve nem verileri görselleştirmek için Power BI'ı kullanın.
author: robinsh
keywords: gerçek zamanlı veri görselleştirme, canlı veri görselleştirme, sensör verilerini Görselleştirme
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 6/06/2019
ms.author: robinsh
ms.openlocfilehash: 7deb1b501d30c8af0cb190f4722d46435afa9b8e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67065886"
---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-power-bi"></a>Power BI'ı kullanarak Azure IOT Hub'ından gerçek zamanlı algılayıcı verilerini Görselleştirme

![Uçtan uca diyagramı](./media/iot-hub-live-data-visualization-in-power-bi/1_end-to-end-diagram.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Öğrenecekleriniz

Power BI'ı kullanarak Azure IOT hub'ınızın aldığı gerçek zamanlı algılayıcı verilerini görselleştirme öğrenin. Bir web uygulaması ile IOT hub'ınızdaki verileri görselleştirmek için bkz denemek isterseniz [Azure IOT hub'ı gerçek zamanlı sensör verilerini görselleştirmek için bir web uygulamasını kullanmak](iot-hub-live-data-visualization-in-web-apps.md).

## <a name="what-you-do"></a>Neler

* IOT hub'ınıza bir tüketici grubu ekleyerek veri erişim için hazırlanın.

* Oluşturma, yapılandırma ve veri aktarımı için bir Stream Analytics işi, Power BI hesabınızda IOT hub'ından çalıştırın.

* Oluşturma ve verileri görselleştirmek için Power BI raporu yayımlayın.

## <a name="what-you-need"></a>Ne gerekiyor

* Tamamlamak [Raspberry Pi çevrimiçi simülatör](iot-hub-raspberry-pi-web-simulator-get-started.md) öğretici veya bir cihaz öğreticileri; Örneğin, [node.js ile Raspberry Pi](iot-hub-raspberry-pi-kit-node-get-started.md). Bu makaleler, aşağıdaki gereksinimleri kapsar:
  
  * Etkin bir Azure aboneliği.
  * Azure IOT hub, aboneliğiniz altında.
  * Azure IOT hub'ınıza ileti gönderen bir istemci uygulaması.

* Power BI hesabı. ([Power BI'ı ücretsiz deneyin](https://powerbi.microsoft.com/))

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Oluşturma, yapılandırma ve bir Stream Analytics işini çalıştır

Bir Stream Analytics işi oluşturarak başlayalım. İş oluşturduktan sonra girişler, çıkışlar ve verileri almak için kullanılan sorgu tanımlayın.

### <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

1. İçinde [Azure portalında](https://portal.azure.com)seçin **kaynak Oluştur** > **nesnelerin interneti** > **Stream Analytics işi**.

2. İş için aşağıdaki bilgileri girin.

   **İş adı**: İş adı. Adın genel olarak benzersiz olması gerekir.

   **Kaynak grubu**: IOT hub'ınıza kullandığı aynı kaynak grubunu kullanın.

   **Konum**: Kaynak grubunuzun aynı konumu kullanın.

   ![Azure'da bir Stream Analytics işi oluşturma](./media/iot-hub-live-data-visualization-in-power-bi/create-stream-analytics-job-azure.png)

3. **Oluştur**’u seçin.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Stream Analytics işine giriş ekleme

1. Stream Analytics işini açın.

2. Altında **iş topolojisi**seçin **girişleri**.

3. İçinde **girişleri** bölmesinde **akış Girişi Ekle**, ardından **IOT hub'ı** aşağı açılan listeden. Yeni Giriş bölmesinde aşağıdaki bilgileri girin:

   **Giriş diğer adı**: Giriş için benzersiz bir diğer ad girin.

   **IOT hub'ı aboneliğinizden sağlamak**: Bu radyo düğmesini seçin.

   **Abonelik**: Bu öğretici için kullandığınız Azure aboneliğini seçin.

   **IOT hub'ı**: Bu öğretici için kullandığınız IOT Hub'ı seçin.

   **Uç nokta**: Seçin **Mesajlaşma**.

   **Paylaşılan erişim ilkesi adı**: Stream Analytics işi IOT hub'ınız için kullanmak istediğiniz paylaşılan erişim ilkesinin adını seçin. Bu öğretici için seçtiğiniz *hizmet*. *Hizmet* ilke yeni IOT hub'ları üzerinde varsayılan olarak oluşturulur ve gönderme ve IOT hub tarafından sunulan bulut tarafındaki uç noktalarda alma izni verir. Daha fazla bilgi için bkz. [erişim denetimi ve izinleri](iot-hub-devguide-security.md#access-control-and-permissions).

   **Paylaşılan Erişim İlkesi anahtarı**: Bu paylaşılan erişim ilkesi adı için yaptığınız seçime göre otomatik olarak doldurulan bir alandır.

   **Tüketici grubu**: Daha önce oluşturduğunuz tüketici grubu seçin.

   Tüm diğer alanları varsayılan olarak bırakın.

   ![Azure'da bir giriş için bir Stream Analytics işi ekleme](./media/iot-hub-live-data-visualization-in-power-bi/add-input-to-stream-analytics-job-azure.png)

4. **Kaydet**’i seçin.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Stream Analytics işine çıkış ekleme

1. Altında **iş topolojisi**seçin **çıkışları**.

2. İçinde **çıkışları** bölmesinde **Ekle** ve **Power BI**.

3. Üzerinde **Power BI - yeni çıkış** bölmesinde **Authorize** ve Power BI hesabınızda oturum açmak için istemleri takip edin.

4. Power BI'da açtıktan sonra aşağıdaki bilgileri girin:

   **Çıkış diğer adı**: Çıkış için bir benzersiz diğer adı.

   **Grup çalışma alanı**: Hedef grup çalışma alanınızı seçin.

   **Veri kümesi adı**: Bir veri kümesi adı girin.

   **Tablo adı**: Tablo adı girin.

   ![Azure Stream Analytics işinde bir çıktı ekleyin](./media/iot-hub-live-data-visualization-in-power-bi/add-output-to-stream-analytics-job-azure.png)

5. **Kaydet**’i seçin.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Stream Analytics işinin sorgusunu yapılandırma

1. **İş Topolojisi**'nin altında **Sorgu**'yu seçin.

2. `[YourInputAlias]` değerini işin giriş diğer adıyla değiştirin.

3. `[YourOutputAlias]` değerini işin çıkış diğer adıyla değiştirin.

   ![Azure Stream Analytics işinde sorgu ekleme](./media/iot-hub-live-data-visualization-in-power-bi/add-query-stream-analytics-job-azure.png)

4. **Kaydet**’i seçin.

### <a name="run-the-stream-analytics-job"></a>Stream Analytics işini çalıştırma

Stream Analytics işinde seçin **genel bakış**, ardından **Başlat** > **artık** > **Başlat**. İş düzgün bir şekilde başlatıldıktan sonra, **Durduruldu** olan iş durumu **Çalışıyor** olarak değiştirilir.

![Azure'da bir Stream Analytics işini çalıştır](./media/iot-hub-live-data-visualization-in-power-bi/run-stream-analytics-job-azure.png)

## <a name="create-and-publish-a-power-bi-report-to-visualize-the-data"></a>Oluşturma ve verileri görselleştirmek için Power BI raporu yayımlama

1. Örnek uygulamanın, Cihazınızda çalıştığından emin olun. Öğreticiler altında başvurabilirsiniz değil, varsa [Cihazınızı ayarlama](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).

2. [Power BI](https://powerbi.microsoft.com/en-us/) hesabınızda oturum açın.

3. Kullandığınız, çalışma alanı seçin **çalışma Alanım**.

4. Seçin **veri kümeleri**.

   Stream Analytics işi için Çıkış'ı oluşturduğunuzda, belirttiğiniz veri kümesi görürsünüz.

5. Oluşturduğunuz veri kümesi, **rapor Ekle** (ilk simge veri kümesi adının sağında).

   ![Microsoft Power BI raporu oluşturma](./media/iot-hub-live-data-visualization-in-power-bi/start-power-bi.png)

6. Zamanla değişen gerçek zamanlı sıcaklığı göstermek için bir çizgi grafik oluşturun.

   1. Üzerinde **görselleştirmeler** rapor oluşturma sayfası bölmesinde bir çizgi grafik eklemek için çizgi grafik simgesini seçin.

   2. **Alanlar** bölmesinde, Stream Analytics işi için çıkış oluştururken belirttiğiniz tabloyu genişletin.

   3. **Görselleştirmeler** bölmesinde **EventEnqueuedUtcTime** öğesini **Eksen** üzerine sürükleyin.

   4. **temperature** öğesini **Değerler** üzerine sürükleyin.

      Bir çizgi grafik oluşturulur. Grafiğin x ekseninde UTC saat dilimine göre tarih ve saat görüntülenir. Grafiğin y ekseninde algılayıcıdan alınan sıcaklık görüntülenir.

      ![Bir çizgi grafik sıcaklık için bir Microsoft Power BI raporuna ekleme](./media/iot-hub-live-data-visualization-in-power-bi/power-bi-add-temp.png)

7. Zamanla değişen gerçek zamanlı nem oranını göstermek için bir çizgi grafik daha oluşturun. Bunu yapmak için aynı adımları izleyin ve yerleştirin **EventEnqueuedUtcTime** x eksenindeki ve **nem** y ekseninde.

   ![Bir çizgi grafik nem için bir Microsoft Power BI raporuna ekleme](./media/iot-hub-live-data-visualization-in-power-bi/power-bi-add-humidity.png)

8. Seçin **Kaydet** raporu kaydetmek için.

9. Seçin **raporları** sol bölmesinde ve ardından oluşturduğunuz rapor oluşturulur.

10. Seçin **dosya** > **Web'de Yayımla**.

11. Seçin **ekleme kodu oluştur**ve ardından **Yayımla**.

Rapor erişimi için herkesle paylaşabilirsiniz rapor bağlantısı ve raporu blog'unuza veya Web tümleştirmek için kullanabileceğiniz bir kod parçacığı edinirsiniz.

![Microsoft Power BI raporunu yayımlama](./media/iot-hub-live-data-visualization-in-power-bi/power-bi-publish.png)

Microsoft sunduğu [Power BI mobil uygulamalarında](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) görüntüleme ve Power BI panolarınızı ve raporlarınızı ile etkileşim kurma için mobil Cihazınızda.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT hub'ından gerçek zamanlı sensör verilerini görselleştirmek için Power BI başarıyla kullandınız.

Azure IOT Hub'ından verileri görselleştirmek başka bir yol için bkz: [Azure IOT hub'ı gerçek zamanlı sensör verilerini görselleştirmek için bir web uygulamasını kullanmak](iot-hub-live-data-visualization-in-web-apps.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
