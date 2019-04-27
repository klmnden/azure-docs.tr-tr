---
title: Azure Data Factory kullanarak makine öğrenimi modellerini güncelleştirme | Microsoft Docs
description: Nasıl oluşturulacağını açıklar Azure Data Factory ve Azure Machine Learning kullanarak öngörülebilir komut zincirleri oluşturma
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 0c0e0e3983344bba76f5f305ecaf73f91110f3bc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60567338"
---
# <a name="updating-azure-machine-learning-models-using-update-resource-activity"></a>Kaynak güncelleştirme etkinliği kullanarak Azure Machine Learning modellerini güncelleştirme

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive etkinliği](data-factory-hive-activity.md) 
> * [Pig etkinliği](data-factory-pig-activity.md)
> * [MapReduce etkinliği](data-factory-map-reduce.md)
> * [Hadoop akış etkinliğinde](data-factory-hadoop-streaming-activity.md)
> * [Spark etkinliği](data-factory-spark.md)
> * [Machine Learning Batch Yürütme Etkinliği](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning Kaynak Güncelleştirme Etkinliği](data-factory-azure-ml-update-resource-activity.md)
> * [Saklı Yordam Etkinliği](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL Etkinliği](data-factory-usql-activity.md)
> * [.NET özel etkinliği](data-factory-use-custom-activities.md)


> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [Data factory'de machine learning modellerini güncelleştirme](../update-machine-learning-models.md).

Bu makalede, ana Azure Data Factory - Azure Machine Learning tümleştirme makale tamamlar: [Azure Machine Learning ve Azure Data Factory kullanarak öngörülebilir komut zincirleri oluşturma](data-factory-azure-ml-batch-execution-activity.md). Zaten yapmadıysanız, bu makalede okumadan önce ana makalesini gözden geçirin. 

## <a name="overview"></a>Genel Bakış
Zaman içinde yeni bir giriş veri kümeleri kullanarak eğitilebileceği Azure ML denemeleri Puanlama Tahmine dayalı modelleri gerekir. Yeniden eğitme ile işiniz bittiğinde ile retrained ML model Puanlama web hizmeti güncelleştirmek istediğiniz. Web Hizmetleri aracılığıyla Azure ML modelleri yeniden eğitme ve güncelleştirme etkinleştirmek için tipik adımları şunlardır:

1. Bir deneme oluşturma [Azure ML Studio](https://studio.azureml.net).
2. Modelle memnun kaldığınızda, Azure ML Studio hem de web hizmetleri yayımlayacağınızı **eğitim denemesini** ve puanlama /**Tahmine dayalı denemeye**.

Aşağıdaki tabloda, bu örnekte kullanılan web hizmetleri açıklanmaktadır.  Bkz: [yeniden eğitme Machine Learning modellerini programlama aracılığıyla](../../machine-learning/machine-learning-retrain-models-programmatically.md) Ayrıntılar için.

- **Web hizmeti eğitim** - eğitim verilerini alır ve eğitilen modelleri üretir. Yeniden eğitme çıktı, Azure Blob depolama alanındaki bir .ilearner dosyasıdır. **Varsayılan uç nokta** , eğitim yayımladığınızda bir web hizmeti olarak denemeler için otomatik olarak oluşturulur. Daha fazla uç noktaları oluşturabilirsiniz, ancak bu örnek yalnızca varsayılan uç nokta kullanır.
- **Puanlama Web hizmeti** - etiketlenmemiş veri örnekleri alır ve Öngörüler sağlar. Tahmin çıktısını bir .csv dosyası veya denemeyi yapılandırmasına bağlı olarak bir Azure SQL veritabanındaki satırlar gibi çeşitli forms olabilir. Bir web hizmeti olarak Tahmine dayalı denemeye yayımladığınızda, varsayılan uç nokta sizin için otomatik olarak oluşturulur. 

Aşağıdaki resimde, eğitim ve uç noktaları Azure ML'de Puanlama arasındaki ilişki gösterilmektedir.

![Web Hizmetleri](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)

Çağırabilirsiniz **web hizmeti eğitim** kullanarak **Azure ML Batch yürütme etkinliği**. Bir eğitim web hizmetini çağırmak için Puanlama veri (Puanlama web hizmeti) bir Azure ML web hizmetini çağırmak olarak aynıdır. Önceki bölümlerde ayrıntılı bir Azure Data Factory işlem hattından bir Azure ML web hizmetini çağırmak nasıl kapsar. 

Çağırabilirsiniz **Puanlama web hizmeti** kullanarak **Azure ML güncelleştirmek kaynak etkinliği** web hizmeti ile yeni eğitilen modeli güncelleştirmek için. Aşağıdaki örneklerde, bağlı hizmet tanımları sağlanır: 

## <a name="scoring-web-service-is-a-classic-web-service"></a>Bir Klasik web hizmeti, Puanlama Web hizmeti
Puanlama web hizmeti ise bir **Klasik web hizmeti**, ikinci oluşturma **varsayılan olmayan ve güncelleştirilebilir uç nokta** Azure portalını kullanarak. Bkz [uç noktaları oluşturma](../../machine-learning/machine-learning-create-endpoint.md) adımları makalesinde bulabilirsiniz. Varsayılan olmayan güncelleştirilebilir uç nokta oluşturduktan sonra aşağıdaki adımları uygulayın:

* Tıklayın **toplu iş yürütme** URI değerini almak için **mlEndpoint** JSON özelliği.
* Tıklayın **kaynak güncelleştirme** URI değerini almak için bağlantı **updateResourceEndpoint** JSON özelliği. Uç nokta sayfasının kendisine (sağ alt köşedeki) API anahtarını açıktır.

![güncelleştirilebilir uç noktası](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

Aşağıdaki örnek, bir örnek JSON tanımı AzureML bağlantılı hizmeti için sağlar. Bağlı hizmet kimlik doğrulaması için apiKey kullanır.  

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

## <a name="scoring-web-service-is-azure-resource-manager-web-service"></a>Azure Resource Manager web hizmeti, Puanlama Web hizmeti 
Web hizmeti bir Azure Resource Manager uç noktasını kullanıma sunar ve web hizmeti yeni türü ise, ikinci eklemek gerekmez **varsayılan olmayan** uç noktası. **UpdateResourceEndpoint** bölümünde bağlı hizmetin şu biçimdedir: 

```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview. 
```

Değerler için yer tutucu URL'nin web hizmeti üzerinde sorgulanırken alabileceğiniz [Azure Machine Learning Web Hizmetleri portalını](https://services.azureml.net/). Yeni güncelleştirme kaynak uç noktası türü, bir AAD (Azure Active Directory) belirteci gerektirir. Belirtin **Serviceprincipalıd** ve **servicePrincipalKey**birçok bağlı hizmeti. Bkz: [hizmet sorumlusu oluşturma ve Azure kaynak yönetme izinlerini atama](../../active-directory/develop/howto-create-service-principal-portal.md). AzureML bağlantılı hizmet tanımı örneği aşağıdadır: 

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

Aşağıdaki senaryoda, daha fazla ayrıntı sağlar. Yeniden eğitme ve bir Azure Data Factory işlem hattı Azure ML modelinden güncelleştirmek için bir örnek var.

## <a name="scenario-retraining-and-updating-an-azure-ml-model"></a>Senaryo: yeniden eğitme ve bir Azure ML model güncelleştiriliyor
Bu bölümde kullanan bir örnek işlem hattı **Azure ML Batch yürütme etkinliği** modeli yeniden eğitme için. İşlem hattı de kullanır **Azure ML kaynak güncelleştirme etkinliği** model Puanlama web hizmeti içinde güncelleştirilecek. Bölüm ayrıca tüm bağlı hizmetler, veri kümeleri ve bu örnekteki işlem hattı JSON parçacıklarını sağlar.

Örnek işlem hattı diyagram görünümü aşağıda verilmiştir. Gördüğünüz gibi Azure ML Batch yürütme etkinliği Eğitim giriş alır ve eğitim çıktı (iLearner dosyası) oluşturur. Azure ML kaynak güncelleştirme etkinliği, bu eğitim çıkış alır ve model Puanlama web hizmeti uç noktasını güncelleştirir. Kaynak güncelleştirme etkinliği herhangi bir çıktı üretmez. PlaceholderBlob yalnızca Azure Data Factory hizmeti tarafından işlem hattını çalıştırmak için gerekli olan bir işlevsiz bir çıktı veri kümesidir.

![işlem hattı diyagramı](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

### <a name="azure-blob-storage-linked-service"></a>Azure Blob Depolama bağlı hizmeti:
Azure depolama şu veri tutar:

* Eğitim verileri. Azure ML eğitim web hizmeti giriş verileri.  
* iLearner dosya. Azure ML eğitim web hizmetinden çıkışı. Bu dosya, kaynak güncelleştirme etkinliği için bir giriş da olabilir.  

Örnek JSON tanımı bağlı hizmetin şu şekildedir:

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

### <a name="training-input-dataset"></a>Eğitim giriş veri kümesi:
Aşağıdaki veri kümesi, Azure ML eğitim web hizmeti giriş eğitim verilerini temsil eder. Azure ML Batch yürütme etkinliği, bu veri kümesi bir girdi olarak alır.

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

### <a name="training-output-dataset"></a>Eğitim çıkış veri kümesi:
Aşağıdaki veri kümesi Azure ML eğitim web hizmetinden çıkış iLearner dosyasını temsil eder. Azure ML Batch yürütme etkinliği, bu veri kümesi üretir. Bu veri kümesi, ayrıca Azure ML kaynak güncelleştirme etkinliği için bir giriş olabilir.

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
Eğitim web hizmeti varsayılan uç noktaya işaret eden bir Azure Machine Learning bağlı hizmetinin aşağıdaki JSON kod parçacığında tanımlar.

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

İçinde **Azure ML Studio**, değerlerini almak için aşağıdakileri **mlEndpoint** ve **apiKey**:

1. Tıklayın **WEB Hizmetleri** sol menüsünde.
2. Tıklayın **web hizmeti eğitim** web hizmetleri listesinde.
3. Yanındaki Kopyala **API anahtarı** metin kutusu. Anahtar, panoya Data Factory JSON düzenleyicisine yapıştırın.
4. İçinde **Azure ML studio**, tıklayın **toplu iş yürütme** bağlantı.
5. Kopyalama **istek URI** gelen **istek** bölümünde ve Data Factory JSON düzenleyicisine yapıştırın.   

### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a>Azure ML güncelleştirilebilir Puanlama uç noktası için bağlı hizmet:
Varsayılan olmayan güncelleştirilebilir bitiş Puanlama web hizmeti noktasına işaret eden bir Azure Machine Learning bağlı hizmetinin aşağıdaki JSON kod parçacığında tanımlar.  

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
Azure ML kaynak güncelleştirme etkinliği herhangi bir çıktı üretmez. Ancak, Azure Data Factory işlem hattı zamanlamasını sürücü için bir çıktı veri kümesi gerektirir. Bu nedenle, bir dummy/yer tutucu veri kümesi bu örnekte kullanırız.  

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
İşlem hattı iki etkinlik içerir: **AzureMLBatchExecution** ve **AzureMLUpdateResource**. Azure ML Batch yürütme etkinliği, giriş olarak eğitim verilerini alır ve çıktı olarak bir iLearner dosyası üretir. Etkinlik, giriş eğitim verilerle eğitim web hizmeti (bir web hizmeti olarak kullanıma sunulan eğitim denemesini) çağırır ve webservice olan ilearner dosyasını alır. PlaceholderBlob yalnızca Azure Data Factory hizmeti tarafından işlem hattını çalıştırmak için gerekli olan bir işlevsiz bir çıktı veri kümesidir.

![işlem hattı diyagramı](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

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
