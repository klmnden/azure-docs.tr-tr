---
title: Azure Media Services'i kullanarak canlı akış genel bakış | Microsoft Docs
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
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: a31cd950ae241eb55c840c716f4679c5a67b1379
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58350022"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Canlı akış ile Azure Media Services v3

Azure Media Services Canlı etkinlikler müşterilerinizin Azure bulutunda dağıtmanıza olanak sağlar. Media Services ile etkinliklerin canlı akış için aşağıdakiler gerekir:  

- Canlı etkinliği yakalamak için kullanılan bir kamera.<br/>Kurulum fikir edinmek için kullanıma [basit ve taşınabilir olay video dişli Kurulum]( https://link.medium.com/KNTtiN6IeT).
- Media Services gönderilir akış bir katkı sinyalleri bir kamera (veya başka bir cihaz, bir dizüstü bilgisayar gibi) dönüştürür canlı bir video Kodlayıcısı. Sinyaller SCTE-35 işaretçileri gibi bir reklam ilgili katkı akışı içerebilir.<br/>Önerilen canlı akış Kodlayıcıları listesi için bkz. [Canlı Kodlayıcıları akış](recommended-on-premises-live-encoders.md). Ayrıca, bu bloguna göz atın: [Üretim OBS ile canlı akış](https://link.medium.com/ttuwHpaJeT).
- Alabilmek için etkinleştirmek, Media Services bileşenleri Önizleme, paket, kayıt, şifrelemek ve müşterilerinize veya başkalarına dağıtım için bir CDN için Canlı etkinlik yayını.

Bu makalede, bir genel bakış ve Media Services ve ilgili diğer makalelere bağlantılar ile canlı akış rehberlik sağlar.

> [!NOTE]
> Şu anda Azure portalında v3 kaynakları yönetmek için kullanamazsınız. Kullanım [REST API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref), veya desteklenen biri [SDK'ları](developers-guide.md).

## <a name="dynamic-packaging"></a>Dinamik paketleme

Media Services sayesinde, Önizleme ve yayın Canlı akışlarınız olanak tanıyan dinamik Packaging](dynamic-packaging-overview.md) yararlanabilirsiniz [MPEG DASH, HLS ve kesintisiz akış biçimlerinde](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) katkı gelen akış hizmetine gönderir. İzleyicilerinize herhangi HLS, DASH veya kesintisiz akış uyumlu yürütücüler ile canlı akış oynatabilirsiniz. Kullanabileceğiniz [Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/index.html) web veya mobil uygulamalar, akışınız şu protokollerin birinde sunmak için.

## <a name="dynamic-encryption"></a>Dinamik şifreleme

Dinamik şifreleme, Canlı veya isteğe bağlı içeriğinizi AES-128 veya üç ana dijital hak yönetimi (DRM) sistemlerinden ile dinamik olarak şifreleme sağlar: Microsoft PlayReady, Google Widevine ve FairPlay Apple. Media Services de AES anahtarları ve DRM sunmaya yönelik bir hizmet sağlar (PlayReady, Widevine ve FairPlay) lisansları yetkili istemcilere. Daha fazla bilgi için [dinamik şifreleme](content-protection-overview.md).

## <a name="dynamic-manifest"></a>Dinamik bildirimi

Dinamik filtreleme izler, biçimleri, bit hızlarına dönüştürme ve oyunculara gönderilen sunu zaman pencereleri sayısını denetlemek için kullanılır. Daha fazla bilgi için [filtreleri ve dinamik bildirimlere](filters-dynamic-manifest-overview.md).

## <a name="live-event-types"></a>Canlı olay türleri

Canlı bir olay iki türden biri olabilir: doğrudan ve canlı kodlama. Media Services v3 sürümünde canlı akış hakkında daha fazla ayrıntı için bkz [Canlı olayları ve canlı çıkışları](live-events-outputs-concept.md).

### <a name="pass-through"></a>Geçiş

![geçiş](./media/live-streaming/pass-through.svg)

Doğrudan kullanırken **canlı olay**, Çoklu bit hızı video akışı oluşturmak ve katkı Canlı (RTMP ya da parçalı MP4 protokolü kullanılarak) olay için akışı göndermek için şirket içi Canlı Kodlayıcı dayanır. Canlı olay ardından gelen video akışları herhangi başka bir işlemeye olmadan taşır. Bu tür bir doğrudan canlı olay uzun süre çalışan Canlı etkinlikler için optimize edilmiştir veya 24 x 365 doğrusal canlı akış. 

### <a name="live-encoding"></a>Live encoding  

![live Encoding](./media/live-streaming/live-encoding.svg)

Media Services ile Live encoding kullanıldığında, tek bit hızlı Canlı (RTMP veya parçalanmış Mp4 protokolünü kullanarak) olay için akışı katkı olarak video göndermek için şirket içi Canlı Kodlayıcı yapılandırırsınız. Bu gelen tek bit hızlı canlı olay kodlar için akış bir [birden çok hızlı video akışına](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming), MPEG-DASH, HLS ve kesintisiz akış gibi protokolleri aracılığıyla cihazları kayıttan yürütmek teslim için kullanılabilir hale getirir. 

## <a name="live-streaming-workflow"></a>Canlı akış iş akışı

Media Services v3 canlı akış iş akışı anlamak için ilk gözden geçirme için sahip ve aşağıdaki kavramları anlama: 

- [Akış uç noktaları](streaming-endpoint-concept.md)
- [Canlı etkinlikler ve canlı çıkışları](live-events-outputs-concept.md)
- [Akış bulucuları](streaming-locators-concept.md)

Canlı akış iş akışı için adımlar şunlardır:

1. Media Services hesabınızı emin **akış uç noktası** çalışıyor. 
2. Oluşturma bir [canlı olay](live-events-outputs-concept.md). <br/>Olay oluşturulurken otomatik başlatma için bunu belirtebilirsiniz. Alternatif olarak, akış başlamaya hazır olduğunuzda olayı başlatın.<br/> Autostart canlı olay true olarak ayarlandığında oluşturulduktan sonra doğru başlatılır. Faturalandırma, canlı olay çalışmaya başladıktan hemen sonra başlar. Daha fazla faturalama durdurmak için canlı olay kaynağı durdurma açıkça çağırmanız gerekir. Daha fazla bilgi için [canlı olay durumları ve faturalandırma](live-event-states-billing.md).
3. Alma URL'leri alma ve akış katkı göndermek için URL'yi kullanmak için şirket içi Kodlayıcı yapılandırın.<br/>Bkz: [gerçek zamanlı kodlayıcılar önerilen](recommended-on-premises-live-encoders.md).
4. Önizleme URL'sini ve aslında kodlayıcıdan giriş alındığını doğrulamak için kullanın.
5. Yeni bir **varlık** nesne.
6. Oluşturma bir **Canlı çıkış** oluşturduğunuz varlık adını kullanın.<br/>**Canlı çıkış** akışa arşiv **varlık**.
7. Oluşturma bir **akış Bulucu** yerleşik ile **akış ilke** türleri.<br/>İçeriğinizi şifrelemek istiyorsanız, gözden [içerik korumaya genel bakış](content-protection-overview.md).
8. Yolları listesini **akış Bulucu** kullanılacak URL'leri geri almak için (Bu belirleyici).
9. Konak adı için alma **akış uç noktası** alanından akışını yapmak istiyor.
10. Ana bilgisayar adı tam URL'sini almak için 9. adım 8. adımdaki URL'yi birleştirin.
11. Kullanımını durdurmak istiyorsanız, **canlı olay** silme ve olay akışı durdurmak zorunda görüntülenebilir, **akış Bulucu**.

## <a name="other-important-articles"></a>Diğer önemli makaleleri

- [Önerilen gerçek zamanlı kodlayıcılar](recommended-on-premises-live-encoders.md)
- [Bir bulut DVR kullanma](live-event-cloud-dvr.md)
- [Canlı olay türleri özellik karşılaştırması](live-event-types-comparison.md)
- [Durumları ve faturalandırma](live-event-states-billing.md)
- [Gecikme süresi](live-event-latency.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Canlı akış Öğreticisi](stream-live-tutorial-with-api.md)
* [Media Services v2'den v3 taşımak için Geçiş Kılavuzu](migrate-from-v2-to-v3.md)
