---
title: Dışarı aktarma veya çalışma alanı verilerini sil
titleSuffix: Azure Machine Learning service
description: Azure portal, CLI, SDK ve kimliği doğrulanmış REST API'leri ile çalışma alanınızı silme veya dışarı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: ph-com
ms.author: pahusban
ms.date: 09/24/2018
ms.custom: seodec18
ms.openlocfilehash: 3f40606d5fae3b3784ac7f1fdcf4977b7fd9eb1f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60819424"
---
# <a name="export-or-delete-your-machine-learning-service-workspace-data"></a>Dışarı aktarma veya, Machine Learning hizmeti çalışma alanı verilerini sil 

Azure Machine Learning'de dışarı aktarma veya kimliği doğrulanmış REST API ile çalışma alanı verilerini sil. Bu makalede nasıl yapılacağı açıklanmaktadır.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="control-your-workspace-data"></a>Çalışma alanı sizin denetiminizdedir
Azure Machine Learning Hizmetleri tarafından depolanan ürün içi verileri dışarı aktarma ve silme işlemi Azure portalı, CLI, SDK'sı aracılığıyla kullanılabilir ve kimliği doğrulanmış REST API'leri. Telemetri verilerini Azure gizlilik portal üzerinden erişilebilir. 

Azure Machine Learning hizmetleri çalıştırma geçmişi belgelerde ve telemetri kayıt hizmeti ile bazı kullanıcı etkileşimi sonucu oluşan kullanıcı bilgilerini kişisel verileri oluşur.

## <a name="delete-workspace-data-with-the-rest-api"></a>REST API'si ile çalışma alanı verilerini sil 

Şu API çağrıları, verileri silmek için DELETE HTTP fiili ile yapılabilir. Bunlar sağlayarak yetkili bir `Authorization: Bearer <arm-token>` istek üstbilgisinde burada `<arm-token>` için AAD erişim belirteci `https://management.core.windows.net/` uç noktası.  

Bu belirteci alma ve Azure uç noktalarına çağrı hakkında bilgi edinmek için [Azure REST API belgelerini](https://docs.microsoft.com/rest/api/azure/).  

Aşağıdaki örneklerde, metni değiştirin {} ile ilişkili kaynak belirlemek örneği adları.

### <a name="delete-an-entire-workspace"></a>Tüm bir çalışma alanını silme

Bu çağrı, çalışma alanının tamamına silmek için kullanın.  
> [!WARNING]
> Tüm bilgiler silinir ve bu çalışma alanında kullanılabilir.

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}?api-version=2018-03-01-preview

### <a name="delete-models"></a>Modeli Sil

Bu çağrı, modelleri ve kimliklerinin bir listesini almak için kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models?api-version=2018-03-01-preview

Tek tek modelleri ile silinebilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models/{id}?api-version=2018-03-01-preview

### <a name="delete-assets"></a>Varlıkları silme

Varlıklar ve kimliklerinin bir listesini almak için bu çağrıyı kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets?api-version=2018-03-01-preview

Tek tek varlıklar ile silinebilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets/{id}?api-version=2018-03-01-preview

### <a name="delete-images"></a>Görüntüleri Sil

Görüntüleri ve kimliklerinin bir listesini almak için bu çağrıyı kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images?api-version=2018-03-01-preview

Birlikte ayrı görüntüleri silinebilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images/{id}?api-version=2018-03-01-preview

### <a name="delete-services"></a>Hizmetleri Sil

Bu çağrı, hizmetleri ve kimliklerinin bir listesini almak için kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services?api-version=2018-03-01-preview

Tek tek Hizmetleri ile silinebilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services/{id}?api-version=2018-03-01-preview

## <a name="export-service-data-with-the-rest-api"></a>REST API sahip hizmet verileri dışarı aktarma

Verileri dışarı aktarma için şu API çağrıları HTTP GET fiili ile yapılabilir. Bunlar sağlayarak yetkili bir `Authorization: Bearer <arm-token>` istek üstbilgisinde burada `<arm-token>` uç noktası için AAD erişim belirteci. `https://management.core.windows.net/`  

Bu belirteci alma ve Azure uç noktalarına çağrı hakkında bilgi edinmek için [Azure REST API belgelerini](https://docs.microsoft.com/rest/api/azure/).   

Aşağıdaki örneklerde, metni değiştirin {} ile ilişkili kaynak belirlemek örneği adları.

### <a name="export-workspace-information"></a>Çalışma alanı bilgileri verme

Tüm çalışma alanlarının bir listesini almak için bu çağrıyı kullanın:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces?api-version=2018-03-01-preview

Tek bir çalışma alanı hakkında bilgi tarafından alınabilir:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}?api-version=2018-03-01-preview

### <a name="export-compute-information"></a>İşlem bilgileri verme

Tüm işlem, bir çalışma alanına bağlı hedefler tarafından elde edilebilir:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroup/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes?api-version=2018-03-01-preview

Tek işlem hedefi hakkında bilgi tarafından alınabilir:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroup/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes/{computeName}?api-version=2018-03-01-preview

### <a name="export-run-history-data"></a>Çalıştırma geçmişini dışarı aktarma
Tüm denemeler ve kendi bilgi listesini almak için bu çağrıyı kullanın:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments 

Belirli bir deneme için tüm çalıştırmalar tarafından alınabilir:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/runs 

Çalıştırma geçmişi öğeleri tarafından elde edilebilir:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/runs/{runId} 

Bir deneme için tüm çalışma ölçümleri tarafından alınabilir:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/metrics 

Tek bir çalışma ölçüm tarafından alınabilir:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/metrics/{metricId}
    
### <a name="export-artifacts"></a>Yapıtları dışarı aktarma

Yapıtlar ve yollarının listesini almak için bu çağrıyı kullanın:

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/artifacts/origins/ExperimentRun/containers/{runId}
    
### <a name="export-notifications"></a>Dışarı aktarma bildirimleri

Saklı görevlerinin listesini almak için bu çağrıyı kullanın:     

    https://{location}.experiments.azureml.net/notification/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/tasks

Bildirimler için tek bir görev tarafından alınabilir:

    https://{location}.experiments.azureml.net/notification/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}tasks/{taskId}

### <a name="export-data-stores"></a>Dışarı aktarma veri depoları

Veri depolarının bir listesini almak için bu çağrıyı kullanın:

    https://{location}.experiments.azureml.net/datastore/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/datastores

Ayrı veri depoları tarafından alınabilir:

    https://{location}.experiments.azureml.net/datastore/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/datastores/{name}

### <a name="export-models"></a>Dışarı aktarma modelleri

Bu çağrı, modelleri ve kimliklerinin bir listesini almak için kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models?api-version=2018-03-01-preview

Tek tek modelleri tarafından alınabilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models/{id}?api-version=2018-03-01-preview

### <a name="export-assets"></a>Dışarı aktarma varlıklar

Varlıklar ve kimliklerinin bir listesini almak için bu çağrıyı kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets?api-version=2018-03-01-preview

Tek tek varlıklar tarafından alınabilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets/{id}?api-version=2018-03-01-preview

### <a name="export-images"></a>Görüntü dışarı aktarma

Görüntüleri ve kimliklerinin bir listesini almak için bu çağrıyı kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images?api-version=2018-03-01-preview

Ayrı görüntüleri tarafından alınabilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images/{id}?api-version=2018-03-01-preview

### <a name="export-services"></a>Dışarı aktarma hizmetleri

Bu çağrı, hizmetleri ve kimliklerinin bir listesini almak için kullanın:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services?api-version=2018-03-01-preview

Tek tek Hizmetleri tarafından alınabilir:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services/{id}?api-version=2018-03-01-preview

### <a name="export-pipeline-experiments"></a>İşlem hattı denemeleri dışarı aktarma

Tek tek denemeleri tarafından alınabilir:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Experiments/{experimentId}

### <a name="export-pipeline-graphs"></a>İşlem hattı grafikleri dışarı aktarma

Tek tek grafikleri tarafından alınabilir:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Graphs/{graphId}

### <a name="export-pipeline-modules"></a>Ardışık Düzen modüllerine dışarı aktarma

Modüller tarafından alınabilir:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Modules/{id}

### <a name="export-pipeline-templates"></a>İşlem hattı şablonlarını dışarı aktarma

Şablonları tarafından alınabilir:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Templates/{templateId}

### <a name="export-pipeline-data-sources"></a>İşlem hattı veri kaynakları Dışarı Aktar

Veri kaynakları tarafından alınabilir:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/DataSources/{id}
