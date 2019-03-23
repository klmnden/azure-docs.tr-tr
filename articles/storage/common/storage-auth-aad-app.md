---
title: Azure Active Directory ile uygulamalarınızı blob ve kuyruk verilere erişmek için kimlik doğrulaması | Microsoft Docs
description: Bir uygulama içinde kimlik doğrulaması ve ardından BLOB'lar ve Kuyruklar isteklerine yetkilendirmek için Azure Active Directory kullanın.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/21/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: a313061f89d33ee2bf5379dbd37495db06b64440
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369522"
---
# <a name="authenticate-with-azure-active-directory-from-an-application-for-access-to-blobs-and-queues"></a>BLOB'lar ve Kuyruklar için erişim için bir uygulamadan Azure Active Directory kimlik doğrulaması

Azure depolama ile Azure Active Directory (Azure AD) kullanarak önemli bir avantajı, kimlik bilgilerinizi artık kodunuzda depolanması gerektiğini ' dir. Bunun yerine, Azure AD'den bir OAuth 2.0 erişim belirteci isteğinde bulunabilirsiniz. Azure AD güvenlik sorumlusunu (kullanıcı, Grup veya hizmet sorumlusu) kimlik doğrulamasını işleyen çalışan uygulama. Doğrulama başarılı olursa, Azure AD erişim belirteci uygulamaya döndürür ve uygulama daha sonra Azure Depolama'ya yönelik isteklerin yetkilendirmek için erişim belirteci kullanabilirsiniz.

Bu makalede, Azure AD ile kimlik doğrulaması için uygulamanızı yapılandırma gösterilmektedir. Kod örneği özellikleri .NET, ancak diğer diller benzer bir yaklaşım kullanın.

Azure Storage uygulamanızı bir güvenlik sorumlusunun kimlik doğrulama gerçekleştirmeden önce güvenlik sorumlusunu için rol tabanlı erişim denetimi (RBAC) ayarlarını yapılandırın. Azure depolama kapsayıcıları ve Kuyruklar için izinleri kapsayacak RBAC rolleri tanımlar. Bu güvenlik sorumlusu, RBAC rolü için bir güvenlik sorumlusu atandığında, bu kaynağa erişim izni verilir. Daha fazla bilgi için [RBAC ile depolama verilere erişim haklarını yönetme](storage-auth-aad-rbac.md).

OAuth 2.0 kodu verme akışı genel bakış için bkz: [Authorize OAuth 2.0 kod kullanarak Azure Active Directory web uygulamalarına erişim akışı](../../active-directory/develop/v1-protocols-oauth-code.md).

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="assign-an-rbac-role-to-an-azure-ad-security-principal"></a>Bir Azure AD güvenlik sorumlusu için bir RBAC rolü atayın

Azure depolama uygulamanızdan bir güvenlik sorumlusunun kimliğini doğrulamak için önce bu güvenlik sorumlusu için rol tabanlı erişim denetimi (RBAC) ayarlarını yapılandırın. Azure depolama kapsayıcıları ve Kuyruklar için izinleri kapsayacak RBAC rolleri tanımlar. Bu güvenlik sorumlusu, RBAC rolü için bir güvenlik sorumlusu atandığında, bu kaynağa erişim izni verilir. Daha fazla bilgi için [RBAC ile Azure Blob ve kuyruk verilere erişim haklarını yönetme](storage-auth-aad-rbac.md).

## <a name="register-your-application-with-an-azure-ad-tenant"></a>Azure AD kiracısı ile uygulamanızı kaydetme

Depolama kaynaklarına erişim yetkisi vermek için Azure AD kullanarak ilk adımı, istemci uygulamanız bir Azure AD kiracısında kaydediyor. Uygulamanızı kaydetmek, sağlayan Azure çağırmanızı [Active Directory Authentication Library](../../active-directory/active-directory-authentication-libraries.md) (ADAL) kodunuzdan. ADAL, uygulamanızın Azure AD ile kimlik doğrulaması için bir API sağlar. Uygulamanızı kaydetmek, Azure depolama API'leri, uygulamaya bir erişim belirteci ile çağrılarından yetkilendirmek de sağlar.

Uygulamanızı kaydettiğinizde, Azure AD'ye uygulamanız ile ilgili bilgileri sağlayın. Ardından Azure AD, bir istemci kimliği sağlar (olarak da adlandırılan bir *uygulama kimliği*), uygulamanızın çalışma zamanında Azure AD ile ilişkilendirmek için kullanın. İstemci kimliği hakkında daha fazla bilgi için bkz: [uygulaması ve Azure Active Directory'de Hizmet sorumlusu nesneleri](../../active-directory/develop/app-objects-and-service-principals.md).

Azure Storage uygulamanızı kaydetmek için adımları izleyin. [bir uygulama eklendiğinde](../../active-directory/develop/quickstart-v1-add-azure-ad-app.md) konusundaki [uygulamaları Azure Active Directory ile tümleştirme](../../active-directory/active-directory-integrating-applications.md). Uygulamanızı yerel bir uygulama kaydederseniz, için geçerli bir URI belirtebilirsiniz **yeniden yönlendirme URI'si**. Değerin gerçek bir uç nokta olması gerekmez.

![Storage uygulamanızı Azure AD'ye kaydetme gösteren ekran görüntüsü](./media/storage-auth-aad-app/app-registration.png)

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

    ![Depolama için ekran gösterme izinleri](media/storage-auth-aad-app/registered-app-permissions-1.png)

6. Altında **izinleri seçin**, yanındaki kutuyu işaretleyin **Azure depolamaya erişimi**, tıklatıp **seçin**.
7. **Bitti**’ye tıklayın.

**Gerekli izinler** windows gösterdiğini Azure AD uygulamanız Azure Active Directory ve Azure depolama erişimi olduğunu. İzinleri verilir Azure AD'ye otomatik olarak ilk kaydettiğinizde uygulamanızı Azure AD'ye.

![Uygulama izinleri gösteren ekran görüntüsü kaydetme](media/storage-auth-aad-app/registered-app-permissions-2.png)

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

Visual Studio'dan Azure depolama istemci Kitaplığı yükleyin. Gelen **Araçları** menüsünde **Nuget Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. Konsoldaki için .NET İstemci Kitaplığı'nın en son sürümünü yüklemek için aşağıdaki komutu yazın:

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
    const string AuthInstance = "https://login.microsoftonline.com/{0}/";
    const string TenantId = "<tenant-id>"; // Tenant or directory ID

    // Construct the authority string from the Azure AD OAuth endpoint and the tenant ID. 
    string authority = string.Format(CultureInfo.InvariantCulture, AuthInstance, TenantId);
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

blob.UploadTextAsync("Blob created by Azure AD authenticated user.");
```

> [!NOTE]
> Azure depolama ile Azure AD tümleştirmesi, Azure depolama işlemleri için HTTPS kullanmanızı gerektirir.

Yukarıdaki örnekte, blok blobu oluşturmak için isteğin yetkilendirme .NET istemci kitaplığı işler. Diğer depolama istemci kitaplıkları, ayrıca, isteğin yetkilendirme işleyin. Ancak, REST API kullanarak bir OAuth belirteci ile bir Azure depolama işlemi çağırıyorsanız, OAuth belirteci kullanarak isteği yetkilendirmek gerekecektir.   

OAuth erişim belirteçleri kullanarak Blob ve kuyruk hizmet işlemlerini aramak üzere erişim belirteci geçirmek **yetkilendirme** üst bilgisini kullanarak **taşıyıcı** şeması ve bir hizmet sürüm 2017-11-09 veya üzeri olarak belirtin Aşağıdaki örnekte gösterilen:

```
GET /container/file.txt HTTP/1.1
Host: mystorageaccount.blob.core.windows.net
x-ms-version: 2017-11-09
Authorization: Bearer eyJ0eXAiOnJKV1...Xd6j
```

Azure depolama REST işlemlerini yetkilendirme hakkında daha fazla bilgi için bkz. [Azure Active Directory ile kimlik doğrulama](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory).

## <a name="next-steps"></a>Sonraki adımlar

- RBAC rolleri için Azure depolama hakkında daha fazla bilgi için bkz: [RBAC ile depolama verilere erişim haklarını yönetme](storage-auth-aad-rbac.md).
- Azure depolama ile Azure kaynakları için yönetilen kimlikleri kullanma hakkında bilgi edinmek için [BLOB'lar ve Kuyruklar Azure ile kimlik doğrulama erişim kimliklerini Azure kaynakları için yönetilen](storage-auth-aad-msi.md).
- Azure CLI ve PowerShell için bir Azure AD kimlikleriyle oturum açma bilgi edinmek için [CLI veya PowerShell ile Azure depolamaya erişmek için bir Azure AD kimlik kullanmak](storage-auth-aad-script.md).
