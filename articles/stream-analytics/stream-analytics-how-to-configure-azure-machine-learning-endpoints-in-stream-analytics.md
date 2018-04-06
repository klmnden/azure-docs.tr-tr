---
title: Stream Analytics Azure Machine Learning uç noktaları kullanma | Microsoft Docs
description: Stream Analytics makine dili kullanıcı tanımlı işlevler
keywords: ''
documentationcenter: ''
services: stream-analytics
author: jseb225
manager: ryanw
ms.assetid: 406b258f-b8c2-4e55-953c-b7f84e8e5354
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeanb
ms.openlocfilehash: a7d76d6015f8e9f08d3493b1c1e237858c341592
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="machine-learning-integration-in-stream-analytics"></a>Stream Analytics öğrenme tümleştirmesine makine
Akış analizi, Azure Machine Learning Uç noktalara duyurmak kullanıcı tanımlı işlevler destekler. Bu özellik için REST API desteği ayrıntılı olarak [Stream Analytics REST API Kitaplığı](https://msdn.microsoft.com/library/azure/dn835031.aspx). Bu makalede bu özelliği Stream Analytics de başarılı uygulama için gerekli ek bilgiler sağlar. Öğreticinin Ayrıca gönderilen ve kullanılabilir [burada](stream-analytics-machine-learning-integration-tutorial.md).

## <a name="overview-azure-machine-learning-terminology"></a>Genel Bakış: Azure Machine Learning terminolojisi
Microsoft Azure Machine Learning derleme, test ve verilerinizi Tahmine dayalı analiz çözümlerini dağıtmak için kullanabileceğiniz bir işbirliğine dayalı, sürükle ve bırak aracı sağlar. Bu aracı adlı *Azure Machine Learning Studio*. Studio Machine Learning kaynakları ile etkileşim kurar ve kolayca derleme, test ve üzerinde tasarımınızı yinelemek için kullanılır. Bu kaynaklar ve tanımları aşağıda verilmiştir.

* **Çalışma alanı**: *çalışma* tüm diğer Machine Learning kaynakları birlikte yönetim ve denetimi için bir kapsayıcı tutan bir kapsayıcıdır.
* **Deneme**: *denemeler* veri kümeleri kullanan ve makine öğrenimi modeli eğitmek için veri bilimcilerine tarafından oluşturulur.
* **Uç nokta**: *uç noktaları* özellikleri giriş olarak alın, belirtilen makine öğrenimi modeline uygulayın ve puanlanmış çıkışı döndürmek için kullanılan Azure Machine Learning nesnesi.
* **Web hizmeti Puanlama**: A *webservice Puanlama* uç noktaları yukarıda belirtildiği gibi koleksiyonudur.

Her uç nokta toplu iş yürütme ve zaman uyumlu yürütme için API'lere sahiptir. Akış analizi, zaman uyumlu yürütme kullanır. Belirli hizmet adlı bir [istek/yanıt hizmet](../machine-learning/studio/consume-web-services.md) AzureML Studio'da.

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a>Machine Learning akış analizi işleri için gerekli kaynakları
Stream Analytics amaçları doğrultusunda işi, bir istek/yanıt uç noktası işleme bir [apikey ile yapılan](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md), ve bir swagger tanımı başarılı yürütme için tüm gerekli. Akış analizi için swagger uç nokta URL'si oluşturur, arabirimi arar ve bir varsayılan UDF tanımı kullanıcıya döndürür bir ek bir uç nokta vardır.

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a>Bir akış analizi ve makine öğrenimi UDF REST API aracılığıyla yapılandırın
REST API'lerini kullanarak işinizi Azure makine dili işlevleri çağırmak üzere yapılandırabilirsiniz. Adımlar aşağıdaki gibidir:

1. Akış Analizi işi oluşturma
2. Bir giriş tanımlayın
3. Bir çıkış tanımlayın
4. Kullanıcı tanımlı bir işlev (UDF) oluşturun
5. UDF çağıran bir akış analizi dönüştürmesi yazma
6. İşi Başlat

## <a name="creating-a-udf-with-basic-properties"></a>Bir UDF temel özellikleri ile oluşturma
Örnek olarak, aşağıdaki örnek kod adlı bir skaler UDF oluşturur *newudf* bir Azure Machine Learning uç noktasına bağlar. Unutmayın *endpoint* (hizmet URI'si), seçtiğiniz hizmet için API Yardım sayfasında bulunabilir ve *apikey ile yapılan* Services ana sayfasında bulunabilir.

````
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>  
````

Örnek istek gövdesi:  

````
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
````

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>RetrieveDefaultDefinition uç noktası için varsayılan UDF çağırın
Çatıyı UDF oluşturulduktan sonra UDF tam tanımı gereklidir. RetreiveDefaultDefinition uç noktası, bir Azure Machine Learning uç noktasına bağlı skaler bir işlev için varsayılan tanımı alma yardımcı olur. Yükü bir Azure Machine Learning uç noktasına bağlı skaler bir işlev için varsayılan UDF tanımını almanızı gerektirir. PUT isteği sırasında zaten sağlanmış gibi gerçek uç noktasını belirtmiyor. Akış analizi açıkça sağlanırsa istek sağlanan uç noktanın çağırır. Aksi durumda ilk olarak başvurulan bir kullanır. Burada tek bir dize türü tek bir çıktı parametresi (cümle) ve döndürür UDF alır bu cümleyi "düşünceleri" etiketi belirten dize.

````
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
````

Örnek istek gövdesi:  

````
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
````

Bu arama bir şey aşağıda istediğiniz bir örnek çıktı.  

````
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
````

## <a name="patch-udf-with-the-response"></a>Düzeltme eki UDF yanıt
Şimdi UDF önceki Yanıtla, aşağıda gösterildiği gibi düzeltme eklerinin gerekir.

````
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
````

İstek gövdesi (RetrieveDefaultDefinition çıktısını):

````
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
````

## <a name="implement-stream-analytics-transformation-to-call-the-udf"></a>UDF çağırmak için Stream Analytics dönüşüm uygulayın
Şimdi giriş her olay için (burada scoreTweet olarak adlandırılır) UDF sorgulayabilir ve bu olay için bir yanıt için bir çıktı yazma.  

````
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
````


## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
