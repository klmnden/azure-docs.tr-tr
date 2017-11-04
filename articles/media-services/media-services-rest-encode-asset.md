---
title: "Medya Kodlayıcı standart kullanarak bir Azure varlık kodlama | Microsoft Docs"
description: "Medya Kodlayıcısı standart Azure Media Services medya içeriği kodlamak için nasıl kullanılacağını öğrenin. Kod örnekleri, REST API kullanın."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 2a7273c6-8a22-4f82-9bfe-4509ff32d4a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: 796f3b5a4dd56a0160986600cbbcf38faf8add56
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-encode-an-asset-by-using-media-encoder-standard"></a>Medya Kodlayıcı standart kullanarak bir varlık kodlama
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
> * [REST](media-services-rest-encode-asset.md)
> * [Portal](media-services-portal-encode.md)
>
>

## <a name="overview"></a>Genel Bakış
Internet üzerinden dijital video teslim etmek için medyayı sıkıştırmanız gerekir. Dijital video dosyaları büyük ve Internet üzerinden veya düzgün görüntülemek, müşterilerinizin cihazlar için teslim etmek için çok büyük olabilir. Kodlama müşterilerinizin medyanızı görüntüleyebilmeniz video ve ses sıkıştırma işlemidir.

Kodlama işleri en yaygın işlemleri Azure Media Services biridir. Kodlama işleri oluşturarak, medya dosyalarını bir kodlamadan diğerine dönüştürebilirsiniz. Kodlarken, Media Services yerleşik Kodlayıcı (Medya Kodlayıcısı standart) kullanabilirsiniz. Bir Media Services iş ortağı tarafından sağlanan bir kodlayıcı de kullanabilirsiniz. Üçüncü taraf kodlayıcılar Azure Market üzerinden kullanılabilir. Kodlayıcı için tanımlanan Önayar dizeleri veya önceden belirlenmiş yapılandırma dosyalarını kullanarak kodlama görevlerine ilişkin ayrıntılar belirtebilirsiniz. Kullanılabilir hazır türlerini görmek için bkz: [Medya Kodlayıcısı standart için görev hazır](http://msdn.microsoft.com/library/mt269960).

Bir veya daha fazla görevleri gerçekleştirmek istediğiniz işleme türüne bağlı olarak her bir iş olabilir. REST API aracılığıyla, işler ve bunların ilgili görevleri aşağıdaki iki yöntemden biriyle oluşturabilirsiniz:

* Görevler, satır içi olarak tanımlanan iş varlıklar üzerinde görevleri gezinti özelliği aracılığıyla gerçekleştirilebilir.
* OData toplu işleme kullanın.

Her zaman Kaynak dosyalarınız Uyarlamalı bit hızlı MP4 kümesi kodlamak ve ardından kullanarak kümesi istenen biçimine dönüştür öneririz [dinamik paketleme](media-services-dynamic-packaging-overview.md).

Çıkış varlığına depolama şifrelenmiş ise, varlık teslim ilkesini yapılandırmanız gerekir. Daha fazla bilgi için bkz: [varlık teslim ilkesini yapılandırma](media-services-rest-configure-asset-delivery-policy.md).

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Varlıklar Media Services erişirken, HTTP istekleri özel üstbilgi alanlarını ve değerlerini ayarlamanız gerekir. Daha fazla bilgi için bkz: [Media Services REST API geliştirme için Kurulum](media-services-rest-how-to-use.md).

Medya işlemcileri başvuran başlamadan önce doğru medya sahip olduğunuzu doğrulayın işlemci kimliği Daha fazla bilgi için bkz: [alma medya işlemcileri](media-services-rest-get-media-processor.md).

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

AMS API'sine bağlanma hakkında daha fazla bilgi için bkz: [Azure AD kimlik doğrulaması ile Azure Media Services API erişim](media-services-use-aad-auth-to-access-ams-api.md). 

>[!NOTE]
>Başarıyla https://media.windows.net için bağladıktan sonra başka bir Media Services URI belirleme 301 bir yeniden yönlendirme alırsınız. Yeni bir URI yapılan sonraki çağrılar yapmanız gerekir.

## <a name="create-a-job-with-a-single-encoding-task"></a>Bir işi ile tek bir kodlama görev oluşturma
> [!NOTE]
> Media Services REST API ile çalışırken, aşağıdaki maddeler geçerlidir:
>
> Varlıklar Media Services erişirken, HTTP istekleri özel üstbilgi alanlarını ve değerlerini ayarlamanız gerekir. Daha fazla bilgi için bkz: [Media Services REST API geliştirme için Kurulum](media-services-rest-how-to-use.md).
>
> Başarıyla https://media.windows.net için bağladıktan sonra başka bir Media Services URI belirleme 301 bir yeniden yönlendirme alırsınız. Yeni bir URI yapılan sonraki çağrılar yapmanız gerekir. AMS API'sine bağlanma hakkında daha fazla bilgi için bkz: [Azure AD kimlik doğrulaması ile Azure Media Services API erişim](media-services-use-aad-auth-to-access-ams-api.md).
>
> JSON kullanarak ve kullanmak için belirterek **__metadata** istekte anahtar sözcüğü (örneğin, bağlantılı nesne başvurular), ayarlamanız gerekir **kabul** başlığına [JSON Verbose biçimi](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/): Kabul: uygulama/json; odata = verbose.
>
>

Aşağıdaki örnekte nasıl oluşturulacağı ve bir video kalitesini ve belirli çözünürlüğü kodlamak için ayarlanmış bir görev ile bir iş sonrası gösterir. Medya Kodlayıcısı standart ile kodlamak, belirtilen görev yapılandırması hazır kullanabilirsiniz [burada](http://msdn.microsoft.com/library/mt269960).

İsteği:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net

    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "Adaptive Streaming", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

Yanıtı:

    HTTP/1.1 201 Created

    . . .

### <a name="set-the-output-assets-name"></a>Çıktı varlığın adı ayarlama
Aşağıdaki örnek assetName özniteliği gösterilmektedir:

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

## <a name="considerations"></a>Dikkat edilmesi gerekenler
* TaskBody özellikleri değişmez değer XML giriş sayısını tanımlamak veya görev tarafından kullanılan varlıklar çıktı için kullanmanız gerekir. Görev konusu XML şema tanımı için XML içeriyor.
* TaskBody tanımı'nda her iç değeri için <inputAsset> ve <outputAsset> JobInputAsset(value) veya JobOutputAsset(value) ayarlanması gerekir.
* Bir görev, birden fazla çıkış varlığına sahip olabilir. Bir JobOutputAsset(x) yalnızca bir kez bir görevin bir İşte bir çıkış olarak kullanılabilir.
* Bir görev bir giriş varlık JobInputAsset veya JobOutputAsset belirtebilirsiniz.
* Görevler bir döngü oluşturmuyor gerekir.
* JobInputAsset veya JobOutputAsset geçirdiğiniz değer parametresi bir varlık için dizin değerini temsil eder. Gerçek varlıkları işi varlık açıklamasında InputMediaAssets ve OutputMediaAssets Gezinti Özellikleri'nde tanımlanır.
* Media Services üzerinde OData v3 oluşturulduğundan, tek tek varlıklar InputMediaAssets ve OutputMediaAssets gezinti özelliği koleksiyonlarda aracılığıyla başvurulan bir "__metadata: URI" ad-değer çifti.
* Media Services ile oluşturulan bir veya daha fazla varlıklarına InputMediaAssets eşler. OutputMediaAssets sistem tarafından oluşturulur. Bunlar, var olan bir varlık başvurusu yok.
* OutputMediaAssets assetName özniteliğini kullanarak adlandırılabilir. Bu öznitelik yok sonra OutputMediaAsset adını ne olursa olsun iç metin değeri <outputAsset> öğesidir iş adı değeri ya da iş kimliği değeri (Name özelliği tanımlanmadı olduğu durumda) soneki. AssetName "SAMPLE" için bir değer ayarlarsanız, örneğin, ardından OutputMediaAsset Name özelliği "Örnek" ayarlanır AssetName için bir değer ayarlamadınız, ancak "NewJob" iş adına ayarlayın, ancak ardından OutputMediaAsset adı "JobOutputAsset (değer) _NewJob." olacaktır

## <a name="create-a-job-with-chained-tasks"></a>Bir iş ile zincirleme görevler oluşturma
Birçok uygulama senaryolarda, geliştiricilerin bir dizi görevleri işleme oluşturmak istiyorum. Media Services'de zincirleme görevleri bir dizi oluşturabilirsiniz. Her görev, farklı işleme adımları gerçekleştirir ve farklı medya işlemcileri kullanabilir. Zincirleme görevleri bir varlık başka varlık üzerinde doğrusal bir dizi görev gerçekleştirmek için bir görevden el. Ancak, bir işin gerçekleştirilen görevleri sırayla olması gerekli değildir. Zincirleme zincirleme görev oluşturduğunuzda **ITask** nesneler, tek bir oluşturulur **IJob** nesnesi.

> [!NOTE]
> Şu anda iş başına 30 görevlerin bir sınırı yoktur. 30'dan fazla görevleri zincir gerekiyorsa, görevleri içeren birden fazla iş oluşturun.
>
>

    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://testrest.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A910ffdc1-2e25-4b17-8a42-61ffd4b8914c')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          },
          {  
             "Configuration":"H264 Smooth Streaming 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-16\"?><taskBody><inputAsset>JobOutputAsset(0)</inputAsset><outputAsset>JobOutputAsset(1)</outputAsset></taskBody>"
          }
       ]
    }


### <a name="considerations"></a>Dikkat edilmesi gerekenler
Görev zincirleme etkinleştirmek için:

* Bir işi, en az iki görev olması gerekir.
* İşte başka bir görev çıktısını girişi olan en az bir görev olması gerekir.

## <a name="use-odata-batch-processing"></a>OData toplu işleme
Aşağıdaki örnek OData toplu işleme iş ve görev oluşturma için nasıl kullanılacağını gösterir. Toplu işleme hakkında daha fazla bilgi için bkz: [açık veri Protokolü (OData) toplu işleme](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).

    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net


    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Content-Type: multipart/mixed; boundary=changeset_122fb0a4-cd80-4958-820f-346309967e4d

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary

    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-ID: 1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {"Name" : "NewTestJob", "InputMediaAssets@odata.bind":["https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A2a22445d-1500-80c6-4b34-f1e5190d33c6')"]}

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary

    POST https://media.windows.net/api/$1/Tasks HTTP/1.1
    Content-ID: 2
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--



## <a name="create-a-job-by-using-a-jobtemplate"></a>Bir JobTemplate kullanarak bir iş oluşturma
Birden çok varlığı ortak bir dizi görevi kullanarak işlerken bir JobTemplate varsayılan görev hazır belirtin veya görevlerin sırasını ayarlamak için kullanın.

Aşağıdaki örnek, satır içi olarak tanımlanan bir TaskTemplate ile bir JobTemplate oluşturulacağını gösterir. TaskTemplate Medya Kodlayıcısı standart MediaProcessor varlık dosyası kodlamak için kullanır. Ancak, diğer MediaProcessors de kullanılabilir.

    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net


    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }


> [!NOTE]
> Diğer Media Services varlıklar farklı olarak, her TaskTemplate için yeni bir GUID tanımlayıcısı tanımlayın ve istek gövdesinde taskTemplateId ve ID özelliği yerleştirin. İçerik kimliği düzeni tanımlamak Azure Media Services varlıklarda tanımlanan düzenini izlemeniz gerekir. Ayrıca, JobTemplates güncelleştirilemiyor. Bunun yerine, güncelleştirilmiş değişikliklerinizi içeren yeni bir tane oluşturmanız gerekir.
>
>

Başarılı olursa, şu yanıtı döndürdü:

    HTTP/1.1 201 Created

    . . .


Aşağıdaki örnek JobTemplate kimliği başvuran bir iş oluşturulacağını gösterir:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net


    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}


Başarılı olursa, şu yanıtı döndürdü:

    HTTP/1.1 201 Created

    . . .



## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bir varlık kodlama için bkz: bir iş oluşturmak nasıl bildiğinize göre [Media Services ile iş ilerleme durumunu denetlemek nasıl](media-services-rest-check-job-progress.md).

## <a name="see-also"></a>Ayrıca bkz.
[Medya işlemcileri Al](media-services-rest-get-media-processor.md)
