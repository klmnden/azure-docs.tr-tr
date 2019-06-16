---
title: Azure StorageAccountSelector UI öğesi | Microsoft Docs
description: Azure portalına yönelik Microsoft.Storage.StorageAccountSelector UI öğesi açıklar.
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
ms.date: 06/28/2018
ms.author: tomfitz
ms.openlocfilehash: c6d4ef50645902aecd57ceb9fc48b7d99bf22d53
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62104882"
---
# <a name="microsoftstoragestorageaccountselector-ui-element"></a>Microsoft.Storage.StorageAccountSelector kullanıcı Arabirimi öğesi
Yeni veya var olan depolama hesabını seçmek için bir denetim.

## <a name="ui-sample"></a>Örnek kullanıcı Arabirimi

Varsayılan değer denetim gösterir.

![Microsoft.Storage.StorageAccountSelector](./media/managed-application-elements/microsoft.storage.storageaccountselector.png)

Yeni bir depolama hesabı oluşturun veya mevcut bir depolama hesabını seçmek kullanıcı denetimi sağlar.

![Yeni Microsoft.Storage.StorageAccountSelector](./media/managed-application-elements/microsoft.storage.storageaccountselector-new.png)

## <a name="schema"></a>Şema

```json
{
  "name": "element1",
  "type": "Microsoft.Storage.StorageAccountSelector",
  "label": "Storage account",
  "toolTip": "",
  "defaultValue": {
    "name": "storageaccount01",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "options": {
    "hideExisting": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Açıklamalar
- Belirtilmişse `defaultValue.name` benzersizlik için otomatik olarak doğrulanır. Depolama hesabı adı benzersiz değilse, kullanıcı farklı bir ad belirtin veya mevcut bir depolama hesabını seçin.
- İçin varsayılan değer `defaultValue.type` olduğu **Premium_LRS**.
- Belirtilen değil herhangi bir türü `constraints.allowedTypes` gizli ve belirtilen değil herhangi bir türü `constraints.excludedTypes` gösterilir. `constraints.allowedTypes` ve `constraints.excludedTypes` hem de isteğe bağlıdır, ancak aynı anda kullanılamaz.
- Varsa `options.hideExisting` olduğu **true**, kullanıcının mevcut bir depolama hesabını seçemezsiniz. Varsayılan değer **false**.

## <a name="sample-output"></a>Örnek çıktı

```json
{
  "name": "storageaccount01",
  "resourceGroup": "rg01",
  "type": "Premium_LRS",
  "newOrExisting": "new"
}
```

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturma, bir giriş için bkz. [createuidefinition dosyasını kullanmaya başlama](create-uidefinition-overview.md).
* Ortak Özellikler UI öğelerinin açıklaması için bkz. [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
