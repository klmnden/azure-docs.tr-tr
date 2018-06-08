---
title: Dışarı aktarma veya deneme silmek ya da Yönetim verilerini - Azure Machine Learning modeli | Microsoft Docs
description: Azure Machine Learning ile verin veya deneme veya model yönetim Azure portalı, CLI, SDK ve kimliği doğrulanmış REST API'leri ile ilişkili hesap verilerinizi silin. Bu makale size nasıl gösterir.
services: machine-learning
author: cjgronlund
ms.author: cgronlun
manager: haining
ms.reviewer: jmartens, mldocs
ms.service: machine-learning
ms.component: desktop-workbench
ms.topic: conceptual
ms.date: 05/22/2018
ms.openlocfilehash: 7db37865c99908e0fd44be3ec04a8493d190e941
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34833521"
---
# <a name="export-or-delete-your-experimentation-or-model-management-data-in-machine-learning"></a>Dışarı aktarma veya deneme veya Machine Learning modeli yönetim verileri silme

Azure Machine Learning ile verin veya kimliği doğrulanmış REST API deneme veya model yönetimiyle ilgili hesap verilerinizi silin. Bu makalede bunun nasıl yapılacağı açıklanmaktadır.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="control-your-account-data"></a>Hesap veri denetimi
Azure Machine Learning deney ve model Yönetimi tarafından depolanan ürün verileri dışarı aktarma ve Azure portal, CLI, SDK ve kimliği doğrulanmış REST API'leri aracılığıyla silme işlemi için kullanılabilir. Telemetri verileri Azure gizlilik Portalı aracılığıyla erişilebilir. 

Azure Machine Learning ile kişisel veriler çalıştırma geçmişi belgelerde ve bazı kullanıcı etkileşimleri hizmetiyle telemetri kayıtlarının kullanıcı bilgisinin oluşur.

## <a name="delete-account-data-with-the-rest-api"></a>REST API ile hesap verilerini sil 

Verileri silmek için aşağıdaki API çağrıları HTTP DELETE fiil ile yapılabilir. Bunlar sağlayarak yetkili bir `Authorization: Bearer <arm-token>` istek üstbilgisinde nerede `<arm-token>` uç noktası için AAD erişim belirteci `https://management.core.windows.net/` uç noktası.  

Bu belirteci alma ve Azure uç noktaları çağrı öğrenmek için bkz: [Azure REST API belgeleri](https://docs.microsoft.com/rest/api/azure/).  

Aşağıdaki örneklerde, metni değiştirin {} ile ilişkili kaynak belirlemek örnek adları.

## <a name="delete-from-a-hosting-account"></a>Bir barındırma hesabından Sil

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningModelManagement/accounts/{account-name}?api-version=2017-09-01-preview      

## <a name="delete-from-the-model-management-service"></a>Model yönetim hizmetinden Sil

### <a name="model-document"></a>Model Belgesi
Bu çağrı modelleri ve kimliklerinin listesini almak için kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models?api-version=2017-09-01-preview"

Tek tek modelleri ile silinebilir:  

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models/{modelId}?api-version=2017-09-01-preview

### <a name="manifest-document"></a>Bildirim belgesi
Bu çağrı, tüm bildirimler ve kendi kimlikleri listesini almak için kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests?api-version=2017-09-01-preview

Tek tek bildirimleri ile silinebilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests/{manifestId}?api-version=2017-09-01-preview

### <a name="service-documents"></a>Hizmet belgeleri
Bu çağrı, tüm hizmetleri ve kimliklerinin listesini almak için kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services?api-version=2017-09-01-preview

Tek tek Hizmetleri ile silinebilir:    

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{serviceName}?api-version=2017-09-01-preview

### <a name="image-document"></a>Resim belgesi
Bu çağrı, tüm görüntüleri ve kimliklerinin listesini almak için kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images?api-version=2017-09-01-preview

Ayrı ayrı görüntülere sahip silinebilir:  

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images/{imageId}?api-version=2017-09-01-preview

## <a name="delete-run-history-artifact-and-notification-data"></a>Çalıştırma geçmişi, yapı ve bildirim verileri silme
Bir proje için çalıştırma geçmişi, yapı ve bildirim depoları tüm ilgili proje belge sildikten sonra silinir:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}/workspaces/{workspace-name}/projects/{project-name}?api-version=2017-05-01-preview
    
## <a name="delete-from-experimentation-account-resource-provider"></a>Deneme hesabı kaynak Sağlayıcısı'ndan Sil
Proje belgeleri ile silinir:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}/workspaces/{workspace-name}/projects/{project-name}?api-version=2017-05-01-preview

Çalışma alanı belgelerini ile silinir:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}/workspaces/{workspace-name}?api-version=2017-05-01-preview

İle tüm deneme hesap silindi:
    
    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}?api-version=2017-05-01-preview


## <a name="export-service-data-with-the-rest-api"></a>REST API sahip hizmet verileri dışarı aktarma
Verileri dışarı aktarmak için aşağıdaki API çağrıları HTTP GET fiil ile yapılabilir. Bunlar sağlayarak yetkili bir `Authorization: Bearer <arm-token>` istek üstbilgisinde nerede `<arm-token>` uç noktası için AAD erişim belirteci `https://management.core.windows.net/`  

Bu belirteci alma ve Azure uç noktaları çağrı öğrenmek için bkz: [Azure REST API belgeleri](https://docs.microsoft.com/rest/api/azure/).   

Aşağıdaki örneklerde, metni değiştirin {} ile ilişkili kaynak belirlemek örnek adları.

## <a name="export-hosting-account-data"></a>Barındırma hesabı verileri dışarı aktarma

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningModelManagement/accounts/{accountName}?api-version=2017-09-01-preview     

## <a name="export-model-management-service-data"></a>Model yönetim hizmeti verileri dışarı aktarma
### <a name="model-document"></a>Model Belgesi

Bu çağrı modelleri ve kimliklerinin listesini almak için kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models?api-version=2017-09-01-preview"

Tek tek modellerin tarafından alınabilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models/{modelId}?api-version=2017-09-01-preview 

### <a name="manifests"></a>Bildirimler
Bu çağrı, tüm bildirimler ve kendi kimlikleri listesini almak için kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests?api-version=2017-09-01-preview

Tek tek bildirimleri tarafından alınabilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests/{manifestId}?api-version=2017-09-01-preview

### <a name="services"></a>Hizmetler
Bu çağrı, tüm hizmetleri ve kimliklerinin listesini almak için kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services?api-version=2017-09-01-preview

Tek tek Hizmetleri tarafından elde edilebilir: 

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{serviceName}?api-version=2017-09-01-preview

### <a name="images"></a>Görüntüler
Bu çağrı, tüm görüntüleri ve kimliklerinin listesini almak için kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images?api-version=2017-09-01-preview

Tek tek Hizmetleri tarafından elde edilebilir: 

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images/{imageId}?api-version=2017-09-01-preview     

## <a name="export-compute-data"></a>İşlem verilerini dışa aktarma
### <a name="compute-clusters"></a>Hesaplama kümeleri
Bu çağrı, tüm işlem kümeleri ve adlarının listesini almak için kullanın:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes?api-version=2018-03-01-preview

Tek tek kümeleri tarafından alınabilir:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes/{compute-name}?api-version=2018-03-01-preview

### <a name="operationalization-clusters"></a>Operationalization kümeleri
Bu çağrı, tüm kümeleri ve adlarının bir listesini almak için kullanın:

    https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.MachineLearningCompute/operationalizationClusters?api-version=2017-06-01-preview

Tek tek kümeleri tarafından alınabilir:

    https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.MachineLearningCompute/operationalizationClusters/{clusterName}?api-version=2017-06-01-preview

## <a name="export-run-history-data"></a>Verme geçmiş verileri çalıştırma
Bu çağrı tüm çalıştırır ve kimliklerinin listesini almak için kullanın:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs

Bu çağrı, tüm denemeler ve kimliklerinin listesini almak için kullanın:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/experiments

Öğeleri tarafından elde edilebilir geçmişi çalıştırın:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}

Öğeleri tarafından elde edilebilir ölçümleri çalıştırın:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runmetrics

Çalışma denemeler tarafından alınabilir:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/experiments/{experimentId}    

Geçmiş yapılar çalıştırın:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}/artifacts

Geçmiş yapılar URI'ler çalıştırın:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}/artifacturi?name={artifactName}

## <a name="export-artifacts"></a>Dışarı aktarma yapıları
Varlıklar ve adlarının listesini almak için bu çağrıyı kullanın:

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/assets

Bu çağrı yapıları ve onların yollarını listesini almak için kullanın:

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/artifacts/origins/{origin}/containers/{runId}
        
        
### <a name="artifact-contents"></a>Yapı içeriği

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/artifacts/contentinfo/ExperimentRun/{runId}/{artifactPath}

### <a name="artifact-documents"></a>Yapı belgeleri

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}/artifacts
        
### <a name="assets"></a>Varlıklar

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/assets/name/{name}

## <a name="export-notifications"></a>Dışarı aktarma bildirimleri

1. Git [Azure portalının kullanıcılar bölümü](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UsersManagementMenuBlade/)ve ardından kullanıcıdan **adı** sütun. 
2. Not **nesne kimliği**ve aşağıdaki çağrısında kullanın:     

        https://{location}.experiments.azureml.net/notification/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/users/{objectId}/jobs

## <a name="export-experimentation-account-information"></a>Deneme hesabı bilgilerini verme
### <a name="experimentation-account-information"></a>Deneme hesabı bilgileri

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}?api-version=2017-05-01-preview
        
### <a name="workspace-information"></a>Çalışma alanı bilgileri

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}?api-version=2017-05-01-preview  

### <a name="project-information"></a>Proje bilgileri

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}?api-version=2017-05-01-preview
