---
title: Azure Active Directory B2C kullanarak bir Microsoft hesabı ile kaydolma ve oturum açma ayarlama | Microsoft Docs
description: Azure Active Directory B2C kullanarak uygulamalarınızı Microsoft hesaplarıyla müşterilere kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/05/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 16e4dbac4c8146b048d4d9b76544677a6111e2a5
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900838"
---
# <a name="set-up-sign-up-and-sign-in-with-a-microsoft-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir Microsoft hesabı ile kaydolma ve oturum açma ayarlama

## <a name="create-a-microsoft-account-application"></a>Bir Microsoft hesabı uygulaması oluşturun

Bir Microsoft hesabı bir Azure Active Directory (Azure AD) B2C kimlik sağlayıcısı olarak kullanmak için kiracınızda temsil ettiği bir uygulama oluşturmak gerekir. Bir Microsoft hesabınız yoksa, adresinden edinebilirsiniz [ https://www.live.com/ ](https://www.live.com/).

1. Oturum [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) Microsoft hesabı kimlik bilgilerinizle.
2. Sağ üst köşedeki seçin **uygulama ekleme**.
3. Sağlayan bir **adı** tıklayın ve uygulama için **Oluştur**.
4. Kayıt sayfasında, değerini kopyalayın **uygulama kimliği**. Microsoft hesabınızla kimlik sağlayıcısı olarak kiracınızda yapılandırmak için kullanın.
5. Seçin **Ekle platform**ve ardından seçin **Web**.
6. Girin `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` içinde **yeniden yönlendirme URL'leri**. Değiştirin **{tenant}** kiracınızın adı (örneğin, contosob2c.onmicrosoft.com) ile.
7. Seçin **yeni parola oluştur** altında **uygulama gizli dizilerini**. Ekranda görüntülenen yeni parolayı kopyalayın. Kiracınızdaki bir kimlik sağlayıcısı olarak bir Microsoft hesabı yapılandırmak için ihtiyacınız. Bu parola, bir önemli güvenlik kimlik bilgisidir.

## <a name="configure-a-microsoft-account-as-an-identity-provider"></a>Bir Microsoft hesabı kimlik sağlayıcısı olarak yapılandırma

1. Oturum [Azure portalında](https://portal.azure.com/) Azure AD B2C kiracınızın genel Yöneticisi olarak.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olmak için Azure portalın sağ üst köşesinde bu dizine geçin. Abonelik bilgilerinizi ve ardından **Dizin Değiştir**’i seçin. 

    ![Azure AD B2C kiracınıza geçiş yapma](./media/active-directory-b2c-setup-msa-app/switch-directories.png)

    Kiracınızı içeren dizini seçin.

    ![Dizin seçme](./media/active-directory-b2c-setup-msa-app/select-directory.png)

3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. Sağlayan bir **adı**. Örneğin, *MSA*.
6. Seçin **kimlik sağlayıcısı türü**seçin **Microsoft Account**, tıklatıp **Tamam**.
7. Seçin **bu kimlik sağlayıcısını ayarlama** olarak daha önce kaydettiğiniz uygulama kimliği girin **istemci kimliği** olarak kaydedilen parolayı girin **gizli**daha önce oluşturduğunuz Microsoft hesabı uygulaması.
8. Tıklayın **Tamam** ve ardından **Oluştur** Microsoft hesabı yapılandırmanızı kaydetmek için.

