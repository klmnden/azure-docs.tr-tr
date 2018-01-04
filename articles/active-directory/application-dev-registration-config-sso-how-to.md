---
title: "Yeni bir çok kiracılı uygulamasının nasıl yapılandırılacağını | Microsoft Docs"
description: "Geliştirme ve Azure AD ile kaydetme özel bir uygulama için çoklu oturum açmayı yapılandırma"
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: c9654a9d03769bbf0e9d0e2c477ad26cefc05150
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-configure-a-new-multi-tenant-application"></a>Yeni bir çok kiracılı uygulama yapılandırma

Federasyon çoklu oturum açma (SSO) uygulamanızda etkinleştirme Openıd Connect, SAML 2.0 ve WS-Fed için Azure AD üzerinden federasyonunu olduğunda otomatik olarak etkinleştirilir. Son kullanıcılarınızın Azure AD varolan bir oturumla zaten sahip olmasına rağmen oturum açmak sorunlarınız varsa, uygulamanızı yanlış olasıdır.

* ADAL/MSAL kullanıyorsanız, olduğundan emin olun **PromptBehavior** kümesine **otomatik** yerine **her zaman**.

* Bir mobil uygulama oluşturuyorsanız, aracılı veya aracılı olmayan SSO'yu etkinleştirmek için ek yapılandırma gerekebilir.

Android için bkz: [etkinleştirme arası SSO'nun Android içinde](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android).<br>

İOS için bkz: [etkinleştirme arası SSO'nun iOS içinde](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios).

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD SSO](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)<br>

[Uygulama SSO Android içinde çapraz etkinleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android)<br>

[Etkinleştirme arası SSO'nun iOS içinde](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios)<br>

[Azuread'i uygulamalara tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)<br>

[İzin ve adının Azuread'i v2.0 için uygulamalar Yakınsanan](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azuread'i StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
