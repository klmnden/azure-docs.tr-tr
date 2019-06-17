---
title: Kaydolma ve oturum açma Microsoft hesabı - Azure Active Directory B2C ile ayarlama
description: Azure Active Directory B2C kullanarak uygulamalarınızı Microsoft hesaplarıyla müşterilere kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 06/11/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 63aa93e72d76d430fc7e026478d91e496f27eae6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67055105"
---
# <a name="set-up-sign-up-and-sign-in-with-a-microsoft-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir Microsoft hesabı ile kaydolma ve oturum açma ayarlama

## <a name="create-a-microsoft-account-application"></a>Bir Microsoft hesabı uygulaması oluşturun

Bir Microsoft hesabı olarak kullanılacak bir [kimlik sağlayıcısı](active-directory-b2c-reference-oidc.md) Azure Active Directory (Azure AD) B2C'de, temsil ettiği kiracınızda uygulama oluşturmanız gerekir. Bir Microsoft hesabınız yoksa, adresinden edinebilirsiniz [ https://www.live.com/ ](https://www.live.com/).

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **uygulama kayıtları**.
1. Seçin **yeni kayıt**
1. Girin bir **adı** uygulamanız için. Örneğin, *MSAapp1*.
1. Altında **desteklenen hesap türleri**seçin **herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları (örneğin, Skype, Xbox, Outlook.com) hesaplarında**. Bu seçenek, Microsoft kimlikleri geniş kümesini hedefler.

   Farklı bir hesap türü seçenekleri hakkında daha fazla bilgi için bkz. [hızlı başlangıç: Microsoft kimlik platformu bir uygulamayı kaydetme](../active-directory/develop/quickstart-register-app.md).
1. Altında **yeniden yönlendirme URI'si (isteğe bağlı)** seçin **Web** girin `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` metin kutusuna. Değiştirin `your-tenant-name` Azure AD B2C Kiracı adınızla.
1. Seçin **kaydetme**
1. Kayıt **uygulama (istemci) kimliği** uygulama genel bakış sayfasında gösterilir. Sonraki bölümde kimlik sağlayıcısı yapılandırırken gerekir.
1. Seçin **sertifikalarını ve gizli anahtarları**
1. Tıklayın **yeni istemci gizli anahtarı**
1. Girin bir **açıklama** örneğin gizliliği için *uygulama parolası 1*ve ardından **Ekle**.
1. Gösterilen uygulama parolası kayıt **değer** sütun. Sonraki bölümde kimlik sağlayıcısı yapılandırırken gerekir.

## <a name="configure-a-microsoft-account-as-an-identity-provider"></a>Bir Microsoft hesabı kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
1. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
1. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
1. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
1. Sağlayan bir **adı**. Örneğin, *MSA*.
1. Seçin **kimlik sağlayıcısı türü**seçin **Microsoft Account**, tıklatıp **Tamam**.
1. Seçin **bu kimlik sağlayıcısını ayarlama** ve daha önce kaydettiğiniz (istemci) uygulama kimliği girin **istemci kimliği** metin kutusu ve içinde kaydettiğiniz istemci gizli anahtarını girin **istemci Gizli dizi** metin kutusu.
1. Tıklayın **Tamam** ve ardından **Oluştur** Microsoft hesabı yapılandırmanızı kaydetmek için.
