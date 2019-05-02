---
title: Öğretici - yerel istemci uygulaması kimlik doğrulaması etkinleştirme - Azure Active Directory B2C | Microsoft Docs
description: .NET Masaüstü uygulama için kullanıcının oturum açmasını sağlamak için Azure Active Directory B2C kullanma Öğreticisi.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.author: davidmu
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 3df54c6805c5117e627afe0a2b4caa0ddd94b182
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64723709"
---
# <a name="tutorial-enable-authentication-in-a-native-client-application-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C kullanarak bir yerel istemci uygulaması ile kimlik doğrulamasını etkinleştirin

Bu öğreticide, bir Windows Presentation Foundation (WPF) masaüstü uygulamasında kullanıcının oturumunu açmak ve kullanıcıyı kaydetmek için Azure Active Directory (Azure AD) B2C’nin nasıl kullanılacağı gösterilir. Azure AD B2C, uygulamalarınızın sosyal medya hesaplarını, Kurumsal hesaplarda ve Azure Active Directory hesaplarını açık standart protokoller kullanarak kimlik doğrulaması sağlar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yerel istemci uygulaması Ekle
> * Örnek uygulamayı yapılandırma
> * Kullanıcı akışı kullanarak kaydolma

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

- [Kullanıcı akışları oluşturma](tutorial-create-user-flows.md) uygulamanızdaki kullanıcı deneyimi sağlar. 
- **.NET masaüstü geliştirme** ve **ASP.NET ve web geliştirme** iş yükleriyle [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi yükleyin.

## <a name="add-the-native-client-application"></a>Yerel istemci uygulaması Ekle

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **uygulamaları**ve ardından **Ekle**.
5. Uygulama için bir ad girin. Örneğin, *nativeapp1*.
6. İçin **içeren web uygulaması / web API'sini**seçin **Hayır**.
7. İçin **yerel istemciyi dahil et**seçin **Evet**.
8. İçin **yeniden yönlendirme URI'si**, özel bir düzen ile geçerli bir yeniden yönlendirme URI'si girin. Yeniden yönlendirme URI'si seçerken iki önemli noktalar vardır:

    - **Benzersiz** -yeniden yönlendirme URI'si şeması her uygulama için benzersiz olmalıdır. Örnekte `com.onmicrosoft.contoso.appname://redirect/path`, `com.onmicrosoft.contoso.appname` düzenidir. Bu düzen gelmelidir. İki uygulama aynı şemayı paylaşıyorsa, kullanıcı bir uygulama seçmek için bir seçenek verilir. Kullanıcı yanlış seçim yaparsa, oturum açma başarısız olur.
    - **Tam** -yeniden yönlendirme URI'SİNİN bir şeması ve yolu olmalıdır. Yol, etki alanından sonra en az bir eğik çizgi içermelidir. Örneğin, `//contoso/` çalışır ve `//contoso` başarısız olur. Yeniden yönlendirme URI'si, alt çizgi gibi özel karakterleri içermeyen emin olun.

9. **Oluştur**’a tıklayın.
10. Özellikler sayfasında örnek yapılandırırken kullanacağınız uygulama Kimliğini kaydedin.

## <a name="configure-the-sample"></a>Örnek yapılandırma

Bu öğreticide, Github'dan indirebileceğiniz örnek yapılandırın. Örnek WPF Masaüstü uygulamasını gösteren kaydolma, oturum açma ve içinde Azure AD B2C korumalı web API'sini çağırır. [Bir zip dosyası indirin](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop/archive/master.zip), [depoya göz atın](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop) veya GitHub’dan örneği kopyalayın.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop.git
```

Uygulama ayarlarını değiştirmek için değiştirme `<your-tenant-name>` kiracınızın adı, replace ile`<application-ID`> kaydettiğiniz uygulama kimliği.

1. `active-directory-b2c-wpf` çözümünü Visual Studio’da açın.
2. `active-directory-b2c-wpf` projesinde, **App.xaml.cs** dosyasını açın ve aşağıdaki güncelleştirmeleri yapın:

    ```C#
    private static string Tenant = "<your-tenant-name>.onmicrosoft.com";
    private static string ClientId = "<application-ID>";
    ```

3. Güncelleştirme **Policysignupsignın** değişken adıyla oluşturduğunuz kullanıcı akışı.

    ```C#
    public static string PolicySignUpSignIn = "B2C_1_signupsignin1";
    ```

## <a name="run-the-sample"></a>Örneği çalıştırma

Tuşuna **F5** oluşturup örneği çalıştırın.

### <a name="sign-up-using-an-email-address"></a>E-posta adresi kullanarak kaydolma

1. Tıklayın **oturum** kullanıcısı olarak kaydolmak için. Bu kullanır **B2C_1_signupsignin1** kullanıcı akışı.
2. Azure AD B2C, kayıt bağlantısı içeren bir oturum açma sayfası sunar. Henüz bir hesabınız yoksa **Hemen kaydol** bağlantısına tıklayın. 
3. Kaydolma iş akışında, kullanıcı kimliğini bir e-posta adresi kullanarak toplamak ve doğrulamak için bir sayfa sunar. Kaydolma iş akışı, ayrıca kullanıcının parolasını ve kullanıcı Akış içinde tanımlanmış istenen öznitelikleri toplar.

    Geçerli bir e-posta adresi kullanın ve doğrulama kodunu kullanarak doğrulamayı gerçekleştirin. Parola ayarlayın. İstenen öznitelikler için değerleri girin. 

    ![Kaydolma iş akışı](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.png)

4. Azure AD B2C kiracısında yerel bir hesap oluşturmak için **Oluştur**’a tıklayın.

Artık kullanıcı, oturum açmak ve masaüstü uygulamasını kullanmak için e-posta adresini kullanabilir.

> [!NOTE]
> **API’yi çağırma** düğmesine tıklarsanız "Yetkisiz" hatasıyla karşılaşırsınız. Bu hatayı, tanıtım kiracısındaki bir kaynağa erişmeye çalıştığınız için alırsınız. Erişim belirteci yalnızca Azure AD kiracınız için geçerli olduğundan, API çağrısı yetkilendirilmez. Kiracınız için korumalı bir web API’si oluşturmak amacıyla sonraki öğreticiye geçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Yerel istemci uygulaması Ekle
> * Örnek uygulamayı yapılandırma
> * Kullanıcı akışı kullanarak kaydolma

> [!div class="nextstepaction"]
> [Öğretici: Azure Active Directory B2C kullanarak bir masaüstü uygulamasından Node.js web API'si için verme erişim](active-directory-b2c-tutorials-spa-webapi.md)
