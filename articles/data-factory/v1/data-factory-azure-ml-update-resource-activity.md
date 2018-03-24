---
title: Azure Data Factory kullanarak Machine Learning modellerini güncelleştirme | Microsoft Docs
description: Nasıl oluşturulacağını açıklar Azure Data Factory ve Azure Machine Learning kullanarak Tahmine dayalı ardışık düzen oluşturun
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 3702f4b7a58e9ca65a8ee309699a7e31b207159b
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="updating-azure-machine-learning-models-using-update-resource-activity"></a>Azure Machine Learning modellerini kaynak güncelleştirme etkinliği kullanarak güncelleştirme

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive etkinliği](data-factory-hive-activity.md) 
> * [Pig etkinliği](data-factory-pig-activity.md)
> * [MapReduce Activity](data-factory-map-reduce.md)
> * [Hadoop akış etkinliği](data-factory-hadoop-streaming-activity.md)
> * [Spark etkinliği](data-factory-spark.md)
> * [Machine Learning Batch Yürütme Etkinliği](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning Kaynak Güncelleştirme Etkinliği](data-factory-azure-ml-update-resource-activity.md)
> * [Saklı Yordam Etkinliği](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL Etkinliği](data-factory-usql-activity.md)
> * [.NET özel etkinlik](data-factory-use-custom-activities.md)


> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [makine öğrenimi modellerini veri fabrikasında sürüm 2 güncelleştirme](../update-machine-learning-models.md).

Bu makalede ana Azure Data Factory - Azure Machine Learning tümleştirme makale tamamlar: [Azure Machine Learning ve Azure Data Factory kullanarak Tahmine dayalı işlem hatlarını oluşturmak](data-factory-azure-ml-batch-execution-activity.md). Zaten yapmadıysanız, bu makalede okumadan önce ana makalesini gözden geçirin. 

## <a name="overview"></a>Genel Bakış
Zaman içinde denemeler Puanlama Azure ML Tahmine dayalı modelleri yeni giriş veri kümeleri kullanarak retrained gerekir. Yeniden eğitme ile tamamladıktan sonra retrained ML modeliyle Puanlama web hizmetini güncelleştirmek istiyorsunuz. Web Hizmetleri aracılığıyla yeniden eğitme ve güncelleştirme Azure ML modelleri etkinleştirmek için tipik adımları şunlardır:

1. Bir deneme oluşturma [Azure ML Studio](https://studio.azureml.net).
2. Modelle memnun kaldığınızda, her ikisi için web hizmetleri yayımlamak için Azure ML Studio kullanma **eğitim denemenizi** ve puanlama /**Tahmine dayalı denemeye**.

Aşağıdaki tabloda bu örnekte kullanılan web hizmetleri açıklanmaktadır.  Bkz: [yeniden eğitme Machine Learning modellerini programlama aracılığıyla](../../machine-learning/machine-learning-retrain-models-programmatically.md) Ayrıntılar için.

- **Web hizmeti eğitim** - eğitim verileri alır ve eğitilmiş modeller üretir. Yeniden eğitme çıktı, Azure Blob depolamada .ilearner dosyasıdır. **Endpoint varsayılan** , eğitim yayımladığınızda, bir web hizmeti olarak denemeler için otomatik olarak oluşturulur. Daha fazla uç noktaları oluşturabilirsiniz, ancak bu örnek yalnızca varsayılan uç noktası kullanır.
- **Web hizmeti Puanlama** - etiketlenmemiş veri örnekleri alır ve tahminleri yapar. Tahmin çıktısını bir .csv dosyası veya deneme yapılandırmasına bağlı olarak bir Azure SQL veritabanında satır gibi çeşitli formlar olabilir. Bir web hizmeti olarak Tahmine dayalı denemeye yayımladığınızda, varsayılan uç sizin için otomatik olarak oluşturulur. 

Aşağıdaki resimde, eğitim ve uç noktaları Azure ML Puanlama arasındaki ilişki gösterilmektedir.

![Web Hizmetleri](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)

Çağırabilirsiniz **web hizmeti eğitim** kullanarak **Azure ML toplu iş yürütme etkinliği**. Eğitim web hizmetini çağırmak için Puanlama verileri (web hizmeti Puanlama) bir Azure ML web Hizmeti'ni çağırmadan olarak aynıdır. Önceki bölümlerde ayrıntılı bir Azure Data Factory işlem hattı Azure ML web hizmetinden çağırmak nasıl kapsar. 

Çağırabilirsiniz **web hizmeti Puanlama** kullanarak **Azure ML güncelleştirme kaynak etkinliği** web hizmeti ile yeni eğitilen modeli güncelleştirmek için. Aşağıdaki örnekler, bağlantılı hizmet tanımlarını sağlar: 

## <a name="scoring-web-service-is-a-classic-web-service"></a>Klasik web hizmeti olan Web hizmeti Puanlama
Puanlama web hizmeti ise bir **Klasik web hizmeti**, ikinci oluşturma **varsayılan olmayan ve güncelleştirilebilir endpoint** Azure portalını kullanarak. Bkz [uç noktalar oluşturmak](../../machine-learning/machine-learning-create-endpoint.md) adımları makalesinde bulabilirsiniz. Varsayılan olmayan güncelleştirilebilir uç nokta oluşturduktan sonra aşağıdaki adımları uygulayın:

* Tıklatın **toplu iş yürütme** URI değeri almak için **mlEndpoint** JSON özelliği.
* Tıklatın **güncelleştirme kaynağı** URI değeri almak için bağlantı **updateResourceEndpoint** JSON özelliği. Uç nokta sayfasında kendisini (sağ alt köşedeki) API anahtardır.

![güncelleştirilebilir uç noktası](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

Aşağıdaki örnek AzureML bağlantılı hizmeti için bir örnek JSON tanım sağlar. Bağlantılı hizmet apikey ile yapılan kimlik doğrulaması için kullanır.  

```json
{
    "name": "updatableScoringEndpoint2",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--scoring experiment--/jobs",
            "apiKey": "endpoint2Key",
            "updateResourceEndpoint": "https://management.azureml.net/workspaces/xxx/webservices/--scoring experiment--/endpoints/endpoint2"
        }
    }
}
```

## <a name="scoring-web-service-is-azure-resource-manager-web-service"></a>Web hizmeti Puanlama Azure Resource Manager web hizmetidir 
Web hizmeti bir Azure Kaynak Yöneticisi uç noktasını kullanıma sunar web hizmetinin yeni tür ise, ikinci eklemek gerekmez **varsayılan olmayan** uç noktası. **UpdateResourceEndpoint** bağlantılı hizmetteki biçimi şöyledir: 

```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview. 
```

Değerleri URL'deki yer tutucu için web hizmeti üzerinde sorgulanırken alabileceğiniz [Azure Machine Learning Web Hizmetleri portalı](https://services.azureml.net/). Güncelleştirme kaynağı endpoint yeni tür bir AAD (Azure Active Directory) belirteci gerektirir. Belirtin **servicePrincipalId** ve **servicePrincipalKey**içinde AzureML bağlantılı hizmetinin. Bkz: [hizmet sorumlusu oluşturma ve Azure kaynak yönetmek için izinleri atamak nasıl](../../azure-resource-manager/resource-group-create-service-principal-portal.md). Örnek AzureML bağlantılı hizmet tanımı aşağıda verilmiştir: 

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "description": "The linked service for AML web service.",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/0000000000000000000000000000000000000/services/0000000000000000000000000000000000000/jobs?api-version=2.0",
            "apiKey": "xxxxxxxxxxxx",
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.MachineLearning/webServices/myWebService?api-version=2016-05-01-preview",
            "servicePrincipalId": "000000000-0000-0000-0000-0000000000000",
            "servicePrincipalKey": "xxxxx",
            "tenant": "mycompany.com"
        }
    }
}
```

Aşağıdaki senaryoyu daha fazla ayrıntı sağlar. Yeniden eğitme ve bir Azure Data Factory işlem hattı Azure ML modellerinden güncelleştirmek için bir örnek vardır.

## <a name="scenario-retraining-and-updating-an-azure-ml-model"></a>Senaryo: yeniden eğitme ve bir Azure ML model güncelleştiriliyor
Bu bölümde kullanan örnek bir işlem hattı **Azure ML toplu iş yürütme etkinliği** bir model yeniden eğitme için. Ardışık Düzen de kullanır **Azure ML kaynak güncelleştirme etkinliği** modeli Puanlama web hizmeti güncelleştirmek için. Ayrıca bu bölüm JSON parçacıklarını tüm bağlı hizmetler, veri kümelerini ve ardışık düzen örnekte için sağlar.

Burada, örnek ardışık diyagram görünümü verilmiştir. Gördüğünüz gibi Azure ML toplu iş yürütme etkinliği eğitim girdi alır ve eğitim çıktı (iLearner dosyası) oluşturur. Azure ML kaynak güncelleştirme etkinliği Bu eğitim çıktıyı alır ve puanlama web hizmeti uç noktası modelinde güncelleştirir. Kaynak güncelleştirme etkinliği herhangi bir çıktı üretmez. PlaceholderBlob Azure Data Factory hizmeti tarafından ardışık çalıştırmak için gerekli olan yalnızca bir kukla çıktı veri kümesi ' dir.

![ardışık düzeni diyagramı](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

### <a name="azure-blob-storage-linked-service"></a>Azure Blob storage bağlı hizmeti:
Azure Storage aşağıdaki veriler tutar:

* Eğitim verileri. Azure ML eğitim web hizmeti için giriş verileri.  
* iLearner dosya. Azure ML eğitim web hizmetinden çıktı. Bu ayrıca güncelleştirme kaynağı etkinliğin girişi dosyasıdır.  

Bağlantılı hizmeti örnek JSON tanımını şöyledir:

```JSON
{
    "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=name;AccountKey=key"
        }
    }
}
```

### <a name="training-input-dataset"></a>Eğitim girdi veri kümesi:
Aşağıdaki veri kümesi Azure ML eğitim web hizmeti için giriş eğitim verilerini temsil eder. Azure ML toplu iş yürütme etkinliği bu veri kümesine bir girdi olarak alır.

```JSON
{
    "name": "trainingData",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "labeledexamples",
            "fileName": "labeledexamples.arff",
            "format": {
                "type": "TextFormat"
            }
        },
        "availability": {
            "frequency": "Week",
            "interval": 1
        },
        "policy": {          
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

### <a name="training-output-dataset"></a>Eğitim çıktı veri kümesi:
Aşağıdaki veri kümesi Azure ML eğitim web hizmetinden çıktı iLearner dosyasını temsil eder. Azure ML toplu iş yürütme etkinliği bu veri kümesini oluşturur. Bu veri kümesi de, Azure ML güncelleştirme kaynağı etkinliğin girişi olur.

```JSON
{
    "name": "trainedModelBlob",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "trainingoutput",
            "fileName": "model.ilearner",
            "format": {
                "type": "TextFormat"
            }
        },
        "availability": {
            "frequency": "Week",
            "interval": 1
        }
    }
}
```

### <a name="linked-service-for-azure-ml-training-endpoint"></a>Azure ML eğitim uç noktası için bağlı hizmet
Aşağıdaki JSON parçacığı eğitim web hizmeti için varsayılan uç noktaları bir Azure Machine Learning bağlantılı hizmeti tanımlar.

```JSON
{    
    "name": "trainingEndpoint",
      "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--training experiment--/jobs",
              "apiKey": "myKey"
        }
      }
}
```

İçinde **Azure ML Studio**, değerlerini almak için aşağıdakileri **mlEndpoint** ve **apikey ile yapılan**:

1. Tıklatın **WEB Hizmetleri** sol menüde.
2. Tıklatın **web hizmeti eğitim** web hizmetleri listesinde.
3. Kopyala'yı tıklatın **API anahtarı** metin kutusu. Anahtar panoya veri fabrikası JSON düzenleyicisine yapıştırın.
4. İçinde **Azure ML studio**, tıklatın **toplu iş yürütme** bağlantı.
5. Kopya **istek URI'si** gelen **isteği** bölümünde ve veri fabrikası JSON düzenleyicisine yapıştırın.   

### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a>Bağlantılı hizmeti Azure ML güncelleştirilebilir Puanlama uç noktası için:
Aşağıdaki JSON parçacığı Puanlama web hizmeti varsayılan olmayan güncelleştirilebilir uç noktasına işaret eden bir Azure Machine Learning bağlantılı hizmeti tanımlar.  

```JSON
{
    "name": "updatableScoringEndpoint2",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/00000000eb0abe4d6bbb1d7886062747d7/services/00000000026734a5889e02fbb1f65cefd/jobs?api-version=2.0",
            "apiKey": "sooooooooooh3WvG1hBfKS2BNNcfwSO7hhY6dY98noLfOdqQydYDIXyf2KoIaN3JpALu/AKtflHWMOCuicm/Q==",
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/myWebService?api-version=2016-05-01-preview",
            "servicePrincipalId": "fe200044-c008-4008-a005-94000000731",
            "servicePrincipalKey": "zWa0000000000Tp6FjtZOspK/WMA2tQ08c8U+gZRBlw=",
            "tenant": "mycompany.com"
        }
    }
}
```

### <a name="placeholder-output-dataset"></a>Yer tutucu çıktı veri kümesi:
Azure ML güncelleştirme kaynağı etkinlik herhangi bir çıktı üretmez. Ancak, Azure Data Factory işlem hattı zamanlamasını sürücü için bir çıkış veri kümesi gerektirir. Bu nedenle, bir kukla/yer tutucu veri kümesi bu örnekte kullanırız.  

```JSON
{
    "name": "placeholderBlob",
    "properties": {
        "availability": {
            "frequency": "Week",
            "interval": 1
        },
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "any",
            "format": {
                "type": "TextFormat"
            }
        }
    }
}
```

### <a name="pipeline"></a>İşlem hattı
Ardışık düzen iki etkinlik vardır: **AzureMLBatchExecution** ve **AzureMLUpdateResource**. Azure ML toplu iş yürütme etkinliği giriş olarak eğitim verileri alır ve bir iLearner dosya bir çıktı olarak üretir. Etkinlik giriş eğitim verilerle eğitim web hizmeti (web hizmeti olarak sunulan eğitim denemenizi) çağırır ve ilearner dosya webservice alır. PlaceholderBlob Azure Data Factory hizmeti tarafından ardışık çalıştırmak için gerekli olan yalnızca bir kukla çıktı veri kümesi ' dir.

![ardışık düzeni diyagramı](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [
            {
                "name": "retraining",
                "type": "AzureMLBatchExecution",
                "inputs": [
                    {
                        "name": "trainingData"
                    }
                ],
                "outputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "typeProperties": {
                    "webServiceInput": "trainingData",
                    "webServiceOutputs": {
                        "output1": "trainedModelBlob"
                    }              
                 },
                "linkedServiceName": "trainingEndpoint",
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            },
            {
                "type": "AzureMLUpdateResource",
                "typeProperties": {
                    "trainedModelName": "Training Exp for ADF ML [trained model]",
                    "trainedModelDatasetName" :  "trainedModelBlob"
                },
                "inputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "outputs": [
                    {
                        "name": "placeholderBlob"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "AzureML Update Resource",
                "linkedServiceName": "updatableScoringEndpoint2"
            }
        ],
        "start": "2016-02-13T00:00:00Z",
           "end": "2016-02-14T00:00:00Z"
    }
}
```
