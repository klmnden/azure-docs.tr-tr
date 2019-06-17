---
title: Azure Media Services tarafından önerilen kodlayıcılar hakkında bilgi edinin | Microsoft Docs
description: Media services tarafından önerilen kodlayıcılar hakkında bilgi edinin
services: media-services
keywords: encoding;encoders;media
author: dbgeorge
manager: johndeu
ms.author: johndeu
ms.date: 03/20/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 373ce1d10af87603b1bdd6339c94891187c35d8c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60332661"
---
# <a name="recommended-on-premises-encoders"></a>Önerilen şirket içi kodlayıcılar
Canlı, Azure Media Services ile akış, giriş akışını almak için kanal nasıl istediğinizi belirtebilirsiniz. Canlı kodlama kanal ile bir şirket içi Kodlayıcı kullanmayı seçerseniz, kodlayıcınız çıktı olarak bir yüksek kaliteli tek bit hızında akışa anında iletme. Bir geçiş kanalı üzerinden ile bir şirket içi Kodlayıcı kullanmayı seçerseniz, tüm istenen çıkış kalitelerini çıktı olarak kodlayıcınız Çoklu bit hızında akışa anında. Daha fazla bilgi için [şirket içi kodlayıcılarda Canlı üzerinde akış](media-services-live-streaming-with-onprem-encoders.md).

Azure Media Services, gerçek zamanlı kodlayıcılar RTMP çıktısı olarak olan aşağıdaki birini kullanarak önerir:
- Adobe Flash Media Live Encoder 3.2
- Haivision Makito X HEVC
- Haivision KB
- Telestream Wirecast 8.1+
- Telestream Wirecast S
- Teradek Slice 756
- TriCaster 8000
- Tricaster Mini HD-4
- OBS Studio
- VMIX
- xStream
- Switcher Studio (iOS)

Azure Media Services, Çoklu bit hızlı parçalanmış-MP4 (kesintisiz akış) çıktı olarak sahip şu gerçek zamanlı kodlayıcılar kullanarak önerir:
- Media Excel Hero Live ve Hero 4K (UHD/HEVC)
- Ateme TITAN Live
- Cisco Digital Media Encoder 2200
- Elemental Live
- Envivio 4Caster C4 Gen III
- Imagine Communications Selenio MCP3

> [!NOTE]
> Gerçek zamanlı bir kodlayıcı tek bit hızında akışa bir geçiş kanalı üzerinden gönderebilir, ancak bit hızı Uyarlamalı akış için bir istemci için izin vermediğinden bu yapılandırma önerilmez.

## <a name="how-to-become-an-on-premises-encoder-partner"></a>Nasıl bir şirket içi Kodlayıcı iş ortağı olun
Şirket içi Kodlayıcı ortak bir Azure Media Services Media Services, kurumsal müşterilere kodlayıcınız önererek ürününüzü yükseltir. Bir şirket içi Kodlayıcı iş ortağı olmak için Uyumluluk, şirket içi Kodlayıcı, Media Services ile doğrulamanız gerekir. Bunu yapmak için aşağıdaki Doğrulamalar tamamlayın:

Kanal doğrulama geçirin
1. Oluşturun veya Azure Media Services hesabınızı ziyaret edin
2. Oluşturma ve başlatma bir **doğrudan** kanal
3. Çoklu bit hızlı canlı akış anında yapmak için kodlayıcınızı yapılandırın.
4. Yayımlanan bir canlı etkinlik oluştur
5. Yaklaşık 10 dakika, gerçek zamanlı Kodlayıcı çalıştırın
6. Canlı etkinliği durdurmak
7. Oluşturma, bir akış uç noktasını başlatın, aşağıdaki gibi bir oynatıcı kullanın [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) , kayıttan yürütme için tüm kalite düzeylerine görünür hiçbir arızalardan bulunmasını arşivlenmiş varlığı izleyin (veya alternatif olarak izleyin ve önizleme URL ile doğrulamak için 6. adımdan önce Canlı oturumda)
8. Canlı Arşiv ve ayarları, Canlı kodlayıcıdan kullanılan sürümü ve akış URL'si yayımlanmış varlık kimliği kaydedin
9. Her bir örnek oluşturduktan sonra kanal durumu Sıfırla
10. (İle ve ad sinyal/açıklamalı alt yazılar/hızları kodlama farklı olmadan), kodlayıcı tarafından desteklenen tüm yapılandırmalar için 9 3 adımları yineleyin

Canlı kodlama kanal doğrulama
1. Oluşturun veya Azure Media Services hesabınızı ziyaret edin
2. Oluşturma ve başlatma bir **live encoding** kanal
3. Tek bit hızında bir canlı akışı anında yapmak için kodlayıcınızı yapılandırın.
4. Yayımlanan bir canlı etkinlik oluştur
5. Yaklaşık 10 dakika, gerçek zamanlı Kodlayıcı çalıştırın
6. Canlı etkinliği durdurmak
7. Oluşturma, bir akış uç noktasını başlatın, aşağıdaki gibi bir oynatıcı kullanın [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) , kayıttan yürütme için tüm kalite düzeylerine görünür hiçbir arızalardan bulunmasını arşivlenmiş varlığı izleyin (veya alternatif olarak izleyin ve önizleme URL ile doğrulamak için 6. adımdan önce Canlı oturumda)
8. Canlı Arşiv ve ayarları, Canlı kodlayıcıdan kullanılan sürümü ve akış URL'si yayımlanmış varlık kimliği kaydedin
9. Her bir örnek oluşturduktan sonra kanal durumu Sıfırla
10. (İle ve ad sinyal/açıklamalı alt yazılar/hızları kodlama çeşitli olmadan), kodlayıcı tarafından desteklenen tüm yapılandırmalar için 9 3 adımları yineleyin.

Dayanıklılık doğrulama
1. Oluşturun veya Azure Media Services hesabınızı ziyaret edin
2. Oluşturma ve başlatma bir **doğrudan** kanal
3. Çoklu bit hızlı canlı akış anında yapmak için kodlayıcınızı yapılandırın.
4. Yayımlanan bir canlı etkinlik oluştur
5. Bir hafta veya daha uzun, gerçek zamanlı Kodlayıcı çalıştırın
6. Gibi bir oynatıcı kullanın [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) Canlı süresi zaman (ya da arşivlenmiş varlığı), kayıttan yürütme sahip hiçbir görünür arızalardan emin olmak için akış izlemek için
7. Canlı etkinliği durdurmak
8. Canlı Arşiv ve ayarları, Canlı kodlayıcıdan kullanılan sürümü ve akış URL'si yayımlanmış varlık kimliği kaydedin

Son olarak, kaydedilen ayarlarınızı gönderin ve canlı arşiv parametreleri Media Services'e postası göndererek amsstreaming@microsoft.com. Media Services, alındığında, gerçek zamanlı Kodlayıcı örnekleri üzerinde doğrulama testleri gerçekleştirir. Media Services ile bu işlem ile ilgili herhangi bir sorunuz başvurabilirsiniz.
