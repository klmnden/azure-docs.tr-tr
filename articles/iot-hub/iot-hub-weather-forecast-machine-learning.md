---
title: "IOT hub'ı verilerle Azure Machine Learning kullanarak tahmin hava durumu | Microsoft Docs"
description: "Yağmur olasılığını tahmin etmek için kullanımı Azure Machine Learning algılayıcı IOT hub'ınızı toplar sıcaklık ve nem verileri temel alan."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: machine learning hava durumu tahmini
ms.assetid: 8ba7d9e7-699c-4448-b353-0f3e1429d198
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: xshi
ms.openlocfilehash: 50ae54b9476c49b80236e295c0bf244df8236cff
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="weather-forecast-using-the-sensor-data-from-your-iot-hub-in-azure-machine-learning"></a>IOT hub'ınızı algılayıcı verilerini Azure Machine Learning kullanarak tahmin hava durumu

![Uçtan uca diyagramı](media/iot-hub-get-started-e2e-diagram/6.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Machine learning, gelecekteki davranışları, sonuçları ve eğilimleri tahmin etmek için var olan verilerden bilgi bilgisayarlar yardımcı olan veri bilimi bir tekniktir. Azure Machine Learning, tahmine dayalı modelleri analiz çözümleri olarak hızlı bir şekilde oluşturmayı ve dağıtmayı mümkün kılan bulut tabanlı ve tahmine dayalı analiz hizmetidir.

## <a name="what-you-learn"></a>Öğrenecekleriniz

Azure Machine Learning tahmin (Yağmur olasılığını) hava durumu için nasıl kullanılacağını öğrenin Azure IOT hub'ınızı sıcaklık ve nem verilerini kullanarak. Yağmur hazırlıklı hava durumu Tahmini modeli çıktısını şansınızdır. Model sıcaklık ve nem göre Yağmur olasılığını tahmin için geçmiş verilerin üzerine yerleşik olarak bulunur.

## <a name="what-you-do"></a>Neler

- Hava durumu Tahmini modeli bir web hizmeti olarak dağıtın.
- IOT hub'ınızı bir tüketici grubu ekleyerek veri erişimi için hazırlanın.
- Stream Analytics işi oluşturmak ve işi yapılandırın:
  - IOT hub'ından sıcaklık ve nem verilerini okur.
  - Yağmur fırsat almak için web hizmeti çağrısı.
  - Sonuç bir Azure blob depolama alanına kaydedin.
- Microsoft Azure Storage Gezgini hava tahmini görüntülemek için kullanın.

## <a name="what-you-need"></a>Ne gerekiyor

- Öğretici [Cihazınızı](iot-hub-raspberry-pi-kit-node-get-started.md) , aşağıdaki gereksinimleri ele alınmaktadır tamamlandı:
  - Etkin bir Azure aboneliği.
  - Azure IOT hub'ı aboneliğinizdeki.
  - Azure IOT hub'ına iletileri gönderen bir istemci uygulaması.
- Bir Azure Machine Learning Studio hesabı. ([Machine Learning Studio ücretsiz deneyin](https://studio.azureml.net/)).

## <a name="deploy-the-weather-prediction-model-as-a-web-service"></a>Bir web hizmeti olarak hava durumu Tahmini modeli dağıtın

1. Git [hava durumu Tahmini modeli sayfasına](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).
1. Tıklatın **Studio'da Aç** Microsoft Azure Machine Learning Studio'da.
   ![Cortana Intelligence Galerisi'nde hava durumu Tahmini modeli sayfasını açın](media/iot-hub-weather-forecast-machine-learning/2_weather-prediction-model-in-cortana-intelligence-gallery.png)
1. ' I tıklatın **çalıştırmak** model adımlarda doğrulanacak. Bu adımı tamamlamak için 2 dakika sürebilir.
   ![Azure Machine Learning Studio'da hava durumu Tahmini modeli açın](media/iot-hub-weather-forecast-machine-learning/3_open-weather-prediction-model-in-azure-machine-learning-studio.png)
1. Tıklatın **WEB hizmetini kurma** > **Tahmine dayalı Web hizmeti**.
   ![Azure Machine Learning Studio'da hava durumu Tahmini modeli dağıtın](media/iot-hub-weather-forecast-machine-learning/4-deploy-weather-prediction-model-in-azure-machine-learning-studio.png)
1. Diyagramda sürükleyin **Web hizmeti girişi** yakın bir yerde Modülü **Score Model** modülü.
1. Bağlantı **Web hizmeti girişi** modülüne **Score Model** modülü.
   ![Azure Machine Learning Studio'da iki modülleri Bağlan](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)
1. ' I tıklatın **çalıştırmak** model adımlarda doğrulanacak.
1. Tıklatın **WEB hizmeti Dağıt** bir web hizmeti olarak modeli dağıtmak için.
1. Model Panoda karşıdan **Excel 2010 veya önceki çalışma kitabı** için **istek/yanıt**.

   > [!Note]
   > Karşıdan yüklediğiniz olun **Excel 2010 veya önceki çalışma kitabı** bilgisayarınızda Excel daha sonraki bir sürümünü çalıştırıyor olsanız bile.

   ![İstek YANITI uç noktası için Excel indirin](media/iot-hub-weather-forecast-machine-learning/5_download-endpoint-app-excel-for-request-response.png)

1. Excel çalışma kitabı açın, Not **WEB hizmeti URL'si** ve **erişim tuşu**.

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Oluşturma, yapılandırma ve akış analizi işi çalıştırma

### <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

1. İçinde [Azure portal](https://ms.portal.azure.com/), tıklatın **yeni** > **nesnelerin interneti** > **Stream Analytics işi**.
1. İş için aşağıdaki bilgileri girin.

   **İş adı**: İş adı. Adın genel olarak benzersiz olması gerekir.

   **Kaynak grubu**: IOT hub'ınızı kullandığı aynı kaynak grubunu kullanın.

   **Konum**: aynı konumu, kaynak grubu olarak kullanın.

   **Panoya Sabitle**: kolay erişim için bu seçeneği IOT hub'ınıza panodan denetleyin.

   ![Stream Analytics işi oluşturma](media/iot-hub-weather-forecast-machine-learning/7_create-stream-analytics-job-azure.png)

1. **Oluştur**'a tıklayın.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Bir akış analizi işine giriş Ekle

1. Akış analizi işi'ni açın.
1. Altında **iş topoloji**, tıklatın **girişleri**.
1. İçinde **girişleri** bölmesinde tıklatın **Ekle**ve ardından aşağıdaki bilgileri girin:

   **Giriş diğer adı**: giriş için benzersiz diğer ad.

   **Kaynak**: seçin **IOT hub'ı**.

   **Tüketici grubu**: oluşturduğunuz tüketici grubu seçin.

   ![Azure akış analizi işine giriş Ekle](media/iot-hub-weather-forecast-machine-learning/8_add-input-stream-analytics-job-azure.png)

1. **Oluştur**'a tıklayın.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Akış analizi işine çıkış ekleme

1. Altında **iş topoloji**, tıklatın **çıkışları**.
1. İçinde **çıkışları** bölmesinde tıklatın **Ekle**ve ardından aşağıdaki bilgileri girin:

   **Çıkış diğer adları**: çıkış için benzersiz diğer ad.

   **Havuz**: seçin **Blob Storage**.

   **Depolama hesabı**: blob depolama alanınızın için depolama hesabı. Bir depolama hesabı oluşturun veya var olan bir kullanın.

   **Kapsayıcı**: blob kaydettiğiniz yere kapsayıcı. Bir kapsayıcı oluşturmak veya mevcut bir kullanın.

   **Olayı seri hale getirme biçimi**: seçin **CSV**.

   ![Azure Stream Analytics işinde bir çıktı ekleyin](media/iot-hub-weather-forecast-machine-learning/9_add-output-stream-analytics-job-azure.png)

1. **Oluştur**'a tıklayın.

### <a name="add-a-function-to-the-stream-analytics-job-to-call-the-web-service-you-deployed"></a>Bir işlev dağıttığınız web hizmetini çağırmak için Stream Analytics işi ekleme

1. Altında **iş topoloji**, tıklatın **işlevleri** > **Ekle**.
1. Aşağıdaki bilgileri girin:

   **İşlev diğer**: girin `machinelearning`.

   **İşlev türü**: seçin **Azure ML**.

   **Alma seçeneği**: seçin **farklı bir abonelik alma**.

   **URL**: Excel çalışma kitabından aşağı ettiğiniz WEB hizmeti URL'sini girin.

   **Anahtar**: Excel çalışma kitabından aşağı ettiğiniz erişim ANAHTARINI girin.

   ![Azure Stream Analytics işinde işlevi ekleme](media/iot-hub-weather-forecast-machine-learning/10_add-function-stream-analytics-job-azure.png)

1. **Oluştur**'a tıklayın.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Stream Analytics işi sorgu yapılandırın

1. Altında **iş topoloji**, tıklatın **sorgu**.
1. Var olan kodu aşağıdaki kodla değiştirin:

   ```sql
   WITH machinelearning AS (
      SELECT EventEnqueuedUtcTime, temperature, humidity, machinelearning(temperature, humidity) as result from [YourInputAlias]
   )
   Select System.Timestamp time, CAST (result.[temperature] AS FLOAT) AS temperature, CAST (result.[humidity] AS FLOAT) AS humidity, CAST (result.[Scored Probabilities] AS FLOAT ) AS 'probabalities of rain'
   Into [YourOutputAlias]
   From machinelearning
   ```

   Değiştir `[YourInputAlias]` iş giriş diğer adı ile.

   Değiştir `[YourOutputAlias]` işinin çıkış diğer adına sahip.

1. **Kaydet** düğmesine tıklayın.

### <a name="run-the-stream-analytics-job"></a>Stream Analytics işini çalıştır

Stream Analytics işinde tıklatın **Başlat** > **şimdi** > **Başlat**. İş başarıyla başladıktan sonra iş durumu değişiklikleri **durduruldu** için **çalıştıran**.

![Stream Analytics işini çalıştır](media/iot-hub-weather-forecast-machine-learning/11_run-stream-analytics-job-azure.png)

## <a name="use-microsoft-azure-storage-explorer-to-view-the-weather-forecast"></a>Microsoft Azure Storage Gezgini hava tahmini görüntülemek için kullanın

Toplama ve IOT hub'ınıza sıcaklık ve nem veri göndermeye başlaması için istemci uygulaması çalıştırın. IOT hub'ınızın aldığı her ileti için Stream Analytics işi Yağmur olasılığını üretmek için hava tahmini web hizmetini çağırır. Sonuç daha sonra Azure blob depolama alanına kaydedildi. Azure Storage Gezgini sonucu görüntülemek için kullanabileceğiniz bir araçtır.

1. [Microsoft Azure Storage Gezgini yükleyip](http://storageexplorer.com/).
1. Azure Depolama Gezgini'ni açın.
1. Azure hesabınızda oturum açın.
1. Aboneliğinizi seçin.
1. Aboneliğinizi tıklayın > **depolama hesapları** > depolama hesabınız > **Blob kapsayıcıları** >, kapsayıcı.
1. Sonuç görmek için bir .csv dosyasını açın. Son sütun Yağmur olasılığını kaydeder.

   ![Azure Machine Learning ile hava tahmini sonucu alın](media/iot-hub-weather-forecast-machine-learning/12_get-weather-forecast-result-azure-machine-learning.png)

## <a name="summary"></a>Özet

IOT hub'ınızı alan sıcaklık ve nem verilerine dayalı Yağmur olasılığını üretmek için Azure Machine Learning başarıyla kullandıysanız.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]