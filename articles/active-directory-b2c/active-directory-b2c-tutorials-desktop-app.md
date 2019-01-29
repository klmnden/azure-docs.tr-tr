---
title: 'Öğretici: Azure Active Directory B2C’yi kullanan hesaplarla masaüstü uygulama kimlik doğrulamasını etkinleştirme | Microsoft Docs'
description: Bir .NET masaüstü uygulamasında kullanıcının oturum açmasını sağlamak için Azure Active Directory B2C’nin nasıl kullanılacağını gösteren öğretici.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.author: davidmu
ms.date: 11/30/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: a99e141a59be654d6d4285be73b0bea60b1e813b
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55166979"
---
# <a name="tutorial-enable-desktop-app-authentication-with-accounts-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C'yi kullanan hesaplarla Masaüstü uygulama kimlik doğrulamasını etkinleştirme

Bu öğreticide, bir Windows Presentation Foundation (WPF) masaüstü uygulamasında kullanıcının oturumunu açmak ve kullanıcıyı kaydetmek için Azure Active Directory (Azure AD) B2C’nin nasıl kullanılacağı gösterilir. Azure AD B2C, uygulamalarınızın sosyal ağ hesaplarda, kurumsal hesaplarda ve Azure Active Directory hesaplarında açık standart protokoller kullanarak kimlik doğrulaması yapmasına izin verir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD B2C kiracınıza örnek masaüstü uygulamasını kaydetme.
> * Kullanıcı için kullanıcı akışları oluşturma kaydolma, oturum açma, profil düzenleme ve parola sıfırlama.
> * Azure AD B2C kiracınızı kullanmak için örnek uygulamayı yapılandırma.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* Kendi [Azure AD B2C Kiracınızı](active-directory-b2c-get-started.md) oluşturun
* **.NET masaüstü geliştirme** ve **ASP.NET ve web geliştirme** iş yükleriyle [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi yükleyin.

## <a name="register-desktop-app"></a>Masaüstü uygulaması kaydetme

Uygulamaların Azure Active Directory’den [erişim belirteçlerini](../active-directory/develop/developer-glossary.md#access-token) alabilmesi için öncelikle kiracınızda [kayıtlı](../active-directory/develop/developer-glossary.md#application-registration) olması gerekir. Uygulama kaydı, kiracınızda uygulama için bir [uygulama kimliği](../active-directory/develop/developer-glossary.md#application-id-client-id) oluşturur. 

[Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Azure Portalı'ndaki Hizmetler listesinden **Azure AD B2C**’yi seçin. 

2. B2C ayarlarında **Uygulamalar**’a ve ardından  **Ekle**’ye tıklayın. 

    Örnek web uygulamasını kiracınıza kaydetmek için aşağıdaki ayaları kullanın:
    
    ![Yeni uygulama ekle](media/active-directory-b2c-tutorials-desktop-app/desktop-app-registration.png)
    
    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Ad** | Örnek WPF Uygulamam | Uygulamanızı müşterilere açıklayan bir **Ad** girin. | 
    | **Web uygulamasını / web API'sini dahil etme** | Hayır | Masaüstü uygulaması için **Hayır**’ı seçin. |
    | **Yerel istemci ekle** | Evet | Bu bir masaüstü uygulaması olduğundan yerel istemci olarak kabul edilir. |
    | **Yönlendirme URI'si** | Varsayılan değerler | Azure AD B2C'nin OAuth 2.0 yanıtındaki kullanıcı aracısını yönlendireceği benzersiz tanımlayıcı. |
    | **Özel yeniden yönlendirme URI'si** | `com.onmicrosoft.contoso.appname://redirect/path` | Girin `com.onmicrosoft.<your tenant name>.<any app name>://redirect/path` kullanıcı akışları bu URI'ye belirteç için. |
    
3. Uygulamanızı kaydetmek için **Oluştur**’a seçeneğine tıklayın.

Kayıtlı uygulamalar Azure AD B2C kiracısı için uygulamalar listesinde görüntülenir. Listeden masaüstü uygulamanızı seçin. Kayıtlı masaüstü uygulamasının özellik bölmesinde görüntülenir.

![Masaüstü uygulaması özellikleri](./media/active-directory-b2c-tutorials-desktop-app/b2c-desktop-app-properties.png)

**Uygulama İstemci Kimliği**’ni not alın. Kimlik, uygulamayı benzersiz bir şekilde tanımlar ve öğreticinin sonraki bölümlerinde uygulamayı yapılandırırken gerekir.

## <a name="create-user-flows"></a>Kullanıcı akışları oluşturma

Kullanıcı deneyimini kimlik görev için bir Azure AD B2C kullanıcı akışını tanımlar. Örneğin, kaydolma, parola değiştirme ve profilleri düzenleme, oturum açılırken, yaygın kullanıcı Akış şunlardır.

### <a name="create-a-sign-up-or-sign-in-user-flow"></a>Kaydolma veya oturum açma kullanıcı akışı oluştur

Ardından masaüstü uygulamasına oturum açma erişmek için kullanıcıların oturum açmak için oluşturma bir **kaydolma veya oturum açma kullanıcı akışı**.

1. Azure AD B2C portal sayfasında, seçin **kullanıcı akışları** tıklatıp **yeni kullanıcı akışı**.
2. Üzerinde **önerilen** sekmesinde **oturum yukarı ve oturum açma**.

    Kullanıcı akışınızı yapılandırmak için aşağıdaki ayarları kullanın:

    ![Kaydolma veya oturum açma kullanıcı Akış ekleme](media/active-directory-b2c-tutorials-desktop-app/add-susi-user-flow.png)

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Ad** | SiUpIn | Girin bir **adı** kullanıcı akışı için. Userjourney adı ön ekine sahip **B2C_1_**. Tam userjourney adı kullandığınız **b2c_1_siupın** örnek kodda. | 
    | **Kimlik sağlayıcıları** | E-posta ile kaydolma | Kimlik sağlayıcı, kullanıcıyı benzersiz şekilde tanımlamak için kullanılır. |

3.  Altında **kullanıcı öznitelikleri ve talepler**, tıklayın **daha fazla Göster** ve aşağıdaki ayarları seçin:

    ![Kullanıcı ekleme öznitelikleri ve talepler](media/active-directory-b2c-tutorials-desktop-app/add-attributes-and-claims.png)

    | Sütun      | Önerilen değerler  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Öznitelik Topla** | Görünen Ad ve Posta Kodu | Kayıt sırasında kullanıcıdan toplanacak öznitelikleri seçin. |
    | **Dönüş talep** | Görünen Ad, Posta Kodu, Kullanıcının yeni olma durumu, Kullanıcının Nesne Kimliği | [Erişim belirtecine](../active-directory/develop/developer-glossary.md#access-token) eklenmesini istediğiniz [talepleri](../active-directory/develop/developer-glossary.md#claim) seçin. |

4. **Tamam** düğmesine tıklayın.

5. Tıklayın **Oluştur** kullanıcı akışınızı oluşturmak için. 

### <a name="create-a-profile-editing-user-flow"></a>Kullanıcı akışı düzenleme profili oluşturma

Kullanıcı profili bilgilerini kendi kendine sıfırlamasına izin vermek için oluşturduğunuz bir **kullanıcı akışı düzenleme profili**.

1. Azure AD B2C portal sayfasında, seçin **kullanıcı akışı** tıklatıp **yeni kullanıcı akışı**.
2. Üzerinde **önerilen** sekmesinde **profil düzenleme**.

    Kullanıcı akışınızı yapılandırmak için aşağıdaki ayarları kullanın:

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Ad** | SiPe | Girin bir **adı** kullanıcı akışı için. Userjourney adı ön ekine sahip **B2C_1_**. Tam userjourney adı kullandığınız **B2C_1_SiPe** örnek kodda. | 
    | **Kimlik sağlayıcıları** | Yerel Hesap Oturum Açma Bilgileri | Kimlik sağlayıcı, kullanıcıyı benzersiz şekilde tanımlamak için kullanılır. |

3. Altında **kullanıcı öznitelikleri**, tıklayın **daha fazla Göster** ve aşağıdaki ayarları seçin:

    | Sütun      | Önerilen değerler  | Açıklama                                        |
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

## <a name="update-desktop-app-code"></a>Masaüstü uygulaması kodunu güncelleştirme

Masaüstü uygulaması ve oluşturulan kullanıcı akışları edindikten sonra Azure AD B2C kiracınızı kullanacak şekilde yapılandırmanız gerekir. Bu öğretici örnek bir masaüstü uygulamasını yapılandıracaksınız. 

[Bir zip dosyası indirin](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop/archive/master.zip), [depoya göz atın](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop) veya GitHub’dan örneği kopyalayın.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop.git
```

Örnek WPF masaüstü uygulaması, bir masaüstü uygulamasının kullanıcıyı kaydetme, kullanıcının oturumunu açma ve korumalı bir web API’sini çağırma işlemleri için Azure AD B2C’yi nasıl kullanabileceğini gösterir.

Kiracınızda uygulama kaydını kullanmak ve oluşturduğunuz kullanıcı akışları yapılandırmak için uygulamayı değiştirmeniz gerekir. 

Uygulama ayarlarını değiştirmek için:

1. `active-directory-b2c-wpf` çözümünü Visual Studio’da açın.

2. `active-directory-b2c-wpf` projesinde, **App.xaml.cs** dosyasını açın ve aşağıdaki güncelleştirmeleri yapın:

    ```C#
    private static string Tenant = "<your-tenant-name>.onmicrosoft.com";
    private static string ClientId = "The Application ID for your desktop app registered in your tenant";
    ```

3. Güncelleştirme **Policysignupsignın** değişken ile *kaydolma veya oturum açma kullanıcı akışı* bir önceki adımda oluşturduğunuz ad. *B2C_1_* ön ekini kullanmayı unutmayın.

    ```C#
    public static string PolicySignUpSignIn = "B2C_1_SiUpIn";
    ```

## <a name="run-the-sample-desktop-application"></a>Örnek masaüstü uygulamasını çalıştırma

Masaüstü uygulamasını oluşturup çalıştırmak için **F5** tuşuna basın. 

Örnek uygulama kaydolma, oturum açma, profil düzenleme ve parola sıfırlama işlemlerini destekler. Bu öğretici, kullanıcının uygulamayı kullanmak için e-posta adresiyle nasıl kaydolacağını vurgular. Diğer senaryoları kendi kendinize keşfedebilirsiniz.

### <a name="sign-up-using-an-email-address"></a>E-posta adresi kullanarak kaydolma

1. Masaüstü uygulamasının bir kullanıcısı olarak kaydolmak için **Oturum Aç** düğmesine tıklayın. Bu kullanır **b2c_1_siupın** kullanıcı akışı, bir önceki adımda tanımladığınız.

2. Azure AD B2C, kayıt bağlantısı içeren bir oturum açma sayfası sunar. Henüz bir hesabınız yoksa **Hemen kaydol** bağlantısına tıklayın. 

3. Kaydolma iş akışında, kullanıcı kimliğini bir e-posta adresi kullanarak toplamak ve doğrulamak için bir sayfa sunar. Kaydolma iş akışı, ayrıca kullanıcının parolasını ve kullanıcı Akış içinde tanımlanmış istenen öznitelikleri toplar.

    Geçerli bir e-posta adresi kullanın ve doğrulama kodunu kullanarak doğrulamayı gerçekleştirin. Parola ayarlayın. İstenen öznitelikler için değerleri girin. 

    ![Kaydolma iş akışı](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.png)

4. Azure AD B2C kiracısında yerel bir hesap oluşturmak için **Oluştur**’a tıklayın.

Artık kullanıcı, oturum açmak ve masaüstü uygulamasını kullanmak için e-posta adresini kullanabilir.

> [!NOTE]
> **API’yi çağırma** düğmesine tıklarsanız "Yetkisiz" hatasıyla karşılaşırsınız. Bu hatayı, tanıtım kiracısındaki bir kaynağa erişmeye çalıştığınız için alırsınız. Erişim belirteci yalnızca Azure AD kiracınız için geçerli olduğundan, API çağrısı yetkilendirilmez. Kiracınız için korumalı bir web API’si oluşturmak amacıyla sonraki öğreticiye geçin. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Azure AD B2C öğreticilerini denemeyi planlıyorsanız Azure AD B2C kiracınızı kullanabilirsiniz. Artık ihtiyaç duymuyorsanız [Azure AD B2C kiracınızı silebilirsiniz](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure AD B2C kiracısı oluşturmayı, kullanıcı akışları oluşturma ve Azure AD B2C kiracınızı kullanmak için örnek masaüstü uygulamasını güncelleştirmeyi öğrendiniz. Bir masaüstü uygulamasındaki korumalı bir web API’sini kaydetmeyi, yapılandırmayı ve çağırmayı öğretmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> 
