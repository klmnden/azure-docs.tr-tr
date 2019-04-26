---
title: Uygulamalar - Azure Active Directory çoklu oturum açma | Microsoft Docs
description: Uygulamaları Azure Active Directory (Azure AD) yapılandırma tek bir oturum açma yöntemi seçin öğrenin. Böylece kullanıcılar her uygulama için hatırlamak ve hesap yönetimi yönetimini basitleştirmek için çoklu oturum açma kullanın.
services: active-directory
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: celested
ms.reviewer: arvindh, japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 75aa0f4755fe3d124094ace3c3e6b8e6ea3b65e0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60441521"
---
# <a name="single-sign-on-to-applications-in-azure-active-directory"></a>Azure Active Directory'de uygulamalar için çoklu oturum açma

Kullanıcılar uygulamalara, Azure Active Directory'de (Azure AD) oturum çoklu oturum açma (SSO) güvenlik ve kolaylık ekler. Bu makalede tek oturum açma yöntemleri açıklar ve uygulamalarınızı yapılandırırken en uygun SSO yöntemi seçmenize yardımcı olur.

- **Çoklu oturum açma ile**, kullanıcıların etki alanına katılmış cihazlara erişmek için bir hesap ile bir kez oturum şirket kaynakları, yazılım olarak hizmet (SaaS) uygulamaları ve web uygulamaları. Kullanıcı, oturum açtıktan sonra Office 365 portalında veya Azure AD MyApps erişim paneli uygulamaları başlatabilir. Yöneticiler kullanıcı hesabı yönetimini merkezden gerçekleştirin ve otomatik olarak ekleyebilir veya grup üyeliğine dayalı uygulamalara kullanıcı erişimini kaldırabilirsiniz.

- **Çoklu oturum açma olmadan**, kullanıcılar uygulamaya özgü hatırlamak ve her uygulama için oturum açın. Office 365 kutusu ve Salesforce gibi her bir uygulama için kullanıcı hesaplarını oluşturmak için BT personeli gerekir. Kullanıcıların parolalarını unutmayın yanı sıra her bir uygulama için oturum açmak için kullanacağınız gerekir.

## <a name="choosing-a-single-sign-on-method"></a>Tek bir oturum açma yöntemi seçme

Bir uygulama için çoklu oturum açmayı yapılandırmak için birkaç yolu vardır. Tek bir oturum açma yöntemini seçme uygulama kimlik doğrulaması için nasıl yapılandırıldığına bağlıdır.

- Bulut uygulamaları için çoklu oturum açmayı Openıd Connect, OAuth, SAML, parola tabanlı, bağlantılı veya devre dışı yöntemler kullanabilirsiniz. 
- Şirket içi uygulamaları, parola tabanlı, tümleşik Windows kimlik doğrulaması, çoklu oturum açma için üst bilgi tabanlı, bağlantılı veya devre dışı yöntemleri kullanabilir. Uygulamalar, uygulama proxy'si için yapılandırıldığında, şirket içi seçenekler çalışır.

Bu akış, hangi çoklu oturum açma yöntemi, sizin durumunuz için en iyi olduğuna karar vermenize yardımcı olur.

![Çoklu oturum açma yöntemi seçin](./media/what-is-single-sign-on/choose-single-sign-on-method-040419.png)

Aşağıdaki tabloda, tek oturum açma yöntemleri özetler ve daha fazla ayrıntı için bağlantılar.

| Çoklu oturum açma yöntemi | Uygulama türleri | Kullanılması gereken durumlar |
| :------ | :------- | :----- |
| [Openıd Connect ve OAuth](#openid-connect-and-oauth) | Yalnızca bulut | Openıd Connect ve OAuth yeni bir uygulama geliştirirken kullanın. Bu protokol, uygulama yapılandırmasını basitleştiren, kullanımı kolay SDK'lar sahip ve MS Graph kullanmak için uygulamanızı sağlar.
| [SAML](#saml-sso) | Bulut ve şirket içi | Mümkün olduğunda, Openıd Connect veya OAuth kullanan değil mevcut uygulamalar için SAML seçin. SAML SAML protokollerden birini kullanarak kimlik doğrulaması uygulamaları için çalışır.|
| [Parola tabanlı](#password-based-sso) | Bulut ve şirket içi | Uygulama, kullanıcı adı ve parola ile kimlik doğrulamasını gerçekleştirdiğinde parola tabanlı seçin. Parola tabanlı çoklu oturum açma güvenli uygulama parola depolama ve bir web tarayıcısı uzantısı veya mobil uygulama kullanarak yeniden yürütme sağlar. Bu yöntem, uygulama tarafından sağlanan mevcut oturum açma işlemi kullanır, ancak yönetici parolaları yönetmek etkinleştirir. |
| [Bağlı](#linked-sso) | Bulut ve şirket içi | Uygulama için çoklu oturum açmayı başka bir kimlik sağlayıcı hizmetine de yapılandırıldığında bağlı çoklu oturum açma seçin. Bu seçenek varsayılan olarak, uygulamayı çoklu oturum açma eklemez. Ancak, uygulamanın tek Active Directory Federasyon Hizmetleri gibi başka bir hizmet kullanılarak uygulanan oturum zaten olabilir.|
| [Devre dışı](#disabled-sso) | Bulut ve şirket içi | Uygulama için çoklu oturum açmayı yapılandırılmaya hazır değilken, devre dışı çoklu oturum açma seçin. Kullanıcıların, bunlar bu uygulamayı başlatmak her seferinde kullanıcı adı ve parola girmeniz gerekir.|
| [Tümleşik Windows kimlik doğrulaması (IWA)](#integrated-windows-authentication-iwa-sso) | yalnızca şirket içi | IWA çoklu oturum açma kullanan uygulamalar için seçin [tümleşik Windows kimlik doğrulaması (IWA)](/aspnet/web-api/overview/security/integrated-windows-authentication), veya talep kullanan uygulamalar. IWA için uygulama kullanıcıların kimliğini doğrulamak için Kerberos Kısıtlı temsilci (KCD) uygulama ara sunucusu bağlayıcıları kullanın. | 
| [Üst bilgi tabanlı](#header-based-sso) | yalnızca şirket içi | Uygulama için kimlik doğrulama üst bilgileri kullandığında üst bilgi tabanlı çoklu oturum açma kullanın. Üst bilgi tabanlı çoklu oturum açma Azure AD için PingAccess gerektirir. Uygulama Ara sunucusu kullanıcının kimliğini doğrulamak için Azure AD kullanır ve ardından bağlayıcı hizmetini üzerinden geçen trafik geçirir.  | 

## <a name="openid-connect-and-oauth"></a>Openıd Connect ve OAuth
Yeni uygulamaları geliştirirken en iyi çoklu oturum açma deneyimini uygulamanız için birden çok cihaz platformları arasında elde etmek için Openıd Connect ve OAuth gibi modern protokolleri kullanın. Kullanıcıları veya yöneticileri için OAuth sağlayan [onay verme](configure-user-consent.md) ister korumalı kaynaklar için [MS Graph](/graph/overview). Kolay benimsemek için sağladığımız [SDK'ları](../develop/reference-v2-libraries.md) uygulamanız için ve ayrıca, uygulamanızı kullanıma hazır [MS Graph](/graph/overview).

Daha fazla bilgi için bkz.

- [OAuth 2.0](../develop/v2-oauth2-auth-code-flow.md)
- [OpenID Connect 1.0](../develop/v2-protocols-oidc.md)
- [Azure Active Directory Geliştirici Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide).

## <a name="saml-sso"></a>SAML SSO
İle **SAML çoklu oturum açma**, Azure AD, kullanıcının Azure AD hesabı kullanarak uygulamaya kimliğini doğrular. Azure AD oturum açma bilgileri uygulamaya bir bağlantı protokolü üzerinden iletişim kurar. SAML tabanlı çoklu oturum açma ile kullanıcılar, SAML Taleplerde tanımladığınız kurallarına göre belirli uygulama rolleri eşleyebilirsiniz.

Uygulama destekliyorsa SAML tabanlı çoklu oturum açma seçin.


SAML tabanlı çoklu oturum açma şu protokolden herhangi birini kullanan uygulamalar için desteklenir:

- SAML 2.0
- WS-Federation

Bir SaaS uygulaması SAML tabanlı çoklu oturum açma için yapılandırmak üzere bkz [yapılandırma SAML tabanlı çoklu oturum açma](configure-single-sign-on-portal.md). Ayrıca, bir hizmet (SaaS) uygulamaları olarak çok sayıda yazılım sahip bir [uygulamaya özgü öğretici](../saas-apps/tutorial-list.md) Bu adım, SAML tabanlı çoklu oturum açma için yapılandırma.

WS-Federasyon için bir uygulamayı yapılandırmak için uygulamanın SAML tabanlı çoklu oturum açma için yapılandırmak için bkz aynı yönergeleri [yapılandırma SAML tabanlı çoklu oturum açma](configure-single-sign-on-portal.md). Adım uygulamayı Azure AD'ye kullanacak şekilde yapılandırmak için WS-Federasyon uç nokta için Azure AD oturum açma URL'sini değiştirmeniz gerekecektir `https://login.microsoftonline.com/<tenant-ID>/wsfed`.

SAML tabanlı çoklu oturum açma için şirket içi bir uygulamayı yapılandırmak için bkz [SAML çoklu oturum açma için şirket içi uygulama ara sunucusu ile](application-proxy-configure-single-sign-on-on-premises-apps.md).

SAML protokolü hakkında daha fazla bilgi için bkz. [çoklu oturum açma SAML Protokolü](../develop/single-sign-on-saml-protocol.md).

## <a name="password-based-sso"></a>Parola tabanlı SSO
Parola tabanlı oturum açma ile kullanıcılar uygulamada bir kullanıcı adı ve parola ile bunların erişim ilk kez oturum. İlk oturum açma işleminden sonra Azure AD kullanıcı ve uygulama parolasını sağlar. 

Parola tabanlı çoklu oturum açma uygulama tarafından sağlanan mevcut kimlik doğrulama işlemi kullanır. Parola çoklu oturum açma bir uygulama için etkinleştirdiğinizde, Azure AD toplar ve kullanıcı adları ve parolalar uygulama için güvenli bir şekilde depolar. Kullanıcı kimlik bilgilerini şifrelenmiş bir duruma dizininde depolanır. 

Parola tabanlı tek seçin ne zaman oturum:

- Uygulamanın, oturum açma SAML çoklu Protokolü desteklemiyor.
- Bir uygulama adı ve parola yerine erişim belirteçleri ve üst bilgileri ile kimlik doğrulaması yapar.

Parola tabanlı çoklu oturum açma bir HTML tabanlı oturum açma sayfası olan tüm bulut tabanlı uygulama için desteklenir. Kullanıcı aşağıdaki tarayıcılardan herhangi birini kullanabilirsiniz:

- Internet Explorer 11 Windows 7 veya üzeri
- Windows 10 Anniversary Edition veya sonrası, Microsoft Edge
- Chrome Windows 7 ve daha sonra ve MacOS x veya sonrası
- Firefox 26,0 veya daha sonra Windows XP SP2 veya üzeri ve Mac OS X 10.6 üzerinde veya üzeri

Bir bulut uygulama parola tabanlı çoklu oturum açma için yapılandırmak üzere bkz [parola çoklu oturum açma için uygulamayı yapılandırma](application-sign-in-problem-password-sso-gallery.md#configure-the-application-for-password-single-sign-on).

Bir şirket içi uygulama için uygulama proxy'si aracılığıyla çoklu oturum açmayı yapılandırmak için bkz [vaulting uygulama proxy'si ile çoklu oturum açma için parola](application-proxy-configure-single-sign-on-password-vaulting.md)

### <a name="how-authentication-works-for-password-based-sso"></a>Kimlik doğrulaması için parola tabanlı SSO nasıl çalışır?

Bir uygulama bir kullanıcının kimliğini doğrulamak için Azure AD dizininden kullanıcının kimlik bilgilerini alır ve bunları uygulamanın oturum açma sayfasına girer.  Azure AD kullanıcı kimlik bilgilerini güvenli bir şekilde bir web tarayıcısı uzantısı veya mobil uygulama geçirir. Bu işlem, bir yönetici kullanıcı kimlik bilgilerini yönetmek etkinleştirir ve kullanıcıların parolalarını unutmayın gerektirmez.

> [!IMPORTANT]
> Kimlik bilgilerini otomatik oturum açma işlemi sırasında kullanıcıdan gizlenmiş olan. Ancak, kimlik bilgileri web hata ayıklama araçlarını kullanarak bulunabilir. Kullanıcıların ve yöneticilerin kimlik bilgilerini doğrudan kullanıcı tarafından girildiği gibi aynı güvenlik ilkeleri izlemeniz gerekir.

### <a name="managing-credentials-for-password-based-sso"></a>Parola tabanlı SSO için kimlik bilgilerini yönetme

Her uygulamanın ya da Azure AD Yöneticisi veya kullanıcılar tarafından yönetilebilir.

Ne zaman Azure AD Yöneticisi kimlik bilgilerini yönetir:  

- Kullanıcı sıfırlama veya kullanıcı adınızı ve parolanızı hatırlayacağınızdan gerekmez. Kullanıcı kendi erişim panellerine veya sağlanan bağlantı yoluyla tıklayarak uygulamaya erişebilir.
- Yönetici kimlik bilgileri yönetim görevlerini gerçekleştirebilirsiniz. Örneğin, yönetici kullanıcı grup üyeliklerini ve çalışan durumuna göre uygulama erişimi güncelleştirebilirsiniz.
- Yönetici, çok sayıda kullanıcı arasında paylaşılan uygulamalara erişim sağlamak için yönetici kimlik bilgilerini kullanabilirsiniz. Örneğin, yönetici bir sosyal medya veya belge paylaşımı uygulama erişimi için bir uygulama erişebilen herkesin izin verebilirsiniz.

Ne zaman son kullanıcı kimlik bilgilerini yönetir:

- Kullanıcılar parolalarını güncelleştirmek ya da gerektiği şekilde silerek yönetebilir. 
- Yöneticiler uygulama için yeni kimlik bilgilerini ayarlamak hala mümkün.


## <a name="linked-sso"></a>Bağlantılı SSO
Bağlantılı oturum açma için çoklu oturum açmayı başka bir hizmet olarak zaten yapılandırılmış bir uygulama için çoklu oturum açma sağlamak için Azure AD'deki sağlar. Bağlı uygulama Office 365 portalında veya Azure AD MyApps portalında son kullanıcılara görünür. Örneğin, bir kullanıcı çoklu oturum açma Active Directory Federasyon Hizmetleri 2.0 (AD FS), Office 365 portalından için yapılandırılmış bir uygulama başlatabilirsiniz. Ek raporlama da Office 365 portalında veya Azure AD MyApps portalından başlatılan bağlantılı uygulamalar için kullanılabilir. 

### <a name="linked-sso-for-application-migration"></a>Uygulama geçiş için bağlantılı SSO

Uygulamaları bir süre geçirirken bağlı SSO tutarlı bir kullanıcı deneyimi sağlar. Uygulamaları Azure Active Directory'ye geçiriyorsanız, bağlantılar, geçirmek istediğiniz tüm uygulamalar için hızlı bir şekilde yayımlamak için bağlantılı çoklu oturum açma kullanabilirsiniz.  Kullanıcılar, tüm bağlantıları bulabilirsiniz [MyApps portalında](../user-help/active-directory-saas-access-panel-introduction.md) veya [Office 365 uygulama başlatıcısında](https://support.office.com/article/meet-the-office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a). Kullanıcılar, bağlı bir uygulama veya geçirilmiş bir uygulamayı eriştiğiniz bilemezsiniz.  

Bir kullanıcı ile bağlantılı bir uygulamaya kimlik doğrulaması yapıldıktan sonra hesap kaydı son kullanıcı çoklu oturum açma erişimi sağlanan önce oluşturulması gerekir. Bu hesap kaydı sağlama ya da otomatik olarak gerçekleşebileceği ya da bir yönetici tarafından el ile ortaya çıkabilir.

## <a name="disabled-sso"></a>Devre dışı SSO

Devre dışı moda uygulama için çoklu oturum açma kullanılmayan anlamına gelir. Çoklu oturum açma devre dışı bırakıldığında, kullanıcıların iki kez kimlik doğrulaması gerekebilir. İlk olarak, Azure AD ile kullanıcıların kimliğini doğrulamak ve bunlar uygulamaya oturum açın. 

Çoklu oturum açma modunu devre dışı:

- Bu uygulamayı Azure AD çoklu oturum açma ile tümleştirmeye hazır değilseniz veya
- Uygulamanın diğer yönleri test ediyorsanız veya
- Bir şirket içi uygulamaya güvenlik katmanı olarak, kullanıcıların kimlik doğrulaması yapmasına gerek yoktur. Kullanıcı kimlik doğrulaması yapması, devre dışı. 

## <a name="integrated-windows-authentication-iwa-sso"></a>Tümleşik Windows kimlik doğrulaması (IWA) SSO

[Uygulama proxy'si](application-proxy.md) çoklu oturum açma (SSO) kullanan uygulamalar sağlar [tümleşik Windows kimlik doğrulaması (IWA)](/aspnet/web-api/overview/security/integrated-windows-authentication), veya talep kullanan uygulamalar. Uygulamanız IWA kullanıyorsa, uygulama proxy'si uygulamaya, Kerberos Kısıtlı temsilci (KCD) kullanarak kimliğini doğrular. Kullanıcı zaten Azure AD'yi kullanarak kimliğinin çünkü Azure Active Directory tarafından güvenilen bir talep kullanan uygulama için çoklu oturum açma çalışır.

Oturum açma tümleşik Windows kimlik doğrulaması tek modunu seçin:

- Çoklu oturum açma kimliğini doğrulayan bir şirket içi uygulamaya IWA ile sağlamak için. 

Şirket içi uygulama IWA için yapılandırmak üzere bkz [uygulamalarınıza uygulama proxy'si ile çoklu oturum açma için Kerberos kısıtlanmış temsil](application-proxy-configure-single-sign-on-with-kcd.md). 

### <a name="how-single-sign-on-with-kcd-works"></a>KCD works ile nasıl çoklu oturum açma
Bir kullanıcı IWA kullanan şirket içi uygulamaya eriştiğinde, bu diyagramda akışı açıklanır.

![Microsoft AAD kimlik doğrulaması akışı diyagramı](./media/application-proxy-configure-single-sign-on-with-kcd/AuthDiagram.png)

1. Kullanıcı, uygulama proxy'si aracılığıyla şirket içi uygulama erişimi için URL'yi girer.
2. Uygulama Ara sunucusu için Azure AD kimlik doğrulama hizmetleri preauthenticate için istek yönlendirir. Bu noktada, Azure AD, tüm geçerli kimlik doğrulama ve yetkilendirme ilkeleri, çok faktörlü kimlik doğrulaması gibi uygulanır. Kullanıcı doğrulanmış olması durumunda, Azure AD belirteç oluşturulur ve kullanıcıya gönderir.
3. Kullanıcı belirteci uygulama ara sunucusuna iletir.
4. Uygulama proxy'si, belirteci doğrular ve belirteçten kullanıcı asıl adı (UPN) alır. Bunu daha sonra istek, UPN ve hizmet asıl adı (SPN) bağlayıcı dually kimliği doğrulanmış ve güvenli bir kanal üzerinden gönderir.
5. Bağlayıcı, şirket içi AD, uygulama için bir Kerberos belirteci almak için kullanıcının kimliğine bürünmek ile Kerberos Kısıtlı temsilci (KCD) anlaşma kullanır.
6. Active Directory Uygulama bağlayıcısı için Kerberos belirteci gönderir.
7. Bağlayıcı AD'den alınan Kerberos belirteci kullanarak uygulama sunucusu, özgün istek gönderir.
8. Uygulama için uygulama proxy'si hizmeti ardından döndürülen bağlayıcı yanıta gönderir ve son kullanıcı.

## <a name="header-based-sso"></a>Üst bilgi tabanlı SSO

Üst bilgi tabanlı çoklu oturum açma, HTTP üst bilgileri için kimlik doğrulaması kullanan uygulamalar için çalışır. Bu oturum açma yöntemi PingAccess adlı bir üçüncü taraf kimlik doğrulama hizmeti kullanır. Bir kullanıcının yalnızca Azure AD'ye kimlik doğrulaması gerekir. 

Üst bilgi tabanlı tek seçin ne zaman oturum:

- Uygulama proxy'si ile PingAccess uygulama için yapılandırılmış

Üst bilgi tabanlı kimlik doğrulamasını yapılandırmak için bkz: [üst bilgi tabanlı kimlik doğrulaması için uygulama proxy'si ile çoklu oturum açma](application-proxy-configure-single-sign-on-with-ping-access.md). 

### <a name="what-is-pingaccess-for-azure-ad"></a>Azure AD için PingAccess nedir?

Azure AD, kullanıcıları için PingAccess kullanarak erişim ve tek kimlik doğrulaması için üst bilgi kullanan uygulamalar için oturum. Uygulama Ara sunucusu, bu uygulamaları diğer, Azure AD kimlik doğrulaması yapmak için kullanarak ve ardından bağlayıcı hizmetini üzerinden trafiği geçirerek gibi davranır. PingAccess hizmeti Azure AD erişim belirteci kimlik doğrulaması gerçekleştikten sonra uygulamaya gönderilen bir üstbilgi biçimine çevirir.

Kurumsal uygulamalarınıza kullanmak oturum açtığında, kullanıcılar farklı bir şey fark olmaz. Bunlar yine de her yerde her cihazda çalışabilirsiniz. Uygulama Ara sunucusu bağlayıcıları doğrudan tüm uygulamalara uzaktan trafiği ve otomatik olarak yük dengelemesi devam edeceğiz.

### <a name="how-do-i-get-a-license-for-pingaccess"></a>Bir lisans için PingAccess nasıl alabilirim?

Bu senaryo Azure AD arasındaki iş ortaklığı ile sunulan bu yana PingAccess, ihtiyacınız ve lisansları için her iki hizmet. Ancak, en fazla 20 uygulamaları kapsayan temel bir PingAccess lisansı Azure AD Premium abonelikleri içerir. 20'den fazla üst bilgi tabanlı uygulamaları yayımlamak gerekiyorsa, ek bir PingAccess lisanstan elde edebilirsiniz. 

Daha fazla bilgi için bkz. [Azure Active Directory sürümleri](../fundamentals/active-directory-whatis.md).


## <a name="related-articles"></a>İlgili makaleler
* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler](../saas-apps/tutorial-list.md)
* [Çoklu oturum açmayı yapılandırma Öğreticisi](configure-single-sign-on-portal.md)
* [Uygulamalara erişimi yönetme giriş](what-is-access-management.md)
* İndirme bağlantısı: [Çoklu oturum açma dağıtım planı](https://aka.ms/SSODeploymentPlan).


