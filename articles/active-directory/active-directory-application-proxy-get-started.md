---
title: "Şirket içi uygulamalara güvenli uzaktan erişim sağlama"
description: "Azure AD uygulama proxy'si, şirket içi uygulamalara güvenli uzaktan erişim sağlamak için kullanmak üzere nasıl ele alınmaktadır."
services: active-directory
documentationcenter: 
author: kgremban
manager: mtillman
ms.assetid: d5450da1-9e06-4d08-8146-011c84922ab5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 3ca7c7919f6cfcece38073520162dc44bbfd748e
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-provide-secure-remote-access-to-on-premises-applications"></a>Şirket içi uygulamalara güvenli uzaktan erişim sağlama

Çalışanlar Bugün herhangi bir zamanda ve herhangi bir CİHAZDAN herhangi bir noktada üretken olmasını istediğiniz. Kendi cihazlarını üzerinde çalışmak tabletler, telefon veya dizüstü bilgisayarlar olmaları olup olmadığını isterler. Ve tüm uygulamaları, her iki SaaS uygulamaları bulutta ve şirket uygulamalarını şirket içi erişmek alabilmeyi beklemektedir. Şirket içi uygulamalara erişim sağlayarak geleneksel sanal özel ağlar (VPN) veya sivil bölge (DMZ'ler) dahil. Yalnızca bu çözümleri karmaşık ve güvenli hale getirmek sabit, ancak bunlar ayarlamak ve yönetmek maliyetli.

Daha iyi bir yolu yoktur!

Modern iş gücü mobil ilk bulut ilk world modern uzaktan erişim çözümü gerekir. Azure AD uygulama proxy'si, bir hizmet olarak uzaktan erişim sunar Azure Active Directory özelliğidir. Bu, dağıtma, kullanma ve yönetme kolayca anlamına gelir.

[!INCLUDE [identity](../../includes/azure-ad-licenses.md)]

## <a name="what-is-azure-active-directory-application-proxy"></a>Azure Active Directory Uygulama proxy'si nedir?
Çoklu oturum açma (SSO) Azure AD uygulama proxy'si sağlar ve şirket içi web uygulamaları için güvenli uzaktan erişim barındırılır. Yayımlamak istediğiniz bazı uygulamalar, SharePoint siteleri, Outlook Web Access veya sahip olduğunuz diğer iş KOLU web uygulamaları içerir. Bu şirket içi web uygulamaları Azure AD, aynı kimlik ve O365 tarafından kullanılan denetim platform ile tümleşiktir. Son kullanıcılar, şirket içi uygulamalarınızı O365 ve Azure AD ile tümleşik diğer SaaS uygulamaları eriştiklerinde aynı şekilde erişebilirsiniz. Ağ altyapısı değiştirmek veya kullanıcılarınız için bu çözümü sağlamak amacıyla VPN gerektiren gerek yoktur.

## <a name="why-is-application-proxy-a-better-solution"></a>Neden daha iyi bir çözüm uygulama proxy'si mi?
Azure AD uygulama proxy'si tüm şirket içi uygulamalara basit, güvenli ve uygun maliyetli uzaktan erişim çözümü sağlar.

Azure AD uygulama proxy'si şöyledir:

* **Basit**
   * Değiştirme veya uygulama proxy'si ile çalışmak için uygulamalarınızı güncelleştirmeyi gerek yoktur. 
   * Kullanıcılarınız tutarlı kimlik doğrulaması deneyimi alır. Çoklu oturum açma hem SaaS uygulamaları için bulut ve uygulamaları şirket içi almak için bunlar MyApps Portalı'nı kullanabilirsiniz. 
* **Güvenlik**
   * Azure AD uygulama proxy'si kullanarak uygulamalarınızı yayımladığınızda, güvenlik analizi ve zengin yetkilendirme denetimleri Azure'da yararlanabilirsiniz. Bulut ölçekli güvenlik ve koşullu erişim ve iki aşamalı doğrulama gibi Azure güvenlik özellikleri alır.
   * Kullanıcılarınızın uzaktan erişim vermek için güvenlik duvarı üzerinden gelen tüm bağlantıları'nı açmak zorunda değilsiniz. 
* **Uygun maliyetli**
   * Saat ve para kaydedebilmeniz için uygulama proxy'si bulutta çalışır. Şirket içi çözümler genellikle ayarlamak ve DMZ'ler, uç sunucuların veya diğer karmaşık altyapılar sürdürmek gerektirir.  

## <a name="what-kind-of-applications-work-with-application-proxy"></a>Ne tür bir uygulama proxy'si ile uygulama iş?
Azure AD uygulama proxy'si ile dahili uygulama farklı türlerine erişebilir:

* Web kullanan uygulamalar [tümleşik Windows kimlik doğrulaması](active-directory-application-proxy-sso-using-kcd.md) kimlik doğrulaması  
* Web form tabanlı kullanan uygulamalar veya [başlığa göre](application-proxy-ping-access.md) erişim  
* Web farklı cihazlarda zengin uygulamalar için kullanıma sunmak istediğiniz API'leri  
* Barındırılan uygulamalara arkasındaki bir [Uzak Masaüstü Ağ Geçidi](application-proxy-publish-remote-desktop.md)  
* Active Directory Authentication Library (ADAL) ile tümleşik zengin istemci uygulamaları

## <a name="how-does-application-proxy-work"></a>Uygulama proxy'si nasıl çalışır?
Uygulama proxy'si iş yapmak için yapılandırmanız gereken iki bileşeni vardır: bir bağlayıcı ve dış uç noktası. 

Ağınızdaki Windows Server'da bulunur basit bir aracı Bağlayıcıdır. Bağlayıcı uygulama şirket içi bulut uygulama proxy'si hizmetinden trafik akışına kolaylaştırır. Gelen bağlantı noktalarının açık veya hiçbir şey DMZ'deki put zorunda kalmamak için yalnızca giden bağlantılar kullanır. Bağlayıcılar, durum bilgisiz ve gerekirse bulut bilgi isteneceğini. Nasıl gibi bağlayıcılar hakkında daha fazla bilgi için Yük Dengeleme ve kimlik doğrulaması, bkz [anlamak Azure AD uygulama proxy'si Bağlayıcılar](application-proxy-understand-connectors.md). 

Kullanıcılarınızın uygulamalarınızı sırasında ağ dışında nasıl ulaşmak dış uç noktadır. Belirlediğiniz doğrudan bir dış URL için ya da gidebilirsiniz veya MyApps Portalı aracılığıyla uygulamaya erişebilir. Kullanıcılar bu uç noktalar birine gidin, Azure AD içinde kimlik doğrulaması ve daha sonra şirket içi uygulama bağlayıcı aracılığıyla yönlendirilir.

 ![Azuread'i uygulama proxy'si diyagramı](./media/active-directory-application-proxy-get-started/azureappproxxy.png)

1. Kullanıcı uygulama proxy'si hizmeti aracılığıyla uygulama erişir ve kimlik doğrulamak için Azure AD oturum açma sayfasına yönlendirilir.
2. Bir başarılı oturum açma işleminden sonra bir belirteç oluşturulur ve istemci cihaza gönderilir.
3. İstemci belirteç belirteçten kullanıcı asıl adı (UPN) ve güvenlik asıl adı (SPN) alır, sonra uygulama ara sunucusu Bağlayıcısı isteği yönlendirir uygulama proxy'si hizmetine gönderir.
4. Çoklu oturum açma yapılandırdıysanız, bağlayıcı kullanıcı adına gerekli ek kimlik doğrulaması gerçekleştirir.
5. Bağlayıcıyı şirket içi uygulama isteği gönderir.  
6. Yanıt, kullanıcıya uygulama proxy'si hizmeti ve bağlayıcı gönderilir.

### <a name="single-sign-on"></a>Çoklu oturum açma
Azure AD uygulama proxy'si, tümleşik Windows kimlik doğrulaması (IWA) kullanan uygulamalar veya talep kullanan uygulamalar için çoklu oturum açma (SSO) sağlar. Uygulamanızı IWA kullanıyorsa, uygulama proxy'si SSO sağlamak için Kerberos Kısıtlı temsilci kullanarak kullanıcı temsil eder. Azure Active Directory güvenleri talep kullanan bir uygulamanız varsa, kullanıcının Azure AD tarafından zaten doğrulandı çünkü SSO çalışır.

Kerberos hakkında daha fazla bilgi için bkz: [tüm Kerberos Kısıtlı temsilci (KCD) hakkında bilmek istediğiniz](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/21/all-you-want-to-know-about-kerberos-constrained-delegation-kcd).

### <a name="managing-apps"></a>Uygulamaları yönetme
Uygulama proxy'si ile uygulamanızı yayımladıktan sonra Azure portalındaki diğer kuruluş uygulaması gibi yönetebilirsiniz. Koşullu erişim ve iki aşamalı doğrulama gibi Azure Active Directory güvenlik özellikleri kullanmak, kullanıcı izinlerini denetlemek ve uygulamanız için marka özelleştirme. 

## <a name="get-started"></a>başlarken

Uygulama proxy'si yapılandırmadan önce desteklenen bir olduğundan emin olun [Azure Active Directory edition](https://azure.microsoft.com/pricing/details/active-directory/) ve genel Yöneticisi olduğunuz bir Azure AD dizini.

Uygulama proxy'si ile iki adımda başlayın:

1. [Uygulama Ara sunucusunu etkinleştirme ve bağlayıcısını](active-directory-application-proxy-enable.md).    
2. [Uygulama yayımlama](active-directory-application-proxy-publish.md) -yayımlanan ve erişilebilir, şirket içi uygulamalarınızı uzaktan almak için hızlı ve kolay Sihirbazı'nı kullanın.

## <a name="whats-next"></a>Sırada ne var?
İlk uygulamanızı yayımladıktan sonra çok daha uygulama proxy'si ile yapabileceğiniz vardır:

* [Çoklu oturum açmayı etkinleştirme](active-directory-application-proxy-sso-using-kcd.md)
* [Kendi etki alanı adınızı kullanarak uygulama yayımlama](active-directory-application-proxy-custom-domains.md)
* [Azure AD uygulama proxy'si bağlayıcılar hakkında bilgi edinin](application-proxy-understand-connectors.md)
* [Mevcut şirket içi Proxy sunucuları ile çalışma](application-proxy-working-with-proxy-servers.md) 
* [Özel bir ana sayfa ayarlama](application-proxy-office365-app-launcher.md)

En yeni haberler ve güncelleştirmeler için [Uygulama Ara Sunucusu bloguna](http://blogs.technet.com/b/applicationproxyblog/) göz atın

