---
title: Uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin | Microsoft Docs
description: Azure Active Directory Uygulama galerisinde çoklu oturum açmayı destekleyen bir uygulama listesinde öğrenin
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/31/2018
ms.author: celested
ms.reviewer: elisol, bryanla
ms.custom: aaddev
ms.openlocfilehash: e5db7b9bed674011c2922f026c301172f347f53f
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666317"
---
# <a name="list-your-application-in-the-azure-active-directory-application-gallery"></a>Azure Active Directory uygulama galerisinde uygulamanızı listeleme


##  <a name="what-is-the-azure-ad-application-gallery"></a>Azure AD uygulama Galerisi nedir?

Azure Active Directory (Azure AD), bulut tabanlı kimlik hizmetidir. [Azure AD uygulama Galerisi](https://azure.microsoft.com/marketplace/active-directory/all/) burada tüm uygulama bağlayıcıları yayımlanır çoklu oturum açma ve kullanıcı sağlama için Azure Marketi'nde uygulama mağazasında olduğu. Kimlik sağlayıcısı olarak Azure AD kullanan müşteriler, yayınlanan buraya farklı SaaS uygulama bağlayıcıları bulun. BT yöneticileri uygulama galerisinden bağlayıcıları ekleyin ve ardından yapılandırın ve çoklu oturum açmayı ve sağlama için bağlayıcıları kullanın. Azure AD çoklu oturum SAML 2.0, Openıd Connect, OAuth ve WS-Federasyon gibi açma için tüm önemli Federasyon protokollerini destekler.

## <a name="what-are-the-benefits-of-listing-an-application-in-the-gallery"></a>Bir uygulama galeride listeleme avantajları nelerdir?

*  Müşteriler, olası en iyi çoklu oturum açma deneyimi bulun.

*  Uygulamasının basit ve en az bir yapılandırmadır.

*  Hızlı arama uygulamanızı galeride bulur.

*  Ücretsiz, temel, ve Azure AD Premium müşterileri Bu tümleştirmeyi tüm kullanabilirsiniz.

*  Karşılıklı müşteriler, bir adım adım yapılandırma Öğreticisi.

*  SCIM kullanan müşteriler, aynı uygulama için sağlama kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- Federe uygulamaları (Open ID ve SAML/WS-Federasyon), uygulamanın Azure AD Galerisi'nde listelenmeyle SaaS modeli desteklemesi gerekir. Kurumsal galeri uygulamalar birden çok müşteri yapılandırmalarının ve hiçbir belirli bir müşteri desteklemelidir.

- Open ID Connect için uygulamanın çok kiracılı olmalıdır ve [Azure AD'ye onay çerçevesine](quickstart-v1-integrate-apps-with-azure-ad.md#overview-of-the-consent-framework) uygulamayı düzgün bir şekilde uygulanmalıdır. Herhangi bir müşteri uygulama onay sağlayabilmesi kullanıcı oturum açma isteği ortak bir uç noktasına gönderebilir. Kiracı kimliği ve belirteçte alınan kullanıcının UPN göre kullanıcı erişimi denetleyebilirsiniz.

- SAML 2.0/WS-Federasyon, SP veya IDP modunda SAML/WS-Federasyon SSO tümleştirme yapabilme olanağı, uygulamanızın olmalıdır. Lütfen bu isteği göndermeden önce düzgün çalıştığından emin olun.

- Parola SSO için beklendiği gibi çoklu oturum açma işleri için parola kasası oluşturma yapılabilir, böylece uygulamanız form kimlik doğrulaması desteklediğinden emin olun.

- Otomatik kullanıcı hazırlama istekler için uygulama yukarıda açıklanan Federasyon protokolünü kullanarak etkin tek oturum açma özelliği ile galeride listelenmelidir. SSO ve kullanıcı henüz listeleniyorsa birlikte portalda, sağlama için talep edebilir.

##  <a name="implementing-sso-using-federation-protocol"></a>Federasyon protokolünü kullanan SSO uygulama

Uygulamanın Azure AD uygulama galerisinde listelemek için öncelikle bir Azure AD tarafından desteklenen aşağıdaki Federasyon protokollerini uygulamak ve Azure AD uygulama Galerisi hüküm ve koşulları kabul ediyorum gerekir. Hüküm ve Koşulları ' Azure AD uygulama Galerisi okuma [burada](https://azure.microsoft.com/en-us/support/legal/active-directory-app-gallery-terms/).

*   **Openıd Connect**: uygulamanız Open ID Connect protokolünü kullanarak Azure AD ile tümleştirmek için izleyin [geliştiricilerin yönergeleri](authentication-scenarios.md).

    ![Openıd Connect galeri uygulamasına listeleme, zaman çizelgesi](./media/howto-app-gallery-listing/openid.png)

    * Openıd Connect, select kullanarak Galeriden uygulamanızı listesine eklemek istiyorsanız **Openıd Connect ve OAuth 2.0** yukarıdaki gibi.

    * Erişim ile ilgili herhangi bir sorun varsa, kişi [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). 

*   **SAML 2.0** veya **WS-Federasyon**: uygulamanız SAML 2.0 destekliyorsa, doğrudan Azure AD kiracısı ile kullanarak tümleştirebilirsiniz [özel bir uygulama eklemek için yönergeleri](../active-directory-saas-custom-apps.md).

    ![Galeri SAML 2.0 veya WS-Federasyon uygulamasına listeleme, zaman çizelgesi](./media/howto-app-gallery-listing/saml.png)

    * Galerisini kullanarak uygulamanızı listesine eklemek isteyip istemediğimi **SAML 2.0** veya **WS-Federasyon**seçin **SAMl 2.0/WS-Federasyon** yukarıdaki gibi.

    * Erişim ile ilgili herhangi bir sorun varsa, kişi [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

## <a name="implementing-sso-using-password-sso"></a>Parola SSO ile SSO uygulama

Yapılandırmak için bir HTML oturum açma sayfasına sahip bir web uygulaması oluşturma [parola tabanlı çoklu oturum açma](../manage-apps/what-is-single-sign-on.md). Parola tabanlı SSO de denir gibi kullanıcı erişim ve Kimlik Federasyonu Desteği web uygulamaları için parolaları yönetmek vaulting, parola sağlar. Kuruluşunuzun sosyal medya uygulaması hesaplara gibi tek bir hesabı paylaşmak birkaç kullanıcı gerektiği senaryolar için de yararlıdır.

![Parola SSO galeri uygulamasına listeleme, zaman çizelgesi](./media/howto-app-gallery-listing/passwordsso.png)

* Parola SSO, select kullanarak Galeriden uygulamanızı listesine eklemek istiyorsanız **parola SSO** yukarıdaki gibi.

* Erişim ile ilgili herhangi bir sorun varsa, kişi [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

##  <a name="updateremove-existing-listing"></a>Mevcut listeyi güncelleştirme/kaldırma

Güncelleştirme ya da Azure AD uygulama galerisinde mevcut bir uygulamayı kaldırmak için önce istekte göndermeniz gerekir [uygulama ağ portalı](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Office 365 hesabı varsa, bu portalda oturum açmak için kullanın. Aksi durumda, oturum açmak için Microsoft hesabınızı (örneğin, Outlook veya Hotmail) kullanın.

* Uygun seçeneği seçin aşağıdaki görüntüde

    ![Saml galeri uygulamasına listeleme, zaman çizelgesi](./media/howto-app-gallery-listing/updateorremove.png)

    * Mevcut bir uygulamayı güncelleştirmek isteyip istemediğinizi seçin **mevcut uygulama listesini güncelleştirme**.

    * Azure AD galeri var olan bir uygulamayı kaldırmak isteyip istemediğinizi seçin **mevcut uygulama listesini Kaldır**

    * Erişim ile ilgili herhangi bir sorun varsa, kişi [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). 

## <a name="submit-the-request-in-the-portal"></a>Portalı'nda isteme

Azure AD ile uygulama tümleştirmenizi çalıştığını test ettikten sonra erişim isteğinizi gönderme sırasında bizim [uygulama ağ portalı](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Office 365 hesabı varsa, bu portalda oturum açmak için kullanın. Aksi durumda, oturum açmak için Microsoft hesabınızı (örneğin, Outlook veya Hotmail) kullanın.

Oturum açtıktan sonra aşağıdaki sayfa açılır. Metin kutusuna erişmesi için bir iş gerekçesi sağlamak ve ardından **erişim isteği**. Ekibimiz, ayrıntıları ve buna göre erişmenizi inceler. Bundan sonra portalda oturum açın ve uygulama için ayrıntılı isteğinizi gönderebilirsiniz.

Erişim ile ilgili herhangi bir sorun varsa, kişi [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

![SharePoint portalında erişim isteği](./media/howto-app-gallery-listing/accessrequest.png)

## <a name="timelines"></a>Zaman çizelgeleri
    
SAML 2.0 veya WS-Federasyon uygulama galerisinde listeleme işlemi için zaman çizelgesi 7-10 iş günü ' dir.

   ![Saml galeri uygulamasına listeleme, zaman çizelgesi](./media/howto-app-gallery-listing/timeline.png)

Bir Openıd Connect uygulama galerisinde listeleme işlemi için zaman çizelgesi 2-5 iş günü ' dir.

   ![Saml galeri uygulamasına listeleme, zaman çizelgesi](./media/howto-app-gallery-listing/timeline2.png)

Galerideki uygulama kullanıcı desteği sağlama ile listeleme işlemi için zaman çizelgesi 40-45 iş gündür.

   ![Saml galeri uygulamasına listeleme, zaman çizelgesi](./media/howto-app-gallery-listing/provisioningtimeline.png)

## <a name="escalations"></a>Yardım istekleri

Tüm çözümler için e-posta Gönder [Azure AD SSO tümleştirme takım](mailto:SaaSApplicationIntegrations@service.microsoft.com) olduğu SaaSApplicationIntegrations@service.microsoft.com ve size mümkün olan en kısa sürede yanıt vereceğiz.