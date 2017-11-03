---
title: "Azure kota hataları | Microsoft Docs"
description: "Kaynak qouta hatalarını çözümlemeyi açıklar."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 09/13/2017
ms.author: tomfitz
ms.openlocfilehash: 6a8024a12c4a79e92e37df0a56b6e6bd0cb6a8d9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-errors-for-resource-quotas"></a>Kaynak kotaları hatalarını çözümleme

Bu makalede kaynakları dağıtırken karşılaşabileceğiniz kotasına hataları açıklanmaktadır.

## <a name="symptom"></a>Belirti

Azure kotaları aşan kaynakları oluşturan bir şablonu dağıtmak, benzer bir dağıtım hatası alırsınız:

```
Code=OperationNotAllowed
Message=Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.
```

Ya da görebilirsiniz:

```
Code=ResourceQuotaExceeded
Message=Creating the resource of type <resource-type> would exceed the quota of <number>
resources of type <resource-type> per resource group. The current resource count is <number>,
please delete some resources of this type before creating a new one.
```

## <a name="cause"></a>Nedeni

Kotalar, kaynak grubu, abonelikler, hesapları ve diğer kapsamları uygulanır. Örneğin, aboneliğinizi bir bölge için çekirdek sayısını sınırlamak için yapılandırılabilir. İzin verilen miktardan daha fazla çekirdeğe sahip bir sanal makineyi dağıtma girişiminde, kota aşıldı bildiren bir hata alırsınız.
Tam kota bilgi için bkz: [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../azure-subscription-service-limits.md).

## <a name="solution"></a>Çözüm

### <a name="solution-1"></a>Çözüm 1

Azure CLI için şunu kullanın `az vm list-usage` sanal makine kotaları bulmak için komutu.

```azurecli
az vm list-usage --location "South Central US"
```

Hangi döndürür:

```azurecli
[
  {
    "currentValue": 0,
    "limit": 2000,
    "name": {
      "localizedValue": "Availability Sets",
      "value": "availabilitySets"
    }
  },
  ...
]
```

### <a name="solution-2"></a>Çözüm 2

İçin PowerShell kullanın **Get-AzureRmVMUsage** sanal makine kotaları bulmak için komutu.

```powershell
Get-AzureRmVMUsage -Location "South Central US"
```

Hangi döndürür:

```powershell
Name                             Current Value Limit  Unit
----                             ------------- -----  ----
Availability Sets                            0  2000 Count
Total Regional Cores                         0   100 Count
Virtual Machines                             0 10000 Count
```

### <a name="solution-3"></a>Çözüm 3

Kota sınırını artırın gerekiyorsa, Portalı'na gidin ve dağıtmak istediğiniz bölgeyi için kotayı artırmak için bir destek sorununu dosya.

> [!NOTE]
> Kaynak grupları için tüm abonelik için tek tek her bölge için kota olduğunu unutmayın. Batı ABD 30 çekirdeğini dağıtımı yapmanız gerekirse, Batı ABD 30 Resource Manager çekirdekleri yapmasını istemek zorunda. Hangi bölgeleri hiçbirinde erişiminiz 30 çekirdek dağıtımı yapmanız gerekirse, tüm bölgelerde 30 Resource Manager çekirdekleri istemeniz gerekir.
>
>