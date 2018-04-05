---
title: Azure bölüm UI öğesi | Microsoft Docs
description: Azure portalı için Microsoft.Common.Section kullanıcı Arabirimi öğesi açıklar.
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
ms.openlocfilehash: 46ea2e3d404ac3ec9b7f909257451991dbb55f53
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="microsoftcommonsection-ui-element"></a>Microsoft.Common.Section UI öğesi
Bir veya daha fazla öğe bir başlık altında grupları denetim.

## <a name="ui-sample"></a>Kullanıcı Arabirimi örneği
![Microsoft.Common.Section](./media/managed-application-elements/microsoft.common.section.png)

## <a name="schema"></a>Şema
```json
{
  "name": "section1",
  "type": "Microsoft.Common.Section",
  "label": "Some section",
  "elements": [
    {
      "name": "element1",
      "type": "Microsoft.Common.TextBox",
      "label": "Some text box 1"
    },
    {
      "name": "element2",
      "type": "Microsoft.Common.TextBox",
      "label": "Some text box 2"
    }
  ],
  "visible": true
}
```

## <a name="remarks"></a>Açıklamalar
- `elements` en az bir öğe içermelidir ve dışındaki tüm öğe türleri içerebilir `Microsoft.Common.Section`.
- Bu öğe desteklemiyor `toolTip` özelliği.

## <a name="sample-output"></a>Örnek çıktı
' Ndaki öğelerin çıkış değerlerine erişmek için `elements`, kullanın [basics()](create-uidefinition-functions.md#basics) veya [steps()](create-uidefinition-functions.md#steps) işlevler ve noktalı gösterim:

```json
basics('section1').element1
```

Türündeki öğeler `Microsoft.Common.Section` hiçbir çıktı değerlere sahip.

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturmak için bir giriş için bkz [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* Kullanıcı Arabirimi öğeleri ortak özellikleri açıklaması için bkz: [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
