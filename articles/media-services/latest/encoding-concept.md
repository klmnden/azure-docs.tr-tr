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
ms.date: 06/08/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: b0a71e8b3ffff822521a23aafd6764bcce9bd4d4
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67303927"
---
# <a name="encoding-with-media-services"></a>Media Services ile kodlama

Terim kodlama Media Services'de dijital video ve/veya ses standart bir biçimden diğerine (a) dosyalarının boyutunu azaltır ve/veya (b) ile uyumlu bir biçimde oluşturan kullanılabilir olmasını sağlamak amacıyla içeren dosyaları dönüştürme işlemi uygular. bir cihazlar ve uygulamaların kapsamlı'yi tıklatın. Bu işlem ayrıca görüntü sıkıştırma veya biçim dönüştürme adlandırılır. Bkz: [veri sıkıştırma](https://en.wikipedia.org/wiki/Data_compression) ve [kodlama ve kodlama dönüştürme nedir?](https://www.streamingmedia.com/Articles/Editorial/What-Is-/What-Is-Encoding-and-Transcoding-75025.aspx) kavramları daha fazla açıklama için.

Cihazları ve uygulamaları tarafından genellikle videoları uyarıların [aşamalı indirme](https://en.wikipedia.org/wiki/Progressive_download) aracılığıyla veya [bit hızı Uyarlamalı akış](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming). 

* Aşamalı indirme ile teslim etme için Azure Media Services dijital medya dosyasını (Ara) dönüştürmek için halinde kullanabileceğiniz bir [MP4](https://en.wikipedia.org/wiki/MPEG-4_Part_14) ile kodlanmış bir video içeren dosya [H.264](https://en.wikipedia.org/wiki/H.264/MPEG-4_AVC) codec ve ile kodlanmış bir ses [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) codec bileşeni. Depolama hesabınızdaki bir varlığa bu MP4 dosyasına yazılır. Azure depolama API veya SDK'larını kullanabilirsiniz (örneğin, [depolama REST API'si](../../storage/common/storage-rest-api-auth.md), [JAVA SDK'sı](../../storage/blobs/storage-quickstart-blobs-java-v10.md), veya [.NET SDK'sı](../../storage/blobs/storage-quickstart-blobs-dotnet.md)) dosyasını doğrudan karşıdan yüklemek için. Çıkış oluşturduysanız, depolama, belirli bir kapsayıcı adına bir varlıkla bu konumu kullanın. Aksi takdirde, Media Services için kullanabileceğiniz [varlık kapsayıcı URL'lerin listesi](https://docs.microsoft.com/rest/api/media/assets/listcontainersas). 
* İçerik, bit hızı Uyarlamalı akış ile teslim edilmek üzere hazırlamak için çoklu bit hızlarında (düşük olarak yüksek) adresindeki kodlanacak Ara dosyayı gerekir. Bit hızını azaltıldığı gibi normal kalitesi, geçişin, bu nedenle görüntü çözünürlüğünü sağlamaktır. Bu bir sözde kodlama Merdiveni içinde – çözünürlüklerine ve bit hızlarına dönüştürme tablosu olur (bkz [otomatik olarak oluşturulan Uyarlamalı bit hızı Merdiveni](autogen-bitrate-ladder.md)). Bunun yapılması, Çoklu bit hızlarında – mezzanine dosyalarınızın kodlama için Media Services kullanabilirsiniz, bir dizi MP4 dosyaları ve ilişkili akış yapılandırma dosyaları depolama hesabınızdaki bir varlık için yazılan, alırsınız. Ardından [dinamik paketleme](dynamic-packaging-overview.md) Media Services akış gibi protokolleri aracılığıyla video teslim özelliği [MPEG-DASH](https://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP) ve [HLS](https://en.wikipedia.org/wiki/HTTP_Live_Streaming). Bu oluşturmanızı gerektiren bir [akış Bulucu](streaming-locators-concept.md) ve akış URL'lerini sonra cihazlar/uygulamalar kendi özelliklerine göre devredilebilir Desteklenen protokoller karşılık gelen derleme.

İsteğe bağlı dinamik paketleme ile kodlama iş akışı aşağıdaki diyagramda gösterilmiştir.

![Dinamik paketleme](./media/dynamic-packaging-overview/media-services-dynamic-packaging.png)

Bu konuda içeriğinizi Media Services v3 ile kodlama konusunda rehberlik sağlar.

## <a name="transforms-and-jobs"></a>Dönüşümler ve işler

Media Services v3 ile kodlanacak oluşturmak gereken bir [dönüştürme](https://docs.microsoft.com/rest/api/media/transforms) ve [iş](https://docs.microsoft.com/rest/api/media/jobs). Dönüştürme bir tarif kodlama ayarları ve çıktılar için tanımlar; İş tarif örneğidir. Daha fazla bilgi için [dönüşümler ve işler](transforms-jobs-concept.md)

Media Services ile kodlarken Kodlayıcı giriş medya dosyalarını nasıl işlenmesi gerektiğini söylemek için hazır kullanın. Örneğin, kodlanmış içeriği görüntü çözünürlüğünü ve/veya istediğiniz ses kanal sayısını belirtebilirsiniz. 

Sektördeki en iyi uygulamalarına göre önerilen yerleşik hazır biri ile hızlıca başlayabilirsiniz veya senaryonuz ya da cihaz belirli gereksinimlerinizi hedeflemek için önceden belirlenmiş bir özel bir yapı seçebilirsiniz. Daha fazla bilgi için [kodla özel dönüştürme](customize-encoder-presets-how-to.md). 

Ocak 2019'ile başlayan Medya Kodlayıcısı MP4 dosyaları üretmek için standart ile kodlarken .mpi yeni bir dosya oluşturulur ve çıktıyı eklenen varlık. Bu MPI dosya performansını artırmak için kullanılmaya [dinamik paketleme](dynamic-packaging-overview.md) ve senaryoları akış.

> [!NOTE]
> Değiştirme veya MPI dosyayı kaldırmak veya gerekir (veya etkinleştirmezsiniz) varlığını hizmetinizdeki herhangi bir bağımlılık olması, böyle bir dosya.

### <a name="creating-job-input-from-an-https-url"></a>Bir HTTPS URL'si iş girdisi oluşturma

Kullanarak videolarınızı işleyin işleri gönderdiğinizde, Media Services'ı giriş videosunun nerede bulacağını söylemeniz gerekir. Seçeneklerden birini iş giriş olarak bir HTTPS URL'si belirtmek içindir. Şu anda, Media Services v3 HTTPS URL'leri öbekli aktarım kodlamasını desteklemez. 

#### <a name="examples"></a>Örnekler

* [.NET ile bir HTTPS URL'si kodlamayı](stream-files-dotnet-quickstart.md)
* [REST ile bir HTTPS URL'si kodlamayı](stream-files-tutorial-with-rest.md)
* [CLI ile bir HTTPS URL'si kodlamayı](stream-files-cli-quickstart.md)
* [Node.js ile bir HTTPS URL'si kodlamayı](stream-files-nodejs-quickstart.md)

### <a name="creating-job-input-from-a-local-file"></a>Yerel bir dosyadan iş girdisi oluşturma

Giriş videosunun, bu durumda, bir dosyayı (yerel olarak veya Azure Blob Depolama alanında depolanan) temel bir giriş varlığı oluşturma, medya hizmeti varlık depolanabilir. 

#### <a name="examples"></a>Örnekler

[Yerleşik hazır kullanarak yerel bir dosya kodlama](job-input-from-local-file-how-to.md)

### <a name="creating-job-input-with-subclipping"></a>İş girdisi ile klip oluşturma

Bir video kodlama, ayrıca trim veya kaynak dosyası küçük ve giriş videosunun istenen bir kısmı olan bir çıkış üretmesine belirtebilirsiniz. Bu işlev ile çalışır [dönüştürme](https://docs.microsoft.com/rest/api/media/transforms) kullanarak oluşturulan [BuiltInStandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) hazır veya [StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) hazır. 

Oluşturmak için belirtebileceğiniz bir [iş](https://docs.microsoft.com/rest/api/media/jobs/create) ile tek bir küçük bir video isteğe bağlı veya canlı arşiv (kayıtlı bir olay). İş girdisi, bir varlık veya bir HTTPS URL'si olabilir.

> [!TIP]
> Video reencoding olmadan bir sublip videonuzun akışını yapmak istiyorsanız, kullanmayı [önceden filtreleme bildirimleri ile dinamik Paketleyici](filters-dynamic-manifest-overview.md).

#### <a name="examples"></a>Örnekler

Örneklere bakın:

* [Alt klip .NET ile video](subclip-video-dotnet-howto.md)
* [Alt klip REST ile bir video](subclip-video-rest-howto.md)

## <a name="built-in-presets"></a>Yerleşik hazır

Media Services şu anda aşağıdaki yerleşik kodlama Önayarları destekler:  

### <a name="builtinstandardencoderpreset"></a>BuiltInStandardEncoderPreset

[BuiltInStandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) standart Kodlayıcı ile giriş video kodlama için önceden belirlenmiş bir yerleşik ayarlamak için kullanılır. 

Şu anda desteklenen aşağıdaki hazır:

- **EncoderNamedPreset.AACGoodQualityAudio** -yalnızca 192 Kb/sn ile kodlanmış stereo ses içeren tek bir MP4 dosyası üretir.
- **EncoderNamedPreset.AdaptiveStreaming** (önerilir). Daha fazla bilgi için [hızı Merdivenini otomatik oluşturma](autogen-bitrate-ladder.md).
- **EncoderNamedPreset.ContentAwareEncodingExperimental** -içeriğe duyarlı kodlaması için Deneysel bir ön ayarı kullanıma sunar. Herhangi bir giriş içeriği göz önünde bulundurulduğunda, hizmeti Uyarlamalı akış en uygun sayıda katmanları, uygun bit hızı ve teslim çözümleme ayarlarını otomatik olarak belirlemeye çalışır. Temel alınan algoritmalar zamanla gelişmesinin devam eder. Çıktı, video ve ses aralıklı MP4 dosyaları içerir. Daha fazla bilgi için [içeriğe duyarlı kodlama Önayarı için Deneysel](cae-experimental.md).
- **EncoderNamedPreset.H264MultipleBitrate1080p** -400 KB/sn ve stereo AAC ses 6000 KB/sn arasında 8 GOP hizalı MP4 dosyaları kümesini oluşturur. Çözüm, 1080 p başlar ve aşağı 360 p geçer.
- **EncoderNamedPreset.H264MultipleBitrate720p** -400 KB/sn ve stereo AAC ses 3400 KB/sn arasında değişen 6 GOP hizalı MP4 dosyaları kümesini oluşturur. Çözüm, 720 p başlar ve aşağı 360 p geçer.
- **EncoderNamedPreset.H264MultipleBitrateSD** -400 KB/sn ve stereo AAC ses 1600 KB/sn arasında değişen 5 GOP hizalı MP4 dosyaları kümesini oluşturur. Çözüm, 480 p başlar ve aşağı 360 p geçer.
- **EncoderNamedPreset.H264SingleBitrate1080p** -burada H.264 codec 6750 KB/sn ve 1080 piksel resim yüksekliği ile video kodlanmış ve stereo ses AAC-LC codec hızı 64 ile kodlanmış bir MP4 dosyası üretir.
- **EncoderNamedPreset.H264SingleBitrate720p** -burada H.264 codec 4500 KB/sn ve 720 piksel resim yüksekliği ile video kodlanmış ve stereo ses AAC-LC codec hızı 64 ile kodlanmış bir MP4 dosyası üretir.
- **EncoderNamedPreset.H264SingleBitrateSD** -burada H.264 codec 2200 KB/sn ve 480 piksel resim yüksekliği ile video kodlanmış ve stereo ses AAC-LC codec hızı 64 ile kodlanmış bir MP4 dosyası üretir.

En güncel liste Önayarları görmek için bkz: [video kodlama için kullanılacak yerleşik hazır](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#encodernamedpreset).

Kullanıma hazır nasıl kullanıldığını görmek için [kodlama ve akış dosyalarını karşıya yükleniyor,](stream-files-tutorial-with-api.md).

### <a name="standardencoderpreset"></a>StandardEncoderPreset

[StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) standart Kodlayıcı ile giriş video kodlama, kullanılacak ayarları açıklanır. Bu dönüşüm hazır özelleştirirken önceden kullanın. 

#### <a name="considerations"></a>Dikkat edilmesi gerekenler

Özel önayarların kullanılmasına oluştururken, aşağıdaki maddeler geçerlidir:

- Yükseklik ve genişlik AVC içeriğin tüm değerleri 4'ün katı olmalıdır.
- Azure Media Services v3 sürümünde, tüm kodlama bit hızlarına dönüştürme bit / saniye cinsindendir. Bu Önayarı kilobit/saniye birimi olarak kullanılan v2 Apı'lerimiz ile farklıdır. V2'de hızı (kilobit/saniye) 128 belirtilmemişse, örneğin, v3 sürümünde, 128000 için (bit/saniye) ayarlanır.

### <a name="customizing-presets"></a>Hazır ayarlarını özelleştirme

Media Services kodlama özgü ihtiyaçları ve gereksinimleri karşılamak için hazır olarak tüm değerleri özelleştirme tam olarak destekler. Kodlayıcı önayarlarını özelleştirme işlemini gösteren örnekler için bkz:

#### <a name="examples"></a>Örnekler

- [.NET ile önayarlarını özelleştirme](customize-encoder-presets-how-to.md)
- [CLI ile önayarlarını özelleştirme](custom-preset-cli-howto.md)
- [REST ile önayarlarını özelleştirme](custom-preset-rest-howto.md)

## <a name="preset-schema"></a>Şema hazır

Media Services v3 sürümünde hazır kesin türü belirtilmiş API'sinde varlıklardır. Bu nesneler için "şema" tanımı bulabilirsiniz [açık API Belirtimi (ya da Swagger)](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01). Önceden oluşturulmuş tanımlarını da görüntüleyebilirsiniz (gibi **StandardEncoderPreset**) içinde [REST API](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset), [.NET SDK'sı](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.standardencoderpreset?view=azure-dotnet) (veya diğer Media Services v3 SDK başvuru belgeleri).

## <a name="scaling-encoding-in-v3"></a>V3 sürümünde kodlama ölçeklendirme

Medya işlemeyi ölçeklendirme için bkz: [CLI ile ölçek](media-reserved-units-cli-how-to.md).

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, görüşlerinizi, güncelleştirmeleri alın

Kullanıma [Azure Media Services topluluğu](media-services-community.md) soru sorun, görüşlerinizi ve medya hizmetleri hakkında güncelleştirmeler almak farklı yollarını görmek için makaleyi.

## <a name="next-steps"></a>Sonraki adımlar

* [Karşıya yükleme, kodlama ve Media Services'i kullanarak akış](stream-files-tutorial-with-api.md)
* [HTTPS kullanarak yerleşik hazır bir URL'den kodlayın](job-input-from-http-how-to.md)
* [Yerleşik hazır kullanarak yerel bir dosya kodlama](job-input-from-local-file-how-to.md)
* [Özel bir senaryonuz ya da cihaz belirli gereksinimlerinizi hedeflemek için önceden derleme](customize-encoder-presets-how-to.md)