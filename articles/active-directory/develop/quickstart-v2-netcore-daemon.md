---
title: Microsoft kimlik platformu .NET Core arka plan programı | Azure
description: Nasıl bir .NET Core işlem bir erişim belirteci alma ve uygulamanın kendi kimliğini kullanarak Microsoft kimlik platformu uç noktası tarafından korunan bir API'yi çağırabilen öğrenin
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: a42cfe1374f3defdf6ed8acc828e6c7e446588bc
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595139"
---
# <a name="quickstart-acquire-a-token-and-call-microsoft-graph-api-from-a-console-app-using-apps-identity"></a>Hızlı Başlangıç: Bir belirteç almak ve Microsoft Graph API'sini çağırmak uygulamanın kimliğini kullanarak bir konsol uygulaması

Bu hızlı başlangıçta, uygulamanın kendi kimliğini kullanarak bir erişim belirteci alabilen ve ardından görüntülemek için Microsoft Graph API'sini çağırmak bir .NET Core uygulaması yazma öğreneceksiniz bir [kullanıcıların listesini](https://docs.microsoft.com/graph/api/user-list) dizinde. Bu senaryo, gözetimsiz, katılımsız iş ya da bir windows hizmeti, bir kullanıcının kimliği yerine uygulama kimliği ile çalıştırmak için gereken yere durumlarda yararlıdır.

![Bu Hızlı Başlangıç ile oluşturulan örnek uygulamasını nasıl çalıştığını gösterir](media/quickstart-v2-netcore-daemon/netcore-daemon-intro.svg)

## <a name="prerequisites"></a>Önkoşullar

Bu Hızlı Başlangıç [.NET Core 2.2](https://www.microsoft.com/net/download/dotnet-core/2.2).

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme

> [!div renderon="docs" class="sxs-lookup"]
>
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * [Express] [Seçenek 1: Kaydet ve otomatik Uygulamanızı yapılandırmak ve ardından, kod örneğini indirin](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [El ile] [Seçeneği 2: Kaydetme ve uygulama ve kod örneğinizi el ile yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1\. seçenek: Kaydet ve otomatik Uygulamanızı yapılandırmak ve ardından, kod örneğini indirin
>
> 1. Yeni Git [Azure Portalı - Uygulama kayıtları](https://portal.azure.com/?Microsoft_AAD_RegisteredApps=true#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/DotNetCoreDaemonQuickstartPage/sourceType/docs) bölmesi.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yönergeleri izleyerek yeni uygulamanızı yalnızca tek tıklamayla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2\. seçenek: Kaydetme ve uygulama ve kod örneğinizi el ile yapılandırma

> [!div renderon="docs"]
> #### <a name="step-1-register-your-application"></a>1\. adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Geliştiriciler için Microsoft identity platformuna gidin [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfası.
> 1. Seçin **yeni kayıt**.
> 1. Zaman **bir uygulamayı kaydetme** sayfası görüntülenirse, uygulamanızın kayıt bilgilerini girin. 
> 1. İçinde **adı** bölümünde, örneğin, uygulamayı kullanıcılara görüntülenecek bir anlamlı uygulama adı girin `Daemon-console`, ardından **kaydetme** uygulama oluşturmak için.
> 1. Kaydedildikten sonra seçin **sertifikaları ve parolaları** menüsü.
> 1. Altında **istemci gizli dizileri**seçin **+ yeni gizli**. Bir ad ve select vermek **Ekle**. Parolayı güvenli bir konuma kopyalayın. Kodunuzda kullanmak için gerekir.
> 1. Şimdi seçin **API izinleri** menüsünde **+ izin Ekle** düğmesini seçme **Microsoft Graph**.
> 1. Seçin **uygulama izinleri**.
> 1. Altında **kullanıcı** düğümünü **User.Read.All**, ardından **izinleri ekleme**

> [!div class="sxs-lookup" renderon="portal"]
> ### <a name="download-and-configure-your-quickstart-app"></a>İndirme ve hızlı başlangıç uygulamanızı yapılandırma
> 
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>1\. adım: Uygulamanızı Azure portalında yapılandırma
> İstemci gizli anahtarı oluşturma ve Graph API'nin eklemek için ihtiyacınız çalışmak bu hızlı başlangıç için kod örneği için **User.Read.All** uygulama izni.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Benim için şu değişiklikleri yapın]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-windows-desktop/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-your-visual-studio-project"></a>2\. adım: Visual Studio projenizi indirin

[Visual Studio projesini indirin](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/archive/msal3x.zip)

#### <a name="step-3-configure-your-visual-studio-project"></a>3\. adım: Visual Studio projenizi yapılandırın

1. Zip dosyasını diskin köküne yakın bir yerel klasöre (örneğin **C:\Azure-Samples**) ayıklayın.
1. Visual Studio'da - çözümü açın **arka plan programı console.sln** (isteğe bağlı).
1. Düzen **appsettings.json** alanların değerlerini `ClientId`, `Tenant` ve `ClientSecret` aşağıdaki:

    ```json
    "Tenant": "Enter_the_Tenant_Id_Here",
    "ClientId": "Enter_the_Application_Id_Here",
    "ClientSecret": "Enter_the_Client_Secret_Here"
    ```
    > > [!div renderon="portal" id="certandsecretspage" class="sxs-lookup"]
    > > [Yeni bir istemci gizli anahtarı oluştur]()
    
    > [!div class="sxs-lookup" renderon="portal"]
    > > [!NOTE]
    > > Bu hızlı başlangıçta Enter_the_Supported_Account_Info_Here destekler.
    
    > [!div renderon="docs"]
    >> Konumlar:
    >> * `Enter_the_Application_Id_Here` - kaydettiğiniz uygulamanın **Uygulama (istemci) Kimliği** değeridir.
    >> * `Enter_the_Tenant_Id_Here` -Bu değerle **Kiracı kimliği** veya **Kiracı adı** (örneğin, contoso.microsoft.com)
    >> * `Enter_the_Client_Secret_Here` -Bu değeri 1. adım üzerinde oluşturulan gizli anahtarla değiştirin.

    > [!div renderon="docs"]
    > > [!TIP]
    > > Değerlerini bulmak için **uygulama (istemci) kimliği**, **dizin (Kiracı) kimliği**uygulamanın Git **genel bakış** Azure portalında sayfası. Yeni bir anahtar oluşturmak için şu adrese gidin **sertifikaları ve parolaları** sayfası.
    
#### <a name="step-4-admin-consent"></a>4\. Adım: Yönetici onayı

Bu noktada uygulamayı çalıştırmayı denerseniz alacağınız *HTTP 403 - Yasak* hata: `Insufficient privileges to complete the operation`. Çünkü böyle herhangi *yalnızca uygulama izni* dizininizin genel Yöneticisi, uygulamaya izin vermeniz gerekir yani yönetici onayı gerektirir. Rolünüze bağlı olarak aşağıdaki seçeneklerden birini seçin:

##### <a name="global-tenant-administrator"></a>Genel Kiracı Yöneticisi

> [!div renderon="docs"]
> Genel Kiracı yöneticisiyseniz, Git **API izinleri** Azure Portal'ın uygulama kaydı (Önizleme) sayfasından seçim yapıp **{Kiracı adı} için yönetici onayı vermek** ({Kiracı adı} olduğu dizininizin adını).

> [!div renderon="portal" class="sxs-lookup"]
> Bir genel Yöneticiyseniz, Git **API izinleri** seçin sayfasında **Enter_the_Tenant_Name_Here için yönetici izni verme**
> > [!div id="apipermissionspage"]
> > [API izinleri sayfasına gidin]()

##### <a name="standard-user"></a>Standart kullanıcı

Kiracınızın standart bir kullanıcı varsa, uygulamanız için yönetici onayı vermek için genel yönetici istemeniz gerekir. Bunu yapmak için şu URL'yi yöneticinize iletin:

```url
https://login.microsoftonline.com/Enter_the_Tenant_Id_Here/adminconsent?client_id=Enter_the_Application_Id_Here
```

> [!div renderon="docs"]
>> Konumlar:
>> * `Enter_the_Tenant_Id_Here` -Bu değerle **Kiracı kimliği** veya **Kiracı adı** (örneğin, contoso.microsoft.com)
>> * `Enter_the_Application_Id_Here` - kaydettiğiniz uygulamanın **Uygulama (istemci) Kimliği** değeridir.

> [!NOTE]
> Hatasıyla karşılaşabilirsiniz *' AADSTS50011: Uygulama için hiç yanıt adresi kayıtlı '* sonra önceki URL'yi kullanarak bir uygulamaya izin veriliyor. Bu sorun bu uygulama ve URL yeniden yönlendirme URI'si - olmadığı için lütfen yoksayın hata.

#### <a name="step-5-run-the-application"></a>5\. Adım: Uygulamayı çalıştırma

Visual Studio kullanıyorsanız, basın **F5** uygulamayı çalıştırmak için Aksi takdirde, uygulamayı komut istemi veya konsol çalıştırın:

```console
cd {ProjectFolder}\daemon-console
dotnet run
```

> Konumlar:
> * *{ProjectFolder}*  zip dosyasını ayıkladığınız klasörü. Örnek **C:\Azure-Samples\active-directory-dotnetcore-daemon-v2**

Sonuç olarak, Azure AD dizininde kullanıcıların listesini görmeniz gerekir.

> [!IMPORTANT]
> Bu hızlı başlangıç uygulama istemci gizli anahtarı gizli bir istemci kendisini tanımlamak için kullanır. İstemci gizli anahtarı güvenlik nedenleriyle, proje dosyalarına düz metin olarak eklendiğinden uygulamayı üretim uygulaması olarak belirlemeden önce bir istemci parolası yerine bir sertifika kullanmanız önerilir. Bir sertifika kullanma hakkında daha fazla bilgi için bkz. [bu yönergeleri](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/#variation-daemon-application-using-client-credentials-with-certificates) Bu örnek için GitHub deposundaki kodu.

## <a name="more-information"></a>Daha fazla bilgi

### <a name="msalnet"></a>MSAL.NET

MSAL ([Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)) kullanıcılarının oturumunu ve Microsoft kimlik platformu tarafından korunan bir API'ye erişmek için kullanılan belirteci istemek için kullanılan bir kitaplık sunulmaktadır. Bu hızlı başlangıçta açıklandığı gibi temsilci izinleri yerine uygulama kendi kimliğini kullanarak belirteçleri ister. Bu örnekte kullanılan kimlik doğrulama akışı olarak bilinir  *[istemci kimlik bilgileri, oauth akışını](v2-oauth2-client-creds-grant-flow.md)* . İstemci kimlik bilgileri akışı ile MSAL.NET kullanma hakkında daha fazla bilgi için bkz. [bu makalede](https://aka.ms/msal-net-client-credentials).

 Visual Studio'nun aşağıdaki komutu çalıştırarak, MSAL.NET yükleyebilirsiniz **Paket Yöneticisi Konsolu**:

```powershell
Install-Package Microsoft.Identity.Client -Pre
```

Alternatif olarak, Visual Studio kullanmıyorsanız MSAL projenize eklemek için aşağıdaki komutu çalıştırabilirsiniz:

```console
dotnet add package Microsoft.Identity.Client
```

### <a name="msal-initialization"></a>MSAL başlatma

Şu kodu ekleyerek MSAL başvurusunu ekleyebilirsiniz:

```csharp
using Microsoft.Identity.Client;
```

Sonra şu kodu kullanarak MSAL'yi başlatın:

```csharp
IConfidentialClientApplication app;
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
                                          .WithClientSecret(config.ClientSecret)
                                          .WithAuthority(new Uri(config.Authority))
                                          .Build();
);
```

> | Konumlar: ||
> |---------|---------|
> | `config.ClientSecret` | Azure portalında uygulama istemci gizli anahtarı oluşturulur. |
> | `config.ClientId` | **Uygulama (istemci) Kimliği**, Azure portalda kayıtlı uygulamadır. Bu değeri Azure portalda uygulamanın **Genel bakış** sayfasında bulabilirsiniz. |
> | `config.Authority`    | (İsteğe bağlı) STS uç noktası kullanıcının kimliğini doğrulamak. Genellikle <https://login.microsoftonline.com/{tenant}> {tenant} Kiracı veya Kiracı kimliği adı olduğu genel bulut için|

Daha fazla bilgi için lütfen bkz [başvuru belgeleri `ConfidentialClientApplication`](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.iconfidentialclientapplication?view=azure-dotnet)

### <a name="requesting-tokens"></a>Belirteç isteme

Uygulamanın kimliğini kullanarak bir belirteci istemek için kullanın `AcquireTokenForClient` yöntemi:

```csharp
result = await app.AcquireTokenForClient(scopes)
                  .ExecuteAsync();
```

> |Konumlar:| |
> |---------|---------|
> | `scopes` | İstenen kapsamlarını içerir. Gizli istemciler için bu benzer biçimi kullanmalıdır `{Application ID URI}/.default` istenen olan statik olarak uygulama nesnesinde tanımlanan ayarlara kapsamları Azure Portalı'nda ayarlayın belirtmek için (Microsoft Graph için `{Application ID URI}` işaret `https://graph.microsoft.com`). Özel Web API'leri için `{Application ID URI}` bölümünde tanımlanan **bir API'yi kullanıma sunmak** Azure Portal'ın uygulama kaydı (Önizleme) bölümünde. |

Daha fazla bilgi için lütfen bkz [başvuru belgeleri `AcquireTokenForClient`](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.confidentialclientapplication.acquiretokenforclientasync?view=azure-dotnet#Microsoft_Identity_Client_ConfidentialClientApplication_AcquireTokenForClientAsync_System_Collections_Generic_IEnumerable_System_String__)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [.NET core arka plan programı örneği](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2)

İzinler ve onay hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [İzinler ve onay](v2-permissions-and-consent.md)

Bu senaryo için kimlik doğrulama akışı hakkında daha fazla bilgi edinmek için Oauth 2.0 istemci kimlik bilgileri akışı bakın:

> [!div class="nextstepaction"]
> [İstemci kimlik bilgileri Oauth akışı](v2-oauth2-client-creds-grant-flow.md)

> [!div class="nextstepaction"]
> [İstemci kimlik bilgileri ile MSAL.NET akışlar](https://aka.ms/msal-net-client-credentials)
