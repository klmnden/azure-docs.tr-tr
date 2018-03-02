---
title: "Azure SKU kullanılamaz hataları | Microsoft Docs"
description: "SKU giderileceğini açıklar dağıtımı sırasında kullanılamaz hatası."
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
ms.openlocfilehash: a6cccfa5097847429d3e402e3d522addc14b8c31
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="resolve-errors-for-sku-not-available"></a>SKU kullanılamaz hatalarını çözümleme

Bu makalede nasıl çözümleneceğini **SkuNotAvailable** hata.

## <a name="symptom"></a>Belirti

Bir kaynak (genellikle bir sanal makine) dağıtırken şu hata kodu ve hata iletisini alıyorsunuz:

```
Code: SkuNotAvailable
Message: The requested tier for resource '<resource>' is currently not available in location '<location>' 
for subscription '<subscriptionID>'. Please try another tier or deploy to a different location.
```

## <a name="cause"></a>Nedeni

(VM boyutu gibi) seçtiniz SKU kaynak seçtiğiniz konum için kullanılabilir olmadığında bu hatayı alırsınız.

## <a name="solution"></a>Çözüm

Bu sorunu çözmek için SKU'ları bir bölgede kullanılabilir olan belirlemeniz gerekir. Kullanılabilir SKU'lar bulmak için PowerShell, portalı veya REST işlemini kullanın.

### <a name="solution-1"></a>Çözüm 1

Kullanım [Get-AzureRmComputeResourceSku](/powershell/module/azurerm.compute/get-azurermcomputeresourcesku) PowerShell'de komutu. Konuma göre sonuçları filtrelenir. Bu komut için PowerShell en son sürümünü olması gerekir.

```powershell
Get-AzureRmComputeResourceSku | where {$_.Locations -icontains "southcentralus"}
```

Sonuçlar SKU'ları listesi için konum ve SKU yönelik kısıtlamaları içerir.

```powershell
ResourceType                Name      Locations Restriction                      Capability Value
------------                ----      --------- -----------                      ---------- -----
availabilitySets         Classic southcentralus             MaximumPlatformFaultDomainCount     3
availabilitySets         Aligned southcentralus             MaximumPlatformFaultDomainCount     3
virtualMachines      Standard_A0 southcentralus
virtualMachines      Standard_A1 southcentralus
virtualMachines      Standard_A2 southcentralus
```

### <a name="solution-2"></a>Çözüm 2

Azure CLI kullanma `az vm list-skus` komutu. Daha sonra kullanabilirsiniz `grep` veya çıkış filtrelemek için benzer bir yardımcı programı.

```bash
$ az vm list-skus --output table
ResourceType      Locations           Name                    Capabilities                       Tier      Size           Restrictions
----------------  ------------------  ----------------------  ---------------------------------  --------  -------------  ---------------------------
availabilitySets  eastus              Classic                 MaximumPlatformFaultDomainCount=3
avilabilitySets   eastus              Aligned                 MaximumPlatformFaultDomainCount=3
availabilitySets  eastus2             Classic                 MaximumPlatformFaultDomainCount=3
availabilitySets  eastus2             Aligned                 MaximumPlatformFaultDomainCount=3
availabilitySets  westus              Classic                 MaximumPlatformFaultDomainCount=3
availabilitySets  westus              Aligned                 MaximumPlatformFaultDomainCount=3
availabilitySets  centralus           Classic                 MaximumPlatformFaultDomainCount=3
availabilitySets  centralus           Aligned                 MaximumPlatformFaultDomainCount=3
```

### <a name="solution-3"></a>Çözüm 3

Kullanım [portal](https://portal.azure.com). Portalında oturum açın ve arabirimi aracılığıyla bir kaynak ekleyin. Değerleri ayarlama gibi bu kaynak için kullanılabilir SKU'lar bakın. Dağıtımın tamamlanması gerekmez.

![kullanılabilir SKU'lar](./media/resource-manager-sku-not-available-errors/view-sku.png)

### <a name="solution-4"></a>Çözüm 4

Sanal makineler için REST API kullanın. Aşağıdaki isteği gönder:

```HTTP 
GET
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/skus?api-version=2016-03-30
```

Kullanılabilir SKU'ları ve bölgeler şu biçimde döndürür:

```json
{
  "value": [
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A0",
      "tier": "Standard",
      "size": "A0",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A1",
      "tier": "Standard",
      "size": "A1",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    ...
  ]
}
```

Bu bölgede uygun SKU bulamıyor veya işletmenizin karşılayan bir alternatif bölge gönderme gereksinimlerini bir [SKU isteği](https://aka.ms/skurestriction) Azure desteği.
