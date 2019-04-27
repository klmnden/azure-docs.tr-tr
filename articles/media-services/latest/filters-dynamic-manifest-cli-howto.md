---
title: Azure Media Services ile filtre oluşturma için CLI kullanma | Microsoft Docs
description: Bu konuda, Media Services ile filtre oluşturma için CLI kullanma gösterilmektedir.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 2ba3de32f4ec3b9f6faf1d5a51da9c1c91e4a2e4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60732442"
---
# <a name="creating-filters-with-cli"></a>CLI ile filtre oluşturma 

İçeriğinizi müşterilere (Canlı etkinlik veya isteğe bağlı Video akışı) sunarken istemcinizi varsayılan varlığın bildirim dosyasında tanımlanan değerinden daha fazla esneklik gerekebilir. Azure Media Services hesap filtreleri ve içeriğiniz için varlık filtrelerini tanımlamanızı sağlar. Daha fazla bilgi için [filtreleri ve dinamik bildirimlere](filters-dynamic-manifest-overview.md).

Bu konuda bir Video isteğe bağlı varlık için bir filtre yapılandırın ve oluşturmak için Media Services v3 için CLI'yi kullanma işlemi gösterilmektedir [hesap filtreleri](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) ve [varlık filtreleri](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest). 

## <a name="prerequisites"></a>Önkoşullar 

- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md). Kaynak grubu adı ve Media Services hesap adını hatırlamak emin olun. 
- Gözden geçirme [filtreleri ve dinamik bildirimlere](filters-dynamic-manifest-overview.md).

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

## <a name="define-a-filter"></a>Bir filtre tanımlar 

Aşağıdaki örnek, son bildirimine eklenmesini izleme seçimi koşulları tanımlar. Bu filtre EC-3 olan tüm ses parçalarını ve 0-1000000 hızına sahip olan herhangi bir video parçaları içerir aralığı.

REST içinde tanımlanan filtrelerini "Özellikler" sarmalayıcı JSON nesnesi içerir.  

```json
[
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
```

## <a name="create-account-filters"></a>Hesap filtreleri oluşturma

Aşağıdaki [az ams hesabına-filtre](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) komut olan izleme seçimlerini Filtresi ile bir hesabı filtresi oluşturur [daha önce tanımlanan](#define-a-filter). 

Komut isteğe bağlı geçirmenize olanak `--tracks` izleme seçimlerini temsil eden JSON içeren bir parametre.  Kullanım @{file} JSON bir dosyadan yüklenemiyor. Azure CLI'yı yerel olarak kullanıyorsanız, tüm dosya yolu belirtin:

```azurecli
az ams account-filter create -a amsAccount -g resourceGroup -n filterName --tracks @tracks.json
```

Ayrıca bkz [filtreleri için JSON örnekler](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate#create_an_account_filter).

## <a name="create-asset-filters"></a>Varlık filtre oluşturma

Aşağıdaki [az ams varlığı-filtre](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest) komut olan izleme seçimlerini Filtresi ile bir varlık filtresi oluşturur [daha önce tanımlanan](#define-a-filter). 

```azurecli
az ams asset-filter create -a amsAccount -g resourceGroup -n filterName --asset-name assetName --tracks @tracks.json
```

Ayrıca bkz [filtreleri için JSON örnekler](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate#create_an_asset_filter).

## <a name="next-step"></a>Sonraki adım

[Stream videoları](stream-files-tutorial-with-api.md) 

## <a name="see-also"></a>Ayrıca bkz.

[Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
