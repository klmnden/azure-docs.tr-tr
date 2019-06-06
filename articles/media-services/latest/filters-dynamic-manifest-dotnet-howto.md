---
title: Azure Media Services .NET SDK ile Filtre Oluşturma
description: Bu konuda, istemci akışı için bir stream'ın belirli bölümlerine kullanabilmesi için filtreler oluşturmayı açıklar. Media Services, bu seçmeli akış elde etmek için olan dinamik bildirimler oluşturur.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/03/2019
ms.author: juliako
ms.openlocfilehash: 2bcb8762b94347f4409507fb89a18cb6c9d0dacd
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66494309"
---
# <a name="create-filters-with-media-services-net-sdk"></a>Media Services .NET SDK ile filtre oluşturma

İçeriğinizi müşterilere (Canlı etkinlik veya isteğe bağlı Video akışı) sunarken istemcinizi varsayılan varlığın bildirim dosyasında tanımlanan değerinden daha fazla esneklik gerekebilir. Azure Media Services hesap filtreleri ve içeriğiniz için varlık filtrelerini tanımlamanızı sağlar. 

Bu özellik ve onu kullanıldığı senaryoların ayrıntılı açıklaması için bkz [dinamik bildirimlerini](filters-dynamic-manifest-overview.md) ve [filtreleri](filters-concept.md).

Bu konuda bir Video isteğe bağlı varlık için bir filtre tanımlar ve oluşturmak için Media Services .NET SDK'sını kullanmayı gösterir [hesap filtreleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.accountfilter?view=azure-dotnet) ve [varlık filtreleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.assetfilter?view=azure-dotnet). 

> [!NOTE]
> Gözden geçirdiğinizden emin olun [presentationTimeRange](filters-concept.md#presentationtimerange).

## <a name="prerequisites"></a>Önkoşullar 

- Gözden geçirme [filtreleri ve dinamik bildirimlere](filters-dynamic-manifest-overview.md).
- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md). Kaynak grubu adı ve Media Services hesap adını hatırlamak emin olun. 
- İçin gereken bilgileri elde [API'lere erişim](access-api-cli-how-to.md)
- Gözden geçirme [karşıya yükleme, kodlama ve Azure Media Services'i kullanarak akış](stream-files-tutorial-with-api.md) görmek için nasıl [.NET SDK'sını kullanmaya başlayın](stream-files-tutorial-with-api.md#start_using_dotnet)

## <a name="define-a-filter"></a>Bir filtre tanımlar  

. NET'te, yapılandırdığınız izleme seçimleri [FilterTrackSelection](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.filtertrackselection?view=azure-dotnet) ve [FilterTrackPropertyCondition](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.filtertrackpropertycondition?view=azure-dotnet) sınıfları. 

Aşağıdaki kod EC-3 olan tüm ses parçalarını ve 0-1000000 hızına sahip olan herhangi bir video parçalar içeren bir filtre tanımlar aralığı.

```csharp
var audioConditions = new List<FilterTrackPropertyCondition>()
{
    new FilterTrackPropertyCondition(FilterTrackPropertyType.Type, "Audio", FilterTrackPropertyCompareOperation.Equal),
    new FilterTrackPropertyCondition(FilterTrackPropertyType.FourCC, "EC-3", FilterTrackPropertyCompareOperation.Equal)
};

var videoConditions = new List<FilterTrackPropertyCondition>()
{
    new FilterTrackPropertyCondition(FilterTrackPropertyType.Type, "Video", FilterTrackPropertyCompareOperation.Equal),
    new FilterTrackPropertyCondition(FilterTrackPropertyType.Bitrate, "0-1000000", FilterTrackPropertyCompareOperation.Equal)
};

List<FilterTrackSelection> includedTracks = new List<FilterTrackSelection>()
{
    new FilterTrackSelection(audioConditions),
    new FilterTrackSelection(videoConditions)
};
```

## <a name="create-account-filters"></a>Hesap filtreleri oluşturma

Aşağıdaki kod, tüm izleme seçimleri içeren bir hesap filtre oluşturmak için .NET kullanma işlemi gösterilmektedir [yukarıda tanımlanan](#define-a-filter). 

```csharp
AccountFilter accountFilterParams = new AccountFilter(tracks: includedTracks);
client.AccountFilters.CreateOrUpdate(config.ResourceGroup, config.AccountName, "accountFilterName1", accountFilter);
```

## <a name="create-asset-filters"></a>Varlık filtre oluşturma

Aşağıdaki kod tüm izleme seçimleri içeren bir varlık filtre oluşturmak için .NET kullanma işlemini gösterir [yukarıda tanımlanan](#define-a-filter). 

```csharp
AssetFilter assetFilterParams = new AssetFilter(tracks: includedTracks);
client.AssetFilters.CreateOrUpdate(config.ResourceGroup, config.AccountName, encodedOutputAsset.Name, "assetFilterName1", assetFilterParams);
```

## <a name="associate-filters-with-streaming-locator"></a>Filtreler akış Bulucu ile ilişkilendirme

Bir akış Bulucu için uygulamak varlık veya hesap filtrelerin listesini belirtebilirsiniz. [Dinamik Paketleyici (akış uç noktası)](dynamic-packaging-overview.md) bu olanlar istemcinizin URL'SİNDE belirtir birlikte filtrelerinin listesi için geçerlidir. Bu birleşim oluşturur bir [dinamik bildirim](filters-dynamic-manifest-overview.md), URL'deki filtreleri + akış Bulucu üzerinde belirttiğiniz filtreleri temel. Filtre uygulamak istediğiniz, ancak URL filtresi adlarında kullanıma sunmak istiyorsanız değil, bu özelliği kullanmanızı öneririz.

Aşağıdaki C# kod gösterir bir akış Bulucusu oluşturmak ve belirtmek nasıl `StreamingLocator.Filters`. Bu alan, isteğe bağlı bir özelliktir bir `IList<string>` filtre adları.

```csharp
IList<string> filters = new List<string>();
filters.Add("filterName");

StreamingLocator locator = await client.StreamingLocators.CreateAsync(
    resourceGroup,
    accountName,
    locatorName,
    new StreamingLocator
    {
        AssetName = assetName,
        StreamingPolicyName = PredefinedStreamingPolicy.ClearStreamingOnly,
        Filters = filters
    });
```
      
## <a name="stream-using-filters"></a>Stream, filtrelerini kullanma

İstemcilerinize filtreleri tanımladıktan sonra bunları akış URL'SİNDE kullanabilirsiniz. Filtreler hızı Uyarlamalı akış için uygulanabilir: Apple HTTP canlı akış (HLS), MPEG-DASH ve kesintisiz akış.

Aşağıdaki tabloda, filtrelerle URL'leri bazı örnekler gösterilmektedir:

|Protocol|Örnek|
|---|---|
|HLS|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|MPEG DASH|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Kesintisiz Akış|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(filter=myAssetFilter)`|

## <a name="next-steps"></a>Sonraki adımlar

[Stream videoları](stream-files-tutorial-with-api.md) 


