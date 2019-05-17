---
title: Azure Media Services tarafından - önerilen kodlayıcılarda canlı akış | Microsoft Docs
description: Media Services tarafından önerilen canlı akış şirket içi Kodlayıcıları hakkında bilgi edinin
services: media-services
keywords: encoding;encoders;media
author: johndeu
manager: johndeu
ms.author: johndeu
ms.date: 01/17/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 50b22cefccf620d7b79202a5c432e2e6a4e3e3be
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/13/2019
ms.locfileid: "65550046"
---
# <a name="recommended-live-streaming-encoders"></a>Önerilen canlı akış kodlayıcılar

Azure Media Services, bir [canlı olay](https://docs.microsoft.com/rest/api/media/liveevents) (kanal), canlı akış içeriği işlemek için bir işlem hattı temsil eder. Canlı olay iki yoldan biriyle Canlı giriş akışları alır.

* Çoklu bit hızlı RTMP veya kesintisiz akış (parçalanmış MP4) akış canlı olay Media Services ile gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmemiş bir şirket içi Canlı Kodlayıcı gönderir. Alınan akışların herhangi başka bir işlemeye gerek kalmadan Canlı olayları geçirin. Bu yöntem çağrılır **doğrudan**. Gerçek zamanlı bir kodlayıcı, doğrudan bir kanala tek bit hızında akışa gönderebilirsiniz. Bit hızı Uyarlamalı akış için bir istemci için izin vermez çünkü bu yapılandırma önerilmemektedir.

  > [!NOTE]
  > Doğrudan geçiş yöntemini kullanmak canlı akış yapmanın en ekonomik yoludur.

* Bir şirket içi Canlı Kodlayıcı tek bit hızında akışa aşağıdaki biçimlerden birinde Media Services ile gerçek zamanlı kodlama gerçekleştirmek için etkinleştirildiği canlı olay gönderir: RTMP veya kesintisiz akış (parçalanmış MP4). Canlı olay gelen tek bit hızlı akışın Çoklu bit hızlı (Uyarlamalı) video akışına gerçek zamanlı kodlama gerçekleştirir.

Media Services ile gerçek zamanlı kodlama hakkında ayrıntılı bilgi için bkz. [canlı akış ile Media Services v3](live-streaming-overview.md).

## <a name="live-encoders-that-output-rtmp"></a>Gerçek zamanlı kodlayıcılar RTMP çıktısı

Media Services, aşağıdaki RTMP çıkışı sağlayan gerçek zamanlı kodlayıcılardan birinin kullanılmasını önerir. Desteklenen URL düzenler `rtmp://` veya `rtmps://`.

> [!NOTE]
> RTMP üzerinden akış yaparken güvenlik duvarı ve/veya ara sunucu ayarlarını kontrol ederek 1935 ve 1936 numaralı giden TCP bağlantı noktalarının açık olduğundan emin olun.

- Adobe Flash Media Live Encoder 3.2
- Haivision KB
- Haivision Makito X HEVC
- OBS Studio
- Switcher Studio (iOS)
- Telestream Wirecast 8.1+
- Telestream Wirecast S
- Teradek Slice 756
- TriCaster 8000
- Tricaster Mini HD-4
- VMIX
- xStream

## <a name="live-encoders-that-output-fragmented-mp4"></a>Parçalanmış MP4 çıkış gerçek zamanlı kodlayıcılar

Media Services, Çoklu bit hızlı kesintisiz akış (parçalanmış MP4) çıktı olarak sahip şu gerçek zamanlı kodlayıcılar kullanarak önerir. Desteklenen URL düzenler `http://` veya `https://`.

- Ateme TITAN Live
- Cisco Digital Media Encoder 2200
- Elemental Live
- Envivio 4Caster C4 Gen III
- Imagine Communications Selenio MCP3
- Media Excel Hero Live ve Hero 4K (UHD/HEVC)

> [!TIP]
>  Canlı etkinlikler (örneğin, İngilizce bir ses kaydı ve İspanyolca bir ses kaydı) birden çok dilde akışa, bu canlı akış için doğrudan bir canlı olay göndermek üzere yapılandırılmış ortam Excel gerçek zamanlı Kodlayıcı ile gerçekleştirebilirsiniz.

## <a name="configuring-on-premises-live-encoder-settings"></a>Yapılandırma gerçek zamanlı Kodlayıcı ayarları şirket

Canlı etkinlik türünüz için geçerli ayarları hakkında daha fazla bilgi için bkz. [canlı olay türlerini karşılaştırma](live-event-types-comparison.md).

### <a name="playback-requirements"></a>Kayıttan yürütme gereksinimleri

İçerik yürütmek için hem bir ses ve video akışı mevcut olması gerekir. Kayıttan yürütme yalnızca video akışının desteklenmiyor.

### <a name="configuration-tips"></a>Yapılandırma ipuçları

- Mümkün olduğunda, bir sabit internet bağlantısı kullanın.
- Bant genişliği gereksinimlerini, ikili akış bit hızlarında zaman belirlerken. Zorunlu olsa da, Ağ Tıkanıklığı etkisini azaltmak için bu basit kuralı yardımcı olur.
- Yazılım tabanlı kodlayıcılar kullanırken, gereksiz tüm programları kapatın.
- Gönderme başlatıldıktan sonra Kodlayıcı yapılandırmanızı değiştirme olay üzerinde olumsuz etkileri vardır. Yapılandırma değişiklikleri kararsız duruma gelmesine neden olabilir. 
- Kendinize, olay ayarlamak için bol zaman verin emin olun. Büyük ölçekli olay için bir saat önce olay Kur'u başlatma öneririz.

## <a name="becoming-an-on-premises-encoder-partner"></a>Bir şirket içi Kodlayıcı iş ortağı olmak

Bir Azure Media Services şirket içi Kodlayıcı iş ortağı olarak, Media Services, kurumsal müşterilere kodlayıcınız önererek ürününüzü yükseltir. Bir şirket içi Kodlayıcı iş ortağı için Uyumluluk, şirket içi Kodlayıcı, Media Services ile doğrulamanız gerekir. Bunu yapmak için aşağıdaki Doğrulamalar tamamlayın.

### <a name="pass-through-live-event-verification"></a>Doğrudan canlı olay doğrulama

1. Media Services hesabınızı doğrulayın **akış uç noktası** çalışıyor. 
2. Oluşturma ve başlatma **doğrudan** canlı olay. <br/> Daha fazla bilgi için [canlı olay durumları ve faturalandırma](live-event-states-billing.md).
3. Alma URL'leri alma ve, şirket içi Kodlayıcı, Çoklu bit hızlı canlı akış medya hizmetlerine göndermek için URL'sini kullanmak üzere yapılandırın.
4. Önizleme URL'sini ve aslında kodlayıcıdan giriş alındığını doğrulamak için kullanın.
5. Yeni bir **varlık** nesne.
6. Oluşturma bir **Canlı çıkış** oluşturduğunuz varlık adını kullanın.
7. Oluşturma bir **akış Bulucu** yerleşik ile **akış ilke** türleri.
8. Yolları listesini **akış Bulucu** kullanılacak URL'leri geri dönebilirsiniz.
9. Ana bilgisayar adını alın **akış uç noktası** gelen akışla aktarmak istediğiniz.
10. 8. adımdaki URL'yi, ana bilgisayar adı tam URL'sini almak için 9. adımda birleştirin.
11. Yaklaşık 10 dakika, gerçek zamanlı Kodlayıcı çalıştırın.
12. Canlı etkinliği durdurmak. 
13. Gibi bir oynatıcı kullanın [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) , kayıttan yürütme yok görünür arızalardan tüm kalite düzeylerine sahip olduğundan emin olmak arşivlenmiş varlığı izlemek için. Veya, izleyin ve canlı oturumda Önizleme URL ile doğrulayın.
14. Canlı Arşiv ve ayarları, Canlı kodlayıcıdan kullanılan sürümü ve yayımlanan akış URL'si varlık kimliği kaydedin.
15. Her bir örnek oluşturduktan sonra canlı olay durumunu sıfırlayın.
16. 5. adım-15 (ile ve ad sinyal, açıklamalı alt yazıları veya farklı bir kodlama hızlarını içermeyen), kodlayıcı tarafından desteklenen tüm yapılandırmalar için yineleyin.

### <a name="live-encoding-live-event-verification"></a>Canlı kodlama canlı olay doğrulama

1. Media Services hesabınızı doğrulayın **akış uç noktası** çalışıyor. 
2. Oluşturma ve başlatma **live encoding** canlı olay. <br/> Daha fazla bilgi için [canlı olay durumları ve faturalandırma](live-event-states-billing.md).
3. Alma URL'leri alma ve tek bit hızında bir canlı akışı Media Services'a iletin yapmak için kodlayıcınızı yapılandırın.
4. Önizleme URL'sini ve aslında kodlayıcıdan giriş alındığını doğrulamak için kullanın.
5. Yeni bir **varlık** nesne.
6. Oluşturma bir **Canlı çıkış** oluşturduğunuz varlık adını kullanın.
7. Oluşturma bir **akış Bulucu** yerleşik ile **akış ilke** türleri.
8. Yolları listesini **akış Bulucu** kullanılacak URL'leri geri dönebilirsiniz.
9. Ana bilgisayar adını alın **akış uç noktası** gelen akışla aktarmak istediğiniz.
10. 8. adımdaki URL'yi, ana bilgisayar adı tam URL'sini almak için 9. adımda birleştirin.
11. Yaklaşık 10 dakika, gerçek zamanlı Kodlayıcı çalıştırın.
12. Canlı etkinliği durdurmak.
13. Gibi bir oynatıcı kullanın [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) , kayıttan yürütme için tüm kalite düzeylerine görünür hiçbir arızalardan bulunmasını arşivlenmiş varlığı izlemek için. Veya, izleyin ve canlı oturumda Önizleme URL ile doğrulayın.
14. Canlı Arşiv ve ayarları, Canlı kodlayıcıdan kullanılan sürümü ve yayımlanan akış URL'si varlık kimliği kaydedin.
15. Her bir örnek oluşturduktan sonra canlı olay durumunu sıfırlayın.
16. 5. adım-15 (ile ve ad sinyal, açıklamalı alt yazıları veya farklı bir kodlama hızlarını içermeyen), kodlayıcı tarafından desteklenen tüm yapılandırmalar için yineleyin.

### <a name="longevity-verification"></a>Dayanıklılık doğrulama

Aynı adımları olarak [doğrudan canlı olay doğrulama](#pass-through-live-event-verification) dışında 11. adım. <br/>10 dakika yerine, bir hafta veya daha uzun, gerçek zamanlı Kodlayıcı çalıştırın. Gibi bir oynatıcı kullanın [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) Canlı süresi zaman (veya bir arşivlenmiş varlığı), kayıttan yürütme sahip hiçbir görünür arızalardan emin olmak için akış izlemek için.

### <a name="email-your-recorded-settings"></a>E-posta, kaydedilmiş ayarları

Son olarak, kaydedilen ayarlarınızı e-posta ve arşiv parametreleri için Azure Media Services canlı amsstreaming@microsoft.com tüm kendi kendine doğrulama denetimleri başarılı bir bildirim olarak. Ayrıca, tüm izlemeler için iletişim bilgilerinizi ekleyin. Bu işlem hakkında herhangi bir sorunuz Azure Media Services ekibiyle iletişime geçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Canlı akış ile Media Services v3](live-streaming-overview.md)
