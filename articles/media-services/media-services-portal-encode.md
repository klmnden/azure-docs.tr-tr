---
title: "Azure portalında Medya Kodlayıcısı standart'ı kullanarak bir varlık kodlama | Microsoft Docs"
description: "Bu öğreticide, Azure portalında Medya Kodlayıcısı standart'ı kullanarak bir varlık kodlama adımları açıklanmaktadır."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 107d9e9a-71e9-43e5-b17c-6e00983aceab
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: ae5f4fd391cbf62b41d1a65f1d8107cefe3a5df3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="encode-an-asset-by-using-media-encoder-standard-in-the-azure-portal"></a>Azure portalında Medya Kodlayıcısı standart'ı kullanarak bir varlık kodlama

> [!NOTE]
> Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

Azure Media Services ile çalışırken en sık karşılaşılan senaryolardan biri, bit hızı Uyarlamalı istemcilerinize akış iletmektir. Media Services teknolojileri aşağıdaki bit hızı Uyarlamalı akış destekler: Apple HTTP canlı akışı (HLS), Microsoft kesintisiz akış ve dinamik Uyarlamalı akış HTTP (DASH, MPEG-DASH olarak da adlandırılır) üzerinden. Videolarınızı bit hızı Uyarlamalı akış için hazırlamak için önce kaynak videonuzu Çoklu bit hızlı dosyaları olarak kodlayın. Videolarınızı kodlamak için Azure Medya Kodlayıcısı standart kullanabilirsiniz.  

Media Services dinamik paketleme olanağı sağlar. Dinamik paketleme ile bu akış biçimlerine yeniden paketleme olmadan, Çoklu bit hızlı MP4s HLS, kesintisiz akış ve MPEG-DASH, sunabilir. Dinamik paketleme kullandığınızda, depolamak ve tek bir depolama biçiminde dosyaları için ödeme yaparsınız. Media Services oluşturur ve ağdaki bir istemcinin isteğini göre uygun yanıtı işlevi görür.

Dinamik paketlemeden yararlanmak için kaynak dosyanızı çoklu bit hızına sahip bir dizi MP4 dosyası olarak kodlamanız gerekir. Kodlama adımları bu makalenin sonraki bölümlerinde gösterilmektedir.

Medyayı işleme ölçeğini öğrenmek için bkz: [ölçeklendirme Azure portalını kullanarak işleme medya](media-services-portal-scale-media-processing.md).

## <a name="encode-in-the-azure-portal"></a>Azure portalında kodlama

İçeriğinizi Medya Kodlayıcısı standart kullanarak kodlamak için:

1. [Azure portalında](https://portal.azure.com/) Azure Media Services hesabınızı seçin.
2. **Ayarlar** > **Varlıklar**’ı seçin. Kodlamak istediğiniz varlığı seçin.
3. **Kodla** düğmesini seçin.
4. **Bir varlık kodla** bölmesinde, **Media Encoder Standard** işlemcisini ve bir ön ayarı seçin. Hazır ayarlar hakkında daha fazla bilgi için bkz. [Bit hızı merdivenini otomatik oluşturma](media-services-autogen-bitrate-ladder-with-mes.md) ve [Media Encoder Standard için görev ön ayarları](media-services-mes-presets-overview.md). Girdi videonuz için en iyi sonucu verecek hazır ayarı seçmeniz önemlidir. Örneğin, girdi videonuzun 1920 &#215; 1080 piksel çözünürlüğü olduğunu biliyorsanız, **H264 Çoklu Bit hızı 1080p** ön ayarını kullanabilirsiniz. Düşük çözünürlüklü bir videonuz varsa (640 &#215; 360), **H264 Çoklu Bit Hızı 1080p** ön ayarını kullanmamalısınız.
   
   Kaynaklarınızın yönetmenize yardımcı olmak için çıktı varlığının ve işin adını düzenleyebilirsiniz.
   
   ![Varlıkları kodlama](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. **Oluştur**’u seçin.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [Kodlama işinin ilerleme durumunu izlemek](media-services-portal-check-job-progress.md) Azure portalında.  

