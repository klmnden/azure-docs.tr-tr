---
title: Azure Active Directory B2C kullanarak bir Amazon hesabıyla kaydolma ve oturum açma ayarlama | Microsoft Docs
description: Azure Active Directory B2C kullanarak uygulamalarınızda Amazon hesaplar kullanan müşteriler için kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: c64b32656db2d3b821833450b4e866b9e33e44cd
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43337354"
---
# <a name="set-up-sign-up-and-sign-in-with-an-amazon-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir Amazon hesabıyla kaydolma ve oturum açma ayarlama

## <a name="create-an-amazon-application"></a>Amazon uygulama oluşturma

Amazon hesabınız Azure Active Directory (Azure AD) B2C'de kimlik sağlayıcısı olarak kullanmak için kiracınızda temsil ettiği bir uygulama oluşturmak gerekir. Bir Amazon hesabınız zaten yoksa, ulaşabilirsiniz [ http://www.amazon.com/ ](http://www.amazon.com/).

1. Oturum [Amazon Geliştirici Merkezi](https://login.amazon.com/) Amazon hesabınızın kimlik bilgileriyle.
2. Zaten yapmadıysanız, tıklayın **Kaydol**, geliştirici kayıt adımları izleyin ve ilkeyi kabul edin.
3. Seçin **yeni uygulamayı kaydedin**.
4. Girin bir **adı**, **açıklama**, ve **gizlilik bildirimi URL'si**ve ardından **Kaydet**.
5. İçinde **Web ayarları** bölümünde, değerlerini kopyalamayı **istemci kimliği**. Seçin **Göster gizli** istemci gizli anahtarı alın ve bunu kopyalayın. Her ikisi de kiracınızdaki bir kimlik sağlayıcısı Amazon hesabı yapılandırmak için gerekir. **İstemci gizli anahtarı** bir önemli güvenlik kimlik bilgisidir.
6. İçinde **Web ayarları** bölümünden **Düzenle**ve enter `https://{tenant}.b2clogin.com` içinde **izin JavaScript kaynakları** ve `https://{tenant}.b2clogin.com/te/{tenant}.onmicrosoft.com/oauth2/authresp` içinde **izin verildi URL'leri dönüş**. Değiştirin **{tenant}** ile kiracınızın adı (örneğin, contosob2c). 
7. **Kaydet**’e tıklayın.

## <a name="configure-an-amazon-account-as-an-identity-provider"></a>Bir Amazon hesap kimlik sağlayıcısı olarak yapılandırın

1. Oturum [Azure portalında](https://portal.azure.com/) Azure AD B2C kiracınızın genel Yöneticisi olarak.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olmak için Azure portalın sağ üst köşesinde bu dizine geçin. Abonelik bilgilerinizi ve ardından **Dizin Değiştir**’i seçin. 

    ![Azure AD B2C kiracınıza geçiş yapma](./media/active-directory-b2c-setup-fb-app/switch-directories.png)

    Kiracınızı içeren dizini seçin.

    ![Dizin seçme](./media/active-directory-b2c-setup-fb-app/select-directory.png)

3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. Girin bir **adı**. Örneğin, *Amazon*.
6. Seçin **kimlik sağlayıcısı türü**seçin **Amazon**, tıklatıp **Tamam**.
7. Seçin **bu kimlik sağlayıcısını ayarlama** ve daha önce olarak kayıtlı istemci Kimliğini girin **istemci kimliği** olarak kaydettiğiniz istemci gizli anahtarını girin **gizli**daha önce oluşturduğunuz Amazon uygulamanın.
8. Tıklayın **Tamam** ve ardından **Oluştur** Amazon yapılandırmanızı kaydetmek için.

