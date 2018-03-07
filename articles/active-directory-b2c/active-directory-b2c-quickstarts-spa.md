---
title: "Azure AD B2C etkin tek sayfa uygulamasında test sürüşü yapma"
description: "Kullanıcıların kimliğini doğrulamak ve onları kaydetmek için Azure Active Directory B2C kullanan örnek tek sayfalı uygulamayı denemek için hızlı başlangıç."
services: active-directory-b2c
documentationcenter: 
author: PatAltimore
manager: mtillman
ms.reviewer: saraford
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: quickstart
ms.date: 2/13/2018
ms.author: patricka
ms.openlocfilehash: e659fd228c2294313a62b331c8e530b7d34073ac
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="quickstart-test-drive-an-azure-ad-b2c-enabled-single-page-app"></a>Hızlı başlangıç: Azure AD B2C etkin tek sayfa uygulamasında test sürüşü yapma

Azure Active Directory (Azure AD) B2C, uygulama, işletme ve müşterilerinizi güvende tutmak için bulut kimlik yönetimi sağlar. Azure AD B2C, uygulamalarınızın sosyal ağ hesaplarında ve kurumsal hesaplarda açık standart protokoller kullanarak kimlik doğrulaması yapmasına izin verir.

Bu hızlı başlangıçta, sosyal kimlik sağlayıcısı kullanıp Azure AD B2C korumalı web API’si çağırarak oturum açmak için Azure AD B2C etkin bir örnek tek sayfalı uygulama kullanırsınız.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* **ASP.NET ve web geliştirme** iş yüküyle [Visual Studio 2017](https://www.visualstudio.com/downloads/).
* [Node.js](https://nodejs.org/en/download/)’yi yükleme
* Bir Facebook, Google, Microsoft veya Twitter sosyal hesabından.

## <a name="download-the-sample"></a>Örneği indirme

GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip) veya örnek web uygulamasını kopyalayın.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

## <a name="run-the-sample-application"></a>Örnek uygulamayı çalıştırın

Bu örneği Node.js komut isteminden çalıştırmak için: 

```
cd active-directory-b2c-javascript-msal-singlepageapp
npm install && npm update
node server.js
```

Node.js uygulaması, localhost’ta dinlediği bağlantı noktası numarasını çıkarır.

```
Listening on port 6420...
```

Bir web tarayıcısından uygulamanızın URL’sine `http://localhost:6420` gözatın.

![Tarayıcıdaki örnek uygulama](media/active-directory-b2c-quickstarts-spa/sample-app-spa.png)

## <a name="create-an-account"></a>Hesap oluşturma

Bir Azure AD B2C ilkesini temel alarak Azure AD B2C **Kaydolma veya Oturum Açma** iş akışı başlatmak için **Oturum aç** düğmesine tıklayın. 

Örnek, sosyal kimlik sağlayıcısı kullanma veya e-posta kullanarak yerel hesap oluşturma gibi çeşitli oturum açma seçeneklerini destekler. Bu hızlı başlangıç için bir Facebook, Google, Microsoft veya Twitter’dan sosyal kimlik sağlayıcısı hesabı kullanın. 

### <a name="sign-up-using-a-social-identity-provider"></a>Sosyal kimlik sağlayıcısı kullanarak kaydolma

Azure AD B2C, örnek web uygulaması için Wingtip Toys adlı kurgusal bir markaya yönelik özel oturum açma sayfası gösterir. 

1. Sosyal kimlik sağlayıcısı kullanarak oturum açmak için kullanmak istediğiniz kimlik sağlayıcısının düğmesine tıklayın.

    ![Oturum Açma veya Kaydolma sağlayıcısı](media/active-directory-b2c-quickstarts-spa/sign-in-or-sign-up-spa.png)

    Sosyal hesap kimlik bilgilerinizi kullanarak kimliğinizi doğrular (oturum açmış) ve uygulamayı sosyal hesabınızdaki bilgileri okuması için yetkilendirirsiniz. Erişim verdiğiniz uygulama, adınız ve yaşadığınız şehir gibi profil bilgilerini sosyal hesabınızdan alabilir. 

2. Kimlik sağlayıcısı için oturum açma işlemini bitirin. Örneğin Twitter’ı seçerseniz, Twitter kimlik bilgilerinizi girin ve **Oturum Aç**’a tıklayın.

    ![Sosyal hesap kullanarak kimlik doğrulama ve yetkilendirme](media/active-directory-b2c-quickstarts-spa/twitter-authenticate-authorize-spa.png)

    Yeni hesap profilinizdeki bilgiler, sosyal hesabınızdaki bilgilerle önceden doldurulmuştur. 

3. Görünen Ad, İş Unvanı ve Şehir alanlarını güncelleştirip **Devam**’a tıklayın.  Girdiğiniz değerler, Azure AD B2C kullanıcı hesabı profiliniz için kullanılır.

    Kimlik sağlayıcısı kullanan yeni bir Azure AD B2C kullanıcı hesabını başarıyla oluşturdunuz. 

## <a name="access-a-protected-web-api-resource"></a>Korumalı bir web API’si kaynağına erişme

Görünen adınızın Web API’si çağrısından JSON nesnesi olarak döndürülmesi için **Web API’si Çağrısı** düğmesine tıklayın. 

![Web API’si yanıtı](media/active-directory-b2c-quickstarts-spa/call-api-spa.png)

Örnek tek sayfalı uygulama, web API’si kaynağına yapılan JSON nesnesini döndürme işlemini gerçekleştirme isteğinde Azure AD erişim belirteci içerir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Azure AD B2C hızlı başlangıçlarını veya öğreticilerini denemeyi planlıyorsanız Azure AD B2C kiracınızı kullanabilirsiniz. Artık ihtiyaç duymuyorsanız [Azure AD B2C kiracınızı silebilirsiniz](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, özel oturum açma sayfasıyla oturum açmak, sosyal kimlik sağlayıcısı ile oturum açmak, Azure AD B2C hesabı oluşturmak ve Azure AD B2C tarafından korunan bir web API’sini çağırmak için Azure AD B2C etkin bir örnek ASP.NET uygulaması kullandınız. 

Sonraki adımda kendi Azure AD B2C kiracınızı oluşturup kiracınızı kullanarak çalıştırmak için örneği yapılandıracaksınız. 

> [!div class="nextstepaction"]
> [Azure portalında Azure Active Directory B2C kiracısı oluşturma](active-directory-b2c-get-started.md)