---
title: B2clogin.com kullanma | Microsoft Docs
description: Login.microsoftonline.com yerine b2clogin.com kullanma hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/29/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 6ad0a5d59b28bf48742c9e1be89b51d2301dd582
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189299"
---
# <a name="using-b2clogincom"></a>b2clogin.com kullanma

>[!IMPORTANT]
>Bu özellik genel önizlemeye sunuldu. 
>

Azure AD B2C hizmetiyle kullanma seçeneği artık sahip `<YourTenantName>.b2clogin.com` kullanmak yerine `login.microsoftonline.com`.  Bu, birçok faydası vardır:
* Artık aynı tanımlama bilgisi üstbilgisi boyut sınırını diğer Microsoft ürünleri ile paylaşın.
* Microsoft tüm başvuruları, URL'de kaldırabilirsiniz (değiştirebilirsiniz `<YourTenantName>.onmicrosoft.com` Kiracı kimliğinizle). Örneğin: `https://<tenantname>.b2clogin.com/tfp/<tenantID>/<policyname>/v2.0/.well-known/openid-configuration`.

 B2clogin.com yararlanmak için aşağıdakileri ayarlayın gerekir:

1. İçin **Şimdi Çalıştır uç noktası** kullandığınızdan emin olun `<YourTenantName>.b2clogin.com` kullanmak yerine `login.microsoftonline.com`.
2. MSAL kullanıyorsanız, ayarlanacak ihtiyacınız `validateauthority=false`.  Belirteci veren haline gelir olmasıdır`<YourTenantName>.b2clogin.com`.
