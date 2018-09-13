---
title: Azure Active Directory B2C kullanarak bir Google hesabı ile kaydolma ve oturum açma ayarlama | Microsoft Docs
description: Google hesapları Azure Active Directory B2C kullanarak uygulamalarınızda sahip müşteriler için kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 5b8ae1d5d3f28c50cbbaedf65c5589fce98d3c68
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44715916"
---
# <a name="set-up-sign-up-and-sign-in-with-a-google-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir Google hesabı ile kaydolma ve oturum açma ayarlama

## <a name="create-a-google-application"></a>Bir Google uygulaması oluşturun

Bir Google hesabı bir Azure Active Directory (Azure AD) B2C kimlik sağlayıcısı olarak kullanmak için kiracınızda temsil ettiği bir uygulama oluşturmak gerekir. Bir Google hesabı yoksa, adresinden edinebilirsiniz [ https://accounts.google.com/SignUp ](https://accounts.google.com/SignUp).

1. Oturum [Google geliştiriciler konsol](https://console.developers.google.com/) Google hesabı kimlik bilgilerinizle.
2. Seçin **proje oluştur**ve ardından **Oluştur**. Projeleri önce oluşturduysanız, proje listesi seçin ve ardından **yeni proje**.
3. Girin bir **proje adı**, tıklayın **Oluştur**ve ardından yeni proje kullandığınızdan emin olun.
3. Seçin **kimlik bilgilerini** sol menüsünü ve ardından **kimlik bilgilerini oluştur** > **Oauth istemcisi kimliği**.
4. Seçin **Yapılandır onay ekranında**.
5. Seçin veya geçerli **e-posta adresi**, sağlayan bir **kullanıcılara gösterilen ürün adı**, tıklatıp **Kaydet**.
6. Altında **uygulama türü**seçin **Web uygulaması**.
7. Girin bir **adı** uygulamanız için girin `https://your-tenant-name.b2clogin.com` içinde **yetkili JavaScript kaynakları**, ve `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` içinde **yetkili yeniden yönlendirme URI'leri**. Değiştirin `your-tenant-name` kiracınızın ada sahip. Tüm harfleri büyük harflerle Azure AD B2C ile Kiracı tanımlansa bile Kiracı adınızın girerken kullanmanız gerekir.
8. **Oluştur**’a tıklayın.
9. Değerlerini kopyalamayı **istemci kimliği** ve **gizli**. Her ikisi de Google kiracınızdaki bir kimlik sağlayıcısı yapılandırmak için gerekir. **İstemci gizli anahtarı** bir önemli güvenlik kimlik bilgisidir.

## <a name="configure-a-google-account-as-an-identity-provider"></a>Bir Google hesabı kimlik sağlayıcısı olarak yapılandırma

1. Oturum [Azure portalında](https://portal.azure.com/) Azure AD B2C kiracınızın genel Yöneticisi olarak.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.  

    ![Azure AD B2C kiracınıza geçiş yapma](./media/active-directory-b2c-setup-goog-app/switch-directories.png)

3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. Girin bir **adı**. Örneğin, *Google*.
6. Seçin **kimlik sağlayıcısı türü**seçin **Google**, tıklatıp **Tamam**.
7. Seçin **bu kimlik sağlayıcısını ayarlama** ve daha önce olarak kayıtlı istemci Kimliğini girin **istemci kimliği** olarak kaydettiğiniz istemci gizli anahtarını girin **gizli**daha önce oluşturduğunuz Google uygulamanın.
8. Tıklayın **Tamam** ve ardından **Oluştur** Google yapılandırmanızı kaydetmek için.

