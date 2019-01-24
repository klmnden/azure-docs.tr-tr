---
title: Azure Active Directory B2C kullanarak bir Amazon hesabıyla kaydolma ve oturum açma ayarlama | Microsoft Docs
description: Azure Active Directory B2C kullanarak uygulamalarınızda Amazon hesaplar kullanan müşteriler için kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: ccb104e2578e5c892f5b0e0275f9f55cc28ee083
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54842860"
---
# <a name="set-up-sign-up-and-sign-in-with-an-amazon-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir Amazon hesabıyla kaydolma ve oturum açma ayarlama

## <a name="create-an-amazon-application"></a>Amazon uygulama oluşturma

Amazon hesabınız Azure Active Directory (Azure AD) B2C'de kimlik sağlayıcısı olarak kullanmak için kiracınızda temsil ettiği bir uygulama oluşturmak gerekir. Bir Amazon hesabınız zaten yoksa, ulaşabilirsiniz [ https://www.amazon.com/ ](https://www.amazon.com/).

1. Oturum [Amazon Geliştirici Merkezi](https://login.amazon.com/) Amazon hesabınızın kimlik bilgileriyle.
2. Zaten yapmadıysanız, tıklayın **Kaydol**, geliştirici kayıt adımları izleyin ve ilkeyi kabul edin.
3. Seçin **yeni uygulamayı kaydedin**.
4. Girin bir **adı**, **açıklama**, ve **gizlilik bildirimi URL'si**ve ardından **Kaydet**. Gizlilik bildirimi, gizlilik bilgileri kullanıcılara sağlar, yönettiğiniz bir sayfadır.
5. İçinde **Web ayarları** bölümünde, değerlerini kopyalamayı **istemci kimliği**. Seçin **Göster gizli** istemci gizli anahtarı alın ve bunu kopyalayın. Her ikisi de kiracınızdaki bir kimlik sağlayıcısı Amazon hesabı yapılandırmak için gerekir. **İstemci gizli anahtarı** bir önemli güvenlik kimlik bilgisidir.
6. İçinde **Web ayarları** bölümünden **Düzenle**ve enter `https://your-tenant-name.b2clogin.com` içinde **izin JavaScript kaynakları** ve `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` içinde **izin verildi URL'leri dönüş**. Değiştirin `your-tenant-name` kiracınızın ada sahip. Tüm harfleri büyük harflerle Azure AD B2C ile Kiracı tanımlansa bile Kiracı adınızın girerken kullanmanız gerekir.
7. **Kaydet**’e tıklayın.

## <a name="configure-an-amazon-account-as-an-identity-provider"></a>Bir Amazon hesap kimlik sağlayıcısı olarak yapılandırın

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. Girin bir **adı**. Örneğin, *Amazon*.
6. Seçin **kimlik sağlayıcısı türü**seçin **Amazon**, tıklatıp **Tamam**.
7. Seçin **bu kimlik sağlayıcısını ayarlama** ve daha önce olarak kayıtlı istemci Kimliğini girin **istemci kimliği** olarak kaydettiğiniz istemci gizli anahtarını girin **gizli**daha önce oluşturduğunuz Amazon uygulamanın.
8. Tıklayın **Tamam** ve ardından **Oluştur** Amazon yapılandırmanızı kaydetmek için.

