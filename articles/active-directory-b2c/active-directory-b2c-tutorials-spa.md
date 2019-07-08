---
title: Öğretici - kimlik doğrulamasını etkinleştirme tek sayfalı bir uygulamada - Azure Active Directory B2C
description: Azure Active Directory B2C'in bir tek sayfalı uygulamada (JavaScript) kullanıcının oturum açmasını sağlamak için kullanmayı öğrenin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 07/08/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 6824cc84c24b41fd82afd39ead3029a212173948
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67624786"
---
# <a name="tutorial-enable-authentication-in-a-single-page-application-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C kullanarak tek sayfalı uygulama kimlik doğrulamasını etkinleştirin

Bu öğreticide Azure Active Directory (Azure AD) B2C oturumu açmak ve tek sayfalı uygulama (SPA) kullanıcıları için nasıl kullanılacağını gösterir. Azure AD B2C, uygulamalarınızın sosyal medya hesaplarını, Kurumsal hesaplarda ve Azure Active Directory hesaplarını açık standart protokoller kullanarak kimlik doğrulaması sağlar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD B2C uygulamasında güncelleştirme
> * Örnek uygulamayı yapılandırma
> * Kullanıcı akışı kullanarak kaydolma

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki Azure AD B2C kaynaklara yerinde adımlara devam etmeden önce bu öğreticide şunlar gerekir:

* [Azure AD B2C kiracısı](tutorial-create-tenant.md)
* [Kayıtlı uygulama](tutorial-register-applications.md) kiracınızdaki
* [Oluşturulan kullanıcı akışları](tutorial-create-user-flows.md) kiracınızdaki

Ayrıca, yerel geliştirme ortamınızda aşağıdakiler gerekir:

* Kod Düzenleyicisi, örneğin [Visual Studio Code](https://code.visualstudio.com/) veya [Visual Studio 2019](https://www.visualstudio.com/downloads/)
* [.NET core SDK 2.0.0](https://www.microsoft.com/net/core) veya üzeri
* [Node.js](https://nodejs.org/en/download/)

## <a name="update-the-application"></a>Uygulamayı güncelleştirme

Önkoşulların bir parçası tamamlanan ikinci öğreticide bir Azure AD B2C web uygulamasında kayıtlı. Öğreticideki örnek ile iletişimi etkinleştirmek için Azure AD B2C uygulamasında bir yeniden yönlendirme URI'si eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
1. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
1. Seçin **uygulamaları**ve ardından *webapp1* uygulama.
1. Altında **yanıt URL'si**, ekleme `http://localhost:6420`.
1. **Kaydet**’i seçin.
1. Özellikler sayfasında, kayıt **uygulama kimliği**. Tek sayfa web uygulamasındaki kod güncelleştirdiğinizde, daha sonraki bir adımda uygulama Kimliğini kullanın.

## <a name="get-the-sample-code"></a>Örnek kodunu alma

Bu öğreticide, Github'dan indirin bir kod örneği, yapılandırın. Örnek kullanıcı kaydolmak için bir tek sayfalı uygulama Azure AD B2C'yi nasıl kullanabileceğiniz ve oturum açın ve korumalı web API çağrısına gösterir.

GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip) veya örneği kopyalayın.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

## <a name="update-the-sample"></a>Örnek güncelleştirme

Örnek elde, Azure AD B2C kiracınızın adının ve daha önceki bir adımda kaydettiğiniz uygulama kimliği ile kodu güncelleştirin.

1. Açık `index.html` örnek dizin kökündeki dosya.
1. İçinde `msalConfig` tanımını değiştirme **ClientID** uygulama kimliği ile bir önceki adımda kaydettiğiniz değeri. Ardından, güncelleştirme **yetkilisi** ile Azure AD B2C kiracınızın adının URI değeri. Ayrıca URI önkoşulları birinde oluşturduğunuz kullanıcı oturumu açma kaydolma/oturum açma akışını adını güncelleştirin (örneğin, *B2C_1_signupsignin1*).

    ```javascript
    var msalConfig = {
        auth: {
            clientId: "00000000-0000-0000-0000-000000000000", //This is your client ID
            authority: "https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/b2c_1_susi", //This is your tenant info
            validateAuthority: false
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    };
    ```

    Bu öğreticide kullanılan kullanıcı Akış adı **B2C_1_signupsignin1**. Farklı bir userjourney adı kullanıyorsanız, ad belirtin `authority` değeri.

## <a name="run-the-sample"></a>Örneği çalıştırma

1. Bir konsol penceresi açın ve örnek içeren dizine geçin. Örneğin:

    ```console
    cd active-directory-b2c-javascript-msal-singlepageapp
    ```
1. Aşağıdaki komutları çalıştırın:

    ```
    npm install && npm update
    node server.js
    ```

    Konsol penceresi, yerel olarak çalışan Node.js sunucusu bağlantı noktası numarasını görüntüler:

    ```
    Listening on port 6420...
    ```

1. Gidin `http://localhost:6420` uygulamayı görüntülemek için tarayıcınızda.

Parola sıfırlama ve kaydolma, oturum açma, profil düzenleme örneği destekler. Bu öğreticide, bir e-posta adresi kullanarak nasıl bir kullanıcı oturum açtığında vurgular.

### <a name="sign-up-using-an-email-address"></a>E-posta adresi kullanarak kaydolma

1. Tıklayın **oturum açma** uygulamasının bir kullanıcısı kaydolmak için. Bu kullanır **B2C_1_signupsignin1** bir önceki adımda belirttiğiniz kullanıcı akışı.
1. Azure AD B2C, kayıt bağlantısı içeren bir oturum açma sayfası sunar. Henüz bir hesabınız yoksa **Hemen kaydol** bağlantısına tıklayın.
1. Kaydolma iş akışında, kullanıcı kimliğini bir e-posta adresi kullanarak toplamak ve doğrulamak için bir sayfa sunar. Kaydolma iş akışı, ayrıca kullanıcının parolasını ve kullanıcı Akış içinde tanımlanmış istenen öznitelikleri toplar.

    Geçerli bir e-posta adresi kullanın ve doğrulama kodunu kullanarak doğrulamayı gerçekleştirin. Parola ayarlayın. İstenen öznitelikler için değerleri girin.

    ![Kaydolma iş akışı](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.png)

1. Azure AD B2C dizininde yerel bir hesap oluşturmak için **Oluştur**’a tıklayın.

Tıkladığınızda **Oluştur**kayıt sayfasını kapatır ve oturum açma sayfasına yeniden görüntülenir.

Artık, uygulamada oturum açmak için e-posta adresinizi ve parolanızı kullanabilirsiniz.

### <a name="error-insufficient-permissions"></a>Hata: yetersiz izin

Oturum açtığınızda, uygulama izinleri yetersiz hatası görüntüler - bu sonra **beklenen**:

`ServerError: AADB2C90205: This application does not have sufficient permissions against this web resource to perform the operation.`

Tanıtım dizinden bir kaynağa erişmeye çalıştığınız, ancak yalnızca Azure AD dizininiz için erişim belirteci geçerliyse bu hatayı alırsınız. API çağrısı, bu nedenle yetkisiz gereklidir.

Serideki sonraki öğretici ile devam edin (bkz [sonraki adımlar](#next-steps)) dizininiz için korumalı web API'si oluşturma.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrendiğiniz nasıl yapılır:

> [!div class="checklist"]
> * Azure AD B2C uygulamasında güncelleştirme
> * Örnek uygulamayı yapılandırma
> * Kullanıcı akışı kullanarak kaydolma

Artık serideki sonraki öğretici SPA korumalı web API'sine erişim vermek ilerleyin:

> [!div class="nextstepaction"]
> [Öğretici: Azure Active Directory B2C kullanarak bir tek sayfalı uygulamasından erişime bir ASP.NET Core Web API'si](active-directory-b2c-tutorials-spa-webapi.md)
