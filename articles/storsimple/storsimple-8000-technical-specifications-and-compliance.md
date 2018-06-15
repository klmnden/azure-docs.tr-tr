---
title: StorSimple Teknik Özellikleri | Microsoft Docs
description: StorSimple donanım bileşenleri için yasal standartları uyumluluk bilgileri ve teknik özellikleri açıklar.
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
ms.openlocfilehash: 95dbd80e740210c3800a0af10071875a6d6f0939
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
ms.locfileid: "27785535"
---
# <a name="technical-specifications-and-compliance-for-the-storsimple-device"></a>Teknik Özellikleri ve StorSimple aygıt için uyumluluğu

## <a name="overview"></a>Genel Bakış

Bu makalede açıklanan yasal standartları ve teknik belirtimler için Microsoft Azure StorSimple Cihazınızı donanım bileşenlerinin uyması. Teknik Özellikleri, güç ve soğutma modülleri (PCMs), disk sürücülerinde, depolama kapasitesi ve Kasaları açıklanmaktadır. Uyumluluk bilgileri uluslararası standartları, güvenliği ve emisyonlarını ve kablo gibi şeyler kapsar.

## <a name="power-and-cooling-module-specifications"></a>Güç ve soğutma modülü belirtimleri

StorSimple cihazı iki 100-240 V çift fan, SBB uyumlu güç soğutma modülleri (PCMs) sahiptir. Bu, bir yedek güç yapılandırmasını sağlar. Bir PCM başarısız olursa cihaz başarısız modülü değiştirilene kadar diğer PCM üzerinde normal şekilde çalışmaya devam eder.

EBOD muhafazası 580 W PCM ve birincil muhafaza 764 W PCM kullanır. Aşağıdaki tablolarda PCMs ile ilişkili teknik özellikleri listelenmektedir.

| Belirtimi | 580 W PCM (EBOD) | 764 W PCM (birincil) |
| --- | --- | --- |
| En büyük çıkış güç |580 W |764 |
| Sıklık |50/60 Hz |50/60 Hz |
| Voltaj aralık seçimi |Otomatik arasında değişen: 90 – 264 V AC, 47/63 Hz |Otomatik arasında değişen: 90-264 V AC, 47/63 Hz |
| En fazla inrush geçerli |20 A |20 A |
| Güç Faktörü Düzeltme |> % 95 nominal giriş gerilimi |> % 95 nominal giriş gerilimi |
| Uyumları |Karşılıyor EN61000-3-2 |Karşılıyor EN61000-3-2 |
| Çıktı |5v bekleme voltaj 2.0 A @ |2.7 A @ 5v bekleme voltaj |
| + 5V 42 @ A |+ 5V 40 @ A | |
| + 38 @ 12V A |+ 38 @ 12V A | |
| Takılabilir hot |Evet |Evet |
| Anahtarlar ve LED'leri |AC açık/kapalı anahtarı ve dört durum göstergesi LED'leri |AC açık/kapalı anahtarı ve altı durum göstergesi LED'leri |
| Soğutma kasası |Soğutma fanları değişken fan hızı denetimiyle Eksenel |Soğutma fanları değişken fan hızı denetimiyle Eksenel |

## <a name="power-consumption-statistics"></a>Güç tüketimi istatistikleri

Aşağıdaki tabloda StorSimple cihazı çeşitli modelleri için (gerçek değerler yayımlanan gösterebilir) tipik güç tüketimi verilerinin listeler.

| Koşullar | 240 V AC | 240 V AC | 240 V AC | 110 V AC | 110 V AC | 110 V AC |
| --- | --- | --- | --- | --- | --- | --- |
|  Fan yavaş sürücüleri boşta |1.45 A |0.31 kW |1057.76 BTU/saat |3.19 A |0.34 kW |1160.13 BTU/saat |
|  Fan yavaş sürücüleri erişme |1.54 A |0.33 kW |1126.01 BTU/saat |3.27 A |0.36 kW |1228.37 BTU/saat |
|  Fan hızlı, sürücüleri boşta, iki PSUs güç kaynağı |2.14 A |0.49 kW |1671.95 BTU/saat |4.99 A |0.54 kW |1842.56 BTU/saat |
|  Fan hızlı, boşta sürücüleri, bir PSU boşta bir güç kaynağı |2.05 A |0.48 kW |1637.83 BTU/saat |4.58 A |0,50 kW |1706.07 BTU/saat |
|  Erişme, desteklenen iki PSUs fanlar hızlı, sürücüler |2.26 A |0.51 kW |1740.19 BTU/saat |4.95 A |0.54 kW |1842.56 BTU/saat |
|  Erişim, sürücüler fanları hızlı bir PSU boşta bir gücü |2.14 A |0.49 kW |1671.95 BTU/saat |4.81 A |0.53 kW |1808.44 BTU/saat |

## <a name="disk-drive-specifications"></a>Disk sürücüsü belirtimleri

StorSimple Cihazınızı en fazla 12 3.5 inç form faktörü seri bağlı SCSI (SAS) disk sürücüleri destekler. Gerçek sürücü katı hal sürücüleri (SSD) veya sabit disk sürücülerinin (HDD'ler) ürün yapılandırmasına bağlı karışımı olabilir. 12 disk sürücüsü yuvası tarafından 3 4 yapılandırma muhafaza önünde bulunur. EBOD muhafazası 12 başka bir disk sürücüleri için ek depolama alanı sağlar. Bunlar her zaman HDD olur.

## <a name="storage-specifications"></a>Depolama belirtimleri

StorSimple cihazları sabit disk ve katı hal sürücüleri bir karışımını 8100 hem 8600 var. 8100 ve 8600 toplam kullanılabilir kapasiteyi, kabaca 15 TB ve 38 TB sırasıyla. Aşağıdaki tabloda SSD ve HDD StorSimple çözüm kapasite bağlamında bulut kapasitesi ayrıntıları belgeler.

| Cihaz modeli / kapasite | 8100 | 8600 |
| --- | --- | --- |
| Sabit disk sürücülerinin (HDD'ler) sayısı |8 |19 |
| Katı hal sürücüleri (SSD) sayısı |4 |5 |
| Tek HDD kapasite |4 TB |4 TB |
| Tek SSD kapasitesi |400 GB |800 GB |
| Yedek kapasite |4 TB |4 TB |
| Kullanılabilir HDD kapasite |14 TB |36 TB |
| Kullanılabilir SSD kapasitesi |800 GB |2 TB |
| Toplam kullanılabilir kapasite * |~ 15 TB |~ 38 TB |
| En fazla çözüm Kapasite (bulut dahil) |200 TB |500 TB |

<sup>* </sup>- *Toplam kullanılabilir kapasite verileri, meta verileri ve arabellekleri için kapasite içerir. Yerel olarak sabitlenmiş birimlerin sağlayabilirsiniz 8100 cihazda 8,5 TB'ye kadar veya daha büyük 8600 aygıtta 22,5 TB'a kadar. Daha fazla bilgi için Git [StorSimple yerel olarak sabitlenmiş birimleri](storsimple-8000-local-volume-faq.md).*

## <a name="enclosure-dimensions-and-weight-specifications"></a>Muhafaza boyut ve Ağırlık belirtimleri

Aşağıdaki tablolarda, boyutlar ve Ağırlık çeşitli muhafaza özelliklerini listeleyin.

### <a name="enclosure-dimensions"></a>Kasa boyutları

Aşağıdaki tabloda milimetre ve inç muhafazada boyutlarını listeler.

| Kasası | Milimetre | İnç |
| --- | --- | --- |
| Yükseklik |87.9 |3.46 |
| Genişliği bağlama Flanş boyunca |483 |19.02 |
| Genişliği muhafaza gövdesi boyunca |443 |17.44 |
| Muhafaza gövdesinin extremity ön bağlama Flanş gelen derinliği |577 |22.72 |
| Kasa, en extremity işlemleri panelinden derinliği |630.5 |24.82 |
| Kasa, en extremity bağlama Flanş gelen derinliği |603 |23.74 |

### <a name="enclosure-weight"></a>Muhafaza ağırlığı

Yapılandırmasına bağlı olarak tam olarak yüklenen bir birincil muhafaza 21 33 kg tartmanız ve bu iki kişiyi gerektirir.

| Kasası | Ağırlık |
| --- | --- |
| Maksimum ağırlık (yapılandırmasına bağlıdır) |30 kg – 33 kg |
| Boş (sürücüler donatılmıştır) |21 – 23 kg |

## <a name="enclosure-environment-specifications"></a>Muhafaza ortam belirtimleri

Bu bölümde muhafaza ortama ilgili özellikleri listeler. Sıcaklık, nem, yüksekliği, darbe, Titreşim, yönlendirme, güvenlik ve elektromanyetik uyumluluk (EMC) Bu kategorideki dahil edilir.

### <a name="temperature-and-humidity"></a>Sıcaklık ve nem oranı

| Kasası | Ortam sıcaklık aralığı | Ortam Bağıl nem oranı | En fazla bulaşabilir ampul |
| --- | --- | --- | --- |
| İşletimsel |5° C - 35° C (41° F - 95° F) |% 20 - % 80-condensing olmayan |28° C (82° F) |
| İşlemsel olmayan |-40° C - 70° C (40° F - 158° F) |%5 - % 100 yoğunlaşmayan |29° C (84° F) |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Hava akışı, yüksekliği, darbe, Titreşim, yönlendirme, güvenlik ve EMC

| Kasası | İşlem özellikleri |
| --- | --- |
| Hava akışı |Sistem hava akışı arkadan öne ' dir. Sistem bir low-pressure, arka Egzoz yüklemesiyle işletilen gerekir. Raf kapıları ve engellerini tarafından oluşturulan geri baskısı 5 Paskal (0,5 mm su ölçer) aşamaz. |
| İşletimsel yüksekliği |-30 metre 3045 metre (-100 ayak 10.000 ayak için), en fazla sıcaklığı 7000 fit 5 ° C tarafından XML'deki derecelendirilir. |
| Yükseklik çalışmaz |-305 metre 12.192 metre (en fazla 40.000 ayak için-1,000 ayak) |
| Darbe, işletimsel |5g 10 ms ½ Sinüs |
| Darbe çalışmaz |30g 10 ms ½ Sinüs |
| Titreşim, işletimsel |0.21g RMS rastgele 5-500 Hz |
| Titreşim, çalışmaz |1.04g RMS 2-200 Hz rastgele |
| Titreşim, yeniden konumlandırma |3g 2-200 Hz sinüsünü |
| Yönlendirme ve bağlama |19" rafa monte (2 EIA birimler) |
| Raf rayları |Minimum 700 mm (31.50 inç) Derinlik uyacak şekilde IEC 297 ile uyumlu rafları |
| Güvenlik ve onaylar |CE ve UL tr 61000-3, IEC 61000-3, UL 61000-3 |
| EMC |EN55022 (CISPR - A), FCC A |

## <a name="international-standards-compliance"></a>Uluslararası standartları uyumluluğu

Microsoft Azure StorSimple Cihazınızı aşağıdaki Uluslararası standartları ile uyumludur:  

* CE - TR 60950-1
* IEC 60950-1 CB raporu
* UL ve UL 60950-1 cUL

## <a name="safety-compliance"></a>Güvenlik uyumluluk

Microsoft Azure StorSimple Cihazınızı aşağıdaki güvenlik derecelendirmeleri uyuyor:

* Sistem ürün türü onayı: UL, cUL, CE
* Güvenlik uyumluluk: UL 60950, IEC 60950, tr 60950

## <a name="emc-compliance"></a>EMC uyumluluk

Microsoft Azure StorSimple Cihazınızı aşağıdaki EMC derecelendirmeleri karşılar.

### <a name="emissions"></a>Emisyonlarını

Aygıt EMC uyumlu emisyonlarını gerçekleştirilen ve dayanıklılık düzeyleri için kullanılır.

* Gerçekleştirilen emisyonlarını sınırlamak düzeyleri: CFR 47 bölümü 15B sınıf A EN55022 sınıf A CISPR sınıf A
* Dayanıklılık emisyonlarını sınırlamak düzeyleri: CFR 47 bölümü 15B sınıf A EN55022 sınıf A CISPR sınıf A

### <a name="harmonics-and-flicker"></a>Uyumları ve Titreşimi

Cihaz EN61000-3-2/3 ile uyumludur.

### <a name="immunity-limit-levels"></a>Karşı dayanıklılık sınırı düzeyleri

Cihaz EN55024 ile uyumludur.

## <a name="ac-power-cord-compliance"></a>AC güç kablosu uyumluluk

Tak ve tam güç kablosu derleme cihaz kullanılıyor ülke için uygun standartlarına uyması gerekir ve bu ülkede kabul edilebilir güvenlik onayları olması gerekir. ABD ve Avrupa için standartlara aşağıdaki tablolarda listelenmektedir.

### <a name="ac-power-cords---usa-must-be-nrtl-listed"></a>AC güç kablosu - ABD (listelenen NRTL olmalıdır)

| Bileşen | Belirtimi |
| --- | --- |
| Kablosu türü |SV veya SVT, 18 AWG minimum, 3 iletken 2.0 ölçümler en fazla uzunluğu |
| Bağlama |NEMA 5-15P grounding türü eki Tak 120 V, 10 A derecelendirilmiş; veya IEC 320 C14, 250 V, 10 A |
| Yuva |IEC 320 C-13, 250 V, 10 A |

### <a name="ac-power-cords---europe"></a>AC güç kablosu - Avrupa

| Bileşen | Belirtimi |
| --- | --- |
| Kablosu türü |Harmonized, H05 VVF 3G1.0 |
| Yuva |IEC 320 C-13, 250 V, 10 A |

## <a name="supported-network-cables"></a>Desteklenen ağ kabloları

Veri 2 ve veri 3, 10 GbE ağ arabirimleri için başvurmak için [desteklenen ağ kablolarını ve modüller listesi](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

## <a name="next-steps"></a>Sonraki adımlar

Şimdi, veri merkezinizdeki StorSimple Cihazınızı dağıtma hazırsınız. Daha fazla bilgi için bkz: [şirket içi Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md).

