---
title: "Azure Active Directory B2C: özel ilkelerini kullanmaya başlama | Microsoft Docs"
description: "Nasıl Azure Active Directory B2C özel ilkelerini kullanmaya başlama"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: mtillman
editor: rojasja
ms.assetid: 658c597e-3787-465e-b377-26aebc94e46d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: joroja;parahk;gsacavdm
ms.openlocfilehash: 826211dca59128a8b87ace44348dd5e2764bc0c3
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-get-started-with-custom-policies"></a>Azure Active Directory B2C: özel ilkelerini kullanmaya başlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makaledeki adımları tamamladıktan sonra özel ilkeniz "yerel hesabı" destekleyecek kaydolma veya oturum açma aracılığıyla bir e-posta adresi ve parola. Ayrıca, kimlik sağlayıcıları (örneğin, Facebook veya Azure Active Directory) eklemek için ortamınızın hazırlar. Azure Active Directory (Azure AD) B2C kimlik deneyimi Framework'ün diğer kullanımlar okumayı önce bu adımları tamamlamak için önerilir.

## <a name="prerequisites"></a>Ön koşullar

Devam etmeden önce tüm kullanıcıların, uygulamaları, ilkeleri ve daha fazla bilgi için bir kapsayıcıdır bir Azure AD B2C kiracısı olduğundan emin olun. Zaten yoksa, yapmanız [bir Azure AD B2C kiracısı oluşturma](active-directory-b2c-get-started.md). Azure AD B2C yerleşik ilke tamamlamak ve devam etmeden önce yerleşik ilkeler uygulamalarını yapılandırmak için tüm geliştiriciler kesinlikle öneririz. Özel ilke çağırmak için ilke adı küçük bir değişiklik yaptıktan sonra uygulamalarınızın her iki tür ilke ile çalışır.

>[!NOTE]
>Özel ilke düzenleme erişmek için kiracınıza bağlı geçerli bir Azure aboneliği gerekir. Siz yapmadıysanız [bir Azure aboneliğine Azure AD B2C kiracısına bağlı](active-directory-b2c-how-to-enable-billing.md) veya Azure aboneliğiniz devre dışı bırakılmış, kimlik deneyimi Framework düğmesi kullanılamaz.

## <a name="add-signing-and-encryption-keys-to-your-b2c-tenant-for-use-by-custom-policies"></a>İmzalama ve şifreleme anahtarlarını B2C kiracınızın kullanmak için özel ilkeleri tarafından ekleyin

1. Açık **kimlik deneyimi Framework** dikey penceresinde, Azure AD B2C Kiracı ayarlarınızı.
2. Seçin **İlkesi anahtarları** kiracınızda kullanılabilir tuşlarını görmek için.
3. Henüz yoksa B2C_1A_TokenSigningKeyContainer oluşturun:<br>
    a. **Add (Ekle)** seçeneğini belirleyin. <br>
    b. Seçin **oluşturmak**.<br>
    c. İçin **adı**, kullanmak `TokenSigningKeyContainer`. <br> 
    Önek `B2C_1A_` otomatik olarak eklenebilir.<br>
    d. İçin **anahtar türü**, kullanın **RSA**.<br>
    e. İçin **tarihleri**, varsayılanları kullanın. <br>
    f. İçin **anahtar kullanımı**, kullanın **imza**.<br>
    g. **Oluştur**’u seçin.<br>
4. Henüz yoksa B2C_1A_TokenEncryptionKeyContainer oluşturun:<br>
 a. **Add (Ekle)** seçeneğini belirleyin.<br>
 b. Seçin **oluşturmak**.<br>
 c. İçin **adı**, kullanmak `TokenEncryptionKeyContainer`. <br>
   Önek `B2C_1A`_ eklenmesi otomatik olarak.<br>
 d. İçin **anahtar türü**, kullanın **RSA**.<br>
 e. İçin **tarihleri**, varsayılanları kullanın.<br>
 f. İçin **anahtar kullanımı**, kullanın **şifreleme**.<br>
 g. **Oluştur**’u seçin.<br>
5. B2C_1A_FacebookSecret oluşturun. <br>
Facebook uygulama gizli anahtarı zaten varsa, kiracınız için bir ilke anahtar olarak ekleyin. Aksi takdirde, böylece ilkelerinizi geçemediğinden bir yer tutucu değerle anahtarı oluşturmanız gerekir.<br>
 a. **Add (Ekle)** seçeneğini belirleyin.<br>
 b. İçin **seçenekleri**, kullanın **el ile**.<br>
 c. İçin **adı**, kullanmak `FacebookSecret`. <br>
 Önek `B2C_1A_` otomatik olarak eklenebilir.<br>
 d. İçinde **gizli** kutusuna, developers.facebook.com, FacebookSecret girin veya `0` yer tutucu olarak. *Bu, Facebook uygulama kimliği değil.* <br>
 e. İçin **anahtar kullanımı**, kullanın **imza**. <br>
 f. Seçin **oluşturma** ve oluşturma onaylayın.

## <a name="register-identity-experience-framework-applications"></a>Kimlik deneyimi Framework uygulamalarını kaydetme

Azure AD B2C altyapısı tarafından kaydolma ve kullanıcılar oturum açma için kullanılan iki ek uygulamalar kaydetmenizi gerektirir.

>[!NOTE]
>Bu etkinleştirme yerel hesap kullanarak oturum açın, iki uygulama oluşturmanız gerekir: IdentityExperienceFramework (bir web uygulaması) ve ProxyIdentityExperienceFramework (yerel bir uygulama) ile temsilci IdentityExperienceFramework uygulama izni. Yerel hesap yalnızca kiracınızda yok. Kullanıcılarınızı benzersiz e-posta adresi/parola bileşimi ile Kiracı kayıtlı uygulamalarınızı erişmek için kaydolun.

### <a name="create-the-identityexperienceframework-application"></a>IdentityExperienceFramework uygulaması oluşturma

1. İçinde [Azure portal](https://portal.azure.com), içine geçiş [Azure AD B2C kiracınızın bağlamında](active-directory-b2c-navigate-to-b2c-context.md).
2. Açık **Azure Active Directory** dikey (değil **Azure AD B2C** dikey penceresinde). Seçmeniz gerekebilir **daha Hizmetleri** bulmak için.
3. Seçin **uygulama kayıtlar**.
4. Seçin **yeni uygulama kaydı**.
   * İçin **adı**, kullanmak `IdentityExperienceFramework`.
   * İçin **uygulama türü**, kullanın **Web app/API**.
   * İçin **oturum açma URL'si**, kullanın `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`, burada `yourtenant` , Azure AD B2C Kiracı etki alanı adıdır.
5. **Oluştur**’u seçin.
6. Bir kez oluşturulduktan sonra yeni oluşturulan uygulamayı seçin **IdentityExperienceFramework**.<br>
   * Seçin **özellikleri**.<br>
   * Uygulama Kimliği kopyalayın ve daha sonra kullanmak üzere kaydedin.

### <a name="create-the-proxyidentityexperienceframework-application"></a>ProxyIdentityExperienceFramework uygulaması oluşturma

1. Seçin **uygulama kayıtlar**.
1. Seçin **yeni uygulama kaydı**.
   * İçin **adı**, kullanmak `ProxyIdentityExperienceFramework`.
   * İçin **uygulama türü**, kullanın **yerel**.
   * İçin **yeniden yönlendirme URI'si**, kullanın `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`, burada `yourtenant` , Azure AD B2C kiracısı.
1. **Oluştur**’u seçin.
1. Oluşturulduktan sonra uygulamayı seçin **ProxyIdentityExperienceFramework**.<br>
   * Seçin **özellikleri**. <br>
   * Uygulama Kimliği kopyalayın ve daha sonra kullanmak üzere kaydedin.
1. Seçin **gerekli izinleri**.
1. **Add (Ekle)** seçeneğini belirleyin.
1. Seçin **bir API seçin**.
1. IdentityExperienceFramework adı arayın. Seçin **IdentityExperienceFramework** sonuçları ve ardından **seçin**.
1. Onay kutusunu işaretleyin **erişim IdentityExperienceFramework**ve ardından **seçin**.
1. Seçin **Bitti**.
1. Seçin **izinler**ve ardından seçerek onaylayın **Evet**.

## <a name="download-starter-pack-and-modify-policies"></a>Başlatıcı paketini karşıdan yüklemek ve ilkelerini değiştirme

Özel ilkeler, Azure AD B2C kiracınızın yüklenebilmesi için gereken XML dosyaları kümesidir. Başlangıç paketleri hızlı bir şekilde giderek sağlamak için sunuyoruz. Aşağıdaki listede her başlangıç paketi teknik profilleri ve kullanıcı Yolculuklar açıklanan senaryoları ulaşmak için gereken en az sayıda içerir:
 * LocalAccounts. Yalnızca yerel hesapların kullanılmasını sağlar.
 * SocialAccounts. Yalnızca sosyal (ya da Federasyon) hesapları kullanımını etkinleştirir.
 * **SocialAndLocalAccounts**. Bu dosyayı gözden geçirme için kullanırız.
 * SocialAndLocalAccountsWithMFA. Sosyal, yerel ve çok faktörlü kimlik doğrulama seçeneklerini buraya dahil edilir.

Her bir başlangıç paketi içerir:

* [Temel dosyanın](active-directory-b2c-overview-custom.md#policy-files) ilkesi. Birkaç değişiklik tabanına gereklidir.
* [Uzantısının](active-directory-b2c-overview-custom.md#policy-files) ilkesi.  Burada çoğu yapılandırma değişikliklerinin yapılması bu dosyasıdır.
* [Bağlı olan taraf dosyaları](active-directory-b2c-overview-custom.md#policy-files) görev özgü dosyaları, uygulamanız tarafından çağrılır.

>[!NOTE]
>XML düzenleyicinizi doğrulama destekliyorsa, başlangıç paketinin kök dizininde bulunan TrustFrameworkPolicy_0.3.0.0.xsd XML Şeması karşı dosyaları doğrulayın. XML şema doğrulaması karşıya yüklemeden önce hataları tanımlar.

 Haydi başlayalım:

1. Github'dan active-directory-b2c-custom-policy-starterpack indirin. [.Zip dosyasını indirdikten](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) veya çalıştırın

    ```console
    git clone https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
    ```
2. SocialAndLocalAccounts klasörünü açın.  Bu klasör temel dosyasında (TrustFrameworkBase.xml) hem yerel hem de sosyal/şirket hesapları için gereken içerik içerir. Sosyal içerik yerel hesaplar Başlarken hazırlarken ve çalışırken için adımlara engellemez.
3. TrustFrameworkBase.xml açın. Bir XML Düzenleyici, gerekirse [Visual Studio Code deneyin](https://code.visualstudio.com/download), basit bir platformlar arası Düzenleyici.
4. Kök `TrustFrameworkPolicy` öğesi, güncelleştirme `TenantId` ve `PublicPolicyUri` değiştirme öznitelikleri `yourtenant.onmicrosoft.com` Azure AD B2C kiracınızın etki alanı adı:
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
   >`PolicyId`Portalda görmek ilke adını ve bu ilke dosyası diğer ilke dosyaları tarafından başvuruluyor adıdır.

5. Dosyayı kaydedin.
6. TrustFrameworkExtensions.xml açın. Aynı iki değiştirerek değişiklik `yourtenant.onmicrosoft.com` , Azure AD B2C kiracısı ile. Aynı değiştirme olun `<TenantId>` üç değişiklikleri toplam öğesi. Dosyayı kaydedin.
7. SignUpOrSignIn.xml açın. Aynı değiştirerek değişiklik `yourtenant.onmicrosoft.com` üç yerde, Azure AD B2C kiracısı ile. Dosyayı kaydedin.
8. Parola sıfırlama açın ve profil dosyalarını düzenleyin. Aynı değiştirerek değişiklik `yourtenant.onmicrosoft.com` Azure AD B2C kiracınızda her dosya üç yerde ile. Dosyaları kaydedin.

### <a name="add-the-application-ids-to-your-custom-policy"></a>Özel ilkeniz uygulama kimlikleri Ekle
Uygulama kimlikleri uzantıları dosyasına ekleyin (`TrustFrameworkExtensions.xml`):

1. Uzantıları dosyasında (TrustFrameworkExtensions.xml) öğesi bulunamıyor `<TechnicalProfile Id="login-NonInteractive">`.
2. Her iki örneği yerine `IdentityExperienceFrameworkAppId` daha önce oluşturduğunuz kimlik deneyimi Framework uygulamanın uygulama Kimliğine sahip. Örnek aşağıda verilmiştir:

   ```xml
   <Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
   ```
3. Her iki örneği yerine `ProxyIdentityExperienceFrameworkAppId` daha önce oluşturduğunuz Proxy kimlik deneyimi Framework uygulamanın uygulama Kimliğine sahip.
4. Uzantıları dosyanızı kaydedin.

## <a name="upload-the-policies-to-your-tenant"></a>İlkeleri kiracınız için karşıya yükleme

1. İçinde [Azure portal](https://portal.azure.com), içine geçiş [bağlam Azure AD B2C kiracınızın](active-directory-b2c-navigate-to-b2c-context.md), açarak **Azure AD B2C** dikey.
2. Seçin **kimlik deneyimi Framework**.
3. Seçin **karşıya İlkesi**.

    >[!WARNING]
    >Özel ilke dosyaları aşağıdaki sırayla yüklenmelidir:

1. TrustFrameworkBase.xml karşıya yükleyin.
2. TrustFrameworkExtensions.xml karşıya yükleyin.
3. SignUpOrSignin.xml karşıya yükleyin.
4. Diğer ilke dosyalarınızı karşıya yükleyin.

Bir dosyayı karşıya yüklendiğinde, ilke dosyasının adı ile eklenir `B2C_1A_`.

## <a name="test-the-custom-policy-by-using-run-now"></a>Özel ilke Şimdi Çalıştır kullanarak test

1. Açık **Azure AD B2C ayarlarını** ve Git **kimlik deneyimi Framework**.

   >[!NOTE]
   >**Şimdi Çalıştır** Kiracı'preregistered için en az bir uygulama gerekiyor. Aşağıdakilerden birini kullanarak B2C kiracınızda uygulamaları kayıtlı **uygulamaları** menü seçimi Azure AD B2C veya yerleşik ve özel ilkeler çağrılacak kimlik deneyimi Framework kullanarak. Uygulama başına yalnızca bir kayıt gerekiyor.<br><br>
   Uygulamaları kaydetmek öğrenmek için Azure AD B2C bkz [başlama](active-directory-b2c-get-started.md) makale veya [uygulama kaydı](active-directory-b2c-app-registration.md) makale.  

2. B2C_1A_signup_signin, karşıya yüklediğiniz bağlı olan taraf (RP) özel İlkesi'ni açın. Seçin **Şimdi Çalıştır**.

3. Bir e-posta adresi kullanarak kaydolabilirsiniz olması gerekir.

4. Doğru yapılandırma sahip olduğunuzu onaylamak için aynı hesapla oturum açın.

>[!NOTE]
>Oturum açma hatası ortak bir nedeni yanlış yapılandırılmış bir IdentityExperienceFramework uygulamadır.


## <a name="next-steps"></a>Sonraki adımlar

### <a name="add-facebook-as-an-identity-provider"></a>Facebook kimlik sağlayıcısı olarak Ekle
Facebook ayarlamak için:
1. [Bir Facebook uygulaması içinde developers.facebook.com yapılandırın](active-directory-b2c-setup-fb-app.md).
2. [Azure AD B2C kiracınızın Facebook uygulama gizli anahtarı eklemek](#add-signing-and-encryption-keys-to-your-b2c-tenant-for-use-by-custom-policies).
3. TrustFrameworkExtensions ilke dosyasında değerini değiştirin `client_id` Facebook uygulama kimliği:

   ```xml
   <TechnicalProfile Id="Facebook-OAUTH">
     <Metadata>
     <!--Replace the value of client_id in this technical profile with the Facebook app ID"-->
       <Item Key="client_id">00000000000000</Item>
   ```
4. TrustFrameworkExtensions.xml ilke dosyası kiracınıza karşıya yükleyin.
5. Kullanarak test **Şimdi Çalıştır** veya kayıtlı uygulamanızı ilkesinden doğrudan çağırma.

### <a name="add-azure-active-directory-as-an-identity-provider"></a>Azure Active Directory kimlik sağlayıcısı olarak Ekle
Bu başlangıç kılavuzuna içinde zaten kullanılan temel dosyanın diğer kimlik sağlayıcılardan eklemek için gereksinim duyduğunuz içeriğin bir kısmı içerir. Oturum açma işlemleri ayarlama hakkında daha fazla bilgi için bkz: [Azure Active Directory B2C: Azure AD hesapları kullanarak oturum](active-directory-b2c-setup-aad-custom.md) makalesi.

Azure AD B2C'ın kimlik deneyimi Framework kullanan özel ilkelerinde genel bakış için bkz: [Azure Active Directory B2C: özel ilkeler](active-directory-b2c-overview-custom.md) makalesi. 
