---
title: Canlı akış için sorun giderme kılavuzu | Microsoft Docs
description: Bu konuda canlı akış sorunlarını giderme hakkında öneriler sağlar.
services: media-services
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.assetid: 3a7f6c1d-ce57-4fa4-a7a6-edb526b3ffbf
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 84e3e9fc18671d7199eeaf638377a6681cf09fb4
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33940933"
---
# <a name="troubleshooting-guide-for-live-streaming"></a>Canlı akış sorun giderme kılavuzu
Bu makalede, bazı canlı akış sorunlarını giderme hakkında öneriler sağlar.

## <a name="issues-related-to-on-premises-encoders"></a>Şirket içi kodlayıcılardan ilgili sorunlar
Bu bölüm, gerçek zamanlı kodlama için etkinleştirilmiş AMS kanallar tek bit hızlı akış göndermek için yapılandırılmış olan şirket içi kodlayıcılardan ilgili sorunları giderme konusunda öneriler sağlar.

### <a name="problem-would-like-to-see-logs"></a>Sorun: günlükleri görmek istediğiniz
* **Olası sorun**: Kodlayıcı günlüklerini bulma sorunları hata ayıklamaya yardımcı olabilecek olamaz.
  
  * **Telestream Wirecast**: logs C:\Users altında genellikle bulabilirsiniz\{username} \AppData\Roaming\Wirecast\ 
  * **Elemental canlı**: sahip bağlantılar günlükleri için yönetim portalında bulabilirsiniz. Tıklayın **istatistiği**, ardından **günlükleri**. Üzerinde **günlük dosyalarını** sayfasında, günlükleri tüm LiveEvent öğelerin bir listesini görürsünüz; geçerli oturumunuz eşleşen birini seçin. 
  * **Canlı medya Kodlayıcı flash**: bulabileceğiniz **günlük dizini...**  giderek **kodlama günlük** sekmesi.

### <a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Sorun: Aşamalı bir akış çıktısı için bir seçenek yoktur
* **Olası sorun**: kullanılan kodlayıcı otomatik olarak titreşimsiz görüntü değil. 
  
    **Sorun giderme adımları**: Titreşim seçeneği Kodlayıcı arabiriminden arayın. XML'deki tarama etkinleştirildikten sonra yeniden aşamalı çıkış ayarlarını denetleyin. 

### <a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a>Sorun: birkaç encoder çıkış ayarları denedi ve hala bağlanılamıyor.
* **Olası sorun**: Azure kodlama kanal değil düzgün sıfırlandı. 
  
    **Sorun giderme adımları**: emin olun Kodlayıcı artık gönderilmesi için AMS, durdurmak ve kanal sıfırlayın. Yeniden çalıştırmayı sonra yeni ayarlarla kodlayıcıyı bağlanmayı deneyin. Sorun bu hala gidermezse, tamamen yeni bir kanal oluşturma deneyin, başarısız birkaç denemeden sonra bazen kanalları bozulabilir.  
* **Olası sorun**: GOP boyutu veya anahtar çerçeve ayarları en uygun değildir. 
  
    **Sorun giderme adımları**: önerilen GOP boyutu veya ana kare aralığıdır iki saniye. Başkalarının saniye kullanırken bazı kodlayıcılar çerçeve sayısı bu ayarda hesaplayın. Örneğin: 30 fps alırken GOP boyutu 60 çerçeve 2 saniyeye eşdeğerdir olacaktır.  
* **Olası sorun**: kapalı bağlantı noktalarını akış engelleme. 
  
    **Sorun giderme adımları**: RTMP akış, 1935 ve 1936 giden bağlantı noktalarının açık olduğunu onaylamak için güvenlik duvarı ve/veya proxy ayarlarını denetleyin. 

> [!NOTE]
> Aşağıdaki ise, hala başarıyla akış uygulayamazsınız sorun giderme adımları sonra Azure portalını kullanarak bir destek bileti gönderin.
> 
> 

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

