---
title: Azure Data Factory kullanarak makine öğrenimi modellerini güncelleştirme | Microsoft Docs
description: Nasıl oluşturulacağını açıklar makine öğrenimi ve Azure Data Factory kullanarak öngörülebilir komut zincirleri oluşturma
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/16/2018
ms.author: shlo
ms.openlocfilehash: 8f1320db0af85f6c83a9daf8e17a691336c9b251
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60335491"
---
# <a name="update-azure-machine-learning-models-by-using-update-resource-activity"></a>Kaynak güncelleştirme etkinliği'ni kullanarak Azure Machine Learning modellerini güncelleştirme
Bu makalede, ana Azure Data Factory - Azure Machine Learning tümleştirme makale tamamlar: [Azure Machine Learning ve Azure Data Factory kullanarak öngörülebilir komut zincirleri oluşturma](transform-data-using-machine-learning.md). Zaten yapmadıysanız, bu makalede okumadan önce ana makalesini gözden geçirin.

## <a name="overview"></a>Genel Bakış
Azure Machine Learning modellerini faaliyete işleminin bir parçası, modelinizi eğitilmiş ve kaydedilir. Ardından, bir Tahmine dayalı Web hizmeti oluşturmak için kullanabilirsiniz. Web hizmeti web siteleri, panolar ve mobil uygulamalarda tüketilebilir.

Machine Learning kullanarak oluşturduğunuz modelleri genellikle statik değildir. Yeni veriler kullanılabilir olduğunda ya da kendi veri tüketici API'si varsa, model eğitilebileceği gerekir. Başvurmak [makine öğrenme modeli yeniden eğitme](../machine-learning/machine-learning-retrain-machine-learning-model.md) Azure Machine learning'de bir model nasıl yeniden eğitebilir hakkında ayrıntılar için.

Yeniden eğitme sık gerçekleşebilir. Batch yürütme etkinliği ve kaynak güncelleştirme etkinliği, Azure Machine Learning modeli yeniden eğitme ve Data Factory kullanarak Tahmine dayalı Web hizmeti güncelleştirme kullanıma hazır hale getirebilirsiniz.

Aşağıdaki resimde, eğitim ve Tahmine dayalı Web Hizmetleri arasındaki ilişki gösterilmektedir.

![Web Hizmetleri](./media/update-machine-learning-models/web-services.png)

## <a name="azure-machine-learning-update-resource-activity"></a>Azure Machine Learning kaynak güncelleştirme etkinliği

Aşağıdaki JSON kod parçacığında, bir Azure Machine Learning Batch Execution etkinliği tanımlar.

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
| name                          | İşlem hattındaki etkinliğin adı     | Evet      |
| description                   | Etkinliğin ne yaptığını açıklayan metin.  | Hayır       |
| türü                          | Azure Machine Learning kaynak güncelleştirme etkinliği için etkinlik türdür **AzureMLUpdateResource**. | Evet      |
| linkedServiceName             | Azure Machine Learning bağlı updateResourceEndpoint özelliği içeren hizmeti. | Evet      |
| trainedModelName              | İçinde Web hizmeti denemesinde güncelleştirilecek olan eğitilen Model modülünün adını | Evet      |
| trainedModelLinkedServiceName | Güncelleştirme işlemi tarafından karşıya yüklenen olan ilearner dosyasını barındıran Azure depolama bağlı hizmetin adı | Evet      |
| trainedModelFilePath          | Güncelleştirme işlemi tarafından karşıya yüklenen olan ilearner dosyasını temsil eden trainedModelLinkedService göreli dosya yolu | Evet      |

## <a name="end-to-end-workflow"></a>Uçtan uca iş akışı

Bir model ve güncelleştirme Tahmine dayalı Web hizmetlerini yeniden eğitme faaliyete geçirmeye yönelik tüm işlemi aşağıdaki adımları içerir:

- Çağırma **Web hizmeti eğitim** kullanarak **Batch yürütme etkinliği**. Bir eğitim Web hizmetini çağırırken aynıdır açıklanan Tahmine dayalı bir Web hizmetini çağırmak [Azure Machine Learning ve Data Factory Batch yürütme etkinliği kullanarak öngörülebilir komut zincirleri oluşturma](transform-data-using-machine-learning.md). Eğitim Web hizmeti çıktısı, Tahmine dayalı Web hizmetini güncelleştirmek için kullanabileceğiniz bir iLearner dosyasıdır.
- Çağırma **kaynak uç noktası güncelleştirme** , **Tahmine dayalı Web hizmeti** kullanarak **kaynak güncelleştirme etkinliği** Web hizmeti ile yeni eğitilen modeli güncelleştirmek için.

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning bağlı hizmeti

Çalışmak yukarıda belirtilen uçtan uca iş akışı için iki Azure Machine Learning bağlı hizmeti oluşturmanız gerekir:

1. Azure Machine Learning eğitim web Service'e bağlı hizmeti bağımsız olarak bir, içinde bahsedilen olarak aynı şekilde, Batch yürütme etkinliği tarafından bu bağlı hizmeti kullanılır [Azure Machine Learning ve Data Factory toplu kullanarak öngörülebilir komut zincirleri oluşturma Yürütme etkinliği](transform-data-using-machine-learning.md). Ardından Tahmine dayalı web hizmetini güncelleştirmek için kaynak güncelleştirme etkinliği tarafından kullanılan bir iLearner dosyasını çıktıdır eğitim web hizmetinin farktır.
2. Bir Azure Machine Learning Tahmine dayalı web hizmeti güncelleştirme kaynağı bitiş noktasına bağlı hizmeti. Bu bağlı hizmeti, yukarıdaki adım döndürülen iLearner dosyasını kullanarak Tahmine dayalı web hizmetini güncelleştirmek için kaynak güncelleştirme etkinliği tarafından kullanılır.

İkinci bağlantılı bir Azure Machine Learning hizmeti için yapılandırma, Azure Machine Learning Web hizmetini Klasik Web hizmeti ya da yeni bir Web hizmeti olduğunda farklıdır. Farklılıkları ayrı olarak aşağıdaki bölümlerde ele alınmıştır.

## <a name="web-service-is-new-azure-resource-manager-web-service"></a>Yeni Azure Resource Manager web hizmeti Web hizmetidir

Web hizmeti bir Azure Resource Manager uç noktasını kullanıma sunar ve web hizmeti yeni türü ise, ikinci eklemek gerekmez **varsayılan olmayan** uç noktası. **UpdateResourceEndpoint** bölümünde bağlı hizmetin şu biçimdedir:

```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview
```

Değerler için yer tutucu URL'nin web hizmeti üzerinde sorgulanırken alabileceğiniz [Azure Machine Learning Web Hizmetleri portalını](https://services.azureml.net/).

Yeni güncelleştirme kaynak uç noktası türü, hizmet sorumlusu kimlik doğrulaması gerektirir. Hizmet sorumlusu kimlik doğrulaması kullanmak, Azure Active Directory (Azure AD) uygulama varlığın kaydedin ve bu izni **katkıda bulunan** veya **sahibi** abonelik veya kaynak grubunda yeri web hizmeti aittir. Bkz: [hizmet sorumlusu oluşturma ve Azure kaynak yönetme izinlerini atama](../active-directory/develop/howto-create-service-principal-portal.md). Bağlı hizmetini tanımlamak için kullandığınız şu değerleri not edin:

- Uygulama Kimliği
- Uygulama anahtarı
- Kiracı Kimliği

Bağlı örnek bir hizmet tanımı aşağıda verilmiştir:

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
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview",
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

Aşağıdaki senaryoda, daha fazla ayrıntı sağlar. Yeniden eğitme ve Azure Machine Learning studio modeller bir Azure Data Factory işlem hattından güncelleştirmek için bir örnek var.


## <a name="sample-retraining-and-updating-an-azure-machine-learning-model"></a>Örnek: Yeniden eğitme ve Azure Machine Learning modeli güncelleştirme

Bu bölümde kullanan bir örnek işlem hattı **Azure Machine Learning studio Batch yürütme etkinliği** modeli yeniden eğitme için. İşlem hattı de kullanır **Azure Machine Learning studio kaynak güncelleştirme etkinliği** model Puanlama web hizmeti içinde güncelleştirilecek. Bölüm ayrıca tüm bağlı hizmetler, veri kümeleri ve bu örnekteki işlem hattı JSON parçacıklarını sağlar.

### <a name="azure-blob-storage-linked-service"></a>Azure Blob Depolama bağlı hizmeti:
Azure depolama şu veri tutar:

* Eğitim verileri. Azure Machine Learning studio web hizmeti eğitim için giriş verileri.
* iLearner dosya. Azure Machine Learning studio web hizmeti eğitim çıktısı. Bu dosya, kaynak güncelleştirme etkinliği için bir giriş da olabilir.

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

### <a name="linked-service-for-azure-machine-learning-studio-training-endpoint"></a>Azure Machine Learning studio eğitim uç noktası için bağlı hizmet
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

İçinde **Azure Machine Learning studio**, değerlerini almak için aşağıdakileri **mlEndpoint** ve **apiKey**:

1. Tıklayın **WEB Hizmetleri** sol menüsünde.
2. Tıklayın **web hizmeti eğitim** web hizmetleri listesinde.
3. Yanındaki Kopyala **API anahtarı** metin kutusu. Anahtar, panoya Data Factory JSON düzenleyicisine yapıştırın.
4. İçinde **Azure Machine Learning studio**, tıklayın **toplu iş yürütme** bağlantı.
5. Kopyalama **istek URI** gelen **istek** bölümünde ve Data Factory JSON düzenleyicisine yapıştırın.

### <a name="linked-service-for-azure-machine-learning-studio-updatable-scoring-endpoint"></a>Bağlı hizmeti Azure Machine Learning studio güncelleştirilebilir Puanlama uç noktası için:
Güncelleştirilebilir Puanlama web hizmeti uç noktasına işaret eden bir Azure Machine Learning bağlı hizmetinin aşağıdaki JSON kod parçacığında tanımlar.

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
İşlem hattı iki etkinlik içerir: **AzureMLBatchExecution** ve **AzureMLUpdateResource**. Batch yürütme etkinliği, giriş olarak eğitim verilerini alır ve çıktı olarak bir iLearner dosyası üretir. Kaynak güncelleştirme etkinliği, ardından bu iLearner dosyasını alır ve Tahmine dayalı web hizmetini güncelleştirmek için kullanın.

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
Anlatan farklı yollarla verileri dönüştürmek aşağıdaki makalelere bakın:

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Hive etkinliği](transform-data-using-hadoop-hive.md)
* [Pig etkinliği](transform-data-using-hadoop-pig.md)
* [MapReduce etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop akış etkinliğinde](transform-data-using-hadoop-streaming.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
