---
title: Akış uç noktaları Azure portalı ile ölçeklendirme | Microsoft Docs
description: Bu öğreticide Azure portal ile akış uç noktalarını ölçeklendirme adımlarında size kılavuzluk eder.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 1008b3a3-2fa1-4146-85bd-2cf43cd1e00e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 23eb51428dcf4961febfb592bf957bb8beeeda57
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61463126"
---
# <a name="scale-streaming-endpoints-with-the-azure-portal"></a>Azure portal ile akış uç noktalarını ölçeklendirme
## <a name="overview"></a>Genel Bakış

> [!NOTE]
> Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

**Premium** akış uç noktaları, adanmış ve ölçeklenebilir bant genişliği kapasitesi sağlar; dolayısıyla gelişmiş iş yükleri için uygundur. **Premium** akış uç noktası olan müşteriler, varsayılan olarak bir akış birimi (SU) alır. Akış uç noktası, SU’lar eklenerek ölçeklendirilebilir. Her SU, uygulamaya ek bant genişliği kapasitesi sağlar. Akış uç noktası türlerini ve CDN yapılandırma hakkında daha fazla bilgi için bkz. [akış uç noktası genel bakış](media-services-streaming-endpoints-overview.md) konu.
 
Bu konuda, ölçeklendirme bir akış uç noktası gösterilmektedir.

Fiyatlandırma ayrıntıları hakkında daha fazla bilgi için bkz. [Media Services Fiyatlandırma Ayrıntıları](https://go.microsoft.com/fwlink/?LinkId=275107).

## <a name="scale-streaming-endpoints"></a>Akış uç noktalarını ölçeklendirme

Akış birim sayısını değiştirmek için aşağıdakileri yapın:

1. [Azure portalında](https://portal.azure.com/) Azure Media Services hesabınızı seçin.
2. İçinde **ayarları** penceresinde **akış uç noktaları**.
3. Ölçeklendirmek istediğiniz akış uç noktasına tıklayın. 

    > [!NOTE] 
    > Yalnızca ölçeklendirebilirsiniz **Premium** akış uç noktaları.

4. Akış birim sayısını belirtmek üzere kaydırıcıyı taşıyın.

    ![Akış uç noktası](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

## <a name="next-steps"></a>Sonraki adımlar
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

