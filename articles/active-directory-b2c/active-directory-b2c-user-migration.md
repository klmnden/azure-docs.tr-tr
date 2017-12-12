---
title: "Azure Active Directory B2C: Kullanıcı geçiş yaklaşımlar"
description: "Temel ve Gelişmiş kavramları grafik API'sini kullanarak ve isteğe bağlı olarak Azure AD B2C özel ilkelerini kullanarak kullanıcı geçişi tartışın."
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 10/04/2017
ms.author: yoelh
ms.openlocfilehash: 25023359e3f1eeb241f6f0e70bcb179aa32974af
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-user-migration"></a>Azure Active Directory B2C: Kullanıcı Geçişi
Azure Active Directory B2C, kimlik sağlayıcısı geçirirken (Azure AD B2C) de gerekebilir kullanıcı hesabını geçirin. Bu makalede, var olan kullanıcı hesaplarını herhangi kimlik sağlayıcısından Azure AD B2C'ye geçirme açıklanmaktadır. Makaleyi Düzenleyici olmasını değildir ancak bunun yerine, iki çeşitli yaklaşımlar açıklar. Geliştirici, her iki yaklaşımın uygunluğuna sorumludur.

## <a name="user-migration-flows"></a>Kullanıcı Geçiş akışlar
Azure AD B2C ile kullanıcılara geçirebilirsiniz [grafik API'si](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet). Kullanıcı Geçiş işlemi iki akar döner:

* **Geçiş öncesi**: ya da bir kullanıcının kimlik bilgilerini (kullanıcı adı ve parola) Temizle erişiminiz olması veya kimlik bilgileri şifrelenir, ancak bunların şifrelerini çözmek için bu akış geçerlidir. Geçiş öncesi işlemleri, eski kimlik sağlayıcısı'ndan kullanıcıların okuma ve Azure AD B2C dizini içinde yeni hesaplar oluşturma içerir.

* **Geçiş öncesi ve parola sıfırlama**: bir kullanıcının parolasını erişilebilir olmadığında bu akış uygular. Örneğin:
    * Parola KARMA biçiminde depolanır.
    * Parola erişemediğiniz bir kimlik sağlayıcısı depolanır. Eski kimlik sağlayıcısı, bir web hizmeti çağrısı yaparak kullanıcı kimlik bilgilerini doğrular.

Her iki akışlar, ilk geçiş öncesi süreci çalıştırmak, kullanıcıların eski kimlik sağlayıcınızdan okuma ve yeni hesapları Azure AD B2C dizini oluşturun. Parola yoksa rastgele oluşturulan bir parola kullanarak hesabı oluşturun. Ardından parolayı değiştirmek için kullanıcıya sor ya da kullanıcı ilk kez oturum açtığında, Azure AD B2C sıfırlayın kullanıcıya sorar.

## <a name="password-policy"></a>Parola İlkesi
Azure AD B2C parola ilkesini (yerel hesaplar için) Azure AD ilkesini temel alır. Azure AD B2C kaydolma veya oturum açma ve parola ilkelerini kullanma "güçlü" parola gücünü sıfırlamak ve parolaları süresi sona ermiyor. Daha fazla bilgi için bkz: [Azure AD parola ilkesi](https://msdn.microsoft.com/library/azure/jj943764.aspx).

Geçirmek istediğiniz hesapları daha zayıf bir parola gücünü kullanıyorsanız [Azure AD B2C tarafından zorlanan güçlü parola gücünü](https://msdn.microsoft.com/library/azure/jj943764.aspx), güçlü parola gereksinimini devre dışı bırakabilirsiniz. Varsayılan Parola İlkesi değiştirmek için ayarlayın `passwordPolicies` özelliğine `DisableStrongPassword`. Örneğin, oluşturma Kullanıcı isteği aşağıdaki gibi değiştirebilirsiniz: 

```JSON
"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"
```

## <a name="step-1-use-graph-api-to-migrate-users"></a>Adım 1: Kullanıcıları geçirmek kullanım grafik API'si
Grafik API'si aracılığıyla Azure AD B2C kullanıcı hesabı (parola ile veya rastgele bir parola ile) oluşturun. Bu bölümde, grafik API'sini kullanarak Azure AD B2C dizini kullanıcı hesapları oluşturma işlemi açıklanmaktadır.

### <a name="step-11-register-your-application-in-your-tenant"></a>1.1. adım: uygulamanızı kiracınızda kaydetme
Grafik API'si ile iletişim kurmak için bir hizmet hesabı yönetici ayrıcalıklarına sahip olmalıdır. Azure AD içinde bir uygulama ve Azure ad kimlik doğrulama kaydedin. Uygulama kimlik bilgileri **uygulama kimliği** ve **uygulama gizli anahtarı**. Uygulama, grafik API'sini çağırmak için bir kullanıcı değil, olarak kendisini görür.

İlk olarak, geçiş uygulamanızı Azure AD'ye kaydedin. Ardından, bir uygulama anahtarı (uygulama gizli anahtarı) oluşturun ve uygulamayı yazma ayrıcalıklarına sahip ayarlayın.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. Azure AD seçin **B2C** hesabınızı üst seçerek Kiracı penceresinin sağ.

3. Sol bölmede seçin **Azure Active Directory** (Azure AD B2C değil). Bulmak için seçmeniz gerekebilir **daha Hizmetleri**.

4. Seçin **uygulama kayıtlar**.

5. Seçin **yeni uygulama kaydı**.

    ![Yeni uygulama kaydı](media/active-directory-b2c-user-migration/pre-migration-app-registration.png)

6. Aşağıdakileri yaparak yeni bir uygulama oluşturun:
    * İçin **adı**, kullanın **B2CUserMigration** veya istediğiniz diğer adı.
    * İçin **uygulama türü**, kullanın **Web app/API**.
    * İçin **oturum açma URL'si**, kullanın **https://localhost** (Bu uygulama için uygun değilse gibi).
    * **Oluştur**’u seçin.

7. Uygulama içinde oluşturulduktan sonra **uygulamaları** listesinde, yeni oluşturulan seçin **B2CUserMigration** uygulama.

8. Seçin **özellikleri**, kopya **uygulama kimliği**ve daha sonra kullanmak üzere kaydedin.

### <a name="step-12-create-the-application-secret"></a>1.2. adım: uygulama gizli anahtarı oluşturma
1. Azure portalında **kayıtlı uygulama** penceresinde, seçin **anahtarları**.

2. (İstemci parolası olarak da bilinir) yeni bir anahtar ekleyin ve daha sonra kullanmak için anahtarı kopyalayın.

    ![Uygulama kimliği ve anahtarları](media/active-directory-b2c-user-migration/pre-migration-app-id-and-key.png)

### <a name="step-13-grant-administrative-permission-to-your-application"></a>1.3. adım: uygulamanızı yönetici izni
1. Azure portalında **kayıtlı uygulama** penceresinde, seçin **gerekli izinleri**.

2. Seçin **Windows Azure Active Directory**.

3. İçinde **erişimi etkinleştir** bölmesi altında **uygulama izinleri**seçin **dizin verilerini okuma ve yazma**ve ardından **kaydetmek**.

4. İçinde **gerekli izinleri** bölmesinde, **izinler**.

    ![Uygulama izinleri](media/active-directory-b2c-user-migration/pre-migration-app-registration-permissions.png)

Şimdi oluşturmak, okumak ve Azure AD B2C kiracınızın kullanıcılardan güncelleştirmek için gereken izinlere sahip bir uygulamaya sahip.

### <a name="step-14-optional-environment-cleanup"></a>1.4. adım: (İsteğe bağlı) ortam temizleme
Okuma ve yazma dizin veri izinlerini yapmak *değil* kullanıcıların silip hakka sahiptir. Uygulamanız (ortamınızı temizlemek için) kullanıcıları silme olanağı vermek için kullanıcı hesabı yönetici izinleri ayarlamak için PowerShell çalıştırılmasını içerir fazladan bir adım gerçekleştirmeniz gerekir. Aksi halde, sonraki bölüme atlayabilirsiniz.

> [!IMPORTANT]
> Bir B2C Kiracı yönetici hesabı kullanmanız gerekir *yerel* B2C kiracısına. Hesap adı sözdizimi  *admin@contosob2c.onmicrosoft.com* .

>[!NOTE]
> Aşağıdaki PowerShell betiğini gerektirir [Azure Active Directory PowerShell sürüm 2](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0).

Bu PowerShell komut dosyasında, aşağıdakileri yapın:
1. Çevrimiçi hizmetiniz bağlayın. Bunu yapmak için çalıştırmanız `Connect-AzureAD` cmdlet Windows PowerShell komut isteminde ve kimlik bilgilerinizi sağlayın. 

2. Kullanım **uygulama kimliği** uygulama kullanıcı hesabı yönetici rolü atama. Tüm yapmanız gereken girin bu rolleri iyi bilinen tanımlayıcıları sahip, bu nedenle, **uygulama kimliği** betikteki.

```PowerShell
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

Değişiklik `$AppId` değeri Azure AD ile **uygulama kimliği**.

## <a name="step-2-pre-migration-application-sample"></a>2. adım: Geçiş öncesi uygulama örneği
[Karşıdan yükleme ve örnek kod çalıştırma](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-user-migration). Bir .zip dosyası olarak indirebilirsiniz.

### <a name="step-21-edit-the-migration-data-file"></a>2.1. adım: geçiş veri dosyasının Düzenle
Örnek uygulaması kukla kullanıcı verilerinizi içeren bir JSON dosyası kullanır. Örnek başarıyla çalışması sonra kendi veritabanındaki verileri kullanmak için kodu değiştirebilirsiniz. Ya da kullanıcı profili bir JSON dosyasına aktarın ve sonra bu dosyayı kullanmak üzere uygulamayı ayarlayın.

JSON dosyasının düzenlemek için açın `AADB2C.UserMigration.sln` Visual Studio çözümü. İçinde `AADB2C.UserMigration` Proje Aç `UsersData.json` dosya.

![Kullanıcı veri dosyası](media/active-directory-b2c-user-migration/pre-migration-data-file.png)

Gördüğünüz gibi dosya kullanıcı varlıkları listesini içerir. Her kullanıcı varlık aşağıdaki özelliklere sahiptir:
* e-posta
* Görünen adı
* FirstName
* Soyadı
* Parola (boş olabilir)

> [!NOTE]
> Derleme zamanında dosyayı Visual Studio kopyalar `bin` dizin.

### <a name="step-22-configure-the-application-settings"></a>2.2. adım: uygulama ayarlarını yapılandırma
Altında `AADB2C.UserMigration` proje, açık *App.config* dosya. Aşağıdaki uygulama ayarları kendi değerlerinizle değiştirin:

```XML
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{The ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{The Key from above}" />
    <add key="MigrationFile" value="{Name of a JSON file containing the users' data; for example, UsersData.json}" />
    <add key="BlobStorageConnectionString" value="{Your connection Azure table string}" />
    
</appSettings>
```

> [!NOTE]
> * Bir Azure tablo bağlantı dizesi kullanımını sonraki bölümlerde açıklanmıştır.
> * B2C Kiracı adı, Kiracı oluşturulduğu sırada girdiğiniz ve Azure portalında görüntülenen etki alanıdır. Kiracı adı genellikle sonekiyle sona erer *. onmicrosoft.com* (örneğin, *contosob2c.onmicrosoft.com*).
>

### <a name="step-23-run-the-pre-migration-process"></a>2.3. adım: geçiş öncesi işlemleri çalıştırma
Sağ `AADB2C.UserMigration` çözüm ve örnek yeniden oluşturma. Başarılı olursa, şimdi olmalıdır bir `UserMigration.exe` içinde bulunan yürütülebilir dosyasını `AADB2C.UserMigration\bin\Debug`. Geçiş işlemi çalıştırmak için aşağıdaki komut satırı parametreleri birini kullanın:

* İçin **parolayla kullanıcıların geçişini**, kullanın `UserMigration.exe 1` komutu.

* İçin **rasgele bir parola kullanıcılarla geçirmek**, kullanın `UserMigration.exe 2` komutu. Bu işlem ayrıca bir Azure tablo varlığına oluşturur. Daha sonra REST API hizmetini çağırmak için ilkeyi yapılandırın. Hizmeti bir Azure tablosu izlemek ve geçiş işlemini yönetmek için kullanır.

![Geçiş işleminin Tanıtımı](media/active-directory-b2c-user-migration/pre-migration-demo.png)

### <a name="step-24-check-the-pre-migration-process"></a>2.4. adım: geçiş öncesi süreci denetleyin
Geçişi doğrulamak için aşağıdaki yöntemlerden birini kullanın:

* Bir kullanıcı için görünen ada göre arama için Azure portalını kullanın:
    
    a. Açık **Azure AD B2C**ve ardından **kullanıcılar ve gruplar**.

    b. Arama kutusuna, kullanıcının görünen adını yazın ve ardından kullanıcının profilini görüntüleyin. 

* Bir kullanıcı oturum açma e-posta adresiyle almak için bu örnek uygulama kullanın:

    a. Şu komutu çalıştırın:

    ```Console
        UserMigration.exe 3 {email address}
    ```
        
    > [!TIP]
    > Çıkış JSON dosyası için aşağıdaki komutu kullanarak da kaydedebilirsiniz:
    >
    >```Console
    >    UserMigration.exe 3 {email address} >> UserProfile.json
    >```

    > [!TIP]
    > Aşağıdaki komutu kullanarak görünen ada göre kullanıcı alabilirsiniz: `UserMigration.exe 4 <Display name>`.

    b. Açık *UserProfile.json* JSON Düzenleyicisi'nde dosya. Visual Studio Code ile bir JSON belgesi seçerek veya Shift + Alt + F seçerek biçimlendirebilirsiniz **belgeyi Biçimlendir** bağlam menüsünde.

    ![UserProfile.json dosyası](media/active-directory-b2c-user-migration/pre-migration-get-by-email2.png)

### <a name="step-25-optional-environment-cleanup"></a>2.5. adım: (İsteğe bağlı) ortam temizleme
Temiz istiyorsanız yukarı Azure AD kiracınıza ve Çalıştır Azure AD dizininden kaldırmasına `UserMigration.exe 5` komutu.

> [!NOTE]
> * Kiracı temizlemek için uygulamanız için kullanıcı hesabı yönetici izinleri yapılandırın.
> * JSON dosyasında listelenen tüm kullanıcılar örnek geçiş uygulaması temizler.

### <a name="step-26-sign-in-with-migrated-users-with-password"></a>2.6. adım: geçirilen kullanıcılarla (parola) oturum açın
Geçiş öncesi süreci ile kullanıcı parolalarını çalıştırdıktan sonra hesapları kullanmak hazır olduğunu ve kullanıcıların Azure AD B2C kullanarak uygulamanıza oturum açabilirsiniz. Kullanıcı parolalarını erişiminiz yoksa, sonraki bölüme geçin.

## <a name="step-3-help-users-reset-their-password"></a>3. adım: kullanıcıların parolalarını sıfırlama Yardım
Rastgele bir parola kullanıcılarla geçirirseniz, parolalarını sıfırlamanız gerekir. Parola sıfırlama yardımcı olmak için parolayı sıfırlamak için bir bağlantı içeren bir Hoş Geldiniz e-posta gönderin.

Parola sıfırlama ilkesini bağlantısını almak için aşağıdakileri yapın:

1. Seçin **Azure AD B2C ayarlarını**ve ardından **parola sıfırlama** İlkesi Özellikleri.

2. Uygulamanızı seçin.

    >[!NOTE]
    >Çalışma şimdi Kiracı'preregistered için en az bir uygulama için gereklidir. Uygulamaları kaydetmek öğrenmek için Azure AD B2C bkz [başlama](active-directory-b2c-get-started.md) makale veya [uygulama kaydı](active-directory-b2c-app-registration.md) makale. 

3. Seçin **Şimdi Çalıştır**ve ardından ilkeyi denetleyin.

4. İçinde **artık uç nokta çalıştırmak** kutusuna URL'yi kopyalayın ve ardından, kullanıcılara gönderin.

    ![Set tanılama günlükleri](media/active-directory-b2c-user-migration/pre-migration-policy-uri.png)

## <a name="step-4-optional-change-your-policy-to-check-and-set-the-user-migration-status"></a>4. adım: (isteğe bağlı) değişiklik denetleyin ve kullanıcı geçiş durumu ayarlamak ilkeniz

> [!NOTE]
> Denetleyin ve kullanıcı geçiş durumu değiştirmek için özel bir ilke kullanmanız gerekir. Daha fazla bilgi için bkz: [özel ilkelerini kullanmaya başlama](active-directory-b2c-get-started-custom.md).
>

Kullanıcıların parola ilk sıfırlamadan oturum açmak çalıştığınızda ilkeniz kolay hata iletisi döndürmelidir. Örneğin: 
>*Parolanızın süresi doldu. Sıfırlamak için parola sıfırlama bağlantısını seçin.* 

Bu isteğe bağlı adım açıklandığı gibi Azure AD B2C özel ilkeler, kullanılmasını gerektiren [özel ilkeleri ile çalışmaya başlama](active-directory-b2c-get-started-custom.md) makalesi.

Bu bölümde, üzerinde oturum açma kullanıcı geçiş durumunu denetlemek için ilkeyi değiştirin. Kullanıcı parola değiştirilmediyse, seçmek için kullanıcıdan bir HTTP 409 hata iletisi döndürmek **parolanızı mı unuttunuz?** bağlantı. 

Parola değiştirme izlemek için bir Azure tablosu kullanın. Geçiş öncesi süreci ile komut satırı parametresi çalıştırdığınızda `2`, Azure bir tabloda bir kullanıcı varlığı oluşturun. Hizmetinizi şunları yapar:

* Oturum açma, Azure AD B2C ilke geçişinizi e-posta iletisine bir giriş talep gönderme RESTful hizmetini çağırır. Hizmet Azure tablosunda e-posta adresi arar. Adresi varsa, hizmet bir hata iletisi oluşturur: *parola değiştirmeli*.

* Kullanıcı parolasını başarıyla değiştirdikten sonra varlık Azure tablodan Kaldır.

>[!NOTE]
>Bir Azure tablosu örnek basitleştirmek için kullanırız. Geçiş durumu, herhangi bir veritabanı veya Azure AD B2C hesabı içindeki özel bir özellik olarak depolayabilirsiniz.

### <a name="41-update-your-application-setting"></a>4.1: uygulama ayarını güncelleştirin
1. RESTful API'si demo test etmek için açık `AADB2C.UserMigration.sln` Visual Studio çözümü Visual Studio'da. 

2. İçinde `AADB2C.UserMigration.API` proje, açık *App.config* dosya. Uygulama ayarı kendi değerle değiştirin:

    ```XML
    <appSettings>
        <add key="BlobStorageConnectionString" value="{The Azure Blob storage connection string"} />
    </appSettings>
    ```

### <a name="step-42-deploy-your-web-application-to-azure-app-service"></a>4.2. adım: web uygulamanızı Azure App Service'e dağıtma
Azure App Service API hizmetinizi yayımlayın. Daha fazla bilgi için bkz: [uygulamanızı Azure App Service'e dağıtma](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy).

### <a name="step-43-add-a-technical-profile-and-technical-profile-validation-to-your-policy"></a>4.3. adım: ilkeniz için bir teknik profili ve teknik profili doğrulama ekleme 
1. Çalışma dizininizi açın *TrustFrameworkExtensions.xml* uzantı ilke dosyası. 

2. Arama `<ClaimsProviders>` düğüm ve düğüm üzerinde aşağıdaki XML parçacığını ekleyin. Değerini değiştirdiğinizden emin olun `ServiceUrl` uç nokta URL'nizi yönlendirin. 

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
    
    <ClaimsProvider>
        <DisplayName>Local Account</DisplayName>
        <TechnicalProfiles>
    
        <!-- This technical profile uses a validation technical profile to authenticate the user. -->
        <TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
            <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="LocalAccountSignIn" />
            </ValidationTechnicalProfiles>
        </TechnicalProfile>
    
        <TechnicalProfile Id="LocalAccountWritePasswordUsingObjectId">
            <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="LocalAccountPasswordReset" />
            </ValidationTechnicalProfiles>
        </TechnicalProfile>
    
        </TechnicalProfiles>
    </ClaimsProvider>
    ```

Bir giriş talep önceki teknik profili tanımlar: `signInName` (e-posta gönder). Oturum açma, talep RESTful uç noktasına gönderilir.

RESTful API için teknik profili tanımladıktan sonra teknik profili çağırmak için Azure AD B2C ilkeniz söyleyin. XML parçacığını geçersiz kılmaları `SelfAsserted-LocalAccountSignin-Email`, temel İlkesi'nde tanımlanır. XML parçacığını de ekler `ValidationTechnicalProfile`, teknik profilinizi işaret eden Referenceıd ile `LocalAccountUserMigration`. 

### <a name="step-44-upload-the-policy-to-your-tenant"></a>4.4. adım: ilke kiracınız için karşıya yükleme
1. İçinde [Azure portal](https://portal.azure.com), geçiş [bağlamı Azure AD B2C kiracınızın](active-directory-b2c-navigate-to-b2c-context.md)ve ardından **Azure AD B2C**.

2. Seçin **kimlik deneyimi Framework**.

3. Seçin **tüm ilkeleri**.

4. Seçin **karşıya İlkesi**.

5. Seçin **varsa ilkesi üzerine** onay kutusu.

6. Karşıya yükleme *TrustFrameworkExtensions.xml* dosya ve doğrulama başarılı olduğundan emin olun.

### <a name="step-45-test-the-custom-policy-by-using-run-now"></a>4.5. adım: Test Şimdi Çalıştır kullanarak özel İlkesi
1. Seçin **Azure AD B2C ayarlarını**ve ardından **kimlik deneyimi Framework**.

2. Açık **B2C_1A_signup_signin**, karşıya ve ardından bağlı olan taraf (RP) özel ilkesini **Şimdi Çalıştır**.

3. Bir geçirilen kullanıcılar kimlik bilgileriyle oturum açın ve ardından çalıştığınızda **oturum**. REST API aşağıdaki hata iletisini oluşturması gerekir:

    ![Set tanılama günlükleri](media/active-directory-b2c-user-migration/pre-migration-error-message.png)

### <a name="step-46-optional-troubleshoot-your-rest-api"></a>4.6. adım: (İsteğe bağlı), REST API sorun giderme
Görüntüleyebileceğiniz ve günlük kaydı bilgilerini neredeyse gerçek zamanlı izleme.

1. RESTful uygulamanızın Ayarlar menüsündeki altında **izleme**seçin **tanılama günlükleri**. 

2. Ayarlama **uygulama günlüğü (dosya sistemi)** için **üzerinde**. 

3. Ayarlama **düzeyi** için **ayrıntılı**.

4. Seçin **Kaydet**

    ![Set tanılama günlükleri](media/active-directory-b2c-user-migration/pre-migration-diagnostic-logs.png)

5. Üzerinde **ayarları** menüsünde, select **günlük akışı**.

6. RESTful API'si çıktısını denetleyin.

Daha fazla bilgi için bkz: [akış günlükleri ve konsol](https://docs.microsoft.com/azure/app-service-web/web-sites-streaming-logs-and-console).

> [!IMPORTANT]
> Tanılama günlüklerini yalnızca geliştirme ve sınama sırasında kullanın. RESTful API'si çıkış üretimde gösterilmemesi gizli bilgiler içerebilir.
>

## <a name="optional-download-the-complete-policy-files"></a>(İsteğe bağlı) Tam ilke dosyaları indirme
Tamamlandıktan sonra [özel ilkelerini kullanmaya başlama](active-directory-b2c-get-started-custom.md) gözden geçirme, öneririz, kendi özel ilke dosyalarını kullanarak senaryonuz derleme. Başvuru için sağladık [örnek ilke dosyaları](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-user-migration). 
