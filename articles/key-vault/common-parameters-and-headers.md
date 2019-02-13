---
title: Ortak parametreler ve üst bilgiler
description: Key Vault kaynaklarla ilgili bunu tüm işlemler için ortak üst bilgileri ve parametreleri.
services: key-vault
documentationcenter: ''
author: bryanla
manager: barbkess
tags: azure-resource-manager
ms.assetid: a715d13ca9-d6e8-4e54-ac5e-0ed9400fb15b15d13ca9-d6e8-4e54-ac5e-0ed9400fb15b
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: bryanla
ms.openlocfilehash: 3fb11ad74e3d1628cbf3f00e2aae648be3eea437
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56107693"
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
     "message": "Anahtar kasası SKU'su geçersiz."  
     }  
   }  

|Öğe adı | Type | Açıklama |
|---|---|---|
| kod | dize | Konusu hatanın türü.|
| message | dize | Hataya neden olan durum açıklaması. |



## <a name="see-also"></a>Ayrıca Bkz.
 [Azure anahtar kasası REST API Başvurusu](/rest/api/keyvault/)
