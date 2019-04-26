---
title: Azure UserNameTextBox UI öğesi | Microsoft Docs
description: Azure portalına yönelik Microsoft.Compute.UserNameTextBox UI öğesi açıklar.
services: managed-applications
documentationcenter: na
author: tfitzmac
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 88ab13329a719ba1e1b8a7b5fba2f7a2d381eca2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60251367"
---
# <a name="microsoftcomputeusernametextbox-ui-element"></a>Microsoft.Compute.UserNameTextBox kullanıcı Arabirimi öğesi
Metin kutusu denetimi ile kullanıcı adlarını Windows ve Linux için yerleşik doğrulama.

## <a name="ui-sample"></a>Örnek kullanıcı Arabirimi
![Microsoft.Compute.UserNameTextBox](./media/managed-application-elements/microsoft.compute.usernametextbox.png)

## <a name="schema"></a>Şema
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.UserNameTextBox",
  "label": "User name",
  "defaultValue": "",
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
  },
  "osPlatform": "Windows",
  "visible": true
}
```

## <a name="remarks"></a>Açıklamalar
- Varsa `constraints.required` ayarlanır **true**, sonra metin kutusuna başarıyla doğrulamak için bir değere sahip olmalıdır. Varsayılan değer **true**.
- `osPlatform` belirtilmiş olmalı ve aşağıdakilerden biri olması **Windows** veya **Linux**.
- `constraints.regex` Normal ifade JavaScript bir desendir. Bu seçenek belirtilmişse, metin kutusunun değerini başarıyla doğrulamak için bir desen eşleşmesi gerekir. Varsayılan değer **null**.
- `constraints.validationMessage` metin kutusunun değeri tarafından belirtilen doğrulama başarısız olduğunda görüntülenecek bir dizedir `constraints.regex`. Belirtilmezse, daha sonra metin kutusunun yerleşik doğrulama iletilerinin kullanılır. Varsayılan değer **null**.
- Bu öğe için belirtilen değere göre yerleşik doğrulama sahip `osPlatform`. Yerleşik doğrulama özel bir normal ifade ile birlikte kullanılabilir. İçin bir değer `constraints.regex` belirtilir, ardından yerleşik ve özel doğrulama tetiklenir.

## <a name="sample-output"></a>Örnek çıktı
```json
"Example name"
```

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturma, bir giriş için bkz. [createuidefinition dosyasını kullanmaya başlama](create-uidefinition-overview.md).
* Ortak Özellikler UI öğelerinin açıklaması için bkz. [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
