---
title: Ortak parametrelerini ve üstbilgileri
description: Parametreleri ve ortak anahtar kasası kaynaklarla ilgili, yapabilecek tüm işlemler için üstbilgiler.
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: a715d13ca9-d6e8-4e54-ac5e-0ed9400fb15b15d13ca9-d6e8-4e54-ac5e-0ed9400fb15b
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: alleonar
ms.openlocfilehash: ead1ac550c9b7c489edefd35d5672a9955e78255
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "34012195"
---
# <a name="common-parameters-and-headers"></a>Ortak parametrelerini ve üstbilgileri

Aşağıdaki bilgiler, anahtar kasası kaynaklarla ilgili, yapabilecek tüm işlemler için ortaktır:

- Değiştir `{api-version}` URI API sürümüyle.
- Değiştir `{subscription-id}` URI, abonelik tanımlayıcısı ile
- Değiştir `{resource-group-name}` kaynak grubu ile. Daha fazla bilgi için Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma konusuna bakın.
- Değiştir `{vault-name}` anahtar kasası adıyla URI.
- Content-Type üstbilgisi application/json değerine ayarlayın.
- Yetkilendirme üst bilgisi bir JSON Web Azure Active Directory (AAD gelen) elde belirteci ayarlayın. Daha fazla bilgi için bkz: [kimlik doğrulaması Azure Resource Manager](authentication-requests-and-responses.md) istekleri.

## <a name="common-error-response"></a>Sık karşılaşılan hata yanıtı
Hizmeti, başarı veya hata durumunu göstermek için HTTP durum kodları kullanır. Ayrıca, aşağıdaki biçimde bir yanıt hataları içerir:

   {  
     "error": {  
     "code": "BadRequest"  
     "iletisi": "anahtar kasası sku geçersiz."  
     }  
   }  

|Öğe adı | Tür | Açıklama |
|---|---|---|
| Kod | dize | Oluştu hata türü.|
| message | dize | Hataya neyin neden olduğunu açıklaması. |



## <a name="see-also"></a>Ayrıca Bkz.
 [Azure anahtar kasası REST API Başvurusu](/rest/api/keyvault/)
