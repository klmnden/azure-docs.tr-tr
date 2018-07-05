---
title: Azure Active Directory B2C Amazon yapılandırmasında | Microsoft Docs
description: Uygulamalarınızda Azure Active Directory B2C ile güvenliği sağlanan Amazon hesaplarıyla tüketicilere kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e3b3d66b913b595e68c03b68990d1a4806952579
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443719"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-amazon-accounts"></a>Azure Active Directory B2C: Kaydolma ve oturum açma tüketicilere Amazon hesapları sağlayın
## <a name="create-an-amazon-application"></a>Amazon uygulama oluşturma
Bir Azure Active Directory (Azure AD) B2C kimlik sağlayıcısı Amazon kullanmak için Amazon uygulama oluşturma ve doğru parametreleri sağlamanız gerekir. Bunu yapmak için bir Amazon hesabınız olmalıdır. Yoksa, adresinden edinebilirsiniz [ http://www.amazon.com/ ](http://www.amazon.com/).

1. Git [Amazon Geliştirici Merkezi](https://login.amazon.com/) ve Amazon hesabı kimlik bilgilerinizle oturum açın.
2. Zaten yapmadıysanız, tıklayın **Kaydol**, geliştirici kayıt adımları izleyin ve ilkeyi kabul edin.
3. Tıklayın **yeni uygulamayı kaydedin**.
   
    ![Amazon Web sitesinde yeni bir uygulamayı kaydetme](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)
4. Uygulama bilgilerini sağlayın (**adı**, **açıklama**, ve **gizlilik bildirimi URL'si**) tıklayıp **Kaydet**.
   
    ![Amazon yeni bir uygulamayı kaydetmek için uygulama bilgilerini sağlama](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)
5. İçinde **Web ayarları** bölümünde, değerlerini kopyalamayı **istemci kimliği** ve **gizli**. ('ye tıklamanız **Göster gizli** bu düğmeyi.) Her ikisi de kiracınızdaki bir kimlik sağlayıcısı Amazon yapılandırmak için gerekir. Tıklayın **Düzenle** bölümünün altındaki. **İstemci gizli anahtarı** bir önemli güvenlik kimlik bilgisidir.
   
    ![Amazon yeni uygulamanızın istemci kimliği ve istemci gizli anahtarını sağlama](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)
6. Girin `https://login.microsoftonline.com` içinde **izin JavaScript kaynakları** alan ve `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` içinde **dönüş URL'leri izin** alan. Değiştirin **{tenant}** kiracınızın adı (örneğin, contoso.onmicrosoft.com) ile. **Kaydet**’e tıklayın. **{Tenant}** duyarlıdır.
   
    ![Yeni uygulamanızı Amazon için JavaScript kaynakları ve dönüş URL'leri sağlama](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a>Kiracınızdaki bir kimlik sağlayıcısı Amazon yapılandırın
1. Bu adımları [B2C özellikleri dikey penceresine gitme](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalında.
2. B2C özellikleri dikey penceresinde tıklayın **kimlik sağlayıcıları**.
3. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
4. Kullanımı kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "Amzn" girin.
5. Tıklayın **kimlik sağlayıcısı türü**seçin **Amazon**, tıklatıp **Tamam**.
6. Tıklayın **bu kimlik sağlayıcısını ayarlama** istemci kimliği ve daha önce oluşturduğunuz Amazon uygulamasının istemci gizli anahtarını girin.
7. Tıklayın **Tamam** ve ardından **Oluştur** Amazon yapılandırmanızı kaydetmek için.

