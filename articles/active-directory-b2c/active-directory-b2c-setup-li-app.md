---
title: Azure Active Directory B2C kullanarak bir LinkedIn hesabıyla kaydolma ve oturum açma ayarlama | Microsoft Docs
description: Azure Active Directory B2C kullanarak uygulamalarınızda LinkedIn hesaplar kullanan müşteriler için kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 8388baf88f5bb723e5b0e47bc93b100d5ce8e3e2
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55159809"
---
# <a name="set-up-sign-up-and-sign-in-with-a-linkedin-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir LinkedIn hesabıyla kaydolma ve oturum açma ayarlama

## <a name="create-a-linkedin-application"></a>Bir LinkedIn uygulaması oluşturun

Bir LinkedIn hesabıyla bir kimlik sağlayıcısı olarak Azure Active Directory (Azure AD) B2C'yi kullanmak için kiracınızda temsil ettiği bir uygulama oluşturmak gerekir. Bir LinkedIn hesabınız yoksa, adresinden edinebilirsiniz [ https://www.linkedin.com/ ](https://www.linkedin.com/).

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

