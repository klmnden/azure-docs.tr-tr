---
title: Canlı akış sorun giderme kılavuzu | Microsoft Docs
description: Bu konu, canlı akış sorunlarını giderme hakkında öneriler sağlar.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: e6b135e14f06ecf4edfbb97913c411f55711854a
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49351461"
---
# <a name="troubleshooting-guide-for-live-streaming"></a>Canlı akış sorun giderme kılavuzu
Bu makalede, bazı canlı akış sorunlarını giderme hakkında öneriler sağlar.

## <a name="issues-related-to-on-premises-encoders"></a>Şirket içi kodlayıcılar ilgili sorunlar
Bu bölüm, tekli bit hızı akışı, gerçek zamanlı kodlama için etkinleştirilmiş kanallar AMS göndermek için yapılandırılan şirket içi Kodlayıcıları ilgili sorunları gidermeye ilişkin öneriler sağlar.

### <a name="problem-would-like-to-see-logs"></a>Sorun: günlüklerini görmek istediğiniz
* **Olası sorun**: hata ayıklaması Kodlayıcı günlüklerini bulma yardımcı olabilecek olamaz.
  
  * **Telestream Wirecast**: genellikle günlükleri C:\Users altında bulabilirsiniz\{kullanıcıadı} \AppData\Roaming\Wirecast\ 
  * **Elemental Live**: Yönetim Portalı'nda günlükler bağlantılar içeriyor bulabilirsiniz. Tıklayarak **istatistikleri**, ardından **günlükleri**. Üzerinde **günlük dosyalarını** sayfasında, günlükleri tüm Livestream öğelerin listesini göreceksiniz; geçerli oturumunuzu eşleşen birini seçin. 
  * **Flash Media Live Encoder**: bulabilirsiniz **günlük dizini...**  giderek **kodlama günlük** sekmesi.

### <a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Sorun: Bir aşamalı akışı çıktısı için seçeneği yoktur
* **Olası sorun**: kullanılan kodlayıcıya otomatik olarak titreşimsiz görüntü değil. 
  
    **Sorun giderme adımları**: bir kodlayıcı arabirimi Titreşim seçeneğini arayın. XML'deki titreşimli etkinleştirildikten sonra tekrar aşamalı çıkış ayarlarını kontrol edin. 

### <a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a>Sorun: birkaç encoder çıkış ayarlar denenir ve hala bağlantı kurulamıyor.
* **Olası sorun**: Azure kodlama kanal değil düzgün sıfırlandı. 
  
    **Sorun giderme adımları**: emin Kodlayıcı artık gönderme AMS'ye durdurun ve kanal sıfırlayın. Yeniden çalıştırmayı sonra yeni ayarlarla kodlayıcınız bağlanmayı deneyin. Sorun bu hala gidermezse, tamamen yeni bir kanal oluşturmaya çalışın, başarısız birkaç denemeden sonra bazen kanalları bozulabilir.  
* **Olası sorun**: GOP boyutu veya anahtar çerçeve ayarları en uygun değildir. 
  
    **Sorun giderme adımları**: GOP önerilen boyutu veya ana kare aralığıdır iki saniye. Başkalarının saniye kullanırken bazı kodlayıcılar çerçeve sayısı bu ayarda hesaplayın. Örneğin: 30 fps alırken GOP boyutu 60 çerçeveler için 2 saniye eşdeğerdir olacaktır.  
* **Olası sorun**: kapalı bağlantı noktaları akış engelleme. 
  
    **Sorun giderme adımları**: RTMP akış, 1935 ve 1936 giden bağlantı noktalarının açık olduğunu doğrulamak için güvenlik duvarı ve/veya proxy ayarlarını kontrol edin. 

> [!NOTE]
> Sonra yine de başarılı bir şekilde akış uygulayamazsınız sorun giderme adımlarını izleyerek varsa, Azure portalını kullanarak bir destek bileti gönderin.
> 
> 

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

