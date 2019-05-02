---
title: Öğretici - kimlik doğrulamasını etkinleştirme tek sayfalı bir uygulamada - Azure Active Directory B2C | Microsoft Docs
description: Bir tek sayfalı uygulamada (JavaScript) kullanıcının oturum açmasını sağlamak için Azure Active Directory B2C’nin nasıl kullanılacağını gösteren öğretici.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.author: davidmu
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 9541d635ff69444459470cf1e486568a58af0a1e
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64730166"
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

* [Kullanıcı akışları oluşturma](tutorial-create-user-flows.md) uygulamanızdaki kullanıcı deneyimi sağlar. 
* **ASP.NET ve web geliştirme** iş yüküyle [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi yükleyin.
* Yükleme [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) veya üzeri
* [Node.js](https://nodejs.org/en/download/)’yi yükleme

## <a name="update-the-application"></a>Uygulamayı güncelleştirme

Önkoşulların bir parçası tamamlanan öğreticide, Azure AD B2C web uygulamasında eklendi. Öğreticideki örnek ile iletişimi etkinleştirmek için Azure AD B2C uygulamasında bir yeniden yönlendirme URI'si eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **uygulamaları**ve ardından *webapp1* uygulama.
5. Altında **yanıt URL'si**, ekleme `http://localhost:6420`.
6. **Kaydet**’i seçin.
7. Özellikler sayfasında, web uygulamasını yapılandırırken kullanacağınız uygulama Kimliğini kaydedin.
8. Seçin **anahtarları**seçin **anahtar üret**seçip **Kaydet**. Web uygulaması yapılandırırken kullanacağınız anahtar kaydedin.

## <a name="configure-the-sample"></a>Örnek yapılandırma

Bu öğreticide, Github'dan indirebileceğiniz örnek yapılandırın. Örnek nasıl bir tek sayfalı uygulama Azure AD B2C kullanıcı kaydolma, oturum açma ve kullanabileceğiniz korumalı web API'si çağırma gösterir.

GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip) veya örneği kopyalayın.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

Ayarları değiştirmek için:

1. Açık `index.html` örnek dosyasında.
2. Örnek uygulama kimliği ve daha önce kaydettiğiniz anahtarını yapılandırın. API'ler ve dizin adları ile değerlerini değiştirerek aşağıdaki kod satırlarını değiştirin:

    ```javascript
    // The current application coordinates were pre-registered in a B2C directory.
    var applicationConfig = {
        clientID: '<Application ID>',
        authority: "https://contoso.b2clogin.com/tfp/contoso.onmicrosoft.com/B2C_1_signupsignin1",
        b2cScopes: ["https://contoso.onmicrosoft.com/demoapi/demo.read"],
        webApi: 'https://contosohello.azurewebsites.net/hello',
    };
    ```

    Bu öğreticide kullanılan kullanıcı Akış adı **B2C_1_signupsignin1**. Farklı bir userjourney adı kullanıyorsanız, kullanıcı akışı adında kullanın `authority` değeri.

## <a name="run-the-sample"></a>Örneği çalıştırma

1. Bir Node.js komut istemi başlatın.
2. Node.js örneğini içeren dizine değiştirin. Örneğin `cd c:\active-directory-b2c-javascript-msal-singlepageapp`
3. Aşağıdaki komutları çalıştırın:

    ```
    npm install && npm update
    node server.js
    ```

    Konsol penceresi uygulamanın barındırıldığı bağlantı noktası numarasını görüntüler.
    
    ```
    Listening on port 6420...
    ```

4. Adresine gidin, bir tarayıcı kullanın `http://localhost:6420` uygulamayı görüntülemek için.

Parola sıfırlama ve kaydolma, oturum açma, profil düzenleme örneği destekler. Bu öğreticide, bir e-posta adresi kullanarak nasıl bir kullanıcı oturum açtığında vurgular.

### <a name="sign-up-using-an-email-address"></a>E-posta adresi kullanarak kaydolma

1. Tıklayın **oturum açma** uygulamasının bir kullanıcısı kaydolmak için. Bu kullanır **B2C_1_signupsignin1** kullanıcı akışı, bir önceki adımda tanımladığınız.
2. Azure AD B2C, kayıt bağlantısı içeren bir oturum açma sayfası sunar. Henüz bir hesabınız yoksa **Hemen kaydol** bağlantısına tıklayın. 
3. Kaydolma iş akışında, kullanıcı kimliğini bir e-posta adresi kullanarak toplamak ve doğrulamak için bir sayfa sunar. Kaydolma iş akışı, ayrıca kullanıcının parolasını ve kullanıcı Akış içinde tanımlanmış istenen öznitelikleri toplar.

    Geçerli bir e-posta adresi kullanın ve doğrulama kodunu kullanarak doğrulamayı gerçekleştirin. Parola ayarlayın. İstenen öznitelikler için değerleri girin. 

    ![Kaydolma iş akışı](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.png)

4. Azure AD B2C dizininde yerel bir hesap oluşturmak için **Oluştur**’a tıklayın.

Şimdi, kullanıcı, oturum açmak ve SPA uygulamasını kullanmak için bir e-posta adresini kullanabilir.

> [!NOTE]
> Oturum açtıktan sonra, uygulama bir “yetersiz izinler” hatası görüntülüyor. Bu hatayı, tanıtım dizinindeki bir kaynağa erişmeye çalıştığınız için alırsınız. Erişim belirteci yalnızca Azure AD dizininiz için geçerli olduğundan, API çağrısı yetkilendirilmez. Dizininiz için korumalı bir web API’si oluşturmak amacıyla sonraki öğreticiye geçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrendiğiniz nasıl yapılır:

> [!div class="checklist"]
> * Azure AD B2C uygulamasında güncelleştirme
> * Örnek uygulamayı yapılandırma
> * Kullanıcı akışı kullanarak kaydolma

> [!div class="nextstepaction"]
> [Öğretici: Azure Active Directory B2C kullanarak bir tek sayfalı uygulamasından erişime bir ASP.NET Core Web API'si](active-directory-b2c-tutorials-spa-webapi.md)
