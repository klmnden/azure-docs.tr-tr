---
title: "Azure yönetilen uygulama PasswordBox UI öğesi | Microsoft Docs"
description: "Azure yönetilen uygulamalar için Microsoft.Common.PasswordBox kullanıcı Arabirimi öğesi açıklar"
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
ms.openlocfilehash: 6d9f2b7cf56375d3a609cff20e928307c13bf2b8
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="microsoftcommonpasswordbox-ui-element"></a>Microsoft.Common.PasswordBox UI öğesi
Bir parolayı onaylayın ve sağlamak için kullanılan bir denetimi. Bu öğe kullandığınız zaman [yönetilen bir Azure uygulama oluşturmaya](publish-service-catalog-app.md).

## <a name="ui-sample"></a>Kullanıcı Arabirimi örneği
![Microsoft.Common.PasswordBox](./media/managed-application-elements/microsoft.common.passwordbox.png)

## <a name="schema"></a>Şema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.PasswordBox",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "",
    "validationMessage": ""
  },
  "options": {
    "hideConfirmation": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Açıklamalar
- Bu öğe desteklemiyor `defaultValue` özelliği.
- Uygulama ayrıntıları için `constraints`, bkz: [Microsoft.Common.TextBox](microsoft-common-textbox.md).
- Varsa `options.hideConfirmation` ayarlanır **doğru**, kullanıcının parolasını onayladığınız için ikinci metin kutusu gizlenir. Varsayılan değer **false**.

## <a name="sample-output"></a>Örnek çıktı
```json
"p4ssw0rd"
```

## <a name="next-steps"></a>Sonraki adımlar
* Yönetilen uygulamaların giriş için bkz: [Azure yönetilen uygulama genel bakış](overview.md).
* UI tanımları oluşturmak için bir giriş için bkz [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* Kullanıcı Arabirimi öğeleri ortak özellikleri açıklaması için bkz: [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
