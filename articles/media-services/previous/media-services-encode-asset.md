---
title: Genel bakış ve Azure üzerinde isteğe bağlı medya kodlayıcılarına karşılaştırma | Microsoft Docs
description: Bu konu, genel bir bakış ve karşılaştırma Azure üzerinde isteğe bağlı medya kodlayıcılarına sağlar.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: e6bfc068-fa46-4d68-b1ce-9092c8f3a3c9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: a976b7c1f697c09082ca0f7978bb23bb4e467e5d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61464190"
---
# <a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a>Genel bakış ve Azure üzerinde isteğe bağlı medya kodlayıcılarına karşılaştırma 

## <a name="encoding-overview"></a>Encoding'e genel bakış

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürüm olan [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/)’ü inceleyin. Ayrıca bkz [geçiş kılavuzuna v2'den v3](../latest/migrate-from-v2-to-v3.md)

Azure Media Services, bulutta medya kodlama için birden fazla seçenek sağlar.

Media Services ile başlıyor, codec bileşenleri ve dosya biçimlerini arasındaki farkı anlamak önemlidir.
Codec sıkıştırma/sıkıştırma algoritmaları dosya biçimleri sıkıştırılmış görüntü tutan kapsayıcılardır ise uygulayan yazılımlardır.

Media Services, yeniden paketlemenize gerek kalmadan, Uyarlamalı bit hızı MP4 veya kesintisiz akış kodlanmış içeriğinizi Media Services tarafından (MPEG DASH, HLS, kesintisiz akış) desteklenen akış biçimlerinde göndermenize olanak tanıyan dinamik paketleme sağlar. Akış biçimlerinde.

AMS hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 

Media Services, bu makalede açıklanan kodlayıcılar aşağıdakileri destekler:

* [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
* [Media Encoder Premium İş Akışı](media-services-encode-asset.md#media-encoder-premium-workflow)

Bu makalede, kısa bir genel bakış isteğe bağlı medya kodlayıcılarına sağlar ve daha ayrıntılı bilgi vermek makalelere bağlantılar sağlar. Konu ayrıca kodlayıcıların karşılaştırılması sağlar.

Varsayılan olarak, aynı anda etkin bir kodlama görevi her Media Services hesabı olabilir. Aynı anda, bir kodlama her ayrılmış birim, satın aldığınız için çalışan birden çok kodlama görevi açmanıza izin kodlama birimler ayırabilirsiniz. Bilgi için [kodlama birimleri ölçeklendirme](media-services-scale-media-processing-overview.md).

## <a name="media-encoder-standard"></a>Media Encoder Standard
### <a name="how-to-use"></a>Nasıl kullanılır
[Media Encoder Standard ile kodlama](media-services-dotnet-encode-with-media-encoder-standard.md)

### <a name="formats"></a>Biçimleri
[Biçimleri ve codec bileşenleri](media-services-media-encoder-standard-formats.md)

### <a name="presets"></a>Hazır
Media Encoder Standard yapılandırılmış açıklanan Kodlayıcı hazır birini kullanarak [burada](https://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

### <a name="input-and-output-metadata"></a>Giriş ve çıkış meta verileri
Kodlayıcıları giriş meta verileri açıklanan [burada](media-services-input-metadata-schema.md).

Kodlayıcıları çıkış meta verilerini açıklanan [burada](media-services-output-metadata-schema.md).

### <a name="generate-thumbnails"></a>Küçük resim oluşturma
Bilgi için [Media Encoder Standard kullanarak küçük resim oluşturma](media-services-advanced-encoding-with-mes.md#thumbnails).

### <a name="trim-videos-clipping"></a>(Kırpma) videoları kırpma
Bilgi için [videoları Media Encoder Standard kullanarak kırpmak nasıl](media-services-advanced-encoding-with-mes.md#trim_video).

### <a name="create-overlays"></a>Katmanları oluşturma
Bilgi için [Media Encoder Standard kullanarak yer paylaşımları oluşturma](media-services-advanced-encoding-with-mes.md#overlay).

### <a name="see-also"></a>Ayrıca bkz.
[Media Services blogu](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)

## <a name="media-encoder-premium-workflow"></a>Media Encoder Premium İş Akışı
### <a name="overview"></a>Genel Bakış
[Premium Azure medya Hizmetleri kodlama ile tanışın](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

### <a name="how-to-use"></a>Nasıl kullanılır
Media Encoder Premium iş akışı, karmaşık iş akışlarını kullanarak yapılandırılır. İş akışı dosyalarını oluşturulabilir ve bu kullanarak güncelleştirilmiş [iş akışı Tasarımcısı](media-services-workflow-designer.md) aracı.

[Premium Azure medya Hizmetleri kodlama kullanma](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

### <a name="known-issues"></a>Bilinen sorunlar
Girdi videonuzun içermiyorsa Kapalı Açıklamalı Altyazı, çıktı varlık hala boş bir TTML dosyası içerir.


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>İlgili makaleler
* [Gelişmiş kodlama görevleri, Media Encoder Standard hazır ayarlarını özelleştirerek gerçekleştirin](media-services-custom-mes-presets-with-dotnet.md)
* [Kotalar ve sınırlamalar](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: https://azure.microsoft.com/pricing/details/media-services/
