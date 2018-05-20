---
title: Azure bölüm UI öğesi | Microsoft Docs
description: Azure portalı için Microsoft.Common.Section kullanıcı Arabirimi öğesi açıklar.
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
ms.date: 03/30/2018
ms.author: tomfitz
ms.openlocfilehash: e6d7d5d7b205d275c72e96df527a354b072a9dd3
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
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
