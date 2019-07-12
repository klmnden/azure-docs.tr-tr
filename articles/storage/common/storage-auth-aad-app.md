---
title: Azure Active Directory ile istemci uygulamasından blob ve kuyruk verilere erişmek için kimlik doğrulaması
description: Bir istemci uygulamasında kimlik doğrulaması, OAuth 2.0 belirteç almak ve Azure Blob Depolama ve kuyruk depolama isteklerini yetkilendirmek için Azure Active Directory kullanın.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/05/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: da10b70b85e284173abbd1779fb1d39f477ca0cd
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723211"
---
# <a name="authenticate-with-azure-active-directory-from-an-application-for-access-to-blobs-and-queues"></a>BLOB'lar ve Kuyruklar için erişim için bir uygulamadan Azure Active Directory kimlik doğrulaması

Önemli bir avantajı, Azure Blob Depolama veya kuyruk depolama ile Azure Active Directory (Azure AD) kullanarak kimlik bilgilerinizi artık kodunuzda depolanacak olmanızdır. Bunun yerine, Microsoft kimlik Platformu (eski adıyla Azure AD) bir OAuth 2.0 erişim belirteci isteğinde bulunabilirsiniz. Azure AD kimliğini doğrular, güvenlik sorumlusunu (kullanıcı, Grup veya hizmet sorumlusu) uygulamayı çalıştırma. Doğrulama başarılı olursa, Azure AD erişim belirteci uygulamaya döndürür ve uygulama daha sonra Azure Blob Depolama veya kuyruk depolama isteklerine yetkilendirmek için erişim belirteci kullanabilirsiniz.

Bu makalede, yerel uygulama veya web uygulaması kimlik doğrulaması için Microsoft kimlik platformu 2.0 yapılandırma gösterilmektedir. Kod örneği özellikleri .NET, ancak diğer diller benzer bir yaklaşım kullanın. Microsoft kimlik platformu 2.0 hakkında daha fazla bilgi için bkz: [Microsoft kimlik Platformu (v2.0) genel bakış](../../active-directory/develop/v2-overview.md).

OAuth 2.0 kodu verme akışı genel bakış için bkz: [Authorize OAuth 2.0 kod kullanarak Azure Active Directory web uygulamalarına erişim akışı](../../active-directory/develop/v2-oauth2-auth-code-flow.md).

## <a name="assign-a-role-to-an-azure-ad-security-principal"></a>Bir Azure AD güvenlik sorumlusu rol atama

Azure depolama uygulamanızdan bir güvenlik sorumlusunun kimliğini doğrulamak için önce bu güvenlik sorumlusu için rol tabanlı erişim denetimi (RBAC) ayarlarını yapılandırın. Azure depolama kapsayıcıları ve Kuyruklar için izinleri kapsayacak yerleşik RBAC rolleri tanımlar. Bu güvenlik sorumlusu, RBAC rolü için bir güvenlik sorumlusu atandığında, bu kaynağa erişim izni verilir. Daha fazla bilgi için [RBAC ile Azure Blob ve kuyruk verilere erişim haklarını yönetme](storage-auth-aad-rbac.md).

## <a name="register-your-application-with-an-azure-ad-tenant"></a>Azure AD kiracısı ile uygulamanızı kaydetme

Depolama kaynaklarına erişim yetkisi vermek için Azure AD kullanarak ilk adımı, istemci uygulamanız bir Azure AD kiracısından ile kaydediyor [Azure portalında](https://portal.azure.com). İstemci uygulamanızı kaydettiğinizde uygulama Azure AD'ye hakkında bilgi sağlayın. Ardından Azure AD, bir istemci kimliği sağlar (olarak da adlandırılan bir *uygulama kimliği*), uygulamanızın çalışma zamanında Azure AD ile ilişkilendirmek için kullanın. İstemci kimliği hakkında daha fazla bilgi için bkz: [uygulaması ve Azure Active Directory'de Hizmet sorumlusu nesneleri](../../active-directory/develop/app-objects-and-service-principals.md).

Azure Storage uygulamanızı kaydetmek için gösterilen adımları [hızlı başlangıç: Microsoft kimlik platformu bir uygulamayı kaydetme](../../active-directory/develop/quickstart-configure-app-access-web-apis.md). Aşağıdaki resimde, bir web uygulaması kaydetmek için genel ayarları gösterilmektedir:

![Storage uygulamanızı Azure AD'ye kaydetme gösteren ekran görüntüsü](./media/storage-auth-aad-app/app-registration.png)

> [!NOTE]
> Uygulamanızı yerel bir uygulama kaydederseniz, için geçerli bir URI belirtebilirsiniz **yeniden yönlendirme URI'si**. Yerel uygulamalar için bu değer gerçek bir URL olması gerekmez. Belirteçleri sağlanan URL belirttiğinden, web uygulamaları için yeniden yönlendirme URI'si geçerli bir URI olmalıdır.

Uygulamanızı kaydettikten sonra uygulama kimliği (veya istemci kimliği) altında görürsünüz **ayarları**:

![İstemci Kimliğini gösteren ekran görüntüsü](./media/storage-auth-aad-app/app-registration-client-id.png)

Bir uygulamayı Azure AD'ye kaydetme hakkında daha fazla bilgi için bkz. [uygulamaları Azure Active Directory ile tümleştirme](../../active-directory/develop/quickstart-v2-register-an-app.md).

## <a name="grant-your-registered-app-permissions-to-azure-storage"></a>Azure depolama için kayıtlı uygulama izinleri verme

Ardından, Azure depolama API'leri çağırmak için uygulama izinleri verin. Bu adım, uygulamanızı Azure AD ile Azure Depolama'ya yönelik isteklerin yetkilendirmek etkinleştirir.

1. Üzerinde **genel bakış** kayıtlı uygulamanızda seçin sayfasında **API izinleri görüntüleme**.
1. İçinde **API izinleri** bölümünden **bir izin eklemek** ve **Microsoft APIs**.
1. Seçin **Azure depolama** görüntülenecek sonuç listesinden **istek API izinleri** bölmesi.
1. Altında **ne tür izinler uygulamanızı gerektiriyor mu?** , kullanılabilir izin türü olup olmadığına bakın **temsilci izinleri**. Bu seçenek, varsayılan olarak seçilidir.
1. İçinde **izinleri seçin** bölümünü **istek API izinleri** bölmesinde yanındaki onay kutusunu işaretleyin **user_impersonation**, ardından **Ekle izinleri**.

    ![Depolama için ekran gösterme izinleri](media/storage-auth-aad-app/registered-app-permissions-1.png)

**API izinleri** bölmesi artık gösterilmektedir, kayıtlı Azure AD uygulaması, hem Microsoft Graph hem de Azure depolama erişimine sahiptir. İlk uygulamanızı Azure AD'ye kaydetme izinleri Microsoft Graph için otomatik olarak verilir.

![Uygulama izinleri gösteren ekran görüntüsü kaydetme](media/storage-auth-aad-app/registered-app-permissions-2.png)

## <a name="create-a-client-secret"></a>İstemci gizli dizi oluşturma

Uygulamanın bir belirteç isterken kimliğini kanıtlamak için bir istemci parolası gerekiyor. İstemci gizli dizi eklemek için aşağıdaki adımları izleyin:

1. Azure portalında uygulama kaydınızı gidin.
1. Seçin **sertifikaları ve parolaları** ayarı.
1. Altında **istemci gizli dizileri**, tıklayın **yeni gizli** yeni bir gizli dizi oluşturmak için.
1. Gizli dizi için bir açıklama sağlayın ve istenen sona erme aralığını seçin.
1. Hemen yeni gizli anahtar değerini güvenli bir konuma kopyalayın. Tam değeri, yalnızca bir kez görüntülenir.

    ![Gizli anahtar ekran görüntüsü](media/storage-auth-aad-app/client-secret.png)

## <a name="client-libraries-for-token-acquisition"></a>Belirteç edinme için istemci kitaplıkları

Uygulamanızı kayıtlı ve verileri Azure Blob Depolama veya kuyruk depolama erişim izni verilmiş bir güvenlik sorumlusu kimliğini doğrulamak ve OAuth 2.0 belirteç almak için uygulamanıza kod ekleyebilirsiniz. Kimlik doğrulaması ve belirteci almak için aşağıdakilerden birini kullanabilirsiniz [Microsoft kimlik platformu kimlik doğrulama kitaplıkları](../../active-directory/develop/reference-v2-libraries.md) veya Openıd Connect 1.0 destekleyen başka bir açık kaynak kitaplığı. Uygulamanızı bir Azure Blob Depolama veya kuyruk depolama isteği yetkilendirmek için erişim belirteci sonra kullanabilirsiniz.

Bir listesi için belirteçlerini almak desteklendiği senaryolar için bkz: [senaryoları](https://aka.ms/msal-net-scenarios) bölümünü [.NET için Microsoft kimlik doğrulama kitaplığı (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) GitHub deposu.

## <a name="net-code-example-create-a-block-blob"></a>.NET kod örneği: Bir blok blobu oluştur

Örnek kodu bir erişim almak Azure AD'den belirteci gösterilmektedir. Erişim belirteci, belirtilen kullanıcı kimlik doğrulaması yapmak ve sonra bir blok blobu oluşturma isteği yetkilendirmek için kullanılır. Bu örnek çalışabilmesi için önce önceki bölümlerde özetlenen adımları izleyin.

Belirteç istemek için aşağıdaki değerlerden birini uygulamanızın kaydını gerekir:

- Azure AD etki alanınızın adı. Bu değeri almak **genel bakış** Azure Active Directory sayfasında.
- Kiracı (veya dizin) kimliği. Bu değeri almak **genel bakış** , uygulama kayıt sayfasında.
- İstemci (veya uygulama) kimliği. Bu değeri almak **genel bakış** , uygulama kayıt sayfasında.
- İstemci yeniden yönlendirme URI'si. Bu değeri almak **kimlik doğrulaması** ayarlarını, uygulama kaydı.
- İstemci gizli anahtarı değeri. Bu değer, daha önce bu kopyalanacağı konumu alır.

### <a name="well-known-values-for-authentication-with-azure-ad"></a>Azure AD ile kimlik doğrulaması için iyi bilinen değerler

Bir güvenlik sorumlusu Azure AD ile kimlik doğrulamak için kodunuzda iyi bilinen bazı değerler eklemeniz gerekir.

#### <a name="azure-ad-authority"></a>Azure AD yetkilisi

Microsoft Genel bulut, temel Azure AD yetkilisi olduğu gibi burada *Kiracı-kimliği* Active Directory Kiracı kimliği (veya dizin kimliği):

`https://login.microsoftonline.com/<tenant-id>/`

Kiracı kimliği, kimlik doğrulaması için kullanılacak Azure AD kiracısı tanımlar. Bu aynı zamanda dizin kimliği olarak adlandırılır Kiracı Kimliğini almak için gidin **genel bakış** sayfasında, Azure portalında uygulama kaydı ve buradan değerini kopyalayın.

#### <a name="storage-resource-id"></a>Depolama kaynak kimliği

Azure depolama kaynak kimliği, Azure Depolama'ya yönelik isteklerin yetkilendirmek için bir belirteç almak için kullanın:

`https://storage.azure.com/`

### <a name="create-a-storage-account-and-container"></a>Bir depolama hesabı ve kapsayıcı oluşturma

Kod örneği çalıştırmak için Azure Active Directory ile aynı Abonelikteki bir depolama hesabı oluşturun. Ardından bu depolama hesabında bir kapsayıcı oluşturun. Örnek kod, bu kapsayıcı içinde blok blobu oluşturacaksınız.

Ardından, açıkça atama **depolama Blob verileri katkıda bulunan** role kullanıcı hesabı altında yükseltemediğiniz durumda örnek kodu. Azure portalında bu rolü atama hakkında yönergeler için bkz: [verilere Azure blob ve kuyruk RBAC ile Azure portalında erişim ver](storage-auth-aad-rbac-portal.md).

> [!NOTE]
> Bir Azure depolama hesabı oluşturduğunuzda, Azure AD ile veri erişim izni otomatik olarak atanmaz. Siz açıkça kendiniz bir RBAC rolü için Azure depolama atamanız gerekir. Abonelik, kaynak grubu, depolama hesabı veya kapsayıcı veya kuyruk düzeyinde atayabilirsiniz.

### <a name="create-a-web-application-that-authorizes-access-to-blob-storage-with-azure-ad"></a>Azure AD ile Blob depolama alanına erişimi yetkilendirir bir web uygulaması oluşturma

Uygulamanızı Azure Storage'a eriştiğinde, bu kullanıcı adına, oturum açmış olan kullanıcının izinlerini kullanarak blob veya kuyruğa kaynaklara erişildiğini anlamı benzeri. Bu kod örneği denemek için bir Azure AD kimliğini kullanarak oturum açmasını ister bir web uygulaması gerekir. Kendi uzantınızı oluşturun veya Microsoft tarafından sağlanan örnek bir uygulama.

Bir belirteç alır ve Azure Depolama'da bir blob oluşturmak için kullandığı bir tamamlanmış örnek web uygulaması üzerinde kullanılabilir [GitHub](https://aka.ms/aadstorage). Gözden geçirme ve tamamlanan örnek çalışan kod örneklerini anlamak için yararlı olabilir. Tamamlanan örnek çalıştırma gösteren yönergeler karşınıza için bölümüne [görünümü ve çalıştırma tamamlanmış örnek](#view-and-run-the-completed-sample).

#### <a name="add-references-and-using-statements"></a>Başvurular ekleyin ve using deyimleri  

Visual Studio'dan Azure depolama istemci Kitaplığı yükleyin. Gelen **Araçları** menüsünde **Nuget Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. .NET için Azure depolama istemci Kitaplığı'ndan gerekli paketleri yüklemek için konsol penceresine aşağıdaki komutları yazın:

```console
Install-Package Microsoft.Azure.Storage.Blob
Install-Package Microsoft.Azure.Storage.Common
```

Ardından, aşağıdaki ekleyin HomeController.cs dosyasına using deyimlerini:

```csharp
using Microsoft.Identity.Client; //MSAL library for getting the access token
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
```

#### <a name="create-a-block-blob"></a>Bir blok blobu oluştur

Bir blok blobu oluşturmak için aşağıdaki kod parçacığını ekleyin:

```csharp
private static async Task<string> CreateBlob(string accessToken)
{
    // Create a blob on behalf of the user
    TokenCredential tokenCredential = new TokenCredential(accessToken);
    StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

    // Replace the URL below with your storage account URL
    CloudBlockBlob blob =
        new CloudBlockBlob(
            new Uri("https://<storage-account>.blob.core.windows.net/<container>/Blob1.txt"),
            storageCredentials);
    await blob.UploadTextAsync("Blob created by Azure AD authenticated user.");
    return "Blob successfully created";
}
```

> [!NOTE]
> OAuth 2.0 belirteç ile BLOB ve kuyruk işlemlerini yetkilendirmek için HTTPS kullanmalıdır.

Yukarıdaki örnekte, blok blobu oluşturmak için isteğin yetkilendirme .NET istemci kitaplığı işler. Diğer diller için Azure depolama istemci kitaplıkları, ayrıca, isteğin yetkilendirme işleyin. Ancak, REST API kullanarak bir OAuth belirteci ile bir Azure depolama işlemi çağırıyorsanız, OAuth belirteci kullanarak isteği yetkilendirmek gerekecektir.

OAuth erişim belirteçleri kullanarak Blob ve kuyruk hizmet işlemlerini aramak üzere erişim belirteci geçirmek **yetkilendirme** üst bilgisini kullanarak **taşıyıcı** şeması ve bir hizmet sürüm 2017-11-09 veya üzeri olarak belirtin Aşağıdaki örnekte gösterilen:

```https
GET /container/file.txt HTTP/1.1
Host: mystorageaccount.blob.core.windows.net
x-ms-version: 2017-11-09
Authorization: Bearer eyJ0eXAiOnJKV1...Xd6j
```

#### <a name="get-an-oauth-token-from-azure-ad"></a>Azure AD'den bir OAuth belirteci alma

Ardından, Azure AD'den bir belirteç istediği bir yöntem ekleyin. Kullanıcı adına, isteği belirteci olacaktır ve GetTokenOnBehalfOfUser yöntemi kullanacağız.

Unutmayın, kısa süre önce oturum açtıysanız ve için bir belirteç isteğinde bulunduğunuzu `storage.azure.com` kaynak, burada kullanıcı izni verebilir tür bir eylemin gerçekleştirilemeyeceğine ilişkin bir kullanıcı Arabirimi ile kullanıcının sunması gerekir. Catch gerektiğini kolaylaştırmak için `MsalUiRequiredException` ve kullanıcı onayı isteği işlevselliği aşağıdaki örnekte gösterildiği gibi ekleyin:

```csharp
public async Task<IActionResult> Blob()
{
    var scopes = new string[] { "https://storage.azure.com/user_impersonation" };
    try
    {
        var accessToken =
            await _tokenAcquisition.GetAccessTokenOnBehalfOfUser(HttpContext, scopes);
        ViewData["Message"] = await CreateBlob(accessToken);
        return View();
    }
    catch (MsalUiRequiredException ex)
    {
        AuthenticationProperties properties = BuildAuthenticationPropertiesForIncrementalConsent(scopes, ex);
        return Challenge(properties);
    }
}
```

İzin verme yetkilendirme gerçekleştirilemeyeceğine ilişkin korunan kaynaklara erişim için bir uygulama için bir kullanıcının işlemidir. Microsoft kimlik platformu 2.0 bir güvenlik sorumlusu en az bir izin kümesi başlangıçta isteği ve izinleri zamanla gerektiği gibi ekleyin, artımlı onay destekler. Kodunuzu bir erişim belirteci isteğinde bulunduğunda, istediğiniz zaman tarafından uygulamanıza gereken izinleri kapsamını belirtmek `scope` parametresi. Artımlı onayı hakkında daha fazla bilgi için başlıklı bölüme bakın **artımlı ve dinamik onay** içinde [neden Microsoft identity platformuna (v2.0) güncelleştirme?](../../active-directory/develop/azure-ad-endpoint-comparison.md#incremental-and-dynamic-consent).

Aşağıdaki yöntem artımlı onay isteme kimlik doğrulaması özelliklerini oluşturur:

```csharp
private AuthenticationProperties BuildAuthenticationPropertiesForIncrementalConsent(string[] scopes, MsalUiRequiredException ex)
{
    AuthenticationProperties properties = new AuthenticationProperties();

    // Set the scopes, including the scopes that ADAL.NET / MSAL.NET need for the Token cache.
    string[] additionalBuildInScopes = new string[] { "openid", "offline_access", "profile" };
    properties.SetParameter<ICollection<string>>(OpenIdConnectParameterNames.Scope, scopes.Union(additionalBuildInScopes).ToList());

    // Attempt to set the login_hint so that the logged-in user is not presented with an account selection dialog.
    string loginHint = HttpContext.User.GetLoginHint();
    if (!string.IsNullOrWhiteSpace(loginHint))
    {
        properties.SetParameter<string>(OpenIdConnectParameterNames.LoginHint, loginHint);

        string domainHint = HttpContext.User.GetDomainHint();
        properties.SetParameter<string>(OpenIdConnectParameterNames.DomainHint, domainHint);
    }

    // Specify any additional claims that are required (for instance, MFA).
    if (!string.IsNullOrEmpty(ex.Claims))
    {
        properties.Items.Add("claims", ex.Claims);
    }

    return properties;
}
```

## <a name="view-and-run-the-completed-sample"></a>Görüntüleme ve tamamlanan örnek çalıştırma

Örnek uygulamayı çalıştırmak için önce klonlayın veya indirdiği [GitHub](https://aka.ms/aadstorage). Ardından uygulama, aşağıdaki bölümlerde açıklandığı gibi güncelleştirin.

### <a name="provide-values-in-the-settings-file"></a>Ayarlar dosyasındaki değerleri sağlayın

Ardından, güncelleştirme *appsettings.json* kendi değerlerinizle gibi dosya:

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "<azure-ad-domain-name>.onmicrosoft.com",
    "TenantId": "<tenant-id>",
    "ClientId": "<client-id>",
    "CallbackPath": "/signin-oidc",
    "SignedOutCallbackPath ": "/signout-callback-oidc",

    // To call an API
    "ClientSecret": "<client-secret>"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

### <a name="update-the-storage-account-and-container-name"></a>Depolama hesabı ve kapsayıcı adını güncelleştir

İçinde *HomeController.cs* dosyası, blok blobu depolama hesabı ve kapsayıcı adını kullanmak üzere başvuran URI güncelleştirin:

```csharp
CloudBlockBlob blob = new CloudBlockBlob(
                      new Uri("https://<storage-account>.blob.core.windows.net/<container>/Blob1.txt"),
                      storageCredentials);
```

### <a name="enable-implicit-grant-flow"></a>Örtük verme akışı etkinleştir

Örneği çalıştırmak için uygulama kaydı için örtük verme akışı yapılandırmanız gerekebilir. Şu adımları uygulayın:

1. Azure portalında uygulama kaydınızı gidin.
1. Yönet bölümünde **kimlik doğrulaması** ayarı.
1. Altında **Gelişmiş ayarlar**, **örtük vermeyi** bölümünde, aşağıdaki görüntüde gösterildiği gibi erişim belirteçleri ve kimlik belirteçlerini etkinleştirme için onay kutularını seçin:

    ![Örtük verme akışı için ayarları etkinleştirin gösteren ekran görüntüsü](media/storage-auth-aad-app/enable-implicit-grant-flow.png)

### <a name="update-the-port-used-by-localhost"></a>Localhost tarafından kullanılan bağlantı noktasını güncelleştirin

Örneği çalıştırdığınızda, yeniden yönlendirme URI'si kullanmak için uygulama kaydında belirtilen güncelleştirmeniz gerektiğini fark edebilirsiniz *localhost* çalışma zamanında atanan bağlantı noktası. Yeniden yönlendirme URI'si atanan bağlantı noktasını kullanacak şekilde güncelleştirmek için aşağıdaki adımları izleyin:

1. Azure portalında uygulama kaydınızı gidin.
1. Yönet bölümünde **kimlik doğrulaması** ayarı.
1. Altında **yeniden yönlendirme URI'leri**, aşağıdaki görüntüde gösterildiği gibi örnek uygulama tarafından kullanılan eşleştirmek için bağlantı noktasını düzenleyin:

    ![Yeniden yönlendirme URI'leri için uygulama kaydı gösteren ekran görüntüsü](media/storage-auth-aad-app/redirect-uri.png)

## <a name="next-steps"></a>Sonraki adımlar

- Microsoft kimlik platformu hakkında daha fazla bilgi için bkz: [Microsoft kimlik platformu](https://docs.microsoft.com/azure/active-directory/develop/).
- RBAC rolleri için Azure depolama hakkında daha fazla bilgi için bkz: [RBAC ile depolama verilere erişim haklarını yönetme](storage-auth-aad-rbac.md).
- Azure depolama ile Azure kaynakları için yönetilen kimlikleri kullanma hakkında bilgi edinmek için [erişim BLOB'lar ve Kuyruklar ile Azure Active Directory ve yönetilen kimlikleri için Azure kaynakları için kimlik doğrulaması](storage-auth-aad-msi.md).
