---
title: Azure metin kutusu kullanıcı Arabirimi öğesi | Microsoft Docs
description: Azure portalına yönelik Microsoft.Common.TextBox UI öğesi açıklar.
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
ms.openlocfilehash: b06e8b49efe8b6de720fa9bb819d4720e4f80fb6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61044563"
---
# <a name="microsoftcommontextbox-ui-element"></a>Microsoft.Common.TextBox kullanıcı Arabirimi öğesi
Biçimlendirilmemiş metinleri düzenlemek için kullanılan bir denetimdir.

## <a name="ui-sample"></a>Örnek kullanıcı Arabirimi
![Microsoft.Common.TextBox](./media/managed-application-elements/microsoft.common.textbox.png)

## <a name="schema"></a>Şema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Example text box 1",
  "defaultValue": "my text value",
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
- Varsa `constraints.required` ayarlanır **true**, sonra metin kutusuna başarıyla doğrulamak için bir değere sahip olmalıdır. Varsayılan değer **false**.
- `constraints.regex` Normal ifade JavaScript bir desendir. Bu seçenek belirtilmişse, metin kutusunun değerini başarıyla doğrulamak için bir desen eşleşmesi gerekir. Varsayılan değer **null**.
- `constraints.validationMessage` metin kutusunun değerini doğrulanamadığında görüntülenecek bir dizedir. Belirtilmezse, daha sonra metin kutusunun yerleşik doğrulama iletilerinin kullanılır. Varsayılan değer **null**.
- İçin bir değer belirtmek mümkündür `constraints.regex` olduğunda `constraints.required` ayarlanır **false**. Bu senaryoda, bir değer metin kutusu başarıyla doğrulamak gerekli değildir. Belirtilmişse, bu normal ifade deseni eşleşmelidir.

## <a name="sample-output"></a>Örnek çıktı

```json
"my text value"
```

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturma, bir giriş için bkz. [createuidefinition dosyasını kullanmaya başlama](create-uidefinition-overview.md).
* Ortak Özellikler UI öğelerinin açıklaması için bkz. [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
