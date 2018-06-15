---
title: Azure UserNameTextBox UI öğesi | Microsoft Docs
description: Azure portalı için Microsoft.Compute.UserNameTextBox kullanıcı Arabirimi öğesi açıklar.
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
ms.date: 10/12/2017
ms.author: tomfitz
ms.openlocfilehash: b65b62389fbb66c9461430d1dd2df42e71bf6cff
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34260731"
---
# <a name="microsoftcomputeusernametextbox-ui-element"></a>Microsoft.Compute.UserNameTextBox UI öğesi
Bir metin kutusu denetiminin Windows ve Linux kullanıcı adları için yerleşik doğrulama.

## <a name="ui-sample"></a>Kullanıcı Arabirimi örneği
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
- Varsa `constraints.required` ayarlanır **doğru**, metin kutusunun başarıyla doğrulamak için bir değer içermesi gerekir. Varsayılan değer **doğru**.
- `osPlatform` belirtilmeli ve birini kullanabilir **Windows** veya **Linux**.
- `constraints.regex` JavaScript normal ifade deseni olur. Belirtilmişse, metin kutusunun değerini başarıyla doğrulamak için desen eşleşmesi gerekir. Varsayılan değer **null**.
- `constraints.validationMessage` metin kutusunun değeri tarafından belirtilen doğrulama başarısız olduğunda görüntülemek için bir dizedir `constraints.regex`. Belirtilmezse, metin kutusunun yerleşik doğrulama iletileri kullanılır. Varsayılan değer **null**.
- Bu öğe için belirtilen değeri temel alan yerleşik doğrulama sahip `osPlatform`. Yerleşik doğrulama bir özel normal ifade ile birlikte kullanılabilir.
İçin bir değer `constraints.regex` belirtildiğinden, yerleşik ve özel doğrulama tetiklenen sonra.

## <a name="sample-output"></a>Örnek çıktı
```json
"tabrezm"
```

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturmak için bir giriş için bkz [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* Kullanıcı Arabirimi öğeleri ortak özellikleri açıklaması için bkz: [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
