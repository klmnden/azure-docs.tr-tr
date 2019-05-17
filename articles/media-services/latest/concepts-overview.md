---
title: Azure Media Services terimler ve kavramlar - Azure | Microsoft Docs
description: Bu konu Azure Media Services terimleri ve kavramları kısa bir genel bakışını veren ve daha fazla ayrıntı için bağlantılar sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/13/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 1e76569c7f5157dce681d15ec8d499b90e080102
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65762303"
---
# <a name="media-services-concepts"></a>Media Services kavramları

Bu konu Azure Media Services terimleri ve kavramları kısa bir genel bakış sağlar. Makalede, Media Services v3 kavramlarda ve işlevlerde ayrıntılı açıklamasını içeren makaleler için bağlantılar da sağlanmıştır. 

Bu konularda açıklandığı gibi temel kavramları, geliştirme başlatılmadan önce incelenmelidir.

> [!NOTE]
> Şu anda, v3 kaynaklarını yönetmek için Azure portalını kullanamıyorsunuz. [REST API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref) veya desteklenen [SDK'lardan](media-services-apis-overview.md#sdks) birini kullanın.

## <a name="terminology"></a>Terminoloji

Bu bölümde, bazı ortak sektör terimleri Media Services v3 API'sine nasıl eşleştiği gösterilir.

### <a name="live-event"></a>Canlı Etkinlik

A **canlı olay** almak, biçim dönüştürme (isteğe bağlı olarak) ve paketleme, video, ses ve gerçek zamanlı meta verileri Canlı akışlar için bir işlem hattını temsil eder.

Media Services v2 API'lerinden geçişini gerçekleştiren müşteriler için **canlı olay** değiştirir **kanal** v2'de varlık. Daha fazla bilgi için [v2'den v3 geçirme](migrate-from-v2-to-v3.md).

### <a name="streaming-endpoint-packaging-and-origin"></a>Akış uç noktası (paketleme ve kaynak)

A **akış uç noktası** yaygın akış medya protokolleri (HLS birini kullanarak doğrudan bir istemci oynatıcı uygulaması için canlı ve isteğe bağlı içerik teslim eden bir dinamik (tam zamanında) paketleme ve kaynak hizmetini temsil eder veya DASH). Ayrıca, **akış uç noktası** sektör lideri benzeri DRM dinamik (tam zamanında) şifreleme sağlar.

Sektör akış ortamda, bu hizmet yaygın olarak adlandırılır bir **Paketleyici** veya **kaynak**.  Bu özellik için sektördeki diğer genel koşulları JITP (Just-de-zaman-Paketleyici) ya da JITE (Just--zaman-şifrelemesi) içerir. 
 
## <a name="cloud-upload-and-storage"></a>Bulutta karşıya yükleme ve depolama

Yönetme, şifreleme, kodlama, çözümleme ve azure'da medya içeriği akışı başlatmak için bir Media Services hesabı oluşturun ve içine dijital dosyalar karşıya yüklemek gereken **varlıklar**.

- [Bulutta karşıya yükleme ve depolama](storage-account-concept.md)
- [Varlıklar kavramı](assets-concept.md)

## <a name="encoding"></a>Kodlama

Yüksek kaliteli dijital medya dosyalarınızın varlıklarına karşıya yükledikten sonra çok çeşitli tarayıcılar ve cihazlar üzerinde yürütülen biçimlere şifreleyebilirsiniz. 

Media Services v3 ile kodlanacak oluşturmanız gerekir **dönüştüren** ve **işleri**.

![Dönüşümler](./media/encoding/transforms-jobs.png)

- [Dönüşümler ve işler](transforms-jobs-concept.md)
- [Media Services ile kodlama](encoding-concept.md)

## <a name="media-analytics"></a>Medya analizi

Video ve ses dosyalarını analiz etmek için de oluşturmak için ihtiyacınız **dönüştüren** ve **işleri**.

- [Video ve ses dosyalarını analiz etme](analyzing-video-audio-files-concept.md)

## <a name="packaging-delivery-protection"></a>Paketleme, teslim, koruma

İçeriğinizi kodlanmış sonra avantajlarından yararlanabilirsiniz **dinamik paketleme**. Medya Hizmetleri'nde bir **akış uç noktası**  /kaynağı, istemci oyuncular medya içeriği teslim etmek için kullanılan dinamik paketleme hizmetidir. Oluşturmak zorunda videoları çıktı varlığı kayıttan yürütme için istemcilere kullanabilmek için bir **akış Bulucu** ve daha sonra akış URL'lerini oluşturabilirsiniz. 

Oluştururken **akış Bulucu**, varlığın adı ek olarak belirtmeniz gerekir. **akış ilke**. **İlkeleri akış** , akış protokollerine tanımlamanıza olanak sağlar ve için şifreleme seçenekleri (varsa), **akış bulucuları**.

Dinamik paketleme, Canlı veya isteğe bağlı içerik akışı olmadığını kullanılır. Aşağıdaki diyagram, talep üzerine akış dinamik paketleme iş akışıyla gösterir.

![Dinamik paketleme](./media/dynamic-packaging-overview/media-services-dynamic-packaging.png)

Media Services ile dinamik olarak Gelişmiş Şifreleme Standardı ile (AES-128) şifrelenmiş canlı ve isteğe bağlı içerik teslim edebilirsiniz veya / ve üç ana dijital hak yönetimi (DRM) sistemlerinden: Microsoft PlayReady, Google Widevine ve FairPlay Apple. Media Services de AES anahtarları ve DRM sunmaya yönelik bir hizmet sağlar (PlayReady, Widevine ve FairPlay) lisansları yetkili istemcilere.

Akışınız şifreleme seçeneklerini belirterek, oluşturma **içerik anahtarı ilke** ilişkilendirin, **akış Bulucu**. **İçerik anahtarı ilke** istemcileri sonlandırmak için içerik anahtarını nasıl teslim edildiğini yapılandırmanızı sağlar.

Aşağıdaki resimde Media Services content protection iş akışı gösterilmektedir: 

![İçerik koruma](./media/content-protection/content-protection.svg)

&#42;dinamik şifreleme, AES-128 "şifresiz anahtar" ve CBCS CENC destekler. 

Media Services kullanabileceğiniz **dinamik bildirimlerini** yalnızca belirli bir işleme veya subclips videonuzun akışını yapmak. Aşağıdaki örnekte, bir kodlayıcı, yedi ISO MP4 video yorumlama (Başlangıç 180 p 1080 p) mezzanine varlık kodlayın için kullanıldı. Kodlanmış varlık dinamik olarak şu protokolden herhangi birini akış paketlenmiş: HLS, MPEG DASH ve kesintisiz.  Diyagramın üstünde filtre varlıkla HLS bildirimi gösterilir (tüm yedi önayarda içerir).  Alt sol, "ott" adlı bir filtre uygulandığı HLS bildirimde gösterilir. Yanıtta çıkartılır alt iki kalite düzeyi sonuçlandı 1 MB/sn altındaki tüm bit hızlarına dönüştürme kaldırmak için "ott" filtresini belirtir. Sağ alt "mobil" adlı bir filtre uygulandığı HLS bildirimde gösterilir. Çözüm olduğu iki sonuçlandı 720 p büyük yorumlama kaldırmak için "mobil" filtresi belirtir 1080 p yorumlama çıkartılır devre dışı.

![İşleme filtreleme](./media/filters-dynamic-manifest-overview/media-services-rendition-filter.png)

- [Dinamik paketleme](dynamic-packaging-overview.md)
- [Akış Uç Noktaları](streaming-endpoint-concept.md)
- [Akış Bulucular](streaming-locators-concept.md)
- [Akış İlkeleri](streaming-policy-concept.md)
- [İçerik Anahtarı İlkeleri](content-key-policy-concept.md)
- [İçerik koruma](content-protection-overview.md)
- [Dinamik bildirimler](filters-dynamic-manifest-overview.md)
- [Filtreleri](filters-concept.md)

## <a name="live-streaming"></a>Canlı akış

Azure Media Services Canlı etkinlikler müşterilerinizin Azure bulutunda dağıtmanıza olanak sağlar. **Canlı Etkinlikler** sırasında canlı video akışları alınır ve işlenir. Oluştururken bir **canlı olay**, uzak bir kodlayıcıdan canlı bir sinyal göndermek için kullanabileceğiniz bir giriş uç noktası oluşturulur. İçine giden akış oluşturduktan sonra **canlı olay**, oluşturarak akış olayını başlayabilirsiniz bir **varlık**, **Canlı çıkış**, ve **akış Bulucu** . **Çıkış canlı** akışa arşiv **varlık** ve aracılığıyla izleyiciler kullanabilmesi **akış uç noktası**. A **canlı olay** iki türden biri olabilir: **doğrudan** ve **live encoding**.

Aşağıdaki görüntüde, doğrudan türü iş akışı gösterilmektedir:

![doğrudan geçiş](./media/live-streaming/pass-through.svg)

- [Canlı akış genel bakış](live-streaming-overview.md)
- [Canlı Etkinlikler ve Canlı Çıkışlar](live-events-outputs-concept.md)

## <a name="monitoring"></a>İzleme

### <a name="event-grid"></a>Olay Kılavuzu

İşinin ilerleme durumunu görmek için kullanmalısınız **Event Grid**. Media Services, canlı olay türleri de gösterir. Event Grid ile uygulamalarınız neredeyse tüm Azure hizmetleri ve özel kaynaklardan gelen olayları takip edip bu olaylara yanıt verebilir. 

- [Event Grid olaylarını işleme](reacting-to-media-services-events.md)
- [Şemaları](media-services-event-schemas.md)

### <a name="azure-monitor"></a>Azure İzleyici

İzleyici ölçümleri ve yardımcı olacak tanılama günlüklerini uygulamalarınızı Azure İzleyici ile nasıl performans gösterdiğini anlayın.

- [Ölçümleri ve tanılama günlükleri](media-services-metrics-diagnostic-logs.md)
- [Tanılama günlükleri şemaları](media-services-diagnostic-logs-schema.md)

## <a name="player-clients"></a>Yürütücü istemcileri

Azure Media Player, birçok farklı tarayıcılar ve cihazlar üzerinde Media Services tarafından akışlı medya içeriği kayıttan yürütmek için kullanabilirsiniz. Azure Media Player, zenginleştirilmiş bir Uyarlamalı akış deneyimi sağlamak için HTML5, medya kaynağı Uzantıları (MSE) ve şifreli medya Uzantıları (EME) gibi endüstri standartlarından yararlanır. 

- [Azure Media Player'a genel bakış](use-azure-media-player.md)

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, görüşlerinizi, güncelleştirmeleri alın

Kullanıma [Azure Media Services topluluğu](media-services-community.md) soru sorun, görüşlerinizi ve medya hizmetleri hakkında güncelleştirmeler almak farklı yollarını görmek için makaleyi.

## <a name="next-steps"></a>Sonraki adımlar

* [Uzak dosya ve akış videosu – REST kodlayın](stream-files-tutorial-with-rest.md)
* [Karşıya yüklenen dosya ve akış video - .NET kodlama](stream-files-tutorial-with-api.md)
* [Stream Canlı - .NET](stream-live-tutorial-with-api.md)
* [Videonuzu - .NET analiz etme](analyze-videos-tutorial-with-api.md)
* [Dinamik şifreleme AES-128 - .NET](protect-with-aes128.md)
* [Birden çok DRM ile - .NET dinamik olarak şifreleyin](protect-with-drm.md) 
