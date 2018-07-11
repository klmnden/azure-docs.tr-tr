---
title: "Hızlı başlangıç: Azure Active Directory B2C'yi kullanarak tek sayfalı bir uygulama için oturum açma bilgilerini ayarlama | Microsoft Docs"
description: Hesap oturum açma bilgileri sağlamak için Azure Active Directory B2C’yi kullanan örnek bir tek sayfalı uygulama çalıştırın.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: quickstart
ms.date: 2/13/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7e600f55b9eeb2682b005b8c84c74ec25e054c36
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37447260"
---
# <a name="quickstart-set-up-sign-in-for-a-single-page-app-using-azure-active-directory-b2c"></a>Hızlı başlangıç: Azure Active Directory B2C'yi kullanarak tek sayfalı bir uygulama için oturum açma bilgilerini ayarlama

Azure Active Directory (Azure AD) B2C, uygulamanız, işletmeniz ve müşterileriniz için koruma sağlamak üzere bulut kimliği yönetimi sunar. Azure AD B2C, uygulamalarınızın sosyal ağ hesaplarında ve kurumsal hesaplarda açık standart protokoller kullanarak kimlik doğrulaması yapmasına izin verir.

Bu hızlı başlangıçta, sosyal kimlik sağlayıcısı kullanıp Azure AD B2C korumalı web API’si çağırarak oturum açmak için Azure AD B2C etkin bir örnek tek sayfalı uygulama kullanırsınız.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* **ASP.NET ve web geliştirme** iş yüküyle [Visual Studio 2017](https://www.visualstudio.com/downloads/).
* [Node.js](https://nodejs.org/en/download/)’yi yükleme
* Facebook’tan, Google’dan, Microsoft’tan veya Twitter’dan bir sosyal hesap.

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

Örnek, sosyal kimlik sağlayıcısı kullanma veya e-posta kullanarak yerel hesap oluşturma gibi çeşitli oturum açma seçeneklerini destekler. Bu hızlı başlangıç için Facebook’tan, Google’dan, Microsoft’tan veya Twitter’dan bir sosyal kimlik sağlayıcısı hesabı kullanın. 

### <a name="sign-up-using-a-social-identity-provider"></a>Sosyal kimlik sağlayıcısı kullanarak kaydolma

Azure AD B2C, örnek web uygulaması için Wingtip Toys adlı bir kurgusal markaya yönelik özel bir oturum açma sayfası sunar. 

1. Sosyal kimlik sağlayıcısı kullanarak kaydolmak için, kullanmak istediğiniz kimlik sağlayıcısının düğmesine tıklayın.

    ![Oturum Açma veya Kaydolma sağlayıcısı](media/active-directory-b2c-quickstarts-spa/sign-in-or-sign-up-spa.png)

    Sosyal hesap kimlik bilgilerinizi kullanarak kimlik doğrulaması (oturum açma) gerçekleştirir ve uygulamaya sosyal hesabınızdaki bilgileri okuma yetkisi verirsiniz. Erişim izni verdiğinizde uygulama sosyal hesabınızdan adınız ve şehriniz gibi profil bilgilerini alabilir. 

2. Kimlik sağlayıcısına ilişkin oturum açma işlemini tamamlayın. Örneğin, Twitter’i seçtiyseniz Twitter kimlik bilgilerinizi girin **Oturum aç**’a tıklayın.

    ![Sosyal hesap kullanarak kimlik doğrulaması ve yetkilendirme gerçekleştirme](media/active-directory-b2c-quickstarts-spa/twitter-authenticate-authorize-spa.png)

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

Bu hızlı başlangıçta özel bir oturum açma sayfasıyla oturum açmak, sosyal kimlik sağlayıcısı ile oturum açmak, bir Azure AD B2C hesabı oluşturmak ve Azure AD B2C tarafından korunan bir web API’sini çağırmak için Azure AD B2C etkin bir örnek ASP.NET uygulaması kullandınız. 

Sonraki adımda kendi Azure AD B2C kiracınızı oluşturup kiracınızı kullanarak çalıştırmak için örneği yapılandıracaksınız. 

> [!div class="nextstepaction"]
> [Azure portalında Azure Active Directory B2C kiracısı oluşturma](tutorial-create-tenant.md)