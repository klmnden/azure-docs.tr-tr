---
title: Kaydolma ve oturum açma Microsoft hesabı - Azure Active Directory B2C ile ayarlanmış | Microsoft Docs
description: Azure Active Directory B2C kullanarak uygulamalarınızı Microsoft hesaplarıyla müşterilere kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: c0d236fbe350a36bc67e26349fe3c3145bd01616
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66508444"
---
# <a name="set-up-sign-up-and-sign-in-with-a-microsoft-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir Microsoft hesabı ile kaydolma ve oturum açma ayarlama

## <a name="create-a-microsoft-account-application"></a>Bir Microsoft hesabı uygulaması oluşturun

Bir Microsoft hesabı olarak kullanılacak bir [kimlik sağlayıcısı](active-directory-b2c-reference-oidc.md) Azure Active Directory (Azure AD) B2C'de, temsil ettiği kiracınızda uygulama oluşturmanız gerekir. Bir Microsoft hesabınız yoksa, adresinden edinebilirsiniz [ https://www.live.com/ ](https://www.live.com/).

1. Oturum [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) Microsoft hesabı kimlik bilgilerinizle.
2. Sağ üst köşedeki seçin **uygulama ekleme**.
3. Girin bir **adı** uygulamanız için. Örneğin, *MSAapp1*.
4. Seçin **yeni parola oluştur** ve kimlik sağlayıcısı yapılandırırken kullanılacak bir parola kopyaladığınızdan emin olun. Ayrıca kopyalayın **uygulama kimliği**. 
5. Seçin **Ekle platform**ve ardından seçin **Web**.
4. Girin `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` içinde **yeniden yönlendirme URL'leri**. Değiştirin `your-tenant-name` kiracınızın ada sahip.
5. **Kaydet**’i seçin.

## <a name="configure-a-microsoft-account-as-an-identity-provider"></a>Bir Microsoft hesabı kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. Sağlayan bir **adı**. Örneğin, *MSA*.
6. Seçin **kimlik sağlayıcısı türü**seçin **Microsoft Account**, tıklatıp **Tamam**.
7. Seçin **bu kimlik sağlayıcısını ayarlama** olarak daha önce kaydettiğiniz uygulama kimliği girin **istemci kimliği** olarak kaydedilen parolayı girin **gizli**daha önce oluşturduğunuz Microsoft hesabı uygulaması.
8. Tıklayın **Tamam** ve ardından **Oluştur** Microsoft hesabı yapılandırmanızı kaydetmek için.

