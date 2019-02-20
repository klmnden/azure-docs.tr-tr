---
title: Azure Active Directory B2C - Google hesabı ile oturum açma ve kaydolma ayarlama | Microsoft Docs
description: Google hesapları Azure Active Directory B2C kullanarak uygulamalarınızda sahip müşteriler için kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 29b988beff0f25bc0d4abcc1c95a70bccf9ba1cb
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56427437"
---
# <a name="set-up-sign-up-and-sign-in-with-a-google-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir Google hesabı ile kaydolma ve oturum açma ayarlama

## <a name="create-a-google-application"></a>Bir Google uygulaması oluşturun

Google hesabı olarak kullanılacak bir [kimlik sağlayıcısı](active-directory-b2c-reference-oauth-code.md) Azure Active Directory (Azure AD) B2C'de, temsil ettiği kiracınızda uygulama oluşturmanız gerekir. Bir Google hesabı yoksa, adresinden edinebilirsiniz [ https://accounts.google.com/SignUp ](https://accounts.google.com/SignUp).

1. Oturum [Google geliştiriciler konsol](https://console.developers.google.com/) Google hesabı kimlik bilgilerinizle.
2. Seçin **proje oluştur**ve ardından **Oluştur**. Projeleri önce oluşturduysanız, proje listesi seçin ve ardından **yeni proje**.
3. Girin bir **proje adı**, tıklayın **Oluştur**ve ardından yeni proje kullandığınızdan emin olun.
3. Seçin **kimlik bilgilerini** sol menüsünü ve ardından **kimlik bilgilerini oluştur** > **Oauth istemcisi kimliği**.
4. Seçin **Yapılandır onay ekranında**.
5. Seçin veya geçerli bir belirtin **e-posta adresi**, sağlayan bir **kullanıcılara gösterilen ürün adı**, ekleyin `b2clogin.com` için **etki alanlarında yetkilendirilmiş**, tıklayın **Kaydet** .
6. Altında **uygulama türü**seçin **Web uygulaması**.
7. Girin bir **adı** uygulamanız için girin `https://your-tenant-name.b2clogin.com` içinde **yetkili JavaScript kaynakları**, ve `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` içinde **yetkili yeniden yönlendirme URI'leri**. Değiştirin `your-tenant-name` kiracınızın ada sahip. Tüm harfleri büyük harflerle Azure AD B2C ile Kiracı tanımlansa bile Kiracı adınızın girerken kullanmanız gerekir.
8. **Oluştur**’a tıklayın.
9. Değerlerini kopyalamayı **istemci kimliği** ve **gizli**. Her ikisi de Google kiracınızdaki bir kimlik sağlayıcısı yapılandırmak için gerekir. **İstemci gizli anahtarı** bir önemli güvenlik kimlik bilgisidir.

## <a name="configure-a-google-account-as-an-identity-provider"></a>Bir Google hesabı kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. Girin bir **adı**. Örneğin, *Google*.
6. Seçin **kimlik sağlayıcısı türü**seçin **Google**, tıklatıp **Tamam**.
7. Seçin **bu kimlik sağlayıcısını ayarlama** ve daha önce olarak kayıtlı istemci Kimliğini girin **istemci kimliği** olarak kaydettiğiniz istemci gizli anahtarını girin **gizli**daha önce oluşturduğunuz Google uygulamanın.
8. Tıklayın **Tamam** ve ardından **Oluştur** Google yapılandırmanızı kaydetmek için.

