---
title: Azure SizeSelector UI öğesi | Microsoft Docs
description: Azure portalı için Microsoft.Compute.SizeSelector kullanıcı Arabirimi öğesi açıklar.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2018
ms.author: tomfitz
ms.openlocfilehash: 3966de95233f32a09d4799630632c2bb6a490d78
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
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
    "excludedSizes": []
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

## <a name="sample-output"></a>Örnek çıktı
```json
"Standard_D1"
```

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturmak için bir giriş için bkz [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* Kullanıcı Arabirimi öğeleri ortak özellikleri açıklaması için bkz: [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
