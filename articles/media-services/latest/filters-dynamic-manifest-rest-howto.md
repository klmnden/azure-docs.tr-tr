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
ms.date: 11/28/2018
ms.author: juliako
ms.openlocfilehash: 5cc670a94958b123ac71b49cbf25661d567e4629
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53083420"
---
# <a name="creating-filters-with-media-services-rest-api"></a>Media Services REST API'si ile filtre oluşturma

İçeriğinizi müşterilere (Canlı etkinlik veya isteğe bağlı Video akışı) sunarken istemcinizi varsayılan varlığın bildirim dosyasında tanımlanan değerinden daha fazla esneklik gerekebilir. Azure Media Services hesap filtreleri ve içeriğiniz için varlık filtrelerini tanımlamanızı sağlar. Daha fazla bilgi için [filtreleri ve dinamik bildirimlere](filters-dynamic-manifest-overview.md).

Bu konuda bir Video isteğe bağlı varlık için bir filtre tanımlar ve oluşturmak için REST API'lerini kullanma işlemi gösterilmektedir [hesap filtreleri](https://docs.microsoft.com/rest/api/media/accountfilters) ve [varlık filtreleri](https://docs.microsoft.com/rest/api/media/assetfilters). 

## <a name="prerequisites"></a>Önkoşullar 

Bu konu başlığı altında açıklanan adımları tamamlamak için için gerekenler:

- Gözden geçirme [filtreleri ve dinamik bildirimlere](filters-dynamic-manifest-overview.md).
- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md). Kaynak grubu adı ve Media Services hesap adını hatırlamak emin olun. 
- [Azure Media Services REST API çağrıları için Postman yapılandırma](media-rest-apis-with-postman.md).

## <a name="define-a-filter"></a>Bir filtre tanımlar  

Aşağıdaki **istek gövdesi** bildirimine eklenmesini izleme seçimi koşullarını tanımlayan örnek. Bu filtre EC-3 ile İngilizce olan tüm ses parçalarını ve 0-1000000 hızına sahip olan herhangi bir video parçaları içerir aralığı.

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
                        "property": "Language",
                        "value": "en",
                        "operation": "Equal"
                    },
                    {
                        "property": "FourCC",
                        "value": "EC-3",
                        "operation": "NotEqual"
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

PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{accountName}/accountFilters/{filterName}?api-version=2018-07-01

Seçin **gövdesi** sekmesi ve Yapıştır json kodunu [daha önce tanımlanan](#define-a-filter).

**Gönder**’i seçin. 

Filtre oluşturuldu.

Daha fazla bilgi için [oluşturma veya güncelleştirme](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate). Ayrıca bkz [filtreleri için JSON örnekler](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate#create_an_account_filter).

## <a name="create-asset-filters"></a>Varlık filtre oluşturma  

İndirdiğiniz "Media Services v3" Postman koleksiyonu içinde seçin **varlıklar**-> ** oluşturma veya güncelleştirme varlık filtre.

**PUT** HTTP istek yöntemine benzerdir:

PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{accountName}/assets/{assetName}/assetFilters/{filterName}?api-version=2018-07-01

Seçin **gövdesi** sekmesi ve Yapıştır json kodunu [daha önce tanımlanan](#define-a-filter).

**Gönder**’i seçin. 

Varlık filtre oluşturuldu.

Oluşturulacak veya güncelleştirilecek varlık filtreleri hakkında ayrıntılı bilgi için bkz. [oluşturma veya güncelleştirme](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate). Ayrıca bkz [filtreleri için JSON örnekler](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate#create_an_asset_filter). 

## <a name="next-steps"></a>Sonraki adımlar

[Stream videoları](stream-files-tutorial-with-rest.md) 
