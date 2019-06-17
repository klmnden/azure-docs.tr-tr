---
title: Azure Active Directory B2C, Graph API'sini kullanma | Microsoft Docs
description: İşlemini otomatikleştirmek için bir uygulama kimliğini kullanarak bir B2C kiracısı için Graph API'sini çağırmak nasıl.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/07/2017
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 88b1d05a47f4a8267ab936a922ac190a925bd5ba
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66510172"
---
# <a name="azure-ad-b2c-use-the-azure-ad-graph-api"></a>Azure AD B2C: Azure AD Graph API’sini kullanma

>[!NOTE]
> Kullanmalısınız [Azure AD Graph API'si](/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-operations-overview) kullanıcıların bir Azure AD B2C dizini yönetmek için. Bu, Microsoft Graph API'den farklıdır. [Burada](https://blogs.msdn.microsoft.com/aadgraphteam/2016/07/08/microsoft-graph-or-azure-ad-graph/) daha fazla bilgi edinin.

Azure Active Directory (Azure AD) B2C kiracıları, çok büyük olma eğilimindedir. Bu, birçok genel kiracı yönetim görevlerini programlı bir şekilde gerçekleştirilmesi gerektiği anlamına gelir. Kullanıcı Yönetimi birincil örnektir. Var olan bir kullanıcı deposu B2C kiracısına geçirmeniz gerekebilir. Kullanıcı kayıt sayfasında, kendi ana bilgisayar ve kullanıcı hesaplarını arka planda, Azure AD B2C dizini oluşturmak isteyebilirsiniz. Bu tür görevleri oluşturabilir, okuyabilir, güncelleştirebilir olanağına sahip olmalıdır ve kullanıcı hesapları silebilirsiniz. Azure AD Graph API'sini kullanarak bu görevler gerçekleştirebilirsiniz.

B2C kiracıları için Graph API ile iletişim birincil iki mod vardır.

* Görevleri gerçekleştirdiğinizde etkileşimli ve bir kez çalıştır görevlerde, B2C kiracısında yönetici hesabı olarak davranmalıdır. Bu mod, bu yönetici için Graph API çağrıları gerçekleştirmek için önce kimlik bilgileriyle oturum açmak bir yönetici gerektirir.
* Otomatik, sürekli görevleri için herhangi bir türde yönetim görevlerini gerçekleştirmek için gerekli ayrıcalıklara sağladığınız hizmet hesabı kullanmanız gerekir. Azure AD'de, uygulama kaydetme ve Azure AD ile kimlik doğrulaması yapabilirsiniz. Bu kullanılarak yapılır bir **uygulama kimliği** kullanan [OAuth 2.0 istemci kimlik bilgileri verme](../active-directory/develop/service-to-service.md). Bu durumda, uygulamanın kendisi, Graph API'sini çağırmak için bir kullanıcı değil, olarak olarak görev yapar.

Bu makalede, otomatik kullanım örneğini gerçekleştirmeyi öğrenin. .NET 4.5 oluşturacaksınız `B2CGraphClient` gerçekleştiren kullanıcı oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri. İstemci, bir Windows komut satırı çeşitli yöntemlerini çağırmasına izin veren arabirimi (CLI) gerekir. Ancak, kod etkileşimsiz, otomatik bir şekilde davranmasına yazılır.

## <a name="get-an-azure-ad-b2c-tenant"></a>Bir Azure AD B2C kiracısı edinme
Uygulamalara veya kullanıcılara oluşturabilmeniz için önce bir Azure AD B2C kiracısı gerekir. Zaten bir kiracı yoksa [Azure AD B2C ile çalışmaya başlama](active-directory-b2c-get-started.md).

## <a name="register-your-application-in-your-tenant"></a>Kiracınızda uygulamanızı kaydetme
Bir B2C kiracısına sahip sonra kullanarak uygulamanızı kaydetmek gereken [Azure portalında](https://portal.azure.com).

> [!IMPORTANT]
> Graph API B2C kiracınızı kullanmak için bir uygulama kullanarak kaydetmeniz gerekir *uygulama kayıtları* Azure portalında service **değil** Azure AD B2C'in *uygulamaları*menüsü. Aşağıdaki yönergeler size uygun bir menüye yol. Azure AD B2C'de kullanıcının kayıtlı mevcut B2C uygulamaları yeniden kullanılamaz *uygulamaları* menüsü.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sayfanın sağ üst köşedeki hesabınızı seçerek Azure AD B2C kiracınızı seçin.
3. Sol gezinti bölmesinde **tüm hizmetleri**, tıklayın **uygulama kayıtları**, tıklatıp **Ekle**.
4. Komut istemlerini izleyin ve yeni bir uygulama oluşturun. 
    1. Seçin **Web uygulaması / API** uygulama türü olarak.    
    2. Sağlamak **tüm oturum açma URL'si** (örneğin `https://B2CGraphAPI`) Bu örnek için uygun olmadığından.  
5. Uygulama olacak şimdi oluşturan uygulamalar listesinde göster elde etmek için **uygulama kimliği** (istemci kimliği olarak da bilinir). Bir sonraki bölümde ihtiyacınız olacak şekilde kopyalayın.
6. Ayarlar menüsünde tıklatın **anahtarları**.
7. İçinde **parolaları** bölüm anahtarı için bir açıklama girin ve bir süre seçin ve ardından **Kaydet**. Bir sonraki bölümde kullanmak için (istemci gizli anahtarı olarak da bilinir) anahtar değerini kopyalayın.

## <a name="configure-create-read-and-update-permissions-for-your-application"></a>Yapılandırma oluşturmak, okumak ve uygulama izinlerini güncelleştirme
Şimdi oluşturun, okuyun, güncelleştirin ve kullanıcıları silmek için gereken tüm izinleri almak için uygulamanızı yapılandırma gerekir.

1. Azure portalında uygulama kayıtları menüde devam ederek, uygulamanızı seçin.
2. Ayarlar menüsünde tıklayarak **gerekli izinler**.
3. Gerekli izinler menüde tıklayarak **Windows Azure Active Directory**.
4. Erişimi etkinleştir menüde **dizin verilerini okuma ve yazma** izni **uygulama izinleri** tıklatıp **Kaydet**.
5. Son olarak, geri gerekli izinleri üzerinde menüden **izinler** düğmesi.

Artık oluşturmak, okumak ve B2C kiracınızın kullanıcılardan güncelleştirme izni olan bir uygulamanız var.

> [!NOTE]
> İzinleri verme, tam olarak işlemek için birkaç dakika sürebilir.
> 
> 

## <a name="configure-delete-or-update-password-permissions-for-your-application"></a>Uygulamanız için parola izinleri güncelleştirme veya silme'ı yapılandırma
Şu anda *dizin verilerini okuma ve yazma* izin vermiyor **değil** kullanıcıları silin veya kullanıcı parolalarını güncelleştirin; olanağı da sunar. Uygulamanızı özelliği silme veya güncelleştirme parolaları kullanıcıya vermek istediğiniz PowerShell ilgili aşağıdaki ek adımları yapmanız gerekir, aksi halde, sonraki bölüme atlayabilirsiniz.

İlk olarak, zaten yüklü değilse, yükleyin [Azure AD PowerShell v1 Modülü (MSOnline)](https://docs.microsoft.com/powershell/azure/active-directory/install-msonlinev1?view=azureadps-1.0):

```powershell
Install-Module MSOnline
```

PowerShell modülü yükledikten sonra Azure AD B2C kiracınıza bağlanın.

> [!IMPORTANT]
> Bir B2C Kiracı yönetici hesabı kullanmanız gerekir **yerel** B2C kiracısı için. Bu hesaplar şöyle görünür: myusername@myb2ctenant.onmicrosoft.com.

```powershell
Connect-MsolService
```

Kullanacağız artık **uygulama kimliği** uygulamanın kullanıcı hesabı yönetici rolü atamak için aşağıdaki betikte yer. Tüm yapmanız gereken giriş için iyi bilinen tanımlayıcılar, bu roller sahip, **uygulama kimliği** aşağıdaki betikte yer.

```powershell
$applicationId = "<YOUR_APPLICATION_ID>"
$sp = Get-MsolServicePrincipal -AppPrincipalId $applicationId
Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId $sp.ObjectId -RoleMemberType servicePrincipal
```

Uygulamanızı şimdi de kullanıcıları silin veya B2C kiracınızın parolalardan güncelleştirme izni vardır.

## <a name="download-configure-and-build-the-sample-code"></a>Karşıdan yükleme, yapılandırma ve örnek kodu derleme
İlk olarak, örnek kodu indirdikten ve çalışmasını alın. Bu durumda biz daha yakından bakalım kazanacaktır.  Yapabilecekleriniz [örnek kodu bir .zip dosyası olarak indirme](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip). Ayrıca, tercih ettiğiniz bir dizine kopyalayabilirsiniz:

```cmd
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

Açık `B2CGraphClient\B2CGraphClient.sln` Visual Studio'daki Visual Studio çözümü. İçinde `B2CGraphClient` dosyasını açın, proje `App.config`. Üç uygulama ayarları, kendi değerlerinizle değiştirin:

```xml
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{The ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{The Key from above}" />
</appSettings>
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Ardından, sağ `B2CGraphClient` çözüm ve yeniden oluşturma örneği. Başarılı olursa, şu anda olmalıdır bir `B2C.exe` bulunan yürütülebilir dosyasını `B2CGraphClient\bin\Debug`.

## <a name="build-user-crud-operations-by-using-the-graph-api"></a>Graph API'sini kullanarak yapı kullanıcı CRUD işlemleri
B2CGraphClient kullanmak için açık bir `cmd` Windows komutunu yazıp, dizinine `Debug` dizin. Ardından çalıştırın `B2C Help` komutu.

```cmd
cd B2CGraphClient\bin\Debug
B2C Help
```

Bu her komut kısa bir açıklamasını görüntüler. Her zaman bu komutlardan birini çağırma `B2CGraphClient` Azure AD Graph API'sine bir istek gönderir.

### <a name="get-an-access-token"></a>Bir erişim belirteci alma
Graph API'sine yapılan tüm istekleri için kimlik doğrulaması erişim belirteci gerektirir. `B2CGraphClient` açık kaynak Active Directory Authentication Library (ADAL) erişim belirteçlerini almak için kullanır. ADAL belirteç edinme basit bir API sağlayarak ve erişim belirteçlerini önbelleğe alma gibi bazı önemli ayrıntıları alma care kolaylaştırır. Ancak belirteçlerini almak için ADAL'ı kullanmak zorunda değilsiniz. Ayrıca, HTTP isteklerini kaynaklı tarafından belirteç alabilir.

> [!NOTE]
> Bu kod örneği, ADAL v2, Graph API ile iletişim kurmak için kullanır.  Azure AD Graph API ile kullanılabilecek erişim belirteçlerini almak için ADAL v2 veya v3 kullanmanız gerekir.
> 
> 

Zaman `B2CGraphClient` çalıştıran bir örneğini oluşturur `B2CGraphClient` sınıfı. Bu sınıf için oluşturucu bir ADAL kimlik doğrulaması iskele kurma ayarlar:

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

Kullanacağız `B2C Get-User` örnek olarak komutu. Zaman `B2C Get-User` , CLI çağrıları gibi ek hiç giriş çağrılan `B2CGraphClient.GetAllUsers(...)` yöntemi. Bu yöntemin çağırdığı `B2CGraphClient.SendGraphGetRequest(...)`, Graph API'si için bir HTTP GET isteği gönderir. Önce `B2CGraphClient.SendGraphGetRequest(...)` gönderir GET isteği, ilk erişim ADAL kullanarak belirteci alır:

```csharp
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

Erişim için Graph API ADAL çağırarak belirteci alabileceğiniz `AuthenticationContext.AcquireToken(...)` yöntemi. Ardından ADAL döndürür bir `access_token` , uygulamanın kimliğini temsil eder.

### <a name="read-users"></a>Kullanıcıların okuma
Kullanıcıların bir listesini alın veya belirli bir kullanıcı grafik API'deki almak istediğinizde, HTTP gönderebilirsiniz `GET` isteği `/users` uç noktası. Bir kiracıdaki tüm kullanıcılar için bir istek şöyle görünür:

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Bu istek görmek için şunu çalıştırın:

 ```cmd
 B2C Get-User
 ```

Dikkat edilecek iki önemli noktalar vardır:

* ADAL alınan erişim belirteci eklenir `Authorization` kullanarak üstbilgi `Bearer` düzeni.
* B2C kiracıları için sorgu parametresini kullanmanız gerekir `api-version=1.6`.

Bu ayrıntılar her ikisi de işlenir `B2CGraphClient.SendGraphGetRequest(...)` yöntemi:

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
B2C kiracınıza kullanıcı hesapları oluştururken, bir HTTP gönderme `POST` isteği `/users` uç noktası:

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

Bu isteği bu özelliklerin çoğu, tüketici kullanıcılar oluşturmak için gereklidir. Daha fazla bilgi edinmek için tıklayın [burada](/previous-versions/azure/ad/graph/api/users-operations#CreateLocalAccountUser). Unutmayın `//` açıklamaları çizim için dahil edilmiştir. Bunları, gerçek bir istekte içermez.

İstek görmek için aşağıdaki komutlardan birini çalıştırın:

```cmd
B2C Create-User ..\..\..\usertemplate-email.json
B2C Create-User ..\..\..\usertemplate-username.json
```

`Create-User` Komutun giriş parametresi olarak bir .json dosyası alabilir. Bu, bir kullanıcı nesnesi JSON gösterimini içerir. Örnek kodda iki örnek .json dosyaları vardır: `usertemplate-email.json` ve `usertemplate-username.json`. Bu dosyalar, gereksinimlerinize uyacak şekilde değiştirebilirsiniz. Yukarıdaki gerekli alanlara ek olarak, bu dosyalarda kullanabileceğiniz çeşitli isteğe bağlı alanları dahil edilir. İsteğe bağlı alanları hakkında ayrıntılı bilgi bulunabilir [Azure AD Graph API varlık başvurusu](/previous-versions/azure/ad/graph/api/entity-and-complex-type-reference#user-entity).

POST isteğini nasıl oluşturulur gördüğünüz `B2CGraphClient.SendGraphPostRequest(...)`.

* İçin bir erişim belirteci ekler `Authorization` isteği üstbilgisi.
* Bu ayarlar `api-version=1.6`.
* Bu JSON kullanıcı nesnesi istek gövdesinde içerir.

> [!NOTE]
> Varolan bir kullanıcı mağazadan geçirmek istediğiniz hesapları daha düşük bir parola gücünü varsa [Azure AD B2C tarafından zorlanan güçlü parola gücü](/previous-versions/azure/jj943764(v=azure.100)), güçlü parola kullanma gereksinimini devre dışı bırakabilirsiniz `DisableStrongPassword` değerini `passwordPolicies` özelliği. Örneğin, yukarıdaki gibi sağlanan oluşturma kullanıcı isteğini değiştirebilirsiniz: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.
> 
> 

### <a name="update-consumer-user-accounts"></a>Tüketici kullanıcı hesaplarını güncelleştirme
Kullanıcı, nesneyi güncelleştirdiğinizde, kullanıcı, nesneyi oluşturmak için kullandığınız gibi bir işlemdir. Ancak bu işlem, HTTP kullanan `PATCH` yöntemi:

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer"                // this request updates only the user's displayName
}
```

Bir kullanıcı yeni veriler ile JSON dosyalarınızı güncelleştirerek güncelleştirmeyi deneyin. Ardından `B2CGraphClient` şu komutları çalıştırmak için:

```cmd
B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

İnceleme `B2CGraphClient.SendGraphPatchRequest(...)` bu isteği gönderme konusunda ayrıntılar için yöntemi.

### <a name="search-users"></a>Kullanıcılarda arama
Çeşitli şekillerde B2C kiracınızdaki kullanıcılar için arama yapabilirsiniz. Bir kullanıcının nesne kimliği veya kullanıcının oturum açma kimliği kullanarak iki adet (yani, `signInNames` özelliği).

Belirli bir kullanıcı için aranacak aşağıdaki komutlardan birini çalıştırın:

```cmd
B2C Get-User <user-object-id>
B2C Get-User <filter-query-expression>
```

Aşağıda birkaç örnek verilmiştir:

```cmd
B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a>Kullanıcıları Sil
Kullanıcı silme işlemi oldukça basittir. HTTP kullanan `DELETE` yöntem ve yapı URL'si ile doğru nesne kimliği:

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Bir örnek görmek için şu komutu girin ve konsola yazdırılır silme isteği görüntüleyin:

```cmd
B2C Delete-User <object-id-of-user>
```

İnceleme `B2CGraphClient.SendGraphDeleteRequest(...)` bu isteği gönderme konusunda ayrıntılar için yöntemi.

Kullanıcı Yönetimi ek olarak Azure AD Graph API'si ile başka birçok eylemi gerçekleştirebilirsiniz. [Azure AD Graph API Başvurusu](/previous-versions/azure/ad/graph/api/api-catalog) örnek istekler yanı sıra her bir eylem hakkında ayrıntılı bilgi sağlar.

## <a name="use-custom-attributes"></a>Özel öznitelikler kullanma
Çoğu tüketici uygulamaları, herhangi bir türde özel kullanıcı profili bilgilerini depolamak gerekir. Bunu yapmanın bir yolu, özel bir öznitelik B2C kiracınızda tanımlamaktır. Ardından, bu öznitelik bir kullanıcı nesnesi üzerinde diğer herhangi bir özelliği kabul aynı şekilde davranabilirsiniz. Özniteliği güncelleştirme, öznitelik Sil, özniteliği tarafından sorgu, öznitelik oturum belirteçleri ve daha fazlasını talep olarak gönder.

Özel bir öznitelik B2C kiracınızda tanımlamak için bkz: [B2C özel öznitelik başvurusu](active-directory-b2c-reference-custom-attr.md).

Kullanarak B2C kiracınıza tanımlanan özel özniteliklere görüntüleyebileceğiniz `B2CGraphClient`:

```cmd
B2C Get-B2C-Application
B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

Bu işlevlerin çıkış gibi her bir özel öznitelik ayrıntılarını gösterir:

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

Tam adı gibi kullanabileceğiniz `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, kullanıcının nesnelerinizin üzerindeki bir özellik olarak.  Yeni özellik ve özelliği için bir değer ile .json dosyanızı güncelleştirin ve ardından çalıştırın:

```cmd
B2C Update-User <object-id-of-user> <path-to-json-file>
```

Kullanarak `B2CGraphClient`, B2C Kiracı kullanıcılarınızın programlı bir şekilde yönetebilen bir hizmet uygulamasına sahip olacaksınız. `B2CGraphClient` Azure AD Graph API için kimlik doğrulaması yapmak için kendi uygulama kimliğini kullanır. Ayrıca, bir istemci gizli anahtarını kullanarak belirteçleri da alır. Bu işlevselliği uygulamanıza eklemenize, B2C uygulamaları için birkaç önemli nokta unutmayın:

* Uygulama uygun Kiracı izinleri gerekir.
* Şimdilik, erişim belirteçlerini almak için ADAL (MSAL değil) kullanmanız gerekir. (De protokol iletilerini doğrudan bir kitaplık kullanılarak olmadan gönderebilirsiniz.)
* Graph API'sini çağırmak kullanınl `api-version=1.6`.
* Oluşturduğunuzda ve tüketici güncelleştirme birkaç özellik yukarıda açıklanan şekilde gereklidir.

Herhangi bir sorunuz veya B2C kiracınızda Graph API'sini kullanarak gerçekleştirmek istediğiniz istekleri eylemler için varsa, bu makalede bir yorum yazın veya GitHub kod örnek deposunda sorun kaydedebilir.

