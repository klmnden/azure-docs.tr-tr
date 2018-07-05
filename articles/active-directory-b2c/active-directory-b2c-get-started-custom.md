---
title: Azure Active Directory B2C özel ilkeleri kullanmaya başlama | Microsoft Docs
description: Azure Active Directory B2C özel ilkeleri kullanmaya başlama konusunda.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e25103d2fcbfc70be7f96f5c0e5fa6abe13fe393
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37446750"
---
# <a name="azure-active-directory-b2c-get-started-with-custom-policies"></a>Azure Active Directory B2C: özel ilkeleri kullanmaya başlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makaledeki adımları tamamladıktan sonra özel ilkeniz "yerel hesabı" destekleyecek kaydolma veya oturum açma aracılığıyla bir e-posta adresi ve parola. Ayrıca, kimlik sağlayıcıları (örneğin, Facebook veya Azure Active Directory) eklemek için ortamınızı hazırlar. Azure Active Directory (Azure AD) B2C kimlik deneyimi çerçevesi diğer kullanımları okumayı önce bu adımları tamamlamak için önerilir.

## <a name="prerequisites"></a>Önkoşullar

Devam etmeden önce tüm kullanıcılar, uygulamaları, ilkeleri ve daha fazla bilgi için bir kapsayıcıdır bir Azure AD B2C kiracısı olduğundan emin olun. Zaten yoksa, yapmanız [bir Azure AD B2C kiracısı oluşturmayı](active-directory-b2c-get-started.md). Azure AD B2C'yi yerleşik ilke tamamlamak ve devam etmeden önce yerleşik ilkeler uygulamalarını yapılandırmak için tüm geliştiriciler önemle öneririz. Özel ilke çağırmak için ilke adına küçük bir değişiklik yaptıktan sonra uygulamalarınızı her iki ilke türünde çalışır.

>[!NOTE]
>Özel ilke düzenleme erişmek için kiracınıza bağlı geçerli bir Azure aboneliği gerekir. Siz yapmadıysanız [Azure AD B2C kiracınızı Azure aboneliğine bağlı](active-directory-b2c-how-to-enable-billing.md) veya Azure aboneliğiniz devre dışı, kimlik deneyimi çerçevesi düğmesi kullanılamaz.

## <a name="add-signing-and-encryption-keys-to-your-b2c-tenant-for-use-by-custom-policies"></a>İmzalama ve şifreleme anahtarları B2C kiracınızı kullanmak için özel ilkeleri tarafından ekleyin.

1. Açık **kimlik deneyimi çerçevesi** dikey penceresinde, Azure AD B2C Kiracı ayarları.
2. Seçin **ilke anahtarları** kiracınızda kullanılabilir tuşlarını görmek için.
3. Henüz yoksa B2C_1A_TokenSigningKeyContainer oluşturun:<br>
    a. **Add (Ekle)** seçeneğini belirleyin. <br>
    b. Seçin **oluşturmak**.<br>
    c. İçin **adı**, kullanın `TokenSigningKeyContainer`. <br> 
    Önek `B2C_1A_` otomatik olarak eklenebilir.<br>
    d. İçin **anahtar türü**, kullanın **RSA**.<br>
    e. İçin **tarihleri**, varsayılan ayarları kullanın. <br>
    f. İçin **anahtar kullanımı**, kullanın **imza**.<br>
    g. **Oluştur**’u seçin.<br>
4. Henüz yoksa B2C_1A_TokenEncryptionKeyContainer oluşturun:<br>
 a. **Add (Ekle)** seçeneğini belirleyin.<br>
 b. Seçin **oluşturmak**.<br>
 c. İçin **adı**, kullanın `TokenEncryptionKeyContainer`. <br>
   Önek `B2C_1A`_ eklenecek otomatik olarak.<br>
 d. İçin **anahtar türü**, kullanın **RSA**.<br>
 e. İçin **tarihleri**, varsayılan ayarları kullanın.<br>
 f. İçin **anahtar kullanımı**, kullanın **şifreleme**.<br>
 g. **Oluştur**’u seçin.<br>
5. B2C_1A_FacebookSecret oluşturun. <br>
Facebook uygulama gizli anahtarı zaten varsa, kiracınız için bir ilke anahtarı ekleyin. Aksi takdirde, böylece ilkelerinizi geçemediğinden bir yer tutucu değerini, anahtarı oluşturmanız gerekir.<br>
 a. **Add (Ekle)** seçeneğini belirleyin.<br>
 b. İçin **seçenekleri**, kullanın **el ile**.<br>
 c. İçin **adı**, kullanın `FacebookSecret`. <br>
 Önek `B2C_1A_` otomatik olarak eklenebilir.<br>
 d. İçinde **gizli** kutusuna, developers.facebook.com, FacebookSecret girin veya `0` yer tutucu olarak. *Facebook uygulama kimliğiniz değil* <br>
 e. İçin **anahtar kullanımı**, kullanın **imza**. <br>
 f. Seçin **Oluştur** ve oluşturma onaylayın.

## <a name="register-identity-experience-framework-applications"></a>Kimlik deneyimi çerçevesi uygulamaları kaydetme

Azure AD B2C, kaydolma ve oturum açma kullanıcıları için altyapısı tarafından kullanılan iki ek uygulama kaydetmelerine izin gerektirir.

>[!NOTE]
>Bu etkin yerel hesap kullanarak oturum açın, iki uygulama oluşturmanız gerekir: IdentityExperienceFramework (bir web uygulaması) ve ProxyIdentityExperienceFramework (yerel uygulama) ile temsilci IdentityExperienceFramework uygulama izni. Yerel hesaplar yalnızca kiracınızda mevcut. Kullanıcılarınızı benzersiz e-posta adresi/parola bileşimi ile Kiracı kayıtlı uygulamalarınıza erişmek için kaydolun.

### <a name="create-the-identityexperienceframework-application"></a>IdentityExperienceFramework uygulaması oluşturma

1. İçinde [Azure portalında](https://portal.azure.com), içine geçiş [Azure AD B2C kiracınızın bağlamında](active-directory-b2c-navigate-to-b2c-context.md).
2. Açık **Azure Active Directory** dikey (değil **Azure AD B2C** dikey penceresinde). Seçmeniz gerekebilir **diğer hizmetler** bulmak için.
3. **Uygulama kayıtları**'nı seçin.
4. **Yeni uygulama kaydı**’nı seçin.
   * İçin **adı**, kullanın `IdentityExperienceFramework`.
   * İçin **uygulama türü**, kullanın **Web uygulaması/API'si**.
   * İçin **oturum açma URL'si**, kullanın `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`burada `yourtenant` , Azure AD B2C Kiracı etki alanı adıdır.
5. **Oluştur**’u seçin.
6. Oluşturulduktan sonra yeni oluşturduğunuz uygulamayı seçin **IdentityExperienceFramework**.<br>
   * Seçin **özellikleri**.<br>
   * Uygulama Kimliği kopyalayın ve daha sonra kullanmak üzere kaydedin.

### <a name="create-the-proxyidentityexperienceframework-application"></a>ProxyIdentityExperienceFramework uygulaması oluşturma

1. **Uygulama kayıtları**'nı seçin.
1. **Yeni uygulama kaydı**’nı seçin.
   * İçin **adı**, kullanın `ProxyIdentityExperienceFramework`.
   * İçin **uygulama türü**, kullanın **yerel**.
   * İçin **yeniden yönlendirme URI'si**, kullanın `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`burada `yourtenant` Azure AD B2C kiracınızın.
1. **Oluştur**’u seçin.
1. Oluşturulduktan sonra uygulamayı seçin **ProxyIdentityExperienceFramework**.<br>
   * Seçin **özellikleri**. <br>
   * Uygulama Kimliği kopyalayın ve daha sonra kullanmak üzere kaydedin.
1. Seçin **gerekli izinler**.
1. **Add (Ekle)** seçeneğini belirleyin.
1. Seçin **bir API seçin**.
1. ' % S'adı IdentityExperienceFramework arayın. Seçin **IdentityExperienceFramework** sonuçları ve ardından **seçin**.
1. Yanındaki onay kutusunu işaretleyin **erişim IdentityExperienceFramework**ve ardından **seçin**.
1. **Done** (Bitti) öğesini seçin.
1. Seçin **izinler**, seçerek onaylayın **Evet**.

## <a name="download-starter-pack-and-modify-policies"></a>Başlangıç paketi indirin ve ilkeleri değiştirme

Özel ilkeler, Azure AD B2C kiracınıza yüklenmesi gerekir XML dosyaları kümesidir. Başlangıç paketleri hızlı bir şekilde kullanmaya başlamanızı sağlayacak sunuyoruz. Aşağıdaki listede her başlangıç paketi teknik profiller ve kullanıcı yolculuklarından açıklanan senaryoları elde etmek için gereken en küçük sayısı içerir:
 * LocalAccounts. Yalnızca yerel hesapları kullanılmasını sağlar.
 * SocialAccounts. Yalnızca sosyal (ya da Federasyon) hesapları kullanılmasını sağlar.
 * **SocialAndLocalAccounts**. Bu dosyayı gözden geçirme için kullanırız.
 * SocialAndLocalAccountsWithMFA. Sosyal medya, yerel ve çok faktörlü kimlik doğrulaması seçenekleri yer almaktadır.

Her başlangıç paketi içerir:

* [Temel dosya](active-directory-b2c-overview-custom.md#policy-files) ilkesinin. Birkaç temel için gereklidir.
* [Uzantısının](active-directory-b2c-overview-custom.md#policy-files) ilkesinin.  Çoğu yapılandırma değişikliği burada yapılan bu dosyasıdır.
* [Bağlı olan taraf dosyaları](active-directory-b2c-overview-custom.md#policy-files) göreve özel dosyalar, uygulamanız tarafından çağrılır.

>[!NOTE]
>XML Düzenleyicisi'ni doğrulama destekliyorsa, dosyaların başlangıç paketi kök dizininde bulunan TrustFrameworkPolicy_0.3.0.0.xsd XML şemasına karşı doğrulayın. XML şema doğrulaması karşıya yüklemeden önce hataları tanımlar.

 Haydi başlayalım:

1. Github'dan active-directory-b2c-custom-policy-starterpack indirin. [.Zip dosyasını indirdikten](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) veya çalıştırın

    ```console
    git clone https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
    ```
2. SocialAndLocalAccounts klasörü açın.  Bu klasördeki temel dosya (TrustFrameworkBase.xml), hem yerel hem de sosyal/şirket hesapları için gereken içeriği. Yerel hesaplar'ı kullanmaya başlamak ve çalıştırmak için adımlara içeriği social engellemez.
3. TrustFrameworkBase.xml açın. Bir XML düzenleyicisini gerekiyorsa [Visual Studio Code deneyin](https://code.visualstudio.com/download), basit bir platformlar arası Düzenleyici.
4. Kök `TrustFrameworkPolicy` öğesinde, güncelleştirme `TenantId` ve `PublicPolicyUri` değiştirme öznitelikleri `yourtenant.onmicrosoft.com` Azure AD B2C kiracınızın etki alanı adına sahip:
   ```xml
    <TrustFrameworkPolicy
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
    xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
    PolicySchemaVersion="0.3.0.0"
    TenantId="yourtenant.onmicrosoft.com"
    PolicyId="B2C_1A_TrustFrameworkBase"
    PublicPolicyUri="http://yourtenant.onmicrosoft.com">
    ```
   >[!NOTE]
   >`PolicyId` Portalda gördüğünüz ilke adı ve diğer ilke dosyaları tarafından başvurulan Bu ilke dosyası tarafından adıdır.

5. Dosyayı kaydedin.
6. TrustFrameworkExtensions.xml açın. Aynı iki değiştirerek değişiklik `yourtenant.onmicrosoft.com` ile Azure AD B2C kiracınızı. Aynı değiştirme işleminde olun `<TenantId>` üç değişiklikleri toplam öğe. Dosyayı kaydedin.
7. SignUpOrSignIn.xml açın. Değiştirilerek değişiklerin olun `yourtenant.onmicrosoft.com` ile Azure AD B2C kiracınızı üç yerde. Dosyayı kaydedin.
8. Parola sıfırlama açma ve profil dosyalarını düzenleyin. Değiştirilerek değişiklerin olun `yourtenant.onmicrosoft.com` üç yerde her dosya, Azure AD B2C kiracısı ile. Dosyaları kaydedin.

### <a name="add-the-application-ids-to-your-custom-policy"></a>Özel ilkenizi uygulama kimlikleri ekleme
Uygulama kimlikleri uzantıları dosyaya ekleyin (`TrustFrameworkExtensions.xml`):

1. Öğesi (TrustFrameworkExtensions.xml) uzantıları dosyasında bulma `<TechnicalProfile Id="login-NonInteractive">`.
2. Yerine `IdentityExperienceFrameworkAppId` daha önce oluşturduğunuz kimlik deneyimi çerçevesi uygulamanın uygulama kimliği. Örnek aşağıda verilmiştir:

   ```xml
   <Item Key="IdTokenAudience">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
   ```
3. Yerine `ProxyIdentityExperienceFrameworkAppId` daha önce oluşturduğunuz Proxy kimlik deneyimi çerçevesi uygulamanın uygulama kimliği.
4. Uzantıları dosyanızı kaydedin.

## <a name="upload-the-policies-to-your-tenant"></a>İlkeler, kiracınıza karşıya yükleme

1. İçinde [Azure portalında](https://portal.azure.com), içine geçiş [Azure AD B2C kiracınızın bağlamında](active-directory-b2c-navigate-to-b2c-context.md)açın **Azure AD B2C** dikey penceresi.
2. Seçin **kimlik deneyimi çerçevesi**.
3. Seçin **karşıya yükleme İlkesi**.

    >[!WARNING]
    >Özel ilke dosyalarını aşağıdaki sırayla yüklenmelidir:

1. TrustFrameworkBase.xml karşıya yükleyin.
2. TrustFrameworkExtensions.xml karşıya yükleyin.
3. SignUpOrSignin.xml karşıya yükleyin.
4. Diğer ilke dosyalarınızı karşıya yükleyin.

Bir dosya yüklendiğinde ilkesi dosyasının adı ile eklenir `B2C_1A_`.

## <a name="test-the-custom-policy-by-using-run-now"></a>Şimdi Çalıştır'i kullanarak özel bir ilkeyi test

1. Açık **Azure AD B2C ayarlarını** gidin **kimlik deneyimi çerçevesi**.

   >[!NOTE]
   >**Şimdi Çalıştır** en az bir uygulamanın Kiracı'da önceden kayıtlı gerekir. Uygulamaları kullanarak ya da B2C kiracısında kaydedilmelidir **uygulamaları** menü seçimini Azure AD B2C'yi veya yerleşik ve özel ilkeleri çağrılacak kimlik deneyimi çerçevesi kullanarak. Uygulama başına yalnızca bir kayıt gerekiyor.<br><br>
   Azure AD B2C uygulamaları kaydetme hakkında bilgi için bkz [başlama](active-directory-b2c-get-started.md) makale veya [uygulama kaydı](active-directory-b2c-app-registration.md) makalesi.  

2. B2C_1A_signup_signin, karşıya yüklediğiniz bağlı olan taraf (RP) özel bir ilkeyi açın. Seçin **Şimdi Çalıştır**.

3. Bir e-posta adresi kullanarak kaydolma olması gerekir.

4. Aynı doğru yapılandırmasına sahip olduğunu onaylamak için hesabınızla oturum açın.

>[!NOTE]
>Oturum açma hatasının yaygın bir nedeni yanlış yapılandırılmış bir IdentityExperienceFramework uygulamadır.


## <a name="next-steps"></a>Sonraki adımlar

### <a name="add-facebook-as-an-identity-provider"></a>Facebook kimlik sağlayıcısı olarak Ekle
Facebook ayarlamak için:
1. [Bir Facebook uygulaması içinde developers.facebook.com yapılandırma](active-directory-b2c-setup-fb-app.md).
2. [Azure AD B2C kiracınıza Facebook uygulama gizli anahtarı ekleme](#add-signing-and-encryption-keys-to-your-b2c-tenant-for-use-by-custom-policies).
3. TrustFrameworkExtensions ilke dosyasında değiştirin `client_id` Facebook uygulama kimliği:

   ```xml
   <TechnicalProfile Id="Facebook-OAUTH">
     <Metadata>
     <!--Replace the value of client_id in this technical profile with the Facebook app ID"-->
       <Item Key="client_id">00000000000000</Item>
   ```
4. Kiracınıza TrustFrameworkExtensions.xml ilke dosyası yükleyin.
5. Kullanarak test **Şimdi Çalıştır** veya uygulamanızı ilkeden doğrudan çağırarak.

### <a name="add-azure-active-directory-as-an-identity-provider"></a>Kimlik sağlayıcısı olarak Azure Active Directory Ekle
Bu başlangıç kılavuzunda zaten kullanılan taban dosyası diğer kimlik sağlayıcılardan eklemek için gereksinim duyduğunuz İçeriklerinden bazılarını içerir. Oturum açma ayarlama hakkında daha fazla bilgi için bkz. [Azure Active Directory B2C: Azure AD hesapları kullanarak oturum](active-directory-b2c-setup-aad-custom.md) makalesi.

Kimlik deneyimi çerçevesi kullanan özel Azure AD B2C ilkeleri genel bakış için bkz. [Azure Active Directory B2C: özel ilkeleri](active-directory-b2c-overview-custom.md) makalesi. 
