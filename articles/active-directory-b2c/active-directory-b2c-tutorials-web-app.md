---
title: Öğretici - kimlik doğrulamasını etkinleştirme bir web uygulaması - Azure Active Directory B2C | Microsoft Docs
description: Bir ASP.NET web uygulamasında kullanıcının oturum açmasını sağlamak için Azure Active Directory B2C’nin nasıl kullanılacağını gösteren öğretici.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 041bcf32035ab6cdc3ee4df06050f75186759f5e
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67835642"
---
# <a name="tutorial-enable-authentication-in-a-web-application-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C kullanarak bir web uygulaması ile kimlik doğrulamasını etkinleştirin

Bu öğreticide Azure Active Directory (Azure AD) B2C oturum açın ve bir ASP.NET web uygulamasında kullanıcı için nasıl kullanılacağını gösterir. Azure AD B2C, uygulamalarınızın sosyal medya hesaplarını, Kurumsal hesaplarda ve Azure Active Directory hesaplarını açık standart protokoller kullanarak kimlik doğrulaması sağlar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD B2C uygulamasında güncelleştirme
> * Örnek uygulamayı yapılandırma
> * Kullanıcı akışı kullanarak kaydolma

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

- [Kullanıcı akışları oluşturma](tutorial-create-user-flows.md) uygulamanızdaki kullanıcı deneyimi sağlar.
- Yükleme [Visual Studio 2019](https://www.visualstudio.com/downloads/) ile **ASP.NET ve web geliştirme** iş yükü.

## <a name="update-the-application"></a>Uygulamayı güncelleştirme

Önkoşulların bir parçası tamamlanan öğreticide, Azure AD B2C web uygulamasında eklendi. Bu öğreticideki örnek ile iletişimi etkinleştirmek için Azure AD B2C uygulamasında bir yeniden yönlendirme URI'si eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **uygulamaları**ve ardından *webapp1* uygulama.
5. Altında **yanıt URL'si**, ekleme `https://localhost:44316`.
6. **Kaydet**’i seçin.
7. Özellikler sayfasında, web uygulamasını yapılandırırken kullanacağınız uygulama Kimliğini kaydedin.
8. Seçin **anahtarları**seçin **anahtar üret**seçip **Kaydet**. Web uygulaması yapılandırırken kullanacağınız anahtar kaydedin.

## <a name="configure-the-sample"></a>Örnek yapılandırma

Bu öğreticide, Github'dan indirebileceğiniz örnek yapılandırın. Örnek ASP.NET basit bir Yapılacaklar listesi sağlamak için kullanır. Örnek kullanır [Microsoft OWIN ara yazılımı bileşenleri](https://docs.microsoft.com/aspnet/aspnet/overview/owin-and-katana/). GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi/archive/master.zip) veya örneği kopyalayın. Örnek dosyayı, yolunun toplam karakter uzunluğu 260'tan az olan bir klasöre ayıkladığınızdan emin olun.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Örnek çözümde aşağıdaki iki projeler şunlardır:

- **TaskWebApp** - oluşturma ve görev listesini düzenleyin. Örnek kullanır **kaydolma veya oturum açma** kaydolma veya kullanıcılarının oturumunu kullanıcı akışı.
- **TaskService** - destekleyen oluşturma, okuma, güncelleştirme ve silme görev listesi işlevi. API'si Azure AD B2C tarafından korunan ve TaskWebApp tarafından çağrılır.

Uygulama kimliği ve daha önce kaydettiğiniz anahtarını içeren kiracınızdaki, kayıtlı uygulamayı kullanmak için örnek değiştirin. Ayrıca, oluşturduğunuz kullanıcı akışları de yapılandırın. Örnek yapılandırma değerlerini Web.config dosyasındaki ayarları olarak tanımlar. Ayarları değiştirmek için:

1. **B2C-WebAPI-DotNet** çözümünü Visual Studio’da açın.
2. İçinde **TaskWebApp** projesini açarsanız **Web.config** dosya. `ida:Tenant` değerini oluşturduğunuz kiracının adıyla değiştirin. `ida:ClientId` değerini kaydettiğiniz uygulama kimliğiyle değiştirin. `ida:ClientSecret` değerini kaydettiğiniz anahtarla değiştirin.
3. **Web.config** dosyasında `ida:SignUpSignInPolicyId` değerini `b2c_1_signupsignin1` ile değiştirin. `ida:EditProfilePolicyId` değerini `b2c_1_profileediting1` ile değiştirin. `ida:ResetPasswordPolicyId` değerini `b2c_1_passwordreset1` ile değiştirin.

## <a name="run-the-sample"></a>Örneği çalıştırma

1. Çözüm Gezgini'nde sağ **TaskWebApp** proje ve ardından **başlangıç projesi olarak ayarla**.
2. Tuşuna **F5**. Varsayılan tarayıcı, `https://localhost:44316/` olan yerel web sitesi adresini açar.

### <a name="sign-up-using-an-email-address"></a>E-posta adresi kullanarak kaydolma

1. Tıklayın **kaydolun / oturum aç** uygulamasının bir kullanıcısı kaydolmak için. **B2c_1_signupsignin1** kullanıcı akışı kullanılır.
2. Azure AD B2C, kayıt bağlantısı içeren bir oturum açma sayfası sunar. Henüz bir hesabınız yoksa bu yana seçin **şimdi kaydolun**. Kaydolma iş akışında, kullanıcı kimliğini bir e-posta adresi kullanarak toplamak ve doğrulamak için bir sayfa sunar. Kaydolma iş akışı, ayrıca kullanıcının parolasını ve kullanıcı Akış içinde tanımlanmış istenen öznitelikleri toplar.
3. Geçerli bir e-posta adresi kullanın ve doğrulama kodunu kullanarak doğrulamayı gerçekleştirin. Parola ayarlayın. İstenen öznitelikler için değerleri girin.

    ![Oturum-içindeki açma/kaydolma iş akışının parçası olarak gösterilen kaydolma sayfası](media/active-directory-b2c-tutorials-web-app/sign-up-workflow.PNG)

4. Azure AD B2C kiracısında yerel bir hesap oluşturmak için **Oluştur**’a tıklayın.

Artık kullanıcı, oturum açmak ve web uygulamasını kullanmak için e-posta adresi kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure AD B2C uygulamasında güncelleştirme
> * Örnek uygulamayı yapılandırma
> * Kullanıcı akışı kullanarak kaydolma

> [!div class="nextstepaction"]
> [Öğretici: Bir ASP.NET web API'sini korumak için Azure Active Directory B2C'yi kullanın](active-directory-b2c-tutorials-web-api.md)
