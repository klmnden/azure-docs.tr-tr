---
title: Azure Active Directory B2C LinkedIn yapılandırmasında | Microsoft Docs
description: Uygulamalarınızda Azure Active Directory B2C ile güvenliği sağlanan LinkedIn hesaplarıyla tüketicilere kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7588711bd1c2a02e2e9a100d2ba182f43e7df488
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37446116"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-linkedin-accounts"></a>Azure Active Directory B2C: Kaydolma ve oturum açma tüketicilere LinkedIn hesapları sağlayın
## <a name="create-a-linkedin-application"></a>Bir LinkedIn uygulaması oluşturun
LinkedIn bir Azure Active Directory (Azure AD) B2C kimlik sağlayıcısı olarak kullanmak için bir LinkedIn uygulama oluşturabilir ve doğru parametreleri sağlamanız gerekir. Bunu yapmak için bir LinkedIn hesabınız olması. Yoksa, adresinden edinebilirsiniz [ https://www.linkedin.com/ ](https://www.linkedin.com/).

1. Git [LinkedIn geliştiricilerin Web sitesi](https://www.developer.linkedin.com/) ve LinkedIn hesabı kimlik bilgilerinizle oturum açın.
2. Tıklayın **uygulamalarım** üst menü çubuğundaki ve ardından **uygulama oluşturma**.
   
    ![LinkedIn - yeni uygulama](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)
3. İçinde **yeni bir uygulama oluşturma** formunda, gerekli bilgileri doldurun (**şirket adı**, **adı**, **açıklama**, **Uygulama Logo URL'si**, **uygulama kullanımı**, **Web sitesi URL'si**, **iş e-posta** ve **İşTelefonu**).
4. Kabul **LinkedIn API kullanım** tıklatıp **Gönder**.
   
    ![LinkedIn - kayıt uygulaması](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)
5. Değerlerini kopyalamayı **istemci kimliği** ve **gizli**. (Bunları altında bulabilirsiniz **kimlik doğrulama anahtarlarını**.) Her ikisi de LinkedIn kiracınızdaki bir kimlik sağlayıcısı yapılandırmak için gerekir.
   
   > [!NOTE]
   > **İstemci gizli anahtarı** bir önemli güvenlik kimlik bilgisidir.
   > 
   > 
6. Girin `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` içinde **yeniden yönlendirme URL'lerini yetkili** alan (altında **OAuth 2.0**). Değiştirin **{tenant}** kiracınızın adı (örneğin, contoso.onmicrosoft.com) ile. Tıklayın **Ekle**ve ardından **güncelleştirme**. **{Tenant}** küçük olmalıdır.
   
    ![LinkedIn - Kurulum uygulama](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a>LinkedIn kiracınızdaki bir kimlik sağlayıcısı olarak yapılandırma
1. Bu adımları [B2C özellikleri dikey penceresine gitme](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalında.
2. B2C özellikleri dikey penceresinde tıklayın **kimlik sağlayıcıları**.
3. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
4. Kullanımı kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "LI" girin.
5. Tıklayın **kimlik sağlayıcısı türü**seçin **LinkedIn**, tıklatıp **Tamam**.
6. Tıklayın **bu kimlik sağlayıcısını ayarlama** istemci kimliği ve daha önce oluşturduğunuz LinkedIn uygulamasının istemci gizli anahtarını girin.
7. Tıklayın **Tamam** ve ardından **Oluştur** LinkedIn yapılandırmanızı kaydetmek için.

