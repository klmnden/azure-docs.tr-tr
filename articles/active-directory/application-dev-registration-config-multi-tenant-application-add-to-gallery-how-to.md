---
title: Azure AD uygulama galerisinde çok müşterili bir uygulama eklemek | Microsoft Docs
description: Özel geliştirilmiş çok müşterili uygulamanızı Azure AD uygulama galerisinde nasıl listeleyebilirsiniz açıklanmaktadır.
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
ms.openlocfilehash: 51a088ccdcc018b85a70f72a5f88fab8de3c7363
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="add-a-multitenant-application-to-the-azure-ad-application-gallery"></a>Azure AD uygulama galerisinde çok müşterili bir uygulamaya ekleyin

## <a name="what-is-the-azure-ad-application-gallery"></a>Azure AD uygulama galerisinde nedir?

Azure Active Directory (Azure AD), bir bulut tabanlı kimlik hizmetidir. [Azure AD uygulama galerisinde](https://azure.microsoft.com/marketplace/active-directory/all/) burada tüm uygulama bağlayıcıları yayımlanır çoklu oturum açma ve kullanıcı hazırlama için Azure Marketi uygulama mağazası bulunmaktadır. Azure AD kimlik sağlayıcısı olarak kullanan müşteriler, burada yayımlanan farklı SaaS uygulama bağlayıcıları bulun. BT yöneticileri uygulama Galeriden bağlayıcılar ekleyin ve sonra yapılandırmak ve çoklu oturum açma ve sağlama için bağlayıcıları kullanın. Azure AD çoklu oturum açma için SAML 2.0, Openıd Connect, OAuth ve WS-Fed dahil olmak üzere tüm büyük Federasyon protokollerini destekler. 

## <a name="if-your-application-supports-saml-or-openidconnect"></a>SAML veya Openıdconnect uygulamanız destekliyorsa
Azure AD uygulama galerisinde listelenen istediğiniz çok müşterili bir uygulamanız varsa, ilk uygulamanızı aşağıdaki tek oturum açma teknolojileri birini destekleyen emin olmalısınız:

- **Openıd Connect**: uygulamanızı listelenen sağlamak için Azure AD çok müşterili uygulama oluşturma ve uygulama [Azure AD onay framework](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#overview-of-the-consent-framework) uygulamanız için. Herhangi bir müşteriye uygulama onay ve böylece ortak bir uç oturum açma isteği gönderin. Kiracı kimliği ve belirteçte alınan kullanıcı UPN göre bir kullanıcının erişimi denetleyebilirsiniz. Uygulama içinde açıklanan işlemi kullanarak gönderme [Azure Active Directory Uygulama galerisinde uygulamanızı listeleme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

- **SAML**: SAML 2.0 uygulamanız destekliyorsa, uygulama galerisinde listelenebilir. ' Ndaki yönergeleri izleyin [Azure Active Directory Uygulama galerisinde uygulamanızı listeleme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a>Uygulamanızı SAML veya Openıdconnect desteklemiyorsa
SAML veya Openıdconnect desteklemeyen uygulamalar, oturum açma parolası tek teknolojisi aracılığıyla uygulama Galerisi içine hala tümleştirilebilir.

Parola tekli parola kasası oluşturma olarak da bilinir oturum, kullanıcı erişimi ve parolaları Kimlik Federasyonu Desteği web uygulamalarına yönetmenizi sağlar. Birkaç kullanıcı, kuruluşunuzun sosyal medya uygulaması hesaplarına gibi tek bir hesap paylaşmak gereken senaryoları için de yararlıdır. 

Bu teknoloji uygulamanızla listesinde istiyorsanız:
1. Yapılandırmak için bir HTML oturum açma sayfasına sahip bir web uygulaması oluşturma [parola çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis). 
2. Bölümünde açıklandığı gibi isteme [Azure Active Directory Uygulama galerisinde uygulamanızı listeleme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

## <a name="escalations"></a>Çözümler

Tüm çözümler için e-posta Gönder [Azure AD SSO tümleştirme takım](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) ve size geri için mümkün olan en kısa sürede elde edersiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [uygulamanızı Azure Active Directory Uygulama galerisinde listelemek](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).
