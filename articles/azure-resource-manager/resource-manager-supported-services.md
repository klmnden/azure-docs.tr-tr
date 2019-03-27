---
title: Azure kaynak sağlayıcıları ve kaynak türleri | Microsoft Docs
description: Resource Manager, kendi şemaları ve kullanılabilir API sürümleri ve kaynakları barındıran bölgeleri destekleyen kaynak sağlayıcıları açıklar.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: 3c7a6fe4-371a-40da-9ebe-b574f583305b
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: tomfitz
ms.openlocfilehash: 520aeb8e47b5e94e6346e682f21f46cb0814f8f3
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58445452"
---
# <a name="azure-resource-providers-and-types"></a>Azure kaynak sağlayıcıları ve türleri

Kaynakları dağıtılırken sık kaynak sağlayıcıları ve türleri hakkında bilgi almak gerekir. Bu makalede şunları öğreneceksiniz:

* Azure'da tüm kaynak sağlayıcılarını görüntüleme
* Bir kaynak sağlayıcısının kayıt durumunu denetleme
* Bir kaynak sağlayıcısını kaydetme
* Görünüm kaynak türleri için bir kaynak sağlayıcısı
* Bir kaynak türü için geçerli konumları
* Bir kaynak türü için geçerli API sürümleri görüntüle

Bu adımları Azure portalı, Azure PowerShell veya Azure CLI aracılığıyla yapabilirsiniz.

## <a name="azure-portal"></a>Azure portal

Tüm kaynak sağlayıcıları ve aboneliğiniz için kayıt durumunu görmek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Tüm Hizmetler**’i seçin.

    ![Abonelikleri seçin](./media/resource-manager-supported-services/select-subscriptions.png)
3. İçinde **tüm hizmetleri** kutusuna **abonelik**ve ardından **abonelikleri**.
4. Aboneliği görüntülemek için abonelik listesinden seçin.
5. Seçin **kaynak sağlayıcıları** ve kullanılabilir kaynak sağlayıcıları listesini görüntüleyin.

    ![Kaynak sağlayıcıları göster](./media/resource-manager-supported-services/show-resource-providers.png)

6. Bir kaynak sağlayıcısı kaydediliyor, aboneliğinizin kaynak sağlayıcısı ile çalışacak şekilde yapılandırır. Kayıt için kapsam her zaman aboneliktir. Varsayılan olarak, birçok kaynak sağlayıcısı otomatik olarak kaydedilir. Ancak, bazı kaynak sağlayıcıları elle kaydetmeniz gerekebilir. Bir kaynak sağlayıcısını kaydetmek için izniniz olmalıdır `/register/action` işlem kaynak sağlayıcısı. Bu işlem, Katkıda Bulunan ve Sahip rolleriyle birlikte sunulur. Bir kaynak sağlayıcısını kaydetmek için seçin **kaydetme**. Önceki ekran görüntüsünde **kaydetme** bağlantısını için vurgulanır **Microsoft.Blueprint**.

    Aboneliğinizde kaynak türleri, kaynak sağlayıcısından hala varsa, bir kaynak Sağlayıcısı kaydı silinemiyor.

Belirli kaynak sağlayıcısı bilgileri görmek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Tüm Hizmetler**’i seçin.

    ![Tüm hizmetleri seçin](./media/resource-manager-supported-services/more-services.png)

3. İçinde **tüm hizmetleri** kutusuna **kaynak Gezgini**ve ardından **kaynak Gezgini**.
4. Genişletin **sağlayıcıları** sağ oku seçerek.

    ![Sağlayıcılarını seçin](./media/resource-manager-supported-services/select-providers.png)

5. Bir kaynak sağlayıcısı ve görüntülemek istediğiniz kaynak türünü genişletin.

    ![Kaynak türü seçin](./media/resource-manager-supported-services/select-resource-type.png)

6. Kaynak Yöneticisi, tüm bölgelerde desteklenir, ancak dağıttığınız kaynakları tüm bölgelerde desteklenmiyor olabilir. Buna ek olarak, aboneliğinizdeki kaynak destekleyen bazı bölgeleri kullanmasını önlemek sınırlamaları olabilir. Kaynak Gezgini, kaynak türü için geçerli konumları görüntüler.

    ![Konumları göster](./media/resource-manager-supported-services/show-locations.png)

7. API sürümü, bir kaynak sağlayıcısı tarafından sunulan REST API işlemleri sürümüne karşılık gelir. Bir kaynak sağlayıcısı yeni özellikleri etkinleştirir gibi yeni bir REST API sürümü serbest bırakır. Kaynak Gezgini, kaynak türü için geçerli API sürümlerini görüntüler.

    ![API sürümleri göster](./media/resource-manager-supported-services/show-api-versions.png)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure aboneliğiniz için kayıt durumu, tüm kaynak sağlayıcılarını görmek için bu seçeneği kullanın:

```azurepowershell-interactive
Get-AzResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
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

Bir kaynak sağlayıcısı kaydediliyor, aboneliğinizin kaynak sağlayıcısı ile çalışacak şekilde yapılandırır. Kayıt için kapsam her zaman aboneliktir. Varsayılan olarak, birçok kaynak sağlayıcısı otomatik olarak kaydedilir. Ancak, bazı kaynak sağlayıcıları elle kaydetmeniz gerekebilir. Bir kaynak sağlayıcısını kaydetmek için izniniz olmalıdır `/register/action` işlem kaynak sağlayıcısı. Bu işlem, Katkıda Bulunan ve Sahip rolleriyle birlikte sunulur.

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
```

Hangi için benzer sonuçlar getirir:

```powershell
ProviderNamespace : Microsoft.Batch
RegistrationState : Registering
ResourceTypes     : {batchAccounts, operations, locations, locations/quotas}
Locations         : {West Europe, East US, East US 2, West US...}
```

Aboneliğinizde kaynak türleri, kaynak sağlayıcısından hala varsa, bir kaynak Sağlayıcısı kaydı silinemiyor.

Belirli kaynak sağlayıcısı bilgileri görmek için bu seçeneği kullanın:

```azurepowershell-interactive
Get-AzResourceProvider -ProviderNamespace Microsoft.Batch
```

Hangi için benzer sonuçlar getirir:

```powershell
{ProviderNamespace : Microsoft.Batch
RegistrationState : Registered
ResourceTypes     : {batchAccounts}
Locations         : {West Europe, East US, East US 2, West US...}

...
```

Bir kaynak sağlayıcısı için kaynak türlerini görmek için bu seçeneği kullanın:

```azurepowershell-interactive
(Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes.ResourceTypeName
```

Döndürür:

```powershell
batchAccounts
operations
locations
locations/quotas
```

API sürümü, bir kaynak sağlayıcısı tarafından sunulan REST API işlemleri sürümüne karşılık gelir. Bir kaynak sağlayıcısı yeni özellikleri etkinleştirir gibi yeni bir REST API sürümü serbest bırakır.

Bir kaynak türü için kullanılabilir API sürümlerini almak için kullanın:

```azurepowershell-interactive
((Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).ApiVersions
```

Döndürür:

```powershell
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Kaynak Yöneticisi, tüm bölgelerde desteklenir, ancak dağıttığınız kaynakları tüm bölgelerde desteklenmiyor olabilir. Buna ek olarak, aboneliğinizdeki kaynak destekleyen bazı bölgeleri kullanmasını önlemek sınırlamaları olabilir.

Bir kaynak türü için desteklenen konumlar almak için kullanın.

```azurepowershell-interactive
((Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

Döndürür:

```powershell
West Europe
East US
East US 2
West US
...
```

## <a name="azure-cli"></a>Azure CLI

Azure aboneliğiniz için kayıt durumu, tüm kaynak sağlayıcılarını görmek için bu seçeneği kullanın:

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

Bir kaynak sağlayıcısı kaydediliyor, aboneliğinizin kaynak sağlayıcısı ile çalışacak şekilde yapılandırır. Kayıt için kapsam her zaman aboneliktir. Varsayılan olarak, birçok kaynak sağlayıcısı otomatik olarak kaydedilir. Ancak, bazı kaynak sağlayıcıları elle kaydetmeniz gerekebilir. Bir kaynak sağlayıcısını kaydetmek için izniniz olmalıdır `/register/action` işlem kaynak sağlayıcısı. Bu işlem, Katkıda Bulunan ve Sahip rolleriyle birlikte sunulur.

```azurecli
az provider register --namespace Microsoft.Batch
```

Bu kayıt döndüren bir ileti, devam eden.

Aboneliğinizde kaynak türleri, kaynak sağlayıcısından hala varsa, bir kaynak Sağlayıcısı kaydı silinemiyor.

Belirli kaynak sağlayıcısı bilgileri görmek için bu seçeneği kullanın:

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

Bir kaynak sağlayıcısı için kaynak türlerini görmek için bu seçeneği kullanın:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[*].resourceType" --out table
```

Döndürür:

```azurecli
Result
---------------
batchAccounts
operations
locations
locations/quotas
```

API sürümü, bir kaynak sağlayıcısı tarafından sunulan REST API işlemleri sürümüne karşılık gelir. Bir kaynak sağlayıcısı yeni özellikleri etkinleştirir gibi yeni bir REST API sürümü serbest bırakır.

Bir kaynak türü için kullanılabilir API sürümlerini almak için kullanın:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].apiVersions | [0]" --out table
```

Döndürür:

```azurecli
Result
---------------
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Kaynak Yöneticisi, tüm bölgelerde desteklenir, ancak dağıttığınız kaynakları tüm bölgelerde desteklenmiyor olabilir. Buna ek olarak, aboneliğinizdeki kaynak destekleyen bazı bölgeleri kullanmasını önlemek sınırlamaları olabilir.

Bir kaynak türü için desteklenen konumlar almak için kullanın.

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" --out table
```

Döndürür:

```azurecli
Result
---------------
West Europe
East US
East US 2
West US
...
```

## <a name="next-steps"></a>Sonraki adımlar

* Resource Manager şablonları oluşturma hakkında bilgi edinmek için [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md). 
* Kaynak sağlayıcısı Şablon Şemaları görüntülemek için bkz: [şablon başvurusu](/azure/templates/).
* Kaynakları dağıtma hakkında bilgi edinmek için [Azure Resource Manager şablonu ile uygulama dağıtma](resource-group-template-deploy.md).
* Bir kaynak sağlayıcısı işlemleri görüntülemek için bkz: [Azure REST API'si](/rest/api/).
