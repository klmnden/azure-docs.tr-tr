---
title: Azure Media Services tarafından - önerilen canlı akış şirket içi Kodlayıcıları hakkında bilgi edinin | Microsoft Docs
description: Media Services tarafından önerilen canlı akış şirket içi Kodlayıcıları hakkında bilgi edinin
services: media-services
keywords: kodlama; kodlayıcılar; medya
author: johndeu
manager: johndeu
ms.author: johndeu
ms.date: 12/14/2018
ms.topic: article
ms.service: media-services
ms.openlocfilehash: d1110669bd0ca8c0ba0caf34ef41861c500bdd33
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2018
ms.locfileid: "53790037"
---
# <a name="recommended-live-streaming-encoders"></a>Önerilen canlı akış kodlayıcılar

Medya Hizmetleri'nde bir [Livestream](https://docs.microsoft.com/rest/api/media/liveevents) (kanal), canlı akış içeriği işlemek için bir işlem hattı temsil eder. Bir Livestream iki yoldan biriyle Canlı giriş akışları alır:

* Çoklu bit hızlı RTMP veya kesintisiz akış (parçalanmış MP4) akış Media Services ile gerçek zamanlı kodlama gerçekleştirmek için etkin değil Livestream için bir şirket içi Canlı Kodlayıcı gönderir. Alınan akışların herhangi başka bir işlemeye olmadan LiveEvents geçirin. Bu yöntem çağrılır **doğrudan**. Gerçek zamanlı bir kodlayıcı tek bit hızında akışa bir geçiş kanalı üzerinden gönderebilir, ancak bit hızı Uyarlamalı akış için bir istemci için izin vermediğinden bu yapılandırma önerilmez.

  > [!NOTE]
  > Doğrudan geçiş yöntemini kullanmak canlı akış yapmanın en ekonomik yoludur.

* Bir şirket içi Canlı Kodlayıcı aşağıdaki biçimlerden birinde Media Services ile gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş Livestream tek bit hızlı akış gönderir: RTMP veya kesintisiz akış (parçalanmış MP4). Livestream gelen tek bit hızlı akışın Çoklu bit hızlı (Uyarlamalı) video akışına gerçek zamanlı kodlama gerçekleştirir.

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

Media Services, Çoklu bit hızlı kesintisiz akış (parçalanmış MP4) çıktı olarak sahip şu gerçek zamanlı kodlayıcılar kullanarak önerir. Desteklenen URL düzenler `rtmp://` veya `rtmps://`.

- Ateme TITAN Canlı
- Cisco dijital medya Kodlayıcısı 2200
- Elemental Live
- Envivio 4Caster C4 Gen III
- Imagine Communications Selenio MCP3
- Medya Excel Hero canlı ve Hero 4 K (UHD/HEVC)

## <a name="how-to-become-an-on-premises-encoder-partner"></a>Nasıl bir şirket içi Kodlayıcı iş ortağı

Bir Azure Media Services şirket içi Kodlayıcı iş ortağı olarak, Media Services, kurumsal müşterilere kodlayıcınız önererek ürününüzü yükseltir. Bir şirket içi Kodlayıcı iş ortağı için Uyumluluk, şirket içi Kodlayıcı, Media Services ile doğrulamanız gerekir. Bunu yapmak için aşağıdaki Doğrulamalar tamamlayın:

### <a name="pass-through-liveevent-verification"></a>Doğrudan Livestream doğrulama

1. Oluşturun veya Azure Media Services hesabınızı ziyaret edin.
2. Oluşturma ve başlatma bir **doğrudan** Livestream.
3. Çoklu bit hızlı canlı akış anında yapmak için kodlayıcınızı yapılandırın.
4. Yayımlanan bir canlı etkinlik oluşturun.
5. Yaklaşık 10 dakika, gerçek zamanlı Kodlayıcı çalıştırın.
6. Canlı etkinliği durdurmak.
7. Oluşturma, bir akış uç noktasını başlatın, aşağıdaki gibi bir oynatıcı kullanın [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) , kayıttan yürütme için tüm kalite düzeylerine görünür hiçbir arızalardan bulunmasını arşivlenmiş varlığı izleyin (veya alternatif olarak izleyin ve önizleme URL ile doğrulamak için Canlı oturumda 6. adımdan önce).
8. Canlı Arşiv ve ayarları, Canlı kodlayıcıdan kullanılan sürümü ve akış URL'si yayımlanmış varlık kimliği kaydedin.
9. Her bir örnek oluşturduktan sonra Livestream durumunu sıfırlayın.
10. (İle ve ad sinyal/açıklamalı alt yazılar/hızları kodlama farklı olmadan), kodlayıcı tarafından desteklenen tüm yapılandırmalar için 9 3 adımları yineleyin.

### <a name="live-encoding-liveevent-verification"></a>Live encoding Livestream doğrulama

1. Oluşturun veya Azure Media Services hesabınızı ziyaret edin.
2. Oluşturma ve başlatma bir **live encoding** Livestream.
3. Tek bit hızında bir canlı akışı anında yapmak için kodlayıcınızı yapılandırın.
4. Yayımlanan bir canlı etkinlik oluşturun.
5. Yaklaşık 10 dakika, gerçek zamanlı Kodlayıcı çalıştırın.
6. Canlı etkinliği durdurmak.
7. Oluşturma, bir akış uç noktasını başlatın, aşağıdaki gibi bir oynatıcı kullanın [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) , kayıttan yürütme için tüm kalite düzeylerine görünür hiçbir arızalardan bulunmasını arşivlenmiş varlığı izleyin (veya alternatif olarak izleyin ve önizleme URL ile doğrulamak için Canlı oturumda 6. adımdan önce).
8. Canlı Arşiv ve ayarları, Canlı kodlayıcıdan kullanılan sürümü ve akış URL'si yayımlanmış varlık kimliği kaydedin.
9. Her bir örnek oluşturduktan sonra Livestream durumunu sıfırlayın.
10. (İle ve ad sinyal/açıklamalı alt yazılar/hızları kodlama çeşitli olmadan), kodlayıcı tarafından desteklenen tüm yapılandırmalar için 9 3 adımları yineleyin.

### <a name="longevity-verification"></a>Dayanıklılık doğrulama

1. Oluşturun veya Azure Media Services hesabınızı ziyaret edin.
2. Oluşturma ve başlatma bir **doğrudan** kanal.
3. Çoklu bit hızlı canlı akış anında yapmak için kodlayıcınızı yapılandırın.
4. Yayımlanan bir canlı etkinlik oluşturun.
5. Bir hafta veya daha uzun, gerçek zamanlı Kodlayıcı çalıştırın.
6. Gibi bir oynatıcı kullanın [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) Canlı süresi zaman (ya da arşivlenmiş varlığı), kayıttan yürütme sahip hiçbir görünür arızalardan emin olmak için akış izlemek için.
7. Canlı etkinliği durdurmak.
8. Canlı Arşiv ve ayarları, Canlı kodlayıcıdan kullanılan sürümü ve akış URL'si yayımlanmış varlık kimliği kaydedin.

Son olarak, kaydedilen ayarlarınızı e-posta ve arşiv parametreleri için Azure Media Services canlı amsstreaming@microsoft.com tüm kendi kendine doğrulama denetimleri başarılı bir bildirim olarak. Ayrıca, tüm izleme kayıtları için iletişim bilgilerinizi içerir. Bu işlem ile ilgili herhangi bir sorunuz Azure Media Services ekibiyle iletişime geçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Canlı akış ile Media Services v3](live-streaming-overview.md)
