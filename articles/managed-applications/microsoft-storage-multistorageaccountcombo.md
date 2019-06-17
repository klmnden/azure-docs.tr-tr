---
title: Azure MultiStorageAccountCombo UI öğesi | Microsoft Docs
description: Azure portalına yönelik Microsoft.Storage.MultiStorageAccountCombo UI öğesi açıklar.
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
ms.openlocfilehash: 08b65770414e9ee1cb5e478427fe7654b2bb9a78
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64725437"
---
# <a name="microsoftstoragemultistorageaccountcombo-ui-element"></a>Microsoft.Storage.MultiStorageAccountCombo kullanıcı Arabirimi öğesi
Ortak bir önek ile başlayan adları ile birkaç depolama hesabı oluşturmak için denetimleri grubudur.

## <a name="ui-sample"></a>Örnek kullanıcı Arabirimi
![Microsoft.Storage.MultiStorageAccountCombo](./media/managed-application-elements/microsoft.storage.multistorageaccountcombo.png)

## <a name="schema"></a>Şema
```json
{
  "name": "element1",
  "type": "Microsoft.Storage.MultiStorageAccountCombo",
  "label": {
    "prefix": "Storage account prefix",
    "type": "Storage account type"
  },
  "toolTip": {
    "prefix": "",
    "type": ""
  },
  "defaultValue": {
    "prefix": "sa",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "count": 2,
  "visible": true
}
```

## <a name="remarks"></a>Açıklamalar
- Değeri `defaultValue.prefix` depolama hesabı adları dizisi oluşturmak için bir veya daha fazla tamsayı ile birleştirilir. Örneğin, varsa `defaultValue.prefix` olduğu **sa** ve `count` olduğu **2**, ardından depolama hesabı adları **sa1** ve **sa2** oluşturulur. Oluşturulan depolama hesabı adları için benzersizlik otomatik olarak doğrulanır.
- Depolama hesabı adları lexicographically temel alınarak oluşturulan `count`. Örneğin, varsa `count` 10'dur ve depolama hesabı adları iki basamaklı tamsayılar (01, 02, 03) ile bitmelidir.
- İçin varsayılan değer `defaultValue.prefix` olduğu **null**ve `defaultValue.type` olduğu **Premium_LRS**.
- Belirtilen değil herhangi bir türü `constraints.allowedTypes` gizli ve belirtilen değil herhangi bir türü `constraints.excludedTypes` gösterilir. `constraints.allowedTypes` ve `constraints.excludedTypes` hem de isteğe bağlıdır, ancak aynı anda kullanılamaz.
- Depolama hesabı adları oluşturma yanı sıra `count` öğesi için uygun çarpan ayarlamak için kullanılır. Statik bir değer gibi destekler **2**, veya dinamik bir değerin başka bir öğeden `[steps('step1').storageAccountCount]`. Varsayılan değer **1**.

## <a name="sample-output"></a>Örnek çıktı

```json
{
  "prefix": "sa",
  "count": 2,
  "resourceGroup": "rg01",
  "type": "Premium_LRS"
}
```

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturma, bir giriş için bkz. [createuidefinition dosyasını kullanmaya başlama](create-uidefinition-overview.md).
* Ortak Özellikler UI öğelerinin açıklaması için bkz. [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
