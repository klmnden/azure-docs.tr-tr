---
title: Azure SignalR hizmeti kullanma hakkında bilgi edinmek için hızlı başlangıç
description: Azure SignalR Hizmeti’ni kullanarak ASP.NET Core MVC uygulamaları ile bir sohbet odası oluşturmaya yönelik hızlı başlangıç.
author: sffamily
ms.service: signalr
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 03/01/2019
ms.author: zhshang
ms.openlocfilehash: 248861848aa905f9cbff01ab60affd7cf21aae78
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60690326"
---
# <a name="quickstart-create-a-chat-room-with-signalr-service"></a>Hızlı Başlangıç: Sohbet odası ile SignalR hizmeti oluşturma


Azure SignalR Hizmeti, geliştiricilerin gerçek zamanlı özelliklerle web uygulamalarını kolayca derlemesine yardımcı olan bir Azure hizmetidir. Bu hizmet [ASP.NET Core 2.0 için SignalR](https://docs.microsoft.com/aspnet/core/signalr/introduction)’yi temel alır.

Bu makalede Azure SignalR Hizmeti ile çalışmaya başlama işlemi gösterilmektedir. Bu hızlı başlangıçta bir ASP.NET Core MVC Web Uygulamasını kullanarak bir sohbet uygulaması oluşturacaksınız. Bu uygulama, gerçek zamanlı içerik güncelleştirmelerini etkinleştirmek üzere Azure SignalR Hizmeti kaynağınızla bağlantı kuracaktır. Web uygulamasını yerel olarak barındıracak ve birden fazla tarayıcı istemcisine bağlanacaksınız. Her istemci, diğer tüm istemcilere içerik güncelleştirmeleri gönderebilecektir. 


Bu hızlı başlangıçtaki adımları tamamlamak için herhangi bir kod düzenleyicisini kullanabilirsiniz. Ancak, Windows, macOS ve Linux platformlarında sağlanan [Visual Studio Code](https://code.visualstudio.com/) mükemmel bir seçenektir.

Bu öğreticinin kodu [AzureSignalR-samples GitHub deposundan](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/ChatRoom) indirilebilir.  Ayrıca, bu hızlı başlangıçta kullanılan Azure kaynakları, [SignalR Hizmeti betiği oluşturma](scripts/signalr-cli-create-service.md) adımlarıyla oluşturulabilir.

![Hızlı Başlangıç Yerel tamamlama](media/signalr-quickstart-dotnet-core/signalr-quickstart-complete-local.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Önkoşullar

* [.NET Core SDK](https://www.microsoft.com/net/download/windows)’sını yükleme
* İndirin veya kopyalayın [AzureSignalR örnek](https://github.com/aspnet/AzureSignalR-samples) GitHub deposu. 

## <a name="create-an-azure-signalr-resource"></a>Azure SignalR kaynağı oluşturma

[!INCLUDE [azure-signalr-create](../../includes/signalr-create.md)]

## <a name="create-an-aspnet-core-web-app"></a>ASP.NET Core web uygulaması oluşturma

Bu bölümde, [.NET Core komut satırı arabirimini (CLI)](https://docs.microsoft.com/dotnet/core/tools/) kullanarak yeni bir ASP.NET Core MVC Web Uygulaması projesi oluşturacaksınız. Visual Studio yerine .NET Core CLI kullanmanın avantajı Windows, macOS ve Linux platformlarında kullanılabilir olmasıdır. 

1. Projeniz için yeni bir klasör oluşturun. Bu hızlı başlangıçta *E:\Testing\chattest* klasörü kullanılacaktır.

2. Yeni klasörde yeni bir ASP.NET Core MVC Web Uygulaması projesi oluşturmak için aşağıdaki komutu yürütün:

        dotnet new mvc


## <a name="add-secret-manager-to-the-project"></a>Projeye Gizli Dizi Yöneticisi ekleme

Bu bölümde, projenize [Gizli Dizi Yöneticisi aracını](https://docs.microsoft.com/aspnet/core/security/app-secrets) ekleyeceksiniz. Gizli Dizi Yöneticisi aracı, geliştirme işine yönelik hassas verileri proje ağacınızın dışında depolar. Bu yaklaşım, uygulama gizli dizilerini kaynak kodunun içinde yanlışlıkla paylaşmayı önlemeye yardımcı olur.

1. *.csproj* dosyanızı açın. *Microsoft.Extensions.SecretManager.Tools* öğesini dahil etmek için bir `DotNetCliToolReference` öğesi ekleyin. Ayrıca, aşağıda gösterildiği gibi bir `UserSecretsId` öğesi ekleyin ve dosyayı kaydedin.

    *chattest.csproj:*

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">
    <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <UserSecretsId>SignalRChatRoomEx</UserSecretsId>
    </PropertyGroup>
    <ItemGroup>
        <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
    </ItemGroup>
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
        <DotNetCliToolReference Include="Microsoft.Extensions.SecretManager.Tools" Version="2.0.0" />
    </ItemGroup>
    </Project>    
    ```

## <a name="add-azure-signalr-to-the-web-app"></a>Web uygulamasına Azure SignalR ekleme

1. Aşağıdaki komutu yürüterek `Microsoft.Azure.SignalR` NuGet paketine bir başvuru ekleyin:

        dotnet add package Microsoft.Azure.SignalR

2. Projeniz için paketleri geri yüklemek üzere aşağıdaki komutu yürütün.

        dotnet restore

3. Gizli Dizi Yöneticisi’ne *Azure:SignalR:ConnectionString* adlı bir gizli dizi ekleyin. 

    Bu gizli dizi, SignalR Hizmetinizin kaynağına erişmeye yarayan bağlantı dizesini içerir. *Azure:SignalR:ConnectionString*, SignalR’nin bir bağlantı kurmak için aradığı varsayılan yapılandırma anahtarıdır. Aşağıdaki komutta yer alan değeri, SignalR Hizmetinizin kaynağına ait bağlantı dizesi ile değiştirin.

    Bu komut, *.csproj* dosyası ile aynı dizinde yürütülmelidir.

    ```
    dotnet user-secrets set Azure:SignalR:ConnectionString "<Your connection string>"    
    ```

    Gizli Dizi Yöneticisi yalnızca yerel olarak barındırıldığı sırada web uygulamasını test etmek için kullanılır. Sonraki bir öğreticide, sohbet uygulamasını Azure’a dağıtacaksınız. Web uygulaması dağıtıldıktan sonra bağlantı dizesini Gizli Dizi Yöneticisi ile depolamak yerine bir uygulama ayarını kullanacaksınız.

    Bu gizli dizi API configuration ile erişilir. Desteklenen tüm platformlarda, yapılandırma API'lerinin yapılandırma adlarında iki nokta üst üste (:) işareti kullanılabilir [Ortama göre yapılandırma](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/index?tabs=basicconfiguration&view=aspnetcore-2.0). 


4. *Startup.cs* dosyasını açın ve `services.AddSignalR().AddAzureSignalR()` yöntemini çağırarak `ConfigureServices` yöntemini Azure SignalR Hizmeti’ni kullanacak şekilde güncelleştirin:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddMvc();
        services.AddSignalR().AddAzureSignalR();
    }
    ```

    `AddAzureSignalR()` konumuna bir parametre geçirmeyen bu kod, SignalR Hizmeti kaynak bağlantı dizesi için varsayılan *Azure:SignalR:ConnectionString* yapılandırma anahtarını kullanır.

5. *Startup.cs* dosyasında da `app.UseStaticFiles()` çağrısını aşağıdaki kod ile değiştirerek `Configure` yöntemini güncelleştirin ve dosyayı kaydedin.

    ```csharp
    app.UseFileServer();
    app.UseAzureSignalR(routes =>
    {
        routes.MapHub<Chat>("/chat");
    });
    ```            

### <a name="add-a-hub-class"></a>Hub sınıfı ekleme

SignalR’de hub, istemciden çağrılabilen bir yöntemler kümesini kullanıma sunan temel bir bileşendir. Bu bölümde, iki yöntemle bir hub sınıf tanımlayacaksınız: 

* `Broadcast`: Bu yöntem, tüm istemciler için bir ileti yayımlar.
* `Echo`: Bu yöntem çağırana geri bir ileti gönderir.

Her iki yöntem de ASP.NET Core SignalR SDK’sı tarafından sağlanan `Clients` arabirimini kullanır. Bu arabirim, istemcilerinize içerik göndermenizi mümkün hale getirerek tüm bağlı istemcilere erişmenizi sağlar.

1. Proje dizininizde *Hub* adlı yeni bir klasör ekleyin. Yeni klasöre *Chat.cs* adlı yeni bir hub kod dosyası ekleyin.

2. Hub sınıfınızı tanımlamak için aşağıdaki kodu *Chat.cs* dosyasına ekleyin ve dosyayı kaydedin. 

    *Chattest* dışında bir proje adı kullandıysanız bu sınıfın ad alanını güncelleştirin.

    ```csharp
    using Microsoft.AspNetCore.SignalR;

    namespace chattest
    {

        public class Chat : Hub
        {
            public void BroadcastMessage(string name, string message)
            {
                Clients.All.SendAsync("broadcastMessage", name, message);
            }

            public void Echo(string name, string message)
            {
                Clients.Client(Context.ConnectionId).SendAsync("echo", name, message + " (echo from server)");
            }
        }
    }
    ```

### <a name="add-the-web-app-client-interface"></a>Web uygulaması istemci arabirimi ekleme

Bu sohbet odası uygulamasının istemci kullanıcı arabirimi, *wwwroot* dizini içindeki *index.html* adlı bir dosyada HTML ve JavaScript ile oluşturulur.

*index.html* dosyasını ve *css* ile *scripts* klasörlerini [örnekler deposunun](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/ChatRoom/wwwroot) *wwwroot* klasöründen projenizin *wwwroot* klasörüne kopyalayın.

*index.html* ana kodu: 

```javascript
var connection = new signalR.HubConnectionBuilder()
                            .withUrl('/chat')
                            .build();
bindConnectionMessage(connection);
connection.start()
    .then(function () {
        onConnected(connection);
    })
    .catch(function (error) {
        console.error(error.message);
    });
```    

*index.html* dosyasındaki kod, Azure SignalR kaynağı ile bir HTTP bağlantısı oluşturmak için `HubConnectionBuilder.build()` çağrısı yapar.

Bağlantı başarılı olursa, o bağlantı `bindConnectionMessage` konumuna geçirilir ve istemciye gönderilen gelen içerik için olay işleyicileri ekler. 

`HubConnection.start()`, hub ile iletişim başlatır. İletişim başlatıldıktan sonra `onConnected()`, düğme olay işleyicilerini ekler. Bu işleyiciler, bağlantıyı kullanarak bu istemcinin tüm bağlı istemcilere içerik güncelleştirmeleri göndermesine olanak tanır.

## <a name="add-a-development-runtime-profile"></a>Geliştirme çalışma zamanı profili ekleme

Bu bölümde, ASP.NET Core için bir geliştirme çalışma zamanı ortamı ekleyeceksiniz. ASP.NET Core çalışma zamanı ortamı hakkında daha fazla bilgi için bkz. [ASP.NET Core’da birden fazla ortamla çalışma](https://docs.microsoft.com/aspnet/core/fundamentals/environments).

1. Projenizde *Properties* adlı yeni bir klasör oluşturun.

2. Klasöre aşağıdaki içerikle *launchSettings.json* adlı yeni bir dosya ekleyin ve dosyayı kaydedin.

    ```json
    {
        "profiles" : 
        {
            "ChatRoom": 
            {
                "commandName": "Project",
                "launchBrowser": true,
                "environmentVariables": 
                {
                    "ASPNETCORE_ENVIRONMENT": "Development"
                },
                "applicationUrl": "http://localhost:5000/"
            }
        }
    }
    ```


## <a name="build-and-run-the-app-locally"></a>Uygulamayı derleme ve yerel olarak çalıştırma

1. .NET Core CLI kullanarak uygulamayı derlemek için komut kabuğunda aşağıdaki komutu yürütün:

        dotnet build

2. Derleme başarıyla tamamlandıktan sonra web uygulamasını yerel olarak çalıştırmak için aşağıdaki komutu yürütün:

        dotnet run

    Uygulama, geliştirme çalışma zamanı profilimizde yapılandırıldığı gibi 5000 numaralı bağlantı noktası üzerinde yerel olarak barındırılır:

        E:\Testing\chattest>dotnet run
        Hosting environment: Development
        Content root path: E:\Testing\chattest
        Now listening on: http://localhost:5000
        Application started. Press Ctrl+C to shut down.    

3. İki tarayıcı penceresi başlatın ve her tarayıcıda `http://localhost:5000` sayfasına göz atın. Adınızı girmeniz istenir. Her iki istemci için bir istemci adı girin ve **Gönder** düğmesini kullanarak her iki istemci arasında ileti içeriğinin gönderilmesini test edin.

    ![Hızlı Başlangıç Yerel tamamlama](media/signalr-quickstart-dotnet-core/signalr-quickstart-complete-local.png)



## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki öğretici ile devam edecekseniz, bu hızlı başlangıçta oluşturulan kaynakları tutabilir ve sonraki öğreticide yeniden kullanabilirsiniz.

Aksi takdirde, hızlı başlangıç örnek uygulamasını tamamladıysanız ücret yansıtılmaması için bu hızlı başlangıçta oluşturulan Azure kaynaklarını silebilirsiniz. 

> [!IMPORTANT]
> Bir kaynak grubunu silme işlemi geri alınamaz ve kaynak grubunun ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. Bu örneği, tutmak istediğiniz kaynakları içeren mevcut bir kaynak grubunda barındırmak için kaynaklar oluşturduysanız, kaynak grubunu silmek yerine her kaynağı kendi ilgili dikey penceresinden tek tek silebilirsiniz.
> 
> 

[Azure portalında](https://portal.azure.com) oturum açın ve **Kaynak grupları**’na tıklayın.

**Ada göre filtrele...** metin kutusuna kaynak grubunuzun adını girin. Bu hızlı başlangıçtaki yönergelerde *SignalRTestResources* adlı bir kaynak grubu kullanılmıştır. Sonuç listesindeki kaynak grubunuzda **...** ve sonra **Kaynak grubunu sil**’e tıklayın.

   
![Sil](./media/signalr-quickstart-dotnet-core/signalr-delete-resource-group.png)


Kaynak grubunun silinmesini onaylamanız istenir. Onaylamak için kaynak grubunuzun adını yazın ve **Sil**’e tıklayın.
   
Birkaç dakika sonra kaynak grubu ve içerdiği kaynakların tümü silinir.



## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta yeni bir Azure SignalR Hizmeti kaynağı oluşturdunuz ve birden fazla bağlı istemciye gerçek zamanlı içerik güncelleştirmeleri göndermek için bir ASP.NET Core Web uygulaması ile birlikte kullandınız. Azure SignalR Hizmeti’ni kullanma hakkında daha fazla bilgi için kimlik doğrulamasını gösteren sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure SignalR Hizmeti kimlik doğrulaması](./signalr-concept-authenticate-oauth.md)


