---
title: "Azure kaynak sağlayıcıları ve kaynak türleri | Microsoft Docs"
description: "Kaynak Yöneticisi, kendi şemaları ve kullanılabilir API sürümleri ve kaynakları barındırabilir bölgeler destek kaynak sağlayıcıları açıklar."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 3c7a6fe4-371a-40da-9ebe-b574f583305b
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: tomfitz
ms.openlocfilehash: 6a9128f45d4199404019cee594842d59c7f1aaf3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="resource-providers-and-types"></a>Kaynak sağlayıcıları ve türleri

Kaynakları dağıtırken sık kaynak sağlayıcıları ve türler hakkında bilgi almak gerekir. Bu makalede, öğrenin:

* Tüm kaynak sağlayıcıları Azure içinde görüntüleme
* Bir kaynak sağlayıcısının kayıt durumunu denetle
* Kayıt kaynak sağlayıcısı
* Bir kaynak sağlayıcısı için Görünüm kaynak türü
* Bir kaynak türü için geçerli konumlar görünümü
* Bir kaynak türü için geçerli API sürümleri görüntüle

Portal, PowerShell veya Azure CLI aracılığıyla bu adımları gerçekleştirebilir.

## <a name="powershell"></a>PowerShell

Tüm kaynak sağlayıcıları Azure ve aboneliğiniz için kayıt durumunu görmek için kullanın:

```powershell
Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
```

Hangi için benzer sonuçlar getirir:

```powershell
ProviderNamespace                RegistrationState
-------------------------------- ------------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Bir kaynak sağlayıcısı kaydediliyor kaynak sağlayıcısı ile çalışmak için aboneliğinizi yapılandırır. Kayıt için her zaman abonelik kapsamıdır. Varsayılan olarak, birçok kaynak sağlayıcısı otomatik olarak kaydedilir. Ancak, bazı kaynak sağlayıcıları elle kaydetmeniz gerekebilir. Kayıt kaynak sağlayıcısı için gerçekleştirmek için izni olmalıdır `/register/action` kaynak sağlayıcısı için işlem. Bu işlem, katkıda bulunan ve sahibi rollerine dahil edilmiştir.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

Hangi için benzer sonuçlar getirir:

```powershell
ProviderNamespace : Microsoft.Batch
RegistrationState : Registering
ResourceTypes     : {batchAccounts, operations, locations, locations/quotas}
Locations         : {West Europe, East US, East US 2, West US...}
```

Aboneliğinizdeki kaynak türleri bu kaynak Sağlayıcısı'ndan hala varsa, bir kaynak Sağlayıcısı kaydı silinemiyor.

Belirli kaynak sağlayıcısı bilgileri görmek için kullanın:

```powershell
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

Hangi için benzer sonuçlar getirir:

```powershell
{ProviderNamespace : Microsoft.Batch
RegistrationState : Registered
ResourceTypes     : {batchAccounts}
Locations         : {West Europe, East US, East US 2, West US...}

...
```

Bir kaynak sağlayıcısı için kaynak türlerini görmek için kullanın:

```powershell
(Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes.ResourceTypeName
```

Hangi döndürür:

```powershell
batchAccounts
operations
locations
locations/quotas
```

API sürümü, bir kaynak sağlayıcısı tarafından yayımlanan REST API işlemleri sürümüne karşılık gelir. Bir kaynak sağlayıcısı yeni özellikleri etkinleştirir gibi REST API yeni bir sürümünü yayımlar. 

Bir kaynak türü için kullanılabilir API sürümü almak için kullanın:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).ApiVersions
```

Hangi döndürür:

```powershell
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Kaynak Yöneticisi tüm bölgelerde desteklenir, ancak dağıttığınız kaynakların tüm bölgelerde desteklenmiyor olabilir. Ayrıca, kaynak destekleyen bazı bölgelerde kullanmasını önlemek aboneliğinizi sınırlamaları olabilir. 

Bir kaynak türü için desteklenen konumlardan almak için kullanın.

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

Hangi döndürür:

```powershell
West Europe
East US
East US 2
West US
...
```

## <a name="azure-cli"></a>Azure CLI
Tüm kaynak sağlayıcıları Azure ve aboneliğiniz için kayıt durumunu görmek için kullanın:

```azurecli
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

Hangi için benzer sonuçlar getirir:

```azurecli
Provider                         Status
-------------------------------- ----------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Bir kaynak sağlayıcısı kaydediliyor kaynak sağlayıcısı ile çalışmak için aboneliğinizi yapılandırır. Kayıt için her zaman abonelik kapsamıdır. Varsayılan olarak, birçok kaynak sağlayıcısı otomatik olarak kaydedilir. Ancak, bazı kaynak sağlayıcıları elle kaydetmeniz gerekebilir. Kayıt kaynak sağlayıcısı için gerçekleştirmek için izni olmalıdır `/register/action` kaynak sağlayıcısı için işlem. Bu işlem, katkıda bulunan ve sahibi rollerine dahil edilmiştir.

```azurecli
az provider register --namespace Microsoft.Batch
```

Devam eden, bu kayıt bir ileti döndürür.

Aboneliğinizdeki kaynak türleri bu kaynak Sağlayıcısı'ndan hala varsa, bir kaynak Sağlayıcısı kaydı silinemiyor.

Belirli kaynak sağlayıcısı bilgileri görmek için kullanın:

```azurecli
az provider show --namespace Microsoft.Batch
```

Hangi için benzer sonuçlar getirir:

```azurecli
{
    "id": "/subscriptions/####-####/providers/Microsoft.Batch",
    "namespace": "Microsoft.Batch",
    "registrationsState": "Registering",
    "resourceTypes:" [
        ...
    ]
}
```

Bir kaynak sağlayıcısı için kaynak türlerini görmek için kullanın:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[*].resourceType" --out table
```

Hangi döndürür:

```azurecli
Result
---------------
batchAccounts
operations
locations
locations/quotas
```

API sürümü, bir kaynak sağlayıcısı tarafından yayımlanan REST API işlemleri sürümüne karşılık gelir. Bir kaynak sağlayıcısı yeni özellikleri etkinleştirir gibi REST API yeni bir sürümünü yayımlar. 

Bir kaynak türü için kullanılabilir API sürümü almak için kullanın:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].apiVersions | [0]" --out table
```

Hangi döndürür:

```azurecli
Result
---------------
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Kaynak Yöneticisi tüm bölgelerde desteklenir, ancak dağıttığınız kaynakların tüm bölgelerde desteklenmiyor olabilir. Ayrıca, kaynak destekleyen bazı bölgelerde kullanmasını önlemek aboneliğinizi sınırlamaları olabilir. 

Bir kaynak türü için desteklenen konumlardan almak için kullanın.

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" --out table
```

Hangi döndürür:

```azurecli
Result
---------------
West Europe
East US
East US 2
West US
...
```

## <a name="portal"></a>Portal

Tüm kaynak sağlayıcıları Azure ve aboneliğiniz için kayıt durumunu görmek için seçin **abonelikleri**.

![Abonelik seç](./media/resource-manager-supported-services/select-subscriptions.png)

Abonelik görüntülemek için seçin.

![Abonelik belirtin](./media/resource-manager-supported-services/subscription.png)

Seçin **kaynak sağlayıcıları** ve kullanılabilir kaynak sağlayıcıları listesini görüntüleyin.

![kaynak sağlayıcıları göster](./media/resource-manager-supported-services/show-resource-providers.png)

Bir kaynak sağlayıcısı kaydediliyor kaynak sağlayıcısı ile çalışmak için aboneliğinizi yapılandırır. Kayıt için her zaman abonelik kapsamıdır. Varsayılan olarak, birçok kaynak sağlayıcısı otomatik olarak kaydedilir. Ancak, bazı kaynak sağlayıcıları elle kaydetmeniz gerekebilir. Kayıt kaynak sağlayıcısı için gerçekleştirmek için izni olmalıdır `/register/action` kaynak sağlayıcısı için işlem. Bu işlem, katkıda bulunan ve sahibi rollerine dahil edilmiştir. Kayıt kaynak sağlayıcısı için seçin **kaydetmek**.

![Kayıt kaynak sağlayıcısı](./media/resource-manager-supported-services/register-provider.png)

Aboneliğinizdeki kaynak türleri bu kaynak Sağlayıcısı'ndan hala varsa, bir kaynak Sağlayıcısı kaydı silinemiyor.

Belirli kaynak sağlayıcısı için bilgileri görmek için seçin **daha fazla hizmet**.

![Daha fazla hizmet seçin](./media/resource-manager-supported-services/more-services.png)

Arama **kaynak Gezgini** ve kullanılabilir seçenekler arasından seçin.

![Kaynak Gezgini seçin](./media/resource-manager-supported-services/select-resource-explorer.png)

**Sağlayıcılar**'ı seçin.

![Sağlayıcı seçin](./media/resource-manager-supported-services/select-providers.png)

Kaynak sağlayıcısı ve görüntülemek istediğiniz kaynak türünü seçin.

![Kaynak türü seçin](./media/resource-manager-supported-services/select-resource-type.png)

Kaynak Yöneticisi tüm bölgelerde desteklenir, ancak dağıttığınız kaynakların tüm bölgelerde desteklenmiyor olabilir. Ayrıca, kaynak destekleyen bazı bölgelerde kullanmasını önlemek aboneliğinizi sınırlamaları olabilir. Kaynak Gezgini kaynak türü için geçerli konumlarını görüntüler.

![Konumları göster](./media/resource-manager-supported-services/show-locations.png)

API sürümü, bir kaynak sağlayıcısı tarafından yayımlanan REST API işlemleri sürümüne karşılık gelir. Bir kaynak sağlayıcısı yeni özellikleri etkinleştirir gibi REST API yeni bir sürümünü yayımlar. Kaynak Gezgini kaynak türü için geçerli API sürümü görüntüler.

![API sürümleri göster](./media/resource-manager-supported-services/show-api-versions.png)

## <a name="next-steps"></a>Sonraki adımlar
* Resource Manager şablonları oluşturma hakkında bilgi edinmek için [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Kaynakları dağıtma hakkında bilgi edinmek için bkz: [Azure Resource Manager şablonu ile bir uygulamayı dağıtmak](resource-group-template-deploy.md).
* Bir kaynak sağlayıcısı için işlemleri görüntülemek için bkz: [Azure REST API](/rest/api/).

