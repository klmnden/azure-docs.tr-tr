---
title: .NET Core kullanarak Azure Media Services v3 ile canlı akış yapma | Microsoft Belgeler
description: Bu öğretici, .NET Core kullanarak Media Services v3 ile canlı akış yapmayı adım adım anlatılmaktadır.
services: media-services
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 06/06/2018
ms.author: juliako
ms.openlocfilehash: 82ef04993b597778c808d649032603a0f3672e4c
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110339"
---
# <a name="stream-live-with-azure-media-services-v3-using-net-core"></a>.NET Core kullanarak Azure Media Services v3 ile canlı akış yapma

Media Services'de canlı akış içeriğini işlemekten [LiveEvents](https://docs.microsoft.com/rest/api/media/liveevents) sorumludur. Bir LiveEvent, daha sonra canlı bir kodlayıcıya sağladığınız bir giriş uç noktası (alma URL'si) sağlar. LiveEvent, canlı giriş akışlarını canlı kodlayıcıdan alır ve bir veya daha fazla [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints) aracılığıyla akış için kullanıma açar. LiveEvent'ler, ayrıca, akışınızı yapılacak başka işlemlerden ve sunumdan önce doğrulamak amacıyla incelemek için kullanacağınız bir önizleme uç noktası (önizleme URL'si) sağlar. Bu öğretici, **geçiş** türü bir canlı olay oluşturmak için .NET Core kullanmayı göstermektedir. 

> [!NOTE]
> Devam etmeden önce [Media Services v3 ile canlı akış](live-streaming-overview.md) konusunu gözden geçirmeyi unutmayın. 

Öğretici şunların nasıl yapıldığını göstermektedir:    

> [!div class="checklist"]
> * Media Services hesabı oluşturma
> * Media Services API’sine erişim
> * Örnek uygulamayı yapılandırma
> * Canlı akışı gerçekleştiren kodu inceleme
> * Olayı [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) ile http://ampdemo.azureedge.net üzerinde izleme
> * Kaynakları temizleme

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Öğreticiyi tamamlamak için aşağıdakiler gereklidir.

* Visual Studio Code veya Visual Studio yükleme
* Etkinlik yayınlamak için kullanılan bir kamera veya (dizüstü gibi) bir cihaz.
* Şirket içi bir canlı kodlayıcı, kameradan alınan sinyalleri Media Services canlı akış hizmetine gönderilen akışlara dönüştürür. Akışın **RTMP** veya **Kesintisiz Akış** biçiminde olması gerekir.

## <a name="download-the-sample"></a>Örneği indirme

Aşağıdaki komutu kullanarak, akış .NET örneğini içeren bir GitHub havuzunu makinenize kopyalayın:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```

Canlı akış örneği [Live](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/Live/MediaV3LiveApp) klasöründe bulunabilir.

> [!IMPORTANT]
> Bu örnek, her kaynak için benzersiz bir sonek kullanır. Hata ayıklamayı iptal eder veya uygulamayı baştan sona çalıştırmadan sonlandırırsanız, hesabınızda birden çok LiveEvent oluşur. <br/>
> Çalışan LiveEvent'leri durdurduğunuzdan emin olun. Aksi takdirde **faturalandırılırsınız**!

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="examine-the-code-that-performs-live-streaming"></a>Canlı akışı gerçekleştiren kodu inceleme

Bu bölümde, *MediaV3LiveApp* projesinin [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs) dosyasında tanımlı işlevler incelenmektedir.

Örnek, temizlik yapmadan birden çok kez çalıştırırsanız ad çakışmaları olmaması için her kaynağa benzersiz bir sonek oluşturur.

> [!IMPORTANT]
> Bu örnek, her kaynak için benzersiz bir sonek kullanır. Hata ayıklamayı iptal eder veya uygulamayı baştan sona çalıştırmadan sonlandırırsanız, hesabınızda birden çok LiveEvent oluşur. <br/>
> Çalışan LiveEvent'leri durdurduğunuzdan emin olun. Aksi takdirde **faturalandırılırsınız**!
 
### <a name="start-using-media-services-apis-with-net-sdk"></a>.NET SDK ile Media Services API’sini kullanmaya başlama

.NET ile Media Services API’lerini kullanmaya başlamak için bir **AzureMediaServicesClient** nesnesi oluşturmanız gerekir. Nesneyi oluşturmak için, Azure AD kullanarak Azure’a bağlanmak üzere istemcinin ihtiyaç duyduğu kimlik bilgilerini sağlamanız gerekir. Makalenin başlangıcında kopyaladığınız koddaki **GetCredentialsAsync** işlevi, yerel yapılandırma dosyasında sağlanan kimlik bilgilerini temel alan ServiceClientCredentials nesnesini oluşturur.  

```csharp
private static async Task<IAzureMediaServicesClient> CreateMediaServicesClientAsync(ConfigWrapper config)
{
    var credentials = await GetCredentialsAsync(config);

    return new AzureMediaServicesClient(config.ArmEndpoint, credentials)
    {
        SubscriptionId = config.SubscriptionId,
    };
}
```

### <a name="create-a-live-event"></a>Canlı etkinlik oluşturma

Bu bölümde **geçiş** türü bir LiveEvent oluşturma gösterilmektedir (LiveEventEncodingType None olarak ayarlıdır). Canlı kodlama için etkinleştirilmiş bir LiveEvent oluşturmak istiyorsanız, LiveEventEncodingType'ı Basic olarak ayarlayın. 

Canlı etkinliği oluştururken belirtmek isteyebileceğiniz bazı başka şeyler:

* Media Services konumu 
* Canlı Etkinliğin akış protokolü (şu anda RTMP ve Kesintisiz Akış protokolleri desteklenmektedir)
       
    LiveEvent veya ilişkili LiveOutput'ları çalışırken protokol seçeneğini değiştiremezsiniz. Farklı protokollere gerek duyuyorsanız, her akış protokolü için ayrı bir LiveEvent oluşturmanız gerekir.  
* Alma ve önizleme için IP kısıtlamaları. Bu LiveEvent'e bir video almasına izin verilen IP adreslerini tanımlayabilirsiniz. İzin verilen IP adresleri tek bir IP adresi (örneğin '10.0.0.1'), bir IP adresi ve CIDR alt ağ maskesi kullanan bir IP aralığı (örneğin '10.0.0.1/22') veya bir IP adresi ve bir noktalı ondalık alt ağ maskesi kullanan bir IP aralığı (örneğin '10.0.0.1(255.255.252.0)') olabilir.
    
    Herhangi bir IP adresi belirtilmezse ve bir kural tanımı yoksa hiçbir IP adresine izin verilmez. Tüm IP adreslerine izin vermek için, bir kural oluşturun ve 0.0.0.0/0 olarak ayarlayın.

Etkinlik oluştururken, etkinliğin otomatik başlatılmasını belirtebilirsiniz. 

```csharp
Console.WriteLine($"Creating a live event named {liveEventName}");
Console.WriteLine();

LiveEventPreview liveEventPreview = new LiveEventPreview
{
    AccessControl = new LiveEventPreviewAccessControl(
        ip: new IPAccessControl(
            allow: new IPRange[]
            {
                new IPRange (
                    name: "AllowAll",
                    address: "0.0.0.0",
                    subnetPrefixLength: 0
                )
            }
        )
    )
};

// This can sometimes take awhile. Be patient.
LiveEvent liveEvent = new LiveEvent(
    location: mediaService.Location, 
    description:"Sample LiveEvent for testing",
    vanityUrl:false,
    encoding: new LiveEventEncoding(
                // Set this to Basic to enable a transcoding LiveEvent, and None to enable a pass-through LiveEvent
                encodingType:LiveEventEncodingType.None, 
                presetName:null
            ),
    input: new LiveEventInput(LiveEventInputProtocol.RTMP), 
    preview: liveEventPreview,
    streamOptions: new List<StreamOptionsFlag?>()
    {
        // Set this to Default or Low Latency.
        // Low latency reduces the amount of buffering Media Services does.
        // Low latency can also reduce the stability of the live stream. 
        StreamOptionsFlag.Default
    }
);

Console.WriteLine($"Creating the LiveEvent, be patient this can take time...");
liveEvent = client.LiveEvents.Create(config.ResourceGroup, config.AccountName, liveEventName, liveEvent, autoStart:true);
```

### <a name="get-ingest-urls"></a>Alma URL’leri alma

Kanal oluşturulduktan sonra, gerçek zamanlı kodlayıcıya sağlayacağınız alma URL’lerini alabilirsiniz. Kodlayıcı bu URL'leri canlı akış girişi için kullanır.


```csharp
// Get the input endpoint to configure the on-premises encoder with
string ingestUrl = liveEvent.Input.Endpoints.First().Url;
Console.WriteLine($"The ingest url to configure the on-premises encoder with is:");
Console.WriteLine($"\t{ingestUrl}");
Console.WriteLine();
```

### <a name="get-the-preview-url"></a>Önizleme URL'sini alma

Kodlayıcıdan gelen girişin gerçekten alındığını önizlemek ve doğrulamak için PreviewEndpoint kullanın.

> [!IMPORTANT]
> Devam etmeden önce videonun Önizleme URL'sine aktığından emin olun!

```sharp
string previewEndpoint = liveEvent.Preview.Endpoints.First().Url;
Console.WriteLine($"The preview url is:");
Console.WriteLine($"\t{previewEndpoint}");
Console.WriteLine();

Console.WriteLine($"Open the live preview in your browser and use the Azure Media Player to monitor the preview playback:");
Console.WriteLine($"\thttps://ampdemo.azureedge.net/?url={previewEndpoint}");
Console.WriteLine();
```

### <a name="create-and-manage-liveevents-and-liveoutputs"></a>LiveEvent ve LiveOutput oluşturma ve yönetme

Akışın LiveEvent'e akmasını sağladıktan sonra bir Asset, LiveOutput ve StreamingLocator oluşturarak etkinliği akışla göndermeye başlayabilirsiniz. Bu, akışı arşivler ve akışın StreamingEndpoint aracılığıyla izleyiciler tarafından kullanılabilmesini sağlar. 

#### <a name="create-an-asset"></a>Asset oluşturma

LiveOutput'un kullanması için bir Asset oluşturun.

```csharp
string assetName = "archiveAsset" + uniqueness;
Console.WriteLine($"Creating an asset named {assetName}");
Console.WriteLine();
Asset asset = client.Assets.CreateOrUpdate(config.ResourceGroup, config.AccountName, assetName, new Asset());
```

#### <a name="create-a-liveoutput"></a>LiveOutput oluşturma

```csharp
string manifestName = "output";
string liveOutputName = "liveOutput" + uniqueness;
Console.WriteLine($"Creating a live output named {liveOutputName}");
Console.WriteLine();

LiveOutput liveOutput = new LiveOutput(assetName: asset.Name, manifestName: manifestName, archiveWindowLength: TimeSpan.FromMinutes(10));
liveOutput = client.LiveOutputs.Create(config.ResourceGroup, config.AccountName, liveEventName, liveOutputName, liveOutput);
```

#### <a name="create-a-streaminglocator"></a>StreamingLocator oluşturma

> [!NOTE]
> Media Services hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 


```csharp
StreamingLocator locator = new StreamingLocator(assetName: assetName, streamingPolicyName: PredefinedStreamingPolicy.ClearStreamingOnly);
locator = client.StreamingLocators.Create(config.ResourceGroup, config.AccountName, streamingLocatorName, locator);

// Get the default Streaming Endpoint on the account
StreamingEndpoint streamingEndpoint = client.StreamingEndpoints.Get(config.ResourceGroup, config.AccountName, "default");

// If it's not running, Start it. 
if (streamingEndpoint.ResourceState != StreamingEndpointResourceState.Running)
{
    Console.WriteLine("Streaming Endpoint was Stopped, restarting now..");
    client.StreamingEndpoints.Start(config.ResourceGroup, config.AccountName, "default");
}

// Get the url to stream the output
ListPathsResponse paths = await client.StreamingLocators.ListPathsAsync(resourceGroupName, accountName, locatorName);

foreach (StreamingPath path in paths.StreamingPaths)
{
    UriBuilder uriBuilder = new UriBuilder();
    uriBuilder.Scheme = "https";
    uriBuilder.Host = streamingEndpoint.HostName;

    uriBuilder.Path = path.Paths[0];
    // Get the URL from the uriBuilder: uriBuilder.ToString()
}
```

### <a name="cleaning-up-resources-in-your-media-services-account"></a>Media Services hesabınızdaki kaynakları temizleme

Olayların akışla aktarılmasını tamamlayıp önceden sağlanan kaynakları temizlemek istediğinizde aşağıdaki yordamı izleyin.

* Kodlayıcıdan akışı göndermeyi durdurun.
* LiveEvent'i durdurun. LiveEvent durdurulduktan sonra herhangi bir ücret uygulanmaz. Tekrar başlatmanız gerektiğinde, aynı alma URL’sine sahip olacağından kodlayıcıyı yeniden yapılandırmanız gerekmez.
* Canlı etkinliğinizin arşivini isteğe bağlı bir akış olarak sunmaya devam etmek istemiyorsanız StreamingEndpoint'inizi durdurabilirsiniz. LiveEvent durdurulmuş durumdaysa herhangi bir ücret uygulanmaz.

```csharp
private static void CleanupLiveEventAndOutput(IAzureMediaServicesClient client, string resourceGroup, string accountName, string liveEventName, string liveOutputName)
{
    // Delete the LiveOutput
    client.LiveOutputs.Delete(resourceGroup, accountName, liveEventName, liveOutputName);

    // Stop and delete the LiveEvent
    client.LiveEvents.Stop(resourceGroup, accountName, liveEventName);
    client.LiveEvents.Delete(resourceGroup, accountName, liveEventName);
}

private static void CleanupLocatorAssetAndStreamingEndpoint(IAzureMediaServicesClient client, string resourceGroup, string accountName, string streamingLocatorName, string assetName)
{
    // Delete the Streaming Locator
    client.StreamingLocators.Delete(resourceGroup, accountName, streamingLocatorName);

    // Delete the Archive Asset
    client.Assets.Delete(resourceGroup, accountName, assetName);
}

private static void CleanupAccount(IAzureMediaServicesClient client, string resourceGroup, string accountName)
{
    try{
        Console.WriteLine("Cleaning up the resources used, stopping the LiveEvent. This can take a few minutes to complete.");
        Console.WriteLine();

        var events = client.LiveEvents.List(resourceGroup, accountName);
        
        foreach (LiveEvent l in events)
        {
            if (l.Name == liveEventName){
                var outputs = client.LiveOutputs.List(resourceGroup, accountName, l.Name);

                foreach (LiveOutput o in outputs)
                {
                    client.LiveOutputs.Delete(resourceGroup, accountName, l.Name, o.Name);
                    Console.WriteLine($"LiveOutput: {o.Name} deleted from LiveEvent {l.Name}. The archived Asset and Streaming URLs are still retained for on-demand viewing.");
                }

                if (l.ResourceState == LiveEventResourceState.Running){
                    client.LiveEvents.Stop(resourceGroup, accountName, l.Name);
                    Console.WriteLine($"LiveEvent: {l.Name} Stopped.");
                    client.LiveEvents.Delete(resourceGroup, accountName, l.Name);
                    Console.WriteLine($"LiveEvent: {l.Name} Deleted.");
                    Console.WriteLine();
                }
            }
        }
    } 
    catch(ApiErrorException e)
    {
        Console.WriteLine("Hit ApiErrorException");
        Console.WriteLine($"\tCode: {e.Body.Error.Code}");
        Console.WriteLine($"\tCode: {e.Body.Error.Message}");
        Console.WriteLine();

    }
}
```        

## <a name="watch-the-event"></a>Olayı izleme

Etkinliği izlemek için, [StreamingLocator oluşturma](#create-a-streaminglocator)'da açıklanan kodu çalıştırdığınızda elde ettiğiniz akış URL'sini kopyalayın ve istediğiniz bir oynatıcıyı kullanın. Akışınızı http://ampdemo.azureedge.net üzerinde test etmek için [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html)'ı kullanabilirsiniz. 

Canlı olay durduğunda otomatik olarak isteğe bağlı içeriğe dönüşür. Olayı durdurduktan ve sildikten sonra dahi, varlığı silmeniz sürece, kullanıcılar arşivlenen içeriğinizin isteğe bağlı içerik olarak akışını gerçekleştirebilir. Bir olay tarafından kullanılıyorsa varlık silinemez; önce olayın silinmesi gerekir. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz Media Services ve depolama hesapları dahil olmak üzere, kaynak grubunuzdaki kaynaklardan herhangi birine artık ihtiyacınız yoksa kaynak grubunu silebilirsiniz. **CloudShell** aracını kullanabilirsiniz.

**CloudShell**’de aşağıdaki komutu yürütün:

```azurecli-interactive
az group delete --name amsResourceGroup
```

> [!IMPORTANT]
> LiveEvent'in çalışır durumda bırakılması maliyet oluşturur. Projenin/programın çökmesi veya herhangi bir nedenle kapatılması durumunda LiveEvent'in maliyet oluşturacak şekilde çalışır durumda kalabileceğini unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

[Dosyaları akışla aktarma](stream-files-tutorial-with-api.md)

