---
title: Media Encoder Standard kullanarak bir Azure varlığı kodlama | Microsoft Docs
description: Azure Media Services'da medya içeriği kodlama için Media Encoder Standard'ı kullanmayı öğrenin. Kod örnekleri, REST API'sini kullanın.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 2a7273c6-8a22-4f82-9bfe-4509ff32d4a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 8db9e60e9ce99eaf2621821825620966b8b8b4ae
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60640087"
---
# <a name="how-to-encode-an-asset-by-using-media-encoder-standard"></a>Media Encoder Standard kullanarak bir varlığı kodlama
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
> * [REST](media-services-rest-encode-asset.md)
> * [Portal](media-services-portal-encode.md)
>
>

## <a name="overview"></a>Genel Bakış

Internet üzerinden dijital video teslim etmek için medyayı sıkıştırmanız gerekir. Dijital video dosyaları büyük ve Internet üzerinden veya müşterilerinizin cihazlar düzgün görüntülenmesi sunmak için çok büyük olabilir. Kodlama, müşterilerinizin ortamınızı görüntülemek için video ve ses sıkıştırma işlemidir.

Kodlama işleri, Azure Media Services'da en sık gerçekleştirilen işlemler biridir. Kodlama işleri oluşturarak, medya dosyalarını bir kodlamadan diğerine dönüştürebilirsiniz. Kodlarken, yerleşik medya Hizmetleri Kodlayıcı (Media Encoder Standard) kullanabilirsiniz. Bir Media Services iş ortağı tarafından verilen bir kodlayıcı de kullanabilirsiniz. Üçüncü taraf Kodlayıcı Azure Marketi aracılığıyla kullanılabilir. Kodlayıcınız için tanımlanan Önayar dizeleri kullanarak veya önceden belirlenmiş yapılandırma dosyalarını kullanarak kodlama görevlerine ilişkin ayrıntılar belirtebilirsiniz. Kullanıma hazır türlerini görmek için bkz: [Media Encoder Standard için görev ön ayarları](https://msdn.microsoft.com/library/mt269960).

Bir veya daha fazla görevi gerçekleştirmek istediğiniz işleme türüne bağlı olarak her bir iş olabilir. REST API aracılığıyla, işlerini ve görevlerini ilgili iki yoldan biriyle oluşturabilirsiniz:

* Görevler, tanımlı satır içi görevleri gezinti özelliği iş varlıklar üzerinde gerçekleştirilebilir.
* OData toplu işlem'i kullanın.

Her zaman kaynak dosyalarını Uyarlamalı bit hızı MP4 kümesi kodlamak ve kullanarak bu küme için istenen biçimi dönüştürme öneririz [dinamik paketleme](media-services-dynamic-packaging-overview.md).

Şifrelenmiş depolama, çıktı varlık ise varlık teslim ilkesini yapılandırmanız gerekir. Daha fazla bilgi için [varlık teslim ilkesini yapılandırma](media-services-rest-configure-asset-delivery-policy.md).

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Varlıklar Media Services erişirken, HTTP isteklerini özel üstbilgi alanlarını ve değerlerini ayarlamanız gerekir. Daha fazla bilgi için [Media Services REST API geliştirme için Kurulum](media-services-rest-how-to-use.md).

Medya işlemcileri başvuran başlamadan önce doğru medya gördüğünüzden emin olun. işlemci kimliği Daha fazla bilgi için [alma medya işlemcileri](media-services-rest-get-media-processor.md).

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

AMS API'ye bağlanma hakkında daha fazla bilgi için bkz: [Azure AD kimlik doğrulamasıyla Azure Media Services API'sine erişim](media-services-use-aad-auth-to-access-ams-api.md). 

## <a name="create-a-job-with-a-single-encoding-task"></a>Tek bir kodlama görevi bir iş oluşturma

> [!NOTE]
> Media Services REST API'si ile çalışırken, aşağıdaki maddeler geçerlidir:
>
> Varlıklar Media Services erişirken, HTTP isteklerini özel üstbilgi alanlarını ve değerlerini ayarlamanız gerekir. Daha fazla bilgi için [Media Services REST API geliştirme için Kurulum](media-services-rest-how-to-use.md).
>
> Zaman JSON kullanan ve kullanılacağını belirtme **__metadata** anahtar sözcüğü ayarlamanız gerekir (örneğin,'başvurusu bağlı nesnede) istekte **kabul** başlığına [ayrıntılıJSONbiçimi](https://www.odata.org/documentation/odata-version-3-0/json-verbose-format/): Kabul et: application/json; odata ayrıntılı =.
>
>

Aşağıdaki örnek nasıl oluşturulacağı ve belirli bir çözümleme ve kaliteli video kodlamak için ayarlanmış bir görev içeren bir iş gönderin. gösterir. Medya Kodlayıcısı standart ile kodlamak, belirtilen görev yapılandırması hazır kullanabileceğiniz [burada](https://msdn.microsoft.com/library/mt269960).

İstek:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
        Authorization: Bearer <ENCODED JWT TOKEN> 
        x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
        Host: media.windows.net

    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "Adaptive Streaming", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

Yanıt:

    HTTP/1.1 201 Created

    . . .

### <a name="set-the-output-assets-name"></a>Çıktı varlık adını ayarlayın
Aşağıdaki örnek assetName özniteliğinin nasıl ayarlanacağı gösterilmektedir:

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

## <a name="considerations"></a>Dikkat edilmesi gerekenler
* TaskBody özellikleri değişmez değer XML giriş sayısını belirtin veya çıkış görev tarafından kullanılan varlıklar için kullanmanız gerekir. Görev makale XML için XML şema tanımı içerir.
* Her iç TaskBody tanımında değeri `<inputAsset>` ve `<outputAsset>` JobInputAsset(value) veya JobOutputAsset(value) olarak ayarlanmalıdır.
* Bir görev birden fazla çıktı varlığına sahip olabilirsiniz. Bir JobOutputAsset(x) yalnızca bir kez bir işteki bir görevin çıktı olarak kullanılabilir.
* Bir görevin Giriş bir varlık JobInputAsset veya JobOutputAsset belirtebilirsiniz.
* Görevleri bir döngü form değil.
* JobInputAsset veya JobOutputAsset için geçirdiğiniz değer parametresi bir varlık için dizin değerini temsil eder. Gerçek varlıkları işi varlık tanımı InputMediaAssets ve OutputMediaAssets Gezinti özellikleri tanımlanır.
* Media Services OData v3 tasarlandığından, tek tek varlıklar InputMediaAssets ve OutputMediaAssets gezinti özelliği koleksiyonlardaki aracılığıyla başvurulan bir "__metadata: URI" ad-değer çifti.
* Media Services'de, oluşturduğunuz bir veya daha fazla varlıklara InputMediaAssets eşler. OutputMediaAssets sistem tarafından oluşturulur. Bunlar, mevcut bir varlık başvurmuyor.
* OutputMediaAssets assetName özniteliğini kullanarak yeniden adlandırılabilir. Bu öznitelik yoksa sonra herhangi bir iç metin değerini OutputMediaAsset adıdır `<outputAsset>` bir sonek iş adı değeri ya da iş kimliği değeri (Name özelliği olmayan tanımlandığı durumda) öğesidir. AssetName "SAMPLE" için bir değer ayarlarsanız, örneğin, ardından OutputMediaAsset Name özelliği "Örnek" ayarlanır AssetName değerini ayarlamadınız, ancak "NewJob" proje adı ayarlayın, ancak ardından OutputMediaAsset adı "JobOutputAsset (değer) _NewJob" olacaktır

## <a name="create-a-job-with-chained-tasks"></a>Zincirleme görev sayısı ile bir iş oluşturma
Birçok uygulama senaryolarında geliştiriciler işleme görevlerini bir dizi oluşturmak istiyorsunuz. Media Services'de, zincir görevleri bir dizi oluşturabilirsiniz. Her görev, farklı işleme adımları gerçekleştirir ve farklı medya işlemcileri kullanabilirsiniz. Zincirleme görev ashley'e başka varlığında doğrusal bir sırada görevleri gerçekleştirmek için bir görevden teslim. Ancak, bir projede gerçekleştirilen görevleri bir dizisinde olması gerekli değildir. Zincirleme zincirleme görev oluşturduğunuzda, **ITask** tek bir nesne oluşturulur **IJob** nesne.

> [!NOTE]
> Şu anda iş başına 30 görevlerin bir sınır yoktur. 30'dan fazla görevleri zinciri gerekiyorsa, görevleri içermek amacıyla birden fazla işi oluşturun.
>
>

    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
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
Görev zinciri oluşturmayı etkinleştirmek için:

* Bir işi, en az iki görev olmalıdır.
* İşin başka bir görev çıktısı olan giriştir en az bir görev olmalıdır.

## <a name="use-odata-batch-processing"></a>OData toplu işleme
Aşağıdaki örnek, bir işi ve görevleri oluşturmak için OData toplu işlem kullanmayı gösterir. Toplu işlem hakkında daha fazla bilgi için bkz: [açık veri Protokolü (OData) toplu işleme](https://www.odata.org/documentation/odata-version-3-0/batch-processing/).

    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17
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
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17
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
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--



## <a name="create-a-job-by-using-a-jobtemplate"></a>Bir JobTemplate kullanarak bir işi oluşturma
Ortak bir dizi görevi kullanarak birden çok varlıkları işlemek, bir JobTemplate varsayılan görev ön ayarları belirtin ya da görev sırasını ayarlamak için kullanın.

Aşağıdaki örnek, bir JobTemplate satır içi olarak tanımlanan bir TaskTemplate ile oluşturma işlemi gösterilmektedir. TaskTemplate Media Encoder Standard MediaProcessor varlık dosyası kodlamak için kullanır. Ancak, diğer MediaProcessors de kullanılabilir.

    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net


    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }


> [!NOTE]
> Diğer bir Media Services varlıklar farklı olarak, yeni bir GUID tanımlayıcısı için her TaskTemplate tanımlayın ve taskTemplateId ve ID özelliği, istek gövdesinde yerleştirin. İçerik kimliği düzeni tanımlamak Azure Media Services varlıkları tanımlanan düzenini izlemeniz gerekir. Ayrıca, JobTemplates güncelleştirilemiyor. Bunun yerine, güncelleştirilmiş değişikliklerinizi ile yeni bir tane oluşturmanız gerekir.
>
>

Başarılı olursa, aşağıdaki yanıt döndürülür:

    HTTP/1.1 201 Created

    . . .


Aşağıdaki örnek, bir JobTemplate kimliği başvuran bir iş oluşturma işlemi gösterilmektedir:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net


    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}


Başarılı olursa, aşağıdaki yanıt döndürülür:

    HTTP/1.1 201 Created

    . . .


## <a name="advanced-encoding-features-to-explore"></a>Özelliklerini keşfetmek için gelişmiş kodlama
* [Küçük resim oluşturma](media-services-dotnet-generate-thumbnail-with-mes.md)
* [Kodlama sırasında küçük resimler oluşturma](media-services-dotnet-generate-thumbnail-with-mes.md#example-of-generating-a-thumbnail-while-encoding)
* [Kodlama sırasında videoları kırpma](media-services-crop-video.md)
* [Kodlama Önayarları özelleştirme](media-services-custom-mes-presets-with-dotnet.md)
* [Yer paylaşımı veya bir video görüntü ile filigranı](media-services-advanced-encoding-with-mes.md#overlay)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bir varlığı kodlama için bkz: bir iş oluşturma işlemini artık bildiğinize [Media Services ile işin ilerleme durumunu denetlemek nasıl](media-services-rest-check-job-progress.md).

## <a name="see-also"></a>Ayrıca bkz.
[Medya işlemcileri Al](media-services-rest-get-media-processor.md)
