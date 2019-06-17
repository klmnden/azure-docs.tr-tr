---
title: Azure portalında Media Encoder Standard kullanarak bir varlığı kodlama | Microsoft Docs
description: Bu öğreticide Azure portalında Media Encoder Standard kullanarak varlık kodlama adımlarında size kılavuzluk eder.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 107d9e9a-71e9-43e5-b17c-6e00983aceab
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 90190f426419e65bd580b9004ae76a2c6b0c12e2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61463160"
---
# <a name="encode-an-asset-by-using-media-encoder-standard-in-the-azure-portal"></a>Azure portalında Media Encoder Standard kullanarak bir varlığı kodlama

> [!NOTE]
> Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

Azure Media Services ile çalışırken en sık karşılaşılan senaryolardan biri bit hızı Uyarlamalı akış istemcilerinize sağlıyor. Media Services şu hızı Uyarlamalı akış teknolojilerini destekler: Apple HTTP canlı akış (HLS), Microsoft kesintisiz akış ve Dynamic Adaptive Streaming http (DASH, ayrıca çağrılan MPEG-DASH). Videolarınızı Uyarlamalı bit hızı akışına hazırlamak için önce kaynak videonuzu Çoklu bit hızı dosyaları kodlayın. Videolarınızı kodlamak için Azure Medya Kodlayıcısı standart kullanabilirsiniz.  

Media Services dinamik paketleme olanağı sağlar. Dinamik paketleme ile bu akış biçimlerine yeniden paketleme olmadan, Çoklu bit hızına sahip MP4 HLS, kesintisiz akış ve MPEG-DASH teslim edebilirsiniz. Dinamik paketleme kullandığınızda, depolayın ve tek bir depolama biçimindeki dosyaları için ödeme yaparsınız. Media Services istemci isteğe göre uygun yanıtı derler ve sunar.

Dinamik paketlemeden yararlanmak için kaynak dosyanızı çoklu bit hızına sahip bir dizi MP4 dosyası olarak kodlamanız gerekir. Kodlama adımları bu makalenin ilerleyen bölümlerinde gösterilmektedir.

Medya işlemeyi ölçeklendirme hakkında bilgi edinmek için [medya işleme Azure portalını kullanarak ölçeklendirme](media-services-portal-scale-media-processing.md).

## <a name="encode-in-the-azure-portal"></a>Azure portalında kodlayın

İçeriğinizi Media Encoder Standard kullanarak kodlamak için:

1. [Azure portalında](https://portal.azure.com/) Azure Media Services hesabınızı seçin.
2. **Ayarlar** > **Varlıklar**’ı seçin. Kodlamak istediğiniz varlığı seçin.
3. **Kodla** düğmesini seçin.
4. **Bir varlık kodla** bölmesinde, **Media Encoder Standard** işlemcisini ve bir ön ayarı seçin. Hazır ayarlar hakkında daha fazla bilgi için bkz. [Bit hızı merdivenini otomatik oluşturma](media-services-autogen-bitrate-ladder-with-mes.md) ve [Media Encoder Standard için görev ön ayarları](media-services-mes-presets-overview.md). Girdi videonuz için en iyi sonucu verecek hazır ayarı seçmeniz önemlidir. Örneğin, girdi videonuzun 1920 &#215; 1080 piksel çözünürlüğü olduğunu biliyorsanız, **H264 Çoklu Bit hızı 1080p** ön ayarını kullanabilirsiniz. Düşük çözünürlüklü bir videonuz varsa (640 &#215; 360), **H264 Çoklu Bit Hızı 1080p** ön ayarını kullanmamalısınız.
   
   Kaynaklarınızın yönetmenize yardımcı olmak için çıktı varlığının ve işin adını düzenleyebilirsiniz.
   
   ![Varlıkları kodlama](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. **Oluştur**’u seçin.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [Kodlama işinin ilerleme durumunu izlemek](media-services-portal-check-job-progress.md) Azure portalında.  

