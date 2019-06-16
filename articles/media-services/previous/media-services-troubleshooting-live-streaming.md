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
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: f502e3228274840d23b9f52512280fc0d9f0553b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60544703"
---
# <a name="troubleshooting-guide-for-live-streaming"></a>Canlı akış sorun giderme kılavuzu  

Bu makalede, bazı canlı akış sorunlarını giderme hakkında öneriler sağlar.

## <a name="issues-related-to-on-premises-encoders"></a>Şirket içi kodlayıcılar ilgili sorunlar
Bu bölüm, tekli bit hızı akışı, gerçek zamanlı kodlama için etkinleştirilmiş kanallar AMS göndermek için yapılandırılan şirket içi Kodlayıcıları ilgili sorunları gidermeye ilişkin öneriler sağlar.

### <a name="problem-would-like-to-see-logs"></a>Sorun: Günlükleri görmek istediğiniz
* **Olası sorun**: Yardımcı olabilecek Kodlayıcı günlükleri, hata ayıklaması bulunamıyor.
  
  * **Telestream Wirecast**: Genellikle, günlükleri C:\Users altında bulabilirsiniz\{kullanıcıadı} \AppData\Roaming\Wirecast\ 
  * **Elemental Live**: Yönetim Portalı'nda günlükler bağlantılar içeriyor bulabilirsiniz. Tıklayarak **istatistikleri**, ardından **günlükleri**. Üzerinde **günlük dosyalarını** sayfasında, günlükleri tüm Livestream öğelerin listesini göreceksiniz; geçerli oturumunuzu eşleşen birini seçin. 
  * **Flash Media Live Encoder**: Bulabilirsiniz **günlük dizini...**  giderek **kodlama günlük** sekmesi.

### <a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Sorun: Aşamalı bir akış çıktısı için seçeneği yoktur
* **Olası sorun**: Kullanılan kodlayıcıya otomatik olarak titreşimsiz görüntü değil. 
  
    **Sorun giderme adımları**: Bir kodlayıcı arabirimi Titreşim seçeneğini arayın. XML'deki titreşimli etkinleştirildikten sonra tekrar aşamalı çıkış ayarlarını kontrol edin. 

### <a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a>Sorun: Birkaç encoder çıkış ayarlar denenir ve hala bağlantı kurulamıyor.
* **Olası sorun**: Azure kodlama kanal düzgün sıfırlandı değil. 
  
    **Sorun giderme adımları**: Kodlayıcı artık AMS'ye gönderme emin olun, durdurun ve kanal sıfırlayın. Yeniden çalıştırmayı sonra yeni ayarlarla kodlayıcınız bağlanmayı deneyin. Sorun bu hala gidermezse, tamamen yeni bir kanal oluşturmaya çalışın, başarısız birkaç denemeden sonra bazen kanalları bozulabilir.  
* **Olası sorun**: GOP boyutu veya anahtar çerçeve ayarları en uygun değildir. 
  
    **Sorun giderme adımları**: Önerilen GOP boyutu veya ana kare aralığı iki saniyedir. Başkalarının saniye kullanırken bazı kodlayıcılar çerçeve sayısı bu ayarda hesaplayın. Örneğin: 30 fps alırken GOP boyutu 2 saniye eşdeğerdir 60 kare olur.  
* **Olası sorun**: Akış kapalı bağlantı noktaları engelliyor. 
  
    **Sorun giderme adımları**: RTMP akış, 1935 ve 1936 giden bağlantı noktalarının açık olduğunu doğrulamak için güvenlik duvarı ve/veya proxy ayarlarını kontrol edin. 

> [!NOTE]
> Sonra yine de başarılı bir şekilde akış uygulayamazsınız sorun giderme adımlarını izleyerek varsa, Azure portalını kullanarak bir destek bileti gönderin.
> 
> 

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

