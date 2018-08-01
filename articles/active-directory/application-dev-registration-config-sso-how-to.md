---
title: Yeni bir çok kiracılı uygulama yapılandırma | Microsoft Docs
description: Geliştirdiğiniz ve Azure AD'ye kaydetme özel bir uygulama için çoklu oturum açmayı yapılandırma
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 97c698fa99c22b689b5e10b91c87345f4eadd647
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365236"
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
