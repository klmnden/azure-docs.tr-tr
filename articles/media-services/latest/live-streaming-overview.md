---
title: Canlı akış ile Azure Media Services v3'e genel bakış | Microsoft Docs
description: Bu makalede, genel bir bakış Canlı Azure Media Services v3 kullanarak akış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/16/2019
ms.author: juliako
ms.openlocfilehash: 0abc3eec380cccae2672d0e9aa4a3a4c7199362f
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295670"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Canlı akış ile Azure Media Services v3

Azure Media Services Canlı etkinlikler müşterilerinizin Azure bulutunda dağıtmanıza olanak sağlar. Media Services ile etkinliklerin canlı akış için aşağıdakiler gerekir:  

- Canlı etkinliği yakalamak için kullanılan bir kamera.<br/>Kurulum fikir edinmek için kullanıma [basit ve taşınabilir olay video dişli Kurulum]( https://link.medium.com/KNTtiN6IeT).

    Bir kamera erişimi yoksa gibi araçlar [Telestream Wirecast](https://www.telestream.net/wirecast/overview.htm) kullanılabilir bir canlı akış video dosyasından oluşturur.
- Media Services gönderilir akış bir katkı sinyalleri bir kamera (veya başka bir cihaz, bir dizüstü bilgisayar gibi) dönüştürür canlı bir video Kodlayıcısı. Sinyaller SCTE-35 işaretçileri gibi bir reklam ilgili katkı akışı içerebilir.<br/>Önerilen canlı akış Kodlayıcıları listesi için bkz. [Canlı Kodlayıcıları akış](recommended-on-premises-live-encoders.md). Ayrıca, bu bloguna göz atın: [Üretim OBS ile canlı akış](https://link.medium.com/ttuwHpaJeT).
- Alabilmek için etkinleştirmek, Media Services bileşenleri Önizleme, paket, kayıt, şifrelemek ve müşterilerinize veya başkalarına dağıtım için bir CDN için Canlı etkinlik yayını.

Bu makalede, bir genel bakış ve Media Services ve ilgili diğer makalelere bağlantılar ile canlı akış rehberlik sağlar.
 
> [!NOTE]
> Şu anda, v3 kaynaklarını yönetmek için Azure portalını kullanamıyorsunuz. [REST API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref) veya desteklenen [SDK'lardan](media-services-apis-overview.md#sdks) birini kullanın.

## <a name="dynamic-packaging"></a>Dinamik paketleme

Media Services ile avantajlarından yararlanabilirsiniz [dinamik paketleme](dynamic-packaging-overview.md), Önizleme ve yayın Canlı akışlarınız olanak tanıyan [MPEG DASH, HLS ve kesintisiz akış biçimlerinde](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) gelen akış katkı Bu hizmete gönderiliyor. İzleyicilerinize herhangi HLS, DASH veya kesintisiz akış uyumlu yürütücüler ile canlı akış oynatabilirsiniz. Kullanabileceğiniz [Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/index.html) web veya mobil uygulamalar, akışınız şu protokollerin birinde sunmak için.

## <a name="dynamic-encryption"></a>Dinamik şifreleme

Dinamik şifreleme, Canlı veya isteğe bağlı içeriğinizi AES-128 veya üç ana dijital hak yönetimi (DRM) sistemlerinden ile dinamik olarak şifreleme sağlar: Microsoft PlayReady, Google Widevine ve FairPlay Apple. Media Services de AES anahtarları ve DRM sunmaya yönelik bir hizmet sağlar (PlayReady, Widevine ve FairPlay) lisansları yetkili istemcilere. Daha fazla bilgi için [dinamik şifreleme](content-protection-overview.md).

## <a name="dynamic-manifest"></a>Dinamik bildirimi

Dinamik filtreleme izler, biçimleri, bit hızlarına dönüştürme ve oyunculara gönderilen sunu zaman pencereleri sayısını denetlemek için kullanılır. Daha fazla bilgi için [filtreleri ve dinamik bildirimlere](filters-dynamic-manifest-overview.md).

## <a name="live-event-types"></a>Canlı olay türleri

[Canlı Etkinlikler](https://docs.microsoft.com/rest/api/media/liveevents) sırasında canlı video akışları alınır ve işlenir. Canlı bir olay iki türden biri olabilir: doğrudan ve canlı kodlama. Media Services v3 sürümünde canlı akış hakkında daha fazla ayrıntı için bkz [Canlı olayları ve canlı çıkışları](live-events-outputs-concept.md).

### <a name="pass-through"></a>Geçiş

![doğrudan geçiş](./media/live-streaming/pass-through.svg)

Doğrudan kullanırken **canlı olay**, Çoklu bit hızı video akışı oluşturmak ve katkı Canlı (RTMP ya da parçalı MP4 giriş protokolü kullanılarak) olay için akışı göndermek için şirket içi Canlı Kodlayıcı dayanır. Canlı olay ardından gelen video akışları herhangi başka bir kodlama dönüştürme olmadan dinamik Paketleyici (akış uç noktası) üzerinden taşır. Bu tür bir doğrudan canlı olay uzun süre çalışan Canlı etkinlikler için optimize edilmiştir veya 24 x 365 doğrusal canlı akış. 

### <a name="live-encoding"></a>Live encoding  

![gerçek zamanlı kodlama](./media/live-streaming/live-encoding.svg)

Bulut ile Media Services encoding kullanıldığında, tek bit hızlı video katkı göndermek için şirket içi Canlı Kodlayıcı yapılandırırsınız akış (en fazla toplam 32Mbps) canlı olay (RTMP ya da parçalı MP4 giriş protokolünü kullanarak). Canlı olay yürütülebilecek gelen tek bit hızlı akış içine [Çoklu bit hızı video akışları](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) teslim artırmak için çeşitli çözünürlükte ve endüstri standardı protokoller üzerinden kayıttan yürütme cihazlara teslim için kullanılabilir hale getirir MPEG-DASH, Apple HTTP canlı akış (HLS) ve Microsoft kesintisiz akış gibi. 

## <a name="live-streaming-workflow"></a>Canlı akış iş akışı

Media Services v3 canlı akış iş akışı anlamak için ilk gözden geçirme için sahip ve aşağıdaki kavramları anlama: 

- [Akış Uç Noktaları](streaming-endpoint-concept.md)
- [Canlı Etkinlikler ve Canlı Çıkışlar](live-events-outputs-concept.md)
- [Akış Bulucular](streaming-locators-concept.md)

### <a name="general-steps"></a>Genel adımlar

1. Media Services hesabınızı emin **akış uç noktası** (kaynak) çalışıyor. 
2. Oluşturma bir [canlı olay](live-events-outputs-concept.md). <br/>Olay oluşturulurken otomatik başlatma için bunu belirtebilirsiniz. Alternatif olarak, akış başlamaya hazır olduğunuzda olayı başlatın.<br/> Autostart canlı olay true olarak ayarlandığında oluşturulduktan sonra doğru başlatılır. Faturalandırma, canlı olay çalışmaya başladıktan hemen sonra başlar. Daha fazla faturalama durdurmak için canlı olay kaynağı durdurma açıkça çağırmanız gerekir. Daha fazla bilgi için [canlı olay durumları ve faturalandırma](live-event-states-billing.md).
3. Alma URL'leri alma ve akış katkı göndermek için URL'yi kullanmak için şirket içi Kodlayıcı yapılandırın.<br/>Bkz: [gerçek zamanlı kodlayıcılar önerilen](recommended-on-premises-live-encoders.md).
4. Önizleme URL'sini ve aslında kodlayıcıdan giriş alındığını doğrulamak için kullanın.
5. Yeni bir **varlık** nesne.
6. Oluşturma bir **Canlı çıkış** oluşturduğunuz varlık adını kullanın.<br/>**Canlı çıkış** akışa arşiv **varlık**.
7. Oluşturma bir **akış Bulucu** ile [yerleşik akış ilke türleri](streaming-policy-concept.md)
8. Yolları listesini **akış Bulucu** kullanılacak URL'leri geri almak için (Bu belirleyici).
9. Konak adı için alma **akış uç noktası** gelen akış istediğiniz (kaynak).
10. Ana bilgisayar adı tam URL'sini almak için 9. adım 8. adımdaki URL'yi birleştirin.
11. Kullanımını durdurmak istiyorsanız, **canlı olay** silme ve olay akışı durdurmak zorunda görüntülenebilir, **akış Bulucu**.

## <a name="other-important-articles"></a>Diğer önemli makaleleri

- [Önerilen gerçek zamanlı kodlayıcılar](recommended-on-premises-live-encoders.md)
- [Bulut DVR kullanma](live-event-cloud-dvr.md)
- [Canlı olay türleri özellik karşılaştırması](live-event-types-comparison.md)
- [Durumlar ve faturalandırma](live-event-states-billing.md)
- [Gecikme süresi](live-event-latency.md)

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, görüşlerinizi, güncelleştirmeleri alın

Kullanıma [Azure Media Services topluluğu](media-services-community.md) soru sorun, görüşlerinizi ve medya hizmetleri hakkında güncelleştirmeler almak farklı yollarını görmek için makaleyi.

## <a name="next-steps"></a>Sonraki adımlar

* [Canlı akış Öğreticisi](stream-live-tutorial-with-api.md)
* [Media Services v2'den v3 taşımak için Geçiş Kılavuzu](migrate-from-v2-to-v3.md)
