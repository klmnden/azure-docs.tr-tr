---
title: Web API'leri (uygulama için bir belirteç alınırken) - Microsoft kimlik platformu çağıran masaüstü uygulaması
description: Web API'leri çağıran bir masaüstü uygulaması oluşturmayı öğrenin (uygulama için bir belirteç alınırken |)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e1fe9594471c6e8f723afff2def940bb675e04fb
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65407000"
---
# <a name="desktop-app-that-calls-web-apis---acquire-a-token"></a>Web API'leri - çağıran Masaüstü uygulamasının bir belirteç Al

Oluşturduktan sonra `IPublicClientApplication`, ardından bir web API'sini çağırmak için kullanacağınız bir belirteç almak için kullanacaksınız.

## <a name="recommended-pattern"></a>Önerilen Düzen

Web API'si tarafından tanımlanan kendi `scopes`. Uygulamanızda, sağladığınız ne olursa olsun deneyimi kullanmak istediğiniz modelidir:

- Çağırarak belirteç önbellekten bir belirteç almak üzere sistematik olarak çalışırken `AcquireTokenSilent`
- Bu çağrı başarısız olursa kullanın `AcquireToken` kullanmak istediğiniz akış (burada tarafından temsil edilen `AcquireTokenXX`)

```CSharp
AuthenticationResult result;
var accounts = await app.GetAccountsAsync();
IAccount account = ChooseAccount(accounts); // for instance accounts.FirstOrDefault
                                            // if the app manages is at most one account  
try
{
 result = await app.AcquireTokenSilent(scopes, account)
                   .ExecuteAsync();
}
catch(MsalUiRequiredException ex)
{
  result = await app.AcquireTokenXX(scopes, account)
                    .WithOptionalParameterXXX(parameter)
                    .ExecuteAsync();
}
```

İşte şimdi bir masaüstü uygulamasında belirteçlerini almak için çeşitli yollar ayrıntısı

## <a name="acquiring-a-token-interactively"></a>Etkileşimli bir belirteç alınırken

Aşağıdaki örnek, Microsoft Graph ile kullanıcının profilini okumak için etkileşimli bir belirteç almak için çok az kod gösterir.

```CSharp
string[] scopes = new string["user.read"];
var app = PublicClientApplicationBuilder.Create(clientId).Build();
var accounts = await app.GetAccountsAsync();
AuthenticationResult result;
try
{
 result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
             .ExecuteAsync();
}
catch(MsalUiRequiredException)
{
 result = await app.AcquireTokenInteractive(scopes)
             .ExecuteAsync();
}
```

### <a name="mandatory-parameters"></a>Zorunlu Parametreler

`AcquireTokenInteractive` yalnızca bir zorunlu parametresi ``scopes``, kendisi için bir belirteç gereklidir kapsamları tanımlamak dizeleri listesini içerir. Microsoft Graph için belirteci ise her Microsoft graph API, API Başvurusu gerekli kapsamları "İzinler" adlı bölümde bulunabilir. Örneğin, için [kullanıcının kişiler listesi](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/user_list_contacts), "User.Read", "Contacts.Read" kapsam kullanılması gerekir. Ayrıca bkz: [Microsoft Graph izinleri başvurusu](https://developer.microsoft.com/graph/docs/concepts/permissions_reference).

Android, üst etkinliği de belirtmeniz gerekir (kullanarak `.WithParentActivityOrWindow`, aşağıya bakın) belirteci bu üst etkinlik etkileşimi sonra geri alır. böylece. Belirtmezseniz, bir özel durum çağırırken oluşturulur `.ExecuteAsync()`.

### <a name="specific-optional-parameters"></a>Belirli bir isteğe bağlı parametreler

#### <a name="withparentactivityorwindow"></a>WithParentActivityOrWindow

Etkileşimli olan, kullanıcı Arabirimi önemlidir. `AcquireTokenInteractive` bir özel isteğe bağlı parametresi, onu destekleyen platformlar için belirtmek için etkinleştirme kullanıcı Arabirimi üst. Bir masaüstü uygulamasında kullanıldığında `.WithParentActivityOrWindow` platforma bağlı olarak farklı bir türe sahip:

```CSharp
// net45
WithParentActivityOrWindow(IntPtr windowPtr)
WithParentActivityOrWindow(IWin32Window window)

// Mac
WithParentActivityOrWindow(NSWindow window)

// .Net Standard (this will be on all platforms at runtime, but only on NetStandard at build time)
WithParentActivityOrWindow(object parent).
```

Notlar:

- Standart .NET, beklenen `object` olan bir `Activity` Android, bir `UIViewController` ios'ta bir `NSWindow` MAC üzerinde ve bir `IWin32Window` veya `IntPr` Windows üzerinde.
- Windows üzerinde çağırmalısınız `AcquireTokenInteractive` Arabiriminden iş parçacığı böylece katıştırılmış tarayıcı uygun UI eşitleme bağlamını alır.  UI iş parçacığından çağırma değil değil düzgün pompa ve/veya kullanıcı Arabirimi ile senaryoları kilitlenme iletileri neden olabilir. Kullanıcı Arabirimi iş parçacığında değilseniz, UI iş parçacığından MSAL çağırma biri zaten kullanmaktır `Dispatcher` WPF üzerinde.
- WPF, bir WPF denetiminde bir pencere elde etmek için kullanıyorsanız, kullanabileceğiniz `WindowInteropHelper.Handle` sınıfı. Ardından, bir WPF denetiminde çağrıdır (`this`):
  
  ```CSharp
  result = await app.AcquireTokenInteractive(scopes)
                    .WithParentActivityOrWindow(new WindowInteropHelper(this).Handle)
                    .ExecuteAsync();
  ```

#### <a name="withprompt"></a>WithPrompt

`WithPrompt()` bir komut istemi'ni belirterek kullanıcı etkileşimini denetlemek için kullanılır

<img src="https://user-images.githubusercontent.com/13203188/53438042-3fb85700-39ff-11e9-9a9e-1ff9874197b3.png" width="25%" />

Aşağıdaki sabitler sınıfı tanımlar:

- ``SelectAccount``: kullanıcının bir oturuma sahip hesaplarını içeren hesabı seçimi iletişim kutusunu göstermek için STS zorlar. Bu seçenek, uygulama geliştiricilerin Kullanıcı Seç farklı kimlikler arasında izin vermek istediğinizde yararlıdır. MSAL göndermek için bu seçeneği sürücüleri ``prompt=select_account`` kimlik sağlayıcı. Bu seçenek varsayılandır ve (hesap, oturum açmak için kullanıcı ve benzeri varlığını. kullanılabilir bilgilere dayanarak mümkün en iyi deneyimi sağlama çalışmaları iyi iş yok ...). Bunu yapmak için iyi bir neden olmadıkça değiştirmeyin.
- ``Consent``: Uygulama geliştirici zorlamak için önce onay verildikten bile onay istemleri. Bu durumda, MSAL gönderir `prompt=consent` kimlik sağlayıcı. Bu seçenek, kullanıcı uygulamayı her kullanılışında onay iletişim sunulan kuruluş idare burada gerektirdiği bazı güvenlik odaklı uygulamalarda kullanılabilir.
- ``ForceLogin``: Bu Kullanıcı istemi gerekli mıydı olsa bile, hizmet tarafından kimlik bilgileri istendiği kullanıcı uygulama geliştiricisinin sağlar. Bu seçenek, bir belirteç alınırken, yeniden-oturumu açma izin vermek için başarısız olursa yararlı olabilir. Bu durumda, MSAL gönderir `prompt=login` kimlik sağlayıcı. Burada kullanıcı relogs-bir uygulamanın belirli bölümleri erişim her zaman içinde kuruluş idare talepleri bazı güvenlik odaklı uygulamalarda kullanılır yeniden gördük.
- ``Never`` (.NET 4.5 ve yalnızca WinRT) kullanıcıdan olmaz, ancak bunun yerine gizli katıştırılmış web görünümü'nde depolanan bir tanımlama bilgisi kullanmayı dener (aşağıya bakın: Web görünümlerde MSAL.NET). Bu seçenek kullanılarak başarısız olabilir ve bu durumda `AcquireTokenInteractive` bir kullanıcı Arabirimi etkileşimi gerekmez ve başka kullanmanız gerekecektir bildirmek için bir özel durum oluşturur `Prompt` parametresi.
- ``NoPrompt``: Herhangi bir istemi kimlik sağlayıcısına göndermez. Bu seçenek yalnızca Azure AD B2C düzenleme profil ilkeleri için kullanışlı olur (bkz [B2C özellikleri](https://aka.ms/msal-net-b2c-specificities)).

#### <a name="withextrascopetoconsent"></a>WithExtraScopeToConsent

Bu değiştirici, kullanıcının önceden çeşitli kaynaklar için önceden onayını istemeniz istediğiniz gelişmiş bir senaryoda kullanılır (ve bu normalde MSAL.NET ile kullanılan artımlı onay kullanmak istemiyorsanız / Microsoft kimlik platformu v2.0). Ayrıntılar için bkz. [nasıl yapılır: çeşitli kaynaklar için Ön onay kullanıcının](scenario-desktop-production.md#how-to-have--the-user-consent-upfront-for-several-resources).

```CSharp
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

#### <a name="withcustomwebui"></a>WithCustomWebUi

##### <a name="withcustomwebui-is-an-extensibility-point"></a>WithCustomWebUi genişletilebilirlik noktasıdır

`WithCustomWebUi` Genel istemci uygulamaları, kendi Arabiriminde verdiğiniz izin veren bir genişletilebilirlik noktasıdır ve Git izin verir ve kimlik sağlayıcısı/Authorize son noktası izin vermek için oturum açın ve onay. MSAL.NET, daha sonra kimlik doğrulama kodu ve bir belirteç almak. Bu örneği için Visual Studio'da uygulamaları (örneğin VS geri bildirim), web etkileşimi sağlar ancak çoğunu yapmak için MSAL.NET için bırakın bulmacaları sağlamak için kullanılır. UI Otomasyonu sağlamak istiyorsanız, da kullanabilirsiniz. Genel istemci uygulamalar, MSAL.NET PKCE standart kullanır ([RFC 7636 - OAuth genel istemciler tarafından kod Exchange kavram anahtarı](https://tools.ietf.org/html/rfc7636)) güvenlik uyulduğundan emin olmak için: Yalnızca MSAL.NET kod kullanmak.

  ```CSharp
  using Microsoft.Identity.Client.Extensions;
  ```

##### <a name="how-to-use-withcustomwebui"></a>WithCustomWebUi kullanma

Kullanmak için `.WithCustomWebUI`, gerekir:
  
  1. Uygulama `ICustomWebUi` arabirimi (bkz [burada](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/blob/053a98d16596be7e9ca1ab916924e5736e341fe8/src/Microsoft.Identity.Client/Extensibility/ICustomWebUI.cs#L32-L70). Temel bir yöntemi uygulaması gerekir `AcquireAuthorizationCodeAsync` (MSAL.NET tarafından hesaplanan) yetkilendirme kodu URL'si kabul, kullanıcı kimlik sağlayıcısı ile etkileşime geçer ve ardından döndüren geri kimlik sağlayıcısı tarafından misiniz URL'si Geri (dahil olmak üzere yetkilendirme kodu), uygulamanızın çağrıldı. Sorunları varsa, uygulamanız oluşturmamalıdır bir `MsalExtensionException` MSAL ile sorunsuz şekilde işbirliği için özel durum.
  2. İçinde `AcquireTokenInteractive` kullanabileceğiniz çağrısı `.WithCustomUI()` özel örneğini geçirerek değiştiricisi web kullanıcı Arabirimi

     ```CSharp
     result = await app.AcquireTokenInteractive(scopes)
                       .WithCustomWebUi(yourCustomWebUI)
                       .ExecuteAsync();
     ```

##### <a name="examples-of-implementation-of-icustomwebui-in-test-automation---seleniumwebui"></a>Test Otomasyonu - SeleniumWebUI ICustomWebUi uygulamasında örnekleri

MSAL.NET takım bu genişletilebilirlik mekanizması yararlanmak için yaptığımız kullanıcı Arabirimi testleri yeniden. İlginizi çeken durumunda göz olabilir [SeleniumWebUI](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/blob/053a98d16596be7e9ca1ab916924e5736e341fe8/tests/Microsoft.Identity.Test.Integration/Infrastructure/SeleniumWebUI.cs#L15-L160) MSAL.NET kaynak kodunda sınıfı

#### <a name="other-optional-parameters"></a>Diğer isteğe bağlı parametreler

Tüm diğer isteğe bağlı parametreler için hakkında daha fazla bilgi `AcquireTokenInteractive` için başvuru belgelerindeki [AcquireTokenInteractiveParameterBuilder](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.apiconfig.acquiretokeninteractiveparameterbuilder?view=azure-dotnet-preview#methods)

## <a name="integrated-windows-authentication"></a>Tümleşik Windows kimlik doğrulaması

Bir etki alanı kullanıcısı oturum açmak istiyorsanız makine bir etki alanına veya Azure AD'ye katılmış, kullanmanız gerekir:

```csharp
AcquireTokenByIntegratedWindowsAuth(IEnumerable<string> scopes)
```

### <a name="constraints"></a>Kısıtlamalar

- AcquireTokenByIntegratedWindowsAuth (IWA) için kullanılabilir, yalnızca **federe** yalnızca olan kullanıcılar, bir Active Directory içinde oluşturulan ve Azure Active Directory tarafından desteklenen kullanıcılar. AAD, AD yedekleme - olmadan doğrudan oluşturulan kullanıcılar **yönetilen** - kullanıcılar, bu kimlik doğrulama akışı kullanamazsınız. Bu sınırlama, kullanıcı adı/parola akışı etkilemez.
- .NET Framework, .NET Core ve UWP platformları için yazılmış uygulamalar IWA içindir
- IWA MFA (çok faktörlü kimlik doğrulamasını) atlamaz. MFA yapılandırıldıysa, MFA testini gerekliyse, MFA kullanıcı etkileşimi gerektirdiğinden IWA başarısız olabilir.
  > [!NOTE]
  > Bu zor olur. IWA etkileşimli olmayan, ancak 2FA kullanıcı etkileşimini gerektirir. Kimlik sağlayıcısı gerçekleştirilecek 2FA istediğinde, kontrol etmez, Kiracı Yöneticisi yapar. Bizim gözlemler farklı bir ülkeden bağlı olmayan VPN aracılığıyla kurumsal ağ ve VPN bağlandığında bazen bile oturum 2FA gereklidir. Belirleyici bir kural kümesi beklemiyoruz, sürekli olarak 2fa'yı gerekli olup olmadığını öğrenmek için yapay ZEKA Azure Active Directory kullanır. IWA başarısız olursa geri dönüş için bir kullanıcı istemi (etkileşimli kimlik doğrulaması veya cihaz kod akışını) gerekir.

- Geçirilen yetkilisi `PublicClientApplicationBuilder` olması gerekir:
  - Kiracı ed (form `https://login.microsoftonline.com/{tenant}/` burada `tenant` Kiracı kimliği veya Kiracı ile ilişkilendirilen bir etki alanını temsil eden GUID.
  - için herhangi bir iş ve Okul hesapları (`https://login.microsoftonline.com/organizations/`)

  > Kişisel Microsoft hesapları desteklenmez (/ Common veya /consumers kiracılar kullanamazsınız)

- Tümleşik Windows kimlik doğrulaması sessiz bir akış olduğundan:
  - Uygulamanızın kullanıcı daha önce uygulamayı kullanmak için onaylı gerekir
  - veya Kiracı Yöneticisi daha önce uygulamayı kullanmak için kiracıdaki tüm kullanıcılar gerekir.
  - Diğer bir deyişle:
    - bir geliştirici olarak ya da basılı **Grant** kendiniz için Azure portalında düğmesi
    - ya da bir kiracı Yöneticisi bastığını **{Kiracı etki alanı} için yönetici onayı vermek/iptal etmek** düğmesine **API izinleri** kaydı uygulama için sekmesinde (bkz [izinleri ekleyin erişim web API'leri](https://docs.microsoft.com/azure/active-directory/develop/quickstart-configure-app-access-web-apis#add-permissions-to-access-web-apis))
    - veya uygulama onayı bir yol sağladığınız (bkz [tek tek kullanıcı onayı isteyen](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#requesting-individual-user-consent))
    - veya uygulama için onayı Kiracı Yöneticisi için bir yol sağladığınız (bkz [yönetici onayı](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#requesting-consent-for-an-entire-tenant))

- Bu akış, .net Masaüstü, .net core ve Windows Evrensel (UWP) uygulamaları için etkinleştirilir. .NET Core platform işletim sistemi için kullanıcı adı sorduğunuzda .NET core, yalnızca kullanıcı adı alan aşırı yükleme, kullanılabilir.
  
Onay hakkında daha fazla bilgi için bkz. [v2.0 izinler ve onay](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent)

### <a name="how-to-use-it"></a>Kullanımı

Yalnızca bir parametresi normalde gerekir (`scopes`). Ancak bağlı olarak Windows yöneticinize Kurulum ilkeleri vardır. yol, windows makinenizde uygulamaları oturum açmış kullanıcı aramak için izin verilmeyen mümkün olabilir. Bu durumda, ikinci bir yöntem kullanmak `.WithUsername()` ve kullanıcı adı UPN biçiminde - oturum açmış olan kullanıcının geçirin `joe@contoso.com`.

Aşağıdaki örnek en güncel durum türünü alabilirsiniz özel durumları ve bunların risk azaltma işlemleri açıklamalarını ile sunar.

```CSharp
static async Task GetATokenForGraph()
{
 string authority = "https://login.microsoftonline.com/contoso.com";
 string[] scopes = new string[] { "user.read" };
 IPublicClientApplication app = PublicClientApplicationBuilder
      .Create(clientId)
      .WithAuthority(authority)
      .Build();

 var accounts = await app.GetAccountsAsync();

 AuthenticationResult result = null;
 if (accounts.Any())
 {
  result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
      .ExecuteAsync();
 }
 else
 {
  try
  {
   result = await app.AcquireTokenByIntegratedWindowsAuth(scopes)
      .ExecuteAsync(CancellationToken.None);
  }
  catch (MsalUiRequiredException ex)
  {
   // MsalUiRequiredException: AADSTS65001: The user or administrator has not consented to use the application
   // with ID '{appId}' named '{appName}'.Send an interactive authorization request for this user and resource.

   // you need to get user consent first. This can be done, if you are not using .NET Core (which does not have any Web UI)
   // by doing (once only) an AcquireToken interactive.

   // If you are using .NET core or don't want to do an AcquireTokenInteractive, you might want to suggest the user to navigate
   // to a URL to consent: https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id={clientId}&response_type=code&scope=user.read

   // AADSTS50079: The user is required to use multi-factor authentication.
   // There is no mitigation - if MFA is configured for your tenant and AAD decides to enforce it,
   // you need to fallback to an interactive flows such as AcquireTokenInteractive or AcquireTokenByDeviceCode
   }
   catch (MsalServiceException ex)
   {
    // Kind of errors you could have (in ex.Message)

    // MsalServiceException: AADSTS90010: The grant type is not supported over the /common or /consumers endpoints. Please use the /organizations or tenant-specific endpoint.
    // you used common.
    // Mitigation: as explained in the message from Azure AD, the authority needs to be tenanted or otherwise organizations

    // MsalServiceException: AADSTS70002: The request body must contain the following parameter: 'client_secret or client_assertion'.
    // Explanation: this can happen if your application was not registered as a public client application in Azure AD
    // Mitigation: in the Azure portal, edit the manifest for your application and set the `allowPublicClient` to `true`
   }
   catch (MsalClientException ex)
   {
      // Error Code: unknown_user Message: Could not identify logged in user
      // Explanation: the library was unable to query the current Windows logged-in user or this user is not AD or AAD
      // joined (work-place joined users are not supported).

      // Mitigation 1: on UWP, check that the application has the following capabilities: Enterprise Authentication,
      // Private Networks (Client and Server), User Account Information

      // Mitigation 2: Implement your own logic to fetch the username (e.g. john@contoso.com) and use the
      // AcquireTokenByIntegratedWindowsAuth form that takes in the username

      // Error Code: integrated_windows_auth_not_supported_managed_user
      // Explanation: This method relies on an a protocol exposed by Active Directory (AD). If a user was created in Azure
      // Active Directory without AD backing ("managed" user), this method will fail. Users created in AD and backed by
      // AAD ("federated" users) can benefit from this non-interactive method of authentication.
      // Mitigation: Use interactive authentication
   }
 }


 Console.WriteLine(result.Account.Username);
}
```

Olası değiştiricilerini AcquireTokenByIntegratedWindowsAuthentication listesi için bkz. [AcquireTokenByIntegratedWindowsAuthParameterBuilder](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.apiconfig.acquiretokenbyintegratedwindowsauthparameterbuilder?view=azure-dotnet-preview#methods)

## <a name="username--password"></a>Kullanıcı adı / parola

Ayrıca kullanıcı adı ve parolasını sağlayarak bir belirteç elde edebilirsiniz. Bu akış sınırlı ve önerilmez, ancak yine de gerekli olduğu durumlarda kullanmak vardır.

### <a name="this-flow-isnt-recommended"></a>Bu akış önerilmez

Bu akış **önerilmez** çünkü bir kullanıcı kendi parolasını isteyen uygulamanızı güvenli değildir. Bu sorun hakkında daha fazla bilgi için bkz. [bu makalede](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/). Windows etki alanına katılmış makinelerde sessiz bir belirteç almak için tercih edilen akışı [tümleşik Windows kimlik doğrulaması](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Integrated-Windows-Authentication). Aksi takdirde kullanabilirsiniz [cihaz kod akışı](https://aka.ms/msal-net-device-code-flow)

> Kullanıcı Arabirimi, onw sağlarsınız etkileşimli senaryolar kullanıcı adı/parola kullanmak istiyorsanız bu bazı durumlarda (DevOps senaryolarını) kullanışlı olsa da, bunu uzağa taşıma hakkında gerçekten düşünmelisiniz. Kullanıcı adı/parola kullanarak, veren birkaç Yukarı:

> - modern kimlik kiracılar Çekirdek: parola fished, yeniden yürütülmesi. Çünkü bu kavramı, geçirilebilir bir paylaşım gizli dizi sahip olabiliyoruz.
> Bu, parolasız ile uyumlu değil.
> - MFA yapmak için gereken kullanıcılar etkileşimi (olduğu gibi) oturum açmanız mümkün olmayacaktır.
> - Kullanıcılara çoklu oturum açmayı mümkün olmayacaktır.

### <a name="constraints"></a>Kısıtlamalar

Aşağıdaki kısıtlamalar da geçerlidir:

- Kullanıcı adı/parola akış koşullu erişim ve multi-Factor authentication ile uyumlu değil: Sonuç olarak, uygulamanız nerede Kiracı Yöneticisi çok faktörlü kimlik doğrulaması gerektiren bir Azure AD kiracısında çalışıyorsa, bu akışı kullanamazsınız. Birçok kuruluş bu gerçekleştirin.
- Yalnızca iş için çalışır ve Okul hesapları (MSA değil)
- Flow, .net Masaüstü ve .net core, ancak çalıştırılmadı UWP üzerinde kullanılabilir

### <a name="b2c-specifics"></a>B2C özellikleri

[B2C ile ROPC kullanma hakkında daha fazla bilgi](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics#resource-owner-password-credentials-ropc-with-b2c).

### <a name="how-to-use-it"></a>Bunu nasıl kullanılır?

`IPublicClientApplication`contains yöntemi `AcquireTokenByUsernamePassword`

Aşağıdaki örnek basitleştirilmiş bir servis talebi sayısını gösterir.

```CSharp
static async Task GetATokenForGraph()
{
 string authority = "https://login.microsoftonline.com/contoso.com";
 string[] scopes = new string[] { "user.read" };
 IPublicClientApplication app;
 app = PublicClientApplicationBuild.Create(clientId)
       .WithAuthority(authority)
       .Build();
 var accounts = await app.GetAccountsAsync();

 AuthenticationResult result = null;
 if (accounts.Any())
 {
  result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
                    .ExecuteAsync();
 }
 else
 {
  try
  {
   var securePassword = new SecureString();
   foreach (char c in "dummy")        // you should fetch the password
    securePassword.AppendChar(c);  // keystroke by keystroke

   result = await app.AcquireTokenByUsernamePassword(scopes,
                                                    "joe@contoso.com",
                                                     securePassword)
                      .ExecuteAsync();
  }
  catch(MsalException)
  {
   // See details below
  }
 }
 Console.WriteLine(result.Account.Username);
}
```

Aşağıdaki örnek en güncel durum türünü alabilirsiniz özel durumları ve bunların risk azaltma işlemleri açıklamalarını ile sunar.

```CSharp
static async Task GetATokenForGraph()
{
 string authority = "https://login.microsoftonline.com/contoso.com";
 string[] scopes = new string[] { "user.read" };
 IPublicClientApplication app;
 app = PublicClientApplicationBuild.Create(clientId)
                                   .WithAuthority(authority)
                                   .Build();
 var accounts = await app.GetAccountsAsync();

 AuthenticationResult result = null;
 if (accounts.Any())
 {
  result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
                    .ExecuteAync();
 }
 else
 {
  try
  {
   var securePassword = new SecureString();
   foreach (char c in "dummy")        // you should fetch the password keystroke
    securePassword.AppendChar(c);  // by keystroke

   result = await app.AcquireTokenByUsernamePassword(scopes,
                                                    "joe@contoso.com",
                                                    securePassword)
                    .ExecuteAsync();
  }
  catch (MsalUiRequiredException ex) when (ex.Message.Contains("AADSTS65001"))
  {
   // Here are the kind of error messages you could have, and possible mitigations

   // ------------------------------------------------------------------------
   // MsalUiRequiredException: AADSTS65001: The user or administrator has not consented to use the application
   // with ID '{appId}' named '{appName}'. Send an interactive authorization request for this user and resource.

   // Mitigation: you need to get user consent first. This can be done either statically (through the portal),
   /// or dynamically (but this requires an interaction with Azure AD, which is not possible with
   // the username/password flow)
   // Statically: in the portal by doing the following in the "API permissions" tab of the application registration:
   // 1. Click "Add a permission" and add all the delegated permissions corresponding to the scopes you want (for instance
   // User.Read and User.ReadBasic.All)
   // 2. Click "Grant/revoke admin consent for <tenant>") and click "yes".
   // Dynamically, if you are not using .NET Core (which does not have any Web UI) by
   // calling (once only) AcquireTokenInteractive.
   // remember that Username/password is for public client applications that is desktop/mobile applications.
   // If you are using .NET core or don't want to call AcquireTokenInteractive, you might want to:
   // - use device code flow (See https://aka.ms/msal-net-device-code-flow)
   // - or suggest the user to navigate to a URL to consent: https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id={clientId}&response_type=code&scope=user.read
   // ------------------------------------------------------------------------

   // ------------------------------------------------------------------------
   // ErrorCode: invalid_grant
   // SubError: basic_action
   // MsalUiRequiredException: AADSTS50079: The user is required to use multi-factor authentication.
   // The tenant admin for your organization has chosen to oblige users to perform multi-factor authentication.
   // Mitigation: none for this flow
   // Your application cannot use the Username/Password grant.
   // Like in the previous case, you might want to use an interactive flow (AcquireTokenInteractive()),
   // or Device Code Flow instead.
   // Note this is one of the reason why using username/password is not recommended;
   // ------------------------------------------------------------------------

   // ------------------------------------------------------------------------
   // ex.ErrorCode: invalid_grant
   // subError: null
   // Message = "AADSTS70002: Error validating credentials.
   // AADSTS50126: Invalid username or password
   // In the case of a managed user (user from an Azure AD tenant opposed to a
   // federated user, which would be owned
   // in another IdP through ADFS), the user has entered the wrong password
   // Mitigation: ask the user to re-enter the password
   // ------------------------------------------------------------------------

   // ------------------------------------------------------------------------
   // ex.ErrorCode: invalid_grant
   // subError: null
   // MsalServiceException: ADSTS50034: To sign into this application the account must be added to
   // the {domainName} directory.
   // or The user account does not exist in the {domainName} directory. To sign into this application,
   // the account must be added to the directory.
   // The user was not found in the directory
   // Explanation: wrong username
   // Mitigation: ask the user to re-enter the username.
   // ------------------------------------------------------------------------
  }
  catch (MsalServiceException ex) when (ex.ErrorCode == "invalid_request")
  {
   // ------------------------------------------------------------------------
   // AADSTS90010: The grant type is not supported over the /common or /consumers endpoints.
   // Please use the /organizations or tenant-specific endpoint.
   // you used common.
   // Mitigation: as explained in the message from Azure AD, the authority you use in the application needs
   // to be tenanted or otherwise "organizations". change the
   // "Tenant": property in the appsettings.json to be a GUID (tenant Id), or domain name (contoso.com)
   // if such a domain is registered with your tenant
   // or "organizations", if you want this application to sign-in users in any Work and School accounts.
   // ------------------------------------------------------------------------

  }
  catch (MsalServiceException ex) when (ex.ErrorCode == "unauthorized_client")
  {
   // ------------------------------------------------------------------------
   // AADSTS700016: Application with identifier '{clientId}' was not found in the directory '{domain}'.
   // This can happen if the application has not been installed by the administrator of the tenant or consented
   // to by any user in the tenant.
   // You may have sent your authentication request to the wrong tenant
   // Cause: The clientId in the appsettings.json might be wrong
   // Mitigation: check the clientId and the app registration
   // ------------------------------------------------------------------------
  }
  catch (MsalServiceException ex) when (ex.ErrorCode == "invalid_client")
  {
   // ------------------------------------------------------------------------
   // AADSTS70002: The request body must contain the following parameter: 'client_secret or client_assertion'.
   // Explanation: this can happen if your application was not registered as a public client application in Azure AD
   // Mitigation: in the Azure portal, edit the manifest for your application and set the `allowPublicClient` to `true`
   // ------------------------------------------------------------------------
  }
  catch (MsalServiceException)
  {
   throw;
  }

  catch (MsalClientException ex) when (ex.ErrorCode == "unknown_user_type")
  {
   // Message = "Unsupported User Type 'Unknown'. Please see https://aka.ms/msal-net-up"
   // The user is not recognized as a managed user, or a federated user. Azure AD was not
   // able to identify the IdP that needs to process the user
   throw new ArgumentException("U/P: Wrong username", ex);
  }
  catch (MsalClientException ex) when (ex.ErrorCode == "user_realm_discovery_failed")
  {
   // The user is not recognized as a managed user, or a federated user. Azure AD was not
   // able to identify the IdP that needs to process the user. That's for instance the case
   // if you use a phone number
   throw new ArgumentException("U/P: Wrong username", ex);
  }
  catch (MsalClientException ex) when (ex.ErrorCode == "unknown_user")
  {
   // the username was probably empty
   // ex.Message = "Could not identify the user logged into the OS. See https://aka.ms/msal-net-iwa for details."
   throw new ArgumentException("U/P: Wrong username", ex);
  }
  catch (MsalClientException ex) when (ex.ErrorCode == "parsing_wstrust_response_failed")
  {
   // ------------------------------------------------------------------------
   // In the case of a Federated user (that is owned by a federated IdP, as opposed to a managed user owned in an Azure AD tenant)
   // ID3242: The security token could not be authenticated or authorized.
   // The user does not exist or has entered the wrong password
   // ------------------------------------------------------------------------
  }
 }

 Console.WriteLine(result.Account.Username);
}
```

Uygulanabilir tüm değiştiricilere hakkında ayrıntılı bilgi için `AcquireTokenByUsernamePassword`, bkz: [AcquireTokenByUsernamePasswordParameterBuilder](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.apiconfig.acquiretokenbyusernamepasswordparameterbuilder?view=azure-dotnet-preview#methods)

## <a name="command-line-tool-without-web-browser"></a>Komut satırı aracı (olmadan web tarayıcısı gerekir)

### <a name="device-code-flow-why-and-how"></a>Cihaz kod akış neden? ve nasıl?

(Yani Web denetimleri yok) bir komut satırı aracı yazıyorsanız ve ya da önceki akışları kullanmak istemiyorsanız kullanmanız gerekecektir `AcquireTokenWithDeviceCode`.

Azure AD ile etkileşimli kimlik doğrulaması gerektiren bir web tarayıcısı (Ayrıntılar için bkz. [web tarayıcıları kullanımını](https://aka.ms/msal-net-uses-web-browser)). Ancak, cihaz veya işletim sistemleri, bir Web tarayıcısı sağlamayan kullanıcıların kimliklerini doğrulamak için cihaz kod akışını (örneğin başka bir bilgisayara veya bir cep telefonu) oturum başka bir cihaz etkileşimli olarak olanak. Cihaz kod akışı kullanarak, uygulama belirteçleri bu cihazları/OS için özellikle tasarlanmış iki adımlı bir işlemle alır. Bu uygulamalara örnek olarak, IOT veya komut satırı araçlarını (CLI) çalışan uygulamalardır. Fikir olmasıdır:

1. Kullanıcı kimlik doğrulaması gerekli olduğunda, uygulama bir kod sağlar ve kullanıcıdan bir URL'ye gidin (örneğin, bir İnternet'e bağlı smartphone) başka bir cihaz kullanın (örneğin, `https://microsoft.com/devicelogin`), burada kullanıcı kodunu girmeniz istenir. Bitti, web sayfası kullanıcı onayı istemlerini ve çok faktörlü kimlik doğrulaması gerekirse dahil olmak üzere bir normal kimlik doğrulama deneyimi aracılığıyla önünü açacak olduğunu.

2. Başarılı kimlik doğrulamadan sonra komut satırı uygulaması ile arka kanal gerekli belirteçleri alır ve ihtiyaç duyduğu web API çağrıları gerçekleştirmek için kullanın.

### <a name="code"></a>Kod

`IPublicClientApplication`adlı bir yöntem içerir `AcquireTokenWithDeviceCode`

```CSharp
 AcquireTokenWithDeviceCode(IEnumerable<string> scopes,
                            Func<DeviceCodeResult, Task> deviceCodeResultCallback)
```

Bu yöntem, parametre alır:

- `scopes` İçin bir erişim belirteci istemek için
- Alacak bir geri çağırma `DeviceCodeResult`

  ![image](https://user-images.githubusercontent.com/13203188/56024968-7af1b980-5d11-11e9-84c2-5be2ef306dc5.png)

Aşağıdaki örnek kod türünü alabilirsiniz özel durumları ve bunların risk azaltma açıklamalarını ile güncel durumda, sunar.

```CSharp
static async Task<AuthenticationResult> GetATokenForGraph()
{
 string authority = "https://login.microsoftonline.com/contoso.com";
 string[] scopes = new string[] { "user.read" };
 IPublicClientApplication pca = PublicClientApplicationBuilder
      .Create(clientId)
      .WithAuthority(authority)
      .Build();

 AuthenticationResult result = null;
 var accounts = await app.GetAccountsAsync();

 // All AcquireToken* methods store the tokens in the cache, so check the cache first
 try
 {
  result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
       .ExecuteAsync();
 }
 catch (MsalUiRequiredException ex)
 {
  // A MsalUiRequiredException happened on AcquireTokenSilent.
  // This indicates you need to call AcquireTokenInteractive to acquire a token
  System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");
 }

 try
 {
  result = await app.AcquireTokenWithDeviceCode(scopes,
      deviceCodeCallback =>
  {
       // This will print the message on the console which tells the user where to go sign-in using
       // a separate browser and the code to enter once they sign in.
       // The AcquireTokenWithDeviceCode() method will poll the server after firing this
       // device code callback to look for the successful login of the user via that browser.
       // This background polling (whose interval and timeout data is also provided as fields in the
       // deviceCodeCallback class) will occur until:
       // * The user has successfully logged in via browser and entered the proper code
       // * The timeout specified by the server for the lifetime of this code (typically ~15 minutes) has been reached
       // * The developing application calls the Cancel() method on a CancellationToken sent into the method.
       //   If this occurs, an OperationCanceledException will be thrown (see catch below for more details).
       Console.WriteLine(deviceCodeResult.Message);
       return Task.FromResult(0);
  }).ExecuteAsync();

  Console.WriteLine(result.Account.Username);
  return result;
 }
 catch (MsalServiceException ex)
 {
  // Kind of errors you could have (in ex.Message)

  // AADSTS50059: No tenant-identifying information found in either the request or implied by any provided credentials.
  // Mitigation: as explained in the message from Azure AD, the authoriy needs to be tenanted. you have probably created
  // your public client application with the following authorities:
  // https://login.microsoftonline.com/common or https://login.microsoftonline.com/organizations

  // AADSTS90133: Device Code flow is not supported under /common or /consumers endpoint.
  // Mitigation: as explained in the message from Azure AD, the authority needs to be tenanted

  // AADSTS90002: Tenant <tenantId or domain you used in the authority> not found. This may happen if there are
  // no active subscriptions for the tenant. Check with your subscription administrator.
  // Mitigation: if you have an active subscription for the tenant this might be that you have a typo in the
  // tenantId (GUID) or tenant domain name.
 }
 catch (OperationCanceledException ex)
 {
  // If you use a CancellationToken, and call the Cancel() method on it, then this may be triggered
  // to indicate that the operation was cancelled.
  // See https://docs.microsoft.com/dotnet/standard/threading/cancellation-in-managed-threads
  // for more detailed information on how C# supports cancellation in managed threads.
 }
 catch (MsalClientException ex)
 {
  // Verification code expired before contacting the server
  // This exception will occur if the user does not manage to sign-in before a time out (15 mins) and the
  // call to `AcquireTokenWithDeviceCode` is not cancelled in between
 }
}
```

## <a name="file-based-token-cache"></a>Dosya tabanlı belirteç önbelleği

MSAL.NET içinde bir bellek içi belirteç önbelleği varsayılan olarak sağlanır.

### <a name="serialization-is-customizable-in-windows-desktop-apps-and-web-appsweb-apis"></a>Windows Masaüstü uygulamaları ve web apps/web API'leri özelleştirilebilir seri hale getirme

Ek olarak, hiçbir şey yapmaz, bellek içi belirteç önbelleği .NET Framework ve .NET core söz konusu olduğunda, uygulama süresi boyunca sürer. Neden serileştirme yepyeni sağlanmayan anlamak unutmayın MSAL .NET Masaüstü/core uygulamaları Konsolu ya da (dosya sistemi erişiminiz olması) Windows uygulamalarını olabilir **aynı zamanda** Web uygulamaları veya web API'si. Veritabanları, önbellekler dağıtılmış, redis önbellekleri vb. gibi bazı belirli önbellek mekanizmalar bu Web uygulamaları ve web API'leri kullanabilir. .NET Masaüstü veya çekirdek kalıcı belirteç önbelleği uygulamanız için seri hale getirme özelleştirmek gerekir.

Sınıflar ve arabirimler belirteç önbelleği serileştirme dahil olan aşağıdaki türleri şunlardır:

- ``ITokenCache``, belirteç önbelleği serileştirme istekleri yanı sıra yöntemlerinin serileştirmek veya seri durumdan çıkarılamıyor cache çeşitli biçimlerde, abone olmak için olayları tanımlar (ADAL v3.0, MSAL 2.x ve MSAL 3.x ADAL v5.0 =)
- ``TokenCacheCallback`` Serileştirme işleyebilmeniz bir geri çağırma olaylarına geçirilir. türünde bağımsız değişkenlerle çağrılması ``TokenCacheNotificationArgs``.
- ``TokenCacheNotificationArgs`` yalnızca sağlar ``ClientId`` uygulama ve kullanıcı için belirteç kullanılabildiği bir başvuru

  ![image](https://user-images.githubusercontent.com/13203188/56027172-d58d1480-5d15-11e9-8ada-c0292f1800b3.png)

> [!IMPORTANT]
> MSAL.NET belirteci önbellekler oluşturur ve size sağlar `IToken` önbelleğe uygulamanın çağırdığınızda `GetUserTokenCache` ve `GetAppTokenCache` yöntemleri. Kendiniz arabirim uygulamak için kullanılmaması. Bir özel belirteç önbelleği serileştirme uyguladığınızda sizin sorumluluğunuzdadır olmaktır:
>
> - Tepki `BeforeAccess` ve `AfterAccess` "olaylar". `BeforeAccess` İse temsilci önbellek seri durumdan çıkarılacak sorumlu `AfterAccess` önbellek serileştirmek için sorumlu biridir.
> - Bu olayların bölümü depolamak veya yüklemek istediğiniz her depolama alanına geçirilen olay bağımsız değişkeni BLOB sayısı.

Stratejiler genel istemci uygulaması (Masaüstü) veya gizli bir istemci uygulaması (web uygulaması/web API'si, arka plan programı uygulama) için bir belirteç önbelleği serileştirme yazıyorsanız bağlı olarak farklıdır.

MSAL'ın V2.x beri birkaç, önbelleğe yalnızca (MSAL ile platformlar arasında de yaygındır birleşik biçimi önbellek) MSAL.NET biçimi seri hale getirmek istiyorsanız ya da desteklemek istiyorsanız bağlı olarak seçenekleriniz [eski](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Token-cache-serialization) ADAL V3 serileştirilmesi belirteç önbelleği.

SSO durumu ADAL.NET arasında paylaşmak için belirteç önbelleği serileştirme özelleştirme 3.x ADAL.NET 5.x yanı sıra, MSAL.NET açıklanmıştır bölümü aşağıdaki örnek: [active-directory-dotnet-v1-to-v2](https://github.com/Azure-Samples/active-directory-dotnet-v1-to-v2)

### <a name="simple-token-cache-serialization-msal-only"></a>Basit bir belirteç önbelleği seri hale getirme (yalnızca MSAL)

Bir Masaüstü uygulamaları için bir belirteç önbelleği özel serileştirme naïve uygulaması örneği aşağıdadır. Burada kullanıcının uygulamayla aynı klasörde bir dosya önbelleğinde belirteç.

Uygulamayı sonra çağırarak serileştirme etkinleştirmeniz ``TokenCacheHelper.EnableSerialization()`` uygulama geçirme `UserTokenCache`

```CSharp
app = PublicClientApplicationBuilder.Create(ClientId)
    .Build();
TokenCacheHelper.EnableSerialization(app.UserTokenCache);
```

Bu yardımcı sınıf, aşağıdaki kod parçacığı gibi görünür:

```CSharp
static class TokenCacheHelper
 {
  public static void EnableSerialization(ITokenCache tokenCache)
  {
   tokenCache.SetBeforeAccess(BeforeAccessNotification);
   tokenCache.SetAfterAccess(AfterAccessNotification);
  }

  /// <summary>
  /// Path to the token cache
  /// </summary>
  public static readonly string CacheFilePath = System.Reflection.Assembly.GetExecutingAssembly().Location + ".msalcache.bin3";

  private static readonly object FileLock = new object();


  private static void BeforeAccessNotification(TokenCacheNotificationArgs args)
  {
   lock (FileLock)
   {
    args.TokenCache.DeserializeMsalV3(File.Exists(CacheFilePath)
            ? ProtectedData.Unprotect(File.ReadAllBytes(CacheFilePath),
                                      null,
                                      DataProtectionScope.CurrentUser)
            : null);
   }
  }

  private static void AfterAccessNotification(TokenCacheNotificationArgs args)
  {
   // if the access operation resulted in a cache update
   if (args.HasStateChanged)
   {
    lock (FileLock)
    {
     // reflect changesgs in the persistent store
     File.WriteAllBytes(CacheFilePath,
                         ProtectedData.Protect(args.TokenCache.SerializeMsalV3(),
                                                 null,
                                                 DataProtectionScope.CurrentUser)
                         );
    }
   }
  }
 }
```

Dosya tabanlı serileştirici (Windows, Mac ve Linux'ta çalışan masaüstü uygulamalar için) ortak istemci uygulamaları için kullanılabilir bir ürün kalitesini belirteci bir önbellek önizlemesini [Microsoft.Identity.Client.Extensions.Msal](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Msal) Açık Kaynak Kitaplığı. Uygulamalarınıza aşağıdaki nuget paketinden içerebilir: [Microsoft.Identity.Client.Extensions.Msal](https://www.nuget.org/packages/Microsoft.Identity.Client.Extensions.Msal/).

> Sorumluluk reddi. Microsoft.Identity.Client.Extensions.Msal kitaplığı MSAL.NET uzantısıdır. Bu kitaplıklar sınıflarda aşamalarından MSAL.NET gelecekte olduğundan veya önemli değişiklikler ile yapabilirsiniz.

### <a name="dual-token-cache-serialization-msal-unified-cache--adal-v3"></a>Çift belirteç önbelleği serileştirme (MSAL birleşik önbellek + ADAL V3)

Belirteç önbelleği serileştirme gerçekleştirmek isterseniz birleşik ikisiyle biçimi önbelleğe (ADAL.NET ortak 4.x ve MSAL.NET 2.x ile diğer MSALs aynı nesil ya da daha eski, aynı platform), aşağıdaki kodla ilham alın :

```CSharp
string appLocation = Path.GetDirectoryName(Assembly.GetEntryAssembly().Location;
string cacheFolder = Path.GetFullPath(appLocation) + @"..\..\..\..");
string adalV3cacheFileName = Path.Combine(cacheFolder, "cacheAdalV3.bin");
string unifiedCacheFileName = Path.Combine(cacheFolder, "unifiedCache.bin");

IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
                                    .Build();
FilesBasedTokenCacheHelper.EnableSerialization(app.UserTokenCache,
                                               unifiedCacheFileName,
                                               adalV3cacheFileName);

```

Bu süre yardımcı sınıfı, şu kod gibi görünür:

```CSharp
using System;
using System.IO;
using System.Security.Cryptography;
using Microsoft.Identity.Client;

namespace CommonCacheMsalV3
{
 /// <summary>
 /// Simple persistent cache implementation of the dual cache serialization (ADAL V3 legacy
 /// and unified cache format) for a desktop applications (from MSAL 2.x)
 /// </summary>
 static class FilesBasedTokenCacheHelper
 {
  /// <summary>
  /// Get the user token cache
  /// </summary>
  /// <param name="adalV3CacheFileName">File name where the cache is serialized with the
  /// ADAL V3 token cache format. Can
  /// be <c>null</c> if you don't want to implement the legacy ADAL V3 token cache
  /// serialization in your MSAL 2.x+ application</param>
  /// <param name="unifiedCacheFileName">File name where the cache is serialized
  /// with the Unified cache format, common to
  /// ADAL V4 and MSAL V2 and above, and also across ADAL/MSAL on the same platform.
  ///  Should not be <c>null</c></param>
  /// <returns></returns>
  public static void EnableSerialization(ITokenCache cache, string unifiedCacheFileName, string adalV3CacheFileName)
  {
   usertokenCache = cache;
   UnifiedCacheFileName = unifiedCacheFileName;
   AdalV3CacheFileName = adalV3CacheFileName;

   usertokenCache.SetBeforeAccess(BeforeAccessNotification);
   usertokenCache.SetAfterAccess(AfterAccessNotification);
  }

  /// <summary>
  /// Token cache
  /// </summary>
  static ITokenCache usertokenCache;

  /// <summary>
  /// File path where the token cache is serialized with the unified cache format
  /// (ADAL.NET V4, MSAL.NET V3)
  /// </summary>
  public static string UnifiedCacheFileName { get; private set; }

  /// <summary>
  /// File path where the token cache is serialized with the legacy ADAL V3 format
  /// </summary>
  public static string AdalV3CacheFileName { get; private set; }

  private static readonly object FileLock = new object();

  public static void BeforeAccessNotification(TokenCacheNotificationArgs args)
  {
   lock (FileLock)
   {
    args.TokenCache.DeserializeAdalV3(ReadFromFileIfExists(AdalV3CacheFileName));
    try
    {
     args.TokenCache.DeserializeMsalV3(ReadFromFileIfExists(UnifiedCacheFileName));
    }
    catch(Exception ex)
    {
     // Compatibility with the MSAL v2 cache if you used one
     args.TokenCache.DeserializeMsalV2(ReadFromFileIfExists(UnifiedCacheFileName));
    }
   }
  }

  public static void AfterAccessNotification(TokenCacheNotificationArgs args)
  {
   // if the access operation resulted in a cache update
   if (args.HasStateChanged)
   {
    lock (FileLock)
    {
     WriteToFileIfNotNull(UnifiedCacheFileName, args.TokenCache.SerializeMsalV3());
     if (!string.IsNullOrWhiteSpace(AdalV3CacheFileName))
     {
      WriteToFileIfNotNull(AdalV3CacheFileName, args.TokenCache.SerializeAdalV3());
     }
    }
   }
  }

  /// <summary>
  /// Read the content of a file if it exists
  /// </summary>
  /// <param name="path">File path</param>
  /// <returns>Content of the file (in bytes)</returns>
  private static byte[] ReadFromFileIfExists(string path)
  {
   byte[] protectedBytes = (!string.IsNullOrEmpty(path) && File.Exists(path))
       ? File.ReadAllBytes(path) : null;
   byte[] unprotectedBytes = encrypt ?
       ((protectedBytes != null) ? ProtectedData.Unprotect(protectedBytes, null, DataProtectionScope.CurrentUser) : null)
       : protectedBytes;
   return unprotectedBytes;
  }

  /// <summary>
  /// Writes a blob of bytes to a file. If the blob is <c>null</c>, deletes the file
  /// </summary>
  /// <param name="path">path to the file to write</param>
  /// <param name="blob">Blob of bytes to write</param>
  private static void WriteToFileIfNotNull(string path, byte[] blob)
  {
   if (blob != null)
   {
    byte[] protectedBytes = encrypt
      ? ProtectedData.Protect(blob, null, DataProtectionScope.CurrentUser)
      : blob;
    File.WriteAllBytes(path, protectedBytes);
   }
   else
   {
    File.Delete(path);
   }
  }

  // Change if you want to test with an un-encrypted blob (this is a json format)
  private static bool encrypt = true;
 }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Masaüstü uygulamasından web API'si çağırma](scenario-desktop-call-api.md)
