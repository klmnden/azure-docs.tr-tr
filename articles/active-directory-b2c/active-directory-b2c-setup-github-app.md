---
title: Azure Active Directory B2C kullanarak bir GitHub hesabı ile kaydolma ve oturum açma ayarlama | Microsoft Docs
description: Azure Active Directory B2C kullanarak uygulamalarınızda GitHub hesabı olan müşteriler için kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 11f3f190c0f55e45c549a8bd1de35f78eb7b752d
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43337438"
---
# <a name="set-up-sign-up-and-sign-in-with-a-github-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir GitHub hesabı ile kaydolma ve oturum açma ayarlama

> [!NOTE]
> Bu özellik önizlemede.
> 

Bir Github hesabı bir Azure Active Directory (Azure AD) B2C kimlik sağlayıcısı olarak kullanmak için kiracınızda temsil ettiği bir uygulama oluşturmanız gerekir. Bir Github hesabı yoksa adresinden edinebilirsiniz [ https://www.github.com/ ](https://www.github.com/).

## <a name="create-a-github-oauth-application"></a>GitHub OAuth uygulaması oluşturma

1. Oturum [GitHub Geliştirici](https://github.com/settings/developers) GitHub kimlik bilgilerinizle Web sitesi.
2. Seçin **OAuth uygulamaları** seçip **yeni bir uygulamayı kaydetme**.
3. Girin bir **uygulama adı** ve **giriş sayfası URL'si**.
4. Girin `https://{tenant}.b2clogin.com/te/{tenant}.onmicrosoft.com/oauth2/authresp` içinde **yetkilendirme geri çağırma URL'si**. Değiştirin **{tenant}** ile Azure AD B2C kiracınızın adı (örneğin, contosob2c).
5. Tıklayın **kaydetme uygulama**.
6. Değerlerini kopyalamayı **istemci kimliği** ve **gizli**. Kiracınız için kimlik sağlayıcısı eklemek için her ikisi de gerekir.

## <a name="configure-a-github-account-as-an-identity-provider"></a>Bir GitHub hesabı kimlik sağlayıcısı olarak yapılandırma

1. Oturum [Azure portalında](https://portal.azure.com/) Azure AD B2C kiracınızın genel Yöneticisi olarak.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olmak için Azure portalın sağ üst köşesinde bu dizine geçin. Abonelik bilgilerinizi ve ardından **Dizin Değiştir**’i seçin. 

    ![Azure AD B2C kiracınıza geçiş yapma](./media/active-directory-b2c-setup-github-app/switch-directories.png)

    Kiracınızı içeren dizini seçin.

    ![Dizin seçme](./media/active-directory-b2c-setup-github-app/select-directory.png)

3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. Sağlayan bir **adı**. Örneğin, *Github*.
6. Seçin **kimlik sağlayıcısı türü**seçin **Github (Önizleme)**, tıklatıp **Tamam**.
7. Seçin **bu kimlik sağlayıcısını ayarlama** ve daha önce olarak kayıtlı istemci kimliğini girin **istemci kimliği** olarak kaydettiğiniz istemci gizli anahtarını girin **gizli**daha önce oluşturduğunuz Github hesabı uygulamanın.
8. Tıklayın **Tamam** ve ardından **Oluştur** Github hesabı yapılandırmanızı kaydetmek için.