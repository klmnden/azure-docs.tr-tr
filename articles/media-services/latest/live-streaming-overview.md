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
ms.date: 02/01/2019
ms.author: juliako
ms.openlocfilehash: e90dd052f6a4af83d2dd794dd405a4700da75bde
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55656351"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Canlı akış ile Azure Media Services v3

Azure Media Services Canlı etkinlikler müşterilerinizin Azure bulutunda dağıtmanıza olanak sağlar. Media Services ile etkinliklerin canlı akış için aşağıdakiler gerekir:  

- Canlı etkinliği yakalamak için kullanılan bir kamera.<br/>Kurulum fikir edinmek için kullanıma [basit ve taşınabilir olay video dişli Kurulum]( https://link.medium.com/KNTtiN6IeT).
- Media Services gönderilir akış bir katkı sinyalleri bir kamera (veya başka bir cihaz, bir dizüstü bilgisayar gibi) dönüştürür canlı bir video Kodlayıcısı. Sinyaller SCTE-35 işaretçileri gibi bir reklam ilgili katkı akışı içerebilir.<br/>Önerilen canlı akış Kodlayıcıları listesi için bkz. [Canlı Kodlayıcıları akış](recommended-on-premises-live-encoders.md). Ayrıca, bu bloguna göz atın: [Üretim OBS ile canlı akış](https://link.medium.com/ttuwHpaJeT).
- Alabilmek için etkinleştirmek, Media Services bileşenleri Önizleme, paket, kayıt, şifrelemek ve müşterilerinize veya başkalarına dağıtım için bir CDN için Canlı etkinlik yayını.

Media Services ile avantajlarından yararlanabilirsiniz **dinamik paketleme**, Önizleme ve yayın Canlı akışlarınız olanak tanıyan [MPEG DASH, HLS ve kesintisiz akış biçimlerinde](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) gelen akış katkı hizmete gönderin. İzleyicilerinize herhangi HLS, DASH veya kesintisiz akış uyumlu yürütücüler ile canlı akış oynatabilirsiniz. Kullanabileceğiniz [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) web veya mobil uygulamalar, akışınız şu protokollerin birinde sunmak için.

Media Services dinamik olarak şifrelenmiş içerik teslim etmenizi sağlar (**dinamik şifreleme**) Gelişmiş Şifreleme Standardı (AES-128) veya herhangi bir üç ana dijital hak yönetimi (DRM) sistemi: Microsoft PlayReady, Google Widevine ve FairPlay Apple. Media Services, yetkili istemcilere AES anahtarları ve DRM lisanslarını teslim etmek üzere bir hizmet de sağlar. İçeriğinizi Media Services ile şifreleme hakkında daha fazla bilgi için bkz: [içerik genel koruma](content-protection-overview.md)

Parçaları, biçimleri ve bit hızlarına dönüştürme sayısını denetlemek için kullanılabilen, dinamik filtreleme ve oyunculara gönderilen sunu zaman pencereleri de uygulayabilirsiniz. Daha fazla bilgi için [filtreleri ve dinamik bildirimlere](filters-dynamic-manifest-overview.md).

Bu makalede, bir genel bakış ve Media Services ile canlı akış rehberlik sağlar.

## <a name="prerequisites"></a>Önkoşullar

Media Services v3 canlı akış iş akışı anlamak için gözden geçirin ve aşağıdaki kavramları anlamanız gerekir: 

- [Akış uç noktaları](streaming-endpoint-concept.md)
- [Canlı etkinlikler ve canlı çıkışları](live-events-outputs-concept.md)

## <a name="live-streaming-workflow"></a>Canlı akış iş akışı

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
