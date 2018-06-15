---
title: Azure Batch hizmeti çözümleri kimlik doğrulaması için Azure Active Directory kullanmak | Microsoft Docs
description: Batch Batch hizmetinde kimlik doğrulaması için Azure AD destekler.
services: batch
documentationcenter: .net
author: dlepow
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/18/2018
ms.author: danlep
ms.openlocfilehash: c28af5a9773cc362663831346b58f599aed6ea9a
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31789274"
---
# <a name="authenticate-batch-service-solutions-with-active-directory"></a>Toplu hizmet çözümlerine Active Directory ile kimlik doğrulaması

Azure Batch destekleyen kimlik doğrulama ile [Azure Active Directory] [ aad_about] (Azure AD). Azure AD, Microsoft'un çok kiracılı bulut tabanlı dizin ve Kimlik Yönetimi Hizmeti ' dir. Azure kendisini müşteriler, hizmet yöneticileri ve kuruluş kullanıcılarının kimliğini doğrulamak için Azure AD kullanır.

Azure AD kimlik doğrulaması ile Azure Batch kullanırken, aşağıdaki iki yöntemden biriyle doğrulayabilir:

- Kullanarak **tümleşik kimlik doğrulaması** uygulama ile etkileşim bir kullanıcının kimliğini doğrulamak için. Tümleşik kimlik doğrulaması kullanarak bir uygulama, bir kullanıcının kimlik bilgilerini toplar ve toplu kaynaklarına erişimde kimlik doğrulaması için bu kimlik bilgilerini kullanır.
- Kullanarak bir **hizmet sorumlusu** katılımsız uygulama kimliğini doğrulamak için. Bir hizmet sorumlusu uygulamanın çalışma zamanında kaynaklara erişirken gösterebilmek için bir uygulama için izinler ve ilke tanımlar.

Azure AD hakkında daha fazla bilgi için bkz: [Azure Active Directory belgeleri](https://docs.microsoft.com/azure/active-directory/).

## <a name="endpoints-for-authentication"></a>Kimlik doğrulaması için uç noktalar

Toplu işlem uygulamaları Azure AD ile kimlik doğrulamak için bazı iyi bilinen uç noktaları kodunuzda eklemeniz gerekir.

### <a name="azure-ad-endpoint"></a>Azure AD uç noktası

Temel Azure AD yetkilisi uç noktası:

`https://login.microsoftonline.com/`

Azure AD ile kimlik doğrulamak için bu uç noktaya Kiracı kimliği (dizin kimliği) ile birlikte kullanın. Kiracı kimliği kimlik doğrulaması için kullanılacak Azure AD kiracısı tanımlar. Kiracı Kimliği almak için özetlenen adımları izleyin [için Azure Active Directory Kiracı Kimliğinizi alma](#get-the-tenant-id-for-your-active-directory):

`https://login.microsoftonline.com/<tenant-id>`

> [!NOTE] 
> Bir hizmet sorumlusunu kullanarak kimlik doğrulaması sırasında Kiracı özgü uç noktası gereklidir. 
> 
> Tümleşik kimlik doğrulaması kullanarak kimlik doğrulaması sırasında isteğe bağlıdır, ancak önerilen Kiracı özgü uç noktadır. Ancak, Azure AD ortak uç nokta de kullanabilirsiniz. Ortak bir uç belirli bir kiracı sağlanmadığında arabirimi toplama genel bir kimlik bilgisi sağlanır. Ortak uç noktası `https://login.microsoftonline.com/common`.
>
>

Azure AD uç noktaları hakkında daha fazla bilgi için bkz: [Azure AD için kimlik doğrulama senaryoları][aad_auth_scenarios].

### <a name="batch-resource-endpoint"></a>Batch kaynak uç noktası

Kullanım **Azure Batch kaynak endpoint** Batch hizmeti isteklerine kimlik doğrulaması için bir belirteç almak için:

`https://batch.core.windows.net/`

## <a name="register-your-application-with-a-tenant"></a>Bir kiracı ile uygulamanızı kaydetme

Kimlik doğrulaması için Azure AD kullanmanın ilk adımı uygulamanız bir Azure AD kiracısında kaydediyor. Uygulamanızı kaydetme, sağlayan Azure çağırabilmeniz için [Active Directory kimlik doğrulama Kitaplığı] [ aad_adal] (ADAL) kodunuzdan. ADAL, uygulamanızdan Azure AD ile kimlik doğrulaması için bir API sağlar. Uygulamanızı kaydetme tümleşik kimlik doğrulaması veya bir hizmet sorumlusu kullanmayı planladığınız gereklidir.

Uygulamanızı kaydederken, Azure AD'ye uygulamanız ile ilgili bilgileri sağlayın. Ardından Azure AD uygulama kimliği sağlar (olarak da adlandırılan bir *istemci kimliği*) çalışma zamanında Azure AD ile uygulamanızı ilişkilendirmek için kullanın. Uygulama kimliği hakkında daha fazla bilgi için bkz: [uygulama ve hizmet asıl nesneler Azure Active Directory'de](../active-directory/develop/active-directory-application-objects.md).

Batch uygulamanızı kaydetmek için adımları [bir uygulama ekleme](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) bölümüne [uygulamaları Azure Active Directory ile tümleştirme][aad_integrate]. Yerel bir uygulama olarak, uygulamanızın kaydederseniz için geçerli bir URI belirtebilirsiniz **yeniden yönlendirme URI'si**. Gerçek bir uç nokta olması gerekmez.

Uygulamanızı kaydınız sonra uygulama kimliği görürsünüz:

![Batch uygulamanızı Azure AD ile kaydetme](./media/batch-aad-auth/app-registration-data-plane.png)

Uygulama Azure AD ile kaydetme hakkında daha fazla bilgi için bkz: [Azure AD için kimlik doğrulama senaryoları](../active-directory/develop/active-directory-authentication-scenarios.md).

## <a name="get-the-tenant-id-for-your-active-directory"></a>İçin Active Directory Kiracı Kimliğinizi alma

Kiracı kimliği uygulamanız için kimlik doğrulama hizmetleri sağlayan Azure AD kiracısı tanımlar. Kiracı Kimliği almak için şu adımları izleyin:

1. Azure portalında Active Directory'nizi seçin.
2. **Özellikler**'e tıklayın.
3. Sağlanan GUID değeri kopyalama **dizin kimliği**. Bu değer Kiracı kimliği olarak da adlandırılır

![Dizin Kimliğini kopyalayın](./media/batch-aad-auth/aad-directory-id.png)


## <a name="use-integrated-authentication"></a>Tümleşik kimlik doğrulamasını kullan

Tümleşik kimlik doğrulaması ile kimlik doğrulaması için Batch hizmeti API'sine bağlanmak için uygulama izinleri vermeniz gerekir. Bu adım, Azure AD ile Batch hizmeti API çağrıları kimliğini doğrulamak, uygulamanızı sağlar.

Seçtiğiniz sonra [uygulamanızı kayıtlı](#register-your-application-with-an-azure-ad-tenant), Batch hizmeti erişim vermek için Azure Portalı'nda aşağıdaki adımları izleyin:

1. Azure portalının sol gezinti bölmesinde seçin **tüm hizmetleri**. Tıklatın **uygulama kayıtlar**.
2. Listedeki uygulamanızın uygulama kayıtların adını arayın:

    ![Uygulama adınız arayın](./media/batch-aad-auth/search-app-registration.png)

3. Uygulama ve tıklayın **ayarları**. İçinde **API erişimini** bölümünde, select **gerekli izinleri**.
4. İçinde **gerekli izinleri** dikey penceresinde tıklatın **Ekle** düğmesi.
5. İçinde **bir API seçin**, Batch API'sini arayın. API'yi bulana kadar aşağıdaki dizelerden her birini arayın:
    1. **MicrosoftAzureBatch**.
    2. **Microsoft Azure Batch**. Daha yeni Azure AD kiracıları bu adı kullanıyor olabilir.
    3. Batch API'sinin kimliği: **ddbf3205-c6bd-46ae-8127-60eb93363864**. 
6. Toplu işlem API bulduğunuzda, seçin ve **seçin**.
7. İçinde **seçin izinleri**, yanındaki onay kutusunu işaretleyin **erişim Azure Batch hizmeti** tıklatıp **seçin**.
8. **Bitti**’ye tıklayın.

**Gerekli izinler** API gösterir, Azure AD uygulaması erişimi ADAL ve toplu işlem hizmeti artık windows. Uygulamanızı Azure AD ile ilk kez kaydederken izinler için ADAL otomatik olarak verilir.

![GRANT API izinleri](./media/batch-aad-auth/required-permissions-data-plane.png)

## <a name="use-a-service-principal"></a>Bir hizmet sorumlusu kullanın 

Katılımsız çalışan bir uygulama kimliğini doğrulamak için bir hizmet sorumlusu kullanın. Uygulamanızı kaydınız sonra Azure portalında bir hizmet sorumlusu yapılandırmak için aşağıdaki adımları izleyin:

1. Uygulamanız için gizli bir anahtar isteyin.
2. Uygulamanıza bir RBAC rolü atayın.

### <a name="request-a-secret-key-for-your-application"></a>Uygulamanız için gizli bir anahtarı iste

Uygulamanızın hizmet sorumlusu ile doğruladığında, Azure AD uygulama kimliği ve gizli bir anahtar gönderir. Oluşturun ve kodunuzdan gizli anahtarı kopyalayın gerekir.

Azure portalında aşağıdaki adımları izleyin:

1. Azure portalının sol gezinti bölmesinde seçin **tüm hizmetleri**. Tıklatın **uygulama kayıtlar**.
2. Listedeki uygulamanızın uygulama kayıtların adını arayın.
3. Uygulama ve tıklayın **ayarları**. İçinde **API erişimini** bölümünde, select **anahtarları**.
4. Bir anahtar oluşturmak için bu anahtar için bir açıklama girin. Ardından bir veya iki yıl anahtarı için bir süre seçin. 
5. Tıklatın **kaydetmek** oluşturmak ve anahtarı görüntülemek için düğmesi. Anahtar değeri dikey penceresinde çıktıktan sonra yeniden erişebilir açamazsınız güvenli bir yere kopyalayın. 

    ![Gizli bir anahtar oluşturun](./media/batch-aad-auth/secret-key.png)

### <a name="assign-an-rbac-role-to-your-application"></a>Uygulamanıza bir RBAC rolü atayın

Hizmet sorumlusu ile kimlik doğrulaması yapmak için uygulamanızı bir RBAC rolü atamanız gerekir. Şu adımları uygulayın:

1. Uygulamanız tarafından kullanılan toplu işlem hesabı için Azure portalında gidin.
2. İçinde **ayarları** select toplu işlem hesabı için dikey **erişim denetimi (IAM)**.
3. **Ekle** düğmesine tıklayın. 
4. Gelen **rol** açılan listesinde, ya da seçin _katkıda bulunan_ veya _okuyucu_ rolü uygulamanız için. Bu rolleri hakkında daha fazla bilgi için bkz: [Azure portalında rol tabanlı erişim denetimi ile çalışmaya başlama](../role-based-access-control/overview.md).  
5. İçinde **seçin** alanında, uygulamanızın adı girin. Uygulamanızı listeden seçin ve'ı tıklatın **kaydetmek**.

Uygulamanız artık bir RBAC rolü atanmış erişim denetimi ayarlarınızda görünmelidir. 

![Uygulamanıza bir RBAC rolü atayın](./media/batch-aad-auth/app-rbac-role.png)

### <a name="get-the-tenant-id-for-your-azure-active-directory"></a>İçin Azure Active Directory Kiracı Kimliğinizi alma

Kiracı kimliği uygulamanız için kimlik doğrulama hizmetleri sağlayan Azure AD kiracısı tanımlar. Kiracı Kimliği almak için şu adımları izleyin:

1. Azure portalında Active Directory'nizi seçin.
2. **Özellikler**'e tıklayın.
3. Sağlanan GUID değeri kopyalama **dizin kimliği**. Bu değer Kiracı kimliği olarak da adlandırılır

![Dizin Kimliğini kopyalayın](./media/batch-aad-auth/aad-directory-id.png)


## <a name="code-examples"></a>Kod örnekleri

Bu bölümdeki kod örnekleri, bir hizmet sorumlusu ile tümleşik kimlik doğrulaması kullanarak Azure AD ile kimlik doğrulaması yapmayı gösterir. Bu kod örnekleri çoğu .NET kullanın, ancak kavramları diğer diller için benzerdir.

> [!NOTE]
> Azure AD kimlik doğrulama belirtecini bir saat sonra süresi dolar. Bir uzun süreli kullanırken **BatchClient** nesne, geçerli bir belirteci her zaman sahip emin olmak için her istek ADAL bir belirteç almak öneririz. 
>
>
> .NET ile bunun için Azure AD'den belirteç alan bir yöntem yazmak ve bu yönteme geçirin bir **BatchTokenCredentials** temsilci olarak nesne. Temsilci yöntemi geçerli bir belirteci sağlandığından emin olmak için her istek Batch hizmeti için çağrılır. Yeni bir belirteç Azure AD'den alınan için varsayılan olarak ADAL belirteçleri, önbelleğe alır. yalnızca gerekli olduğunda. Azure AD'de belirteçleri hakkında daha fazla bilgi için bkz: [Azure AD için kimlik doğrulama senaryoları][aad_auth_scenarios].
>
>

### <a name="code-example-using-azure-ad-integrated-authentication-with-batch-net"></a>Kod örneği: Batch .NET ile tümleşik kimlik doğrulaması Azure AD kullanma

Batch .NET tümleşik kimlik doğrulamasında kullanılacak başvuru [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) paketi ve [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) paket.

Aşağıdakiler dahil `using` deyimlerinde kodunuzu:

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Kiracı kimliğini de dahil olmak üzere, kodunuzu Azure AD uç başvurusu Kiracı Kimliği almak için özetlenen adımları izleyin [için Azure Active Directory Kiracı Kimliğinizi alma](#get-the-tenant-id-for-your-active-directory):

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

Başvuru Batch hizmeti kaynak uç noktası:

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

Batch hesabınıza başvurusu:

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

Uygulamanız için uygulama kimliği (istemci kimliği) belirtin. Uygulama kimliği, Azure portalında uygulama kaydınızı edinilebilir:

```csharp
private const string ClientId = "<application-id>";
```

Ayrıca yerel bir uygulama olarak, uygulamanızın kaydolduysanız yeniden yönlendirme, belirttiğiniz URI'sini kopyalayın. URI kodunuzda belirtilen yeniden yönlendirme yeniden yönlendirilen uygulamayı kaydolurken sağladığınız URI eşleşmesi gerekir:

```csharp
private const string RedirectUri = "http://mybatchdatasample";
```

Azure AD'den kimlik doğrulama belirtecini almak üzere geri arama yöntemi yazın. **GetAuthenticationTokenAsync** uygulama ile etkileşim bir kullanıcı kimlik doğrulaması için ADAL çağrıları burada gösterilen geri çağırma yöntemi. **AcquireTokenAsync** ADAL tarafından sağlanan yöntemi (Bu zaten kimlik bilgilerini önbelleğe sürece) kullanıcı bunları sağlar sonra kullanıcıdan kimlik bilgilerini ve uygulama kazançlar ister:

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    var authContext = new AuthenticationContext(AuthorityUri);

    // Acquire the authentication token from Azure AD.
    var authResult = await authContext.AcquireTokenAsync(BatchResourceUri, 
                                                        ClientId, 
                                                        new Uri(RedirectUri), 
                                                        new PlatformParameters(PromptBehavior.Auto));

    return authResult.AccessToken;
}
```

Oluşturmak bir **BatchTokenCredentials** temsilcisi bir parametre olarak alan nesne. Açmak için bu kimlik bilgilerini kullanan bir **BatchClient** nesnesi. Kullanan **BatchClient** nesne Batch hizmeti karşı sonraki işlemleri için:

```csharp
public static async Task PerformBatchOperations()
{
    Func<Task<string>> tokenProvider = () => GetAuthenticationTokenAsync();

    using (var client = await BatchClient.OpenAsync(new BatchTokenCredentials(BatchAccountUrl, tokenProvider)))
    {
        await client.JobOperations.ListJobs().ToListAsync();
    }
}
```

### <a name="code-example-using-an-azure-ad-service-principal-with-batch-net"></a>Kod örneği: Azure AD hizmet sorumlusu Batch .NET ile kullanma

Bir hizmet sorumlusu Batch .NET gelen kimlik doğrulaması yapmak için başvuru [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) paketi ve [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) paket.

Aşağıdakiler dahil `using` deyimlerinde kodunuzu:

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Kiracı kimliğini de dahil olmak üzere, kodunuzu Azure AD uç başvurusu Bir hizmet sorumlusu kullanırken, bir kiracı özel uç noktası sağlamanız gerekir. Kiracı Kimliği almak için özetlenen adımları izleyin [için Azure Active Directory Kiracı Kimliğinizi alma](#get-the-tenant-id-for-your-active-directory):

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

Başvuru Batch hizmeti kaynak uç noktası:  

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

Batch hesabınıza başvurusu:

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

Uygulamanız için uygulama kimliği (istemci kimliği) belirtin. Uygulama kimliği, Azure portalında uygulama kaydınızı edinilebilir:

```csharp
private const string ClientId = "<application-id>";
```

Azure portalından kopyalandığından gizli anahtarı belirtin:

```csharp
private const string ClientKey = "<secret-key>";
```

Azure AD'den kimlik doğrulama belirtecini almak üzere geri arama yöntemi yazın. **GetAuthenticationTokenAsync** katılımsız kimlik doğrulaması için ADAL çağrıları burada gösterilen geri çağırma yöntemi:

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
    AuthenticationResult authResult = await authContext.AcquireTokenAsync(BatchResourceUri, new ClientCredential(ClientId, ClientKey));

    return authResult.AccessToken;
}
```

Oluşturmak bir **BatchTokenCredentials** temsilcisi bir parametre olarak alan nesne. Açmak için bu kimlik bilgilerini kullanan bir **BatchClient** nesnesi. Ardından, kullanan **BatchClient** nesne Batch hizmeti karşı sonraki işlemleri için:

```csharp
public static async Task PerformBatchOperations()
{
    Func<Task<string>> tokenProvider = () => GetAuthenticationTokenAsync();

    using (var client = await BatchClient.OpenAsync(new BatchTokenCredentials(BatchAccountUrl, tokenProvider)))
    {
        await client.JobOperations.ListJobs().ToListAsync();
    }
}
```
### <a name="code-example-using-an-azure-ad-service-principal-with-batch-python"></a>Kod örneği: Azure AD hizmet sorumlusu Batch Python ile kullanma

Bir hizmet sorumlusu Batch Python gelen kimlik doğrulaması yapmak için yükleme ve başvuru [azure-batch](https://pypi.org/project/azure-batch/) ve [azure ortak](https://pypi.org/project/azure-common/) modüller.


```python
from azure.batch import BatchServiceClient
from azure.common.credentials import ServicePrincipalCredentials
```

Bir hizmet sorumlusu kullanırken, Kiracı kimliğini sağlamalıdır Kiracı Kimliği almak için özetlenen adımları izleyin [için Azure Active Directory Kiracı Kimliğinizi alma](#get-the-tenant-id-for-your-active-directory):

```python
TENANT_ID = "<tenant-id>";
```

Başvuru Batch hizmeti kaynak uç noktası:  

```python
RESOURCE = "https://batch.core.windows.net/";
```

Batch hesabınıza başvurusu:

```python
BATCH_ACCOUNT_URL = "https://myaccount.mylocation.batch.azure.com";
```

Uygulamanız için uygulama kimliği (istemci kimliği) belirtin. Uygulama kimliği, Azure portalında uygulama kaydınızı edinilebilir:

```python
CLIENT_ID = "<application-id>";
```

Azure portalından kopyalandığından gizli anahtarı belirtin:

```python
SECRET = "<secret-key>";
```

Oluşturma bir **ServicePrincipalCredentials** nesnesi:

```python
credentials = ServicePrincipalCredentials(
    client_id=CLIENT_ID,
    secret=SECRET,
    tenant=TENANT_ID,
    resource=RESOURCE
)
```

Açmak için hizmet asıl kimlik bilgilerini kullanan bir **BatchServiceClient** nesnesi. Ardından, kullanan **BatchServiceClient** Batch hizmeti karşı sonraki işlemleri için nesne.

```python
    batch_client = BatchServiceClient(
    credentials,
    base_url=BATCH_ACCOUNT_URL
)
```

## <a name="next-steps"></a>Sonraki adımlar

* Azure AD hakkında daha fazla bilgi için bkz: [Azure Active Directory belgeleri](https://docs.microsoft.com/azure/active-directory/). ADAL'nin kullanımı gösteren ayrıntılı örnekler kullanılabilir [Azure Kod örnekleri](https://azure.microsoft.com/resources/samples/?service=active-directory) kitaplığı.

* Hizmet sorumluları hakkında daha fazla bilgi için bkz: [uygulama ve hizmet asıl nesneler Azure Active Directory'de](../active-directory/develop/active-directory-application-objects.md). Azure Portalı'nı kullanarak bir hizmet sorumlusu oluşturmak için bkz: [Active Directory kaynaklara erişebilir uygulama ve hizmet sorumlusu oluşturmak için kullanım portal](../resource-group-create-service-principal-portal.md). Bir hizmet sorumlusu, PowerShell veya Azure CLI ile de oluşturabilirsiniz.

* Azure AD kullanarak toplu yönetimini uygulama kimliğini doğrulamak için bkz: [Active Directory ile kimlik doğrulaması toplu yönetim çözümleri](batch-aad-auth-management.md).

* Bir Azure AD belirteci kullanarak kimlik doğrulaması toplu istemci nasıl oluşturulacağı Python örneği için bkz [Python betiği ile Azure toplu işlem özel görüntüsünü dağıtma](https://github.com/azurebigcompute/recipes/blob/master/Azure%20Batch/CustomImages/CustomImagePython.md) örnek.

[aad_about]: ../active-directory/active-directory-whatis.md "Azure Active Directory nedir?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Azure AD için kimlik doğrulama senaryoları"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Uygulamaları Azure Active Directory ile tümleştirme"
[azure_portal]: http://portal.azure.com
