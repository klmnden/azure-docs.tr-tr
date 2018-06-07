---
title: Bir depolama uygulamasından (Önizleme) Azure AD ile kimlik doğrulaması | Microsoft Docs
description: Azure Storage uygulamadan (Önizleme) Azure AD ile kimlik doğrulaması.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 05/18/2018
ms.author: tamram
ms.openlocfilehash: 1bf4a8bba3b93c16f67d46f65292709ef2a1bba2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661011"
---
# <a name="authenticate-with-azure-ad-from-an-azure-storage-application-preview"></a>Bir Azure Storage uygulamasını (Önizleme) Azure AD ile kimlik doğrulaması

Azure Storage ile Azure Active Directory (Azure AD) kullanmanın önemli bir avantajı, kimlik bilgilerinizi artık kodunuzda depolanması gerektiğini ' dir. Bunun yerine, Azure AD'den bir OAuth 2.0 erişim belirteci isteyebilirsiniz. Azure AD kimlik doğrulaması için güvenlik sorumlusu (kullanıcı, Grup veya hizmet sorumlusu) işleme uygulamayı çalıştırmayı. Doğrulama başarılı olursa, Azure AD erişim belirteci uygulamaya döndürür ve uygulama daha sonra Azure depolama isteklerine yetkilendirmek için erişim belirteci kullanabilirsiniz.

Bu makalede, Azure AD ile uygulamanızı kimlik doğrulaması için yapılandırma gösterilmektedir. Kod örneği özellikleri .NET, ancak diğer diller benzer bir yaklaşım kullanın.

Azure Storage uygulamanızı bir güvenlik sorumlusunun kimliğini doğrulamadan önce o güvenlik sorumlusu için rol tabanlı erişim denetimi (RBAC) ayarlarını yapılandırın. Azure depolama kapsayıcıları ve Kuyruklar izinlerini kapsayan RBAC rolleri tanımlar. RBAC rolü için bir güvenlik sorumlusu atandığında, bu güvenlik sorumlusu bu kaynağa erişim izni verilir. Daha fazla bilgi için bkz: [RBAC (Önizleme) depolama verilerle Yönet erişim hakları](storage-auth-aad-rbac.md).

OAuth 2.0 kod grant akışı genel bakış için bkz: [Authorize OAuth 2.0 kodunu kullanarak Azure Active Directory web uygulamaları için erişim akış](../../active-directory/develop/active-directory-protocols-oauth-code.md).

> [!IMPORTANT]
> Bu önizleme yalnızca üretim dışı kullanılması amaçlanmıştır. Azure AD tümleştirmesi için Azure Storage genel kullanıma bildirilmiş kadar üretim hizmet düzeyi sözleşmelerine (SLA) kullanılamaz. Azure AD tümleştirme senaryonuz için henüz desteklenmiyorsa paylaşılan anahtar yetkilendirme veya SAS belirteci uygulamalarınızı kullanmaya devam. ' Nin önizlemesi hakkında ek bilgi için bkz: [Azure Active Directory'yi (Önizleme) kullanarak Azure Storage erişimi kimlik doğrulaması](storage-auth-aad.md).
>
> Önizleme sırasında RBAC rolü atamalarını yayılması beş dakika kadar sürebilir.

## <a name="register-your-application-with-an-azure-ad-tenant"></a>Azure AD kiracısı ile uygulamanızı kaydetme

Depolama kaynaklarına erişim yetkisi vermek için Azure AD kullanmanın ilk adımı, istemci uygulamanızı Azure AD kiracısı içinde kaydediyor. Uygulamanızı kaydetme, sağlayan Azure çağırabilmeniz için [Active Directory kimlik doğrulama Kitaplığı](../../active-directory/active-directory-authentication-libraries.md) (ADAL) kodunuzdan. ADAL, uygulamanızdan Azure AD ile kimlik doğrulaması için bir API sağlar. Uygulamanızı kaydetme, Azure depolama API'leri bu uygulamaya bir erişim belirteci ile gelen çağrıları yetkilendirmek de sağlar.

Uygulamanızı kaydederken, Azure AD'ye uygulamanız ile ilgili bilgileri sağlayın. Ardından Azure AD sağlayan bir istemci kimliği (olarak da adlandırılan bir *uygulama kimliği*) çalışma zamanında Azure AD ile uygulamanızı ilişkilendirmek için kullanın. İstemci kimliği hakkında daha fazla bilgi için bkz: [uygulama ve hizmet asıl nesneler Azure Active Directory'de](../../active-directory/develop/active-directory-application-objects.md).

Azure Storage uygulamanızı kaydetmek için adımları [bir uygulama ekleme](../../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) bölümüne [uygulamaları Azure Active Directory ile tümleştirme](../../active-directory/active-directory-integrating-applications.md). Yerel bir uygulama olarak, uygulamanızın kaydederseniz için geçerli bir URI belirtebilirsiniz **yeniden yönlendirme URI'si**. Değerin gerçek bir uç nokta olması gerekmez.

![Storage uygulamanızı Azure AD ile kaydetmek nasıl gösteren ekran görüntüsü](./media/storage-auth-aad-app/app-registration.png)

Uygulamanızı kaydınız sonra uygulama kimliği (veya istemci kimliği) altında göreceğiniz **ayarları**:

![İstemci kimliği gösteren ekran görüntüsü](./media/storage-auth-aad-app/app-registration-client-id.png)

Uygulama Azure AD ile kaydetme hakkında daha fazla bilgi için bkz: [uygulamaları Azure Active Directory ile tümleştirme](../../active-directory/develop/active-directory-integrating-applications.md). 

## <a name="grant-your-registered-app-permissions-to-azure-storage"></a>Azure depolama alanına, kayıtlı uygulama izinleri

Ardından, Azure depolama API'leri çağırmak için uygulama izinleri vermeniz gerekir. Bu adım, Azure AD ile Azure depolama çağrılarını yetkilendirmek uygulamanızı sağlar.

1. Azure portalının sol gezinti bölmesinde seçin **tüm hizmetleri**, arayın ve **uygulama kayıtlar**.
2. Önceki adımda oluşturduğunuz kayıtlı uygulamanın adını arayın.
3. Kayıtlı uygulamanızı seçin ve'ı tıklatın **ayarları**. İçinde **API erişimini** bölümünde, select **gerekli izinleri**.
4. İçinde **gerekli izinleri** dikey penceresinde tıklatın **Ekle** düğmesi.
5. Altında **bir API seçin**, "Azure Storage" için arama yapın ve seçin **Azure Storage** sonuçları listesinden.

    ![Depolama izinlerini gösteren ekran görüntüsü](media/storage-auth-aad-app/registered-app-permissions-1.png)

6. Altında **seçin izinleri**, yanındaki kutuyu işaretleyin **erişim Azure Storage**, tıklatıp **seçin**.
7. **Bitti**’ye tıklayın.

**Gerekli izinler** windows şimdi gösterir, Azure AD uygulama Azure Active Directory ve Azure Storage erişimi olduğunu. İzinler verildikten Azure AD ile otomatik olarak ilk kaydettiğinizde, uygulamanızın Azure AD ile.

![Uygulama izinleri gösteren ekran görüntüsü kaydetme](media/storage-auth-aad-app/registered-app-permissions-2.png)

## <a name="net-code-example-create-a-block-blob"></a>.NET kodu örneği: blok blobu oluşturma

Kod örneği bir erişmek Azure AD'den belirteci gösterilmiştir. Erişim belirteci, belirtilen kullanıcının kimliğini doğrulamak ve sonra bir blok blobu oluşturma isteği yetkilendirmek için kullanılır. Bu örnek çalışabilmesi için önce önceki bölümlerde açıklanan adımları izleyin.

> [!NOTE]
> Azure Storage hesabınızı sahibi olarak, otomatik olarak verilere erişim izni atanmaz. Açıkça kendiniz RBAC rolü için Azure Storage atamanız gerekir. Abonelik, kaynak grubu, depolama hesabı veya kapsayıcı veya sıra düzeyinde atayabilirsiniz. 
>
> Örneğin, örnek kod bir sahip olduğu bir depolama hesabı ve kullanıcı kimliğinizi altında çalıştırmak için RBAC rolü Blob veri katkıda bulunan için kendinize atamanız gerekir. Aksi takdirde, blob oluşturma çağrısı HTTP durum kodu 403 (Yasak) ile başarısız olur. Daha fazla bilgi için bkz: [RBAC (Önizleme) depolama verilerle Yönet erişim hakları](storage-auth-aad-rbac.md).

### <a name="well-known-values-for-authentication-with-azure-ad"></a>Azure AD ile kimlik doğrulaması için bilinen değerleri

Azure AD ile bir güvenlik sorumlusunun kimliğini doğrulamak için iyi bilinen bazı değerler kodunuzda eklemeniz gerekir.

#### <a name="azure-ad-oauth-endpoint"></a>Azure AD OAuth uç noktası

Temel Azure AD yetkili uç nokta için OAuth 2.0 gibidir, burada *Kiracı kimliği* Active Directory Kiracı kimliği (veya dizin kimliği):

`https://login.microsoftonline.com/<tenant-id>/oauth2/token`

Kiracı kimliği kimlik doğrulaması için kullanılacak Azure AD kiracısı tanımlar. Kiracı Kimliği almak için özetlenen adımları izleyin **için Azure Active Directory Kiracı Kimliğinizi alma**.

#### <a name="storage-resource-id"></a>Depolama kaynak kimliği

Azure depolama isteklerine kimlik doğrulaması için bir belirteç almak için Azure Storage kaynak kimliği kullanın:

`https://storage.azure.com/`

### <a name="get-the-tenant-id-for-your-azure-active-directory"></a>İçin Azure Active Directory Kiracı Kimliğinizi alma

Kiracı Kimliği almak için şu adımları izleyin:

1. Azure portalında Active Directory'nizi seçin.
2. **Özellikler**'e tıklayın.
3. Sağlanan GUID değeri kopyalama **dizin kimliği**. Bu değer Kiracı kimliği olarak da adlandırılır

![Kiracı kimliği kopyalamak nasıl gösteren ekran görüntüsü](./media/storage-auth-aad-app/aad-tenant-id.png)

### <a name="add-references-and-using-statements"></a>Başvurular ekleyin ve using deyimleri  

Visual Studio'da Azure Storage istemci kitaplığı önizleme sürümünü yükleyin. Gelen **Araçları** menüsünde, select **Nuget Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. Konsola aşağıdaki komutu yazın:

```
Install-Package https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0  
```

Ardından, aşağıdaki ekleyin kodunuzu deyimlerini kullanarak:

```dotnet
using Microsoft.IdentityModel.Clients.ActiveDirectory; //ADAL client library for getting the access token
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
```

### <a name="get-an-oauth-token-from-azure-ad"></a>Azure AD'den bir OAuth belirteci alma

Ardından, Azure AD'den bir belirteç isteklerini bir yöntem ekleyin. Belirteç istemek için çağrı [AuthenticationContext.AcquireTokenAsync](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.clients.activedirectory.authenticationcontext.acquiretokenasync) yöntemi.

```dotnet
static string GetUserOAuthToken()
{
    const string ResourceId = "https://storage.azure.com/"; // Storage resource endpoint
    const string AuthEndpoint = "https://login.microsoftonline.com/{0}/oauth2/token"; // Azure AD OAuth endpoint
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

### <a name="create-the-block-blob"></a>Blok blobu oluşturma

Son olarak, yeni depolama kimlik bilgileri oluşturmak için erişim belirtecini kullanır ve blob oluşturmak için bu kimlik bilgilerini kullanın:

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
> Azure Storage ile Azure AD tümleştirme Azure depolama işlemleri için HTTPS kullanmanızı gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure depolama için RBAC rolleri hakkında daha fazla bilgi için bkz: [RBAC (Önizleme) depolama verilerle Yönet erişim hakları](storage-auth-aad-rbac.md).
- Yönetilen hizmet kimliği Azure Storage ile kullanma hakkında bilgi edinmek için bkz: [bir Azure yönetilen hizmet kimliği (Önizleme) Azure AD ile kimlik doğrulama](storage-auth-aad-msi.md).
- Azure CLI ve PowerShell ile bir Azure AD kimlik günlüğe kaydetme hakkında bilgi edinmek için [CLI veya PowerShell (Önizleme) ile Azure depolama alanına erişmek için bir Azure AD kimlik kullanmak](storage-auth-aad-script.md).
- Azure depolama ekibi blog gönderisi, Azure BLOB'ları ve kuyrukları için Azure AD tümleştirme hakkında ek bilgi için bkz: [Azure depolama için Azure Önizleme AD Authentication Duyurusu](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).



