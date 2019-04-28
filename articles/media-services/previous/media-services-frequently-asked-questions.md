---
title: Azure Media Services hakkında sık sorulan sorular | Microsoft Docs
description: Sık sorulan sorular (SSS)
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
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 3164445dc28a2436d8492902d865414da8d75702
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61463931"
---
# <a name="frequently-asked-questions"></a>Sık sorulan sorular

Bu makalede Azure Media Services (AMS) kullanıcı topluluğu tarafından harekete geçirilen sık sorulan soruları ele alır.

## <a name="general-ams-faqs"></a>Genel AMS SSS

S: Apple iOS cihazlarına nasıl akışını?

Y: Ekle "(biçim = m3u8-aapl)" yerel cihazlar Apple iOS arka HLS içerik tüketimi için döndürülecek akış kaynağı sunucusu bildirmek için URL "/ bildirimi" bölümünü yoluna (Ayrıntılar için bkz [içerik sağlama konusuna](media-services-deliver-content-overview.md)),

S: Dizin oluşturma nasıl ölçekleme?

Y: Ayrılmış birimler Encoding ve dizin görevleri için aynıdır. Yönergeleri takip edin [ölçek Encoding'e ayrılan birim nasıl](media-services-scale-media-processing-overview.md). **Not** dizin oluşturucu performans ayrılmış birim türüyle etkilenmez.

S: Ben karşıya, kodlanmış ve video yayımladınız. Bu akış çalıştığınızda video nedeni ne olacağını yürütülmüyor?

Y: En yaygın nedenlerinden biri olduğundan, kayıttan yürütme aktarmaya çalıştığınız akış uç noktası olmayan **çalıştıran** durumu.  

S: Canlı akış üzerinde birleştirme yapabilirim?

Y: Bilgisayarınızda önceden oluşturmak gerekebilir birleştirme Canlı akışlar üzerinde şu anda Azure Media Services'da sunulmaz.

S: Azure CDN canlı akış ile kullanabilir miyim?

Y: Media Services, Azure CDN ile tümleştirmeyi destekler (daha fazla bilgi için [akış uç noktalarını yönetme Media Services hesabı nasıl](media-services-portal-manage-streaming-endpoints.md)).  Canlı akış CDN ile kullanabilirsiniz. Azure Media Services, kesintisiz akış, HLS ve MPEG-DASH çıkışları sağlar. Tüm bu biçimleri veri aktarmak için HTTP kullanmasına ve HTTP önbelleğe alma avantajlarından yararlanın. Canlı video/ses veriler parçalara bölünür ve bu tek tek parçaları CDN'de önbelleğe gerçek akış. Yalnızca veri ihtiyaçları yenilenmesi için bildirim veri olacaktır. CDN, bildirim verileri düzenli aralıklarla yeniler.

S: Azure Media services, depolama görüntüleri destekliyor mu?

Y: Yalnızca JPEG veya PNG görüntülerini depolamak için kullanmak istiyorsanız, bu Azure Blob Depolama alanında saklamanız gerekir. Bunları Video veya ses varlıklar ile ilişkili tutmak istemediğiniz sürece, bunları Media Services hesabınızı yerleştirme için hiçbir avantajı yoktur. Veya video Kodlayıcısı içinde yer paylaşımları olarak görüntüleri kullanmak için bir gereksinimi varsa. Media Encoder Standard videoları en üstünde yer paylaşımı görüntüleri destekler ve hangi JPEG ve desteklenen PNG listeler giriş biçimleri. Daha fazla bilgi için [yer paylaşımları oluşturma](media-services-advanced-encoding-with-mes.md#overlay).

S: Nasıl varlıklar bir Media Services hesabından diğerine kopyalayabilirim?

Y: Varlıklar Media Services hesabından diğerine kopyalamak için .NET kullanarak kullanın [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) genişletme yöntemi içinde kullanılabilir [Azure Media Services .NET SDK uzantıları](https://github.com/Azure/azure-sdk-for-media-services-extensions/) depo. Daha fazla bilgi için [bu](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) forum dizisinin.

S: AMS ile çalışırken, dosya adlandırma desteklenen karakterler nelerdir?

Y: Media Services IAssetFile.Name özelliğinin değeri, URL'leri akış içeriği için (örneğin, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) oluştururken kullanır. Bu nedenle, yüzde kodlama izin verilmez. Değerini **adı** özelliği aşağıdakilerden herhangi birini içeremez [yüzde kodlama-ayrılmış karakterleri](https://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ". Ayrıca, yalnızca bir olabilir '.' Dosya adı uzantısı.

S: REST kullanarak bağlanmasına nasıl?

Y: AMS API'ye bağlanma hakkında daha fazla bilgi için bkz: [Azure AD kimlik doğrulamasıyla Azure Media Services API'sine erişim](media-services-use-aad-auth-to-access-ams-api.md). 

S: Bir video'ı kodlama işlemi sırasında nasıl döndürebilirsiniz?

Y: [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) döndürme açısı tarafından 90/180/270 destekler. Varsayılan "Auto" nerede gelen MP4/MOV dosyasında döndürme meta verileri algılamak ve için dengelemek için çalışır, davranıştır. Şunlar **kaynakları** öğesi tanımlı json hazır birine [burada](media-services-mes-presets-overview.md):

    "Version": 1.0,
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [

    ...


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
