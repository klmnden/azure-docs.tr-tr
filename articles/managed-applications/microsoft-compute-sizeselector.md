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
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 9009d29e281ace179ad1dd2021c7cf35e3dc611a
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37084816"
---
# <a name="microsoftcomputesizeselector-ui-element"></a>Microsoft.Compute.SizeSelector UI öğesi
Bir veya daha fazla sanal makine örnekleri için bir boyut seçmek için bir denetim.

## <a name="ui-sample"></a>Kullanıcı Arabirimi örneği

Kullanıcı Seçici öğe tanımında varsayılan değerlerle görür.

![Microsoft.Compute.SizeSelector](./media/managed-application-elements/microsoft.compute.sizeselector.png)

Denetim seçtikten sonra kullanıcı kullanılabilir boyutlara genişletilmiş görünümünü görür.

![Genişletilmiş Microsoft.Compute.SizeSelector](./media/managed-application-elements/microsoft.compute.sizeselector-expanded.png)

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
  "options": {
    "hideDiskTypeFilter": false
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
- `recommendedSizes` en az bir boyut olması gerekir. İlk önerilen boyut, varsayılan olarak kullanılır. Kullanılabilir boyutların listesi önerilen durumuna göre sıralı değil. Kullanıcı önerilen durumuna göre sıralamak için bu sütun seçebilirsiniz.
- Önerilen boyut Seçilen konumda kullanılabilir değilse, boyutu otomatik olarak atlandı. Bunun yerine, önerilen bir sonraki boyuta kullanılır.
- `constraints.allowedSizes` ve `constraints.excludedSizes` hem isteğe bağlıdır, ancak aynı anda kullanılamaz. Kullanılabilir boyutların listesi çağırarak belirlenebilir [listesinden bir abonelik için kullanılabilir sanal makine boyutlarını](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region). Belirtilen olmayan herhangi bir boyutta `constraints.allowedSizes` gizlenir ve belirtilen olmayan herhangi bir boyutta `constraints.excludedSizes` gösterilir.
- `osPlatform` belirtilmeli ve birini kullanabilir **Windows** veya **Linux**. Sanal makinelerin donanım maliyetlerini belirlemek için kullanılır.
- `imageReference` üçüncü taraf görüntüleri için sağlanan ancak birinci taraf görüntüleri için atlandı. Sanal makinelerin yazılım maliyetleri belirlemek için kullanılır.
- `count` öğesi için uygun çarpanı ayarlamak için kullanılır. Statik bir değer gibi destekleyen **2**, veya gibi başka bir öğeden dinamik bir değer `[steps('step1').vmCount]`. Varsayılan değer **1**.
- `numAvailabilityZonesRequired` 1, 2 veya 3 olabilir.
- Varsayılan olarak, `hideDiskTypeFilter` olan **false**. Disk türü filtresi tüm disk türlerini ya da yalnızca SSD görmek kullanıcının sağlar.

## <a name="sample-output"></a>Örnek çıktı
```json
"Standard_D1"
```

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturmak için bir giriş için bkz [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* Kullanıcı Arabirimi öğeleri ortak özellikleri açıklaması için bkz: [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
