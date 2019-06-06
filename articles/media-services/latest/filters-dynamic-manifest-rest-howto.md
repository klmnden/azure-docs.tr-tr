---
title: Filtre oluşturma ile Azure Media Services REST API | Microsoft Docs
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
ms.openlocfilehash: 01c1711fb70d31fe84c7e20272de0eb7ce82c879
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66494230"
---
# <a name="creating-filters-with-media-services-rest-api"></a>Media Services REST API'si ile filtre oluşturma

İçeriğinizi müşterilere (Canlı etkinlik veya isteğe bağlı Video akışı) sunarken istemcinizi varsayılan varlığın bildirim dosyasında tanımlanan değerinden daha fazla esneklik gerekebilir. Azure Media Services hesap filtreleri ve içeriğiniz için varlık filtrelerini tanımlamanızı sağlar. 

Bu özellik ve onu kullanıldığı senaryoların ayrıntılı açıklaması için bkz [dinamik bildirimlerini](filters-dynamic-manifest-overview.md) ve [filtreleri](filters-concept.md).

Bu konuda bir Video isteğe bağlı varlık için bir filtre tanımlar ve oluşturmak için REST API'lerini kullanma işlemi gösterilmektedir [hesap filtreleri](https://docs.microsoft.com/rest/api/media/accountfilters) ve [varlık filtreleri](https://docs.microsoft.com/rest/api/media/assetfilters). 

> [!NOTE]
> Gözden geçirdiğinizden emin olun [presentationTimeRange](filters-concept.md#presentationtimerange).

## <a name="prerequisites"></a>Önkoşullar 

Bu konu başlığı altında açıklanan adımları tamamlamak için için gerekenler:

- Gözden geçirme [filtreleri ve dinamik bildirimlere](filters-dynamic-manifest-overview.md).
- [Azure Media Services REST API çağrıları için Postman yapılandırma](media-rest-apis-with-postman.md).

    Konunun son adımı takip edin [Azure AD belirteci Al](media-rest-apis-with-postman.md#get-azure-ad-token). 

## <a name="define-a-filter"></a>Bir filtre tanımlar  

Aşağıdaki **istek gövdesi** bildirimine eklenmesini izleme seçimi koşullarını tanımlayan örnek. Bu filtre EC-3 olan tüm ses parçalarını ve 0-1000000 hızına sahip olan herhangi bir video parçaları içerir aralığı.

```json
{
    "properties": {
        "tracks": [
          {
                "trackSelections": [
                    {
                        "property": "Type",
                        "value": "Audio",
                        "operation": "Equal"
                    },
                    {
                        "property": "FourCC",
                        "value": "EC-3",
                        "operation": "Equal"
                    }
                ]
            },
            {
                "trackSelections": [
                    {
                        "property": "Type",
                        "value": "Video",
                        "operation": "Equal"
                    },
                    {
                        "property": "Bitrate",
                        "value": "0-1000000",
                        "operation": "Equal"
                    }
                ]
            }
        ]
     }
}
```

## <a name="create-account-filters"></a>Hesap filtreleri oluşturma

İndirdiğiniz Postman'ın koleksiyonda seçin **hesap filtreleri**->**oluşturma veya güncelleştirme Hesap Filtresi**.

**PUT** HTTP istek yöntemine benzerdir:

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{accountName}/accountFilters/{filterName}?api-version=2018-07-01
```

Seçin **gövdesi** sekmesi ve Yapıştır json kodunu [daha önce tanımlanan](#define-a-filter).

**Gönder**’i seçin. 

Filtre oluşturuldu.

Daha fazla bilgi için [oluşturma veya güncelleştirme](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate). Ayrıca bkz [filtreleri için JSON örnekler](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate#create_an_account_filter).

## <a name="create-asset-filters"></a>Varlık filtre oluşturma  

İndirdiğiniz "Media Services v3" Postman koleksiyonu içinde seçin **varlıklar**->**oluşturma veya güncelleştirme varlık filtre**.

**PUT** HTTP istek yöntemine benzerdir:

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{accountName}/assets/{assetName}/assetFilters/{filterName}?api-version=2018-07-01
```

Seçin **gövdesi** sekmesi ve Yapıştır json kodunu [daha önce tanımlanan](#define-a-filter).

**Gönder**’i seçin. 

Varlık filtre oluşturuldu.

Oluşturulacak veya güncelleştirilecek varlık filtreleri hakkında ayrıntılı bilgi için bkz. [oluşturma veya güncelleştirme](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate). Ayrıca bkz [filtreleri için JSON örnekler](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate#create_an_asset_filter). 

## <a name="next-steps"></a>Sonraki adımlar

[Stream videoları](stream-files-tutorial-with-rest.md) 
