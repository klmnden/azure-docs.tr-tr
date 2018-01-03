---
title: "Azure Data Factory kullanarak makine öğrenimi modellerini güncelleştirme | Microsoft Docs"
description: "Nasıl oluşturulacağını açıklar Azure Data Factory kullanarak Tahmine dayalı işlem hatlarını oluşturmak ve makine öğrenme"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: shlo
ms.openlocfilehash: a33855213c4bd3a677c8ebbed6624c85138d8ea6
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="update-azure-machine-learning-models-by-using-update-resource-activity"></a>Güncelleştirme kaynağı etkinliğini kullanarak Azure Machine Learning modellerini güncelleştir
Bu makalede ana Azure Data Factory - Azure Machine Learning tümleştirme makale tamamlar: [Azure Machine Learning ve Azure Data Factory kullanarak Tahmine dayalı işlem hatlarını oluşturmak](transform-data-using-machine-learning.md). Zaten yapmadıysanız, bu makalede okumadan önce ana makalesini gözden geçirin. 

## <a name="overview"></a>Genel Bakış
Azure Machine Learning modellerini faaliyete geçirmeye yönelik işleminin bir parçası olarak, modelinizi eğitilmiş ve kaydedilir. Ardından, predicative bir Web hizmeti oluşturmak için kullanabilirsiniz. Web hizmeti web siteleri, panolar ve mobil uygulamaları tüketilebilir.

Machine Learning kullanarak oluşturduğunuz modelleri genellikle statik değildir. Yeni veriler kullanılabilir olduğunda ya da kendi veri tüketici API varsa, model retrained gerekir. Başvurmak [makine öğrenimi modeline yeniden eğitme](../machine-learning/machine-learning-retrain-machine-learning-model.md) Azure Machine Learning modelinde nasıl çağırma hakkında ayrıntılar için. 

Yeniden eğitme sık oluşabilir. Toplu iş yürütme etkinliği ve güncelleştirme kaynağı etkinliği ile yeniden eğitme ve Data Factory kullanarak Tahmine dayalı Web hizmeti güncelleştirme Azure Machine Learning modeli faaliyete geçirebilirsiniz. 

Aşağıdaki resimde, eğitim ve Tahmine dayalı Web Hizmetleri arasındaki ilişki gösterilmektedir. 

![Web Hizmetleri](./media/update-machine-learning-models/web-services.png)

## <a name="azure-machine-learning-update-resource-activity"></a>Azure Machine Learning kaynak güncelleştirme etkinliği 

Aşağıdaki JSON parçacığında, bir Azure Machine Learning toplu iş yürütme etkinliği tanımlar.

```json
{
    "name": "amlUpdateResource",
    "type": "AzureMLUpdateResource",
    "description": "description",
    "linkedServiceName": {
        "type": "LinkedServiceReference",
        "referenceName": "updatableScoringEndpoint2"
    },
    "typeProperties": {
        "trainedModelName": "ModelName",
        "trainedModelLinkedServiceName": {
                    "type": "LinkedServiceReference",
                    "referenceName": "StorageLinkedService"
                },
        "trainedModelFilePath": "ilearner file path"
    }
}
```




| Özellik                      | Açıklama                              | Gerekli |
| :---------------------------- | :--------------------------------------- | :------- |
| ad                          | İşlem hattında etkinlik adı     | Evet      |
| açıklama                   | Etkinlik yaptığı açıklayan metin.  | Hayır       |
| type                          | Azure Machine Learning güncelleştirme kaynağı etkinliği için etkinlik türüdür **AzureMLUpdateResource**. | Evet      |
| linkedServiceName             | Azure Machine Learning updateResourceEndpoint özelliği içeren hizmeti bağlı. | Evet      |
| trainedModelName              | İçinde Web hizmeti denemesinde güncelleştirilecek olan eğitilen Model modülünün adını | Evet      |
| trainedModelLinkedServiceName | Güncelleştirme işlemi tarafından karşıya ilearner dosya tutan Azure Storage bağlı hizmetin adı | Evet      |
| trainedModelFilePath          | Güncelleştirme işlemi tarafından karşıya ilearner dosyasını temsil etmek için trainedModelLinkedService göreli dosya yolu | Evet      |


## <a name="end-to-end-workflow"></a>Uçtan uca iş akışı

Bir modeli ve güncelleştirme Tahmine dayalı Web hizmetleri yeniden eğitme faaliyete geçirmeye yönelik tüm işlemi aşağıdaki adımları içerir: 

- Çağırma **Web hizmeti eğitim** kullanarak **toplu iş yürütme etkinliği**. Eğitim Web hizmeti çağırma aynıdır açıklanan Tahmine dayalı bir Web hizmetini çağırmak [Azure Machine Learning ve veri fabrikası toplu iş yürütme etkinliği kullanarak Tahmine dayalı işlem hatlarını oluşturmak](transform-data-using-machine-learning.md). Web hizmeti eğitim çıktısını Tahmine dayalı Web hizmetini güncelleştirmek için kullanabileceğiniz bir iLearner dosyasıdır. 
- Çağırma **kaynak uç noktasını güncelleyin** , **Tahmine dayalı Web hizmeti** kullanarak **kaynak güncelleştirme etkinliği** Web hizmeti ile yeni eğitilen modeli güncelleştirmek için. 

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning bağlı hizmeti

Çalışmak yukarıda sözü edilen uçtan uca iş akışı için iki bağlı Azure Machine Learning Hizmetleri oluşturmanız gerekir: 

1. Bir Azure Machine eğitim web hizmetine bağlı hizmet Learning, bölümünde belirtildiği şekilde, toplu iş yürütme etkinliği tarafından bu bağlı hizmetin kullanılır [Azure Machine Learning ve veri fabrikası toplu kullanarak Tahmine dayalı ardışık düzen oluşturun Yürütme etkinliği](transform-data-using-machine-learning.md). Eğitim web hizmeti çıktısını sonra Tahmine dayalı web hizmetini güncelleştirmek için kaynak güncelleştirme etkinliği tarafından kullanılan bir iLearner dosyasıdır farktır. 
2. Bir Azure Machine Learning Tahmine dayalı web hizmeti güncelleştirme kaynağı uç noktasına bağlı. Bu bağlı hizmetin yukarıdaki adım döndürülen iLearner dosyasını kullanarak Tahmine dayalı web hizmetini güncelleştirmek için kaynak güncelleştirme etkinliği tarafından kullanılır. 

Azure Machine Learning Web hizmetinizi Klasik Web hizmeti veya yeni bir Web hizmeti için ikinci Azure Machine Learning bağlantılı hizmeti, yapılandırma farklı olur. Farkları ayrı olarak aşağıdaki bölümlerde ele alınmıştır. 

## <a name="web-service-is-new-azure-resource-manager-web-service"></a>Yeni Azure Resource Manager web hizmeti Web hizmetidir 

Web hizmeti bir Azure Kaynak Yöneticisi uç noktasını kullanıma sunar web hizmetinin yeni tür ise, ikinci eklemek gerekmez **varsayılan olmayan** uç noktası. **UpdateResourceEndpoint** bağlantılı hizmetteki biçimi şöyledir: 

```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview. 
```

Değerleri URL'deki yer tutucu için web hizmeti üzerinde sorgulanırken alabileceğiniz [Azure Machine Learning Web Hizmetleri portalı](https://services.azureml.net/). 

Güncelleştirme kaynağı endpoint yeni tür hizmet asıl kimlik doğrulaması gerektirir. Hizmet asıl kimlik doğrulaması kullanmak için Azure Active Directory (Azure AD) bir uygulama varlığı kaydetmek ve onu vermek için **katkıda bulunan** veya **sahibi** rol abonelik veya kaynak grubu yeri web hizmeti ait. Bkz: [hizmet sorumlusu oluşturma ve Azure kaynak yönetmek için izinleri atamak nasıl](../azure-resource-manager/resource-group-create-service-principal-portal.md). Bağlantılı hizmet tanımlamak için kullandığınız aşağıdaki değerleri not edin:

- Uygulama Kimliği
- Uygulama anahtarı 
- Kiracı Kimliği

Örnek bağlantılı hizmet tanımı aşağıda verilmiştir: 

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "description": "The linked service for AML web service.",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/0000000000000000  000000000000000000000/services/0000000000000000000000000000000000000/jobs?api-version=2.0",
            "apiKey": {
            "type": "SecureString",
            "value": "APIKeyOfEndpoint1"
            },
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview. ",
            "servicePrincipalId": "000000000-0000-0000-0000-0000000000000",
            "servicePrincipalKey": {
            "type": "SecureString",
            "value": "servicePrincipalKey"
            },
            "tenant": "mycompany.com"
        }
    }
}
```

Aşağıdaki senaryoyu daha fazla ayrıntı sağlar. Yeniden eğitme ve bir Azure Data Factory işlem hattı Azure ML modellerinden güncelleştirmek için bir örnek vardır.


## <a name="sample-retraining-and-updating-an-azure-machine-learning-model"></a>Örnek: Yeniden eğitme ve bir Azure Machine Learning modeli güncelleştiriliyor

Bu bölümde kullanan örnek bir işlem hattı **Azure ML toplu iş yürütme etkinliği** bir model yeniden eğitme için. Ardışık Düzen de kullanır **Azure ML kaynak güncelleştirme etkinliği** modeli Puanlama web hizmeti güncelleştirmek için. Ayrıca bu bölüm JSON parçacıklarını tüm bağlı hizmetler, veri kümelerini ve ardışık düzen örnekte için sağlar.

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
Aşağıdaki JSON parçacığı Puanlama web hizmeti güncelleştirilebilir uç noktaya işaret eden bir Azure Machine Learning bağlantılı hizmeti tanımlar.  

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

### <a name="pipeline"></a>İşlem hattı
Ardışık düzen iki etkinlik vardır: **AzureMLBatchExecution** ve **AzureMLUpdateResource**. Toplu iş yürütme etkinliği giriş olarak eğitim verileri alır ve iLearner dosyasını bir çıktı olarak üretir. Kaynak güncelleştirme etkinliği sonra bu iLearner dosya alır ve Tahmine dayalı web hizmetini güncelleştirmek için kullanın. 

```JSON
{
    "name": "LookupPipelineDemo",
    "properties": {
        "activities": [
            {
                "name": "amlBEGetilearner",
                "description": "Use AML BES to get the ileaner file from training web service",
                "type": "AzureMLBatchExecution",
                "linkedServiceName": {
                    "referenceName": "trainingEndpoint",
                    "type": "LinkedServiceReference"
                },
                "typeProperties": {
                    "webServiceInputs": {
                        "input1": {
                            "LinkedServiceName":{
                                "referenceName": "StorageLinkedService",
                                "type": "LinkedServiceReference"
                            }, 
                            "FilePath":"azuremltesting/input"
                        }, 
                        "input2": {
                            "LinkedServiceName":{
                                "referenceName": "StorageLinkedService",
                                "type": "LinkedServiceReference" 
                            }, 
                            "FilePath":"azuremltesting/input"
                        }        
                    },
                    "webServiceOutputs": {
                        "output1": {
                            "LinkedServiceName":{
                                "referenceName": "StorageLinkedService",
                                "type": "LinkedServiceReference"   
                            }, 
                            "FilePath":"azuremltesting/output"
                        }        
                    }
                }
            },
            {
                "name": "amlUpdateResource",
                "type": "AzureMLUpdateResource",
                "description": "Use AML Update Resource to update the predict web service",
                "linkedServiceName": {
                    "type": "LinkedServiceReference",
                    "referenceName": "updatableScoringEndpoint2"
                },
                "typeProperties": {
                    "trainedModelName": "ADFV2Sample Model [trained model]",
                    "trainedModelLinkedServiceName": {
                                "type": "LinkedServiceReference",
                                "referenceName": "StorageLinkedService"
                            },
                    "trainedModelFilePath": "azuremltesting/output/newModelForArm.ilearner"
                },
                "dependsOn": [ 
                    { 
                        "activity": "amlbeGetilearner", 
                        "dependencyConditions": [ "Succeeded" ] 
                    }
                 ]

            }
        ]
    }
}
```
## <a name="next-steps"></a>Sonraki adımlar
Diğer yollarla verileri dönüştürmek açıklanmaktadır aşağıdaki makalelere bakın: 

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Hive etkinliği](transform-data-using-hadoop-hive.md)
* [Pig etkinliği](transform-data-using-hadoop-pig.md)
* [MapReduce etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop akış etkinliği](transform-data-using-hadoop-streaming.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
