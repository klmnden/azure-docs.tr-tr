---
title: "Test Azure AD B2C tek sayfalı uygulama sürücü | Microsoft Docs"
description: "Test sürücü oturum açın, kaydolma, profili düzenlemek ve bir Azure AD B2C test ortamı'nı kullanarak kullanıcı Yolculuklar parola sıfırlama"
services: active-directory-b2c
documentationcenter: 
author: saraford
manager: krassk
editor: PatAltimore
ms.assetid: 5a8a46af-28bb-4b70-a7f0-01a5240d0255
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 10/31/2017
ms.author: saraford
ms.openlocfilehash: 22da1ae317ba685d32f93d3331cf794b568891ec
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="test-drive-a-single-page-application-configured-with-azure-ad-b2c"></a>Test sürücü Azure AD B2C ile yapılandırılmış bir tek sayfalı uygulama

## <a name="about-this-sample"></a>Bu örnek hakkında

Azure Active Directory B2C uygulaması, iş ve korumalı müşteriler tutmak için bulut kimlik yönetimi sağlar.  Bu hızlı başlangıç örnek bir tek sayfalı uygulama göstermek için kullanır:

* Kullanarak **kaydolun veya oturum** ilkesi oluşturun veya bir sosyal kimlik sağlayıcısı veya bir e-posta adresi kullanarak yerel bir hesapla oturum açın. 
* **Bir API çağırma** görünen adınızı bir Azure AD B2C almak için kaynak güvenli.

## <a name="prerequisites"></a>Ön koşullar

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi aşağıdaki iş yükleri ile yükleyin:
    - **ASP.NET ve web geliştirme**

* [Node.js](https://nodejs.org/en/download/)’yi yükleme

* Facebook, Google, Microsoft veya Twitter sosyal hesabından. Sosyal bir hesabınız yoksa, geçerli bir eposta adresi gereklidir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Örneği indirme

[İndirme veya örnek uygulama kopyalama](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp) github'dan.

## <a name="run-the-sample-application"></a>Örnek uygulamayı çalıştırın

Bu örnekte Node.js komut isteminden çalıştırmak için: 

```
cd active-directory-b2c-javascript-msal-singlepageapp
npm install && npm update
node server.js
```

Konsol penceresi, bilgisayarınızda çalışan web uygulaması için bağlantı noktası numarasını gösterir.

```
Listening on port 6420...
```

Açık `http://localhost:6420` web uygulamasına erişmek için bir web tarayıcısında.


![Örnek uygulamayı tarayıcıda](media/active-directory-b2c-quickstarts-spa/sample-app-spa.png)

## <a name="create-an-account"></a>Bir hesap oluşturun

Tıklatın **oturum açma** Azure AD B2C başlatmak için düğmesini **kaydolun veya oturum** iş akışı. Bir hesap oluşturulurken, var olan sosyal kimlik sağlayıcısı hesabını veya bir e-posta hesabı kullanabilirsiniz.

### <a name="sign-up-using-a-social-identity-provider"></a>Sosyal kimlik sağlayıcısı kullanarak kaydolun

Sosyal kimlik sağlayıcısı kullanarak oturum açmak için kullanmak istediğiniz kimlik sağlayıcısı düğmesine tıklayın. Bir e-posta adresi kullanmayı tercih ederseniz, atlamak [bir e-posta adresi kullanarak kaydolma](#sign-up-using-an-email-address) bölümü.

![Oturum açın veya kaydolun sağlayıcısı](media/active-directory-b2c-quickstarts-spa/sign-in-or-sign-up-spa.png)

Sosyal hesabınızın kimlik bilgileri ve sosyal hesabınızdan bilgileri okumak için uygulamayı yetkilendirmeniz (oturum açma) kullanarak kimliğini doğrulaması gerekir. Erişim vererek, uygulama adı ve şehir gibi sosyal hesabından profil bilgilerini alabilir. 

![Kimlik doğrulama ve sosyal bir hesabı kullanarak Yetkilendirme](media/active-directory-b2c-quickstarts-spa/twitter-authenticate-authorize-spa.png)

Yeni hesap profili bilgilerinizi sosyal hesabınızdan bilgilerle önceden doldurulmuş haldedir. 

![Yeni hesap kayıt profili ayrıntıları](media/active-directory-b2c-quickstarts-spa/new-account-sign-up-profile-details-spa.png)

Görünen ad, iş unvanı ve şehir alanları güncelleştirin ve tıklayın **devam**.  Girdiğiniz değerler Azure AD B2C kullanıcı hesabı profiliniz için kullanılır.

Bir kimlik sağlayıcısı kullanan yeni bir Azure AD B2C kullanıcı hesabını başarıyla oluşturdunuz. 

Sonraki adım: [bir kaynak çağrısına](#call-a-resource) bölümü.

### <a name="sign-up-using-an-email-address"></a>Bir e-posta adresi kullanarak kaydolun

Sosyal hesabı kimlik doğrulaması kullanmayı seçerseniz, geçerli bir eposta adresi kullanarak bir Azure AD B2C kullanıcı hesabı oluşturabilirsiniz. Bir Azure AD B2C yerel kullanıcı hesabı kimlik sağlayıcısı olarak Azure Active Directory kullanır. E-posta adresinizi kullanmak için tıklatın **bir hesabınız yok mu? Şimdi kaydolun** bağlantı.

![Oturum açın veya e-posta kullanarak kaydolun](media/active-directory-b2c-quickstarts-spa/sign-in-or-sign-up-email-spa.png)

Geçerli bir e-posta adresi girin ve tıklayın **Gönder doğrulama kodu**. Geçerli bir e-posta adresi, Azure AD B2C ' doğrulama kodu almak için gereklidir. 

E-posta almak ve tıklatın doğrulama kodunu girin **kodunu doğrulayın**.

Profil bilgilerinizi ekleyin ve tıklatın **oluşturma**.

![E-posta kullanarak yeni hesabıyla kaydolun](media/active-directory-b2c-quickstarts-spa/sign-up-new-account-profile-email-web.png)

Yeni bir Azure AD B2C yerel kullanıcı hesabı başarıyla oluşturdunuz.

## <a name="call-a-resource"></a>Bir kaynak çağırın

Oturum açtıktan sonra tıklayabilirsiniz **Web API çağrısı** bir JSON nesnesi olarak Web API çağrısından döndürülen görünen adınızı olmasını düğmesi. 

![Web API yanıtı](media/active-directory-b2c-quickstarts-spa/call-api-spa.png)

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adım, Kiracı kullanarak çalıştırmak için örnek kendi Azure AD B2C kiracısı oluşturma ve yapılandırmaktır. 

> [!div class="nextstepaction"]
> [Azure portalında bir Azure Active Directory B2C kiracısı oluşturma](active-directory-b2c-get-started.md)