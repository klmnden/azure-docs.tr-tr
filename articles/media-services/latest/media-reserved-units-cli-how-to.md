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
ms.date: 11/11/2018
ms.author: juliako
ms.openlocfilehash: dd587e5fc2082d1e496fbc05d5b25cf6692413bc
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51713070"
---
# <a name="scaling-media-processing"></a>Medya işlemeyi ölçeklendirme

Azure Media Services, medya ayrılmış birimi (MRU) yöneterek, hesabınızdaki medya işlemeyi ölçeklendirme sağlar. Ayrıntılı bir bakış için bkz: [medya işlemeyi ölçeklendirme](../previous/media-services-scale-media-processing-overview.md). 

Bu makalede nasıl kullanılacağını gösterir [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref) MRU ölçeklendirebilirsiniz.

> [!NOTE]
> Ses analizi ve Video analizi işleri, Media Services v3 tarafından tetiklenen veya Video Indexer için 10 S3 MRU hesabınızla sağlama önemle tavsiye edilir. <br/>10'dan fazla S3 MRU gerekiyorsa, kullanarak bir destek bileti açın [Azure portalında](https://portal.azure.com/).

## <a name="prerequisites"></a>Önkoşullar 

- Yükleyin ve bu makalede Azure CLI 2.0 veya sonraki bir sürüm gerektirir, CLI'yı yerel olarak kullanın. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

    Şu anda tüm [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref) komutlar Azure Cloud Shell içinde çalışır. CLI'yi yerel olarak kullanmak için önerilir.

- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md).

## <a name="scale-media-reserved-units-with-cli"></a>Ölçek medya ayrılmış birimleri CLI ile

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
