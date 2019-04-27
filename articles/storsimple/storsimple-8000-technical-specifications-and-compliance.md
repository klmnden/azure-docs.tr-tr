---
title: StorSimple Teknik Özellikleri | Microsoft Docs
description: Teknik Özellikler ve Mevzuat standartlarına StorSimple donanım bileşenleri için uyumluluk bilgilerini açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/02/2017
ms.author: alkohli
ms.openlocfilehash: b36d721896bd7b4f95d831eded500a96969937c5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60631899"
---
# <a name="technical-specifications-and-compliance-for-the-storsimple-device"></a>Teknik Özellikler ve StorSimple cihaz için uyumluluğu

## <a name="overview"></a>Genel Bakış

Microsoft Azure StorSimple cihazınızın donanım bileşenleri, teknik belirtimler ve bu makalede açıklanan Mevzuat standartlarına uyması. Teknik Özellikleri, güç ve soğutma modülleri (PCMs), disk sürücülerini, depolama kapasitesi ve Kasaları açıklanmaktadır. Uyumluluk bilgileri Uluslararası Standartlar, güvenliği ve emisyonlarını ve kablo gibi şeyleri kapsar.

## <a name="power-and-cooling-module-specifications"></a>Güç ve soğutma modülü belirtimleri

StorSimple cihazı iki 100-240 V çift fan, SBB uyumlu Power soğutma modülleri (PCMs) sahiptir. Bu, bir yedek güç yapılandırması sağlar. Bir PCM başarısız olursa cihaz başarısız modülü değiştirilene kadar diğer PCM'yi üzerinde normal şekilde çalışmaya devam eder.

EBOD muhafazası bir 580 W PCM ve birincil bir 764 W PCM kullanır. Aşağıdaki tablolarda PCMs ile ilgili teknik özellikler listelenmektedir.

| Belirtimi | 580 W PCM (EBOD) | 764 W PCM (birincil) |
| --- | --- | --- |
| En yüksek çıkış gücü |580 W |764 |
| Sıklık |50/60 Hz |50/60 Hz |
| Voltaj aralık seçimi |Otomatik arasında değişen: 90 – 264 V AC, 47/63 Hz |Otomatik arasında değişen: 90 - 264 V AC, 47/63 Hz |
| En fazla inrush geçerli |20 A |20 A |
| Güç Faktörlü düzeltme |> %95 nominal giriş gerilimi |> %95 nominal giriş gerilimi |
| Uyumları |Karşılayan EN61000 3 2 |Karşılayan EN61000 3 2 |
| Çıktı |5v bekleme voltaj \@ 2.0 A |5v bekleme voltaj \@ 2.7 A |
| +5V \@ 42 A |+5V \@ 40 A | |
| +12V \@ 38 A |+12V \@ 38 A | |
| Takılabilir seyrek |Evet |Evet |
| Anahtarlar ve LED'leri |AC Kapat anahtar ve dört durumu gösterge LED'lerini |AC Kapat anahtar ve altı durumu gösterge LED'lerini |
| Soğutma kasası |Soğutma fanları değişken fanı hızı denetimi ile Eksenel |Soğutma fanları değişken fanı hızı denetimi ile Eksenel |

## <a name="power-consumption-statistics"></a>Güç tüketimi istatistikleri

Aşağıdaki tabloda StorSimple cihazı için çeşitli modelleri (gerçek değerleri yayımlanan değişebilir) tipik güç tüketimi verilerinin listeler.

| Koşullar | 240 V AC | 240 V AC | 240 V AC | 110 V AC | 110 V AC | 110 V AC |
| --- | --- | --- | --- | --- | --- | --- |
|  Fanlar yavaş sürücülere boşta |1.45 A |0.31 kW |1057.76 BTU/SA |3.19 A |0.34 kW |1160.13 BTU/SA |
|  Fanlar yavaş sürücülere erişme |1.54 A |0.33 kW |1126.01 BTU/SA |3.27 A |0.36 kW |1228.37 BTU/SA |
|  Fanlar daha hızlı sürücüleri boşta, iki PSUs güç kaynağı |2.14 A |0.49 kW |1671.95 BTU/SA |4.99 A |0.54 kW |1842.56 BTU/SA |
|  Fanlar hızlı, boşta sürücüleri, bir PSU boş bir güç kaynağı |2.05 A |0.48 kW |1637.83 BTU/SA |4.58 A |0,50 kW |1706.07 BTU/SA |
|  Daha hızlı sürücüleri erişme, desteklenen iki PSUs fanı |2.26 A |0.51 kW |1740.19 BTU/SA |4.95 A |0.54 kW |1842.56 BTU/SA |
|  Erişim, sürücüleri fanları hızlı bir PSU boş bir desteklenen |2.14 A |0.49 kW |1671.95 BTU/SA |4.81 A |0.53 kW |1808.44 BTU/SA |

## <a name="disk-drive-specifications"></a>Disk sürücüsü belirtimleri

StorSimple Cihazınızı 12 adede kadar 3,5 inçlik form faktörü seri ekli SCSI (SAS) disk sürücüleri destekler. Gerçek sürücü, katı hal sürücüleri (SSD'ler) ya da ürün yapılandırmasına bağlı olarak sabit disk sürücülerinin (HDD'ler) bir karışımı olabilir. 12 disk sürücüsü yuvaları tarafından 3 4 yapılandırmada muhafaza önünde yer alır. EBOD muhafazası 12 başka bir disk sürücüleri için ek depolama alanı sağlar. Bunlar, HDD'ler her zaman aynıdır.

## <a name="storage-specifications"></a>Depolama özellikleri

StorSimple cihazları, 8100 ve 8600 için sabit disk sürücüleri ve katı hal sürücülerinin bir karışımına sahip. 8100 ve 8600 için toplam kullanılabilir kapasite de yaklaşık 15 TB ve 38 TB sırasıyla. Aşağıdaki tabloda StorSimple çözümü kapasite bağlamında bulut kapasitesi SSD ve HDD ayrıntılarını içermektedir.

| Cihaz modeli / kapasite | 8100 | 8600 |
| --- | --- | --- |
| Sabit disk sürücülerinin (HDD'ler) sayısı |8 |19 |
| Katı hal sürücüleri (SSD'ler) sayısı |4 |5 |
| Tek HDD kapasite |4 TB |4 TB |
| Tek bir SSD kapasite |400 GB |800 GB |
| Yedek kapasite |4 TB |4 TB |
| HDD kullanılabilir kapasite |14 TB |36 TB |
| Kullanılabilir SSD kapasite |800 GB |2 TB |
| Toplam kullanılabilir kapasite * |~ 15 TB |~ 38 TB |
| En fazla çözüm Kapasite (bulut dahil) |200 TB |500 TB |

<sup>* </sup>- *Toplam kullanılabilir kapasite verileri, meta verileri ve arabellekleri için kapasite içerir. Yerel olarak sabitlenmiş birimler sağlayabileceğiniz 8100 cihazında 8,5 TB'a kadar veya daha büyük 8600 cihazında 22,5 TB'a kadar artırma. Daha fazla bilgi için Git [StorSimple yerel olarak sabitlenmiş birimler](storsimple-8000-local-volume-faq.md).*

## <a name="enclosure-dimensions-and-weight-specifications"></a>Kutu boyut ve Ağırlık belirtimleri

Aşağıdaki tablolar, boyut ve Ağırlık çeşitli kasa özellikleri listeler.

### <a name="enclosure-dimensions"></a>Kutu boyutları

Aşağıdaki tabloda milimetre ve inç muhafazada boyutlarını listeler.

| Kutu | Milimetre | İnç yükseklik |
| --- | --- | --- |
| Yükseklik |87.9 |3.46 |
| Bağlama Flanş arasında genişliği |483 |19.02 |
| Kutu gövdesine arasında genişliği |443 |17.44 |
| Kasa gövdesinin extremity ön bağlama Flanş gelen derinliği |577 |22.72 |
| Kutu, en extremity operations panelinden derinliği |630.5 |24.82 |
| Kutu, en extremity bağlama Flanş gelen derinliği |603 |23.74 |

### <a name="enclosure-weight"></a>Kasa ağırlığı

Yapılandırmasına bağlı olarak tam olarak yüklenen bir birincil muhafaza 21 ' için 33 kg masaya yatırın ve işlenmesi iki kişi gerektirir.

| Kutu | Ağırlık |
| --- | --- |
| Maksimum ağırlığa (yapılandırmasına bağlıdır) |30 kg – 33 kg |
| Boş (sürücüler donatılmıştır) |21 – 23 kg |

## <a name="enclosure-environment-specifications"></a>Kutu ortamı özellikleri

Bu bölümde muhafaza ortamıyla ilgili özellikleri listeler. Sıcaklık, nem, yükseklik, darbe, Titreşim, yönlendirme, güvenlik ve elektromanyetik uyumluluk (EMC), bu kategoriye dahil edilir.

### <a name="temperature-and-humidity"></a>Sıcaklık ve nem oranı

| Kutu | Ortam sıcaklığı aralığı | Ortam Bağıl nem oranı | En fazla ıslak ampul |
| --- | --- | --- | --- |
| İşletimsel |5° C - 35° C (41° F - 95° F) |% %20 80-condensing olmayan |28° C (82° F) |
| İşlem dışı |-40° C - 70° C (40° F - 158° F) |% %5 100 yoğunlaşmayan |29° C (84° F) |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Hava akışı, yükseklik, darbe, Titreşim, yönlendirme, güvenlik ve EMC

| Kutu | İşletimsel belirtimleri |
| --- | --- |
| Hava akışı |Sistem hava akışı arkadan öne ' dir. Sistem low-pressure, arka Egzozu yüklemesiyle çalıştırılması gerekir. Raf kapılar ve engellerini tarafından oluşturulan ters baskı denen 5 Paskal (0,5 mm su ölçer) aşmamalıdır. |
| İşletimsel yüksekliği |-30 ölçümlerine 3045 ölçümleri (10.000 feet için ayak-100), en fazla sıcaklığı 7000 fit 5 ° C tarafından serbest derecelendirilir. |
| Yükseklik çalışmaz |-305 ölçümlerine 12.192 ölçümleri (-1,000 ayak 40.000 ayak için) |
| Darbe işletimsel |5g 10 ms ½ Sinüs |
| Darbe çalışmaz |30g 10 ms ½ Sinüs |
| Titreşim işletimsel |0.21g RMS 5-500 Hz rastgele |
| Titreşim çalışmaz |1.04g RMS 2-200 Hz rastgele |
| Titreşim, yeniden konumlandırma |3g 2-200 Hz Sinüs |
| Yönlendirme ve bağlama |19" raf bağlama (2 EIA birimler) |
| Raf rails |En düşük 700 mm (31.50 inç) Derinlik uyacak şekilde IEC 297 ile uyumlu rafları |
| Güvenlik ve onaylar |CE ve UL tr 61000-3, IEC 61000-3, UL 61000 3 |
| EMC |EN55022 (CISPR - A), FCC A |

## <a name="international-standards-compliance"></a>Uluslararası standartlarla uyumluluk

Microsoft Azure StorSimple Cihazınızı aşağıdaki Uluslararası Standartlar ile uyumludur:  

* CE - TR 60950-1
* CB rapora IEC 60950-1
* UL ve cUL UL 60950-1'için

## <a name="safety-compliance"></a>Güvenlik uyumluluk

Microsoft Azure StorSimple Cihazınızı aşağıdaki güvenlik derecelendirmeleri karşılar:

* Sistemi ürün türü onayı: UL, cUL, CE
* Güvenlik uyumluluk: UL 60950, IEC 60950, TR 60950

## <a name="emc-compliance"></a>EMC uyumluluk

Microsoft Azure StorSimple Cihazınızı aşağıdaki EMC derecelendirmeleri karşılar.

### <a name="emissions"></a>Emisyonlarını

Cihaz EMC-emisyonlarını yürütülür ve dayanıklılık düzeyleri için uyumludur.

* Emisyonlarını sınırı düzeyleri yürütülür: CFR 47 bölümü 15B sınıfı EN55022 CISPR sınıf A sınıfı
* Emisyonlarını sınırı düzeyleri dayanıklılık: CFR 47 bölümü 15B sınıfı EN55022 CISPR sınıf A sınıfı

### <a name="harmonics-and-flicker"></a>Uyumları ve titreşimini

Cihaz EN61000-3-2/3 ile uyumludur.

### <a name="immunity-limit-levels"></a>Karşı dayanıklılık sınırı düzeyleri

Cihaz EN55024 ile uyumludur.

## <a name="ac-power-cord-compliance"></a>AC güç kablosu uyumluluk

Tak ve tam güç kablosu derleme cihaz kullanılıyor ülke için uygun standartlara uyduğumuzu gerekir ve bu ülkede kullanılabilir güvenlik onayları olması gerekir. Aşağıdaki tablolar, ABD ve Avrupa için standartları listeler.

### <a name="ac-power-cords---usa-must-be-nrtl-listed"></a>AC güç kablosu - USA (listelenen NRTL olmalıdır)

| Bileşen | Belirtimi |
| --- | --- |
| Kablosu türü |SV veya SVT, 18 AWG en az 3 conductor 2.0 ölçümleri en fazla uzunluk |
| Tak |120 V, A 10 derecelendirilmiş NEMA 5-15 P grounding türü eki Tak; veya IEC 320 C14 250 V, A 10 |
| Yuva |IEC 320 C-13, 250 V, 10 A |

### <a name="ac-power-cords---europe"></a>AC güç kablosu - Avrupa

| Bileşen | Belirtimi |
| --- | --- |
| Kablosu türü |Harmonized H05 VVF 3G1.0 |
| Yuva |IEC 320 C-13, 250 V, 10 A |

## <a name="supported-network-cables"></a>Desteklenen ağ kablosu

DATA 2 ve DATA 3, 10 GbE ağ arabirimleri için başvurmak için [desteklenen ağ kablosu ve modüller listesi](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

## <a name="next-steps"></a>Sonraki adımlar

Şimdi veri merkezinizde StorSimple cihazını dağıtmaya hazırsınız. Daha fazla bilgi için [şirket içi Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md).

