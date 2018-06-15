---
title: Azure Media Services tarafından önerilen kodlayıcılar hakkında bilgi edinin | Microsoft Docs
description: Media services tarafından önerilen kodlayıcılar hakkında bilgi edinin
services: media-services
keywords: kodlama kodlayıcılar; ortam
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: d0c5536d2339470eac058250cc14e1f250b86d90
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33790275"
---
# <a name="recommended-on-premises-encoders"></a>Önerilen şirket içi kodlayıcılar
Canlı, Azure Media Services ile akış nasıl kanalınızı Giriş akışı almak istediğinizi belirtebilirsiniz. Bir şirket içi Kodlayıcı ile canlı bir kodlama kanalı kullanmayı seçerseniz, kodlayıcıyı çıkış olarak yüksek kaliteli tek bit hızında akışa itme. Bir geçiş kanalı ile bir şirket içi Kodlayıcı kullanmayı seçerseniz, tüm istenen çıkış nitelikleri ile çıkış olarak kodlayıcıyı Çoklu bit hızında akışa itme. Daha fazla bilgi için bkz: [şirket içi kodlayıcılarla canlı akış](media-services-live-streaming-with-onprem-encoders.md).

Azure Media Services RTMP çıktı olarak sahip gerçek zamanlı kodlayıcılar aşağıdaki birini kullanarak önerir:
- Adobe Flash medya gerçek zamanlı kodlayıcıya 3.2
- Haivision Makito X HEVC
- Telestream Wirecast 8.1
- Teradek dilim 756
- TriCaster 8000
- Tricaster Mini HD-4

Çoklu bit hızlı kesintisiz akış çıktı olarak sahip şu gerçek zamanlı kodlayıcılar birini kullanarak Azure Media Services önerir:
- Ateme TITAN Canlı
- Cisco dijital medya Kodlayıcısı 2200
- Elemental dinamik
- Envivio 4Caster C4 Gen III
- İletişim Selenio MCP3 düşünün
- Dinamik medya Excel kahramanı

> [!NOTE]
> Gerçek zamanlı Kodlayıcı bir geçiş kanalı üzerinden bir tek bit hızında akışa gönderebilir, ancak bit hızı Uyarlamalı akış için istemci için izin vermediği için bu yapılandırma önerilmez.

## <a name="how-to-become-an-on-prem-encoder-partner"></a>Bir şirket içi Kodlayıcı ortağı olmaya
Bir Azure Media Services şirket içi Kodlayıcı iş ortağı olarak, Kurumsal müşterilerin kodlayıcıyı önererek Media Services ürününüzü yükseltir. Bir şirket içi Kodlayıcı ortağı olmak için şirket içi kodlayıcıyı uyumluluk Media Services ile doğrulamanız gerekir. Bunu yapmak için aşağıdaki Doğrulamalar tamamlayın:

Kanal doğrulama geçirin
1. Oluşturma veya Azure Media Services hesabınızı ziyaret edin
2. Oluşturma ve başlatma bir **doğrudan** kanal
3. Çoklu bit hızında bir canlı akışı göndermeyi kodlayıcıyı yapılandırın.
4. Yayımlanan bir canlı olay oluşturma
5. Yaklaşık 10 dakika, gerçek zamanlı Kodlayıcı çalıştırın
6. Canlı olay Durdur
7. Varlık kimliği, akış URL'si Canlı Arşiv ayarları ve canlı kodlayıcıdan kullanılan sürümü için yayımlanan kayıt
8. Her örnek oluşturduktan sonra kanal durumu Sıfırla
9. Kodlayıcıyı (ile ve ad sinyal/resim yazıları/hızları kodlama farklı olmadan) tarafından desteklenen tüm yapılandırmalar için 8 aracılığıyla 3 adımları yineleyin

Canlı kodlama kanal doğrulama
1. Oluşturma veya Azure Media Services hesabınızı ziyaret edin
2. Oluşturma ve başlatma bir **kodlama canlı** kanal
3. Tek bit hızında bir canlı akışı göndermeyi kodlayıcıyı yapılandırın.
4. Yayımlanan bir canlı olay oluşturma
5. Yaklaşık 10 dakika, gerçek zamanlı Kodlayıcı çalıştırın
6. Canlı olay Durdur
7. Varlık kimliği, akış URL'si Canlı Arşiv ayarları ve canlı kodlayıcıdan kullanılan sürümü için yayımlanan kayıt
8. Her örnek oluşturduktan sonra kanal durumu Sıfırla
9. Kodlayıcıyı (ile ve ad sinyal/resim yazıları/hızları kodlama çeşitli olmadan) tarafından desteklenen tüm yapılandırmalar için 8 aracılığıyla 3 adımları yineleyin

Dayanıklılık doğrulama
1. Oluşturma veya Azure Media Services hesabınızı ziyaret edin
2. Oluşturma ve başlatma bir **doğrudan** kanal
3. Çoklu bit hızında bir canlı akışı göndermeyi kodlayıcıyı yapılandırın.
4. Yayımlanan bir canlı olay oluşturma
5. Bir hafta için veya daha uzun, gerçek zamanlı Kodlayıcı çalıştırın
6. Canlı olay Durdur
7. Varlık kimliği, akış URL'si Canlı Arşiv ayarları ve canlı kodlayıcıdan kullanılan sürümü için yayımlanan kayıt

Son olarak, kaydedilen ayarlarınızı göndermek ve canlı arşiv parametreleri Media Services için e-postayla gönderme tarafından amsstreaming@microsoft.com. Alındığında, Media Services, gerçek zamanlı Kodlayıcı örnekleri üzerinde doğrulama testleri gerçekleştirir. Bu işlem ile ilgili herhangi bir sorunuz ile Media Services başvurabilirsiniz.