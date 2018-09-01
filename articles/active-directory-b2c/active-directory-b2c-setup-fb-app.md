---
title: Azure Active Directory B2C kullanarak bir Facebook hesabıyla kaydolma ve oturum açma ayarlama | Microsoft Docs
description: Azure Active Directory B2C kullanarak uygulamalarınızda Facebook hesabı olan müşteriler için kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 985285b463d66770f97a431705d5b9198b632592
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344615"
---
# <a name="set-up-sign-up-and-sign-in-with-a-facebook-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir Facebook hesabıyla kaydolma ve oturum açma ayarlama

## <a name="create-a-facebook-application"></a>Bir Facebook uygulaması oluşturun

Bir Facebook hesabıyla bir kimlik sağlayıcısı olarak Azure Active Directory (Azure AD) B2C'yi kullanmak için kiracınızda temsil ettiği bir uygulama oluşturmak gerekir. Bir Facebook hesabı yoksa adresinden edinebilirsiniz [ https://www.facebook.com/ ](https://www.facebook.com/).

1. Oturum [geliştiriciler için Facebook](https://developers.facebook.com/) Facebook hesabı kimlik bilgilerinizle.
2. Zaten yapmadıysanız, Facebook geliştiricisi olarak kaydolma gerekir. Bunu yapmak için **kaydetme** sayfanın sağ üst köşesindeki Facebook'ın ilkeleri kabul edin ve kayıt adımları tamamlayın.
3. Seçin **uygulamalarım** ve ardından **yeni uygulama Ekle**. 
4. Girin bir **görünen ad** ve geçerli bir **ilgili kişi e-posta**.
5. Tıklayın **uygulama kimliği oluşturma**. Bu, Facebook platform ilkeleri kabul edin ve çevrimiçi güvenlik denetimini Tamamla gerektirebilir.
6. Seçin **ayarları** > **temel**.
7. Sayfanın en altında seçin **Platform Ekle**ve ardından **Web sitesi**.
8. Girin `https://{tenantname}.b2clogin.com/` içinde **Site URL'si**. Bir URL girin **gizlilik ilkesi URL'si**, örneğin `http://www.contoso.com`.
9. Seçin **değişiklikleri kaydetmek**.
11. Sayfanın üst kısmında değerini kopyalayın **uygulama kimliği**. 
12. Tıklayın **Göster** ve değerini kopyalayın **uygulama gizli anahtarı**. Facebook kimlik sağlayıcısı olarak kiracınızda yapılandırmak için bu ikisinin de kullanın. **Uygulama gizli anahtarı** bir önemli güvenlik kimlik bilgisidir.
13. Seçin **ürünleri**ve ardından **ayarlanan** altında **Facebook oturum açma**.
14. Seçin **ayarları** altında **Facebook oturum açma**.
15. Girin `https://{tenantname}.b2clogin.com/te/{tenant}.onmicrosoft.com/oauth2/authresp` içinde **geçerli OAuth yeniden yönlendirme URI'leri** . Değiştirin **{tenant}** ile kiracınızın adı (örneğin, contosob2c). Tıklayın **Değişiklikleri Kaydet** sayfanın alt kısmındaki.
16. Facebook uygulamanızı Azure AD B2C için kullanılabilir hale getirmek için seçin **uygulama incelemesi**ayarlayın **Uygulamam olun genel?** için **Evet**, örneğin birkategoriseçin`Business and Pages`ve ardından **Onayla**.

## <a name="configure-a-facebook-account-as-an-identity-provider"></a>Bir Facebook hesabıyla bir kimlik sağlayıcısı olarak yapılandırma

1. Oturum [Azure portalında](https://portal.azure.com/) Azure AD B2C kiracınızın genel Yöneticisi olarak.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olmak için Azure portalın sağ üst köşesinde bu dizine geçin. Abonelik bilgilerinizi ve ardından **Dizin Değiştir**’i seçin. 

    ![Azure AD B2C kiracınıza geçiş yapma](./media/active-directory-b2c-setup-fb-app/switch-directories.png)

    Kiracınızı içeren dizini seçin.

    ![Dizin seçme](./media/active-directory-b2c-setup-fb-app/select-directory.png)

3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. Girin bir **adı**. Örneğin, *Facebook*.
6. Seçin **kimlik sağlayıcısı türü**seçin **Facebook**, tıklatıp **Tamam**.
7. Seçin **bu kimlik sağlayıcısını ayarlama** olarak daha önce kaydedilen uygulama kimliği girin **istemci kimliği** olarak kayıtlı uygulama gizli anahtarı girin **gizli** , daha önce oluşturduğunuz Facebook uygulama).
8. Tıklayın **Tamam** ve ardından **Oluştur** Facebook yapılandırmanızı kaydetmek için.