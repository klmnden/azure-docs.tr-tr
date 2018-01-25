---
title: "Uygulamanız Azure Active Directory Uygulama galerisinde listeleme"
description: "Çoklu oturum açma Azure Active Directory Galerisi'nde destekleyen bir uygulama listelemek nasıl | Microsoft Azure"
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
ms.openlocfilehash: feb09aa8f8e22ad6fbda6a490d251c500bedf3ee
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2018
---
# <a name="listing-your-application-in-the-azure-active-directory-application-gallery"></a>Uygulamanız Azure Active Directory Uygulama galerisinde listeleme


##  <a name="what-is-azure-ad-app-gallery"></a>Azure AD uygulama galerisinde nedir?

Azure AD bir bulut tabanlı kimlik hizmetidir. [Azure AD uygulama galerisinde](https://azure.microsoft.com/marketplace/active-directory/all/) burada tüm uygulama bağlayıcıları yayımlanır çoklu oturum açma ve kullanıcı hazırlama için bir ortak deposudur. Azure AD kimlik sağlayıcısı olarak kullanan müşterilerimizin karşılıklı, burada yayımlanan farklı SaaS uygulama bağlayıcıları, arayın. BT yöneticisi uygulama Galeriden Bağlayıcısı ekler ve yapılandırır ve çoklu oturum açma ve sağlama için kullanın. Azure AD çoklu oturum açma için SAML 2.0, Openıd Connect, OAuth ve WS-Fed gibi tüm önemli Federasyon protokollerini destekler. 

## <a name="what-are-the-benefits-of-listing-the-application-in-the-gallery"></a>Uygulama galerisinde listeleme yararları nelerdir?

*  Müşteriler için en iyi olası tek oturum açma deneyimi sağlar.

*  Uygulamasının basit ve en düşük yapılandırma.

*  Müşteriler, uygulama arayın ve galeriyi bulunamadı. 

*  Müşteri bu tümleştirme Azure AD SKU ücretsiz, Basic veya Premium bağımsız olarak kullanabilirsiniz.

*  Adım yapılandırma Öğreticisi tarafından karşılıklı müşteriler için adım.

*  SCIM'yi kullanıyorsanız kullanıcı aynı uygulama için sağlamayı etkinleştir.


##  <a name="what-are-the-pre-requisites"></a>Ön koşullar nelerdir?

Bir uygulamayı Azure AD galerisinde listelemek için uygulamanın önce Azure AD tarafından desteklenen Federasyon protokollerden birini uygulamak gerekir. Hüküm ve koşulları Azure AD uygulama galerisinde, buradan okuyun. Kullanıyorsanız: 

*   **Openıd Connect** - Azure AD çok kiracılı uygulama oluşturma ve uygulama [Azure AD onay framework](active-directory-integrating-applications.md#overview-of-the-consent-framework) uygulamanız için. Böylece tüm müşteri uygulama onay sağlayabilir ortak uç noktasına oturum açma isteği gönderin. Kiracı kimliği ve belirteçte alınan kullanıcı UPN bağlı müşteri kullanıcı erişimi denetleyebilirsiniz. Uygulamanızı Azure AD ile tümleştirmek için izleyebileceğiniz [Geliştirici yönergeleri](active-directory-authentication-scenarios.md).

*   **SAML 2.0 veya WS-Fed** – uygulamanızı SP veya IDP modunda SAML/WS-Fed SSO tümleştirme yapmak için bir özellik olması gerekir. SAML 2.0 destekleyen herhangi bir uygulama kullanarak doğrudan bir Azure AD kiracısı ile tümleştirilebilir [özel bir uygulama eklemek için yönergeleri](../active-directory-saas-custom-apps.md).

*   **Parola SSO** – yapılandırmak için bir HTML oturum açma sayfasına sahip bir web uygulaması oluşturma [parola tabanlı çoklu oturum açma](../active-directory-appssoaccess-whatis.md). SSO, parola tabanlı ya da gibi kullanıcı erişimi ve Kimlik Federasyonu Desteği web uygulamalarına parolaları yönetmek vaulting, parola sağlar. Birkaç kullanıcı, kuruluşunuzun sosyal medya uygulaması hesaplarına gibi tek bir hesap paylaşmak için gereken yere senaryoları için de yararlıdır. 

## <a name="process-for-submitting-the-request-in-the-portal"></a>Portal istek göndermek için işlem

Uygulama tümleştirmesi Azure AD ile çalışıp çalışmadığını test ettikten sonra üzerinde erişim için bir istek göndermeniz gerekir bizim [uygulama ağ Portal](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Office 365 hesabı varsa, kullanan Aksi durumda bu portalda oturum açma, oturum açmak için Microsoft ID (Live ID, Outlook, Hotmail vb.) kullanın. Erişim istemek için sayfayı görürsünüz. Bir iş gerekçesinin metin kutusuna girin ve tıklayın **istek erişimi**. Ekibimiz tüm ayrıntılarını gözden geçirin ve uygun şekilde erişim verin. Bundan sonra portalında oturum açın ve uygulama için ayrıntılı isteğinizi gönderebilirsiniz.

Erişim ile ilgili herhangi bir sorunla karşılaştığınız, başvurun [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

![SharePoint Portal erişim isteği](./media/active-directory-app-gallery-listing/accessrequest.png)

## <a name="timelines"></a>Zaman çizelgeleri
    
*   İşlem listeleme SAML 2.0 veya Galerisi - WS-Fed uygulamasına **7-10 iş günü**

   ![Galeri saml uygulamasına listeleme, zaman çizelgesi](./media/active-directory-app-gallery-listing/timeline.png)

*   Openıd Connect listeleme işlemi uygulama Galerisi - içine **2-5 iş günü**

   ![Galeri saml uygulamasına listeleme, zaman çizelgesi](./media/active-directory-app-gallery-listing/timeline2.png)

## <a name="escalations"></a>Çözümler

Tüm çözümler için e-posta bırakma [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) ve biz, EE dönebilirsiniz.

