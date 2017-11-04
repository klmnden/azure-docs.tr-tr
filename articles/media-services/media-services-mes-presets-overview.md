---
title: "Görev hazır Medya Kodlayıcısı standart (MES) | Microsoft Docs"
description: "Hizmet tanımlı örnek hazır için Medya Kodlayıcısı standart (MES) genel bakış ve konu sağlar."
author: Juliako
manager: cfow
editor: johndeu
services: media-services
documentationcenter: 
ms.assetid: f243ed1c-ac9c-4300-a5f7-f092cf9853b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2017
ms.author: juliako
ms.openlocfilehash: 5753b1dffe5a1a4ee069b83f58e9c2dac433b89d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="sample-presets-for-media-encoder-standard-mes"></a>Medya Kodlayıcısı standart (MES) için örnek hazır

**Medya Kodlayıcısı standart** önceden tanımlanmış sistem kodlama işler oluştururken kullanabileceğiniz hazır kodlama kümesini tanımlar. "Uyarlamalı Media Services ile akış için video kodlamak istiyorsanız önceden akış" kullanılması önerilir. Bu önceden belirlenmiş, Medya Kodlayıcısı standart işlem belirttiğinizde [bit hızı Merdiveni otomatik oluşturma](media-services-autogen-bitrate-ladder-with-mes.md). 

### <a name="creating-custom-presets-from-samples"></a>Özel hazır örnekleri oluşturma
Media Services, belirli kodlama ihtiyaçları ve gereksinimleri karşılamak üzere hazır içindeki tüm değerleri özelleştirme tam olarak destekler. Bir kodlama hazır özelleştirmeniz gerekiyorsa, biri ile başlamalıdır Bu bölümde özel yapılandırmanız için bir şablon olarak sağlanan sistem önayarlarını aşağıda. Her öğe için hangi her bir öğe bu hazır anlamına gelir ve geçerli değerleri açıklamalar için bkz [Medya Kodlayıcısı standart şema](media-services-mes-schema.md) konu.  
  
> [!NOTE]
>  4 k kodlar için bir hazır kullanırken, alması gereken `S3` ayrılmış birim türü. Daha fazla bilgi için bkz: [ölçek kodlama nasıl](https://azure.microsoft.com/documentation/articles/media-services-portal-encoding-units).  

#### <a name="video-rotation-default-setting-in-presets"></a>Hazır video döndürme varsayılan ayarı:
Medya Kodlayıcısı standart ile çalışırken, video döndürme varsayılan olarak etkindir. Videonuzu dikey modunda bir mobil cihazda kayıtlı değilse, ardından bu hazır bunları kodlamadan önce yatay modu için döndürüleceğini.
 
## <a name="available-presets"></a>Kullanılabilir hazır ayarlar: 

 [H264 Çoklu bit hızı 1080p ses 5.1](media-services-mes-preset-H264-Multiple-Bitrate-1080p-Audio-5.1.md) 6000 kbps ila 400 kbps ve AAC 5.1 ses arasında değişen 8 GOP hizalı MP4 dosyaları kümesi üretir.  
  
 [H264 Çoklu bit hızı 1080p](media-services-mes-preset-H264-Multiple-Bitrate-1080p.md) 6000 kbps ila 400 kbps ve stereo AAC ses arasında değişen 8 GOP hizalı MP4 dosyaları kümesi üretir.  
  
 [H264 Çoklu bit hızı 16 x 9 iOS için](media-services-mes-preset-H264-Multiple-Bitrate-16x9-for-iOS.md) 8500 kbps ila 200 KB/sn ile stereo AAC ses arasında değişen 8 GOP hizalı MP4 dosyaları kümesi üretir.  
  
 [H264 Çoklu bit hızı 16 x 9 SD ses 5.1](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD-Audio-5.1.md) 1900 kbps ila 400 kbps ve AAC 5.1 ses arasında değişen 5 GOP hizalı MP4 dosyaları kümesi üretir.  
  
 [H264 Çoklu bit hızı 16 x 9 SD](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD.md) 1900 kbps ila 400 kbps ve stereo AAC ses arasında değişen 5 GOP hizalı MP4 dosyaları kümesi üretir.  
  
 [H264 Çoklu bit hızı 4K ses 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4K-Audio-5.1.md) 20000 kbps ila 1000 KB/sn ve AAC 5.1 ses arasında değişen 12 GOP hizalı MP4 dosyaları kümesi üretir.  
  
 [H264 Çoklu bit hızı 4K](media-services-mes-preset-H264-Multiple-Bitrate-4K.md) 20000 kbps ila 1000 KB/sn ile stereo AAC ses arasında değişen 12 GOP hizalı MP4 dosyaları kümesi üretir.  
  
 [H264 Çoklu bit hızı 4 x 3 iOS için](media-services-mes-preset-H264-Multiple-Bitrate-4x3-for-iOS.md) 8500 kbps ila 200 KB/sn ile stereo AAC ses arasında değişen 8 GOP hizalı MP4 dosyaları kümesi üretir.  
  
 [H264 Çoklu bit hızı 4 x 3 SD ses 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD-Audio-5.1.md) 1600 kbps ila 400 kbps ve AAC 5.1 ses arasında değişen 5 GOP hizalı MP4 dosyaları kümesi üretir.  
  
 [H264 Çoklu bit hızı 4 x 3 SD](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD.md) 1600 kbps ila 400 kbps ve stereo AAC ses arasında değişen 5 GOP hizalı MP4 dosyaları kümesi üretir.  
  
 [H264 Çoklu bit hızı 720p ses 5.1](media-services-mes-preset-H264-Multiple-Bitrate-720p-Audio-5.1.md) 3400 kbps ila 400 kbps ve AAC 5.1 ses arasında değişen 6 GOP hizalı MP4 dosyaları kümesi üretir.  
  
 [H264 Çoklu bit hızı 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) 3400 kbps ila 400 kbps ve stereo AAC ses arasında değişen 6 GOP hizalı MP4 dosyaları kümesi üretir.  
  
 [H264 Çoklu bit hızı 1080p ses 5.1](media-services-mes-preset-H264-Single-Bitrate-1080p-Audio-5.1.md) 6750 kbps ve AAC 5.1 ses bit hızı ile tek bir MP4 dosyası oluşturur.  
  
 [H264 Çoklu bit hızı 1080p](media-services-mes-preset-H264-Single-Bitrate-1080p.md) 6750 kbps stereo AAC ses ve bit hızı ile tek bir MP4 dosyası oluşturur.  
  
 [H264 Çoklu bit hızı 4K ses 5.1](media-services-mes-preset-H264-Single-Bitrate-4K-Audio-5.1.md) 18000 kbps ve AAC 5.1 ses bit hızı ile tek bir MP4 dosyası oluşturur.  
  
 [H264 Çoklu bit hızı 4K](media-services-mes-preset-H264-Single-Bitrate-4K.md) 18000 kbps stereo AAC ses ve bit hızı ile tek bir MP4 dosyası oluşturur.  
  
 [H264 Çoklu bit hızı 4 x 3 SD ses 5.1](media-services-mes-preset-H264-Single-Bitrate-4x3-SD-Audio-5.1.md) 1800 kbps ve AAC 5.1 ses bit hızı ile tek bir MP4 dosyası oluşturur.  
  
 [H264 Çoklu bit hızı 4 x 3 SD](media-services-mes-preset-H264-Single-Bitrate-4x3-SD.md) 1800 kbps stereo AAC ses ve bit hızı ile tek bir MP4 dosyası oluşturur.  
  
 [H264 Çoklu bit hızı 16 x 9 SD ses 5.1](media-services-mes-preset-H264-Single-Bitrate-16x9-SD-Audio-5.1.md) 2200 kbps ve AAC 5.1 ses bit hızı ile tek bir MP4 dosyası oluşturur.  
  
 [H264 Çoklu bit hızı 16 x 9 SD](media-services-mes-preset-H264-Single-Bitrate-16x9-SD.md) 2200 kbps stereo AAC ses ve bit hızı ile tek bir MP4 dosyası oluşturur.  
  
 [H264 Çoklu bit hızı 720p ses 5.1](media-services-mes-preset-H264-Single-Bitrate-720p-Audio-5.1.md) 4500 kbps ve AAC 5.1 ses bit hızı ile tek bir MP4 dosyası oluşturur.  
  
 [H264 Çoklu bit hızı Android 720p](media-services-mes-preset-H264-Single-Bitrate-720p-for-Android.md) hazır bir bit hızı 2000 kbps ve stereo AAC ile tek bir MP4 dosyası oluşturur.  
  
 [H264 Çoklu bit hızı 720p](media-services-mes-preset-H264-Single-Bitrate-720p.md) 4500 kbps stereo AAC ses ve bit hızı ile tek bir MP4 dosyası oluşturur.  
  
 [H264 Çoklu bit hızı yüksek kaliteli SD Android için](media-services-mes-preset-H264-Single-Bitrate-High-Quality-SD-for-Android.md) 500 Kb/sn ve stereo AAC ses bit hızı ile tek bir MP4 dosyası üreten...  
  
 [H264 Çoklu bit hızı düşük kalite SD Android için](media-services-mes-preset-H264-Single-Bitrate-Low-Quality-SD-for-Android.md) 56 kbps stereo AAC ses ve bit hızı ile tek bir MP4 dosyası oluşturur.  
  
 Media Services kodlayıcıya ilgili daha fazla bilgi için bkz: [kodlama isteğe bağlı Azure Media Services ile](https://azure.microsoft.com/en-us/documentation/articles/media-services-encode-asset/).
