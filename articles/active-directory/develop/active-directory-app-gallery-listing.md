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
ms.date: 04/27/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: 1d315cf63bcbf37b6b03b5a965ac615282526682
ms.sourcegitcommit: 1131386137462a8a959abb0f8822d1b329a4e474
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2017
---
# <a name="listing-your-application-in-the-azure-active-directory-application-gallery"></a>Uygulamanız Azure Active Directory Uygulama galerisinde listeleme
Çoklu oturum açma Azure Active Directory ile destekleyen bir uygulama listelemek için [Azure AD galeri](https://azure.microsoft.com/marketplace/active-directory/all/), uygulamanın önce aşağıdaki tümleştirme modlarından birini uygulamanız gerekir:

* **Openıd Connect** -Openıd Connect kimlik doğrulaması ve Azure AD API yapılandırmasını onayı için kullanarak Azure AD ile doğrudan tümleştirme. Uygulamanızı SAML desteklemez ve yalnızca bir tümleştirme başlıyorsanız, ardından öneri modu budur.
* **SAML** – üçüncü taraf kimlik sağlayıcıları SAML protokolü kullanarak yapılandırma yeteneğini uygulamanız zaten sahip.

Her iki modda listeleme gereksinimleri aşağıda verilmiştir.

## <a name="openid-connect-integration"></a>Openıd Connect tümleştirme
Uygulamanızı aşağıdaki Azure AD ile tümleştirmek için [Geliştirici yönergeleri](active-directory-authentication-scenarios.md). Ardından aşağıdaki sorular tamamlamak ve göndermek waadpartners@microsoft.com.

* Uygulamanızla Azure AD ekibi tarafından tümleştirme test etmek için kullanılan bir test Kiracı ya da hesabı için kimlik bilgilerini sağlayın.  
* Azure AD ekibi nasıl oturum açabilir ve kullanarak uygulamanızı örneğini Azure ad connect üzerinde yönergelerinizi [Azure AD onay framework](active-directory-integrating-applications.md#overview-of-the-consent-framework). 
* Çoklu oturum açma ile uygulamanızı test etmek Azure AD ekibi için gerekli tüm ek yönergeler sağlar. 
* Aşağıdaki bilgileri sağlayın:

> Şirket adı:
> 
> Şirket Web sitesi:
> 
> Uygulama adı:
> 
> Uygulama açıklaması (200 karakter sınırı):
> 
> Uygulama Web sitesi (bilgilendirme):
> 
> Uygulamanın teknik destek Web sitesi veya kişi bilgileri:
> 
> Uygulama Ayrıntıları https://portal.azure.com gösterildiği gibi uygulamanın uygulama Kimliğini:
> 
> Uygulama kaydolma müşteriler için kaydolun nereye URL'si ve/ya da uygulama satın alın:
> 
> Uygulamanızın (kullanılabilir kategoriler için bkz. Azure Active Directory Marketi) altında listelenecek en çok üç kategorileri seçin:
> 
> Uygulama küçük simgesi (PNG dosyası, 45px, düz bir arka plan rengi tarafından 45px) ekleyin:
> 
> Uygulama büyük simge (PNG dosyası, 215px, düz bir arka plan rengi tarafından 215px) ekleyin:
> 
> Uygulama Logo (PNG dosyası, 122px, saydam arka plan rengi tarafından 150px) ekleyin:
> 
> 

## <a name="saml-integration"></a>SAML tümleştirme
SAML 2.0 destekleyen herhangi bir uygulama kullanarak doğrudan bir Azure AD kiracısı ile tümleştirilebilir [özel bir uygulama eklemek için bu yönergeleri](../application-config-sso-how-to-configure-federated-sso-non-gallery.md). Uygulama tümleştirmesi Azure AD ile çalışıp çalışmadığını test ettikten sonra aşağıdaki bilgileri göndermek < mailto:waadpartners@microsoft.com >.

* Uygulamanızla Azure AD ekibi tarafından tümleştirme test etmek için kullanılan bir test Kiracı ya da hesabı için kimlik bilgilerini sağlayın.  
* SAML oturum açma URL'si, veren URL'si (varlık kimliği) ve yanıt URL'si (onaylama tüketici hizmeti) değerleri, uygulamanız için açıklandığı gibi sağlamak [burada](../application-config-sso-how-to-configure-federated-sso-non-gallery.md). SAML meta veri dosyasının bir parçası olarak bu değer genellikle sağlarsanız, sonra Lütfen, de gönderebilirsiniz.
* Azure AD kimlik sağlayıcısı SAML 2.0 kullanarak uygulamanızdaki yapılandırma kısa bir açıklamasını sağlar. Uygulamanızı yapılandırma Azure AD kimlik sağlayıcısı Self Servis Yönetim Portalı üzerinden destekliyorsa, sonra lütfen yukarıda verilen kimlik bilgileri bu ayarlamanıza olanak içerir emin olun.
* Aşağıdaki bilgileri sağlayın:

> Şirket adı:
> 
> Şirket Web sitesi:
> 
> Uygulama adı:
> 
> Uygulama açıklaması (200 karakter sınırı):
> 
> Uygulama Web sitesi (bilgilendirme):
> 
> Uygulamanın teknik destek Web sitesi veya kişi bilgileri:
> 
> Uygulama kaydolma müşteriler için kaydolun nereye URL'si ve/ya da uygulama satın alın:
> 
> Altında listelenen uygulamanız için en çok üç kategorileri seçin (kullanılabilir kategorilerini görmek için [Azure Active Directory Marketi](https://azure.microsoft.com/marketplace/active-directory/))):
> 
> Uygulama küçük simgesi (PNG dosyası, 45px, düz bir arka plan rengi tarafından 45px) ekleyin:
> 
> Uygulama büyük simge (PNG dosyası, 215px, düz bir arka plan rengi tarafından 215px) ekleyin:
> 
> Uygulama Logo (PNG dosyası, 122px, saydam arka plan rengi tarafından 150px) ekleyin:
> 
> 

