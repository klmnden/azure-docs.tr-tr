---
title: Kaydolma ve oturum açma - Azure Active Directory B2C bir LinkedIn hesabıyla ayarlamanız | Microsoft Docs
description: Azure Active Directory B2C kullanarak uygulamalarınızda LinkedIn hesaplar kullanan müşteriler için kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: de58f960842e0a4f8e9b964774ce62b3e2772113
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60316390"
---
# <a name="set-up-sign-up-and-sign-in-with-a-linkedin-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir LinkedIn hesabıyla kaydolma ve oturum açma ayarlama

## <a name="create-a-linkedin-application"></a>Bir LinkedIn uygulaması oluşturun

LinkedIn hesabı olarak kullanılacak bir [kimlik sağlayıcısı](active-directory-b2c-reference-oauth-code.md) Azure Active Directory (Azure AD) B2C'de, temsil ettiği kiracınızda uygulama oluşturmanız gerekir. Bir LinkedIn hesabınız yoksa, adresinden edinebilirsiniz [ https://www.linkedin.com/ ](https://www.linkedin.com/).

1. Oturum [LinkedIn geliştiricilerin Web sitesi](https://www.developer.linkedin.com/) LinkedIn hesabınızın kimlik bilgileriyle.
2. Seçin **uygulamalarım**ve ardından **uygulama oluşturma**.
3. Girin **şirket adı**, **uygulama adı**, **uygulama açıklaması**, **uygulama logosu**, **uygulama kullanımı** , **Web sitesi URL'si**, **iş e-posta**, ve **iş telefonu**.
4. Kabul **LinkedIn API kullanım** tıklatıp **Gönder**.
5. Değerlerini kopyalamayı **istemci kimliği** ve **gizli**. Bunların altında bulabilirsiniz **kimlik doğrulama anahtarlarını**. Her ikisi de LinkedIn kiracınızdaki bir kimlik sağlayıcısı yapılandırmak için gerekir. **İstemci gizli anahtarı** bir önemli güvenlik kimlik bilgisidir.
6. Girin `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` içinde **yeniden yönlendirme URL'leri yetkili**. Değiştirin `your-tenant-name` kiracınızın ada sahip. Tüm harfleri büyük harflerle Azure AD B2C ile Kiracı tanımlansa bile Kiracı adınızın girerken kullanmanız gerekir. Seçin **Ekle**ve ardından **güncelleştirme**.

## <a name="configure-a-linkedin-account-as-an-identity-provider"></a>Bir LinkedIn hesabıyla bir kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. Sağlayan bir **adı**. Örneğin, *LinkedIn*.
6. Seçin **kimlik sağlayıcısı türü**seçin **LinkedIn**, tıklatıp **Tamam**.
7. Seçin **bu kimlik sağlayıcısını ayarlama** ve daha önce olarak kayıtlı istemci kimliğini girin **istemci kimliği** olarak kaydettiğiniz istemci gizli anahtarını girin **gizli**daha önce oluşturduğunuz LinkedIn hesabı uygulamanın.
8. Tıklayın **Tamam** ve ardından **Oluştur** LinkedIn hesabı yapılandırmanızı kaydetmek için.

## <a name="migration-from-v10-to-v20"></a>V2.0 v1.0 geçiş

Yakın zamanda LinkedIn [, API'nin v1.0 gelen v2.0 için güncelleştirilmiş](https://engineering.linkedin.com/blog/2018/12/developer-program-updates). Geçişin bir parçası, Azure AD B2C'yi yalnızca kayıt sırasında LinkedIn kullanıcının tam adını almak kullanabilirsiniz. Bir e-posta adresi toplanan özniteliklerinden biri ise sırasında kaydolma, kullanıcı el ile e-posta adresi girin ve doğrulayın.
