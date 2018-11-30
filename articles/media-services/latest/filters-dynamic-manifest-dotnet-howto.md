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
ms.date: 11/28/2018
ms.author: juliako
ms.openlocfilehash: 23e83b98288f9ac1fe23e01b9a91d81daa3b0f47
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52632390"
---
# <a name="create-filters-with-media-services-net-sdk"></a>Media Services .NET SDK ile filtre oluşturma

İçeriğinizi müşterilere (Canlı etkinlik veya isteğe bağlı Video akışı) sunarken istemcinizi varsayılan varlığın bildirim dosyasında tanımlanan değerinden daha fazla esneklik gerekebilir. Azure Media Services hesap filtreleri ve içeriğiniz için varlık filtrelerini tanımlamanızı sağlar. Daha fazla bilgi için [filtreleri ve dinamik bildirimlere](filters-dynamic-manifest-overview.md).

Bu konuda bir Video isteğe bağlı varlık için bir filtre tanımlar ve oluşturmak için Media Services .NET SDK'sını kullanmayı gösterir [hesap filtreleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.accountfilter?view=azure-dotnet) ve [varlık filtreleri](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.assetfilter?view=azure-dotnet). 

## <a name="prerequisites"></a>Önkoşullar 

- Gözden geçirme [filtreleri ve dinamik bildirimlere](filters-dynamic-manifest-overview.md).
- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md). Kaynak grubu adı ve Media Services hesap adını hatırlamak emin olun. 
- İçin gereken bilgileri elde [API'lere erişim](access-api-cli-how-to.md)
- Gözden geçirme [karşıya yükleme, kodlama ve Azure Media Services'i kullanarak akış](stream-files-tutorial-with-api.md) görmek için nasıl [.NET SDK'sını kullanmaya başlayın](stream-files-tutorial-with-api.md#start_using_dotnet)

## <a name="define-a-filter"></a>Bir filtre tanımlar  

. NET'te, yapılandırdığınız izleme seçimleri [FilterTrackSelection](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.filtertrackselection?view=azure-dotnet) ve [FilterTrackPropertyCondition](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.filtertrackpropertycondition?view=azure-dotnet) sınıfları. 

Aşağıdaki kod ile EC-3 İngilizce olan tüm ses parçaları ve 0-1000000 hızına sahip olan herhangi bir video parçalar içeren bir filtre tanımlar aralığı.

```csharp
var audioConditions = new List<FilterTrackPropertyCondition>()
{
    new FilterTrackPropertyCondition(FilterTrackPropertyType.Language, "en-us", FilterTrackPropertyCompareOperation.Equal),
    new FilterTrackPropertyCondition(FilterTrackPropertyType.FourCC, "EC-3", FilterTrackPropertyCompareOperation.Equal)
};

var videoConditions = new List<FilterTrackPropertyCondition>()
{
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

## <a name="next-steps"></a>Sonraki adımlar

[Stream videoları](stream-files-tutorial-with-api.md) 


