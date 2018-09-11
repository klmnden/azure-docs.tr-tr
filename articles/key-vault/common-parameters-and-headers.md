---
title: Ortak parametreler ve üst bilgiler
description: Key Vault kaynaklarla ilgili bunu tüm işlemler için ortak üst bilgileri ve parametreleri.
services: key-vault
documentationcenter: ''
author: bryanla
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: a715d13ca9-d6e8-4e54-ac5e-0ed9400fb15b15d13ca9-d6e8-4e54-ac5e-0ed9400fb15b
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: bryanla
ms.openlocfilehash: dae5e1ab6244d2898bc218ed5db3b6b2b90150cf
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44294062"
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

   {  
     "error": {  
     "code": "BadRequest"  
     "message": "anahtar kasası SKU'su geçersiz."  
     }  
   }  

|Öğe adı | Tür | Açıklama |
|---|---|---|
| Kod | dize | Konusu hatanın türü.|
| message | dize | Hataya neden olan durum açıklaması. |



## <a name="see-also"></a>Ayrıca Bkz.
 [Azure anahtar kasası REST API Başvurusu](/rest/api/keyvault/)
