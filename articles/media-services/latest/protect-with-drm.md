---
title: Azure Media Services ile DRM dinamik şifreleme lisans teslim hizmetini kullanma | Microsoft Docs
description: Azure Media Services'ı kullanarak Microsoft PlayReady, Google Widevine veya Apple FairPlay lisansları ile şifrelenmiş akışlarınızı sunabilirsiniz.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/10/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: f53ae122e9888f3e537a3557b6ac5bd76856c2eb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60995882"
---
# <a name="use-drm-dynamic-encryption-and-license-delivery-service"></a>DRM dinamik şifreleme ve lisans teslim hizmetini kullanma

Azure Media Services’ı kullanarak [PlayReady dijital hak yönetimi (DRM)](https://www.microsoft.com/playready/overview/) korumalı MPEG-DASH, Kesintisiz Akış ve HTTP Canlı Akış (HLS) akışları sunabilirsiniz. Media Services ile **Google Widevine** DRM lisanslarıyla şifrelenmiş DASH akışlarını da sunabilirsiniz. PlayReady ve Widevine, ortak şifreleme (ISO/IEC 23001-7 CENC) belirtimi uyarınca şifrelenir. Media Services ayrıca HLS içeriğinizi **Apple FairPlay** (AES-128 CBC) ile şifrelemenizi de sağlar. 

Media Services ayrıca PlayReady, Widevine ve FairPlay DRM lisansları teslim etmek üzere bir hizmet sunar. Kullanıcı DRM korumalı içerik istediğinde, oynatıcı uygulaması Media Services lisans hizmetinden bir lisans ister. Oynatıcı uygulaması yetkiliyse Media Services lisans hizmeti tarafından oynatıcıya bir lisans verilir. Lisans, istemci oynatıcısının içeriğin şifresini çözmek ve akışını yapmak için kullanabileceği şifre çözme anahtarını içerir.

Bu makalede, [DRM ile şifreleme](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM) örneği kullanılmıştır. Bu örnekte diğerlerine ek olarak aşağıdaki konularla da ilgili bilgi verilmektedir:

* Uyarlamalı bit hızı kodlaması için yerleşik bir ön ayar kullanan şifreleme Dönüşümü oluşturma ve doğrudan bir [HTTPs kaynak URL'sinden](job-input-from-http-how-to.md) dosya alma.
* Belirtecinizin doğrulanması için kullanılan imzalama anahtarını ayarlama.
* Anahtarların belirtilen yapılandırmayla teslim edilmesi için uyulması gereken içerik anahtarı ilkesi gereksinimlerini (kısıtlamalarını) belirleme. 

    * Yapılandırma 
    
        Bu örnekte [PlayReady](playready-license-template-overview.md) ve [Widevine](widevine-license-template-overview.md) lisansları, Media Services lisans teslim hizmetiyle teslim edilebilecek şekilde yapılandırılmıştır. Bu örnek uygulamada [FairPlay](fairplay-license-overview.md) lisansı yapılandırılmıyor olsa da FairPlay yapılandırması için kullanabileceğiniz bir metot sunulmaktadır. Dilerseniz FairPlay yapılandırmasını ek bir seçenek olarak ekleyebilirsiniz.

    * Kısıtlama

        Uygulama, ilkeye JWT belirteci türü kısıtlaması uygular.

* Belirtilen varlık için belirtilen akış ilkesi adıyla bir StreamingLocator oluşturun. Bu durumda önceden tanımlanmış ilke kullanılmaktadır. İki içerik anahtarı üzerinde StreamingLocator ayarlar: AES-128 (Zarf) ve CENC (PlayReady ve Widevine).  
    
    StreamingLocator oluşturulduktan sonra çıkış varlığı yayımlanır ve istemciler tarafından kayıttan yürütülebilir.

    > [!NOTE]
    > Akış yapmak istediğiniz StreamingEndpoint öğesinin Çalışıyor durumunda olduğundan emin olun.

* Azure Media Player için hem DASH bildirimini hem de şifrelenmiş PlayReady içeriğini kayıttan yürütmek için gerekli olan PlayReady belirtecini içeren bir URL oluşturun. Bu örnekte belirtecin geçerlilik süresi 1 saat olarak belirlenmiştir. 

    Bir tarayıcı açın ve oluşturulan URL'yi yapıştırarak URL'nin ve belirtecin sizin için doldurulmuş olduğu Azure Media Player tanıtım sayfasını açabilirsiniz.  

    ![DRM ile koruma](./media/protect-with-drm/playready_encrypted_url.png)

> [!NOTE]
> Her bir varlığı birden fazla şifreleme türü (AES-128, PlayReady, Widevine, FairPlay) ile şifreleyebilirsiniz. Birlikte kullanılabilecek türler hakkında bilgi almak için bkz. [Akış protokolleri ve şifreleme türleri](content-protection-overview.md#streaming-protocols-and-encryption-types).

Bu makalede anlatılan örnek aşağıdaki sonucu üretir:

![AMS DRM ile korunan video](./media/protect-with-drm/ams_player.png)

## <a name="prerequisites"></a>Önkoşullar

Öğreticiyi tamamlamak için aşağıdakiler gereklidir.

* [İçerik korumaya genel bakış](content-protection-overview.md) makalesini gözden geçirin.
* [Erişim denetimi ile birden çok DRM ile içerik koruma sistemi tasarlama](design-multi-drm-system-with-access-control.md) makalesini gözden geçirin.
* Visual Studio Code veya Visual Studio yükleme
* [Bu hızlı başlangıçta](create-account-cli-quickstart.md) açıklandığı gibi yeni bir Azure Media Services hesabı oluşturun.
* [Erişim API'lerini](access-api-cli-how-to.md) kullanarak Media Services API'lerini kullanmak için gerekli kimlik bilgilerini edinin.
* Uygulama yapılandırma dosyasında (appsettings.json) gerekli değerleri ayarlayın.

## <a name="download-code"></a>Kodu indirin

Aşağıdaki komutu kullanarak, bu makalede anlatılan eksiksiz .NET örneğini içeren bir GitHub havuzunu makinenize kopyalayın:

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
"Encrypt with DRM" örneği [EncryptWithDRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM) klasöründe bulunur.

> [!NOTE]
> Örnek, uygulamayı her çalıştırdığınızda benzersiz kaynaklar oluşturur. Genelde dönüşümler ve ilkeler (var olan kaynak gerekli yapılandırmalara sahipse) gibi var olan kaynaklar yeniden kullanılır. 

## <a name="start-using-media-services-apis-with-net-sdk"></a>.NET SDK ile Media Services API’sini kullanmaya başlama

.NET ile Media Services API’lerini kullanmaya başlamak için bir **AzureMediaServicesClient** nesnesi oluşturmanız gerekir. Nesneyi oluşturmak için, Azure AD kullanarak Azure’a bağlanmak üzere istemcinin ihtiyaç duyduğu kimlik bilgilerini sağlamanız gerekir. Makalenin başlangıcında kopyaladığınız koddaki **GetCredentialsAsync** işlevi, yerel yapılandırma dosyasında sağlanan kimlik bilgilerini temel alan ServiceClientCredentials nesnesini oluşturur. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#CreateMediaServicesClient)]

## <a name="create-an-output-asset"></a>Çıktı varlığı oluşturma  

Çıktı [Varlığı](https://docs.microsoft.com/rest/api/media/assets), kodlama işinizin sonucunu depolar.  

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#CreateOutputAsset)]
 
## <a name="get-or-create-an-encoding-transform"></a>Kodlama Dönüşümü alma veya oluşturma

Yeni bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) örneği oluştururken çıktı olarak neyi üretmesi istediğinizi belirtmeniz gerekir. Gerekli parametre, aşağıdaki kodda gösterildiği gibi bir **TransformOutput** nesnesidir. Her **TransformOutput** bir **Ön ayar** içerir. **Ön ayar**, video ve/veya ses işleme işlemlerinin istenen **TransformOutput** nesnesini oluşturmak üzere kullanılacak adım adım yönergelerini açıklar. Bu makalede açıklanan örnek, **AdaptiveStreaming** adlı yerleşik bir Ön Ayar kullanır. Ön Ayar, giriş çözünürlüğü ve bit hızını temel alarak, giriş videosunu otomatik olarak oluşturulan bir bit hızı basamağına (bit hızı-çözünürlük çiftleri) kodlar ve her bir bit hızı-çözünürlük çiftine karşılık gelen H.264 video ve AAC sesi ile ISO MP4 dosyaları üretir. 

Yeni bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) oluşturmadan önce ilk olarak aşağıdaki kodda gösterildiği gibi **Get** yöntemi ile bir dönüşümün zaten var olup olmadığını denetlemeniz gerekir.  Media Services v3’te varlıklar üzerindeki **Get** yöntemleri, varlığın mevcut olmaması durumunda **null** değerini döndürür (büyük/küçük harfe duyarlı ad denetimi).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#EnsureTransformExists)]

## <a name="submit-job"></a>İşi gönderme

Yukarıda bahsedildiği gibi [Transform](https://docs.microsoft.com/rest/api/media/transforms) nesnesi tarif, [Job](https://docs.microsoft.com/rest/api/media/jobs) ise bu **Transform** nesnesini belirli bir giriş videosu veya ses içeriğine uygulamak için Media Services’e gönderilen gerçek istektir. **İş** giriş videosunun konumu ve çıktının konumu gibi bilgileri belirtir.

Bu öğreticide işin girişini doğrudan bir [HTTPs kaynak URL'sinden](job-input-from-http-how-to.md) alınan bir dosyaya göre oluşturacaksınız.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#SubmitJob)]

## <a name="wait-for-the-job-to-complete"></a>İşin tamamlanmasını bekleyin

İşin tamamlanması biraz sürüyor ve tamamlandığında bildirim almak istiyorsunuz. Aşağıdaki kod örneği, [İş](https://docs.microsoft.com/rest/api/media/jobs)’in durumu için hizmette nasıl yoklama yapılacağını gösterir. Yoklama, olası gecikme süresi nedeniyle üretim uygulamaları için önerilen en iyi uygulamalardan biri değildir. Yoklama, bir hesap üzerinde gereğinden fazla kullanılırsa kısıtlanabilir. Geliştiricilerin onun yerine Event Grid kullanmalıdır. Bkz. [Olayları özel bir web uç noktasına yönlendirme](job-state-events-cli-how-to.md).

**İş** genellikle şu durumlardan geçer: **Zamanlanmış**, **sıraya alınan**, **işleme**, **tamamlandı** (son durumu). İş bir hatayla karşılaştıysa **Hata** durumunu alırsınız. İş iptal edilme sürecindeyse **İptal Ediliyor** ve **İptal Edildi** durumunu alırsınız.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#WaitForJobToFinish)]

## <a name="create-a-contentkeypolicy"></a>ContentKeyPolicy oluşturma

İçerik anahtarı, Varlıklarınıza güvenli bir şekilde erişilmesini sağlar. İçerik anahtarının son kullanıcılarına nasıl teslim edileceğini yapılandıran bir içerik anahtarı ilkesi oluşturmanız gerekir. İçerik anahtarı, StreamingLocator ile ilişkilendirilir. Media Services ayrıca şifreleme anahtarlarını ve lisansları yetkili kullanıcılara teslim eden anahtar teslim hizmetini de sunar. 

Anahtarların belirtilen yapılandırmayla teslim edilmesi için uyulması gereken içerik anahtarı ilkesi gereksinimlerini (kısıtlamalarını) belirlemeniz gerekir. Bu örnekte aşağıdaki yapılandırmaları ve gereksinimleri belirleyeceğiz:

* Yapılandırma 

    [PlayReady](playready-license-template-overview.md) ve [Widevine](widevine-license-template-overview.md) lisansları, Media Services lisans teslim hizmetiyle teslim edilebilecek şekilde yapılandırılmıştır. Bu örnek uygulamada [FairPlay](fairplay-license-overview.md) lisansı yapılandırılmıyor olsa da FairPlay yapılandırması için kullanabileceğiniz bir metot sunulmaktadır. FairPlay yapılandırmasını ek bir seçenek olarak ekleyebilirsiniz.

* Kısıtlama

    Uygulama, ilkeye JWT belirteci türü kısıtlaması uygular.

Oynatıcı bir akış isteğinde bulunduğunda Media Services belirtilen anahtarı kullanarak içeriğinizi dinamik olarak şifreler. Oynatıcı, akışın şifresini çözmek için anahtar teslim hizmetinden anahtarı ister. Hizmet, kullanıcının anahtarı alma yetkisine sahip olup olmadığını belirlemek için anahtar için belirlediğiniz içerik anahtarı ilkesini değerlendirir.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="create-a-streaminglocator"></a>StreamingLocator oluşturma

Kodlama tamamlandıktan ve içerik anahtarı ilkesi ayarlandıktan sonra sıradaki adım, çıktı Varlığındaki videoyu yürütmek için istemcilerin kullanımına sunmaktır. Bunu iki adımda gerçekleştirirsiniz: 

1. [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) oluşturma
2. İstemcilerin kullanabileceği akış URL'sini oluşturma. 

**StreamingLocator** oluşturma işlemine yayımlama denir. Varsayılan olarak **StreamingLocator**, API çağrılarını yapmanızdan hemen sonra geçerli olur ve isteğe bağlı başlangıç ve bitiş süreleri yapılandırmadıkça silinene kadar devam eder. 

Bir [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) oluştururken istenen **StreamingPolicyName** değerini belirtmeniz gerekir. Bu öğreticide Azure Media Services'a içeriğin akış için nasıl yayımlanacağını belirten bir önceden tanımlı StreamingPolicies öğesi kullanılmaktadır. Bu örnekte StreamingLocator.StreamingPolicyName öğesini "Predefined_MultiDrmCencStreaming" ilkesi olarak belirliyoruz. Bu ilke, bulucuda iki içerik anahtarı (zarf ve CENC) oluşturulmasını ve ayarlanmasını istediğinizi belirtir. Bu nedenle zarf, PlayReady ve Widevine şifrelemeleri uygulanır (anahtar, yapılandırılan DRM lisanslarına göre kayıttan yürütme istemcisine teslim edilir). Akışınızı CBCS (FairPlay) ile de şifrelemek isterseniz "Predefined_MultiDrmStreaming" öğesini kullanın. 

> [!IMPORTANT]
> Özel [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies)’yi kullanırken Media Service hesabınız için bu tür ilkelerin sınırlı bir kümesini tasarlamanız ve aynı şifreleme seçenekleri ve protokoller gerekli olduğunda StreamingLocators için bunları kullanmanız gerekir. Media Service hesabınızda StreamingPolicy girişlerinin sayısı için bir kota bulunur. Her StreamingLocator için yeni bir StreamingPolicy oluşturmamanız gerekir.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#CreateStreamingLocator)]

## <a name="get-a-test-token"></a>Test belirteci alma
        
Bu öğreticide içerik anahtarı ilkesi için belirteç sınırlaması getirilmektedir. Belirteç kısıtlamalı ilkenin beraberinde bir güvenlik belirteci hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Media Services, [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) biçimindeki belirteçleri destekler ve örnekte de bu belirteç yapılandırılmaktadır.

ContentKeyPolicy içinde kullanılan ContentKeyIdentifierClaim, anahtar teslim hizmetine sunulan belirtecin içinde ContentKey tanımlayıcısına sahip olması gerektiğini belirtir. Bu örnekte StreamingLocator oluşturmak için bir içerik anahtarı belirtmiyoruz, sistem bizim için rastgele bir anahtar oluşturuyor. Test belirtecini oluşturmak için ContentKeyIdentifierClaim talebine yerleştirilecek ContentKeyId değerini almamız gerekiyor.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetToken)]

## <a name="build-a-dash-streaming-url"></a>DASH akış URL'si oluşturma

Artık [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) oluşturulduğuna göre, akış URL'lerini alabilirsiniz. Bir URL derlemek için [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints) konak adı ile **StreamingLocator** yolunu birleştirmeniz gerekir. Bu örnekte *varsayılan* **StreamingEndpoint** değeri kullanılır. İlk kez Media Service hesabı oluşturduğunuzda bu *varsayılan* **StreamingEndpoint** durdurulmuş durumdadır, bu nedenle **Start** çağrısı yapmanız gerekir.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetMPEGStreamingUrl)]

## <a name="clean-up-resources-in-your-media-services-account"></a>Media Services hesabınızdaki kaynakları temizleme

Genellikle, yeniden kullanmayı planladığınız nesneler dışında her şeyi temizlemeniz gerekir (genellikle Dönüşümleri yeniden kullanırsınız ve StreamingLocators vb. nesneleri tutarsınız). Deneme sonrasında hesabınızın temiz olmasını istiyorsanız, yeniden kullanmayı planlamadığınız kaynakları silmeniz gerekir.  Örneğin, aşağıdaki kod İşleri siler.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#CleanUp)]

## <a name="next-steps"></a>Sonraki adımlar

[AES-128 ile koruma](protect-with-aes128.md) hakkında bilgi edinin
