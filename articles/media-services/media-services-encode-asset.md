---
title: "Genel bakış ve isteğe bağlı medya kodlayıcılar üzerinde Azure karşılaştırması | Microsoft Docs"
description: "Bu konuda, genel bir bakış ve Azure karşılaştırma isteğe bağlı medya kodlayıcılar üzerine sağlar."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: e6bfc068-fa46-4d68-b1ce-9092c8f3a3c9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: juliako
ms.openlocfilehash: 538a6ab60168735c2626a93cdeedd8d4999a6efc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a>Genel bakış ve isteğe bağlı medya kodlayıcılar üzerinde Azure karşılaştırması
## <a name="encoding-overview"></a>Kodlama genel bakış
Azure Media Services, bulutta medya kodlama için birden çok seçenekler sağlar.

Media Services ile başlıyor, codec bileşenleri ve dosya biçimlerini arasındaki farkı anlamak önemlidir.
Codec bileşenleri sıkıştırma/açma algoritmaları dosya biçimleri sıkıştırılmış video tutan kapsayıcılar ise uygulayan yazılımlardır.

Media Services, bu yeniden paketlemeden zorunda kalmadan, bit hızı Uyarlamalı MP4 veya kesintisiz akış kodlanmış içeriğinizi Media Services tarafından (MPEG DASH, HLS, kesintisiz akış) desteklenen akış biçimlerinde göndermenizi sağlayan dinamik paketleme sağlar. Akış biçimlerine.

>[!NOTE]
>AMS hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. Yararlanmak için [dinamik paketleme](media-services-dynamic-packaging-overview.md), aşağıdakileri yapmanız gerekir:
>
>Ayrıca, kaynak dosyanızı bit hızı Uyarlamalı MP4 dosyası ya da (kodlama adımları Bu öğreticinin ilerleyen bölümlerinde gösterilmektedir) Uyarlamalı bit hızlı kesintisiz akış dosyaları kümesine kodlayın.

Media Services, bu makalede açıklanan isteğe bağlı kodlayıcılar üzerinde aşağıdakileri destekler:

* [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
* [Media Encoder Premium İş Akışı](media-services-encode-asset.md#media-encoder-premium-workflow)

Bu makalede, kısa bir genel bakış isteğe bağlı medya kodlayıcılar sağlar ve daha ayrıntılı bilgi vermek makalelerinin bağlantıları sağlar. Konu aynı zamanda kodlayıcılar karşılaştırması sağlar.

>[!NOTE]
>Varsayılan olarak, aynı anda bir etkin kodlama görev her Media Services hesabına sahip olabilir. Eşzamanlı olarak, satın aldığınız her kodlama ayrılmış birim için bir tane çalışan birden çok kodlama görevleri yapmanıza izin kodlama birimleri ayırabilirsiniz. Bilgi için bkz: [kodlama birimleri ölçeklendirme](media-services-scale-media-processing-overview.md).

## <a name="media-encoder-standard"></a>Media Encoder Standard
### <a name="how-to-use"></a>Nasıl kullanılır
[Medya Kodlayıcısı standart ile kodlamak nasıl](media-services-dotnet-encode-with-media-encoder-standard.md)

### <a name="formats"></a>Biçimleri
[Biçimleri ve codec bileşenleri](media-services-media-encoder-standard-formats.md)

### <a name="presets"></a>Hazır
Medya Kodlayıcısı standart yapılandırılmış açıklanan Kodlayıcı hazır ayarlarından birini kullanarak [burada](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

### <a name="input-and-output-metadata"></a>Giriş ve çıkış meta verileri
Kodlayıcılar giriş meta verileri açıklanan [burada](media-services-input-metadata-schema.md).

Kodlayıcılar çıkış meta verileri açıklanan [burada](media-services-output-metadata-schema.md).

### <a name="generate-thumbnails"></a>Küçük resimler oluşturma
Bilgi için bkz: [medya Kodlayıcı standart kullanarak küçük resimleri oluşturmak nasıl](media-services-advanced-encoding-with-mes.md#thumbnails).

### <a name="trim-videos-clipping"></a>Videoları (kırpma) kırpma
Bilgi için bkz: [medya Kodlayıcı standart kullanarak videolar kırpma nasıl](media-services-advanced-encoding-with-mes.md#trim_video).

### <a name="create-overlays"></a>Yer paylaşımları oluşturma
Bilgi için bkz: [medya Kodlayıcı standart kullanarak yer paylaşımları oluşturma](media-services-advanced-encoding-with-mes.md#overlay).

### <a name="see-also"></a>Ayrıca bkz.
[Media Services blogu](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)

## <a name="media-encoder-premium-workflow"></a>Media Encoder Premium İş Akışı
### <a name="overview"></a>Genel Bakış
[Premium Azure Media Services kodlama Tanıtımı](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

### <a name="how-to-use"></a>Nasıl kullanılır
Medya Kodlayıcısı Premium iş akışı, karmaşık iş akışları kullanılarak yapılandırılır. İş akışı dosyalarını oluşturulabilir ve kullanarak güncelleştirilen [iş akışı Tasarımcısı](media-services-workflow-designer.md) aracı.

[Premium Azure Media Services kodlama kullanma](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

### <a name="known-issues"></a>Bilinen sorunlar
Giriş videonuzun içermiyorsa kapalı açıklamalı alt yazı, çıkış varlık hala boş bir TTML dosya içerir.


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>İlgili makaleler
* [Medya Kodlayıcısı standart hazır özelleştirerek gelişmiş kodlama görevleri gerçekleştirme](media-services-custom-mes-presets-with-dotnet.md)
* [Kotalar ve kısıtlamaları](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
