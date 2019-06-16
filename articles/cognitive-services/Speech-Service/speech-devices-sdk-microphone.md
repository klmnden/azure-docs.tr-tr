---
title: Konuşma cihaz SDK'sı mikrofon dizi önerileri - konuşma Hizmetleri
titleSuffix: Azure Cognitive Services
description: Konuşma cihaz SDK'sı mikrofon dizi öneriler sunar. Aşağıdaki dizi geometriler Microsoft ses Stack ile kullanmak için önerilir. Ses kaynakları konumunu ve ortam gürültü reddi mikrofonlar bağımlılıkları olan daha büyük sayıda belirli uygulamalar, kullanıcı senaryoları ve cihaz form faktörü geliştirilir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 63dd64e900cf68e708032569ca75ac2e8b221491
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65236994"
---
# <a name="speech-devices-sdk-microphone-array-recommendations"></a>Konuşma cihaz SDK'sı mikrofon dizi önerileri

Bu makalede, bir mikrofon dizisi için konuşma cihaz SDK'sı tasarlamayı öğrenin.

Konuşma cihaz SDK'sı mikrofon geometry ve Bileşen Seçimi dahil olmak üzere aşağıdaki yönergelere göre tasarlanmış bir mikrofon dizisi ile en iyi şekilde çalışır. Kılavuzu, tümleştirme ve elektrik konuları da verilir.

## <a name="microphone-geometry"></a>Mikrofon geometri

Aşağıdaki dizi geometriler Microsoft ses Stack ile kullanmak için önerilir. Ses kaynakları konumunu ve ortam gürültü reddi mikrofonlar bağımlılıkları olan daha büyük sayıda belirli uygulamalar, kullanıcı senaryoları ve cihaz form faktörü geliştirilir.

|          | Döngüsel bir dizi    |       |  Doğrusal bir dizi              |                |
|----------|-------------------|-------------------|----------------|----------------|
|          |<img src="media/speech-devices-sdk/7-mic-c.png" alt="7 mic circular array" width="150"/>|<img src="media/speech-devices-sdk/4-mic-c.png" alt="4 mic circular array" width="150"/>|<img src="media/speech-devices-sdk/4-mic-l.png" alt="4 mic linear array" width="150"/>|<img src="media/speech-devices-sdk/2-mic-l.png" alt="2 mic linear array" width="150"/>|
| \# Mikrofon  | 7                 | 4                 | 4              | 2              |
| Geometri | 6 dış 1 Merkezi, RADIUS eşit aralıklı 42,5 aa =| 3 dış 1 Merkezi, RADIUS eşit aralıklı 42,5 aa = | Uzunluğu 120 aa = aralığı 40 aa = | Aralık 40 aa = |

Mikrofon kanalları her dizi 0'dan artırılması, yukarıda gösterilen numaralandırmayı göre sıralanmalıdır.  Ses çalma yankı iptali gerçekleştirmek için bir diğer başvuru akışını Microsoft ses yığını gerektirir.

## <a name="component-selection"></a>Bileşen Seçimi

Mikrofon bileşenleri bozulma gürültü ve ücretsiz bir sinyal doğru bir şekilde oluşturmaya seçilmelidir.

Mikrofon seçerken önerilen özellikler şunlardır:

| Parametre                         | Önerilen                       |
|-----------------------------------|-----------------------------------|
| SNR                               | \> 65 dB (1 kHz sinyal 94 dBSPL, A ağırlıklı etkisiz)   |
| Genliğe eşleştirme                | ± 1 dB @ 1 kHz                     |
| Eşleştirme aşaması                    | 1 kHz @ 2° ±                       |
| Akustik aşırı yükleme noktası (AOP)     | \> 120 dBSPL (Bu = % 10)          |
| Bit hızı                          | En az 24-bit                    |
| Örnekleme Oranı                     | Minimum 16 kHz\*                   |
| Directivity                       | Mikrofonsa                   |
| Sıklık yanıt                | ± 3 dB, 200-8000 Hz kayan maskesi\*|
| Güvenilirlik                       | Depolama sıcaklık aralığının-40 ° C-70 ° C<br />İşletim sıcaklık aralığının 55 ° C-20 ° C  |

*\*Daha yüksek bir örnekleme oranı ya da "geniş" sıklığı aralıkları yüksek kaliteli iletişim (VoIP) uygulamaları için gerekli olabilir*

İyi Bileşen Seçimi kullanılan bileşenlerinin performansı impairing önlemek için iyi electroacoustic Tümleştirmesi ile eşleştirilmelidir. Benzersiz kullanım örnekleri ek gereksinimleri de başlatılmalarını (örneğin: sıcaklık aralıkları işletim).

## <a name="microphone-array-integration"></a>Mikrofon dizi tümleştirme

Bir cihazı ve sonra herhangi bir sabit kazanç veya EQ tümleştirildiğinde dizilerin performans aşağıdaki önerileri karşılamalıdır:

|  Parametre        |    Önerilen |
|--------------------|----------------------------------------------------|
|  SNR                 | \> 65 dB (1 kHz sinyal 94 dBSPL, A ağırlıklı etkisiz) |
|  Çıkış duyarlılık  | -26 dBFS/Pa 1 (önerilen) kHz @ |
|  Genliğe eşleştirme  | ± 2 dB, 200-8000 Hz |
|  Eşleştirme aşaması      | ± 5° 200-8000 Hz |
| BU %                 | ≤ %1, 200-8000 Hz, 94 dBSPL, 5 sırası |
|  Sıklık yanıt  | ± 6 dB, 200-8000 Hz kayan maskesi\* |

*\*"Geniş" sıklığı aralıkları yüksek kaliteli iletişim (VoIP) uygulamaları için gerekli olabilir*

## <a name="speaker-integration-recommendations"></a>Konuşmacı tümleştirme önerileri

Yankı iptali konuşmacıları içeren konuşma tanıma cihazlar için gerekli olduğundan, konuşmacı seçimi ve tümleştirme için ek öneriler sağlanır.

| Parametre                         | Önerilen                       |
|-----------------------------------|-----------------------------------|
| Doğrusallık konuları          | Hiçbir doğrusal olmayan bir işlemden sonra Konuşmacı başvurusu, aksi takdirde bir donanım tabanlı geri döngü başvuru akış gereklidir  |
| Konuşmacı geri döngü                  | Sağlanan WASAPI, özel API'ler, özel ALSA Eklentisi (Linux) ya da üretici yazılımı kanal sağlanan      |
| BU %                              | 3 dizisi bantları en az 5. sıra, @ 0,8 m ≤ %6.3, 500 315 Hz ≤ %5 630 5000 Hz 70 dBA kayıttan yürütme                 |
| Mikrofon için Yankı bağlantısı      | \> -10 dB ITU-T G.122 Annex B.4 yöntemiyle TCLw normalleştirilmiş MIC düzeyi<br />TCLw TCLwmeasured = \+ (çıkış duyarlılık hedef düzeyi - ölçülür)<br />TCLw TCLwmeasured = \+ (düzeyi - (-26) ölçülür) |

## <a name="integration-design-architecture"></a>Tümleştirme tasarım mimarisi

Mikrofon bir cihazda oturum tümleştirdiğinizde mimarisi için aşağıdaki yönergeleri gereklidir:

| Parametre                         | Öneri                    |
|-----------------------------------|-----------------------------------|
| MIC bağlantı noktası benzerlik               | Tüm mikrofon bağlantı dizideki aynı uzunlukta olan    |
| MIC bağlantı noktası boyutları               | Bağlantı noktası boyut Ø0.8 1.0 mm. Bağlantı noktası uzunluğu / bağlantı noktası çapı \< 2              |
| MIC mühürleme                       | Yığın-Yukarı birörnek uygulanan gaskets mühürleme. Önerilir \> köpük gaskets için % 70'in sıkıştırma oranı     |
| MIC güvenilirlik                   | Kafes tozunun ve (alt için PCB arasında mikrofonlar verilir ve contası/üst kapak mühürleme) giriş önlemek için kullanılması gereken  |
| MIC yalıtım                     | Rubber gaskets ve tümleşik konuşmacıları nedeniyle Titreşim yollar yalıtmak için özellikle yapısı üzerinden Titreşim ayırma      |
| Örnekleme saati                    | Cihaz ses ile düşük kayması nedenlerdir değişimi ve ücretsiz olmalıdır    |
| Kayıt özelliği                 | Cihazın aynı anda tek bir kanalı ham akışları kaydetmek mümkün olması gerekir |
| USB                               | Tüm USB ses giriş cihazlarını tanımlayıcıları göre ayarlamanız gerekir [USB ses cihazları Rev3 belirtimi](https://www.usb.org/document-library/usb-audio-devices-rev-30-and-adopters-agreement) |
| Mikrofon geometri               | Sürücüleri uygulanmalı [mikrofon dizi geometri tanımlayıcıları](https://docs.microsoft.com/windows-hardware/drivers/audio/ksproperty-audio-mic-array-geometry) doğru  |
| Bulunabilirlik                   | Cihazlar, hiçbir undiscoverable ya da denetlenemeyen donanım, üretici yazılımı ya da cihaz öğesine/öğesinden 3 taraf yazılım tabanlı doğrusal olmayan ses işleme algoritmalarını olmamalıdır|
| Biçimini yakalama                    | Yakalama biçimleri 16 kHz ve önerilen 24 bit derinlik minimum örnekleme oranını kullanmanız gerekir      |

## <a name="electrical-architecture-considerations"></a>Elektrik mimarisi konuları

Uygunsa, dizi bir USB ana bilgisayar (örneğin, Microsoft ses yığını çalıştıran SoC) bağlanabilir ve konuşma Hizmetleri veya diğer uygulamalar için arabirim.

Donanım bileşenleri PDM TDM dönüştürme gibi dinamik aralık ve mikrofon SNR korunduğundan içinde yeniden örnekleyicileri emin olmanız gerekir.

Yüksek hızlı USB ses sınıfı 2.0 için gerekli bant genişliğini daha yüksek bir örnek fiyatları ve bitler yedi kanalda kadar sağlamak için herhangi bir ses MCU içinde desteklenmelidir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma cihaz SDK'sı hakkında daha fazla bilgi edinin](speech-devices-sdk.md)
