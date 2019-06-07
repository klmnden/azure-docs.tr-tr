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
ms.openlocfilehash: e45fe20e93d81c1cfd1f868b40f76743558758bb
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66754971"
---
# <a name="authenticate-with-azure-active-directory-from-an-application-for-access-to-blobs-and-queues"></a>BLOB'lar ve Kuyruklar için erişim için bir uygulamadan Azure Active Directory kimlik doğrulaması

Önemli bir avantajı, Azure Blob Depolama veya kuyruk depolama ile Azure Active Directory (Azure AD) kullanarak kimlik bilgilerinizi artık kodunuzda depolanacak olmanızdır. Bunun yerine, Microsoft kimlik Platformu (eski adıyla Azure AD) bir OAuth 2.0 erişim belirteci isteğinde bulunabilirsiniz. Azure AD kimliğini doğrular, güvenlik sorumlusunu (kullanıcı, Grup veya hizmet sorumlusu) uygulamayı çalıştırma. Doğrulama başarılı olursa, Azure AD erişim belirteci uygulamaya döndürür ve uygulama daha sonra Azure Blob Depolama veya kuyruk depolama isteklerine yetkilendirmek için erişim belirteci kullanabilirsiniz.

Bu makalede, Azure AD ile yerel uygulamanızı veya web uygulaması kimlik doğrulaması için yapılandırma gösterilmektedir. Kod örneği özellikleri .NET, ancak diğer diller benzer bir yaklaşım kullanın.

OAuth 2.0 kodu verme akışı genel bakış için bkz: [Authorize OAuth 2.0 kod kullanarak Azure Active Directory web uygulamalarına erişim akışı](../../active-directory/develop/v2-oauth2-auth-code-flow.md).

## <a name="assign-an-rbac-role-to-an-azure-ad-security-principal"></a>Bir Azure AD güvenlik sorumlusu için bir RBAC rolü atayın

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
1. İçinde **API izinleri** bölümünden **bir izin eklemek** ve **Kuruluşum kullandığı API'leri**.
1. Altında **Kuruluşum kullandığı API'leri** bölümünde "Azure Depolama'ya yönelik" arayın ve seçin **Azure depolama** görüntülenecek sonuç listesinden **istek API izinleri** bölme.

    ![Depolama için ekran gösterme izinleri](media/storage-auth-aad-app/registered-app-permissions-1.png)

1. Altında **ne tür izinler uygulamanızı gerektiriyor mu?** , kullanılabilir izin türü olduğuna dikkat edin **temsilci izinleri**. Bu seçenek, varsayılan olarak seçilidir.
1. İçinde **izinleri seçin** bölümünü **istek API izinleri** bölmesinde yanındaki onay kutusunu işaretleyin **user_impersonation**, ardından **Ekle izinleri**.
1. **API izinleri** bölmesi artık gösterir, Azure AD uygulaması hem Microsoft Graph ve Azure depolama erişimi olduğunu. İlk uygulamanızı Azure AD'ye kaydetme izinleri Microsoft Graph için otomatik olarak verilir.

    ![Uygulama izinleri gösteren ekran görüntüsü kaydetme](media/storage-auth-aad-app/registered-app-permissions-2.png)

## <a name="libraries-for-token-acquisition"></a>Belirteç edinme için kitaplıkları

Uygulamanızı kayıtlı ve verileri Azure Blob Depolama veya kuyruk depolama erişim izni verilmiş bir güvenlik sorumlusu kimliğini doğrulamak ve OAuth 2.0 belirteç almak için uygulamanıza kod ekleyebilirsiniz. Kimlik doğrulaması ve belirteci almak için aşağıdakilerden birini kullanabilirsiniz [Microsoft kimlik platformu kimlik doğrulama kitaplıkları](../../active-directory/develop/reference-v2-libraries.md) veya Openıd Connect 1.0 destekleyen başka bir açık kaynak kitaplığı. Uygulamanızı bir Azure Blob Depolama veya kuyruk depolama isteği yetkilendirmek için erişim belirteci sonra kullanabilirsiniz.

Bir listesi için belirteçlerini almak desteklendiği senaryolar için bkz: [senaryoları](https://aka.ms/msal-net-scenarios) bölümünü [.NET için Microsoft kimlik doğrulama kitaplığı (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) GitHub deposu.

## <a name="net-code-example-create-a-block-blob"></a>.NET kod örneği: Bir blok blobu oluştur

Örnek kodu bir erişim almak Azure AD'den belirteci gösterilmektedir. Erişim belirteci, belirtilen kullanıcı kimlik doğrulaması yapmak ve sonra bir blok blobu oluşturma isteği yetkilendirmek için kullanılır. Bu örnek çalışabilmesi için önce önceki bölümlerde özetlenen adımları izleyin.

> [!NOTE]
> Bir Azure depolama hesabınızın sahibi olarak, otomatik olarak veri erişim izni atanmaz. Siz açıkça kendiniz bir RBAC rolü için Azure depolama atamanız gerekir. Abonelik, kaynak grubu, depolama hesabı veya kapsayıcı veya kuyruk düzeyinde atayabilirsiniz.
>
> Örneğin, bir depolama hesabı sahibi olduğunuz ve kendi kullanıcı kimliği altında örnek kodu çalıştırmak için RBAC rolü için Blob verileri katkıda bulunan kendinize atamanız gerekir. Aksi takdirde, blob oluşturmak için çağrı 403 (Yasak) HTTP durum kodu ile başarısız olur. Daha fazla bilgi için [RBAC ile depolama verilere erişim haklarını yönetme](storage-auth-aad-rbac.md).

### <a name="well-known-values-for-authentication-with-azure-ad"></a>Azure AD ile kimlik doğrulaması için iyi bilinen değerler

Bir güvenlik sorumlusu Azure AD ile kimlik doğrulamak için kodunuzda iyi bilinen bazı değerler eklemeniz gerekir.

#### <a name="azure-ad-authority"></a>Azure AD yetkilisi

Microsoft Genel bulut, temel Azure AD yetkilisi olduğu gibi burada *Kiracı-kimliği* Active Directory Kiracı kimliği (veya dizin kimliği):

`https://login.microsoftonline.com/<tenant-id>/`

Kiracı kimliği, kimlik doğrulaması için kullanılacak Azure AD kiracısı tanımlar. Kiracı Kimliğini almak için başlıklı bölümde özetlenen adımları izleyin. **için Azure Active Directory Kiracı Kimliğinizi öğrenmenin**.

#### <a name="storage-resource-id"></a>Depolama kaynak kimliği

Azure depolama kaynak kimliği, Azure Depolama'ya yönelik isteklerin yetkilendirmek için bir belirteç almak için kullanın:

`https://storage.azure.com/`

### <a name="get-the-tenant-id-for-your-azure-active-directory"></a>İçin Azure Active Directory Kiracı Kimliğinizi alma

Kiracı Kimliğini almak için şu adımları izleyin:

1. Azure portalında Active Directory'nizi seçin.
2. **Özellikler**'e tıklayın.
3. Sağlanan GUID değeri kopyalayın **dizin kimliği**. Bu değer Kiracı kimliği olarak da adlandırılır

![Kiracı kimliği kopyalanmasını gösteren ekran görüntüsü](./media/storage-auth-aad-app/aad-tenant-id.png)

## <a name="set-up-a-basic-web-app-to-authenticate-to-azure-ad"></a>Azure AD ile kimlik doğrulaması için bir temel web uygulaması ayarlama

Uygulamanızı Azure Storage'a eriştiğinde, bu kullanıcı adına benzeri. Bu kod örneği denemek için bir web ihtiyacınız vardır. kullanıcıdan bir uygulamayı bir Azure AD kimliğini kullanarak oturum açabilir. Bu indirebileceğiniz [kod örneği](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2) Azure AD hesabınızla kimlik doğrulayan bir temel web uygulamasını test etmek için.

### <a name="completed-sample"></a>Tamamlanan örnek

Bu makaledeki örnek kodun tam çalışma sürümü yüklenebilir [GitHub](http://aka.ms/aadstorage). Gözden geçirme ve tam bir örnek çalıştırmak, kod örnekleri anlamak için yararlı olabilir.

### <a name="add-references-and-using-statements"></a>Başvurular ekleyin ve using deyimleri  

Visual Studio'dan Azure depolama istemci Kitaplığı yükleyin. Gelen **Araçları** menüsünde **Nuget Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. .NET için Azure depolama istemci Kitaplığı'ndan gerekli paketleri yüklemek için konsol penceresine aşağıdaki komutları yazın:

```console
Install-Package Microsoft.Azure.Storage.Blob
Install-Package Microsoft.Azure.Storage.Common
```

Ardından, aşağıdaki ekleyin HomeController.cs dosyasına using deyimlerini:

```csharp
using System;
using Microsoft.Identity.Client; //MSAL library for getting the access token
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
```

### <a name="create-a-block-blob"></a>Bir blok blobu oluştur

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
            new Uri("https://<storage-account>.blob.core.windows.net/sample-container/Blob1.txt"),
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

### <a name="get-an-oauth-token-from-azure-ad"></a>Azure AD'den bir OAuth belirteci alma

Ardından, Azure AD'den bir belirteç istediği bir yöntem ekleyin. Kullanıcı adına, isteği belirteci olacaktır ve GetTokenOnBehalfOfUser yöntemi kullanacağız.

Belirteç istemek için aşağıdaki değerlerden birini uygulamanızın kaydını gerekir,

- Kiracı (veya dizin) kimliği
- İstemci (veya uygulama) kimliği
- İstemci yeniden yönlendirme URI'si

Yalnızca oturum ve için bir belirteç isteğinde bulunduğunuzu unutmayın `storage.azure.com` kaynak, burada kullanıcı izni verebilir tür bir eylemin gerçekleştirilemeyeceğine ilişkin bir kullanıcı Arabirimi ile kullanıcının sunması gerekir. Catch gerektiğini kolaylaştırmak için `MsalUiRequiredException` ve kullanıcı onayı isteği işlevselliği aşağıdaki örnekte gösterildiği gibi ekleyin:

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

İzin verme yetkilendirme gerçekleştirilemeyeceğine ilişkin korunan kaynaklara erişim için bir uygulama için bir kullanıcının işlemidir. Microsoft kimlik platformu 2.0 bir güvenlik sorumlusu en az bir izin kümesi başlangıçta isteği ve izinleri zamanla gerektiği gibi ekleyin, artımlı onay destekler. Kodunuzu bir erişim belirteci isteğinde bulunduğunda, istediğiniz zaman tarafından uygulamanıza gereken izinleri kapsamını belirtmek `scope` parametresi. Aşağıdaki yöntem artımlı onay isteme kimlik doğrulaması özelliklerini oluşturur:

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

## <a name="next-steps"></a>Sonraki adımlar

- Microsoft kimlik platformu hakkında daha fazla bilgi için bkz: [Microsoft kimlik platformu](https://docs.microsoft.com/azure/active-directory/develop/).
- RBAC rolleri için Azure depolama hakkında daha fazla bilgi için bkz: [RBAC ile depolama verilere erişim haklarını yönetme](storage-auth-aad-rbac.md).
- Azure depolama ile Azure kaynakları için yönetilen kimlikleri kullanma hakkında bilgi edinmek için [erişim BLOB'lar ve Kuyruklar ile Azure Active Directory ve yönetilen kimlikleri için Azure kaynakları için kimlik doğrulaması](storage-auth-aad-msi.md).
