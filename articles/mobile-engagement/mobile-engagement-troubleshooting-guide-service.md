---
title: "Sorun giderme kılavuzu - hizmet azure Mobile Engagement"
description: "Azure Mobile Engagement için kılavuzları sorunlarını giderme"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8b4275da-c0b4-4690-824a-48e9d7a1fc6e
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: f13fd0540b783120014b3a8d4e41f78808c7fade
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-guide-for-service-issues"></a>Hizmet sorunları için sorun giderme kılavuzu
Azure Mobile Engagement nasıl çalıştığı ile karşılaşabileceğiniz olası sorunlar şunlardır:

## <a name="service-outages"></a>Hizmet kesintisi
### <a name="issue"></a>Sorun
* Azure Mobile Engagement hizmet kesintileri tarafından nedeni görünmesini sorunları.

### <a name="causes"></a>Neden olur.
* Azure Mobile Engagement hizmet kesintileri tarafından nedeni görünmesini sorunları birkaç farklı nedenlerden kaynaklanabilir:
  * İlk olarak sistemle ilgili tüm Azure Mobile Engagement görünür yalıtılmış sorunları
  * Bilinen sorunlar sunucu kesintileri (her zaman sunucu durumunda gösterir) nedeni:
  * Gecikmeler, hedefleme hataları, rozet güncelleştirmesini sorunları, toplama, çalışma anında durakları istatistikleri durdurma zamanlama, API'nin Dur çalışma, yeni uygulamalar veya kullanıcılar oluşturulamaz, DNS hataları ve zaman aşımı hataları UI, API veya uygulamaları bir aygıtta.
  * Bulut bağımlılık kesintileri [Azure hizmet durumu](http://status.azure.com/)
  * Bildirim Hizmetleri (PNS) bağımlılık kesintileri bildirme
  * Uygulama mağazası kesintileri

1) Sorun sistemle ilgili olup olmadığını sınamak için aynı işlevin farklı bir test edebilirsiniz

* Azure Mobile Engagement uygulama tümleşik
* Test cihazı
* Test aygıt işletim sistemi sürümü
* Kampanya
* Yönetici kullanıcı hesabı
* Tarayıcı (IE, Firefox, Chrome, vb.)
* Bilgisayar

2) Sorun kullanıcı Arabirimi veya API'nin yalnızca etkilerse test etmek için:

* Azure Mobile Engagement UI ve Azure Mobile Engagement API'nin aynı işlevin sınayın.

3) Cep telefonu ağınızla sorunun olup olmadığını test etmek için:

* WIFI üzerinden ve 3 G cep telefonu ağınız bağlıyken internet bağlıyken sınayın.
* Güvenlik duvarını herhangi bir Azure Mobile Engagement IP adreslerini veya bağlantı noktalarını engellemediğinden emin olun.

4) Sorunun cihazınızla olup olmadığını test etmek için:

* Cihazınızı Azure Mobile Engagement için başka bir Azure Mobile Engagement tümleşik uygulamayla bağlanabiliyor ise test edin.
* Azure Mobile Engagement Arabiriminde görülebilir telefonunuzdan gelen olayları, işler ve kilitlenme oluşturabilir sınayın. 
* Kendi cihaz Kimliği'nde tabanlı aygıtınıza Azure Mobile Engagement Arabiriminden anında iletme bildirimleri gönderebilir, test etme 

5) Uygulamanızla sorunun olup olmadığını test etmek için:

* Yükleyin ve yerine bir öykünücü fiziksel CİHAZDAN uygulamanızı test edin:

6) İşletim sistemi yükseltmeleri son kullanıcıya çözümlemek için SDK yükseltme gerektiren cihazlar ile sorunun olup olmadığını test etmek için:

* İşletim sisteminin farklı sürümlerini farklı cihazlarda uygulamanızı test edin.
* SDK'ın en son sürümünü kullandığınızdan emin olun.

## <a name="connectivity-and-incorrect-information-issues"></a>Bağlantısı ve yanlış bilgi sorunları
### <a name="issue"></a>Sorun
* Azure Mobile Engagement UI günlüğü sorunları.
* Azure Mobile Engagement API'nin hatalarla bağlantı.
* Uygulama bilgileri etiketleri Device API'si aracılığıyla karşıya yükleme sorunları.
* Sorunları günlüklerini veya dışarı aktarılan verileri Azure Mobile Engagement'tan yükleniyor.
* Azure Mobile Engagement Arabiriminde gösterilecek yanlış bilgileri.
* Azure Mobile Engagement günlüklerde gösterilen yanlış bilgileri.

### <a name="causes"></a>Neden olur.
* Kullanıcı hesabınızın görevi gerçekleştirmek için yeterli izinlere sahip olduğunu doğrulayın.
* Sorun bir bilgisayar veya yerel ağınızdaki yalıtılmış olmadığını onaylayın.
* Azure Mobile Engagement hizmet sahip olmayan kesintileri bildirilen onaylayın.
* Uygulama bilgileri etiketi dosyalarınızı bu kuralların hepsi izleyin onaylayın:
  * Yalnızca UTF8 karakter kümesi (ANSI karakter kümesini desteklenmiyor) kullanın.
  * Virgül "," ayırıcı karakter olarak (bir hizmet virgül .csv Ayırıcı karakterini değiştirmek için istemek için açabilirsiniz "," noktalı virgül gibi başka bir karakter için ";").
  * Tüm küçük Boole değerleri için "true" ve "false" kullanın.
  * 35 MB maksimum dosya boyutundan daha küçük bir dosya kullanın.

