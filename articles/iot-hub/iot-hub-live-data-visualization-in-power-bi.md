---
title: "Azure IOT Hub – Power BI algılayıcı verilerini gerçek zamanlı veri Görselleştirme | Microsoft Docs"
description: "Algılayıcı toplanan ve Azure IOT hub'ına gönderilen sıcaklık ve nem verileri görselleştirmek için Power BI kullanın."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "gerçek zamanlı veri görselleştirme, dinamik veri görselleştirme algılayıcı verileri Görselleştirme"
ms.assetid: e67c9c09-6219-4f0f-ad42-58edaaa74f61
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: xshi
ms.openlocfilehash: 82915a92464f240591777595e878a534cde0136c
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
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

1. İçinde [Azure portal](https://portal.azure.com), tıklatın **kaynak oluşturma** > **nesnelerin interneti** > **Stream Analytics işi**.
1. İş için aşağıdaki bilgileri girin.

   **İş adı**: İş adı. Adın genel olarak benzersiz olması gerekir.

   **Kaynak grubu**: IOT hub'ınızı kullandığı aynı kaynak grubunu kullanın.

   **Konum**: aynı konumu, kaynak grubu olarak kullanın.

   **Panoya sabitle**: Panodan IoT hub'ınıza kolay erişim için bu seçeneği işaretleyin.

   ![Stream Analytics işi oluşturma](media/iot-hub-live-data-visualization-in-power-bi/2_create-stream-analytics-job-azure.png)

1. **Oluştur**’a tıklayın.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Bir akış analizi işine giriş Ekle

1. Akış analizi işi'ni açın.
1. Altında **iş topoloji**, tıklatın **girişleri**.
1. İçinde **girişleri** bölmesinde tıklatın **Ekle**ve ardından aşağıdaki bilgileri girin:

   **Giriş diğer adı**: giriş için benzersiz diğer ad.

   **Kaynak**: seçin **IOT hub'ı**.

   **Tüketici grubu**: yeni oluşturduğunuz tüketici grubu seçin.
1. **Oluştur**’a tıklayın.

   ![Azure akış analizi işine giriş Ekle](media/iot-hub-live-data-visualization-in-power-bi/3_add-input-to-stream-analytics-job-azure.png)

### <a name="add-an-output-to-the-stream-analytics-job"></a>Akış analizi işine çıkış ekleme

1. Altında **iş topoloji**, tıklatın **çıkışları**.
1. İçinde **çıkışları** bölmesinde tıklatın **Ekle**ve ardından aşağıdaki bilgileri girin:

   **Çıkış diğer adları**: çıkış için benzersiz diğer ad.

   **Havuz**: seçin **Power BI**.
1. Tıklatın **Authorize**ve Power BI hesabınızda oturum açın.
1. Yetkilendirildikten sonra aşağıdaki bilgileri girin:

   **Çalışma grubu**: hedef grup çalışma alanınızı seçin.

   **Veri kümesi adı**: bir veri kümesi adı girin.

   **Tablo adı**: Tablo adı girin.
1. **Oluştur**’a tıklayın.

   ![Azure Stream Analytics işinde bir çıktı ekleyin](media/iot-hub-live-data-visualization-in-power-bi/4_add-output-to-stream-analytics-job-azure.png)

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Stream Analytics işi sorgu yapılandırın

1. Altında **iş topoloji**, tıklatın **sorgu**.
1. Değiştir `[YourInputAlias]` iş giriş diğer adı ile.
1. Değiştir `[YourOutputAlias]` işinin çıkış diğer adına sahip.
1. **Kaydet**’e tıklayın.

   ![Azure Stream Analytics işinde sorgu ekleme](media/iot-hub-live-data-visualization-in-power-bi/5_add-query-stream-analytics-job-azure.png)

### <a name="run-the-stream-analytics-job"></a>Stream Analytics işini çalıştır

Stream Analytics işinde tıklatın **Başlat** > **şimdi** > **Başlat**. İş başarıyla başladıktan sonra iş durumu değişiklikleri **durduruldu** için **çalıştıran**.

![Azure Stream Analytics işini çalıştır](media/iot-hub-live-data-visualization-in-power-bi/6_run-stream-analytics-job-azure.png)

## <a name="create-and-publish-a-power-bi-report-to-visualize-the-data"></a>Oluşturma ve verileri görselleştirmek için bir Power BI raporu yayımlama

1. Örnek uygulama Cihazınızda çalıştığından emin olun. Öğreticiler altında başvurabilirsiniz değil, varsa [Cihazınızı](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).
1. Oturum açın, [Power BI](https://powerbi.microsoft.com/en-us/) hesabı.
1. Stream Analytics işi çıkış oluşturduğunuzda ayarladığınız Grup çalışma alanına gidin.
1. Tıklatın **veri kümeleri akış**.

   Stream Analytics işi çıkış oluşturduğunuzda, belirttiğiniz listelenen dataset görmeniz gerekir.
1. Altında **Eylemler**, bir rapor oluşturmak için önce simgeyi tıklatın.

   ![Microsoft Power BI raporu oluşturma](media/iot-hub-live-data-visualization-in-power-bi/7_create-power-bi-report-microsoft.png)

1. Gerçek zamanlı sıcaklık zamanla göstermek için bir çizgi grafiği oluşturun.
   1. Rapor oluşturma sayfasında bir çizgi grafiği ekleyin.
   1. Üzerinde **alanları** bölmesinde, Stream Analytics işi çıkış oluşturduğunuzda belirtilen tablonun genişletin.
   1. Sürükleme **EventEnqueuedUtcTime** için **eksen** üzerinde **görselleştirmeleri** bölmesi.
   1. Sürükleme **sıcaklık** için **değerleri**.

      Şimdi bir çizgi grafiği oluşturulur. X eksenindeki tarih ve saati UTC saat diliminde görüntüler. Y ekseni sıcaklık algılayıcı görüntüler.

      ![Bir çizgi grafiği sıcaklık için bir Microsoft Power BI raporu ekleme](media/iot-hub-live-data-visualization-in-power-bi/8_add-line-chart-for-temperature-to-power-bi-report-microsoft.png)

1. Gerçek zamanlı nem zamanla göstermek için başka bir çizgi grafiği oluşturun. Bunu yapmak için yukarıdaki aynı adımları izleyin ve yerleştirin **EventEnqueuedUtcTime** eksenindeki ve **nem** y ekseni üzerinde.

   ![Bir çizgi grafiği nem için bir Microsoft Power BI raporu ekleme](media/iot-hub-live-data-visualization-in-power-bi/9_add-line-chart-for-humidity-to-power-bi-report-microsoft.png)

1. Tıklatın **kaydetmek** raporu kaydetmek için.
1. Tıklatın **dosya** > **Web'de Yayımla**.
1. Tıklatın **oluşturma katıştırma kodunu**ve ardından **Yayımla**.

Rapor bağlantısı sağlanan rapor erişim ve raporu blog'unuza veya Web sitesi tümleştirmek için bir kod parçacığını bir kişiyle paylaşabilirsiniz.

![Microsoft Power BI rapor yayımlayın](media/iot-hub-live-data-visualization-in-power-bi/10_publish-power-bi-report-microsoft.png)

Microsoft ayrıca sunar [Power BI mobil uygulamalarından](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) görüntüleme ve Power BI panolarınızı ve raporlarınızı ile etkileşim için mobil Cihazınızda.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT hub'ından gerçek zamanlı algılayıcı verilerini görselleştirmek için Power BI başarıyla kullandıysanız.
Azure IOT Hub verilerini görselleştirmek için alternatif bir yolu yoktur. Bkz: [Azure IOT hub'ı gerçek zamanlı algılayıcı verilerini görselleştirmek için kullanım Azure Web Apps](iot-hub-live-data-visualization-in-web-apps.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
