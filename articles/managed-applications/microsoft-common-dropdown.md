---
title: "Azure yönetilen uygulama açılır UI öğesi | Microsoft Docs"
description: "Azure yönetilen uygulamalar için Microsoft.Common.DropDown kullanıcı Arabirimi öğesi açıklar"
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
ms.openlocfilehash: a3cbb7363c04043eb253e91e058753bc271e4e01
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="microsoftcommondropdown-ui-element"></a>Microsoft.Common.DropDown UI öğesi
Seçim denetim bir açılır liste. Bu öğe kullandığınız zaman [yönetilen bir Azure uygulama oluşturmaya](publish-service-catalog-app.md).

## <a name="ui-sample"></a>Kullanıcı Arabirimi örneği
![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft.common.dropdown.png)

## <a name="schema"></a>Şema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.DropDown",
  "label": "Some drop down",
  "defaultValue": "Foo",
  "toolTip": "",
  "constraints": {
    "allowedValues": [
      {
        "label": "Foo",
        "value": "Bar"
      },
      {
        "label": "Baz",
        "value": "Qux"
      }
    ]
  },
  "visible": true
}
```

## <a name="remarks"></a>Açıklamalar
- Etiketi `constraints.allowedValues` öğenin görünen metindir ve değerini öğesi seçildiğinde çıktı değeri.
- Belirtilmişse, varsayılan değer mevcut bir etiketi olmalıdır `constraints.allowedValues`. Belirtilmezse, listedeki ilk öğe `constraints.allowedValues` seçilir. Varsayılan değer **null**.
- `constraints.allowedValues`en az bir öğe içermesi gerekir.
- Bu öğe desteklemiyor `constraints.required` özelliği. Bu davranış benzetmek için bir etiket ve değerine sahip bir öğe eklemek `""` (boş dize) için `constraints.allowedValues`.

## <a name="sample-output"></a>Örnek çıktı
```json
"Bar"
```

## <a name="next-steps"></a>Sonraki adımlar
* Yönetilen uygulamaların giriş için bkz: [Azure yönetilen uygulama genel bakış](overview.md).
* UI tanımları oluşturmak için bir giriş için bkz [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* Kullanıcı Arabirimi öğeleri ortak özellikleri açıklaması için bkz: [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
