---
title: "Test sürücü bir Azure AD B2C'ın etkin web uygulaması | Microsoft Docs"
description: "Test sürücü oturum açın, kaydolma, profili düzenlemek ve bir Azure AD B2C test ortamı'nı kullanarak kullanıcı Yolculuklar parola sıfırlama"
services: active-directory-b2c
documentationcenter: .net
author: saraford
manager: krassk
editor: PatAltimore
ms.assetid: 2ffb780d-2c51-4c2e-b8d6-39c40a81a77e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/31/2017
ms.author: patricka
ms.openlocfilehash: 07f2c21409176d30f4570e267a4472745f843f85
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2017
---
# <a name="test-drive-an-azure-ad-b2c-enabled-web-app"></a>Web uygulaması test sürücü bir Azure AD B2C etkin

Azure Active Directory B2C uygulaması, iş ve korumalı müşteriler tutmak için bulut kimlik yönetimi sağlar. Bu hızlı başlangıç örnek bir Yapılacaklar listesi uygulaması göstermek için kullanır:

> [!div class="checklist"]
> * Oturum ile özel oturum açma sayfası açın.
> * Sosyal kimlik sağlayıcısı kullanarak oturum açın.
> * Oluşturma ve Azure AD B2C hesabı ve kullanıcı profili yönetme.
> * Bir web API'sini Azure AD B2C tarafından güvence altına çağrılıyor.

## <a name="prerequisites"></a>Ön koşullar

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) ile **ASP.NET ve web geliştirme** iş yükü. 
* Facebook, Google, Microsoft veya Twitter sosyal hesabından.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Örneği indirme

[İndirme veya örnek uygulama kopyalama](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi) github'dan.

## <a name="run-the-app-in-visual-studio"></a>Visual Studio'da Uygulama Çalıştırma

Örnek uygulama proje klasöründe açın `B2C-WebAPI-DotNet.sln` Visual Studio'da çözüm. 

Çözüm, iki projeden oluşan bir örnek Yapılacaklar listesi uygulamasıdır:

* **TaskWebApp** – burada kullanıcının yönetebileceği kendi Yapılacaklar listesi öğeleri bir ASP.NET MVC web uygulaması.  
* **TaskService** – işlemlerini yöneten bir ASP.NET Web API arka uç kullanıcının yapılacaklar listesi öğelerini üzerinde gerçekleştirilir. Web uygulaması bu web API'si çağıran ve sonuçları görüntüler.

Bu Hızlı Başlangıç için her ikisini de çalıştırmanız gerekir. `TaskWebApp` ve `TaskService` aynı anda projeleri. 

1. Visual Studio menüsünde seçin **projeleri > başlangıç projelerini Ayarla...** . 
2. Seçin **birden fazla başlangıç projesi** radyo düğmesi.
3. Değişiklik **eylem** için her iki projeye de **Başlat**. **Tamam** düğmesine tıklayın.

![Visual Studio Başlangıç Sayfası Ayarla](media/active-directory-b2c-quickstarts-web-app/setup-startup-projects.png)

Seçin **hata ayıklama > hata ayıklamayı Başlat** oluşturun ve her iki uygulamayı çalıştırın. Her uygulamanın kendi bir tarayıcı sekmesi açar:

`https://localhost:44316/`-Bu sayfaya ASP.NET web uygulamasıdır. Bu hızlı başlangıç uygulamada ile doğrudan etkileşim.
`https://localhost:44332/`-Bu ASP.NET web uygulaması tarafından çağrılan API web sayfasıdır.

## <a name="create-an-account"></a>Bir hesap oluşturun

Tıklatın **kaydolun / oturum** başlatmak için ASP.NET web uygulamasının bağlantı **kaydolun veya oturum** iş akışı. Bir hesap oluşturulurken, var olan sosyal kimlik sağlayıcısı hesabını veya bir e-posta hesabı kullanabilirsiniz. Bu Hızlı Başlangıç için bir Facebook, Google, Microsoft veya Twitter sosyal kimlik sağlayıcısı hesabını kullanın.

![Örnek ASP.NET web uygulaması](media/active-directory-b2c-quickstarts-web-app/web-app-sign-in.png)

### <a name="sign-up-using-a-social-identity-provider"></a>Sosyal kimlik sağlayıcısı kullanarak kaydolun

Sosyal kimlik sağlayıcısı kullanarak oturum açmak için kullanmak istediğiniz kimlik sağlayıcısı düğmesine tıklayın. 

![Oturum açın veya kaydolun sağlayıcısı](media/active-directory-b2c-quickstarts-web-app/sign-in-or-sign-up-web.png)

Sosyal hesabınızın kimlik bilgileri ve sosyal hesabınızdan bilgileri okumak için uygulamayı yetkilendirmeniz (oturum açma) kullanarak kimliğini doğrulaması gerekir. Erişim vererek, uygulama adı ve şehir gibi sosyal hesabından profil bilgilerini alabilir. 

Oturum açma işleminin kimlik sağlayıcısı için Son'u tıklatın. Örneğin, Twitter seçerseniz, Twitter kimlik bilgilerinizi girin ve tıklatın **oturum**.

![Kimlik doğrulama ve sosyal bir hesabı kullanarak Yetkilendirme](media/active-directory-b2c-quickstarts-web-app/twitter-authenticate-authorize-web.png)

Yeni Azure AD B2C hesap profili bilgilerinizi sosyal hesabınızdan bilgilerle önceden doldurulmuş haldedir.

Görünen ad, iş unvanı ve şehir alanları güncelleştirin ve tıklayın **devam**.  Girdiğiniz değerler Azure AD B2C kullanıcı hesabı profiliniz için kullanılır.

![Yeni hesap kayıt profili ayrıntıları](media/active-directory-b2c-quickstarts-web-app/new-account-sign-up-profile-details-web.png)

Başarıyla vardır:

> [!div class="checklist"]
> * Bir kimlik sağlayıcısı kullanılarak kimlik doğrulaması.
> * Bir Azure AD B2C kullanıcı hesabı oluşturuldu. 

## <a name="edit-your-profile"></a>Profilinizi düzenleme

Azure Active Directory B2C profillerini güncelleştirme yapmalarına izin vermek için işlevsellik sağlar. Web uygulaması menü çubuğunda, profil adına tıklayın ve seçin **Düzenle profili** oluşturduğunuz profili düzenlemek için.

![Profili düzenleme](media/active-directory-b2c-quickstarts-web-app/edit-profile-web.png)

Değişiklik, **görünen adı** ve **Şehir**.  Tıklatın **devam** profilinizi güncelleştirmek için.

![Profil güncelleştirme](media/active-directory-b2c-quickstarts-web-app/update-profile-web.png)

Görünen adınızı sayfanın sağ üst kısmında gösterilen güncelleştirilmiş adı dikkat edin. 

## <a name="access-a-secured-web-api-resource"></a>Güvenli web API kaynak erişimi

Tıklatın **Yapılacaklar listesi** girin ve yapılacaklar listesi öğelerinizi değiştirmek için. ASP.NET web uygulaması, web API kaynak isteme izni kullanıcının yapılacaklar listesi öğeleri üzerinde işlem gerçekleştirmek için isteğinde bir erişim belirteci içerir. 

Metin girin **yeni öğe** metin kutusu. Tıklatın **Ekle** Azure AD B2C çağırmak için bir Yapılacaklar listesi öğesini ekler web API güvenliği.

![Bir Yapılacaklar listesi öğesi ekleme](media/active-directory-b2c-quickstarts-web-app/add-todo-item-web.png)

Bir Azure AD B2C güvenli web API yetkili bir arama yapmak için Azure AD B2C kullanıcı hesabınız başarıyla kullanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç içinde kullanılan örnek dahil olmak üzere diğer Azure AD B2C senaryoları denemek için kullanılabilir:

* Bir e-posta adresi kullanarak yeni bir yerel hesap oluşturma.
* Yerel hesap parolanızı sıfırlama.

Kendi Azure AD B2C kiracısı oluşturma içine inceleyin ve kendi Kiracı kullanarak çalıştırmak için örnek yapılandırma için hazır olduğunuzda aşağıdaki öğretici deneyin.

> [!div class="nextstepaction"]
> [Azure Active Directory B2C kaydolma, oturum açma profili düzenleme ve parola sıfırlama ile bir ASP.NET web uygulaması oluşturma](active-directory-b2c-devquickstarts-web-dotnet-susi.md)