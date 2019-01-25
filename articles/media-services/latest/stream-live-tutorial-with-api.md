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
ms.date: 01/22/2019
ms.author: juliako
ms.openlocfilehash: c59ebc0672970c6ee8d00daae373036e2768e318
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54888201"
---
# <a name="tutorial-stream-live-with-media-services-v3-using-apis"></a>Öğretici: Media Services v3 ile canlı Stream API'leri kullanma

Azure Media Services, [Canlı olayları](https://docs.microsoft.com/rest/api/media/liveevents) canlı akış içeriğinin işlemekten sorumludur. Canlı bir olay giriş uç noktası sağlar (alma URL'si) sonra bir gerçek zamanlı kodlayıcıya sağlamak. Canlı olay gerçek zamanlı Kodlayıcı giriş Canlı akışları alır ve bir veya daha fazla akış için kullanılabilir hale getirir [akış uç noktalarını](https://docs.microsoft.com/rest/api/media/streamingendpoints). LiveEvent'ler, ayrıca, akışınızı yapılacak başka işlemlerden ve sunumdan önce doğrulamak amacıyla incelemek için kullanacağınız bir önizleme uç noktası (önizleme URL'si) sağlar. Bu öğretici, **geçiş** türü bir canlı olay oluşturmak için .NET Core kullanmayı göstermektedir. 

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
> Çalışan Canlı olayları durdurduğunuzdan emin olun. Aksi takdirde **faturalandırılırsınız**!

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="examine-the-code-that-performs-live-streaming"></a>Canlı akışı gerçekleştiren kodu inceleme

Bu bölümde, *MediaV3LiveApp* projesinin [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs) dosyasında tanımlı işlevler incelenmektedir.

Örnek, temizlik yapmadan birden çok kez çalıştırırsanız ad çakışmaları olmaması için her kaynağa benzersiz bir sonek oluşturur.

> [!IMPORTANT]
> Bu örnek, her kaynak için benzersiz bir sonek kullanır. Hata ayıklama iptal veya uygulama üzerinden çalışan olmadan sonlandırın, birden çok canlı olay ile hesabınızı elde edebilirsiniz. <br/>
> Çalışan Canlı olayları durdurduğunuzdan emin olun. Aksi takdirde **faturalandırılırsınız**!
 
### <a name="start-using-media-services-apis-with-net-sdk"></a>.NET SDK ile Media Services API’sini kullanmaya başlama

.NET ile Media Services API’lerini kullanmaya başlamak için bir **AzureMediaServicesClient** nesnesi oluşturmanız gerekir. Nesneyi oluşturmak için, Azure AD kullanarak Azure’a bağlanmak üzere istemcinin ihtiyaç duyduğu kimlik bilgilerini sağlamanız gerekir. Makalenin başlangıcında kopyaladığınız koddaki **GetCredentialsAsync** işlevi, yerel yapılandırma dosyasında sağlanan kimlik bilgilerini temel alan ServiceClientCredentials nesnesini oluşturur. 

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateMediaServicesClient)]

### <a name="create-a-live-event"></a>Canlı etkinlik oluşturma

Bu bölüm nasıl oluşturulacağını gösterir. bir **doğrudan** canlı olay (LiveEventEncodingType hiçbiri olarak ayarlayın) türü. Bir Live LiveEventEncodingType kümesi gerçek zamanlı kodlama için etkinleştirilen olay oluşturmak istiyorsanız **standart**. 

Canlı etkinliği oluştururken belirtmek isteyebileceğiniz bazı başka şeyler:

* Media Services konumu 
* Canlı olay için Akış Protokolü (şu anda RTMP ve kesintisiz akış protokollerini desteklenmektedir).<br/>Canlı olay veya kendi ilişkili Canlı çıkış çalışıyor durumdayken protokol seçeneğini değiştiremezsiniz. Farklı protokollere ihtiyacınız varsa, her bir akış protokolü için ayrı canlı olay oluşturmanız gerekir.  
* Alma ve önizleme için IP kısıtlamaları. Bu canlı olay video almasına izin verilen IP adreslerini tanımlayabilirsiniz. İzin verilen IP adresleri tek bir IP adresi (örneğin '10.0.0.1'), bir IP adresi ve CIDR alt ağ maskesi kullanan bir IP aralığı (örneğin '10.0.0.1/22') veya bir IP adresi ve bir noktalı ondalık alt ağ maskesi kullanan bir IP aralığı (örneğin '10.0.0.1(255.255.252.0)') olabilir.<br/>Herhangi bir IP adresi belirtilmezse ve bir kural tanımı yoksa hiçbir IP adresine izin verilmez. Tüm IP adreslerine izin vermek için, bir kural oluşturun ve 0.0.0.0/0 olarak ayarlayın.<br/>IP adresleri aşağıdaki biçimlerden birinde olması gerekir: IPv4 adresi 4 sayılarla CIDR adres aralığı.
* Etkinlik oluştururken, etkinliğin otomatik başlatılmasını belirtebilirsiniz. <br/>Autostart canlı olay true olarak ayarlandığında, oluşturulduktan sonra başlatılacak. Bu, canlı olay startsrunning olan en kısa sürede fatura başlatır anlamına gelir. Daha fazla faturalama durdurmak için canlı olay kaynağı durdurma açıkça çağırmanız gerekir. Daha fazla bilgi için [canlı olay durumları ve faturalandırma](live-event-states-billing.md).

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateLiveEvent)]

### <a name="get-ingest-urls"></a>Alma URL’leri alma

Canlı olay oluşturulduktan sonra alabilirsiniz gerçek zamanlı kodlayıcıya sağlayacağınız URL'lerini alabilirsiniz. Kodlayıcı bu URL'leri canlı akış girişi için kullanır.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#GetIngestURL)]

### <a name="get-the-preview-url"></a>Önizleme URL'sini alma

Kodlayıcıdan gelen girişin gerçekten alındığını önizlemek ve doğrulamak için PreviewEndpoint kullanın.

> [!IMPORTANT]
> Devam etmeden önce videonun Önizleme URL'sine aktığından emin olun!

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#GetPreviewURLs)]

### <a name="create-and-manage-live-events-and-live-outputs"></a>Canlı olayları ve canlı çıktılar oluşturma ve yönetme

Canlı olay giden akış oluşturduktan sonra bir varlık, Canlı çıkış ve akış Bulucu oluşturarak akış olayını başlayabilirsiniz. Bu olay, akışı arşivler ve akışın Akış Uç Noktası aracılığıyla izleyiciler tarafından kullanılabilmesini sağlar. 

#### <a name="create-an-asset"></a>Asset oluşturma

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateAsset)]

Kullanılacak Canlı çıkışı için bir varlık oluşturun.

#### <a name="create-a-live-output"></a>Canlı bir çıkış oluşturma

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateLiveOutput)]

#### <a name="create-a-streaming-locator"></a>Akış Bulucusu oluşturma

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
* Canlı etkinliği durdurmak. Canlı olay durdurulduktan sonra herhangi bir ücret alınmayacaktır. Tekrar başlatmanız gerektiğinde, aynı alma URL’sine sahip olacağından kodlayıcıyı yeniden yapılandırmanız gerekmez.
* Canlı olayınızın arşivini isteğe bağlı bir akış olarak sunmaya devam etmek istemiyorsanız Akış Uç Noktanızı durdurabilirsiniz. Canlı olay durdurulmuş durumda ise, herhangi bir ücret alınmayacaktır.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupLiveEventAndOutput)]

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupLocatorAssetAndStreamingEndpoint)]

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupAccount)]   

## <a name="watch-the-event"></a>Olayı izleme

Olay izlemek için kod açıklanan çalıştırdığınızda aldığınız akış URL'sini kopyalayın [bir akış Bulucu](#create-a-streaminglocator) ve tercih ettiğiniz bir oynatıcı kullanın. Akışınızı http://ampdemo.azureedge.net üzerinde test etmek için [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html)'ı kullanabilirsiniz. 

Canlı olay durduğunda otomatik olarak isteğe bağlı içeriğe dönüşür. Olayı durdurduktan ve sildikten sonra dahi, varlığı silmeniz sürece, kullanıcılar arşivlenen içeriğinizin isteğe bağlı içerik olarak akışını gerçekleştirebilir. Bir olay tarafından kullanılıyorsa varlık silinemez; önce olayın silinmesi gerekir. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz Media Services ve depolama hesapları dahil olmak üzere, kaynak grubunuzdaki kaynaklardan herhangi birine artık ihtiyacınız yoksa kaynak grubunu silebilirsiniz.

Aşağıdaki CLI komutunu yürütün:

```azurecli-interactive
az group delete --name amsResourceGroup
```

> [!IMPORTANT]
> Çalışan canlı olay bırakarak fatura maliyetleri doğurur. Proje/program kilitlendiğinde veya varsa unutmayın herhangi bir nedenle kapalı, bir faturalama durumda çalışan canlı olay bırakabilir.

## <a name="next-steps"></a>Sonraki adımlar

[Dosyaları akışla aktarma](stream-files-tutorial-with-api.md)
 
