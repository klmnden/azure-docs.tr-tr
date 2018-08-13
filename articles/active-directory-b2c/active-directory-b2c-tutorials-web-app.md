---
title: 'Öğretici: Bir web uygulamasının Azure Active Directory B2C kullanarak hesaplarla kimlik doğrulaması yapmasını sağlama | Microsoft Docs'
description: Bir ASP.NET web uygulamasında kullanıcının oturum açmasını sağlamak için Azure Active Directory B2C’nin nasıl kullanılacağını gösteren öğretici.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.author: davidmu
ms.date: 1/23/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.component: B2C
ms.openlocfilehash: ed34dcfb2aa488f4e7e34294b46de68624811afd
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39609047"
---
# <a name="tutorial-enable-a-web-application-to-authenticate-with-accounts-using-azure-active-directory-b2c"></a>Öğretici: Bir web uygulamasının Azure Active Directory B2C kullanarak hesaplarla kimlik doğrulaması yapmasını sağlama

Bu öğreticide, bir ASP.NET web uygulamasında kullanıcı oturumu açmak ve kullanıcı kaydetmek için Azure Active Directory (Azure AD) B2C’nin nasıl kullanılacağı açıklanmaktadır. Azure AD B2C, uygulamalarınızın sosyal ağ hesaplarda, kurumsal hesaplarda ve Azure Active Directory hesaplarında açık standart protokoller kullanarak kimlik doğrulaması yapmasına izin verir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD B2C kiracınıza örnek bir ASP.NET web uygulaması kaydetme.
> * Kullanıcı kaydı, oturum açma, profil düzenleme ve parola sıfırlama işlemleri için ilkeler oluşturma.
> * Azure AD B2C kiracınızı kullanmak için örnek web uygulamasını yapılandırma. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* Kendi [Azure AD B2C Kiracınızı](active-directory-b2c-get-started.md) oluşturun
* **ASP.NET ve web geliştirme** iş yüküyle [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi yükleyin.

## <a name="register-web-app"></a>Web uygulamasını kaydedin

Uygulamaların Azure Active Directory’den [erişim belirteçlerini](../active-directory/develop/developer-glossary.md#access-token) alabilmesi için öncelikle kiracınızda [kayıtlı](../active-directory/develop/developer-glossary.md#application-registration) olması gerekir. Uygulama kaydı, kiracınızda uygulama için bir [uygulama kimliği](../active-directory/develop/developer-glossary.md#application-id-client-id) oluşturur. 

[Azure portalında](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin. Artık önceki öğreticide oluşturduğunuz kiracıyı kullanıyor olmanız gerekir. 

2. B2C ayarlarında **Uygulamalar**’a ve ardından  **Ekle**’ye tıklayın. 

    Örnek web uygulamasını kiracınıza kaydetmek için aşağıdaki ayaları kullanın:

    ![Yeni uygulama ekle](media/active-directory-b2c-tutorials-web-app/web-app-registration.png)
    
    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Ad** | Örnek Web Uygulamam | Uygulamanızı müşterilere açıklayan bir **Ad** girin. | 
    | **Web uygulamasını / web API'sini dahil etme** | Yes | Web uygulaması için **Evet**’i seçin. |
    | **Örtük akışa izin verme** | Yes | Uygulama [OpenID Connect oturumu](active-directory-b2c-reference-oidc.md) kullandığından **Evet**’i seçin. |
    | **Yanıt URL'si** | `https://localhost:44316` | Yanıt URL'leri, Azure AD B2C'nin, uygulamanız tarafından istenen belirteçleri döndürdüğü uç noktalardır. Bu öğreticide örnek yerel olarak (localhost) çalışır ve 44316 numaralı bağlantı noktasını dinler. |
    | **Yerel istemci ekle** | Hayır | Bu, bir web uygulaması olduğu için ve yerel bir istemci olmadığı için Hayır’ı seçin. |
    
3. Uygulamanızı kaydetmek için **Oluştur**’a seçeneğine tıklayın.

Kayıtlı uygulamalar Azure AD B2C kiracısı için uygulamalar listesinde görüntülenir. Listeden web uygulamanızı seçin. Web uygulamasının özellik bölmesi görüntülenir.

![Web uygulaması özellikleri](./media/active-directory-b2c-tutorials-web-app/b2c-web-app-properties.png)

**Uygulama Kimliği**’ni not alın. Kimlik, uygulamayı benzersiz bir şekilde tanımlar ve öğreticinin sonraki bölümlerinde uygulamayı yapılandırırken gerekir.

### <a name="create-a-client-password"></a>İstemci parolası oluşturma

Azure AD B2C, [istemci uygulamaları](../active-directory/develop/developer-glossary.md#client-application) için OAuth2 kimlik doğrulaması kullanır. Web uygulamaları [gizli istemciler](../active-directory/develop/developer-glossary.md#web-client) olup istemci kimliği veya uygulama kimliği ve gizli anahtar, istemci parolası ya da uygulama anahtarı gerektirir.

1. Kayıtlı web uygulaması için Anahtarlar sayfasını seçin ve **Anahtar oluştur**’a tıklayın.

2. Uygulama anahtarını görüntülemek için **Kaydet**’e tıklayın.

    ![uygulama genel anahtarları sayfası](media/active-directory-b2c-tutorials-web-app/app-general-keys-page.png)

Anahtar, portalda bir sefer görüntülenir. Anahtar değerini kopyalamanız ve kaydetmeniz önemlidir. Uygulamanızı yapılandırırken bu değere ihtiyaç duyarsınız. Anahtarı güvenli bir şekilde saklayın. Anahtarı herkese açık bir şekilde paylaşmayın.

## <a name="create-policies"></a>İlkeleri oluşturma

Azure AD B2C ilkesi, kullanıcı iş akışlarını tanımlar. Örneğin, oturum açma, kaydolma, parola değiştirme ve profilleri düzenleme genel iş akışlarıdır.

### <a name="create-a-sign-up-or-sign-in-policy"></a>Kaydolma veya oturum açma ilkesi oluşturma

Erişim sağlamak üzere kullanıcıları kaydetmek ve web uygulamasında oturum açmalarını sağlamak için bir **kaydolma veya oturum açma ilkesi** oluşturun.

1. Azure AD B2C portal sayfasında **Kaydolma veya oturum açma ilkeleri**’ni seçin ve **Ekle**’ye tıklayın.

    İlkenizi yapılandırmak için aşağıdaki ayarları kullanın:

    ![Kaydolma veya oturum açma ilkesi ekle](media/active-directory-b2c-tutorials-web-app/add-susi-policy.png)

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Ad** | SiUpIn | İlke için bir **Ad** girin. İlke adı, **b2c_1_** önekine sahip olur. Örnek kodda, **b2c_1_SiUpIn** olan tam ilke adını kullanın. | 
    | **Kimlik sağlayıcı** | E-posta ile kaydolma | Kimlik sağlayıcı, kullanıcıyı benzersiz şekilde tanımlamak için kullanılır. |
    | **Kaydolma öznitelikleri** | Görünen Ad ve Posta Kodu | Kayıt sırasında kullanıcıdan toplanacak öznitelikleri seçin. |
    | **Uygulama talepleri** | Görünen Ad, Posta Kodu, Kullanıcının yeni olma durumu, Kullanıcının Nesne Kimliği | [Erişim belirtecine](../active-directory/develop/developer-glossary.md#access-token) eklenmesini istediğiniz [talepleri](../active-directory/develop/developer-glossary.md#claim) seçin. |

2. İlkenizi oluşturmak için **Oluştur**'a tıklayın. 

### <a name="create-a-profile-editing-policy"></a>Profil düzenleme ilkesi oluşturma

Kullanıcıların, kullanıcı profili bilgilerini kendi kendine sıfırlamasına olanak tanımak için bir **profil düzenleme ilkesi** oluşturun.

1. Azure AD B2C portal sayfasında **Profil düzenleme ilkeleri**’ni seçin ve **Ekle**’ye tıklayın.

    İlkenizi yapılandırmak için aşağıdaki ayarları kullanın:

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Ad** | SiPe | İlke için bir **Ad** girin. İlke adı, **b2c_1_** önekine sahip olur. Örnek kodda, **b2c_1_SiPe** olan tam ilke adını kullanın. | 
    | **Kimlik sağlayıcı** | Yerel Hesap Oturum Açma Bilgileri | Kimlik sağlayıcı, kullanıcıyı benzersiz şekilde tanımlamak için kullanılır. |
    | **Profil öznitelikleri** | Görünen Ad ve Posta Kodu | Profil düzenleme işlemi sırasında kullanıcıların değiştirebileceği öznitelikleri seçin. |
    | **Uygulama talepleri** | Görünen Ad, Posta Kodu, Kullanıcının Nesne Kimliği | Başarılı bir profil düzenleme işleminden sonra [erişim belirtecine](../active-directory/develop/developer-glossary.md#access-token) eklenmesini istediğiniz [talepleri](../active-directory/develop/developer-glossary.md#claim) seçin. |

2. İlkenizi oluşturmak için **Oluştur**'a tıklayın. 

### <a name="create-a-password-reset-policy"></a>Parola sıfırlama ilkesi oluşturma

Uygulamanızda parola sıfırlama özelliği sunmak için bir **parola sıfırlama ilkesi** oluşturmanız gerekir. Bu ilke, parola sıfırlama işlemi sırasında müşteri deneyimini ve başarıyla tamamlandığında uygulamanın aldığı belirteçlerin içeriğini açıklar.

1. Azure AD B2C portal sayfasında **Parola sıfırlama ilkeleri**’ni seçin ve **Ekle**’ye tıklayın.

    İlkenizi yapılandırmak için aşağıdaki ayarları kullanın.

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Ad** | SSPR | İlke için bir **Ad** girin. İlke adı, **b2c_1_** önekine sahip olur. Örnek kodda, **b2c_1_SSPR** olan tam ilke adını kullanın. | 
    | **Kimlik sağlayıcı** | E-posta adresi kullanarak parola sıfırlama | Bu, kullanıcıyı benzersiz şekilde tanımlamak için kullanılan kimlik sağlayıcıdır. |
    | **Uygulama talepleri** | Kullanıcının Nesne Kimliği | Başarılı bir parola sıfırlama işleminden sonra [erişim belirtecine](../active-directory/develop/developer-glossary.md#access-token) eklenmesini istediğiniz [talepleri](../active-directory/develop/developer-glossary.md#claim) seçin. |

2. İlkenizi oluşturmak için **Oluştur**'a tıklayın. 

## <a name="update-web-app-code"></a>Web uygulaması kodunu güncelleştirme

Artık web uygulamasını kaydettiğinize ve ilkeleri oluşturduğunuza göre, uygulamanızı Azure AD B2C kiracınızı kullanacak şekilde yapılandırmanız gerekir. Bu öğreticide, GitHub’dan indirebileceğiniz örnek bir web uygulaması yapılandırırsınız. 

GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi/archive/master.zip) veya örnek web uygulamasını kopyalayın. Örnek dosyayı, yolunun toplam karakter uzunluğu 260'tan az olan bir klasöre ayıkladığınızdan emin olun.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Örnek ASP.NET web uygulaması, yapılacak işler listesi oluşturmak ve güncelleştirmek için kullanılan basit bir görev listesi uygulamasıdır. Kullanıcıların uygulamayı Azure AD B2C kiracınızda kullanmasına izin vermek için [Microsoft OWIN ara yazılım bileşenleri](https://docs.microsoft.com/aspnet/aspnet/overview/owin-and-katana/) kullanılır. Azure AD B2C ilkesi oluşturduğunuzda kullanıcılar uygulamaya erişmek için kimlik olarak kullanmak üzere bir sosyal ağ hesabı kullanabilir veya hesap oluşturabilir. 

Örnek çözümde iki proje vardır:

**Örnek web uygulaması (TaskWebApp):** Görev listesi oluşturmak ve düzenlemek için kullanılan web uygulaması. Web uygulaması, kullanıcıların oturum açması için **kaydolma veya oturum açma** ilkesi kullanır.

**Örnek web API’si uygulaması (TaskService):** Görev listesi oluşturma, okuma, güncelleştirme ve silme işlevlerini destekleyen web API’si. Web API’si Azure AD B2C tarafından korunur ve web uygulaması tarafından çağrılır.

Uygulama kimliğini ve önceden kaydettiğiniz anahtarı içeren kiracınızdaki uygulama kaydını kullanmak için uygulamayı değiştirmeniz gerekir. Ayrıca oluşturduğunuz ilkeleri yapılandırmanız gerekir. Örnek web uygulaması, yapılandırma değerlerini Web.config dosyasındaki uygulama ayarları olarak tanımlar. Uygulama ayarlarını değiştirmek için:

1. **B2C-WebAPI-DotNet** çözümünü Visual Studio’da açın.

2. **TaskWebApp** web uygulaması projesinde **Web.config** dosyasını açın. `ida:Tenant` değerini oluşturduğunuz kiracının adıyla değiştirin. `ida:ClientId` değerini kaydettiğiniz uygulama kimliğiyle değiştirin. `ida:ClientSecret` değerini kaydettiğiniz anahtarla değiştirin.

3. **Web.config** dosyasında `ida:SignUpSignInPolicyId` değerini `b2c_1_SiUpIn` ile değiştirin. `ida:EditProfilePolicyId` değerini `b2c_1_SiPe` ile değiştirin. `ida:ResetPasswordPolicyId` değerini `b2c_1_SSPR` ile değiştirin.

## <a name="run-the-sample-web-app"></a>Örnek web uygulamasını çalıştırma

Çözüm Gezgini’nde **TaskWebApp** projesine sağ tıklayın ve sonra **StartUp Projesi olarak ayarla**’ya tıklayın

Web uygulamasını başlatmak için **F5**’e basın. Varsayılan tarayıcı, `https://localhost:44316/` olan yerel web sitesi adresini açar. 

Örnek uygulama kaydolma, oturum açma, profil düzenleme ve parola sıfırlama işlemlerini destekler. Bu öğretici, kullanıcının uygulamayı kullanmak için e-posta adresiyle nasıl kaydolacağını vurgular. Diğer senaryoları kendi kendinize keşfedebilirsiniz.

### <a name="sign-up-using-an-email-address"></a>E-posta adresi kullanarak kaydolma

1. Web uygulamasının kullanıcısı olarak kaydolmak için üst başlıktaki **Kaydol / Oturum aç** bağlantısına tıklayın. Burada, önceki adımda tanımladığınız **b2c_1_SiUpIn** ilkesi kullanılır.

2. Azure AD B2C, kayıt bağlantısı içeren bir oturum açma sayfası sunar. Henüz bir hesabınız yoksa **Hemen kaydol** bağlantısına tıklayın. 

3. Kaydolma iş akışında, kullanıcı kimliğini bir e-posta adresi kullanarak toplamak ve doğrulamak için bir sayfa sunar. Ayrıca kaydolma iş akışı, kullanıcının parolasını ve ilkede tanımlanmış istenen öznitelikleri toplar.

    Geçerli bir e-posta adresi kullanın ve doğrulama kodunu kullanarak doğrulamayı gerçekleştirin. Parola ayarlayın. İstenen öznitelikler için değerleri girin. 

    ![Kaydolma iş akışı](media/active-directory-b2c-tutorials-web-app/sign-up-workflow.png)

4. Azure AD B2C kiracısında yerel bir hesap oluşturmak için **Oluştur**’a tıklayın.

Artık kullanıcı, oturum açmak ve web uygulamasını kullanmak için e-posta adresini kullanabilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Azure AD B2C öğreticilerini denemeyi planlıyorsanız Azure AD B2C kiracınızı kullanabilirsiniz. Artık ihtiyaç duymuyorsanız [Azure AD B2C kiracınızı silebilirsiniz](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir Azure AD B2C kiracısı oluşturmayı, ilkeleri oluşturmayı ve Azure AD B2C kiracınızı kullanmak için örnek web uygulamasını güncelleştirmeyi öğrendiniz. Azure AD B2C kiracınız tarafından korunan bir ASP.NET web API’sini kaydetmeyi, yapılandırmayı ve çağırmayı öğrenmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Öğretici: Bir ASP.NET web API’sini korumak için Azure Active Directory B2C’yi kullanma](active-directory-b2c-tutorials-web-api.md)
