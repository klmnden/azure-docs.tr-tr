---
title: Bir GitHub hesabı - Azure Active Directory B2C ile oturum açma ve kaydolma | Microsoft Docs
description: Azure Active Directory B2C kullanarak uygulamalarınızda GitHub hesabı olan müşteriler için kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 88d92e081a7b852035dd2b7d3bc9e4e29fefaddd
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66508571"
---
# <a name="set-up-sign-up-and-sign-in-with-a-github-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir GitHub hesabı ile kaydolma ve oturum açma ayarlama

> [!NOTE]
> Bu özellik önizlemede.
> 

Bir GitHub hesabı olarak kullanılacak bir [kimlik sağlayıcısı](active-directory-b2c-reference-oauth-code.md) Azure Active Directory (Azure AD) B2C'de, temsil ettiği kiracınızda uygulama oluşturmanız gerekir. Bir GitHub hesabı yoksa adresinden edinebilirsiniz [ https://www.github.com/ ](https://www.github.com/).

## <a name="create-a-github-oauth-application"></a>GitHub OAuth uygulaması oluşturma

1. Oturum [GitHub Geliştirici](https://github.com/settings/developers) GitHub kimlik bilgilerinizle Web sitesi.
2. Seçin **OAuth uygulamaları** seçip **yeni bir OAuth uygulaması**.
3. Girin bir **uygulama adı** ve **giriş sayfası URL'si**.
4. Girin `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` içinde **yetkilendirme geri çağırma URL'si**. Değiştirin `your-tenant-name` Azure AD B2C kiracınızın adı. Azure AD B2C kodunda büyük harfler ile Kiracı tanımlansa bile Kiracı adınızın girerken tamamen küçük harf kullanın.
5. Tıklayın **kaydetme uygulama**.
6. Değerlerini kopyalamayı **istemci kimliği** ve **gizli**. Kiracınız için kimlik sağlayıcısı eklemek için her ikisi de gerekir.

## <a name="configure-a-github-account-as-an-identity-provider"></a>Bir GitHub hesabı kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. Sağlayan bir **adı**. Örneğin, *GitHub*.
6. Seçin **kimlik sağlayıcısı türü**seçin **GitHub (Önizleme)** , tıklatıp **Tamam**.
7. Seçin **bu kimlik sağlayıcısını ayarlama** ve daha önce olarak kayıtlı istemci kimliğini girin **istemci kimliği** olarak kaydettiğiniz istemci gizli anahtarını girin **gizli**daha önce oluşturduğunuz GitHub hesabı uygulamanın.
8. Tıklayın **Tamam** ve ardından **Oluştur** GitHub hesabı yapılandırmanızı kaydetmek için.
