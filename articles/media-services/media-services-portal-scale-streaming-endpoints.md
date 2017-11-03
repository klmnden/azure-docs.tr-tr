---
title: "Akış uç noktaları Azure portal ile ölçek | Microsoft Docs"
description: "Bu öğreticide Azure portal ile akış uç noktalarını ölçeklendirme adımları açıklanmaktadır."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 1008b3a3-2fa1-4146-85bd-2cf43cd1e00e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2017
ms.author: juliako
ms.openlocfilehash: 156c5d0b007e33a9181d2847fc7b517c6f816494
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="scale-streaming-endpoints-with-the-azure-portal"></a>Azure portal ile akış uç noktalarını ölçeklendirme
## <a name="overview"></a>Genel Bakış

> [!NOTE]
> Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

**Premium** akış uç noktaları, adanmış ve ölçeklenebilir bant genişliği kapasitesi sağlar; dolayısıyla gelişmiş iş yükleri için uygundur. **Premium** akış uç noktası olan müşteriler, varsayılan olarak bir akış birimi (SU) alır. Akış uç noktası, SU’lar eklenerek ölçeklendirilebilir. Her SU, uygulamaya ek bant genişliği kapasitesi sağlar. Akış uç nokta türleri ve CDN yapılandırma hakkında daha fazla bilgi için bkz: [akış uç noktası genel bakış](media-services-streaming-endpoints-overview.md) konu.
 
Bu konu bir akış uç ölçeklendirme gösterir.

Fiyatlandırma ayrıntıları hakkında daha fazla bilgi için bkz. [Media Services Fiyatlandırma Ayrıntıları](http://go.microsoft.com/fwlink/?LinkId=275107).

## <a name="scale-streaming-endpoints"></a>Akış uç noktalarını ölçeklendirme

Akış birim sayısını değiştirmek için aşağıdakileri yapın:

1. [Azure portalında](https://portal.azure.com/) Azure Media Services hesabınızı seçin.
2. İçinde **ayarları** penceresinde, seçin **akış uç noktaları**.
3. Ölçeklendirmek istediğiniz akış uç noktasına tıklayın. 

    > [!NOTE] 
    > Yalnızca ölçeklendirebilirsiniz **Premium** akış uç noktaları.

4. Akış birim sayısını belirtmek üzere kaydırıcıyı taşıyın.

    ![Akış uç noktası](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

## <a name="next-steps"></a>Sonraki adımlar
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

