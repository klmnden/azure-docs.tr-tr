---
title: Azure Media Services v3 ile canlı Stream | Microsoft Docs
description: Bu öğretici, .NET Core kullanarak Media Services v3 ile canlı akış yapmayı adım adım anlatılmaktadır.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 12/28/2018
ms.author: juliako
ms.openlocfilehash: 858c062c2b3d61b38247e323bf70d2768d33b257
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2019
ms.locfileid: "53969344"
---
# <a name="tutorial-stream-live-with-media-services-v3-using-apis"></a>Öğretici: Media Services v3 ile canlı Stream API'leri kullanma

Azure Media Services, [LiveEvents](https://docs.microsoft.com/rest/api/media/liveevents) canlı akış içeriğinin işlemekten sorumludur. Bir LiveEvent, daha sonra canlı bir kodlayıcıya sağladığınız bir giriş uç noktası (alma URL'si) sağlar. LiveEvent, canlı giriş akışlarını canlı kodlayıcıdan alır ve bir veya daha fazla [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints) aracılığıyla akış için kullanıma açar. LiveEvent'ler, ayrıca, akışınızı yapılacak başka işlemlerden ve sunumdan önce doğrulamak amacıyla incelemek için kullanacağınız bir önizleme uç noktası (önizleme URL'si) sağlar. Bu öğretici, **geçiş** türü bir canlı olay oluşturmak için .NET Core kullanmayı göstermektedir. 

> [!NOTE]
> Devam etmeden önce [Media Services v3 ile canlı akış](live-streaming-overview.md) konusunu gözden geçirmeyi unutmayın. 

Öğretici şunların nasıl yapıldığını göstermektedir:    

> [!div class="checklist"]
> * Media Services API’sine erişim
> * Örnek uygulamayı yapılandırma
> * Canlı akışı gerçekleştiren kodu inceleme
> * Olayı [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) ile http://ampdemo.azureedge.net üzerinde izleme
> * Kaynakları temizleme

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Öğreticiyi tamamlamak için aşağıdakiler gereklidir.

- Visual Studio Code veya Visual Studio yükleyin.
- Yükleyin ve bu makalede Azure CLI 2.0 veya sonraki bir sürüm gerektirir, CLI'yı yerel olarak kullanın. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

    Şu anda tüm [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref) komutlar Azure Cloud Shell içinde çalışır. CLI'yi yerel olarak kullanmak için önerilir.

- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md).

    Media Services hesap adını ve kaynak grubu adı için kullanılan değerleri unutmayın emin olun

- Etkinlik yayınlamak için kullanılan bir kamera veya (dizüstü gibi) bir cihaz.
- Şirket içi bir canlı kodlayıcı, kameradan alınan sinyalleri Media Services canlı akış hizmetine gönderilen akışlara dönüştürür. Akışın **RTMP** veya **Kesintisiz Akış** biçiminde olması gerekir.

## <a name="download-the-sample"></a>Örneği indirme

Aşağıdaki komutu kullanarak, akış .NET örneğini içeren bir GitHub havuzunu makinenize kopyalayın:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```

Canlı akış örneği [Live](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/Live/MediaV3LiveApp) klasöründe bulunabilir.

> [!IMPORTANT]
> Bu örnek, her kaynak için benzersiz bir sonek kullanır. Hata ayıklamayı iptal eder veya uygulamayı baştan sona çalıştırmadan sonlandırırsanız, hesabınızda birden çok LiveEvent oluşur. <br/>
> Çalışan LiveEvent'leri durdurduğunuzdan emin olun. Aksi takdirde **faturalandırılırsınız**!

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="examine-the-code-that-performs-live-streaming"></a>Canlı akışı gerçekleştiren kodu inceleme

Bu bölümde, *MediaV3LiveApp* projesinin [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs) dosyasında tanımlı işlevler incelenmektedir.

Örnek, temizlik yapmadan birden çok kez çalıştırırsanız ad çakışmaları olmaması için her kaynağa benzersiz bir sonek oluşturur.

> [!IMPORTANT]
> Bu örnek, her kaynak için benzersiz bir sonek kullanır. Hata ayıklamayı iptal eder veya uygulamayı baştan sona çalıştırmadan sonlandırırsanız, hesabınızda birden çok LiveEvent oluşur. <br/>
> Çalışan LiveEvent'leri durdurduğunuzdan emin olun. Aksi takdirde **faturalandırılırsınız**!
 
### <a name="start-using-media-services-apis-with-net-sdk"></a>.NET SDK ile Media Services API’sini kullanmaya başlama

.NET ile Media Services API’lerini kullanmaya başlamak için bir **AzureMediaServicesClient** nesnesi oluşturmanız gerekir. Nesneyi oluşturmak için, Azure AD kullanarak Azure’a bağlanmak üzere istemcinin ihtiyaç duyduğu kimlik bilgilerini sağlamanız gerekir. Makalenin başlangıcında kopyaladığınız koddaki **GetCredentialsAsync** işlevi, yerel yapılandırma dosyasında sağlanan kimlik bilgilerini temel alan ServiceClientCredentials nesnesini oluşturur. 

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateMediaServicesClient)]

### <a name="create-a-live-event"></a>Canlı etkinlik oluşturma

Bu bölümde **geçiş** türü bir LiveEvent oluşturma gösterilmektedir (LiveEventEncodingType None olarak ayarlıdır). Gerçek zamanlı kodlama için standart LiveEventEncodingType kümesi için etkinleştirilen bir Livestream oluşturmak istiyorsanız. 

Canlı etkinliği oluştururken belirtmek isteyebileceğiniz bazı başka şeyler:

* Media Services konumu 
* Canlı Etkinliğin akış protokolü (şu anda RTMP ve Kesintisiz Akış protokolleri desteklenmektedir)
       
    LiveEvent veya ilişkili LiveOutput'ları çalışırken protokol seçeneğini değiştiremezsiniz. Farklı protokollere gerek duyuyorsanız, her akış protokolü için ayrı bir LiveEvent oluşturmanız gerekir.  
* Alma ve önizleme için IP kısıtlamaları. Bu LiveEvent'e bir video almasına izin verilen IP adreslerini tanımlayabilirsiniz. İzin verilen IP adresleri tek bir IP adresi (örneğin '10.0.0.1'), bir IP adresi ve CIDR alt ağ maskesi kullanan bir IP aralığı (örneğin '10.0.0.1/22') veya bir IP adresi ve bir noktalı ondalık alt ağ maskesi kullanan bir IP aralığı (örneğin '10.0.0.1(255.255.252.0)') olabilir.
    
    Herhangi bir IP adresi belirtilmezse ve bir kural tanımı yoksa hiçbir IP adresine izin verilmez. Tüm IP adreslerine izin vermek için, bir kural oluşturun ve 0.0.0.0/0 olarak ayarlayın.

Etkinlik oluştururken, etkinliğin otomatik başlatılmasını belirtebilirsiniz. 

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateLiveEvent)]

### <a name="get-ingest-urls"></a>Alma URL’leri alma

Livestream oluşturulduktan sonra alabilirsiniz gerçek zamanlı kodlayıcıya sağlayacağınız URL'lerini alabilirsiniz. Kodlayıcı bu URL'leri canlı akış girişi için kullanır.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#GetIngestURL)]

### <a name="get-the-preview-url"></a>Önizleme URL'sini alma

Kodlayıcıdan gelen girişin gerçekten alındığını önizlemek ve doğrulamak için PreviewEndpoint kullanın.

> [!IMPORTANT]
> Devam etmeden önce videonun Önizleme URL'sine aktığından emin olun!

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#GetPreviewURLs)]

### <a name="create-and-manage-liveevents-and-liveoutputs"></a>LiveEvent ve LiveOutput oluşturma ve yönetme

Akışın LiveEvent'e akmasını sağladıktan sonra bir Asset, LiveOutput ve StreamingLocator oluşturarak etkinliği akışla göndermeye başlayabilirsiniz. Bu, akışı arşivler ve akışın StreamingEndpoint aracılığıyla izleyiciler tarafından kullanılabilmesini sağlar. 

#### <a name="create-an-asset"></a>Asset oluşturma

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateAsset)]

LiveOutput'un kullanması için bir Asset oluşturun.

#### <a name="create-a-liveoutput"></a>LiveOutput oluşturma

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateLiveOutput)]

#### <a name="create-a-streaminglocator"></a>StreamingLocator oluşturma

> [!NOTE]
> Media Services hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateStreamingLocator)]

```csharp

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

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupLiveEventAndOutput)]

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupLocatorAssetAndStreamingEndpoint)]

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupAccount)]   

## <a name="watch-the-event"></a>Olayı izleme

Etkinliği izlemek için, [StreamingLocator oluşturma](#create-a-streaminglocator)'da açıklanan kodu çalıştırdığınızda elde ettiğiniz akış URL'sini kopyalayın ve istediğiniz bir oynatıcıyı kullanın. Akışınızı http://ampdemo.azureedge.net üzerinde test etmek için [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html)'ı kullanabilirsiniz. 

Canlı olay durduğunda otomatik olarak isteğe bağlı içeriğe dönüşür. Olayı durdurduktan ve sildikten sonra dahi, varlığı silmeniz sürece, kullanıcılar arşivlenen içeriğinizin isteğe bağlı içerik olarak akışını gerçekleştirebilir. Bir olay tarafından kullanılıyorsa varlık silinemez; önce olayın silinmesi gerekir. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz Media Services ve depolama hesapları dahil olmak üzere, kaynak grubunuzdaki kaynaklardan herhangi birine artık ihtiyacınız yoksa kaynak grubunu silebilirsiniz.

Aşağıdaki CLI komutunu yürütün:

```azurecli-interactive
az group delete --name amsResourceGroup
```

> [!IMPORTANT]
> LiveEvent'in çalışır durumda bırakılması maliyet oluşturur. Projenin/programın çökmesi veya herhangi bir nedenle kapatılması durumunda LiveEvent'in maliyet oluşturacak şekilde çalışır durumda kalabileceğini unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

[Dosyaları akışla aktarma](stream-files-tutorial-with-api.md)

