---
title: Azure medya ayrılmış birimi - ölçeklendirmek için CLI kullanma | Microsoft Docs
description: Bu konuda, ölçeklendirme medya Azure Media Services ile işleme için CLI kullanma gösterilmektedir.
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
ms.date: 01/24/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: b40ab6bcc2f718eda85ff64d69a6689e12d60ab8
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2019
ms.locfileid: "55094844"
---
# <a name="scaling-media-processing"></a>Medya işlemeyi ölçeklendirme

Azure Media Services, medya ayrılmış birimi (MRU) yöneterek, hesabınızdaki medya işlemeyi ölçeklendirme sağlar. Ayrıntılı bir bakış için bkz: [medya işlemeyi ölçeklendirme](../previous/media-services-scale-media-processing-overview.md). 

Bu makalede nasıl kullanılacağını gösterir [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref) MRU ölçeklendirebilirsiniz.

> [!NOTE]
> Ses analizi ve Video analizi işleri, Media Services v3 tarafından tetiklenen veya Video Indexer için 10 S3 MRU hesabınızla sağlama önemle tavsiye edilir. <br/>10'dan fazla S3 MRU gerekiyorsa, kullanarak bir destek bileti açın [Azure portalında](https://portal.azure.com/).

## <a name="prerequisites"></a>Önkoşullar 

[Bir Media Services hesabı oluşturma](create-account-cli-how-to.md).

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

## <a name="scale-media-reserved-units-with-cli"></a>Ölçek medya ayrılmış birimleri CLI ile

`mru` komutunu çalıştırın.

Aşağıdaki [az ams hesabınızı mru](https://docs.microsoft.com/cli/azure/ams/account/mru?view=azure-cli-latest) komut, medya ayrılmış birimi "amsaccount" hesabını kullanarak kümeleri **sayısı** ve **türü** parametreleri.

```azurecli
az account set mru -n amsaccount -g amsResourceGroup --count 10 --type S3
```

## <a name="billing"></a>Faturalandırma

Hesabınızdaki sayısı, türü ve MRU sağlanan süre göre ücretlendirilir. Herhangi bir işi çalıştırma olup olmadığını ücretleri uygulanır. Ayrıntılı bir açıklaması için SSS bölümüne bakın. [Media Services fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) sayfası.   

## <a name="next-step"></a>Sonraki adım

[Videoları analiz etme](analyze-videos-tutorial-with-api.md) 

## <a name="see-also"></a>Ayrıca bkz.

[Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
