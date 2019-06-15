---
title: Azure AD uygulama Galerisi çok kiracılı bir uygulama ekleme | Microsoft Docs
description: Özel olarak geliştirilmiş çok kiracılı uygulamanızı Azure AD uygulama galerisinde nasıl listeleyebilirsiniz açıklar.
services: active-directory
documentationCenter: na
author: rwike77
manager: CelesteDG
ms.assetid: 92c1651a-675d-42c8-b337-f78e7dbcc40d
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: ryanwi
ms.reviewer: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0434ec889a791079e2019bcb1d04f5ce0fceb731
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65545550"
---
# <a name="add-a-multitenant-application-to-the-azure-ad-application-gallery"></a>Azure AD uygulama Galerisi çok kiracılı bir uygulama ekleme

## <a name="what-is-the-azure-ad-application-gallery"></a>Azure AD uygulama Galerisi nedir?

Azure Active Directory (Azure AD), bulut tabanlı kimlik hizmetidir. [Azure AD uygulama Galerisi](https://azure.microsoft.com/marketplace/active-directory/all/) burada tüm uygulama bağlayıcıları yayımlanır çoklu oturum açma ve kullanıcı sağlama için Azure Marketi'nde uygulama mağazasında olduğu. Kimlik sağlayıcısı olarak Azure AD kullanan müşteriler, yayınlanan buraya farklı SaaS uygulama bağlayıcıları bulun. BT yöneticileri uygulama galerisinden bağlayıcıları ekleyin ve ardından yapılandırın ve çoklu oturum açmayı ve sağlama için bağlayıcıları kullanın. Azure AD çoklu oturum açma için SAML 2.0, Openıd Connect, OAuth ve WS-Federasyon dahil olmak üzere, tüm önemli Federasyon protokollerini destekler. 

## <a name="if-your-application-supports-saml-or-openidconnect"></a>Uygulamanız SAML veya Openıdconnect destekliyorsa
Azure AD uygulama galerisinde listelenmesini istediğiniz çok kiracılı bir uygulama varsa, ilk uygulamanızı aşağıdaki tek oturum açma teknolojileri birini destekleyen emin olmalısınız:

- **Openıd Connect**: Listelenen uygulamanızı sağlamak için Azure AD'de çok kiracılı bir uygulama oluşturmak ve uygulamak [Azure AD'ye onay çerçevesine](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) uygulamanız için. Herhangi bir müşteri uygulama onay sağlayabilmesi için ortak bir uç nokta oturum açma isteği gönderin. Kiracı kimliği ve belirteçte alınan kullanıcının UPN göre bir kullanıcının erişimi denetleyebilirsiniz. İçinde açıklanan işlemi kullanarak uygulama gönderme [Azure Active Directory Uygulama galerisinde uygulamanızı listeleme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

- **SAML**: Uygulamanız SAML 2.0 destekliyorsa, app Galerisi'nde listelenebilir. Bölümündeki yönergeleri [Azure Active Directory Uygulama galerisinde uygulamanızı listeleme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a>Uygulamanız SAML veya Openıdconnect desteklemiyorsa
SAML veya Openıdconnect desteklemeyen uygulamaları, oturum açma parolası tek teknoloji app Galerisi'nde içine yine de tümleştirilebilir.

Parola tek parola kasası oluşturma, olarak da adlandırılan oturum, kullanıcı erişim ve Kimlik Federasyonu Desteği web uygulamalarına parolalarını yönetmenize olanak sağlar. Kuruluşunuzun sosyal medya uygulaması hesaplara gibi tek bir hesabı paylaşmak birkaç kullanıcı gerektiği senaryolar için de yararlıdır. 

Bu teknoloji uygulamanızla listelemek istiyorsanız:
1. Yapılandırmak için bir HTML oturum açma sayfasına sahip bir web uygulaması oluşturma [parola çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis). 
2. Bölümünde anlatıldığı gibi talebinizi [Azure Active Directory Uygulama galerisinde uygulamanızı listeleme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

## <a name="escalations"></a>Yardım istekleri

Tüm çözümler için e-posta Gönder [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) ve biz size hemen döneceğiz.

## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).
