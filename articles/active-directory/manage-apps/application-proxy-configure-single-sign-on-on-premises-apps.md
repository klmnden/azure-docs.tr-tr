---
title: SAML çoklu oturum açma için Azure Active Directory Uygulama Proxy'si (Önizleme) ile şirket içi uygulamalar | Microsoft Docs
description: Şirket içi için çoklu oturum açma sağlamak öğrenin SAML kimlik doğrulaması ile güvenliği sağlanan uygulama proxy'si aracılığıyla yayımlanan uygulamaları.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: celested
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0a6f385cae99e5bb605b75f84e642e17e01d0f54
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57792896"
---
# <a name="saml-single-sign-on-for-on-premises-applications-with-application-proxy-preview"></a>SAML çoklu oturum açma için uygulama ara sunucusu (Önizleme) ile şirket içi uygulamalar

Şirket içi için çoklu oturum açma (SSO) sağlayabilir SAML kimlik doğrulaması ile güvenliği sağlanan uygulama proxy'si aracılığıyla yayımlanan uygulamaları. SAML çoklu oturum açma ile Azure Active Directory (Azure AD) kullanıcının Azure AD hesabı kullanarak uygulamaya kimliğini doğrular. Azure AD oturum açma bilgileri uygulamaya bir bağlantı protokolü üzerinden iletişim kurar. SAML tabanlı çoklu oturum açma ile kullanıcılar, SAML Taleplerde tanımladığınız kurallarına göre belirli uygulama rolleri eşleyebilirsiniz.

Uygulamalar tarafından verilen SAML belirteçlerini kullanamayabilir **Azure Active Directory**. Bu yapılandırma, bir şirket içi kimlik sağlayıcısı kullanan uygulamalar için geçerli değildir. Bu senaryolar için incelemeniz önerilir [kaynakları geçirmek için uygulamaların Azure AD'ye](migration-resources.md).

SAML SSO uygulama ara sunucusu ile SAML belirteci şifreleme özelliği ile de çalışır. Daha fazla bilgi için bkz. [yapılandırma Azure AD SAML belirteci şifreleme](howto-saml-token-encryption.md).

## <a name="publish-the-on-premises-application-with-application-proxy"></a>Uygulama Ara sunucusu ile şirket içi uygulama yayımlama

Şirket içi uygulamaları için SSO sağlamadan önce uygulama proxy'si etkinleştirdiyseniz ve yüklü bir Bağlayıcınız emin olun. Bkz: [Azure AD'de uygulama ara sunucusu üzerinden uzaktan erişim için şirket içi uygulama ekleme](application-proxy-add-on-premises-application.md) bilgi edinmek için nasıl.

Öğreticide geçerken aşağıdakileri göz önünde bulundurun:

* Öğreticide yönergelere göre uygulamanızı yayımlayın. Seçtiğinizden emin olun **Azure Active Directory** olarak **ön kimlik doğrulaması** yöntemi uygulamanız için (adım 4 [şirket içi bir uygulamayı Azure AD'ye ekleme](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad
)).
* Kopyalama **dış URL** uygulama için.
* En iyi uygulama, mümkün olduğunda özel etki alanları için bir en iyi duruma getirilmiş kullanıcı deneyimini kullanın. Daha fazla bilgi edinin [Azure AD uygulama proxy'sinde özel etki alanları ile çalışma](application-proxy-configure-custom-domain.md).
* Uygulamayı en az bir kullanıcı ekleyin ve test hesabının şirket içi uygulamaya erişimi olduğundan emin olun.

## <a name="set-up-saml-sso"></a>SAML SSO'yu ayarlama

1. Azure portalında **Azure Active Directory > Kurumsal uygulamalar** ve uygulamayı listeden seçin.
1. Uygulamanın gelen **genel bakış** sayfasında **çoklu oturum açma**.
1. Seçin **SAML** çoklu oturum açma yöntemi olarak.
1. İçinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme **temel SAML yapılandırma** veri ve adımları izleyerek [Enter temel SAML yapılandırma](configure-single-sign-on-non-gallery-applications.md#saml-based-single-sign-on) SAML tabanlı yapılandırmak için uygulama için kimlik doğrulaması.

    * Emin **yanıt URL'si** kök ile eşleşen veya bir yol altında **dış URL** Azure AD'de uygulama ara sunucusu üzerinden uzaktan erişim için eklediğiniz şirket içi uygulama için.

    ![Temel SAML yapılandırma verilerini girin](./media/application-proxy-configure-single-sign-on-on-premises-apps/basic-saml-configuration.png)

    > [!NOTE]
    > Arka uç uygulaması bekliyorsa **yanıt URL'si** İç URL olması için kullanıcıların cihazlarında oturum My Apps güvenli uzantıyı yüklemek gerekecektir. Bu uzantı için uygun uygulama ara Sunucusu hizmeti otomatik olarak yönlendirir. Uzantıyı yüklemek için bkz: [My Apps güvenli oturum açma uzantısı](../user-help/active-directory-saas-access-panel-introduction.md#my-apps-secure-sign-in-extension).

## <a name="test-your-app"></a>Uygulamanızı test etme

Tüm adımları tamamladıktan sonra uygulamanızı ve çalışıyor olması gerekir. Uygulamayı test etmek için:

1. Bir tarayıcı açın ve uygulama yayımlandığında oluşturduğunuz dış URL'ye gidin. 
1. Bir uygulamaya atanan test hesapla oturum açın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD uygulama proxy'si nasıl çoklu oturum açma sağlar?](application-proxy-single-sign-on.md)
- [Uygulama Ara sunucusu sorunlarını giderme](application-proxy-troubleshoot.md)
