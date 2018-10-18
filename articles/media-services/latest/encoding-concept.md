---
title: Azure Media Services ile bulutta kodlama | Microsoft Docs
description: Bu konuda Azure Media Services'ı kullanarak kodlama işlemi açıklanmaktadır
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: 452502d5d6a0a35f642de7e14b2a7ee7fc573bfa
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49378678"
---
# <a name="encoding-with-azure-media-services"></a>Azure Media Services ile kodlama

Azure Media Services, çok çeşitli tarayıcılar ve cihazlar üzerinde yürütülen biçimleri dijital yüksek kaliteli medya dosyalarınızın kodlayın sağlar. Örneğin, içeriğinizi Apple'ın HLS veya MPEG DASH biçimlerinde akışla göndermek isteyebilirsiniz. Media Services Ayrıca, video veya ses içeriğini analiz etmenize olanak tanır. Bu konuda size rehberlik içeriğinizi Media Services v3 ile kodlama.

Media Services v3 ile kodlanacak, Dönüşüm ve bir iş oluşturmanız gerekir. Dönüşüm kodlama ayarları ve çıktılar için tarif tanımlar ve iş tarif örneğidir. Daha fazla bilgi için [dönüşümler ve işler](transform-concept.md)

Azure Media Services ile kodlarken Kodlayıcı giriş medya dosyalarını nasıl işlenmesi gerektiğini söylemek için hazır kullanın. Örneğin, kodlanmış içeriği görüntü çözünürlüğünü ve/veya istediğiniz ses kanal sayısını belirtebilirsiniz. 

Sektördeki en iyi uygulamalarına göre önerilen yerleşik hazır biri ile hızlıca başlayabilirsiniz veya senaryonuz ya da cihaz belirli gereksinimlerinizi hedeflemek için önceden belirlenmiş bir özel bir yapı seçebilirsiniz. Daha fazla bilgi için [Azure Media Services ile kodlama](encoding-concept.md). 

## <a name="built-in-presets"></a>Yerleşik hazır

Media Services şu anda aşağıdaki yerleşik kodlama Önayarları destekler:  

|**Önceden tanımlı ayar adı**|**Senaryo**|**Ayrıntılar**|
|---|---|---|
|**AudioAnalyzerPreset**|Ses analizi|Yapay ZEKA tabanlı analiz işlemleri konuşma transkripsiyonu dahil olmak üzere, önceden tanımlı bir dizi hazır geçerlidir. Şu anda hazır içerik tek bir ses kaydı ile işlenmesini destekler.<br/>Ses yükü dilini girişinde 'dil etiketi-region' BCP-47 biçimi kullanarak belirtebilirsiniz (örneğin, ' en-US'). Desteklenen dillerin listesi olduğundan, 'en-US', 'en-GB', 'es-ES', "es-MX", "fr-FR", 'it-IT', 'ja-JP', 'pt-BR', 'zh-CN'.|
|**VideoAnalyzerPreset**|Ses ve video analiz etme|Ses hem video öngörüleri (zengin meta veriler) ayıklar ve çıkaran bir JSON biçim dosyası. Yalnızca ses video dosyası işlenirken içgörü isteyip istemediğinizi belirtebilirsiniz. Daha fazla bilgi için [Çözümle video](analyze-videos-tutorial-with-api.md).|
|**BuiltInStandardEncoderPreset**|Akış|Standart Kodlayıcı ile giriş video kodlama için önceden belirlenmiş bir yerleşik ayarlamak için kullanılır. <br/>Şu anda desteklenen aşağıdaki hazır:<br/>**EncoderNamedPreset.AdaptiveStreaming** (önerilir). Daha fazla bilgi için [hızı Merdivenini otomatik oluşturma](autogen-bitrate-ladder.md).<br/>**EncoderNamedPreset.AACGoodQualityAudio** -yalnızca 192 Kb/sn ile kodlanmış stereo ses içeren tek bir MP4 dosyası üretir.<br/>**EncoderNamedPreset.H264MultipleBitrate1080p** -400 KB/sn ve stereo AAC ses 6000 KB/sn arasında 8 GOP hizalı MP4 dosyaları kümesini oluşturur. Çözüm, 1080 p başlar ve aşağı 360 p geçer.<br/>**EncoderNamedPreset.H264MultipleBitrate720p** -400 KB/sn ve stereo AAC ses 3400 KB/sn arasında değişen 6 GOP hizalı MP4 dosyaları kümesini oluşturur. Çözüm, 720 p başlar ve aşağı 360 p geçer.<br/>**EncoderNamedPreset.H264MultipleBitrateSD** -400 KB/sn ve stereo AAC ses 1600 KB/sn arasında değişen 5 GOP hizalı MP4 dosyaları kümesini oluşturur. Çözüm, 480 p başlar ve aşağı 360 p geçer.<br/><br/>Daha fazla bilgi için [kodlama ve akış dosyalarını karşıya yükleniyor,](stream-files-tutorial-with-api.md).|
|**StandardEncoderPreset**|Akış|Standart Kodlayıcı ile giriş video kodlama, kullanılacak ayarları açıklanır. <br/>Bu dönüşüm hazır özelleştirirken önceden kullanın. Daha fazla bilgi için [nasıl dönüşüm önayarlarını özelleştirme](customize-encoder-presets-how-to.md).|

## <a name="custom-presets"></a>Özel önayarların kullanılmasına

Media Services kodlama özgü ihtiyaçları ve gereksinimleri karşılamak için hazır olarak tüm değerleri özelleştirme tam olarak destekler. Kullandığınız **StandardEncoderPreset** dönüştürme hazır özelleştirirken hazır. İçin bir ayrıntılı açıklamaları ve örnek için bkz. [Kodlayıcı önayarlarını özelleştirme](customize-encoder-presets-how-to.md).

## <a name="scaling-encoding-in-v3"></a>V3 sürümünde kodlama ölçeklendirme

Şu anda, RU ayarlamak için Azure portalı veya AMS v2 API'leri kullanmayı imajlarını (açıklandığı [medya işlemeyi ölçeklendirme](../previous/media-services-scale-media-processing-overview.md). 

## <a name="next-steps"></a>Sonraki adımlar

### <a name="tutorials"></a>Öğreticiler

Aşağıdaki tutorals içeriğinizi Media Services ile kodlama göster:

* [Karşıya yükleme, kodlama ve Azure Media Services'i kullanarak akış](stream-files-tutorial-with-api.md)
* [Azure Media Services ile videoları analiz etme](analyze-videos-tutorial-with-api.md)

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

