---
title: Azure metin kutusu kullanıcı Arabirimi öğesi | Microsoft Docs
description: Azure portalı için Microsoft.Common.TextBox kullanıcı Arabirimi öğesi açıklar.
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
ms.openlocfilehash: f11098f4af2e85df16e00ce6dac6e7c4b5b34e2b
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="microsoftcommontextbox-ui-element"></a>Microsoft.Common.TextBox UI öğesi
Biçimlendirilmemiş metin düzenlemek için kullanılan bir denetimi.

## <a name="ui-sample"></a>Kullanıcı Arabirimi örneği
![Microsoft.Common.TextBox](./media/managed-application-elements/microsoft.common.textbox.png)

## <a name="schema"></a>Şema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Some text box",
  "defaultValue": "my value",
  "toolTip": "Use only allowed characters",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
  },
  "visible": true
}
```

## <a name="remarks"></a>Açıklamalar
- Varsa `constraints.required` ayarlanır **doğru**, metin kutusunun başarıyla doğrulamak için bir değer içermesi gerekir. Varsayılan değer **false**.
- `constraints.regex` JavaScript normal ifade deseni olur. Belirtilmişse, metin kutusunun değerini başarıyla doğrulamak için desen eşleşmesi gerekir. Varsayılan değer **null**.
- `constraints.validationMessage` metin kutusunun değerini doğrulanamadığında görüntülenecek bir dizedir. Belirtilmezse, metin kutusunun yerleşik doğrulama iletileri kullanılır. Varsayılan değer **null**.
- İçin bir değer belirtin mümkündür `constraints.regex` zaman `constraints.required` ayarlanır **false**. Bu senaryoda, bir değer başarıyla doğrulamak metin kutusu için gerekli değildir. Belirtilmişse, normal ifade deseni eşleşmesi gerekir.

## <a name="sample-output"></a>Örnek çıktı

```json
"my value"
```

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturmak için bir giriş için bkz [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* Kullanıcı Arabirimi öğeleri ortak özellikleri açıklaması için bkz: [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
