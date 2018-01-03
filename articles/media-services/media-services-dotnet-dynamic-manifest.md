---
title: "Azure Media Services .NET SDK ile Filtre Oluşturma"
description: "Bu konuda, istemci akış belirli bölümlerine bir akış için kullanabilmeniz için filtreleri oluşturmayı açıklar. Media Services bu seçmeli akış elde etmek için dinamik bildirimleri oluşturur."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 2f6894ca-fb43-43c0-9151-ddbb2833cafd
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 07/21/2017
ms.author: juliako;cenkdin
ms.openlocfilehash: 6c43473b86c14679ace558de478bd95f41d476da
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="creating-filters-with-azure-media-services-net-sdk"></a>Azure Media Services .NET SDK ile Filtre Oluşturma
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-dynamic-manifest.md)
> * [REST](media-services-rest-dynamic-manifest.md)
> 
> 

2.11 sürümünden başlayarak, Media Services, varlıklarınızı filtrelerini tanımlamanızı sağlar. Bu filtreler müşterilerinizin gibi şeyler seçmesine izin veren sunucu tarafı kurallardır: kayıttan yürütme (yerine tüm video oynatma), bir video yalnızca bir bölümünü veya yalnızca bir alt kümesini, müşterinizin aygıt (yerine varlıkla ilişkilendirilen tüm yorumlama) işleyebilir ses ve video yorumlama belirtin. Bu varlıklarınızı filtreleme aracılığıyla elde edilen **dinamik bildirim**video akışını sağlamak için Müşteri'nin istek üzerine oluşturulan s tabanlı üzerinde belirtilen filtreler.

Daha ayrıntılı filtreler ve dinamik bildirim ilgili bilgi için bkz: [dinamik bildirimleri genel bakış](media-services-dynamic-manifest-overview.md).

Bu konu, Media Services .NET SDK'sı oluşturmak, güncelleştirmek ve filtreleri silmek için nasıl kullanılacağını gösterir. 

Bir filtre güncelleştirirseniz, akış kuralları yenilemek için uç noktası için 2 dakika sürebilir unutmayın. İçerik bu filtre kullanarak sunulduğu (ve proxy'ler ve CDN önbellekleri önbelleğe alınmış), bu filtre güncelleştiriliyor player hatalarına neden olabilir. Bu filtre güncelleştirdikten sonra önbelleğini temizlemek için önerilir. Bu seçenek yoksa, başka bir filtre kullanmayı düşünün. 

## <a name="types-used-to-create-filters"></a>Filtreleri oluşturmak için kullanılan türleri
Aşağıdaki türlerden filtreleri oluşturulurken kullanılır: 

* **IStreamingFilter**.  Bu tür aşağıdaki REST API tabanlı [filtresi](https://docs.microsoft.com/rest/api/media/operations/filter)
* **IStreamingAssetFilter**. Bu tür aşağıdaki REST API tabanlı [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)
* **PresentationTimeRange**. Bu tür aşağıdaki REST API tabanlı [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)
* **FilterTrackSelectStatement** ve **IFilterTrackPropertyCondition**. Aşağıdaki REST API'leri temel alarak bu tür [FilterTrackSelect ve FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)

## <a name="createupdatereaddelete-global-filters"></a>Genel filtrelerin oluşturma/güncelleştirme/okuma/silme
Aşağıdaki kod, oluştur, Güncelleştir, okumak için .NET kullanın ve varlık filtreleri silmek gösterilmektedir.

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


## <a name="createupdatereaddelete-asset-filters"></a>Oluşturma/güncelleştirme/okuma/silme varlık filtreleri
Aşağıdaki kod, oluştur, Güncelleştir, okumak için .NET kullanın ve varlık filtreleri silmek gösterilmektedir.

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




## <a name="build-streaming-urls-that-use-filters"></a>Akış filtreleri kullanın URL'lerini derleme
Yayımlama ve varlıklarınızı teslim etmek hakkında daha fazla bilgi için bkz: [müşteriler genel bakış için içerik teslim](media-services-deliver-content-overview.md).

Aşağıdaki örnekler, akış URL'leri filtreleri ekleme gösterir.

**MPEG DASH** 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP canlı akış (HLS) V4**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP canlı akış (HLS) V3**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Kesintisiz akış**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Dinamik bildirimleri genel bakış](media-services-dynamic-manifest-overview.md)

