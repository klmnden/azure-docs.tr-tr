---
title: 'Öğretici: Azure Active Directory B2C’yi kullanan hesaplarla tek sayfalı uygulama kimlik doğrulamasını etkinleştirme | Microsoft Docs'
description: Bir tek sayfalı uygulamada (JavaScript) kullanıcının oturum açmasını sağlamak için Azure Active Directory B2C’nin nasıl kullanılacağını gösteren öğretici.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.author: davidmu
ms.date: 3/02/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.component: B2C
ms.openlocfilehash: 1680ff136dfa2ccb2ca3fd92f5045d47190e75fc
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34712530"
---
# <a name="tutorial-enable-single-page-app-authentication-with-accounts-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C’yi kullanan hesaplarla tek sayfalı uygulama kimlik doğrulamasını etkinleştirme

Bu öğreticide, bir tek sayfalı uygulamada kullanıcı oturumu açmak ve kullanıcı kaydetmek için Azure Active Directory (Azure AD) B2C’nin nasıl kullanılacağı açıklanmaktadır. Azure AD B2C, uygulamalarınızın sosyal ağ hesaplarda, kurumsal hesaplarda ve Azure Active Directory hesaplarında açık standart protokoller kullanarak kimlik doğrulaması yapmasına izin verir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD B2C kiracınızda örnek bir tek sayfalı uygulama kaydedin.
> * Kullanıcı kaydı, oturum açma, profil düzenleme ve parola sıfırlama işlemleri için ilkeler oluşturma.
> * Azure AD B2C kiracınızı kullanmak için örnek uygulamayı yapılandırma.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* Kendi [Azure AD B2C Kiracınızı](active-directory-b2c-get-started.md) oluşturun
* **ASP.NET ve web geliştirme** iş yüküyle [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi yükleyin.
* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) veya üzeri
* [Node.js](https://nodejs.org/en/download/)’yi yükleme

## <a name="register-single-page-app"></a>Tek sayfalı uygulamayı kaydet

Uygulamaların Azure Active Directory’den [erişim belirteçlerini](../active-directory/develop/active-directory-dev-glossary.md#access-token) alabilmesi için öncelikle kiracınızda [kayıtlı](../active-directory/develop/active-directory-dev-glossary.md#application-registration) olması gerekir. Uygulama kaydı, kiracınızda uygulama için bir [uygulama kimliği](../active-directory/develop/active-directory-dev-glossary.md#application-id-client-id) oluşturur. 

[Azure portalında](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Azure Portalı'ndaki Hizmetler listesinden **Azure AD B2C**’yi seçin. 

2. B2C ayarlarında **Uygulamalar**’a ve ardından  **Ekle**’ye tıklayın. 

    Örnek web uygulamasını kiracınıza kaydetmek için aşağıdaki ayaları kullanın:
    
    ![Yeni uygulama ekle](media/active-directory-b2c-tutorials-spa/spa-registration.png)
    
    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Ad** | Örnek tek sayfalı uygulamam | Uygulamanızı müşterilere açıklayan bir **Ad** girin. | 
    | **Web uygulamasını / web API'sini dahil etme** | Yes | Bir tek sayfalı uygulama için **Evet**’i seçin. |
    | **Örtük akışa izin verme** | Yes | Uygulama [OpenID Connect oturumu](active-directory-b2c-reference-oidc.md) kullandığından **Evet**’i seçin. |
    | **Yanıt URL'si** | `http://localhost:6420` | Yanıt URL'leri, Azure AD B2C'nin, uygulamanız tarafından istenen belirteçleri döndürdüğü uç noktalardır. Bu öğreticide örnek yerel olarak (localhost) çalışır ve 6420 numaralı bağlantı noktasını dinler. |
    | **Yerel istemci ekle** | Hayır | Bu, yerel bir istemci değil bir tek sayfalı uygulama olduğu için Hayır’ı seçin. |
    
3. Uygulamanızı kaydetmek için **Oluştur**’a seçeneğine tıklayın.

Kayıtlı uygulamalar Azure AD B2C kiracısı için uygulamalar listesinde görüntülenir. Listeden tek sayfalı uygulamanızı seçin. Kayıtlı tek sayfalı uygulamanın özellik bölmesinde görüntülenir.

![Tek sayfalı uygulama özellikleri](./media/active-directory-b2c-tutorials-spa/b2c-spa-properties.png)

**Uygulama İstemci Kimliği**’ni not alın. Kimlik, uygulamayı benzersiz bir şekilde tanımlar ve öğreticinin sonraki bölümlerinde uygulamayı yapılandırırken gerekir.

## <a name="create-policies"></a>İlkeleri oluşturma

Azure AD B2C ilkesi, kullanıcı iş akışlarını tanımlar. Örneğin, oturum açma, kaydolma, parola değiştirme ve profilleri düzenleme genel iş akışlarıdır.

### <a name="create-a-sign-up-or-sign-in-policy"></a>Kaydolma veya oturum açma ilkesi oluşturma

Erişim sağlamak üzere kullanıcıları kaydetmek ve web uygulamasında oturum açmalarını sağlamak için bir **kaydolma veya oturum açma ilkesi** oluşturun.

1. Azure AD B2C portal sayfasında **Kaydolma veya oturum açma ilkeleri**’ni seçin ve **Ekle**’ye tıklayın.

    İlkenizi yapılandırmak için aşağıdaki ayarları kullanın:

    ![Kaydolma veya oturum açma ilkesi ekle](media/active-directory-b2c-tutorials-web-app/add-susi-policy.png)

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Ad** | SiUpIn | İlke için bir **Ad** girin. İlke adı, **B2C_1_** ön ekine sahip olur. Örnek kodda **B2C_1_SiUpIn** olan tam ilke adını kullanırsınız. | 
    | **Kimlik sağlayıcı** | E-posta ile kaydolma | Kimlik sağlayıcı, kullanıcıyı benzersiz şekilde tanımlamak için kullanılır. |
    | **Kaydolma öznitelikleri** | Görünen Ad ve Posta Kodu | Kayıt sırasında kullanıcıdan toplanacak öznitelikleri seçin. |
    | **Uygulama talepleri** | Görünen Ad, Posta Kodu, Kullanıcının yeni olma durumu, Kullanıcının Nesne Kimliği | [Erişim belirtecine](../active-directory/develop/active-directory-dev-glossary.md#access-token) eklenmesini istediğiniz [talepleri](../active-directory/develop/active-directory-dev-glossary.md#claim) seçin. |

2. İlkenizi oluşturmak için **Oluştur**'a tıklayın. 

### <a name="create-a-profile-editing-policy"></a>Profil düzenleme ilkesi oluşturma

Kullanıcıların, kullanıcı profili bilgilerini kendi kendine sıfırlamasına olanak tanımak için bir **profil düzenleme ilkesi** oluşturun.

1. Azure AD B2C portal sayfasında **Profil düzenleme ilkeleri**’ni seçin ve **Ekle**’ye tıklayın.

    İlkenizi yapılandırmak için aşağıdaki ayarları kullanın:

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Ad** | SiPe | İlke için bir **Ad** girin. İlke adı, **B2C_1_** ön ekine sahip olur. Örnek kodda, **B2C_1_SiPe** olan tam ilke adını kullanırsınız. | 
    | **Kimlik sağlayıcı** | Yerel Hesap Oturum Açma Bilgileri | Kimlik sağlayıcı, kullanıcıyı benzersiz şekilde tanımlamak için kullanılır. |
    | **Profil öznitelikleri** | Görünen Ad ve Posta Kodu | Profil düzenleme işlemi sırasında kullanıcıların değiştirebileceği öznitelikleri seçin. |
    | **Uygulama talepleri** | Görünen Ad, Posta Kodu, Kullanıcının Nesne Kimliği | Başarılı bir profil düzenleme işleminden sonra [erişim belirtecine](../active-directory/develop/active-directory-dev-glossary.md#access-token) eklenmesini istediğiniz [talepleri](../active-directory/develop/active-directory-dev-glossary.md#claim) seçin. |

2. İlkenizi oluşturmak için **Oluştur**'a tıklayın. 

### <a name="create-a-password-reset-policy"></a>Parola sıfırlama ilkesi oluşturma

Uygulamanızda parola sıfırlama özelliği sunmak için bir **parola sıfırlama ilkesi** oluşturmanız gerekir. Bu ilke, parola sıfırlama işlemi sırasında müşteri deneyimini ve başarıyla tamamlandığında uygulamanın aldığı belirteçlerin içeriğini açıklar.

1. Azure AD B2C portal sayfasında **Parola sıfırlama ilkeleri**’ni seçin ve **Ekle**’ye tıklayın.

    İlkenizi yapılandırmak için aşağıdaki ayarları kullanın.

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Ad** | SSPR | İlke için bir **Ad** girin. İlke adı, **B2C_1_** ön ekine sahip olur. Örnek kodda, **B2C_1_SSPR** olan tam ilke adını kullanırsınız. | 
    | **Kimlik sağlayıcı** | E-posta adresi kullanarak parola sıfırlama | Bu, kullanıcıyı benzersiz şekilde tanımlamak için kullanılan kimlik sağlayıcıdır. |
    | **Uygulama talepleri** | Kullanıcının Nesne Kimliği | Başarılı bir parola sıfırlama işleminden sonra [erişim belirtecine](../active-directory/develop/active-directory-dev-glossary.md#access-token) eklenmesini istediğiniz [talepleri](../active-directory/develop/active-directory-dev-glossary.md#claim) seçin. |

2. İlkenizi oluşturmak için **Oluştur**'a tıklayın. 

## <a name="update-single-page-app-code"></a>Tek sayfalı uygulama kodunu güncelleştir

Artık bir uygulama kaydettiğinize ve ilkeleri oluşturduğunuza göre, uygulamanızı Azure AD B2C kiracınızı kullanacak şekilde yapılandırmanız gerekir. Bu öğreticide, GitHub’dan indirebileceğiniz örnek bir SPA JavaScript uygulaması yapılandırırsınız. 

GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip) veya örnek web uygulamasını kopyalayın.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```
Örnek uygulama, bir tek sayfalı uygulamanın kullanıcıyı kaydetme, kullanıcının oturumunu açma ve korumalı bir web API’sini çağırma işlemleri için Azure AD B2C’yi nasıl kullanabileceğini gösterir. Kiracınızda uygulama kaydını kullanmak için uygulamayı değiştirmeniz ve oluşturduğunuz ilkeleri yapılandırmanız gerekir. 

Uygulama ayarlarını değiştirmek için:

1. Node.js tek sayfalı uygulama örneğinde `index.html` dosyasını açın.
2. Örneği Azure AD B2C kiracı kayıt bilgileriyle yapılandırın. Aşağıdaki kod satırlarını değiştirin:

    ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    var applicationConfig = {
        clientID: '<Application ID for your SPA obtained from portal app registration>',
        authority: "https://login.microsoftonline.com/tfp/<your-tenant-name>.onmicrosoft.com/B2C_1_SiUpIn",
        b2cScopes: ["https://fabrikamb2c.onmicrosoft.com/demoapi/demo.read"],
        webApi: 'https://fabrikamb2chello.azurewebsites.net/hello',
    };
    ```

    Bu ilkede kullanılan ilke adı **B2C_1_SiUpIn**’dir. Farklı bir ilke adı kullanıyorsanız, `authority` değeri içinde ilke adınızı kullanın.

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

4. Bir tarayıcı kullanarak uygulamayı görüntülemek için `http://localhost:6420` adresine gidin.

Örnek uygulama kaydolma, oturum açma, profil düzenleme ve parola sıfırlama işlemlerini destekler. Bu öğretici, kullanıcının uygulamayı kullanmak için e-posta adresiyle nasıl kaydolacağını vurgular. Diğer senaryoları kendi kendinize keşfedebilirsiniz.

### <a name="sign-up-using-an-email-address"></a>E-posta adresi kullanarak kaydolma

1. SPA uygulamasının bir kullanıcısı olarak kaydolmak için **Oturum Aç**’a tıklayın. Burada, önceki adımda tanımladığınız **B2C_1_SiUpIn** ilkesi kullanılır.

2. Azure AD B2C, kayıt bağlantısı içeren bir oturum açma sayfası sunar. Henüz bir hesabınız yoksa **Hemen kaydol** bağlantısına tıklayın. 

3. Kaydolma iş akışında, kullanıcı kimliğini bir e-posta adresi kullanarak toplamak ve doğrulamak için bir sayfa sunar. Ayrıca kaydolma iş akışı, kullanıcının parolasını ve ilkede tanımlanmış istenen öznitelikleri toplar.

    Geçerli bir e-posta adresi kullanın ve doğrulama kodunu kullanarak doğrulamayı gerçekleştirin. Parola ayarlayın. İstenen öznitelikler için değerleri girin. 

    ![Kaydolma iş akışı](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.png)

4. Azure AD B2C kiracısında yerel bir hesap oluşturmak için **Oluştur**’a tıklayın.

Artık kullanıcı, oturum açmak ve SPA uygulamasını kullanmak için e-posta adresini kullanabilir.

> [!NOTE]
> Oturum açtıktan sonra, uygulama bir “yetersiz izinler” hatası görüntülüyor. Bu hatayı, tanıtım kiracısındaki bir kaynağa erişmeye çalıştığınız için alırsınız. Erişim belirteci yalnızca Azure AD kiracınız için geçerli olduğundan, API çağrısı yetkilendirilmez. Kiracınız için korumalı bir web API’si oluşturmak amacıyla sonraki öğreticiye geçin. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Azure AD B2C öğreticilerini denemeyi planlıyorsanız Azure AD B2C kiracınızı kullanabilirsiniz. Artık ihtiyaç duymuyorsanız [Azure AD B2C kiracınızı silebilirsiniz](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir Azure AD B2C kiracısı oluşturmayı, ilkeleri oluşturmayı ve Azure AD B2C kiracınızı kullanmak için örnek tek sayfalı uygulamayı güncelleştirmeyi öğrendiniz. Bir masaüstü uygulamasındaki korumalı bir web API’sini kaydetmeyi, yapılandırmayı ve çağırmayı öğretmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> 
