---
title: Azure Active Directory ile blob ve kuyruk verilere erişmek için uygulamalarınızdan (Önizleme) kimlik doğrulaması | Microsoft Docs
description: Bir uygulama içinde kimlik doğrulaması ve ardından Azure depolama kaynaklarını (Önizleme) istekleri yetkilendirmek için Azure Active Directory kullanın.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/07/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: 6a0b7139fd8d216397090154a4324c8e4305a939
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816387"
---
# <a name="authenticate-with-azure-active-directory-from-an-azure-storage-application-preview"></a>Bir Azure depolama uygulaması (Önizleme) Azure Active Directory ile kimlik doğrulaması

Azure depolama ile Azure Active Directory (Azure AD) kullanarak önemli bir avantajı, kimlik bilgilerinizi artık kodunuzda depolanması gerektiğini ' dir. Bunun yerine, Azure AD'den bir OAuth 2.0 erişim belirteci isteğinde bulunabilirsiniz. Azure AD güvenlik sorumlusunu (kullanıcı, Grup veya hizmet sorumlusu) kimlik doğrulamasını işleyen çalışan uygulama. Doğrulama başarılı olursa, Azure AD erişim belirteci uygulamaya döndürür ve uygulama daha sonra Azure Depolama'ya yönelik isteklerin yetkilendirmek için erişim belirteci kullanabilirsiniz.

Bu makalede, Azure AD ile kimlik doğrulaması için uygulamanızı yapılandırma gösterilmektedir. Kod örneği özellikleri .NET, ancak diğer diller benzer bir yaklaşım kullanın.

Azure Storage uygulamanızı bir güvenlik sorumlusunun kimlik doğrulama gerçekleştirmeden önce güvenlik sorumlusunu için rol tabanlı erişim denetimi (RBAC) ayarlarını yapılandırın. Azure depolama kapsayıcıları ve Kuyruklar için izinleri kapsayacak RBAC rolleri tanımlar. Bu güvenlik sorumlusu, RBAC rolü için bir güvenlik sorumlusu atandığında, bu kaynağa erişim izni verilir. Daha fazla bilgi için [Yönet RBAC (Önizleme) ile depolama verilere erişim hakları](storage-auth-aad-rbac.md).

OAuth 2.0 kodu verme akışı genel bakış için bkz: [Authorize OAuth 2.0 kod kullanarak Azure Active Directory web uygulamalarına erişim akışı](../../active-directory/develop/v1-protocols-oauth-code.md).

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="register-your-application-with-an-azure-ad-tenant"></a>Azure AD kiracısı ile uygulamanızı kaydetme

Depolama kaynaklarına erişim yetkisi vermek için Azure AD kullanarak ilk adımı, istemci uygulamanız bir Azure AD kiracısında kaydediyor. Uygulamanızı kaydetmek, sağlayan Azure çağırmanızı [Active Directory Authentication Library](../../active-directory/active-directory-authentication-libraries.md) (ADAL) kodunuzdan. ADAL, uygulamanızın Azure AD ile kimlik doğrulaması için bir API sağlar. Uygulamanızı kaydetmek, Azure depolama API'leri, uygulamaya bir erişim belirteci ile çağrılarından yetkilendirmek de sağlar.

Uygulamanızı kaydettiğinizde, Azure AD'ye uygulamanız ile ilgili bilgileri sağlayın. Ardından Azure AD, bir istemci kimliği sağlar (olarak da adlandırılan bir *uygulama kimliği*), uygulamanızın çalışma zamanında Azure AD ile ilişkilendirmek için kullanın. İstemci kimliği hakkında daha fazla bilgi için bkz: [uygulaması ve Azure Active Directory'de Hizmet sorumlusu nesneleri](../../active-directory/develop/app-objects-and-service-principals.md).

Azure Storage uygulamanızı kaydetmek için adımları izleyin. [bir uygulama eklendiğinde](../../active-directory/develop/quickstart-v1-add-azure-ad-app.md) konusundaki [uygulamaları Azure Active Directory ile tümleştirme](../../active-directory/active-directory-integrating-applications.md). Uygulamanızı yerel bir uygulama kaydederseniz, için geçerli bir URI belirtebilirsiniz **yeniden yönlendirme URI'si**. Değerin gerçek bir uç nokta olması gerekmez.

![Storage uygulamanızı Azure AD'ye kaydetme işlemini gösteren ekran görüntüsü](./media/storage-auth-aad-app/app-registration.png)

Uygulamanızı kaydettikten sonra uygulama kimliği (veya istemci kimliği) altında görürsünüz **ayarları**:

![İstemci Kimliğini gösteren ekran görüntüsü](./media/storage-auth-aad-app/app-registration-client-id.png)

Bir uygulamayı Azure AD'ye kaydetme hakkında daha fazla bilgi için bkz. [uygulamaları Azure Active Directory ile tümleştirme](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md). 

## <a name="grant-your-registered-app-permissions-to-azure-storage"></a>Azure depolama için kayıtlı uygulama izinleri verme

Ardından, Azure depolama API'leri çağırmak için uygulama izinleri vermeniz gerekir. Bu adım, uygulamanızı Azure AD ile Azure depolama alanına çağrı yetkilendirmek etkinleştirir.

1. Azure portalının sol taraftaki gezinti bölmesinde **tüm hizmetleri**, araması **uygulama kayıtları**.
2. Önceki adımda oluşturduğunuz kayıtlı uygulama adını arayın.
3. Kayıtlı uygulamanızı seçip tıklayın **ayarları**. İçinde **API erişimi** bölümünden **gerekli izinler**.
4. İçinde **gerekli izinler** dikey penceresinde tıklayın **Ekle** düğmesi.
5. Altında **bir API seçin**, "Azure Depolama'ya yönelik" arayın ve seçin **Azure depolama** sonuçlar listesinden.

    ![Depolama için izinleri gösteren ekran görüntüsü](media/storage-auth-aad-app/registered-app-permissions-1.png)

6. Altında **izinleri seçin**, yanındaki kutuyu işaretleyin **Azure depolamaya erişimi**, tıklatıp **seçin**.
7. **Bitti**’ye tıklayın.

**Gerekli izinler** windows gösterdiğini Azure AD uygulamanız Azure Active Directory ve Azure depolama erişimi olduğunu. İzinleri verilir Azure AD'ye otomatik olarak ilk kaydettiğinizde uygulamanızı Azure AD'ye.

![Uygulama izinleri gösteren ekran görüntüsü kaydetme](media/storage-auth-aad-app/registered-app-permissions-2.png)

## <a name="net-code-example-create-a-block-blob"></a>.NET kod örneği: bir blok blobu oluştur

Örnek kodu bir erişim almak Azure AD'den belirteci gösterilmektedir. Erişim belirteci, belirtilen kullanıcı kimlik doğrulaması yapmak ve sonra bir blok blobu oluşturma isteği yetkilendirmek için kullanılır. Bu örnek çalışabilmesi için önce önceki bölümlerde özetlenen adımları izleyin.

> [!NOTE]
> Bir Azure depolama hesabınızın sahibi olarak, otomatik olarak veri erişim izni atanmaz. Siz açıkça kendiniz bir RBAC rolü için Azure depolama atamanız gerekir. Abonelik, kaynak grubu, depolama hesabı veya kapsayıcı veya kuyruk düzeyinde atayabilirsiniz. 
>
> Örneğin, bir depolama hesabı sahibi olduğunuz ve kendi kullanıcı kimliği altında örnek kodu çalıştırmak için RBAC rolü için Blob verileri katkıda bulunan kendinize atamanız gerekir. Aksi takdirde, blob oluşturmak için çağrı 403 (Yasak) HTTP durum kodu ile başarısız olur. Daha fazla bilgi için [Yönet RBAC (Önizleme) ile depolama verilere erişim hakları](storage-auth-aad-rbac.md).

### <a name="well-known-values-for-authentication-with-azure-ad"></a>Azure AD ile kimlik doğrulaması için iyi bilinen değerler

Bir güvenlik sorumlusu Azure AD ile kimlik doğrulamak için kodunuzda iyi bilinen bazı değerler eklemeniz gerekir.

#### <a name="azure-ad-oauth-endpoint"></a>Azure AD OAuth uç noktası

Temel Azure AD yetkilisi uç noktası için OAuth 2.0 olduğu gibi burada *Kiracı-kimliği* Active Directory Kiracı kimliği (veya dizin kimliği):

`https://login.microsoftonline.com/<tenant-id>/oauth2/token`

Kiracı kimliği, kimlik doğrulaması için kullanılacak Azure AD kiracısı tanımlar. Kiracı Kimliğini almak için özetlenen adımları izleyin. **için Azure Active Directory Kiracı Kimliğinizi öğrenmenin**.

#### <a name="storage-resource-id"></a>Depolama kaynak kimliği

Azure depolama kaynak kimliği, Azure Depolama'ya yönelik isteklerin kimliğini doğrulamak için bir belirteç almak için kullanın:

`https://storage.azure.com/`

### <a name="get-the-tenant-id-for-your-azure-active-directory"></a>İçin Azure Active Directory Kiracı Kimliğinizi alma

Kiracı Kimliğini almak için şu adımları izleyin:

1. Azure portalında Active Directory'nizi seçin.
2. **Özellikler**'e tıklayın.
3. Sağlanan GUID değeri kopyalayın **dizin kimliği**. Bu değer Kiracı kimliği olarak da adlandırılır

![Kiracı kimliği kopyalanmasını gösteren ekran görüntüsü](./media/storage-auth-aad-app/aad-tenant-id.png)

### <a name="add-references-and-using-statements"></a>Başvurular ekleyin ve using deyimleri  

Visual Studio'da Azure depolama istemci kitaplığı önizleme sürümünü yükleyin. Gelen **Araçları** menüsünde **Nuget Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. Konsoldaki için .NET İstemci Kitaplığı'nın en son sürümünü yüklemek için aşağıdaki komutu yazın:

```
Install-Package WindowsAzure.Storage
```

Ayrıca, ADAL'ın en son sürümünü yükleyin:

```
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

Ardından, aşağıdaki ekleyin using deyimlerini kodunuz:

```dotnet
using System.Globalization;
using Microsoft.IdentityModel.Clients.ActiveDirectory; //ADAL client library for getting the access token
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
```

### <a name="get-an-oauth-token-from-azure-ad"></a>Azure AD'den bir OAuth belirteci alma

Ardından, Azure AD'den bir belirteç istediği bir yöntem ekleyin. Belirteç istemek için çağrı [AuthenticationContext.AcquireTokenAsync](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.clients.activedirectory.authenticationcontext.acquiretokenasync) yöntemi. Daha önce uyguladığınız adımlar aşağıdaki değerlerden sahip olduğunuzdan emin olun:

- Kiracısını (dizin) kimliği
- İstemci (uygulama) kimliği
- İstemci yeniden yönlendirme URI'si

```dotnet
static string GetUserOAuthToken()
{
    const string ResourceId = "https://storage.azure.com/";
    const string AuthEndpoint = "https://login.microsoftonline.com/{0}/oauth2/token";
    const string TenantId = "<tenant-id>"; // Tenant or directory ID

    // Construct the authority string from the Azure AD OAuth endpoint and the tenant ID. 
    string authority = string.Format(CultureInfo.InvariantCulture, AuthEndpoint, TenantId);
    AuthenticationContext authContext = new AuthenticationContext(authority);

    // Acquire an access token from Azure AD. 
    AuthenticationResult result = authContext.AcquireTokenAsync(ResourceId, 
                                                                "<client-id>", 
                                                                new Uri(@"<client-redirect-uri>"), 
                                                                new PlatformParameters(PromptBehavior.Auto)).Result;

    return result.AccessToken;
}
```

### <a name="create-the-block-blob"></a>Blok blobu oluştur

Son olarak, yeni depolama kimlik bilgileri oluşturmak için erişim belirteci kullanın ve blob oluşturmak için bu kimlik bilgilerini kullanın:

```dotnet
// Get the access token.
string accessToken = GetUserOAuthToken();

// Use the access token to create the storage credentials.
TokenCredential tokenCredential = new TokenCredential(accessToken);
StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

// Create a block blob using those credentials
CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://storagesamples.blob.core.windows.net/sample-container/Blob1.txt"), storageCredentials);
```

> [!NOTE]
> Azure depolama ile Azure AD tümleştirmesi, Azure depolama işlemleri için HTTPS kullanmanızı gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

- RBAC rolleri için Azure depolama hakkında daha fazla bilgi için bkz: [Yönet RBAC (Önizleme) ile depolama verilere erişim hakları](storage-auth-aad-rbac.md).
- Azure depolama ile Azure kaynakları için yönetilen kimlikleri kullanma hakkında bilgi edinmek için [BLOB'lar ve Kuyruklar Azure ile kimlik doğrulama erişim kimlikleri (Önizleme) Azure kaynakları için yönetilen](storage-auth-aad-msi.md).
- Azure CLI ve PowerShell ile bir Azure AD kimlikleriyle oturum öğrenmek için bkz. [CLI veya PowerShell (Önizleme) ile Azure depolamaya erişmek için bir Azure AD kimlik kullanmak](storage-auth-aad-script.md).
- Azure AD tümleştirmesi için Azure BLOB'ları ve kuyrukları hakkında ek bilgi için bkz: gönderin, Azure depolama ekibi blogu [Önizleme, Azure AD kimlik doğrulaması için Azure depolama ile tanışın](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).



