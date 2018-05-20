---
title: Azure SizeSelector UI öğesi | Microsoft Docs
description: Azure portalı için Microsoft.Compute.SizeSelector kullanıcı Arabirimi öğesi açıklar.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/30/2018
ms.author: tomfitz
ms.openlocfilehash: d1b4974c78a5cdb7b4eb885797319b283be2d393
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="microsoftcomputesizeselector-ui-element"></a>Microsoft.Compute.SizeSelector UI öğesi
Bir veya daha fazla sanal makine örnekleri için bir boyut seçmek için bir denetim.

## <a name="ui-sample"></a>Kullanıcı Arabirimi örneği
![Microsoft.Compute.SizeSelector](./media/managed-application-elements/microsoft.compute.sizeselector.png)

## <a name="schema"></a>Şema
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.SizeSelector",
  "label": "Size",
  "toolTip": "",
  "recommendedSizes": [
    "Standard_D1",
    "Standard_D2",
    "Standard_D3"
  ],
  "constraints": {
    "allowedSizes": [],
    "excludedSizes": [],
    "numAvailabilityZonesRequired": 3,
    "zone": "3"
  },
  "osPlatform": "Windows",
  "imageReference": {
    "publisher": "MicrosoftWindowsServer",
    "offer": "WindowsServer",
    "sku": "2012-R2-Datacenter"
  },
  "count": 2,
  "visible": true
}
```

## <a name="remarks"></a>Açıklamalar
- `recommendedSizes` en az bir boyut içermelidir. İlk önerilen boyut, varsayılan olarak kullanılır.
- Önerilen boyut Seçilen konumda kullanılabilir durumda değilse, boyutu otomatik olarak atlandı. Bunun yerine, önerilen bir sonraki boyuta kullanılır.
- Belirtilen olmayan herhangi bir boyutta `constraints.allowedSizes` gizlenir ve belirtilen olmayan herhangi bir boyutta `constraints.excludedSizes` gösterilir.
`constraints.allowedSizes` ve `constraints.excludedSizes` hem isteğe bağlıdır, ancak aynı anda kullanılamaz. Kullanılabilir boyutların listesi çağırarak belirlenebilir [listesinden bir abonelik için kullanılabilir sanal makine boyutlarını](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region).
- `osPlatform` belirtilmeli ve birini kullanabilir **Windows** veya **Linux**. Sanal makinelerin donanım maliyetlerini belirlemek için kullanılır.
- `imageReference` üçüncü taraf görüntüleri için sağlanan ancak birinci taraf görüntüleri için atlandı. Sanal makinelerin yazılım maliyetleri belirlemek için kullanılır.
- `count` öğesi için uygun çarpanı ayarlamak için kullanılır. Statik bir değer gibi destekleyen **2**, veya gibi başka bir öğeden dinamik bir değer `[steps('step1').vmCount]`. Varsayılan değer **1**.
- `numAvailabilityZonesRequired` 1, 2 veya 3 olabilir.

## <a name="sample-output"></a>Örnek çıktı
```json
"Standard_D1"
```

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturmak için bir giriş için bkz [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* Kullanıcı Arabirimi öğeleri ortak özellikleri açıklaması için bkz: [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
