---
title: "Hızlı Başlangıç: .NET için Anomali algılayıcısı SDK'sını kullanarak zaman serisi verilerinde anomalileri algılayın"
titleSuffix: Azure Cognitive Services
description: Anomali algılayıcı hizmeti ile zaman serisi verilerinizdeki anormallikleri algılamak başlatın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 07/01/2019
ms.author: aahi
ms.openlocfilehash: a75196e035585a7501cdd842fb5b80ceff424dcc
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67721565"
---
# <a name="quickstart-anomaly-detector-client-library-for-net"></a>Hızlı Başlangıç: .NET için anomali algılayıcısı istemci kitaplığı

.NET için Anomali algılayıcısı istemci kitaplığını kullanmaya başlayın. Temel görevleri için örnek kod deneyin ve paketi yüklemek için aşağıdaki adımları izleyin. Anomali algılayıcı hizmeti otomatik olarak en sığdırma modeller üzerinde sektör, senaryo veya veri hacmi bağımsız olarak kullanarak zaman serisi verilerinizle prosesler bulmanızı sağlar.

Anomali algılayıcısı istemci kitaplığı için .NET için kullanın:

* Toplu iş olarak anomalileri algılayın
* En son veri noktası durumunu anomali algılama

[API başvuru belgeleri](https://docs.microsoft.com/dotnet/api/Microsoft.Azure.CognitiveServices.AnomalyDetector?view=azure-dotnet-preview) | [kitaplığı kaynak kodunu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/AnomalyDetector) | [paketini (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.AnomalyDetector/) | [örnekleri](https://github.com/Azure-Samples/anomalydetector)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği - [ücretsiz oluşturun](https://azure.microsoft.com/free/)
* Geçerli sürümü [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)

## <a name="setting-up"></a>Ayarlama

### <a name="create-an-anomaly-detector-resource"></a>Bir Anomali algılayıcısı kaynağı oluşturun

[!INCLUDE [anomaly-detector-resource-creation](../../../../includes/cognitive-services-anomaly-detector-resource-cli.md)]

### <a name="create-a-new-c-app"></a>Yeni bir C# uygulama

Tercih edilen Düzenleyicisi veya IDE içinde yeni bir .NET Core uygulaması oluşturun. 

Dotnet (örneğin, cmd, PowerShell veya Bash) bir konsol penceresinde kullanın `new` adıyla yeni bir konsol uygulaması oluşturmak için komut `anomaly-detector-quickstart`. Bu komut, bir basit "Hello World" oluşturur C# tek bir dosya ile proje: **Program.cs**. 

```console
dotnet new console -n anomaly-detector-quickstart
```

Yeni oluşturulan uygulama klasörüne dizin değiştirin. İle uygulama oluşturabilirsiniz:

```console
dotnet build
```

Yapı çıkış, uyarı veya hata içermelidir. 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

### <a name="install-the-client-library"></a>İstemci Kitaplığı'nı yükleyin

Uygulama dizininde Anomali algılayıcısı istemci kitaplığı için .NET şu komutla yükleyin:

```console
dotnet add package Microsoft.Azure.CognitiveServices.AnomalyDetector --version 0.8.0-preview
```

Visual Studio IDE kullanıyorsanız, istemci kitaplığı NuGet paketi olarak kullanılabilir. 

## <a name="object-model"></a>Nesne modeli

Anomali algılayıcısı istemci bir [AnomalyDetectorClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclient) Azure kullanarak kimlik doğrulaması nesne [ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.apikeyserviceclientcredentials), anahtarınızı içerir. İstemci, anomali algılama iki yöntem sunar: Bir veri kümesinin tamamında kullanılmasındaki [EntireDetectAsync()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.entiredetectasync)ve en son verileri kullanarak noktası [LastDetectAsync()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.lastdetectasync). 

Zaman serisi verileri, bir dizi olarak gönderilir [noktaları](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request.series?view=azure-dotnet-preview#Microsoft_Azure_CognitiveServices_AnomalyDetector_Models_Request_Series) içinde bir [istek](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request) nesne. `Request` Nesne verileri tanımlamak için özellikleri içerir ([ayrıntı düzeyi](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request.granularity) gibi) ve anomali algılama için parametreleri. 

Anomali algılayıcısı yanıtı geçerli bir [EntireDetectResponse](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.entiredetectresponse) veya [LastDetectResponse](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.lastdetectresponse) kullanılan yöntemine bağlı olarak bir nesne. 

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları Detector Anomali istemci kitaplığı ile .NET için aşağıdakileri yapın gösterilmektedir:

* [İstemci kimlik doğrulaması](#authenticate-the-client)
* [Bir dosyadan zaman serisi veri kümesini yüklemek](#load-time-series-data-from-a-file)
* [Tüm veri kümesinde anomalileri algılayın](#detect-anomalies-in-the-entire-data-set) 
* [En son veri noktası durumunu anomali algılama](#detect-the-anomaly-status-of-the-latest-data-point)

### <a name="add-the-main-method"></a>Main yöntemini ekleyin

Proje dizininden:

1. Tercih edilen Düzenleyici ya da IDE Program.cs dosyasını açın
2. Aşağıdaki `using` yönergeleri

[!code-csharp[using statements](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=usingStatements)]

> [!NOTE]
> Bu hızlı başlangıçta, seçtiğiniz varsayar [bir ortam değişkeni oluşturulan](../../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication) Anomali algılayıcısı anahtarınızı adlı `ANOMALY_DETECTOR_KEY`.

İçinde uygulamanın `main()` yöntemi, kaynağınızın Azure konumu ve bir ortam değişkeni olarak anahtarınız için değişkenler oluşturun. Uygulama başlatıldıktan sonra ortam değişkenini oluşturduysanız, düzenleyici, IDE ya da çalışan kabuğunu kapatılmasını ve değişken erişmek için yeniden gerekir.

[!code-csharp[Main method](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=mainMethod)]

### <a name="authenticate-the-client"></a>İstemci kimlik doğrulaması

Yeni bir yöntem içinde bir istemci uç noktasını ve anahtarı ile örneği. Oluşturma bir [ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.apikeyserviceclientcredentials?view=azure-dotnet-preview) nesne anahtarınızla ve uç noktanız ile oluşturmak için kullanmak bir [AnomalyDetectorClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-dotnet-preview) nesne. 

[!code-csharp[Client authentication function](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=createClient)]
    
### <a name="load-time-series-data-from-a-file"></a>Bir dosyadan zaman serisi verileri yükleme

Bu hızlı başlangıçtan için örnek verileri indirme [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/example-data/request-data.csv):
1. Tarayıcınızda, sağ **ham**
2. Tıklayın **bağlantı olarak Kaydet**
3. Dosyayı uygulama dizininize bir .csv dosyası olarak kaydedin.

Bu zaman serisi verilerini .csv dosyası olarak biçimlendirilir ve Anomali algılayıcısı API'sine gönderilir.

Zaman serisi verileri okumak ve eklemek için yeni bir yöntem oluşturma bir [istek](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request?view=azure-dotnet-preview) nesne. Çağrı `File.ReadAllLines()` dosya yoluyla ve listesini oluşturmak [noktası](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.point?view=azure-dotnet-preview) nesneleri ve şeridi herhangi bir yeni satır karakteri. Değerleri ayıklamak ve tarih damgası sayısal değerini ayırın ve bunları yeni bir ekleme `Point` nesne. 

Olun bir `Request` bir dizi noktası, nesne ve `Granularity.Daily` için [ayrıntı düzeyi](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.granularity?view=azure-dotnet-preview) (veya dönemsellik) veri noktalarının.

[!code-csharp[load the time series data file](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=GetSeriesFromFile)]

### <a name="detect-anomalies-in-the-entire-data-set"></a>Tüm veri kümesinde anomalileri algılayın 

İstemcinin çağıran bir yöntem oluşturma [EntireDetectAsync()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.entiredetectasync?view=azure-dotnet-preview#Microsoft_Azure_CognitiveServices_AnomalyDetector_AnomalyDetectorClientExtensions_EntireDetectAsync_Microsoft_Azure_CognitiveServices_AnomalyDetector_IAnomalyDetectorClient_Microsoft_Azure_CognitiveServices_AnomalyDetector_Models_Request_System_Threading_CancellationToken_) yöntemiyle `Request` nesne ve yanıt olarak await bir [EntireDetectResponse](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.entiredetectresponse?view=azure-dotnet-preview) nesne. Zaman serisi anomalileri içeriyorsa, yanıtın yineleme [IsAnomaly](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.entiredetectresponse.isanomaly?view=azure-dotnet-preview) değerler ve tüm olan Yazdır `true`. Karşılanmadığı, bu değerler, anormal veri noktası dizini karşılık gelir.

[!code-csharp[EntireDetectSampleAsync() function](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=entireDatasetExample)]

### <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>En son veri noktası durumunu anomali algılama

İstemcinin çağıran bir yöntem oluşturma [LastDetectAsync()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.lastdetectasync?view=azure-dotnet-preview#Microsoft_Azure_CognitiveServices_AnomalyDetector_AnomalyDetectorClientExtensions_LastDetectAsync_Microsoft_Azure_CognitiveServices_AnomalyDetector_IAnomalyDetectorClient_Microsoft_Azure_CognitiveServices_AnomalyDetector_Models_Request_System_Threading_CancellationToken_) yöntemiyle `Request` nesne ve yanıt olarak await bir [LastDetectResponse](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.lastdetectresponse?view=azure-dotnet-preview) nesne. Yanıtın denetleyin [IsAnomaly](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.lastdetectresponse.isanomaly?view=azure-dotnet-preview) gönderilen en son veri noktası veya bir anomali olup olmadığını belirlemek için özniteliği. 

[!code-csharp[LastDetectSampleAsync() function](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=latestPointExample)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Dotnet ile uygulamayı çalıştırmak `run` uygulama dizininize komutu.

```dotnet
dotnet run
```

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
