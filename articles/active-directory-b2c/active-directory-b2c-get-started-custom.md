---
title: Azure Active Directory B2C özel ilkeleri kullanmaya başlama | Microsoft Docs
description: Azure Active Directory B2C özel ilkeleri kullanmaya başlama konusunda.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 66ce0438e0cca62cbebb993be4940a93973c78d3
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2019
ms.locfileid: "55078993"
---
# <a name="get-started-with-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C özel ilkeleri kullanmaya başlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

[Özel ilkeler](active-directory-b2c-overview-custom.md) , Azure Active Directory (Azure AD) B2C kiracınızın davranışlarını tanımlayan yapılandırma dosyalarıdır. Bu makalede, bir e-posta adresi ve parola kullanarak yerel hesap kaydolma veya oturum açma destekleyen özel bir ilke oluşturun. Ayrıca, Facebook gibi kimlik sağlayıcılarla eklemek için ortamınızın de hazırlayın.

## <a name="prerequisites"></a>Önkoşullar

Zaten yoksa, yapmanız [bir Azure AD B2C kiracısı oluşturmayı](tutorial-create-tenant.md) Azure aboneliğinize bağlı.

## <a name="add-signing-and-encryption-keys"></a>İmzalama ve şifreleme anahtarları Ekle

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme. 
3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Genel bakış sayfasında **kimlik deneyimi çerçevesi - PREVIEW**.

### <a name="create-the-signing-key"></a>İmzalama anahtarı oluşturma

1. Seçin **ilke anahtarları** seçip **Ekle**.
2. İçin **seçenekleri**, seçin `Generate`.
3. İçinde **adı**, girin `TokenSigningKeyContainer`. Önek `B2C_1A_` otomatik olarak eklenebilir.
4. İçin **anahtar türü**seçin **RSA**.
5. İçin **anahtar kullanımı**seçin **imza**.
6. **Oluştur**’a tıklayın.

### <a name="create-the-encryption-key"></a>Şifreleme anahtarını oluşturma

1. Seçin **ilke anahtarları** seçip **Ekle**.
2. İçin **seçenekleri**, seçin `Generate`.
3. İçinde **adı**, girin `TokenEncryptionKeyContainer`. Önek `B2C_1A`_ eklenecek otomatik olarak.
4. İçin **anahtar türü**seçin **RSA**.
5. İçin **anahtar kullanımı**seçin **şifreleme**.
6. **Oluştur**’a tıklayın.

### <a name="create-the-facebook-key"></a>Facebook anahtarı oluşturma

Zaten bir [Facebook uygulama gizli anahtarı](active-directory-b2c-setup-fb-app.md), kiracınız için bir ilke anahtarı ekleyin. Aksi takdirde, böylece ilkelerinizi geçemediğinden bir yer tutucu değerini, anahtarı oluşturmanız gerekir.

1. Seçin **ilke anahtarları** seçip **Ekle**.
2. İçin **seçenekleri**, seçin `Manual`.
3. İçin **adı**, girin `FacebookSecret`. Önek `B2C_1A_` otomatik olarak eklenebilir.
4. İçinde **gizli**, Facebook gizli developers.facebook.com girin veya `0` yer tutucu olarak. Uygulama Kimliği gizli dizi budur.
5. İçin **anahtar kullanımı**seçin **imza**.
6. **Oluştur**’a tıklayın.

## <a name="register-an-application"></a>Bir uygulamayı kaydetme

Uygulamaya kaydolun ve kiracınızda var olan yerel bir hesap ile oturum açmasına etkinleştirmek için Azure Active Directory (Azure AD) B2C kaydedilmiştir. Kullanıcılarınızı benzersiz e-posta adresi ve parola ile kayıtlı bir uygulamaya erişmek için kaydolun.

1. Seçin **tüm hizmetleri** Azure portalının sol üst köşedeki arayın ve seçin **uygulama kayıtları**.
2. **Yeni uygulama kaydı**’nı seçin.
3. İçin **adı**, girin `ProxyIdentityExperienceFramework`.
4. İçin **uygulama türü**, seçin **yerel**.
5. İçin **yeniden yönlendirme URI'si**, girin `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com`burada `your-tenant-name` Azure AD B2C kiracınızın adıdır.
6. **Oluştur**’a tıklayın. Oluşturulduktan sonra uygulama kimliği kopyalayın ve daha sonra kullanmak üzere kaydedin.
7. Seçin **izinler**, seçerek onaylayın **Evet**.

## <a name="download-starter-pack-and-modify-policies"></a>Başlangıç paketi indirin ve ilkeleri değiştirme

Özel ilkeler, Azure AD B2C kiracınıza yüklenmesi gerekir XML dosyaları kümesidir. Başlangıç paketleri dosyaları hızlı bir şekilde kullanmaya başlamanızı sağlayacak sağlanır. Aşağıdaki listede her başlangıç paketi teknik profiller ve kullanıcı yolculuklarından açıklanan senaryoları elde etmek için gereken en küçük sayısı içerir:

- LocalAccounts - yalnızca yerel hesaplar kullanımını etkinleştirir.
- SocialAccounts - yalnızca sosyal (ya da Federasyon) hesapları kullanımını etkinleştirir.
- SocialAndLocalAccounts - kullanımı yerel hesaplar ve sosyal medya hesaplarını sağlar.
- SocialAndLocalAccountsWithMFA - etkinleştirir sosyal, yerel ve çok faktörlü kimlik doğrulaması seçenekleri.

Her başlangıç paketi içerir:

- Temel dosya. Birkaç temel için gereklidir.
* Uzantı dosyası.  Çoğu yapılandırma değişikliği burada yapılan bu dosyasıdır.
* Bağlı olan taraf dosyalar. Göreve özel dosyalar, uygulamanız tarafından çağrılır.

>[!NOTE]
>XML Düzenleyicisi'ni doğrulama destekliyorsa, dosyaların başlangıç paketi kök dizininde bulunan TrustFrameworkPolicy_0.3.0.0.xsd XML şemasına karşı doğrulayın. XML şema doğrulaması karşıya yüklemeden önce hataları tanımlar.

1. [.Zip dosyasını indirdikten](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) veya çalıştırın:

    ```console
    git clone https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
    ```

2. Tüm dosyaları değiştirme SocialAndLocalAccounts klasöründe Düzenle `yourtenant` kiracınız için ada sahip. Örneğin, `contosoTenant.onmicrosoft.com`. Bir XML düzenleyicisini gerekiyorsa [Visual Studio Code deneyin](https://code.visualstudio.com/download), basit bir platformlar arası Düzenleyici.

### <a name="add-application-ids-to-the-custom-policy"></a>Uygulama kimlikleri için özel ilke ekleme

Uygulama Kimliği uzantıları dosyaya ekleyin *TrustFrameworkExtensions.xml*.

1. Açık *TrustFrameworkExtensions.xml* öğesini bulun ve dosya `<TechnicalProfile Id="login-NonInteractive">`.
2. Her iki değeri değiştirin `client_id` ve `resource_id` daha önce oluşturduğunuz ProxyIdentityExperienceFramework uygulamanın uygulama kimliği.
3. Uzantıları dosyanızı kaydedin.

## <a name="upload-the-policies"></a>İlke karşıya yükleme

1. Kimlik deneyimi çerçevesi özel ilkeler sayfasında **karşıya yükleme İlkesi**.
1. Bu sırada, karşıya yükleme *TrustFrameworkBase.xml*, *TrustFrameworkExtensions.xml*, *SignUpOrSignin.xml*, *ProfileEdit.xml* , ve *PasswordReset.xml*. Bir dosya yüklendiğinde ilkesi dosyasının adı ile eklenir `B2C_1A_`.

## <a name="test-the-custom-policy"></a>Özel bir ilkeyi test etme

1. Özel ilkeler sayfasında, seçin **B2C_1A_signup_signin**. 
2. Seçin **Şimdi Çalıştır**.

3. Bir e-posta adresi kullanarak kaydolma olması gerekir.

4. Aynı doğru yapılandırmasına sahip olduğunu onaylamak için hesabınızla oturum açın.

## <a name="add-facebook-as-an-identity-provider"></a>Facebook kimlik sağlayıcısı olarak Ekle

1. Yapılandırma bir [Facebook uygulaması](active-directory-b2c-setup-fb-app.md).
2. İçinde *TrustFrameworkExtensions.xml* dosyası, değiştirin `client_id` Facebook uygulama kimliği:

   ```xml
   <TechnicalProfile Id="Facebook-OAUTH">
     <Metadata>
     <!--Replace the value of client_id in this technical profile with the Facebook app ID"-->
       <Item Key="client_id">00000000000000</Item>
   ```
3. Karşıya yükleme *TrustFrameworkExtensions.xml* kiracınıza dosya.
4. Kullanarak test **Şimdi Çalıştır** veya uygulamanızı ilkeden doğrudan çağırarak.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Active Directory kimlik sağlayıcısı olarak ekleyin. Bu başlangıç kılavuzunda zaten kullanılan taban dosyası diğer kimlik sağlayıcılardan eklemek için gereksinim duyduğunuz İçeriklerinden bazılarını içerir. Oturum açma ayarlama hakkında daha fazla bilgi için bkz: [özel ilkeleri Active Directory B2C kullanarak bir Azure Active Directory hesabı ile kaydolma ve oturum açma ayarlama](active-directory-b2c-setup-aad-custom.md) makalesi.
