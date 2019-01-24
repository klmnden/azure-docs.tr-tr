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
ms.date: 01/22/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: d236f00e70e08c7bce2a94c5bd4fb64f1fa99bbc
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54826607"
---
# <a name="encoding-with-media-services"></a>Media Services ile kodlama

Azure Media Services, çok çeşitli tarayıcılar ve cihazlar üzerinde yürütülen biçimleri dijital yüksek kaliteli medya dosyalarınızın kodlayın sağlar. Örneğin, içeriğinizi Apple'ın HLS veya MPEG DASH biçimlerinde akışla göndermek isteyebilirsiniz. Bu konuda içeriğinizi Media Services v3 ile kodlama konusunda rehberlik sağlar.

Media Services v3 ile kodlanacak, Dönüşüm ve bir iş oluşturmanız gerekir. Dönüşüm kodlama ayarları ve çıktılar için tarif tanımlar ve iş tarif örneğidir. Daha fazla bilgi için [dönüşümler ve işler](transform-concept.md)

Media Services ile kodlarken Kodlayıcı giriş medya dosyalarını nasıl işlenmesi gerektiğini söylemek için hazır kullanın. Örneğin, kodlanmış içeriği görüntü çözünürlüğünü ve/veya istediğiniz ses kanal sayısını belirtebilirsiniz. 

Sektördeki en iyi uygulamalarına göre önerilen yerleşik hazır biri ile hızlıca başlayabilirsiniz veya senaryonuz ya da cihaz belirli gereksinimlerinizi hedeflemek için önceden belirlenmiş bir özel bir yapı seçebilirsiniz. Daha fazla bilgi için [kodla özel dönüştürme](customize-encoder-presets-how-to.md). 

Ocak 2019'ile başlayan Medya Kodlayıcısı MP4 dosyaları üretmek için standart ile kodlarken .mpi yeni bir dosya oluşturulur ve çıktıyı eklenen varlık. Bu MPI dosya dinamik paketleme ve akış senaryoları için performansı artırmak için tasarlanmıştır.

> [!NOTE]
> Değiştirme veya MPI dosyayı kaldırmak veya gerekir (veya etkinleştirmezsiniz) varlığını hizmetinizdeki herhangi bir bağımlılık olması, böyle bir dosya.

## <a name="built-in-presets"></a>Yerleşik hazır

Media Services şu anda aşağıdaki yerleşik kodlama Önayarları destekler:  

|**Önceden tanımlı ayar adı**|**Senaryo**|**Ayrıntılar**|
|---|---|---|
|**BuiltInStandardEncoderPreset**|Akış|Standart Kodlayıcı ile giriş video kodlama için önceden belirlenmiş bir yerleşik ayarlamak için kullanılır. <br/>Şu anda desteklenen aşağıdaki hazır:<br/>**EncoderNamedPreset.AdaptiveStreaming** (önerilir). Daha fazla bilgi için [hızı Merdivenini otomatik oluşturma](autogen-bitrate-ladder.md).<br/>**EncoderNamedPreset.AACGoodQualityAudio** -yalnızca 192 Kb/sn ile kodlanmış stereo ses içeren tek bir MP4 dosyası üretir.<br/>**EncoderNamedPreset.H264MultipleBitrate1080p** -400 KB/sn ve stereo AAC ses 6000 KB/sn arasında 8 GOP hizalı MP4 dosyaları kümesini oluşturur. Çözüm, 1080 p başlar ve aşağı 360 p geçer.<br/>**EncoderNamedPreset.H264MultipleBitrate720p** -400 KB/sn ve stereo AAC ses 3400 KB/sn arasında değişen 6 GOP hizalı MP4 dosyaları kümesini oluşturur. Çözüm, 720 p başlar ve aşağı 360 p geçer.<br/>**EncoderNamedPreset.H264MultipleBitrateSD** -400 KB/sn ve stereo AAC ses 1600 KB/sn arasında değişen 5 GOP hizalı MP4 dosyaları kümesini oluşturur. Çözüm, 480 p başlar ve aşağı 360 p geçer.<br/><br/>Daha fazla bilgi için [kodlama ve akış dosyalarını karşıya yükleniyor,](stream-files-tutorial-with-api.md).|
|**StandardEncoderPreset**|Akış|Standart Kodlayıcı ile giriş video kodlama, kullanılacak ayarları açıklanır. <br/>Bu dönüşüm hazır özelleştirirken önceden kullanın. Daha fazla bilgi için [nasıl dönüşüm önayarlarını özelleştirme](customize-encoder-presets-how-to.md).|

## <a name="custom-presets"></a>Özel önayarların kullanılmasına

Media Services kodlama özgü ihtiyaçları ve gereksinimleri karşılamak için hazır olarak tüm değerleri özelleştirme tam olarak destekler. Kullandığınız **StandardEncoderPreset** dönüştürme hazır özelleştirirken hazır. İçin bir ayrıntılı açıklamaları ve örnek için bkz. [Kodlayıcı önayarlarını özelleştirme](customize-encoder-presets-how-to.md).

## <a name="scaling-encoding-in-v3"></a>V3 sürümünde kodlama ölçeklendirme

Müşteriler şu anda, RU ayarlamak için Azure portalı veya Media Services v2 API'leri kullanmak zorunda (açıklandığı [medya işlemeyi ölçeklendirme](../previous/media-services-scale-media-processing-overview.md). 

## <a name="next-steps"></a>Sonraki adımlar

### <a name="tutorials"></a>Öğreticiler

Aşağıdaki öğreticide içeriğinizi Media Services ile kodlama gösterilmektedir:

* [Karşıya yükleme, kodlama ve Media Services'i kullanarak akış](stream-files-tutorial-with-api.md)

### <a name="code-samples"></a>Kod örnekleri

Aşağıdaki kod örnekleri, Media Services ile kodlama gösteren kod içerir:

* [.NET Core](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore)
* [Azure CLI](https://github.com/Azure/azure-docs-cli-python-samples/tree/master/media-services)

### <a name="sdks"></a>SDK’lar

İçeriğinizi kodlamak için aşağıdaki desteklenen Media Services v3 Sdk'lardan birini kullanabilirsiniz.

* [Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
* [REST](https://docs.microsoft.com/rest/api/media/transforms)
* [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/mediaservices/management?view=azure-dotnet)
* [Java](https://docs.microsoft.com/java/api/overview/azure/mediaservices)
* [Python](https://docs.microsoft.com/python/api/overview/azure/media-services?view=azure-python)

