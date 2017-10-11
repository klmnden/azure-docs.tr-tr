---
title: ".NET için Azure CDN kitaplığını kullanmaya başlama | Microsoft Docs"
description: "Visual Studio kullanarak Azure CDN yönetmek için .NET uygulamaları yazma öğrenin."
services: cdn
documentationcenter: .net
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 63cf4101-92e7-49dd-a155-a90e54a792ca
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 5379586355ece98af6295236d6cbd09cb31c742b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-cdn-development"></a>Azure CDN ile geliştirmeye başlama
> [!div class="op_single_selector"]
> * [Node.js](cdn-app-dev-node.md)
> * [.NET](cdn-app-dev-net.md)
> 
> 

Kullanabileceğiniz [.NET için Azure CDN Kitaplığı](https://msdn.microsoft.com/library/mt657769.aspx) oluşturulması ve CDN profili ve uç noktaları Yönetimi otomatik hale getirmek için.  Bu öğreticide, kullanılabilir işlemleri çeşitli gösteren basit bir .NET konsol uygulaması oluşturma aşamasından açıklanmaktadır.  Bu öğretici, ayrıntılı olarak .NET için Azure CDN kitaplığı tüm yönlerini açıklamak için tasarlanmamıştır.

Bu öğreticiyi tamamlamak için Visual Studio 2015 gerekir.  [Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) ücretsiz olarak karşıdan yüklenebilir.

> [!TIP]
> [Bu öğreticinin tamamlanan projeden](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) MSDN'de indirilebilir.
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a>Projenizi oluşturmak ve Nuget paketleri ekleme
Bizim CDN profili için bir kaynak grubu oluşturduk artık ve CDN profili ve uç noktaları grubu içindeki yönetmek için Azure AD uygulama izni verilen göre biz uygulamamızı oluşturmaya başlayabilirsiniz.

Visual Studio 2015'içinde ' ı tıklatın **dosya**, **yeni**, **proje...**  yeni proje iletişim kutusunu açın.  Genişletme **Visual C#**seçeneğini belirleyip **Windows** sol taraftaki bölmede.  Tıklatın **konsol uygulaması** Orta bölmede.  Projenizi adlandırın ve ardından **Tamam**.  

![Yeni Proje](./media/cdn-app-dev-net/cdn-new-project.png)

Projemizin Nuget paketlerini içinde yer alan bazı Azure kitaplıkları kullanmak zordur.  Bu projeye ekleyelim.

1. Tıklatın **Araçları** menüsünde **Nuget Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**.
   
    ![Nuget paketlerini yönetme](./media/cdn-app-dev-net/cdn-manage-nuget.png)
2. Paket Yöneticisi konsolunda yüklemek için aşağıdaki komutu yürütün **Active Directory Authentication Library (ADAL)**:
   
    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`
3. Yüklemek için aşağıdakileri yürütün **Azure CDN Yönetimi Kitaplığı**:
   
    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a>Yönergeleri, sabitleri, ana yöntemi ve yardımcı yöntemler
Şimdi yazılmış programımız temel yapısını alın.

1. Program.cs sekmesinde geri, yerini `using` üst aşağıdaki yönergeleri:
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using Microsoft.Azure.Management.Cdn;
    using Microsoft.Azure.Management.Cdn.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```
2. Bizim yöntemler kullanacağı bazı sabitleri tanımlamak gerekir.  İçinde `Program` sınıfı, ancak önce `Main` yöntemi, aşağıdaki ekleyin.  Dahil olmak üzere, yer tutucular değiştirdiğinizden emin olun  **&lt;köşeli&gt;**, gerektiği gibi kendi değerlere sahip.
   
    ```csharp
    //Tenant app constants
    private const string clientID = "<YOUR CLIENT ID>";
    private const string clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    private const string authority = "https://login.microsoftonline.com/<YOUR TENANT ID>/<YOUR TENANT DOMAIN NAME>";
   
    //Application constants
    private const string subscriptionId = "<YOUR SUBSCRIPTION ID>";
    private const string profileName = "CdnConsoleApp";
    private const string endpointName = "<A UNIQUE NAME FOR YOUR CDN ENDPOINT>";
    private const string resourceGroupName = "CdnConsoleTutorial";
    private const string resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```
3. Ayrıca sınıf düzeyinde iki bu değişkenleri tanımlayın.  Bunlar daha sonra profil ve uç nokta zaten var olup olmadığını belirlemek için kullanacağız.
   
    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```
4. Değiştir `Main` yöntemini aşağıdaki şekilde:
   
   ```csharp
   static void Main(string[] args)
   {
       //Get a token
       AuthenticationResult authResult = GetAccessToken();
   
       // Create CDN client
       CdnManagementClient cdn = new CdnManagementClient(new TokenCredentials(authResult.AccessToken))
           { SubscriptionId = subscriptionId };
   
       ListProfilesAndEndpoints(cdn);
   
       // Create CDN Profile
       CreateCdnProfile(cdn);
   
       // Create CDN Endpoint
       CreateCdnEndpoint(cdn);
   
       Console.WriteLine();
   
       // Purge CDN Endpoint
       PromptPurgeCdnEndpoint(cdn);
   
       // Delete CDN Endpoint
       PromptDeleteCdnEndpoint(cdn);
   
       // Delete CDN Profile
       PromptDeleteCdnProfile(cdn);
   
       Console.WriteLine("Press Enter to end program.");
       Console.ReadLine();
   }
   ```
5. Bizim diğer yöntemlerden bazıları "Evet/Hayır" sorularını kullanıcıdan adımıdır.  Biraz daha kolay yapmak için aşağıdaki yöntemi ekleyin:
   
    ```csharp
    private static bool PromptUser(string Question)
    {
        Console.Write(Question + " (Y/N): ");
        var response = Console.ReadKey();
        Console.WriteLine();
        if (response.Key == ConsoleKey.Y)
        {
            return true;
        }
        else if (response.Key == ConsoleKey.N)
        {
            return false;
        }
        else
        {
            // They pressed something other than Y or N.  Let's ask them again.
            return PromptUser(Question);
        }
    }
    ```

Programımız temel yapısını yazılır, çağıran yöntemleri oluşturmanız gerekir `Main` yöntemi.

## <a name="authentication"></a>Kimlik Doğrulaması
Azure CDN Yönetimi Kitaplığı kullanabilmeniz için önce kimliğinizi bizim hizmet sorumlusunun kimliğini doğrulamak ve kimlik doğrulama belirtecini almak gerekiyor.  Bu yöntem, belirtecini almak için ADAL kullanır.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority); 
    ClientCredential credential = new ClientCredential(clientID, clientSecret);
    AuthenticationResult authResult = 
        authContext.AcquireTokenAsync("https://management.core.windows.net/", credential).Result;

    return authResult;
}
```

Tek tek kullanıcı kimlik doğrulaması kullanıyorsanız, `GetAccessToken` yöntemi biraz farklı görünür.

> [!IMPORTANT]
> Yalnızca bireysel kullanıcı kimlik doğrulaması, bir hizmet sorumlusu yerine seçmek, bu kod örneği kullanın.
> 
> 

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority);
    AuthenticationResult authResult = authContext.AcquireTokenAsync("https://management.core.windows.net/",
        clientID, new Uri("http://<redirect URI>"), new PlatformParameters(PromptBehavior.RefreshSession)).Result;

    return authResult;
}
```

Değiştirdiğinizden emin olun `<redirect URI>` yeniden yönlendirme URI'si Azure AD'de uygulama kaydolurken girdiğiniz ile.

## <a name="list-cdn-profiles-and-endpoints"></a>Liste CDN profili ve uç noktaları
Şimdi biz CDN işlemlerini gerçekleştirmek için hazır.  Bizim yöntemi yapacağı ilk şey tüm profil ve uç noktaları bizim kaynak grubunda listesidir ve bizim sabitler belirtilen profil ve uç nokta adları için bir eşleşme bulursa, çoğaltmaları oluşturmak denemeyin şekilde, Not için daha sonra sağlar.

```csharp
private static void ListProfilesAndEndpoints(CdnManagementClient cdn)
{
    // List all the CDN profiles in this resource group
    var profileList = cdn.Profiles.ListByResourceGroup(resourceGroupName);
    foreach (Profile p in profileList)
    {
        Console.WriteLine("CDN profile {0}", p.Name);
        if (p.Name.Equals(profileName, StringComparison.OrdinalIgnoreCase))
        {
            // Hey, that's the name of the CDN profile we want to create!
            profileAlreadyExists = true;
        }

        //List all the CDN endpoints on this CDN profile
        Console.WriteLine("Endpoints:");
        var endpointList = cdn.Endpoints.ListByProfile(p.Name, resourceGroupName);
        foreach (Endpoint e in endpointList)
        {
            Console.WriteLine("-{0} ({1})", e.Name, e.HostName);
            if (e.Name.Equals(endpointName, StringComparison.OrdinalIgnoreCase))
            {
                // The unique endpoint name already exists.
                endpointAlreadyExists = true;
            }
        }
        Console.WriteLine();
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>CDN profili ve uç noktaları oluşturma
Ardından, bir profil oluşturacağız.

```csharp
private static void CreateCdnProfile(CdnManagementClient cdn)
{
    if (profileAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating profile {0}.", profileName);
        ProfileCreateParameters profileParms =
            new ProfileCreateParameters() { Location = resourceLocation, Sku = new Sku(SkuName.StandardVerizon) };
        cdn.Profiles.Create(profileName, profileParms, resourceGroupName);
    }
}
```

Profil oluşturulduktan sonra bir uç nokta oluşturacağız.

```csharp
private static void CreateCdnEndpoint(CdnManagementClient cdn)
{
    if (endpointAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating endpoint {0} on profile {1}.", endpointName, profileName);
        EndpointCreateParameters endpointParms =
            new EndpointCreateParameters()
            {
                Origins = new List<DeepCreatedOrigin>() { new DeepCreatedOrigin("Contoso", "www.contoso.com") },
                IsHttpAllowed = true,
                IsHttpsAllowed = true,
                Location = resourceLocation
            };
        cdn.Endpoints.Create(endpointName, endpointParms, profileName, resourceGroupName);
    }
}
```

> [!NOTE]
> Yukarıdaki örnekte uç nokta adlı bir kaynak atar *Contoso* bir ana bilgisayar adı ile `www.contoso.com`.  Bu, kendi kaynağın konak adına işaret edecek şekilde değiştirmeniz gerekir.
> 
> 

## <a name="purge-an-endpoint"></a>Bir uç noktası
Uç noktası oluşturuldu varsayıldığında, biz bizim programında gerçekleştirmek isteyebileceğiniz ortak bir görev bizim endpoint içeriği temizleme.

```csharp
private static void PromptPurgeCdnEndpoint(CdnManagementClient cdn)
{
    if (PromptUser(String.Format("Purge CDN endpoint {0}?", endpointName)))
    {
        Console.WriteLine("Purging endpoint. Please wait...");
        cdn.Endpoints.PurgeContent(endpointName, profileName, resourceGroupName, new List<string>() { "/*" });
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

> [!NOTE]
> Dize yukarıdaki örnekte `/*` bitiş noktası yolu kök her şeyi temizlemek istediğiniz gösterir.  Bu denetimi için eşdeğerdir **temizleme tüm** Azure portal'ın "Temizle" iletişim. İçinde `CreateCdnProfile` yöntemi, oluşturduğum bizim profili için farklı bir **verizon'dan Azure CDN** kod kullanarak profil `Sku = new Sku(SkuName.StandardVerizon)`, böylece bu başarılı olur.  Ancak, **akamai'den Azure CDN** profilleri desteklemez **temizleme tüm**, Bu öğretici için bir Akamai profili kullanıyordu, t belirli yolları eklemeniz gerekir.
> 
> 

## <a name="delete-cdn-profiles-and-endpoints"></a>CDN profili ve uç noktaları Sil
Son yöntemleri bizim uç noktası ve profil silinmesine neden olur.

```csharp
private static void PromptDeleteCdnEndpoint(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN endpoint {0} on profile {1}?", endpointName, profileName)))
    {
        Console.WriteLine("Deleting endpoint. Please wait...");
        cdn.Endpoints.DeleteIfExists(endpointName, profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}

private static void PromptDeleteCdnProfile(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN profile {0}?", profileName)))
    {
        Console.WriteLine("Deleting profile. Please wait...");
        cdn.Profiles.DeleteIfExists(profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

## <a name="running-the-program"></a>Programın çalıştırılması
Biz artık derleyebilir ve tıklayarak program Çalıştır **Başlat** Visual Studio'da düğmesi.

![Program çalışıyor](./media/cdn-app-dev-net/cdn-program-running-1.png)

Program yukarıdaki istemi ulaştığında, kaynak grubunuzdaki Azure portalına dönün ve profilin oluşturulduğunu görmek mümkün olması gerekir.

![Başarılı!](./media/cdn-app-dev-net/cdn-success.png)

Biz, ardından program kalan çalıştırmak ister onaylayabilirsiniz.

![Program Tamamlanıyor](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a>Sonraki Adımlar
Bu kılavuzda tamamlanmış projeden görmek için [örneği indirmek](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).

.NET için Azure CDN Yönetimi Kitaplığı hakkındaki ek belgeleri bulmak için görüntülemek [MSDN'de başvuru](https://msdn.microsoft.com/library/mt657769.aspx).

CDN kaynaklarınızı yönetmek [PowerShell](cdn-manage-powershell.md).

