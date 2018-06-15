---
title: Azure MultiStorageAccountCombo UI öğesi | Microsoft Docs
description: Azure portalı için Microsoft.Storage.MultiStorageAccountCombo kullanıcı Arabirimi öğesi açıklar.
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
ms.openlocfilehash: a4ec5a97f8655c0b5b53dea129d4648a05f6ef85
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34261165"
---
# <a name="microsoftstoragemultistorageaccountcombo-ui-element"></a>Microsoft.Storage.MultiStorageAccountCombo UI öğesi
Ortak bir önek ile başlayan adlarla, birden çok depolama hesabı oluşturmak için denetimleri grubudur.

## <a name="ui-sample"></a>Kullanıcı Arabirimi örneği
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
- Değeri `defaultValue.prefix` depolama hesabı adları dizisini oluşturmak için bir veya daha fazla tamsayılar ile birleştirilir. Örneğin, varsa `defaultValue.prefix` olan **foobar** ve `count` olan **2**, ardından depolama hesabı adları **foobar1** ve **foobar2** oluşturulur. Oluşturulan depolama hesabı adları için benzersizlik otomatik olarak doğrulanır.
- Depolama hesabı adları lexicographically temel alınarak oluşturulan `count`. Örneğin, varsa `count` 10'dur ve sonra depolama hesabı adları ile 2 basamaklı tamsayılar bitiş (01, 02, 03, vs.).
- İçin varsayılan değer `defaultValue.prefix` olan **null**ve `defaultValue.type` olan **Premium_LRS**.
- Belirtilen olmayan herhangi bir türü `constraints.allowedTypes` gizlenir ve belirtilen olmayan herhangi bir türü `constraints.excludedTypes` gösterilir.
`constraints.allowedTypes` ve `constraints.excludedTypes` hem isteğe bağlıdır, ancak aynı anda kullanılamaz.
- Depolama hesabı adları oluşturuluyor yanı sıra `count` öğesi için uygun çarpanı ayarlamak için kullanılır. Statik bir değer gibi destekleyen **2**, veya gibi başka bir öğeden dinamik bir değer `[steps('step1').storageAccountCount]`. Varsayılan değer **1**.

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
* UI tanımları oluşturmak için bir giriş için bkz [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* Kullanıcı Arabirimi öğeleri ortak özellikleri açıklaması için bkz: [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
