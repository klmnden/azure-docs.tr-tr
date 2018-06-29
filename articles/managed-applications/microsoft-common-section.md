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
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 90ffae3dd8b05041c34d766e464eb68f793f6066
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37062987"
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
  "label": "Example section",
  "elements": [
    {
      "name": "text1",
      "type": "Microsoft.Common.TextBox",
      "label": "Example text box 1"
    },
    {
      "name": "text2",
      "type": "Microsoft.Common.TextBox",
      "label": "Example text box 2"
    }
  ],
  "visible": true
}
```

## <a name="remarks"></a>Açıklamalar
- `elements` en az bir öğe olmalı ve dışında tüm öğesi türlerine sahip `Microsoft.Common.Section`.
- Bu öğe desteklemiyor `toolTip` özelliği.

## <a name="sample-output"></a>Örnek çıktı
' Ndaki öğelerin çıkış değerlerine erişmek için `elements`, kullanın [basics()](create-uidefinition-functions.md#basics) veya [steps()](create-uidefinition-functions.md#steps) işlevler ve noktalı gösterim:

```json
steps('configuration').section1.text1
```

Türündeki öğeler `Microsoft.Common.Section` hiçbir çıktı değerlere sahip.

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturmak için bir giriş için bkz [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* Kullanıcı Arabirimi öğeleri ortak özellikleri açıklaması için bkz: [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
