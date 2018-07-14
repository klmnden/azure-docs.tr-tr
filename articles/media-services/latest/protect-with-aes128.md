---
title: Azure Media Services AES dinamik şifreleme kullanma | Microsoft Docs
description: İçeriğinizi AES 128 bit şifreleme anahtarları ile Microsoft Azure Media Services'ı kullanarak şifreli sunun. Media Services, şifreleme anahtarlarını yetkili kullanıcıların sunan anahtar dağıtımı hizmetiyle de sağlar. Bu konu nasıl dinamik olarak AES-128 ile şifrelemek ve anahtar dağıtımı hizmetiyle nasıl kullanılacağını gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: juliako
ms.openlocfilehash: b62c528716d9386b9da6ddee260fd1ec382fb4a5
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39036794"
---
# <a name="use-aes-128-dynamic-encryption-and-the-key-delivery-service"></a>AES-128 dinamik şifreleme ve anahtar teslim hizmetini kullanma

HTTP canlı akışı (HLS), MPEG-DASH ve kesintisiz akış ile AES 128 bit şifreleme anahtarları kullanılarak şifrelenmiş sunmak için Media Services'ı kullanabilirsiniz. Media Services, şifreleme anahtarlarını yetkili kullanıcıların sunan anahtar dağıtımı hizmetiyle de sağlar. Media Services için bir varlık şifrelemek isterseniz, şifreleme anahtarını StreamingLocator ile ilişkilendirmek ve ayrıca içerik anahtar ilkeyi yapılandırın. Bir akışa bir oynatıcı tarafından istendiğinde Media Services dinamik olarak içeriğinizi AES şifreleme kullanarak şifrelemek için belirtilen anahtar kullanır. Akış şifresini çözmek için player anahtar anahtar teslim hizmetinden ister. Kullanıcı anahtarı almak için yetki verilip verilmediğini belirlemek için anahtar için belirtilen içerik anahtarı ilkesi hizmet tarafından değerlendirilir.

Makale temel almaktadır [EncryptWithAES](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES) örnek. Örneği doğrudan bir dosya alır ve önceden yerleşik Uyarlamalı bit hızlı kodlama için kullandığı bir kodlama dönüştürme oluşturma işlemini gösterir bir [HTTPs kaynak URL](job-input-from-http-how-to.md). Çıktı varlığına (ClearKey) AES şifrelemesi kullanılarak yayımlanır. Örnekteki çıktı, DASH bildirimi hem AES belirtecin içeriği kayıttan yürütme için gerekli dahil olmak üzere Azure Media Player URL'dir. Örnek, JWT belirteci süre sonu 1 saate ayarlar. Bir tarayıcı açın ve URL'yi ve sizin için zaten şu biçimde doldurulan belirteci ile Azure Media Player tanıtım sayfasını başlatmak için elde edilen URL'yi yapıştırın: ```https://ampdemo.azureedge.net/?url= {dash Manifest URL} &aes=true&aestoken=Bearer%3D{ JWT Token here}```.

> [!NOTE]
> Her varlık birden fazla şifreleme türü (AES-128, PlayReady, Widevine, FairPlay) ile şifreleyebilirsiniz. Bkz: [akış protokolleri ve şifreleme türlerini](content-protection-overview.md#streaming-protocols-and-encryption-types)ne birleştirmek mantıklıdır görmek için.

## <a name="prerequisites"></a>Önkoşullar

Öğreticiyi tamamlamak için aşağıdakiler gereklidir.

* Gözden geçirme [içerik korumaya genel bakış](content-protection-overview.md) makalesi.
* Visual Studio Code veya Visual Studio yükleme
* Açıklanan şekilde yeni bir Azure Media Services hesabı oluşturma [Bu hızlı başlangıçta](create-account-cli-quickstart.md).
* Aşağıdaki kullanarak Media Services API'leri kullanmak için gerekli kimlik bilgilerini alma [erişimi API'leri](access-api-cli-how-to.md)

## <a name="download-code"></a>Kodu indir

Bu konuda aşağıdaki komutu kullanarak makinenize kopyalama tam .NET örneği içeren bir GitHub deposu ele:

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
"Şifreleme ile AES-128" örnek bulunan [EncryptWithAES](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES) klasör.

> [!NOTE]
> Örnek, uygulamayı her çalıştırdığınızda benzersiz kaynakları oluşturur. Dönüşümler ve ilkeleri gibi mevcut kaynaklar genellikle, yeniden kullanır (varsa kaynak yapılandırmaları gerekli). 

## <a name="start-using-media-services-apis-with-net-sdk"></a>.NET SDK ile Media Services API’sini kullanmaya başlama

.NET ile Media Services API’lerini kullanmaya başlamak için bir **AzureMediaServicesClient** nesnesi oluşturmanız gerekir. Nesneyi oluşturmak için, Azure AD kullanarak Azure’a bağlanmak üzere istemcinin ihtiyaç duyduğu kimlik bilgilerini sağlamanız gerekir. Makalenin başlangıcında kopyaladığınız koddaki **GetCredentialsAsync** işlevi, yerel yapılandırma dosyasında sağlanan kimlik bilgilerini temel alan ServiceClientCredentials nesnesini oluşturur. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateMediaServicesClient)]

## <a name="create-an-output-asset"></a>Çıktı varlığı oluşturma  

Çıktı [Varlığı](https://docs.microsoft.com/rest/api/media/assets), kodlama işinizin sonucunu depolar. Kodlama tamamlandıktan sonra çıktı varlığı (ClearKey) AES şifrelemesi kullanılarak yayımlanır.  

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateOutputAsset)]
 
## <a name="get-or-create-an-encoding-transform"></a>Almaya veya oluşturmaya bir kodlama dönüştürme

Yeni bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) örneği oluştururken çıktı olarak neyi üretmesi istediğinizi belirtmeniz gerekir. Gerekli parametre, aşağıdaki kodda gösterildiği gibi bir **TransformOutput** nesnesidir. Her **TransformOutput** bir **Ön ayar** içerir. **Ön ayar**, video ve/veya ses işleme işlemlerinin istenen **TransformOutput** nesnesini oluşturmak üzere kullanılacak adım adım yönergelerini açıklar. Bu makalede açıklanan örnek, **AdaptiveStreaming** adlı yerleşik bir Ön Ayar kullanır. Ön Ayar, giriş çözünürlüğü ve bit hızını temel alarak, giriş videosunu otomatik olarak oluşturulan bir bit hızı basamağına (bit hızı-çözünürlük çiftleri) kodlar ve her bir bit hızı-çözünürlük çiftine karşılık gelen H.264 video ve AAC sesi ile ISO MP4 dosyaları üretir. 

Yeni bir oluşturmadan önce [dönüştürme](https://docs.microsoft.com/rest/api/media/transforms), zaten kullanarak bir tane varsa öncelikle denetleme yapmalıdır **Al** yöntemi, aşağıdaki kodda gösterildiği gibi.  Media Services v3’te varlıklar üzerindeki **Get** yöntemleri, varlığın mevcut olmaması durumunda **null** değerini döndürür (büyük/küçük harfe duyarlı ad denetimi).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#EnsureTransformExists)]

## <a name="submit-job"></a>İşi Gönder

Yukarıda bahsedildiği gibi [Transform](https://docs.microsoft.com/rest/api/media/transforms) nesnesi tarif, [Job](https://docs.microsoft.com/rest/api/media/jobs) ise bu **Transform** nesnesini belirli bir giriş videosu veya ses içeriğine uygulamak için Media Services’e gönderilen gerçek istektir. **İş** giriş videosunun konumu ve çıktının konumu gibi bilgileri belirtir.

Bu öğreticide, doğrudan alınan bir dosyayı temel işin girdisini oluştururuz bir [HTTPs kaynak URL](job-input-from-http-how-to.md).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#SubmitJob)]

## <a name="wait-for-the-job-to-complete"></a>İşin tamamlanmasını bekleyin

İşin tamamlanması biraz sürüyor ve tamamlandığında bildirim almak istiyorsunuz. Aşağıdaki kod örneği, [İş](https://docs.microsoft.com/rest/api/media/jobs)’in durumu için hizmette nasıl yoklama yapılacağını gösterir. Yoklama, olası gecikme süresi nedeniyle üretim uygulamaları için önerilen en iyi uygulamalardan biri değildir. Yoklama, bir hesap üzerinde gereğinden fazla kullanılırsa kısıtlanabilir. Geliştiricilerin onun yerine Event Grid kullanmalıdır. Bkz. [Olayları özel bir web uç noktasına yönlendirme](job-state-events-cli-how-to.md).

**İş** genellik şu aşamalardan geçer: **Zamanlandı**, **Kuyruğa Alındı**, **İşleniyor**, **Tamamlandı** (son aşama). İş bir hatayla karşılaştıysa **Hata** durumunu alırsınız. İş iptal edilme sürecindeyse **İptal Ediliyor** ve **İptal Edildi** durumunu alırsınız.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#WaitForJobToFinish)]

## <a name="create-a-contentkey-policy"></a>ContentKey ilkesi oluşturma

Bir içerik anahtarı için varlıklarınızı güvenli erişim sağlar. İstemciler sonlandırmak için içerik anahtarını nasıl teslim edildiğini yapılandıran içerik bir anahtar ilkesi oluşturmanız gerekir. İçerik anahtarı StreamingLocator ile ilişkilidir. Media Services, şifreleme anahtarlarını yetkili kullanıcıların sunan anahtar dağıtımı hizmetiyle de sağlar. 

Bir akışa bir oynatıcı tarafından istendiğinde Media Services belirtilen anahtarı (Bu durumda, AES şifrelemesi kullanılarak.) içeriğinizi dinamik olarak şifrelemek için kullanır. Akış şifresini çözmek için player anahtar anahtar teslim hizmetinden ister. Kullanıcı anahtarı almak için yetki verilip verilmediğini belirlemek için anahtar için belirtilen içerik anahtarı ilkesi hizmet tarafından değerlendirilir.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="get-a-token"></a>Bir belirteç Al
        
Bu öğreticide, biz bir belirteç kısıtlamasına sahip içerik anahtar ilke için belirtin. Belirteç kısıtlamalı ilkenin beraberinde bir güvenlik belirteci hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Media Services belirteçleri destekler [JSON Web belirteci](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) biçimlerindeki ve olduğu örnekte biz yapılandırın.

Anahtar teslim hizmeti için sunulan belirteci ContentKey tanımlayıcısını içinde olması gerektiğini anlamına gelir ContentKeyPolicy ContentKeyIdentifierClaim kullanılır. Örnekte, biz StreamingLocator oluştururken bir içerik anahtarı belirtmezseniz, bizim için rastgele bir sistem oluşturur. Test oluşturmak için belirteci, biz ContentKeyId ContentKeyIdentifierClaim talebi moduna almanız gerekir.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetToken)]

## <a name="create-a-streaminglocator"></a>StreamingLocator oluşturma

Kodlama tamamlandıktan sonra sıradaki adım, çıktı Varlığındaki videoyu yürütmek için istemcilerin kullanımına sunmaktır. Bunu iki adımda gerçekleştirebilirsiniz: ilk olarak, bir [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) oluşturun ve ikinci olarak, istemcilerin kullanabildiği akış URL’lerini derleyin. 

**StreamingLocator** oluşturma işlemine yayımlama denir. Varsayılan olarak **StreamingLocator**, API çağrılarını yapmanızdan hemen sonra geçerli olur ve isteğe bağlı başlangıç ve bitiş süreleri yapılandırmadıkça silinene kadar devam eder. 

Bir [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) oluştururken istenen **StreamingPolicyName** değerini belirtmeniz gerekir. Bu öğreticide, Azure Media Services akış için içerik yayımlamak anlatır PredefinedStreamingPolicies birini kullanacağız. Bu örnekte, AES zarfı şifreleme (key HTTPS ve DRM lisans yoluyla kayıttan yürütme istemciye teslim edildiğinden da ClearKey şifreleme bilinir) uygulanır.

> [!IMPORTANT]
> Özel [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies)’yi kullanırken Media Service hesabınız için bu tür ilkelerin sınırlı bir kümesini tasarlamanız ve aynı şifreleme seçenekleri ve protokoller gerekli olduğunda StreamingLocators için bunları kullanmanız gerekir. Media Service hesabınızda StreamingPolicy girişlerinin sayısı için bir kota bulunur. Her StreamingLocator için yeni bir StreamingPolicy oluşturmamanız gerekir.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateStreamingLocator)]

## <a name="build-a-dash-streaming-url"></a>DASH akış URL'si oluşturmak

Şimdi [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) olmuştur oluşturulmuş akış URL'lerini alabilirsiniz. Bir URL derlemek için [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints) konak adı ile **StreamingLocator** yolunu birleştirmeniz gerekir. Bu örnekte *varsayılan* **StreamingEndpoint** değeri kullanılır. İlk kez Media Service hesabı oluşturduğunuzda bu *varsayılan* **StreamingEndpoint** durdurulmuş durumdadır, bu nedenle **Start** çağrısı yapmanız gerekir.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetMPEGStreamingUrl)]

## <a name="clean-up-resources-in-your-media-services-account"></a>Media Services hesabınızdaki kaynakları temizleme

Genellikle, yeniden kullanmayı planladığınız nesneler dışında her şeyi temizlemeniz gerekir (genellikle Dönüşümleri yeniden kullanırsınız ve StreamingLocators vb. nesneleri tutarsınız). Deneme sonrasında hesabınızın temiz olmasını istiyorsanız, yeniden kullanmayı planlamadığınız kaynakları silmeniz gerekir.  Örneğin, aşağıdaki kod İşleri siler.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CleanUp)]

## <a name="next-steps"></a>Sonraki adımlar

[Genel Bakış](content-protection-overview.md)
