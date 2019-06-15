---
title: Azure açılan kullanıcı Arabirimi öğesi | Microsoft Docs
description: Azure portalına yönelik Microsoft.Common.DropDown UI öğesi açıklar.
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
ms.openlocfilehash: e78fa419b067c0bad48229dcfd8d4e986fc16903
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62117310"
---
# <a name="microsoftcommondropdown-ui-element"></a>Microsoft.Common.DropDown kullanıcı Arabirimi öğesi
Seçim denetimi bir açılan liste.

## <a name="ui-sample"></a>Örnek kullanıcı Arabirimi
![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft.common.dropdown.png)

## <a name="schema"></a>Şema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.DropDown",
  "label": "Example drop down",
  "defaultValue": "Value two",
  "toolTip": "",
  "constraints": {
    "allowedValues": [
      {
        "label": "Value one",
        "value": "one"
      },
      {
        "label": "Value two",
        "value": "two"
      }
    ],
    "required": true
  },
  "visible": true
}
```

## <a name="remarks"></a>Açıklamalar

- Etiketi `constraints.allowedValues` görüntüleme metni için bir öğe ve değeri seçildiğinde, öğenin çıktı değeri.
- Varsayılan değer belirtilmişse, mevcut bir etiketi olmalıdır `constraints.allowedValues`. Belirtilmezse, listedeki ilk öğe `constraints.allowedValues` seçilir. Varsayılan değer **null**.
- `constraints.allowedValues` en az bir öğe olmalıdır.
- Gerekli bir değer benzetmek için bir etiket ve değerini sahip bir öğe eklemek `""` (boş dize) için `constraints.allowedValues`.

## <a name="sample-output"></a>Örnek çıktı
```json
"two"
```

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturma, bir giriş için bkz. [createuidefinition dosyasını kullanmaya başlama](create-uidefinition-overview.md).
* Ortak Özellikler UI öğelerinin açıklaması için bkz. [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
