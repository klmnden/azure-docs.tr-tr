---
title: Azure Media Services ile bulutta kodlama | Microsoft Docs
description: Bu konu Azure Media Services kullanırken kodlama işlemi açıklar
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/21/2018
ms.author: juliako
ms.openlocfilehash: e1c7536c59b110ae3dd753ff5f4b01195f8dadca
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655783"
---
# <a name="encoding-with-azure-media-services"></a>Azure Media Services ile kodlama

Azure Media Services, çok çeşitli tarayıcılar ve cihazlar üzerinde çalınabilir biçimlere yüksek kaliteli dijital medya dosyalarınızın kodlanacak sağlar. Örneğin, içeriğiniz Apple'nın HLS ya da MPEG DASH biçimleri akışına isteyebilirsiniz. Media Services Ayrıca, video ve ses içeriklerini analiz etmenize olanak tanır. Bu konu size rehberlik içeriğinizi Media Services v3 ile kodlama.

Media Services v3 ile kodlamak için bir dönüşüm ve bir iş oluşturmanız gerekir. Tarif kodlama ayarlarınızı ve çıkış için bir dönüşüm tanımlar ve iş tarif örneğidir. Daha fazla bilgi için bkz: [dönüştüren ve işleri](transform-concept.md)

Azure Media Services ile kodlama, giriş medya dosyalarınızın nasıl işleneceğini Kodlayıcı bildirmek için hazır kullanın. Örneğin, kodlanmış içeriği ekran çözünürlüğü ve/veya istediğiniz ses kanal sayısını belirtebilirsiniz. 

Hızlı bir şekilde endüstri en iyi uygulamalarına göre önerilen yerleşik hazır ayarlarından birini kullanmaya veya özel bir senaryo veya aygıt gereksinimlerinizi hedeflemek için önceden oluşturmak seçebilirsiniz. 

Kodlayıcı hakkındaki ayrıntıları edinmenize [OpenAPI belirtimi](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/preview/2018-03-30-preview). 

## <a name="built-in-presets"></a>Yerleşik hazır

Media Services şu anda aşağıdaki yerleşik kodlama hazır destekler:  

|**Önceden tanımlı ayar adı**|**Senaryo**|**Ayrıntılar**|
|---|---|---|
|**AudioAnalyzerPreset**|Ses analiz etme|Önceden ayarlanmış AI çözümleme işlemleri konuşma transcription dahil olmak üzere, önceden tanımlanmış bir dizi geçerlidir. Şu anda, önceden ayarlanmış tek bir ses parçası olan içeriği işlenmesini destekler.<br/>Ses yükü dilini 'dil etiketi bölge' BCP 47 biçimi kullanarak girişinde belirtebilirsiniz (örneğin, ' en-US'). Desteklenen dillerin listesi olan, 'en-US', 'tr-GB', 'es-ES', 'es-MX', 'fr-FR', 'it-IT', 'ja-JP', 'pt-BR', 'zh-CN'.|
|**VideoAnalyzerPreset**|Ses ve video analiz etme|Hem ses ve video gelen Öngörüler (zengin meta verileri) ayıklar ve bir JSON biçim dosyası çıkarır. Yalnızca bir video dosyasını işlerken ses öngörüleri ayıklamak isteyip istemediğinizi belirtebilirsiniz. Daha fazla bilgi için bkz: [Çözümle video](analyze-videos-tutorial-with-api.md).|
|**BuiltInStandardEncoderPreset**|Akış|Standart Kodlayıcı ile giriş video kodlama için önceden belirlenmiş bir yerleşik ayarlamak için kullanılır. <br/>Aşağıdaki hazır şu anda desteklenir:<br/>**EncoderNamedPreset.AdaptiveStreaming** (önerilen). Daha fazla bilgi için bkz: [bit hızı Merdiveni otomatik olarak oluşturmak](autogen-bitrate-ladder.md).<br/>**EncoderNamedPreset.AACGoodQualityAudio** -yalnızca 192 Kb/sn ile kodlanmış stereo ses içeren tek bir MP4 dosyası oluşturur.<br/>**EncoderNamedPreset.H264MultipleBitrate1080p** -6000 kbps ila 400 kbps ve stereo AAC ses arasında değişen 8 GOP hizalı MP4 dosyaları kümesi üretir. Çözümleme sırasında 1080 p başlatır ve aşağıya doğru 360 p geçer.<br/>**EncoderNamedPreset.H264MultipleBitrate720p** -400 kbps ve stereo AAC ses 3400 kbps ila arasında değişen 6 GOP hizalı MP4 dosyaları kümesi üretir. Çözümleme sırasında 720 p başlatır ve aşağıya doğru 360 p geçer.<br/>**EncoderNamedPreset.H264MultipleBitrateSD** -400 kbps ve stereo AAC ses 1600 kbps ila arasında değişen 5 GOP hizalı MP4 dosyaları kümesi üretir. Çözümleme sırasında 480 p başlatır ve aşağıya doğru 360 p geçer.<br/><br/>Daha fazla bilgi için bkz: [kodlama ve akış dosyalarını Uploading](stream-files-tutorial-with-api.md).|
|**StandardEncoderPreset**|Akış|Standart Kodlayıcı ile giriş video kodlama sırasında kullanılacak ayarları açıklanmaktadır. <br/>Bu dönüştürme hazır özelleştirirken önceden kullanın. Daha fazla bilgi için bkz: [dönüştürme hazır özelleştirmek nasıl](customize-encoder-presets-how-to.md).|

## <a name="custom-presets"></a>Özel hazır

Media Services, belirli kodlama ihtiyaçları ve gereksinimleri karşılamak üzere hazır içindeki tüm değerleri özelleştirme tam olarak destekler. Kullandığınız **StandardEncoderPreset** dönüştürme hazır özelleştirirken hazır. İçin bir ayrıntılı açıklamaları ve örnek bkz [encoder hazır ayarlarını özelleştirmek nasıl](customize-encoder-presets-how-to.md).

## <a name="scaling-encoding-in-v3"></a>V3 kodlama ölçeklendirme

Müşteriler şu anda, RUs ayarlamak için Azure portal veya AMS v2 API'leri kullanmak zorunda (açıklandığı gibi [medyayı işleme ölçeklendirme](../previous/media-services-scale-media-processing-overview.md). 

## <a name="next-steps"></a>Sonraki adımlar

### <a name="tutorials"></a>Öğreticiler

Aşağıdaki tutorals içeriğinizi Media Services ile kodlama göster:

* [Karşıya yükleme, kodlama ve Azure Media Services'i kullanarak akış](stream-files-tutorial-with-api.md)
* [Azure Media Services ile video Çözümle](analyze-videos-tutorial-with-api.md)

### <a name="code-samples"></a>Kod örnekleri

Aşağıdaki kod örnekleri, Media Services ile kodlama gösteren kod içerir:

* [.NET Core](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore)
* [CLI 2.0](https://github.com/Azure/azure-docs-cli-python-samples/tree/master/media-services)

### <a name="sdks"></a>SDK’lar

İçeriğinizi kodlamak için aşağıdaki desteklenen Media Services v3 SDK'ları birini kullanabilirsiniz.

* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
* [REST](https://docs.microsoft.com/rest/api/media/transforms)
* [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/mediaservices/management?view=azure-dotnet)
* [Java](https://docs.microsoft.com/java/api/overview/azure/mediaservices)
* [Python](https://docs.microsoft.com/python/api/overview/azure/media-services?view=azure-python)

