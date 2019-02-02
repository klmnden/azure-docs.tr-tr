---
title: Azure Media Services tarafından - önerilen canlı akış şirket içi Kodlayıcıları hakkında bilgi edinin | Microsoft Docs
description: Media Services tarafından önerilen canlı akış şirket içi Kodlayıcıları hakkında bilgi edinin
services: media-services
keywords: encoding;encoders;media
author: johndeu
manager: johndeu
ms.author: johndeu
ms.date: 01/17/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: c3e42ba9fe84ded8c60fc71f19de785945852116
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55656677"
---
# <a name="recommended-live-streaming-encoders"></a>Önerilen canlı akış kodlayıcılar

Medya Hizmetleri'nde bir [canlı olay](https://docs.microsoft.com/rest/api/media/liveevents) (kanal), canlı akış içeriği işlemek için bir işlem hattı temsil eder. Canlı olay iki yoldan biriyle Canlı giriş akışları alır:

* Çoklu bit hızlı RTMP veya kesintisiz akış (parçalanmış MP4) akış canlı olay Media Services ile gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmemiş bir şirket içi Canlı Kodlayıcı gönderir. Alınan akışların herhangi başka bir işlemeye gerek kalmadan Canlı olayları geçirin. Bu yöntem çağrılır **doğrudan**. Gerçek zamanlı bir kodlayıcı tek bit hızında akışa bir geçiş kanalı üzerinden gönderebilir, ancak bit hızı Uyarlamalı akış için bir istemci için izin vermediğinden bu yapılandırma önerilmez.

  > [!NOTE]
  > Doğrudan geçiş yöntemini kullanmak canlı akış yapmanın en ekonomik yoludur.

* Bir şirket içi Canlı Kodlayıcı tek bit hızında akışa aşağıdaki biçimlerden birinde Media Services ile gerçek zamanlı kodlama gerçekleştirmek için etkinleştirildiği canlı olay gönderir: RTMP veya kesintisiz akış (parçalanmış MP4). Canlı olay gelen tek bit hızlı akışın Çoklu bit hızlı (Uyarlamalı) video akışına gerçek zamanlı kodlama gerçekleştirir.

Media Services ile gerçek zamanlı kodlama hakkında ayrıntılı bilgi için bkz. [canlı akış ile Media Services v3](live-streaming-overview.md).

## <a name="live-encoders-that-output-rtmp"></a>Gerçek zamanlı kodlayıcılar RTMP çıktısı

Media Services RTMP çıktısı olarak sahip şu gerçek zamanlı kodlayıcılar kullanarak önerir. Desteklenen URL düzenler `rtmp://` veya `rtmps://`.

> [!NOTE]
 > RTMP akış, 1935 ve 1936 giden TCP bağlantı noktalarının açık olduğunu doğrulamak için güvenlik duvarı ve/veya proxy ayarlarını kontrol edin.<br/>
 RTMPS akış, 2935 ve 2936 giden TCP bağlantı noktalarının açık olduğunu doğrulamak için güvenlik duvarı ve/veya proxy ayarlarını kontrol edin.

- Adobe Flash Media Live Encoder 3.2
- Haivision KB
- Haivision Makito X HEVC
- OBS Studio
- Değiştirici Studio (iOS)
- Telestream Wirecast 8.1 +
- Telestream Wirecast S
- Teradek dilim 756
- TriCaster 8000
- Tricaster Mini HD-4
- VMIX
- xStream

## <a name="live-encoders-that-output-fragmented-mp4"></a>Parçalanmış MP4 çıkış gerçek zamanlı kodlayıcılar

Media Services, Çoklu bit hızlı kesintisiz akış (parçalanmış MP4) çıktı olarak sahip şu gerçek zamanlı kodlayıcılar kullanarak önerir. Desteklenen URL düzenler `http://` veya `https://`.

- Ateme TITAN Canlı
- Cisco dijital medya Kodlayıcısı 2200
- Elemental Live
- Envivio 4Caster C4 Gen III
- Imagine Communications Selenio MCP3
- Medya Excel Hero canlı ve Hero 4 K (UHD/HEVC)

## <a name="configuring-on-premises-live-encoder-settings"></a>Yapılandırma gerçek zamanlı Kodlayıcı ayarları şirket

Canlı etkinlik türünüz için geçerli ayarları hakkında daha fazla bilgi için bkz. [canlı olay türlerini karşılaştırma](live-event-types-comparison.md).

### <a name="playback-requirements"></a>Kayıttan yürütme gereksinimleri

Hem bir ses ve video akışı kayıttan yürütme içeriği sırada mevcut olması gerekir. Kayıttan yürütme yalnızca video akışının desteklenmiyor.

### <a name="configuration-tips"></a>Yapılandırma ipuçları

- Mümkün olduğunda, bir sabit internet bağlantısı kullanın.
- Bir iyi bant genişliği gereksinimlerini belirlerken için udur akış bit hızlarına dönüştürme çift. Bu zorunlu bir gereksinim olmamasına karşın, Ağ Tıkanıklığı etkisini azaltmaya yardımcı olur.
- Kodlayıcıları kullanarak yazılım tabanlı, gereksiz tüm programları kapatın.
- Gönderme başlatıldıktan sonra kodlayıcı yapılandırması değiştirmeyin. Bu olayla ilgili olumsuz etkilere sahiptir ve olay kararsız duruma gelmesine neden olabilir. 
- Kendiniz etkinliğiniz kurmak için bol zaman vermek emin olun. Büyük ölçekli olay için saat önce olay Kur'u başlatmak için önerilir.

## <a name="how-to-become-an-on-premises-encoder-partner"></a>Nasıl bir şirket içi Kodlayıcı iş ortağı

Bir Azure Media Services şirket içi Kodlayıcı iş ortağı olarak, Media Services, kurumsal müşterilere kodlayıcınız önererek ürününüzü yükseltir. Bir şirket içi Kodlayıcı iş ortağı için Uyumluluk, şirket içi Kodlayıcı, Media Services ile doğrulamanız gerekir. Bunu yapmak için aşağıdaki Doğrulamalar tamamlayın:

### <a name="pass-through-live-event-verification"></a>Doğrudan canlı olay doğrulama

1. Media Services hesabınızı emin **akış uç noktası** çalışıyor. 
2. Oluşturma ve başlatma **doğrudan** canlı olay. <br/> Daha fazla bilgi için [canlı olay durumları ve faturalandırma](live-event-states-billing.md).
3. Alma URL'leri alma ve göndermeye URL'sini kullanmak üzere, şirket içi Kodlayıcı yapılandırma Media Services için bir Çoklu bit hızlı canlı akış.
4. Önizleme URL'sini ve aslında kodlayıcıdan giriş alındığını doğrulamak için kullanın.
5. Yeni bir **varlık** nesne.
6. Oluşturma bir **Canlı çıkış** oluşturduğunuz varlık adını kullanın.
7. Oluşturma bir **akış Bulucu** yerleşik ile **akış ilke** türleri.
8. Yolları listesini **akış Bulucu** kullanılacak URL'leri geri dönebilirsiniz.
9. Konak adı için alma **akış uç noktası** alanından akışını yapmak istiyor.
10. Ana bilgisayar adı tam URL'sini almak için 9. adım 8. adımdaki URL'yi birleştirin.
11. Yaklaşık 10 dakika, gerçek zamanlı Kodlayıcı çalıştırın.
12. Canlı etkinliği durdurmak. 
13. Gibi bir oynatıcı kullanın [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) , kayıttan yürütme için tüm kalite düzeylerine görünür hiçbir arızalardan bulunmasını arşivlenmiş varlığı izleyin (ya da alternatif olarak izleyin ve önizleme URL'si aracılığıyla Canlı oturumda doğrulamak için).
14. Canlı Arşiv ve ayarları, Canlı kodlayıcıdan kullanılan sürümü ve akış URL'si yayımlanmış varlık kimliği kaydedin.
15. Her bir örnek oluşturduktan sonra canlı olay durumunu sıfırlayın.
16. (İle ve ad sinyal/açıklamalı alt yazılar/hızları kodlama farklı olmadan), kodlayıcı tarafından desteklenen tüm yapılandırmalar için 15-5 adımlarını tekrarlayın.

### <a name="live-encoding-live-event-verification"></a>Canlı kodlama canlı olay doğrulama

1. Media Services hesabınızı emin **akış uç noktası** çalışıyor. 
2. Oluşturma ve başlatma **live encoding** canlı olay. <br/> Daha fazla bilgi için [canlı olay durumları ve faturalandırma](live-event-states-billing.md).
3. Alma URL'leri alma ve tek bit hızında bir canlı akışı Media Services'a iletin yapmak için kodlayıcınızı yapılandırın.
4. Önizleme URL'sini ve aslında kodlayıcıdan giriş alındığını doğrulamak için kullanın.
5. Yeni bir **varlık** nesne.
6. Oluşturma bir **Canlı çıkış** oluşturduğunuz varlık adını kullanın.
7. Oluşturma bir **akış Bulucu** yerleşik ile **akış ilke** türleri.
8. Yolları listesini **akış Bulucu** kullanılacak URL'leri geri dönebilirsiniz.
9. Konak adı için alma **akış uç noktası** alanından akışını yapmak istiyor.
10. Ana bilgisayar adı tam URL'sini almak için 9. adım 8. adımdaki URL'yi birleştirin.
11. Yaklaşık 10 dakika, gerçek zamanlı Kodlayıcı çalıştırın.
12. Canlı etkinliği durdurmak.
13. Gibi bir oynatıcı kullanın [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) , kayıttan yürütme için tüm kalite düzeylerine görünür hiçbir arızalardan bulunmasını arşivlenmiş varlığı izleyin (ya da alternatif olarak izleyin ve önizleme URL'si aracılığıyla Canlı oturumda doğrulamak için).
14. Canlı Arşiv ve ayarları, Canlı kodlayıcıdan kullanılan sürümü ve akış URL'si yayımlanmış varlık kimliği kaydedin.
15. Her bir örnek oluşturduktan sonra canlı olay durumunu sıfırlayın.
16. (İle ve ad sinyal/açıklamalı alt yazılar/hızları kodlama farklı olmadan), kodlayıcı tarafından desteklenen tüm yapılandırmalar için 15-5 adımlarını tekrarlayın.

### <a name="longevity-verification"></a>Dayanıklılık doğrulama

Aynı adımları olarak [doğrudan canlı olay doğrulama](#pass-through-live-event-verification) 11. adım dışında. <br/>10 dakika yerine, bir hafta veya daha uzun, gerçek zamanlı Kodlayıcı çalıştırın. Gibi bir oynatıcı kullanın [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) Canlı süresi zaman (ya da arşivlenmiş varlığı), kayıttan yürütme sahip hiçbir görünür arızalardan emin olmak için akış izlemek için.

### <a name="email-your-recorded-settings"></a>E-posta, kaydedilmiş ayarları

Son olarak, kaydedilen ayarlarınızı e-posta ve arşiv parametreleri için Azure Media Services canlı amsstreaming@microsoft.com tüm kendi kendine doğrulama denetimleri başarılı bir bildirim olarak. Ayrıca, tüm izleme kayıtları için iletişim bilgilerinizi içerir. Bu işlem ile ilgili herhangi bir sorunuz Azure Media Services ekibiyle iletişime geçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Canlı akış ile Media Services v3](live-streaming-overview.md)
