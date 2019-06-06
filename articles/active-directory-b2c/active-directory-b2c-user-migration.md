---
title: Azure Active Directory B2C'de kullanıcı geçişini yaklaşıyor | Microsoft Docs
description: Graph API'sini kullanarak ve isteğe bağlı olarak Azure AD B2C özel ilkeleri kullanarak kullanıcı geçişi, temel ve Gelişmiş kavramlar açıklanmaktadır.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/04/2017
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 1e913b02f99095afb7ee1a3f2122e3c1fe1a60b5
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66507661"
---
# <a name="azure-active-directory-b2c-user-migration"></a>Azure Active Directory B2C: Kullanıcı Geçişi
Azure Active Directory B2C kimlik sağlayıcınız geçirirken (Azure AD B2C) de gerekebilir kullanıcı hesabını geçirin. Bu makalede, var olan kullanıcı hesaplarını herhangi bir kimlik sağlayıcısından Azure AD B2C'ye geçirme açıklanmaktadır. Makalede aşağıdakilerin olması değildir ancak bunun yerine, bunu birkaç senaryolar açıklanmaktadır. Geliştirici, her bir yaklaşıma uygunluğu sorumludur.

## <a name="user-migration-flows"></a>Kullanıcı Geçiş akışları
Azure AD B2C ile kullanıcılar ile geçirebileceğiniz [Azure AD Graph API'si][B2C-GraphQuickStart]. Kullanıcı Geçiş işlemini iki akar döner:

- **Geçiş öncesi**: Bu akış, ya da açık bir kullanıcının kimlik bilgilerini (kullanıcı adı ve parola) erişiminiz veya kimlik bilgileri şifrelenir, ancak bunların şifresini geçerlidir. Geçiş öncesi işlemleri, eski bir kimlik sağlayıcısından gelen kullanıcıları okuma ve yeni hesaplar Azure AD B2C dizini oluşturma içerir.

- **Geçiş öncesi ve parola sıfırlama**: Bir kullanıcının parolasını erişilebilir değil. Bu akış uygulanır. Örneğin:
   - Parola KARMASI biçiminde depolanır.
   - Parola, erişemediğiniz bir kimlik sağlayıcısı depolanır. Eski kimlik sağlayıcınız bir web hizmeti çağrı yaparak kullanıcı kimlik bilgilerini doğrular.

Her iki akış, ilk geçiş öncesi işlemleri çalıştırmak, kullanıcılar eski kimliği sağlayıcınızdan okuyun ve yeni hesaplar Azure AD B2C dizini oluşturun. Parola yoksa rastgele oluşturulmuş bir parola kullanarak hesabı oluşturun. Ardından kullanıcının parolasını değiştirmesini isteyin veya kullanıcı ilk kez oturum açtığında, Azure AD B2C sıfırlamak için kullanıcıya sorar.

## <a name="password-policy"></a>Parola İlkesi
Azure AD B2C parola ilkesini (yerel hesaplar için) Azure AD ilkesine bağlıdır. Azure AD B2C kaydolma veya oturum açma ve parola Sıfırla "güçlü" parola gücünü ilkeleri kullanın ve parolaları sona ermez. Daha fazla bilgi için [Azure AD parola ilkesi][AD-PasswordPolicies].

Geçirmek istediğiniz hesapları daha zayıf bir parola gücünü kullanıyorsanız [Azure AD B2C tarafından zorlanan güçlü parola gücü][AD-PasswordPolicies], güçlü bir parola gereksinimini devre dışı bırakabilirsiniz. Varsayılan Parola ilkesini değiştirmek için Ayarla `passwordPolicies` özelliğini `DisableStrongPassword`. Örneğin, kullanıcı isteği oluştur şu şekilde değiştirebilirsiniz:

```JSON
"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"
```

## <a name="step-1-use-azure-ad-graph-api-to-migrate-users"></a>1. adım: Kullanıcıları geçirme için Azure AD Graph API'sini kullanın
(Parola veya rastgele bir parola ile) Graph API aracılığıyla Azure AD B2C kullanıcı hesabı oluşturun. Bu bölümde, Graph API'sini kullanarak Azure AD B2C dizininde kullanıcı hesaplarını oluşturma işlemi açıklanmaktadır.

### <a name="step-11-register-your-application-in-your-tenant"></a>Adım 1.1: Kiracınızda uygulamanızı kaydetme
Graph API ile iletişim kurmak için öncelikle bir hizmet hesabı yönetici ayrıcalıklarına sahip olması gerekir. Azure AD'de bir uygulama ve kimlik doğrulaması Azure AD'ye kaydedin. Uygulama kimlik bilgileri **uygulama kimliği** ve **uygulama gizli anahtarı**. Uygulama, kendisini, Graph API'sini çağırmak için bir kullanıcı değil, olarak görür.

İlk olarak, geçiş uygulamanızı Azure AD'ye kaydetme. Ardından, bir uygulama anahtarı (uygulama gizli anahtarı) oluşturma ve uygulama yazma ayrıcalıkları ile ayarlayın.

1. [Azure portalında][Portal] oturum açın.
   
1. Azure AD seçin **B2C** Kiracı hesabınızı üst seçerek sağ.
   
1. Sol bölmede seçin **Azure Active Directory** (Azure AD B2C değil). Bulmak için seçmeniz gerekebilir **diğer hizmetler**.
   
1. **Uygulama kayıtları**'nı seçin.
   
1. **Yeni uygulama kaydı**’nı seçin.
   
   ![Yeni uygulama kaydı](media/active-directory-b2c-user-migration/pre-migration-app-registration.png)
   
1. Aşağıdakileri yaparak yeni bir uygulama oluşturun:
   - İçin **adı**, kullanın **B2CUserMigration** veya istediğiniz herhangi bir ad.
   - İçin **uygulama türü**, kullanın **Web uygulaması/API'si**.
   - İçin **oturum açma URL'si**, kullanın `https://localhost` (Bu uygulama için uygun değilse gibi).
   - **Oluştur**’u seçin.
   
1. Uygulama içinde oluşturulduktan sonra **uygulamaları** listesinde, yeni oluşturulan seçin **B2CUserMigration** uygulama.
   
1. Seçin **özellikleri**, kopyalama **uygulama kimliği**ve daha sonra kullanmak üzere kaydedin.

### <a name="step-12-create-the-application-secret"></a>Adım 1.2: Uygulama gizli anahtarı oluşturma
1. Azure portalında **kayıtlı uygulama** penceresinde **anahtarları**.
   
1. (İstemci parolası olarak da bilinir) yeni bir anahtar ekleyin ve ardından daha sonra kullanmak için anahtarı kopyalayın.
   
   ![Uygulama kimliği ve anahtarlar](media/active-directory-b2c-user-migration/pre-migration-app-id-and-key.png)
   
### <a name="step-13-grant-administrative-permission-to-your-application"></a>1.3. adım: Uygulamanız için yönetici izni verme
1. Azure portalında **kayıtlı uygulama** penceresinde **gerekli izinler**.

1. Seçin **Windows Azure Active Directory**.
   
1. İçinde **erişimini etkinleştir** bölmesi altında **uygulama izinleri**seçin **dizin verilerini okuma ve yazma**ve ardından **Kaydet**.
   
1. İçinde **gerekli izinler** bölmesinde **izinler**.
   
   ![Uygulama izinleri](media/active-directory-b2c-user-migration/pre-migration-app-registration-permissions.png)
   
Artık bir uygulama oluşturmak, okumak ve kullanıcıların Azure AD B2C kiracınızı güncelleştirmek için gerekli izinlere sahip olması.

### <a name="step-14-optional-environment-cleanup"></a>Adım 1.4: (İsteğe bağlı) Ortam temizleme
Okuma ve yazma dizin veri izinleri yapmak *değil* kullanıcıları silmek hakkını içerir. Uygulamanız kullanıcıların (ortamınızı temiz) silme olanağı vermek için kullanıcı hesabı yöneticisi izinlerini ayarlamak için PowerShell çalıştırmayı içeren fazladan bir adım gerçekleştirmeniz gerekir. Aksi halde, sonraki bölüme atlayabilirsiniz.

> [!IMPORTANT]
> Bir B2C Kiracı yönetici hesabı kullanmalısınız *yerel* B2C kiracısı için. Hesap adı sözdizimi *yönetici\@contosob2c.onmicrosoft.com*.

>[!NOTE]
> Aşağıdaki PowerShell betiğini gerektirir [Azure Active Directory PowerShell sürüm 2][AD-Powershell].

Bu PowerShell Betiği aşağıdakileri yapın:
1. Çevrimiçi hizmetinize bağlanın. Bunu yapmak için çalıştırın `Connect-AzureAD` cmdlet Windows PowerShell komut isteminde ve kimlik bilgilerinizi sağlayın.
   
1. Kullanım **uygulama kimliği** uygulama kullanıcı hesabı yönetici rolü atama. Tüm yapmanız gereken, bu nedenle girin iyi bilinen tanımlayıcılar, bu roller sahip, **uygulama kimliği** betikteki.
   
```powershell
Connect-AzureAD

$AppId = "<Your application ID>"

# Fetch Azure AD application to assign to role
$roleMember = Get-AzureADServicePrincipal -Filter "AppId eq '$AppId'"

# Fetch User Account Administrator role instance
$role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq 'User Account Administrator'}

# If role instance does not exist, instantiate it based on the role template
if ($role -eq $null) {
    # Instantiate an instance of the role template
    $roleTemplate = Get-AzureADDirectoryRoleTemplate | Where-Object {$_.displayName -eq 'User Account Administrator'}
    Enable-AzureADDirectoryRole -RoleTemplateId $roleTemplate.ObjectId

    # Fetch User Account Administrator role instance again
    $role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq 'User Account Administrator'}
}

# Add application to role
Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId $roleMember.ObjectId

# Fetch role membership for role to confirm
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
```

Değişiklik `$AppId` Azure AD değeriyle **uygulama kimliği**.

## <a name="step-2-pre-migration-application-sample"></a>2. adım: Geçiş öncesi uygulama örneği
[İndirme ve örnek kodu çalıştırma][UserMigrationSample]. Bir .zip dosyası olarak indirebilirsiniz.

### <a name="step-21-edit-the-migration-data-file"></a>2.1. adım: Geçiş Deposu dosyası Düzenle
Örnek uygulamayı işlevsiz kullanıcı verilerinizi içeren bir JSON dosyası kullanır. Örnek çalıştırabilmeniz sonra kendi veritabanındaki verileri kullanmak için kodu değiştirebilirsiniz. Veya kullanıcı profili bir JSON dosyasına aktarın ve ardından bu dosyayı kullanmak için uygulamayı ayarlayın.

JSON dosyasını düzenlemek için açın `AADB2C.UserMigration.sln` Visual Studio çözümü. İçinde `AADB2C.UserMigration` projesini açarsanız `UsersData.json` dosya.

![Kullanıcı veri dosyası](media/active-directory-b2c-user-migration/pre-migration-data-file.png)

Gördüğünüz gibi kullanıcı varlıkları listesi dosyası içerir. Her kullanıcı varlığı, aşağıdaki özelliklere sahiptir:
- email
- displayName
- FirstName
- Soyadı
- Parola (boş olabilir)

> [!NOTE]
> Derleme zamanında Visual Studio dosyaya kopyalar `bin` dizin.

### <a name="step-22-configure-the-application-settings"></a>2.2. adım: Uygulama ayarlarını yapılandırın
Altında `AADB2C.UserMigration` projesini açarsanız *App.config* dosya. Aşağıdaki uygulama ayarlarını kendi değerlerinizle değiştirin:

```XML
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{The ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{The Client Secret Key from above}" />
    <add key="MigrationFile" value="{Name of a JSON file containing the users' data; for example, UsersData.json}" />
    <add key="BlobStorageConnectionString" value="{Your connection Azure table string}" />
</appSettings>
```

> [!NOTE]
> - Bir Azure tablosu bağlantı dizesi kullanımına sonraki bölümde açıklanmıştır.
> - B2C Kiracı adınızın, Kiracı oluşturulduğu sırada girdiğiniz ve Azure portalında görüntülenen etki alanıdır. Kiracı adı genellikle sonekiyle sona erer *. onmicrosoft.com* (örneğin, *contosob2c.onmicrosoft.com*).

### <a name="step-23-run-the-pre-migration-process"></a>Adım 2.3: Geçiş öncesi işlemleri çalıştırma
Sağ `AADB2C.UserMigration` çözüm ve sonra da örnek yeniden oluşturun. Başarılı olursa, şu anda olmalıdır bir `UserMigration.exe` bulunan yürütülebilir dosyasını `AADB2C.UserMigration\bin\Debug\net461`. Geçiş işlemini çalıştırmak için aşağıdaki komut satırı parametrelerini birini kullanın:

- İçin **parolayla kullanıcıları geçirme**, kullanın `UserMigration.exe 1` komutu.

- İçin **rastgele bir parola ile kullanıcıları geçirme**, kullanın `UserMigration.exe 2` komutu. Bu işlem, Azure tablo varlığına da oluşturur. Daha sonra REST API hizmetini çağırmak için bir ilke yapılandırın. Hizmet, geçiş işlemini yönetmek ve izlemek için bir Azure tablosu kullanır.

![Geçiş işleminin Tanıtımı](media/active-directory-b2c-user-migration/pre-migration-demo.png)

### <a name="step-24-check-the-pre-migration-process"></a>2.4. adım: Geçiş öncesi işlemleri denetleyin
Geçişi doğrulamak için aşağıdaki yöntemlerden birini kullanın:

- Bir kullanıcı için görünen ada göre aramak için Azure portalını kullanın:
   
   1. Açık **Azure AD B2C**ve ardından **kullanıcılar ve gruplar**.
   
   1. Arama kutusuna kullanıcının görünen adını yazın ve ardından kullanıcının profilini görüntüleyin.
   
- Bir kullanıcı oturum açma e-posta adresine göre almak için bu örnek uygulama kullanın:
   
   1. Şu komutu çalıştırın:
   
      ```Console
          UserMigration.exe 3 {email address}
      ```
      
      > [!TIP]
      > Aşağıdaki komutu kullanarak görünen ada göre kullanıcı alabilirsiniz: `UserMigration.exe 4 "<Display name>"`.
      
   1. UserProfile.json dosyayı kullanıcının bilgileri görmek için bir JSON düzenleyicisinde açın.
   
      ![UserProfile.json dosyası](media/active-directory-b2c-user-migration/pre-migration-get-by-email2.png)
      
### <a name="step-25-optional-environment-cleanup"></a>2.5. adım: (İsteğe bağlı) Ortam temizleme
Temizlemek istiyorsanız Azure AD kiracınıza yukarı ve çalıştırma Azure AD dizininden kaldırmasına `UserMigration.exe 5` komutu.

> [!NOTE]
> * Kiracı temizlemek için uygulamanız için kullanıcı hesabı yöneticisi izinlerini yapılandırın.
> * JSON dosyasında listelenen tüm kullanıcılar örnek geçiş uygulama temizler.

### <a name="step-26-sign-in-with-migrated-users-with-password"></a>2.6. adım: Geçirilen kullanıcı (parola) oturum açın
Geçiş öncesi işlemleri ile kullanıcı parolalarını çalıştırdıktan sonra hesabı kullanmaya hazır olursunuz ve kullanıcılar Azure AD B2C'yi kullanarak uygulamanızı oturum açabilir. Kullanıcı parolaları için erişiminiz yoksa, sonraki bölüme geçin.

## <a name="step-3-help-users-reset-their-password"></a>3. adım: Kullanıcının parolasını sıfırlamasını kullanıcıların yardımcı olma
Rastgele bir parola ile kullanıcıların geçiş işlemi gerçekleştirirseniz, bunlar parolalarını sıfırlamanız gerekir. Parola sıfırlama yardımcı olmak için bir parola sıfırlama bağlantısı ile Hoş Geldiniz e-posta gönderin.

Parola sıfırlama ilkenizi bağlantısını almak için aşağıdakileri yapın:

1. Seçin **Azure AD B2C ayarlarını**ve ardından **parolayı Sıfırla** İlkesi Özellikleri.

1. Uygulamanızı seçin.

    > [!NOTE]
    > Çalıştırma artık Kiracı'da önceden kayıtlı için en az bir uygulama gerektirir. Azure AD B2C uygulamaları kaydetme hakkında bilgi için bkz [başlama] [ B2C-GetStarted] makale veya [uygulama kaydı] [ B2C-AppRegister] makalesi.

1. Seçin **Şimdi Çalıştır**ve ardından ilkeyi olup olmadığını denetleyin.

1. İçinde **Şimdi Çalıştır uç noktası** kutusuna URL'yi kopyalayın ve ardından kullanıcılarınıza gönderin.

    ![Kümesi tanılama günlükleri](media/active-directory-b2c-user-migration/pre-migration-policy-uri.png)

## <a name="step-4-optional-change-your-policy-to-check-and-set-the-user-migration-status"></a>4. Adım: (İsteğe bağlı) İlkenizi denetleyin ve kullanıcı geçiş durumu ayarlamak için değiştirin

> [!NOTE]
> Denetleyin ve kullanıcı geçiş durumu değiştirmek için özel bir ilke kullanmanız gerekir. Kurulum yönergeleri [özel ilkeleri kullanmaya başlama] [ B2C-GetStartedCustom] tamamlanmış olması gerekir.
>

Parolayı sıfırlamadan önce oturum açmak kullanıcılara çalıştığınızda, ilkenizi kolay hata iletisi döndürmelidir. Örneğin:
>*Parolanızın süresi doldu. Sıfırlamak için parolayı Sıfırla bağlantısını seçin.*

Bu isteğe bağlı bir adım açıklandığı gibi özel ilkeler Azure AD B2C kullanımı gerektirir [özel ilkeleri kullanmaya başlama] [ B2C-GetStartedCustom] makalesi.

Bu bölümde, üzerinde oturum açma kullanıcı geçiş durumu denetlemek için ilke değiştirin. Kullanıcı parola değiştirilmediyse, seçmek için kullanıcıdan bir HTTP 409 hata iletisi döndürmek **parolanızı mı unuttunuz?** bağlantı.

Parola değişikliğini izlemek için bir Azure tablosu kullanın. Geçiş öncesi işlemleri komut satırı parametresi çalıştırdığınızda `2`, bir kullanıcı varlığı içinde bir Azure tablosu oluşturun. Hizmetinizi şunları yapar:

- Oturum açma, Azure AD B2C İlkesi geçişinizi e-posta iletisine bir giriş talep gönderme RESTful hizmeti çağırır. Hizmet, Azure tablosunda e-posta adresi arar. Adresi varsa, hizmet bir hata iletisi oluşturur: *Parola değiştirmeli*.

- Kullanıcı parolasını başarıyla değiştirdikten sonra Azure tablo varlık kaldırın.

>[!NOTE]
>Örneği basitleştirmek amacıyla bir Azure tablosu kullanıyoruz. Geçiş durumu, herhangi bir veritabanı veya Azure AD B2C hesabı içindeki özel bir özellik olarak depolayabilirsiniz.

### <a name="41-update-your-application-setting"></a>4.1: Uygulama ayarını güncelleştirme
1. RESTful API'si Tanıtımı test etmek için açık `AADB2C.UserMigration.sln` Visual Studio'da.

1. İçinde `AADB2C.UserMigration.API` projesini açarsanız *Web.config* dosya. Yapılandırılmış bir ayarı değiştirin [adım 2.2](#step-22-configure-the-application-settings):

    ```json
    {
        "BlobStorageConnectionString": "{The Azure Blob storage connection string}",
        ...
    }
    ```

### <a name="step-42-deploy-your-web-application-to-azure-app-service"></a>4.2. adım: Web uygulamanızı Azure App Service'e dağıtma
Çözüm Gezgini'nde sağ `AADB2C.UserMigration.API`, "Yayımla..." seçeneğini belirleyin. Azure App Service'e yayımlama için yönergeleri izleyin. Daha fazla bilgi için [uygulamanızı Azure App Service'e dağıtma][AppService-Deploy].

### <a name="step-43-add-a-technical-profile-and-technical-profile-validation-to-your-policy"></a>4.3. adım: İlkeniz için bir teknik profili ve teknik profili doğrulama ekleme
1. Çözüm Gezgini'nde "Çözüm öğeleri" genişletin ve açık *TrustFrameworkExtensions.xml* ilke dosyası.
1. Değişiklik `TenantId`, `PublicPolicyUri` ve `<TenantId>` alanlarını `yourtenant.onmicrosoft.com` kiracınızın adı.
1. Altında `<TechnicalProfile Id="login-NonInteractive">` öğesi, tüm örneklerinin yerine `ProxyIdentityExperienceFrameworkAppId` ve `IdentityExperienceFrameworkAppId` yapılandırılmış uygulama kimlikleri [özel ilkeleri kullanmaya başlama][B2C-GetStartedCustom].
1. Altında `<ClaimsProviders>` düğümü, aşağıdaki XML kod parçacığı bulunamadı. Değiştirin `ServiceUrl` Azure uygulama hizmeti URL'nizi yönlendirin.

    ```XML
    <ClaimsProvider>
      <DisplayName>REST APIs</DisplayName>
      <TechnicalProfiles>

        <TechnicalProfile Id="LocalAccountSignIn">
          <DisplayName>Local account just in time migration</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="ServiceUrl">http://{your-app}.azurewebsites.net/api/PrePasswordReset/LoalAccountSignIn</Item>
            <Item Key="AuthenticationType">None</Item>
            <Item Key="SendClaimsIn">Body</Item>
          </Metadata>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="signInName" PartnerClaimType="email" />
          </InputClaims>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>

        <TechnicalProfile Id="LocalAccountPasswordReset">
          <DisplayName>Local account just in time migration</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="ServiceUrl">http://{your-app}.azurewebsites.net/api/PrePasswordReset/PasswordUpdated</Item>
            <Item Key="AuthenticationType">None</Item>
            <Item Key="SendClaimsIn">Body</Item>
          </Metadata>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
          </InputClaims>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

Bir giriş talebi teknik profil tanımlar: `signInName` (e-posta olarak gönder). Oturum açma işleminde, RESTful uç noktanıza talep gönderilir.

RESTful API'niz için teknik profil tanımladıktan sonra teknik profil çağırmak için Azure AD B2C İlkesi söyleyin. XML kod parçacığı geçersiz kılmalar `SelfAsserted-LocalAccountSignin-Email`, temel ilkede tanımlanmıştır. XML kod parçacığı da ekler `ValidationTechnicalProfile`, teknik profilinizi işaret eden Referenceıd ile `LocalAccountUserMigration`.

### <a name="step-44-upload-the-policy-to-your-tenant"></a>4.4. adım: Kiracınız için ilkeyi karşıya yükle
1. İçinde [Azure portalında][Portal], geçiş [Azure AD B2C kiracınızın bağlamında][B2C-NavContext]ve ardından **Azure AD B2C**.

1. Seçin **kimlik deneyimi çerçevesi**.

1. Seçin **tüm ilkeleri**.

1. Seçin **karşıya yükleme İlkesi**.

1. Seçin **ilke varsa üzerine** onay kutusu.

1. Karşıya yükleme *TrustFrameworkExtensions.xml* dosya ve doğrulama başarılı olun.

### <a name="step-45-test-the-custom-policy-by-using-run-now"></a>4.5. adım: Şimdi Çalıştır'i kullanarak özel bir ilkeyi test
1. Seçin **Azure AD B2C ayarlarını**ve ardından **kimlik deneyimi çerçevesi**.

1. Açık **B2C_1A_signup_signin**, yüklenmiş ve ardından bağlı olan taraf (RP) özel ilke **Şimdi Çalıştır**.

1. Bir geçirilen kullanıcıların kimlik bilgileri ile oturum oturum açın ve ardından çalıştığınızda **oturum**. REST API'nizi aşağıdaki hata iletisini notsupportedexception oluşturmalıdır:

    ![Kümesi tanılama günlükleri](media/active-directory-b2c-user-migration/pre-migration-error-message.png)

### <a name="step-46-optional-troubleshoot-your-rest-api"></a>4.6. adım: (İsteğe bağlı) REST API'nizi sorunlarını giderme
Görüntüleyebileceğiniz ve günlük kaydı bilgilerini neredeyse gerçek zamanlı olarak izleyin.

1. RESTful uygulamanızın Ayarlar menüsündeki altında **izleme**seçin **tanılama günlükleri**.

1. Ayarlama **uygulama günlüğü (dosya sistemi)** için **üzerinde**.

1. Ayarlama **düzeyi** için **ayrıntılı**.

1. Seçin **Kaydet**

    ![Kümesi tanılama günlükleri](media/active-directory-b2c-user-migration/pre-migration-diagnostic-logs.png)

1. Üzerinde **ayarları** menüsünde **günlük akışı**.

1. RESTful API çıktısını denetleyin.

> [!IMPORTANT]
> Yalnızca geliştirme ve test sırasında tanılama günlükleri kullanın. RESTful API çıkış, üretimde sunulmamalıdır gizli bilgiler içerebilir.
>

## <a name="optional-download-the-complete-policy-files"></a>(İsteğe bağlı) Tüm ilke dosyalarını indirme
Tamamladıktan sonra [özel ilkeleri kullanmaya başlama] [ B2C-GetStartedCustom] izlenecek yol, öneririz senaryonuz kendi özel ilke dosyalarını kullanarak oluşturun. Referans olması açısından sağladık [örnek ilke dosyaları][UserMigrationSample].

[AD-PasswordPolicies]: https://docs.microsoft.com/azure/active-directory/active-directory-passwords-policy
[AD-Powershell]: https://docs.microsoft.com/powershell/azure/active-directory/install-adv2
[AppService-Deploy]: https://docs.microsoft.com/aspnet/core/tutorials/publish-to-azure-webapp-using-vs
[B2C-AppRegister]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration
[B2C-GetStarted]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-get-started
[B2C-GetStartedCustom]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-get-started-custom
[B2C-GraphQuickStart]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet
[B2C-NavContext]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-navigate-to-b2c-context
[Portal]: https://portal.azure.com/
[UserMigrationSample]: https://github.com/yoelhor/Azure-AD-B2C-UserMigration
