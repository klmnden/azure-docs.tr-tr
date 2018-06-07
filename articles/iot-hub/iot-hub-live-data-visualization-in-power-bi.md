---
title: Azure IOT Hub – Power BI algılayıcı verilerini gerçek zamanlı veri Görselleştirme | Microsoft Docs
description: Algılayıcı toplanan ve Azure IOT hub'ına gönderilen sıcaklık ve nem verileri görselleştirmek için Power BI kullanın.
author: rangv
manager: ''
keywords: gerçek zamanlı veri görselleştirme, dinamik veri görselleştirme algılayıcı verileri Görselleştirme
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 4/11/2018
ms.author: rangv
ms.openlocfilehash: a3c54fe635fe0f8988c321684a815e9896922587
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34634354"
---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-power-bi"></a>Azure IOT Hub'ın Power BI kullanarak gerçek zamanlı algılayıcı verilerini görselleştirmek

![Uçtan uca diyagramı](media/iot-hub-get-started-e2e-diagram/4.png)


[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Öğrenecekleriniz

Azure IOT hub'ınızın aldığı Power BI tarafından gerçek zamanlı algılayıcı verilerini görselleştirmek öğrenin. Web uygulamalarını IOT hub'ınızı verileri görselleştirmek denemek istiyorsanız, lütfen bkz [Azure IOT hub'ı gerçek zamanlı algılayıcı verilerini görselleştirmek için kullanım Azure Web Apps](iot-hub-live-data-visualization-in-web-apps.md).

## <a name="what-you-do"></a>Neler

- IOT hub'ınızı bir tüketici grubu ekleyerek veri erişimi için hazırlanın.
- Oluşturmak, yapılandırmak ve Power BI hesabınızı IOT hub'ından veri aktarımı için bir akış analizi işi çalıştırın.
- Oluşturma ve verileri görselleştirmek için bir Power BI raporu yayımlama.

## <a name="what-you-need"></a>Ne gerekiyor

- Öğretici [Cihazınızı](iot-hub-raspberry-pi-kit-node-get-started.md) , aşağıdaki gereksinimleri ele alınmaktadır tamamlandı:
  - Etkin bir Azure aboneliği.
  - Azure IOT hub'ı aboneliğinizdeki.
  - Azure IOT hub'ına iletileri gönderen bir istemci uygulaması.
- Power BI hesabınız. ([Power BI ücretsiz deneyin](https://powerbi.microsoft.com/))

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Oluşturma, yapılandırma ve akış analizi işi çalıştırma

### <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

1. [Azure portalında](https://portal.azure.com), **Kaynak oluştur** > **Nesnelerin İnterneti** > **Stream Analytics işi**'ne tıklayın.
1. İş için aşağıdaki bilgileri girin.

   **İş adı**: İşin adı. Adın genel olarak benzersiz olması gerekir.

   **Kaynak grubu**: IOT hub'ınızı kullandığı aynı kaynak grubunu kullanın.

   **Konum**: aynı konumu, kaynak grubu olarak kullanın.

   **Panoya sabitle**: Panodan IoT hub'ınıza kolay erişim için bu seçeneği işaretleyin.

   ![Stream Analytics işi oluşturma](media/iot-hub-live-data-visualization-in-power-bi/2_create-stream-analytics-job-azure.png)

1. **Oluştur**’a tıklayın.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Stream Analytics işine giriş ekleme

1. Akış analizi işi'ni açın.
1. **İş Topolojisi**'nin altında **Girişler**'e tıklayın.
1. İçinde **girişleri** bölmesinde tıklatın **Ekle**ve ardından aşağıdaki bilgileri girin:

   **Giriş diğer adı**: giriş için benzersiz diğer ad.

   **Kaynak**: seçin **IOT hub'ı**.

   **Tüketici grubu**: yeni oluşturduğunuz tüketici grubu seçin.
1. **Oluştur**’a tıklayın.

   ![Azure akış analizi işine giriş Ekle](media/iot-hub-live-data-visualization-in-power-bi/3_add-input-to-stream-analytics-job-azure.png)

### <a name="add-an-output-to-the-stream-analytics-job"></a>Stream Analytics işine çıkış ekleme

1. **İş Topolojisi**'nin altında **Çıkışlar**'a tıklayın.
1. İçinde **çıkışları** bölmesinde tıklatın **Ekle**ve ardından aşağıdaki bilgileri girin:

   **Çıkış diğer adı**: Çıkışın benzersiz diğer adı.

   **Havuz**: seçin **Power BI**.
1. Tıklatın **Authorize**ve Power BI hesabınızda oturum açın.
1. Yetkilendirildikten sonra aşağıdaki bilgileri girin:

   **Çalışma grubu**: hedef grup çalışma alanınızı seçin.

   **Veri kümesi adı**: bir veri kümesi adı girin.

   **Tablo adı**: Tablo adı girin.
1. **Oluştur**’a tıklayın.

   ![Azure Stream Analytics işinde bir çıktı ekleyin](media/iot-hub-live-data-visualization-in-power-bi/4_add-output-to-stream-analytics-job-azure.png)

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Stream Analytics işinin sorgusunu yapılandırma

1. **İş Topolojisi**'nin altında **Sorgu**'ya tıklayın.
1. `[YourInputAlias]` değerini işin giriş diğer adıyla değiştirin.
1. `[YourOutputAlias]` değerini işin çıkış diğer adıyla değiştirin.
1. **Kaydet**’e tıklayın.

   ![Azure Stream Analytics işinde sorgu ekleme](media/iot-hub-live-data-visualization-in-power-bi/5_add-query-stream-analytics-job-azure.png)

### <a name="run-the-stream-analytics-job"></a>Stream Analytics işini çalıştırma

Stream Analytics işinde **Başlat** > **Şimdi** > **Başlat**'a tıklayın. İş düzgün bir şekilde başlatıldıktan sonra, **Durduruldu** olan iş durumu **Çalışıyor** olarak değiştirilir.

![Azure Stream Analytics işini çalıştır](media/iot-hub-live-data-visualization-in-power-bi/6_run-stream-analytics-job-azure.png)

## <a name="create-and-publish-a-power-bi-report-to-visualize-the-data"></a>Oluşturma ve verileri görselleştirmek için bir Power BI raporu yayımlama

1. Örnek uygulama Cihazınızda çalıştığından emin olun. Öğreticiler altında başvurabilirsiniz değil, varsa [Cihazınızı](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).
1. Oturum açın, [Power BI](https://powerbi.microsoft.com/en-us/) hesabı.
1. Stream Analytics işi çıkış oluşturduğunuzda ayarladığınız Grup çalışma alanına gidin.
1. Tıklatın **veri kümeleri akış**.

   Stream Analytics işi için çıkış oluştururken belirttiğiniz veri kümesinin listelendiğini görüyor olmalısınız.
1. **EYLEMLER**'in altında, ilk simgeye tıklayarak bir rapor oluşturun.

   ![Microsoft Power BI raporu oluşturma](media/iot-hub-live-data-visualization-in-power-bi/7_create-power-bi-report-microsoft.png)

1. Zamanla değişen gerçek zamanlı sıcaklığı göstermek için bir çizgi grafik oluşturun.
   1. Rapor oluşturma sayfasında bir çizgi grafiği ekleyin.
   1. **Alanlar** bölmesinde, Stream Analytics işi için çıkış oluştururken belirttiğiniz tabloyu genişletin.
   1. **Görselleştirmeler** bölmesinde **EventEnqueuedUtcTime** öğesini **Eksen** üzerine sürükleyin.
   1. **temperature** öğesini **Değerler** üzerine sürükleyin.

      Şimdi bir çizgi grafiği oluşturulur. Grafiğin x ekseninde UTC saat dilimine göre tarih ve saat görüntülenir. Grafiğin y ekseninde algılayıcıdan alınan sıcaklık görüntülenir.

      ![Bir çizgi grafiği sıcaklık için bir Microsoft Power BI raporu ekleme](media/iot-hub-live-data-visualization-in-power-bi/8_add-line-chart-for-temperature-to-power-bi-report-microsoft.png)

1. Zamanla değişen gerçek zamanlı nem oranını göstermek için bir çizgi grafik daha oluşturun. Bunu yapmak için yukarıdaki aynı adımları izleyin ve yerleştirin **EventEnqueuedUtcTime** eksenindeki ve **nem** y ekseni üzerinde.

   ![Bir çizgi grafiği nem için bir Microsoft Power BI raporu ekleme](media/iot-hub-live-data-visualization-in-power-bi/9_add-line-chart-for-humidity-to-power-bi-report-microsoft.png)

1. Raporu kaydetmek için **Kaydet**’e tıklayın.
1. Tıklatın **dosya** > **Web'de Yayımla**.
1. Tıklatın **oluşturma katıştırma kodunu**ve ardından **Yayımla**.

Rapor bağlantısı sağlanan rapor erişim ve raporu blog'unuza veya Web sitesi tümleştirmek için bir kod parçacığını bir kişiyle paylaşabilirsiniz.

![Microsoft Power BI rapor yayımlayın](media/iot-hub-live-data-visualization-in-power-bi/10_publish-power-bi-report-microsoft.png)

Microsoft ayrıca sunar [Power BI mobil uygulamalarından](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) görüntüleme ve Power BI panolarınızı ve raporlarınızı ile etkileşim için mobil Cihazınızda.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT hub'ından gerçek zamanlı algılayıcı verilerini görselleştirmek için Power BI başarıyla kullandıysanız.
Azure IOT Hub verilerini görselleştirmek için alternatif bir yolu yoktur. Bkz: [Azure IOT hub'ı gerçek zamanlı algılayıcı verilerini görselleştirmek için kullanım Azure Web Apps](iot-hub-live-data-visualization-in-web-apps.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
