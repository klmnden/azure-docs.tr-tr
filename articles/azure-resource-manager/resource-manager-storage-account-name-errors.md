---
title: "Azure depolama hesabı adı hataları | Microsoft Docs"
description: "Depolama hesabı adı belirtirken karşılaşabileceğiniz hatalar açıklanır."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 09/13/2017
ms.author: tomfitz
ms.openlocfilehash: dc045827fbd38054a334ff22eb30e0db6a31bac8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-errors-for-storage-account-names"></a>Depolama hesabı adları için hataları çözümleyin

Bu makalede, bir depolama hesabını dağıtırken karşılaşabileceğiniz adlandırma hatalar açıklanır.

## <a name="symptom"></a>Belirti

Depolama hesabı adı yasaklanmış karakterleri içeriyorsa, benzer bir hata alırsınız:

```
Code=AccountNameInvalid
Message=S!torageckrexph7isnoc is not a valid storage account name. Storage account name must be 
between 3 and 24 characters in length and use numbers and lower-case letters only.
```

Depolama hesapları için Azure arasında benzersiz kaynak için bir ad sağlamanız gerekir. Benzersiz bir ad belirtmezseniz, benzer bir hata alırsınız:

```
Code=StorageAccountAlreadyTaken
Message=The storage account named mystorage is already taken.
```

Aboneliğinizde var olan bir depolama hesabıyla aynı ada sahip bir depolama hesabı dağıtmak, ancak farklı bir konum sağlayın, depolama hesabı zaten var. farklı bir konumda belirten bir hata alırsınız. Var olan depolama hesabını silmek ya da var olan depolama hesabı ile aynı konumda sağlayın.

## <a name="cause"></a>Nedeni

Depolama hesabı adları 3 ile 24 karakter uzunluğunda olmalıdır ve rakamlar ve yalnızca küçük harfler kullanmanız gerekir. Adın benzersiz olması gerekir.

## <a name="solution"></a>Çözüm

### <a name="solution-1"></a>Çözüm 1

Depolama hesabı adının benzersiz olduğundan emin olun. Adlandırma kuralınızın sonucu ile birleştirerek benzersiz bir ad oluşturabilirsiniz [uniqueString](resource-group-template-functions-string.md#uniquestring) işlevi.

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
"type": "Microsoft.Storage/storageAccounts",
```

### <a name="solution-2"></a>Çözüm 2

Depolama hesabı adı 24 karakterden uzun olmadığından emin olun. [UniqueString](resource-group-template-functions-string.md#uniquestring) işlevi 13 karakter döndürür. Bir önek birleştirme ya da için sonek **uniqueString** neden, 11 karakter olan bir değer sağlayın veya daha az.

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

### <a name="solution-3"></a>Çözüm 3

Depolama hesabınızın adının herhangi bir büyük harf veya özel karakterler içermediğinden emin olun.