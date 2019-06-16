---
title: Microsoft Azure veri kutusu Edge teknik özellikler ve uyumluluk | Microsoft Docs
description: Teknik belirtimler ve Azure veri kutusu Ucunuzdaki uygunluğu hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/25/2019
ms.author: alkohli
ms.openlocfilehash: 52fb32a8b34c62fe94ab35e2c051d996ab8bef10
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60755143"
---
# <a name="azure-data-box-edge-technical-specifications"></a>Azure veri kutusu Edge Teknik Özellikler

Microsoft Azure veri kutusu Edge cihazınızın donanım bileşenleri, teknik belirtimler ve bu makalede açıklanan Mevzuat standartlarına uyması. Güç kaynağı birimleri (PSUs), depolama kapasitesi, kasalar ve ortam standartları teknik özellikleri açıklanmaktadır. 

## <a name="power-supply-unit-specifications"></a>Güç kaynağı birim belirtimleri

Veri kutusu sınır cihazı, iki 100-240 V güç kaynağı birimleri (PSUs) ile yüksek performanslı fanlar sahiptir. İki PSUs bir yedek güç yapılandırmasını sağlar. Bir PSU başarısız olursa cihaz başarısız modülü değiştirilene kadar diğer PSU üzerinde normal şekilde çalışmaya devam eder. Aşağıdaki tabloda PSUs, teknik özellikler listelenmektedir.

| Belirtimi           | 750 W PSU                  |
|-------------------------|----------------------------|
| En yüksek çıkış gücü    | 750 W                     |
| Sıklık               | 50/60 Hz                   |
| Voltaj aralık seçimi | Otomatik arasında değişen: 100-240 V AC |
| Takılabilir seyrek           | Evet                        |

<!--## Power consumption statistics

The following table lists the typical power consumption data (actual values may vary from the published) for the Data Box Edge device.-->

## <a name="storage-specifications"></a>Depolama özellikleri

Veri kutusu uç cihazlarına 10 X 2.5" NVMe SSD, her 1.6 TB'lık kapasiteye sahip. Bu SSD 2 işletim sistemi diskleri ve diğer 8 veri diskleri. Cihaz için kullanılabilir toplam kapasite kabaca 12,5 TB'tır. Aşağıdaki tabloda, cihazın depolama kapasitesi için ayrıntılar bulunur.

|     Belirtimi                          |     Değer             |
|--------------------------------------------|-----------------------|
|    Katı hal sürücüleri (SSD'ler) sayısı     |    8                  |
|    Tek bir SSD kapasite                     |    1,6 TB             |
|    Toplam Kapasite                          |    12,8 TB            |
|    Toplam kullanılabilir kapasite *                  |    ~ 12,5 TB            |

**Bazı alanı, iç kullanım için ayrılmıştır.*

## <a name="enclosure-dimensions-and-weight-specifications"></a>Kutu boyut ve Ağırlık belirtimleri

Aşağıdaki tablolar, boyut ve Ağırlık çeşitli kasa özellikleri listeler.

### <a name="enclosure-dimensions"></a>Kutu boyutları

Aşağıdaki tabloda milimetre ve inç muhafazada boyutlarını listeler.

|     Kutu     |     Milimetre     |     İnç yükseklik     |
|-------------------|---------------------|----------------|
|    Yükseklik         |    44.45            |    1.75"          |
|    Genişlik          |    434.1           |    17.09"          |
|    Uzunluk          |    740.4           |    29.15"          |

Aşağıdaki tabloda milimetre ve inç sevkiyat pakette boyutlarını listeler.

|     Paket     |     Milimetre     |     İnç yükseklik     |
|-------------------|---------------------|----------------|
|    Yükseklik         |    311.2            |    12.25"          |
|    Genişlik          |    642.8          |    25.31"          |
|    Uzunluk          |   1,051.1          |    41.38"          |

### <a name="enclosure-weight"></a>Kasa ağırlığı

Cihaz paketi 66 lbs hafiftir. ve bunu işlemek iki kişi gerektirir. Cihaz ağırlığını muhafaza yapılandırmasına bağlıdır.

|     Kutu                                 |     Ağırlık          |
|-----------------------------------------------|---------------------|
|    Pakete dahil olmak üzere toplam ağırlığı       |    61 lbs.          |
|    Cihazın ağırlığı                       |    35 lbs.          |

## <a name="enclosure-environment-specifications"></a>Kutu ortamı özellikleri

Bu bölümde muhafaza ortam sıcaklığı, nem ve yükseklik gibi ilgili özellikleri listeler.

### <a name="temperature-and-humidity"></a>Sıcaklık ve nem oranı

|     Kutu         |     Ortam sıcaklığı aralığı     |     Ortam Bağıl nem oranı     |     En fazla dew noktası     |
|-----------------------|--------------------------------------|--------------------------------------|---------------------------|
|    İşletimsel        |    10° C - 35° C (50° F - 86° F)         |    % %10 80 yoğunlaşmayan.         |    29° C (84° F)            |
|    İşlem dışı    |    -40 ° C 65 ° C (-40 ° F - 149 ° F)     |    5-%95 yoğunlaşmayan.          |    33° C (91° F)            |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Hava akışı, yükseklik, darbe, Titreşim, yönlendirme, güvenlik ve EMC

|     Kutu                           |     İşletimsel belirtimleri                                                                                                                                                                                         |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    Hava akışı                              |    Sistem hava akışı arkadan öne ' dir. Sistem low-pressure, arka Egzozu yüklemesiyle çalıştırılması gerekir. <!--Back pressure created by rack doors and obstacles should not exceed 5 pascals (0.5 mm water gauge).-->    |
|    İşletimsel olarak, en fazla yükseklik        |    XML'deki derecelendirilmiş sıcaklığı maksimum 3048 ölçümleri (10.000 feet) tarafından belirlenen [belirtimleri XML'deki derecelendirme sıcaklığı](#operating-temperature-de-rating-specifications).                                                                                |
|    Maksimum yükseklik, çalışmaz    |    12\.000 ölçümleri (39,370 fit)                                                                                                                                                                                         |
|    Darbe işletimsel                   |    11 milisaniye cinsinden 6 yönleri için 6 G                                                                                                                                                                         |
|    Darbe çalışmaz               |    2 milisaniye cinsinden 6 yönleri için 71 G                                                                                                                                                                           |
|    Titreşim işletimsel               |    0, 26 G<sub>RMS</sub> 350 Hz rastgele için 5 Hz                                                                                                                                                                                     |
|    Titreşim çalışmaz           |    1.88 G<sub>RMS</sub> 10 Hz ila 500 Hz 15 dakika (test altı dışlamada.)                                                                                                                                                  |
|    Yönlendirme ve bağlama             |    19" raf bağlama                                                                                                                                                                                        |
|    Güvenlik ve onaylar                 |    TR 60950-1:2006 A1:2010 + A2:2013 + A11:2009 + A12:2011 / IEC 60950-1:2005 ed2 + A1:2009 A2:2013 tr 62311:2008                                                                                                                                                                       |
|    EMC                                  |    FCC A, ICES-003 <br>EN 55032:2012/CISPR 32:2012  <br>EN 55032:2015/CISPR 32:2015  <br>EN 55024:2010 +A1:2015/CISPR 24:2010 +A1:2015  <br>TR 61000-3-2:2014 / IEC 61000-3-2:2014 (sınıfı D)   <br>EN 61000-3-3:2013/IEC 61000-3-3:2013                                                                                                                                                                                         |
|    Enerji             |    Commission Regulation (EU) No. 617/2013                                                                                                                                                                                        |
|    RoHS           |    TR 50581:2012                                                                                                                                                                                        |


### <a name="operating-temperature-de-rating-specifications"></a>Sıcaklığı belirtimleri XML'deki derecelendirme

|     Sıcaklık XML'deki derecelendirme çalıştırma     |     Ortam sıcaklığı aralığı                                                         |
|--------------------------------------------|------------------------------------------------------------------------------------------|
|    En fazla 35 ° C (95° F)                       |    En yüksek sıcaklık tarafından ° C 1/300 m (1 ° F/547 ft) 950 milyon (3,117 ft) azaltılır.    |
|    35° C-40° C (95° F 104° f)            |    En yüksek sıcaklık tarafından 1 ° C/175 m (1 ° F/319 ft) 950 milyon (3,117 ft) azaltılır.    |
|    40° C-45° C (104° F 113° F)           |    En yüksek sıcaklık tarafından 1 ° C/125 m (1 ° F/228 ft) 950 milyon (3,117 ft) azaltılır.    |


## <a name="next-steps"></a>Sonraki adımlar

- [Azure veri kutusu Ucunuzdaki dağıtma](data-box-edge-deploy-prep.md)
