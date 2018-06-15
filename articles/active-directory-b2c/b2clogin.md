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
ms.openlocfilehash: c41c02acaeffa170d55f3c59f34a4b1ecae1c523
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34712462"
---
# <a name="using-b2clogincom"></a>b2clogin.com kullanma

>[!IMPORTANT]
>Bu özellik genel Önizleme sürümü 
>

Şimdi Azure AD B2C hizmetiyle kullanma seçeneğini sahip `<YourTenantName>.b2clogin.com` kullanmak yerine `login.microsoftonline.com`.  Birçok avantaj vardır:
* Diğer Microsoft ürünleri ile aynı tanımlama bilgisi üstbilgisi boyut sınırını artık paylaşır
* URL'niz Microsoft yapılan tüm başvuruları kaldırabilirsiniz (değiştirebilirsiniz `<YourTenantName>.onmicrosoft.com` Kiracı kimliği ile)

 B2clogin.com yararlanmak için aşağıdaki bazıları ayarlamanız gerekir:

1. İçin **artık uç nokta çalıştırmak** kullandığınızdan emin olun `<YourTenantName>.b2clogin.com` kullanmak yerine `login.microsoftonline.com`.
2. MSAL kullanıyorsanız, ayarlamanız gerekir `validateauthority=false`.  Belirteç Verenin hale olmasıdır`<YourTenantName>.b2clogin.com`.