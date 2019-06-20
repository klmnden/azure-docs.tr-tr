---
title: Ortak parametreler ve üst bilgiler
description: Key Vault kaynaklarla ilgili bunu tüm işlemler için ortak üst bilgileri ve parametreleri.
services: key-vault
author: msmbaldwin
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: 01ac02ca0e449dcac8fcd05450566fd8138acba3
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "64696682"
---
# <a name="common-parameters-and-headers"></a>Ortak parametreler ve üst bilgiler

Aşağıdaki bilgiler, Key Vault kaynaklarla ilgili bunu tüm işlemler için ortaktır:

- Değiştirin `{api-version}` URI'si, api-version ile.
- Değiştirin `{subscription-id}` URI, abonelik tanımlayıcısı ile
- Değiştirin `{resource-group-name}` kaynak grubu ile. Daha fazla bilgi için Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma konusuna bakın.
- Değiştirin `{vault-name}` ile anahtar kasanızın adının URI.
- Content-Type üst bilgisi, application/json değerine ayarlayın.
- Yetkilendirme üst bilgisi bir JSON Web Azure Active Directory (AAD gelen) edindiğiniz belirteci ayarlayın. Daha fazla bilgi için [kimlik doğrulaması Azure Resource Manager](authentication-requests-and-responses.md) istekleri.

## <a name="common-error-response"></a>Genel hata yanıtı
Hizmet, başarıyı veya başarısızlığı göstermek için HTTP durum kodları kullanır. Ayrıca, aşağıdaki biçimde bir yanıt hatalar içerir:

```
   {  
     "error": {  
     "code": "BadRequest",  
     "message": "The key vault sku is invalid."  
     }  
   }  
```

|Öğe adı | Tür | Açıklama |
|---|---|---|
| code | string | Konusu hatanın türü.|
| message | string | Hataya neden olan durum açıklaması. |



## <a name="see-also"></a>Ayrıca Bkz.
 [Azure anahtar kasası REST API Başvurusu](/rest/api/keyvault/)
