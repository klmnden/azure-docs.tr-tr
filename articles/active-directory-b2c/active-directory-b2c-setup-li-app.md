---
title: Azure Active Directory B2C LinkedIn yapılandırmasında | Microsoft Docs
description: Uygulamalarınızda Azure Active Directory B2C tarafından güvenliği sağlanan LinkedIn hesaplarıyla tüketiciye kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 6db3832031a1bb960ee40c0e4fb8c3d0591a976c
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34711707"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-linkedin-accounts"></a>Azure Active Directory B2C: Kaydolma ve oturum açma LinkedIn hesaplarıyla tüketicileri sağlayın
## <a name="create-a-linkedin-application"></a>Bir LinkedIn uygulaması oluşturma
LinkedIn Azure Active Directory (Azure AD) B2C bir kimlik sağlayıcısı olarak kullanmak için bir LinkedIn uygulaması oluşturmak ve doğru parametrelerle sağlamanız gerekir. Bunu yapmak için bir LinkedIn hesabı gerekir. Bir sahip değilseniz, yerinde edinebilirsiniz [ https://www.linkedin.com/ ](https://www.linkedin.com/).

1. Git [LinkedIn geliştiricilerin Web sitesi](https://www.developer.linkedin.com/) ve LinkedIn hesabı kimlik bilgilerinizle oturum açın.
2. Tıklatın **My uygulamaları** üst menü çubuğu ve ardından **uygulama oluştur**.
   
    ![LinkedIn - yeni uygulama](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)
3. İçinde **yeni bir uygulama oluşturmak** formunda, ilgili bilgileri doldurun (**şirket adı**, **adı**, **açıklama**, **Uygulama Logo URL'si**, **uygulama kullanımı**, **Web sitesi URL'si**, **iş e-posta** ve **iş telefonu**).
4. Kabul **LinkedIn API Kullanım Koşulları'nı** tıklatıp **gönderme**.
   
    ![LinkedIn - kayıt uygulama](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)
5. Değerleri kopyalamak **istemci kimliği** ve **gizli**. (Bunları altında bulabilirsiniz **kimlik doğrulaması anahtarları**.) Her ikisi de kiracınızda kimlik sağlayıcısı LinkedIn yapılandırmak için gerekir.
   
   > [!NOTE]
   > **İstemci parolası** önemli güvenlik kimlik bilgileri.
   > 
   > 
6. Girin `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` içinde **yönlendirme URL'si yetkili** alan (altında **OAuth 2.0**). Değiştir **{tenant}** , kiracının adlı (örneğin, contoso.onmicrosoft.com). Tıklatın **Ekle**ve ardından **güncelleştirme**. **{Tenant}** değeri küçük olmalıdır.
   
    ![LinkedIn - Kurulum uygulama](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a>Kimlik sağlayıcısı kiracınızda LinkedIn yapılandırın
1. Aşağıdaki adımları izleyin [B2C özellikleri dikey penceresine gidin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalındaki.
2. B2C özellikleri dikey penceresinde **kimlik sağlayıcıları**.
3. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
4. Kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "LI" girin.
5. Tıklatın **kimlik sağlayıcısı türü**seçin **LinkedIn**, tıklatıp **Tamam**.
6. Tıklatın **bu kimlik sağlayıcısı'nı ayarlama** ve istemci Kimliğini ve daha önce oluşturduğunuz LinkedIn uygulamanın istemci parolasını girin.
7. Tıklatın **Tamam** ve ardından **oluşturma** LinkedIn yapılandırmanızı kaydetmek için.

