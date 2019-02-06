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
ms.date: 02/03/2019
ms.author: juliako
ms.openlocfilehash: e84f74fe4678a65a33c9cc728f290e7c905b2261
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2019
ms.locfileid: "55743744"
---
# <a name="transforms-and-jobs"></a>Dönüşümler ve İşler
 
Kullanım [dönüştüren](https://docs.microsoft.com/rest/api/media/transforms) kodlama veya videoları analiz için ortak görevler yapılandırmak için. Her **dönüştürme** bir tarif veya bir iş akışı, video veya ses dosyalarını işlemek için görevler açıklanmaktadır. Tek bir dönüştürme, birden fazla kural uygulayabilirsiniz. Örneğin, bir dönüştürme belirli bir bit hızı anda bir MP4 dosyasına her video kodlanmış ve bir küçük resim videonun ilk çerçevesinden oluşturulmasını belirtebilirsiniz. Dönüştürme, dahil etmek istediğiniz her bir kural için bir TransformOutput girdi eklersiniz. Media Services v3 API veya herhangi bir yayımlanan SDK'ları kullanarak Media Services hesabınızı dönüştürmeler oluşturabilirsiniz. Media Services hesabınızı oluşturmak ve dağıtmak için Resource Manager şablonları kullanabilmeniz için API Azure Resource Manager tarafından yönetilen Media Services v3 dönüştürür. Rol tabanlı erişim denetimi, dönüşümler erişimi kilitlemek için kullanılabilir.

Güncelleştirme işlemi [dönüştürme](https://docs.microsoft.com/rest/api/media/transforms) varlık açıklaması veya temel alınan TransformOutputs önceliklerini değişiklikler için tasarlanmıştır. Tüm devam eden işleri tamamladığınızda bu güncelleştirmeler yapılması önerilir. Tarif yeniden düşünüyorsanız, yeni bir dönüştürme oluşturmanız gerekir.

A [iş](https://docs.microsoft.com/rest/api/media/jobs) uygulamak için Azure Media Services için fiili istek **dönüştürme** belirli bir giriş video veya ses içeriği için. Dönüştürme oluşturulduktan sonra Media Services API'leri ve yayımlanan SDK'ları hiçbirini kullanarak işleri gönderebilirsiniz. **İş** konumun video giriş ve çıkış konumunu gibi bilgileri belirtir. Giriş video kullanarak konumu belirtebilirsiniz: HTTPS URL'leri, SAS URL'lerini veya [varlıklar](https://docs.microsoft.com/rest/api/media/assets). İşlerin durumunu ve ilerleme durumunu izleme olayları Event Grid ile tarafından alınabilir. Daha fazla bilgi için [EventGrid kullanarak olayları izleme](job-state-events-cli-how-to.md).

Güncelleştirme işlemi [iş](https://docs.microsoft.com/rest/api/media/jobs) varlığı değiştirmek için kullanılabilir *açıklama*ve *öncelik* iş gönderildikten sonra özellikleri. Bir değişiklik *öncelik* özelliği yalnızca proje yine de kuyruğa alınmış bir durumda ise etkin. İş işleme başladı ya da sona önceliğini değiştirme etkisi yoktur.

> [!NOTE]
> Özelliklerini **dönüştürme** ve **iş** DateTime türü her zaman UTC biçiminde olan.

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

## <a name="paging"></a>Sayfalama

Bkz: [filtreleme, sıralama, Media Services varlıklarının sayfalandırma](entities-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

[Karşıya yükleme, kodlama ve video dosyaları akışla aktarma](stream-files-tutorial-with-api.md)
