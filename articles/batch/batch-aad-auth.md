---
title: Azure Batch hizmeti çözümlerinin kimliğini doğrulamak için Azure Active Directory'yi kullanın. | Microsoft Docs
description: Batch, Batch hizmetinden gelen kimlik doğrulaması için Azure AD destekler.
services: batch
documentationcenter: .net
author: laurenhughes
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
ms.author: lahugh
ms.openlocfilehash: 5cda3f99a263e8eef13ee2e8d8e6453eda0f4cb6
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341185"
---
# <a name="authenticate-batch-service-solutions-with-active-directory"></a>Batch hizmeti çözümlerinin Active Directory ile kimlik doğrulaması

Azure Batch ile kimlik doğrulamasını destekleyen [Azure Active Directory][aad_about] (Azure AD). Azure AD sağlayan bir Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Azure kendisi, müşteriler, hizmet yöneticileri ve kurumsal kullanıcıların kimliğini doğrulamak için Azure AD kullanır.

Azure Batch ile Azure AD kimlik doğrulamasını kullanırken, iki yoldan biriyle kimlik doğrulaması yapabilir:

- Kullanarak **tümleşik kimlik doğrulaması** uygulamayla etkileşim bir kullanıcının kimliğini doğrulamak için. Tümleşik kimlik doğrulaması kullanarak bir uygulama, bir kullanıcının kimlik bilgilerini toplar ve Batch kaynakları kimlik doğrulaması yapmak için bu kimlik bilgilerini kullanır.
- Kullanarak bir **hizmet sorumlusu** katılımsız bir uygulamanın kimlik doğrulaması için. Bir hizmet sorumlusu, uygulamanın çalışma zamanında kaynaklara erişirken temsil etmek için ilke ve uygulama izinlerini tanımlar.

Azure AD hakkında daha fazla bilgi edinmek için [Azure Active Directory belgeleri](https://docs.microsoft.com/azure/active-directory/).

## <a name="endpoints-for-authentication"></a>Kimlik doğrulaması için uç noktaları

Batch uygulamaları Azure AD ile kimlik doğrulamak için bazı iyi bilinen uç noktaları kodunuzda eklemeniz gerekir.

### <a name="azure-ad-endpoint"></a>Azure AD uç noktası

Temel Azure AD yetkilisi uç noktası:

`https://login.microsoftonline.com/`

Azure AD kimlik doğrulaması için bu endpoint Kiracı kimliği (dizin kimliği) ile birlikte kullanın. Kiracı kimliği, kimlik doğrulaması için kullanılacak Azure AD kiracısı tanımlar. Kiracı Kimliğini almak için özetlenen adımları izleyin. [için Azure Active Directory Kiracı Kimliğinizi öğrenmenin](#get-the-tenant-id-for-your-active-directory):

`https://login.microsoftonline.com/<tenant-id>`

> [!NOTE] 
> Kiracıya özgü uç nokta, bir hizmet sorumlusunu kullanarak kimlik doğrulaması sırasında gereklidir. 
> 
> Kiracıya özgü uç nokta, tümleşik kimlik doğrulamasını kullanarak kimlik doğrulaması sırasında isteğe bağlı ancak önerilen değerdir. Ancak, Azure AD genel uç noktası kullanabilirsiniz. Ortak uç nokta, belirli bir kiracıda değil sağlanan arabirim veri toplama genel bir kimlik bilgisi sağlanır. Ortak uç nokta `https://login.microsoftonline.com/common`.
>
>

Azure AD uç noktaları hakkında daha fazla bilgi için bkz. [Azure AD için kimlik doğrulama senaryoları][aad_auth_scenarios].

### <a name="batch-resource-endpoint"></a>Batch kaynak uç noktası

Kullanım **Azure Batch kaynak uç noktası** Batch hizmeti isteklerine kimlik doğrulaması için bir belirteç almak için:

`https://batch.core.windows.net/`

## <a name="register-your-application-with-a-tenant"></a>Bir kiracı ile uygulamanızı kaydetme

Azure AD kimlik doğrulaması kullanarak ilk adımı, uygulamanızı bir Azure AD kiracısında kaydediyor. Uygulamanızı kaydetmek, sağlayan Azure çağırmanızı [Active Directory Authentication Library][aad_adal] (ADAL) kodunuzdan. ADAL, uygulamanızın Azure AD ile kimlik doğrulaması için bir API sağlar. Uygulamanızı kaydetmek tümleşik kimlik doğrulaması veya hizmet sorumlusu kullanmayı planladığınız gereklidir.

Uygulamanızı kaydettiğinizde, Azure AD'ye uygulamanız ile ilgili bilgileri sağlayın. Ardından Azure AD uygulama kimliği sağlar (olarak da adlandırılan bir *istemci kimliği*), uygulamanızın çalışma zamanında Azure AD ile ilişkilendirmek için kullanın. Uygulama kimliği hakkında daha fazla bilgi için bkz: [uygulaması ve Azure Active Directory'de Hizmet sorumlusu nesneleri](../active-directory/develop/app-objects-and-service-principals.md).

Batch uygulamanızı kaydetmek için adımları izleyin. [bir uygulama eklendiğinde](../active-directory/develop/quickstart-register-app.md) konusundaki [uygulamaları Azure Active Directory ile tümleştirme][aad_integrate]. Uygulamanızı yerel bir uygulama kaydederseniz, için geçerli bir URI belirtebilirsiniz **yeniden yönlendirme URI'si**. Gerçek bir uç nokta olması gerekmez.

Uygulamanızı kaydettikten sonra uygulama kimliği görürsünüz:

![Toplu işlem uygulamanızı Azure AD'ye kaydetme](./media/batch-aad-auth/app-registration-data-plane.png)

Bir uygulamayı Azure AD'ye kaydetme hakkında daha fazla bilgi için bkz. [Azure AD için kimlik doğrulama senaryoları](../active-directory/develop/authentication-scenarios.md).

## <a name="get-the-tenant-id-for-your-active-directory"></a>İçin Active Directory Kiracı Kimliğinizi alma

Kiracı kimliği, uygulamanız için kimlik doğrulama hizmetleri sağlayan Azure AD kiracısını tanımlar. Kiracı Kimliğini almak için şu adımları izleyin:

1. Azure portalında Active Directory'nizi seçin.
2. **Özellikler**'e tıklayın.
3. Sağlanan GUID değeri kopyalayın **dizin kimliği**. Bu değer Kiracı kimliği olarak da adlandırılır

![Dizin kimliği kopyalayın](./media/batch-aad-auth/aad-directory-id.png)


## <a name="use-integrated-authentication"></a>Tümleşik kimlik doğrulaması kullan

Tümleşik kimlik doğrulaması ile kimlik doğrulamak için Batch hizmeti API'sine bağlanmak için uygulama izinleri vermeniz gerekir. Bu adım, Azure AD ile Batch hizmeti API çağrıları kimlik doğrulaması uygulamanızı sağlar.

Uygulamanızı kaydettikten sonra Batch hizmetine erişim vermek için Azure portalında aşağıdaki adımları izleyin:

1. Azure portalının sol taraftaki gezinti bölmesinde **tüm hizmetleri**. Tıklayın **uygulama kayıtları**.
2. Listede, uygulamanızın uygulama kayıtları adı arayın:

    ![Uygulamanızın adı arayın](./media/batch-aad-auth/search-app-registration.png)

3. Uygulamayı tıklayarak **ayarları**. İçinde **API erişimi** bölümünden **gerekli izinler**.
4. İçinde **gerekli izinler** dikey penceresinde tıklayın **Ekle** düğmesi.
5. İçinde **bir API seçin**, Batch API'sini arayın. API'yi bulana kadar aşağıdaki dizelerden her birini arayın:
    1. **MicrosoftAzureBatch**.
    2. **Microsoft Azure Batch**. Daha yeni Azure AD kiracıları bu adı kullanıyor olabilir.
    3. Batch API'sinin kimliği: **ddbf3205-c6bd-46ae-8127-60eb93363864**. 
6. Batch API'sini bulduktan sonra seçin ve **seçin**.
7. İçinde **izinleri seçin**, yanındaki onay kutusunu işaretleyin **erişim Azure Batch hizmeti** tıklatıp **seçin**.
8. **Bitti**’ye tıklayın.

**Gerekli izinler** API, Azure AD uygulamanız ADAL hem Batch erişimi olduğunu gösterir. hizmeti şimdi windows. İlk uygulamanızı Azure AD'ye kaydetme izinleri için ADAL otomatik olarak verilir.

![GRANT API izinleri](./media/batch-aad-auth/required-permissions-data-plane.png)

## <a name="use-a-service-principal"></a>Bir hizmet sorumlusu kullanma 

Katılımsız çalışan bir uygulama kimliğini doğrulamak için bir hizmet sorumlusu kullanın. Uygulamanızı kaydettikten sonra bir hizmet sorumlusu yapılandırmak için Azure portalında aşağıdaki adımları izleyin:

1. Uygulamanız için bir gizli anahtar isteyin.
2. Uygulamanıza bir RBAC rolü atayın.

### <a name="request-a-secret-key-for-your-application"></a>İstek, uygulamanız için bir gizli anahtar

Uygulamanızı bir hizmet sorumlusu ile kimlik doğrulamasını gerçekleştirdiğinde, hem uygulama kimliği ve gizli anahtarı Azure AD'ye gönderir. Oluşturun ve kodunuzdan gizli anahtarı kopyalayın gerekecektir.

Azure portalında aşağıdaki adımları izleyin:

1. Azure portalının sol taraftaki gezinti bölmesinde **tüm hizmetleri**. Tıklayın **uygulama kayıtları**.
2. Listede, uygulamanızın uygulama kayıtları adını aratın.
3. Uygulamayı tıklayarak **ayarları**. İçinde **API erişimi** bölümünden **anahtarları**.
4. Bir anahtar oluşturmak için bu anahtar için bir açıklama girin. Ardından bir veya iki yıl anahtarı için bir süre seçin. 
5. Tıklayın **Kaydet** oluşturup anahtarı görüntülemek için düğme. Anahtar değerini güvenli bir yere kopyalayın dikey pencereden ayrıldıktan sonra yeniden erişmek mümkün olmayacaktır. 

    ![Gizli anahtar oluşturma](./media/batch-aad-auth/secret-key.png)

### <a name="assign-an-rbac-role-to-your-application"></a>Uygulamanıza bir RBAC rolü atayın

Bir hizmet sorumlusu ile kimlik doğrulamak için uygulamanıza bir RBAC rolü atamanız gerekir. Şu adımları uygulayın:

1. Azure portalında, uygulamanız tarafından kullanılan Batch hesabına gidin.
2. İçinde **ayarları** select Batch hesabı dikey penceresinde **erişim denetimi (IAM)** .
3. Tıklayın **rolü atamalarını** sekmesi.
4. Tıklayın **rol ataması Ekle** düğmesi. 
5. Gelen **rol** açılan, ya da seçin _katkıda bulunan_ veya _okuyucu_ uygulamanız için rol. Bu roller hakkında daha fazla bilgi için bkz. [Azure portalında rol tabanlı erişim denetimi ile çalışmaya başlama](../role-based-access-control/overview.md).  
6. İçinde **seçin** uygulamanızın adını girin. Listeden uygulama seçin ve tıklayın **Kaydet**.

Uygulamanız, artık bir RBAC rolü atanmış erişim denetimi ayarlarınızı görüntülenmelidir. 

![Uygulamanıza bir RBAC rolü atayın](./media/batch-aad-auth/app-rbac-role.png)

### <a name="get-the-tenant-id-for-your-azure-active-directory"></a>İçin Azure Active Directory Kiracı Kimliğinizi alma

Kiracı kimliği, uygulamanız için kimlik doğrulama hizmetleri sağlayan Azure AD kiracısını tanımlar. Kiracı Kimliğini almak için şu adımları izleyin:

1. Azure portalında Active Directory'nizi seçin.
2. **Özellikler**'e tıklayın.
3. Sağlanan GUID değeri kopyalayın **dizin kimliği**. Bu değer Kiracı kimliği olarak da adlandırılır

![Dizin kimliği kopyalayın](./media/batch-aad-auth/aad-directory-id.png)


## <a name="code-examples"></a>Kod örnekleri

Bu bölümdeki kod örnekleri, bir hizmet sorumlusu ile tümleşik kimlik doğrulaması kullanarak Azure AD ile kimlik doğrulaması yapma gösterilmektedir. Bu kod örnekleri çoğu .NET kullanın, ancak diğer diller için benzer bir kavramlardır.

> [!NOTE]
> Bir Azure AD kimlik doğrulama belirtecini bir saat sonra süresi dolar. Uzun süreli bir kullanırken **BatchClient** nesnesi, geçerli bir belirteç her zaman olduğundan emin olmak için her istek İOS'tan bir belirteç almak öneririz. 
>
>
> . NET'te bunun için Azure AD'den belirteci alır bir yöntem yazmaktır ve bu yönteme geçirin bir **BatchTokenCredentials** temsilci olarak nesnesi. Geçerli bir belirteç sağlandığından emin olmak için her istek Batch hizmetine temsilci yöntemi çağrılır. Azure AD'den alınan yeni bir belirteç için varsayılan olarak ADAL belirteç önbelleğe alır. yalnızca gerektiğinde. Azure AD'de belirteçleri hakkında daha fazla bilgi için bkz. [Azure AD için kimlik doğrulama senaryoları][aad_auth_scenarios].
>
>

### <a name="code-example-using-azure-ad-integrated-authentication-with-batch-net"></a>Kod örneği: Tümleşik Batch .NET ile Azure AD kullanarak kimlik doğrulaması

Batch .NET tümleşik kimlik doğrulaması ile kimlik doğrulaması için başvuru [Azure Batch .NET](https://www.nuget.org/packages/Microsoft.Azure.Batch/) paket ve [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) paket.

Şunlar `using` kodunuzu deyimlerinde:

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Azure AD uç noktası'Kiracı kimliği de dahil olmak üzere, kodunuzda başvurusu Kiracı Kimliğini almak için özetlenen adımları izleyin. [için Azure Active Directory Kiracı Kimliğinizi öğrenmenin](#get-the-tenant-id-for-your-active-directory):

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

Başvuru Batch hizmeti kaynak uç noktası:

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

Batch hesabınıza başvuru:

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

Uygulamanız için uygulama kimliği (istemci kimliği) belirtin. Uygulama kimliği, Azure portalında uygulama kaydınızı edinilebilir:

```csharp
private const string ClientId = "<application-id>";
```

Ayrıca, uygulamanızın yerel bir uygulama kayıtlı yeniden yönlendirme URI'si, belirttiğiniz kopyalayın. Yeniden yönlendirme URI'si kodunuzda belirtilen yeniden yönlendirme URI'si uygulama kaydolurken sağladığınız eşleşmesi gerekir:

```csharp
private const string RedirectUri = "http://mybatchdatasample";
```

Azure ad kimlik doğrulama belirteci almak için bir geri çağırma yöntemi yazın. **GetAuthenticationTokenAsync** uygulamayla etkileşim bir kullanıcı kimlik doğrulaması için ADAL çağrıları burada gösterilen geri çağırma yöntemi. **AcquireTokenAsync** (zaten kimlik bilgilerini önbelleğe sürece) kullanıcı bunları sağlar sonra ADAL tarafından sağlanan yöntemi ister kullanıcıdan kimlik bilgilerini ve uygulama işlemi devam eder:

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

Oluşturmak bir **BatchTokenCredentials** temsilcinin parametre olarak alan nesne. Bu kimlik bilgilerini kullanarak bir **BatchClient** nesne. Bu hesabı kullanabilirsiniz **BatchClient** nesne Batch hizmetinden sonraki işlemleri için:

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

### <a name="code-example-using-an-azure-ad-service-principal-with-batch-net"></a>Kod örneği: Bir Azure AD hizmet sorumlusu ile Batch .NET kullanma

Batch .NET gelen hizmet sorumlusu kimlik doğrulaması yapmak için başvuru [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) paket ve [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) paket.

Şunlar `using` kodunuzu deyimlerinde:

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Azure AD uç noktası'Kiracı kimliği de dahil olmak üzere, kodunuzda başvurusu Bir hizmet sorumlusunu kullanırken bir kiracıya özgü uç noktası sağlamanız gerekir. Kiracı Kimliğini almak için özetlenen adımları izleyin. [için Azure Active Directory Kiracı Kimliğinizi öğrenmenin](#get-the-tenant-id-for-your-active-directory):

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

Başvuru Batch hizmeti kaynak uç noktası:  

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

Batch hesabınıza başvuru:

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

Uygulamanız için uygulama kimliği (istemci kimliği) belirtin. Uygulama kimliği, Azure portalında uygulama kaydınızı edinilebilir:

```csharp
private const string ClientId = "<application-id>";
```

Azure Portalı'ndan kopyaladığınız gizli anahtarını belirtin:

```csharp
private const string ClientKey = "<secret-key>";
```

Azure ad kimlik doğrulama belirteci almak için bir geri çağırma yöntemi yazın. **GetAuthenticationTokenAsync** katılımsız kimlik doğrulaması için ADAL çağrıları burada gösterilen geri çağırma yöntemi:

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
    AuthenticationResult authResult = await authContext.AcquireTokenAsync(BatchResourceUri, new ClientCredential(ClientId, ClientKey));

    return authResult.AccessToken;
}
```

Oluşturmak bir **BatchTokenCredentials** temsilcinin parametre olarak alan nesne. Bu kimlik bilgilerini kullanarak bir **BatchClient** nesne. Ardından, kullanan **BatchClient** nesne Batch hizmetinden sonraki işlemleri için:

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
### <a name="code-example-using-an-azure-ad-service-principal-with-batch-python"></a>Kod örneği: Bir Azure AD hizmet sorumlusu ile Batch Python kullanma

Batch Python gelen hizmet sorumlusu kimlik doğrulaması yapmak için yükleme ve başvuru [azure-batch](https://pypi.org/project/azure-batch/) ve [azure ortak](https://pypi.org/project/azure-common/) modüller.


```python
from azure.batch import BatchServiceClient
from azure.common.credentials import ServicePrincipalCredentials
```

Bir hizmet sorumlusunu kullanırken, Kiracı kimliğini sağlamanız gerekir Kiracı Kimliğini almak için özetlenen adımları izleyin. [için Azure Active Directory Kiracı Kimliğinizi öğrenmenin](#get-the-tenant-id-for-your-active-directory):

```python
TENANT_ID = "<tenant-id>"
```

Başvuru Batch hizmeti kaynak uç noktası:  

```python
RESOURCE = "https://batch.core.windows.net/"
```

Batch hesabınıza başvuru:

```python
BATCH_ACCOUNT_URL = "https://myaccount.mylocation.batch.azure.com"
```

Uygulamanız için uygulama kimliği (istemci kimliği) belirtin. Uygulama kimliği, Azure portalında uygulama kaydınızı edinilebilir:

```python
CLIENT_ID = "<application-id>"
```

Azure Portalı'ndan kopyaladığınız gizli anahtarını belirtin:

```python
SECRET = "<secret-key>"
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

Açmak için hizmet sorumlusu kimlik bilgileri kullanın. bir **BatchServiceClient** nesne. Ardından, kullanan **BatchServiceClient** Batch hizmetinden sonraki işlemleri için nesne.

```python
    batch_client = BatchServiceClient(
    credentials,
    base_url=BATCH_ACCOUNT_URL
)
```

## <a name="next-steps"></a>Sonraki adımlar

* Azure AD hakkında daha fazla bilgi edinmek için [Azure Active Directory belgeleri](https://docs.microsoft.com/azure/active-directory/). ADAL kullanan gösteren ayrıntılı örnekler kullanılabilir [Azure Kod örnekleri](https://azure.microsoft.com/resources/samples/?service=active-directory) kitaplığı.

* Hizmet sorumluları hakkında daha fazla bilgi için bkz. [uygulaması ve Azure Active Directory'de Hizmet sorumlusu nesneleri](../active-directory/develop/app-objects-and-service-principals.md). Azure portalını kullanarak bir hizmet sorumlusu oluşturmak için bkz: [Active Directory kaynaklarına erişmek uygulama ve hizmet sorumlusu oluşturmak için portalı kullanma](../active-directory/develop/howto-create-service-principal-portal.md). Ayrıca, PowerShell veya Azure CLI ile hizmet sorumlusu oluşturabilirsiniz.

* Batch yönetimi uygulamaları Azure AD kullanarak kimlik doğrulaması yapmak için bkz: [Active Directory ile kimlik doğrulaması Batch yönetimi çözümleri](batch-aad-auth-management.md).

* Bir Azure AD belirtecini kullanarak kimlik doğrulaması Batch istemcisi oluşturma Python örneği için bkz. [Azure Batch özel görüntü dağıtımı ile bir Python betiği](https://github.com/azurebigcompute/recipes/blob/master/Azure%20Batch/CustomImages/CustomImagePython.md) örnek.

[aad_about]:../active-directory/fundamentals/active-directory-whatis.md "Azure Active Directory nedir?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Azure AD için kimlik doğrulama senaryoları"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Uygulamaları Azure Active Directory ile tümleştirme"
[azure_portal]: https://portal.azure.com
