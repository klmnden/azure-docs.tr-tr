---
title: Grafik API'sini - Azure AD B2C kullanın | Microsoft Docs
description: İşlemini otomatikleştirmek için bir uygulama kimliği kullanarak bir B2C Kiracı için grafik API'sini çağırmak nasıl.
services: active-directory-b2c
documentationcenter: .net
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 08/07/2017
ms.author: davidmu
ms.openlocfilehash: 2f95df26abcd2c0d5b62c395f92c359170d6d701
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-ad-b2c-use-the-azure-ad-graph-api"></a>Azure AD B2C: Azure AD Graph API kullanın

>[!NOTE]
> Kullanmalısınız [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview?f=255&MSPPError=-2147217396) Azure AD B2C dizini kullanıcıları yönetme. Bu, Microsoft Graph API'SİNDEN farklıdır. [Burada](https://blogs.msdn.microsoft.com/aadgraphteam/2016/07/08/microsoft-graph-or-azure-ad-graph/) daha fazla bilgi edinin.

Azure Active Directory (Azure AD) B2C kiracılar çok büyük olma eğilimindedir. Bu, çok sayıda genel kiracı yönetim görevlerini programlı olarak yapılması gereken anlamına gelir. Kullanıcı Yönetimi buna birincil bir örnektir. B2C Kiracı için var olan bir kullanıcı deposunun geçirmeniz gerekebilir. Kullanıcı kaydı kendi sayfasında barındırmak ve arka planda Azure AD B2C dizininizde kullanıcı hesapları oluşturmak isteyebilirsiniz. Bu tür bir görev oluşturma, okuma, güncelleştirme gerektirir ve kullanıcı hesaplarını silin. Azure AD grafik API'sini kullanarak bu görevleri yapabilirsiniz.

B2C kiracılar için grafik API'si ile iletişim iki birincil modu mevcuttur.

* Görevleri gerçekleştirdiğinizde etkileşimli, bir kez çalıştır görevler için bir yönetici hesabı B2C Kiracı olarak işlem yapmalıdır. Bu modu yönetici, yönetici grafik API'si yapılan her çağrı gerçekleştirmeden önce kimlik bilgileriyle oturum gerektirir.
* Otomatik, sürekli görevleri için bazı yönetim görevlerini gerçekleştirmek için gerekli ayrıcalıklara sağladığınız hizmet hesabı türünü kullanmanız gerekir. Azure AD'de uygulama kaydetme ve Azure AD ile kimlik doğrulaması tarafından bunu yapabilirsiniz. Bu kullanılarak yapılır bir **uygulama kimliği** kullanan [OAuth 2.0 istemci kimlik bilgileri vermenizi](../active-directory/develop/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api). Bu durumda, uygulama grafik API'sini çağırmak için bir kullanıcı değil, olarak kendisini olarak görev yapar.

Bu makalede, biz otomatik kullanım örneğini gerçekleştirmek nasıl ele alacağız. Göstermek için size bir .NET 4.5 yapı `B2CGraphClient` gerçekleştiren kullanıcı oluşturma, okuma, güncelleştirme ve Sil (CRUD) işlemleridir. İstemci, bir Windows komut satırı çeşitli yöntemlerini çağırmasına izin veren arabirimi (CLI) sahip olacaktır. Bununla birlikte, kod etkileşimsiz, otomatik bir şekilde davranmasına yazılır.

## <a name="get-an-azure-ad-b2c-tenant"></a>Bir Azure AD B2C kiracısı edinme
Uygulama veya kullanıcıların oluşturabilir veya Azure AD ile etkileşimde önce Azure AD B2C Kiracı ve Kiracı genel Yönetici hesabında gerekir. Zaten bir kiracı yoksa [Azure AD B2C ile çalışmaya başlama](active-directory-b2c-get-started.md).

## <a name="register-your-application-in-your-tenant"></a>Kiracınızda uygulamanızı kaydetme
B2C Kiracı aldıktan sonra aracılığıyla uygulamanızı kaydetmeniz gerekir [Azure Portal](https://portal.azure.com).

> [!IMPORTANT]
> Grafik API'si B2C kiracınızın kullanmak için genel kullanarak özel bir uygulamayı kaydetme gerekecektir *uygulama kayıtlar* Azure Portal menüsünde **değil** Azure AD B2C'in  *Uygulamaları* menüsü. Azure AD B2C'de 's kayıtlı zaten varolan B2C uygulamaları tekrar kullanamazsınız *uygulamaları* menüsü.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sayfanın sağ üst köşesinde hesabınızı seçerek Azure AD B2C kiracınızın seçin.
3. Sol Gezinti Bölmesi'nde seçin **tüm hizmetleri**, tıklatın **uygulama kayıtlar**, tıklatıp **Ekle**.
4. Komut istemlerini izleyin ve yeni bir uygulama oluşturun. 
    1. Seçin **Web uygulaması / API** uygulama türü olarak.    
    2. Sağlamak **URI herhangi bir yeniden yönlendirme** (örneğin https://B2CGraphAPI) Bu örnek için uygun değil olarak.  
5. Uygulama şimdi yukarı uygulamalar listesinde göster elde etmek için **uygulama kimliği** (istemci kimliği olarak da bilinir). Bir sonraki bölümde gereksinim duyacağınız kopyalayın.
6. Ayarlar menüsünde tıklatın **anahtarları** ve yeni bir anahtar (istemci gizli anahtarı olarak da bilinir) ekleyin. Ayrıca daha sonraki bir bölüme kullanmak için kopyalayın.

## <a name="configure-create-read-and-update-permissions-for-your-application"></a>Yapılandırma oluşturmak, okumak ve güncelleştirmek, uygulamanız için izinler
Şimdi oluşturma, okuma, güncelleştirme ve kullanıcıları silmek için gerekli tüm izinleri almak için uygulamanızı yapılandırmanız gerekir.

1. Azure portal'ın uygulama kayıtlar menüde devam etmeden, uygulamanızı seçin.
2. Ayarlar menüsünde tıklatın **gerekli izinleri**.
3. Gerekli izinleri menüde tıklayın **Windows Azure Active Directory**.
4. Erişimi etkinleştir menüde seçin **dizin verilerini okuma ve yazma** iznini **uygulama izinleri** tıklatıp **kaydetmek**.
5. Gerekli izinleri menüsünde geri, son olarak, tıklayın **izinler** düğmesi.

Artık oluşturmak, okumak ve B2C kiracınızın kullanıcılardan güncelleştirmek için izni olan bir uygulama var.

> [!NOTE]
> Tam olarak işlemek için birkaç dakika sürebilir izni veriliyor izinleri olun.
> 
> 

## <a name="configure-delete-permissions-for-your-application"></a>Uygulamanız için silme izinleri yapılandırma
Şu anda *dizin verilerini okuma ve yazma* izin vermez **değil** kullanıcıları silme gibi tüm silme işlemleri yapmak için özelliğini içerir. Uygulamanız kullanıcıların silme olanağı vermek istiyorsanız, PowerShell ile ilgili aşağıdaki ek adımları yapmanız gerekir, aksi halde, sonraki bölüme atlayabilirsiniz.

İlk olarak, zaten yüklü yoksa, yükleme [Azure AD PowerShell v1 Modülü (MSOnline)](https://docs.microsoft.com/powershell/azure/active-directory/install-msonlinev1?view=azureadps-1.0):

```powershell
Install-Module MSOnline
```

Yükledikten sonra PowerShell modülü Azure AD B2C kiracınızın bağlayın.

> [!IMPORTANT]
> Bir B2C Kiracı yönetici hesabı kullanmanız gerekir **yerel** B2C kiracısına. Bu hesapları şuna: myusername@myb2ctenant.onmicrosoft.com.

```powershell
Connect-MsolService
```

Kullanacağız artık **uygulama kimliği** uygulama kullanıcıları silmek için olanak tanıyan kullanıcı hesabı yönetici rolü atamak için aşağıdaki komut dosyasında. Tüm yapmanız gereken giriş bu rolleri iyi bilinen tanımlayıcıları sahip, bu nedenle, **uygulama kimliği** aşağıdaki betikte yer.

```powershell
$applicationId = "<YOUR_APPLICATION_ID>"
$sp = Get-MsolServicePrincipal -AppPrincipalId $applicationId
Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId $sp.ObjectId -RoleMemberType servicePrincipal
```

Uygulamanızı şimdi de B2C kiracınızın kullanıcıları silme izni vardır.

## <a name="download-configure-and-build-the-sample-code"></a>Karşıdan yükleme, yapılandırma ve örnek kod derleme
İlk olarak, örnek kodu indirin ve çalışmasını alın. Ardından biz yakından bakmak sürer.  Yapabilecekleriniz [örnek kod bir .zip dosyası olarak karşıdan](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip). Ayrıca tercih ettiğiniz dizinine kopyalayabilirsiniz:

```cmd
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

Açık `B2CGraphClient\B2CGraphClient.sln` Visual Studio çözümü Visual Studio'da. İçinde `B2CGraphClient` projesi, dosyayı açma `App.config`. Üç uygulama ayarlarını kendi değerlerinizle değiştirin:

```xml
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{The ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{The Key from above}" />
</appSettings>
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Ardından, sağ tıklayın `B2CGraphClient` çözüm ve örnek yeniden oluşturma. Başarılı olursa, şimdi olmalıdır bir `B2C.exe` içinde bulunan yürütülebilir dosyasını `B2CGraphClient\bin\Debug`.

## <a name="build-user-crud-operations-by-using-the-graph-api"></a>Grafik API'sini kullanarak yapı kullanıcı CRUD işlemleri
B2CGraphClient kullanmak için açık bir `cmd` Windows komut istemine ve dizininize değiştirmek `Debug` dizin. Ardından çalıştırın `B2C Help` komutu.

```cmd
cd B2CGraphClient\bin\Debug
B2C Help
```

Bu her komut kısa bir açıklamasını görüntüler. Aşağıdaki komutlardan birini çağırma her zaman `B2CGraphClient` Azure AD grafik API'sine isteğinde bulunur.

### <a name="get-an-access-token"></a>Bir erişim belirteci alma
Grafik API'si için herhangi bir istek kimlik doğrulaması için bir erişim belirteci gerektirir. `B2CGraphClient` erişim belirteci almak için açık kaynak Active Directory Authentication Library (ADAL) kullanır. ADAL belirteç edinme basit bir API sağlayarak ve erişim belirteçleri önbelleğe alma gibi bazı önemli ayrıntıları alma verdiğiniz kolaylaştırır. Belirteçleri, ancak almak için ADAL kullanmanız gerekmez. Ayrıca, HTTP isteklerini hazırlayın tarafından belirteç alabilir.

> [!NOTE]
> Bu kod örneği ADAL v2 grafik API'si ile iletişim kurmak için kullanır.  Azure AD grafik API'si ile kullanılabilen erişim belirteçleri almak için ADAL v2 veya v3 kullanılması gerekir.
> 
> 

Zaman `B2CGraphClient` çalıştığında, bir örneğini oluşturur `B2CGraphClient` sınıfı. Bu sınıf oluşturucusu ADAL kimlik doğrulaması iskele kurma ayarlar:

```csharp
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // The client_id, client_secret, and tenant are provided in Program.cs, which pulls the values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // The AuthenticationContext is ADAL's primary class, in which you indicate the tenant to use.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // The ClientCredential is where you pass in your client_id and client_secret, which are
    // provided to Azure AD in order to receive an access_token by using the app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

Kullanacağız `B2C Get-User` bir örnek olarak komutu. Zaman `B2C Get-User` , CLI aramaları gibi ek tüm girişleri çağrılan `B2CGraphClient.GetAllUsers(...)` yöntemi. Bu yöntemi çağırır `B2CGraphClient.SendGraphGetRequest(...)`, grafik API'si için bir HTTP GET isteği gönderir. Önce `B2CGraphClient.SendGraphGetRequest(...)` gönderir GET isteği, onu önce bir erişim ADAL kullanarak belirtecini alır:

```csharp
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

Access grafik API'si için ADAL çağırarak belirteci alma `AuthenticationContext.AcquireToken(...)` yöntemi. Ardından ADAL döndüren bir `access_token` , uygulamanın kimliğini temsil eder.

### <a name="read-users"></a>Kullanıcıların okuma
Kullanıcıların bir listesini almak veya belirli bir kullanıcı grafik API'SİNDEN elde etmek istediğinizde, bir HTTP gönderebilirsiniz `GET` isteği `/users` uç noktası. Tüm Kiracı kullanıcılar için bir istek şöyle görünür:

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Bu istek görmek için çalıştırın:

 ```cmd
 B2C Get-User
 ```

Dikkat edilecek iki önemli noktalar şunlardır:

* ADAL edinilen erişim belirteci eklenen `Authorization` kullanarak üstbilgi `Bearer` düzeni.
* B2C kiracılar için sorgu parametresi kullanmalısınız `api-version=1.6`.

Bu ayrıntıları her ikisi de işlenir `B2CGraphClient.SendGraphGetRequest(...)` yöntemi:

```csharp
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure to use the 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append the access token for the Graph API to the Authorization header of the request by using the Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a>Tüketici kullanıcı hesapları oluşturun
B2C kiracınızda kullanıcı hesapları oluştururken, bir HTTP gönderebilirsiniz `POST` isteği `/users` uç noktası:

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required to create consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier the user uses to sign in to the account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set to 'LocalAccount'
    "displayName": "Joe Consumer",                // a value that can be used for displaying to the end user
    "mailNickname": "joec",                        // an email alias for the user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set to false
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

Bu isteği bu özelliklerin çoğu tüketici kullanıcıları oluşturmak için gereklidir. Daha fazla bilgi edinmek için tıklayın [burada](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser). Unutmayın `//` açıklamaları çizim için dahil edilmiştir. Bunları, gerçek bir istekte dahil etmeyin.

İstek görmek için aşağıdaki komutlardan birini çalıştırın:

```cmd
B2C Create-User ..\..\..\usertemplate-email.json
B2C Create-User ..\..\..\usertemplate-username.json
```

`Create-User` Komutu bir .json dosyası olarak bir giriş parametresi alır. Bu, bir kullanıcı nesnesinin bir JSON temsili içerir. Örnek kodda iki örnek .json dosyası vardır: `usertemplate-email.json` ve `usertemplate-username.json`. Bu dosyalar, gereksinimlerinize uyacak şekilde değiştirebilirsiniz. Yukarıdaki gerekli alanlara ek olarak, bu dosyaları kullanabileceğiniz birkaç isteğe bağlı alanları dahil edilir. İsteğe bağlı alanları ayrıntıları bulunabilir [Azure AD Graph API varlık başvurusu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).

POST isteğini nasıl oluşturulur görebilirsiniz `B2CGraphClient.SendGraphPostRequest(...)`.

* Bir erişim belirteci iliştirir `Authorization` isteği üstbilgisi.
* Bu ayarlar `api-version=1.6`.
* Bu JSON kullanıcı nesnesi istek gövdesinde içerir.

> [!NOTE]
> Varolan bir kullanıcı mağazadan geçirmek istediğiniz hesapları varsa, daha düşük parola gücünü [Azure AD B2C tarafından zorlanan güçlü parola gücünü](https://msdn.microsoft.com/library/azure/jj943764.aspx), güçlü parola kullanma gereksinimini devre dışı bırakabilirsiniz `DisableStrongPassword` değer `passwordPolicies` özelliği. Örneğin, yukarıdaki gibi sağlanan oluşturma Kullanıcı isteği değiştirebilirsiniz: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.
> 
> 

### <a name="update-consumer-user-accounts"></a>Tüketici kullanıcı hesaplarını güncelleştirme
Kullanıcı nesneleri güncelleştirdiğinizde, kullanıcı nesneleri oluşturmak için kullandığınız bir benzer bir işlemdir. Ancak bu işlem HTTP kullanan `PATCH` yöntemi:

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",                // this request updates only the user's displayName
}
```

Bir kullanıcı yeni verilerle, JSON dosyalarınızın güncelleştirerek güncelleştirmeyi deneyin. Daha sonra kullanabilirsiniz `B2CGraphClient` aşağıdaki komutlardan birini çalıştırmak için:

```cmd
B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

İnceleme `B2CGraphClient.SendGraphPatchRequest(...)` bu isteği göndermek hakkında ayrıntılar için yöntem.

### <a name="search-users"></a>Kullanıcılarda arama
B2C kiracınızda çeşitli şekillerde kullanıcılar için arama yapabilirsiniz. Kullanıcının kullanarak bir nesne kimliği veya kullanıcının oturum açma tanımlayıcı kullanarak iki (yani, `signInNames` özelliği).

Belirli bir kullanıcı için aramak için aşağıdaki komutlardan birini çalıştırın:

```cmd
B2C Get-User <user-object-id>
B2C Get-User <filter-query-expression>
```

Aşağıda birkaç örnek verilmiştir:

```cmd
B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a>Kullanıcıları silme
Bir kullanıcının silinmesi için basit bir işlemdir. HTTP kullanmak `DELETE` yöntemi ve yapı doğru URL'nin nesne kimliği:

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Bir örnek görmek için şu komutu girin ve konsola yazdırılır silme isteği görüntüleyin:

```cmd
B2C Delete-User <object-id-of-user>
```

İnceleme `B2CGraphClient.SendGraphDeleteRequest(...)` bu isteği göndermek hakkında ayrıntılar için yöntem.

Azure AD grafik API'si kullanıcı yönetimine ek olarak birçok başka eylemler gerçekleştirebilir. [Azure AD Graph API Başvurusu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) örnek istekleri yanı sıra her bir eylemde ayrıntıları sağlar.

## <a name="use-custom-attributes"></a>Özel öznitelikler kullanma
Çoğu tüketici uygulamaları herhangi bir tür özel kullanıcı profili bilgilerini depolamak gerekir. Bunu yapmak için bir yolu, özel bir öznitelik B2C kiracınızda tanımlamaktır. Sonra bu öznitelik bir kullanıcı nesnesi üzerinde herhangi bir özellik işle aynı şekilde davranabilirsiniz. Öznitelik güncelleştirmek, öznitelik Sil, özniteliğin sorgu, öznitelik oturum açma belirteçleri ve daha fazla talep olarak gönder.

Özel bir öznitelik B2C kiracınızda tanımlamak için bkz: [B2C özel öznitelik başvurusu](active-directory-b2c-reference-custom-attr.md).

B2C kiracınızda kullanarak tanımlanan özel özniteliklere görüntüleyebilirsiniz `B2CGraphClient`:

```cmd
B2C Get-B2C-Application
B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

Bu işlevler çıktısı gibi her özel öznitelik ayrıntılarını ortaya çıkarır:

```json
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

Tam adı gibi kullanabilir `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, kullanıcı nesneleri bir özellik olarak.  Yeni özellik ve özelliği için bir değer ile .json dosyanızı güncelleştirin ve ardından çalıştırın:

```cmd
B2C Update-User <object-id-of-user> <path-to-json-file>
```

Kullanarak `B2CGraphClient`, B2C Kiracı kullanıcılarınızın program aracılığıyla yönetebilen bir hizmet uygulaması sahip. `B2CGraphClient` Azure AD grafik API'sine kimliğini doğrulamak için kendi uygulama kimliğini kullanır. Ayrıca, istemci parolasını kullanarak belirteçleri da alır. Bu işlev uygulamanıza dahil, B2C uygulamalar için birkaç önemli nokta unutmayın:

* Uygulama doğru Kiracı izinleri gerekir.
* Şimdilik, erişim belirteçleri almak için ADAL (MSAL değil) kullanmanız gerekir. (Ayrıca protokol iletilerini doğrudan bir kitaplık kullanılarak olmadan gönderebilirsiniz.)
* Grafik API'si çağırdığınızda, kullanabilirsiniz `api-version=1.6`.
* Oluşturduğunuzda ve tüketici kullanıcıları güncelleştirmek birkaç özelliği yukarıda açıklandığı gibi gereklidir.

Herhangi bir sorunuz veya Eylemler B2C kiracınızın grafik API'sini kullanarak gerçekleştirmek istediğiniz isteklerinde varsa, bu makalede bir yorum yazın veya bir sorunu GitHub kod örnek deposunda dosya.

