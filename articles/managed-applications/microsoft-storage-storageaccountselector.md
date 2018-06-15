---
title: Azure StorageAccountSelector UI öğesi | Microsoft Docs
description: Azure portalı için Microsoft.Storage.StorageAccountSelector kullanıcı Arabirimi öğesi açıklar.
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
ms.openlocfilehash: 4a18d629b959e4f1abf4ec2df28a31180efec89f
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34261046"
---
# <a name="microsoftstoragestorageaccountselector-ui-element"></a>Microsoft.Storage.StorageAccountSelector UI öğesi
Yeni veya var olan depolama hesabını seçmek için bir denetim.

## <a name="ui-sample"></a>Kullanıcı Arabirimi örneği
![Microsoft.Storage.StorageAccountSelector](./media/managed-application-elements/microsoft.storage.storageaccountselector.png)

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
- Belirtilmişse, `defaultValue.name` benzersizlik için otomatik olarak doğrulanır. Depolama hesabı adı benzersiz değilse, kullanıcının farklı bir ad belirtin veya mevcut bir depolama hesabını seçin.
- İçin varsayılan değer `defaultValue.type` olan **Premium_LRS**.
- Belirtilen olmayan herhangi bir türü `constraints.allowedTypes` gizlenir ve belirtilen olmayan herhangi bir türü `constraints.excludedTypes` gösterilir.
`constraints.allowedTypes` ve `constraints.excludedTypes` hem isteğe bağlıdır, ancak aynı anda kullanılamaz.
- Varsa `options.hideExisting` olan **doğru**, kullanıcının mevcut bir depolama hesabını seçemezsiniz. Varsayılan değer **false**.


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
* UI tanımları oluşturmak için bir giriş için bkz [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* Kullanıcı Arabirimi öğeleri ortak özellikleri açıklaması için bkz: [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
