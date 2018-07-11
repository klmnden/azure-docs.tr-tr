---
title: Azure Active Directory B2C kullanarak bir Google hesabı ile kaydolma ve oturum açma ayarlama | Microsoft Docs
description: Google hesapları Azure Active Directory B2C kullanarak uygulamalarınızda sahip müşteriler için kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: dd0bf50d73b70e37195e8e5e45336b68e4e883e7
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37915648"
---
# <a name="set-up-sign-up-and-sign-in-with-a-google-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir Google hesabı ile kaydolma ve oturum açma ayarlama

## <a name="create-a-google-application"></a>Bir Google uygulaması oluşturun

Bir Google hesabı bir Azure Active Directory (Azure AD) B2C kimlik sağlayıcısı olarak kullanmak için kiracınızda temsil ettiği bir uygulama oluşturmak gerekir. Bir Google hesabı yoksa, adresinden edinebilirsiniz [ https://accounts.google.com/SignUp ](https://accounts.google.com/SignUp).

1. Oturum [Google geliştiriciler konsol](https://console.developers.google.com/) Google hesabı kimlik bilgilerinizle.
2. Seçin **proje oluştur**ve ardından **Oluştur**. Projeleri önce oluşturduysanız, proje listesi seçin ve ardından **yeni proje**.
3. Girin bir **proje adı**ve ardından **Oluştur**.
3. Seçin **kimlik bilgilerini** sol menüsünü ve ardından **kimlik bilgilerini oluştur** > **Oauth istemcisi kimliği**.
4. Seçin **Yapılandır onay ekranında**.
5. Seçin veya geçerli **e-posta adresi**, sağlayan bir **kullanıcılara gösterilen ürün adı**, tıklatıp **Kaydet**.
6. Altında **uygulama türü**seçin **Web uygulaması**.
7. Girin bir **adı** uygulamanız için girin `https://login.microsoftonline.com` içinde **yetkili JavaScript kaynakları**, ve `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` içinde **yetkili yeniden yönlendirme URI'leri**. Değiştirin **{tenant}** kiracınızın adı (örneğin, contosob2c.onmicrosoft.com) ile.
8. **Oluştur**’a tıklayın.
9. Değerlerini kopyalamayı **istemci kimliği** ve **gizli**. Her ikisi de Google kiracınızdaki bir kimlik sağlayıcısı yapılandırmak için gerekir. **İstemci gizli anahtarı** bir önemli güvenlik kimlik bilgisidir.

## <a name="configure-a-google-account-as-an-identity-provider"></a>Bir Google hesabı kimlik sağlayıcısı olarak yapılandırma

1. Oturum [Azure portalında](https://portal.azure.com/) Azure AD B2C kiracınızın genel Yöneticisi olarak.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olmak için Azure portalın sağ üst köşesinde bu dizine geçin. Abonelik bilgilerinizi ve ardından **Dizin Değiştir**’i seçin. 

    ![Azure AD B2C kiracınıza geçiş yapma](./media/active-directory-b2c-setup-fb-app/switch-directories.png)

    Kiracınızı içeren dizini seçin.

    ![Dizin seçme](./media/active-directory-b2c-setup-fb-app/select-directory.png)

3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. Girin bir **adı**. Örneğin, *Google*.
6. Seçin **kimlik sağlayıcısı türü**seçin **Google**, tıklatıp **Tamam**.
7. Seçin **bu kimlik sağlayıcısını ayarlama** ve daha önce olarak kayıtlı istemci Kimliğini girin **istemci kimliği** olarak kaydettiğiniz istemci gizli anahtarını girin **gizli**daha önce oluşturduğunuz Google uygulamanın.
8. Tıklayın **Tamam** ve ardından **Oluştur** Google yapılandırmanızı kaydetmek için.

