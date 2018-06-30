---
title: B2clogin.com kullanarak | Microsoft Docs
description: B2clogin.com yerine login.microsoftonline.com kullanma hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/29/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: c4b3122984cdcb324f7b86e44a62e111d6ca0a29
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37131601"
---
# <a name="using-b2clogincom"></a>b2clogin.com kullanma

>[!IMPORTANT]
>Bu özellik genel Önizleme sürümü 
>

Şimdi Azure AD B2C hizmetiyle kullanma seçeneğini sahip `<YourTenantName>.b2clogin.com` kullanmak yerine `login.microsoftonline.com`.  Birçok avantaj vardır:
* Artık diğer Microsoft ürünleri ile aynı tanımlama bilgisi üstbilgisi boyut sınırını paylaşır.
* URL'niz Microsoft yapılan tüm başvuruları kaldırabilirsiniz (değiştirebilirsiniz `<YourTenantName>.onmicrosoft.com` Kiracı kimliği ile). Örneğin: `https://<tenantname>.b2clogin.com/tfp/<tenantname>/<policyname>/v2.0/.well-known/openid-configuration`.

 B2clogin.com yararlanmak için aşağıdaki bazıları ayarlamanız gerekir:

1. İçin **artık uç nokta çalıştırmak** kullandığınızdan emin olun `<YourTenantName>.b2clogin.com` kullanmak yerine `login.microsoftonline.com`.
2. MSAL kullanıyorsanız, ayarlamanız gerekir `validateauthority=false`.  Belirteç Verenin hale olmasıdır`<YourTenantName>.b2clogin.com`.