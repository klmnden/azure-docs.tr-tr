---
title: 'Öğretici: Azure Active Directory B2C’yi kullanan hesaplarla tek sayfalı uygulama kimlik doğrulamasını etkinleştirme | Microsoft Docs'
description: Bir tek sayfalı uygulamada (JavaScript) kullanıcının oturum açmasını sağlamak için Azure Active Directory B2C’nin nasıl kullanılacağını gösteren öğretici.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.author: davidmu
ms.date: 11/30/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.component: B2C
ms.openlocfilehash: cce76a0e97e039ec6e6c3a976d1fc7caca7fde73
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52834443"
---
# <a name="tutorial-enable-single-page-app-authentication-with-accounts-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C’yi kullanan hesaplarla tek sayfalı uygulama kimlik doğrulamasını etkinleştirme

Bu öğreticide, bir tek sayfalı uygulamada kullanıcı oturumu açmak ve kullanıcı kaydetmek için Azure Active Directory (Azure AD) B2C’nin nasıl kullanılacağı açıklanmaktadır. Azure AD B2C, uygulamalarınızın sosyal ağ hesaplarda, kurumsal hesaplarda ve Azure Active Directory hesaplarında açık standart protokoller kullanarak kimlik doğrulaması yapmasına izin verir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD B2C dizininizde örnek bir tek sayfalı uygulama kaydedin.
> * Kullanıcı için kullanıcı akışları oluşturma kaydolma, oturum açma, profil düzenleme ve parola sıfırlama.
> * Azure AD B2C dizininizi kullanmak için örnek uygulamayı yapılandırma.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* Kendi [Azure AD B2C Dizininizi](active-directory-b2c-get-started.md) oluşturun
* **ASP.NET ve web geliştirme** iş yüküyle [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi yükleyin.
* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) veya üzeri
* [Node.js](https://nodejs.org/en/download/)’yi yükleme

## <a name="register-single-page-app"></a>Tek sayfalı uygulamayı kaydet

Uygulamaların Azure Active Directory’den [erişim belirteçlerini](../active-directory/develop/developer-glossary.md#access-token) alabilmesi için öncelikle dizininizde [kayıtlı](../active-directory/develop/developer-glossary.md#application-registration) olması gerekir. Uygulama kaydı, dizininizde uygulama için bir [uygulama kimliği](../active-directory/develop/developer-glossary.md#application-id-client-id) oluşturur. 

[Azure portalda](https://portal.azure.com/) Azure AD B2C dizininizin genel yöneticisi olarak oturum açın.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Azure Portalı'ndaki Hizmetler listesinden **Azure AD B2C**’yi seçin. 

2. B2C ayarlarında **Uygulamalar**’a ve ardından  **Ekle**’ye tıklayın. 

    Örnek web uygulamasını dizininize kaydetmek için aşağıdaki ayaları kullanın:
    
    ![Yeni uygulama ekle](media/active-directory-b2c-tutorials-spa/spa-registration.png)
    
    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Ad** | Örnek tek sayfalı uygulamam | Uygulamanızı müşterilere açıklayan bir **Ad** girin. | 
    | **Web uygulamasını / web API'sini dahil etme** | Evet | Bir tek sayfalı uygulama için **Evet**’i seçin. |
    | **Örtük akışa izin verme** | Evet | Uygulama [OpenID Connect oturumu](active-directory-b2c-reference-oidc.md) kullandığından **Evet**’i seçin. |
    | **Yanıt URL'si** | `http://localhost:6420` | Yanıt URL'leri, Azure AD B2C'nin, uygulamanız tarafından istenen belirteçleri döndürdüğü uç noktalardır. Bu öğreticide örnek yerel olarak (localhost) çalışır ve 6420 numaralı bağlantı noktasını dinler. |
    | **Yerel istemci ekle** | Hayır | Bu, yerel bir istemci değil bir tek sayfalı uygulama olduğu için Hayır’ı seçin. |
    
3. Uygulamanızı kaydetmek için **Oluştur**’a seçeneğine tıklayın.

Kayıtlı uygulamalar Azure AD B2C dizini için uygulamalar listesinde görüntülenir. Listeden tek sayfalı uygulamanızı seçin. Kayıtlı tek sayfalı uygulamanın özellik bölmesinde görüntülenir.

![Tek sayfalı uygulama özellikleri](./media/active-directory-b2c-tutorials-spa/b2c-spa-properties.png)

**Uygulama İstemci Kimliği**’ni not alın. Kimlik, uygulamayı benzersiz bir şekilde tanımlar ve öğreticinin sonraki bölümlerinde uygulamayı yapılandırırken gerekir.

## <a name="create-user-flows"></a>Kullanıcı akışları oluşturma

Kullanıcı deneyimini kimlik görev için bir Azure AD B2C kullanıcı akışını tanımlar. Örneğin, kaydolma, parola değiştirme ve profilleri düzenleme, oturum açılırken, yaygın kullanıcı Akış şunlardır.

### <a name="create-a-sign-up-or-sign-in-user-flow"></a>Kaydolma veya oturum açma kullanıcı akışı oluştur

Ardından web uygulamasına oturum açma erişmek için kullanıcıların oturum açmak için oluşturma bir **kaydolma veya oturum açma kullanıcı akışı**.

1. Azure AD B2C portal sayfasında, seçin **kullanıcı akışları** tıklatıp **yeni kullanıcı akışı**.
2. Üzerinde **önerilen** sekmesinde **oturum yukarı ve oturum açma**.

    Kullanıcı akışınızı yapılandırmak için aşağıdaki ayarları kullanın:

    ![Kaydolma veya oturum açma kullanıcı Akış ekleme](media/active-directory-b2c-tutorials-spa/add-susi-user-flow.png)

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Ad** | SiUpIn | Girin bir **adı** kullanıcı akışı için. Userjourney adı ön ekine sahip **B2C_1_**. Tam userjourney adı kullandığınız **b2c_1_siupın** örnek kodda. | 
    | **Kimlik sağlayıcıları** | E-posta ile kaydolma | Kimlik sağlayıcı, kullanıcıyı benzersiz şekilde tanımlamak için kullanılır. |

3. Altında **kullanıcı öznitelikleri ve talepler**, tıklayın **daha fazla Göster** ve aşağıdaki ayarları seçin:

    ![Kaydolma veya oturum açma kullanıcı Akış ekleme](media/active-directory-b2c-tutorials-spa/add-attributes-and-claims.png)

    | Sütun      | Önerilen değer  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Öznitelik Topla** | Görünen Ad ve Posta Kodu | Kayıt sırasında kullanıcıdan toplanacak öznitelikleri seçin. |
    | **Dönüş talep** | Görünen Ad, Posta Kodu, Kullanıcının yeni olma durumu, Kullanıcının Nesne Kimliği | [Erişim belirtecine](../active-directory/develop/developer-glossary.md#access-token) eklenmesini istediğiniz [talepleri](../active-directory/develop/developer-glossary.md#claim) seçin. |

4. **Tamam** düğmesine tıklayın.
5. Tıklayın **Oluştur** kullanıcı akışınızı oluşturmak için. 

### <a name="create-a-profile-editing-user-flow"></a>Kullanıcı akışı düzenleme profili oluşturma

Kullanıcı profili bilgilerini kendi kendine sıfırlamasına izin vermek için oluşturduğunuz bir **kullanıcı akışı düzenleme profili**.

1. Azure AD B2C portal sayfasında, seçin **kullanıcı akışları** tıklatıp **yeni kullanıcı akışı**.
2. Üzerinde **önerilen** sekmesinde **profil düzenleme**.

    Kullanıcı akışınızı yapılandırmak için aşağıdaki ayarları kullanın:

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Ad** | SiPe | Girin bir **adı** kullanıcı akışı için. Userjourney adı ön ekine sahip **B2C_1_**. Tam userjourney adı kullandığınız **B2C_1_SiPe** örnek kodda. | 
    | **Kimlik sağlayıcıları** | Yerel Hesap Oturum Açma Bilgileri | Kimlik sağlayıcı, kullanıcıyı benzersiz şekilde tanımlamak için kullanılır. |

3.  Altında **kullanıcı öznitelikleri**, tıklayın **daha fazla Göster** ve aşağıdaki ayarları seçin:

    | Sütun      | Önerilen değer  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Öznitelik Topla** | Görünen Ad ve Posta Kodu | Profil düzenleme işlemi sırasında kullanıcıların değiştirebileceği öznitelikleri seçin. |
    | **Dönüş talep** | Görünen Ad, Posta Kodu, Kullanıcının Nesne Kimliği | Başarılı bir profil düzenleme işleminden sonra [erişim belirtecine](../active-directory/develop/developer-glossary.md#access-token) eklenmesini istediğiniz [talepleri](../active-directory/develop/developer-glossary.md#claim) seçin. |

4. **Tamam** düğmesine tıklayın.
5. Tıklayın **Oluştur** kullanıcı akışınızı oluşturmak için. 

### <a name="create-a-password-reset-user-flow"></a>Parola sıfırlama kullanıcı akışı oluştur

Parola sıfırlama uygulamanızı etkinleştirmek için oluşturmak gereken bir **parola sıfırlama kullanıcı akışı**. Bu kullanıcı akışını yönelik tüketici deneyimi başarıyla tamamlandığında sırasında parola sıfırlama ve uygulamanın aldığı belirteçlerin içeriğini açıklar.

1. Azure AD B2C portal sayfasında, seçin **kullanıcı akışları** tıklatıp **yeni kullanıcı akışı**.
2. Üzerinde **önerilen** sekmesinde **parola sıfırlama**.

    Kullanıcı akışınızı yapılandırmak için aşağıdaki ayarları kullanın.

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Ad** | SSPR | Girin bir **adı** kullanıcı akışı için. Userjourney adı ön ekine sahip **B2C_1_**. Tam userjourney adı kullandığınız **B2C_1_SSPR** örnek kodda. | 
    | **Kimlik sağlayıcıları** | E-posta adresi kullanarak parola sıfırlama | Bu, kullanıcıyı benzersiz şekilde tanımlamak için kullanılan kimlik sağlayıcıdır. |

3. Altında **uygulama taleplerini**, tıklayın **daha fazla Göster** ve aşağıdaki ayarları seçin:

    | Sütun      | Önerilen değer  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Dönüş talep** | Kullanıcının Nesne Kimliği | Başarılı bir parola sıfırlama işleminden sonra [erişim belirtecine](../active-directory/develop/developer-glossary.md#access-token) eklenmesini istediğiniz [talepleri](../active-directory/develop/developer-glossary.md#claim) seçin. |

4. **Tamam** düğmesine tıklayın.
5. Tıklayın **Oluştur** kullanıcı akışınızı oluşturmak için. 

## <a name="update-single-page-app-code"></a>Tek sayfalı uygulama kodunu güncelleştir

Artık bir uygulama kaydettiğinize ve oluşturulan kullanıcı akışları sahip olduğunuza göre uygulamanızı Azure AD B2C dizininizi kullanacak şekilde yapılandırmanız gerekir. Bu öğreticide, GitHub’dan indirebileceğiniz örnek bir SPA JavaScript uygulaması yapılandırırsınız. 

GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip) veya örnek web uygulamasını kopyalayın.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```
Örnek uygulama, bir tek sayfalı uygulamanın kullanıcıyı kaydetme, kullanıcının oturumunu açma ve korumalı bir web API’sini çağırma işlemleri için Azure AD B2C’yi nasıl kullanabileceğini gösterir. Oluşturduğunuz kullanıcı akışları yapılandırın ve dizininizde uygulama kaydını kullanmak için uygulamayı değiştirmeniz gerekir. 

Uygulama ayarlarını değiştirmek için:

1. Node.js tek sayfalı uygulama örneğinde `index.html` dosyasını açın.
2. Örneği Azure AD B2C dizin kayıt bilgileriyle yapılandırın. Aşağıdaki kod satırlarını değiştirin (değerlerin yerine dizininizin ve API'lerin adını yazmayı unutmayın):

    ```javascript
    // The current application coordinates were pre-registered in a B2C directory.
    var applicationConfig = {
        clientID: '<Application ID for your SPA obtained from portal app registration>',
        authority: "https://fabrikamb2c.b2clogin.com/tfp/fabrikamb2c.onmicrosoft.com/B2C_1_<Sign-up or sign-in policy name>",
        b2cScopes: ["https://fabrikamb2c.onmicrosoft.com/demoapi/demo.read"],
        webApi: 'https://fabrikamb2chello.azurewebsites.net/hello',
    };
    ```

    Bu öğreticide kullanılan kullanıcı Akış adı **b2c_1_siupın**. Kullanıcı akışı adında kullanın, farklı bir userjourney adı kullanıyorsanız, `authority` değeri.

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

1. SPA uygulamasının bir kullanıcısı olarak kaydolmak için **Oturum Aç**’a tıklayın. Bu kullanır **b2c_1_siupın** kullanıcı akışı, bir önceki adımda tanımladığınız.

2. Azure AD B2C, kayıt bağlantısı içeren bir oturum açma sayfası sunar. Henüz bir hesabınız yoksa **Hemen kaydol** bağlantısına tıklayın. 

3. Kaydolma iş akışında, kullanıcı kimliğini bir e-posta adresi kullanarak toplamak ve doğrulamak için bir sayfa sunar. Kaydolma iş akışı, ayrıca kullanıcının parolasını ve kullanıcı Akış içinde tanımlanmış istenen öznitelikleri toplar.

    Geçerli bir e-posta adresi kullanın ve doğrulama kodunu kullanarak doğrulamayı gerçekleştirin. Parola ayarlayın. İstenen öznitelikler için değerleri girin. 

    ![Kaydolma iş akışı](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.png)

4. Azure AD B2C dizininde yerel bir hesap oluşturmak için **Oluştur**’a tıklayın.

Artık kullanıcı, oturum açmak ve SPA uygulamasını kullanmak için e-posta adresini kullanabilir.

> [!NOTE]
> Oturum açtıktan sonra, uygulama bir “yetersiz izinler” hatası görüntülüyor. Bu hatayı, tanıtım dizinindeki bir kaynağa erişmeye çalıştığınız için alırsınız. Erişim belirteci yalnızca Azure AD dizininiz için geçerli olduğundan, API çağrısı yetkilendirilmez. Dizininiz için korumalı bir web API’si oluşturmak amacıyla sonraki öğreticiye geçin. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Azure AD B2C öğreticilerini denemeyi planlıyorsanız Azure AD B2C dizininizi kullanabilirsiniz. Artık ihtiyaç duymuyorsanız [Azure AD B2C dizininizi silebilirsiniz](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure AD B2C dizini oluşturma, kullanıcı akışları oluşturma ve Azure AD B2C dizininizi kullanmak için örnek tek sayfalı uygulamayı güncelleştirmeyi öğrendiniz. Bir masaüstü uygulamasındaki korumalı bir web API’sini kaydetmeyi, yapılandırmayı ve çağırmayı öğretmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure AD B2C kod örnekleri](https://azure.microsoft.com/resources/samples/?service=active-directory-b2c&sort=0)
