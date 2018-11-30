---
title: Ölçeklendirme medya ayrılmış birimleri - Azure | Microsoft Docs
description: Bu konuda, ölçeklendirme medya işleme Azure Media Services ile bir genel bakıştır.
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
ms.openlocfilehash: 9099429097efb17629e88318430f004f0f763cc5
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52337049"
---
# <a name="creating-filters-with-cli"></a>CLI ile filtre oluşturma 

İçeriğinizi müşterilere (Canlı etkinlik veya isteğe bağlı Video akışı) sunarken istemcinizi varsayılan varlığın bildirim dosyasında tanımlanan değerinden daha fazla esneklik gerekebilir. Azure Media Services hesap filtreleri ve içeriğiniz için varlık filtrelerini tanımlamanızı sağlar. Daha fazla bilgi için [filtreleri ve dinamik bildirimlere](filters-dynamic-manifest-overview.md).

Bu konuda bir Video isteğe bağlı varlık için bir filtre yapılandırın ve oluşturmak için Media Services v3 için CLI'yi kullanma işlemi gösterilmektedir [hesap filtreleri](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) ve [varlık filtreleri](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest). 

## <a name="prerequisites"></a>Önkoşullar 

- Yükleyin ve bu makalede Azure CLI 2.0 veya sonraki bir sürüm gerektirir, CLI'yı yerel olarak kullanın. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

    Şu anda tüm [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref) komutlar Azure Cloud Shell içinde çalışır. CLI'yi yerel olarak kullanmak için önerilir.
- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md). Kaynak grubu adı ve Media Services hesap adını hatırlamak emin olun. 
- Gözden geçirme [filtreleri ve dinamik bildirimlere](filters-dynamic-manifest-overview.md).

## <a name="define-a-filter"></a>Bir filtre tanımlar 

Aşağıdaki örnek, son bildirimine eklenmesini izleme seçimi koşulları tanımlar. Bu filtre EC-3 ile İngilizce olan tüm ses parçalarını ve 0-1000000 hızına sahip olan herhangi bir video parçaları içerir aralığı.

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
```

## <a name="create-account-filters"></a>Hesap filtreleri oluşturma

Aşağıdaki [az ams hesabına-filtre](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) komut olan izleme seçimlerini Filtresi ile bir hesabı filtresi oluşturur [daha önce tanımlanan](#define-a-filter). 

```azurecli
az ams account-filter create -a amsAccount -g resourceGroup -n filterName --tracks @C:\tracks.json
```

Ayrıca bkz [filtreleri için JSON örnekler](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate#create_an_account_filter).

## <a name="create-asset-filters"></a>Varlık filtre oluşturma

Aşağıdaki [az ams varlığı-filtre](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest) komut olan izleme seçimlerini Filtresi ile bir varlık filtresi oluşturur [daha önce tanımlanan](#define-a-filter). 

```azurecli
az ams asset-filter create -a amsAccount -g resourceGroup -n filterName --asset-name assetName --tracks @C:\tracks.json
```

Ayrıca bkz [filtreleri için JSON örnekler](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate#create_an_asset_filter).

## <a name="next-step"></a>Sonraki adım

[Stream videoları](stream-files-tutorial-with-api.md) 

## <a name="see-also"></a>Ayrıca bkz.

[Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
