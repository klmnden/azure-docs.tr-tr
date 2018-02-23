---
title: "Azure AD uygulama galerisinde için çok kiracılı uygulama ekleme | Microsoft Docs"
description: "Özel geliştirilen çok kiracılı uygulamanızı Azure AD uygulama galerisinde nasıl listeleyebilirsiniz açıklar"
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 92c1651a-675d-42c8-b337-f78e7dbcc40d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2018
ms.author: jeedes
ms.openlocfilehash: f29f7cbf118d4d70c1ea2cca174ff0cf0ba9bd14
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="how-to-add-a-multi-tenant-application-to-the-azure-ad-application-gallery"></a>Azure AD uygulama galerisinde çok kiracılı uygulamaya ekleme

## <a name="what-is-the-azure-ad-application-gallery"></a>Azure AD uygulama galerisinde nedir?

Azure AD bir bulut tabanlı kimlik hizmetidir. [Azure AD uygulama galerisinde](https://azure.microsoft.com/marketplace/active-directory/all/) burada tüm uygulama bağlayıcıları yayımlanır çoklu oturum açma ve kullanıcı hazırlama için bir ortak deposudur. Azure AD kimlik sağlayıcısı olarak kullanan müşterilerimizin karşılıklı, burada yayımlanan farklı SaaS uygulama bağlayıcıları, arayın. BT yöneticisi uygulama Galeriden Bağlayıcısı ekler ve yapılandırır ve çoklu oturum açma ve sağlama için kullanın. Azure AD çoklu oturum açma için SAML 2.0, Openıd Connect, OAuth ve WS-Fed gibi tüm önemli Federasyon protokollerini destekler. 

## <a name="if-your-application-supports-saml-or-openidconnect"></a>SAML veya Openıdconnect uygulamanız destekliyorsa
Azure AD uygulama galerisinde listelemek istediğiniz bir çok kiracılı uygulamanız varsa, ilk uygulamanızı aşağıdaki tek oturum açma teknolojileri birini destekleyen emin olmalısınız:

1. **Openıd Connect** - Azure AD çok kiracılı uygulama oluşturma ve uygulama [Azure AD onay framework](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications#overview-of-the-consent-framework) uygulamanız için. Böylece tüm müşteri uygulama onay sağlayabilir ortak uç noktasına oturum açma isteği gönderin. Kiracı kimliği ve belirteçte alınan kullanıcı UPN bağlı müşteri kullanıcı erişimi denetleyebilirsiniz. Lütfen bu belirtildiği gibi uygulama gönderme [makale](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing).

2. **SAML** – SAML 2.0 uygulamanız destekliyorsa sonra biz uygulamanızı galerisinde listelemek ve yönergeleri listelenen [burada](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing)

Bu tek oturum açma modlarından birini uygulamanız destekliyorsa ve çok kiracılı uygulamanızı Azure AD uygulama galerisinde listelemek istediğiniz, belirtilen adımları takip edebilirsiniz [bu](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing) makalesi. 

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a>Uygulamanızı SAML veya Openıdconnect desteklemiyorsa
Uygulamanız bu modlarından birini bile desteklemiyorsa, biz yine bizim parola çoklu oturum açma teknolojisini kullanarak bizim Galerisine tümleştirebilirsiniz.

**Parola SSO** – yapılandırmak için bir HTML oturum açma sayfasına sahip bir web uygulaması oluşturma [parola tabanlı çoklu oturum açma](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis). SSO, parola tabanlı ya da gibi kullanıcı erişimi ve Kimlik Federasyonu Desteği web uygulamalarına parolaları yönetmek vaulting, parola sağlar. Birkaç kullanıcı, kuruluşunuzun sosyal medya uygulaması hesaplarına gibi tek bir hesap paylaşmak için gereken yere senaryoları için de yararlıdır. 

Bu teknoloji, uygulamanızdan sonra listesinde istiyorsanız, lütfen açıklandığı gibi isteme [bu](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing) makalesi.

## <a name="escalations"></a>Çözümler

Tüm çözümler için e-posta bırakma [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) ve biz, EE dönebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamanız Azure Active Directory Uygulama galerisinde listelemek nasıl](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)
