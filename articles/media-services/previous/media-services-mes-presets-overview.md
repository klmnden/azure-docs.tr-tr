---
title: Görev ön ayarları için medya Kodlayıcı standart (MES) | Microsoft Docs
description: Hizmet tarafından tanımlanan örnek hazır için medya Kodlayıcı standart (MES) genel bakış ve konu sağlar.
author: Juliako
manager: femila
editor: johndeu
services: media-services
documentationcenter: ''
ms.assetid: f243ed1c-ac9c-4300-a5f7-f092cf9853b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 50c52369a5a957a4dd6279cac5079e2dea023106
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61463412"
---
# <a name="sample-presets-for-media-encoder-standard-mes"></a>Medya Kodlayıcısı standart (MES) için örnek hazır

**Media Encoder Standard** önceden tanımlanmış sistem önayarlarını kodlama işi oluştururken kullanabileceğiniz kodlama kümesi tanımlar. "Uyarlamalı kodlama için Media Services ile akış video istiyorsanız hazır akış" kullanmak için önerilir. Bu önceden oluşturulmuş, Media Encoder Standard olacak belirttiğinizde [hızı Merdivenini otomatik oluşturma](media-services-autogen-bitrate-ladder-with-mes.md). 

### <a name="creating-custom-presets-from-samples"></a>Özel önayarların kullanılmasına örnekleri oluşturma
Media Services kodlama özgü ihtiyaçları ve gereksinimleri karşılamak için hazır olarak tüm değerleri özelleştirme tam olarak destekler. Bir kodlama Önayarı özelleştirmeniz gerekirse, biri ile başlamalıdır, bu bölümde özel yapılandırmanız için şablon olarak sağlanan sistem önayarlarını aşağıda. Bu hazır anlamına gelir ve geçerli değerler her hangi bir öğenin her öğe için açıklamalar için bkz [Media Encoder Standard şeması](media-services-mes-schema.md) konu.  
  
> [!NOTE]
>  4 k kodlar için önceden ayarlanmış kullanırken almalısınız `S3` ayrılmış birim türü. Daha fazla bilgi için [ölçek kodlama nasıl](https://azure.microsoft.com/documentation/articles/media-services-portal-encoding-units).  

#### <a name="video-rotation-default-setting-in-presets"></a>Ekran döndürme varsayılan ayarı hazır:
Media Encoder Standard ile çalışırken, ekran döndürme, varsayılan olarak etkindir. Bir mobil cihazda dikey modda videonuz kaydedilmiş, ardından bu hazır bunları kodlama önce yatay modda döndürüleceğini.
 
## <a name="available-presets"></a>Kullanıma hazır: 

 [H264 Çoklu bit hızı 1080p ses 5.1](media-services-mes-preset-H264-Multiple-Bitrate-1080p-Audio-5.1.md) 400 KB/sn ve AAC 5.1 ses 6000 KB/sn arasında 8 GOP hizalı MP4 dosyaları kümesini oluşturur.  
  
 [H264 Çoklu bit hızı 1080p](media-services-mes-preset-H264-Multiple-Bitrate-1080p.md) 400 KB/sn ve stereo AAC ses 6000 KB/sn arasında 8 GOP hizalı MP4 dosyaları kümesini oluşturur.  
  
 [H264 Çoklu bit hızı 16 x 9 iOS için](media-services-mes-preset-H264-Multiple-Bitrate-16x9-for-iOS.md) 8500 kbps ila 200 KB/sn ve stereo AAC ses arasında değişen 8 GOP hizalı MP4 dosyaları kümesini oluşturur.  
  
 [H264 Çoklu bit hızı 16 x 9 SD ses 5.1](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD-Audio-5.1.md) 400 KB/sn ve AAC 5.1 ses 1900 KB/sn arasında değişen 5 GOP hizalı MP4 dosyaları kümesini oluşturur.  
  
 [H264 Çoklu bit hızı 16 x 9 SD](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD.md) 400 KB/sn ve stereo AAC ses 1900 KB/sn arasında değişen 5 GOP hizalı MP4 dosyaları kümesini oluşturur.  
  
 [H264 Çoklu bit hızı 4K ses 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4K-Audio-5.1.md) 20000 kbps ila 1000 KB/sn ve AAC 5.1 ses arasında değişen 12 GOP hizalı MP4 dosyaları kümesini oluşturur.  
  
 [H264 Çoklu bit hızı 4K](media-services-mes-preset-H264-Multiple-Bitrate-4K.md) 20000 kbps ila 1000 KB/sn ve stereo AAC ses arasında değişen 12 GOP hizalı MP4 dosyaları kümesini oluşturur.  
  
 [H264 Çoklu bit hızı 4 x 3 iOS için](media-services-mes-preset-H264-Multiple-Bitrate-4x3-for-iOS.md) 8500 kbps ila 200 KB/sn ve stereo AAC ses arasında değişen 8 GOP hizalı MP4 dosyaları kümesini oluşturur.  
  
 [H264 Çoklu bit hızı 4 x 3 SD ses 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD-Audio-5.1.md) 400 KB/sn ve AAC 5.1 ses 1600 KB/sn arasında değişen 5 GOP hizalı MP4 dosyaları kümesini oluşturur.  
  
 [H264 Çoklu bit hızı 4 x 3 SD](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD.md) 400 KB/sn ve stereo AAC ses 1600 KB/sn arasında değişen 5 GOP hizalı MP4 dosyaları kümesini oluşturur.  
  
 [H264 Çoklu bit hızı 720p ses 5.1](media-services-mes-preset-H264-Multiple-Bitrate-720p-Audio-5.1.md) 400 KB/sn ve AAC 5.1 ses 3400 KB/sn arasında değişen 6 GOP hizalı MP4 dosyaları kümesini oluşturur.  
  
 [H264 Çoklu bit hızı 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) 400 KB/sn ve stereo AAC ses 3400 KB/sn arasında değişen 6 GOP hizalı MP4 dosyaları kümesini oluşturur.  
  
 [H264 tekli bit hızı 1080p ses 5.1](media-services-mes-preset-H264-Single-Bitrate-1080p-Audio-5.1.md) 6750 KB/sn ve AAC 5.1 ses bir bit hızı ile tek bir MP4 dosyası oluşturur.  
  
 [H264 tekli bit hızı 1080p](media-services-mes-preset-H264-Single-Bitrate-1080p.md) 6750 kbps stereo AAC ses ve bir bit hızı ile tek bir MP4 dosyası oluşturur.  
  
 [H264 tekli bit hızı 4K ses 5.1](media-services-mes-preset-H264-Single-Bitrate-4K-Audio-5.1.md) 18000 KB/sn ve AAC 5.1 ses bir bit hızı ile tek bir MP4 dosyası oluşturur.  
  
 [H264 tekli bit hızı 4K](media-services-mes-preset-H264-Single-Bitrate-4K.md) 18000 kbps stereo AAC ses ve bir bit hızı ile tek bir MP4 dosyası oluşturur.  
  
 [H264 tekli bit hızı 4 x 3 SD ses 5.1](media-services-mes-preset-H264-Single-Bitrate-4x3-SD-Audio-5.1.md) 1800 KB/sn ve AAC 5.1 ses bir bit hızı ile tek bir MP4 dosyası oluşturur.  
  
 [H264 tekli bit hızı 4 x 3 SD](media-services-mes-preset-H264-Single-Bitrate-4x3-SD.md) 1800 kbps stereo AAC ses ve bir bit hızı ile tek bir MP4 dosyası oluşturur.  
  
 [H264 tekli bit hızı 16 x 9 SD ses 5.1](media-services-mes-preset-H264-Single-Bitrate-16x9-SD-Audio-5.1.md) 2200 KB/sn ve AAC 5.1 ses bir bit hızı ile tek bir MP4 dosyası oluşturur.  
  
 [H264 tekli bit hızı 16 x 9 SD](media-services-mes-preset-H264-Single-Bitrate-16x9-SD.md) 2200 kbps stereo AAC ses ve bir bit hızı ile tek bir MP4 dosyası oluşturur.  
  
 [H264 tekli bit hızı 720p ses 5.1](media-services-mes-preset-H264-Single-Bitrate-720p-Audio-5.1.md) 4500 KB/sn ve AAC 5.1 ses bir bit hızı ile tek bir MP4 dosyası oluşturur.  
  
 [H264 tekli bit hızı 720p Android](media-services-mes-preset-H264-Single-Bitrate-720p-for-Android.md) önceden ayarlanmış bir bit hızı 2000 KB/sn ve stereo AAC ile tek bir MP4 dosyası üretir.  
  
 [H264 tekli bit hızı 720p](media-services-mes-preset-H264-Single-Bitrate-720p.md) 4500 kbps stereo AAC ses ve bir bit hızı ile tek bir MP4 dosyası oluşturur.  
  
 [H264 Çoklu bit hızı yüksek kaliteli SD Android](media-services-mes-preset-H264-Single-Bitrate-High-Quality-SD-for-Android.md) 500 Kb/sn ve stereo AAC ses bir bit hızı ile tek bir MP4 dosyası üretir...  
  
 [H264 Çoklu bit hızı düşük kaliteli SD Android](media-services-mes-preset-H264-Single-Bitrate-Low-Quality-SD-for-Android.md) 56 Kb/sn ve stereo AAC ses bir bit hızı ile tek bir MP4 dosyası oluşturur.  
  
 Media Services kodlayıcıya ilgili daha fazla bilgi için bkz. [kodlama isteğe bağlı Azure Media Services ile](https://azure.microsoft.com/documentation/articles/media-services-encode-asset/).
