---
title: Azure Stream Analytics Machine Learning uç noktaları kullanma
description: Bu makalede, Azure Stream Analytics'te makine dili kullanıcı tanımlı işlevleri kullanmayı açıklar.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.openlocfilehash: dfb34f8c0fca792618860e0a8d5a1bf1736f3611
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65416043"
---
# <a name="machine-learning-integration-in-stream-analytics-preview"></a>Machine Learning tümleştirme Stream Analytics (Önizleme)
Stream Analytics, Azure Machine Learning Uç noktalara çağıran kullanıcı tanımlı işlevleri destekler. Bu özelliği için REST API desteği ayrıntılı olarak [Stream Analytics REST API Kitaplığı](https://msdn.microsoft.com/library/azure/dn835031.aspx). Bu makalede, Stream Analytics bu özelliği başarılı uygulaması için gerekli ek bilgileri sağlar. Bir öğretici de forumumuza gönderildi ve kullanılabilir [burada](stream-analytics-machine-learning-integration-tutorial.md).

## <a name="overview-azure-machine-learning-terminology"></a>Genel Bakış: Azure Machine Learning terminolojisi
Microsoft Azure Machine Learning, derleme, test etme ve verilerinizde Tahmine dayalı analiz çözümleri dağıtmak için kullanabileceğiniz bir işbirliğine dayalı, sürükle ve bırak aracı sağlar. Bu aracı adlı *Azure Machine Learning Studio*. Studio, Machine Learning kaynakları ile etkileşim kurar ve kolayca oluşturun, test ve tasarımınızı yinelemek için kullanılır. Bu kaynakları ve bunların tanımlarının aşağıda verilmiştir.

* **Çalışma alanı**: *Çalışma* birlikte yönetim ve denetim için bir kapsayıcı içindeki diğer tüm Machine Learning kaynakları bir arada tutan bir kapsayıcıdır.
* **Deneme**: *Denemeleri* veri kümelerini kullanan ve makine öğrenme modeli eğitmek için veri uzmanları tarafından oluşturulur.
* **Uç nokta**: *Uç noktaları* Azure Machine Learning giriş olarak özelliklerini almak, belirtilen makine öğrenme modeli uygulamak ve puanlanmış çıkışı döndürür için kullanılan nesne.
* **Puanlama Web hizmeti**: A *Puanlama Web hizmeti* uç noktaları yukarıda da belirtildiği gibi koleksiyonudur.

Her uç nokta toplu yürütme ve zaman uyumlu yürütme için API vardır. Stream Analytics, zaman uyumlu yürütme kullanır. Hizmete adlı bir [istek/yanıt hizmeti](../machine-learning/studio/consume-web-services.md) Azure Machine Learning Studio'da.

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a>Machine Learning Stream Analytics işleri için gerekli kaynakları
Stream Analytics amaçları için iş, bir istek/yanıt uç noktası işlenirken bir [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md), ve bir swagger tanımı başarılı yürütme için tüm gerekli. Stream Analytics, swagger uç nokta URL'sini oluşturur, arabirimi oluşturan arar ve varsayılan UDF tanımı kullanıcıya döndürür. ek bir uç nokta sahiptir.

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a>Bir Stream Analytics ve Machine Learning REST API aracılığıyla UDF yapılandırın
REST API'lerini kullanarak işinizi Azure makine dil işlevleri çağırmak için yapılandırabilirsiniz. Adımlar aşağıdaki gibidir:

1. Akış Analizi işi oluşturma
2. Bir girdi tanımlama
3. Bir çıkış tanımlayın
4. Bir kullanıcı tanımlı işlev (UDF) oluşturma
5. UDF çağıran bir Stream Analytics dönüştürme yazma
6. İşi başlatma

## <a name="creating-a-udf-with-basic-properties"></a>Temel özelliklere sahip bir UDF oluşturma
Örneğin, aşağıdaki örnek kod adlı bir skaler UDF oluşturur *newudf* bir Azure Machine Learning uç noktasına bağlanır. Unutmayın *uç nokta* (hizmet URI'si), seçtiğiniz hizmet API Yardım sayfasında bulunabilir ve *apiKey* Services ana sayfasında bulunabilir.

```
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
```

Örnek istek gövdesi:

```json
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77fb4b46bf2a30c63c078dca/services/b7be5e40fd194258796fb402c1958eaf/execute ",
                        "apiKey": "replacekeyhere"
                    }
                }
            }
        }
    }
```

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>Varsayılan UDF için RetrieveDefaultDefinition uç noktası çağrısı
UDF eksiksiz tanımını çatıyı UDF oluşturulduktan sonra gereklidir. RetrieveDefaultDefinition uç nokta bir Azure Machine Learning uç noktasına bağlı bir skaler işlevi için varsayılan tanımını almanıza yardımcı olur. Aşağıdaki yükü, bir Azure Machine Learning uç noktasına bağlı bir skaler işlevi için varsayılan UDF tanımı gerektirir. PUT isteği sırasında zaten sağlandıysa gibi gerçek bir uç noktasını belirtmiyor. Stream Analytics, açıkça sağlanırsa, istekte sağlanan uç nokta çağırır. Aksi takdirde ilk olarak başvurulan bir kullanır. Burada tek bir dize parametresi (bir cümlede) ve döndürür tek bir çıkış türü UDF alır, cümle için "yaklaşım" etiketinin belirten dize.

```
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
```

Örnek istek gövdesi:

```json
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
```

Bu arama bir şey aşağıda istediğiniz bir örnek çıktı.

```json
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
```

## <a name="patch-udf-with-the-response"></a>Düzeltme eki UDF yanıt
Artık UDF ile önceki yanıt, aşağıda gösterildiği gibi Yama gerekir.

```
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
```

İstek gövdesi (RetrieveDefaultDefinition çıktısını):

```json
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
```

## <a name="implement-stream-analytics-transformation-to-call-the-udf"></a>UDF çağırmak için Stream Analytics Dönüşüm Uygulama
Artık her giriş olayı için (burada scoreTweet adlı) UDF sorgulayabilir ve bu olay için bir yanıt yazmak için bir çıktı.

```json
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
```


## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
