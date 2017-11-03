---
title: "Azure AD uygulama galerisinde için çok kiracılı uygulama ekleme | Microsoft Docs"
description: "Özel geliştirilen çok kiracılı uygulamanızı Azure AD uygulama galerisinde nasıl listeleyebilirsiniz açıklar"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 208f0d40bd7a8e8f35f16e1fcb09c305d833dbb2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-add-a-multi-tenant-application-to-the-azure-ad-application-gallery"></a>Azure AD uygulama galerisinde çok kiracılı uygulamaya ekleme

## <a name="what-is-the-azure-ad-application-gallery"></a>Azure AD uygulama galerisinde nedir?

Azure AD uygulama galerisinde uygulamanızı Azure Active Directory müşterilerin etkisi genişletmek ve uygulamanızın Market'te ulaşmak için tüm milyonlarca önünde almak için harika bir yoludur. Uygulamanızı Azure AD uygulama galerisinde nasıl listesinde aşağıdaki adımları açıklanmaktadır.

## <a name="if-your-application-supports-saml-or-openidconnect"></a>SAML veya Openıdconnect uygulamanız destekliyorsa
Azure AD uygulama galerisinde listelemek istediğiniz bir çok kiracılı uygulamanız varsa, ilk uygulamanızı aşağıdaki tek oturum açma teknolojileri birini destekleyen emin olmalısınız:

1. **Openıd Connect** -Openıd Connect kimlik doğrulaması ve Azure AD API yapılandırmasını onayı için kullanarak Azure AD ile doğrudan tümleştirme. Uygulamanızı SAML desteklemez ve yalnızca bir tümleştirme başlıyorsanız, ardından öneri modu budur.
2. **SAML** – üçüncü taraf kimlik sağlayıcıları SAML protokolü kullanarak yapılandırma yeteneğini uygulamanız zaten sahip.

Bu tek oturum açma modlarından birini uygulamanız destekliyorsa ve çok kiracılı uygulamanızı Azure AD uygulama galerisinde listelemek istediğiniz, belgede yer alan adımlar izleyebilirsiniz. Hızlıca başlamak için bir e-posta Gönder  **waadpartners@microsoft.com** .

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a>Uygulamanızı SAML veya Openıdconnect desteklemiyorsa
Uygulamanız bu modlarından birini bile desteklemiyorsa, biz yine bizim parola çoklu oturum açma teknolojisini kullanarak bizim Galerisine tümleştirebilirsiniz. Bu seçenek keşfetmek isterseniz, e-posta gönderebilirsiniz  **waadpartners@microsoft.com** .

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamanız Azure Active Directory Uygulama galerisinde listelemek nasıl](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)
