---
title: "Azure Media Services sık sorulan sorular | Microsoft Docs"
description: "Sık sorulan sorular (SSS)"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 5374f7f4-c189-43ef-8b7f-f2f4141e2748
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: juliako
ms.openlocfilehash: ab66994b0212593aff1384b0801f3359eb0a3751
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2018
---
# <a name="frequently-asked-questions"></a>Sık sorulan sorular

Bu makalede Azure Media Services (AMS) kullanıcı topluluğu tarafından gerçekleştirilen sık sorulan sorular giderir.

## <a name="general-ams-faqs"></a>Genel AMS SSS

S: Apple iOS cihazlarına nasıl akış

A: eklemek "(format = m3u8-aapl)" (Ayrıntılar için bkz: (içerik teslim) [yerel cihazlar Apple iOS geri HLS içerik tüketimi için döndürülecek akış kaynak sunucu bildirmek için URL "/ bildirimi" bölümünü yoluna Media-services-teslim-içerik-overview.md]),

S: nasıl dizin ölçeklendirme?

Y: ayrılan birimler kodlama ve dizin oluşturma görevleri için aynıdır. Yönergeleri izleyerek [ölçek kodlamaya ayrılan birimler nasıl](media-services-scale-media-processing-overview.md). **Not** dizin oluşturucu performans ayrılmış birim türü tarafından etkilenmez.

S: karşıya kodlanmış ve video yayımlandı. Bu akış çalıştığınızda ne video neden olacağını yürütülmüyor?

En yaygın nedenleri şunlardan biri olan içinden çalıştığınız kayıttan yürütme akış uç noktasına sahip olmadığınız **çalıştıran** durumu.  

S: bir canlı akış üzerinde birleştirme yapabilirim?

A: bilgisayarınızda önceden oluşturmak gerekebilir birleştirme Canlı akışlar üzerinde Azure Media Services ile şu anda önerilmez.

S: Azure CDN canlı akış ile kullanabilir miyim?

Y: Media Services, Azure CDN ile tümleştirmeyi destekler (daha fazla bilgi için bkz: [akış uç noktalarını yönetme Media Services hesabı nasıl](media-services-portal-manage-streaming-endpoints.md)).  Canlı akış CDN ile kullanabilirsiniz. Azure Media Services, kesintisiz akış, HLS ve MPEG-DASH çıkışları sağlar. Bu biçimler verileri aktarmak için HTTP kullanmasına ve HTTP önbelleğe alma avantajlarını elde edersiniz. Canlı video/ses veri parçada ayrılmıştır ve bu tek tek parçaları CDN'de önbelleğe gerçek akış içinde. Yalnızca veri ihtiyaçları yenilenmesi bildirim veri olacaktır. CDN bildirim verileri düzenli aralıklarla yeniler.

S: mu Azure Media services depolanmasını görüntüleri destekliyor?

A:, yalnızca JPEG veya PNG görüntüleri depolamak için arıyorsanız, Azure Blob Storage de tutmanız gerekir. Video veya ses varlıklar ile ilişkili kalmasını istemiyorsanız Media Services hesabınızı koyma hiçbir faydası yoktur. Ya da bir video Kodlayıcısı içindeki yer paylaşımları olarak görüntüleri kullanmak için bir gereksiniminiz varsa. Medya Kodlayıcısı standart destekler videolar en üstünde yer paylaşımı görüntüler ve hangi JPEG ve desteklenen gibi PNG listeler olan giriş biçimleri. Daha fazla bilgi için bkz: [oluşturma yer paylaşımları](media-services-advanced-encoding-with-mes.md#overlay).

S: nasıl ı varlıklar bir Media Services hesaptan diğerine kopyalayabilirsiniz.

A: varlıklar bir Media Services hesaptan diğerine kopyalamak için .NET, kullanarak [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) genişletme yöntemi bulunan [Azure Media Services .NET SDK uzantıları](https://github.com/Azure/azure-sdk-for-media-services-extensions/) deposu. Daha fazla bilgi için bkz: [bu](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) Forumu iş parçacığı.

S: AMS ile çalışırken, dosya adlandırma desteklenen karakter nelerdir?

Y: Media Services IAssetFile.Name özelliğinin değeri, URL'ler akış içeriğini (örneğin, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) oluştururken kullanır. Bu nedenle, yüzde kodlama izin verilmiyor. Değeri **adı** özelliği aşağıdakilerden herhangi birini içeremez [yüzde kodlama-ayrılmış karakterleri](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ". Ayrıca, yalnızca bir olabilir '.' Dosya adı uzantısı.

S: REST kullanarak bağlanmasına nasıl?

Y: AMS API'sine bağlanma hakkında bilgi için [Azure AD kimlik doğrulaması ile Azure Media Services API erişim](media-services-use-aad-auth-to-access-ams-api.md). 

S: nasıl ı kodlama işlemi sırasında bir video döndürebilirsiniz.

Y: [Medya Kodlayıcısı standart](media-services-dotnet-encode-with-media-encoder-standard.md) 180/90/270 açıları tarafından dönüşünü destekler. "Otomatik" nerede gelen MP4/MOV dosyasında döndürme meta verileri algılamak ve bunun için dengelemek için çalışır, varsayılan davranıştır. Aşağıdakiler dahil **kaynakları** tanımlı json hazır ayarlarından birini öğesine [burada](media-services-mes-presets-overview.md):

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
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
