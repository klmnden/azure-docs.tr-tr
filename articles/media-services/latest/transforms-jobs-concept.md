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
ms.date: 02/20/2019
ms.author: juliako
ms.openlocfilehash: 1c2ec576211741390ef91233101261a7881e4180
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61466757"
---
# <a name="transforms-and-jobs"></a>Dönüşümler ve İşler

Bu konu hakkında ayrıntılar sağlar. [dönüştüren](https://docs.microsoft.com/rest/api/media/transforms) ve [işleri](https://docs.microsoft.com/rest/api/media/jobs) ve bu varlıkları arasındaki ilişkiyi açıklar. Dönüşümler/işleri iş akışı aşağıdaki diyagramda gösterilmiştir.

![Dönüştürmeler](./media/encoding/transforms-jobs.png)

> [!NOTE]
> Özelliklerini **dönüştürme** ve **iş** DateTime türü her zaman UTC biçiminde olan.

## <a name="transforms"></a>Dönüştürmeler

Kullanım **dönüştüren** kodlama veya videoları analiz için ortak görevler yapılandırmak için. Her **dönüştürme** bir tarif veya bir iş akışı, video veya ses dosyalarını işlemek için görevler açıklanmaktadır. Tek bir dönüştürme, birden fazla kural uygulayabilirsiniz. Örneğin, bir dönüştürme belirli bir bit hızı anda bir MP4 dosyasına her video kodlanmış ve bir küçük resim videonun ilk çerçevesinden oluşturulmasını belirtebilirsiniz. Dönüştürme, dahil etmek istediğiniz her bir kural için bir TransformOutput girdi eklersiniz. Media Services v3 API veya herhangi bir yayımlanan SDK'ları kullanarak Media Services hesabınızı dönüştürmeler oluşturabilirsiniz. Media Services hesabınızı oluşturmak ve dağıtmak için Resource Manager şablonları kullanabilmeniz için API Azure Resource Manager tarafından yönetilen Media Services v3 dönüştürür. Rol tabanlı erişim denetimi, dönüşümler erişimi kilitlemek için kullanılabilir.

Güncelleştirme işlemi [dönüştürme](https://docs.microsoft.com/rest/api/media/transforms) varlık açıklaması veya temel alınan TransformOutputs önceliklerini değişiklikler için tasarlanmıştır. Tüm devam eden işleri tamamladığınızda bu güncelleştirmeler yapılması önerilir. Tarif yeniden düşünüyorsanız, yeni bir dönüştürme oluşturmanız gerekir.

## <a name="jobs"></a>İşler

A **iş** uygulamak için Azure Media Services için fiili istek **dönüştürme** belirli bir giriş video veya ses içeriği için. Dönüştürme oluşturulduktan sonra Media Services API'leri ve yayımlanan SDK'ları hiçbirini kullanarak işleri gönderebilirsiniz. **İş** konumun video giriş ve çıkış konumunu gibi bilgileri belirtir. Giriş video kullanarak konumu belirtebilirsiniz: HTTPS URL'leri, SAS URL'lerini veya [varlıklar](https://docs.microsoft.com/rest/api/media/assets).  

Kullanım [iş HTTPS girişten](job-input-from-http-how-to.md) içeriğinizi zaten bir URL ile erişilebilir olması ve kaynak dosyasını Azure'a depolamak gerekmez (örneğin, S3'ten içeri aktarın). Bu yöntem Ayrıca, Azure Blob Depolama alanında içeriği varsa, ancak herhangi bir varlığı dosya gerek uygundur. Şu anda, bu yöntem yalnızca giriş için tek bir dosyayı destekler.
 
Kullanım [iş giriş varlığı](job-input-from-local-file-how-to.md) giriş içeriği zaten bir varlığı veya içeriği yerel dosyasında depolanır. Akış veya (örneğin, mp4 indirmek için yayımlama ancak de konuşma metin veya yüz algılama yapmak istediğiniz istediğiniz) indirme için giriş varlığı yayımlamayı düşünüyorsanız bu da iyi bir seçenektir. Bu yöntem, çok dosyalı varlıklar (örneğin, yerel olarak kodlanmış kümeleri akış MBR) destekler.
 
İşlerin durumunu ve ilerleme durumunu izleme olayları Event Grid ile tarafından alınabilir. Daha fazla bilgi için [EventGrid kullanarak olayları izleme](job-state-events-cli-how-to.md).

Güncelleştirme işlemi [iş](https://docs.microsoft.com/rest/api/media/jobs) varlığı değiştirmek için kullanılabilir *açıklama*ve *öncelik* iş gönderildikten sonra özellikleri. Bir değişiklik *öncelik* özelliği yalnızca proje yine de kuyruğa alınmış bir durumda ise etkin. İş işleme başladı ya da sona önceliğini değiştirme etkisi yoktur.

## <a name="typical-workflow"></a>Tipik iş akışı

1. Dönüşüm oluşturma 
2. Dönüşüm altında işlerini gönderme 
3. Liste dönüşümler 
4. Gelecekte kullanmayı planlamıyorsanız bir dönüşüm silin. 

### <a name="example"></a>Örnek

Tüm videolarınızı ilk karesine – bir küçük resim olarak çıkarmak istediğinizi varsayalım, uygulamanız gereken adımları şunlardır: 

1. Tarif veya videolarınızı işlemek için bir kural tanımlama - "ilk çerçeve video küçük resmi kullan". 
2. Her video için hizmet söyleyin: 
    1. Bu video nerede bulacağını,  
    2. Çıktı küçük resim görüntüsü yazmak yer. 

A **dönüştürme** tarif sonra (adım 1) oluşturun ve bu tarif (Adım 2) kullanarak göndermek yardımcı olur.

## <a name="job-error-codes"></a>İş hata kodları

Bkz: [hata kodları](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode).

## <a name="paging"></a>Paging

Bkz: [filtreleme, sıralama, Media Services varlıklarının sayfalandırma](entities-overview.md).

## <a name="configure-media-reserved-units"></a>Yapılandırma medya ayrılmış birimleri

Ses analizi ve Video analizi işleri, Media Services v3 tarafından tetiklenen veya Video Indexer için 10 S3 medya ayrılmış birimi (MRU) hesabınızla sağlama önemle tavsiye edilir. 10'dan fazla S3 MRU gerekiyorsa, kullanarak bir destek bileti açın [Azure portalında](https://portal.azure.com/).

Ayrıntılar için bkz [medya CLI ile işlemeyi ölçeklendirme](media-reserved-units-cli-how-to.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: .NET kullanarak videoları karşıya yükleme, kodlama ve akışla aktarma](stream-files-tutorial-with-api.md)
- [Öğretici: Media Services v3 ile .NET kullanarak videoları analiz etme](analyze-videos-tutorial-with-api.md)
