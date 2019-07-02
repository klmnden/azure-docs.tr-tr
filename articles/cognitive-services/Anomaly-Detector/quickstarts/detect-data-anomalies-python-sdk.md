---
title: 'Hızlı Başlangıç: Anomali algılayıcısı kitaplığı ve Python kullanarak veri anormallikleri | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Toplu olarak ya da akış verileri, veri serisinde prosesler algılamak için Anomali algılayıcısı API'sini kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: article
ms.date: 07/01/2019
ms.author: aahi
ms.openlocfilehash: 1d89ed8f40547142d41af9c587fc8fc000fa4dd9
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503680"
---
# <a name="quickstart-anomaly-detector-client-library-for-python"></a>Hızlı Başlangıç: Python için anomali algılayıcısı istemci kitaplığı

.NET için Anomali algılayıcısı istemci kitaplığını kullanmaya başlayın. Temel görevleri için örnek kod deneyin ve paketi yüklemek için aşağıdaki adımları izleyin. Anomali algılayıcı hizmeti otomatik olarak en sığdırma modeller üzerinde sektör, senaryo veya veri hacmi bağımsız olarak kullanarak zaman serisi verilerinizle prosesler bulmanızı sağlar.

## <a name="key-concepts"></a>Önemli kavramlar

Python için Anomali algılayıcısı istemci kitaplığını kullanın:

* Toplu istek olarak, zaman serisi veri kümesi genelinde anormallikleri
* En son veri noktası anomali durumunu, zaman serisi içinde algılayın

[Kitaplık referans belgelerine](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector?view=azure-python) | [kitaplığı kaynak kodunu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-anomalydetector) | [paket (Pypı)](https://pypi.org/project/azure-cognitiveservices-anomalydetector/) | [örnekleri](https://github.com/Azure-Samples/anomalydetector)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği - [ücretsiz oluşturun](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)
* [Pandas verileri analiz kitaplığı](https://pandas.pydata.org/)
 
## <a name="setting-up"></a>Ayarlama

### <a name="create-an-anomaly-detector-resource"></a>Bir Anomali algılayıcısı kaynağı oluşturun

[!INCLUDE [anomaly-detector-resource-creation](../../../../includes/cognitive-services-anomaly-detector-resource-cli.md)]

### <a name="install-the-client-library"></a>İstemci Kitaplığı'nı yükleyin

Python'ı yükledikten sonra istemci kitaplığı ile yükleyebilirsiniz:

```console
pip install --upgrade azure-cognitiveservices-anomalydetector
```

## <a name="object-model"></a>Nesne modeli

Anomali algılayıcısı istemci bir [AnomalyDetectorClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python) anahtarınızı kullanarak Azure'da kimlik doğrulayan bir nesne. İstemci, anomali algılama iki yöntem sunar: Bir veri kümesinin tamamında kullanılmasındaki [entire_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#entire-detect-body--custom-headers-none--raw-false----operation-config-)ve en son verileri kullanarak noktası [Last_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#last-detect-body--custom-headers-none--raw-false----operation-config-). 

Zaman serisi verileri, bir dizi olarak gönderilir [noktaları](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.point(class)?view=azure-python) içinde bir [istek](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.request(class)?view=azure-python) nesne. `Request` Nesne verileri tanımlamak için özellikleri içerir ([ayrıntı düzeyi](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.granularity?view=azure-python) gibi) ve anomali algılama için parametreleri. 

Anomali algılayıcısı yanıt bir [LastDetectResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.lastdetectresponse?view=azure-python) veya [EntireDetectResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.entiredetectresponse?view=azure-python) kullanılan yöntemine bağlı olarak nesnesi. 

## <a name="getting-started"></a>Başlarken

Tercih edilen Düzenleyicisi veya IDE içinde yeni bir Python uygulaması oluşturun. Ardından aşağıdaki içeri aktarma bildirimleri dosyanıza ekleyin. 

[!code-python[import declarations](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=imports)]

> [!NOTE]
> Bu hızlı başlangıçta, seçtiğiniz varsayar [bir ortam değişkeni oluşturulan](../../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication) Anomali algılayıcısı anahtarınızı adlı `ANOMALY_DETECTOR_KEY`.

Anahtarınızı bir ortam değişkeni olarak, bir zaman serisi veri dosyası ve aboneliğinizin azure konumu için yol değişkenleri oluşturun. Örneğin, `westus2`. 

[!code-python[Vars for the key, path location and data path](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=initVars)]

## <a name="code-examples"></a>Kod örnekleri 

Bu kod parçacıkları Detector Anomali istemci kitaplığı ile .NET için aşağıdakileri yapın gösterilmektedir:

* [İstemci kimlik doğrulaması](#authenticate-the-client)
* [Bir dosyadan zaman serisi veri kümesini yüklemek](#load-time-series-data-from-a-file)
* [Tüm veri kümesinde anomalileri algılayın](#detect-anomalies-in-the-entire-data-set) 
* [En son veri noktası durumunu anomali algılama](#detect-the-anomaly-status-of-the-latest-data-point)

### <a name="authenticate-the-client"></a>İstemci kimlik doğrulaması

Azure konumu değişkeniniz için uç nokta ekleyin ve anahtarınızla istemci kimlik doğrulaması.

[!code-python[Client authentication](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=client)]

### <a name="load-time-series-data-from-a-file"></a>Bir dosyadan zaman serisi verileri yükleme

Bu hızlı başlangıçtan için örnek verileri indirme [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/example-data/request-data.csv):
1. Tarayıcınızda, sağ **ham**
2. Tıklayın **bağlantı olarak Kaydet**
3. Dosyayı uygulama dizininize bir .csv dosyası olarak kaydedin.

Bu zaman serisi verilerini .csv dosyası olarak biçimlendirilir ve Anomali algılayıcısı API'sine gönderilir.

Veri dosyanızla Pandas kitaplığın yük `read_csv()` metodu ve oluşturma, veri serisi depolamak için bir boş listenin değişken. Dosya yineleme yapma ve verileri olarak ekleme bir [noktası](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.point%28class%29?view=azure-python) nesne. Bu nesne .csv veri dosyanızın satırlardan sayısal değer ve zaman damgası içerir. 

[!code-python[Load the data file](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=loadDataFile)]

Oluşturma bir [istek](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.request%28class%29?view=azure-python) nesnesi, zaman serisi ile ve [ayrıntı düzeyi](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.granularity?view=azure-python) (veya dönemsellik), veri noktalarının. Örneğin, `Granularity.daily`.

[!code-python[Create the request object](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=request)]

### <a name="detect-anomalies-in-the-entire-data-set"></a>Tüm veri kümesinde anomalileri algılayın 

İstemcinin kullanarak tüm zaman serisi verilerini aracılığıyla anormallikleri algılamak için API çağrısı [entire_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#entire-detect-body--custom-headers-none--raw-false----operation-config-) yöntemi. Döndürülen Store [EntireDetectResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.entiredetectresponse?view=azure-python) nesne. Yanıtın yineleme `is_anomaly` listelemek ve herhangi bir dizin yazdırma `true` değerleri. Karşılanmadığı, bu değerler, anormal veri noktası dizini karşılık gelir.

[!code-python[Batch anomaly detection sample](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=detectAnomaliesBatch)]

### <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>En son veri noktası durumunu anomali algılama

En son veri noktanız istemcinin kullanarak bir anomali olup olmadığını belirlemek için Anomali algılayıcısı API'ye çağrı [last_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#last-detect-body--custom-headers-none--raw-false----operation-config-) yöntemi ve döndürülen deposu [LastDetectResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.lastdetectresponse?view=azure-python) nesne. Yanıtın `is_anomaly` o noktasının anomali durumunu belirten bir Boole değeridir.  

[!code-python[Batch anomaly detection sample](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=latestPointDetection)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı, IDE veya komut satırı ile çalıştırmak `python` komut ve dosya adı.
 
## <a name="clean-up-resources"></a>Kaynakları temizleme

Temizleme ve Bilişsel hizmetler abonelik kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, kaynak grubuyla ilişkili diğer tüm kaynakları siler.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

Kaynak grubunu ve ilişkili kaynakları kaldırmak için aşağıdaki cloud shell komutu da çalıştırabilirsiniz. Bu işlemin tamamlanması birkaç dakika sürebilir. 

```azurecli-interactive
az group delete --name example-anomaly-detector-resource-group
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>[Azure Databricks ile akış anomali algılama](../tutorials/anomaly-detection-streaming-databricks.md)

* Nedir [Anomali algılayıcısı API?](../overview.md)
* [En iyi uygulamalar](../concepts/anomaly-detection-best-practices.md) Anomali algılayıcısı API'si kullanılırken.
* Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/sdk/csharp-sdk-sample.cs).
