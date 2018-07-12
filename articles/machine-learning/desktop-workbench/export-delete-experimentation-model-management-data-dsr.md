---
title: Dışarı aktarma veya deneme silmek ya da yönetim verileri - Azure Machine Learning modeli | Microsoft Docs
description: Azure Machine Learning'de dışarı aktarma veya deneme veya model yönetimi ile Azure portal, CLI, SDK ve kimliği doğrulanmış REST API'leri için ilgili hesabı verilerinizi silin. Bu makalede, nasıl gösterir.
services: machine-learning
author: cjgronlund
ms.author: cgronlun
manager: haining
ms.reviewer: jmartens, mldocs
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.date: 05/22/2018
ms.openlocfilehash: 5475ce3be24321b15ab78a078b758c25843f0ed3
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38724025"
---
# <a name="export-or-delete-your-experimentation-or-model-management-data-in-machine-learning"></a>Dışarı aktarma veya deneme veya Machine Learning model yönetimi verilerini sil

Azure Machine Learning'de dışarı aktarma veya kimliği doğrulanmış REST API'si ile deneme veya model yönetimi ile ilgili hesap verilerinizi silin. Bu makalede nasıl yapılacağı açıklanmaktadır.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="control-your-account-data"></a>Hesap verilerinizi denetleyin
Azure Machine Learning deneme ve model Yönetimi tarafından depolanan ürün içi verileri dışarı aktarma ve silme işlemi Azure portal, CLI, SDK ve kimliği doğrulanmış REST API'leri aracılığıyla kullanılabilir. Telemetri verilerini Azure gizlilik portal üzerinden erişilebilir. 

Azure Machine Learning'de kişisel verileri çalıştırma geçmişi belgelerde ve telemetri kayıt hizmeti ile bazı kullanıcı etkileşimi sonucu oluşan kullanıcı bilgilerden oluşur.

## <a name="delete-account-data-with-the-rest-api"></a>REST API'si ile hesap verilerini sil 

Şu API çağrıları, verileri silmek için DELETE HTTP fiili ile yapılabilir. Bunlar sağlayarak yetkili bir `Authorization: Bearer <arm-token>` istek üstbilgisinde burada `<arm-token>` uç noktası için AAD erişim belirteci `https://management.core.windows.net/` uç noktası.  

Bu belirteci alma ve Azure uç noktalarına çağrı hakkında bilgi edinmek için [Azure REST API belgelerini](https://docs.microsoft.com/rest/api/azure/).  

Aşağıdaki örneklerde, metni değiştirin {} ile ilişkili kaynak belirlemek örneği adları.

## <a name="delete-from-a-hosting-account"></a>Bir barındırma hesaptan Sil

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningModelManagement/accounts/{account-name}?api-version=2017-09-01-preview      

## <a name="delete-from-the-model-management-service"></a>Model yönetim hizmetinden Sil

### <a name="model-document"></a>Model Belgesi
Bu çağrı, modelleri ve kimliklerinin bir listesini almak için kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models?api-version=2017-09-01-preview"

Tek tek modelleri ile silinebilir:  

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models/{modelId}?api-version=2017-09-01-preview

### <a name="manifest-document"></a>Belge listesi
Tüm bildirimler ve kimliklerinin bir listesini almak için bu çağrıyı kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests?api-version=2017-09-01-preview

Tek tek bildirimleri ile silinebilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests/{manifestId}?api-version=2017-09-01-preview

### <a name="service-documents"></a>Hizmeti belgeleri
Tüm hizmetler ve kimliklerinin bir listesini almak için bu çağrıyı kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services?api-version=2017-09-01-preview

Tek tek Hizmetleri ile silinebilir:    

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{serviceName}?api-version=2017-09-01-preview

### <a name="image-document"></a>Resim belgesi
Tüm görüntüleri ve kimliklerinin bir listesini almak için bu çağrıyı kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images?api-version=2017-09-01-preview

Birlikte ayrı görüntüleri silinebilir:  

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images/{imageId}?api-version=2017-09-01-preview

## <a name="delete-run-history-artifact-and-notification-data"></a>Çalıştırma geçmişi, yapı ve bildirim verilerini sil
Bir proje için çalıştırma geçmişi, yapı ve bildirim depoları tüm ilgili proje belge sildikten sonra silinir:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}/workspaces/{workspace-name}/projects/{project-name}?api-version=2017-05-01-preview
    
## <a name="delete-from-experimentation-account-resource-provider"></a>Deneme hesabı kaynak sağlayıcısından gelen Sil
Proje belgelerini ile silinir:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}/workspaces/{workspace-name}/projects/{project-name}?api-version=2017-05-01-preview

Çalışma alanı belgeleri ile silinir:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}/workspaces/{workspace-name}?api-version=2017-05-01-preview

İle tüm deneme hesabı silindi:
    
    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}?api-version=2017-05-01-preview


## <a name="export-service-data-with-the-rest-api"></a>REST API sahip hizmet verileri dışarı aktarma
Verileri dışarı aktarma için şu API çağrıları HTTP GET fiili ile yapılabilir. Bunlar sağlayarak yetkili bir `Authorization: Bearer <arm-token>` istek üstbilgisinde burada `<arm-token>` uç noktası için AAD erişim belirteci. `https://management.core.windows.net/`  

Bu belirteci alma ve Azure uç noktalarına çağrı hakkında bilgi edinmek için [Azure REST API belgelerini](https://docs.microsoft.com/rest/api/azure/).   

Aşağıdaki örneklerde, metni değiştirin {} ile ilişkili kaynak belirlemek örneği adları.

## <a name="export-hosting-account-data"></a>Barındırma hesabı verileri dışarı aktarma

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningModelManagement/accounts/{accountName}?api-version=2017-09-01-preview     

## <a name="export-model-management-service-data"></a>Model Yönetimi hizmeti verileri dışarı aktarma
### <a name="model-document"></a>Model Belgesi

Bu çağrı, modelleri ve kimliklerinin bir listesini almak için kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models?api-version=2017-09-01-preview"

Tek tek modelleri tarafından alınabilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models/{modelId}?api-version=2017-09-01-preview 

### <a name="manifests"></a>Bildirimler
Tüm bildirimler ve kimliklerinin bir listesini almak için bu çağrıyı kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests?api-version=2017-09-01-preview

Tek tek bildirimleri tarafından alınabilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests/{manifestId}?api-version=2017-09-01-preview

### <a name="services"></a>Hizmetler
Tüm hizmetler ve kimliklerinin bir listesini almak için bu çağrıyı kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services?api-version=2017-09-01-preview

Tek tek Hizmetleri tarafından alınabilir: 

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{serviceName}?api-version=2017-09-01-preview

### <a name="images"></a>Görüntüler
Tüm görüntüleri ve kimliklerinin bir listesini almak için bu çağrıyı kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images?api-version=2017-09-01-preview

Tek tek Hizmetleri tarafından alınabilir: 

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images/{imageId}?api-version=2017-09-01-preview     

## <a name="export-compute-data"></a>İşlem verileri dışarı aktarma
### <a name="compute-clusters"></a>Hesaplama kümeleri
Tüm işlem kümeleri ve adlarının listesini almak için bu çağrıyı kullanın:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes?api-version=2018-03-01-preview

Tek tek kümeleri tarafından alınabilir:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes/{compute-name}?api-version=2018-03-01-preview

### <a name="operationalization-clusters"></a>Kullanıma hazır hale getirme kümeleri
Tüm kümeler ve adlarının listesini almak için bu çağrıyı kullanın:

    https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.MachineLearningCompute/operationalizationClusters?api-version=2017-06-01-preview

Tek tek kümeleri tarafından alınabilir:

    https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.MachineLearningCompute/operationalizationClusters/{clusterName}?api-version=2017-06-01-preview

## <a name="export-run-history-data"></a>Çalıştırma geçmişini dışarı aktarma
Tüm çalıştırmalar ve kimliklerinin bir listesini almak için bu çağrıyı kullanın:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs

Tüm denemeler ve kimliklerinin bir listesini almak için bu çağrıyı kullanın:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/experiments

Çalıştırma geçmişi öğeleri tarafından elde edilebilir:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}

Öğeleri ile alınan ölçümleri çalıştırın:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runmetrics

Çalıştırma denemeleri tarafından alınabilir:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/experiments/{experimentId}    

Geçmiş yapıtları çalıştırın:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}/artifacts

Geçmiş yapıtları URI'ler çalıştırın:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}/artifacturi?name={artifactName}

## <a name="export-artifacts"></a>Yapıtları dışarı aktarma
Varlıklar ve adlarının listesini almak için bu çağrıyı kullanın:

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/assets

Yapıtlar ve yollarının listesini almak için bu çağrıyı kullanın:

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/artifacts/origins/{origin}/containers/{runId}
        
        
### <a name="artifact-contents"></a>Yapıt içeriği

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/artifacts/contentinfo/ExperimentRun/{runId}/{artifactPath}

### <a name="artifact-documents"></a>Yapıt belgeleri

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}/artifacts
        
### <a name="assets"></a>Varlıklar

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/assets/name/{name}

## <a name="export-notifications"></a>Dışarı aktarma bildirimleri

1. Git [kullanıcılar bölümü Azure Portalı'nın](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UsersManagementMenuBlade/)ve ardından kullanıcıdan **adı** sütun. 
2. Not **nesne kimliği**ve aşağıdaki çağrıyı kullanın:     

        https://{location}.experiments.azureml.net/notification/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/users/{objectId}/jobs

## <a name="export-experimentation-account-information"></a>Deneme hesabı bilgileri verme
### <a name="experimentation-account-information"></a>Deneme hesabı bilgileri

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}?api-version=2017-05-01-preview
        
### <a name="workspace-information"></a>Çalışma alanı bilgileri

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}?api-version=2017-05-01-preview  

### <a name="project-information"></a>Proje bilgileri

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}?api-version=2017-05-01-preview
