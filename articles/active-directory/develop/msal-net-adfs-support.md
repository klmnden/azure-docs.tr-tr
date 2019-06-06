---
title: AD FS, .NET için Microsoft kimlik doğrulama Kitaplığı'nda desteği | Azure
description: Microsoft kimlik doğrulama Kitaplığı'ndaki Active Directory Federasyon Hizmetleri (AD FS) desteği için .NET (MSAL.NET) hakkında daha fazla bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/03/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5963c5e83b5af3848edd934328caa1f095bd184
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66676736"
---
# <a name="active-directory-federation-services-support-in-msalnet"></a>Active Directory Federasyon Hizmetleri MSAL.NET desteği
Windows Server'da Active Directory Federasyon Hizmetleri (AD FS) Openıd Connect eklemenize olanak tanır ve OAuth 2.0 tabanlı kimlik doğrulaması ve yetkilendirme geliştiriyorsanız ve bu uygulamaları yüklü uygulamalar için kimlik doğrulaması, kullanıcıların doğrudan AD FS. Daha fazla bilgi için okuma [geliştiriciler için AD FS senaryoları](/windows-server/identity/ad-fs/overview/ad-fs-scenarios-for-developers).

Microsoft kimlik doğrulama kitaplığı için .NET (MSAL.NET), AD FS karşı kimlik doğrulaması için iki senaryoyu destekler:

- Azure Active Directory, kendisi için MSAL.NET konuşmalar olan *Federasyon* AD FS ile.
- MSAL.NET konuşmalar *doğrudan* AD FS sürümü olduğu Openıd (AD FS 2019'den itibaren) Connect uyumlu bir AD FS yetkilisine. Çalışan uygulamalar ile kimlik doğrulaması, MSAL.NET sağlayan doğrudan AD FS'ye bağlanma [Azure Stack](https://azure.microsoft.com/overview/azure-stack/).

## <a name="msal-connects-to-azure-ad-which-is-federated-with-ad-fs"></a>MSAL, AD FS ile Federasyon Azure AD'ye bağlanır.
Yönetilen kullanıcıları (kullanıcılar Azure AD'de yönetilen) oturum açar veya Federasyon kullanıcıları (örneğin, AD FS başka bir kimlik sağlayıcısı tarafından yönetilen kullanıcılara) Azure AD'ye bağlanma MSAL.NET destekler. MSAL.NET Federasyon kullanıcıları, olgu hakkında bilgi sahibi değildir. İlgili olduğu kadar Azure AD'ye anlatıyor.

[Yetkilisi](msal-client-application-configuration.md#authority) , bu durumda, her zamanki yetkilisi (yetkilisi konak adı + Kiracı, ortak ya da kuruluşların) kullanın.

### <a name="acquiring-a-token-interactively"></a>Etkileşimli bir belirteç alınırken
Çağırdığınızda `AcquireTokenInteractive` yöntemi, kullanıcı deneyimini, genellikle:

1. Kullanıcı hesabı kimliklerine girer.
2. Azure AD kısaca "kuruluşunuzun sayfasına yönlendiriliyorsunuz" iletisi görüntülenir.
3. Kullanıcı, kimlik sağlayıcısı oturum açma sayfasına yönlendirilir. Oturum açma sayfası, genellikle kuruluş logosu ile özelleştirilir.

Desteklenen AD FS federasyon Bu senaryoda sürümleridir AD FS v2, AD FS v3 (Windows Server 2012 R2) ve AD FS v4 (AD FS 2016).

### <a name="acquiring-a-token-using-acquiretokenbyintegratedauthentication-or-acquiretokenbyusernamepassword"></a>AcquireTokenByIntegratedAuthentication veya AcquireTokenByUsernamePassword kullanarak bir belirtecini alma
Bir belirteç kullanarak alırken `AcquireTokenByIntegratedAuthentication` veya `AcquireTokenByUsernamePassword` yöntemleri, MSAL.NET alır başvurmak için kimlik sağlayıcısı kullanıcı adına göre.  MSAL.NET alır bir [SAML 1.1 belirteci](reference-saml-tokens.md) sonra kimlik sağlayıcısı bağlanılıyor.  MSAL.NET ardından sağlar SAML belirteci bir kullanıcı onayı olarak Azure AD'ye (benzer şekilde [on-behalf-of akışı](msal-authentication-flows.md#on-behalf-of)) JWT'nin geri dönebilirsiniz.

## <a name="msal-connects-directly-to-ad-fs"></a>MSAL AD FS'ye doğrudan bağlanır.
MSAL.NET, Open ID Connect uyumlu olan AD FS 2019 bağlanmayı destekler. Doğrudan AD FS'ye bağlanırken, uygulamanızı oluşturmak için kullanılacak isteyeceksiniz yetkilisi benzer `https://mysite.contoso.com/adfs/`.

Şu anda (Bu Openıd Connect uyumlu olmayan) AD FS 2016 ya da AD FS v2 doğrudan bir bağlantı desteklemek için hiçbir plan yok. AD FS 2016 doğrudan bağlanmasını gerektiren senaryolar desteklemeniz gerekiyorsa, lütfen en son sürümünü kullanın [Azure Active Directory Authentication Library](active-directory-authentication-libraries.md#microsoft-supported-client-libraries). AD FS 2019 için şirket içi sisteminize yükseltildiğinde MSAL.NET kullanmanız mümkün olacaktır.
