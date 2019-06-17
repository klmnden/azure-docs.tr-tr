---
title: Dönüşümler ve işler Azure Media Services | Microsoft Docs
description: Media Services hizmetini kullanırken, dönüşüm kurallar veya videolarınızı işlemek için özellikleri tanımlamak için oluşturmanız gerekir. Bu makalede, dönüştürme nedir ve nasıl kullanılacağını hakkında genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/08/2019
ms.author: juliako
ms.openlocfilehash: 01b386c820a09af0e616698aabc58a886c30bb09
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65550939"
---
# <a name="transforms-and-jobs"></a>Dönüşümler ve İşler

Bu konu hakkında ayrıntılar sağlar. [dönüştüren](https://docs.microsoft.com/rest/api/media/transforms) ve [işleri](https://docs.microsoft.com/rest/api/media/jobs) ve bu varlıkları arasındaki ilişkiyi açıklar. 

## <a name="overview"></a>Genel Bakış 

### <a name="transformsjobs-workflow"></a>Dönüşümler/işleri iş akışı

Dönüşümler/işleri iş akışı aşağıdaki diyagramda gösterilmiştir.

![Dönüşümler](./media/encoding/transforms-jobs.png)

#### <a name="typical-workflow"></a>Tipik iş akışı

1. Dönüşüm oluşturma 
2. Dönüşüm altında işlerini gönderme 
3. Liste dönüşümler 
4. Gelecekte kullanmayı planlamıyorsanız bir dönüşüm silin. 

#### <a name="example"></a>Örnek

Tüm videolarınızı ilk karesine – bir küçük resim olarak çıkarmak istediğinizi varsayalım, uygulamanız gereken adımları şunlardır: 

1. Tarif veya videolarınızı işlemek için bir kural tanımlama - "ilk çerçeve video küçük resmi kullan". 
2. Her video için hizmet söyleyin: 
    1. Bu video nerede bulacağını,  
    2. Çıktı küçük resim görüntüsü yazmak yer. 

A **dönüştürme** tarif sonra (adım 1) oluşturun ve bu tarif (Adım 2) kullanarak göndermek yardımcı olur.

> [!NOTE]
> Özelliklerini **dönüştürme** ve **iş** DateTime türü her zaman UTC biçiminde olan.

## <a name="transforms"></a>Dönüşümler

Kullanım **dönüştüren** kodlama veya videoları analiz için ortak görevler yapılandırmak için. Her **dönüştürme** bir tarif veya bir iş akışı, video veya ses dosyalarını işlemek için görevler açıklanmaktadır. Tek bir dönüştürme, birden fazla kural uygulayabilirsiniz. Örneğin, bir dönüştürme belirli bir bit hızı anda bir MP4 dosyasına her video kodlanmış ve bir küçük resim videonun ilk çerçevesinden oluşturulmasını belirtebilirsiniz. Dönüştürme, dahil etmek istediğiniz her bir kural için bir TransformOutput girdi eklersiniz. Önayarlar, dönüştürme giriş medya dosyalarını nasıl işlenmesi gerektiğini söylemek için kullanın.

### <a name="viewing-schema"></a>İzleme şeması

Media Services v3 sürümünde hazır kesin türü belirtilmiş API'sinde varlıklardır. Bu nesneler için "şema" tanımı bulabilirsiniz [açık API Belirtimi (ya da Swagger)](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01). Önceden oluşturulmuş tanımlarını da görüntüleyebilirsiniz (gibi **StandardEncoderPreset**) içinde [REST API](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset), [.NET SDK'sı](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.standardencoderpreset?view=azure-dotnet) (veya diğer Media Services v3 SDK başvuru belgeleri).

### <a name="creating-transforms"></a>Dönüşümler oluşturma

REST, CLI kullanarak dönüşümler oluşturmak ya da yayımlanan SDK'ları birini kullanın. Media Services hesabınızı oluşturmak ve dağıtmak için Resource Manager şablonları kullanabilmeniz için API Azure Resource Manager tarafından yönetilen Media Services v3 dönüştürür. Rol tabanlı erişim denetimi, dönüşümler erişimi kilitlemek için kullanılabilir.

### <a name="updating-transforms"></a>Dönüşümler güncelleştiriliyor

Güncelleştirmeniz gerekirse, [dönüştürme](https://docs.microsoft.com/rest/api/media/transforms), kullanın **güncelleştirme** işlemi. Açıklama veya temel alınan TransformOutputs önceliklerini değişiklikler yapmak için tasarlanmıştır. Tüm devam eden işleri tamamladığınızda bu güncelleştirmeler yapılması önerilir. Tarif yeniden düşünüyorsanız, yeni bir dönüştürme oluşturmanız gerekir.

### <a name="transform-object-diagram"></a>Nesneyi diyagram dönüştürme

Aşağıdaki diyagramda gösterildiği **dönüştürme** nesnenin ve başvurduğu türetme ilişkiler dahil olmak üzere nesneleri. Gri oklar, proje başvuruları ve yeşil ok sınıfı türetme ilişkileri göster bir tür gösterir.<br/>Resmi tam boyutlu görüntülemek için tıklayın.  

<a href="./media/api-diagrams/transform-large.png" target="_blank"><img src="./media/api-diagrams/transform-small.png"></a> 

## <a name="jobs"></a>İşler

A **iş** uygulamak için Azure Media Services için fiili istek **dönüştürme** belirli bir giriş video veya ses içeriği için. Dönüştürme oluşturulduktan sonra Media Services API'leri ve yayımlanan SDK'ları hiçbirini kullanarak işleri gönderebilirsiniz. **İş** konumun video giriş ve çıkış konumunu gibi bilgileri belirtir. Giriş video kullanarak konumu belirtebilirsiniz: HTTPS URL'leri, SAS URL'lerini veya [varlıklar](https://docs.microsoft.com/rest/api/media/assets).  

### <a name="job-input-from-https"></a>HTTPS iş girişten

Kullanım [iş HTTPS girişten](job-input-from-http-how-to.md) içeriğinizi zaten bir URL ile erişilebilir olması ve kaynak dosyasını Azure'a depolamak gerekmez (örneğin, S3'ten içeri aktarın). Bu yöntem Ayrıca, Azure Blob Depolama alanında içeriği varsa, ancak herhangi bir varlığı dosya gerek uygundur. Şu anda, bu yöntem yalnızca giriş için tek bir dosyayı destekler.

### <a name="asset-as-job-input"></a>İş giriş varlığı

Kullanım [iş giriş varlığı](job-input-from-local-file-how-to.md) giriş içeriği zaten bir varlığı veya içeriği yerel dosyasında depolanır. Akış veya (örneğin, mp4 indirmek için yayımlama ancak de konuşma metin veya yüz algılama yapmak istediğiniz istediğiniz) indirme için giriş varlığı yayımlamayı düşünüyorsanız bu da iyi bir seçenektir. Bu yöntem, çok dosyalı varlıklar (örneğin, yerel olarak kodlanmış kümeleri akış MBR) destekler.

### <a name="checking-job-progress"></a>İşin ilerleme durumunu denetleme

İşlerin durumunu ve ilerleme durumunu izleme olayları Event Grid ile tarafından alınabilir. Daha fazla bilgi için [EventGrid kullanarak olayları izleme](job-state-events-cli-how-to.md).

### <a name="updating-jobs"></a>İşleri güncelleştiriliyor

Güncelleştirme işlemi [iş](https://docs.microsoft.com/rest/api/media/jobs) varlığı değiştirmek için kullanılabilir *açıklama*ve *öncelik* iş gönderildikten sonra özellikleri. Bir değişiklik *öncelik* özelliği yalnızca proje yine de kuyruğa alınmış bir durumda ise etkin. İş işleme başladı ya da sona önceliğini değiştirme etkisi yoktur.

### <a name="job-object-diagram"></a>İş nesnesi diyagramı

Aşağıdaki diyagramda gösterildiği **iş** nesnenin ve başvurduğu türetme ilişkiler dahil olmak üzere nesneleri.<br/>Resmi tam boyutlu görüntülemek için tıklayın.  

<a href="./media/api-diagrams/job-large.png" target="_blank"><img src="./media/api-diagrams/job-small.png"></a> 

## <a name="configure-media-reserved-units"></a>Yapılandırma medya ayrılmış birimleri

Ses analizi ve Video analizi işleri, Media Services v3 tarafından tetiklenen veya Video Indexer için 10 S3 medya ayrılmış birimi (MRU) hesabınızla sağlama önemle tavsiye edilir. 10'dan fazla S3 MRU gerekiyorsa, kullanarak bir destek bileti açın [Azure portalında](https://portal.azure.com/).

Ayrıntılar için bkz [medya CLI ile işlemeyi ölçeklendirme](media-reserved-units-cli-how-to.md).

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, görüşlerinizi, güncelleştirmeleri alın

Kullanıma [Azure Media Services topluluğu](media-services-community.md) soru sorun, görüşlerinizi ve medya hizmetleri hakkında güncelleştirmeler almak farklı yollarını görmek için makaleyi.

## <a name="see-also"></a>Ayrıca bkz.

* [Hata kodları](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode)
* [Media Services varlıkların filtreleme, sıralama, sayfalama](entities-overview.md)

## <a name="next-steps"></a>Sonraki adımlar

- Geliştirmeye başlamadan önce gözden [Media Services v3 API'leri ile geliştirme](media-services-apis-overview.md) (API, adlandırma kurallarını, vb. erişme hakkında bilgi içerir.)
- Bu öğreticilere göz atın:

    - [Öğretici: .NET kullanarak videoları karşıya yükleme, kodlama ve akışla aktarma](stream-files-tutorial-with-api.md)
    - [Öğretici: Media Services v3 ile .NET kullanarak videoları analiz etme](analyze-videos-tutorial-with-api.md)
