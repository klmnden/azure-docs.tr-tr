---
title: Azure Active Directory B2C kullanarak bir Twitter hesabıyla kaydolma ve oturum açma ayarlama | Microsoft Docs
description: Azure Active Directory B2C kullanarak uygulamalarınızı Twitter hesaplarında sahip müşteriler için kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 6d8e9245e95c08aad69cd05f338b6260e554469b
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43337799"
---
# <a name="set-up-sign-up-and-sign-in-with-a-twitter-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir Twitter hesabıyla kaydolma ve oturum açma ayarlama

## <a name="create-a-twitter-application"></a>Twitter uygulaması oluşturma

Azure Active Directory (Azure AD) B2C'de kimlik sağlayıcısı olarak Twitter hesabıyla kullanmak için kiracınızda temsil ettiği bir uygulama oluşturmak gerekir. Twitter hesabıyla yoksa adresinden edinebilirsiniz [ https://twitter.com/signup ](https://twitter.com/signup).

1. Oturum [Twitter uygulamaları](https://apps.twitter.com/) Twitter kimlik bilgilerinizle.
2. Seçin **yeni uygulama oluştur**.
3. Girin **adı**, **açıklama**, ve **Web sitesi**.
4. Girin `https://{tenant}.b2clogin.com/te/{tenant}.onmicrosoft.com/{policyId}/oauth1/authresp` içinde **geri çağırma URL'leri**. Değiştirin **{tenant}** kiracınızın adı (örneğin, contosob2c) ile ve **{Policyıd}** ilke kimliğinizle (örneğin, b2c_1_policy). Twitter hesabı kullanan tüm ilkeleri için bir geri çağırma URL'si eklemeniz gerekir. 
5. Kabul **Geliştirici sözleşmesi** seçip **kendi Twitter uygulamanızı oluşturun**.
7. Seçin **anahtarlar ve erişim belirteçleri** sekmesi.
8. Değerini kopyalayın **tüketici anahtarı** ve **tüketici gizli**. Her ikisi de kiracınızdaki bir kimlik sağlayıcısı olarak Twitter hesabı yapılandırmak için gerekir.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Kiracınızdaki bir kimlik sağlayıcısı olarak twitter'ı yapılandırma

1. Oturum [Azure portalında](https://portal.azure.com/) Azure AD B2C kiracınızın genel Yöneticisi olarak.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olmak için Azure portalın sağ üst köşesinde bu dizine geçin. Abonelik bilgilerinizi ve ardından **Dizin Değiştir**’i seçin. 

    ![Azure AD B2C kiracınıza geçiş yapma](./media/active-directory-b2c-setup-twitter-app/switch-directories.png)

    Kiracınızı içeren dizini seçin.

    ![Dizin seçme](./media/active-directory-b2c-setup-twitter-app/select-directory.png)

3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. Sağlayan bir **adı**. Örneğin, *Twitter*.
6. Seçin **kimlik sağlayıcısı türü**seçin **Twitter**, tıklatıp **Tamam**.
7. Seçin **bu kimlik sağlayıcısını ayarlama** tüketici anahtarını girebilirsiniz **istemci kimliği** ve **tüketici gizli** için **gizli**.
8. Tıklayın **Tamam**ve ardından **Oluştur** Twitter yapılandırmanızı kaydetmek için.