---
title: Azure MultiStorageAccountCombo UI öğesi | Microsoft Docs
description: Azure portalı için Microsoft.Storage.MultiStorageAccountCombo kullanıcı Arabirimi öğesi açıklar.
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
ms.openlocfilehash: c395c076a4910e124c1b93ebc61b5e491b2b53ff
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
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
