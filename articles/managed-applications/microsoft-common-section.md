---
title: "Azure yönetilen uygulama bölümü UI öğesi | Microsoft Docs"
description: "Azure yönetilen uygulamalar için Microsoft.Common.Section kullanıcı Arabirimi öğesi açıklar"
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
ms.date: 10/12/2017
ms.author: tomfitz
ms.openlocfilehash: 5a460fde88982c53a7ef3a1ec444d50c1c482fc4
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="microsoftcommonsection-ui-element"></a>Microsoft.Common.Section UI öğesi
Bir veya daha fazla öğe bir başlık altında grupları denetim. Bu öğe kullandığınız zaman [yönetilen bir Azure uygulama oluşturmaya](publish-service-catalog-app.md).

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
- `elements`en az bir öğe içermelidir ve dışındaki tüm öğe türleri içerebilir `Microsoft.Common.Section`.
- Bu öğe desteklemiyor `toolTip` özelliği.

## <a name="sample-output"></a>Örnek çıktı
' Ndaki öğelerin çıkış değerlerine erişmek için `elements`, kullanın [basics()](create-uidefinition-functions.md#basics) veya [steps()](create-uidefinition-functions.md#steps) işlevler ve noktalı gösterim:

```json
basics('section1').element1
```

Türündeki öğeler `Microsoft.Common.Section` hiçbir çıktı değerlere sahip.

## <a name="next-steps"></a>Sonraki adımlar
* Yönetilen uygulamaların giriş için bkz: [Azure yönetilen uygulama genel bakış](overview.md).
* UI tanımları oluşturmak için bir giriş için bkz [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* Kullanıcı Arabirimi öğeleri ortak özellikleri açıklaması için bkz: [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
