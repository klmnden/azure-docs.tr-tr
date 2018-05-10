---
title: Azure AD uygulama proxy'si ile uygulamaları için çoklu oturum açma | Microsoft Docs
description: Tek oturum açma için yayımlanan şirket içi uygulamalarınızı Azure portalında Azure AD uygulama proxy'si ile açın.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 3c69450601d84f62d05ca6cc8930fd8e9a8e4203
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="password-vaulting-for-single-sign-on-with-application-proxy"></a>Çoklu oturum açma için uygulama proxy'si ile vaulting parola

Azure Active Directory Uygulama proxy'si uzak çalışanlar güvenli bir şekilde bunları çok erişebilmesi için şirket içi uygulamaları yayımlama üretkenlik geliştirmenize yardımcı olur. Azure portalında, aynı zamanda çoklu oturum açma (SSO) bu uygulamalara ayarlayabilirsiniz. Kullanıcılarınız yalnızca Azure AD ile kimlik doğrulaması yapmanız ve yeniden oturum açmak zorunda kalmadan, Kurumsal uygulama erişebilir.

Uygulama proxy'si destekleyen birkaç [tek oturum açma modları](application-proxy-sso-overview.md). Parola tabanlı oturum açma kimlik doğrulaması için bir kullanıcı adı/parola bileşimi kullanan uygulamalar için tasarlanmıştır. Uygulamanız için parola tabanlı oturum açma yapılandırdığınızda şirket içi uygulamaya bir kez oturum açmak, kullanıcılarınızın sahip. Bundan sonra Azure Active Directory oturum açma bilgileri depolar ve kullanıcılarınızın, uzaktan erişim otomatik olarak uygulamaya sağlar. 

Zaten varsa yayımlanan ve uygulama proxy'si ile uygulamanızı test. Aksi takdirde, adımları [Azure AD uygulama proxy'si ile uygulama yayımlama](application-proxy-publish-azure-portal.md) tekrar buraya gelin. 

## <a name="set-up-password-vaulting-for-your-application"></a>Uygulamanız için vaulting parola ayarlama

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Seçin **Azure Active Directory** > **kurumsal uygulamalar** > **tüm uygulamaları**.
3. Listeden SSO'su ayarlamak istediğiniz uygulamayı seçin.  
4. Seçin **çoklu oturum açma**.

   ![Çoklu oturum açma seçin](./media/application-proxy-sso-azure-portal/select-sso.png)

5. SSO modu için **parola tabanlı oturum açma**.
6. Oturum açma URL'sini, burada kullanıcılar kullanıcı adı ve şirket ağı dışından uygulamanıza oturum açmak için parola girin, sayfa için URL'yi girin. Bu uygulama proxy'si aracılığıyla uygulama yayımlandığında oluşturduğunuz dış URL olabilir. 

   ![Parola tabanlı oturum açma seçin ve URL'nizi girin](./media/application-proxy-sso-azure-portal/password-sso.png)

7. **Kaydet**’i seçin.

<!-- Need to repro?
7. The page should tell you that a sign-in form was successfully detected at the provided URL. If it doesn't, select **Configure [your app name] Password Single Sign-on Settings** and choose **Manually detect sign-in fields**. Follow the instructions to point out where the sign-in credentials go. 
-->

## <a name="test-your-app"></a>Uygulamanızı test etme

Uygulamanız için uzaktan erişim için yapılandırılmış dış URL'ye gidin. Bu uygulama (veya erişimle ayarladığınız test hesabının kimlik bilgilerini) için kimlik bilgilerinizle oturum. Başarıyla oturum açtıktan sonra uygulamayı bırakın ve kimlik bilgilerinizi tekrar girmeden döndürülmesini yapabiliyor olmanız gerekir. 

## <a name="next-steps"></a>Sonraki adımlar

- Diğer yollarını uygulamak için okuma [uygulama proxy'si ile çoklu oturum açma](application-proxy-sso-overview.md)
- Hakkında bilgi edinin [uygulamaları Azure AD uygulama proxy'si ile uzaktan erişim için güvenlik konuları](application-proxy-security-considerations.md)