---
title: Video AES-128 ile şifrelemek için Azure Media Services'i kullanma | Microsoft Docs
description: İçeriğinizi AES 128 bit şifreleme anahtarları ile Microsoft Azure Media Services'ı kullanarak şifreli sunun. Media Services, şifreleme anahtarlarını yetkili kullanıcıların sunan anahtar dağıtımı hizmetiyle de sağlar. Bu konu nasıl dinamik olarak AES-128 ile şifrelemek ve anahtar dağıtımı hizmetiyle nasıl kullanılacağını gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2019
ms.author: juliako
ms.openlocfilehash: aa6b4ef76b039e9e24b4a72cfb6e76dcfae8378d
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62111996"
---
# <a name="tutorial-use-aes-128-dynamic-encryption-and-the-key-delivery-service"></a>Öğretici: AES-128 dinamik şifreleme ve anahtar teslim hizmetini kullanma

HTTP canlı akışı (HLS), MPEG-DASH ve kesintisiz akış ile AES 128 bit şifreleme anahtarları kullanılarak şifrelenmiş sunmak için Media Services'ı kullanabilirsiniz. Media Services, şifreleme anahtarlarını yetkili kullanıcıların sunan anahtar dağıtımı hizmetiyle de sağlar. Media Services'ın dinamik olarak videonuzu şifrelemek isterseniz, şifreleme anahtarını akış Bulucu ile ilişkilendirmek ve ayrıca içerik anahtar ilkeyi yapılandırın. Bir akışa bir oynatıcı tarafından istendiğinde Media Services belirtilen anahtarı içeriğinizi AES-128 ile dinamik olarak şifrelemek için kullanır. Oynatıcı, akışın şifresini çözmek için anahtar teslim hizmetinden anahtarı ister. Hizmet, kullanıcının anahtarı alma yetkisine sahip olup olmadığını belirlemek için anahtar için belirlediğiniz içerik anahtarı ilkesini değerlendirir.

Her bir varlığı birden fazla şifreleme türü (AES-128, PlayReady, Widevine, FairPlay) ile şifreleyebilirsiniz. Birlikte kullanılabilecek türler hakkında bilgi almak için bkz. [Akış protokolleri ve şifreleme türleri](content-protection-overview.md#streaming-protocols-and-encryption-types). Ayrıca bkz [DRM ile koruma](protect-with-drm.md).

Örnek çıktısı Bu makale, Azure Media Player için bir URL, bildirim URL'sini ve içeriğini oynatmak için gereken AES belirteci içerir. Örnek, JWT belirteci süre sonu 1 saate ayarlar. Bir tarayıcı açın ve URL'yi ve sizin için zaten şu biçimde doldurulan belirteci ile Azure Media Player tanıtım sayfasını başlatmak için elde edilen URL'yi yapıştırın: ```https://ampdemo.azureedge.net/?url= {dash Manifest URL} &aes=true&aestoken=Bearer%3D{ JWT Token here}```.

Bu öğretici şunların nasıl yapıldığını gösterir:    

> [!div class="checklist"]
> * İndirme [EncryptWithAES](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES) makalesinde açıklanan örnek
> * .NET SDK ile Media Services API’sini kullanmaya başlama
> * Çıktı varlığı oluşturma
> * Bir kodlama dönüştürme oluşturma
> * Bir iş gönderdiniz
> * İşin tamamlanmasını bekleyin
> * Bir içerik anahtarı ilkesi oluşturma
> * JWT belirteci kısıtlama kullanılacak ilkeyi yapılandırma 
> * Akış Bulucusu oluşturma
> * Video AES (ClearKey) ile şifrelemek için akış Bulucu yapılandırın
> * Test belirteci alma
> * Bir akış URL'si oluşturmanıza
> * Kaynakları temizleme

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Öğreticiyi tamamlamak için aşağıdakiler gereklidir.

* Gözden geçirme [içerik korumaya genel bakış](content-protection-overview.md) makale
* Visual Studio Code veya Visual Studio yükleme
* [Bir Media Services hesabı oluşturma](create-account-cli-quickstart.md)
* [Erişim API'lerini](access-api-cli-how-to.md) kullanarak Media Services API'lerini kullanmak için gerekli kimlik bilgilerini edinin.

## <a name="download-code"></a>Kodu indirin

Aşağıdaki komutu kullanarak, bu makalede anlatılan eksiksiz .NET örneğini içeren bir GitHub havuzunu makinenize kopyalayın:

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
"Şifreleme ile AES-128" örnek bulunan [EncryptWithAES](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES) klasör.

> [!NOTE]
> Örnek, uygulamayı her çalıştırdığınızda benzersiz kaynaklar oluşturur. Genelde dönüşümler ve ilkeler (var olan kaynak gerekli yapılandırmalara sahipse) gibi var olan kaynaklar yeniden kullanılır. 

## <a name="start-using-media-services-apis-with-net-sdk"></a>.NET SDK ile Media Services API’sini kullanmaya başlama

.NET ile Media Services API’lerini kullanmaya başlamak için bir **AzureMediaServicesClient** nesnesi oluşturmanız gerekir. Nesneyi oluşturmak için, Azure AD kullanarak Azure’a bağlanmak üzere istemcinin ihtiyaç duyduğu kimlik bilgilerini sağlamanız gerekir. Makalenin başlangıcında kopyaladığınız koddaki **GetCredentialsAsync** işlevi, yerel yapılandırma dosyasında sağlanan kimlik bilgilerini temel alan ServiceClientCredentials nesnesini oluşturur. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateMediaServicesClient)]

## <a name="create-an-output-asset"></a>Çıktı varlığı oluşturma  

Çıktı [Varlığı](https://docs.microsoft.com/rest/api/media/assets), kodlama işinizin sonucunu depolar.  

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateOutputAsset)]
 
## <a name="get-or-create-an-encoding-transform"></a>Kodlama Dönüşümü alma veya oluşturma

Yeni bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) örneği oluştururken çıktı olarak neyi üretmesi istediğinizi belirtmeniz gerekir. Gerekli parametre, aşağıdaki kodda gösterildiği gibi bir **TransformOutput** nesnesidir. Her **TransformOutput** bir **Ön ayar** içerir. **Ön ayar**, video ve/veya ses işleme işlemlerinin istenen **TransformOutput** nesnesini oluşturmak üzere kullanılacak adım adım yönergelerini açıklar. Bu makalede açıklanan örnek, **AdaptiveStreaming** adlı yerleşik bir Ön Ayar kullanır. Ön Ayar, giriş çözünürlüğü ve bit hızını temel alarak, giriş videosunu otomatik olarak oluşturulan bir bit hızı basamağına (bit hızı-çözünürlük çiftleri) kodlar ve her bir bit hızı-çözünürlük çiftine karşılık gelen H.264 video ve AAC sesi ile ISO MP4 dosyaları üretir. 

Yeni bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) oluşturmadan önce ilk olarak aşağıdaki kodda gösterildiği gibi **Get** yöntemi ile bir dönüşümün zaten var olup olmadığını denetlemeniz gerekir.  Media Services v3’te varlıklar üzerindeki **Get** yöntemleri, varlığın mevcut olmaması durumunda **null** değerini döndürür (büyük/küçük harfe duyarlı ad denetimi).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#EnsureTransformExists)]

## <a name="submit-job"></a>İşi gönderme

Yukarıda bahsedildiği gibi [Transform](https://docs.microsoft.com/rest/api/media/transforms) nesnesi tarif, [Job](https://docs.microsoft.com/rest/api/media/jobs) ise bu **Transform** nesnesini belirli bir giriş videosu veya ses içeriğine uygulamak için Media Services’e gönderilen gerçek istektir. **İş** giriş videosunun konumu ve çıktının konumu gibi bilgileri belirtir.

Bu öğreticide işin girişini doğrudan bir [HTTPs kaynak URL'sinden](job-input-from-http-how-to.md) alınan bir dosyaya göre oluşturacaksınız.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#SubmitJob)]

## <a name="wait-for-the-job-to-complete"></a>İşin tamamlanmasını bekleyin

İşin tamamlanması biraz sürüyor ve tamamlandığında bildirim almak istiyorsunuz. Aşağıdaki kod örneği, [İş](https://docs.microsoft.com/rest/api/media/jobs)’in durumu için hizmette nasıl yoklama yapılacağını gösterir. Yoklama, olası gecikme süresi nedeniyle üretim uygulamaları için önerilen en iyi uygulamalardan biri değildir. Yoklama, bir hesap üzerinde gereğinden fazla kullanılırsa kısıtlanabilir. Geliştiricilerin onun yerine Event Grid kullanmalıdır. Bkz. [Olayları özel bir web uç noktasına yönlendirme](job-state-events-cli-how-to.md).

**İş** genellikle şu durumlardan geçer: **Zamanlanmış**, **sıraya alınan**, **işleme**, **tamamlandı** (son durumu). İş bir hatayla karşılaştıysa **Hata** durumunu alırsınız. İş iptal edilme sürecindeyse **İptal Ediliyor** ve **İptal Edildi** durumunu alırsınız.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#WaitForJobToFinish)]

## <a name="create-a-content-key-policy"></a>Bir içerik anahtarı ilkesi oluşturma

İçerik anahtarı, Varlıklarınıza güvenli bir şekilde erişilmesini sağlar. Oluşturmak için ihtiyacınız bir **içerik anahtarı ilke** istemcileri sonlandırmak için içerik anahtarını nasıl teslim edildiğini yapılandırır. İçerik anahtarı ile ilişkili **akış Bulucu**. Media Services, şifreleme anahtarlarını yetkili kullanıcıların sunan anahtar dağıtımı hizmetiyle de sağlar. 

Bir akışa bir oynatıcı tarafından istendiğinde Media Services belirtilen anahtarı (Bu durumda, AES şifrelemesi kullanılarak.) içeriğinizi dinamik olarak şifrelemek için kullanır. Oynatıcı, akışın şifresini çözmek için anahtar teslim hizmetinden anahtarı ister. Hizmet, kullanıcının anahtarı alma yetkisine sahip olup olmadığını belirlemek için anahtar için belirlediğiniz içerik anahtarı ilkesini değerlendirir.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="create-a-streaming-locator"></a>Akış Bulucusu oluşturma

Kodlama tamamlandıktan ve içerik anahtarı ilkesi ayarlandıktan sonra sıradaki adım, çıktı Varlığındaki videoyu yürütmek için istemcilerin kullanımına sunmaktır. Bunu iki adımda gerçekleştirirsiniz: 

1. Oluşturma bir [akış Bulucusu](https://docs.microsoft.com/rest/api/media/streaminglocators)
2. İstemcilerin kullanabileceği akış URL'sini oluşturma. 

Oluşturma işlemi **akış Bulucu** yayımlama denir. Varsayılan olarak, **akış Bulucu** API çağrılarını hemen sonra geçerli olduğunu ve isteğe bağlı bir başlangıç ve bitiş zamanlarını yapılandırmadığınız sürece silinene kadar sürer. 

Oluştururken bir [akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators), istenen belirtmeniz gerekecektir **StreamingPolicyName**. Bu öğreticide Azure Media Services'a içeriğin akış için nasıl yayımlanacağını belirten bir PredefinedStreamingPolicies öğesini kullanılmaktadır. Bu örnekte, AES zarfı şifreleme (key HTTPS ve DRM lisans yoluyla kayıttan yürütme istemciye teslim edildiğinden da ClearKey şifreleme bilinir) uygulanır.

> [!IMPORTANT]
> Özel bir kullanırken [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies), medya hizmeti hesabınız için sınırlı sayıda gibi ilkelerin tasarlamanıza ve protokolleri ve aynı şifreleme seçenekleri gerektiğinde bunları sizin için akış bulucuları aşağıdaki yeniden kullanımı. Media Service hesabınızda StreamingPolicy girişlerinin sayısı için bir kota bulunur. Yeni bir StreamingPolicy her akış Bulucu için oluşturduğunuz değil.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateStreamingLocator)]

## <a name="get-a-test-token"></a>Test belirteci alma
        
Bu öğreticide içerik anahtarı ilkesi için belirteç sınırlaması getirilmektedir. Belirteç kısıtlamalı ilkenin beraberinde bir güvenlik belirteci hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Media Services, [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) biçimindeki belirteçleri destekler ve örnekte de bu belirteç yapılandırılmaktadır.

ContentKeyIdentifierClaim kullanılan **içerik anahtarı ilke**, anahtar teslim hizmeti için sunulan belirteci içerik anahtarı tanımlayıcısı içinde olması gerektiğini anlamına gelir. Bu örnekte biz içerik eklemediğiniz akış Bulucu oluşturulurken anahtar, sistem oluşturulan bizim için rastgele bir tane. Test belirtecini oluşturmak için ContentKeyIdentifierClaim talebine yerleştirilecek ContentKeyId değerini almamız gerekiyor.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetToken)]

## <a name="build-a-dash-streaming-url"></a>DASH akış URL'si oluşturma

Şimdi [akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators) olmuştur oluşturulmuş akış URL'lerini alabilirsiniz. URL oluşturmak için birleştirmek gereken [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints) ana bilgisayar adı ve **akış Bulucu** yolu. Bu örnekte *varsayılan* **akış uç noktası** kullanılır. Bir medya hizmeti hesabı oluşturduğunuzda bu *varsayılan* **akış uç noktası** çağırmanız gerekir böylece durdurulmuş bir durumda olacaktır **Başlat**.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetMPEGStreamingUrl)]

## <a name="clean-up-resources-in-your-media-services-account"></a>Media Services hesabınızdaki kaynakları temizleme

Genel olarak, yeniden kullanmak için planlama nesneler dışında her şeyi temizlemeniz gerekir (genellikle, dönüşümler yeniden kullanır ve korunur akış Bulucular vb..). Deneme sonrasında hesabınızın temiz olmasını istiyorsanız, yeniden kullanmayı planlamadığınız kaynakları silmeniz gerekir.  Örneğin, aşağıdaki kod İşleri siler.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CleanUp)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz Media Services ve depolama hesapları dahil olmak üzere, kaynak grubunuzdaki kaynaklardan herhangi birine artık ihtiyacınız yoksa kaynak grubunu silebilirsiniz. 

Aşağıdaki CLI komutunu yürütün:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="provide-feedback"></a>Geri bildirimde bulunma

Kullanıma [Azure Media Services topluluğu](media-services-community.md) soru sorun, görüşlerinizi ve medya hizmetleri hakkında güncelleştirmeler almak farklı yollarını görmek için makaleyi.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [DRM ile koruma](protect-with-drm.md)
