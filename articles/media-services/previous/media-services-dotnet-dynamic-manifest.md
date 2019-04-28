---
title: Azure Media Services .NET SDK ile Filtre Oluşturma
description: Bu konuda, istemci akışı için bir stream'ın belirli bölümlerine kullanabilmesi için filtreler oluşturmayı açıklar. Media Services, bu seçmeli akış elde etmek için olan dinamik bildirimler oluşturur.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 2f6894ca-fb43-43c0-9151-ddbb2833cafd
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako;cenkdin
ms.openlocfilehash: 05b899658b5c58e15b2f30ab759eb49319979fee
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61465567"
---
# <a name="creating-filters-with-media-services-net-sdk"></a>Media Services .NET SDK ile filtre oluşturma 
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-dynamic-manifest.md)
> * [REST](media-services-rest-dynamic-manifest.md)
> 
> 

2.17 sürümünden başlayarak, Media Services, varlıklarınız için filtrelerini tanımlamanızı sağlar. Bu filtreler müşterileriniz gibi şeyler seçmesine izin veren sunucu tarafı kurallar şunlardır: yalnızca bir bölümünü (yerine tüm video oynatma), bir video kayıttan yürütme ya da yalnızca bir alt kümesini (yerine, müşterinizin cihaz işleyebilir ses ve video yorumlama belirtin bir varlıkla ilişkilendirilen tüm yorumlama). Bu varlıklarınızı filtreleme yoluyla elde edilir **dinamik bildirim**müşterinizin istek üzerine video akışı oluşturan s üzerinde belirtilen filtreleri temel.

Daha ayrıntılı filtreler ve dinamik bildirim ilgili bilgi için bkz. [dinamik bildirimlerin genel bakış](media-services-dynamic-manifest-overview.md).

Bu makalede, oluşturmak, güncelleştirmek ve filtreleri silmek için Media Services .NET SDK'sını kullanmayı gösterir. 

Bir filtre güncelleştirirseniz, akış uç kuralları yenilemek için iki dakika sürebilir unutmayın. İçerik için bu filtreyi kullanarak hizmet (ve bu önbellekler proxy'leri ve CDN önbelleğe), bu filtre güncelleştirme player hatalarına neden olabilir. Her zaman filtresi güncelleştirdikten sonra önbelleğini temizleyin. Bu seçenek, mümkün değildir. farklı bir filtre kullanmayı düşünün. 

## <a name="types-used-to-create-filters"></a>Filtreler oluşturmak için kullanılan türleri
Aşağıdaki türleri filtreler oluşturulurken kullanılır: 

* **IStreamingFilter**.  Bu tür, aşağıdaki REST API ile ilgili temel [filtresi](https://docs.microsoft.com/rest/api/media/operations/filter)
* **IStreamingAssetFilter**. Bu tür, aşağıdaki REST API ile ilgili temel [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)
* **PresentationTimeRange**. Bu tür, aşağıdaki REST API ile ilgili temel [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)
* **FilterTrackSelectStatement** ve **IFilterTrackPropertyCondition**. Aşağıdaki REST API'leri temel alarak bu tür [FilterTrackSelect ve FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)

## <a name="createupdatereaddelete-global-filters"></a>Genel filtrelerin oluşturma/güncelleştirme/okuma/silme
Aşağıdaki kod, .NET oluşturma, güncelleştirme, okuma ve varlık filtreleri silme gösterilmektedir.

```csharp
    string filterName = "GlobalFilter_" + Guid.NewGuid().ToString();

    List<FilterTrackSelectStatement> filterTrackSelectStatements = new List<FilterTrackSelectStatement>();

    FilterTrackSelectStatement filterTrackSelectStatement = new FilterTrackSelectStatement();
    filterTrackSelectStatement.PropertyConditions = new List<IFilterTrackPropertyCondition>();
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackNameCondition("Track Name", FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackBitrateRangeCondition(new FilterTrackBitrateRange(0, 1), FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackTypeCondition(FilterTrackType.Audio, FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatements.Add(filterTrackSelectStatement);

    // Create
    IStreamingFilter filter = _context.Filters.Create(filterName, new PresentationTimeRange(), filterTrackSelectStatements);

    // Update
    filter.PresentationTimeRange = new PresentationTimeRange(timescale: 500);
    filter.Update();

    // Read
    var filterUpdated = _context.Filters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filter.Delete();
```

## <a name="createupdatereaddelete-asset-filters"></a>Oluşturma/güncelleştirme/okuma/silme varlık filtreleri
Aşağıdaki kod, .NET oluşturma, güncelleştirme, okuma ve varlık filtreleri silme gösterilmektedir.

```csharp
    string assetName = "AssetFilter_" + Guid.NewGuid().ToString();
    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);

    string filterName = "AssetFilter_" + Guid.NewGuid().ToString();


    // Create
    IStreamingAssetFilter filter = asset.AssetFilters.Create(filterName,
                                        new PresentationTimeRange(), 
                                        new List<FilterTrackSelectStatement>());

    // Update
    filter.PresentationTimeRange = 
            new PresentationTimeRange(start: 6000000000, end: 72000000000);

    filter.Update();

    // Read
    asset = _context.Assets.Where(c => c.Id == asset.Id).FirstOrDefault();
    var filterUpdated = asset.AssetFilters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filterUpdated.Delete();

```


## <a name="build-streaming-urls-that-use-filters"></a>Akış Filtreleri kullanan URL'lerini oluşturun
Yayımlama ve varlıklarınızı teslim hakkında daha fazla bilgi için bkz: [müşterilerin genel bakış için içerik teslim](media-services-deliver-content-overview.md).

Aşağıdaki örnekler, akış URL'leri için filtreleri ekleyin gösterilmektedir.

**MPEG DASH** 

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP canlı akış (HLS) V4**

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP canlı akış (HLS) V3**

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Kesintisiz akış**

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Dinamik bildirimlere genel bakış](media-services-dynamic-manifest-overview.md)

