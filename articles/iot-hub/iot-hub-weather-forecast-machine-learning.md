---
title: IOT Hub'ından veri ile Azure Machine Learning kullanarak hava durumu tahmini | Microsoft Docs
description: IOT hub'ınıza algılayıcıdan toplar sıcaklık ve nem verilere Yağmur olasılığını tahmin etmek için kullanımı Azure Machine Learning bağlı.
author: robinsh
manager: philmea
keywords: machine learning hava durumu tahmini
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: robinsh
ms.openlocfilehash: ffc2e5fb588ce6861f5df6cefdf810c1a015c043
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61441092"
---
# <a name="weather-forecast-using-the-sensor-data-from-your-iot-hub-in-azure-machine-learning"></a>Azure Machine Learning'de, IOT hub'ından sensör verilerini kullanarak hava durumu tahmini

![Uçtan uca diyagramı](media/iot-hub-get-started-e2e-diagram/6.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Makine öğrenimi, bilgisayarların gelecekteki davranışları, sonuçları ve eğilimleri tahmin etmek için var olan verilerden yardımcı olan veri bilimi tekniğidir. Azure Machine Learning, tahmine dayalı modelleri analiz çözümleri olarak hızlı bir şekilde oluşturmayı ve dağıtmayı mümkün kılan bulut tabanlı ve tahmine dayalı analiz hizmetidir.

## <a name="what-you-learn"></a>Öğrenecekleriniz

Azure Machine Learning, tahmin (Yağmur olasılığı) hava durumu için kullanmayı öğrenin kullanarak Azure IOT hub'ınıza sıcaklık ve nem verileri. Yağmur olasılığı hazırlanmış hava durumu tahmini modelinin çıkış alınır. Model sıcaklık ve nem göre Yağmur olasılığını tahmin için geçmiş veri üzerinde oluşturulmuştur.

## <a name="what-you-do"></a>Neler

- Hava durumu Tahmini modeli bir web hizmeti olarak dağıtalım.
- IOT hub'ınıza bir tüketici grubu ekleyerek veri erişim için hazırlanın.
- Stream Analytics işi oluşturmak ve işe yapılandırın:
  - Sıcaklık ve nem verilerini, IOT hub'ından okur.
  - Yağmur olasılığı almak için web hizmeti çağrısı.
  - Sonuç, bir Azure blob depolama alanına kaydedin.
- Hava durumu tahminini görüntülemek için Microsoft Azure Depolama Gezgini'ni kullanırsınız.

## <a name="what-you-need"></a>Ne gerekiyor

- Tamamlamak [Raspberry Pi çevrimiçi simülatör](iot-hub-raspberry-pi-web-simulator-get-started.md) öğretici veya bir cihaz öğreticileri; Örneğin, [node.js ile Raspberry Pi](iot-hub-raspberry-pi-kit-node-get-started.md). Bu, aşağıdaki gereksinimleri kapsar:
  - Etkin bir Azure aboneliği.
  - Azure IOT hub, aboneliğiniz altında.
  - Azure IOT hub'ınıza ileti gönderen bir istemci uygulaması.
- Bir Azure Machine Learning Studio hesabı. ([Machine Learning Studio'yu ücretsiz olarak deneyin](https://studio.azureml.net/)).

## <a name="deploy-the-weather-prediction-model-as-a-web-service"></a>Hava durumu Tahmini modeli bir web hizmeti olarak dağıtma

1. Git [hava durumunu tahmin modeli sayfasına](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).
1. Tıklayın **Studio'da Aç** Microsoft Azure Machine Learning Studio'da.
   ![Cortana Intelligence Galerisi'nde hava durumunu tahmin modeli sayfayı açın](media/iot-hub-weather-forecast-machine-learning/2_weather-prediction-model-in-cortana-intelligence-gallery.png)
1. Tıklayın **çalıştırma** modelinde adımları doğrulamak için. Bu adımı tamamlamak için 2 dakika sürebilir.
   ![Hava durumu Tahmini modeli Azure Machine Learning Studio'da açın.](media/iot-hub-weather-forecast-machine-learning/3_open-weather-prediction-model-in-azure-machine-learning-studio.png)
1. Tıklayın **WEB hizmeti'kurmak** > **Tahmine dayalı Web hizmeti**.
   ![Azure Machine Learning Studio'da hava durumu tahmini model dağıtma](media/iot-hub-weather-forecast-machine-learning/4-deploy-weather-prediction-model-in-azure-machine-learning-studio.png)
1. Diyagramda, sürükleyin **Web hizmeti giriş** yakın bir yerde Modülü **Score Model** modülü.
1. Connect **Web hizmeti giriş** modülüne **Score Model** modülü.
   ![Azure Machine Learning Studio'da iki modül bağlanma](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)
1. Tıklayın **ÇALIŞTIRMA** modelinde adımları doğrulamak için.
1. Tıklayın **WEB hizmeti Dağıt** için bir web hizmeti olarak modeli dağıtacağız.
1. Modelin Panoda indirme **Excel 2010 veya önceki çalışma kitabı** için **istek/yanıt**.

   > [!Note]
   > Karşıdan yüklediğiniz olun **Excel 2010 veya önceki çalışma kitabı** bilgisayarınızda Excel daha sonraki bir sürümünü çalıştırıyor olsanız bile.

   ![İstek YANITI uç noktası için Excel indirin](media/iot-hub-weather-forecast-machine-learning/5_download-endpoint-app-excel-for-request-response.png)

1. Excel çalışma kitabını açın, Not **WEB hizmeti URL'si** ve **erişim anahtarı**.

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Oluşturma, yapılandırma ve bir Stream Analytics işini çalıştır

### <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

1. [Azure portalında](https://portal.azure.com/), **Kaynak oluştur** > **Nesnelerin İnterneti** > **Stream Analytics işi**'ne tıklayın.
1. İş için aşağıdaki bilgileri girin.

   **İş adı**: İş adı. Adın genel olarak benzersiz olması gerekir.

   **Kaynak grubu**: IOT hub'ınıza kullandığı aynı kaynak grubunu kullanın.

   **Konum**: Kaynak grubunuzun aynı konumu kullanın.

   **Panoya Sabitle**: Kolay erişim için bu seçeneği panodan IOT hub'ınıza denetleyin.

   ![Azure'da bir Stream Analytics işi oluşturma](media/iot-hub-weather-forecast-machine-learning/7_create-stream-analytics-job-azure.png)

1. **Oluştur**’a tıklayın.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Stream Analytics işine giriş ekleme

1. Stream Analytics işini açın.
1. **İş Topolojisi**'nin altında **Girişler**'e tıklayın.
1. İçinde **girişleri** bölmesinde tıklayın **Ekle**ve ardından aşağıdaki bilgileri girin:

   **Giriş diğer adı**: Giriş için benzersiz diğer adı.

   **Kaynak**: Seçin **IOT hub'ı**.

   **Tüketici grubu**: Oluşturduğunuz tüketici grubu seçin.

   ![Azure'da bir Stream Analytics işinin girdisi Ekle](media/iot-hub-weather-forecast-machine-learning/8_add-input-stream-analytics-job-azure.png)

1. **Oluştur**’a tıklayın.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Stream Analytics işine çıkış ekleme

1. **İş Topolojisi**'nin altında **Çıkışlar**'a tıklayın.
1. İçinde **çıkışları** bölmesinde tıklayın **Ekle**ve ardından aşağıdaki bilgileri girin:

   **Çıkış diğer adı**: Çıkış için benzersiz diğer adı.

   **Havuz**: Seçin **Blob Depolama**.

   **Depolama hesabı**: Blob depolama için depolama hesabı. Bir depolama hesabı oluşturun veya var olanı kullanın.

   **kapsayıcı**: Blob kaydedildiği kapsayıcı. Bir kapsayıcı oluşturun veya var olanı kullanın.

   **Olay serileştirme biçimi**: Seçin **CSV**.

   ![Azure Stream Analytics işinde bir çıktı ekleyin](media/iot-hub-weather-forecast-machine-learning/9_add-output-stream-analytics-job-azure.png)

1. **Oluştur**’a tıklayın.

### <a name="add-a-function-to-the-stream-analytics-job-to-call-the-web-service-you-deployed"></a>Bir işlev, dağıtılan web hizmeti çağırmak için Stream Analytics işi ekleme

1. Altında **iş topolojisi**, tıklayın **işlevleri** > **Ekle**.
1. Aşağıdaki bilgileri girin:

   **İşlev diğer adı**: `machinelearning` yazın.

   **İşlev türü**: Seçin **Azure ML**.

   **İçeri aktarma seçeneği**: Seçin **farklı bir abonelikten içeri aktarma**.

   **URL**: Excel çalışma kitabından aşağı not ettiğiniz WEB hizmeti URL'si girin.

   **Anahtar**: Excel çalışma kitabından aşağı not ettiğiniz erişim ANAHTARINI girin.

   ![Azure Stream Analytics işinde bir işlev ekleme](media/iot-hub-weather-forecast-machine-learning/10_add-function-stream-analytics-job-azure.png)

1. **Oluştur**’a tıklayın.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Stream Analytics işinin sorgusunu yapılandırma

1. **İş Topolojisi**'nin altında **Sorgu**'ya tıklayın.
1. Varolan kodu aşağıdaki kodla değiştirin:

   ```sql
   WITH machinelearning AS (
      SELECT EventEnqueuedUtcTime, temperature, humidity, machinelearning(temperature, humidity) as result from [YourInputAlias]
   )
   Select System.Timestamp time, CAST (result.[temperature] AS FLOAT) AS temperature, CAST (result.[humidity] AS FLOAT) AS humidity, CAST (result.[Scored Probabilities] AS FLOAT ) AS 'probabalities of rain'
   Into [YourOutputAlias]
   From machinelearning
   ```

   `[YourInputAlias]` değerini işin giriş diğer adıyla değiştirin.

   `[YourOutputAlias]` değerini işin çıkış diğer adıyla değiştirin.

1. **Kaydet**’e tıklayın.

### <a name="run-the-stream-analytics-job"></a>Stream Analytics işini çalıştırma

Stream Analytics işinde **Başlat** > **Şimdi** > **Başlat**'a tıklayın. İş düzgün bir şekilde başlatıldıktan sonra, **Durduruldu** olan iş durumu **Çalışıyor** olarak değiştirilir.

![Stream Analytics işini çalıştırma](media/iot-hub-weather-forecast-machine-learning/11_run-stream-analytics-job-azure.png)

## <a name="use-microsoft-azure-storage-explorer-to-view-the-weather-forecast"></a>Hava durumu tahminini görüntülemek için Microsoft Azure Depolama Gezgini'ni kullanma

Toplama ve IOT hub'ınıza sıcaklık ve nem verileri göndermeye başlamak için istemci uygulamasını çalıştırın. IOT hub'ınızın aldığı her ileti için Stream Analytics işi Yağmur olasılığı üretmek için hava durumu tahminini web hizmeti çağırır. Sonuç, ardından, Azure blob depolama alanına kaydedilir. Azure Depolama Gezgini sonucu görüntülemek için kullanabileceğiniz bir araçtır.

1. [Microsoft Azure Depolama Gezgini'ni indirip](https://storageexplorer.com/).
1. Azure Depolama Gezgini'ni açın.
1. Azure hesabınızda oturum açın.
1. Aboneliğinizi seçin.
1. Aboneliğinize tıklayın > **depolama hesapları** > depolama hesabınız > **Blob kapsayıcıları** > kapsayıcı.
1. Sonuçları görmek için bir .csv dosyasını açın. Yağmur olasılığı son sütun kaydeder.

   ![Azure Machine Learning ile hava durumu tahminini sonucunu Al](media/iot-hub-weather-forecast-machine-learning/12_get-weather-forecast-result-azure-machine-learning.png)

## <a name="summary"></a>Özet

Azure Machine Learning, IOT hub'ınızın aldığı sıcaklık ve nem verilere dayalı Yağmur olasılığı üretmek için başarıyla kullandınız.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]