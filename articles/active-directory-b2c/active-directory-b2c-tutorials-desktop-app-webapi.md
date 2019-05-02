---
title: Öğretici - erişim izni verme Node.js web API'si bir masaüstü uygulamasından - Azure Active Directory B2C | Microsoft Docs
description: Bir Node.js web API’sini korumak ve bir .NET masaüstü uygulamasından çağırmak için Active Directory B2C kullanmaya yönelik öğretici.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.author: davidmu
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: ef48c0f78083217581594b481b15a74a49fef4f9
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64715849"
---
# <a name="tutorial-grant-access-to-a-nodejs-web-api-from-a-desktop-app-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C kullanarak bir masaüstü uygulamasından Node.js web API'si için verme erişim

Bu öğreticide bir Windows Presentation Foundation (WPF) masaüstü uygulamasından bir Azure Active Directory (Azure AD) B2C korumalı Node.js web API kaynağını çağırma gösterilmektedir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Web API'si uygulaması ekleme
> * Web API'si için Kapsamları yapılandırma
> * Web API'sine izinler verin
> * Örnek uygulamayı güncelleştirme

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Önkoşullar ve adımlar tamamlamak [Öğreticisi: Azure Active Directory B2C'yi kullanan hesaplarla Masaüstü uygulama kimlik doğrulamasını etkinleştirme](active-directory-b2c-tutorials-desktop-app.md).

## <a name="add-a-web-api-application"></a>Web API'si uygulaması ekleme

Web API'si kaynaklarına kabul edebilir ve korunan kaynak isteklerini yanıtlamak önce kiracınızda bir erişim belirteci mevcut istemci uygulamalar tarafından kayıtlı olması gerekir. 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **uygulamaları**ve ardından **Ekle**.
5. Uygulama için bir ad girin. Örneğin, *webapi1*.
6. İçin **içeren web uygulaması / web API'sini** ve **örtük akışa izin ver**seçin **Evet**.
7. İçin **yanıt URL'si**, Azure AD B2C, uygulamanız tarafından istenen belirteçleri döndürdüğü bir uç noktasını girin. Bu öğreticide örnek yerel olarak çalışır ve adresindeki dinler `https://localhost:5000`.
8. İçin **uygulama kimliği URI'si**, web API'niz için kullanılan tanımlayıcı girin. Tam etki alanı ile birlikte URI tanımlayıcısı sizin için oluşturulur. Örneğin, `https://contosotenant.onmicrosoft.com/api`.
9. **Oluştur**’a tıklayın.
10. Özellikler sayfasında, web uygulamasını yapılandırırken kullanacağınız uygulama Kimliğini kaydedin.

## <a name="configure-scopes"></a>Kapsamlarını yapılandırma

Kapsamlar korumalı kaynaklara erişimi yönetmenin bir yolunu sağlar. Kapsamlar web API’si tarafından kapsam tabanlı erişim denetimi uygulamak için kullanılır. Örneğin, bazı kullanıcılar hem okuma hem de yazma erişimine sahipken diğer kullanıcıların salt okunur izinleri olabilir. Bu öğreticide web API’si için okuma izinlerini tanımlayacaksınız.

1. Seçin **uygulamaları**ve ardından *webapi1*.
2. Seçin **yayımlanan kapsamlar**.
3. İçin **kapsam**, girin `Hello.Read`ve açıklama yazın `Read access to hello`.
4. İçin **kapsam**, girin `Hello.Write`ve açıklama yazın `Write access to hello`.
5. **Kaydet**’e tıklayın.

Yayımlanan kapsamlar web API’sine bir istemci uygulama izni vermek için kullanılabilir.

## <a name="grant-permissions"></a>İzinleri verme

Bir uygulamadan korumalı web API'sini çağırmak için API'ye uygulama izinleri vermeniz gerekir. Önkoşul öğreticisinde, Azure AD B2C'adlı bir web uygulamasında oluşturulan *app1*. Web API'sini çağırmak için bu uygulamayı kullanın.

1. Seçin **uygulamaları**ve ardından *nativeapp1*.
2. Seçin **API erişimi**ve ardından **Ekle**.
3. İçinde **API seçin** açılır menüsünde, select *webapi1*.
4. İçinde **kapsamları seçin** açılır menüsünde, select **Hello.Read** ve **Hello.Write** daha önce tanımladığınız kapsamları.
5. **Tamam** düğmesine tıklayın.

Bir kullanıcı WPF Masaüstü uygulamasını kullanmak için Azure AD B2C ile kimlik doğrulaması yapar. Masaüstü uygulaması, korumalı web API'sine erişmek için Azure AD B2C bir yetkilendirme izni alır.

## <a name="configure-the-sample"></a>Örnek yapılandırma

API'yi kaydettikten ve kapsamları tanımladıktan sonra web API kodunda Azure AD B2C kiracınızı kullanacak şekilde yapılandırın. Bu öğreticide, Github'dan indirebileceğiniz örnek Node.js web uygulamasını yapılandırın. 

GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi/archive/master.zip) veya örnek web uygulamasını kopyalayın.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi.git
```
Node.js web API’si örneği API’ye yapılan çağrıları korumak için Azure AD B2C’yi etkinleştirmek üzere Passport.js kitaplığını kullanır. 

1. `index.js` dosyasını açın.
2. Örneği Azure AD B2C kiracı kayıt bilgileriyle yapılandırın. Aşağıdaki kod satırlarını değiştirin:

    ```nodejs
    var tenantID = "<your-tenant-name>.onmicrosoft.com";
    var clientID = "<application-ID>";
    var policyName = "B2C_1_signupsignin1";
    ```

## <a name="run-the-sample"></a>Örneği çalıştırma

1. Bir Node.js komut istemi başlatın.
2. Node.js örneğini içeren dizine değiştirin. Örneğin `cd c:\active-directory-b2c-javascript-nodejs-webapi`
3. Aşağıdaki komutları çalıştırın:
    ```
    npm install && npm update
    ```
    ```
    node index.js
    ```

### <a name="run-the-desktop-application"></a>Masaüstü uygulamasını çalıştırma

1. Açık **active directory b2c wpf** Visual Studio'daki çözüm.
2. Masaüstü uygulamasını çalıştırmak için **F5**'e basın.
3. [Bir masaüstü uygulamasında Azure Active Directory B2C ile kullanıcılar için kimlik doğrulaması gerçekleştirme öğreticisinde](active-directory-b2c-tutorials-desktop-app.md) kullanılan e-posta adresi ve parolayı kullanarak oturum açın.
4. **API’yı Çağır** düğmesine tıklayın. 

Masaüstü uygulaması için web API'sine bir istek gönderir ve oturum açmış kullanıcının görünen adı ile bir yanıt alır. Korumalı masaüstü uygulaması, Azure AD B2C kiracınızdaki korumalı web API'sini çağırır.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Web API'si uygulaması ekleme
> * Web API'si için Kapsamları yapılandırma
> * Web API'sine izinler verin
> * Örnek uygulamayı güncelleştirme

> [!div class="nextstepaction"]
> [Öğretici: Kimlik sağlayıcıları Azure Active Directory B2C uygulamalarınızın ekleyin](tutorial-add-identity-providers.md)
