---
title: Azure Active Directory B2C kullanarak bir Twitter hesabıyla kaydolma ve oturum açma ayarlama | Microsoft Docs
description: Azure Active Directory B2C kullanarak uygulamalarınızı Twitter hesaplarında sahip müşteriler için kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 5732293510a75a3c40df5cf3d31978c5ce599791
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44720166"
---
# <a name="set-up-sign-up-and-sign-in-with-a-twitter-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir Twitter hesabıyla kaydolma ve oturum açma ayarlama

## <a name="create-a-twitter-application"></a>Twitter uygulaması oluşturma

Azure Active Directory (Azure AD) B2C'de kimlik sağlayıcısı olarak Twitter hesabıyla kullanmak için kiracınızda temsil ettiği bir uygulama oluşturmak gerekir. Twitter hesabıyla yoksa adresinden edinebilirsiniz [ https://twitter.com/signup ](https://twitter.com/signup).

1. Oturum [Twitter uygulamaları](https://apps.twitter.com/) Twitter kimlik bilgilerinizle.
2. Seçin **uygulama oluşturma**.
3. Girin **uygulama adı**, **uygulama açıklaması**, ve **Web sitesi URL'si**.
4. Girin `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/your-policy-name/oauth1/authresp` içinde **geri çağırma URL'leri**. Değiştirin `your-tenant-name` kiracınızın adını ve `your-policy-name` ilkenizin adını. Örneğin, `b2c_1_signupsignin`. Tüm harfleri büyük harflerle Azure AD B2C ile tanımlanan bile kiracınızın adı, ilke adı girerken kullanmanız gerekir.
5. Kabul **Geliştirici sözleşmesi** seçip **Oluştur**.
7. Seçin **anahtarlar ve erişim belirteçleri** sekmesi.
8. Değerlerini kopyalamayı **API anahtarı** ve **API gizli anahtarı**. Her ikisi de kiracınızdaki bir kimlik sağlayıcısı olarak Twitter hesabı yapılandırmak için gerekir.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Kiracınızdaki bir kimlik sağlayıcısı olarak twitter'ı yapılandırma

1. Oturum [Azure portalında](https://portal.azure.com/) Azure AD B2C kiracınızın genel Yöneticisi olarak.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.  

    ![Azure AD B2C kiracınıza geçiş yapma](./media/active-directory-b2c-setup-twitter-app/switch-directories.png)

3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. Sağlayan bir **adı**. Örneğin, *Twitter*.
6. Seçin **kimlik sağlayıcısı türü**seçin **Twitter**, tıklatıp **Tamam**.
7. Seçin **bu kimlik sağlayıcısını ayarlama** ve API anahtarını girin **istemci kimliği** ve API gizli anahtarınızı **gizli**.
8. Tıklayın **Tamam**ve ardından **Oluştur** Twitter yapılandırmanızı kaydetmek için.