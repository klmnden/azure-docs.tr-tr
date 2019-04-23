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
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/08/2019
ms.author: celested
ms.reviewer: elisol, bryanla
ms.custom: aaddev, seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1b5ec950a91f3ed0099873e40c7235a9d59f0cb2
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59787399"
---
# <a name="how-to-list-your-application-in-the-azure-active-directory-application-gallery"></a>Nasıl yapılır: Azure Active Directory uygulama galerisinde uygulamanızı listeleme

Bu makalede, bir Azure AD uygulama galerisinde listeleyin, çoklu oturum açma (SSO) uygulama ve listenin yönetme gösterilmektedir.

## <a name="what-is-the-azure-ad-application-gallery"></a>Azure AD uygulama Galerisi nedir?

- Müşteriler, olası en iyi çoklu oturum açma deneyimi bulun.
- Uygulamasının basit ve en az bir yapılandırmadır.
- Hızlı arama uygulamanızı galeride bulur.
- Ücretsiz, temel, ve Azure AD Premium müşterileri Bu tümleştirmeyi tüm kullanabilirsiniz.
- Karşılıklı müşteriler, bir adım adım yapılandırma Öğreticisi.
- SCIM kullanan müşteriler, aynı uygulama için sağlama kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- Federe uygulamaları (Open ID ve SAML/WS-Federasyon), uygulamanın Azure AD Galerisi'nde listelenmeyle SaaS modeli desteklemesi gerekir. Kurumsal galeri uygulamalar birden çok müşteri yapılandırmalarının ve hiçbir belirli bir müşteri desteklemelidir.

- Open ID Connect için uygulamanın çok kiracılı olmalıdır ve [Azure AD'ye onay çerçevesine](consent-framework.md) uygulamayı düzgün bir şekilde uygulanmalıdır. Herhangi bir müşteri uygulama onay sağlayabilmesi kullanıcı oturum açma isteği ortak bir uç noktasına gönderebilir. Kiracı kimliği ve belirteçte alınan kullanıcının UPN göre kullanıcı erişimi denetleyebilirsiniz.

- SAML 2.0/WS-Federasyon, SP veya IDP modunda SAML/WS-Federasyon SSO tümleştirme yapabilme olanağı, uygulamanızın olmalıdır. Lütfen bu isteği göndermeden önce düzgün çalıştığından emin olun.

- Parola SSO için uygulamanız beklendiği gibi çoklu oturum açma işleri için parola kasası oluşturma yapılabilir, böylece form kimlik doğrulaması desteklediğinden emin olun.

- Otomatik kullanıcı hazırlama istekler için uygulamanın SAML 2.0/WS-Federasyon kullanan çoklu oturum açma özelliği etkin galeride listelenmelidir. SSO ve kullanıcı henüz listeleniyorsa birlikte portalda, sağlama için talep edebilir.

>[!NOTE]
>Biz portalımıza üzerinde yeni istekleri alma durdurduktan için SCIM'yi bağlayıcı istekleri yüksek sayıda çalıştırılmakta olan. Lütfen daha ayrıntılı bir açıklama yapılana kadar isteklerinizi üzerinde tutun. Biz bu gecikmeyi ve bunun neden olabileceği rahatsızlık için özür.

## <a name="submit-the-request-in-the-portal"></a>Portalı'nda isteme

Azure AD ile uygulama tümleştirmenizi çalıştığını test ettikten sonra erişim isteğinizi gönderme sırasında bizim [uygulama ağ portalı](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Office 365 hesabı varsa, bu portalda oturum açmak için kullanın. Aksi durumda, oturum açmak için Microsoft hesabınızı (örneğin, Outlook veya Hotmail) kullanın.

Oturum açma sonra aşağıdaki sayfaya görünüyorsa, kişi [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) ve istek göndermek için kullanmak istediğiniz e-posta hesabı sağlayın. Ardından Azure AD ekip hesabı Microsoft uygulama ağ Portalı'nda ekler.

![SharePoint portalında erişim isteği](./media/howto-app-gallery-listing/errorimage.png)

Hesap eklendikten sonra Microsoft uygulama ağ portalda oturum açın.

Oturum açma sonra aşağıdaki sayfası görünürse, metin kutusuna erişmesi için bir iş gerekçesi sağlamak ve ardından **erişim isteği**.

  ![SharePoint portalında erişim isteği](./media/howto-app-gallery-listing/accessrequest.png)

Ekibimiz, ayrıntıları ve buna göre erişmenizi inceler. İsteğiniz onaylandıktan sonra portalında oturum açın ve tıklayarak talebinizi **gönderme isteği (ISV)** kutucuğuna form giriş sayfası.

![SharePoint portal giriş sayfası](./media/howto-app-gallery-listing/homepage.png)

> [!NOTE]
> Erişim ile ilgili herhangi bir sorun varsa, kişi [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

## <a name="implementing-sso-using-federation-protocol"></a>Federasyon protokolünü kullanan SSO uygulama

Uygulamanın Azure AD uygulama galerisinde listelemek için öncelikle bir Azure AD tarafından desteklenen aşağıdaki Federasyon protokollerini uygulamak ve Azure AD uygulama Galerisi hüküm ve koşulları kabul ediyorum gerekir. Hüküm ve Koşulları ' Azure AD uygulama Galerisi okuma [burada](https://azure.microsoft.com/support/legal/active-directory-app-gallery-terms/).

- **Openıd Connect**: Uygulamanız Open ID Connect protokolünü kullanarak Azure AD ile tümleştirmek için izleyin [geliştiricilerin yönergeleri](authentication-scenarios.md).

    ![Openıd Connect galeri uygulamasına listeleme, zaman çizelgesi](./media/howto-app-gallery-listing/openid.png)

    * Openıd Connect, select kullanarak Galeriden uygulamanızı listesine eklemek istiyorsanız **Openıd Connect ve OAuth 2.0** yukarıdaki gibi.
    * Erişim ile ilgili herhangi bir sorun varsa, kişi [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). 

- **SAML 2.0** veya **WS-Federasyon**: Uygulamanız SAML 2.0 destekliyorsa, doğrudan Azure AD kiracısı ile kullanarak tümleştirebilirsiniz [özel bir uygulama eklemek için yönergeleri](../active-directory-saas-custom-apps.md).

  ![Galeri SAML 2.0 veya WS-Federasyon uygulamasına listeleme, zaman çizelgesi](./media/howto-app-gallery-listing/saml.png)

  * Galerisini kullanarak uygulamanızı listesine eklemek isteyip istemediğimi **SAML 2.0** veya **WS-Federasyon**seçin **SAMl 2.0/WS-Federasyon** yukarıdaki gibi.
  * Erişim ile ilgili herhangi bir sorun varsa, kişi [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

## <a name="implementing-sso-using-password-sso"></a>Parola SSO kullanarak SSO uygulama

Yapılandırmak için bir HTML oturum açma sayfasına sahip bir web uygulaması oluşturma [parola tabanlı çoklu oturum açma](../manage-apps/what-is-single-sign-on.md). Parola tabanlı SSO de denir gibi kullanıcı erişim ve Kimlik Federasyonu Desteği web uygulamaları için parolaları yönetmek vaulting, parola sağlar. Kuruluşunuzun sosyal medya uygulaması hesaplara gibi tek bir hesabı paylaşmak birkaç kullanıcı gerektiği senaryolar için de yararlıdır.

![Parola SSO galeri uygulamasına listeleme, zaman çizelgesi](./media/howto-app-gallery-listing/passwordsso.png)

* Parola SSO, select kullanarak Galeriden uygulamanızı listesine eklemek istiyorsanız **parola SSO** yukarıdaki gibi.
* Erişim ile ilgili herhangi bir sorun varsa, kişi [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

## <a name="updateremove-existing-listing"></a>Mevcut listeyi güncelleştirme/kaldırma

Güncelleştirme ya da Azure AD uygulama galerisinde mevcut bir uygulamayı kaldırmak için önce istekte göndermeniz gerekir [uygulama ağ portalı](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Office 365 hesabı varsa, bu portalda oturum açmak için kullanın. Aksi durumda, oturum açmak için Microsoft hesabınızı (örneğin, Outlook veya Hotmail) kullanın.

- Aşağıdaki görüntüde gösterildiği gibi uygun seçeneği belirleyin:

    ![Saml galeri uygulamasına listeleme, zaman çizelgesi](./media/howto-app-gallery-listing/updateorremove.png)

    * Mevcut bir uygulamayı güncelleştirmek isteyip istemediğinizi seçin **mevcut uygulama listesini güncelleştirme**.
    * Azure AD galeri var olan bir uygulamayı kaldırmak isteyip istemediğinizi seçin **mevcut uygulama listesini Kaldır**.
    * Erişim ile ilgili herhangi bir sorun varsa, kişi [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). 

## <a name="listing-requests-by-customers"></a>Müşteriler tarafından istekleri listesi

Müşteriler, uygulamaya tıklayarak listeleme isteği gönderebilir **uygulama isteyen müşteriler tarafından** -> **yeni istek Gönder**.

![Müşteri istenen uygulama kutucuğu](./media/howto-app-gallery-listing/customer-submit-request.png)

Müşteri akışını uygulamalar tarafından istenen aşağıda verilmiştir

![Müşteri uygulamaları akış istendi](./media/howto-app-gallery-listing/customer-request.png)

## <a name="timelines"></a>Zaman çizelgeleri

SAML 2.0 veya WS-Federasyon uygulama galerisinde listeleme işlemi için zaman çizelgesi 7-10 iş günü ' dir.

   ![Saml galeri uygulamasına listeleme, zaman çizelgesi](./media/howto-app-gallery-listing/timeline.png)

Bir Openıd Connect uygulama galerisinde listeleme işlemi için zaman çizelgesi 2-5 iş günü ' dir.

   ![Saml galeri uygulamasına listeleme, zaman çizelgesi](./media/howto-app-gallery-listing/timeline2.png)

## <a name="escalations"></a>Yardım istekleri

Tüm çözümler için e-posta Gönder [Azure AD SSO tümleştirme takım](mailto:SaaSApplicationIntegrations@service.microsoft.com) olduğu SaaSApplicationIntegrations@service.microsoft.com ve size mümkün olan en kısa sürede yanıt vereceğiz.
