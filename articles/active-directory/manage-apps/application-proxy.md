---
title: Şirket içi uygulamalara güvenli uzaktan erişim sağlama
description: Azure AD uygulama proxy'si, şirket içi uygulamalara güvenli uzaktan erişim sağlamak için nasıl kullanılacağı ele alınmaktadır.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/31/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: ec5c75b5de912988efeb5167107f6d0dfe07da2e
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53139971"
---
# <a name="how-to-provide-secure-remote-access-to-on-premises-applications"></a>Şirket içi uygulamalara güvenli uzaktan erişim sağlama

Günümüzde çalışanlar her yerden, her zaman ve tüm cihazlardan çalışmak istemektedir. Kendi cihazlarında iş satış noktası tabletleri, telefonlar veya dizüstü oluşmasından isterler. Ve tüm uygulamalar, her iki SaaS uygulamaları bulutta ve şirket içi Kurumsal uygulama erişebilmesi beklerler. Şirket içi uygulamalara erişmek için bugüne kadar sanal özel ağlar (VPN) veya temiz bölgeler (DMZ) kullanılıyordu. Bunlar yalnızca karmaşık ve güvenliği zor sağlanan çözümler değil aynı zamanda kurulumu ve yönetimi yüksek maliyetli seçeneklerdir.

Artık çok daha iyi bir seçenek var!

Modern bir iş gücü mobil öncelikli olarak bulut öncelikli dünyada bir modern uzaktan erişim çözümü gerekir. Azure AD uygulama proxy'si, uzaktan erişim hizmet olarak sunan Azure Active Directory özelliğidir. Bu, dağıtmak, kullanmak ve yönetmek kolay anlamına gelir.

[!INCLUDE [identity](../../../includes/azure-ad-licenses.md)]

## <a name="what-is-azure-active-directory-application-proxy"></a>Azure Active Directory Uygulama proxy'si nedir?
Azure AD uygulama ara sunucusu, çoklu oturum açma (SSO) sağlar ve şirket içi web uygulamaları için güvenli uzaktan erişim barındırılır. Bazı uygulamalar yayımlamak istediğiniz SharePoint siteleri, Outlook Web Access veya sahip olduğunuz tüm diğer LOB web uygulamaları içerir. Bu şirket içi web uygulamaları, kimlik ve denetim platformu, O365 tarafından kullanılan Azure AD ile tümleştirilir. Son kullanıcılar, şirket içi uygulamalarınızı, O365 ve Azure AD ile tümleştirilmiş diğer SaaS uygulamalarına eriştikleri aynı şekilde erişebilirsiniz. Ağ altyapısını değiştirmek veya VPN kullanıcılarınız için bu çözümü sunma konusunda gerekli gerekmez.

## <a name="why-is-application-proxy-a-better-solution"></a>Neden daha iyi bir çözüm uygulama proxy'si mi?
Azure AD uygulama ara sunucusu, tüm şirket içi uygulamalarınız için bir basit, güvenli ve uygun maliyetli bir uzaktan erişim çözümü sağlar.

Azure AD uygulama ara sunucusu aşağıdaki gibidir:

* **Basit**
   * Değiştirmeniz veya uygulamalarınızı uygulaması Ara sunucusu ile çalışacak şekilde güncelleştirmeniz gerekmez. 
   * Kullanıcılarınız, tutarlı bir kimlik doğrulama deneyimi alır. Her iki SaaS uygulamalarında Çoklu oturum açma Bulut ve şirket uygulamalarını almak için kullanıcılar MyApps portalında kullanabilirsiniz. 
* **Güvenlik**
   * Azure AD uygulama proxy'si kullanarak uygulamalarınızı yayımladığınızda, Azure'da zengin yetkilendirme denetimlerinden ve güvenlik analizinden yararlanarak alabilir. Bulut ölçeğinde güvenlik ve koşullu erişim ve iki aşamalı doğrulama gibi Azure güvenlik özelliklerine sahip olursunuz.
   * Herhangi bir gelen bağlantı aracılığıyla kullanıcılarınızın uzaktan erişim vermek için güvenlik duvarını açmak zorunda değilsiniz. 
* **Uygun maliyetli**
   * Uygulama proxy'si bulutta çalıştığı için zamandan ve paradan tasarruf edebilirsiniz. Şirket içi çözümler genellikle ayarlayıp DMZ'ler, kenar sunucu veya diğer karmaşık altyapı kurmanız bakımını gerektirir.  

## <a name="what-kind-of-applications-work-with-application-proxy"></a>Ne tür bir uygulama proxy'si ile uygulamaların çalışma?
Azure AD uygulama proxy'si ile iç uygulamaları farklı türde erişebilirsiniz:

* Web kullanan uygulamaları [tümleşik Windows kimlik doğrulaması](application-proxy-configure-single-sign-on-with-kcd.md) kimlik doğrulaması  
* Web form tabanlı kullanan uygulamalar veya [üst bilgi tabanlı](application-proxy-configure-single-sign-on-with-ping-access.md) erişim  
* Web için zengin uygulamalar farklı cihazlarda kullanıma sunmak istiyorsanız API'leri  
* Arkasında barındırılan uygulamalarını bir [Uzak Masaüstü Ağ Geçidi](application-proxy-integrate-with-remote-desktop-services.md)  
* Active Directory Authentication Library (ADAL) ile tümleşik olan zengin istemci uygulamaları

## <a name="how-does-application-proxy-work"></a>Uygulama proxy'si nasıl çalışır?
Uygulama proxy'si çalışır hale getirmek için yapılandırmanız gereken iki bileşeni vardır: bir bağlayıcı ve uç nokta. 

Bağlayıcı ağınızdaki bir Windows Server üzerinde yer alan bir basit aracısıdır. Uygulama proxy'si hizmeti bulutta trafik akışını, uygulama şirket içi bağlayıcı kolaylaştırır. Gelen bağlantı noktalarının açık veya her şeyi bir DMZ'ye yerleştirmeniz zorunda kalmamak için yalnızca giden bağlantılar kullanır. Bağlayıcılar, durum bilgisiz olduğundan ve gerektiği şekilde buluttan bilgi isteme. Nasıl gibi bağlayıcılar hakkında daha fazla bilgi için Yük Dengeleme ve kimlik doğrulaması için bkz [anlamak Azure AD uygulama ara sunucusu bağlayıcıları](application-proxy-connectors.md). 

Uç nokta bir URL olabilir veya bir [son kullanıcı portalı](end-user-experiences.md). Kullanıcılar, uygulamalar, Ağ üzerindeyken dışında bir dış URL erişerek ulaşabilirsiniz. Ağınızdaki kullanıcılar, bir URL veya bir son kullanıcı portalı uygulamaya erişebilir. Kullanıcılar bu uç noktaları birine gittiğinizde, Azure AD'de kimlik doğrulaması yapmak ve şirket içi uygulama Bağlayıcısı üzerinden yönlendirilir.

 ![AzureAD uygulama proxy'si diyagramı](./media/application-proxy/azureappproxxy.png)

1. Kullanıcı, bir uç noktası aracılığıyla uygulama eriştiğini sonra kullanıcının Azure AD oturum açma sayfasına yönlendirilir. 
2. Bir başarılı oturum açma işleminden sonra bir belirteç oluşturulur ve kullanıcının istemci cihaza gönderilir.
3. İstemci belirteci belirteçten güvenlik asıl adı (SPN) ve kullanıcı asıl adı (UPN) alır, sonra uygulama Proxy Bağlayıcısı isteği yönlendirir uygulama ara Sunucusu hizmetine gönderir.
4. Çoklu oturum açma yapılandırdıysanız, bağlayıcı kullanıcı adına gerekli tüm ek kimlik doğrulaması gerçekleştirir.
5. Bağlayıcıyı şirket içi uygulamaya isteği gönderir.  
6. Yanıt, kullanıcıya uygulama proxy'si hizmeti ve bağlayıcı gönderilir.

### <a name="single-sign-on"></a>Çoklu oturum açma
Azure AD uygulama proxy'si, tümleşik Windows kimlik doğrulaması (IWA) kullanan uygulamalar veya talep kullanan uygulamalar için çoklu oturum açma (SSO) sağlar. Uygulamanız IWA kullanıyorsa, uygulama proxy'si SSO sağlamak için Kerberos kısıtlanmış temsil kullanarak kullanıcının kimliğine bürünür. Azure Active Directory tarafından güvenilen bir talep kullanan bir uygulamanız varsa, kullanıcının Azure AD tarafından zaten kimlik doğrulaması için SSO çalışır.

Kerberos hakkında daha fazla bilgi için bkz: [tüm Kerberos Kısıtlı temsilci (KCD) hakkında bilmek istediğiniz](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/21/all-you-want-to-know-about-kerberos-constrained-delegation-kcd).

### <a name="managing-apps"></a>Uygulamaları yönetme
Uygulama Ara sunucusu ile uygulama yayınlandıktan sonra Azure portalında başka bir kuruluş uygulaması gibi yönetebilirsiniz. Azure Active Directory koşullu erişim ve iki aşamalı doğrulama gibi güvenlik özelliklerini kullanın, kullanıcının izinlerini denetlemek ve uygulamanız için markalamayı özelleştirme. 

## <a name="get-started"></a>başlarken

Uygulama proxy'si yapılandırmadan önce desteklenen bir olduğundan emin olun [Azure Active Directory sürüm](https://azure.microsoft.com/pricing/details/active-directory/) ve genel Yöneticisi olduğunuz bir Azure AD dizini.

Uygulama Ara sunucusu ile iki adımda kullanmaya başlayın:

1. [Uygulama Ara sunucusunu etkinleştirme ve bağlayıcısını](application-proxy-add-on-premises-application.md).    
2. [Uygulamaları yayımlama](application-proxy-add-on-premises-application.md) -yayımlanmış ve erişilebilen şirket içi uygulamalarınızı uzaktan almak için hızlı ve kolay Sihirbazı'nı kullanın.

## <a name="whats-next"></a>Sırada ne var?
İlk uygulamanızı yayımladıktan sonra çok uygulama ara sunucusu ile yapabileceğiniz daha fazlası vardır:

* [Çoklu oturum açmayı etkinleştirme](application-proxy-configure-single-sign-on-with-kcd.md)
* [Kendi etki alanı adınızı kullanarak uygulama yayımlama](application-proxy-configure-custom-domain.md)
* [Azure AD uygulama ara sunucusu bağlayıcıları hakkında bilgi edinin](application-proxy-connectors.md)
* [Mevcut şirket içi Proxy sunucuları ile çalışma](application-proxy-configure-connectors-with-proxy-servers.md) 
* [Özel bir ana sayfa olarak ayarla](application-proxy-configure-custom-home-page.md)

En yeni haberler ve güncelleştirmeler için [Uygulama Ara Sunucusu bloguna](https://blogs.technet.com/b/applicationproxyblog/) göz atın

