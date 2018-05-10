---
title: B2clogin.com| kullanma Microsoft Docs
description: B2clogin.com yerine login.microsoftonline.com kullanma hakkında bilgi edinin.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 04/29/2018
ms.author: davidmu
ms.openlocfilehash: d2737e995b91caa2dcc55027baa2b552e64af23e
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
#<a name="using-b2clogincom"></a>b2clogin.com kullanma

>[!IMPORTANT]
>Bu özellik genel Önizleme sürümü 
>

Şimdi Azure AD B2C hizmetiyle kullanma seçeneğini sahip `<YourTenantName>.b2clogin.com` kullanmak yerine `login.microsoftonline.com`.  Birçok avantaj vardır:
* Diğer Microsoft ürünleri ile aynı tanımlama bilgisi üstbilgisi boyut sınırını artık paylaşır
* URL'niz Microsoft yapılan tüm başvuruları kaldırabilirsiniz (değiştirebilirsiniz `<YourTenantName>.onmicrosoft.com` Kiracı kimliği ile)

 B2clogin.com yararlanmak için aşağıdaki bazıları ayarlamanız gerekir:

1. İçin **artık uç nokta çalıştırmak** kullandığınızdan emin olun `<YourTenantName>.b2clogin.com` kullanmak yerine `login.microsoftonline.com`.
2. MSAL kullanıyorsanız, ayarlamanız gerekir `validateauthority=false`.  Belirteç Verenin hale olmasıdır`<YourTenantName>.b2clogin.com`.