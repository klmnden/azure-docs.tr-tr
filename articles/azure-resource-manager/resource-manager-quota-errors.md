---
title: Azure kotası hataları | Microsoft Docs
description: Kaynak kotası hatalarını çözümlemeyi açıklar.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 03/09/2018
ms.author: tomfitz
ms.openlocfilehash: 7938f2c47e4af8d8804191fbb9e55b379f9554ef
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60390226"
---
# <a name="resolve-errors-for-resource-quotas"></a>Kaynak kotaları hataları çözün

Bu makalede, kaynakları dağıtırken karşılaşabileceğiniz kotasını hataları açıklanır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="symptom"></a>Belirti

Azure, kotaları aşan kaynakları oluşturan bir şablonu dağıtmak, şuna benzer bir dağıtım hatası alırsınız:

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

Kaynak grubu, Abonelikleri, hesapları ve diğer kapsamları kotaları geçerlidir. Örneğin, Aboneliğinize bir bölgedeki çekirdek sayısını sınırlamak için yapılandırılabilir. İzin verilen tutarından daha fazla çekirdeğe sahip sanal makineyi dağıtma girişiminde bulunursanız kotası aşıldı bildiren bir hata alırsınız.
Eksiksiz bir kota için bilgi [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md).

## <a name="troubleshooting"></a>Sorun giderme

### <a name="azure-cli"></a>Azure CLI

Azure CLI için şunu kullanın `az vm list-usage` sanal makine kotaları bulmak için komutu.

```azurecli
az vm list-usage --location "South Central US"
```

Döndürür:

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

### <a name="powershell"></a>PowerShell

PowerShell için şunu kullanın **Get-AzVMUsage** sanal makine kotaları bulmak için komutu.

```powershell
Get-AzVMUsage -Location "South Central US"
```

Döndürür:

```powershell
Name                             Current Value Limit  Unit
----                             ------------- -----  ----
Availability Sets                            0  2000 Count
Total Regional Cores                         0   100 Count
Virtual Machines                             0 10000 Count
```

## <a name="solution"></a>Çözüm

Bir kota artırım talebinde bulunmak portala gidin ve bir destek sorunu oluşturun. Destek sorunu, dağıtmak istediğiniz bölgeyi için kota artış istemek.

> [!NOTE]
> Kaynak grupları için tüm abonelik için tek tek her bölge için kota olduğunu unutmayın. Batı ABD'deki 30 çekirdeği dağıtmanız gerekiyorsa, Batı ABD bölgesinde 30 Resource Manager çekirdekleri yapmasını istemek zorunda. Hangi bölgeler hiçbirinde erişiminiz 30 çekirdeği dağıtmanız gerekiyorsa, tüm bölgelerde 30 Resource Manager çekirdekleri istemeniz gerekir.
>
>

1. **Abonelikler**'i seçin.

   ![Subscriptions](./media/resource-manager-quota-errors/subscriptions.png)

2. Kotasını artırmanız gereken aboneliği seçin.

   ![Abonelik seçme](./media/resource-manager-quota-errors/select-subscription.png)

3. Seçin **kullanım ve kotalar**

   ![Kullanım ve kotalar seçin](./media/resource-manager-quota-errors/select-usage-quotas.png)

4. Sağ üst köşedeki seçin **istek artışı**.

   ![İstek artışı](./media/resource-manager-quota-errors/request-increase.png)

5. Artırmanız gereken kota türünün formlarını doldurun.

   ![Formu doldurun](./media/resource-manager-quota-errors/forms.png)