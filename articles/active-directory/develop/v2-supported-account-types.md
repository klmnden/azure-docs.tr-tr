---
title: Desteklenen hesapları uygulamalarında (dinleyici) - Microsoft kimlik platformu
description: İzleyiciler ve uygulamalarda desteklenen hesap türleri hakkında kavramsal belgeler
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: celested
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: d751e28859a3ae1104b28c76d0edfedb2a859eb4
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65406516"
---
# <a name="supported-account-types"></a>Desteklenen hesap türleri

Bu makalede, uygulamaları (bazen izleyiciler olarak adlandırılır) hangi hesapları türleri desteklenir açıklanmaktadır.

<!-- This section can be in an include for many of the scenarios (SPA, Web App signing-in users, protecting a Web API, Desktop (depending on the flows), Mobile -->

## <a name="supported-accounts-types-in-microsoft-identity-platform-applications"></a>Microsoft Identity platformu uygulamalarında desteklenen hesapları türleri

Microsoft Azure genel bulut ortamında, çoğu uygulama türleri ile herhangi bir hedef kitleye kullanıcılar oturum açabilir:

- Bir satır, iş kolu (LOB) uygulaması yazıyorsanız, kendi kuruluşunuzda kullanıcıların oturum açabilirsiniz. Bu tür bir uygulama bazen adlı **tek kiracılı**.
- Bir ISV iseniz, bir uygulama yazabilirsiniz oturum açtığında kullanıcıları:

  - Herhangi bir kuruluşta. Bu tür bir uygulama adlı bir **çok kiracılı** web uygulaması. BT'nin, bazen okumanız oturum açtığında, kullanıcıların iş veya Okul hesaplarıyla.
  - Kendi iş veya okul veya kişisel Microsoft hesabı ile.
  - Yalnızca kişisel Microsoft hesabı.
    > [!NOTE]
    > Şu anda yalnızca bir uygulama için kaydederek Microsoft kimlik platformu kişisel Microsoft hesapları destekler **iş, okul veya kişisel Microsoft hesapları**ve ardından oturum açma için uygulama kodunda belirterek kısıtlama gibi uygulama oluştururken bir Azure AD yetkilisi `https://login.onmicrosoftonline.com/consumers`.

- Bir iş için tüketici uygulama yazıyorsanız, ayrıca Azure AD B2C'yi kullanarak kullanıcıları kendi sosyal kimliklerle oturum açabilirsiniz.

## <a name="certain-authentication-flows-dont-support-all-the-account-types"></a>Belirli kimlik doğrulama akışları tüm hesap türleri desteği

Hesap türleri için bazı belirli kimlik doğrulama akışları ile kullanılamaz. Örneğin, masaüstü, UWP uygulamaları veya arka plan programı uygulamaları:

- Arka plan programı uygulamaları, yalnızca Azure Active Directory kuruluşlar ile kullanılabilir. Bu Microsoft Kişisel hesaplarının (yönetici onayı hiçbir zaman verilecek) işlemek için arka plan programı uygulamalarını kullanma girişimi mantıklı değildir.  
- (Kuruluşunuz veya herhangi bir kuruluş), yalnızca iş veya Okul hesapları ile tümleşik Windows kimlik doğrulaması akışı kullanabilirsiniz. Aslında, tümleşik Windows kimlik doğrulaması etki alanı hesaplarıyla çalışır ve makine etki alanına katılmış olmasını ya da Azure AD'ye katılmış gerektirir. Bu akış, kişisel Microsoft Accounts mantıklı değildir.
- [Kaynak sahibi parola Grant](./v2-oauth-ropc.md) (kullanıcı adı/parola), kişisel Microsoft hesapları ile kullanılamaz. Aslında, kişisel Microsoft hesapları, kullanıcının her oturum açma oturumunda kişisel kaynaklara erişim için onay gerektirir. İşte bu nedenle, bu davranışı etkileşimli olmayan akışları ile uyumlu değildir.
- Cihaz kod akışı, henüz kişisel Microsoft hesapları ile çalışmaz.

## <a name="supported-account-types-in-national-clouds"></a>Ulusal bulutlarda desteklenen hesap türleri

 Uygulamalar kullanıcıların uygulamasında oturum ayrıca [Ulusal Bulutlar](authentication-national-cloud.md). Ancak, kişisel Microsoft hesapları (tarafından bu bulut tanımı) bu bulutlarında desteklenmez. İşte bu desteklenen bir hesap türlerini (tek kiracılı) kuruluşunuz ya da kuruluş (çok kiracılı uygulamalar) bu Bulutlar azaltılır.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [Azure Active Directory'de kiralama](./single-and-multi-tenant-apps.md)
- Daha fazla bilgi edinin [Ulusal Bulutlar](./authentication-national-cloud.md)