---
title: AES Azure Media Services dinamik şifreleme kullanabilmek | Microsoft Docs
description: Microsoft Azure Media Services kullanarak AES 128 bit şifreleme anahtarları ile şifrelenmiş içeriğinizi teslim etmek. Media Services, yetkili kullanıcıların şifreleme anahtarları sunan anahtar teslim hizmeti de sağlar. Bu konu anahtar teslim hizmeti dinamik olarak AES-128 ile şifrelemek ve nasıl kullanılacağını gösterir.
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
ms.date: 06/26/2018
ms.author: juliako
ms.openlocfilehash: 0da5bbee6d0d6401a35c301a8b35dc0efa77da7d
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133122"
---
# <a name="use-aes-128-dynamic-encryption-and-the-key-delivery-service"></a>AES-128 dinamik şifreleme ve anahtar teslim hizmetini kullanma

Media Services, HTTP canlı akışı (HLS), MPEG DASH ve 128 bit şifreleme anahtarları kullanılarak AES ile şifrelenmiş kesintisiz akış sunmak için kullanabilirsiniz. Media Services, yetkili kullanıcıların şifreleme anahtarları sunan anahtar teslim hizmeti de sağlar. Medya Hizmetleri için bir varlık şifrelemek isterseniz, şifreleme anahtarını StreamingLocator ile ilişkilendirmek ve ayrıca içerik anahtarı ilkesi yapılandırabilirsiniz. Bir akış player tarafından istendiğinde Media Services belirtilen anahtarı dinamik olarak içeriğinizi AES şifreleme kullanarak şifrelemek için kullanır. Akış şifresini çözmek için player anahtar anahtar teslim hizmetinden ister. Kullanıcı anahtarı alınamadı yetkilendirilip yetkilendirilmediğini belirlemek için hizmet anahtar için belirtilen yetkilendirme ilkelerini değerlendirir.

Makaleyi dayanır [EncryptWithAES](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES) örnek. Örnek nasıl bit hızı Uyarlamalı kodlama için önceden belirlenmiş bir yerleşik kullanan bir kodlama dönüştürme oluşturulacağını gösterir ve doğrudan bir dosya alır bir [HTTPs kaynak URL](job-input-from-http-how-to.md). Çıkış varlığına (ClearKey) AES şifreleme kullanarak yayımlanır. Örnek çıkışı tire bildirimi ve içeriği kayıttan yürütmek için gereken AES belirteci dahil olmak üzere Azure Media Player için bir URL'dir. Örnek JWT belirtecinin süre sonu 1 saat olarak ayarlar. Bir tarayıcı açın ve URL'yi ve sizin için önceden doldurulan belirteci ile Azure Media Player tanıtım sayfasını başlatmak için sonuçta elde edilen URL'sini yapıştırın (şu biçimde: ``` https://ampdemo.azureedge.net/?url= {dash Manifest URL} &aes=true&aestoken=Bearer%3D{ JWT Token here}```.)

> [!NOTE]
> Her varlık türleriyle birden çok şifreleme (AES-128, PlayReady, Widevine, FairPlay) şifreleyebilirsiniz. Bkz: [akış protokolleri ve şifreleme türleri](content-protection-overview.md#streaming-protocols-and-encryption-types), ne birleştirmek mantıklıdır görmek için.

## <a name="prerequisites"></a>Önkoşullar

Öğreticiyi tamamlamak için aşağıdakiler gereklidir.

* Gözden geçirme [içerik korumaya genel bakış](content-protection-overview.md) makalesi.
* Visual Studio Code veya Visual Studio yükleme
* Bölümünde açıklandığı gibi yeni bir Azure Media Services hesabı oluşturma [Bu Hızlı Başlangıç](create-account-cli-quickstart.md).
* Aşağıdaki kullanarak Media Services API'ları kullanmak için gerekli kimlik bilgilerini almak [erişim API'leri](access-api-cli-how-to.md)

## <a name="download-code"></a>Kodu indirme

Bu konuda aşağıdaki komutu kullanarak makinenize kopya tam .NET örnek içeren GitHub deposunu ele:

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
"Şifrelemek ile AES-128" örnek bulunan [EncryptWithAES](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES) klasör.

> [!NOTE]
> Uygulamanın çalıştırışınızda benzersiz kaynakları bir örnek oluşturur. Genellikle, dönüşümler ve ilkeleri gibi mevcut kaynaklarla yeniden kullanılacak (varsa kaynak yapılandırmaları gerekli). 

## <a name="start-using-media-services-apis-with-net-sdk"></a>.NET SDK ile Media Services API’sini kullanmaya başlama

.NET ile Media Services API’lerini kullanmaya başlamak için bir **AzureMediaServicesClient** nesnesi oluşturmanız gerekir. Nesneyi oluşturmak için, Azure AD kullanarak Azure’a bağlanmak üzere istemcinin ihtiyaç duyduğu kimlik bilgilerini sağlamanız gerekir. Makalenin başlangıcında kopyaladığınız koddaki **GetCredentialsAsync** işlevi, yerel yapılandırma dosyasında sağlanan kimlik bilgilerini temel alan ServiceClientCredentials nesnesini oluşturur. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateMediaServicesClient)]

## <a name="create-an-output-asset"></a>Çıktı varlığı oluşturma  

Çıktı [Varlığı](https://docs.microsoft.com/rest/api/media/assets), kodlama işinizin sonucunu depolar. Kodlama tamamlandıktan sonra (ClearKey) AES şifreleme kullanarak çıkış varlık yayımlandı.  

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateOutputAsset)]
 
## <a name="get-or-create-an-encoding-transform"></a>Alma veya bir kodlama dönüştürme oluşturun

Yeni bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) örneği oluştururken çıktı olarak neyi üretmesi istediğinizi belirtmeniz gerekir. Gerekli parametre, aşağıdaki kodda gösterildiği gibi bir **TransformOutput** nesnesidir. Her **TransformOutput** bir **Ön ayar** içerir. **Ön ayar**, video ve/veya ses işleme işlemlerinin istenen **TransformOutput** nesnesini oluşturmak üzere kullanılacak adım adım yönergelerini açıklar. Bu makalede açıklanan örnek, **AdaptiveStreaming** adlı yerleşik bir Ön Ayar kullanır. Ön Ayar, giriş çözünürlüğü ve bit hızını temel alarak, giriş videosunu otomatik olarak oluşturulan bir bit hızı basamağına (bit hızı-çözünürlük çiftleri) kodlar ve her bir bit hızı-çözünürlük çiftine karşılık gelen H.264 video ve AAC sesi ile ISO MP4 dosyaları üretir. 

Yeni bir oluşturmadan önce [dönüştürme](https://docs.microsoft.com/rest/api/media/transforms), kullanarak zaten bir tane varsa ilk denetlemelisiniz **almak** aşağıdaki kodda gösterildiği gibi yöntemi.  Media Services v3’te varlıklar üzerindeki **Get** yöntemleri, varlığın mevcut olmaması durumunda **null** değerini döndürür (büyük/küçük harfe duyarlı ad denetimi).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#EnsureTransformExists)]

## <a name="submit-job"></a>İşi Gönder

Yukarıda bahsedildiği gibi [Transform](https://docs.microsoft.com/rest/api/media/transforms) nesnesi tarif, [Job](https://docs.microsoft.com/rest/api/media/jobs) ise bu **Transform** nesnesini belirli bir giriş videosu veya ses içeriğine uygulamak için Media Services’e gönderilen gerçek istektir. **İş** giriş videosunun konumu ve çıktının konumu gibi bilgileri belirtir.

Bu öğreticide, doğrudan alınan bir dosyayı temel iş giriş oluşturuyoruz bir [HTTPs kaynak URL](job-input-from-http-how-to.md).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#SubmitJob)]

## <a name="wait-for-the-job-to-complete"></a>İşin tamamlanmasını bekleyin

İşin tamamlanması biraz sürüyor ve tamamlandığında bildirim almak istiyorsunuz. Aşağıdaki kod örneği, [İş](https://docs.microsoft.com/rest/api/media/jobs)’in durumu için hizmette nasıl yoklama yapılacağını gösterir. Yoklama, olası gecikme süresi nedeniyle üretim uygulamaları için önerilen en iyi uygulamalardan biri değildir. Yoklama, bir hesap üzerinde gereğinden fazla kullanılırsa kısıtlanabilir. Geliştiricilerin onun yerine Event Grid kullanmalıdır. Bkz. [Olayları özel bir web uç noktasına yönlendirme](job-state-events-cli-how-to.md).

**İş** genellik şu aşamalardan geçer: **Zamanlandı**, **Kuyruğa Alındı**, **İşleniyor**, **Tamamlandı** (son aşama). İş bir hatayla karşılaştıysa **Hata** durumunu alırsınız. İş iptal edilme sürecindeyse **İptal Ediliyor** ve **İptal Edildi** durumunu alırsınız.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#WaitForJobToFinish)]

## <a name="create-a-contentkey-policy"></a>ContentKey ilkesi oluşturma

Bir içerik anahtarı varlıklarınızı için güvenli erişim sağlar. İçerik anahtarı istemcileri sona erdirmek için nasıl teslim yapılandırır içerik bir anahtar ilkesi oluşturmanız gerekir. İçerik anahtarı StreamingLocator ile ilişkilidir. Media Services, yetkili kullanıcıların şifreleme anahtarları sunan anahtar teslim hizmeti de sağlar. 

Bir akış player tarafından istendiğinde Media Services belirtilen anahtarı içeriğinizi (Bu durumda, AES şifreleme kullanarak.) dinamik olarak şifrelemek için kullanır. Akış şifresini çözmek için player anahtar anahtar teslim hizmetinden ister. Kullanıcı anahtarı alınamadı yetkilendirilip yetkilendirilmediğini belirlemek için hizmet anahtar için belirtilen yetkilendirme ilkelerini değerlendirir.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="get-a-token"></a>Belirteç alın
        
Bu öğreticide, bir belirteç kısıtlamasına sahip içerik anahtarı ilkesi için belirtin. Belirteç kısıtlamalı ilkenin beraberinde bir güvenlik belirteci hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Media Services belirteçleri destekler [JSON Web belirteci](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) biçimleri ve olduğundan örnek biz yapılandırın.

ContentKeyIdentifierClaim anahtar teslim hizmete sunulan belirteci ContentKey tanıtıcısı içinde olmalı anlamına gelir ContentKeyPolicy kullanılır. Örnekte, biz StreamingLocator oluştururken, bir içerik anahtarı belirtmezseniz, sistem bize için rastgele bir tane oluşturur. Test oluşturmak için belirteç, biz ContentKeyIdentifierClaim talep yerleştirilecek ContentKeyId almanız gerekir.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetToken)]

## <a name="create-a-streaminglocator"></a>StreamingLocator oluşturma

Kodlama tamamlandıktan sonra sıradaki adım, çıktı Varlığındaki videoyu yürütmek için istemcilerin kullanımına sunmaktır. Bunu iki adımda gerçekleştirebilirsiniz: ilk olarak, bir [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) oluşturun ve ikinci olarak, istemcilerin kullanabildiği akış URL’lerini derleyin. 

**StreamingLocator** oluşturma işlemine yayımlama denir. Varsayılan olarak **StreamingLocator**, API çağrılarını yapmanızdan hemen sonra geçerli olur ve isteğe bağlı başlangıç ve bitiş süreleri yapılandırmadıkça silinene kadar devam eder. 

Bir [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) oluştururken istenen **StreamingPolicyName** değerini belirtmeniz gerekir. Bu öğreticide, Azure Media Services akışla içerik yayımlamak anlatır PredefinedStreamingPolicies birini kullanıyoruz. Bu örnekte, AES zarfı şifreleme (anahtar HTTPS ve DRM lisans yoluyla kayıttan yürütme istemciye teslim edildiğinden de ClearKey şifreleme da bilinir) uygulanır.

> [!IMPORTANT]
> Özel [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies)’yi kullanırken Media Service hesabınız için bu tür ilkelerin sınırlı bir kümesini tasarlamanız ve aynı şifreleme seçenekleri ve protokoller gerekli olduğunda StreamingLocators için bunları kullanmanız gerekir. Media Service hesabınızda StreamingPolicy girişlerinin sayısı için bir kota bulunur. Her StreamingLocator için yeni bir StreamingPolicy oluşturmamanız gerekir.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateStreamingLocator)]

## <a name="build-a-dash-streaming-url"></a>Akış URL'si bir tire derleme

Şimdi [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) bırakıldı oluşturulan, akış URL'lerini alabilirsiniz. Bir URL derlemek için [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints) konak adı ile **StreamingLocator** yolunu birleştirmeniz gerekir. Bu örnekte *varsayılan* **StreamingEndpoint** değeri kullanılır. İlk kez Media Service hesabı oluşturduğunuzda bu *varsayılan* **StreamingEndpoint** durdurulmuş durumdadır, bu nedenle **Start** çağrısı yapmanız gerekir.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetMPEGStreamingUrl)]

## <a name="clean-up-resources-in-your-media-services-account"></a>Media Services hesabınızdaki kaynakları temizleme

Genellikle, yeniden kullanmayı planladığınız nesneler dışında her şeyi temizlemeniz gerekir (genellikle Dönüşümleri yeniden kullanırsınız ve StreamingLocators vb. nesneleri tutarsınız). Deneme sonrasında hesabınızın temiz olmasını istiyorsanız, yeniden kullanmayı planlamadığınız kaynakları silmeniz gerekir.  Örneğin, aşağıdaki kod İşleri siler.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CleanUp)]

## <a name="next-steps"></a>Sonraki adımlar

[Genel Bakış](content-protection-overview.md)