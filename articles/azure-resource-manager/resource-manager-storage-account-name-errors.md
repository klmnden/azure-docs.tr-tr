---
title: Azure depolama hesabı adı hataları | Microsoft Docs
description: Bir depolama hesabı adı belirtilirken karşılaşabileceğiniz hatalar açıklanmaktadır.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 03/09/2018
ms.author: tomfitz
ms.openlocfilehash: c3d4d764b1076c8705cfa64d6c0b38e3b8c1a801
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60390107"
---
# <a name="resolve-errors-for-storage-account-names"></a>Depolama hesabı adları için hataları çözümleyin

Bu makalede, bir depolama hesabını dağıtırken karşılaşabileceğiniz adlandırma hataları açıklanır.

## <a name="symptom"></a>Belirti

Depolama hesabınızın adını yasaklanmış karakterler içeriyorsa, gibi bir hata alırsınız:

```
Code=AccountNameInvalid
Message=S!torageckrexph7isnoc is not a valid storage account name. Storage account name must be 
between 3 and 24 characters in length and use numbers and lower-case letters only.
```

Depolama hesapları için Azure'da benzersiz olan bir kaynak için bir ad sağlamanız gerekir. Benzersiz bir ad belirtmezseniz, gibi bir hata alırsınız:

```
Code=StorageAccountAlreadyTaken
Message=The storage account named mystorage is already taken.
```

Aboneliğinizde mevcut bir depolama hesabı aynı ada sahip bir depolama hesabı dağıtmak, ancak farklı bir konum sağlayın, depolama hesabı zaten farklı bir konumda var olduğunu belirten bir hata alırsınız. Mevcut depolama hesabını silin veya mevcut bir depolama hesabı aynı konumda sağlayın.

## <a name="cause"></a>Nedeni

Depolama hesabı adları 3 ila 24 karakter uzunluğunda olmalı ve sayı ve yalnızca küçük harflerden oluşmalıdır. Ad benzersiz olmalıdır.

## <a name="solution"></a>Çözüm

Depolama hesabı adının benzersiz olduğundan emin olun. Benzersiz bir ad adlandırma kuralınızın sonucunu birleştirerek oluşturabileceğiniz [uniqueString](resource-group-template-functions-string.md#uniquestring) işlevi.

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
"type": "Microsoft.Storage/storageAccounts",
```

Depolama hesabınızın adını 24 karakterden uzun olmadığından emin olun. [UniqueString](resource-group-template-functions-string.md#uniquestring) işlevi 13 karakter döndürür. Birleştirme bir önek veya sonek için **uniqueString** neden, 11 karakter olan bir değer sağlayın ya da daha az.

```json
"parameters": {
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name."
      }
    }
}
```

Depolama hesabı adınızı herhangi bir büyük harf veya özel karakterler içermediğinden emin olun.