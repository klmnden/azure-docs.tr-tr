---
title: Azure Media Services Livestream türleri | Microsoft Docs
description: Bu makale Livestream türlerini karşılaştıran ayrıntılı bir tablo.
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
ms.date: 06/13/2019
ms.author: juliako
ms.openlocfilehash: 93f01513841d1174fea796f1615ab05df0a41af4
ms.sourcegitcommit: 6e6813f8e5fa1f6f4661a640a49dc4c864f8a6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67150376"
---
# <a name="live-event-types-comparison"></a>Canlı olay türlerini karşılaştırma

Azure Media Services, bir [canlı olay](https://docs.microsoft.com/rest/api/media/liveevents) iki türden biri olabilir: live encoding ve doğrudan. 

## <a name="types-comparison"></a>Tür karşılaştırması 

Aşağıdaki tablo, canlı olay türlerinin özellikleri karşılaştırılır. Oluşturma işlemi sırasında kullanarak türleri Ayarla [LiveEventEncodingType](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventencodingtype):

* **LiveEventEncodingType.None** -şirket içi Canlı Kodlayıcı, bir Çoklu bit hızlı akış gönderir. Alınan akışların herhangi başka bir işlemeye gerek kalmadan canlı olay geçirir. 
* **LiveEventEncodingType.Standard** - bir şirket içinde gerçek zamanlı Kodlayıcı, Media Services ve canlı olay bir tek bit hızlı akışın Çoklu bit hızı akışları oluşturur gönderir. Katkı akış 720 p veya daha yüksek çözünürlükte ise **Default720p** hazır kodlama (ayrıntıları, makalenin sonraki bölümlerinde izleyin) 6 çözümleme/hızı eşleri grubudur.
* **LiveEventEncodingType.Premium1080p** - bir şirket içinde gerçek zamanlı Kodlayıcı, Media Services ve canlı olay bir tek bit hızlı akışın Çoklu bit hızı akışları oluşturur gönderir. Default1080p hazır çözüm/hızı çiftleri (ayrıntıları makalenin sonraki bölümlerinde izleyin) çıkış kümesini belirtir. 

| Özellik | Doğrudan Canlı etkinlik | Standart veya Premium1080p Canlı etkinlik |
| --- | --- | --- |
| Tekli bit hızı girişi Çoklu bit hızlarında buluta halinde kodlanır |Hayır |Evet |
| Katkı için en yüksek ekran çözünürlüğü akışı |4K (4096 x 2160 en 60 kare/sn) |1080p (1920 x 1088 en 30 kare/sn)|
| Önerilen en yüksek katmanlarında akışı katkı|12 adede kadar|Bir ses|
| En yüksek katmanlarında çıkış| Aynı giriş|En fazla 6 (sistem önayarlarını aşağıya bakın)|
| Akış katkı en fazla toplam bant genişliği|60 Mbps|Yok|
| Tek bir katman katkısı için maksimum hızı |20 Mbps|20 Mbps|
| Ses parçalarını birden çok dil desteği|Evet|Hayır|
| Desteklenen giriş video codec bileşenleri |H.264/AVC ve H.265/HEVC|H.264/AVC|
| Desteklenen çıkış video codec bileşenleri|Aynı giriş|H.264/AVC|
| Desteklenen video bit derinliği, giriş ve çıkış|10-bit dahil olmak üzere HDR 10/HLG kadar|8-bit|
| Desteklenen giriş ses codec bileşenleri|AAC-LC, HE-AAC v1, HE-AAC v2|AAC-LC, HE-AAC v1, HE-AAC v2|
| Desteklenen çıkış ses codec bileşenleri|Aynı giriş|AAC-LC|
| Çıkış video en yüksek ekran çözünürlüğü|Aynı giriş|Standart - 720p Premium1080p - 1080p|
| Giriş video en yüksek kare hızı|60 kare/saniye|Standart veya Premium1080p - 30 kare/saniye|
| Giriş protokolleri|RTMP, parçalanmış-MP4 (kesintisiz akış)|RTMP, parçalanmış-MP4 (kesintisiz akış)|
| Fiyat|Bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/media-services/) ve "Canlı Video" sekmesine tıklayın|Bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/media-services/) ve "Canlı Video" sekmesine tıklayın|
| Maksimum Çalıştırma süresi| 24 saat x 365 gün Canlı doğrusal | En fazla 24 saat|
| Veri özelliği aracılığıyla katıştırılmış CEA 608/708 geçirmek için açıklamalı alt yazılar|Evet|Evet|
| Maskeleme görüntülerini ekleme desteği|Hayır|Hayır|
| API aracılığıyla sinyal ad desteği| Hayır|Hayır|
| Sinyal SCTE-35 bant içi iletiler ad desteği|Evet|Evet|
| Akış katkısı kısa takılması kurtarma olanağı|Evet|Kısmi|
| Tekdüzen olmayan giriş GOPs desteği|Evet|Hayır-giriş GOP süresi sabit olmalıdır|
| Değişken kare hızı girişi için destek|Evet|Yok – giriş kare hızı düzeltilmesi gerekir. Küçük farklılıklar, örneğin, yüksek bir hareket sahneler sırasında izin verilir. Ancak katkı akış kare hızı bırakılamıyor (örneğin, 15'e kare/saniye).|
| Otomatik akışı kapatmaya Canlı zaman giriş olayının kaybolur|Hayır|12 çalışan hiçbir LiveOutput ise saat sonra|

## <a name="system-presets"></a>Sistem önayarlarını

Çözünürlüklerine ve bit hızlarına dönüştürme gerçek zamanlı Kodlayıcı çıkışı bulunan tarafından belirlenen [presetName](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventencoding). Kullanılıyorsa bir **standart** Canlı Kodlayıcı (LiveEventEncodingType.Standard) sonra *Default720p* hazır aşağıda açıklanan 6 çözümleme/hızı çiftleri kümesini belirtir. Aksi takdirde kullanıyorsanız bir **Premium1080p** Canlı Kodlayıcı (LiveEventEncodingType.Premium1080p) sonra *Default1080p* hazır çözüm/hızı çiftleri çıkış kümesini belirtir.

> [!NOTE]
> Standart gerçek zamanlı kodlama için Kurulum olmuştur - bir hata alırsınız, canlı bir olay için önceden Default1080p uygulanamıyor. Önceden bir Premium1080p gerçek zamanlı Kodlayıcı Default720p uygulamak çalışırsanız da bir hata alırsınız.

### <a name="output-video-streams-for-default720p"></a>Video Çıkış akışları Default720p için

Katkı akış 720 p veya daha yüksek çözünürlükte ise **Default720p** hazır aşağıdaki 6 katmanları akışa kodlayın. Aşağıdaki tabloda, bit hızı KB/sn, MaxFPS izin verilen en yüksek kare hızı temsil eder (içinde kare/saniye), profil kullanılan H.264 profilini temsil eder.

| Bit hızı | Genişlik | Yükseklik | MaxFPS | Profil |
| --- | --- | --- | --- | --- |
| 3500 |1280 |720 |30 |Yüksek |
| 2200 |960 |540 |30 |Yüksek |
| 1350 |704 |396 |30 |Yüksek |
| 850 |512 |288 |30 |Yüksek |
| 550 |384 |216 |30 |Yüksek |
| 200 |340 |192 |30 |Yüksek |

> [!NOTE]
> Canlı kodlama Önayarı özelleştirmek istiyorsanız lütfen Azure portalı üzerinden bir destek bileti açın. İstediğiniz çözünürlüklerin ve bit hızlarının yer aldığı tabloyu paylaşmanız gerekir. 720p çözünürlükte yalnızca bir katman ve toplamda en fazla 6 katman bulunduğundan emin olun. Ayrıca standart bir gerçek zamanlı Kodlayıcı için bir ön ayarı isteyen belirtin.
> Zaman içinde belirli değerleri çözünürlük ve bit hızlarına dönüştürme yapılabileceğine

### <a name="output-video-streams-for-default1080p"></a>Video Çıkış akışları Default1080p için

Katkı akış 1080 p çözümlenmesi ise **Default1080p** hazır aşağıdaki 6 katmanları akışa kodlayın.

| Bit hızı | Genişlik | Yükseklik | MaxFPS | Profil |
| --- | --- | --- | --- | --- |
| 5500 |1920 |1080 |30 |Yüksek |
| 3000 |1280 |720 |30 |Yüksek |
| 1600 |960 |540 |30 |Yüksek |
| 800 |640 |360 |30 |Yüksek |
| 400 |480 |270 |30 |Yüksek |
| 200 |320 |180 |30 |Yüksek |

> [!NOTE]
> Canlı kodlama Önayarı özelleştirmek istiyorsanız lütfen Azure portalı üzerinden bir destek bileti açın. İstediğiniz çözünürlüklerin ve bit hızlarının yer aldığı tabloyu paylaşmanız gerekir. Yalnızca bir katmanında 1080 p ve en fazla 6 katmanları olduğundan emin olun. Ayrıca bir Premium1080p gerçek zamanlı Kodlayıcı için bir ön ayarı isteyen belirtin.
> Belirli değerlerin çözünürlük ve bit hızlarına dönüştürme zamanla ayarlanmış.

### <a name="output-audio-stream-for-default720p-and-default1080p"></a>Çıkış ses Stream Default720p ve Default1080p

Her ikisi için de *Default720p* ve *Default1080p* ses hazır kodlanmış stereo AAC-LC, 128 Kb/sn için. Örnekleme hızını, ses kaydının akışı katkısı izler.

## <a name="implicit-properties-of-the-live-encoder"></a>Gerçek zamanlı Kodlayıcı örtük özellikleri

Önceki bölümde açıkça denetlenebilir hazır aracılığıyla - katmanlar, çözünürlük ve bit hızlarına dönüştürme sayısı gibi gerçek zamanlı Kodlayıcı özelliklerini açıklar. Bu bölümde, örtük özelliklerini açıklar.

### <a name="group-of-pictures-gop-duration"></a>Grubu (GOP) resimleri süresi

Gerçek zamanlı Kodlayıcı izleyen [GOP](https://en.wikipedia.org/wiki/Group_of_pictures) yapısı akışı - katkı çıkış katmanları için aynı GOP süresine sahip olacak anlamına gelir. Bu nedenle, GOP süresi (genellikle 2 saniye) düzeltmiştir bir katkı akış oluşturmak için şirket içi Kodlayıcı yapılandırmanız önerilir. Bu, giden HLS ve MPEG DASH akışları hizmetinden de sabit olduğunu GOP sürelerini garanti eder. Çoğu cihazlar tarafından kabul edileceği GOP süreleri Small çeşitleri olasıdır.

### <a name="frame-rate"></a>Kare hızı

Gerçek zamanlı Kodlayıcı, tek tek video akışı - katkı çıkış katmanları aynı süreleri çerçevelerle olacağı anlamına gelir çerçevelerde süreleri de izler. Bu nedenle, kare hızı giderilen bir katkı akış oluşturmak için şirket içi Kodlayıcı yapılandırmanız önerilir (en fazla 30 kare/saniye). Bu, giden HLS ve MPEG DASH akışları hizmetinden de sabit, kare hızları sürelerini garanti eder. Kare hızları küçük farklılıklar nedeniyle çoğu cihazlar tarafından izin, ancak gerçek zamanlı Kodlayıcı doğru yürütülecek bir çıktı oluşturur garantisi yoktur. Şirket içi gerçek zamanlı Kodlayıcı çerçeveleri (örn. engelliyor olabilir değil Düşük pil koşullarda) veya herhangi bir şekilde kare hızı Çeşitleme uygulanıyor.

### <a name="resolution-of-contribution-feed-and-output-layers"></a>Katkı çözümlenmesi akışı ve Katmanlar çıkış

Gerçek zamanlı Kodlayıcı upconverting önlemek için yapılandırılmış akış katkı. Sonuç olarak çıkış katmanları en yüksek çözünürlüğünü, akış katkı aşmaz.

Örneğin, bir katkı için 720 p akışı gönderirseniz Canlı kodlama Default1080p için yapılandırılmış bir canlı etkinlik, çıkış yalnızca 3 MB/sn, en aşağı 200 hızı 1080 p giderek 720 p ile başlayarak 5 katmanları sahip olur. Veya 360 p akışı bir katkı gönderirseniz Live standart için yapılandırılmış bir olaya live encoding, çıktı 3 katmanları (çözünürlükte 288 p, 216 p ve 192 p) içerir. Standart Canlı kodlayıcıya oluşan, örneğin 160 x 90 piksel katkı akışını gönderirseniz bozuk durumda bir katmanında aynı bit hızı akışı katkı, 160 x 90 çözünürlükte çıkış içerir.

### <a name="bitrate-of-contribution-feed-and-output-layers"></a>Katkı, bit hızı akışı ve Katmanlar çıkış

Gerçek zamanlı Kodlayıcı, bit hızı akışı katkı bağımsız olarak hazır bit hızı ayarlarında uymanız yapılandırılır. Sonuç olarak çıkış katmanların bit hızı akışı katkı, aşabilir. 1 MB/sn hızında 720 p çözünürlükte akışı bir katkısı gönderirseniz, örneğin, çıkış katmanları aynı kalacak [tablo](live-event-types-comparison.md#output-video-streams-for-default720p) yukarıda.

## <a name="next-steps"></a>Sonraki adımlar

[Canlı akış genel bakış](live-streaming-overview.md)
