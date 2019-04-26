---
title: Azure SizeSelector UI öğesi | Microsoft Docs
description: Azure portalına yönelik Microsoft.Compute.SizeSelector UI öğesi açıklar.
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
ms.openlocfilehash: e5be5635964ebeedc7be4d1d1f5403e4d281b55c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60251280"
---
# <a name="microsoftcomputesizeselector-ui-element"></a>Microsoft.Compute.SizeSelector kullanıcı Arabirimi öğesi
Bir veya daha fazla sanal makine örnekleri için bir boyut seçmek için bir denetim.

## <a name="ui-sample"></a>Örnek kullanıcı Arabirimi

Kullanıcı Seçici öğe tanımında varsayılan değerlerle görür.

![Microsoft.Compute.SizeSelector](./media/managed-application-elements/microsoft.compute.sizeselector.png)

Kullanıcı denetimi seçtikten sonra Genişletilmiş görünümde kullanılabilir boyutlar görür.

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
- `recommendedSizes` en az bir boyuta sahip olmalıdır. İlk önerilen boyut, varsayılan olarak kullanılır. Kullanılabilir boyutların listesi önerilen durumuna göre sıralanmamış. Önerilen durumuna göre sıralamak için bu sütun kullanıcı seçebilirsiniz.
- Önerilen boyut Seçilen konumda kullanılabilir değilse, boyutu otomatik olarak atlanır. Bunun yerine, bir sonraki önerilen boyuta kullanılır.
- `constraints.allowedSizes` ve `constraints.excludedSizes` hem de isteğe bağlıdır, ancak aynı anda kullanılamaz. Kullanılabilir boyutların listesi çağırarak belirlenebilir [listesinden bir abonelik için kullanılabilir sanal makine boyutlarını](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region). Belirtilen değil herhangi bir boyutta `constraints.allowedSizes` gizlidir, belirtilen değil tüm boyutlardaki `constraints.excludedSizes` gösterilir.
- `osPlatform` belirtilmiş olmalı ve aşağıdakilerden biri olması **Windows** veya **Linux**. Sanal makinelerin donanım maliyetlerini belirlemek için kullanılır.
- `imageReference` ancak üçüncü taraf görüntüleri için sağlanan birinci taraf görüntülerini atlanır. Sanal makinelerin yazılım maliyetleri belirlemek için kullanılır.
- `count` öğesi için uygun çarpan ayarlamak için kullanılır. Statik bir değer gibi destekler **2**, veya dinamik bir değerin başka bir öğeden `[steps('step1').vmCount]`. Varsayılan değer **1**.
- `numAvailabilityZonesRequired` 1, 2 veya 3 olabilir.
- Varsayılan olarak, `hideDiskTypeFilter` olduğu **false**. Disk türü filtresi tüm disk türleri veya yalnızca SSD görmesine olanak sağlar.

## <a name="sample-output"></a>Örnek çıktı
```json
"Standard_D1"
```

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturma, bir giriş için bkz. [createuidefinition dosyasını kullanmaya başlama](create-uidefinition-overview.md).
* Ortak Özellikler UI öğelerinin açıklaması için bkz. [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
