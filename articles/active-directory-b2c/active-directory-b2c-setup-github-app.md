---
title: Azure Active Directory B2C kullanarak bir GitHub hesabı ile kaydolma ve oturum açma ayarlama | Microsoft Docs
description: Azure Active Directory B2C kullanarak uygulamalarınızda GitHub hesabı olan müşteriler için kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 92ae3e0697c2ecd345c8e75690d40a81ef473ba4
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44713791"
---
# <a name="set-up-sign-up-and-sign-in-with-a-github-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir GitHub hesabı ile kaydolma ve oturum açma ayarlama

> [!NOTE]
> Bu özellik önizlemede.
> 

Bir Github hesabı bir Azure Active Directory (Azure AD) B2C kimlik sağlayıcısı olarak kullanmak için kiracınızda temsil ettiği bir uygulama oluşturmanız gerekir. Bir Github hesabı yoksa adresinden edinebilirsiniz [ https://www.github.com/ ](https://www.github.com/).

## <a name="create-a-github-oauth-application"></a>GitHub OAuth uygulaması oluşturma

1. Oturum [GitHub Geliştirici](https://github.com/settings/developers) GitHub kimlik bilgilerinizle Web sitesi.
2. Seçin **OAuth uygulamaları** seçip **yeni bir OAuth uygulaması**.
3. Girin bir **uygulama adı** ve **giriş sayfası URL'si**.
4. Girin `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` içinde **yetkilendirme geri çağırma URL'si**. Değiştirin `your-tenant-name` Azure AD B2C kiracınızın adı.
5. Tıklayın **kaydetme uygulama**.
6. Değerlerini kopyalamayı **istemci kimliği** ve **gizli**. Kiracınız için kimlik sağlayıcısı eklemek için her ikisi de gerekir.

## <a name="configure-a-github-account-as-an-identity-provider"></a>Bir GitHub hesabı kimlik sağlayıcısı olarak yapılandırma

1. Oturum [Azure portalında](https://portal.azure.com/) Azure AD B2C kiracınızın genel Yöneticisi olarak.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.  

    ![Azure AD B2C kiracınıza geçiş yapma](./media/active-directory-b2c-setup-github-app/switch-directories.png)

3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. Sağlayan bir **adı**. Örneğin, *Github*.
6. Seçin **kimlik sağlayıcısı türü**seçin **Github (Önizleme)**, tıklatıp **Tamam**.
7. Seçin **bu kimlik sağlayıcısını ayarlama** ve daha önce olarak kayıtlı istemci kimliğini girin **istemci kimliği** olarak kaydettiğiniz istemci gizli anahtarını girin **gizli**daha önce oluşturduğunuz Github hesabı uygulamanın.
8. Tıklayın **Tamam** ve ardından **Oluştur** Github hesabı yapılandırmanızı kaydetmek için.