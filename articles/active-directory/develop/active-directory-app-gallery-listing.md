---
title: "Uygulamanız Azure Active Directory Uygulama galerisinde listeleme | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory Uygulama galerisinde destekleyen bir uygulama listelemek nasıl"
services: active-directory
documentationcenter: dev-center-name
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/09/2018
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: 502fb555bd3b381c9be0ff04e210cc07f9bf6cd8
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="list-your-application-in-the-azure-active-directory-application-gallery"></a>Azure Active Directory uygulama galerisinde uygulamanızı listeleme


##  <a name="what-is-the-azure-ad-application-gallery"></a>Azure AD uygulama galerisinde nedir?

Azure Active Directory (Azure AD), bir bulut tabanlı kimlik hizmetidir. [Azure AD uygulama galerisinde](https://azure.microsoft.com/marketplace/active-directory/all/) burada tüm uygulama bağlayıcıları yayımlanır çoklu oturum açma ve kullanıcı hazırlama için Azure Marketi uygulama mağazası bulunmaktadır. Azure AD kimlik sağlayıcısı olarak kullanan müşteriler, burada yayımlanan farklı SaaS uygulama bağlayıcıları bulun. BT yöneticileri uygulama Galeriden bağlayıcılar ekleyin ve sonra yapılandırmak ve çoklu oturum açma ve sağlama için bağlayıcıları kullanın. Azure AD çoklu oturum SAML 2.0, Openıd Connect, OAuth ve WS-Fed dahil olmak üzere açma için tüm ana Federasyon protokollerini destekler. 

## <a name="what-are-the-benefits-of-listing-an-application-in-the-gallery"></a>Bir uygulama galerisinde listeleme yararları nelerdir?

*  Müşteri olası en iyi tek oturum açma deneyimi bulun.

*  Uygulamasının basit ve en az bir yapılandırmadır. 

*  Hızlı arama galerisinde uygulamanızı bulur.

*  , Basic, ücretsiz ve Azure AD Premium müşteriler tüm bu tümleştirme özelliğini kullanabilirsiniz. 

*  Karşılıklı müşteriler yapılandırma hakkında adım adım öğretici alın. 

*  SCIM'yi kullanan müşteriler, aynı uygulama için sağlama kullanabilirsiniz.


##  <a name="prerequisites-implement-federation-protocol"></a>Önkoşullar: Uygulama Federasyon Protokolü

Bir uygulamayı Azure AD uygulama galerisinde listelemek için ilk Azure AD tarafından desteklenen aşağıdaki Federasyon protokollerden birini yapması gerekir. Hüküm ve koşulları Azure AD uygulama galerisinde, buradan okuyun. 

*   **Openıd Connect**: çok kiracılı uygulama Azure AD oluşturma ve uygulama [Azure AD onay framework](active-directory-integrating-applications.md#overview-of-the-consent-framework) uygulamanız için. Herhangi bir müşteriye uygulama onay ve böylece ortak bir uç oturum açma isteği gönderin. Kiracı kimliği ve belirteçte alınan kullanıcı UPN göre kullanıcı erişimi denetleyebilirsiniz. Uygulamanızı Azure AD ile tümleştirmek için izleyin [geliştiricilerinin yönergeleri](active-directory-authentication-scenarios.md).

*   **SAML 2.0** veya **WS-Fed**: uygulamanızı SP veya IDP modunda SAML/WS-Fed SSO tümleştirme yapmak için özelliği olması gerekir. Uygulamanızı SAML 2.0 destekliyorsa, doğrudan Azure AD kiracısı ile kullanarak tümleştirebilirsiniz [özel bir uygulama eklemek için yönergeleri](../active-directory-saas-custom-apps.md).

*   **Parola SSO**: yapılandırmak için bir HTML oturum açma sayfasına sahip bir web uygulaması oluşturma [parola tabanlı çoklu oturum açma](../active-directory-appssoaccess-whatis.md). SSO, parola tabanlı ya da gibi kullanıcı erişimi ve Kimlik Federasyonu Desteği web uygulamalarına parolaları yönetmek vaulting, parola sağlar. Birkaç kullanıcı, kuruluşunuzun sosyal medya uygulaması hesaplarına gibi tek bir hesap paylaşmak gereken senaryoları için de yararlıdır. 

## <a name="submit-the-request-in-the-portal"></a>Portalı'nda isteme

Uygulama tümleştirmesi Azure AD ile çalışıp çalışmadığını test ettikten sonra erişim için isteğiniz gönderme sırasında bizim [uygulama ağ Portal](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Office 365 hesabı varsa, bu portalda oturum açmak için kullanın. Aksi durumda, oturum açmak için Microsoft hesabınızı (örneğin, Outlook veya Hotmail) kullanın.

Oturum açtıktan sonra aşağıdaki sayfası görüntülenir. Metin kutusuna erişmesi için bir iş gerekçesinin sağlayın ve ardından **istek erişimi**. Ekibimiz gözden geçirir Ayrıntılar ve buna göre erişim sağlar. Bundan sonra portalında oturum açın ve uygulama için ayrıntılı isteğinizi gönderebilirsiniz.

Erişim ile ilgili herhangi bir sorun varsa, başvurun [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

![SharePoint Portal erişim isteği](./media/active-directory-app-gallery-listing/accessrequest.png)

## <a name="timelines"></a>Zaman çizelgeleri
    
SAML 2.0 veya WS-Fed uygulama galerisinde listeleme işlemi için zaman çizelgesi 7-10 iş gündür.

   ![Galeri saml uygulamasına listeleme, zaman çizelgesi](./media/active-directory-app-gallery-listing/timeline.png)

Bir Openıd Connect uygulama galerisinde listeleme işlemi için zaman çizelgesi 2-5 iş günü verilir.

   ![Galeri saml uygulamasına listeleme, zaman çizelgesi](./media/active-directory-app-gallery-listing/timeline2.png)

## <a name="escalations"></a>Çözümler

Tüm çözümler için e-posta Gönder [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) ve mümkün olan en kısa sürede yanıt vermemiz.

