---
title: Kaydolma ve oturum açma - Azure Active Directory B2C bir Facebook hesabıyla ayarlama | Microsoft Docs
description: Azure Active Directory B2C kullanarak uygulamalarınızda Facebook hesabı olan müşteriler için kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: b4182eae848fdb862b770e9707e9de31bcdb4e0a
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64703514"
---
# <a name="set-up-sign-up-and-sign-in-with-a-facebook-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir Facebook hesabıyla kaydolma ve oturum açma ayarlama

## <a name="create-a-facebook-application"></a>Bir Facebook uygulaması oluşturun

Bir Facebook hesabı olarak kullanılacak bir [kimlik sağlayıcısı](active-directory-b2c-reference-oauth-code.md) Azure Active Directory (Azure AD) B2C'de, temsil ettiği kiracınızda uygulama oluşturmanız gerekir. Bir Facebook hesabı yoksa adresinden edinebilirsiniz [ https://www.facebook.com/ ](https://www.facebook.com/).

1. Oturum [geliştiriciler için Facebook](https://developers.facebook.com/) Facebook hesabı kimlik bilgilerinizle.
2. Zaten yapmadıysanız, Facebook geliştiricisi olarak kaydolma gerekir. Bunu yapmak için **kaydetme** sayfanın sağ üst köşesindeki Facebook'ın ilkeleri kabul edin ve kayıt adımları tamamlayın.
3. Seçin **uygulamalarım** ve ardından **yeni uygulama Ekle**. 
4. Girin bir **görünen ad** ve geçerli bir **ilgili kişi e-posta**.
5. Tıklayın **uygulama kimliği oluşturma**. Bu, Facebook platform ilkeleri kabul edin ve çevrimiçi güvenlik denetimini Tamamla gerektirebilir.
6. Seçin **ayarları** > **temel**.
7. Seçin bir **kategori**, örneğin `Business and Pages`. Bu değer tarafından Facebook gerekli, ancak Azure AD B2C için kullanılmaz.
8. Sayfanın en altında seçin **Platform Ekle**ve ardından **Web sitesi**.
9. İçinde **Site URL'si**, girin `https://your-tenant-name.b2clogin.com/` değiştirerek `your-tenant-name` kiracınızın ada sahip. Bir URL girin **gizlilik ilkesi URL'si**, örneğin `http://www.contoso.com`. İlke URL'si, uygulamanız için gizlilik bilgileri sağlamak için bakımını bir sayfadır.
10. Seçin **değişiklikleri kaydetmek**.
11. Sayfanın üst kısmında değerini kopyalayın **uygulama kimliği**. 
12. Tıklayın **Göster** ve değerini kopyalayın **uygulama gizli anahtarı**. Facebook kimlik sağlayıcısı olarak kiracınızda yapılandırmak için bu ikisinin de kullanın. **Uygulama gizli anahtarı** bir önemli güvenlik kimlik bilgisidir.
13. Seçin **ürünleri**ve ardından **ayarlanan** altında **Facebook oturum açma**.
14. Seçin **ayarları** altında **Facebook oturum açma**.
15. İçinde **geçerli OAuth yeniden yönlendirme URI'leri**, girin `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`. Değiştirin `your-tenant-name` kiracınızın ada sahip. Tıklayın **Değişiklikleri Kaydet** sayfanın alt kısmındaki.
16. Facebook uygulamanızı Azure AD B2C için kullanılabilir hale getirmek için durumu üst seçiciyi sayfanın sağ gidip **üzerinde** uygulama genel yapın ve ardından **Onayla**.  Bu noktada gelen durumu değiştiğinde **geliştirme** için **canlı**.

## <a name="configure-a-facebook-account-as-an-identity-provider"></a>Bir Facebook hesabıyla bir kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme. 
3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. Girin bir **adı**. Örneğin, *Facebook*.
6. Seçin **kimlik sağlayıcısı türü**seçin **Facebook**, tıklatıp **Tamam**.
7. Seçin **bu kimlik sağlayıcısını ayarlama** olarak daha önce kaydedilen uygulama kimliği girin **istemci kimliği** olarak kayıtlı uygulama gizli anahtarı girin **gizli** , daha önce oluşturduğunuz Facebook uygulaması.
8. Tıklayın **Tamam** ve ardından **Oluştur** Facebook yapılandırmanızı kaydetmek için.
