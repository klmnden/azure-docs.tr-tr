---
title: Azure bölümde kullanıcı Arabirimi öğesi | Microsoft Docs
description: Azure portalına yönelik Microsoft.Common.Section UI öğesi açıklar.
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
ms.openlocfilehash: c89b45dd4d8e6c2964f3d2bcbb6c3cef445c79e6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64698887"
---
# <a name="microsoftcommonsection-ui-element"></a>Microsoft.Common.Section kullanıcı Arabirimi öğesi
Başlık altında bir veya daha fazla öğe gruplandırır bir denetimdir.

## <a name="ui-sample"></a>Örnek kullanıcı Arabirimi
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
- `elements` en az bir elemana sahip olmalı ve hariç tüm öğe türleri olabilir `Microsoft.Common.Section`.
- Bu öğe desteklemiyor `toolTip` özelliği.

## <a name="sample-output"></a>Örnek çıktı
İçindeki öğe çıkış değerlere erişmek için `elements`, kullanın [basics()](create-uidefinition-functions.md#basics) veya [steps()](create-uidefinition-functions.md#steps) işlevleri ve nokta gösterimi:

```json
steps('configuration').section1.text1
```

Türünde öğeler `Microsoft.Common.Section` hiçbir çıktı değerlere sahip.

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturma, bir giriş için bkz. [createuidefinition dosyasını kullanmaya başlama](create-uidefinition-overview.md).
* Ortak Özellikler UI öğelerinin açıklaması için bkz. [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
