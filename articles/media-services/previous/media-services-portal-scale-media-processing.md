---
title: Azure portalını kullanarak medya işlemeyi ölçeklendirme | Microsoft Docs
description: Bu öğreticide, Azure portalını kullanarak işleme ölçeklendirme medya adımları gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: e500f733-68aa-450c-b212-cf717c0d15da
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/05/2018
ms.author: juliako
ms.openlocfilehash: 2aabdc503a694de46796b7c3afeedfcdb2f39ca9
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51037881"
---
# <a name="change-the-reserved-unit-type"></a>Ayrılmış birim türünü değiştirme
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encoding-units.md)
> * [Portal](media-services-portal-scale-media-processing.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a>Genel Bakış

Media Services hesabı bir Ayrılmış Birim Türüyle ilişkilendirilir ve bu da medya işleme görevlerinizin ne hızda işleneceğini belirler. Şu ayrılmış birim türlerinden birini seçebilirsiniz: **S1**, **S2** veya **S3**. Örneğin, aynı kodlama işi **S2** ayrılmış birim türünü kullandığınızda **S1** türüne göre daha hızlı çalışır.

Ayrılmış birim türünü belirtmenin yanı sıra, hesabınızın **Ayrılmış Birimler** (RU) ile sağlanmasını da belirtebilirsiniz. Sağlanan RU sayısı, verili bir hesapta eşzamanlı olarak işlenebilecek medya görevlerinin sayısını belirler.

>[!NOTE]
>RU, tüm medya işlemesini paralel hale getirmek için çalışır ve Azure Media Indexer’ın kullanıldığı dizin oluşturma işleri de buna dahildir. Bununla birlikte kodlamadan farklı olarak, dizin oluşturma işleri daha hızlı ayrılmış birimlerde daha hızlı işlenmez.

> [!IMPORTANT]
> Gözden geçirdiğinizden emin olun [genel bakış](media-services-scale-media-processing-overview.md) medya konu işlemeyi ölçeklendirme hakkında daha fazla bilgi almak için konu.
> 
> 

## <a name="scale-media-processing"></a>Medya işlemeyi ölçeklendirme
Ayrılmış birim türünü ve ayrılmış birim sayısını değiştirmek için aşağıdakileri yapın:

1. [Azure portalında](https://portal.azure.com/) Azure Media Services hesabınızı seçin.
2. İçinde **ayarları** penceresinde **medya ayrılmış birimleri**.
   
    Seçilen ayrılmış birim türünü ayrılmış birim sayısını değiştirmek için kullanın **medya hizmet birimi** kaydırıcı ekranın üstünde.
   
    Değiştirilecek **AYRILMIŞ birim TÜRÜNÜ**, tıklayarak **ayrılmış işleme birimlerinin hızı** çubuğu. Ardından istediğiniz fiyatlandırma katmanını seçin: S1, S2 veya S3.
   
3. Yaptığınız değişiklikleri kaydetmek için KAYDET düğmesine basın.
   
    KAYDET'i seçtiğinizde yeni bir ayrılmış birim ayrılır.

## <a name="next-steps"></a>Sonraki adımlar
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

