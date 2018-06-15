---
title: 'Öğretici: Azure Active Directory B2C’yi kullanan hesaplarla masaüstü uygulama kimlik doğrulamasını etkinleştirme | Microsoft Docs'
description: Bir .NET masaüstü uygulamasında kullanıcının oturum açmasını sağlamak için Azure Active Directory B2C’nin nasıl kullanılacağını gösteren öğretici.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.author: davidmu
ms.date: 2/28/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.component: B2C
ms.openlocfilehash: ff9cfd0f1f3d8ee62b7f93d88023b3dedce3e7be
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34711741"
---
# <a name="tutorial-enable-desktop-app-authentication-with-accounts-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C’yi kullanan hesaplarla masaüstü uygulama kimlik doğrulamasını etkinleştirme

Bu öğreticide, bir Windows Presentation Foundation (WPF) masaüstü uygulamasında kullanıcının oturumunu açmak ve kullanıcıyı kaydetmek için Azure Active Directory (Azure AD) B2C’nin nasıl kullanılacağı gösterilir. Azure AD B2C, uygulamalarınızın sosyal ağ hesaplarda, kurumsal hesaplarda ve Azure Active Directory hesaplarında açık standart protokoller kullanarak kimlik doğrulaması yapmasına izin verir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD B2C kiracınıza örnek masaüstü uygulamasını kaydetme.
> * Kullanıcı kaydı, oturum açma, profil düzenleme ve parola sıfırlama işlemleri için ilkeler oluşturma.
> * Azure AD B2C kiracınızı kullanmak için örnek uygulamayı yapılandırma.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* Kendi [Azure AD B2C Kiracınızı](active-directory-b2c-get-started.md) oluşturun
* **.NET masaüstü geliştirme** ve **ASP.NET ve web geliştirme** iş yükleriyle [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi yükleyin.

## <a name="register-desktop-app"></a>Masaüstü uygulaması kaydetme

Uygulamaların Azure Active Directory’den [erişim belirteçlerini](../active-directory/develop/active-directory-dev-glossary.md#access-token) alabilmesi için öncelikle kiracınızda [kayıtlı](../active-directory/develop/active-directory-dev-glossary.md#application-registration) olması gerekir. Uygulama kaydı, kiracınızda uygulama için bir [uygulama kimliği](../active-directory/develop/active-directory-dev-glossary.md#application-id-client-id) oluşturur. 

[Azure portalında](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Azure Portalı'ndaki Hizmetler listesinden **Azure AD B2C**’yi seçin. 

2. B2C ayarlarında **Uygulamalar**’a ve ardından  **Ekle**’ye tıklayın. 

    Örnek web uygulamasını kiracınıza kaydetmek için aşağıdaki ayaları kullanın:
    
    ![Yeni uygulama ekle](media/active-directory-b2c-tutorials-desktop-app/desktop-app-registration.png)
    
    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Ad** | Örnek WPF Uygulamam | Uygulamanızı müşterilere açıklayan bir **Ad** girin. | 
    | **Web uygulamasını / web API'sini dahil etme** | Hayır | Masaüstü uygulaması için **Hayır**’ı seçin. |
    | **Yerel istemci ekle** | Yes | Bu bir masaüstü uygulaması olduğundan yerel istemci olarak kabul edilir. |
    | **Yönlendirme URI'si** | Varsayılan değerler | Azure AD B2C'nin OAuth 2.0 yanıtındaki kullanıcı aracısını yönlendireceği benzersiz tanımlayıcı. |
    | **Özel yeniden yönlendirme URI'si** | `com.onmicrosoft.contoso.appname://redirect/path` | `com.onmicrosoft.<your tenant name>.<any app name>://redirect/path` URI’sini girin. İlkeler bu URI’ye belirteç gönderir. |
    
3. Uygulamanızı kaydetmek için **Oluştur**’a seçeneğine tıklayın.

Kayıtlı uygulamalar Azure AD B2C kiracısı için uygulamalar listesinde görüntülenir. Listeden masaüstü uygulamanızı seçin. Kayıtlı masaüstü uygulamasının özellik bölmesinde görüntülenir.

![Masaüstü uygulaması özellikleri](./media/active-directory-b2c-tutorials-desktop-app/b2c-desktop-app-properties.png)

**Uygulama İstemci Kimliği**’ni not alın. Kimlik, uygulamayı benzersiz bir şekilde tanımlar ve öğreticinin sonraki bölümlerinde uygulamayı yapılandırırken gerekir.

## <a name="create-policies"></a>İlkeleri oluşturma

Azure AD B2C ilkesi, kullanıcı iş akışlarını tanımlar. Örneğin, oturum açma, kaydolma, parola değiştirme ve profilleri düzenleme genel iş akışlarıdır.

### <a name="create-a-sign-up-or-sign-in-policy"></a>Kaydolma veya oturum açma ilkesi oluşturma

Erişim sağlamak üzere kullanıcıları kaydetmek ve masaüstü uygulamasında oturum açmalarını sağlamak için bir **kaydolma veya oturum açma ilkesi** oluşturun.

1. Azure AD B2C portal sayfasında **Kaydolma veya oturum açma ilkeleri**’ni seçin ve **Ekle**’ye tıklayın.

    İlkenizi yapılandırmak için aşağıdaki ayarları kullanın:

    ![Kaydolma veya oturum açma ilkesi ekle](media/active-directory-b2c-tutorials-desktop-app/add-susi-policy.png)

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

## <a name="update-desktop-app-code"></a>Masaüstü uygulaması kodunu güncelleştirme

Masaüstü uygulaması kaydedip ilke oluşturduğunuza göre artık uygulamanızı Azure AD B2C kiracınızı kullanacak şekilde yapılandırmanız gerekir. Bu öğretici örnek bir masaüstü uygulamasını yapılandıracaksınız. 

GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop/archive/master.zip) veya örneği kopyalayın.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop.git
```

Örnek WPF masaüstü uygulaması, bir masaüstü uygulamasının kullanıcıyı kaydetme, kullanıcının oturumunu açma ve korumalı bir web API’sini çağırma işlemleri için Azure AD B2C’yi nasıl kullanabileceğini gösterir.

Kiracınızda uygulama kaydını kullanmak için uygulamayı değiştirmeniz ve oluşturduğunuz ilkeleri yapılandırmanız gerekir. 

Uygulama ayarlarını değiştirmek için:

1. `active-directory-b2c-wpf` çözümünü Visual Studio’da açın.

2. `active-directory-b2c-wpf` projesinde, **App.xaml.cs** dosyasını açın ve aşağıdaki güncelleştirmeleri yapın:

    ```C#
    private static string Tenant = "<your-tenant-name>.onmicrosoft.com";
    private static string ClientId = "The Application ID for your desktop app registered in your tenant";
    ```

3. **PolicySignUpSignIn** değişkenini, bir önceki adımda oluşturduğunuz *kaydolma veya oturum açma ilkesi* adıyla güncelleştirin. *B2C_1_* ön ekini kullanmayı unutmayın.

    ```C#
    public static string PolicySignUpSignIn = "B2C_1_SiUpIn";
    ```

## <a name="run-the-sample-desktop-application"></a>Örnek masaüstü uygulamasını çalıştırma

Masaüstü uygulamasını oluşturup çalıştırmak için **F5** tuşuna basın. 

Örnek uygulama kaydolma, oturum açma, profil düzenleme ve parola sıfırlama işlemlerini destekler. Bu öğretici, kullanıcının uygulamayı kullanmak için e-posta adresiyle nasıl kaydolacağını vurgular. Diğer senaryoları kendi kendinize keşfedebilirsiniz.

### <a name="sign-up-using-an-email-address"></a>E-posta adresi kullanarak kaydolma

1. Masaüstü uygulamasının bir kullanıcısı olarak kaydolmak için **Oturum Aç** düğmesine tıklayın. Burada, önceki adımda tanımladığınız **B2C_1_SiUpIn** ilkesi kullanılır.

2. Azure AD B2C, kayıt bağlantısı içeren bir oturum açma sayfası sunar. Henüz bir hesabınız yoksa **Hemen kaydol** bağlantısına tıklayın. 

3. Kaydolma iş akışında, kullanıcı kimliğini bir e-posta adresi kullanarak toplamak ve doğrulamak için bir sayfa sunar. Ayrıca kaydolma iş akışı, kullanıcının parolasını ve ilkede tanımlanmış istenen öznitelikleri toplar.

    Geçerli bir e-posta adresi kullanın ve doğrulama kodunu kullanarak doğrulamayı gerçekleştirin. Parola ayarlayın. İstenen öznitelikler için değerleri girin. 

    ![Kaydolma iş akışı](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.png)

4. Azure AD B2C kiracısında yerel bir hesap oluşturmak için **Oluştur**’a tıklayın.

Artık kullanıcı, oturum açmak ve masaüstü uygulamasını kullanmak için e-posta adresini kullanabilir.

> [!NOTE]
> **API’yi çağırma** düğmesine tıklarsanız "Yetkisiz" hatasıyla karşılaşırsınız. Bu hatayı, tanıtım kiracısındaki bir kaynağa erişmeye çalıştığınız için alırsınız. Erişim belirteci yalnızca Azure AD kiracınız için geçerli olduğundan, API çağrısı yetkilendirilmez. Kiracınız için korumalı bir web API’si oluşturmak amacıyla sonraki öğreticiye geçin. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Azure AD B2C öğreticilerini denemeyi planlıyorsanız Azure AD B2C kiracınızı kullanabilirsiniz. Artık ihtiyaç duymuyorsanız [Azure AD B2C kiracınızı silebilirsiniz](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure AD B2C kiracısı oluşturmayı, ilke oluşturmayı ve örnek masaüstü uygulamasını Azure AD B2C kiracınızı kullanacak şekilde güncelleştirmeyi öğrendiniz. Bir masaüstü uygulamasındaki korumalı bir web API’sini kaydetmeyi, yapılandırmayı ve çağırmayı öğretmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> 