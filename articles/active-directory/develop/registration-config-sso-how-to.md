---
title: Yeni bir çok kiracılı uygulama yapılandırma | Microsoft Docs
description: Geliştirdiğiniz ve Azure AD'ye kaydetme özel bir uygulama için çoklu oturum açmayı yapılandırma
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: celested
ms.openlocfilehash: 2aa3efcdec57a3e236d2b9739636656b35d16cd6
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44724228"
---
# <a name="how-to-configure-a-new-multi-tenant-application"></a>Yeni bir çok kiracılı uygulama yapılandırma

Federasyon çoklu oturum açma (SSO) uygulamanızda etkinleştirme Openıd Connect, SAML 2.0 veya WS-Federasyon için Azure AD ile Federasyon olduğunda otomatik olarak etkinleştirilir. Zaten Azure AD ile varolan bir oturuma sahip olmasına rağmen oturum açmak son kullanıcılarınızın sorunlarınız varsa, uygulamanızı yanlış olasıdır.

* ADAL/MSAL kullanıyorsanız olduğundan emin olun **PromptBehavior** kümesine **otomatik** yerine **her zaman**.

* Mobil bir uygulama oluşturuyorsanız, aracılı veya aracısız SSO'yu etkinleştirmek için ek yapılandırma gerekebilir.

Android için bkz: [etkinleştirme arası SSO'nun Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android).<br>

İOS için bkz: [etkinleştirme arası SSO'nun ios'ta](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios).

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD SSO](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)<br>

[Android uygulama SSO çapraz etkinleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android)<br>

[Etkinleştirme arası SSO'nun iOS](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios)<br>

[AzureAD uygulamaları tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)<br>

[Uygulama onayı ve AzureAD v2.0 için kullanarak yakınsanmış](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[AzureAD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
