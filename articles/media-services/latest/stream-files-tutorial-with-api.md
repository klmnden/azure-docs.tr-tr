---
title: Karşıya yükleme, kodlama ve akışını Azure Media Services v3 ile .NET kullanarak | Microsoft Docs
description: Bir dosyayı karşıya yükleyin ve video kodlamak için bu öğreticideki adımları izleyin ve .NET kullanarak içeriğinizi Media Services v3 ile akış.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/22/2019
ms.author: juliako
ms.openlocfilehash: 66ee2c110edfdbd0e33c69d45dee8040654d421a
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65149157"
---
# <a name="tutorial-upload-encode-and-stream-videos-using-net"></a>Öğretici: .NET’i kullanarak videoları karşıya yükleme, kodlama ve akışla aktarma

Azure Media Services, çok çeşitli tarayıcılar ve cihazlar üzerinde yürütülen biçimlerini kullanarak medya dosyalarınızı kodlayın sağlar. Örneğin, içeriğinizi Apple'ın HLS veya MPEG DASH biçimlerinde akışla göndermek isteyebilirsiniz. Akışla göndermeden önce yüksek kaliteli dijital medya dosyanızı kodlamanız gerekir. Kodlama yönergeleri için bkz. [Kodlama kavramı](encoding-concept.md). Bu öğretici yerel video dosyasını karşıya yükler ve karşıya yüklenen dosyayı kodlar. Ayrıca, HTTPS URL’si aracılığıyla erişilebilir hale getirdiğiniz içerikleri de kodlayabilirsiniz. Daha fazla bilgi için bkz. [HTTP(s) URL'sinde iş girişi oluşturma](job-input-from-http-how-to.md).

![Videoyu yürütme](./media/stream-files-tutorial-with-api/final-video.png)

Bu öğretici şunların nasıl yapıldığını gösterir:    

> [!div class="checklist"]
> * Bu konu başlığı altında açıklanan örnek uygulamasını indirin
> * Karşıya yüklenen, kodlanan ve akışı yapılan kodu inceleme
> * Uygulamayı çalıştırma
> * Akış URL’sini test etme
> * Kaynakları temizleme

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

- Visual Studio yüklü değilse, [Visual Studio Community 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15)’yi edinebilirsiniz.
- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md).<br/>Media Services hesap adını ve kaynak grubu adı için kullanılan değerleri unutmayın emin olun.
- Bağlantısındaki [erişim Azure Media Services API'sine Azure CLI ile](access-api-cli-how-to.md) ve kimlik bilgilerini kaydedin. Bunları API'ye erişmek için kullanmanız gerekecektir.

## <a name="download-and-configure-the-sample"></a>İndirme ve örnek yapılandırma

Aşağıdaki komutu kullanarak, akış .NET örneğini içeren bir GitHub havuzunu makinenize kopyalayın:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```

Örnek, [UploadEncodeAndStreamFiles](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/UploadEncodeAndStreamFiles) klasöründe yer alır.

Açık [appsettings.json](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/appsettings.json) içinde proje indirilir. Aldığınız kimlik değerleri Değiştir [API'leri erişme](access-api-cli-how-to.md).

## <a name="examine-the-code-that-uploads-encodes-and-streams"></a>Karşıya yüklenen, kodlanan ve akışı yapılan kodu inceleme

Bu bölümde, *UploadEncodeAndStreamFiles* projesinin [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs) dosyasında tanımlı işlevler incelenmektedir.

Örnek aşağıdaki eylemleri gerçekleştirir:

1. Yeni bir oluşturur **dönüştürme** (ilk olarak, belirtilen dönüşüm var olup olmadığını denetler). 
2. Bir çıkış oluşturur **varlık** kodlama olarak kullanılan **iş**çıktı.
3. Bir girdi oluşturma **varlık** ve belirtilen yerel görüntü dosyası içine yükler. Varlık, işin girişi olarak kullanılır. 
4. Kodlama işinin girişi ile oluşturulan çıktı gönderir.
5. İşin durumunu denetler.
6. Oluşturur bir **akış Bulucusu**.
7. Akış URL'leri oluşturur.

### <a name="a-idstartusingdotnet-start-using-media-services-apis-with-net-sdk"></a><a id="start_using_dotnet" />.NET SDK'sı ile Media Services API'leri kullanmaya başlayın

.NET ile Media Services API’lerini kullanmaya başlamak için bir **AzureMediaServicesClient** nesnesi oluşturmanız gerekir. Nesneyi oluşturmak için, Azure AD kullanarak Azure’a bağlanmak üzere istemcinin ihtiyaç duyduğu kimlik bilgilerini sağlamanız gerekir. Makalenin başlangıcında kopyaladığınız koddaki **GetCredentialsAsync** işlevi, yerel yapılandırma dosyasında sağlanan kimlik bilgilerini temel alan ServiceClientCredentials nesnesini oluşturur. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateMediaServicesClient)]

### <a name="create-an-input-asset-and-upload-a-local-file-into-it"></a>Bir giriş varlığı oluşturma ve içine yerel dosya yükleme 

**CreateInputAsset** işlevi yeni bir giriş [Varlığı](https://docs.microsoft.com/rest/api/media/assets) oluşturur ve içine belirtilen yerel video dosyasını yükler. Bu **varlık** , kodlama işinin girdisi olarak kullanılır. Media Services v3, giriş olarak bir **iş** olabilir bir **varlık**, Media Services hesabınıza HTTPS URL'leri aracılığıyla kullanılabilir hale getirdiğiniz içerik olabilir. Bir HTTPS URL’sinden kodlama yapmayı öğrenmek için [bu](job-input-from-http-how-to.md) makaleye bakın.  

Media Services v3’te dosyaları karşıya yüklemek için Azure Depolama API’lerini kullanırsınız. Aşağıdaki .NET kod parçacığı bunun nasıl yapıldığını gösterir.

Aşağıdaki işlev şu eylemleri gerçekleştirir:

* Oluşturur bir **varlık** 
* Yazılabilir bir alır [SAS URL'si](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) varlık için [depolama kapsayıcısında](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet?tabs=windows#upload-blobs-to-the-container)
* SAS URL’sini kullanarak dosyayı depolamadaki kapsayıcıya yükler

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]

### <a name="create-an-output-asset-to-store-the-result-of-a-job"></a>Bir işin sonucunu depolamak için çıktı varlığı oluşturma 

Çıktı [Varlığı](https://docs.microsoft.com/rest/api/media/assets), kodlama işinizin sonucunu depolar. Proje, bu çıktı varlığının sonuçlarını "output" klasörüne indiren **DownloadResults** işlevini tanımlar, böylece elinizde neyin olduğunu görebilirsiniz.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateOutputAsset)]

### <a name="create-a-transform-and-a-job-that-encodes-the-uploaded-file"></a>Bir Dönüşüm ve karşıya yüklenen dosyayı kodlayan İş oluşturma

Media Services’te içerik kodlarken veya işlerken, kodlama ayarlarını bir tarif olarak ayarlamak yaygın bir modeldir. Daha sonra bu tarifi bir videoya uygulamak üzere bir **İş** gönderirsiniz. Her yeni video için yeni işleri göndererek, kitaplığınızda tüm videoları için söz konusu tarif uyguladığınızı. Media Services içinde tarif, **Dönüşüm** olarak adlandırılır. Daha fazla bilgi için [Dönüşümler ve İşler](transform-concept.md) konusuna bakın. Bu öğreticide açıklanan örnek, videoyu çeşitli iOS ve Android cihazlarına akışla aktarmak için kodlayan bir tarifi tanımlar. 

#### <a name="transform"></a>Dönüşüm

Yeni bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) örneği oluştururken çıktı olarak neyi üretmesi istediğinizi belirtmeniz gerekir. Gerekli parametre, aşağıdaki kodda gösterildiği gibi bir **TransformOutput** nesnesidir. Her **TransformOutput** bir **Ön ayar** içerir. **Ön ayar**, video ve/veya ses işleme işlemlerinin istenen **TransformOutput** nesnesini oluşturmak üzere kullanılacak adım adım yönergelerini açıklar. Bu makalede açıklanan örnek, **AdaptiveStreaming** adlı yerleşik bir Ön Ayar kullanır. Ön Ayar, giriş çözünürlüğü ve bit hızını temel alarak, giriş videosunu otomatik olarak oluşturulan bir bit hızı basamağına (bit hızı-çözünürlük çiftleri) kodlar ve her bir bit hızı-çözünürlük çiftine karşılık gelen H.264 video ve AAC sesi ile ISO MP4 dosyaları üretir. Bu Ön Ayar hakkında bilgi için bkz. [otomatik oluşturulan bit hızı basamağı](autogen-bitrate-ladder.md).

Yerleşik bir EncoderNamedPreset ön ayarını veya özel ön ayarları kullanabilirsiniz. Daha fazla bilgi için bkz. [Kodlayıcı önayarlarını özelleştirme](customize-encoder-presets-how-to.md).

Bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) oluştururken ilk olarak aşağıdaki kodda gösterildiği gibi **Get** yöntemi ile bir dönüşümün zaten var olup olmadığını denetlemeniz gerekir.  Media Services v3’te varlıklar üzerindeki **Get** yöntemleri, varlığın mevcut olmaması durumunda **null** değerini döndürür (büyük/küçük harfe duyarlı ad denetimi).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#EnsureTransformExists)]

#### <a name="job"></a>İş

Yukarıda bahsedildiği gibi [Transform](https://docs.microsoft.com/rest/api/media/transforms) nesnesi tarif, [Job](https://docs.microsoft.com/rest/api/media/jobs) ise bu **Transform** nesnesini belirli bir giriş videosu veya ses içeriğine uygulamak için Media Services’e gönderilen gerçek istektir. **İş** giriş videosunun konumu ve çıktının konumu gibi bilgileri belirtir.

Bu örnekte giriş videosu, yerel makinenizden yüklenmiştir. Bir HTTPS URL’sinden kodlama yapmayı öğrenmek için [bu](job-input-from-http-how-to.md) makaleye bakın.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#SubmitJob)]

### <a name="wait-for-the-job-to-complete"></a>İşin tamamlanmasını bekleyin

İşin tamamlanması biraz sürüyor ve tamamlandığında bildirim almak istiyorsunuz. Aşağıdaki kod örneği, [İş](https://docs.microsoft.com/rest/api/media/jobs)’in durumu için hizmette nasıl yoklama yapılacağını gösterir. Yoklama, olası gecikme süresi nedeniyle üretim uygulamaları için önerilen en iyi uygulamalardan biri değildir. Yoklama, bir hesap üzerinde gereğinden fazla kullanılırsa kısıtlanabilir. Geliştiricilerin onun yerine Event Grid kullanmalıdır.

Event Grid yüksek kullanılabilirlik, tutarlı performans ve dinamik ölçek için tasarlanmıştır. Event Grid ile uygulamalarınız neredeyse tüm Azure hizmetleri ve özel kaynaklardan gelen olayları takip edip bu olaylara yanıt verebilir. Basit, HTTP tabanlı reaktif olay işleme özelliği, olayların akıllı filtrelenmesi ve yönlendirilmesi sayesinde etkili çözümler oluşturmanıza yardımcı olur.  Bkz. [Olayları özel bir web uç noktasına yönlendirme](job-state-events-cli-how-to.md).

**İş** genellikle şu durumlardan geçer: **Zamanlanmış**, **sıraya alınan**, **işleme**, **tamamlandı** (son durumu). İş bir hatayla karşılaştıysa **Hata** durumunu alırsınız. İş iptal edilme sürecindeyse **İptal Ediliyor** ve **İptal Edildi** durumunu alırsınız.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#WaitForJobToFinish)]

### <a name="job-error-codes"></a>İş hata kodları

Bkz: [hata kodları](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode).

### <a name="get-a-streaming-locator"></a>Akış Bulucusu Al

Kodlama tamamlandıktan sonra sıradaki adım, çıktı Varlığındaki videoyu yürütmek için istemcilerin kullanımına sunmaktır. Bunu iki adımda gerçekleştirebilirsiniz: ilk olarak, oluşturun bir [akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators)ve ikinci olarak, istemcilerin kullandığı akış URL'leri oluşturun. 

Oluşturma işlemi bir **akış Bulucu** yayımlama denir. Varsayılan olarak, **akış Bulucu** API çağrılarını hemen sonra geçerli olduğunu ve isteğe bağlı bir başlangıç ve bitiş zamanlarını yapılandırmadığınız sürece silinene kadar sürer. 

Bir [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) oluştururken istenen **StreamingPolicyName** değerini belirtmeniz gerekir. Bu örnekte, temiz (şifrelenmemiş) içeriğin akışını yapacağınız için önceden tanımlı temiz akış ilkesi **PredefinedStreamingPolicy.ClearStreamingOnly** kullanılır.

> [!IMPORTANT]
> Özel bir kullanırken [akış ilke](https://docs.microsoft.com/rest/api/media/streamingpolicies), medya hizmeti hesabınız için sınırlı sayıda tür ilkeleri tasarlayın ve aynı şifreleme seçenekleri ve protokolleri gerektiğinde, StreamingLocators için yeniden kullanabilmesi gerekir. Medya hizmeti hesabınızı akış İlkesi girdi sayısı için bir kota vardır. Yeni bir akış ilke her akış Bulucu için oluşturduğunuz değil.

Aşağıdaki kod, benzersiz bir locatorName ile işlevi çağırdığınızı varsayar.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateStreamingLocator)]

Bu konudaki örnek akış anlatılır, ancak aynı çağrı, videoları aşamalı indirme aracılığıyla sunmak için bir akış Bulucusu oluşturmak için kullanabilirsiniz.

### <a name="get-streaming-urls"></a>Akış URL'leri alma

Şimdi [akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators) olmuştur oluşturulmuş akış URL'leri gösterildiği alabileceğiniz **GetStreamingURLs**. URL oluşturmak için birleştirmek gereken [akış uç noktası](https://docs.microsoft.com/rest/api/media/streamingendpoints) ana bilgisayar adı ve **akış Bulucu** yolu. Bu örnekte *varsayılan* **akış uç noktası** kullanılır. Bir medya hizmeti hesabı oluşturduğunuzda bu *varsayılan* **akış uç noktası** çağırmanız gerekir böylece durdurulmuş bir durumda olacaktır **Başlat**.

> [!NOTE]
> Bu yöntemde, oluşturulurken kullanılan locatorName ihtiyacınız **akış Bulucu** çıkış varlık.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#GetStreamingURLs)]

### <a name="clean-up-resources-in-your-media-services-account"></a>Media Services hesabınızdaki kaynakları temizleme

Genellikle, yeniden kullanmayı planladığınız nesneler dışında her şeyi temizlemeniz gerekir (genellikle Dönüşümleri yeniden kullanırsınız ve StreamingLocators vb. nesneleri tutarsınız). Deneme sonrasında hesabınızın temiz olmasını istiyorsanız, yeniden kullanmayı planlamadığınız kaynakları silmeniz gerekir.  Örneğin, aşağıdaki kod İşleri siler.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CleanUp)]

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

1. *EncodeAndStreamFiles* uygulamasını çalıştırmak için Ctrl+F5 tuşlarına basın.
2. Akış URL’lerinden birini konsoldan kopyalayın.

Bu örnekte, farklı protokolleri kullanarak videoyu kayıttan yürütmek için kullanılabilen URL’ler gösterilir:

![Çıktı](./media/stream-files-tutorial-with-api/output.png)

## <a name="test-the-streaming-url"></a>Akış URL’sini test etme

Bu makalede, akışı test etmek için Azure Media Player kullanılmaktadır. 

> [!NOTE]
> Oynatıcı bir https sitesinde barındırılıyorsa, "https" URL’sini güncelleştirdiğinizden emin olun.

1. Bir web tarayıcısı açın ve [https://aka.ms/azuremediaplayer/](https://aka.ms/azuremediaplayer/) sayfasına gidin.
2. **URL:** kutusuna, uygulamayı çalıştırdığınızda aldığınız akış URL değerlerinden birini yapıştırın. 
3. **Oynatıcıyı Güncelleştir** düğmesine basın.

Azure Media Player, test için kullanılabilir, ancak üretim ortamında kullanılmamalıdır. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz Media Services ve depolama hesapları dahil olmak üzere, kaynak grubunuzdaki kaynaklardan herhangi birine artık ihtiyacınız yoksa kaynak grubunu silebilirsiniz.

Aşağıdaki CLI komutunu yürütün:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="multithreading"></a>Çoklu iş parçacığı kullanımı

Azure Media Services v3 SDK’ları, iş parçacığı güvenli değildir. Çok iş parçacıklı uygulama geliştirirken, iş parçacığı başına yeni bir AzureMediaServicesClient nesnesi oluşturmanız ve kullanmanız gerekir.

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, görüşlerinizi, güncelleştirmeleri alın

Kullanıma [Azure Media Services topluluğu](media-services-community.md) soru sorun, görüşlerinizi ve medya hizmetleri hakkında güncelleştirmeler almak farklı yollarını görmek için makaleyi.

## <a name="next-steps"></a>Sonraki adımlar

Videonuzu karşıya yükleme, kodlama ve akışla aktarma hakkında bilgi edindiğinize göre aşağıdaki makaleye bakabilirsiniz: 

> [!div class="nextstepaction"]
> [Videoları analiz etme](analyze-videos-tutorial-with-api.md)
