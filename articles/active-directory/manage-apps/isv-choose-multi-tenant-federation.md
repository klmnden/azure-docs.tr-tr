---
title: Çok kiracılı uygulamanız için doğru Federasyon protokol seçin
description: Azure Active Directory ile tümleştirme, bağımsız yazılım satıcıları için yönergeler
services: active-directory
author: barbaraselden
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: baselden
ms.reviewer: jeeds
ms.collection: M365-identity-device-management
ms.openlocfilehash: 53b315b87200b37cda215a29a65be9babaf54f43
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67795188"
---
# <a name="choose-the-right-federation-protocol-for-your-multi-tenant-application"></a>Çok kiracılı uygulamanız için doğru Federasyon protokol seçin

Yazılım olarak hizmet (SaaS) uygulaması geliştirdiğinizde, sizin ve müşterilerinizin gereksinimlerine en iyi karşılayan federation Protokolü seçmeniz gerekir. Bu karar, müşterilerinizin Office 365 ve Azure AD ekosistemlerinde kullanılabilir verileri tümleştirmek için kültürünüz ve geliştirme platformunuz temel alır.

Tam listesi görmek [protokolleri için SSO tümleştirmeler kullanılabilir](what-is-single-sign-on.md) Azure Active Directory ile.
Aşağıdaki tablo karşılaştırır 
* Açık kimlik doğrulaması 2.0 (OAuth 2.0)
* Open ID Connect (OIDC)
* Güvenlik onaylama işlemi biçimlendirme dili (SAML)
* Web Hizmetleri Federasyonu (WSFed)

| Özellik| OAuth / OIDC| SAML / WSFed |
| - |-|-|
| Web tabanlı çoklu oturum açma| √| √ |
| Web tabanlı çoklu oturum kapatmak| √| √ |
| Mobile tabanlı çoklu oturum açma| √| √* |
| Çoklu oturum kapatmak Mobile tabanlı| √| √* |
| Mobil uygulamalar için koşullu erişim ilkeleri| √| X |
| Mobil uygulamalar için sorunsuz MFA deneyimi| √| X |
| Microsoft Graph erişim| √| X |

\* Örnekleri veya Kılavuzu olası ancak Microsoft sunmaz.

## <a name="oauth-20-and-open-id-connect"></a>OAuth 2.0 ve Open ID Connect

OAuth 2.0 bir [endüstri standardı](https://oauth.net/2/) yetkilendirme için protokol. OIDC (Openıd Connect) olan bir [sektör standardı](https://openid.net/connect/) Oath 2.0 protokolünü üzerinde oluşturulan kimlik kimlik doğrulama katmanı.

### <a name="benefits"></a>Avantajlar

Microsoft kimlik doğrulama ve yetkilendirme protokolleri için yerleşik olarak sahip oldukları gibi OIDC/OAuth 2.0 kullanılmasını önerir. SAML ile yetkilendirme ayrıca uygulamalıdır.

Devralınmış bu protokolleri yetkilendirme uygulamanızın erişim ve zengin kullanıcı ve kuruluş verilerine Microsoft Graph API ile tümleştirmenize olanak tanır.

OAuth 2.0 ve OIDC kullanarak uygulamanız için SSO'yu benimsenmesi, müşterilerinizin son kullanıcı deneyimini basitleştirir. İzin kümeleri gerekli, ardından otomatik olarak yönetici veya son kullanıcı tümleştirip gösterilir kolayca tanımlayabilirsiniz.

Ek olarak, bu protokolleri kullanarak uygulamalara erişimi denetlemek için koşullu erişim ve MFA ilkelerini kullanmak, müşterilerin sağlar. Microsoft kitaplıkları sağlar ve [kod örnekleri birden çok teknoloji platformda](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Samples) geliştirme yardımcı olacak.  

### <a name="implementation"></a>Uygulama

Bir OAuth 2.0 sağlayıcısı Microsoft Identity ile uygulamanızı kaydetmeniz. Ardından da OAuth 2.0 tabanlı herhangi diğer kimlik sağlayıcısıyla ile tümleştirmek istediğiniz kaydedin. 

Uygulamanızı kaydetmek ve bu protokolleri, web uygulamaları için SSO için uygulama hakkında daha fazla bilgi için bkz: [Openıd Connect ile Azure Active Directory kullanarak web uygulamalarına erişim yetkisi verme](../develop/sample-v2-code.md).  Bu protokollerin mobil uygulamalarında SSO için uygulama hakkında daha fazla bilgi için aşağıdakilere bakın: 

* [Android](../develop/quickstart-v2-android.md)

* [iOS](../develop/quickstart-v2-ios.md)

* [Evrensel Windows Platformu](../develop/quickstart-v2-uwp.md)

## <a name="saml-20-and-wsfed"></a>SAML 2.0 ve WSFed

Güvenlik onaylama işlemi biçimlendirme dili (SAML) genellikle web uygulamaları için kullanılır. Bkz: [Azure SAML protokolü kullanan](../develop/active-directory-saml-protocol-reference.md) genel bakış. 

Web Hizmetleri Federasyonu (WSFed) olan bir [sektör standardı](http://docs.oasis-open.org/wsfed/federation/v1.2/ws-federation.html) genellikle .net platformu kullanılarak geliştirilen web uygulamaları için kullanılır.

### <a name="benefits"></a>Avantajlar

SAML 2.0 olgun bir standart ve çoğu teknoloji platformları destek açık kaynak kitaplıkları için SAML 2.0 ' dir. SAML SSO yapılandırmak için bir yönetim arabirimi, müşterilerinizin sağlayabilirsiniz. Microsoft Azure AD ve SAML 2 destekleyen herhangi bir kimlik sağlayıcısı SAML SSO yapılandırabilirsiniz

### <a name="trade-offs"></a>Denge

Multi-Factor Authentication (MFA) dahil olmak üzere bazı koşullu erişim ilkeleri, mobil uygulamalar için SAML 2.0 veya WSFed protokollerini kullanarak, bir düşürülmüş deneyimine sahip olursunuz. Ayrıca, Microsoft Graph erişmek istiyorsanız, gerekli belirteçleri oluşturmak için OAuth 2.0 aracılığıyla kimlik doğrulaması uygulamak gerekir. 

### <a name="implementation"></a>Uygulama

Microsoft kitaplıkları için SAML uygulaması sağlayın veya belirli kitaplıkları önerilir. Birçok açık kaynak kitaplıkları kullanılabilir.

## <a name="sso-and-using-microsoft-graph-rest-api"></a>SSO ve kullanarak Microsoft Graph Rest API'si 

Microsoft Graph, Microsoft 365, Office 365, Windows 10 ve Enterprise Mobility ve Security ve Dynamics 365 gibi ek ürünler dahil olmak üzere tüm veri yapısı olur. Bu kullanıcılar, gruplar, takvim, posta, dosyaları ve daha fazla söz konusu kullanıcı üretkenliği gibi varlıkları core şemalarını içerir. Microsoft Graph, geliştiricilerin bir REST tabanlı API, Microsoft Graph veri bağlama ve bağlayıcılar geliştiriciler izin için üç arabirimi sunar Microsoft Graph'a kendi verilerini eklemek için.  

SSO için yukarıdaki protokolden herhangi birini kullanarak, uygulamanızın Microsoft Graph REST API üzerinden mevcut zengin verilere erişim sağlar. Bu, müşterilerinizin Microsoft 365'te planlamalarına daha fazla değer elde sağlar. Örneğin, uygulamanızın, müşterilerinizin Office 365 örneği ve yüzey kullanıcıların Microsoft Office ve SharePoint uygulamanızın içinde öğeler tümleştirmek için Microsoft Graph API'si çağırabilirsiniz. 

OAuth2 kullanacağından geliştirme deneyiminizi sorunsuz ise Open ID Connect kimlik doğrulaması için kullanıyorsanız, temel açık belirteçlerini almak için kimliği Connect, Microsoft Graph API'leri çağırmak için kullanılabilir. Uygulamanız SAML veya WSFed kullanıyorsa, Microsoft Graph API'leri çağırmak için gereken belirteçlerini almak için bu OAuth2 almak için uygulamanızda ek kod eklemeniz gerekir. 

## <a name="next-steps"></a>Sonraki Adımlar

[Çok kiracılı uygulamanız için SSO'yu etkinleştirin](isv-sso-content.md)

[Çok kiracılı uygulamanızın belgeleri oluşturma](isv-create-sso-documentation.md)
