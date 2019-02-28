---
title: Medya Hizmetleri - Azure ile bulutta kodlama | Microsoft Docs
description: Bu konuda Azure Media Services'ı kullanarak kodlama işlemi açıklanmaktadır
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/25/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: ccf298c99851dc2418da894431c5c86adafe59b3
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56959451"
---
# <a name="encoding-with-media-services"></a>Media Services ile kodlama

Azure Media Services, içeriğinizi çok çeşitli tarayıcılar ve cihazlar üzerinde çalınabilir şekilde yüksek kaliteli dijital medya dosyalarınızın Uyarlamalı bit hızı MP4 dosyaları olarak kodlayın sağlar. Başarılı bir Media Services kodlama işinin bir çıktı oluşturur varlık kümesiyle Uyarlamalı bit hızlı MP4'ler göndermenizi ve bildirim dosyaları (sunucu ve istemci). Bu noktada, avantajlarından yararlanabilirsiniz [dinamik paketleme](dynamic-packaging-overview.md).

Video çıkış yapmak için varlık kayıttan yürütme için istemciler tarafından kullanılabilir, sahip olduğunuz oluşturmak bir **akış Bulucu** ve daha sonra akış URL'lerini oluşturabilirsiniz. Ardından, bildirimde belirtilen biçime bağlı olarak, istemcilerinizin seçmiş oldukları protokolünde akış alırsınız.

Aşağıdaki diyagram, talep üzerine akış dinamik paketleme iş akışıyla gösterir.

![Dinamik kodlama](./media/dynamic-packaging-overview/media-services-dynamic-packaging.png)

Bu konuda içeriğinizi Media Services v3 ile kodlama konusunda rehberlik sağlar.

## <a name="transforms-and-jobs"></a>Dönüşümler ve işler

Media Services v3 ile kodlanacak oluşturmak gereken bir [dönüştürme](https://docs.microsoft.com/rest/api/media/transforms) ve [iş](https://docs.microsoft.com/rest/api/media/jobs). Dönüşüm kodlama ayarları ve çıktılar için tarif tanımlar ve iş tarif örneğidir. Daha fazla bilgi için [dönüşümler ve işler](transforms-jobs-concept.md)

Media Services ile kodlarken Kodlayıcı giriş medya dosyalarını nasıl işlenmesi gerektiğini söylemek için hazır kullanın. Örneğin, kodlanmış içeriği görüntü çözünürlüğünü ve/veya istediğiniz ses kanal sayısını belirtebilirsiniz. 

Sektördeki en iyi uygulamalarına göre önerilen yerleşik hazır biri ile hızlıca başlayabilirsiniz veya senaryonuz ya da cihaz belirli gereksinimlerinizi hedeflemek için önceden belirlenmiş bir özel bir yapı seçebilirsiniz. Daha fazla bilgi için [kodla özel dönüştürme](customize-encoder-presets-how-to.md). 

Ocak 2019'ile başlayan Medya Kodlayıcısı MP4 dosyaları üretmek için standart ile kodlarken .mpi yeni bir dosya oluşturulur ve çıktıyı eklenen varlık. Bu MPI dosya performansını artırmak için kullanılmaya [dinamik paketleme](dynamic-packaging-overview.md) ve senaryoları akış.

> [!NOTE]
> Değiştirme veya MPI dosyayı kaldırmak veya gerekir (veya etkinleştirmezsiniz) varlığını hizmetinizdeki herhangi bir bağımlılık olması, böyle bir dosya.

## <a name="built-in-presets"></a>Yerleşik hazır

Media Services şu anda aşağıdaki yerleşik kodlama Önayarları destekler:  

### <a name="builtinstandardencoderpreset-preset"></a>BuiltInStandardEncoderPreset hazır

[BuiltInStandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) standart Kodlayıcı ile giriş video kodlama için önceden belirlenmiş bir yerleşik ayarlamak için kullanılır. 

Şu anda desteklenen aşağıdaki hazır:

- **EncoderNamedPreset.AdaptiveStreaming** (önerilir). Daha fazla bilgi için [hızı Merdivenini otomatik oluşturma](autogen-bitrate-ladder.md).
- **EncoderNamedPreset.AACGoodQualityAudio** -yalnızca 192 Kb/sn ile kodlanmış stereo ses içeren tek bir MP4 dosyası üretir.
- **EncoderNamedPreset.H264MultipleBitrate1080p** -400 KB/sn ve stereo AAC ses 6000 KB/sn arasında 8 GOP hizalı MP4 dosyaları kümesini oluşturur. Çözüm, 1080 p başlar ve aşağı 360 p geçer.
- **EncoderNamedPreset.H264MultipleBitrate720p** -400 KB/sn ve stereo AAC ses 3400 KB/sn arasında değişen 6 GOP hizalı MP4 dosyaları kümesini oluşturur. Çözüm, 720 p başlar ve aşağı 360 p geçer.
- **EncoderNamedPreset.H264MultipleBitrateSD** -400 KB/sn ve stereo AAC ses 1600 KB/sn arasında değişen 5 GOP hizalı MP4 dosyaları kümesini oluşturur. Çözüm, 480 p başlar ve aşağı 360 p geçer.<br/><br/>Daha fazla bilgi için [kodlama ve akış dosyalarını karşıya yükleniyor,](stream-files-tutorial-with-api.md).

### <a name="standardencoderpreset-preset"></a>StandardEncoderPreset hazır

[StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) standart Kodlayıcı ile giriş video kodlama, kullanılacak ayarları açıklanır. Bu dönüşüm hazır özelleştirirken önceden kullanın. 

#### <a name="custom-presets"></a>Özel önayarların kullanılmasına

Media Services kodlama özgü ihtiyaçları ve gereksinimleri karşılamak için hazır olarak tüm değerleri özelleştirme tam olarak destekler. Kullandığınız **StandardEncoderPreset** dönüştürme hazır özelleştirirken hazır. İçin bir ayrıntılı açıklamaları ve örnek için bkz. [Kodlayıcı önayarlarını özelleştirme](customize-encoder-presets-how-to.md).

## <a name="scaling-encoding-in-v3"></a>V3 sürümünde kodlama ölçeklendirme

Medya işlemeyi ölçeklendirme için bkz: [CLI ile ölçek](media-reserved-units-cli-how-to.md).

## <a name="next-steps"></a>Sonraki adımlar

* [HTTPS kullanarak yerleşik hazır bir URL'den kodlayın](job-input-from-http-how-to.md)
* [Yerleşik hazır kullanarak yerel bir dosya kodlama](job-input-from-local-file-how-to.md)
* [Özel bir senaryonuz ya da cihaz belirli gereksinimlerinizi hedeflemek için önceden derleme](customize-encoder-presets-how-to.md)
* [Karşıya yükleme, kodlama ve Media Services'i kullanarak akış](stream-files-tutorial-with-api.md)
