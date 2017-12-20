---
title: "Azure portalını kullanarak ölçek medyayı işleme | Microsoft Docs"
description: "Bu öğretici, Azure portalını kullanarak işleme ölçeklendirme medya adımlarda size yol gösterir."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: e500f733-68aa-450c-b212-cf717c0d15da
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.openlocfilehash: d2312803a4471e207d3696ca8350a86e3c4761e6
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
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

> [!NOTE]
> Java SDK'ın en son sürümünü alın ve Java ile geliştirmeye başlamak için bkz: [Media Services için Java istemcisi SDK ile çalışmaya başlama](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use). <br/>
> Media Services için en yeni PHP SDK'yi karşıdan yüklemek için sürümünde 0.5.7 Microsoft/WindowAzure paket Ara [Packagist depo](https://packagist.org/packages/microsoft/windowsazure#v0.5.7).  

## <a name="overview"></a>Genel Bakış

Media Services hesabı bir Ayrılmış Birim Türüyle ilişkilendirilir ve bu da medya işleme görevlerinizin ne hızda işleneceğini belirler. Şu ayrılmış birim türlerinden birini seçebilirsiniz: **S1**, **S2** veya **S3**. Örneğin, aynı kodlama işi **S2** ayrılmış birim türünü kullandığınızda **S1** türüne göre daha hızlı çalışır.

Ayrılmış birim türünü belirtmenin yanı sıra, hesabınızın **Ayrılmış Birimler** (RU) ile sağlanmasını da belirtebilirsiniz. Sağlanan RU sayısı, verili bir hesapta eşzamanlı olarak işlenebilecek medya görevlerinin sayısını belirler.

>[!NOTE]
>RU, tüm medya işlemesini paralel hale getirmek için çalışır ve Azure Media Indexer’ın kullanıldığı dizin oluşturma işleri de buna dahildir. Bununla birlikte kodlamadan farklı olarak, dizin oluşturma işleri daha hızlı ayrılmış birimlerde daha hızlı işlenmez.

> [!IMPORTANT]
> Gözden geçirdiğinizden emin olun [genel bakış](media-services-scale-media-processing-overview.md) konu işleme medya ölçeklendirme hakkında daha fazla bilgi için konu.
> 
> 

## <a name="scale-media-processing"></a>Medya işlemeyi ölçeklendirme
Ayrılmış birim türünü ve ayrılan birim sayısını değiştirmek için aşağıdakileri yapın:

1. [Azure portalında](https://portal.azure.com/) Azure Media Services hesabınızı seçin.
2. İçinde **ayarları** penceresinde, seçin **medya ayrılmış birimleri**.
   
    Seçilen ayrılmış birim türü için ayrılan birim sayısını değiştirmek için kullanmak **medya sunulan birimleri** kaydırıcı.
   
    Değiştirmek için **AYRILMIŞ birim türü**, S1, S2 ve S3 tuşuna basın.
   
    ![İşlemci sayfası](./media/media-services-portal-scale-media-processing/media-services-scale-media-processing.png)
3. Yaptığınız değişiklikleri kaydetmek için KAYDET düğmesine basın.
   
    Yeni bir ayrılmış birimler Kaydet bastığınızda ayrılır.

## <a name="next-steps"></a>Sonraki adımlar
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

